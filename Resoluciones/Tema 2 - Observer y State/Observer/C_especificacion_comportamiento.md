# Patrón Observer – Especificación del comportamiento

**Tema 2 – Punto 1.** Rediseño del CU *Registrar pago de cuota de un préstamo*. El escenario es el pago de una cuota de un préstamo **al día** con notificación vía **mail y SMS**.

## Problema de diseño

En el análisis, el gestor resolvía la notificación con sus propios métodos: `buscarFormaNotificacion()`, `enviarNotificacion()`, `getCorreo()` y `getTelefono()`, y les hablaba directamente a `InterfazCorreo.enviarCorreo()` y a `InterfazSMS.enviarSMS()`. Agregar un nuevo medio, por ejemplo WhatsApp o una notificación push, obligaba a modificar el gestor.

Con el Observer, el gestor actúa como **sujeto concreto**. Suscribe los observadores que corresponden a la forma de notificación del cliente y, cuando el pago se concreta, los **notifica** a todos de la misma manera: llama al método polimórfico `actualizar()` de la interfaz `IObservadorPagoCuota`. El gestor no conoce qué medio usa cada observador.

Se usa la variante **Push**: el sujeto envía por parámetro todos los datos de la notificación, como en Líneas Aéreas, Frigorífico y Consultorio.

## Roles del patrón

| Rol | Clase |
|---|---|
| Sujeto abstracto | `ISujeto` |
| Sujeto concreto | `GestorRegistrarPagoCuota` |
| Observador abstracto | `IObservadorPagoCuota` |
| Observadores concretos | `InterfazCorreo`, `InterfazSMS` |

## Especificación por clase

### ISujeto (sujeto abstracto)

- **observadores: IObservadorPagoCuota[]**: colección de observadores suscriptos.
- **suscribir(observador)**: agrega el observador recibido a la colección.
- **quitar(observador)**: elimina el observador de la colección.
- **notificar()**: recorre la colección e invoca `actualizar(...)` sobre cada observador.

### GestorRegistrarPagoCuota (sujeto concreto)

- **notificarPagoCuota()**: método que **dispara el patrón**. Se ejecuta después de registrar el pago y de actualizar el estado del préstamo.
  1. Invoca a `buscarFormaNotificacion()`.
  2. Crea los observadores con `crearObservadores()`.
  3. En un loop, llama a `suscribir()` por cada observador creado.
  4. Por último, llama a `notificar()`.
- **buscarFormaNotificacion()**: obtiene del cliente logueado `getFormaNotificacion()` (por ejemplo `["MAIL","SMS"]`), `getNombre()`, `getCorreo()` y `getTelefono()`. También obtiene los números del préstamo y de la cuota con `getNumero()`.
- **crearObservadores(formaNotificacion): IObservadorPagoCuota[]**: por cada forma de notificación activa crea el observador correspondiente: `"MAIL"` → `new InterfazCorreo()`, `"SMS"` → `new InterfazSMS()`. Si el cliente solo tiene SMS (flujo alternativo **A9**), solo se crea `InterfazSMS`.
- **suscribir() / quitar() / notificar()**: implementan las operaciones de `ISujeto`. En `notificar()` se pasan los datos del pago: nombre, email y teléfono del cliente, número de préstamo y de cuota, monto pagado, fecha y hora, y si el préstamo quedó **finalizado**. El último dato lo informa `prestamoSeleccionado.estaFinalizado()`.
- **Métodos que se eliminan del gestor:** `enviarNotificacion()` y las llamadas directas a `enviarCorreo()` y `enviarSMS()`.

### IObservadorPagoCuota (observador abstracto)

- **actualizar(nombreCliente, email, telefono, nroPrestamo, nroCuota, montoPagado, fechaHoraPago, prestamoFinalizado)**: **método polimórfico**. Recibe todos los datos necesarios, porque la implementación es Push. Recibe tanto el email como el teléfono, y cada observador usa el contacto que le corresponde. Así la signatura es la misma para todos.

### InterfazCorreo (observador concreto)

- **new()**: crea la interfaz.
- **actualizar(...)**: arma el mensaje con `armarMensaje()` e invoca `enviarCorreo(email, asunto, cuerpoCorreo)`.
- **armarMensaje(...)**: arma el texto informando el **pago exitoso de la cuota** (número de cuota, monto y fecha). Si `prestamoFinalizado = true`, agrega que el **préstamo fue finalizado** y que el monto pagado corresponde a la última cuota.
- **enviarCorreo(...)**: se comunica con el actor `ServidorCorreo` (`enviarEmail()`).

### InterfazSMS (observador concreto)

- **new()**: crea la interfaz.
- **actualizar(...)**: arma un texto corto con `armarMensaje()` e invoca `enviarSMS(telefono, textoSMS)`.
- **enviarSMS(...)**: se comunica con el actor `ServidorSMS` (`enviarMensajeSMS()`).

## Pseudocódigo

```java
// ===== Sujeto abstracto =====
interface ISujeto {
    IObservadorPagoCuota[] observadores;
    void suscribir(IObservadorPagoCuota observador);
    void quitar(IObservadorPagoCuota observador);
    void notificar();
}

// ===== Observador abstracto =====
interface IObservadorPagoCuota {
    void actualizar(String nombreCliente, String email, String telefono,
                    int nroPrestamo, int nroCuota, Double montoPagado,
                    DateTime fechaHoraPago, Boolean prestamoFinalizado);
}

// ===== Sujeto concreto =====
public class GestorRegistrarPagoCuota implements ISujeto {
    private IObservadorPagoCuota[] observadores;
    ...
    public void tomarConfirmacion() {
        registrarPagoCuota();                       // <<include>> Registrar pago
        prestamoSeleccionado.pagarCuota(...);       // rediseñado con State
        prestamoFinalizado = prestamoSeleccionado.estaFinalizado();
        notificarPagoCuota();
        finCU();
    }

    public void notificarPagoCuota() {
        buscarFormaNotificacion();
        IObservadorPagoCuota[] creados = crearObservadores(formaNotificacionCliente);
        for (IObservadorPagoCuota obs : creados) {
            suscribir(obs);
        }
        notificar();
    }

    public void buscarFormaNotificacion() {
        formaNotificacionCliente = clienteLogueado.getFormaNotificacion();
        nombreCliente   = clienteLogueado.getNombre();
        emailCliente    = clienteLogueado.getCorreo();
        telefonoCliente = clienteLogueado.getTelefono();
        nroPrestamo     = prestamoSeleccionado.getNumero();
        nroCuota        = cuotaSeleccionada.getNumero();
    }

    public IObservadorPagoCuota[] crearObservadores(String[] formas) {
        List<IObservadorPagoCuota> lista = new ArrayList();
        for (String forma : formas) {
            if (forma.equals("MAIL")) lista.add(new InterfazCorreo());
            if (forma.equals("SMS"))  lista.add(new InterfazSMS());
        }
        return lista.toArray();
    }

    public void suscribir(IObservadorPagoCuota o) { observadores.add(o); }
    public void quitar(IObservadorPagoCuota o)    { observadores.remove(o); }

    public void notificar() {
        for (int i = 0; i < observadores.length(); i++) {
            observadores[i].actualizar(nombreCliente, emailCliente, telefonoCliente,
                    nroPrestamo, nroCuota, montoAAbonar, fechaActual, prestamoFinalizado);
        }
    }
}

// ===== Observadores concretos =====
public class InterfazCorreo implements IObservadorPagoCuota {
    private String asunto; private String cuerpoCorreo;

    public void actualizar(String nombre, String email, String tel, int nroPrestamo,
                           int nroCuota, Double monto, DateTime fechaHora, Boolean finalizado) {
        asunto = finalizado ? "Préstamo finalizado" : "Pago de cuota registrado";
        cuerpoCorreo = armarMensaje(nombre, nroPrestamo, nroCuota, monto, fechaHora, finalizado);
        enviarCorreo(email, asunto, cuerpoCorreo);
    }
    public String armarMensaje(...) {
        String msj = "Hola " + nombre + ", se registró el pago de la cuota " + nroCuota
                   + " del préstamo " + nroPrestamo + " por $" + monto + " el " + fechaHora + ".";
        if (finalizado) msj += " Con este pago tu préstamo quedó FINALIZADO.";
        return msj;
    }
    public void enviarCorreo(String email, String asunto, String cuerpo) {
        servidorCorreo.enviarEmail(email, asunto, cuerpo);
    }
}

public class InterfazSMS implements IObservadorPagoCuota {
    private String textoSMS;

    public void actualizar(String nombre, String email, String tel, int nroPrestamo,
                           int nroCuota, Double monto, DateTime fechaHora, Boolean finalizado) {
        textoSMS = armarMensaje(nombre, nroPrestamo, nroCuota, monto, fechaHora, finalizado);
        enviarSMS(tel, textoSMS);
    }
    public String armarMensaje(...) { /* versión corta del mensaje */ }
    public void enviarSMS(String telefono, String texto) {
        servidorSMS.enviarMensajeSMS(telefono, texto);
    }
}
```

## Notas

- La dinámica muestra el escenario del CU, con notificación activa vía mail y SMS (paso 14 y observación 2). Con el flujo alternativo **A9** (solo SMS) se crea y se suscribe únicamente `:InterfazSMS`.
- El mismo mecanismo cubre las dos notificaciones que pide la consigna: el **pago exitoso de la cuota** y la **finalización del préstamo**, que se diferencian con el parámetro `prestamoFinalizado`.
