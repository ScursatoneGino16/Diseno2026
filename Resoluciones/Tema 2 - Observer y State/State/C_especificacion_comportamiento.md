# Patrón State – Especificación del comportamiento

**Tema 2 – Punto 2.** Diseñar un mecanismo que permita modificar el comportamiento variable de la clase **Préstamo**, de acuerdo con la situación en la que se encuentra. El escenario es el pago de la **última cuota** de un préstamo **al día**, con lo que el préstamo pasa a **Finalizado**.

## Problema de diseño

En el análisis, el gestor decidía el nuevo estado del préstamo:
- llamaba a `determinarEstadoPrestamo()` y `validarCuotasRestantes()`;
- luego a `finalizarPrestamo()` y `buscarEstadoFinalizado()`, con un *loop Recorrer Estados* que preguntaba `sosAmbitoPrestamo()` y `sosFinalizado()`;
- por último llamaba a `prestamo.finalizar()`.

La lógica de "qué hacer según el estado actual" quedaba repartida en condicionales.

Con el State, cada estado de la máquina de estados del Préstamo es una clase concreta que hereda de la clase abstracta `Estado`. El préstamo (contexto) tiene un puntero a su `estadoActual` y **le delega** el comportamiento. Cada estado sabe a qué estado pasa y él mismo crea el siguiente estado y el `CambioEstado` correspondiente. Desaparecen la **búsqueda de estados** y los condicionales en el gestor.

## Roles del patrón

| Rol | Clase |
|---|---|
| Contexto | `Prestamo` |
| Estado abstracto | `Estado` (clase abstracta) |
| Estados concretos | `Creado`, `AlDia`, `EnMora`, `Rechazado`, `Finalizado`, `ConAccionJudicial` |

## Transiciones según la máquina de estados del Préstamo

| Estado | Evento / método | Guarda | Estado destino |
|---|---|---|---|
| Creado | `acreditar()` (2. Evaluar solicitud) | evaluación positiva | AlDia |
| Creado | `rechazar()` (2. Evaluar solicitud) | evaluación negativa | Rechazado |
| AlDia | `pagarCuota()` (3. Registrar pago) | última cuota = NO | AlDia (no cambia) |
| AlDia | `pagarCuota()` (3. Registrar pago) | última cuota = SI | **Finalizado** |
| AlDia | `vencerCuota()` (4. Validar vencimiento) | cuotas vencidas = SI | EnMora |
| EnMora | `pagarCuota()` (3. Registrar pago) | cuotas vencidas = SI | EnMora (no cambia) |
| EnMora | `pagarCuota()` (3. Registrar pago) | cuotas vencidas = NO | AlDia |
| EnMora | `pagarCuota()` (3. Registrar pago) | última cuota = SI | Finalizado |
| EnMora | `iniciarAccionJudicial()` (10.) | 180 días de la 1ª cuota impaga | ConAccionJudicial |
| Rechazado, Finalizado, ConAccionJudicial | — | — | estados finales: todos los métodos lanzan excepción |

## Especificación por clase

### GestorRegistrarPagoCuota

- **pagarCuota()**: invoca `prestamoSeleccionado.pagarCuota(cuota, estadoPagada, fechaHoraActual)`. El gestor ya no determina el estado del préstamo.
- Después le pregunta al préstamo `estaFinalizado()`. Lo necesita para la notificación del Observer.
- **Métodos que se eliminan:** `determinarEstadoPrestamo()`, `finalizarPrestamo()`, `buscarEstadoFinalizado()` y el loop *Recorrer Estados* del ámbito Préstamo.

### Prestamo (contexto)

- **estadoActual: Estado**: puntero que se **agrega en el diseño** para aplicar el patrón.
- **pagarCuota(cuota, estadoPagada, fechaHoraActual)**:
  1. Le indica a la cuota que se pague con `cuota.pagar(estadoPagada)`.
  2. Recalcula y actualiza el saldo con `calcularSaldoPendiente()` y `setSaldoPendiente()`.
  3. **Delega** la transición en su estado actual: `estadoActual.pagarCuota(this, fechaHoraActual, cambioEstado, cuota)`. Se pasa a sí mismo, para que el estado pueda setearle el nuevo estado, y también los historiales y las cuotas.
- **setEstadoActual(estado)**: actualiza el puntero al estado actual.
- **agregarCambioEstado(cambioEstado)**: agrega el nuevo `CambioEstado` a su colección.
- **estaFinalizado(): Boolean**: delega en `estadoActual.esFinalizado()`.
- **estasActivo(): Boolean**: delega en `estadoActual.esActivo()`. Reemplaza el `*sosActual()` → `estasActivo()` → `sosActivo()` de la búsqueda de préstamos activos del análisis.

### Estado (clase abstracta)

- Define **todas** las operaciones de transición: `acreditar()`, `rechazar()`, `pagarCuota()`, `vencerCuota()` e `iniciarAccionJudicial()`. Por defecto lanzan una excepción (*operación no válida para el estado actual*), y cada subclase redefine solo las que son válidas para ella.
- **pagarCuota(prestamo, fechaHoraActual, cambiosEstado, cuotas)**: **método polimórfico** del escenario.
- **esActivo()** y **esFinalizado()**: por defecto devuelven `false`.

### AlDia (estado concreto del escenario)

- **pagarCuota(...)**:
  1. Invoca `validarCuotasRestantes(cuotas)`, que recorre las cuotas preguntando `estasPagada()`.
  2. Si **quedan cuotas sin pagar** (última cuota = NO), el préstamo sigue Al Día y no se hace ningún cambio (flujo alternativo **A7**).
  3. Si **era la última cuota** (última cuota = SI), se ejecuta la transición *finalizar*:
     - `finalizarCambioEstadoActual()`: recorre los cambios de estado con `*sosActual()` y al actual le setea `setFechaHoraFin(fechaHoraActual)`;
     - `crearEstado()`: hace `new Finalizado()`;
     - `crearCambioEstado(fechaHoraActual, nuevoEstado)`: hace `new CambioEstado(fechaHoraActual, nuevoEstado)`;
     - `prestamo.setEstadoActual(nuevoEstado)` y `prestamo.agregarCambioEstado(nuevoCambioEstado)`.
- **vencerCuota(...)**: lo invoca el proceso de medianoche cuando vence una cuota. Crea `EnMora` y su `CambioEstado` de la misma forma.
- **esActivo()**: devuelve `true`.

### EnMora

- **pagarCuota(...)**:
  - si era la última cuota, pasa a **Finalizado**;
  - si no, verifica `hayCuotasVencidas(cuotas)`: si no quedan cuotas vencidas pasa a **AlDia** (flujo **A6**); si quedan, sigue **EnMora** (flujo **A5**).

  Los cambios de estado se hacen con la misma secuencia que en `AlDia`.
- **vencerCuota(...)**: autotransición, sigue en mora.
- **iniciarAccionJudicial(...)**: crea `ConAccionJudicial`.
- **esActivo()**: devuelve `true`.

### Creado

- **acreditar(...)** crea `AlDia` y **rechazar(...)** crea `Rechazado`, siempre con su `CambioEstado`.

### Rechazado, Finalizado, ConAccionJudicial

- Son estados finales: no redefinen transiciones, así que heredan la excepción.
- `Finalizado.esFinalizado()` devuelve `true`.

## Pseudocódigo

```java
// ===== Gestor =====
public void pagarCuota() {
    prestamoSeleccionado.pagarCuota(cuotaSeleccionada, estadoPagada, fechaActual);
    prestamoFinalizado = prestamoSeleccionado.estaFinalizado();
}

// ===== Contexto =====
public class Prestamo {
    private Estado estadoActual;
    private CambioEstado[] cambioEstado;
    private Cuota[] cuota;
    ...
    public void pagarCuota(Cuota c, EstadoCuota estadoPagada, DateTime fechaHoraActual) {
        c.pagar(estadoPagada);
        setSaldoPendiente(calcularSaldoPendiente());
        estadoActual.pagarCuota(this, fechaHoraActual, cambioEstado, cuota);   // delega
    }
    public void setEstadoActual(Estado e)            { estadoActual = e; }
    public void agregarCambioEstado(CambioEstado ce) { cambioEstado.add(ce); }
    public Boolean estaFinalizado()                  { return estadoActual.esFinalizado(); }
    public Boolean estasActivo()                     { return estadoActual.esActivo(); }
}

// ===== Estado abstracto =====
public abstract class Estado {
    protected String nombre;
    public void acreditar(Prestamo p, DateTime f, CambioEstado[] h)            { throw new OperacionInvalidaException(); }
    public void rechazar(Prestamo p, DateTime f, CambioEstado[] h)             { throw new OperacionInvalidaException(); }
    public void pagarCuota(Prestamo p, DateTime f, CambioEstado[] h, Cuota[] c){ throw new OperacionInvalidaException(); }
    public void vencerCuota(Prestamo p, DateTime f, CambioEstado[] h)          { throw new OperacionInvalidaException(); }
    public void iniciarAccionJudicial(Prestamo p, DateTime f, CambioEstado[] h){ throw new OperacionInvalidaException(); }
    public Boolean esActivo()     { return false; }
    public Boolean esFinalizado() { return false; }
}

// ===== Estado concreto AlDia =====
public class AlDia extends Estado {
    public void pagarCuota(Prestamo prestamo, DateTime fechaHoraActual,
                           CambioEstado[] cambiosEstado, Cuota[] cuotas) {
        if (validarCuotasRestantes(cuotas)) {
            return;                                  // [última cuota = NO] sigue AlDia
        }
        // [última cuota = SI] / finalizar()
        finalizarCambioEstadoActual(cambiosEstado, fechaHoraActual);
        Estado nuevo = crearEstado();                                  // new Finalizado()
        CambioEstado ce = crearCambioEstado(fechaHoraActual, nuevo);   // new CambioEstado(...)
        prestamo.setEstadoActual(nuevo);
        prestamo.agregarCambioEstado(ce);
    }
    public Boolean validarCuotasRestantes(Cuota[] cuotas) {   // true si quedan cuotas impagas
        for (Cuota c : cuotas) if (!c.estasPagada()) return true;
        return false;
    }
    public void finalizarCambioEstadoActual(CambioEstado[] h, DateTime f) {
        for (CambioEstado ce : h) if (ce.sosActual()) ce.setFechaHoraFin(f);
    }
    public Estado crearEstado() { return new Finalizado(); }
    public CambioEstado crearCambioEstado(DateTime f, Estado e) { return new CambioEstado(f, e); }

    public void vencerCuota(Prestamo prestamo, DateTime f, CambioEstado[] h) {
        finalizarCambioEstadoActual(h, f);
        Estado nuevo = new EnMora();
        prestamo.setEstadoActual(nuevo);
        prestamo.agregarCambioEstado(new CambioEstado(f, nuevo));
    }
    public Boolean esActivo() { return true; }
}

// ===== Estado concreto EnMora =====
public class EnMora extends Estado {
    public void pagarCuota(Prestamo prestamo, DateTime f, CambioEstado[] h, Cuota[] cuotas) {
        Estado nuevo;
        if (!validarCuotasRestantes(cuotas))   nuevo = new Finalizado();  // última cuota
        else if (!hayCuotasVencidas(cuotas))   nuevo = new AlDia();       // se regulariza
        else return;                                                      // sigue en mora
        finalizarCambioEstadoActual(h, f);
        prestamo.setEstadoActual(nuevo);
        prestamo.agregarCambioEstado(new CambioEstado(f, nuevo));
    }
    public Boolean hayCuotasVencidas(Cuota[] cuotas) {
        for (Cuota c : cuotas) if (c.estasVencida()) return true;
        return false;
    }
    public void iniciarAccionJudicial(Prestamo p, DateTime f, CambioEstado[] h) {
        finalizarCambioEstadoActual(h, f);
        Estado nuevo = new ConAccionJudicial();
        p.setEstadoActual(nuevo);
        p.agregarCambioEstado(new CambioEstado(f, nuevo));
    }
    public Boolean esActivo() { return true; }
    // validarCuotasRestantes(), finalizarCambioEstadoActual(): igual que en AlDia
}

// ===== Estados finales =====
public class Finalizado extends Estado        { public Boolean esFinalizado() { return true; } }
public class Rechazado extends Estado         { }
public class ConAccionJudicial extends Estado { }
```

## Notas

- El patrón se aplica **solo al Préstamo**, que es lo que pide la consigna. Los estados de la **Cuota** se mantienen como en el análisis: el gestor sigue buscando el estado *Pagada* con su loop. Para que no choque con la clase abstracta `Estado`, en el diseño esa entidad se llama `EstadoCuota`.
- `CambioEstado` se sigue usando como historial. Su asociación `estado` ahora apunta a la clase abstracta `Estado`.
