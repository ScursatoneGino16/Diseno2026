# Resumen – 10 Diseño Arquitectónico: Estilos (2026)

## 1. Conceptos base

- **Estilo arquitectónico:** define **cómo se organizan los componentes y cómo interactúan**. Sirve para nombrar un diseño recurrente. **No determina tecnología ni implementación.** Se elige según las necesidades: escalabilidad, mantenibilidad, rendimiento, integración.
- **Patrón arquitectónico:** organización estructural con subsistemas predefinidos, sus responsabilidades y las reglas de relación entre ellos. Resuelve problemas recurrentes. **Es de menor escala que un estilo:** un diseño puede tener varios patrones, pero suele tener **un estilo dominante**.
- **Platónicos vs. embebidos:** los platónicos son ideales, los de los libros. Los embebidos son los de los sistemas reales: violan las reglas estrictas, generalmente a cambio de alguna compensación.
- **Big Ball of Mud (arquitectura espagueti):** lo que queda cuando **no se define una arquitectura**. Alto acoplamiento, mantenimiento muy difícil, imposible de explicar a alguien nuevo.

## 2. Taxonomía (la pregunta que responde cada categoría)

| Categoría | Pregunta | Estilos vistos |
|---|---|---|
| **Descomposición** | ¿En cuántas aplicaciones separo el sistema? (escalabilidad organizacional y técnica) | Monolítico, Microservicios |
| **Estructura interna** | ¿Cómo organizo el código por dentro? (mantenibilidad) | Layered, Hexagonal |
| **Distribución** | ¿Cómo se organizan físicamente los nodos? ¿Quién inicia la comunicación? | Cliente/Servidor, N-Tier |
| **Comunicación** | ¿Cómo fluye la información entre componentes? | EDA (Pub/Sub, Broker, Messaging, Process Coordinator), Request-Response / REST |
| Procesamiento | ¿Cómo se transforma la información dentro de un componente? | (no se desarrolla) |

> **Los estilos se combinan:** cada uno resuelve un problema distinto en un nivel distinto. **Las arquitecturas reales son híbridas.** El *framework arquitectónico* es justamente la combinación de un estilo de cada categoría.

## 3. Descomposición

### Monolítico
Aplicación **autosuficiente**: todos los módulos se empaquetan y se despliegan **como uno solo**. Su rasgo principal es la **independencia** (no depende de nada externo). Por dentro puede estar dividido en módulos. Sus únicas APIs suelen ser UI↔servidor, servidor↔BD y servidor↔dependencias externas.

| ✅ Pros                                                          | ❌ Contras                                          |
| --------------------------------------------------------------- | -------------------------------------------------- |
| Fácil de desarrollar desde cero y de probar                     | Si falla, falla todo                               |
| Escala simple con un balanceador (replicando todo)              | Cualquier cambio obliga a redesplegar todo         |
| Pocos puntos de falla, autónomo                                 | Al crecer, se vuelve difícil de mantener y depurar |
| **Performance:** procesamiento local, sin llamadas distribuidas | Escalar un módulo implica escalar toda la app      |
|                                                                 | Anclado a un stack tecnológico                     |

### Microservicios (MSA)
Surge en un workshop en Venecia (2011); el nombre se adopta en 2012. Ejemplos: Netflix (unos 700 microservicios), Amazon, Spotify, Uber.
- Servicios **pequeños, independientes y poco acoplados**. Cada uno hace **una tarea** con **su propia BD**: acepta datos redundantes a cambio de bajo acoplamiento.
- Se comunican por **API**, típicamente REST y a menudo a través de un API Gateway. Cada uno puede usar un **lenguaje y una BD distintos** (persistencia políglota).
- Características:
  - organizados por **capacidades de negocio**
  - **"productos, no proyectos"** ("you build it, you run it")
  - **gestión de datos descentralizada**
- Ventajas:
  - escalabilidad y despliegue fácil
  - agilidad y testeabilidad
  - reusabilidad e interoperabilidad
- El **tamaño no forma parte de la definición**: hay microservicios que ocupan a un equipo entero y otros de unos cientos de líneas.

| Aspecto | Microservicios | Monolítico |
|---|---|---|
| Implementación | Difícil (sistema distribuido) | Fácil |
| Despliegue | Fácil e independiente (CI/CD) | Difícil, siempre completo |
| Escalabilidad | Por servicio | Toda la app |
| Administración | Descentralizada | Centralizada |
| Orientación a negocio | Sí | Levemente |

**¿Cuándo usar microservicios?**
1. Para evolucionar con cambios pequeños: **empezar monolítico** y extraer un módulo a la vez.
2. Cuando hace falta **despliegue independiente sin tiempo de inactividad**.
3. Cuando se necesitan **distintas tecnologías de BD** para particionar los datos.
4. Cuando se busca **autonomía de equipos**.

## 4. Estructura interna

### Layered (en capas)
Es el estilo más común. Cada capa tiene una responsabilidad:
- **Presentación**
- **Lógica de negocio**, dividida en aplicación (reglas y procesos) y dominio (conceptos y datos de negocio)
- **Acceso a datos**
- **Infraestructura**

En la práctica (forma embebida) se saltean capas, y eso anula los beneficios del estilo.

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| Fácil de desarrollar (muy conocido) | **Performance:** pasar de capa en capa por la red degrada |
| Fácil de probar (capa por capa) | **Escalabilidad:** escala de forma monolítica |
| Fácil de mantener (se ubica rápido el defecto) | **Despliegue:** de abajo hacia arriba; un cambio puede exigir redesplegar todo |
| **Seguridad:** las capas pueden ir en subredes aisladas | **Tolerancia a fallos:** si cae una capa, caen en cascada las superiores |
| | Anclado a un stack |

**Tipos de partición:**
- **Técnica:** por capas (presentación, negocio, datos).
- **De dominio:** por flujos de trabajo o dominios. Modela mejor el negocio, se alinea con el monolito modular y los microservicios, y es fácil de migrar a una arquitectura distribuida. Su desventaja es que el código de personalización queda repartido en varios lugares.

### Hexagonal (Puertos y Adaptadores, Alistair Cockburn)
- **Núcleo (hexágono):** la lógica de negocio, aislada de la tecnología.
- **Puertos:** **interfaces** definidas por el negocio, independientes de la tecnología. Son la entrada y salida del núcleo.
- **Adaptadores:** código externo que conecta un puerto con una tecnología concreta (por ejemplo, un controlador REST o un repositorio de BD). Puede haber **varios adaptadores por puerto**.
- **Lado conductor (izquierdo, primario):** los actores que **inician** la interacción, como UI o controladores. El adaptador **usa** el puerto y un servicio de aplicación lo implementa.
- **Lado conducido (derecho, secundario):** lo que la aplicación "impulsa", como la BD o servicios externos. El servicio usa el puerto y el **adaptador lo implementa**, quedando fuera del hexágono.
- **Inversión de dependencias:** todas las dependencias del código **apuntan hacia el núcleo** (regla de dependencia). En el lado conducido, la dependencia va **en sentido opuesto a la llamada**.
- **Beneficios:**
  - bajo acoplamiento y alta cohesión
  - **testeabilidad** (se mockean los adaptadores)
  - mantenibilidad e **intercambiabilidad** (cambiar la BD o el canal = un adaptador nuevo)
  - combina bien con **DDD**
- **Cuándo usarla:**
  - cuando hay que integrarse con **múltiples sistemas externos**
  - con gran volumen de datos
  - cuando hay que escalar horizontalmente
  - cuando hay que probar exhaustivamente

**Layered vs. Hexagonal:**
- **Layered** está centrada en la **BD** (*database-driven design*): genera dependencias transitivas, límites borrosos, posible duplicación de lógica y es difícil intercambiar infraestructura.
- **Hexagonal** pone **primero la lógica de negocio** y la persistencia después. El negocio guía a la persistencia, no al revés, y queda claro dónde va cada componente externo (APIs, terceros).

## 5. Distribución

### Cliente/Servidor
- Los **clientes inician** la comunicación y conocen a los servidores. **Los servidores no conocen a los clientes** hasta que estos los contactan. Los servidores se pueden replicar, lo que sirve cuando la carga es variable.
- Variantes:
  - two-tier y three-tier / N-tier
  - master-slave
  - peer-to-peer
- Formas de implementarlo:
  - **acceso a datos remoto**
  - **presentación remota**
  - **lógica dividida**: la más compleja, porque hay programas separados en cliente y servidor
- Evolución histórica:
  1. escritorio + servidor de BD
  2. navegador + servidor web
  3. tres niveles: front, servidor de aplicaciones y servidor de BD

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| **Centralización** (los datos no quedan desactualizados) | Actualizar clientes pesados |
| **Seguridad** (el servidor detrás de un firewall) | Concurrencia inesperada en el servidor |
| Cliente fácil de instalar | **Todo o nada:** si cae el servidor, caen todos los clientes |
| Separación de responsabilidades | Protocolos de bajo nivel (sockets, RPC) |
| Portabilidad | Depuración difícil (clientes distribuidos) |

### N-Tier
- **Separación de intereses** en niveles: presentación, negocio y datos (EIS).
- **Comunicación sincrónica pedido-respuesta**, en una sola dirección (cliente → web → negocio → EIS). Cada nivel espera la respuesta del siguiente.
- **Distribución flexible:** los niveles pueden correr en la misma máquina o en máquinas distintas.

## 6. Comunicación

### EDA – Event Driven Architecture
Arquitectura **asincrónica y distribuida**, pensada para escalar mucho. Un **evento** es un cambio significativo de estado. Los componentes lanzan eventos y otros reaccionan a ellos, posiblemente generando nuevos eventos.

Hay dos topologías:
- **Broker:** **sin control central**. Cada procesador emite eventos que disparan a otros, formando una cadena.
- **Mediator:** **con control central**. El mediador decide qué procesadores se llaman y en qué orden. Puede haber varios mediadores, en paralelo o en jerarquía.

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| **Escalabilidad:** cada consumidor escala por separado | **Testeabilidad:** hay que esperar toda la cadena de eventos |
| **Despliegue** sin dependencias | **Desarrollo:** lo asincrónico es complejo; hay que manejar errores y riesgo de inconsistencia |
| **Performance:** cada paso agrega latencia, pero se compensa con paralelismo | |
| **Flexibilidad:** cada procesador tiene una sola responsabilidad; lo nuevo es un procesador nuevo | |

**Variantes de EDA:**

| Estilo | Idea clave | Usar cuando… |
|---|---|---|
| **Publish & Subscribe** | Publicantes → **tópico** → suscriptores. Es **muchos a muchos** y nadie conoce al otro. Suscripción dinámica, QoS configurable, el bus puede ser durable, ordenado o por lotes | Hay **notificaciones** asincrónicas uno-a-muchos o muchos-a-muchos |
| **Broker (Agente)** | **Hub-and-spoke:** el agente **rutea y transforma** mensajes entre puertos de entrada y salida | Hay **integración** entre sistemas con **formatos distintos** que requieren grandes transformaciones |
| **Messaging** | Emisor → **cola** → receptor. Es asincrónico: el emisor sigue sin esperar y el mensaje **se entrega eventualmente** aunque el receptor esté caído | La **conectividad es transitoria** o los servidores son poco confiables |
| **Process Coordinator** | Un **coordinador** encapsula la secuencia de pasos de un proceso de negocio y llama a los servidores en orden. Puede ser sincrónico o asincrónico | Hay **procesos de negocio complejos** con varios servicios; se gana un solo punto para cambiar y monitorear |

### Request–Response / REST
- **Request-response:** el cliente envía una solicitud y el servidor responde.
- **REST:** conjunto de **restricciones arquitectónicas** para sistemas distribuidos. Es la alternativa a SOAP/WSDL. Sus elementos son datos (recursos), **conectores** (interfaces abstractas) y componentes (clientes, servidores e intermediarios).
- **Principios:**
  - **sin estado:** cada request lleva toda la información
  - **cliente-servidor**
  - **interfaz uniforme**
  - **cacheable**
  - **en capas**
  - **código bajo demanda** (opcional)
- **Ventajas:**
  - interoperable (cualquier lenguaje)
  - **escalable**, porque al no tener estado se pueden agregar nodos
  - bajo acoplamiento
  - testeable
  - reutilizable
- **Desventajas:**
  - **performance**: latencia, conexión y autenticación en cada request
  - **más puntos de falla**
  - necesita **gobierno** de servicios para evitar duplicados

## 7. Framework arquitectónico – ejemplo "Fanáticos del Fútbol"

Elegir el framework es parte de **Diseñar la arquitectura**, dentro del proceso: Requerimientos → **Diseño** (elegir framework + distribuir componentes) → Validación.

| Categoría | Versión Microservicios | Versión Monolítica |
|---|---|---|
| Descomposición | Microservicios | Monolítico |
| Estructura interna | **Hexagonal** | **Layered – partición técnica** |
| Distribución | C/S N-Tier | C/S N-Tier |
| Comunicación | **REST** + EDA **Broker** (importación minuto a minuto y cobros offline) + EDA **Pub/Sub** (notificaciones del partido y del club) | EDA **Broker** (ídem) + EDA **Pub/Sub** (ídem) |

> **Regla para el parcial:** elegí **un estilo por categoría** y justificá cada elección con un RNF o una frase del enunciado.
