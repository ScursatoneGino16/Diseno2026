# Tercer Parcial – Tema A – Punto C: Especificación del comportamiento

CU 3 "Registrar pago de cuota de un préstamo". Escenario: pago de la cuota de un préstamo en mora con recargo por mora porcentual y cobro por ente externo SENASA.

---

## 1.C – Patrón Adapter

**GestorRegistrarPagoCuota (Cliente del patrón)**

**validarPrestamoEnteExterno():** Método que dispara la ejecución del patrón. Le solicita al préstamo seleccionado el ente de cobro externo mediante `obtenerEnteCobroExterno()` y lo guarda en el atributo `enteCobroExterno`. Con esto valida que el préstamo esté marcado como cobro por ente externo y determina a qué central de deudores corresponde (en el escenario, SENASA).

**obtenerCodigoPagoEnteExterno():** Obtiene los datos necesarios para consultar a la central de deudores: el DNI del cliente logueado mediante `getDNI()`, y la fecha y hora de acreditación y el monto otorgado del préstamo seleccionado mediante `getFechaHoraAcreditacion()` y `getMontoOtorgado()`. Luego llama a `crearAdaptador()` para obtener el adaptador correspondiente al ente, e invoca su método polimórfico `obtenerCodigoPago()` enviándole esos datos por parámetro. El código de pago que retorna lo guarda en el atributo `codigoPagoEnteExterno`. El Gestor solo conoce la interfaz `IAdaptadorCentralDeudores`, no el formato de cada central.

**crearAdaptador():** Según el ente de cobro externo del préstamo, crea el adaptador concreto que corresponde: `AdaptadorSENASA` si el ente es SENASA o `AdaptadorAGIP` si es AGIP. En el escenario crea el AdaptadorSENASA. Lo retorna como `IAdaptadorCentralDeudores`.

**Prestamo**

**obtenerEnteCobroExterno():** Retorna el ente de cobro externo con el que está marcado el préstamo.

**getFechaHoraAcreditacion():** Retorna la fecha y hora de acreditación del préstamo.

**getMontoOtorgado():** Retorna el monto otorgado al cliente.

**Cliente**

**getDNI():** Retorna el DNI del cliente.

**AdaptadorSENASA (Adapter)**

**new():** Crea la instancia del adaptador. Tiene como atributo el nombre de la empresa (CreditIn), que es el dato con el que se filtra la respuesta de SENASA.

**obtenerCodigoPago():** Implementa el método polimórfico de `IAdaptadorCentralDeudores` que invoca el Gestor. Guarda en sus atributos el DNI del cliente, la fecha y hora de acreditación y el monto otorgado recibidos por parámetro. Luego llama a `solicitarHistorialPrestamos()`, a `adaptarHistorialPrestamos()` y a `filtrarPrestamoVigente()`, y retorna al Gestor el código de pago obtenido.

**solicitarHistorialPrestamos():** Método polimórfico que traduce el pedido al formato que entiende SENASA y desencadena la comunicación con el servicio externo: invoca el método `consultarDeuda()` del ServicioSENASA enviándole el DNI del cliente y la fecha hora de acreditación. El servicio retorna un JSON en formato cadena de caracteres con el historial de préstamos del cliente acreditados para esa fecha (monto original, fecha de acreditación, código de pago y nombre de la empresa prestadora), que se guarda en el atributo `historialRecibido`.

**adaptarHistorialPrestamos():** Método polimórfico que interpreta la cadena JSON recibida de SENASA y la adapta al formato que se necesita: una matriz de String donde cada fila es un préstamo del historial y cada columna uno de sus datos (monto original, fecha de acreditación, código de pago y nombre de la empresa), que se guarda en el atributo `historialAdaptado`.

**filtrarPrestamoVigente():** Método polimórfico que recorre las filas de la matriz del historial adaptado y busca el préstamo cuyo nombre de empresa prestadora coincide con el nombre de la empresa del adaptador (CreditIn) y cuyo monto original coincide con el monto otorgado. Guarda en el atributo `codigoPago` el código de pago de ese préstamo y lo retorna.

**ServicioSENASA (Adaptable)**

**consultarDeuda():** Servicio externo de SENASA. Recibe el DNI del cliente y una fecha hora, y devuelve el historial de préstamos acreditados en formato JSON.

**AdaptadorAGIP (Adapter, no se ejecuta en el escenario)**

**obtenerCodigoPago():** Igual que en AdaptadorSENASA: guarda los datos recibidos, llama a `solicitarHistorialPrestamos()`, `adaptarHistorialPrestamos()` y `filtrarPrestamoVigente()`, y retorna el código de pago.

**solicitarHistorialPrestamos():** Invoca `consultarAdeudamiento()` del ServicioAGIP enviándole el DNI del cliente y el código de empresa prestadora (atributo del adaptador). El servicio retorna un XML en formato cadena de caracteres con el historial de préstamos acreditados por la empresa a ese cliente (fecha de acreditación, monto final y código de pago).

**adaptarHistorialPrestamos():** Interpreta el XML recibido y lo convierte en una matriz de String donde cada fila es un préstamo y cada columna uno de sus datos (fecha de acreditación, monto final y código de pago).

**filtrarPrestamoVigente():** Busca en el historial adaptado el préstamo por fecha de acreditación y monto, y retorna su código de pago.

La aplicación del patrón permite que el Gestor obtenga el código de pago de cualquier central de deudores a través de una única interfaz, sin importar si responde en JSON o en XML. Cada adaptador envuelve a su servicio externo y, mediante los mismos métodos polimórficos, le traduce el pedido, interpreta la respuesta y devuelve siempre el mismo dato: el código de pago correspondiente al préstamo. Para agregar una nueva central solo hay que crear un nuevo adaptador.

---

## 2.C – Patrón Strategy

**GestorRegistrarPagoCuota (Contexto)**

**calcularMontoAAbonar():** Método que dispara la ejecución del patrón. Obtiene la fecha actual mediante `getFechaActual()` y le solicita al préstamo seleccionado su definición de mora mediante `obtenerDefinicionMora()`, guardándola en el atributo `definicionMoraPrestamo`. Luego llama a `crearEstrategia()`, a `calcularDiasAtraso()` y a `calcularRecargoAplicable()`. Por último calcula el monto a abonar sumando el monto de la cuota y el recargo obtenido.

**getFechaActual():** Obtiene la fecha y hora actual del sistema y la guarda en el atributo `fechaActual`.

**crearEstrategia():** Le pregunta a la definición de mora si define un cálculo porcentual mediante `esPorcentual()`. Si es así crea la `EstrategiaCalculoPorcentual`; de lo contrario crea la `EstrategiaCalculoFijo`. En el escenario crea la estrategia porcentual. La guarda en el atributo `estrategiaCalculoMora` y la retorna como `IEstrategiaCalculoMora`.

**calcularDiasAtraso():** Igual que en el análisis. Calcula la cantidad de días de atraso de la cuota como la diferencia entre la fecha actual y la fecha de vencimiento de la cuota, y la guarda en el atributo `diasRetraso`.

**calcularRecargoAplicable():** Delega el cálculo del recargo a la estrategia invocando el método polimórfico homónimo `calcularRecargoAplicable()` de la estrategia, enviándole por parámetro el monto base de la cuota, los días de atraso y la definición de mora. El resultado lo guarda en el atributo `montoRecargo`. A diferencia del análisis, ya no contiene el condicional que elegía la fórmula.

**Prestamo**

**obtenerDefinicionMora():** Le solicita a su tipo de préstamo la definición de mora mediante `obtenerDefinicionMora()` y la retorna.

**TipoPrestamo**

**obtenerDefinicionMora():** Retorna la definición de mora asociada al tipo de préstamo.

**DefinicionMora**

**esPorcentual():** Indica si la definición de mora establece un recargo porcentual, retornando verdadero cuando tiene definido un porcentaje de recargo.

**getPorcentajeRecargo():** Retorna el porcentaje de recargo diario.

**getMontoFijoRecargo():** Retorna el monto fijo de recargo por día de atraso (lo usa la estrategia de cálculo fijo).

**EstrategiaCalculoPorcentual (EstrategiaConcreta)**

**new():** Crea la instancia de la estrategia, sin datos iniciales.

**calcularRecargoAplicable():** Implementa el método polimórfico de `IEstrategiaCalculoMora`. Le solicita a la definición de mora recibida por parámetro el porcentaje de recargo mediante `getPorcentajeRecargo()` y luego llama a su método propio `calcularRecargoPorcentual()`, enviándole el monto base, el porcentaje obtenido y los días de atraso. Retorna al Gestor el recargo calculado.

**calcularRecargoPorcentual():** Método propio de esta estrategia. Aplica la fórmula del recargo porcentual: `recargo = montoBase * porcentajeRecargo * diasAtraso`, es decir, el porcentaje diario sobre el monto de la cuota acumulado por cada día de atraso. Retorna el recargo obtenido.

**EstrategiaCalculoFijo (EstrategiaConcreta, no se ejecuta en el escenario)**

**calcularRecargoAplicable():** Implementa el método polimórfico. Le solicita a la definición de mora el monto fijo mediante `getMontoFijoRecargo()` y calcula el recargo como `montoFijoRecargo * diasAtraso`. No utiliza el monto base, pero lo recibe porque la firma es común a todas las estrategias.

La aplicación del patrón encapsula cada fórmula de cálculo del recargo por mora en una estrategia concreta y elimina el condicional del Gestor. El Gestor (contexto) crea la estrategia según la definición de mora del tipo de préstamo y luego le delega el cálculo a través de la interfaz `IEstrategiaCalculoMora`. Para agregar una nueva forma de cálculo de mora solo hay que crear una nueva estrategia.
