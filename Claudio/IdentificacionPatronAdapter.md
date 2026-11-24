# Entrega 3 – Rediseño de la realización del CU 36 aplicando el patrón Adapter

## 1. Identificación del patrón implementado

**Patrón:** Adapter (Adaptador), patrón estructural de Gamma, de ámbito de objetos.

**Propósito:** convertir la interfaz de un objeto para que otro pueda comprenderla. El adaptador *envuelve* al objeto adaptado y oculta la complejidad de la conversión.

### Problema que resuelve

En el CU 36 *Consultar seguimiento de bolsines* (pasos 4 y 5), el sistema le pide a cada GPS Tracker la ubicación de los bolsines en estado Enviado. Según la Observación 1 del CU, hay tres modelos de GPS Tracker y cada uno expone una interfaz diferente:

| Modelo | Endpoint | Parámetros | Retorno |
|---|---|---|---|
| XTR-4500L | `getBolsinLocation()` | API Key, nro. de bolsín (int), código CM **origen** (String) | JSON en una cadena de caracteres (para cada ítem: nro. de bolsín, latitud, longitud y fecha y hora de la última actualización) |
| NavTrack QX-7A | `retrieveTrackingData()` | API Key, nro. de bolsín (int), código CM **destino** (String) | un único String con ítems separados por comas (nro. de bolsín, latitud, longitud y fecha y hora del último reporte) |
| GeoPulse MTR-900 | `fetchCargoPositions()` | API Key, nro. de bolsín (int) | matriz (nro. de bolsín, latitud, longitud y fecha y hora de la última actualización) |

Los tres endpoints difieren en nombre, parámetros y formato de respuesta, pero el sistema siempre necesita lo mismo: el número de bolsín, la latitud, la longitud y la fecha y hora de la última actualización.

En el análisis, la `InterfazGPSTracker` se comunicaba directamente con el actor GPS Tracker. Resolver los tres modelos ahí obligaría a llenarla de condicionales por modelo, mezclando en una sola clase la lógica de comunicación y de conversión de los tres formatos. Eso da baja cohesión, alto acoplamiento con cada proveedor y obliga a modificar la clase cada vez que se incorpore un modelo nuevo.

### Justificación

Con el patrón Adapter:

- La `InterfazGPSTracker` (**Cliente**) trabaja siempre contra una única interfaz, `IAdaptadorGPSTracker` (**Target**), con un único método polimórfico `obtenerUbicacionBolsin()`.
- Cada modelo tiene su **Adaptador concreto** (`AdaptadorXTR4500L`, `AdaptadorNavTrackQX7A` y `AdaptadorGeoPulseMTR900`). Cada uno envuelve a su servicio externo (**Adaptable**): le traduce el pedido a los parámetros que espera, interpreta la respuesta (JSON, CSV o matriz) y siempre devuelve el mismo formato, `String[]` = {numeroBolsin, latitud, longitud, fechaHoraUltimaActualizacion}.
- Para agregar un nuevo modelo de GPS Tracker solo hay que crear un nuevo adaptador, sin modificar el Gestor ni la lógica existente (principio abierto/cerrado).
- Esta solución es coherente con la arquitectura de la Entrega 2: la lógica de conversión queda dentro del microservicio genérico `ServicioIntegracionGPSTracker` y no dentro de un servicio core (RNF 6, Dispositivo GPS Tracker).

### Participantes

| Rol en el patrón | Clase |
|---|---|
| Cliente | `InterfazGPSTracker` |
| Target (interfaz objetivo) | `IAdaptadorGPSTracker` |
| Adapter | `AdaptadorXTR4500L`, `AdaptadorNavTrackQX7A`, `AdaptadorGeoPulseMTR900` |
| Adaptee (adaptable) | `ServicioXTR4500L`, `ServicioNavTrackQX7A`, `ServicioGeoPulseMTR900` |

### Método de enganche

El patrón se engancha en el método **`buscarUbicacionBolsines()`** del `GestorSegBolsines` (mensaje 25 del diagrama de secuencia de análisis). Los mensajes 01 a 24 y todo lo que sigue desde `obtenerMapaBolsines()` no cambian, así que no se vuelven a diagramar: en el diagrama de secuencia de diseño figura una nota indicando que la secuencia sigue como en el análisis.

## 2. Especificación de la estructura resultante

### GestorSegBolsines «control» (no cambia, solo se detalla la firma)

| Visibilidad | Atributo / Método | Descripción |
|---|---|---|
| - | `codigoCMUsuarioLogueado: String` | Código de la CM del usuario logueado (CM origen de los bolsines). |
| - | `numerosBolsines: int[]` | Números de los bolsines en estado Enviado. |
| - | `codigosCMDestino: String[]` | Código de la CM destino de cada bolsín. |
| - | `modelosGPSBolsines: String[]` | Modelo del DispositivoGPS de cada bolsín. |
| - | `ubicacionesBolsines: String[][]` | Una fila por bolsín: {numeroBolsin, latitud, longitud, fechaHoraUltimaActualizacion}. |
| + | `buscarUbicacionBolsines(): void` | **Método de enganche.** Para cada bolsín en estado Enviado invoca `obtenerUbicacionBolsin()` de la InterfazGPSTracker, enviándole el número de bolsín, el código de la CM origen, el código de la CM destino y el modelo de GPS. Agrega cada ubicación retornada a `ubicacionesBolsines`. No sabe qué modelo de GPS hay detrás ni en qué formato responde. |

### InterfazGPSTracker «boundary» – Cliente

| Visibilidad | Atributo / Método | Descripción |
|---|---|---|
| - | `apiKeysModelosGPS: String[][]` | Una fila por modelo: {modelo, apiKey}. |
| - | `adaptadorGPS: IAdaptadorGPSTracker` | Adaptador concreto que se está usando. |
| - | `ubicacionObtenida: String[]` | Ubicación adaptada que retorna al Gestor. |
| + | `obtenerUbicacionBolsin(numeroBolsin: int, codigoCMOrigen: String, codigoCMDestino: String, modeloGPS: String): String[]` | Llama a `crearAdaptador()` y luego delega en el método polimórfico `obtenerUbicacionBolsin()` del adaptador creado. Retorna la ubicación en el formato común. |
| - | `crearAdaptador(modeloGPS: String): IAdaptadorGPSTracker` | Obtiene la API Key del modelo con `obtenerApiKey()` y, según el modelo, crea `AdaptadorXTR4500L`, `AdaptadorNavTrackQX7A` o `AdaptadorGeoPulseMTR900` con `new(apiKey)`. Lo retorna como `IAdaptadorGPSTracker`. En el escenario crea el AdaptadorXTR4500L. |
| - | `obtenerApiKey(modeloGPS: String): String` | Busca en `apiKeysModelosGPS` la API Key del modelo. |

### IAdaptadorGPSTracker «interface» – Target

| Visibilidad | Método | Descripción |
|---|---|---|
| + | `obtenerUbicacionBolsin(numeroBolsin: int, codigoCMOrigen: String, codigoCMDestino: String): String[]` | Método polimórfico. Recibe todos los datos que puede necesitar cualquier modelo y retorna siempre {numeroBolsin, latitud, longitud, fechaHoraUltimaActualizacion}. |

### AdaptadorXTR4500L «adapter» (se ejecuta en el escenario)

| Visibilidad | Atributo / Método | Descripción |
|---|---|---|
| - | `apiKey: String` | API Key identificatoria del modelo. |
| - | `numeroBolsin: int` | Bolsín consultado. |
| - | `codigoCMOrigen: String` | CM origen, que es el dato que exige este modelo. |
| - | `ubicacionRecibida: String` | JSON en cadena de caracteres recibido del servicio. |
| - | `ubicacionAdaptada: String[]` | Ubicación en el formato común. |
| + | `new(apiKey: String): AdaptadorXTR4500L` | Crea el adaptador y guarda la API Key. |
| + | `obtenerUbicacionBolsin(numeroBolsin: int, codigoCMOrigen: String, codigoCMDestino: String): String[]` | Implementa el método polimórfico: guarda el número de bolsín y el código de la CM origen (no usa la CM destino), llama a `solicitarUbicacion()` y a `adaptarUbicacion()` y retorna `ubicacionAdaptada`. |
| - | `solicitarUbicacion(): void` | Invoca `getBolsinLocation(apiKey, numeroBolsin, codigoCMOrigen)` del ServicioXTR4500L y guarda el JSON en `ubicacionRecibida`. |
| - | `adaptarUbicacion(): void` | Interpreta el JSON. Si contiene varios ítems, toma el de fecha y hora de actualización más reciente y arma {numeroBolsin, latitud, longitud, fechaHoraUltimaActualizacion}. |

### AdaptadorNavTrackQX7A «adapter»

| Visibilidad | Atributo / Método | Descripción |
|---|---|---|
| - | `apiKey: String`, `numeroBolsin: int`, `codigoCMDestino: String` | Datos que exige este modelo (usa la CM **destino**). |
| - | `ubicacionRecibida: String` | String con ítems separados por comas. |
| - | `ubicacionAdaptada: String[]` | Ubicación en el formato común. |
| + | `new(apiKey: String): AdaptadorNavTrackQX7A` | Crea el adaptador. |
| + | `obtenerUbicacionBolsin(...): String[]` | Guarda el número de bolsín y el código de la CM destino (no usa la CM origen), llama a `solicitarUbicacion()` y a `adaptarUbicacion()`. |
| - | `solicitarUbicacion(): void` | Invoca `retrieveTrackingData(apiKey, numeroBolsin, codigoCMDestino)`. |
| - | `adaptarUbicacion(): void` | Separa el String por comas y lo ordena en el formato común. |

### AdaptadorGeoPulseMTR900 «adapter»

| Visibilidad | Atributo / Método | Descripción |
|---|---|---|
| - | `apiKey: String`, `numeroBolsin: int` | Este modelo no necesita códigos de CM. |
| - | `ubicacionRecibida: String[][]` | Matriz recibida. |
| - | `ubicacionAdaptada: String[]` | Ubicación en el formato común. |
| + | `new(apiKey: String): AdaptadorGeoPulseMTR900` | Crea el adaptador. |
| + | `obtenerUbicacionBolsin(...): String[]` | Guarda el número de bolsín (ignora ambos códigos de CM), llama a `solicitarUbicacion()` y a `adaptarUbicacion()`. |
| - | `solicitarUbicacion(): void` | Invoca `fetchCargoPositions(apiKey, numeroBolsin)`. |
| - | `adaptarUbicacion(): void` | Recorre la matriz, toma la fila con la fecha y hora de actualización más reciente y la convierte al formato común. |

### Servicios externos «adaptable»

| Clase | Método |
|---|---|
| ServicioXTR4500L | `+ getBolsinLocation(apiKey: String, numeroBolsin: int, codigoCMOrigen: String): String` |
| ServicioNavTrackQX7A | `+ retrieveTrackingData(apiKey: String, numeroBolsin: int, codigoCMDestino: String): String` |
| ServicioGeoPulseMTR900 | `+ fetchCargoPositions(apiKey: String, numeroBolsin: int): String[][]` |

> **Visibilidad:** la interfaz `IAdaptadorGPSTracker` expone solo el método público que usa el cliente. `solicitarUbicacion()` y `adaptarUbicacion()` son pasos internos de cada adaptador, por eso son privados (cada adaptador los implementa a su manera). Lo mismo pasa con `crearAdaptador()` y `obtenerApiKey()` en la InterfazGPSTracker.

## 3. Vista dinámica

Ver `DiagramaSecuenciaAdapter.plantuml`. Escenario: bolsines con GPS Tracker modelo **XTR-4500L** (flujo descripto del CU 36). Mantiene la numeración del análisis desde el mensaje 25 (`buscarUbicacionBolsines()`).

## 4. Vista estática

Ver `DiagramaClasesAdapter.plantuml`.
