# Patrón Adapter – Especificación del comportamiento

**Tema 1 – Punto 1.** Rediseño del CU *Registrar pago de cuota de un préstamo*. El escenario es el pago de una cuota vencida de un préstamo en mora, con cobro por el ente externo **SENASA**.

## Problema de diseño

En el análisis, el gestor se comunicaba con la boundary `InterfazCentralDeudores` mediante `obtenerCodigoPago()`. Pero existen **dos centrales de deudores** con interfaces incompatibles:

| Servicio | Método | Recibe | Devuelve | Cómo se filtra el préstamo vigente |
|---|---|---|---|---|
| SENASA | `consultarDeuda` | DNI + fecha y hora | JSON (String) con montoOriginal, fechaAcreditacion, codigoPago, nombreEmpresa | por **nombre de la empresa** y **monto** (original) |
| AGIP | `consultarAdeudamiento` | DNI + código de empresa | XML (String) con fechaAcreditacion, montoFinal, codigoPago | por **fecha** (de acreditación) y **monto** (final) |

Con el Adapter, el gestor ve una sola interfaz (`IAdaptadorCentralDeudores`). Cada adaptador concreto traduce los parámetros al formato de su servicio, interpreta la respuesta (JSON o XML) y devuelve lo que necesita el gestor: el **código de pago**. Para agregar una nueva central alcanza con un nuevo adaptador, sin tocar el gestor.

## Roles del patrón

| Rol | Clase |
|---|---|
| Cliente | `GestorRegistrarPagoCuota` |
| Objetivo (Target) | `IAdaptadorCentralDeudores` |
| Adaptadores concretos | `AdaptadorSENASA`, `AdaptadorAGIP` |
| Adaptables (Adaptee) | `ServicioSENASA`, `ServicioAGIP` |

## Especificación por clase

### GestorRegistrarPagoCuota (cliente del patrón)

- **validarPrestamoEnteExterno(): Boolean**: método del análisis. Le pide al préstamo seleccionado `obtenerEnteCobroExterno()`. Si devuelve `null`, el préstamo no se cobra por ente externo y el patrón no se dispara.
- **obtenerCodigoPagoEnteExterno()**: método del análisis rediseñado. Obtiene los datos con los que se identifica el préstamo en la central:
  - del cliente logueado, `getDNI()`;
  - del préstamo seleccionado, `getFechaHoraAcreditacion()`, `getMontoOtorgado()` y `calcularMontoTotalADevolver()`.

  Después invoca a `crearAdaptadorCentralDeudores()` y delega en el adaptador la obtención del código de pago.
- **crearAdaptadorCentralDeudores(enteCobroExterno: String): IAdaptadorCentralDeudores**: **método de enganche**. Según el ente asignado al préstamo, crea el adaptador concreto (`"SENASA"` → `AdaptadorSENASA`, `"AGIP"` → `AdaptadorAGIP`) y lo guarda en el atributo `adaptadorCentralDeudores`, tipado con la interfaz.

### IAdaptadorCentralDeudores (interfaz objetivo)

- **obtenerCodigoPago(dni, fechaHoraAcreditacion, montoOtorgado, montoTotalADevolver): String**: **método polimórfico**. Es el único método que usa el gestor. Recibe todos los datos que puede necesitar cualquiera de los servicios. Cada adaptador usa los que le corresponden, para que la signatura sea la misma en los dos.
- **pedirHistorialPrestamos()**, **adaptarHistorialPrestamos()** y **filtrarPrestamoVigente(): String**: métodos polimórficos que cada adaptador implementa a su manera.

### AdaptadorSENASA

- **new()**: crea el adaptador y su referencia al `ServicioSENASA`.
- **obtenerCodigoPago(...)**: guarda los parámetros en sus atributos e invoca, en orden, a `pedirHistorialPrestamos()`, `adaptarHistorialPrestamos()` y `filtrarPrestamoVigente()`. Devuelve el código de pago.
- **pedirHistorialPrestamos()**: transforma la fecha y hora de acreditación al formato que espera SENASA con `convertirFechaHora()`. Luego invoca `consultarDeuda(dni, fechaHora)` del servicio y guarda el JSON recibido en `historialRecibido`.
- **adaptarHistorialPrestamos()**: recorre el JSON y arma la matriz `historialAdaptado`, con una fila por préstamo y las columnas `[montoOriginal, fechaAcreditacion, codigoPago, nombreEmpresa]`. Convierte el monto a `Double`.
- **filtrarPrestamoVigente(): String**: recorre la matriz y devuelve el `codigoPago` de la fila con `nombreEmpresa = "CreditIn"` y `montoOriginal = montoOtorgado`.

### AdaptadorAGIP

- **new()**: crea el adaptador y su referencia al `ServicioAGIP`.
- **obtenerCodigoPago(...)**: igual que el de SENASA. No usa `montoOtorgado`, porque AGIP informa el **monto final**.
- **pedirHistorialPrestamos()**: invoca `consultarAdeudamiento(dni, codigoEmpresa)`, usando el código de CreditIn ante AGIP que el adaptador tiene como atributo. Guarda el XML recibido.
- **adaptarHistorialPrestamos()**: recorre los nodos del XML y arma la matriz `historialAdaptado`, con las columnas `[fechaAcreditacion, montoFinal, codigoPago]`. Convierte la fecha con `convertirFecha()`.
- **filtrarPrestamoVigente(): String**: devuelve el `codigoPago` de la fila cuya fecha de acreditación coincide con la del préstamo y cuyo `montoFinal = montoTotalADevolver`.

### Prestamo (método nuevo de diseño)

- **calcularMontoTotalADevolver(): Double**: devuelve `montoOtorgado * (1 + tasaInteres)`, que es el monto total sin recargos. Se usa para comparar contra el *monto final* que informa AGIP.

## Pseudocódigo

```java
// ===== Gestor (cliente del patrón) =====
public void obtenerCodigoPagoEnteExterno() {
    dniClienteLogueado    = clienteLogueado.getDNI();
    fechaHoraAcreditacion = prestamoSeleccionado.getFechaHoraAcreditacion();
    montoOtorgado         = prestamoSeleccionado.getMontoOtorgado();
    montoTotalADevolver   = prestamoSeleccionado.calcularMontoTotalADevolver();

    adaptadorCentralDeudores = crearAdaptadorCentralDeudores(enteCobroExterno);
    codigoPagoEnteExterno = adaptadorCentralDeudores.obtenerCodigoPago(
            dniClienteLogueado, fechaHoraAcreditacion, montoOtorgado, montoTotalADevolver);
}

// método de enganche
public IAdaptadorCentralDeudores crearAdaptadorCentralDeudores(String ente) {
    if (ente.equals("SENASA")) return new AdaptadorSENASA();
    if (ente.equals("AGIP"))   return new AdaptadorAGIP();
    return null;
}

// ===== Interfaz objetivo =====
interface IAdaptadorCentralDeudores {
    String obtenerCodigoPago(String dni, DateTime fechaHoraAcreditacion,
                             Double montoOtorgado, Double montoTotalADevolver);
    void   pedirHistorialPrestamos();
    void   adaptarHistorialPrestamos();
    String filtrarPrestamoVigente();
}

// ===== Adaptador concreto SENASA =====
class AdaptadorSENASA implements IAdaptadorCentralDeudores {
    private ServicioSENASA servicioSENASA = new ServicioSENASA();
    private String nombreEmpresa = "CreditIn";
    private String dni; private DateTime fechaHoraAcreditacion; private Double montoOtorgado;
    private String historialRecibido;   // JSON
    private String[][] historialAdaptado;

    public String obtenerCodigoPago(String dni, DateTime fha, Double montoOtorgado, Double montoTotal) {
        this.dni = dni; this.fechaHoraAcreditacion = fha; this.montoOtorgado = montoOtorgado;
        pedirHistorialPrestamos();
        adaptarHistorialPrestamos();
        return filtrarPrestamoVigente();
    }
    public void pedirHistorialPrestamos() {
        String fechaHora = convertirFechaHora(fechaHoraAcreditacion);  // ej: "2026-03-01T10:15:00"
        historialRecibido = servicioSENASA.consultarDeuda(dni, fechaHora);
    }
    public void adaptarHistorialPrestamos() {
        JSONArray prestamos = parsearJSON(historialRecibido);
        historialAdaptado = new String[prestamos.length()][4];
        for (int i = 0; i < prestamos.length(); i++) {
            historialAdaptado[i][0] = prestamos[i].get("montoOriginal");
            historialAdaptado[i][1] = prestamos[i].get("fechaAcreditacion");
            historialAdaptado[i][2] = prestamos[i].get("codigoPago");
            historialAdaptado[i][3] = prestamos[i].get("nombreEmpresa");
        }
    }
    public String filtrarPrestamoVigente() {
        for (String[] fila : historialAdaptado) {
            if (fila[3].equals(nombreEmpresa) && Double.parse(fila[0]) == montoOtorgado)
                return fila[2];               // codigoPago
        }
        return null;
    }
}

// ===== Adaptador concreto AGIP =====
class AdaptadorAGIP implements IAdaptadorCentralDeudores {
    private ServicioAGIP servicioAGIP = new ServicioAGIP();
    private String codigoEmpresa;           // código de CreditIn en AGIP
    private String dni; private DateTime fechaHoraAcreditacion; private Double montoTotalADevolver;
    private String historialRecibido;       // XML
    private String[][] historialAdaptado;

    public String obtenerCodigoPago(String dni, DateTime fha, Double montoOtorgado, Double montoTotal) {
        this.dni = dni; this.fechaHoraAcreditacion = fha; this.montoTotalADevolver = montoTotal;
        pedirHistorialPrestamos();
        adaptarHistorialPrestamos();
        return filtrarPrestamoVigente();
    }
    public void pedirHistorialPrestamos() {
        historialRecibido = servicioAGIP.consultarAdeudamiento(dni, codigoEmpresa);
    }
    public void adaptarHistorialPrestamos() {
        NodeList prestamos = parsearXML(historialRecibido).getElementsByTagName("prestamo");
        historialAdaptado = new String[prestamos.length()][3];
        for (int i = 0; i < prestamos.length(); i++) {
            historialAdaptado[i][0] = prestamos[i].get("fechaAcreditacion");
            historialAdaptado[i][1] = prestamos[i].get("montoFinal");
            historialAdaptado[i][2] = prestamos[i].get("codigoPago");
        }
    }
    public String filtrarPrestamoVigente() {
        for (String[] fila : historialAdaptado) {
            if (convertirFecha(fila[0]).equals(fechaHoraAcreditacion.getDate())
                    && Double.parse(fila[1]) == montoTotalADevolver)
                return fila[2];
        }
        return null;
    }
}
```

## Notas

- La dinámica muestra el escenario descripto (SENASA). Para AGIP la secuencia es la misma: cambia el objeto creado (`:AdaptadorAGIP`) y el mensaje al servicio (`consultarAdeudamiento(dni, codigoEmpresa)`). Es el flujo alternativo **A9**.
- En los dos casos se devuelve el **código de pago** del préstamo, como pide la consigna.
- La boundary `InterfazCentralDeudores` del análisis desaparece. Su responsabilidad pasa a los adaptadores, que encapsulan la comunicación con cada servicio.
