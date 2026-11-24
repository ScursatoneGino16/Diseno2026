# Resumen – Parcial 2 Teórico · Diseño de Sistemas de Información (UTN FRC/FRVM)

> **Fuentes analizadas**
> - **Diapositivas de la cátedra**:
>   - *09 Diseño Arquitectónico parte 1*
>   - *10 Diseño Arquitectónico – Estilos 2026*
>   - *Arquitectura de Microservicios 2026*
>   - *RNF ISO 25000*
>   - *Repaso Diseño Arquitectónico*
>   - Caso *Fanáticos del Fútbol* (versión monolítica v1.15 y versión con microservicios v1.10)
> - **Papers/artículos**: Kruchten, *"4+1" View Model* (IEEE Software, 1995) y el artículo *Modelo 4+1 para Dummies* (jarroba).
> - **Bibliografía de referencia**, de donde salen las diapositivas:
>   - Gorton, *Essential Software Architecture*: atributos de calidad, proceso de arquitectura, patrones y documentación.
>   - Fairbanks, *Just Enough Software Architecture*, cap. 14: estilos.
>   - Blancarte, *Introducción a la arquitectura de software*: estilos, ventajas y desventajas.
>   - Richards & Ford, *Fundamentals of Software Architecture*: falacias, partición, Big Ball of Mud.
>   - Richardson, *Microservices Patterns*.
>   - Newman, *Building Microservices*.
>   - Raj et al., *Architectural Patterns*.
>
> - **Resúmenes de otros alumnos** (agregados en la v2 de este archivo):
>   - *RESUMEN ARQUITECTURA (1).pdf*: resumen corto con **preguntas de parciales anteriores** respondidas.
>   - *RESUMEN PARCIAL 2 DSI.pdf*: resumen 2021 de Zoy, Bolcato, Pairetti y Alvarez, UTN FRVM. Cubre la Unidad 2 (Diseño en el PUD) y la Unidad 3 (Arquitectura).
>   - *Resumen_2do_parcial_DSI.pdf*: manuscrito con **preguntas tipo parcial**, conceptos clave y ejemplos de aplicación de cada estilo.
>
> Las diapositivas de la cátedra son lo central del parcial. Los libros se usan para ampliar y precisar.
>
> 🆕 Las partes agregadas o corregidas a partir de los resúmenes están marcadas con **🆕**. Si un resumen contradice a las diapositivas 2026, manda la diapositiva, y se aclara en cada caso.

---

## Índice
1. [Diseño: contexto general](#1-diseño-contexto-general)
2. [Arquitectura: Rol, Proceso y Producto](#2-arquitectura-rol-proceso-y-producto)
3. [El Rol del Arquitecto](#3-el-rol-del-arquitecto)
4. [El Proceso de Diseño Arquitectónico](#4-el-proceso-de-diseño-arquitectónico)
5. [Requerimientos No Funcionales y su impacto arquitectónico](#5-requerimientos-no-funcionales-y-su-impacto-arquitectónico)
6. [Calidad: ISO/IEC 25000 / 25010](#6-calidad-isoiec-25000--25010)
7. [Diseño Arquitectónico: framework, componentes y vistas](#7-diseño-arquitectónico-framework-componentes-y-vistas)
8. [Modelo 4+1 de Kruchten y vistas del PUD](#8-modelo-41-de-kruchten-y-vistas-del-pud)
9. [Validación de la arquitectura](#9-validación-de-la-arquitectura)
10. [Modelado y documentación de la arquitectura](#10-modelado-y-documentación-de-la-arquitectura)
11. [Sistemas distribuidos y sus 8 falacias](#11-sistemas-distribuidos-y-sus-8-falacias)
12. [Estilos vs. Patrones arquitectónicos](#12-estilos-vs-patrones-arquitectónicos)
13. [Taxonomía de estilos (mapa mental de la cátedra)](#13-taxonomía-de-estilos-mapa-mental-de-la-cátedra)
14. [Descomposición: Monolítico vs Microservicios](#14-descomposición-monolítico-vs-microservicios)
15. [Estructura interna: Layered y Hexagonal](#15-estructura-interna-layered-y-hexagonal)
16. [Distribución: Cliente/Servidor y N-Tier](#16-distribución-clienteservidor-y-n-tier)
17. [Comunicación: EDA, Pub/Sub, Broker, Messaging, Process Coordinator, REST](#17-comunicación)
18. [Otros estilos (Fairbanks / Blancarte)](#18-otros-estilos-fairbanks--blancarte)
19. [Patrones para Microservicios (Richardson)](#19-patrones-para-microservicios-richardson)
20. [Caso de estudio: App Fanáticos del Fútbol](#20-caso-de-estudio-app-fanáticos-del-fútbol)
21. [Preguntas de repaso con respuestas](#21-preguntas-de-repaso-con-respuestas)
22. [Cuadros comparativos para memorizar](#22-cuadros-comparativos-para-memorizar)
23. [🆕 Preguntas de parciales anteriores (de los resúmenes)](#23--preguntas-de-parciales-anteriores)
24. [🆕 Anexo: Unidad 2, el Diseño en el PUD (resumen 2021)](#24--anexo-unidad-2-el-diseño-en-el-pud)

---

## 1. Diseño: contexto general

**¿Qué aspectos de un Sistema de Información se diseñan?**
- Datos
- Procesos
- Procedimientos manuales
- Formas de E/S
- Experiencia de usuario (UX)
- **Arquitectura**, que está en el centro y condiciona a todos los demás.

🆕 **¿Qué es diseñar?**
- Es un proceso iterativo que **transforma un modelo lógico** (el del WF de Análisis) **en un modelo físico**: lo baja a una tecnología y una implementación concretas, en un contexto de negocio dado.
- **En el diseño es donde se tienen en cuenta los RNF.**

🆕 **Análisis vs. Diseño** (resumen 2021, basado en el PUD):

| Modelo de Análisis                                                    | Modelo de Diseño                                                                            |
| --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **Conceptual**: modela la solución en términos **lógicos** (el "qué") | **Físico**: es un *plano de la implementación* y modela en términos **físicos** (el "cómo") |
| Genérico respecto al diseño, aplicable a varios diseños               | Específico para una implementación                                                          |
| Tres estereotipos de clases: control, entidad e interfaz (boundary)   | Cualquier cantidad de estereotipos, según el lenguaje                                       |
| Menos formal, más barato, menos capas                                 | Más formal, más caro, más capas                                                             |
| Es un bosquejo del diseño, incluida su arquitectura                   | Es el manifiesto del diseño, incluida su arquitectura                                       |
| Puede no mantenerse durante todo el ciclo de vida                     | **Debe mantenerse** durante todo el ciclo de vida                                           |
| Más diagramas de comunicación                                         | Más **diagramas de secuencia** (realizaciones de diseño)                                    |

**Descripción de cada aspecto que se diseña:**
- **Arquitectónico**: responde a los requerimientos de calidad con **decisiones estratégicas**. Es lo primero que se diseña y está en el centro, porque fija los lineamientos para el resto.
- **Datos**: transforma los requerimientos en las estructuras necesarias para la **persistencia** (en la cátedra, el mapeo a una BD relacional).
- **Procesos**: convierte las realizaciones de CU de análisis en **realizaciones de CU de diseño**, sumando los RNF y aplicando patrones de diseño.
- **Interacción humano-máquina**: no es solo diseñar interfaces, sino todas las interacciones entre el usuario y el sistema.
- **Formas de E/S**:
  - Los **sistemas en lote** necesitan diseñar formularios de entrada.
  - Los **sistemas en línea y de tiempo real** no los necesitan.
  - Todos los sistemas diseñan salidas.
- **Procedimientos manuales**: cómo se inserta el sistema en el proceso de negocio, incluidos los procesos alternativos (por ejemplo, qué pasa si se corta la luz).

> 🆕 **No confundir** el *proceso de diseño arquitectónico* (Gorton: 3 actividades) con el *Workflow de Diseño del PUD*. El WF de Diseño *incluye* la actividad de diseñar la arquitectura, pero no son lo mismo.

**Principios de buen diseño** (actividad de Verdadero/Falso del Repaso):

| Afirmación                                                       | V/F                                              |
| ---------------------------------------------------------------- | ------------------------------------------------ |
| El diseño debe inventar soluciones nuevas siempre que pueda      | **FALSO**: no hay que reinventar lo ya inventado |
| El diseño debe poder rastrearse hasta el modelo de análisis      | **VERDADERO**                                    |
| Un buen diseño maximiza la distancia intelectual con el problema | **FALSO**: la *minimiza*                         |
| El diseño debe estructurarse para admitir cambios                | **VERDADERO**                                    |

🆕 La lista completa de principios incluye uno más: **el diseño debe presentar uniformidad e integración**.

---

## 2. Arquitectura: Rol, Proceso y Producto

La palabra *"Arquitectura"* se aborda desde **tres miradas**:

| Mirada | Qué es | Ejemplos |
|---|---|---|
| **ROL** (arquitecto) | Quién la hace y qué responsabilidades tiene | Negociar requerimientos, comunicar la arquitectura |
| **PROCESO** | Cómo se hace | Determinar requerimientos → Diseñar → Validar |
| **PRODUCTO** (la arquitectura) | Lo que se obtiene | Documento de Arquitectura de Software, modelo de despliegue, vistas |

**El dominio de la arquitectura** (diagrama de la cátedra):
- **El "qué"**: cualidades de la arquitectura, representación, atributos de calidad del sistema.
- **El "porqué"**: requerimientos del sistema que la arquitectura satisface. También restringen la arquitectura.
- **El "quién"**: el arquitecto, con sus habilidades y los involucrados.
- **El "cómo"**: los procesos, la organización y la tecnología.

### Definiciones de Arquitectura de Software
- **IEEE 1471-2000**: "organización fundamental de un sistema, encarnada en sus componentes, sus relaciones entre sí y con el entorno, y los principios que rigen su diseño y evolución".
- **Bass, Clements y Kazman (SEI)**: "la estructura o estructuras del sistema, que comprenden elementos de software, las propiedades visibles externamente de esos elementos y las relaciones entre ellos".
- **RUP**: "conjunto de decisiones importantes sobre la organización de un sistema…".
- **Perry & Wolf (citado por Kruchten)**: *Arquitectura = {Elementos, Formas, Fundamento/Restricciones}*.
- **Blancarte**: "diseño de más alto nivel de la estructura de un sistema, que consiste en un conjunto de patrones y abstracciones que proporcionan un marco claro para la implementación".
- **Leyes de la arquitectura (Richards & Ford)**:
  - 1ª ley: *"Todo en arquitectura de software es un trade-off"*.
  - 2ª ley: *"El porqué es más importante que el cómo"*.

> 🆕 **Definición "de cátedra"** (la que aparece en los resúmenes y conviene usar en el parcial):
> **La arquitectura de software es el conjunto de DECISIONES SIGNIFICATIVAS que se toman sobre un producto para cumplir con los requerimientos.** Explica cómo se satisfacen los requerimientos funcionales y no funcionales: **principalmente resuelve los RNF, afectando lo menos posible a la funcionalidad** (los CU).
> 🆕 **Arquitectura ≠ diseño**: un diseño es una *instancia* de una arquitectura, como un objeto es instancia de una clase. A partir de una misma arquitectura pueden crearse muchos diseños.

🆕 **Áreas de los requerimientos no funcionales** (la "materia prima" de la arquitectura):
- **Restricciones técnicas**: imponen una tecnología, por ejemplo "debe usarse Oracle".
- **Restricciones de negocio**: en general no son negociables.
- **Atributos de calidad**: performance, escalabilidad, disponibilidad, usabilidad, portabilidad, etc.

🆕 **Dificultades de los RNF:**
- No hay reglas para saber cuándo se llegó a la solución óptima.
- Hay soluciones buenas y malas, no correctas e incorrectas.
- Un problema en un RNF puede ser síntoma de un problema mayor.
- Deben ser **objetivos y testeables (medibles)**.

**Ventajas de diseñar y documentar la arquitectura (Bass):**
1. Comunicación con los involucrados.
2. Análisis del sistema, porque permite analizar decisiones tempranas y saber si se cumplen los requerimientos críticos.
3. Reutilización a gran escala. 🆕 Esto permite las **arquitecturas de línea de producto**: una misma arquitectura se reutiliza en una familia de sistemas relacionados, y por eso documentarla es clave.

---

## 3. El Rol del Arquitecto

**Responsabilidades del arquitecto:**
- Revisa y **negocia los requerimientos**.
- **Documenta** la arquitectura.
- **Comunica** la arquitectura y se asegura de que los involucrados la comprendan.
- **Direcciona los requerimientos no funcionales** hacia la arquitectura.
- **Configura la arquitectura de hardware**.
- Se asegura de que la arquitectura **sea respetada**.
- Trabaja con el **Administrador del Proyecto** en la planificación, la estimación, la distribución de tareas y la calendarización.

**Ciclo de Negocio de la Arquitectura (Bass, Clements y Kazman):**
- Los conductores o fuerzas son la funcionalidad, las calidades, las consideraciones de negocio, la organización y su cultura, la tecnología, los procesos y las habilidades.
- Estas fuerzas **conducen** al arquitecto, que **produce** la arquitectura y, a partir de ella, el sistema.
- El sistema a su vez retroalimenta a la organización.

**Documentos que produce el arquitecto:**
- Descripción de la arquitectura / Documento de Arquitectura de Software
- Lineamientos de diseño
- Lineamientos de programación
- Modelo de diseño
- Modelo de implementación
- Modelo de despliegue
- Prototipo arquitectónico

**Comunica la arquitectura a involucrados con intereses distintos:**
| Involucrado | Interés |
|---|---|
| Usuario final | Funcionalidad, facilidad de uso, confiabilidad |
| Cliente | Precio, entrega a tiempo |
| Ventas y soporte de campo | Facilidad de integración, de personalización y de instalación |
| Jefe de desarrollo | Costos de desarrollo, habilidades del equipo |
| Desarrollador | Estructura de dependencias entre partes |
| Responsable de mantenimiento | Estabilidad, mantenibilidad, facilidad de introducir modificaciones, facilidad para diagnosticar problemas |
| Administrador del sistema | Performance, carga de trabajo, facilidad de prueba |

---

## 4. El Proceso de Diseño Arquitectónico

**Diseño arquitectónico** es la *asignación de los modelos de requerimientos esenciales a una tecnología específica*. Se lo define como **"el plan del diseño"**

**Arquitectura en contexto:**
1. Análisis de dominio, de requerimientos y de riesgos.
2. Diseño de la arquitectura de software, que se hace en paralelo con el diseño de la arquitectura de hardware. Ambos se restringen mutuamente y pueden modificar requerimientos.
3. Diseño detallado, codificación, integración y testing.

### Las 3 actividades del proceso (iterativo, según Gorton)
| # | Actividad | Qué implica |
|---|---|---|
| 1 | **Determinar los requerimientos arquitectónicos** | Crear un modelo de los requerimientos que conducirán el diseño arquitectónico, y priorizarlos |
| 2 | **Diseño arquitectónico** | Definir la **estructura** y las **responsabilidades de los componentes** que constituirán la arquitectura |
| 3 | **Validación** | "Probar" la arquitectura, normalmente recorriendo el diseño contra los requerimientos existentes y contra cualquier requerimiento futuro, posible o conocido |

Las tres actividades se repiten en un ciclo: de la validación se puede volver al diseño o a los requerimientos.

### Priorización de requerimientos arquitectónicos
Al inicio alcanza con tres categorías:
- **Alto**: la aplicación *debe* soportarlo. Son los que **conducen el diseño de la arquitectura**.
- **Medio**: habrá que soportarlo en algún momento, pero no necesariamente en el primer release.
- **Bajo**: forma parte de la lista de deseos. Es deseable, pero no conduce el diseño.

> **La priorización es engañosa** porque los requerimientos **entran en conflicto entre sí** (trade-offs). Por ejemplo, mejorar la performance perjudica la mantenibilidad.

🆕 **Detalles de cada actividad** (resúmenes):
- **Determinar los requerimientos arquitectónicos**:
  - **Entrada**: la salida del WF de Requerimientos, es decir los RNF y los CU.
  - Primero se seleccionan los **RNF significativos para la arquitectura (SPA)** según el contexto del proyecto. Después se buscan los **CU que los resuelven**.
  - **Salida**: el conjunto de requerimientos arquitectónicos **priorizados**.
  - Cómo se usan las prioridades en un proceso iterativo:
    - **Alto**: se resuelve desde la **primera iteración**.
    - **Medio**: puede quedar para la 2ª o 3ª iteración.
    - **Bajo**: para las últimas.
  - La priorización **solo tiene sentido en un proceso iterativo e incremental**. Si se omite un requerimiento de prioridad alta, después puede obligar a rehacer todo.
  - **Los requerimientos en conflicto deben tener el mismo nivel de prioridad.**
  - En la práctica se arma una **tabla**: nombre del RNF, descripción, si es SPA (Sí/No) y justificación. Un ejemplo es la tabla del caso FF en el §20.1.
  - Ejemplos:
    - **Impacta en la arquitectura** ("SÍ"): desarrollo web + mobile, o una licencia de Oracle ya comprada.
    - **No impacta** ("NO"): el formato de la contraseña, los colores o logos de la empresa, el lenguaje sin licencia (es una decisión de negocio).
  - Redacción correcta: "el sistema **DEBE**", nunca "debería".
- **Diseño arquitectónico**: recibe los requerimientos SPA con su grado de impacto. *Es la etapa en la que entran los estilos y patrones.* Ver el §7.
- **Validación**: compara si la arquitectura satisface los requerimientos. Si aparecen problemas, se hacen cambios y se vuelve a evaluar, por ejemplo renegociando algún RNF y regresando a la determinación. *La arquitectura debe evolucionar con el tiempo.*

---

## 5. Requerimientos No Funcionales y su impacto arquitectónico

Cada RNF crítico **sugiere una estrategia arquitectónica** (Sommerville / cátedra):

| RNF crítico | Sugerencia para la arquitectura |
|---|---|
| **Performance (desempeño)** | Ubicar las operaciones críticas en **pocos subsistemas** con poca comunicación entre ellos, usando **componentes de granularidad gruesa** |
| **Seguridad** | Usar una **estructura en capas**, con los recursos más críticos protegidos en las **capas internas** y un alto nivel de validación en ellas |
| **Protección (safety)** | Ubicar las operaciones de protección en **un solo subsistema o en pocos**, lo que reduce costos y problemas de validación |
| **Disponibilidad** | Incluir **componentes redundantes** que puedan reemplazarse y actualizarse **sin detener el sistema** |
| **Mantenibilidad** | Usar **componentes autocontenidos de granularidad fina** que se cambien con facilidad, separar productores de consumidores de datos y **evitar estructuras de datos compartidas** |

### Conflictos arquitectónicos (trade-offs)
- Los componentes de **granularidad gruesa mejoran la performance** pero **reducen la mantenibilidad**.
- Los **datos redundantes mejoran la disponibilidad** pero **dificultan la seguridad**.
- Agregar **seguridad** suele implicar más comunicación, por lo que **degrada la performance**.

Gorton agrega otros ejemplos: un sistema muy seguro puede ser difícil de integrar, uno muy disponible puede sacrificar performance y uno de alta performance puede quedar atado a una plataforma, perdiendo portabilidad.

🆕 **Aclaraciones de los resúmenes sobre cada RNF:**
- **Performance**: "granularidad gruesa" significa pocos componentes grandes, con menos interacciones entre ellos y **menos accesos a disco**.
- **Seguridad**: ubicar lo que se quiere proteger **lo más lejos posible del usuario final** (en las capas internas) y **todo junto en el menor espacio posible**, para reducir el esfuerzo de protección. Básicamente se trata de gestionar bien los permisos.
- **Protección**: los resúmenes la interpretan de dos maneras distintas.
  - Como *safety*: apunta a la funcionalidad, a si se va a **poder hacer** algo en un momento dado.
  - Como control de permisos concentrado en pocos subsistemas.
  - En cualquiera de los dos casos, la clave es **localizarla en pocos subsistemas**.
- **Disponibilidad**: **se atiende con redundancia** (componentes o información duplicados), que da arquitecturas tolerantes a fallos. Hay que evaluar *cuánta* disponibilidad se necesita: si es poca, molesta a los usuarios, y si es demasiada, genera gasto y desperdicio de recursos.
- **Mantenibilidad**: se logra con **alta cohesión y bajo acoplamiento**.

🆕 **¿Qué es un conflicto arquitectónico?** (pregunta de parcial)
Es una situación en la que dos o más requerimientos arquitectónicos tienen **demandas opuestas**, de modo que satisfacer uno perjudica al otro. Algunas soluciones propuestas en el resumen:
- **Granularidad gruesa vs. mantenibilidad**: usar un **enfoque híbrido**.
  - Las partes **estables y críticas en performance** (por ejemplo, el motor de pagos) van como componentes gruesos.
  - Las partes **que cambian mucho** (por ejemplo, el carrito) van como componentes finos o microservicios.
- **Redundancia vs. seguridad**: usar **seguridad por capas y centralizada**.
  - Las políticas se gestionan desde un único punto y se aplican igual en todos los nodos.
  - Los parches y la configuración se automatizan con orquestación (por ejemplo, Kubernetes).
  - Se monitorean las desviaciones de configuración.
- ⚠️ En una versión de la consigna dice "la redundancia mejora la disponibilidad pero hace la seguridad **más fácil**". Es un error de tipeo: lo correcto es **más difícil**.

🆕 **Pregunta de parcial: "Mencione 2 RNF y explique cómo impacta en la arquitectura considerarlos prioritarios".**
Ejemplo de respuesta:
- **Seguridad**: estructura en capas, recursos críticos en las capas internas y alta validación.
- **Disponibilidad**: componentes redundantes, reemplazables sin detener el sistema.
- Además, conviene señalar que **si los dos son críticos hay conflicto**: los datos redundantes mejoran la disponibilidad pero complican la seguridad.

> Los requerimientos de atributos de calidad deben ser **concretos y medibles**. Gorton da un ejemplo malo y uno bueno:
> - Malo: "debe ser escalable".
> - Bueno: "escalar de 100 a 10.000 escritorios sin aumentar el esfuerzo o costo de instalación".

**Escalabilidad (Gorton):**
- Puede referirse a carga de requests, conexiones simultáneas, tamaño de datos o despliegue.
- Hay dos formas de escalar:
  - **Scale up**: más CPU o memoria en la misma máquina (escalado vertical).
  - **Scale out**: distribuir en más máquinas (escalado horizontal).
- Otros atributos que menciona Gorton: **portabilidad**, **testeabilidad** y **soportabilidad**.

---

## 6. Calidad: ISO/IEC 25000 / 25010

**Objetivos de la ISO/IEC 25000 (SQuaRE):**
- Especificar requisitos de calidad de software.
- Evaluar y medir la calidad de software.

**Modelos de la familia:**
- **Calidad en uso**: ISO 25010:2011
- **Calidad de producto**: ISO 25010:2011
- **Calidad de datos**: ISO 25012:2008

> Los RNF **deben especificarse de manera medible y verificable**.

**Calidad de producto**: se refiere a propiedades **estáticas** del software y propiedades **dinámicas** del sistema de computadoras.

### Modelo de Calidad de Producto: 8 características y sus subcaracterísticas

| Característica | Definición | Subcaracterísticas |
|---|---|---|
| **Adecuación funcional** | Grado en que el producto provee funciones que satisfacen las necesidades | Completitud, Corrección, Pertinencia funcional |
| **Eficiencia de desempeño** | Cantidad de recursos usados bajo condiciones establecidas | **Comportamiento temporal** (tiempos de respuesta y procesamiento, tasas de rendimiento), **Utilización de recursos**, **Capacidad** (límites máximos: usuarios concurrentes, ancho de banda, transacciones, tamaño de la BD) |
| **Compatibilidad** | Realizar las funciones compartiendo el mismo entorno de HW y SW | **Coexistencia** (compartir el entorno sin perjudicar a otros), **Interoperabilidad** (intercambiar información y usarla) |
| **Usabilidad** | Usarse con efectividad, eficiencia y satisfacción en un contexto dado | Reconocimiento de adecuación, Aprendizaje, Operabilidad, Protección frente a errores de usuario, Estética de la interfaz, Accesibilidad |
| **Fiabilidad** | Ejecutar funciones bajo condiciones específicas durante un período | **Madurez**, **Disponibilidad** (% de tiempo operativo; combina madurez, tolerancia a fallos y recuperabilidad), **Tolerancia a fallos**, **Capacidad de recuperación** |
| **Seguridad** | Proteger la información para que cada uno acceda según su autorización | **Confidencialidad**, **Integridad**, **No repudio**, **Autenticidad**, **Responsabilidad** (trazar una acción unívocamente a su autor) |
| **Mantenibilidad** | Efectividad y eficiencia para modificar el producto | **Modularidad**, **Reusabilidad**, **Analizabilidad**, **Capacidad de ser modificado**, **Capacidad de ser probado** |
| **Portabilidad** | Transferirse de un entorno de HW, SW u operativo a otro | **Adaptabilidad** (incluye la **escalabilidad** de capacidades internas), **Facilidad de instalación**, **Capacidad de ser reemplazado** |

**Tácticas para la fiabilidad (disponibilidad):**
1. **Detección** de fallas, por ejemplo con monitoreo.
2. **Preparación** para la recuperación y reparación, por ejemplo con respaldos.
3. **Reintroducción** una vez corregido, por ejemplo con sincronización o rollback.
4. **Prevención**, por ejemplo con monitores de procesos.

Blancarte clasifica los atributos de calidad en dos grupos:
- **Observables** en ejecución: performance, seguridad, disponibilidad, funcionalidad y usabilidad.
- **No observables**: modificabilidad, portabilidad, reusabilidad, testeabilidad y escalabilidad.

---

## 7. Diseño Arquitectónico: framework, componentes y vistas

La actividad de diseño recibe como **entrada** los requerimientos de arquitectura y tiene dos pasos iterativos:
1. **Elegir el framework de arquitectura**.
2. **Distribuir los componentes**.

Como **salida** produce las **vistas de arquitectura** y el **documento de arquitectura**.

### 7.1 Elegir el framework de arquitectura
- La mayoría de las aplicaciones responden a un **pequeño número de arquitecturas probadas y bien comprendidas**.
- **Usar soluciones conocidas minimiza los riesgos.**
- **No hay fórmula mágica.** El prerrequisito es **conocer los estilos arquitectónicos principales** y qué atributos de calidad resuelve cada uno.
- Los estilos son **herramientas, no decisiones exclusivas**. Se **combinan** porque cada uno resuelve un problema diferente y opera a distinto nivel. **Las arquitecturas reales son híbridas por diseño.**
- 🆕 **Framework arquitectónico** = el **conjunto de patrones arquitectónicos** (normalmente **3 o 4**, "no son muchos") que se eligen para organizar y estructurar este producto en particular. Los patrones arquitectónicos trabajan a nivel de **subsistemas y capas**, mientras que los patrones de diseño trabajan a nivel de clases u objetos.
- 🆕 **Patrones "de cátedra" según los resúmenes anteriores (2021)**: la cátedra trabajaba con **6 patrones**, los de Gorton:
  1. Layered
  2. N-Tier Client/Server
  3. Messaging
  4. Publish & Subscribe
  5. Broker
  6. Process Coordinator

  En **2026** la cátedra amplió esto con la taxonomía del §13: descomposición (monolítico o microservicios), estructura interna (layered o hexagonal), distribución (C/S o N-Tier) y comunicación (EDA, REST). Los 6 patrones clásicos siguen incluidos.

### 7.2 Distribuir componentes
Una vez elegido el framework, que define la comunicación general, hay que identificar:
- Los **componentes principales** y cómo se insertan en el framework.
- Los **servicios o interfaces** que soporta cada componente.
- Las **responsabilidades** de cada componente, es decir, qué hace cuando recibe un pedido.
- Las **dependencias** entre componentes.
- Las **particiones candidatas a distribuirse** entre servidores de la red.

🆕 En la práctica, **distribuir componentes** es aplicar los patrones del framework: crear los componentes que cada patrón necesita, con sus interfaces y relaciones.
- **En el documento de arquitectura hay que justificar por qué se eligió cada patrón** y sus **consecuencias positivas y negativas**. "Lo importante de cada patrón es que se justifique."
- **Salida**: las **vistas arquitectónicas**, que son el *artefacto principal, no negociable*, y el **documento de arquitectura**, que justifica las decisiones.

### 7.3 Lineamientos para el diseño de componentes (Gorton)
- **Minimizar las dependencias** entre componentes y buscar bajo acoplamiento. Recordar que *si se cambia algo, hay que volver a testearlo*.
- Diseñar componentes que encapsulen **responsabilidades altamente cohesivas**. Esto limita los tipos de cambio y reduce el mantenimiento y el testing.
- **Aislar las dependencias con el middleware y los productos COTS** de infraestructura. Es más difícil de construir e introduce penalidades de performance, pero facilita cambiar o actualizar esa infraestructura.
- Usar la **descomposición** para estructurar los componentes **jerárquicamente**.
- **Minimizar las llamadas entre componentes**, agrupándolas en llamadas más gruesas, sobre todo si están distribuidos.

### 7.4 Modelos, Vistas y Puntos de Vista ("mismo objeto, diferentes modelos para diferentes propósitos")
| Concepto | Definición |
|---|---|
| **Modelo** | Descripción **completa** de un sistema desde una perspectiva particular y a un nivel de abstracción específico |
| **Vista** | **Proyección** de uno o más modelos, vista desde una perspectiva o punto de ventaja particular, que **omite las entidades no relevantes** para esa perspectiva |
| **Punto de vista** | **Definición o descripción de una vista**: su contenido, significado y representación (lenguajes, técnicas de modelado) |

> Las vistas son proyecciones para **un involucrado en particular y sus intereses**. Por eso **distintos involucrados necesitan distintas vistas del mismo sistema**. El punto de vista *define* la vista.

Analogía de jarroba: el plano de una casa es una **vista**, y la leyenda que explica qué significa cada símbolo es el **punto de vista**. Cuando se usa UML, el punto de vista ya está bien definido por el lenguaje.

---

## 8. Modelo 4+1 de Kruchten y vistas del PUD

**Paper**: *Architectural Blueprints — The "4+1" View Model of Software Architecture*, P. Kruchten (Rational), IEEE Software, 1995. Es compatible con **IEEE 1471-2000**.

**Problema que resuelve:** un único diagrama de cajas y flechas intenta representar todo a la vez y queda ambiguo. ¿Las cajas son procesos, código o máquinas? ¿Las flechas son dependencias o flujo de datos? La propuesta es usar **varias vistas concurrentes**, cada una dedicada a un conjunto de intereses de los involucrados, y tratar por separado los requerimientos funcionales y los no funcionales.

| Vista (Kruchten) | Vista en el PUD | Qué captura | Involucrados | Intereses | Notación / UML |
|---|---|---|---|---|---|
| **Lógica** | Vista de Diseño | Modelo de objetos del diseño. Soporta los **requerimientos funcionales**: abstracciones clave del dominio (clases, interfaces, colaboraciones). Es el "vocabulario" | Usuario final | Funcionalidad | Diagramas de clases, comunicación y secuencia (o E-R si el sistema está centrado en datos) |
| **De Procesos** | Vista de Proceso | **Concurrencia y sincronización**: procesos, tareas e hilos (clases activas). Contempla RNF como **performance, disponibilidad, escalabilidad**, tolerancia a fallos e integridad | Integradores | Performance, escalabilidad | Diagramas de actividad o de procesos |
| **De Desarrollo** | Vista de Implementación | **Organización estática del software** en el entorno de desarrollo: módulos, subsistemas, librerías y capas. Cubre el montaje del sistema y la **administración de la configuración** | Programadores, gestión del software | Organización, reuso, portabilidad, línea de producto | Diagramas de componentes y paquetes |
| **Física** | Vista de Despliegue | **Mapeo del software sobre el hardware** (nodos): topología, distribución, entrega e instalación. Contempla disponibilidad, confiabilidad, performance y escalabilidad | Ingenieros de sistemas | Topología, comunicaciones | Diagrama de despliegue |
| **+1 Escenarios** | Vista de Casos de Uso | Un conjunto chico de **escenarios o casos de uso importantes** que **une y relaciona las otras 4 vistas** | Todos | Comprensibilidad | Diagramas de casos de uso y de interacción de objetos |

**La vista "+1" es redundante con las demás** (por eso se la cuenta aparte) y cumple dos funciones:
1. **Conductora**: sirve para *descubrir* los elementos arquitectónicos durante el diseño.
2. **Validadora**: sirve para *ilustrar y validar* la arquitectura una vez diseñada, en papel y como punto de partida de las pruebas del prototipo arquitectónico.

**Otros puntos clave del paper:**
- En cada vista se aplica la fórmula de Perry & Wolf (elementos, formas y fundamentos). Además, **cada vista puede usar su propio estilo arquitectónico**, por lo que en un sistema conviven varios estilos:
  - Vista lógica: estilo orientado a objetos.
  - Vista de procesos: pipes & filters o cliente/servidor.
  - Vista de desarrollo: **estilo en capas**, con 4 a 6 capas, donde cada subsistema solo depende de su misma capa o de capas inferiores.
- **Correspondencia entre vistas**: las vistas no son totalmente independientes.
  - De lógica a procesos se evalúa la autonomía de los objetos (activos, pasivos o protegidos), su persistencia, subordinación y distribución. Hay dos estrategias para hacerlo: *inside-out* y *outside-in*.
  - De lógica a desarrollo: una clase corresponde a un módulo y una categoría de clases a un subsistema.
  - De procesos a física: los procesos se mapean sobre los nodos.
  - Cuanto más grande es el proyecto, mayor es la distancia entre las vistas.
- **Adaptar el modelo (tailoring)**: no todos los sistemas requieren las 5 vistas.
  - Con **un solo procesador** se omite la vista **física/de despliegue**.
  - Con **un solo proceso** se omite la vista de **procesos**.
  - En **programas muy chicos**, la vista lógica y la de desarrollo pueden unificarse (la cátedra dice que se puede saltar la de implementación).
  - **Los escenarios son útiles siempre.**
  - Se pueden **agregar otras vistas**, como la de **datos** o la de **seguridad**. Kruchten dice que una vista de datos puede incluirse dentro de la lógica.
- **Proceso iterativo y guiado por escenarios**:
  1. Se eligen pocos escenarios según su riesgo y criticidad.
  2. Se arma una arquitectura preliminar (*strawman*).
  3. Se recorren los escenarios sobre ella y se descubren los elementos arquitectónicos.
  4. Se actualizan las 4 vistas.
  5. Se implementa, se prueba y se mide.
  6. Se registran las lecciones aprendidas y se vuelve a iterar.
  Después de 2 o 3 iteraciones la arquitectura se estabiliza. El prototipo es **evolutivo**: no se descarta, termina siendo el sistema.
- **Documentación**: un *Software Architecture Document* organizado según las 4+1 vistas y una *Software Design Guidelines* con las decisiones que deben respetarse. El índice del SAD es:
  1. Alcance
  2. Referencias
  3. Arquitectura
  4. Objetivos y restricciones
  5. Arquitectura lógica
  6. Arquitectura de procesos
  7. Arquitectura de desarrollo
  8. Arquitectura física
  9. Escenarios
  10. Tamaño y performance
  11. Calidad

**¿Qué vista necesito?** (actividad del Repaso)
| Inquietud del involucrado | Vista |
|---|---|
| ¿En qué servidores o nodos se instala cada parte? | **Despliegue** |
| ¿Qué clases colaboran para realizar este caso de uso? | **Diseño** (lógica) |
| ¿Aguantará la carga de usuarios concurrentes? | **Proceso** |
| ¿Cómo se ensamblan los componentes y se gestiona la configuración? | **Implementación** |
| ¿Qué hace el sistema para el usuario? | **Casos de Uso** |

> ⚠️ **Confusión de nombres:** en algunos textos la "vista de desarrollo" de Kruchten se llama "de implementación" o incluso "de despliegue". La vista "física" equivale a la de "despliegue" del PUD. Lo importante es **qué se documenta en cada vista**, no el nombre.
>
> 🆕 Esto pasa en los resúmenes de alumnos. Usan la nomenclatura del artículo de jarroba:
> - **Escenarios**: CU significativos para la arquitectura.
> - **Lógica**: la funcionalidad para el usuario final.
> - **De interacción**: flujos de control, concurrencia y sincronización. Equivale a la vista de **Procesos**.
> - **"De despliegue"**: el punto de vista del desarrollador y la gestión de los componentes de código. Equivale a la vista de **Desarrollo/Implementación**.
> - **Física**: la distribución del software en el hardware y la topología.
>
> Si en el parcial preguntan por el 4+1, conviene **aclarar la equivalencia** con las vistas del PUD.

### 🆕 8.1 Vistas del PUD: qué hace cada una y qué diagramas usa
La arquitectura se describe con **5 vistas interrelacionadas**. Cada una puede tener un **aspecto estático** y otro **dinámico**. El modelo tuvo éxito porque **es el único que incluye la funcionalidad** (la vista de CU) entre sus vistas. La consigna es **"menos es más"**: vistas lo más pequeñas posible.

| Vista PUD | Se centra en | Diagramas estáticos | Diagramas dinámicos |
|---|---|---|---|
| **Casos de Uso** (comportamiento) | Comportamiento percibido por usuarios, analistas y testers. Se eligen los **CU significativos para la arquitectura** | Casos de uso | Interacción (secuencia y comunicación), estados, actividad |
| **Diseño** (vocabulario) | Clases, interfaces y colaboraciones. Soporta los RF | Clases y **componentes** (en la cátedra, componentes) | Secuencia |
| **Procesos / Interacción** (flujo de control) | Concurrencia, sincronización, **clases activas** | Componentes, con énfasis en las clases activas | Secuencia (y actividad) |
| **Implementación** (ensamblado) | Artefactos para ensamblar y ejecutar el sistema, gestión de la configuración, subsistemas, interfaces y comunicación entre capas | Componentes / artefactos | Secuencia |
| **Despliegue** (topología de hardware) | Los nodos sobre los que corre el sistema | **Despliegue** | Secuencia |

**Tabla del resumen manuscrito** (pregunta de parcial "Explique cada vista y sus diagramas"):
| Vista | Qué muestra | Diagramas |
|---|---|---|
| CU | Los CU de software esenciales | CU, secuencia y comunicación |
| Diseño | Cómo se provee la funcionalidad, mediante subsistemas y relaciones | Clases, secuencia y componentes |
| Proceso | Cómo se comunican los procesos del sistema | Componentes, secuencia y actividad |
| Despliegue (desarrollo) | El punto de vista del desarrollador: gestión del software en componentes de código y sus dependencias | Componentes y secuencia |
| Física | El hardware y la distribución física | Despliegue y secuencia |

**Tipos de vistas (viewtypes)**, que agrupan vistas y se asocian a patrones:
- **De módulo (estáticas)**: muestran la estructura del código, sus componentes, módulos y capas, sin ejecución. Por ejemplo, *Layered*. Diagramas UML: componentes y paquetes.
- **De ejecución (runtime / comportamiento)**: muestran cómo se comunican en ejecución las capas, componentes o subsistemas. Por ejemplo, *C/S, Pub/Sub, Broker, Messaging*. Diagramas UML: CU, secuencia, actividad y estados.
- **De distribución (asignación)**: muestran cómo se distribuye el software en el hardware. Por ejemplo, *N-Tier, Mirrored, Rack, Farm*. Diagramas UML: despliegue y comunicación.

**Vinculación de las vistas con los Workflows del PUD:**
- El PUD es **centrado en la arquitectura** y a la vez **conducido por casos de uso**, lo que equilibra lo funcional con lo no funcional. Por eso la arquitectura se piensa **desde el primer WF**.
- La vista de **CU**:
  - Su parte estática se arma en el **WF de Requerimientos**.
  - Su parte dinámica sale del **WF de Análisis**.
- **El resto de las vistas se diseña en el WF de Diseño.**
  - ⚠️ Es un error decir que la vista de despliegue se diseña en el WF de Despliegue, o la de implementación en el WF de Implementación.
  - Esos WF **no modelan**: solo bajan a la realidad lo que se modeló en Diseño.
- La arquitectura se define y estabiliza en la **fase de Elaboración**. El diseño es el centro de atención al final de Elaboración y al comienzo de Construcción.

**¿Son necesarias todas las vistas?**
- No siempre. Por ejemplo, un sistema personal en una sola PC no necesita la de despliegue.
- Según el resumen 2021, la **parte estática no negociable** son las vistas de **CU, Diseño e Implementación**.
- Kruchten no propone una **vista de datos/persistencia** ni una **vista de seguridad**, pero muchas veces hacen falta.

### 🆕 8.2 Cómo se construyen las vistas en la práctica de la cátedra
En la cátedra se trabaja con vistas **estáticas**: la **vista de la funcionalidad**, la **vista de diseño** (con diagrama de componentes) y la **vista de despliegue** (distribución de software en hardware).

**1) Vista arquitectónica de la funcionalidad (CU):**
- Se elige la **menor cantidad posible de CU** que reflejen *todas* las decisiones arquitectónicas.
  - Normalmente es el **10 a 15 % de la funcionalidad**.
  - En FF hay unos **200 CU y se representan con 15**.
  - El resto de los CU repite las decisiones que ya se tomaron con estos.
- **CU que no pueden faltar:**
  - CU representativos de cada **patrón arquitectónico** elegido.
  - Un **ABMC completo y complejo** de una entidad de negocio importante, que represente a todos los demás ABMC.
  - La **transacción más compleja** del sistema.
  - **Un informe, una estadística y un reporte**, aunque no haya un RNF asociado, porque fijan cómo se accede a los datos y cómo se muestran.
  - Los **CU temporales o procesos automáticos**.
  - El **inicio de sesión**, por la autenticación y el acceso.
  - Los CU que resuelven cada **RNF significativo**.
- Cada CU incluido se **justifica**.

**2) Vista de diseño, con diagrama de componentes:**
- El componente es una pieza de código. El diagrama de componentes es **el único diagrama UML que modela el software desde un aspecto "físico"**.
- Se organiza en **capas** al estilo Layered y se ponen los componentes dentro de cada subsistema o capa.
- **Interfaces:**
  - Una interfaz es una clase abstracta con solo las firmas de los métodos. *No es la clase boundary del análisis.*
  - **Provista**: el componente tiene el código y ofrece ese comportamiento.
  - **Requerida**: el componente necesita ese servicio.
  - La provista y la requerida deben ser compatibles, y en la práctica tienen el mismo nombre.
  - **Una interfaz nunca puede aparecer sola**, sin estar vinculada a un componente.
  - La **relación de realización** va del componente a la interfaz y se dibuja como la herencia, pero con línea punteada.
- **Capa de persistencia** (es casi igual en todos los sistemas):
  - Tiene un componente *BaseDeDatos* y un componente *Persistencia* que ofrece dos interfaces provistas:
    - **IDesmaterializar**: convierte objetos en filas.
    - **IMaterializar**: convierte filas en objetos.
  - Los componentes que **no necesitan persistencia** se pintan de **otro color**.
- **Reglas:**
  - Dentro de un mismo subsistema, los componentes se relacionan con **dependencias**.
  - Entre subsistemas o capas distintas, se relacionan con **interfaces**.
  - Si la interfaz la provee o requiere un solo componente, va en ese componente. Si la usan todos los del subsistema, va en el subsistema.
  - Un componente puede resolver **varios** RNF o CU, y **un componente no equivale a una clase** del dominio: puede agrupar varias.
- Para armar esta vista hay que integrar todo lo anterior: los RNF significativos, los patrones (cada uno con su componente asociado), los alcances y la vista de funcionalidad.

**3) Vista de despliegue, con diagrama de despliegue:**
- Es **el único diagrama UML que modela hardware**.
- **Nodos**: recursos computacionales con memoria y, a menudo, capacidad de procesamiento. Se dibujan como **cubos, no cuadrados**.
  - Los componentes son *lo que se ejecuta* y los nodos son *donde se ejecuta*.
- **Conexiones**: asociaciones entre nodos (Ethernet, bus, satélite…).
- **Cómo se construye:**
  1. Identificar los nodos.
  2. Estereotiparlos.
  3. Asignar cada componente significativo a un nodo.
  4. Considerar la duplicación de componentes.
- La cátedra usa la variante **"distribución de software en hardware"**.
- **Reglas de ubicación (ejemplo FF):**
  - Presentación web e interfaces con externos en el **servidor web**.
  - Componentes de lógica de negocio, persistencia y generadores de informes en el **servidor de aplicaciones**.
  - Solo el componente *BaseDeDatos* en el **servidor de BD**: el de persistencia *no* va ahí.
  - Los **servidores externos** se distinguen con otro color y no hace falta detallar lo que tienen adentro.

---

## 9. Validación de la arquitectura

- Aumenta la **confianza del equipo** en que la arquitectura cumple su propósito.
- Debe hacerse **dentro de las restricciones de tiempo y presupuesto**. El truco es ser **tan riguroso y eficiente como sea posible**.
- **¿Por qué es un desafío?** Porque la arquitectura es un **diseño**: **no puede ejecutarse ni probarse** directamente para ver si cumple los requerimientos.
- Hay **dos técnicas probadas**, y ambas sirven para identificar fallas, debilidades y áreas de riesgo:

### 1) Uso de Escenarios (técnica del SEI)
- Los escenarios se relacionan con aspectos arquitectónicos como los **atributos de calidad** y muestran las consecuencias de las decisiones de diseño.
- Son **artefactos simples**: se define un **estímulo** que impacta en la arquitectura y se trabaja la **respuesta** de la arquitectura a ese estímulo.

| Atributo | Estímulo | Respuesta |
|---|---|---|
| Disponibilidad | Falla la conexión de red para los mensajes de los clientes | Los mensajes se almacenan en el servidor MOM hasta que se restablezca la comunicación |
| Modificabilidad | Debe estar disponible un conjunto nuevo de componentes de análisis de datos | Se reconstruye la aplicación con las librerías nuevas y se actualiza la configuración en cada escritorio |
| Seguridad | No se recibieron pedidos en una sesión durante 10 minutos | La sesión se considera insegura, se invalidan las credenciales y el usuario debe volver a loguearse |
| Modificabilidad | El proveedor del motor de transformaciones se retiró del negocio | Se compra un motor nuevo y se reimplementa la capa de servicio abstracto que lo envuelve. Los clientes no se ven afectados |
| Escalabilidad | La carga de usuarios concurrentes se duplica durante las 3 semanas de inscripción | El servidor de aplicaciones se escala a un cluster de 2 máquinas |

### 2) Construcción de Prototipos
- **¿Cuándo no alcanza un escenario?** Cuando la pregunta no puede contestarse con confianza solo razonando. Ejemplo: *"¿Se pueden procesar 500 órdenes en 5 minutos?"*. La única forma de responderlo con confianza es construir un prototipo.
- Los prototipos son **versiones mínimas y restringidas** de la aplicación, hechas para probar algún **aspecto riesgoso o poco comprendido**.
- Tienen dos propósitos:
  - **Prueba de concepto**: ¿puede construirse la arquitectura tal como fue diseñada de modo que satisfaga los requerimientos?
  - **Prueba de tecnología**: ¿la tecnología elegida (middleware, librerías, aplicaciones integradas) se comporta como se espera?
- En ambos casos aportan evidencia que no se puede obtener de otra forma.

---

## 10. Modelado y documentación de la arquitectura

**Modelado de la arquitectura:**
- Mapea los **requerimientos funcionales del análisis a una arquitectura tecnológica** y debe tratar los **RNF**.
- **No existe la solución perfecta**: se elige la **óptima** para las circunstancias.
- Debe considerar:
  - volúmenes de datos
  - funcionalidad más demandada
  - distribución geográfica
  - distribución del procesamiento
  - dónde se guardan los datos
  - qué procesos corren en qué procesadores y cuánta comunicación necesitan

**Salidas del modelo arquitectónico:**
- Distribución geográfica de los requerimientos de computación.
- Configuración y cantidad de niveles de hardware.
- Componentes de hardware para clientes y servidores.
- Sistema operativo.
- Mecanismos y lenguajes de comunicación de la red.
- Paradigma de desarrollo.
- Lenguaje de presentación (front end) y lenguaje de fondo (back end).
- Sistema de administración de base de datos (DBMS).
- Ubicación de los procesos.
- Ubicación de los datos físicos.
- Estrategias de sincronización de los datos distribuidos.

**¿Para qué se usa la documentación?**
- Expresar el sistema y su evolución.
- Comunicar a los involucrados.
- Evaluar y comparar arquitecturas de forma consistente.
- Planificar, administrar y ejecutar el desarrollo.
- Expresar las características persistentes y los principios que guían el cambio aceptable.
- Verificar la implementación.

🆕 **¿Por qué documentar la arquitectura?**
- Para que otros, tanto involucrados como miembros del equipo, puedan **entender y evaluar** el diseño.
- Para que **uno mismo lo entienda** al volver después de mucho tiempo.
- Para que otros **aprendan** del razonamiento detrás de la arquitectura.
- Para poder **analizar** el diseño: evaluar el rendimiento o generar métricas como el acoplamiento y la cohesión.
- **La tecnología y el contexto cambian constantemente**, así que hay que dejar escrito *qué* se decidió, *por qué* y *cuándo*.
- "Documentar" **no implica un formato específico** (puede ser una wiki o un Word). Importa el contenido, no el contenedor.

🆕 **¿Qué documentar?**
- Interfaces de los componentes.
- Limitaciones de los subsistemas.
- Escenarios de test.
- Decisiones de compra de componentes de terceros.
- Estructura del equipo y dependencias de calendario.
- Servicios externos.
- Datos de la descripción: fecha, estado, alcance, contexto, historia de cambios, glosario, resumen y referencias.

🆕 **¿Por qué se documenta con vistas?** (pregunta de parcial)
- Según Kruchten, la arquitectura es **demasiado amplia y tiene demasiados intereses** como para representarla en un único diagrama.
- Cada vista la muestra desde **un punto de vista distinto**, con los elementos útiles para **un grupo de involucrados**.
- Las vistas permiten comunicar el diseño de forma clara y acotada, identificar componentes y dependencias, detectar problemas y evaluar si la arquitectura cumple los RNF.

### Template para documentar la arquitectura (Gorton, cap. 6)
1. **Nombre y contexto del producto.**
2. **Requerimientos arquitectónicos:**
   - objetivos clave
   - casos de uso arquitectónicos
   - involucrados (usuarios, compradores, desarrolladores, mantenedores)
   - requerimientos de los involucrados (propósito o misión, factibilidad, riesgos, mantenibilidad, despliegue y evolución)
   - **restricciones**
   - **RNF**
   - **riesgos**
3. **Solución:**
   - patrones arquitectónicos relevantes
   - **selección de puntos de vista**. Para cada uno: nombre, involucrados, intereses, lenguaje o técnicas, pruebas de consistencia y completitud, técnicas de análisis y heurísticas
   - **vistas** (una por punto de vista)
   - consistencia entre vistas
   - **razón (rationale) de la arquitectura**
   - aspectos de implementación
4. **Análisis de la arquitectura:** análisis de escenarios y riesgos.

---

## 11. Sistemas distribuidos y sus 8 falacias

**Tipos de sistemas:**
- **Personales**: no distribuidos, corren en una PC.
- **Embebidos**: un procesador o un grupo integrado de procesadores.
- **Distribuidos**: un grupo de procesadores cooperativos conectados por una red.

Hoy prácticamente todos los sistemas grandes son distribuidos.

**Características (ventajas) de los sistemas distribuidos:**
- **Compartición de recursos** de hardware y software.
- **Apertura**: se basan en protocolos estándar y combinan productos de distintos vendedores.
- **Concurrencia**: varios procesos operan al mismo tiempo en distintas computadoras.
- **Tolerancia a fallas**, gracias a la disponibilidad de varios recursos.
- **Escalabilidad**, en tres dimensiones:
  - **Tamaño**: poder agregar recursos (expandir vs. ampliar).
  - **Distribución**: dispersar los recursos sin perder rendimiento.
  - **Manejabilidad**: poder administrarlo aunque crezca o abarque varias organizaciones.

**Desventajas:**
- **Complejidad**: son más difíciles de comprender y probar.
- **Seguridad**: es difícil asegurar la integridad y evitar la degradación del servicio.
- **Manejabilidad**: los defectos se propagan entre máquinas, por lo que son más difíciles de gestionar.
- **Impredecibilidad**: la respuesta depende de la carga total, de la organización y de la red.

**Ataques de los que deben defenderse:**
- **Intercepción**: afecta la confidencialidad.
- **Interrupción**: el servicio no puede entregarse, por ejemplo en un DoS.
- **Modificación**: se alteran datos o servicios.
- **Fabricación**: se genera información falsa para obtener privilegios.

### Las 8 falacias de los sistemas distribuidos (L. P. Deutsch, Sun, 1994)
1. **La red es confiable / está disponible.** No lo es, por eso existen los timeouts y los circuit breakers.
2. **La latencia es cero.** Una llamada remota tarda milisegundos y una local nanosegundos. Hay que conocer la latencia promedio y los percentiles 95 a 99.
3. **El ancho de banda es infinito.** El *stamp coupling* (devolver 500 KB cuando se necesitan 200 bytes) puede saturar la red.
4. **La red es segura.** Cada endpoint debe asegurarse, lo que aumenta mucho la superficie de ataque.
5. **La topología nunca cambia.** Un cambio "menor" de red puede invalidar los supuestos de latencia.
6. **Hay un único administrador.** En realidad hay muchos y hace falta coordinarse con ellos.
7. **El costo de transporte es cero.** Hace falta hardware, gateways, firewalls y proxies, lo que cuesta dinero. No es lo mismo que la latencia.
8. **La red es homogénea.** Hay varios vendedores de hardware que no siempre interoperan bien, y esto impacta en las falacias 1, 2 y 3.

Richards & Ford agregan otros desafíos de lo distribuido: logging distribuido, transacciones distribuidas, mantenimiento y versionado de contratos.

### 🆕 11.1 Complementos del resumen 2021 (Anexo 3)
- **¿Qué se distribuye?** El **procesamiento**, no los datos. Un sistema puede ser centralizado y tener la BD distribuida: lo que define a un sistema distribuido es que el procesamiento ocurre en varios procesadores.
- **Middleware**:
  - Es software que se ubica **entre el sistema operativo y la aplicación** y administra y soporta los componentes de un sistema distribuido.
  - Suele ser **"enlatado" (off-the-shelf)**: monitores de transacciones, conversores de datos, controladores de comunicación, motores de BD, brokers y colas.
  - Las arquitecturas actuales se apoyan totalmente en middleware.
- **Capas vs. niveles**: cuando se habla de software se habla de **capas**, y cuando se habla de hardware, de **niveles**.
  - **2 niveles**: cliente y servidor.
  - **3 niveles**: clientes, servidor de aplicaciones y servidor central de BD. Se separa el servidor por seguridad.
  - **n niveles**: varios servidores, cada uno atendiendo a grupos de clientes.
  - Recomendación: poner la **BD lo más lejos posible del usuario final**.
- **Cliente pesado vs. cliente liviano**:
  - La presentación siempre va en el cliente y la BD siempre en el servidor. Lo que se decide es **dónde va la lógica de negocio**.
  - **Cliente pesado**: la lógica de negocio está en el cliente. Ejemplos: una app de escritorio, WhatsApp Desktop, Zoom instalado.
  - **Cliente liviano**: el cliente solo tiene la presentación. Ejemplos: las apps web, WhatsApp Web.
  - También existen soluciones intermedias, con parte de la lógica en cada lado.
- **Estilos para sistemas distribuidos** (Sommerville):
  1. **Maestro-esclavo**:
     - Un proceso maestro se encarga de la coordinación, el cómputo y la comunicación, y los esclavos hacen tareas específicas (por ejemplo, leer sensores).
     - Se usa en **sistemas de tiempo real** y cuando el procesamiento distribuido es **predecible**.
  2. **Cliente-servidor** de 2 niveles y multinivel.
  3. **Componentes distribuidos.**
  4. **Peer-to-peer**:
     - **Descentralizado**: todos los nodos son iguales. Una desventaja es que una petición puede ser respondida por varios nodos de forma ambigua.
     - **Semicentralizado**: un **nodo "super-par"** mantiene la lista de servicios que ofrece cada nodo.
     - Se usa en cómputo intensivo divisible y en el intercambio de información sin almacenamiento central.
  5. **Map-Reduce**: para grandes volúmenes de datos (buscadores, redes sociales), con recuperación ante fallas.
- **Estilos de la vista de distribución (hardware)**:
  - **Espejada (Mirrored)**: hardware idéntico duplicado que trabaja en *lock-step*. Si uno falla, sigue el otro. Aumenta la disponibilidad, siempre que las copias estén en hardware separado.
  - **Rack**: servidores apilados que comparten red y tienen comunicación interna muy rápida.
  - **Granja (Server Farm)**: muchas computadoras o racks en un mismo lugar, usados como recurso masivo. Las aplicaciones deben ser **sin estado** y se escala agregando más hardware igual. Es como se organizan los data center.

---

## 12. Estilos vs. Patrones arquitectónicos

| | **Estilo arquitectónico** | **Patrón arquitectónico** |
|---|---|---|
| Definición (cátedra) | Define **cómo organizar los componentes y cómo interactúan**. Establece un **marco de referencia** para construir aplicaciones que comparten atributos y características por los cuales se las identifica y clasifica | Expresa una **organización estructural fundamental** o esquema. Provee **subsistemas predefinidos**, especifica sus **responsabilidades** e incluye **reglas y pautas** para organizar las relaciones entre ellos |
| Propósito | **Sirve para nombrar un diseño arquitectónico recurrente**. No resuelve un problema concreto | **Da solución a un problema recurrente**. Mejora la partición y promueve la reutilización del diseño |
| Tecnología | **No determina tecnología ni detalles de implementación** | Comprende un conjunto de principios que dan forma a una aplicación y definen sus características básicas y su comportamiento |
| Escala | Mayor: un sistema suele tener **un estilo dominante** | **Menor que los estilos**: pueden aparecer **varios patrones en un mismo diseño** |
| Cómo se elige | Según necesidades como escalabilidad, mantenibilidad, rendimiento o integración | — |

- **La distinción no es tajante**: hay casos difíciles de clasificar y a veces se usan como sinónimos (Fairbanks).
- Jerarquía según Blancarte: los **estilos** son lo más general, se implementan con **patrones arquitectónicos**, y estos a su vez con **patrones de diseño** (GoF: creacionales, estructurales y de comportamiento).
  - Los patrones de diseño se centran en clases y objetos, y si se cambian el impacto es local.
  - Los patrones arquitectónicos se centran en componentes, y su impacto es global.
- Según Fairbanks, un estilo define un **conjunto de tipos de elementos** (módulos, componentes, conectores, puertos) y **restricciones** sobre cómo usarlos. **Las restricciones son las que garantizan los atributos de calidad.**
  - Ventajas de usar estilos: son restricciones prefabricadas, dan consistencia y comprensibilidad, mejoran la comunicación (con el nombre alcanza), permiten reusar diseño, aseguran atributos de calidad y permiten analizar el sistema ("sin restricciones no hay análisis").

**Platónicos vs. Embebidos (Fairbanks):**
- **Platónicos**: idealizados, aparecen en los libros y rara vez de la misma forma en el código.
- **Embebidos (embodied)**: aparecen en sistemas reales y a menudo **violan las restricciones** estrictas de los platónicos. **Esa violación implica una gran compensación**: ya no se puede contar con las propiedades del estilo, porque esas propiedades derivan de las restricciones.

**Big Ball of Mud ("la arquitectura de la no arquitectura")**, también llamada *arquitectura spaghetti*:
- Es el estilo que tiene un producto **cuando no se define una arquitectura** o cuando los estilos no se mantienen en el tiempo.
- Surge de un equipo **sin un plan** de construcción.
- Sus partes están **altamente acopladas**, el mantenimiento es muy difícil y explicarle a alguien nuevo cómo encaja todo es casi imposible.
- Tiene una deuda técnica altísima y datos prácticamente globales (Fairbanks).
- Según Fairbanks, el estilo en capas es el que **más se busca** y el Big Ball of Mud es el que **más se logra**.

---

## 13. Taxonomía de estilos (mapa mental de la cátedra)

| Dimensión | Pregunta que responde | Estilos vistos |
|---|---|---|
| **Descomposición** | ¿En cuántas aplicaciones separo el sistema y cómo? Responde a la escalabilidad organizacional y técnica | **Monolítico**, **Microservicios** |
| **Estructura interna** | ¿Cómo organizo el sistema por dentro? ¿Cómo se escribe el código? Responde a la mantenibilidad | **Layered (capas)**, **Hexagonal** |
| **Distribución** | ¿Cómo se organiza físicamente (topología)? ¿Quién inicia la comunicación? ¿Cómo se conectan los nodos? | **Cliente/Servidor**, **N-Tier** |
| **Comunicación** | ¿Cómo fluye la información? ¿Cómo interactúan los componentes? | **EDA** (Pub/Sub, Broker, Messaging, Process Coordinator), **Request-Response / REST** |
| **Procesamiento** | ¿Cómo fluye y se transforma la información dentro de un componente? | Por ejemplo Pipes & Filters, Batch, Map-Reduce (no se desarrollan en clase) |

---

## 14. Descomposición: Monolítico vs Microservicios

### 14.1 Arquitectura Monolítica
- Son aplicaciones **autosuficientes** que contienen toda la funcionalidad necesaria, **sin dependencias externas**. Su capacidad más notable es la **INDEPENDENCIA**.
- El nombre viene de *mono* (único) y *lítico* (pétreo, rígido).
- **Todos los componentes y módulos se empaquetan y despliegan como uno solo**: todos los módulos y niveles están unidos y actúan como un cuerpo singular.
- La mayor parte del código del servidor está en un programa que se comunica con una o más BD. **Dentro del monolito el código puede igual dividirse en módulos**: eso es el *monolito modular*.
- Las únicas API definidas suelen estar entre la **UI y el servidor** (REST/HTTP), entre el **servidor y la BD** (lenguaje de consulta) y entre el **servidor y sus dependencias externas**.

| Pros | Contras |
|---|---|
| Fácil de desarrollar, sobre todo desde cero | Se vuelve una aplicación más compleja con el tiempo |
| Fácil de probar | **Si falla, falla todo** ("todo o nada") |
| Simple de escalar con un balanceador de carga, replicando la app completa | Difícil de mantener y depurar |
| Pocos puntos de falla, porque no depende de nada más | Cambiar una característica obliga a **redesplegar toda la aplicación** |
| Autónoma | Al crecer aumenta la complejidad y se dificulta el escalado |
| **Performance**: procesamiento local, sin llamadas distribuidas | **Escalar un módulo requiere escalar toda la aplicación** |
| | **Anclado a un stack tecnológico** |

### 14.2 Arquitectura de Microservicios (MSA)
- **Origen**: se discutió en un workshop de arquitectos en Venecia en 2011, y en mayo de 2012 se eligió el nombre "Microservicios".
- La usan Amazon, Netflix (alrededor de **700 microservicios**: historial, cobro, streaming de video, recomendaciones…), Spotify y Uber.
- **Definición**:
  - La aplicación se compone de componentes **pequeños, independientes y poco acoplados**, y cada uno actúa como una aplicación o servicio independiente.
  - Cada servicio puede funcionar solo, pero interactúa con otros cuando hace falta.
  - Es un **tipo de SOA**.
  - Cada servicio controla **una función única con sus propios datos**: **cada servicio tiene su propia BD**. Esto puede generar datos redundantes, pero asegura bajo acoplamiento.
  - Cada servicio puede estar en un **lenguaje distinto** y usar **otro tipo de BD**.
  - "Micro" significa que cada servicio se enfoca en hacer **una sola tarea**.
  - Se comunican por **API**, normalmente REST y a menudo a través de un **API Gateway**.
- Newman la define así: *"servicios desplegables de forma independiente, modelados alrededor de un dominio de negocio"*. Su concepto clave es el **despliegue independiente**: para lograrlo hacen falta bajo acoplamiento, contratos estables y **no compartir bases de datos** (ocultamiento de información y dueño de su propio estado).

**Características (cátedra / Fowler):**
- **Organizado en torno a capacidades de negocio.**
- **Productos, no proyectos**: el principio de Amazon "*you build it, you run it*", donde el equipo es responsable total del software en producción.
- **Gestión de datos descentralizada**: persistencia o bases de datos **políglotas**.

**Ventajas:**
- Alta escalabilidad
- Agilidad
- Facilidad de despliegue
- Testeabilidad
- Facilidad de desarrollo
- Reusabilidad
- Interoperabilidad

**Desventajas (Blancarte):**
- **Performance**, por la latencia de red.
- **Múltiples puntos de falla.**
- **Trazabilidad** difícil, porque los logs están distribuidos.
- Exige **madurez del equipo**.
- Además, aparece el riesgo del anti-patrón *Golden Hammer* ("si todo lo que tenés es un martillo, todo parece un clavo").

**Tamaño de un microservicio:** la definición **no dice nada sobre el tamaño**. En la práctica varían muchísimo: algunos ocupan a un equipo entero y otros tienen unos cientos de líneas. **El tamaño no es parte de la definición.**

**¿Cuándo usar microservicios?**
- **Evolución por pequeños cambios**: empezar con un **monolito** de proceso único y **refactorizar iterativamente**, convirtiendo **un módulo por vez** en microservicio. Es lo que recomienda Martin Fowler ("Monolith first").
- Cuando se necesita **despliegue independiente sin tiempo de inactividad**.
- Cuando se quieren **usar distintas tecnologías de BD** para particionar los datos.
- Cuando se busca **mejorar la autonomía de los equipos** distribuyendo responsabilidades.

### 14.3 Monolítico vs Microservicios (cuadro de la cátedra)
| Aspecto | Microservicios | Monolítica |
|---|---|---|
| **Implementación** | Difícil: es un sistema distribuido complejo, aunque implementar cada servicio por separado es fácil | Fácil al principio, pero se vuelve compleja al crecer |
| **Despliegue** | **Fácil**: cada módulo se despliega por separado, lo que facilita CI/CD | **Difícil** al crecer, porque cualquier cambio exige desplegar todo y se vuelve lento |
| **Escalabilidad** | **Fácil**: se escala cada servicio de forma individual | **Difícil**: escalar un componente implica escalar todo |
| **Administración** | **Descentralizada** | **Centralizada** |
| **Orientación a negocio** | **Sí** | Levemente |
| **Mantenimiento** | La descentralización aumenta la complejidad, pero son más fáciles de mantener y depurar | Comunicar los problemas es fácil por ser centralizado, pero mantener una aplicación muy grande es difícil |
| **Combinación con otros servicios** | Fácil: los servicios externos interactúan con módulos individuales | Difícil, porque es una entidad única fuertemente empaquetada |
| Código, procesos e infraestructura | **Independientes** | **Compartidos** |

**Cubo de escalado (Richardson / *The Art of Scalability*):**
- **Eje X**: clonar instancias idénticas detrás de un balanceador. Es la forma típica de escalar un monolito.
- **Eje Z**: instancias idénticas, pero cada una atiende un subconjunto de los datos según un atributo del request (*sharding*).
- **Eje Y**: **descomposición funcional en servicios**. Esto es lo que hacen los microservicios.

---

## 15. Estructura interna: Layered y Hexagonal

### 15.1 Arquitectura Estratificada o en Capas (Layered)
- Es tal vez el estilo **más común**, tanto que muchos asumen que todo sistema debería ser en capas. Por eso hay sistemas **forzados** a serlo.
- Organiza los componentes en una **serie de capas, cada una con una responsabilidad específica**.
- Según Fairbanks:
  - Cada capa es una **máquina virtual** para la capa superior.
  - La relación entre capas es de **"usa"**.
  - En la forma pura, una capa **solo usa la inmediata inferior**.
  - Las capas inferiores no conocen a las superiores, salvo mediante **callbacks**.
  - Promueve **modificabilidad, portabilidad y reusabilidad**.

**Capas comunes:**
- **Presentación**: entrada y salida del usuario, interfaz.
- **Lógica de negocio**, que se divide en:
  - **Capa de aplicación**: reglas y procesos que gobiernan el comportamiento.
  - **Capa de dominio**: datos y conceptos del negocio, reglas de negocio.
- **Acceso a datos**: acceso y manipulación de datos, normalmente en una BD.
- **Infraestructura**: SO, DBMS, protocolos de red.

Las capas genéricas de una aplicación son: Presentación (incluye la capa de servicios web), Lógica de Negocio y Administración de Datos.

🆕 **Complementos de los resúmenes:**
- Es un patrón de la **vista de módulo** y presenta una **vista estática** del software: cómo se organizan los componentes de código en paquetes o capas.
- Cada capa se comunica **solo con las adyacentes**, en forma **sincrónica**.
- Lo habitual son **3 capas básicas** (presentación, lógica de negocio y datos), o **4 si se agrega la capa de servicios web**.
- **¿Cuándo se usa?** Al construir sobre sistemas existentes, cuando el desarrollo se reparte entre varios equipos y cuando hay un requerimiento de seguridad multinivel.
- **Ventaja extra**: permite **sustituir capas completas** mientras se conserve su interfaz.
- **Excepción típica**: si el producto es web y de escritorio, la presentación de escritorio salta la capa de servicios web y habla directo con la de lógica de negocio.

**Variantes:**
- Permitir **saltar a capas inferiores**. Blancarte lo llama **capas abiertas** (se pueden saltar, pero hay que justificarlo) y **capas cerradas** (no se pueden saltar).
- **Capas compartidas o verticales**, que todas pueden usar. Estiran la definición de capa al límite y tienen sentido como conveniencia visual para mostrar la dependencia de un módulo compartido.
- **Nota**: la forma platónica varía mucho de la embebida. **Saltear capas niega los atributos de calidad** que son su beneficio. Aun así sigue siendo útil, porque agrupa módulos con funcionalidad coherente.

| Ventajas | Desventajas |
|---|---|
| **Fácil de desarrollar**: muy conocida y muy usada | **Performance**: pasar de capa en capa, sobre todo por red, la degrada |
| **Fácil de probar** cada capa por separado | **Escalabilidad**: al ser monolítica, se escala replicando toda la app (escalado monolítico) |
| **Fácil de mantener**: es fácil ubicar dónde está un defecto o dónde aplicar un cambio | **Complejidad de despliegue**: se despliega de abajo hacia arriba y un cambio chico puede exigir redesplegar todo |
| **Seguridad**: las capas pueden aislarse en subredes distintas | **Anclada a un stack tecnológico** |
| | **Tolerancia a fallos**: si falla una capa, las superiores fallan en cascada |

**Partición técnica vs. partición de dominio (Richards & Ford):**
- **Partición técnica**: los componentes de nivel superior se organizan por capacidad técnica (presentación, negocio, persistencia). Un flujo como "CatalogCheckout" termina **disperso en todas las capas**.
- **Partición de dominio**: los componentes de nivel superior se organizan por **flujos de trabajo o dominios**, inspirado en DDD.
  - Ventajas: se modela más cerca de cómo funciona el negocio, se alinea con el **monolito modular** y con los **microservicios**, el flujo de mensajes coincide con el dominio del problema y es fácil migrar a una arquitectura distribuida.
  - Desventaja: el código de personalización aparece en varios lugares.

### 15.2 Arquitectura Hexagonal (Puertos y Adaptadores, Alistair Cockburn)
- La lógica de negocio **define puertos** a los que se conectan **adaptadores** de forma **intercambiable**.
- El hexágono es solo un símbolo que deja espacio para dibujar muchos puertos y adaptadores.
- Crea una **capa de abstracción que protege el núcleo** y lo aísla de herramientas y tecnologías externas.

**Elementos:**
- **Aplicación (núcleo)**: es el hexágono. Contiene la lógica de negocio y de dominio, recibe comandos o consultas por los puertos y envía pedidos a actores externos (como una BD) también a través de puertos.
- **Puertos**: puntos de entrada y salida **independientes de la tecnología**, normalmente **interfaces** en el código. Definen cómo se comunican los actores externos con la aplicación y cómo se comunica la aplicación con sistemas externos (BD, brokers de mensajes, otras apps). **Son internos y los define el negocio.**
- **Adaptadores**: código externo que conecta un puerto con una tecnología concreta. Por ejemplo, un **controlador REST** es un adaptador. **Puede haber tantos adaptadores por puerto como haga falta** sin poner en riesgo los puertos ni la aplicación.
- **Edge**: maneja la comunicación entre el núcleo y los sistemas externos.

**Lado conductor vs. lado conducido (Driving vs. Driven):**
| | **Lado conductor (primario, izquierda)** | **Lado conducido (secundario, derecha)** |
|---|---|---|
| Quién | Actores que **inician** la interacción, por ejemplo un controlador que recibe la entrada del usuario | Actores a los que la aplicación **"impulsa a comportarse"**, por ejemplo un adaptador de BD |
| Puerto | La interfaz y su implementación (un servicio de aplicación) están **dentro** del hexágono | La interfaz está **dentro** del hexágono y la implementación está en el **adaptador, fuera** |
| Dependencia | El adaptador **usa** el puerto. Depende de una abstracción y no sabe quién reacciona | El servicio de aplicación **usa** el puerto y el adaptador lo **implementa**. Aquí está la **inversión de dependencia** |

**Inversión de dependencias:**
- La **"regla de dependencia"** dice que **todas las dependencias del código fuente apuntan hacia adentro**, hacia el núcleo.
- La dificultad aparece en los puertos secundarios: el núcleo tiene que llamar a la BD, que está afuera, sin depender de ella.
- La solución es el **Principio de Inversión de Dependencia (DIP)**: el núcleo define la interfaz del puerto y el adaptador de "bajo nivel" (el repositorio) **se ve obligado a implementar la abstracción definida por el núcleo**, que es de alto nivel.
- Así, la dependencia del código queda **opuesta a la dirección de la llamada**.

**¿Por qué usarla?**
- Aísla completamente la lógica de aplicación y de dominio, lo que la vuelve **totalmente testeable** y hace fácil *mockear* las dependencias.
- Las interfaces se diseñan **"por propósito" y no por tecnología**, así se evita quedar atado a un stack y este puede evolucionar.
- Cambiar la persistencia, o hacer que la llame un bot de Slack en lugar de un humano, solo requiere **un adaptador nuevo**.
- **Combina muy bien con DDD**, porque protege la lógica de dominio para que no se escape del núcleo.

**Beneficios:**
- Bajo acoplamiento
- Alta cohesión
- Testeabilidad
- Mantenimiento más fácil
- Mejor modelado del dominio
- Escalabilidad
- **Intercambiabilidad** de adaptadores
- Reusabilidad
- Legibilidad

**Conviene usarla cuando se necesita:**
- integrarse con **múltiples sistemas externos**
- manejar un gran volumen de datos
- escalar horizontalmente
- **probar exhaustivamente**

### 15.3 Hexagonal vs. Layered
| Layered | Hexagonal |
|---|---|
| Se la critica por estar **centrada en la base de datos** (*database-driven design*): la app se planifica alrededor de la BD | **Pone la lógica de negocio por encima de la BD**: primero se diseña el negocio y la persistencia se piensa después |
| Problemas: dependencias transitivas, límites de capa borrosos, aislamiento pobre, dificultad para probar, mantener e intercambiar infraestructura | **Los cambios del negocio impulsan los cambios en la persistencia, y no al revés** |
| Suele generar más dependencias y **duplicar lógica de negocio** | Cuando se suman API REST o servicios de terceros, da **pautas claras de dónde va cada componente** |

---

## 16. Distribución: Cliente/Servidor y N-Tier

Variantes de cliente/servidor:
- Two-tier
- Three-tier
- N-tier
- Master-slave
- Peer-to-peer

### 16.1 Cliente/Servidor (dos capas)
- La aplicación se modela como un **conjunto de servicios que proveen los servidores** y un conjunto de **clientes que los usan**.
- **Los clientes conocen a los servidores** y deben saber cómo encontrarlos. **Los servidores no necesitan conocer a los clientes** y no conocen su identidad hasta que estos los contactan.
- **Los clientes inician la comunicación.** Hay una relación **asimétrica**.
- Clientes y servidores son **procesos lógicos**: aunque es un estilo distribuido, **puede correr en una sola computadora**.
- Como los servidores pueden replicarse, se usa **cuando la carga es variable**.
- Su uso más común es aprovechar la **capacidad gráfica de la PC** y **proteger los datos en un host central**.
- Fairbanks agrega:
  - Variantes: conexión síncrona o asíncrona, con o sin estado, topología estática o dinámica. Un caso es IMAP, donde el servidor envía actualizaciones solo después de que el cliente lo contactó.
  - Promueve **mantenibilidad, evolucionabilidad e integración de sistemas legacy**, ya que los cambios de reglas se hacen en un solo lugar: el servidor.

| Ventajas | Desventajas |
|---|---|
| **Centralización**: los clientes no se quedan con información desactualizada | **Actualizaciones** de los clientes pesados: hay que actualizar muchas terminales |
| **Seguridad**: el servidor está protegido por firewall o subredes | **Concurrencia**: demasiados usuarios simultáneos complican al servidor |
| **Instalación fácil** del cliente | **Todo o nada**: si cae el servidor, todos los clientes quedan inoperables |
| **Separación de responsabilidades**: la lógica de negocio está separada del cliente | **Protocolos de bajo nivel** (sockets, HTTP, RPC) |
| **Portabilidad**: cada parte puede correr en una plataforma distinta | **Depuración** difícil, porque los clientes están distribuidos |

🆕 **Complementos de los resúmenes:**
- Es **sincrónico**. Los **roles son fijos**: el que es cliente siempre es cliente y el que es servidor siempre es servidor. Un ejemplo es el servidor de cola de impresión.
- El C/S "puro" sirve cuando hay **dos capas**. Para más capas surge **N-Tier**, que es su **versión embebida**.
- En **N-Tier**, la capa de presentación es **cliente puro**, la última es **servidor puro** y **las intermedias son mixtas** (son cliente de la de abajo y servidor de la de arriba). En otras palabras, *los extremos son puros y las capas intermedias son mixtas*.
- Ejemplo de aplicación: un **banco en línea**.

**Evolución de C/S según la época:**
1. Escritorio + servidor de BD.
2. Navegador + servidor web, que a su vez se conecta a un servidor de BD.
3. Tres niveles: BD + servidor de aplicaciones + frontend en HTML y JavaScript.

**Formas de implementar C/S:**
- **Acceso a datos remoto**: el cliente tiene la presentación y la lógica, y el servidor tiene los datos.
- **Presentación remota**: el cliente solo presenta y el servidor tiene la lógica y los datos.
- **Lógica dividida**: la funcionalidad se reparte entre cliente y servidor. Es la **más compleja**, porque ambos necesitan programas compilados por separado y hay que definir qué funciones van en cada lado y qué diálogos de comunicación existen entre ellos.

### 16.2 N-Tier
- Son **varias instancias de cliente/servidor encadenadas** formando niveles (tiers).
- Los pedidos fluyen **en una sola dirección**.
- Cada nivel tiene una responsabilidad exclusiva. El caso típico de 3 niveles es: UI (cliente) → lógica de negocio (servidor de la UI y cliente de la persistencia) → persistencia.
- Es un híbrido entre la vista de runtime y la de asignación: los tiers son agrupaciones lógicas que suelen, aunque no siempre, asignarse a hardware distinto.

**Propiedades clave (Gorton):**
- **Separación de intereses**: presentación, negocio y datos en niveles distintos.
- **Comunicación sincrónica**: pedido-respuesta. Los pedidos van en una dirección (cliente → web → negocio → EIS) y cada nivel espera la respuesta del otro.
- **Distribución flexible**: todos los niveles pueden correr en la misma máquina o cada uno en la suya. En la web, el cliente suele ser el browser.

**Atributos de calidad (Gorton):**
- **Disponibilidad**: los servidores de cada nivel se replican.
- **Manejo de fallas**: *failover* transparente.
- **Modificabilidad**: alta, por la separación de intereses.
- **Performance**: probada. Hay que minimizar las llamadas entre niveles.
- **Escalabilidad**: replicando servidores se escala hacia arriba (up) y hacia afuera (out). El nivel de datos suele ser el cuello de botella.
- Es apropiado para **muchos clientes y requests cortos**.

---

## 17. Comunicación

### 17.1 Event Driven Architecture (EDA)
- Es una arquitectura **asíncrona y distribuida**, pensada para aplicaciones **altamente escalables**.
- Las aplicaciones lanzan **eventos** y otros componentes **reaccionan** a ellos, los procesan y posiblemente generan nuevos eventos.
- **Evento**: un *cambio significativo en el estado* de la aplicación (un dato que cambió o una acción) que merece ser observado.
- El sistema "reacciona" a eventos de la UI o internos, mediante **procesadores de eventos**.

**Dos topologías:**
| **Broker** | **Mediator** |
|---|---|
| **Sin control central** del procesamiento | **Con algún control central** |
| Cada procesador emite sus propios eventos y activa a otros, formando una **cadena de eventos** | Los **mediadores de eventos** deciden **qué procesadores se llaman y en qué orden** |
| No hace falta administrar las conexiones entre procesadores | Puede haber varios mediadores, en paralelo o en jerarquía |
| 🆕 Se usa para **eventos simples**: los procesadores esperan eventos para tomarlos y procesarlos | 🆕 Se usa para **eventos complejos**, que requieren un orden de ejecución |

🆕 **Pregunta de parcial: "Mencione patrones que planteen comunicaciones asíncronas y describalos, indicando cuándo se recomienda cada uno".**
Hay que responder con EDA y sus variantes:
- **Messaging**: cuando la conectividad es transitoria y no se necesita una respuesta inmediata.
- **Publish & Subscribe**: notificaciones de uno a muchos.
- **Broker**: cuando hacen falta transformaciones de formato.
- **Process Coordinator**: procesos de negocio complejos de varios pasos. Puede ser síncrono o asíncrono.

**Ejemplos de aplicación** (resumen manuscrito):
- **EDA**: un sistema de monitoreo de sensores, que envían eventos a un bus.
- **Pub/Sub**: YouTube (suscripciones).
- **Messaging**: un banco o el procesamiento de pedidos. Los servidores de mail funcionan así.
- **Broker**: mensajería con conversión de formatos (por ejemplo, el *lector baqueano*).
- **Process Coordinator**: un sistema de reservas o de expedientes judiciales.

| Ventajas | Desventajas |
|---|---|
| **Escalabilidad**: cada consumidor escala por separado con acoplamiento mínimo | **Testeabilidad**: no alcanza con generar un evento y esperar el resultado. Hay que esperar toda la cadena de eventos derivados |
| **Despliegue**: por el bajo acoplamiento, sin dependencias ni precondiciones | **Desarrollo**: el código asíncrono es complicado y requiere manejadores de errores avanzados. La falla de un procesador no detiene al resto, lo que puede generar **inconsistencias** |
| **Performance**: cada paso agrega retraso, pero la asincronía permite **procesamiento en paralelo** | |
| **Flexibilidad**: cada procesador tiene una única responsabilidad y está desacoplado. Un requerimiento nuevo se resuelve con un procesador nuevo | |

**Variantes de EDA vistas:** Publish & Subscribe, Broker, Messaging y Process Coordinator.

### 17.2 Publicar-Suscribir (Publish & Subscribe)
- Componentes independientes **publican** eventos en un **tópico** y otros **se suscriben** a él.
- El publicante **no sabe para qué ni quién** recibe el evento, y el suscriptor **no sabe quién lo publicó**: **depende solo del evento**.
- Un tópico puede tener **varios publicantes**, que aparecen y desaparecen dinámicamente. Los suscriptores se suscriben y desuscriben dinámicamente.
- **Aplica a elementos de tiempo de ejecución**, así que es parte de la vista de ejecución (runtime).

**Propiedades:**
- **Mensajería muchos a muchos.**
- **QoS configurable**: entrega confiable o no confiable, y **punto a punto** (un mensaje distinto por suscriptor) o **multicast/broadcast** (un mensaje que reciben todos).
- **Bajo acoplamiento.**

**Consideraciones:**
- Es muy flexible para mensajes asíncronos **uno-a-muchos, muchos-a-uno y muchos-a-muchos**.
- Su gran beneficio es que **desacopla a productores de consumidores**, lo que lo hace **mantenible y evolucionable**.
- El **bus de eventos agrega indirección**, lo que puede afectar la performance, aunque los productos existentes están bien optimizados.
- Los canales o buses pueden ser:
  - **Durables**: no pierden mensajes ante una falla, pero agregan latencia.
  - **Con entrega en orden** o priorizada.
  - **Con entrega en lotes**.
- Fairbanks destaca que en este estilo **"el conector es la estrella"**.
- 🆕 Hay **dos formas** de recibir las notificaciones:
  - **Push**: la notificación le llega al suscriptor sin que la pida, aunque la app no esté activa. **Interrumpe.** Es típico de las apps mobile.
  - **Pull** (en el resumen aparece como "pool"): no interrumpe. Avisa que hay algo nuevo y el usuario lo consulta si quiere.
- 🆕 **Si hace falta disponibilidad, se replica el tópico.**
- 🆕 La difusión de muchos a muchos (multicast) es más rápida y pareja que el envío punto a punto, donde a algunos suscriptores les llega antes que a otros.
- 🆕 Si un componente no se suscribe primero al tópico, no recibe la información.
- 🆕 Desventaja: el bus de eventos agrega una indirección que puede afectar la performance.

### 17.3 Agente (Broker)
- **Hub-and-spoke**: el broker es el concentrador y los remitentes y receptores se conectan como rayos, mediante **puertos asociados a un formato de mensaje**.
- **Ruteo de mensajes**: entrega lo que llega a un puerto de entrada en el puerto de salida que corresponde.
- **Transformación de mensajes**: convierte el formato de origen en el formato de destino.
- **Es adecuado cuando los mensajes requieren grandes transformaciones** entre formatos. Desacopla al remitente del receptor, cada uno usa su formato nativo, y **centraliza la lógica de transformación**.
- Según Gorton, puede volverse **cuello de botella** y su throughput es menor que el de la mensajería simple. Para alta disponibilidad hay que replicarlo.
- 🆕 Además de **formatear**, tiene una función de **enrutamiento**: dirige los datos al destino correcto.
  - Puede tener **varios puertos de salida con formatos distintos**, según lo que necesite cada componente.
  - Se usa típicamente para la **comunicación con servidores o dispositivos externos**. En FF son la importación del minuto a minuto de DataFactory y los archivos de Rapipago y Pago Fácil.

### 17.4 Mensajería (Messaging)
- **Desacopla al emisor del receptor mediante una cola intermedia** (MOM).
- El emisor sabe que el mensaje **eventualmente se entregará**, aunque la red esté caída o el receptor no esté disponible.
- Tiene **bajo acoplamiento**, lo que da alta modificabilidad.
- Su problema es que un cambio en el formato del mensaje puede obligar a cambiar el otro extremo. Los **mensajes auto-descriptivos** reducen esa dependencia.

**Propiedades:**
- **Comunicación asíncrona**: el cliente deja el mensaje en la cola y sigue sin esperar.
- **QoS configurable**: rápida y no confiable, o lenta y confiable. Se puede coordinar con transacciones de la BD.
- **Bajo acoplamiento.**

**Consideraciones:**
- Es una solución resistente cuando la **conectividad es transitoria**, por la red o porque los servidores son poco confiables.
- Se puede implementar pedido-respuesta sincrónico con un **par de colas pedido-respuesta**.
- Si el procesamiento se divide en pasos conectados por colas, el resultado equivale a *pipes & filters*.
- 🆕 Puede configurarse como sincrónico, pero no tiene sentido tener una cola sincrónica. **Para tener redundancia, el elemento que se replica es la cola.** Los servidores de mail usan este patrón.

### 17.5 Coordinador de Proceso (Process Coordinator)
- **Encapsula el proceso**: el coordinador contiene la secuencia de pasos de un proceso de negocio, que puede ser arbitrariamente compleja. Es el **único punto de definición** del proceso: recibe el pedido de inicio, llama a los servidores en orden y emite el resultado.
- **Bajo acoplamiento**: los servidores **no conocen su rol** en el proceso ni el orden de los pasos. Solo ofrecen servicios.
- **Comunicación flexible**: síncrona (espera la respuesta) o asíncrona (con callback o cola de respuesta).
- Se usa para **procesos de negocio complejos que invocan a varios servidores**. Como la lógica está en un solo lugar, es más fácil cambiar, administrar y monitorear el proceso.
- Según Gorton:
  - El coordinador es un **punto único de falla**, así que hay que replicarlo.
  - El **manejo de fallas es complejo**, porque puede requerir **transacciones compensatorias**.
  - La performance queda limitada por el **paso más lento**.
- 🆕 Su gran ventaja es que **si cambia la lógica del proceso, se cambia en un solo lugar**. Su desventaja es que puede volverse un **cuello de botella**.
- 🆕 **Replicar el coordinador** admite dos configuraciones:
  1. Con los dos activos se **duplica la capacidad de procesamiento** y mejora el tiempo de respuesta.
  2. Con uno en espera (*standby*) se gana **tolerancia a fallos**, pero no velocidad.
- 🆕 Ejemplos: sistemas judiciales o de expedientes, auditorías médicas de obras sociales, sistemas de reservas.

### 17.6 Request-Response y REST
- **Request-Response**: define la interacción entre sistemas. El cliente envía una solicitud y el servidor devuelve la respuesta correspondiente.
- **REST (Representational State Transfer)**: es un **conjunto de restricciones arquitectónicas** que forman un estilo para sistemas distribuidos.
  - Surgió como alternativa a SOAP y a los web services basados en WSDL.
  - Ignora los detalles de implementación y la sintaxis del protocolo, y se enfoca en los **roles de los componentes**, las restricciones de interacción y la **representación de los datos**.

**Estructura de REST:**
- **Datos**: se transmiten **a donde serán procesados**, en lugar de enviarse ya procesados.
- **Conectores**: interfaces abstractas que encapsulan el acceso a los recursos y la transferencia. Las solicitudes son **sin estado**.
- **Componentes**: usan los conectores, por ejemplo los navegadores, los web servers (Apache, IIS) y los componentes intermedios, que actúan como cliente y servidor a la vez.

**Principios rectores (restricciones) de REST:**
1. **Cliente-Servidor**: separación de intereses, para que cliente y servidor evolucionen de forma independiente.
2. **Sin estado (stateless)**: cada request contiene toda la información necesaria y el servidor no usa contexto guardado.
3. **Almacenable en caché**: cada respuesta se etiqueta como cacheable o no.
4. **Interfaz uniforme**: aplica el principio de generalidad, lo que simplifica la arquitectura y da visibilidad.
5. **Sistema en capas (layered)**: capas jerárquicas que restringen el comportamiento de los componentes.
6. **Código bajo demanda** (opcional): el cliente puede descargar y ejecutar scripts.

| Ventajas | Desventajas |
|---|---|
| **Interoperable**: se puede usar desde cualquier lenguaje | **Performance**: cada request tiene un costo de latencia, conexión y autenticación |
| **Escalable**: al ser sin estado, se agregan nodos fácilmente | **Más puntos de falla** |
| **Bajo acoplamiento**: interfaces abstractas y recursos accedidos por URL | **Gobierno**: hay que ordenar los servicios para evitar que distintos equipos dupliquen funcionalidad |
| **Testeable**: cada recurso se prueba de forma aislada | |
| **Reutilizable**: envía datos crudos y el cliente decide cómo representarlos | |

Fairbanks aclara que **REST puede verse como un *patrón*** dentro de un estilo cliente/servidor, porque restringe el formato de los mensajes.

---

## 18. Otros estilos (Fairbanks / Blancarte)
Estos estilos no los desarrolla la cátedra, pero aparecen en la bibliografía:

| Estilo | Vista | Restricción clave | Atributos que promueve |
|---|---|---|---|
| **Pipe-and-Filter** | Runtime | Filtros independientes que se comunican solo por pipes, con procesamiento **incremental**. Por ejemplo `cat | grep | cut` | Reconfigurabilidad, reusabilidad, concurrencia. No sirve para aplicaciones interactivas |
| **Batch-Sequential** | Runtime | Cada etapa procesa **todo** su input antes de pasar al siguiente | Reusabilidad, modificabilidad, throughput |
| **Model-Centered** (repositorio, datos compartidos) | Runtime | Las vistas y controladores interactúan **solo a través del modelo central** (MVC, blackboard) | Modificabilidad, extensibilidad, concurrencia |
| **Peer-to-Peer** | Runtime | Nodos **igualitarios**: cada uno puede ser cliente y servidor | Disponibilidad, resiliencia, escalabilidad. No tiene punto único de falla |
| **Map-Reduce** | Runtime + asignación | Un master y workers de map y reduce sobre un dataset divisible | Escalabilidad, performance, disponibilidad |
| **Mirrored / Rack / Farm** | Asignación | Hardware duplicado, racks o granjas de servidores | Performance, disponibilidad |
| **Microkernel** (Blancarte) | — | Un núcleo mínimo más plugins | Extensibilidad |
| **SOA** (Blancarte) | — | Servicios empresariales, a menudo con un bus (ESB) | Reuso, integración |

### 🆕 18.1 Estilos que aparecen en los resúmenes (con sus calificaciones, según *Fundamentals*)
| Estilo | Descripción | Ventajas | Desventajas |
|---|---|---|---|
| **Pipeline** (pipes & filters) | Pipes que transportan datos en una sola dirección y filtros que los procesan | Bajo costo, simplicidad, modularidad | Tolerancia a fallos, escalabilidad |
| **Microkernel** | Un núcleo de funcionalidad mínima más **plugins** que aportan la funcionalidad (los plugins son lo más importante) | Bajo costo, rendimiento, simplicidad, testing | Tolerancia a fallos, escalabilidad |
| **Partición de dominio** (monolito modular) | Módulos organizados por **contexto de negocio** dentro de un monolito | Modificabilidad, simplicidad, mantenimiento, adaptación al cambio | Código repetido, escalabilidad |
| **Basada en servicios** | Servicios independientes y autosuficientes, parecidos a los microservicios, pero **que comparten una misma BD** | Desplegabilidad, tolerancia a fallos, resiliencia, escalabilidad | — |
| **Basada en espacios** (*space-based*) | Varios procesadores en paralelo que comparten recursos en memoria y reparten la carga | Elasticidad, rendimiento, escalabilidad, resiliencia | Simplicidad, testing, costo |
| **EDA** (Fundamentals) | Responde a eventos significativos en lugar de conectar componentes directamente | Escalabilidad, evolución, tolerancia a fallos, rendimiento, modularidad | Simplicidad, testing |
| **Microservicios** (Fundamentals) | Ver el §14 | Desplegabilidad, elasticidad, evolución, tolerancia a fallos, escalabilidad, resiliencia, testing | Simplicidad, rendimiento, comunicación, costo |
| **P2P** | Cada nodo es cliente y servidor a la vez. No hay servidor central, y cuantos más nodos hay, más "servidores" hay | Disponibilidad, escalabilidad, descentralización, tolerancia a fallos | Rendimiento, simplicidad, seguridad |
| **Master-Slave** | Un maestro coordina y crea procesos hijos que toman parte de la carga | — | — |
| **MVC** | Sirve tanto en arquitecturas monolíticas como distribuidas. Separa **Modelo** (entidad y lógica de negocio), **Vista** (lo visual) y **Controlador** (cómo reacciona la interfaz al usuario e intermedia entre los otros dos) | Los datos cambian independientemente de su representación y se admiten varias vistas de los mismos datos | Agrega código y complejidad si el modelo es simple |

**¿Cuándo conviene P2P?** Para compartir archivos, para streaming y en las criptomonedas.

#### 🆕 SOA (Service Oriented Architecture), que aparece como pregunta de parcial
- Es un estilo para **arquitecturas distribuidas** cuyos objetivos principales son la **reutilización** y la **interoperabilidad**.
- Organiza la aplicación en **servicios** que cumplen tareas de negocio y que se construyen con **estándares abiertos**, así que se comunican sin importar la tecnología de cada uno.
- Cada servicio (web service) expone un **contrato (WSDL)** con sus operaciones y tipos de datos (**XSD / XML Schema**). Usa XML (o JSON) sobre HTTP.
- La comunicación suele coordinarse con un **Service Bus (ESB)**, que actúa de intermediario y reduce el acoplamiento.

| Ventajas | Desventajas |
|---|---|
| **Interoperabilidad** entre tecnologías distintas | **Performance**: la composición de varios servicios agrega latencia |
| **Reutilización** de servicios en tareas de negocio más grandes | **Múltiples puntos de falla**: si falla un servicio, falla la tarea completa |
| **Escalabilidad** selectiva | **Volumetría**: no sirve para grandes volúmenes de datos en tiempo real |
| **Testeabilidad** de cada servicio | **Gobierno**: hay que evitar servicios redundantes. Hace falta un gobierno estricto |
| **Bajo acoplamiento** gracias a los contratos | Mayor complejidad en la seguridad y en la gestión de identidad |

**SOA vs. Microservicios:**
| SOA | Microservicios |
|---|---|
| Servicios **más grandes**, que representan funciones de negocio completas | Servicios **muy pequeños**, con una sola responsabilidad |
| Suele depender de un **ESB** | Se comunican con **APIs livianas** (REST) |
| Busca **integrar sistemas empresariales heterogéneos** | Busca **agilidad, escalabilidad e independencia tecnológica** |
| Pueden compartir datos o BD | **Cada uno tiene su propia BD** |

#### 🆕 Ejemplos de aplicación de cada estilo (resumen manuscrito)
| Estilo | Ejemplo |
|---|---|
| Layered | Una app web dividida en presentación, lógica y BD que se comunican de forma adyacente |
| Partición de dominio | Un e-commerce dividido en los dominios de pedidos y pagos, cada uno como servicio independiente |
| C/S | Un banco en línea |
| EDA | Monitoreo de sensores mediante un bus de eventos |
| REST | Una app web RESTful que responde en JSON o XML |
| Microservicios | Un e-commerce con un microservicio de pedidos y otro de pagos |
| SOA | Una empresa con varios sistemas que comparten datos mediante servicios |
| MVC | Una app web |
| Pub/Sub | YouTube |
| Broker | Mensajería con conversión de formatos |
| Process Coordinator | Un sistema de reservas |
| Messaging | Un banco o un sistema de procesamiento en tiempo real |

**Patrones arquitectónicos que trata Blancarte:**
- DTO
- DAO
- Polling
- Webhook
- Load Balance
- Service Registry
- Service Discovery
- **API Gateway**
- Access Token
- SSO
- Store and Forward
- **Circuit Breaker**
- Log Aggregation

---

## 19. Patrones para Microservicios (Richardson)

Los patrones se organizan en **tres capas**:
- **Patrones de Infraestructura**: aspectos por fuera del desarrollo, como el despliegue.
- **Aplicación de infraestructura**: infraestructura que también impacta en el desarrollo.
- **Patrones de Aplicación**: problemas que enfrentan los desarrolladores, como la descomposición o los datos.

### 19.1 Patrones de Descomposición (capa de aplicación)
- **Descomposición por capacidad de negocio**: una capacidad es *lo que hace* la organización. Es estable en el tiempo, aunque cambie *cómo* se hace. Por ejemplo, "depositar cheque" se hizo en ventanilla, luego en cajero y hoy desde el celular.
- **Descomposición por subdominio (DDD)**: cada subdominio se mapea a un servicio con **su propio modelo de dominio**. En el ejemplo FTGO, cada subdominio pasa a ser un servicio.

**Conceptos de DDD:**
- **Subdominio**: parte del dominio, según una descomposición conceptual.
- **Contexto acotado (Bounded Context)**: el ámbito operacional donde un modelo particular es aplicable y está bien definido (normalmente un subsistema). **Cada contexto acotado es un servicio o un conjunto de servicios.**
- **Tipos de subdominio:**
  - **Core (núcleo)**: donde se hace la inversión estratégica. Es prioritario y **distingue a la organización de la competencia**.
  - **Soporte**: facilita el core. Es necesario y específico del negocio.
  - **Genérico**: no es central y es común a muchos negocios. Puede **tercerizarse o comprarse**.
- **Diseño estratégico**: alinear cada contexto acotado con un subdominio.

**Guía para trabajar con DDD:**
1. Identificar el dominio principal.
2. Definir los contextos acotados con límites claros. La funcionalidad de un servicio debe limitarse a un único contexto.
3. Establecer un **lenguaje ubicuo**, compartido entre los expertos del dominio y los desarrolladores.

**Principios de diseño (Richardson):**
- **SRP** (Single Responsibility Principle): "una clase o servicio debe tener una sola razón para cambiar".
- **CCP** (Common Closure Principle): lo que cambia por la misma razón va en el mismo servicio. Es el **antídoto contra el "monolito distribuido"**.

**Criterios para validar los servicios identificados:**
- Cada servicio tiene una **única responsabilidad**.
- Dividir la funcionalidad en dos servicios **no genera comunicación excesiva**. Si la genera, probablemente pertenecen al mismo servicio.
- Cada servicio tiene un **tamaño manejable** por un equipo pequeño.
- **Ninguna interdependencia obliga a desplegar dos o más servicios a la vez.**
- Los servicios están **débilmente acoplados** y evolucionan de forma independiente.
- Los límites **no comprometen la coherencia ni la integridad de los datos**.
- **Ante la duda, empezar con microservicios más gruesos**, porque dividir un servicio es más fácil que reubicar funcionalidad entre servicios existentes.

**Obstáculos para descomponer (Richardson):**
- Latencia de red.
- Menor disponibilidad por la comunicación síncrona.
- Mantener la consistencia de datos entre servicios. Se resuelve con **Sagas**: secuencias de transacciones locales coordinadas por mensajes, con consistencia eventual.
- Obtener una vista consistente de los datos.
- Las **God classes**, como "Order", que concentra todo.

### 19.2 Patrón API Gateway
- **Problema**: ¿cómo acceden los clientes a los servicios individuales?
- **Desafíos:**
  - La granularidad de los servicios no coincide con la que necesita el cliente.
  - Distintos clientes necesitan datos distintos.
  - El rendimiento de red varía según el tipo de cliente.
  - La partición en servicios cambia con el tiempo y debe ocultarse.
  - Algunos protocolos internos no son compatibles con la web.
- **Solución**: un **punto de entrada único** para todos los clientes. Algunos requests los **enruta** directamente y otros los **distribuye** entre varios servicios (composición).
  - Puede exponer **una API distinta para cada cliente**.
  - Puede implementar **seguridad** (autorización).
  - Se encarga del **enrutamiento, la composición y la traducción de protocolos**.
  - Puede **enmascarar fallas** devolviendo datos cacheados o por defecto.
  - **Oculta la ubicación y los detalles de implementación** de los servicios.
  - **No alberga lógica de negocio, orquestación ni mediación.**

| Beneficios | Desventajas |
|---|---|
| Aísla a los clientes de cómo se divide la aplicación | **Mayor complejidad**: es otra pieza a desarrollar, desplegar y gestionar |
| Aísla a los clientes de la ubicación de las instancias | **Mayor tiempo de respuesta**, por el salto de red extra |
| Ofrece la API óptima para cada cliente | |
| **Reduce la cantidad de requests de ida y vuelta**, lo que mejora la experiencia del usuario | |
| Es esencial para las apps móviles | |
| Simplifica el cliente | |
| Traduce de un protocolo web estándar a los protocolos internos | |

- **¿Por qué usarlo?** Por **seguridad**, **escalabilidad** (gestión de tráfico, caché), **interoperabilidad** (mediación y transformación de formatos), **monitoreo y registro** centralizados, y **monetización** de las APIs.
- **Variante Backends for Frontends (BFF)**: **un API Gateway separado para cada clase de cliente** (web, móvil, etc.).

### 19.3 Patrón Una Base de Datos por Servicio (Database per Service)
- **Problema**: ¿cómo manejar la BD en microservicios?
- **Solución**: cada servicio maneja **sus datos en forma privada** y solo se accede a ellos **a través de su API**. La BD es parte de la implementación del servicio y **no puede accederse directamente desde otros servicios**.
- Ejemplo del flujo de un pedido en FTGO:
  1. El cliente hace el pedido.
  2. El servicio de Pedidos crea el pedido.
  3. El servicio de Pagos cobra y envía el acuse de recibo.
  4. El servicio de Restaurante prepara el pedido y envía el acuse.
  5. El servicio de Entrega organiza la entrega y envía el acuse.
- **Beneficios**: bajo acoplamiento y la posibilidad de **usar la BD más conveniente** para cada servicio.
- **Desventajas**: implementación compleja y la complejidad de manejar a la vez BD SQL y NoSQL.

### 19.4 Patrones de Despliegue (capa de infraestructura)
**Fuerzas comunes** que estos patrones buscan resolver:
- Los servicios están escritos en distintos lenguajes y frameworks.
- Tienen múltiples instancias.
- Deben escalarse y desplegarse de forma independiente.
- Las instancias deben estar aisladas.
- Hay que construir y desplegar rápido.
- Hay que poder limitar CPU y memoria.
- Hay que monitorear cada instancia.
- El despliegue debe ser confiable y lo más rentable posible.

| Patrón | Solución | Beneficios | Desventajas |
|---|---|---|---|
| **Single Service per Host** | Cada instancia en su **propio host** | Instancias **aisladas**, sin conflictos de recursos ni de versiones de dependencias. Consumo acotado a un host. Fácil de monitorear y redesplegar | **Uso de recursos menos eficiente**, porque hay más hosts |
| → **Service Instance per VM** | Empaquetar el servicio como **imagen de VM** | Fácil de escalar con más instancias. La VM encapsula la tecnología. Hay aislamiento y límites de CPU y memoria. Se apoya en IaaS madura (AWS: balanceador elástico, autoscaling) | **Construir la imagen de VM es lento** |
| → **Service Instance per Container** | Empaquetar el servicio como **imagen de contenedor (Docker)** | Fácil de escalar. Encapsula la tecnología, tiene aislamiento y límites. **Muy rápido de construir e iniciar**, porque arranca solo el proceso y no un SO completo | La infraestructura de contenedores es **menos rica** que la de VMs |
| **Multiple Services per Host** | Varias instancias de distintos servicios en un mismo host: un proceso JVM por instancia (por ejemplo Tomcat) o varias en la misma JVM (WARs, OSGI) | **Uso de recursos más eficiente** y despliegue rápido | Riesgo de **conflictos de recursos y de versiones de dependencias**. Es difícil limitar y monitorear los recursos de cada instancia. **Es imposible aislar cada instancia** |

---

## 20. Caso de estudio: App Fanáticos del Fútbol

**Objetivo del producto**: gestionar los fixtures de los campeonatos en que participa el club, el cobro de adhesiones de socios, los beneficios para socios, los sorteos y sus ganadores, y las noticias del club.

**Alcances:**
- Administración de campeonatos, clubes, usuarios y perfiles, sorteos, beneficios, comercios adheridos y socios.
- Gestión de noticias y su publicación, solicitudes de adhesión, fixtures, inscripciones a sorteos, sesiones de usuario, información importada de partidos en tiempo real y cobros.

**Roles de usuario:**
- **Hincha**: usa la app desde el smartphone para ver noticias y beneficios. Si es socio, accede a más beneficios en sorteos y descuentos.
- **Usuario de la App FF**: socio o hincha que se asocia y paga su adhesión. Recibe mails con el estado de la solicitud y el comprobante.
- **Responsable de Socios**: evalúa las solicitudes de adhesión.
- **Administrador del Club**: administra los campeonatos y los clubes rivales.
- **Administrador de Redes Sociales**: administra beneficios, comercios adheridos y sorteos, publica noticias y eventos minuto a minuto, y envía las notificaciones.

### 20.1 Análisis de RNF (SPA = Significativo Para la Arquitectura)
| Nro | RNF | ¿SPA? | Justificación |
|---|---|---|---|
| 1 | Autenticación de usuarios (módulo de permisos) | **SÍ** | Debe controlar el acceso a funciones y datos desde el inicio |
| 2 | Password (8 a 12 caracteres alfanuméricos, empieza con número, sin secuencias) | NO | Es parte de la política de seguridad, no de la estructura |
| 3 | Lenguaje sin costo de licencia | NO | Es una decisión de negocio que no impacta la arquitectura |
| 4 | Módulo Web (administración) | **SÍ** | El lenguaje debe soportar desarrollo web |
| 5 | Módulo Mobile (Android e iOS) | **SÍ** | Requiere soporte mobile multiplataforma |
| 6 | Cobro online (Mercado Pago) | **SÍ** | Hace falta un componente que se comunique con MP |
| 7 | Cobro por Rapipago y Pago Fácil (archivos .txt) | **SÍ** | Hace falta un componente que importe los cobros *fuera de línea* |
| 8 | Envío de mail con comprobantes | **SÍ** | Hace falta un componente y un servidor de mail (Exchange o Mercury) |
| 9 | Formato y tamaño de adjuntos (jpg, png o gif de menos de 50 KB) | NO | Se guardan en la misma BD como un campo más |
| 10 | Importación de eventos del partido (web service de DataFactory) | **SÍ** | Hace falta un componente que importe en tiempo real |
| 11 | Notificaciones push | **SÍ** | Hacen falta componentes de notificación a móviles |
| 12 | Publicación en redes sociales (en tiempo real) | **SÍ** | Hace falta una interfaz con varios web services |

### 20.2 Vista Arquitectónica de la Funcionalidad (casos de uso arquitectónicamente significativos)
Se eligen casos de uso que cubren los RNF significativos y los aspectos técnicos riesgosos:
- **CU 8 Iniciar sesión**: resuelve el manejo de sesión y los permisos (RNF 1 y 2).
- **CU 1 a 4, ABMC de Campeonato**: resuelve el acceso a BD, la UI y los algoritmos básicos (RNF 4, Web).
- **CU 20 Diagramar fixture**: es una transacción web compleja que define algoritmos, UI, acceso a BD y granularidad de componentes.
- **CU 40 Procesar comprobantes de cobro off-line** (RNF 7).
- **CU 15 Registrar solicitud de asociación**: es una transacción mobile compleja con logs, BD y UI (RNF 5).
- **CU 41 y 42, Registrar confirmación de cobro y cobro de la solicitud**: cobro online (RNF 6).
- **CU 18 Enviar comprobante de inscripción**: envío de mail (RNF 8).
- **CU 45 Enviar notificaciones push**: usa los componentes que implementan Publish & Subscribe (RNF 11).
- **CU 5 Publicar noticias** en X e Instagram (RNF 12).
- **CU 26 Calcular ganadores del sorteo**: es un proceso automático. Hay que decidir cuándo corre, cuidar la performance, minimizar el acceso a BD y no afectar a los usuarios.
- **CU 27 Importar minuto a minuto** desde DataFactory (RNF 10).

### 20.3 Framework arquitectónico: elección de estilos (las dos versiones)
| Dimensión | **Versión Monolítica** | **Versión Microservicios** |
|---|---|---|
| **Descomposición** | Monolítico | Microservicios |
| **Estructura interna** | **Layered, con partición técnica** | **Hexagonal** |
| **Distribución** | C/S N-Tier | C/S N-Tier |
| **Comunicación** | EDA **Broker** para la importación del minuto a minuto y de los cobros off-line. EDA **Publish & Subscribe** para las notificaciones de eventos del partido e información del club | **REST**, más EDA Broker para la importación del minuto a minuto y de los cobros off-line, más EDA Pub/Sub para las notificaciones de eventos del partido y del club |

**Detalle de Publish & Subscribe en FF:**
- Hay dos componentes publicantes: *Minuto a Minuto* y *Eventos del Club*.
- Publican en los tópicos *"Notificación de eventos minuto a minuto"* y *"Eventos del club"*.
- Los suscriptores son las apps FF de iOS y Android.

### 20.4 Versión Monolítica: Vista del Diseño (subsistemas, componentes e interfaces)
- **CapaPresentacionWeb**: CobroOffPresWeb, SociosPresWeb, NoticiasPresWeb, CampeonatoPresWeb, SorteoPresWeb, UsuariosPresWeb y BeneficiosPresWeb.
- **CapaPresentacionMobile**: AsociarsePresMobile, BeneficiosPresMobile, SorteosPresMobile, NoticiasPresMobile, EventosPartidoPresMobile y UsuarioPresMobile.
- **CapaLógicaNegocioPersistente**, con estos subsistemas:
  - GestionNoticias (PublicadorNoticiasRS, Noticia)
  - GestionSorteos (CalcularSorteo, Juego, Sorteo)
  - GestionCampeonatos (Campeonato, Club, Fixture, EventoPartido)
  - GestionBeneficios (CatálogoBeneficio, ConvenioBeneficio, Beneficio, ComercioAdherido)
  - GestionUsuarios (Usuario, Perfil, Sesión)
  - GestionSocios (Socio, Cobro, SolicitudAdhesion)
- **CapaLógicaNegocioNoPersistente**:
  - ProcesosImportadoresYNotificadores: ImportadorMinAMin, NotificadorPushEventosClub, NotificadorPushMinutoAMinuto, NotificadorEMail.
  - GestiónCobro: GeneradorCpteCobro, ImportadorCobroOffLine, ProcesadorCobroOnLine.
- **CapaAdministracionDatos**: el subsistema Persistencia (interfaces IMaterializar e IDesmaterializar) y la BaseDeDatos.

**Vista de Despliegue (monolítica):**
- PC Usuario con Browser.
- Smartphones Android e iOS con la CapaPresentacionMobile.
- Un **Servidor Web y de Aplicaciones** con el Webserver, la CapaPresentacionWeb, todos los subsistemas de negocio y Persistencia.
- **Servidor de BD.**
- **Servidores externos**: Correo (Exchange), DataFactory (MinutoAMinuto), Mercado Pago, X y Meta.

### 20.5 Versión Microservicios
**Descomposición del problema en dominios (Service Containment Spaces):**
- **Dominios Core**: Socio, Campeonato, Noticia, Beneficio y Cobro.
- **Dominios de Soporte**: Sorteo y Evento.
- **Dominios Genéricos**: Usuarios y Accesos, Integración X, Integración Instagram, Notificaciones Push, Notificaciones Email e Integración Mercado Pago.

**Vista de Diseño Global:**
- Las UI (WebUI, MobileAppAndroid y MobileAppIOS) y los externos (ServicioExchange, ServicioMinutoAMinuto, ServicioInstagram, ServicioX y ServicioMP) se conectan a un **API Gateway**.
- El API Gateway enruta a los microservicios.
- **Cada microservicio core o de soporte tiene su propia BD**: ServicioBeneficio, ServicioCampeonato, ServicioSorteo, ServicioCobro, ServicioEvento, ServicioNoticia y ServicioSocio.
- Los microservicios genéricos de integración (IntegracionInstagram, IntegracionX, NotificacionesEmail, NotificacionPush y MercadoPago) no tienen BD propia.
- **ServicioUsuarioYAccesos es un microservicio tercerizado**, con BD Usuario. Como es un dominio genérico, se puede comprar.
- En el diseño detallado, el API Gateway incluye componentes WebAPIGateway, MobileAPIGateway y PublicAPIGateway (uno por tipo de cliente, al estilo **BFF**). Cada microservicio tiene sus componentes internos y su persistencia.

**Vista de Despliegue (microservicios):**
- Un **Servidor API Gateway FF** con Webserver, API Gateway y WebUI.
- **Un servidor por microservicio** (Campeonato, Cobro, Socio, Sorteo, Beneficio, Noticia, Evento, Integración X, Integración Mercado Pago, Notificaciones Push, Notificaciones Email, Instagram), cada uno con su BD cuando corresponde. Esto es el patrón *Single Service per Host*.
- Servidor de Usuario (tercerizado).
- Servidores externos: Correo, Meta, X, DataFactory y Mercado Pago.

---

## 21. Preguntas de repaso con respuestas

1. **¿En qué fase del PUD estamos y qué la distingue?**
   La arquitectura se define y estabiliza en la **fase de Elaboración**, que establece una **línea base de la arquitectura** y mitiga los riesgos técnicos. En Inicio solo se esboza una arquitectura candidata. El PUD está **centrado en la arquitectura**, guiado por casos de uso, y es iterativo e incremental. 🆕 *Los resúmenes lo confirman*: el diseño es el centro de atención al final de Elaboración y al comienzo de Construcción.
2. **¿Diferencia esencial entre análisis y diseño?**
   El análisis modela la solución en términos **lógicos/conceptuales**: el "qué", independiente de la tecnología. El diseño la modela en términos **físicos**: el "cómo", como plano de la implementación en una tecnología concreta, incorporando los RNF. 🆕 Ver la tabla completa en el §1.
3. **¿Qué es diseñar, en una frase?**
   Transformar los requerimientos (el modelo de análisis) en un plan de solución implementable con una tecnología concreta.
4. **¿Qué aspectos se diseñan y por qué la arquitectura está en el centro?**
   Se diseñan los datos, los procesos, los procedimientos manuales, las formas de E/S, la UX y la arquitectura. La arquitectura está en el centro porque es la estructura que condiciona y conecta a todos los demás.
5. **Tres miradas de la arquitectura:** Rol, Proceso y Producto.
6. **Tres responsabilidades del arquitecto:** negociar los requerimientos, documentar y comunicar la arquitectura, y asegurarse de que se respete. Ver la lista completa en el §3.
7. **Las 3 actividades del proceso:** determinar los requerimientos arquitectónicos, diseñar la arquitectura y validarla. Ver el §4.
8. **¿Qué sugiere cada RNF?** Ver la tabla del §5.
9. **¿Por qué la priorización es engañosa?** Porque los requerimientos entran en conflicto entre sí. Al inicio se usan tres categorías: **Alto, Medio y Bajo**.
10. **¿Por qué elegir un framework con arquitecturas probadas?** Porque **minimiza los riesgos**: son soluciones conocidas con propiedades de calidad conocidas, y no hay fórmula mágica para inventar una nueva.
11. **Diferenciá modelo, vista y punto de vista.** Ver el §7.4.
12. **Vistas del PUD / 4+1 y qué captura cada una.** Ver el §8.
13. **¿Todos los sistemas requieren todas las vistas?**
    No. Con un procesador único se omite la de despliegue, con un proceso simple se omite la de procesos, y en programas muy pequeños se omite la de implementación. También se pueden agregar vistas, como la de datos o la de seguridad.
14. **¿Por qué validar la arquitectura es un desafío?**
    Porque es un diseño que no se puede ejecutar. Hay dos técnicas: **escenarios** y **prototipos**. Se necesita un prototipo cuando el escenario no puede responderse con confianza razonando, por ejemplo con requisitos cuantitativos de performance como "500 órdenes en 5 minutos", o para probar una tecnología.
15. **¿Qué son los sistemas distribuidos? Características y desventajas.** Ver el §11.

---

## 22. Cuadros comparativos para memorizar

### Estilo → pregunta que responde → atributo que favorece
| Estilo | Dimensión | Favorece | Penaliza |
|---|---|---|---|
| Monolítico | Descomposición | Simplicidad, performance local, facilidad de prueba | Escalabilidad selectiva, despliegue, stack único |
| Microservicios | Descomposición | Escalabilidad, despliegue independiente, autonomía de los equipos, persistencia políglota | Performance (red), complejidad distribuida, trazabilidad |
| Layered | Estructura interna | Mantenibilidad, testeabilidad, seguridad, portabilidad | Performance, escalabilidad, tolerancia a fallas |
| Hexagonal | Estructura interna | Testeabilidad, intercambiabilidad, aislamiento del dominio (DDD) | Más abstracciones e indirección |
| C/S | Distribución | Centralización, seguridad, separación de responsabilidades | Punto único de falla ("todo o nada"), concurrencia |
| N-Tier | Distribución | Separación de intereses, escalabilidad por nivel, modificabilidad | Latencia entre niveles, la BD como cuello de botella |
| EDA | Comunicación | Escalabilidad, desacoplamiento, flexibilidad | Testeabilidad, complejidad de desarrollo, consistencia |
| Pub/Sub | Comunicación | Mantenibilidad, evolucionabilidad (1-N, N-N) | Indirección del bus |
| Broker | Comunicación | Transformación y ruteo centralizados | Cuello de botella |
| Messaging | Comunicación | Asincronía, resiliencia ante conectividad transitoria | Dependencia del formato de mensaje |
| Process Coordinator | Comunicación | Proceso de negocio centralizado y modificable | Punto único de falla, compensaciones |
| REST | Comunicación | Interoperabilidad, escalabilidad (stateless), reutilización | Latencia por request, más puntos de falla, gobierno |

### Síncrono vs. asíncrono
- **Síncronos**: C/S, N-Tier, REST (request-response).
- **Asíncronos**: Messaging, Pub/Sub, EDA. El Broker y el Process Coordinator pueden ser de cualquiera de los dos tipos.

### Frases clave para el parcial
- "Diseño arquitectónico = asignación de modelos de requerimientos esenciales a una tecnología específica: el plan del diseño."
- "Los estilos son herramientas, no decisiones exclusivas; las arquitecturas reales son híbridas por diseño."
- "Usar soluciones conocidas minimiza los riesgos."
- "Las vistas son proyecciones de uno o más modelos para un involucrado y sus intereses."
- "Si lo cambiás, lo tenés que re-testear" (minimizar dependencias).
- "Violar las restricciones de un estilo platónico implica perder los atributos de calidad que este garantiza."
- "Un estilo no resuelve un problema: nombra un diseño recurrente. Un patrón resuelve un problema recurrente."
- "Todo en arquitectura es un trade-off" / "El porqué es más importante que el cómo."
- "Empezar monolítico y extraer microservicios de a uno" (Fowler, Monolith first).
- "Cada microservicio, su propia base de datos; nunca compartir BD."
- "El API Gateway no contiene lógica de negocio."

---

## 23. 🆕 Preguntas de parciales anteriores

Estas preguntas salen de *RESUMEN ARQUITECTURA (1)* y del manuscrito *Preguntas tipo 2do Parcial*. Para cada una se indica dónde está la respuesta en este resumen.

| # | Pregunta | Dónde está la respuesta |
|---|---|---|
| 1 | Explicar el proceso de diseño arquitectónico de Ian Gorton y sus salidas | §4 (actividades, entradas y salidas) + §7 + §9 |
| 2 | ¿Qué es la arquitectura de software y cómo se documenta? | §2 (definición de cátedra) + §10 + §8 (4+1) |
| 3 | Explicar las vistas del modelo 4+1 y qué diagramas UML usa cada una | §8 y §8.1 |
| 4 | Características de SOA, ventajas y desventajas | §18.1 (SOA) |
| 5 | ¿Por qué la arquitectura se documenta con vistas? ¿Qué diagrama usa cada una? | §10 + §8.1 (tipos de vista: módulo, comportamiento, distribución) |
| 6 | Patrones arquitectónicos que intervienen en EDA | §17: Pub/Sub, Messaging, Broker, Process Coordinator |
| 7 | ¿Qué es una arquitectura distribuida? Ventajas y desventajas | §11 |
| 8 | ¿Cuál es el rol del arquitecto? | §3 |
| 9 | ¿Qué es un conflicto arquitectónico? Proponga soluciones para (a) granularidad gruesa vs. mantenibilidad y (b) redundancia vs. seguridad | §5 (conflicto y soluciones) |
| 10 | Diferencias entre el estilo monolítico y el de microservicios | §14.3 |
| 11 | ¿Qué es un estilo arquitectónico? ¿En qué actividad del proceso se usa? Describa 2 estilos | §12. Se usa en la actividad **Diseño arquitectónico**, en la tarea **Elegir el framework** |
| 12 | ¿Por qué documentar la arquitectura? ¿Cuál es el proceso de Gorton? ¿En qué fase del PUD se diseña la arquitectura? | §10 + §4 + **Elaboración** (§8.1) |
| 13 | Explicar la arquitectura hexagonal | §15.2 |
| 14 | Mencione 2 RNF y explique su impacto si se los considera prioritarios | §5 |
| 15 | Mencione los patrones con comunicación asíncrona y cuándo usar cada uno | §17 (EDA) |

**Respuestas modelo breves** (para repasar rápido):
- **Rol del arquitecto (definición corta)**: es quien define cómo se comunican los componentes, **negocia** con todos los involucrados y con todos los aspectos del contexto del proyecto (es un **mediador**), y se encarga de producir los modelos (diseño, implementación y despliegue), las vistas y la documentación.
- **Estilo arquitectónico (definición corta)**: es un esquema fundamental de alto nivel. Es un conjunto de principios, reglas y restricciones que define los **tipos de componentes**, **cómo se conectan** y las **reglas de composición**. No es una arquitectura concreta, sino una "plantilla" o paradigma. Se aplica al **elegir el framework** dentro del diseño arquitectónico.
- **Patrón vs. estilo, según los resúmenes**: el estilo está en un nivel de abstracción más alto (la estructura general) y el patrón resuelve problemas puntuales, sin afectar a toda la aplicación. En la práctica, *"en la cátedra los tomamos como lo mismo"*. Ver el §12 para la versión completa.
- **Arquitectura distribuida**: es aquella en la que el **procesamiento** se reparte entre varias computadoras conectadas por una red, en lugar de confinarse a una sola.
  - Ventajas: compartición de recursos, apertura, concurrencia, tolerancia a fallas y escalabilidad.
  - Desventajas: complejidad, seguridad, manejabilidad e impredecibilidad.

---

## 24. 🆕 Anexo: Unidad 2, el Diseño en el PUD

> Este anexo resume la Unidad 2 del *RESUMEN PARCIAL 2 DSI (2021)*. **No aparece en las diapositivas 2026 de esta carpeta**, así que conviene confirmar si entra en el parcial. Está resumido para tenerlo a mano.

### 24.1 Workflow de Diseño en el PUD
**Propósitos del WF de Diseño:**
- Comprender en profundidad los RNF y las restricciones tecnológicas.
- Crear la entrada para el WF de Implementación.
- Descomponer la implementación en partes manejables para equipos concurrentes.
- Capturar temprano las interfaces entre subsistemas.
- Crear una abstracción "sin costuras" de la implementación.

**Artefactos:**
- **Modelo de Diseño**: una jerarquía de subsistemas con clases de diseño, realizaciones de CU-diseño e interfaces.
- **Clase de diseño**: usa la sintaxis del lenguaje de programación, con visibilidad y posiblemente activa. Una clase bien diseñada es **completa y suficiente, sencilla, de alta cohesión y de bajo acoplamiento**.
- **Realización de CU-diseño**: describe cómo se realiza un CU con objetos de diseño. Incluye diagramas de clases, **diagramas de secuencia**, el flujo de sucesos-diseño y los requisitos de implementación.
- **Subsistema de diseño**: es cohesivo, débilmente acoplado y puede encapsular productos reutilizados o sistemas heredados.
- **Interfaz**: separa la especificación (operaciones) de la implementación (métodos).
- **Descripción de la arquitectura**: la vista del modelo de diseño.
- **Modelo de Despliegue**: nodos, conexiones y la correspondencia entre la arquitectura de software y la de hardware.
- **Descripción de la arquitectura**: la vista del modelo de despliegue.

**Trabajadores:**
- **Arquitecto**: responsable de la integridad de los modelos de diseño y despliegue y de su arquitectura.
- **Ingeniero de casos de uso**: responsable de las realizaciones de CU-diseño.
- **Ingeniero de componentes**: responsable de las clases y subsistemas, sus operaciones, atributos e interfaces.

**Actividades:**
1. **Diseño de la arquitectura**, a cargo del arquitecto. Identifica:
   - los nodos y la configuración de red
   - los subsistemas y sus interfaces
   - las clases de diseño significativas, como las activas
   - los mecanismos genéricos de diseño (persistencia, distribución, rendimiento)
2. **Diseñar un caso de uso**, a cargo del ingeniero de CU.
3. **Diseñar una clase**, a cargo del ingeniero de componentes: operaciones, atributos, asociaciones, generalizaciones, métodos, estados y requisitos especiales.
4. **Diseñar un subsistema**: mantener las dependencias (mejor hacia interfaces que hacia subsistemas), las interfaces provistas y los contenidos.

### 24.2 Estrategia de Prototipado
- Un prototipo es la **primera versión** de un producto, con solo algunas características: tiene funcionalidad limitada y poca fiabilidad. Cuesta alrededor del **10 % del presupuesto** y lleva pocos días.
- **Objetivos**: aclarar los requerimientos y verificar la factibilidad del diseño.
- **Son especialmente útiles** cuando:
  - el dominio es riesgoso
  - el costo de que los usuarios rechacen el sistema es alto
  - se usan tecnologías nuevas
  - la inversión es alta
- **Tipos según su propósito:**
  - de **interfaz de usuario**
  - **funcional / operacional**
  - **arquitectónico** (evalúa decisiones de infraestructura, tecnología e integración)
  - **modelos de rendimiento**
- **Tipos según su utilidad:**
  - **Desechable (rápido)**: sirve para validar los requisitos y después se descarta. El riesgo es que termine convertido en el sistema final.
  - **Evolutivo**: crece hasta convertirse en el sistema.
- **Tipos según su alcance:**
  - **Vertical**: desarrolla completa alguna función.
  - **Horizontal**: desarrolla parcialmente todas las funciones.
- **Etapas:**
  1. Identificar los requerimientos conocidos.
  2. Desarrollar un modelo de trabajo.
  3. Lograr la participación del usuario.
  4. Revisar el prototipo.
  5. Iterar el refinamiento.
- **Riesgos:**
  - El cliente cree que el prototipo ya es el sistema.
  - Uno se acostumbra a malas decisiones iniciales.
  - Es difícil de administrar.
  - Puede salir caro.

### 24.3 Ensamblado de componentes e ISBC
- El **ensamblado de componentes** es un modelo **evolutivo**, parecido al espiral. Arma la aplicación a partir de componentes ya preparados (COTS) que se buscan en una **biblioteca**.
- La **ISBC** sigue la filosofía de **"comprar, no construir"**. Sus fundamentos son:
  - componentes independientes, especificados por sus interfaces
  - estándares de componentes
  - middleware
  - un proceso de desarrollo que acompañe
- **Características de un componente**: estandarizado, independiente, componible, implementable (desplegable) y documentado.
- **Procesos:**
  - **Desarrollo *para* reuso**: generalizar componentes existentes. Hay un *trade-off* entre la generalidad y la facilidad de uso.
  - **Desarrollo *con* reuso**: descubrir y adaptar componentes existentes.
- **Composición de componentes:**
  - **Secuencial**: con "código pegamento".
  - **Jerárquica**: la interfaz provista de un componente se conecta con la requerida de otro.
  - **Aditiva**: se suman las interfaces de dos componentes.
- **Incompatibilidades de interfaz:**
  - de **parámetros**
  - de **operación**
  - **operación incompleta**
- **Problemas de la ISBC:**
  - la confiabilidad de componentes que son cajas negras
  - quién paga la certificación
  - la dificultad de predecir propiedades emergentes
  - el equilibrio entre los requerimientos y los componentes disponibles
- **Beneficios:**
  - reutilización
  - pruebas más simples
  - mantenimiento más simple
  - mayor calidad
  - ciclos más cortos (estudios citan **−70 % de tiempo**)
  - mejor retorno de la inversión (ROI)

### 24.4 Diseño de Interacción Humano-Máquina (IHC)
- La **IHC** es la disciplina del diseño, evaluación e implementación de sistemas interactivos para uso humano. Es **mucho más que diseñar interfaces**: incluye documentación, capacitación, soporte y contexto de uso. Es interdisciplinaria (ergonomía, psicología, etc.).
- Para que la **información** tenga valor debe ser **adecuada, oportuna e ilustrativa**.
- **Utilidad**: el sistema provee las operaciones necesarias.
- **Usabilidad**: es el grado de facilidad de uso. Reduce los costos y aumenta la productividad.
  - Se mide con atributos: **aprendizaje**, **velocidad de funcionamiento**, **robustez**, **recuperación** y **adaptación**.
- **Proceso de diseño de interfaz:**
  1. Análisis de los usuarios y sus tareas.
  2. Prototipos en papel o desechables.
  3. Prototipos evolutivos con pruebas de usabilidad.
  4. Versiones beta.
- **Lineamientos de color**:
  - no más de 4 o 5 colores por ventana y 7 por sistema
  - usar el color para indicar cambios de estado
  - aplicar el código de colores de forma consistente: rojo para error, amarillo para advertencia, verde para éxito
  - cuidar las combinaciones de colores
- **Heurísticas de Nielsen (10):**
  1. Visibilidad del estado del sistema.
  2. Relación entre el sistema y el mundo real.
  3. Control y libertad del usuario.
  4. Consistencia y estándares.
  5. Prevención de errores.
  6. Reconocer antes que recordar.
  7. Flexibilidad y eficiencia de uso.
  8. Diseño estético y minimalista.
  9. Ayudar a reconocer, diagnosticar y recuperarse de errores.
  10. Ayuda y documentación.
- **8 reglas de oro de Shneiderman:**
  1. Buscar la coherencia.
  2. Permitir atajos (*shortcuts*).
  3. Dar realimentación.
  4. Diseñar diálogos que tengan un cierre.
  5. Manejar los errores de forma simple.
  6. Permitir deshacer fácilmente.
  7. Que el usuario sienta que tiene el control.
  8. Reducir la carga de memoria a corto plazo.
