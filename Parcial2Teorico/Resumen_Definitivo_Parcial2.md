# Resumen Definitivo – Parcial 2 Teórico (DSI · UTN)

> Este resumen contiene solo lo que sirve para responder las preguntas de parcial. El resumen completo está en `Resumen_Parcial2_Teorico.md`.

## 0. Cómo es el parcial

Así fue el parcial tomado en Córdoba el 26/09/2026:

| Parte | Puntos | Qué preguntan |
|---|---|---|
| **1 – Preguntas conceptuales** | 40 + 5 opcionales | **Situaciones concretas** en las que hay que **elegir y justificar**. No piden definiciones sueltas |
| **2 – Aplicación práctica** | 60 | Sobre un caso: justificar los CU de la vista de funcionalidad (10), hacer la vista de diseño con microservicios usando un diagrama de componentes (35) y la vista de despliegue (15) |

**Receta para responder cualquier pregunta conceptual:**
1. **Identificar los RNF y restricciones** del enunciado: escalar, equipo chico, integración externa, disponibilidad, etc.
2. **Elegir** el estilo, la vista o la solución.
3. **Justificar** relacionando cada elección con un RNF del enunciado. Decir **qué se gana y qué se pierde**, porque todo en arquitectura es un trade-off.
4. Cuando lo pidan, **comparar con las alternativas descartadas**.

---

## 1. Parcial de Córdoba (26/09/2026): respuestas de la Parte 1

### Pregunta 1 (10 pts): vistas del 4+1 y diagramas UML
> *En la revisión de arquitectura de un sistema bancario participan cuatro interesados: un **usuario de negocio** que quiere entender qué operaciones puede realizar un cliente, un **DBA** que quiere ver cómo se despliegan los nodos y la replicación entre centros de datos, un **líder técnico** que quiere ver cómo se organiza el código en módulos reutilizables, y un **especialista en concurrencia** que quiere revisar cómo se coordinan los hilos que procesan transacciones simultáneas.*

| Interesado | Vista 4+1 (Kruchten) / PUD | Por qué | Diagrama(s) UML |
|---|---|---|---|
| **Usuario de negocio** ("qué operaciones puede hacer un cliente") | **Vista Lógica** / de Diseño (Kruchten: el usuario final busca **funcionalidad**), complementada por la vista de **Escenarios / CU (+1)** | Representa la **funcionalidad que el sistema le brinda al usuario final**: los servicios y operaciones disponibles | **Casos de uso** para *qué* operaciones hay. **Clases** y **secuencia/comunicación** para mostrar cómo se realizan |
| **DBA** (despliegue de nodos y replicación entre data centers) | **Vista Física** (Kruchten) = **Vista de Despliegue** (PUD) | Mapea el software sobre el **hardware (nodos)**, la topología y la distribución. Contempla disponibilidad, confiabilidad y escalabilidad, y la replicación es una cuestión de disponibilidad | **Diagrama de despliegue** (nodos, conexiones y componentes/BD asignados a cada nodo) |
| **Líder técnico** (organización del código en módulos reutilizables) | **Vista de Desarrollo** (Kruchten) = **Vista de Implementación** (PUD) | Muestra la **organización estática del software** en el entorno de desarrollo: módulos, subsistemas, capas, dependencias, **reuso** y gestión de la configuración | **Diagrama de componentes** (y de **paquetes**) |
| **Especialista en concurrencia** (coordinación de hilos) | **Vista de Procesos** (Kruchten) = vista de Procesos / Interacción (PUD) | Captura la **concurrencia y sincronización**: procesos, tareas, hilos y **clases activas**. Contempla performance, escalabilidad e integridad | **Diagrama de actividad** (con calles o *fork/join* para el paralelismo) y **diagrama de secuencia**. En la cátedra, componentes resaltando las clases activas |

> 💡 Conviene cerrar con esto: la vista **+1 (Escenarios/CU)** une y valida las otras cuatro. Cada interesado necesita una **proyección distinta** del mismo sistema, y por eso la arquitectura se documenta con vistas.
>
> ⚠️ **Lógica o CU para el usuario de negocio**: según Kruchten, el usuario final se asocia con la vista **Lógica** (funcionalidad). En el Repaso de la cátedra, "¿Qué hace el sistema para el usuario?" se respondía con **Casos de Uso**. Para no perder puntos, conviene nombrar la vista Lógica y mencionar que se apoya en el diagrama de CU (escenarios).

---

### Pregunta 2 (15 pts): framework arquitectónico para un sistema de RRHH
> *Una PYME necesita un sistema interno de gestión de RRHH (altas de empleados, licencias, liquidación de sueldos).*
> - *RF: administración de empleados y tipos de licencias, y generación de recibos de sueldo.*
> - *RNF: sistema autocontenido que no requiere integrarse con sistemas externos. No necesita escalar por separado ninguna funcionalidad, pero sí se pide una estructura clara que separe presentación, lógica de negocio y acceso a datos para facilitar el mantenimiento. La persistencia será una BD relacional.*
> - *Contexto: un único equipo de 3 desarrolladores debe desarrollarlo y mantenerlo.*

**Framework propuesto:**

| Dimensión              | Estilo elegido                                                                                                                  | Justificación con el enunciado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Descomposición**     | **Monolítico**                                                                                                                  | El sistema es **autocontenido**: el monolito es una aplicación autosuficiente, sin dependencias externas. **No hace falta escalar funcionalidades por separado**, que es la principal razón para distribuir. Un **equipo único de 3 personas** no puede absorber la complejidad operativa de los microservicios (API Gateway, una BD por servicio, despliegues múltiples, latencia, consistencia distribuida). El monolito es **fácil de desarrollar, probar y desplegar**, tiene **mejor performance** (llamadas locales) y **menos puntos de falla** |
| **Estructura interna** | **Layered (en capas), con partición técnica**: Presentación → Lógica de Negocio → Acceso a Datos / Persistencia → BD relacional | Es exactamente lo que pide el RNF: **separar presentación, lógica y acceso a datos**. Cada capa solo usa la inmediata inferior, lo que da **mantenibilidad** (es fácil ubicar dónde cambiar o dónde está un defecto), **testeabilidad por capa** y la posibilidad de **sustituir una capa** si se conserva su interfaz. La capa de persistencia encapsula el acceso a la **BD relacional** con *materializar/desmaterializar*. Es el estilo más conocido, lo que conviene a un equipo chico                                                            |
| **Distribución**       | **Cliente/Servidor N-Tier** (3 niveles: cliente, servidor de aplicación, servidor de BD)                                        | Separa responsabilidades y centraliza los datos y la lógica en el servidor, con la seguridad que eso implica (los datos de sueldos son sensibles). Al ser interno, puede ser un **cliente liviano** (browser). La comunicación entre niveles es **pedido-respuesta sincrónica**                                                                                                                                                                                                                                                                        |
| **Comunicación**       | **Request-Response sincrónico** entre capas/tiers. **No hace falta** EDA, Broker, Messaging ni Pub/Sub                          | **No hay integración con sistemas externos**: no hay formatos que transformar (que justificaría un Broker), ni conectividad transitoria (que justificaría Messaging), ni notificaciones uno-a-muchos (que justificaría Pub/Sub). Agregarlos sumaría complejidad sin beneficio                                                                                                                                                                                                                                                                          |

**Alternativas descartadas** (sumar si sobra tiempo):
- **Microservicios**: su escalado independiente, despliegue independiente y autonomía de equipos no los pide nadie aquí. Solo agregaría costo y complejidad.
- **Hexagonal**: sus ventajas aparecen cuando hay **múltiples sistemas externos o adaptadores intercambiables**, y este sistema no se integra con nada. La separación pedida se logra de forma más simple con Layered.

---

### Pregunta 3 (15 pts): monolítico, monolito modular o microservicios
> *Una plataforma de e-commerce consolidada tiene varios equipos autónomos (catálogo, pagos, envíos, recomendaciones). Cada capacidad tiene patrones de carga muy distintos (el catálogo recibe 100 veces más tráfico que envíos), y la empresa necesita que cada equipo despliegue sus cambios sin coordinar con los demás ni afectar la disponibilidad del resto del sistema.*

**Recomendación: Arquitectura de Microservicios**, descompuesta **por capacidad de negocio o subdominio**: Catálogo, Pagos, Envíos y Recomendaciones.

**Justificación, punto por punto del enunciado:**
1. **"Cada equipo despliega sin coordinar con los demás"**: cada microservicio es un **artefacto independiente**, con su propio ciclo de vida y CI/CD. Es la **despliegue independiente** que Newman considera la idea central del estilo. Además se alinea con el principio de **productos, no proyectos** ("you build it, you run it").
2. **"Patrones de carga muy distintos (catálogo ×100)"**: los microservicios permiten **escalar cada servicio por separado**. Se escala solo Catálogo, con muchas instancias detrás de un balanceador (eje X del cubo), y Envíos queda con pocas. Esto es la descomposición funcional del **eje Y**.
3. **"Sin afectar la disponibilidad del resto"**: cada servicio corre en su propio proceso o contenedor y tiene **su propia base de datos** (patrón *Database per Service*). Una falla o un despliegue en Recomendaciones **no tira** al Catálogo ni a Pagos: hay **aislamiento de fallas**, a diferencia del "todo o nada" del monolito.
4. **"Equipos autónomos"**: cada equipo es dueño de su servicio y puede elegir su tecnología y su BD (**persistencia políglota**).
5. **"Plataforma consolidada"**: el dominio ya es **maduro** y está bien entendido. Esta es la condición que pide Fowler (*Monolith first*) para partir en microservicios.

**Comparación con los estilos que no se eligen:**

| Criterio del enunciado | **Monolítico** | **Monolito modular** | **Microservicios** ✅ |
|---|---|---|---|
| Despliegue independiente por equipo | ❌ Una sola unidad de despliegue: cualquier cambio redespliega todo y obliga a coordinar | ❌ Los módulos están separados por dominio en el código, pero se **despliegan juntos** como una sola unidad | ✅ Cada servicio se despliega solo |
| Escalado diferenciado (catálogo ×100) | ❌ Escalar el catálogo implica **replicar toda la app** (escalado monolítico) | ❌ Igual: se escala la aplicación completa | ✅ Escalado selectivo por servicio |
| Disponibilidad aislada | ❌ "Si falla, falla todo" | ❌ Comparte proceso: un módulo puede tirar todo | ✅ Cada servicio tiene su proceso y su BD |
| Autonomía de equipos | ❌ Un único codebase y un único stack | ⚠️ Mejora la organización por dominio, pero sigue habiendo un único stack y un único release | ✅ Cada equipo es dueño de su servicio |

**Costos que se aceptan** (mencionarlos suma puntos, porque muestra el trade-off):
- Es un sistema distribuido más complejo: aparecen las **falacias de la red**, la latencia y más puntos de falla.
- La consistencia entre servicios es **eventual** (se resuelve con sagas).
- Hace falta un **API Gateway** como punto de entrada único, más monitoreo y trazabilidad distribuidos.
- Se necesita un equipo maduro, que en este caso existe.

---

### Pregunta 4 (opcional, 5 pts): conflicto arquitectónico
> *Explique qué es un conflicto arquitectónico, identifique el de esta situación y proponga cómo abordarlo. Una aplicación de turnos médicos para adultos mayores debe implementar múltiples factores de autenticación (contraseña, código por SMS y pregunta de seguridad) para proteger datos sensibles de salud, pero el equipo de UX reporta que muchos usuarios abandonan el login por la cantidad de pasos y no logran sacar el turno.*

**¿Qué es un conflicto arquitectónico?**
Es una situación en la que **dos o más requerimientos no funcionales tienen demandas opuestas**: la decisión que favorece a uno perjudica al otro. Como los atributos de calidad no son independientes entre sí, el arquitecto debe **hacer explícita la tensión**, **priorizar con los interesados** y tomar una decisión de compromiso **documentada**. Por eso la priorización de requerimientos es engañosa.

**Conflicto identificado:** **Seguridad vs. Usabilidad** (ISO 25010):
- **Seguridad**: confidencialidad, autenticidad e integridad de los **datos sensibles de salud**. Pide más factores de autenticación.
- **Usabilidad**: operabilidad, accesibilidad, facilidad de aprendizaje y protección frente a errores del usuario, para un público de **adultos mayores**. Pide menos pasos.
- Las consecuencias ya son visibles: los usuarios abandonan y la aplicación **deja de cumplir su función principal**, que es sacar turnos.

**Cómo abordarlo** (proponer varias opciones y justificar):
1. **Seguridad proporcional al riesgo (autenticación escalonada o adaptativa)**: no todas las operaciones necesitan el mismo nivel.
   - **Sacar o consultar un turno** pide un factor simple, o un segundo factor solo si el riesgo lo amerita (dispositivo nuevo, ubicación inusual).
   - **Ver la historia clínica o datos sensibles** pide la autenticación multifactor completa.
   - Esto aplica la sugerencia arquitectónica de **ubicar los recursos críticos en capas internas** con alta validación, sin castigar las operaciones de bajo riesgo.
2. **Reemplazar factores difíciles por otros más simples pero igual de seguros**:
   - **Biometría** del dispositivo (huella o rostro).
   - **Dispositivo de confianza** recordado, con una sesión segura de mayor duración.
   - Un *magic link* o código de un solo uso, en lugar de pregunta de seguridad más SMS más contraseña.
   - Se reducen los pasos sin bajar la seguridad.
3. **Centralizar la autenticación en un único subsistema o servicio**: aplica la táctica de "localizar la protección/seguridad en pocos subsistemas", que permite ajustar la política sin tocar el resto.
4. **Negociar y priorizar con los interesados** (negocio, UX y seguridad o legales), documentar la decisión y **validar** con **escenarios** (por ejemplo, el estímulo "usuario mayor saca un turno" debe responderse en N pasos) y con un **prototipo** de UX probado con usuarios reales.

---

### Bonus: Parte 2, punto 1 (justificar los CU de la vista de funcionalidad, caso Cambalache)
**Criterio:** se eligen pocos CU que **cubran todos los RNF significativos para la arquitectura** y los riesgos, de modo que el resto de los CU repita decisiones ya tomadas.

| CU incluido | RNF y aspecto arquitectónico que resuelve |
|---|---|
| **6. Iniciar sesión con biometría** | **RNF 3 Seguridad de usuarios** (autenticación, autorización y manejo de sesión) y **RNF 4 Logueo con biometría**: integración con la librería de *Biometría Android*, actor secundario BA. Pertenece al dominio genérico *Login biométrico / Autenticación* |
| **3. Confirmar venta de producto** | Es la **transacción más compleja del dominio core** (*Compra-Venta Solidaria*): aplica la **política de venta y donación** (porcentaje mínimo por rango de precios), sube el comprobante y actualiza el fondo solidario y el grado. Define el acceso a datos, la lógica de negocio y la UI mobile (**RNF 1 App Android**) |
| **7. Enviar notificación de entrega** | **RNF 5 Notificaciones push** mediante **FCM**, actor secundario. Es un **proceso automático** (AP: N/A) que resuelve el envío desacoplado de eventos, al estilo Publish & Subscribe / EDA. Representa también a los CU 8 y 9, que son notificaciones del mismo tipo |
| **10. Consultar productos a vender** | **RNF 6 Interfaz con Google Maps**, actor secundario: muestra en tiempo real dónde retirar el producto. Es la **consulta** representativa: acceso a datos de lectura, fotos y presentación |

El **RNF 2 (Servicio en la nube, AWS / DevOps)** no se resuelve con un CU: se refleja en la **vista de despliegue**. El RNF 1 (App Android) queda cubierto por todos los CU, que son mobile.

---

## 2. Núcleo teórico ordenado por tipo de pregunta

### A. Vistas 4+1: "¿qué vista necesita este interesado?"
| Vista Kruchten | Vista PUD | Interesado | Interés | Diagrama UML |
|---|---|---|---|---|
| **Lógica** | Diseño | Usuario final | **Funcionalidad**, el "vocabulario" (clases, interfaces, colaboraciones) | Clases, secuencia, comunicación (en la cátedra, componentes) |
| **Procesos** | Procesos / Interacción | Integrador | **Concurrencia, sincronización, hilos y clases activas**, performance, escalabilidad | Actividad, secuencia |
| **Desarrollo** | Implementación | Programador, gestor del software | **Organización del código**: módulos, subsistemas, capas, reuso, configuración | Componentes, paquetes |
| **Física** | Despliegue | Ingeniero de sistemas | **Nodos, topología**, distribución, disponibilidad | Despliegue |
| **+1 Escenarios** | Casos de Uso | Todos | **Une y valida las 4 vistas**. Sirve para descubrir y validar la arquitectura | Casos de uso (+ secuencia) |

- **¿Por qué se documenta con vistas?** Un solo diagrama no puede mostrar todo sin volverse ambiguo. Cada vista es una **proyección del modelo para un involucrado y sus intereses**.
- **Modelo, vista y punto de vista**:
  - **Modelo**: descripción completa desde una perspectiva.
  - **Vista**: proyección del modelo que omite lo irrelevante.
  - **Punto de vista**: la definición de la vista (contenido y notación).
- **¿Hacen falta todas las vistas?** No.
  - Con un solo procesador se omite la física.
  - Con un solo proceso se omite la de procesos.
  - En un programa muy chico se omite la de implementación.
  - Se pueden agregar otras, como la de datos o la de seguridad.

### B. "Defina el framework arquitectónico para este caso"
**Framework** = conjunto de **3 o 4 estilos o patrones** que estructuran el producto. Se elige en la actividad **Diseño arquitectónico**, en la tarea **"Elegir el framework"**. Hay que cubrir las cuatro dimensiones:

| Dimensión | Opciones | Elegí… si el enunciado dice… |
|---|---|---|
| **Descomposición** | Monolítico / Microservicios | **Monolito**: equipo chico, sistema autocontenido, no hay que escalar partes por separado, dominio nuevo o inmaduro, se prioriza la performance. **Microservicios**: varios equipos autónomos, despliegue independiente, cargas distintas por funcionalidad, alta disponibilidad aislada, dominio maduro |
| **Estructura interna** | Layered / Hexagonal | **Layered**: separar presentación, negocio y datos, mantenibilidad simple, equipo que la conoce. **Hexagonal**: muchos sistemas externos o adaptadores intercambiables, núcleo de negocio que hay que proteger o testear aislado, DDD, cambiar la persistencia sin tocar el negocio |
| **Distribución** | C/S / N-Tier | **N-Tier**: separar cliente, lógica y BD en niveles, con muchos clientes y requests cortos. Es lo habitual |
| **Comunicación** | REST (request-response), EDA: Broker, Pub/Sub, Messaging, Process Coordinator | **REST**: comunicación síncrona entre servicios, típica de microservicios. **Broker**: hay que **transformar formatos** con sistemas externos (importaciones, archivos de terceros). **Pub/Sub**: **notificaciones uno-a-muchos** (push, eventos). **Messaging**: **conectividad transitoria**, no hace falta respuesta inmediata, que no se pierdan pedidos. **Process Coordinator**: un **proceso de negocio complejo de varios pasos** en distintos servidores |

**Ejemplo de la cátedra (caso Fanáticos del Fútbol):**
| | Monolítico | Microservicios |
|---|---|---|
| Descomposición | Monolítico | Microservicios |
| Estructura interna | Layered, partición técnica | Hexagonal |
| Distribución | C/S N-Tier | C/S N-Tier |
| Comunicación | EDA Broker (minuto a minuto y cobros off-line) + Pub/Sub (notificaciones) | **REST** + Broker + Pub/Sub |

### C. Monolítico vs. Monolito modular vs. Microservicios
|                      | Monolítico                                                                | Monolito modular (partición de dominio)                                                | Microservicios                                                                                                                        |
| -------------------- | ------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Unidad de despliegue | Una                                                                       | **Una**                                                                                | Una por servicio                                                                                                                      |
| Organización         | Técnica (capas)                                                           | **Por dominio o módulos** de negocio                                                   | Por capacidad de negocio o subdominio (DDD)                                                                                           |
| Escalado             | Toda la app                                                               | Toda la app                                                                            | **Selectivo por servicio**                                                                                                            |
| Fallas               | Todo o nada                                                               | Todo o nada                                                                            | **Aisladas**                                                                                                                          |
| BD                   | Una                                                                       | Una (idealmente con esquemas por módulo)                                               | **Una por servicio**                                                                                                                  |
| Pros                 | Simple, fácil de desarrollar y probar, performance, pocos puntos de falla | Orden por negocio, fácil migrar a microservicios después, modificabilidad              | Despliegue independiente, escalabilidad, autonomía, agilidad, tecnología políglota                                                    |
| Contras              | Difícil de mantener al crecer, redespliegue total, un solo stack          | Igual que el monolito en despliegue y escalado. El código de personalización se repite | Complejidad distribuida, latencia, más puntos de falla, trazabilidad difícil, consistencia eventual, costo, necesita un equipo maduro |
| Cuándo               | Equipo chico, dominio nuevo, sistema autocontenido                        | Monolito que crece y se quiere ordenar o preparar para migrar                          | Varios equipos, cargas desiguales, despliegue continuo independiente, dominio maduro                                                  |

- **Fowler, *Monolith first***: empezar con un monolito y extraer microservicios **de a uno** cuando el dominio madura.
- **Patrones de microservicios:**
  - **API Gateway**: punto de entrada único. Hace ruteo, composición, traducción de protocolos y seguridad. **No contiene lógica de negocio.**
  - **BFF**: un gateway por tipo de cliente.
  - **Database per Service.**
  - **Descomposición** por capacidad de negocio o por subdominio, con subdominios **core**, **de soporte** y **genéricos**. Los genéricos se pueden tercerizar.
  - **Despliegue**: *Single Service per Host* (en VM o en contenedor) y *Multiple Services per Host*.

### D. Conflictos arquitectónicos y RNF
**Definición:** dos o más RNF con demandas opuestas. Se abordan **haciendo explícito el trade-off, priorizando con los interesados, buscando una solución de compromiso y documentándola**.

| RNF crítico | Estrategia arquitectónica |
|---|---|
| Performance | **Granularidad gruesa**: operaciones críticas en pocos subsistemas, poca comunicación |
| Seguridad | **Capas**, con los recursos críticos en las capas internas y alta validación |
| Protección | Localizarla en **uno o pocos subsistemas** |
| Disponibilidad | **Redundancia**: componentes reemplazables sin detener el sistema |
| Mantenibilidad | **Granularidad fina**, componentes autocontenidos, alta cohesión y bajo acoplamiento, sin estructuras compartidas |

**Conflictos clásicos y cómo resolverlos:**
| Conflicto | Solución típica |
|---|---|
| Granularidad gruesa (performance) vs. mantenibilidad | **Híbrido**: componentes gruesos en las partes estables y críticas en performance, finos en las que cambian mucho |
| Redundancia (disponibilidad) vs. seguridad | **Seguridad centralizada** y aplicada igual en todos los nodos, con parches y configuración automatizados |
| Seguridad (más capas y validaciones) vs. performance | Aplicar la seguridad fuerte **solo a lo crítico**. Caché, sesión o token para no revalidar en cada paso |
| Seguridad vs. usabilidad | **Autenticación escalonada o adaptativa**, biometría o dispositivo de confianza, MFA solo para lo sensible |

### E. Proceso de diseño arquitectónico (Gorton): iterativo
1. **Determinar los requerimientos arquitectónicos**:
   - **Entrada**: los RNF y CU del WF de Requerimientos.
   - Se seleccionan los RNF **significativos para la arquitectura (SPA)** y los CU que los resuelven.
   - Se **priorizan** en **Alto** (conducen el diseño, primera iteración), **Medio** (más adelante) y **Bajo** (lista de deseos).
   - La priorización es **engañosa**, porque los requerimientos entran en conflicto entre sí.
2. **Diseño arquitectónico**:
   - (a) **Elegir el framework** basado en estilos probados, porque las soluciones conocidas minimizan los riesgos.
   - (b) **Distribuir los componentes**: definir componentes, interfaces, responsabilidades, dependencias y particiones distribuibles.
   - **Salida**: las **vistas** y el **documento de arquitectura**, que justifica cada decisión.
3. **Validación**: es un desafío porque la arquitectura es un diseño y **no se puede ejecutar**. Hay dos técnicas:
   - **Escenarios** (técnica del SEI): un estímulo y la respuesta esperada de la arquitectura para un atributo de calidad.
   - **Prototipos**: **prueba de concepto** (¿se puede construir así?) y **prueba de tecnología** (¿la tecnología responde?). Se usan cuando el escenario no alcanza, por ejemplo con "500 órdenes en 5 minutos".

- La arquitectura se estabiliza en la **fase de Elaboración** del PUD, que está **centrado en la arquitectura** y guiado por casos de uso.
- **Lineamientos para los componentes:**
  - minimizar dependencias y llamadas entre componentes
  - alta cohesión
  - aislar el middleware y los productos COTS
  - descomponer jerárquicamente

### F. Estilos de comunicación: cuándo usar cada uno
| Estilo | Sincronía | Clave | Usar cuando… | Riesgo |
|---|---|---|---|---|
| **Request-Response / REST** | Síncrono | Cliente-servidor, **sin estado**, cacheable, interfaz uniforme, capas | Integración simple entre servicios o apps, interoperabilidad | Latencia por request, más puntos de falla, gobierno de APIs |
| **EDA – Broker** (topología) | Asíncrono | Sin control central: los eventos se encadenan | Eventos simples, alta escalabilidad | Testing difícil, inconsistencias |
| **EDA – Mediator** | Asíncrono | Un mediador decide el orden de los procesadores | Eventos complejos con orden | El mediador puede volverse cuello de botella |
| **Publish & Subscribe** | Asíncrono | Tópico, **muchos a muchos**, publicador y suscriptor no se conocen | **Notificaciones** uno-a-muchos (push) | La indirección del bus afecta la performance |
| **Broker (agente)** | Ambos | Hub-and-spoke, **ruteo + transformación de formatos** | Sistemas externos con **otros formatos** | Cuello de botella |
| **Messaging (colas)** | Asíncrono | Cola intermedia, entrega garantizada | **Conectividad transitoria**, no hace falta respuesta inmediata | Dependencia del formato de mensaje |
| **Process Coordinator** | Ambos | Encapsula la secuencia de un proceso de negocio | **Proceso complejo de varios pasos** en varios servidores | Punto único de falla, compensaciones |

**Qué se replica para tener disponibilidad:** el **tópico** en Pub/Sub, la **cola** en Messaging, el **broker** y el **coordinador**.

### G. Layered vs. Hexagonal
| Layered | Hexagonal (puertos y adaptadores) |
|---|---|
| Capas: presentación → negocio (aplicación y dominio) → datos → infraestructura. Cada capa usa solo la inferior | **Núcleo** de negocio aislado. Los **puertos** son interfaces que define el negocio y los **adaptadores** son las implementaciones tecnológicas intercambiables |
| ✅ Fácil de desarrollar, probar y mantener. Seguridad (las capas pueden aislarse en subredes) | ✅ Bajo acoplamiento, **testeabilidad** (se *mockean* los adaptadores), **intercambiabilidad**, protege el dominio (DDD) |
| ❌ Performance, escalabilidad monolítica, despliegue completo, fallas en cascada, un solo stack. Diseño **centrado en la BD** | ❌ Más abstracciones |
| Saltear capas (variante embebida) **anula sus beneficios** | **Lado conductor** (UI, REST): el adaptador usa el puerto. **Lado conducido** (BD, mail): el adaptador implementa el puerto, y ahí está la **inversión de dependencias** (todas las dependencias apuntan al núcleo) |

### H. Sistemas distribuidos
- Son sistemas cuyo **procesamiento** se reparte entre varios procesadores conectados por una red.
- **Características (ventajas):** compartición de recursos, apertura, concurrencia, tolerancia a fallas y escalabilidad (en tamaño, distribución y manejabilidad).
- **Desventajas:** complejidad, seguridad, manejabilidad (las fallas se propagan) e impredecibilidad.
- **Ataques:** intercepción, interrupción, modificación y fabricación.
- **Las 8 falacias:**
  1. La red es confiable.
  2. La latencia es cero.
  3. El ancho de banda es infinito.
  4. La red es segura.
  5. La topología no cambia.
  6. Hay un único administrador.
  7. El transporte no cuesta.
  8. La red es homogénea.

### I. Respuestas cortas a preguntas de parciales anteriores
- **Arquitectura de software**: es el **conjunto de decisiones significativas** sobre la organización del sistema (componentes, relaciones, entorno y principios que rigen su diseño y evolución). Resuelve **principalmente los RNF afectando lo menos posible a los RF**. Se documenta con **vistas (4+1)** y un **documento de arquitectura** que justifica las decisiones.
- **Rol del arquitecto:**
  - revisa y **negocia** los requerimientos
  - **documenta** y **comunica** la arquitectura
  - direcciona los RNF hacia la arquitectura
  - configura el hardware
  - se asegura de que la arquitectura se respete
  - colabora con el líder del proyecto en planificación y estimación
- **Estilo vs. patrón:**
  - El **estilo** organiza todo el sistema, nombra un diseño recurrente, no determina tecnología, y suele haber uno dominante.
  - El **patrón** es de menor escala, resuelve un problema recurrente y puede haber varios.
  - **Platónico** es el ideal de los libros. **Embebido** es el real, que viola restricciones y pierde atributos de calidad.
- **Big Ball of Mud**: lo que queda cuando no hay arquitectura definida. Es altamente acoplado e inmantenible.
- **¿Por qué documentar?** Para comunicar a los involucrados, analizar y evaluar el diseño, permitir reutilización a gran escala, y porque el contexto y la tecnología cambian: hay que registrar *por qué* se decidió algo.
- **SOA:**
  - Servicios reutilizables con estándares abiertos, contratos WSDL/XSD y un ESB.
  - Ventajas: interoperabilidad, reutilización, escalabilidad, testeabilidad y bajo acoplamiento.
  - Desventajas: performance, múltiples puntos de falla, volumetría y gobierno.
  - **SOA vs. microservicios**: SOA tiene servicios más grandes, con ESB, y busca integración empresarial. Los microservicios son pequeños, independientes, cada uno con su BD y APIs livianas.
- **ISO 25010, las 8 características de calidad de producto:**
  1. Adecuación funcional
  2. Eficiencia de desempeño
  3. Compatibilidad
  4. Usabilidad
  5. Fiabilidad
  6. Seguridad
  7. Mantenibilidad
  8. Portabilidad

  Los RNF deben ser **medibles y verificables**.

### J. Para la parte práctica (recordatorio rápido)
- **Vista de funcionalidad**: pocos CU, del 10 al 15 %, que cubran:
  - cada RNF significativo
  - cada patrón elegido
  - un ABMC complejo
  - la transacción más compleja
  - un informe o reporte
  - los procesos automáticos
  - el inicio de sesión

  Cada uno **se justifica con el RNF que resuelve**.
- **Vista de diseño con microservicios**:
  - UI (Web/Mobile) → **API Gateway** → microservicios.
  - Los dominios **core y de soporte** tienen **cada uno su BD**.
  - Los **genéricos** son servicios de integración o notificación, que pueden ser tercerizados.
  - Los **servicios externos** (FCM, Google Maps, biometría) se conectan con interfaces.
  - Componentes con **interfaces provistas y requeridas**. Dentro de un subsistema se usan dependencias.
- **Vista de despliegue**:
  - **Nodos como cubos**: dispositivo Android, servidor del API Gateway, un servidor o contenedor por microservicio con su BD (*single service per host*), y los **servidores externos** en otro color.
  - Si el despliegue es en la nube, se representan los **nodos cloud** (en el caso Cambalache, AWS).
