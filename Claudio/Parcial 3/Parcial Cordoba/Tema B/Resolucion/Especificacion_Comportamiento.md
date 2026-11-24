# Tercer Parcial – Tema B – Punto C: Especificación del comportamiento

CU 3 "Registrar pago de cuota de un préstamo". Escenario: pago de la última cuota de un préstamo al día, con notificación vía mail y SMS.

---

## 1.C – Patrón Observer

**GestorRegistrarPagoCuota (SujetoConcreto)**

**notificarPagoCuota():** Método que dispara la ejecución del patrón. Antes de él, el Gestor ya registró el pago de la cuota y determinó que el préstamo quedó finalizado, por lo que cuenta con los atributos `montoPagado` y `prestamoFinalizado` cargados. Primero llama a `buscarFormaNotificacion()` y a `buscarDatosContacto()` para obtener la información del cliente logueado, luego a `crearObservadores()` para crear y suscribir las interfaces que serán observadores, después a `notificar()` para informarles el pago, y por último a `finCU()`.

**buscarFormaNotificacion():** Le solicita al cliente logueado su forma de notificación activa mediante `getFormaNotificacion()` y la guarda en el atributo `formaNotificacionCliente`. Este dato determina qué observadores se van a crear (en el escenario, mail y SMS).

**buscarDatosContacto():** Le solicita al cliente logueado su correo electrónico mediante `getCorreo()` y su número de teléfono mediante `getTelefono()`, y los guarda en los atributos `emailCliente` y `telefonoCliente`. Se obtienen antes de notificar porque se envían por parámetro a los observadores (solución Push).

**crearObservadores():** Según la forma de notificación del cliente, crea las interfaces que actuarán como observadores: `InterfazCorreo` si tiene activa la notificación por mail e `InterfazSMS` si la tiene por SMS. En el escenario se crean las dos. Luego llama en un loop al método `suscribir()` del SujetoConcreto (el mismo GestorRegistrarPagoCuota), para que uno a uno vaya suscribiendo los observadores. El loop finaliza cuando no hay más interfaces creadas.

**suscribir(observador: IObservadorPagoCuota):** Implementa el método de `ISujeto`. Suscribe al observador recibido por parámetro, en este caso la InterfazCorreo y la InterfazSMS, agregándolo a la colección `observadores`.

**notificar():** Implementa el método de `ISujeto`. Recorre la colección de observadores suscriptos y llama al método `actualizar()` de cada uno, enviándole por parámetro el correo y el teléfono del cliente, el monto pagado en la última cuota y el indicador de préstamo finalizado. El Gestor solo conoce la interfaz `IObservadorPagoCuota`, no las clases concretas, por lo que se pueden agregar nuevos medios de notificación sin modificarlo.


**Cliente**

**getFormaNotificacion():** Retorna la forma de notificación activa del cliente (mail, SMS o ambas).

**getCorreo():** Retorna el correo electrónico del cliente.

**getTelefono():** Retorna el número de teléfono del cliente.

**ObservadoresConcretos (InterfazCorreo e InterfazSMS)**

**actualizar():** Implementación polimórfica del método de `IObservadorPagoCuota`. Recibe la información necesaria para generar la notificación. Cada interfaz recibe tanto el correo como el teléfono y usa el que le corresponde: InterfazCorreo llama a `enviarCorreo()` e InterfazSMS llama a `enviarMensaje()`, pasándoles los mismos datos.

**enviarCorreo():** Se encarga de realizar el envío del email. Arma el mensaje informando el pago de la cuota por el monto pagado y, si el préstamo quedó finalizado, agrega que el préstamo se encuentra finalizado. Luego lo envía a la dirección de correo recibida por parámetro mediante `enviarEmail()` del Servidor de Correo.

**enviarMensaje():** Se encarga de realizar el envío del SMS. Arma un mensaje corto informando el pago de la cuota por el monto pagado y, si el préstamo quedó finalizado, agrega que el préstamo se encuentra finalizado. Luego lo envía al número de teléfono recibido por parámetro mediante `enviarSMS()` del Servidor SMS.

La aplicación del patrón es con una solución tipo **Push**: se envía por parámetro el correo y el teléfono para que cada interfaz realice el envío por su medio, el monto pagado en la última cuota y el indicador de que el préstamo fue finalizado, que es lo que pide informar el paso 14 del caso de uso.

---

## 2.C – Patrón State

**GestorRegistrarPagoCuota**

**finalizarPrestamo():** Método que dispara la ejecución del patrón. Es el momento en que el préstamo cambia de estado (transición AlDia → Finalizado, evento "3. Registrar pago de cuota [última cuota = SÍ] / finalizar()" de la máquina de estados). Obtiene la fecha y hora actual llamando a `getFechaActual()` y le pide al préstamo seleccionado que se finalice mediante `finalizar()`, enviándole esa fecha por parámetro. A diferencia del análisis, el Gestor ya no recorre los estados buscando el estado Finalizado (se eliminan `buscarEstadoFinalizado()`, `sosAmbitoPrestamo()` y `sosFinalizado()`), porque de eso se encarga el estado actual del préstamo.

**getFechaActual():** Obtiene la fecha y hora actual del sistema y la guarda en el atributo `fechaActual`.

**Prestamo (Contexto)**

**finalizar(fechaHoraActual: DateTime):** El préstamo no decide por sí mismo cómo finalizarse, sino que delega en su estado actual llamando a `finalizar()` del objeto apuntado por `estadoActual`. Le envía por parámetro a sí mismo (`this`), la fecha y hora actual y su historial de cambios de estado, porque el estado no conoce al préstamo y necesita esos datos para realizar el cambio.

**setEstadoActual(estado: Estado):** Cambia el puntero `estadoActual` del préstamo al nuevo estado recibido por parámetro. A partir de este momento el préstamo se comporta según el estado Finalizado.

**setCambioEstado(cambioEstado: CambioEstado):** Agrega el nuevo cambio de estado recibido por parámetro al historial de cambios de estado del préstamo.

**Estado (clase abstracta)**

**finalizar(prestamo: Prestamo, fechaHoraActual: DateTime, historial: CambioEstado[]):** Método polimórfico declarado para todos los estados. En la clase abstracta tiene un comportamiento básico por defecto, que es no realizar ninguna acción (criterio de diseño asumido). Así lo heredan los estados desde los cuales no se puede finalizar el préstamo (Creado, Rechazado, Finalizado y ConAccionJudicial). Solo lo redefinen AlDia y EnMora, que son los estados que tienen la transición a Finalizado.

**AlDia (EstadoConcreto)**

**finalizar(prestamo: Prestamo, fechaHoraActual: DateTime, historial: CambioEstado[]):** Redefine el método polimórfico de Estado para implementar la transición AlDia → Finalizado. Como finalizar no tiene otro comportamiento además del cambio de estado, llama a `cambiarEstado()` reenviando el préstamo, la fecha y hora actual y el historial.

**cambiarEstado(prestamo: Prestamo, fechaHoraActual: DateTime, historial: CambioEstado[]):** Realiza el cambio de estado del préstamo. Primero recorre en un loop el historial recibido por parámetro preguntando a cada cambio de estado si es el actual mediante `sosActual()`; al encontrarlo, le asigna la fecha y hora de fin con `setFechaHoraFin()`. Luego llama a `crearEstado()` para crear el estado Finalizado y a `crearCambioEstado()` para crear el nuevo cambio de estado con ese estado. Por último, le asigna al préstamo el nuevo estado mediante `setEstadoActual()` y le agrega el nuevo cambio de estado mediante `setCambioEstado()`.

**crearEstado():** Crea y retorna una instancia del estado Finalizado. Como el estado AlDia conoce cuál es su estado siguiente para esta transición, lo crea directamente en lugar de buscarlo entre todos los estados, como se hacía en el análisis.

**crearCambioEstado(fechaHoraActual: DateTime, estado: Estado):** Crea y retorna un nuevo CambioEstado, enviándole por parámetro la fecha y hora actual como fecha de inicio y el nuevo estado.

**Finalizado (EstadoConcreto)**

**new():** Crea la instancia del estado Finalizado, sin datos iniciales.

**CambioEstado**

**sosActual():** Indica si el cambio de estado es el vigente, retornando verdadero cuando no tiene fecha y hora de fin asignada.

**setFechaHoraFin(fechaHoraFin: DateTime):** Asigna la fecha y hora de fin recibida por parámetro, cerrando el cambio de estado vigente.

**new(fechaHoraInicio: DateTime, estado: Estado):** Crea un nuevo cambio de estado con la fecha y hora de inicio y el estado recibidos por parámetro, sin fecha y hora de fin, por lo que queda como el cambio de estado actual.

La aplicación del patrón permite que el comportamiento del préstamo varíe según la situación en la que se encuentra sin usar condicionales: cada estado concreto sabe qué transiciones son válidas desde él y cuál es su estado siguiente, y el préstamo solo delega en su estado actual. Los objetos estado no guardan referencia al préstamo, por eso se envía el préstamo (`this`) y su historial por parámetro en cada método polimórfico.
