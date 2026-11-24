 # Resumen conciso para el Parcial 2 – Diseño de Sistemas

---

## 1. Proceso de Diseño Arquitectónico (Ian Gorton)

**Diseño arquitectónico:** asignación de los requerimientos esenciales a una tecnología específica; es **"el plan del diseño"**. Según Gorton es un proceso **iterativo** de 3 actividades. De la validación se puede volver al diseño o a los requerimientos.

### 1) Determinar los requerimientos arquitectónicos
- **Entrada:** los RF (casos de uso) y RNF que salen del WF de Requerimientos.
- Se seleccionan los **RNF significativos para la arquitectura (SPA)** y después los **CU que los resuelven**.
- Se **priorizan**:
  - **Alto:** el sistema *debe* soportarlo. **Conduce el diseño** y se resuelve en la 1ª iteración.
  - **Medio:** se soportará más adelante, no necesariamente en el primer release.
  - **Bajo:** forma parte de la lista de deseos.
- Ojo: **los requerimientos entran en conflicto** (trade-offs, por ejemplo performance vs. mantenibilidad). Los requerimientos en conflicto deben tener la **misma prioridad**.
- **Salida:** el conjunto de requerimientos arquitectónicos priorizados.

### 2) Diseño arquitectónico
Define la **estructura** y las **responsabilidades de los componentes**. Tiene dos pasos:
- **a. Elegir el framework arquitectónico:** la combinación de estilos o patrones probados, uno por categoría (descomposición, estructura interna, distribución, comunicación). Usar soluciones conocidas **minimiza el riesgo**. Hay que **justificar cada elección** con los RNF.
- **b. Distribuir componentes:** identificar los componentes principales, sus **interfaces**, sus **responsabilidades**, sus **dependencias** y qué partes se distribuyen en distintos servidores.
  - Lineamientos: **bajo acoplamiento**, **alta cohesión**, aislar el middleware, descomponer jerárquicamente y minimizar las llamadas entre componentes.
- **Salida:** las **vistas arquitectónicas** y el **documento de arquitectura**.

### 3) Validación
"Probar" un diseño que **no se puede ejecutar**, recorriéndolo contra los requerimientos actuales y futuros. Hay dos técnicas:
- **Escenarios:** se plantea un **estímulo** sobre un atributo de calidad y se analiza la **respuesta** de la arquitectura. Ejemplo: si se duplica la carga, el servidor se escala a un cluster.
- **Prototipos:** versiones mínimas que prueban un aspecto **riesgoso**. Pueden ser una **prueba de concepto** (¿se puede construir así?) o una **prueba de tecnología** (¿el middleware o la librería se comporta como se espera?).

Si hay problemas, se corrige el diseño o se **renegocian** los RNF y se vuelve a empezar.

---

## 2. Modelo de vistas 4+1 (Kruchten, 1995)

Un solo diagrama no alcanza para describir la arquitectura, así que se usan **varias vistas concurrentes**. Cada vista atiende los intereses de un tipo de involucrado.
- **Vista:** proyección de un modelo para un involucrado, que omite lo irrelevante.
- **Punto de vista:** define cómo se construye y se lee esa vista.

| Vista (Kruchten)  | Equivalente PUD | Qué muestra                                                                                                           | Involucrado                            | Diagramas UML                                                           |
| ----------------- | --------------- | --------------------------------------------------------------------------------------------------------------------- | -------------------------------------- | ----------------------------------------------------------------------- |
| **Lógica**        | Diseño          | Funcionalidad para el usuario: clases, interfaces y colaboraciones del dominio (RF)                                   | Usuario final, analista                | **Clases**, **secuencia**, **comunicación** (componentes en la cátedra) |
| **De Procesos**   | Procesos        | **Concurrencia y sincronización**: procesos, hilos, clases activas. Cubre performance, escalabilidad y disponibilidad | Integradores                           | **Actividad**, **secuencia**, componentes (clases activas)              |
| **De Desarrollo** | Implementación  | Organización **estática del software**: módulos, subsistemas, capas, librerías y gestión de la configuración          | Programadores, líder técnico           | **Componentes**, **paquetes**                                           |
| **Física**        | Despliegue      | **Mapeo del software sobre el hardware**: nodos, topología, comunicaciones                                            | Ing. de sistemas, DBA, infraestructura | **Despliegue**                                                          |
| **+1 Escenarios** | Casos de Uso    | Los **CU significativos para la arquitectura**, que **unen y validan** las otras 4 vistas                             | Todos                                  | **Casos de uso**, secuencia / comunicación                              |

- La vista **+1** es redundante con las demás. Sirve para **descubrir** los elementos durante el diseño y para **validar** la arquitectura una vez diseñada.
- **No siempre hacen falta las 5:**
  - Con un solo nodo se omite la **física**.
  - Con un solo proceso se omite la de **procesos**.
  - Los **escenarios sirven siempre**.
- Cada vista puede usar un estilo distinto. Por ejemplo, la de desarrollo suele usar el estilo en capas.

---

## 3. Descomposición: Monolítico vs. Microservicios

### Monolítico
**Definición:** aplicación **autosuficiente** en la que todos los módulos se empaquetan y **se despliegan como una sola unidad**, compartiendo código, procesos, infraestructura y una base de datos. Su rasgo principal es la **independencia**.

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| Fácil de desarrollar desde cero | Si falla, **falla todo** |
| Fácil de probar | Un cambio exige **redesplegar toda la app** |
| **Mejor performance:** las llamadas son locales | Al crecer, es difícil de mantener y depurar |
| Pocos puntos de falla, autónomo | Escalar un módulo implica **escalar todo** |
| Escala simple: se replica detrás de un balanceador | **Anclado a un stack tecnológico** |

### Microservicios (MSA)
**Definición:** la aplicación se compone de **servicios pequeños, independientes y poco acoplados**. Cada servicio:
- cubre **una capacidad de negocio** y tiene **su propia BD**;
- se **despliega por separado**;
- se comunica con los demás mediante **APIs** (REST o eventos).

Es una evolución de SOA.

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| **Despliegue independiente** (CI/CD), sin parar todo | Es un **sistema distribuido complejo** (aplican las 8 falacias) |
| **Escalabilidad selectiva** por servicio | Peor **performance**: latencia de red entre servicios |
| **Aislamiento de fallas** | Datos redundantes y **consistencia eventual** |
| **Autonomía de equipos** ("you build it, you run it") | Testing e implementación del conjunto son difíciles |
| **Libertad tecnológica**: persistencia políglota | Requiere DevOps, monitoreo y gobierno |
| Fácil de mantener, reusable, interoperable | Más puntos de falla |

### Comparación

| Aspecto | Monolítico | Microservicios |
|---|---|---|
| Unidad de despliegue | Una sola | Una por servicio |
| Código, procesos e infraestructura | Compartidos | Independientes |
| Base de datos | Única, compartida | Una por servicio |
| Implementación inicial | **Fácil** | **Difícil** |
| Despliegue | Difícil (siempre completo) | **Fácil e independiente** |
| Escalabilidad | Toda la app | **Por servicio** |
| Fallas | En cascada (todo o nada) | Aisladas |
| Performance | **Mejor** (llamadas locales) | Peor (llamadas por red) |
| Tecnología | Un solo stack | Políglota |
| Administración | Centralizada | Descentralizada |
| Orientación al negocio | Leve | **Alta** |
| Ideal para | Equipo chico, dominio simple o nuevo, bajo presupuesto | Muchos equipos, escalado desigual, deploys frecuentes |

> **Regla:** empezar con un monolito y migrar a microservicios cuando hagan falta despliegue independiente, escalado desigual o autonomía de equipos.

---

## 4. Estructura interna: Layered vs. Hexagonal

### Layered (en capas)
**Definición:** organiza los componentes en **capas horizontales jerárquicas**:
- Presentación
- Lógica de negocio (aplicación + dominio)
- Acceso a datos
- Infraestructura

Cada capa tiene una responsabilidad y **solo usa a la capa inferior**. Es el estilo más común. La **partición** puede ser **técnica** (por capas) o **de dominio** (por flujos de negocio).

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| Fácil de desarrollar: es muy conocido | **Performance:** cada paso entre capas suma costo |
| Fácil de **probar** capa por capa | **Escalabilidad monolítica** |
| Fácil de **mantener**: se ubica rápido el defecto | **Despliegue** acoplado: un cambio exige redesplegar |
| **Seguridad:** las capas pueden ir en subredes aisladas | **Tolerancia a fallos:** si cae una capa, caen en cascada las superiores |
| | Centrada en la BD y anclada a un stack |

### Hexagonal (Puertos y Adaptadores)
**Definición:** el **núcleo de negocio** queda en el centro, aislado de la tecnología.
- Se comunica con el exterior solo a través de **puertos**: interfaces definidas por el negocio.
- Del otro lado de cada puerto hay **adaptadores**: implementaciones concretas, como REST, BD o colas.
- Se distingue un **lado conductor**, que inicia (UI, API), y un **lado conducido**, que es invocado (BD, servicios externos).
- Las dependencias **apuntan hacia el núcleo**, por inversión de dependencias.

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| **Bajo acoplamiento** entre el negocio y la tecnología | Más compleja: más interfaces y clases |
| **Alta testeabilidad**: los adaptadores se mockean | **Sobrediseño** para sistemas chicos o CRUD |
| **Intercambiabilidad**: cambiar la BD o el canal = un adaptador nuevo | Curva de aprendizaje mayor |
| Fácil de mantener y combina bien con **DDD** | Las indirecciones agregan algo de overhead |
| Ideal para integrarse con **muchos sistemas externos** | |

### Comparación

| Aspecto                 | Layered                                                | Hexagonal                                          |
| ----------------------- | ------------------------------------------------------ | -------------------------------------------------- |
| Forma                   | Capas apiladas (vertical)                              | Núcleo + puertos/adaptadores (adentro/afuera)      |
| Centro del diseño       | La **base de datos** (database-driven)                 | La **lógica de negocio**                           |
| Dependencias            | De arriba hacia abajo, hasta la BD                     | **Hacia el núcleo** (inversión de dependencias)    |
| Cambiar infraestructura | Difícil, porque impacta en las capas                   | Fácil: solo se cambia el adaptador                 |
| Testeo                  | Por capa, con dependencias transitivas                 | Núcleo aislado, mocks simples                      |
| Complejidad             | **Baja**, muy conocida                                 | Mayor                                              |
| Riesgo típico           | Límites borrosos, saltear capas, lógica duplicada      | Sobrediseño                                        |
| Ideal para              | Sistemas simples o medianos, equipos chicos, monolitos | Muchas integraciones externas, microservicios, DDD |

---

## 5. Distribución: Cliente/Servidor vs. N-Tier

### Cliente/Servidor
**Definición:** el sistema se divide en **servidores** que proveen servicios y **clientes** que los consumen.
- El **cliente inicia** la comunicación y conoce al servidor.
- **El servidor no conoce a los clientes** hasta que lo contactan.
- La forma clásica es de **2 capas** (front/back).
- Variantes: acceso a datos remoto, presentación remota y lógica dividida.

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| **Centralización** de datos: no quedan desactualizados | **Todo o nada:** si cae el servidor, caen todos los clientes |
| **Seguridad:** el servidor está detrás de un firewall | **Concurrencia:** el servidor puede saturarse |
| Cliente fácil de instalar | Actualizar los clientes pesados |
| Separación de responsabilidades | Protocolos de bajo nivel (sockets, RPC) |
| Portabilidad: cada parte en su plataforma | Depuración difícil (clientes distribuidos) |

### N-Tier
**Definición:** extiende C/S a **N niveles físicos o lógicos separados**, por ejemplo cliente → web → negocio → datos (EIS).
- La comunicación es **sincrónica pedido-respuesta** y va **en una sola dirección**: cada nivel espera al siguiente.
- La distribución es **flexible**: todos los niveles en una máquina o cada uno en la suya.

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| **Separación de intereses** por nivel | **Latencia:** más saltos de red |
| **Escalabilidad:** cada nivel se escala o replica por separado | Mayor complejidad de despliegue e infraestructura |
| **Seguridad:** los niveles van en subredes; la BD nunca queda expuesta | Lo sincrónico acopla en el tiempo: si un nivel cae o se demora, bloquea a los anteriores |
| **Mantenibilidad:** se cambia un nivel sin tocar los demás | Más puntos de falla y costo de hardware |
| Cliente liviano (navegador), sin instalar nada | |

### Comparación

| Aspecto           | Cliente/Servidor (2 capas)                 | N-Tier                                                  |
| ----------------- | ------------------------------------------ | ------------------------------------------------------- |
| Niveles           | 2 (cliente + servidor)                     | 3 o más (presentación, negocio, datos…)                 |
| Lógica de negocio | En el cliente (pesado) o en el servidor/BD | En un **nivel propio** (servidor de aplicaciones)       |
| Cliente           | Puede ser pesado y hay que actualizarlo    | Liviano (navegador)                                     |
| Escalabilidad     | Limitada por el servidor                   | **Por nivel**                                           |
| Seguridad         | Buena (servidor central)                   | **Mejor**: la BD queda aislada detrás de varios niveles |
| Performance       | Menos saltos                               | Más saltos de red                                       |
| Complejidad       | Baja                                       | Media/Alta                                              |
| Ideal para        | Sistemas chicos o de escritorio, LAN       | Apps web empresariales                                  |

> Ambos usan comunicación **sincrónica request-response, iniciada por el cliente**. **N-Tier es la generalización de C/S.**

---

## 6. Comunicación: Sincrónica (Request-Response / REST) vs. Asincrónica (EDA)

### Request-Response / REST
**Definición:**
- **Request-response:** el cliente envía una **solicitud** y **espera** la respuesta del servidor.
- **REST:** conjunto de **restricciones arquitectónicas** para sistemas distribuidos. Los recursos se acceden por URL con interfaz uniforme (HTTP). Es la alternativa a SOAP/WSDL.
- **Principios de REST:**
  - **sin estado**
  - cliente-servidor
  - interfaz uniforme
  - cacheable
  - en capas
  - código bajo demanda

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| **Interoperable**: cualquier lenguaje | **Performance:** latencia, conexión y autenticación en cada request |
| **Escalable**: al no tener estado, se agregan nodos | **Acoplamiento temporal**: si el servidor no responde, el cliente se bloquea o falla |
| Bajo acoplamiento (interfaces abstractas, URL) | **Más puntos de falla** |
| **Testeable**: cada recurso por separado | Necesita **gobierno** para evitar servicios duplicados |
| Reutilizable: el cliente decide cómo mostrar los datos | |

### EDA – Event Driven Architecture
**Definición:** arquitectura **asincrónica y distribuida**.
- Los componentes **emiten eventos**, es decir cambios significativos de estado.
- Otros componentes **reaccionan** a esos eventos y pueden generar nuevos eventos.
- Hay dos topologías:
  - **Broker:** sin control central; los eventos se encadenan.
  - **Mediator:** un mediador central decide qué procesadores se llaman y en qué orden.

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| **Escalabilidad:** cada consumidor escala por separado | **Testeabilidad** difícil: hay que esperar toda la cadena de eventos |
| **Muy bajo acoplamiento** | **Desarrollo** complejo: manejo de errores asincrónicos |
| **Despliegue** independiente | Riesgo de **inconsistencia** entre componentes |
| **Flexibilidad:** lo nuevo es un procesador nuevo | Cada paso agrega latencia (se compensa con paralelismo) |
| Tolerante a fallas: si cae un consumidor, el resto sigue | Flujo difícil de seguir y depurar |

### Variantes de EDA

**Publish & Subscribe**
- **Definición:** los publicantes emiten eventos a un **tópico** y todos los **suscriptores** del tópico los reciben. Es **muchos a muchos** y nadie conoce al otro.
- ✅ Bajo acoplamiento, suscripción dinámica, QoS configurable y fácil de extender.
- ❌ El bus agrega indirección y puede dañar la performance. El publicante no sabe quién recibió el evento.
- **Usar para:** **notificaciones** a muchos destinatarios.

**Broker (Agente)**
- **Definición:** un **concentrador** (hub-and-spoke) recibe mensajes por puertos de entrada, los **rutea** y los **transforma** al formato del receptor.
- ✅ Cada sistema usa su formato nativo y la lógica de transformación queda centralizada, fácil de modificar.
- ❌ El broker es un **punto único de falla** y un cuello de botella.
- **Usar para:** **integrar sistemas externos o heterogéneos** con formatos distintos.

**Messaging (Mensajería)**
- **Definición:** el emisor deja el mensaje en una **cola** y sigue sin esperar. El mensaje **se entrega eventualmente**, aunque el receptor o la red estén caídos.
- ✅ Bajo acoplamiento, **alta disponibilidad y tolerancia a fallas**, QoS configurable (confiable o rápido).
- ❌ Los cambios de formato del mensaje afectan a los emisores. No hay respuesta inmediata.
- **Usar para:** **conectividad intermitente**, procesos offline o servidores poco confiables.

**Process Coordinator**
- **Definición:** un **coordinador** encapsula la **secuencia de pasos** de un proceso de negocio y llama a los servidores en orden. La comunicación puede ser sincrónica o asincrónica.
- ✅ El proceso queda en un solo lugar, fácil de cambiar y **monitorear**. Los servidores no conocen el proceso.
- ❌ El coordinador es un punto central de falla y de complejidad.
- **Usar para:** **procesos de negocio complejos** que involucran varios servicios (orquestación).

### Comparación: Request-Response/REST vs. EDA

| Aspecto | Request-Response / REST | EDA |
|---|---|---|
| Sincronía | **Sincrónica**: el cliente espera | **Asincrónica**: se emite y se sigue |
| Acoplamiento | Medio: el cliente conoce al servidor | **Muy bajo**: emisor y receptor no se conocen |
| Relación | Uno a uno | Uno a muchos / muchos a muchos |
| Respuesta | Inmediata | Eventual (consistencia eventual) |
| Si cae el receptor | Falla la operación | El evento o mensaje queda en espera |
| Testeo y depuración | **Simple** | Complejo |
| Escalabilidad | Buena (sin estado) | **Muy alta** (consumidores independientes) |
| Ideal para | Consultas y operaciones que necesitan respuesta ya (CRUD, front ↔ API) | Notificaciones, integraciones, procesos largos u offline |

### Comparación entre las variantes de EDA

| Estilo | Intermediario | Relación | Clave | Caso típico |
|---|---|---|---|---|
| **Pub/Sub** | Tópico o bus | Muchos a muchos | Difusión de eventos | Notificar a usuarios o suscriptores |
| **Broker** | Agente (hub) | Muchos a muchos con ruteo | **Ruteo y transformación** de formatos | Integrar sistemas externos |
| **Messaging** | Cola | Punto a punto | **Entrega garantizada** aunque el receptor esté caído | Cobros offline, conexión intermitente |
| **Process Coordinator** | Coordinador | Uno (coordinador) a muchos (servidores) | **Orquesta los pasos** de un proceso | Procesar un pedido o una venta en varios pasos |

> **Ejemplo de la cátedra (Fanáticos del Fútbol):** REST para la app ↔ servidor; **EDA Broker** para importar los datos minuto a minuto y los cobros offline desde sistemas externos; **Pub/Sub** para notificar los eventos del partido y del club.
