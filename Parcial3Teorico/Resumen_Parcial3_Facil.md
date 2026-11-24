# Diseño de Sistemas: Resumen fácil para el 3er parcial (teórico)

> **Cómo leer esto**
> - Cada tema arranca con **"En criollo"**: la idea en 2 o 3 líneas. Si solo te acordás de eso, ya tenés la base.
> - Después viene el detalle, con lo que piden las diapositivas.
> - Al final de cada tema hay una caja **🎯 Lo que preguntan** con lo que salió en el parcial anterior.
> - Al final del todo están **las 40 preguntas del parcial anterior con la respuesta correcta y el porqué**, y un **repaso relámpago** para leer antes de entrar.
>
> **Formato del parcial (2025):** 40 preguntas de opción múltiple, **una sola respuesta correcta** cada una, 1 punto cada una. Muchas opciones son del tipo "a) y c)", así que **leé todas las opciones antes de marcar**.

---

## 0. Las 3 ideas que explican TODO

Si entendés estas tres ideas, el 80% del parcial sale por sentido común.

1. **Alta cohesión**: cada clase hace **una sola cosa** y la hace bien. (Un cuchillo corta, no también abre latas, atornilla y hace café).
2. **Bajo acoplamiento**: las clases **dependen poco** unas de otras. Si cambio una, no se rompen las demás.
3. **Depender de abstracciones (interfaces), no de cosas concretas.** Como un enchufe: la lámpara no sabe ni le importa qué central eléctrica le da la luz; solo conoce "el enchufe".

> Casi todos los principios, patrones y decisiones de persistencia del parcial son **formas distintas de lograr estas tres cosas**. Cuando dudes entre dos opciones, elegí la que **baje el acoplamiento**, **suba la cohesión** o **use una interfaz**.

---

# 1. PROCESO DE DISEÑO

### En criollo
> El **análisis** dice **QUÉ** hay que resolver (el problema). El **diseño** dice **CÓMO** lo vamos a construir (la solución).
> Diseñar = pasar de un **modelo lógico** (una maqueta) a un **modelo físico** (el plano del arquitecto), teniendo en cuenta las restricciones del negocio y de la tecnología.

## 1.1 Dónde está el diseño en el PUD/RUP

El Proceso Unificado tiene etapas (workflows) en cadena. Cada una produce un modelo:

| Workflow | Produce | ¿De qué dominio? |
|---|---|---|
| Modelado de negocio | Modelo de negocio | |
| Requerimientos | Modelo de requerimientos | **Problema** |
| Análisis | Modelo de análisis | **Problema** |
| **Diseño** | **Modelo de diseño + Modelo de despliegue** | **Solución** |
| Implementación | Modelo de implementación | Solución |
| Prueba | Modelo de prueba | Solución |
| Despliegue | Versión del producto | |

## 1.2 Qué es diseñar (las 2 definiciones de la diapositiva)

1. Aplicar **técnicas y principios** para definir un sistema con **suficiente detalle como para poder construirlo** físicamente.
2. Proceso **iterativo** de transformar un **modelo lógico en un modelo físico**, teniendo en cuenta las **restricciones del negocio**.

## 1.3 Análisis vs. Diseño ⭐ (salió en el parcial)

**Frase para memorizar:** *el análisis modela la solución en términos **lógicos**; el diseño, en términos **físicos**.*

En la metodología estructurada estaba clarísimo:
- **Análisis** = descripción del **problema** → modelo esencial/conceptual → lo hace el **analista**.
- **Diseño** = descripción de la **solución** → modelo de implementación → lo hace el **diseñador**.

**Tabla del PUD (muy preguntable):**

| | **Análisis** | **Diseño** |
|---|---|---|
| Tipo de modelo | **Conceptual** (abstracción, no se mete con la implementación) | **Físico** (plano de la implementación) |
| ¿Genérico? | **Sí**: sirve para varios diseños | **No**: es para una implementación concreta |
| Formalidad | **Menos** formal | **Más** formal |
| Secuencia | Dinámico, **poco** centrado en la secuencia | Dinámico, **muy** centrado en la secuencia |
| Arquitectura | **Bosquejo** del diseño | **Manifiesto** del diseño |

> 🧠 Truco: **B**osquejo va con **A**nálisis (lo primero, lo borrador); **M**anifiesto va con diseño (lo definitivo).

**Tendencias actuales:** darle protagonismo a los **requerimientos**, prestar atención a la **arquitectura**, usar las **mismas herramientas de modelado** (UML) en todas las etapas y **usar un proceso**. Hoy la frontera entre análisis y diseño es **cada vez más difusa**.

## 1.4 Qué hay que diseñar: 6 aspectos (con la Arquitectura en el centro)

| Aspecto | Qué es, en simple |
|---|---|
| **Arquitectura** (el centro) | El **conjunto de decisiones importantes** sobre **cómo cumplir los requerimientos NO funcionales** (rendimiento, seguridad, disponibilidad…) tocando lo menos posible la funcionalidad. Son decisiones **estratégicas**. El **diseño arquitectónico** es el proceso para llegar a ella, pasando del modelo lógico a una **tecnología específica**. |
| **Datos** | Cómo **organizar, guardar y acceder** a los datos: persistencia, volúmenes, backups, transacciones, seguridad y control de acceso. |
| **Procesos** | Transformar las **realizaciones de casos de uso de análisis** en **realizaciones de casos de uso de diseño**, agregando lo no funcional (persistencia, concurrencia, seguridad) **usando patrones de diseño**. |
| **Experiencia de usuario (UX)** | Que usar el sistema sea **intuitivo, eficiente y satisfactorio**, de punta a punta. |
| **Formas de entrada/salida** | Cómo entra la información y cómo salen los resultados. Las de **entrada** solo hacen falta cuando el dato **entra en un momento distinto al que ocurre** (sistemas **batch / por lotes**: alguien llena un papel y otro lo carga después). Las de **salida** se usan **siempre** (en línea, batch y tiempo real). |
| **Procedimientos manuales** | Cómo se **integra el software al negocio**: que las tareas manuales se hagan bien y con pocos errores humanos. |

> ⚠️ La arquitectura se define por los requerimientos **NO funcionales**, no por los funcionales.

### Requerimientos funcionales vs. no funcionales (salió en el parcial)
- **Funcional** = **qué hace** el sistema: "el usuario puede registrar su cuenta", "puede editar sus datos".
- **No funcional** = **cómo de bien** lo hace (calidad, restricciones): "disponibilidad del 99%", "responder en menos de 2 segundos", "debe ser seguro".
> Truco: si tiene un **número de calidad** (porcentaje, segundos, cantidad de usuarios) o habla de seguridad, rendimiento o disponibilidad, es **no funcional**.

## 1.5 Cómo saber si un diseño es bueno (3 características)
1. **Cumple los requisitos**: los **explícitos** del análisis y los **implícitos** que quiere el cliente.
2. **Es legible y comprensible**: sirve de guía para los que programan, prueban y dan soporte.
3. **Da una imagen completa**: cubre **datos, funciones y comportamiento**.

## 1.6 Principios del proceso de diseño (5)
1. Se tiene que poder **rastrear hasta el análisis** (trazabilidad).
2. **No reinventar la rueda.**
3. **Minimizar la "distancia intelectual"** entre el software y el problema real (que se parezca a la realidad que modela).
4. Tener **uniformidad e integración**.
5. Estar **preparado para el cambio**.

## 1.7 Workflow de diseño

**Para qué sirve (5 propósitos):**
1. Entender a fondo los **requerimientos no funcionales y las restricciones tecnológicas**.
2. Ser la **entrada para la implementación**.
3. **Partir el trabajo** en pedazos manejables para que **varios equipos trabajen en paralelo**.
4. **Capturar las interfaces entre subsistemas.**
5. Crear una **abstracción de la solución** sin atarse a una tecnología.

**En qué fase aporta qué:**
- **Elaboración** → contribuye a una **arquitectura estable y robusta**.
- **Construcción** → crea el **plano para la implementación**.

**Quién hace qué:**

| Trabajador | Es responsable de |
|---|---|
| **Arquitecto** | Modelo de diseño, modelo de despliegue y descripción de la arquitectura |
| **Ingeniero de casos de uso** | Realizaciones de casos de uso de diseño |
| **Ingeniero de componentes** | Clases de diseño, subsistemas de diseño e interfaces |

**Entradas → Salidas:**
- **Entra:** modelo de análisis (realizaciones de CU de análisis, clases y paquetes de análisis) + descripción de la arquitectura.
- **Sale:** **modelo de diseño** (realizaciones de CU de diseño, clases de diseño, interfaces, descripción de la arquitectura) + **modelo de despliegue**.

## 1.8 Diagramas UML 2.0 en el diseño ⭐ (salió en el parcial)

| Modelan **ESTRUCTURA** (cómo está armado) | Modelan **COMPORTAMIENTO** (qué pasa / cómo se mueve) |
|---|---|
| De **clases**, de **paquetes**, de **estructura compuesta**, de **componentes**, de **despliegue**, de **objetos** | De **casos de uso**, de **actividad**, de **máquina de estados**, y los de interacción: de **secuencia**, de **comunicación**, de **timing**, de **descripción de la interacción** |

> 🧠 Truco: si el diagrama muestra **piezas** (clases, componentes, nodos de despliegue), es de **estructura**. Si muestra **cosas que pasan en el tiempo** (mensajes, pasos, estados), es de **comportamiento**. El de **secuencia es de comportamiento**, no de estructura.

> 🎯 **Lo que preguntan:** diferencia análisis/diseño (análisis = problema y modelo lógico; diseño = solución y plano para implementar), qué diagramas son de estructura (clases y despliegue, no secuencia) y cuáles requerimientos son no funcionales.

---

# 2. PRINCIPIOS DE DISEÑO

### En criollo
> Son **consejos de buena práctica**, más generales que los patrones. No son recetas: son **una forma de pensar** al diseñar. Todos buscan lo mismo: **alta cohesión y bajo acoplamiento**, para que el software sea fácil de mantener y de cambiar.

**Qué tiene que considerar un buen diseño:** independencia de componentes (**cohesión** + **acoplamiento**), tratamiento de **excepciones** y **prevención/tolerancia de defectos**.

## 2.1 Los principios "generales"

### DRY: Don't Repeat Yourself (no te repitas)
- **Qué dice:** no copies y pegues código. **Cada regla o dato tiene que estar en un único lugar.**
- **Por qué:** si la misma regla está en 5 lugares y cambia, tenés que acordarte de cambiarla en los 5. Si te olvidás de uno, aparece un bug.
- **Cómo:** creando **abstracciones** (un método, una clase o una superclase que tenga lo común).

### SoC: Separation of Concerns (separación de intereses)
- **Qué dice:** dividí el sistema en **partes separadas**, cada una con **un tema distinto** y que se pisen lo menos posible. Como un cajón de cubiertos: cada cosa en su lugar.
- **Se aplica en 2 niveles:**
  - **Arquitectónico:** separar en **capas** (interfaz, negocio, datos).
  - **De programación:** separar en clases y métodos.
- **Ejemplo de clase:** una clase `Pago` que además tenía los datos del cliente y un `borrarOrden()` mezclaba temas. Se separa en `Pago`, `Orden` y `Cliente`.
- **Beneficios:** maneja la complejidad, mejora la cohesión, baja el acoplamiento, facilita extender (OCP), permite reutilizar y hace los módulos menos frágiles.

### YAGNI: You Ain't Gonna Need It (no lo vas a necesitar)
- **Qué dice:** **no programes cosas "por si algún día hacen falta".** El futuro siempre cambia, así que es tiempo perdido.
- Ejemplo: agregar funciones "en reserva" que hoy nadie usa **viola YAGNI** (salió en el parcial).

### KISS: Keep It Simple, Stupid (mantenelo simple)
- **Qué dice:** la solución más simple que funcione es la mejor.
- **Implica:** no subir infinitamente el nivel de abstracción; no meter funciones "en reserva" (para eso es más preciso YAGNI); no meter una librería gigante para usar dos funciones; partir lo complejo en piezas simples (acá se parece a DRY); usar la precisión y el detalle **necesarios**, no los máximos posibles.
- **Beneficios:** código de mejor calidad, más fácil de mantener, ampliar y modificar.

> ⚠️ **YAGNI vs. KISS:** YAGNI = "**no agregues** lo que no necesitás". KISS = "**hacé simple** lo que sí necesitás".

### Ley de Demeter / Principio de Menor Conocimiento ("no hables con extraños") ⭐
- **Qué dice:** cada objeto tiene que **conocer lo menos posible** a los demás. Hablá solo con tus **amigos inmediatos**.
- **Propósito (salió en el parcial):** **producir objetos débilmente acoplados.**
- **Mal ejemplo:** `empleado.getEmpresa().getDireccion().getCiudad()`. El empleado está hablando con un "extraño" (la dirección de la empresa). Si cambia `Direccion`, se rompe `Empleado`.
- **Regla: un método M de un objeto O solo puede llamar métodos de:**
  1. **O mismo**,
  2. **los parámetros** de M,
  3. **los objetos que M crea**,
  4. **los atributos directos** de O.
- **Ventajas:** más legible, más cohesivo, más fácil de mantener y adaptar; menos cambios en cadena.
- **Desventaja:** hacen falta **muchos métodos envoltorio (wrappers)** para pasar información entre objetos lejanos.

### Tell, Don't Ask (decí, no preguntes)
- **Mal:** le **pedís los datos** a un objeto y **decidís vos afuera** qué hacer.
- **Bien:** le **decís al objeto que haga** la cosa. La lógica queda **junto a los datos**.
- Cadena de la diapositiva: no usar los objetos para pedirles cosas → decirles que hagan cosas → un objeto se define por su comportamiento → distribución de responsabilidades → **polimorfismo**.

### Interfaz y relación de realización
- **Interfaz:** un **contrato** que dice **qué métodos** tiene que tener una clase, **pero no cómo** se implementan. Permite definir comportamiento común sin conocer los detalles.
- **Realización** (en UML, línea **punteada** con triángulo vacío): una clase concreta **implementa** la interfaz y se compromete a implementar **todos** sus métodos.

### Program to an Interface (programar hacia la interfaz, no hacia la implementación) ⭐
- **Qué dice:** **dependé de interfaces o clases abstractas, no de clases concretas.** Así, si cambia la implementación, el código cliente **no se entera**.
- **Origen del nombre:** el IMP de ARPANET. Las computadoras no se hablaban directamente: todas hablaban con el IMP usando un **protocolo común** (una "interfaz").
- **Ejemplo del Equipo:** `Equipo.agregarJugador(???)`. Si recibe `JugadorBasket`, `JugadorTenis`, `JugadorFutbol`… queda atado a 5 clases.
  - **Solución en 4 pasos:** (1) ver qué necesita el Equipo de los jugadores (`getDeporte()`, `jugar()`); (2) crear la interfaz `Atleta` con esos métodos; (3) que cada jugador **implemente** `Atleta`; (4) que el Equipo dependa de la interfaz: `agregarJugador(Atleta)`.
- **Trampas:** (1) tener la interfaz pero **seguir usando las clases concretas**; (2) **una interfaz para todo** (abusar).
- **Cuándo usar interfaces:** cuando hay **varias implementaciones** posibles, o para **desacoplarte de un sistema externo**.

### Favorecer la composición sobre la herencia ⭐
- **Herencia = reuso de "caja blanca":** la hija **ve por dentro** al padre. Se define **en compilación** (estático). **Rompe el encapsulamiento** y **acopla fuerte**.
- **Composición = reuso de "caja negra":** un objeto **tiene** a otro y lo usa **solo por su interfaz**, sin ver adentro. Se puede **cambiar en tiempo de ejecución**. **No rompe el encapsulamiento** y **acopla débil**.
- **Delegación** (salió en el parcial): una clase **le pasa parte de su trabajo a otro objeto** pidiéndole que lo haga. Ejemplo: `Ventana.area()` hace `return rectangulo.area()`. La ventana **no hereda** de Rectángulo: lo **tiene** y le delega.
- **Frase clave:** *la delegación es reuso a nivel de **objetos**; la herencia es reuso a nivel de **clases**.*
- **Resultado:** acoplamiento **débil** y reuso **a nivel de objetos** (respuesta del parcial).

| | Herencia | Composición |
|---|---|---|
| Relación | "**es un**" | "**tiene un**" |
| Reuso | Caja **blanca** | Caja **negra** (delegación) |
| ¿Cuándo se define? | En compilación (fijo) | En ejecución (se puede cambiar) |
| Encapsulamiento | Lo **rompe** | Lo **respeta** |
| Acoplamiento | **Alto** | **Bajo** |
| Flexibilidad | Baja | Alta |

## 2.2 SOLID ⭐⭐

| Letra | Nombre | En una frase | Logra… |
|---|---|---|---|
| **S** | **Single Responsibility** (responsabilidad única) | Una clase tiene **una sola razón para cambiar** | **Alta cohesión** |
| **O** | **Open/Closed** (abierto/cerrado) | **Abierta para extender, cerrada para modificar**: agregás clases nuevas en vez de tocar las que andan | **Flexibilidad** |
| **L** | **Liskov Substitution** (sustitución de Liskov) | Una **hija tiene que poder reemplazar al padre** sin romper nada | **Herencia bien hecha** |
| **I** | **Interface Segregation** (segregación de interfaces) | **Interfaces chicas y específicas**: nadie debe depender de métodos que no usa | **Bajo acoplamiento** |
| **D** | **Dependency Inversion** (inversión de dependencias) | Alto y bajo nivel dependen de **abstracciones**, no uno del otro | **Flexibilidad, reuso** |

### S: Responsabilidad única (SRP)
- Ejemplo del robot: ✗ "soy chef, jardinero, pintor y chofer"; ✓ un robot por tarea.
- Ejemplo del **Automóvil**: un auto que **se arranca** y **se detiene** está bien; un auto que **se lava solo**, **se conduce solo** o **se cambia las ruedas** viola SRP. Se arregla creando `Lavadero`, `Conductor` y `Mecanico`.

### O: Abierto/Cerrado (OCP)
- Ejemplo del **Pintor**: ✗ cada estilo nuevo obliga a **modificar** `pintar()` con otro `if`. ✓ Se crea una clase abstracta `EstiloPintura` con hijas `EstiloModerno`, `EstiloCubista`… Un estilo nuevo = **una clase nueva**, y el Pintor no se toca.
- **Corolario: "encapsulá lo que varía"**: separá lo que cambia y ponelo en otra clase. **Es la base de Strategy y State.**

### L: Sustitución de Liskov (LSP)
- Robot: Sam hace café; Eden es hija de Sam. Si le pedís café a Eden y te da agua, **la herencia está mal**.
- "Si parece un pato y hace cuac como un pato, pero **necesita pilas**… capaz que la abstracción está mal".
- ✗ **Herencia de datos:** `Persona ← Empleado, Cliente` solo para reusar atributos, aunque no se comportan igual.
- ✓ **Herencia de comportamiento:** `Empleado ← EmpleadoProduccion, EmpleadoComercial`, cada uno redefine `calcularSueldo()`. Cualquiera sirve donde se espera un `Empleado`.
- **Respuesta del parcial:** *las subclases pueden reemplazar a sus superclases sin alterar el comportamiento esperado.* (Una hija **no puede** violar las pre/postcondiciones del padre "porque es más rápida").

### I: Segregación de interfaces (ISP)
- Robot: un pizarrón que dice "todos los robots: girar, mover brazos, mover antenas", y uno contesta "¡no tengo antenas!".
- Ejemplo: una interfaz gorda `Vehiculo` (acelerar, frenar, sacarCD, prenderLuces…) que usa una `Autopista`, que no necesita `sacarCD()`. Se parte en interfaces chicas, una por cliente (`VehiculoA` para Autopista, `VehiculoE` para Estacionamiento).
- **Respuesta del parcial:** *no obligar a los clientes a depender de métodos que no usan* **y** *dividir las interfaces en partes más chicas y específicas*.

### D: Inversión de dependencias (DIP)
- **Dos premisas:**
  - **A:** los módulos de **alto nivel** (la lógica de negocio) **no dependen** de los de **bajo nivel** (BD, red, archivos). **Ambos dependen de abstracciones.**
  - **B:** las abstracciones **no dependen de los detalles**; los **detalles dependen de las abstracciones**.
- Robot: ✗ "corto pizza con mi brazo-cortapizza soldado"; ✓ "corto pizza con la herramienta que me den".
- "¿Soldarías una lámpara directo a los cables de la pared?" No: usás un **enchufe** (la interfaz).
- **Ejemplo del parcial:** para poder pasar de leer un archivo local a leer una API sin tocar las clases de alto nivel, se define una **interfaz abstracta para el origen de datos**, y las clases de alto nivel dependen de **esa interfaz**.
- Se relaciona con el **principio de Hollywood**: "no nos llames, nosotros te llamamos".

## 2.3 Principios de implementación en POO (para programar)
CamelCase, métodos cortos (7±2 líneas), escribir para quien lo va a mantener, **principio del menor asombro** (el nombre del método dice lo que hace), abrazar el cambio, reusar, usar convenciones. En síntesis: **"no me hagas pensar"**.

> 🎯 **Lo que preguntan:** propósito de Demeter (objetos débilmente acoplados), qué viola el código "en reserva" (YAGNI), qué acoplamiento da la composición (débil y a nivel de objetos), qué es delegación, qué es ISP, qué es DIP, qué es LSP y programar hacia la interfaz.

---

# 3. PATRONES DE DISEÑO

### En criollo
> Un patrón es una **solución probada y con nombre para un problema de diseño que se repite.** Es como una receta de cocina que ya funcionó mil veces: no la inventás de nuevo, la aplicás. Son **independientes del lenguaje** y de **escala media** (mejoran un pedazo del sistema, **no cambian la arquitectura**).

> 🎯 **LOS 5 PATRONES QUE SE EVALÚAN: State, Strategy, Observer, Adapter e Iterator.** Los otros conviene conocerlos de nombre.

## 3.1 Conceptos generales

- **Patrón vs. idioma:** los dos resuelven problemas recurrentes, pero el **patrón es genérico e independiente del lenguaje**; el **idioma** es de **bajo nivel** y **específico de un lenguaje**.
- **Los 4 elementos de un patrón:** **nombre, problema, solución y consecuencias**.
- **Plantilla GoF:** nombre, propósito, sinónimos, motivación, aplicabilidad, estructura, participantes, colaboraciones, consecuencias, implementación, código de ejemplo, usos conocidos, patrones relacionados.

**¿A qué ayudan los patrones? (6)**
1. **Encontrar los objetos**, incluso los que no existen en el mundo real ("clases de **fabricación pura**", como un gestor o una interfaz).
2. **Determinar la granularidad** (cuánta responsabilidad tiene cada clase).
3. **Especificar interfaces.**
4. **Especificar la implementación** ("programar hacia la interfaz").
5. **Favorecer la reutilización** ("composición sobre herencia").
6. **Diseñar para el cambio** (cada patrón deja que **un aspecto varíe** sin afectar al resto).

**¿Para qué sirven, en resumen? (respuestas del parcial)**
- **Estandarizan soluciones probadas** que favorecen la **reutilización, el mantenimiento y el desacoplamiento**.
- Su **consecuencia fundamental** es **aislar a los clientes de las clases concretas**, lo que da **flexibilidad, extensibilidad y portabilidad**.
- **Aíslan los cambios** en distintas partes del sistema y dan un **lenguaje común** para que los desarrolladores se entiendan.
- ❌ **No** reemplazan a los principios SOLID, **no** eliminan la herencia ni las interfaces, **no** garantizan persistencia automática y **no** crean dependencias fuertes.

## 3.2 Clasificación (GoF)

| | **Creación** (cómo se crean objetos) | **Estructura** (cómo se arman/combinan clases y objetos) | **Comportamiento** (cómo se comunican y reparten responsabilidades) |
|---|---|---|---|
| **De clase** (usa herencia) | Factory Method | **Adapter** | Interpreter, Template Method |
| **De objeto** (usa composición) | Abstract Factory, Builder, Prototype, Singleton | **Adapter**, Bridge, Composite, Decorator, Facade, Flyweight, Proxy | Chain of Responsibility, Command, **Iterator**, Mediator, Memento, **Observer**, **State**, **Strategy**, Visitor |

> 🧠 **Para no confundirte:** de los 5 evaluables, **Adapter es el único de ESTRUCTURA** (y el único que es de clase **y** de objeto). **State, Strategy, Observer e Iterator son de COMPORTAMIENTO.** Ninguno es de creación.

---

## 3.3 ⭐ STATE (Estado): comportamiento

### En criollo
> Un objeto que **se comporta distinto según en qué estado está**. En vez de llenar todo de `if (estado == "confirmado") ... else if ...`, **cada estado es una clase**, y el objeto **le delega** el trabajo a su estado actual. **Parece que el objeto cambiara de clase.**

**Ejemplo mental:** un semáforo, o un pedido de compras. "Cancelar" hace algo distinto si el pedido está en el carrito, confirmado o ya despachado.

**Problema que resuelve:** con muchos estados, cada método termina con un **condicional monstruoso**, y cualquier cambio en las transiciones obliga a tocar **todos** esos condicionales.

**Cuándo usarlo:** cuando el comportamiento **depende del estado** y cambia **en tiempo de ejecución**, o cuando hay **grandes CASE/switch** que preguntan por el estado.

**Estructura:**
```
 Contexto ◇──────► Estado (abstracta o interfaz)
 solicitud()        operacion()
 { estado.operacion() }   △
                ┌─────────┴─────────┐
          EstadoConcretoA     EstadoConcretoB
          operacion()          operacion()
```

| Participante | Qué hace | Ejemplo de clase (Venta Digital) |
|---|---|---|
| **Contexto** | Recibe los pedidos y **los delega** al estado actual; guarda la referencia al estado (`setEstado`) | `Pedido` |
| **Estado** | Declara **todos** los métodos que dependen del estado | `EstadoPedido` (abstracta) |
| **Estados concretos** | Cada uno implementa **lo que hace en ese estado** | `EnCarroDeCompras`, `Confirmado`, `EnPreparacion`, `Despachado`, `Cancelado`, `Finalizado` |

- **Transiciones:** el estado concreto **crea el siguiente estado** y se lo pasa al Pedido (`setEstado(nuevo)`). Ejemplo: `EnCarroDeCompras.confirmarPedido()` crea un `Confirmado`.
- **Criterio de la cátedra:** en `EstadoPedido` todos los métodos tienen un **comportamiento por defecto** (los `boolean` devuelven `false`, los que devuelven un estado devuelven `null`), y cada estado concreto **redefine solo lo que puede hacer**.

**¿Herencia o interfaz para los estados? (salió en el parcial)**
> Conviene que los estados **hereden de una clase abstracta común** cuando **varios estados comparten parte del comportamiento**: lo común va en la clase abstracta y así **no se duplica código** (DRY).

**Consecuencias:**
- ✅ **Todo el comportamiento de un estado queda en una sola clase.**
- ✅ **Reemplaza los CASE por polimorfismo.**
- ✅ **Evita estados inconsistentes** y hace las **transiciones explícitas**.
- ❌ **Aumenta el número de clases/objetos.** (Solución: **compartir** los objetos estado).

**Principios que cumple:** SRP (cada estado se ocupa de lo suyo), OCP (un estado nuevo = una clase nueva), DIP (el contexto depende de la abstracción `Estado`).

---

## 3.4 ⭐ STRATEGY (Estrategia): comportamiento

### En criollo
> Hay **varias formas (algoritmos) de hacer la misma cosa**. Cada forma va a **su propia clase**, todas con la **misma interfaz**, y el contexto usa la que esté configurada. Se pueden **intercambiar** sin tocar al que las usa.

**Ejemplo mental:** el GPS que calcula la ruta "en auto", "a pie" o "en bici". Misma pregunta, distinto algoritmo. O el DT de fútbol que elige la estrategia: ataque, defensa, contragolpe.

**Cuándo usarlo:** cuando necesitás **variantes de un algoritmo**, o una clase está llena de **CASE** para elegir entre comportamientos.

**Estructura:** ¡**igual a la de State**!
```
 Contexto ◇──────► Estrategia (interfaz)
 interfaceContexto()   algoritmo()
                          △
                 ┌────────┴────────┐
            EstrategiaA       EstrategiaB
```

| Participante | Qué hace | Ejemplo de clase (Liga de Fútbol) |
|---|---|---|
| **Estrategia** | Interfaz **común a todos los algoritmos** | `IEstrategiaSistemaDeCampeonato` (`calcularCantFechas`, `calcularCantPartidos`) |
| **Estrategias concretas** | Cada una implementa **una variante** del algoritmo | `EstrategiaTodosContraTodos`, `EstrategiaEliminatorias` |
| **Contexto** | Tiene la referencia a la estrategia, **la configura** (antes de usarla) y **ejecuta** los métodos polimórficos | `SistemaCampeonato` |

- En "todos contra todos": fechas = clubes − 1; partidos = fechas × (clubes / 2). En "eliminatorias" la fórmula es otra. Sin el patrón, son `if` dentro del gestor.

**Ejemplo del parcial (OrdenDePago):** una clase con muchos `if` para calcular el monto según la forma de pago → **aplicar Strategy** (una estrategia por forma de cálculo) **y** cumplir **OCP** (agregar formas nuevas sin tocar `OrdenDePago`).

**Consecuencias:**
- ✅ Define una **familia de algoritmos** relacionados.
- ✅ Es una **alternativa a crear subclases** del contexto.
- ✅ **Elimina los CASE.**
- ✅ Se puede **cambiar el algoritmo en tiempo de ejecución**.
- ⚠️ El cliente **tiene que conocer las diferencias** entre estrategias para elegir.
- ❌ **Aumenta el número de objetos.**

**Principios que cumple:** **OCP** (algoritmo nuevo = clase nueva, sin tocar lo existente), **programar hacia la interfaz** (el contexto depende de la interfaz Estrategia, no de las concretas), SRP y DIP.

## 3.5 ⭐⭐ State vs. Strategy (la pregunta trampa)

**Tienen el MISMO diagrama de clases. Cambia la INTENCIÓN (el propósito).**

| | **State** | **Strategy** |
|---|---|---|
| ¿Para qué? | El objeto **cambia de comportamiento solo**, según su **estado interno** | Se **configura el contexto con un algoritmo** intercambiable |
| ¿Quién decide la clase concreta? | **Los propios estados** (o el contexto) hacen las transiciones | El **cliente o el contexto** elige una vez |
| ¿Cambia sola durante la vida del objeto? | **Sí**, pasa de un estado a otro automáticamente | En general **no** |
| ¿Las concretas se conocen entre sí? | **Sí** (un estado crea al siguiente) | **No** (son independientes) |

> 🧠 **Frase para el parcial:** *Strategy configura al contexto con un algoritmo o comportamiento intercambiable; State permite que el contexto cambie de comportamiento automáticamente según su estado interno.*

---

## 3.6 ⭐ OBSERVER (Observador / Publicar-Suscribir): comportamiento

### En criollo
> Como **suscribirse a un canal de YouTube**: el canal (**sujeto**) tiene una **lista de suscriptores** (**observadores**). Cuando sube un video (cambia), **les avisa a todos**. El canal **no sabe quién es cada suscriptor**: solo sabe que todos entienden "te aviso" (la interfaz).

**Propósito:** dependencia **uno a muchos**: cuando un objeto cambia, **todos los que dependen de él se enteran y se actualizan solos**.

**Ejemplo GoF:** una tabla de datos (el modelo) mostrada en un gráfico de barras y uno de torta. Si cambia un número, **se actualizan los dos gráficos**.

**Cuándo usarlo:** cuando un cambio en un objeto obliga a cambiar otros y **no sabés cuántos son**, o cuando hay que avisar **sin saber quiénes son**.

**Estructura:**
```
 Sujeto (interfaz)                    Observador (interfaz)
 suscribir(o) / quitar(o)  ──1..*──►  actualizar()
 notificar() { para cada o: o.actualizar() }
        △                                   △
 SujetoConcreto                      ObservadorConcreto
 (guarda el estado que importa)      (se mantiene consistente con el sujeto)
```

| Participante | Qué hace | Ejemplo de clase (Venta Digital) |
|---|---|---|
| **Sujeto** | Interfaz para **suscribir, quitar y notificar** | `ISujetoNotificacionPedido` |
| **Observador** | Interfaz para **actualizarse** | `IObserverNotificacionPedido` |
| **Sujeto concreto** | Guarda el estado y **avisa** cuando cambia | `GestorPedido` |
| **Observadores concretos** | Reaccionan al aviso | `NotificacionConfirmacionPedido`, `NotificacionDespachoRetrasado` (mandan mails) |

### PULL vs. PUSH

| | **PULL ("tirar")** | **PUSH ("empujar")** |
|---|---|---|
| El sujeto… | **solo avisa** que algo cambió | **manda los datos** junto con el aviso |
| El observador… | **va a buscar** lo que necesita | ya **recibe todo** |
| Ejemplo | `enviarMail(pedido)` y el observador pide `getNumero()` | `actualizar(nroPedido, fecha, email, detalle)` |

**Consecuencias:**
- ✅ **Acoplamiento abstracto y mínimo** entre Sujeto y Observador, porque el sujeto **solo conoce la interfaz** del observador, **no las clases concretas** (respuesta del parcial).
- ✅ Se pueden **agregar o quitar observadores sin tocar el sujeto**.
- ✅ Comunicación **broadcast** (a todos a la vez).
- ❌ **Actualizaciones inesperadas** en cascada; con una interfaz simple, el observador tiene que **adivinar qué cambió**.

**GestorDeCambios:** si la actualización es compleja, un objeto intermedio maneja quién observa a quién. Es un **Mediator** y, como hay uno solo, un **Singleton**. (Es un agregado opcional: el patrón **no exige** usarlo).

**Principios que cumple (salió en el parcial):** **DIP** (el sujeto depende de la interfaz Observador), **OCP** (observadores nuevos sin modificar el sujeto) y **SRP** (la notificación queda separada de la lógica principal). **NO** es LSP: los observadores **no heredan del sujeto**.

---

## 3.7 ⭐ ADAPTER (Adaptador / Wrapper): ESTRUCTURA

### En criollo
> Un **adaptador de enchufe** de viaje. Tenés una clase (muchas veces **de terceros**) que hace lo que necesitás, pero **con una interfaz que no encaja** con tu código. Ponés una clase en el medio que **traduce** de una interfaz a la otra. **Ni tu código ni la clase de terceros cambian**: solo agregás el adaptador.

**Ejemplo de la diapositiva:** tu sistema maneja **XML** y una librería de análisis solo acepta **JSON** → un "adaptador XML a JSON" en el medio.

**Ejemplo de clase (tarjetas de crédito):** el sistema tiene que autorizar ventas con Visa y Mastercard, y cada una tiene **su propia API** distinta.

| Participante | Qué hace | Ejemplo de clase |
|---|---|---|
| **Cliente** | Tiene la **lógica de negocio** | `GestorAutorizacionVenta` |
| **Objetivo (Target)** | La **interfaz que el cliente sabe usar** | `IAutorizadorTarjetaDeCredito` |
| **Adaptable (Adaptee)** | La clase de terceros **incompatible** | `ServicioAutorizadorOperacionesTarjetaVisa` |
| **Adaptador** | Implementa la interfaz Objetivo y **envuelve** al Adaptable, traduciendo las llamadas | `AutorizadorTarjetaDeCreditoVisa`, `...Mastercard` |

**Dos versiones:**
- **Adaptador de OBJETO** (el más usado): usa **composición**: el adaptador **tiene adentro** al adaptable. Sirve **en cualquier lenguaje**.
- **Adaptador de CLASE:** usa **herencia múltiple** (hereda del Objetivo **y** del Adaptable). **Solo funciona en lenguajes con herencia múltiple** (C++ sí; Java y C# no), y solo sirve para un adaptable concreto.

**Pasos para implementarlo:** (1) identificar al cliente y las partes a adaptar; (2) diseñar una interfaz común; (3) crear un adaptador por cada parte, que **contenga** una instancia de la clase adaptada; (4) el cliente pasa a depender de la interfaz.

**Consecuencias:**
- ✅ **SRP:** separa el **código de conversión** de la **lógica de negocio**. **Es "la ventaja" que preguntan.**
- ✅ **OCP:** agregar una tarjeta nueva = un adaptador nuevo, **sin tocar al cliente**.
- ✅ **DIP:** el cliente depende de la interfaz, no de la implementación concreta.
- ✅ **DRY:** si varias partes repetían el código de conversión, el adaptador lo **centraliza en una sola clase** (salió en el parcial).
- ❌ Más clases e interfaces (más complejidad). A veces es más simple cambiar la clase de servicio.

> ⚠️ El Adapter **no obliga a modificar al cliente**, y el adaptador de objeto **no hereda**: compone.

---

## 3.8 ⭐ ITERATOR (Iterador / Cursor): comportamiento

### En criollo
> Sacar el **"cómo recorrer"** una colección **afuera** de la colección, a un objeto **iterador** que sabe **en qué posición va**. Así recorrés cualquier colección (lista, árbol, lo que sea) **sin saber cómo está guardada por dentro**.

**Ejemplo mental:** el control remoto con "canal siguiente". No te importa cómo guarda la tele los canales: apretás "siguiente" y listo.

**Propósito:** acceder **secuencialmente** a los elementos de un agregado (colección) **sin exponer su representación interna**.

**Cuándo usarlo:** para recorrer sin mostrar la estructura interna, para permitir **varios recorridos a la vez**, o para tener **una forma uniforme** de recorrer distintas colecciones (**iteración polimórfica**).

**Estructura:**
```
 Agregado (interfaz)       Cliente        Iterador (interfaz)
 crearIterador()     ◄────────┴───────►   primero() / siguiente()
        △                                 haTerminado() / actual()
 AgregadoConcreto  ── crea ──►               △
 crearIterador(){ return new IteradorConcreto(this) }
                                       IteradorConcreto (guarda la posición)
```

| Participante | Qué hace | Ejemplo de clase |
|---|---|---|
| **Iterador** | Interfaz para **recorrer y acceder** (`primero`, `siguiente`, `haTerminado`, `actual`) | `IIterador` |
| **Iterador concreto** | Lo implementa y **guarda la posición actual** | `IteradorArticulos`, `IteradorSecciones` |
| **Agregado** | Interfaz para **crear** un iterador | `IAgregado` |
| **Agregado concreto** | **Crea y devuelve** el iterador que corresponde | `Catalogo`, `SeccionCatalogo` |

- **Quién crea el iterador:** el **agregado**, con `crearIterador()` (eso es un **Factory Method**).
- Puede tener **filtros** (`cumpleFiltro()`), por ejemplo para mostrar solo artículos activos.

**Consecuencias:**
- ✅ **Simplifica la interfaz de la colección** (le saca los métodos de recorrido).
- ✅ Permite **varios recorridos simultáneos** (cada iterador guarda su propio estado).
- ✅ Soporta **distintas formas de recorrer** (alcanza con cambiar de iterador).
- ❌ Puede ser excesivo para colecciones simples.

**Principios que cumple:** **SRP** (la responsabilidad de recorrer pasa a otra clase; es "la" respuesta del parcial), SoC, OCP y programar hacia la interfaz.

---

## 3.9 Otros patrones (conocelos de nombre y propósito)

| Patrón | Tipo | En criollo |
|---|---|---|
| **Singleton** | Creación | **Una sola instancia** de la clase y un acceso global (ej.: el generador de números de pedido, la configuración, el log) |
| **Factory Method** | Creación | Las **subclases deciden qué objeto crear** |
| **Abstract Factory** | Creación | Crear **familias de objetos relacionados** (ej.: todo "argentino" o todo "chileno") |
| **Builder** | Creación | Construir un objeto complejo **paso a paso**; el mismo proceso da distintas representaciones |
| **Prototype** | Creación | Crear objetos **copiando** uno existente |
| **Composite** | Estructura | **Árboles todo/parte**: tratar igual a una hoja y a un grupo (ej.: catálogo → secciones → artículos) |
| **Decorator** | Estructura | Agregar funcionalidades **envolviendo** al objeto |
| **Facade** | Estructura | Una **interfaz simple** delante de un subsistema complicado |
| **Proxy** | Estructura | Un **sustituto** que controla el acceso a otro objeto |
| **Bridge** | Estructura | Separar una abstracción de su implementación para que varíen por separado |
| **Flyweight** | Estructura | **Compartir** objetos chiquitos para ahorrar memoria |
| **Template Method** | Comportamiento | El **esqueleto del algoritmo** está en el padre y las hijas completan pasos (principio de Hollywood) |
| **Command** | Comportamiento | Convertir un **pedido en un objeto** (para encolar o deshacer) |
| **Chain of Responsibility** | Comportamiento | El pedido pasa por una **cadena** hasta que alguien lo atiende |
| **Mediator** | Comportamiento | Un objeto central **coordina** cómo se hablan los demás |
| **Memento** | Comportamiento | Guardar una **"foto" del estado** para poder deshacer, sin romper el encapsulamiento |
| **Visitor** | Comportamiento | Agregar **operaciones nuevas** sin cambiar las clases de los elementos |

## 3.10 ⭐⭐ Relación patrones ↔ principios (lo preguntan mucho)

| Patrón | Principios que **favorece** | Por qué |
|---|---|---|
| **Strategy** | **OCP**, programar hacia la interfaz, DIP, SRP | Algoritmo nuevo = clase nueva; el contexto depende de la interfaz |
| **State** | **OCP**, SRP, DIP | Estado nuevo = clase nueva; cada estado se ocupa de lo suyo |
| **Observer** | **DIP**, **OCP**, **SRP** | El sujeto conoce solo la interfaz; observadores nuevos sin tocar el sujeto |
| **Adapter** | **SRP**, **OCP**, **DIP**, DRY | La conversión va aparte; adaptadores nuevos sin tocar al cliente; el cliente depende de la interfaz |
| **Iterator** | **SRP**, SoC, OCP | Recorrer pasa a ser responsabilidad del iterador |

**Afirmaciones FALSAS típicas (no las marques):**
- ❌ "Singleton favorece SRP al centralizar muchas responsabilidades en una instancia". Centralizar muchas responsabilidades es **justo lo contrario** de SRP.
- ❌ "Observer viola DIP porque depende de clases concretas". Depende de **interfaces**.
- ❌ "Observer cumple LSP porque los observadores heredan del sujeto". **No heredan del sujeto.**
- ❌ "Strategy obliga a que todas las estrategias tengan el mismo comportamiento" o "elimina la necesidad de interfaces".

> 🎯 **Lo que preguntan:** OrdenDePago con `if` → Strategy + OCP; cuándo usar herencia en State (comportamiento común); por qué Observer baja el acoplamiento (sujeto conoce solo la interfaz); ventaja del Adapter de objetos (separar conversión de negocio); qué principio respeta Iterator (SRP); State vs. Strategy (intención); asociaciones patrón-principio correctas.

---

# 4. DISEÑO DE PERSISTENCIA

### En criollo
> Los objetos viven en la **memoria RAM**, que se borra cuando cerrás el programa. Para que **sobrevivan**, hay que guardarlos en una **base de datos**. Como casi siempre es una **base relacional (tablas)**, y los objetos no son tablas, hay que **traducir objetos ⇄ tablas**. Esa "incompatibilidad" se llama **problema de impedancia**.

## 4.1 Objetos y paradigmas
- **Objeto** = **identidad** (qué lo distingue de los demás) + **estado** (el valor de sus atributos: qué sabe) + **comportamiento** (sus métodos: qué puede hacer).
  - Ejemplo: clase `Auto` (marca, modelo, color, dominio…) y el objeto "mi auto" (Chevrolet, Classic, Blanco, MBO056, 2012).
- **Paradigma estructurado:** **datos por un lado y funciones por otro**; usa bases relacionales.
- **Paradigma OO:** datos y comportamiento **juntos**.
- El diseño **mezcla los dos mundos**: programamos con objetos pero guardamos en tablas.
- **Persistencia:** que un objeto **sobreviva a la ejecución del programa que lo creó** (pasa de la memoria **volátil** a la **persistente**).

## 4.2 Problema de impedancia ⭐

**Definición (respuesta del parcial):** *la dificultad de compatibilizar los modelos de datos de un sistema orientado a objetos con las estructuras tabulares de una base de datos relacional.*

Por qué pasa:
1. Las tablas solo manejan **tipos primitivos** (número, texto, fecha). Los objetos tienen tipos **complejos** creados por el usuario.
2. La información está **en objetos**, así que hay que **transformar la estructura de objetos a tablas**.
3. En una tabla se guardan **datos, no comportamiento** (no se guardan métodos).
4. Las tablas no aceptan **multivalores** (listas de objetos) ni **herencia** directa.
5. **Genera un fuerte acoplamiento entre la aplicación y el DBMS.** Por eso, **la menor cantidad posible de clases tiene que conocer la base de datos.**

**Objetos vs. tablas (salió en el parcial):** en los objetos, la relación se hace con **referencias directas en memoria** (un objeto "apunta" a otro). En las tablas se hace con **claves foráneas (FK)**.

**¿Cómo se reduce el problema? (respuesta del parcial)**
- Usando **frameworks ORM** (mapeo objeto-relacional), que **automatizan la conversión** objetos ⇄ tablas.
- **Definiendo interfaces abstractas entre la capa de dominio y la de persistencia** (aplicar **DIP**).
- ❌ **No** se reduce eliminando la capa de persistencia ni accediendo a la BD directo desde las clases de dominio (eso lo empeora).

## 4.3 Cómo pasar clases a tablas: 5 reglas (de memoria)
1. **Una tabla por clase.**
2. **Cada atributo primitivo → una columna.** Si el atributo es **complejo**, se hace **otra tabla** o se **reparte en varias columnas**.
3. **La clave primaria (PK) es el identificador único** del objeto (conviene que la genere el sistema).
4. **Cada objeto (instancia) → una fila.**
5. **Cada asociación con cardinalidad mayor que 1** (por ejemplo 0..N) → **una tabla nueva** que conecta las tablas de los objetos asociados.

## 4.4 Relaciones que hay que persistir
- **Asociación:** dos clases conectadas (`Gasto → TipoDeGasto`). Tiene multiplicidad y navegabilidad.
  - **Agregación:** "todo/parte" (`Factura ◇ DetalleFactura`).
  - **Composición:** "todo/parte" con **vidas unidas**: si muere el todo, mueren las partes (`Estantería ◆ Estante`).
- **Generalización (herencia):** `CuentaBancaria ← CuentaCorriente, CuentaDeAhorro`.

**Cómo se mapean las asociaciones:**

| Multiplicidad | Cómo queda en tablas |
|---|---|
| **1 a 1** | Una **FK** en una de las dos tablas (o una columna extra) |
| **1 a muchos** | **FK en la tabla del lado "muchos"** (cada `DetalleFactura` tiene la FK de su `Factura`) |
| **Muchos a muchos** | **Tabla intermedia** con las PK de las dos tablas |

## 4.5 Cómo se mapea la HERENCIA: 3 opciones ⭐

Ejemplo de clase: `Persona` (dni, apellido, nombre, fechaNac, sexo) ← `Conductor` y `AutoridadConstatacion` (legajo, nroPlaca).

| Opción | Cómo queda | 👍 | 👎 |
|---|---|---|---|
| **1. Simular la herencia** | **Una tabla para el padre y una para cada hijo.** Los hijos tienen **FK al padre** | **Menos redundancia**, es la más fiel a la herencia (la recomendada) | Hacen falta **JOINs**: consultas más complejas y lentas |
| **2. Eliminar el padre** | **Solo tablas de los hijos**, cada una **con los atributos del padre copiados** | Sin JOINs, tablas limpias | **Datos duplicados**; si cambia un atributo común, hay que tocar todas las tablas |
| **3. Eliminar los hijos** | **Una sola tabla** con **todos** los atributos de padre e hijos, más una columna **"tipo"** | Simple y rápida | **Muchas columnas en NULL** (un conductor no tiene nroPlaca) |

> 🧠 El nombre dice **qué tabla desaparece**: "eliminar el padre" = no hay tabla Persona; "eliminar los hijos" = solo queda la tabla Persona.

## 4.6 Modelos de persistencia: 2 ⭐⭐

### A) Súper Objeto Persistente
- **Idea:** todas las clases que se guardan **heredan** de una superclase (`ObjectToStore` / "SuperObjetoPersistente") que ya trae los métodos para guardarse y leerse. Como Superman: todos heredan sus poderes.
- Partes (ejemplo de Jacobson): `Object`, `ObjectState`, `PersistentState`, `PersistentObjectAdministrador` (convierte y guarda) y **`RDBPort`, el único que conoce la BD y habla SQL**.
- **Arquitectura:** GUI → Modelo de objetos de negocio (**la persistencia está ADENTRO de las clases de negocio**) → BD.
- ✅ **Fácil de implementar** y **no crece la estructura de clases**.
- ❌ **Viola SRP y SoC**: cada clase de negocio **mezcla lógica de negocio con acceso a datos** (respuesta del parcial). También **rompe OCP** y **aumenta el acoplamiento**.

### B) Esquema de Persistencia (correspondencia indirecta)
- **Idea:** la persistencia va en una **capa aparte**, entre el negocio y la BD. Las clases de negocio **no saben** cómo se guardan.
- **Arquitectura:** GUI → Modelo de negocio → **[Esquema de persistencia]** → BD.
- ✅ **Mantiene la cohesión** (el negocio solo hace negocio), **soporta extensibilidad** y tiene **mejor reusabilidad**.
- ✅ Respeta **OCP** y **DIP**: se pueden **agregar clases nuevas sin modificar las de dominio**, porque un framework de persistencia da una **capa de abstracción** (respuesta del parcial).
- ❌ **Más clases.**

| | **Súper Objeto Persistente** | **Esquema de Persistencia** |
|---|---|---|
| ¿Dónde está la persistencia? | **Adentro** de las clases de negocio (por **herencia**) | En una **capa aparte** |
| Ventajas | Fácil de implementar; no crece la estructura | Cohesión, extensibilidad, reusabilidad |
| Principios | **Viola SRP/SoC**, rompe OCP | **Respeta SRP, OCP y DIP** |
| ¿Cuál gana? | | **El Esquema** (la balanza de la diapositiva se inclina hacia acá) |

## 4.7 Bases de datos orientadas a objetos (BDOO / ODBMS)
**Características (8):** objetos complejos, identidad de objetos (independiente de sus valores), encapsulamiento, tipos o clases, jerarquía (herencia), ligadura tardía (polimorfismo), completitud y extensibilidad (agregar tipos nuevos).

**Beneficios:** los objetos **se guardan tal cual**, **no hay que convertir tipos** y el lenguaje de la BD puede ser **el mismo** que el de la aplicación. **Eliminan el problema de impedancia.**

## 4.8 Frameworks y framework de persistencia ⭐
**Framework:**
- Es un **conjunto extensible de clases e interfaces que colaboran** para dar los servicios de la **parte central e invariable** de un subsistema.
- Tiene clases **concretas y sobre todo abstractas**; muchas veces hay que **crear subclases** de las suyas para usarlo.
- Se basa en el **principio de Hollywood: "no nos llame, nosotros lo llamaremos"**: **el framework llama a tu código**, no al revés.
- Da **mucho más reuso** que clases sueltas.

> ⚠️ **Framework ≠ librería.** A una **librería la llamás vos** cuando querés. Un **framework controla el flujo y te llama a vos** (Hollywood). En el parcial, la opción "son librerías que el programador invoca directamente" es **falsa**.

**Un framework de persistencia tiene que dar, como mínimo:**
1. **Guardar y recuperar objetos** (almacenar / recuperar).
2. **Confirmar y deshacer transacciones (commit / rollback).**

**Vocabulario útil:** **materializar** = convertir una **fila → objeto**; **desmaterializar** = convertir un **objeto → fila**.

> 🎯 **Lo que preguntan:** qué es la impedancia y cómo se reduce (ORM + interfaces/DIP); diferencia de relaciones objetos/tablas (referencias vs. FK); Súper Objeto viola SRP; Esquema respeta OCP; qué da un framework de persistencia (commit/rollback y guardar/recuperar); frameworks = Hollywood.

---

# 5. EVOLUCIÓN DEL SOFTWARE

### En criollo
> El software **no se termina cuando se entrega**: **tiene que cambiar todo el tiempo** para seguir siendo útil. Esos cambios se hacen de dos formas: con **mantenimiento** (los arreglos y mejoras de todos los días) o con **reingeniería** (rehacer por dentro un sistema viejo que vale mucho pero está hecho un desastre). El **refactoring** es ordenar el código de a poco, **sin cambiar lo que hace**.

## 5.1 Contexto
- Antes se decía "**mantenimiento** del software"; ahora se dice "**evolución** del software".
- **Metáfora del iceberg:** se esperaba que el mantenimiento fuera solo la punta visible, pero abajo hay una **masa enorme de problemas y costos**. "En los 70 hundía un portaaviones; hoy hundiría a toda la armada".

**¿Por qué hace falta tanto mantenimiento?**
- Mucho del software que usamos tiene **20 años o más**.
- Se hizo cuando lo que importaba era el **tamaño del programa y el espacio de almacenamiento**.
- Se **migró a nuevas plataformas**, hardware y sistemas operativos.
- Se le fueron **agregando mejoras** para los usuarios.
- **¡Y todo sin pensar en la arquitectura!**

**Razones:** **arquitecturas mal diseñadas**, **mala codificación**, **lógica inadecuada** y **documentación escasa o inexistente**.

## 5.2 Ciclo de vida y proceso de evolución
- **Desarrollo y evolución son un continuo** (modelo espiral): especificación → implementación → validación → operación → release 1, 2, 3…
- **Ciclo de vida:** **Desarrollo inicial → Evolución → Servicio → Retiro gradual.**
  - En **evolución** se hacen cambios grandes; en **servicio**, solo arreglos chicos; en **retiro gradual**, se usa pero ya no se cambia.
- **Proceso de evolución:** **peticiones de cambio → análisis de impacto → planificación de versiones → implementación de cambios → liberación del sistema.** La planificación separa en reparación de fallas, adaptación de la plataforma y perfeccionamiento del sistema.
- **Dos procesos de cambio:**
  - **Normal:** peticiones de cambio → **análisis de requerimientos → actualización de requerimientos** → desarrollo.
  - **Reparación de emergencia:** peticiones de cambio → **análisis del código fuente → modificación del código** → entrega. ⚠️ **Se saltea los requerimientos y el diseño**: va **directo al código** (por eso degrada el sistema y desactualiza la documentación).

## 5.3 Estrategias de evolución
Son 2: **MANTENIMIENTO** y **REINGENIERÍA**. (Al evaluar sistemas viejos también aparece la **sustitución**).

## 5.4 Tipos de mantenimiento ⭐⭐

**De dónde salen los cambios:** **correcciones** (modifican requerimientos existentes), **mejoras** (crean requerimientos nuevos) y cambios que **modifican la instalación sin cambiar los requerimientos**.

| Tipo (nombre de la cátedra) | Nombre "clásico" | Qué es | % del esfuerzo |
|---|---|---|---|
| **Reparación de fallas** | Correctivo | **Arreglar errores** (bugs). Los de código son baratos; los de diseño, más caros; los de requerimientos, **los más caros** | **17%** |
| **Adaptación ambiental** | Adaptativo | Adaptar el software a un **cambio del entorno**: hardware, sistema operativo, plataforma | **18%** |
| **Incorporación y modificación de funcionalidad** (adición de funcionalidad) | Perfectivo | Cambios porque **cambian los requerimientos o el negocio**; **mejoras para los usuarios** | **65%** ⭐ |

> 🧠 **Para recordar:** **la mayor parte del mantenimiento (65%) NO es arreglar bugs**, es **agregar o cambiar funcionalidad**. Los bugs son solo el 17%. Orden: **17 – 18 – 65**.

- El **mantenimiento preventivo** es el **refactoring**: mejora la estructura **sin agregar funcionalidad** para **prevenir** problemas futuros.

**¿Por qué el mantenimiento es una parte tan grande del ciclo de vida? (respuesta del parcial)** Porque los sistemas **tienen que adaptarse a cambios de plataforma y entorno** **y** porque **aparecen nuevas necesidades de los usuarios** que obligan a mejorar la funcionalidad.

## 5.5 Costos de mantenimiento
- **Gráfico Sistema 1 vs. Sistema 2:** el Sistema 1 gastó **un poco más en desarrollo** para hacerlo mantenible y **mucho menos en mantenimiento**, así que **en total le salió más barato**. Moraleja: **invertir en mantenibilidad desde el principio conviene.**

**Factores de costo de mantenimiento (4):**
1. **Estabilidad del equipo:** cuando se entrega, el equipo se disuelve, y los nuevos tienen que aprender todo desde cero.
2. **Prácticas de desarrollo deficientes:** el que desarrolla no tiene incentivo para hacerlo mantenible (muchas veces el mantenimiento lo hace otra empresa).
3. **Habilidades del personal:** al mantenimiento suelen ir los **menos experimentados**, y los sistemas viejos usan lenguajes obsoletos.
4. **Antigüedad y estructura del programa:** cuanto más viejo y más cambiado, **más degradada** está la estructura.

**Motivación del personal (cómo mejorarlo):** relacionar el software con los objetivos de la organización, relacionar las recompensas con el rendimiento, **integrar mantenimiento con desarrollo**, tener **presupuesto para mantenimiento preventivo**, **planificar el mantenimiento desde el principio** y planificar esfuerzo en mantenibilidad.

## 5.6 Predicción
- **Predicción de cambios:** hay que entender la **relación entre el sistema y su entorno**. Los sistemas **muy acoplados** al entorno cambian **cada vez que cambia el entorno**. Factores:
  1. **Número y complejidad de las interfaces** del sistema.
  2. **Número de requerimientos volátiles** (los que cambian seguido).
  3. **Los procesos de negocio** que usan el sistema.
- **Predicción del mantenimiento:** ver **qué partes van a dar problemas y costar caro**. Los cambios **degradan** el sistema; el costo de mantenimiento depende de **cuántos cambios** hay, y el costo de cada cambio depende de la **mantenibilidad**.

| Predicción | Pregunta que responde |
|---|---|
| **Mantenibilidad** | ¿Qué partes serán más afectadas por cambios? ¿Cuáles serán más caras de mantener? |
| **Cambios en el sistema** | ¿Cuántos pedidos de cambio se esperan? |
| **Costos de mantenimiento** | ¿Cuánto costará el año que viene? ¿Y en todo el ciclo de vida? |

## 5.7 Leyes de Lehman ⭐ (8 leyes de cómo evolucionan los sistemas grandes)

| # | Ley | En criollo |
|---|---|---|
| 1 | **Cambio continuo** | Un programa que se usa en el mundo real **tiene que cambiar o se vuelve cada vez menos útil** |
| 2 | **Incremento de complejidad** | Cada cambio **lo desordena más**. Hay que **gastar recursos extra** para mantenerlo ordenado (refactoring) |
| 3 | **Evolución de grandes programas** | Es **autorregulada**: tamaño, tiempo entre versiones y cantidad de errores son **más o menos constantes** en cada versión |
| 4 | **Estabilidad organizacional** | El **ritmo de evolución es constante** y **no depende de cuántos recursos pongas** (más gente no lo acelera) |
| 5 | **Conservación de la familiaridad** | El **cambio en cada versión es más o menos el mismo** |
| 6 | **Crecimiento continuo** | La funcionalidad **tiene que crecer siempre** para que el usuario siga conforme |
| 7 | **Disminución de la calidad** | La calidad **baja** si no se adapta a los cambios del entorno |
| 8 | **Retroalimentación** | La evolución es un sistema de **retroalimentación multiagente y multiciclo** |

**¿Dónde se aplican?** No está del todo comprobado. En general sirven para **sistemas grandes, hechos a medida, para organizaciones grandes**. **No está claro** para software enlatado, sistemas con muchos componentes comerciales (COTS), organizaciones chicas ni sistemas medianos.

## 5.8 Valoración de un sistema heredado (legacy) ⭐⭐

**Sistema heredado:** un sistema **viejo** que **todavía es útil** (a veces crítico), pero con tecnología obsoleta, estructura degradada y poca documentación.

**Matriz Valor de negocio × Calidad del sistema:**

```
                    CALIDAD BAJA          CALIDAD ALTA
VALOR ALTO   │   REINGENIERÍA      │   MANTENIMIENTO
VALOR BAJO   │   SUSTITUCIÓN       │   MANTENIMIENTO (eventualmente sustitución si suben los costos)
```

> 🧠 **Trucos:**
> - **Calidad alta → mantenimiento** (siempre).
> - **Valor alto + calidad baja → reingeniería** (vale mucho, pero está mal hecho: se arregla por dentro).
> - **Valor bajo + calidad baja → sustitución** (no vale la pena: se reemplaza).
> - Respuesta del parcial: "Alto valor/Baja calidad y Bajo valor/Baja calidad" = **Reingeniería / Sustitución**.

## 5.9 Reingeniería ⭐⭐

### En criollo
> Agarrar un sistema **viejo** que **vale mucho** pero está **mal hecho por dentro**, **entenderlo** y **reestructurarlo** para que sea **más fácil de mantener**, **sin cambiar lo que hace** (la funcionalidad).

**Fórmula de la diapositiva:**
> **Ingeniería reversa + Δ (cambios) + Ingeniería delantera = REINGENIERÍA**

| Parte | Qué es |
|---|---|
| **Ingeniería reversa** | **NO es un proceso de cambio**: solo **analiza** el programa y **saca información** (documentación, diseño) |
| **Δ (delta)** | Las **alteraciones** al sistema original |
| **Ingeniería delantera** | El **proceso tradicional**: de la especificación al diseño y al código |

```
Ingeniería delantera:  Especificación ──► Diseño e implementación ──► Sistema nuevo
Reingeniería:          Sistema existente ──► Comprensión y transformación ──► Sistema reingenierizado
```

**Tópicos (actividades) de la reingeniería (5):**
1. **Traducción de código fuente:** pasar de un lenguaje viejo a uno nuevo (o a una versión nueva).
2. **Ingeniería reversa:** analizar y **documentar** el programa.
3. **Mejora de la estructura del programa:** reordenar la estructura de control para que se lea mejor.
4. **Modularización del programa:** **agrupar** las partes relacionadas y **eliminar redundancia**.
5. **Reingeniería de datos:** cambiar los **datos** (esquemas, limpieza) para que acompañen los cambios del programa.

**Ventajas (2) frente a hacer uno nuevo desde cero:**
- **Reduce riesgos:** hacer software nuevo tiene **riesgos altos** (de desarrollo, de personal, de especificación).
- **Reduce costos:** la reingeniería **cuesta bastante menos** que desarrollar de cero.

> ⚠️ Es **falso** que la reingeniería sea "más costosa y riesgosa que hacer uno nuevo" y que "no requiera entender el sistema". **Sí** requiere entenderlo (por eso tiene ingeniería reversa).

**Factores de costo de la reingeniería (4):** **calidad** del software a reingenierizar, **herramientas** de soporte disponibles, **cuántos datos** hay que convertir y **personal experto** disponible.

**Factores de complejidad:** complejidad de **control** (se mide por los `if`), complejidad de **datos**, **tamaño de los módulos**, **largo de los identificadores** (nombres largos = más legible) y **comentarios** (más comentarios = tal vez más fácil de mantener).

**Respuesta del parcial:** la reingeniería **permite mejorar la calidad estructural sin cambiar los requerimientos funcionales** **e implica ingeniería inversa y reestructuración del programa existente**.

## 5.10 Refactoring ⭐⭐

### En criollo
> **Ordenar el código por dentro sin cambiar lo que hace por fuera.** Como ordenar tu pieza: después de ordenar, sigue teniendo las mismas cosas, pero ahora encontrás todo. Si las pruebas pasaban antes, **tienen que seguir pasando después**.

**Qué es:**
- Una **forma disciplinada de introducir cambios reduciendo la posibilidad de meter errores**.
- Una transformación que **preserva el comportamiento externo** y **mejora la estructura interna**.

**Son cambios que:**
- **No cambian el comportamiento observable** (todas las pruebas siguen pasando).
- **Eliminan la duplicación** y la **complejidad innecesaria**.
- **Mejoran la calidad.**
- Hacen el código **más simple, más fácil de entender** y **más fácil de cambiar**.

**¿Por qué hacerlo?** Para **evitar el deterioro del diseño** (ley 2 de Lehman), limpiar el desorden, simplificar, hacer el código más legible, **encontrar errores**, **reducir el tiempo de depuración**, incorporar lo que aprendemos de la aplicación y porque **rehacer es parte de todo proceso creativo**.

**¿Cómo se hace? Ciclo de 5 pasos:**
1. **Encontrar código que "huela mal"** (duplicado, métodos larguísimos, `switch` enormes, código "por si acaso").
2. **Asegurarse de que las pruebas pasen.**
3. **Decidir cómo simplificarlo.**
4. **Hacer las simplificaciones.**
5. **Volver a correr las pruebas** para confirmar que siguen pasando. (Y volver al paso 1).

**Refactoring en contexto:**
- **Refactoring y TDD** (desarrollo guiado por pruebas): primero se escribe la prueba, después el código, y después se refactoriza. **Las pruebas son la red de seguridad** que permite refactorizar sin miedo.
- **Refactoring = mantenimiento preventivo:** **no agrega funcionalidad**; mejora la estructura para que **los cambios futuros sean más fáciles**. También **reduce la deuda técnica**.

**Respuesta del parcial:** todas estas son correctas: busca mejorar la estructura interna sin cambiar el comportamiento observable, suele aplicarse como mantenimiento preventivo, reduce la deuda técnica y se relaciona con TDD.

## 5.11 ⭐⭐ Reingeniería vs. Refactoring (pregunta clásica)

| | **Reingeniería** | **Refactoring** |
|---|---|---|
| ¿Cuándo? | **Puntualmente**, cuando el sistema ya está degradado y mantenerlo cuesta mucho | **Continuamente**, todo el tiempo |
| ¿Sobre qué? | **Sistemas heredados** completos | El **código actual**, de a pedacitos |
| Alcance | **Grande**: puede tocar **lenguaje, datos y arquitectura** | **Chico** e incremental |
| ¿Cómo? | Con **herramientas automáticas** (traducción, ingeniería reversa) | **Cambios pequeños** guiados por **pruebas** |
| Objetivo | **Generar una nueva versión más mantenible** del sistema | **Evitar** que la estructura se degrade (preventivo) |
| ¿Cambia la funcionalidad? | **No** | **No** |

> 🧠 **Frase para el parcial:** *el refactoring mejora la estructura interna y la calidad del código sin alterar su comportamiento observable; la reingeniería implica cambios más amplios en la arquitectura o la tecnología del sistema (una reestructuración profunda).*

> ❌ **Trampas:** "el refactoring es adaptación ambiental", "el refactoring implica ingeniería inversa y delantera" (eso es reingeniería), "tienen el mismo alcance", "la reingeniería actúa sobre porciones pequeñas" (es al revés) y "el refactoring busca mejorar la eficiencia de ejecución" (busca mejorar la **estructura**, no la velocidad).

> 🎯 **Lo que preguntan:** qué tipo de mantenimiento consume más esfuerzo (incorporación y modificación de funcionalidad, 65%); matriz de sistemas heredados; qué es reingeniería; refactoring vs. reingeniería; qué es refactoring.

---

# 6. EL PARCIAL ANTERIOR (06/11/2025): 40 preguntas con la respuesta correcta y el porqué

> Son las respuestas **según la corrección del profesor** (de la grilla corregida). Las preguntas están resumidas.

| #   | Tema           | Pregunta (resumida)                                                           | ✅ Respuesta                                                                                                                                                                                                    | Por qué                                                                                                                                                                    |
| --- | -------------- | ----------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Principios     | Propósito principal de la **Ley de Demeter**                                  | **b)** Producir objetos **débilmente acoplados**                                                                                                                                                               | Es el propósito textual de la diapositiva. Los wrappers son su *desventaja*, no su propósito                                                                               |
| 2   | Principios     | Incluir funciones "en reserva" que hoy no sirven, ¿qué viola?                 | **c)** **YAGNI**                                                                                                                                                                                               | "No lo vas a necesitar"                                                                                                                                                    |
| 3   | Principios     | Composición sobre herencia, ¿qué acoplamiento promueve?                       | **c)** **Débil**, con reuso a nivel de **objetos**                                                                                                                                                             | Caja negra / delegación                                                                                                                                                    |
| 4   | Patrones       | `OrdenDePago` llena de `if` según el tipo de pago                             | **e)** **a) y d)**: aplicar **Strategy** y aplicar **OCP**                                                                                                                                                     | Cada forma de pago es una estrategia; se extiende sin modificar                                                                                                            |
| 5   | State          | ¿Cuándo usar herencia en la jerarquía de estados?                             | **a)** Cuando **varios estados comparten comportamiento** y se puede poner en una **clase abstracta común**                                                                                                    | Evita duplicar (DRY)                                                                                                                                                       |
| 6   | Observer       | ¿Cómo reduce el acoplamiento Sujeto–Observadores?                             | **c)** **Acoplamiento abstracto y mínimo**: el Sujeto conoce a los observadores **solo a través de la interfaz**                                                                                               | No conoce las clases concretas. El GestorDeCambios no es obligatorio                                                                                                       |
| 7   | Adapter        | Principal ventaja del **Adapter de objetos** en responsabilidades             | **a)** **Separa la lógica de adaptación** (conversión) **de la lógica de negocio**                                                                                                                             | Es SRP. No hereda (eso es el de clase) y no obliga a modificar al cliente                                                                                                  |
| 8   | Iterator       | Al delegar el recorrido a un IteradorConcreto, ¿qué principio respeta?        | **a)** **SRP**                                                                                                                                                                                                 | Recorrer pasa a ser responsabilidad de otra clase                                                                                                                          |
| 9   | State/Strategy | Diferencia de **intención** entre Strategy y State                            | ⚠️ La grilla dice **a)**, pero según la teoría la correcta es **b)**: *Strategy configura al contexto con un algoritmo intercambiable; State cambia el comportamiento automáticamente según el estado interno* | La opción a) tiene **invertidas** las definiciones. Probablemente sea un error de la grilla: si te toca, respondé con la teoría (**b**) y, si te la marcan mal, consultalo |
| 10  | Persistencia   | Impacto del **Súper Objeto Persistente** sobre SRP                            | **b)** **Tiende a violar SRP**, porque mezcla negocio con acceso a datos                                                                                                                                       | La persistencia se hereda y queda adentro de cada clase de negocio                                                                                                         |
| 11  | Persistencia   | ¿Cómo favorece OCP el **Esquema de Persistencia**?                            | **c)** Usa un **framework de persistencia** como capa de abstracción, para **agregar clases sin modificar las de dominio**                                                                                     |                                                                                                                                                                            |
| 12  | DIP            | Cambiar el origen de datos (archivo → API) sin tocar las clases de alto nivel | **b)** **Definir una interfaz abstracta** del origen de datos; el alto nivel depende de **la abstracción**                                                                                                     | Esto es DIP                                                                                                                                                                |
| 13  | Persistencia   | Un framework de persistencia debe proveer como mínimo…                        | **b)** **Commit/rollback** y **almacenar/recuperar objetos**                                                                                                                                                   | Son las 2 funciones de la diapositiva                                                                                                                                      |
| 14  | Persistencia   | Afirmación correcta sobre frameworks de persistencia                          | **b)** Aplican el **principio de Hollywood**: el framework controla y **llama** a tu código                                                                                                                    | "Son librerías que invocás directamente" es **falso**: eso es una librería                                                                                                 |
| 15  | Principios     | Aplicación correcta de "programar hacia la interfaz"                          | **a)** **Depender de interfaces o abstracciones**, para que los cambios en las implementaciones no afecten al cliente                                                                                          |                                                                                                                                                                            |
| 16  | Patrones       | **Consecuencia fundamental** de usar patrones                                 | **b)** **Aislar a los clientes de las clases concretas**, lo que da flexibilidad, extensibilidad y portabilidad                                                                                                |                                                                                                                                                                            |
| 17  | Evolución      | Diferencia fundamental Refactoring vs. Reingeniería                           | **c)** Refactoring = mejora estructura y calidad **sin alterar el comportamiento**; Reingeniería = **reestructuración profunda** que genera una nueva especificación                                           |                                                                                                                                                                            |
| 18  | Evolución      | Matriz: alto valor/baja calidad y bajo valor/baja calidad                     | **c)** **Reingeniería / Sustitución**                                                                                                                                                                          |                                                                                                                                                                            |
| 19  | Evolución      | Mantenimiento que incorpora mejoras y consume **más esfuerzo**                | **c)** **Incorporación y modificación de la funcionalidad** (65%)                                                                                                                                              |                                                                                                                                                                            |
| 20  | Patrones       | ¿A qué contribuyen los patrones?                                              | **a)** A **estandarizar soluciones probadas** (reuso, mantenimiento, desacoplamiento)                                                                                                                          | No reemplazan a SOLID ni garantizan persistencia                                                                                                                           |
| 21  | Evolución      | ¿Por qué el mantenimiento es una parte tan grande del ciclo de vida?          | **d)** **a) y b)**: adaptarse a **cambios de plataforma/entorno** y **nuevas necesidades de los usuarios**                                                                                                     | Ojo: la grilla **no** incluye la c) (arquitecturas deficientes y poca documentación)                                                                                       |
| 22  | Evolución      | La reingeniería…                                                              | **e)** **a) y c)**: mejora la calidad estructural **sin cambiar los requerimientos funcionales** e **implica ingeniería inversa y reestructuración**                                                           | No es más cara que hacerlo nuevo y sí requiere entender el sistema                                                                                                         |
| 23  | Evolución      | Enunciados correctos sobre refactoring                                        | **f)** **Todos**: mejora la estructura sin cambiar el comportamiento, es mantenimiento preventivo, reduce la deuda técnica y se relaciona con TDD                                                              |                                                                                                                                                                            |
| 24  | Persistencia   | Diferencia entre relación de objetos y relación de tablas                     | **a)** Objetos: **referencias directas en memoria**; tablas: **claves foráneas**                                                                                                                               |                                                                                                                                                                            |
| 25  | Diseño         | Diagramas UML 2.0 que modelan **estructura**                                  | **e)** **b) y c)**: **despliegue** y **clases**                                                                                                                                                                | El de **secuencia es de comportamiento**                                                                                                                                   |
| 26  | DIP            | ¿Cómo se relacionan los módulos según DIP?                                    | **b)** Alto y bajo nivel **dependen de abstracciones**                                                                                                                                                         |                                                                                                                                                                            |
| 27  | Persistencia   | ¿Qué reduce el problema de impedancia?                                        | **e)** **a) y c)**: usar **ORM** y **definir interfaces abstractas** entre dominio y persistencia (DIP)                                                                                                        | Eliminar la capa de persistencia lo empeora                                                                                                                                |
| 28  | Persistencia   | ¿Qué es el problema de impedancia?                                            | **a)** La **dificultad de compatibilizar** el modelo de objetos con las **tablas** de una BD relacional                                                                                                        | No es "la dependencia de los tipos del motor de BD"                                                                                                                        |
| 29  | ISP            | ¿Qué propone Segregación de Interfaces?                                       | **f)** **c) y d)**: no obligar a depender de métodos que no se usan y **dividir las interfaces** en partes chicas                                                                                              |                                                                                                                                                                            |
| 30  | Principios     | ¿Qué es **delegación**?                                                       | **a)** Una clase **cede parte de su responsabilidad a otro objeto**, pidiéndole que haga una operación                                                                                                         | Heredar para reusar **no** es delegar                                                                                                                                      |
| 31  | Diseño         | Ejemplos de **requerimientos no funcionales**                                 | **g)** **b) y c)**: **disponibilidad del 99%** y **respuesta en menos de 2 segundos**                                                                                                                          | Registrar cuentas o editar datos son funcionales                                                                                                                           |
| 32  | Adapter        | Código repetido para adaptar varias APIs: ¿qué viola y cómo ayuda el Adapter? | **a)** Viola **DRY**; el Adapter **centraliza la conversión** en una sola clase                                                                                                                                | La d) es falsa: el Adapter **no impide** extender                                                                                                                          |
| 33  | Strategy       | Relación de Strategy con "programar hacia la interfaz"                        | **a)** Las clases **dependen de abstracciones**, así se puede **cambiar el algoritmo en ejecución** sin modificar al cliente                                                                                   |                                                                                                                                                                            |
| 34  | Patrones       | Asociación correcta patrón ↔ principio                                        | **f)** **a) y d)**: **Strategy favorece OCP** y **Adapter favorece DIP**                                                                                                                                       | Singleton **no** favorece SRP; Observer **no** viola DIP                                                                                                                   |
| 35  | Observer       | Principios que se cumplen al aplicar Observer                                 | **e)** **a), b) y c)**: **DIP**, **OCP** y **SRP**                                                                                                                                                             | **No** es LSP: los observadores no heredan del sujeto                                                                                                                      |
| 36  | Principios     | ¿Qué representa "composición sobre herencia"?                                 | **e)** **a) y c)**: **combinar objetos mediante referencias** en vez de jerarquías rígidas y **reusar comportamiento sin modificar las clases existentes**                                                     |                                                                                                                                                                            |
| 37  | Diseño         | Diferencia entre Análisis y Diseño en el PUD                                  | **c)** Análisis: **descripción del problema, modelo lógico**. Diseño: **descripción de la solución, plano para la implementación**                                                                             |                                                                                                                                                                            |
| 38  | Patrones       | ¿Cómo contribuyen los patrones a la calidad?                                  | **e)** **a) y c)**: **aíslan los cambios** (extensibilidad, reuso, flexibilidad) y dan un **lenguaje común**                                                                                                   | No eliminan las interfaces ni crean dependencias fuertes                                                                                                                   |
| 39  | Evolución      | Diferencia de **alcance** entre reingeniería y refactoring                    | **a)** Refactoring: estructura interna sin cambiar el comportamiento; reingeniería: **cambios más amplios** (arquitectura o tecnología)                                                                        |                                                                                                                                                                            |
| 40  | LSP            | Ejemplo correcto de **Liskov**                                                | **a)** Las **subclases pueden reemplazar a sus superclases** sin alterar el comportamiento esperado                                                                                                            | Una hija **no puede** violar las pre/postcondiciones del padre                                                                                                             |

### Patrones que se repiten en las preguntas
- Casi la mitad del parcial es **"¿qué principio cumple/viola X?"**. Repasá la tabla **3.10** y la sección de SOLID.
- **Muchas respuestas correctas son combinadas** ("a) y c)", "a), b) y c)"). Antes de marcar una sola letra, fijate si hay otra opción que también sea verdadera.
- Las opciones falsas suelen ser **absolutos** ("elimina la necesidad de…", "garantiza…", "obliga a…", "se aplica únicamente…") o **definiciones invertidas** (decir de State lo que es de Strategy, o de refactoring lo que es de reingeniería).

---

# 7. REPASO RELÁMPAGO (leelo antes de entrar)

**Diseño**
- Análisis = **problema**, modelo **lógico**, **bosquejo**. Diseño = **solución**, modelo **físico**, **manifiesto**.
- 6 aspectos: **Arquitectura** (centro; resuelve los **no funcionales**), Datos, Procesos (con **patrones**), UX, Formas de E/S (entrada solo en **batch**), Procedimientos manuales.
- UML de **estructura**: clases, paquetes, componentes, **despliegue**, objetos. **Secuencia = comportamiento.**
- No funcional = calidad con número (99%, 2 segundos), seguridad, rendimiento.

**Principios**
- Todo apunta a **alta cohesión + bajo acoplamiento**.
- DRY (no repetir) · SoC (separar en capas) · **YAGNI** (no "por si acaso") · KISS (simple) · **Demeter** (solo con amigos → **débilmente acoplados**) · Tell don't ask · **Programar a la interfaz** · **Composición > herencia** (caja negra, delegación, acoplamiento débil, reuso a nivel de objetos).
- **S**: una razón para cambiar · **O**: extender sin modificar · **L**: la hija reemplaza al padre · **I**: interfaces chicas · **D**: todos dependen de abstracciones.

**Patrones evaluables**

| Patrón | Tipo | En una frase | Principios |
|---|---|---|---|
| **State** | Comportamiento | El objeto **cambia de comportamiento según su estado**; cada estado es una clase | OCP, SRP, DIP |
| **Strategy** | Comportamiento | **Algoritmos intercambiables** con la misma interfaz | **OCP**, programar a la interfaz |
| **Observer** | Comportamiento | **1 a muchos**: el sujeto avisa a los suscriptos (pull/push) | **DIP, OCP, SRP** |
| **Adapter** | **Estructura** | **Traduce** una interfaz incompatible (objeto = composición; clase = herencia múltiple) | **SRP**, OCP, DIP, DRY |
| **Iterator** | Comportamiento | **Recorre** sin mostrar cómo está guardado; el agregado crea el iterador | **SRP** |

- **State y Strategy: mismo diagrama, distinta intención.**

**Persistencia**
- **Impedancia** = objetos ≠ tablas → acopla la app a la BD. Se reduce con **ORM + interfaces (DIP)**.
- 5 reglas: tabla por clase · atributo → columna · PK = identificador · objeto → fila · asociación > 1 → tabla nueva.
- 1 a N → **FK del lado N**. N a N → **tabla intermedia**.
- Herencia: **simular** (padre + hijos con FK) · **eliminar el padre** (solo hijos) · **eliminar los hijos** (una tabla con "tipo").
- **Súper Objeto** (hereda la persistencia; fácil, pero **viola SRP**) vs. **Esquema** (capa aparte; **cohesión, extensibilidad, reuso, OCP**).
- Framework de persistencia: **guardar/recuperar + commit/rollback**. Frameworks = **Hollywood** ("no nos llame, nosotros lo llamamos").
- Las BDOO **eliminan** la impedancia.

**Evolución**
- Mantenimiento: **fallas 17% · adaptación ambiental 18% · funcionalidad 65%** (la mayor).
- 4 factores de costo: **estabilidad del equipo, malas prácticas, habilidades del personal, antigüedad y estructura**.
- Lehman: cambio continuo, más complejidad, autorregulación, estabilidad organizacional, familiaridad, crecimiento continuo, baja de calidad, retroalimentación.
- Matriz: **calidad alta → mantenimiento**; **valor alto + calidad baja → reingeniería**; **valor bajo + calidad baja → sustitución**.
- **Reingeniería = ingeniería reversa + Δ + ingeniería delantera.** Tópicos: traducción, ingeniería reversa, mejora de estructura, modularización, reingeniería de datos. Ventajas: **menos riesgo y menos costo**.
- **Refactoring** = mejorar la estructura interna **sin cambiar el comportamiento** (las pruebas siguen pasando). **Mantenimiento preventivo**, va con **TDD** y reduce la deuda técnica.
- Reingeniería = **grande y puntual** (sistemas heredados). Refactoring = **chico y continuo**.

¡Éxitos! 💪

---

# 8. PREGUNTAS A DESARROLLAR: las más probables, con respuesta modelo

> **Cómo responder cualquier pregunta a desarrollar (fórmula):**
> 1. **Definí** el concepto en 1 o 2 oraciones.
> 2. **Explicá cómo funciona** o en qué partes se divide.
> 3. **Compará o mostrá ventajas y desventajas.**
> 4. **Poné un ejemplo corto.**
> 5. **Cerrá** relacionándolo con un principio o con otro tema (SRP, OCP, Lehman, mantenimiento…). Eso suma mucho.
>
> No hace falta escribir una novela: con 10 a 15 renglones bien ordenados alcanza.

Están ordenadas de **más probable a menos probable**. Las de Evolución son las candidatas fuertes porque son las más "teóricas" y en el multiple choice entran poco.

---

### 8.1 ⭐⭐⭐ Diferencias entre reingeniería y refactoring

**Respuesta modelo:**
El **refactoring** es una transformación del código que **mejora su estructura interna sin cambiar su comportamiento observable**. Se hace **de forma continua**, con **cambios pequeños** guiados por **pruebas** (se corren antes y después de cada cambio; se relaciona con TDD). Es **mantenimiento preventivo**: no agrega funcionalidad, pero evita que el diseño se degrade (2ª ley de Lehman) y reduce la deuda técnica.

La **reingeniería** también **mantiene la funcionalidad**, pero es una **reestructuración profunda** de un **sistema heredado** completo para generar una **versión más mantenible**. Se hace **puntualmente**, cuando el sistema ya está degradado. Se resume en: **ingeniería reversa + Δ (cambios) + ingeniería delantera**. Puede incluir traducción del código fuente, ingeniería reversa, mejora de la estructura, modularización y reingeniería de datos, y suele usar herramientas automáticas.

**En común:** ninguna de las dos cambia lo que el sistema hace; las dos buscan mejorar la mantenibilidad.
**Diferencia clave:** **alcance** (pedacitos de código vs. sistema entero, incluso lenguaje, datos y arquitectura) y **frecuencia** (continuo vs. puntual).

---

### 8.2 ⭐⭐⭐ ¿Qué es la reingeniería? Proceso, actividades y ventajas

**Respuesta modelo:**
Es **reestructurar o reescribir parte o todo un sistema heredado sin cambiar su funcionalidad**, para que sea más fácil de mantener. Se aplica cuando el sistema tiene **alto valor de negocio pero baja calidad**.
**Fórmula:** ingeniería reversa (analizar y documentar el sistema; no cambia nada) + Δ (las alteraciones) + ingeniería delantera (de la especificación al diseño y al código).
**Actividades (5):** 1) traducción del código fuente, 2) ingeniería reversa, 3) mejora de la estructura del programa, 4) modularización, 5) reingeniería de datos.
**Ventajas frente a hacer un sistema nuevo:** **menor riesgo** (desarrollar de cero tiene riesgos de especificación, personal, etc.) y **menor costo**.
**Factores de costo:** calidad del software original, herramientas disponibles, volumen de datos a convertir y personal experto disponible.

---

### 8.3 ⭐⭐⭐ Tipos de mantenimiento (con ejemplos y porcentajes)

**Respuesta modelo:**
El mantenimiento es el proceso de **cambiar el sistema después de entregado**. Hay 3 tipos:
- **Reparación de fallas (correctivo), ~17%:** corregir errores. Los de código son baratos; los de diseño, más caros; los de requerimientos, los más caros. *Ej.: un cálculo de IVA que da mal.*
- **Adaptación ambiental (adaptativo), ~18%:** adaptar el software a un cambio del entorno (hardware, sistema operativo, plataforma) sin cambiar la funcionalidad. *Ej.: migrar a una versión nueva de la base de datos.*
- **Incorporación y modificación de funcionalidad (perfectivo), ~65%:** cambios porque cambian los requerimientos o el negocio. *Ej.: agregar pago con QR.*
- (Extra) **Preventivo = refactoring:** mejorar la estructura para facilitar cambios futuros.

**Conclusión:** la mayor parte del esfuerzo **no es arreglar bugs** sino **agregar y modificar funcionalidad**. Por eso conviene **invertir en mantenibilidad desde el desarrollo** (el gráfico del Sistema 1 vs. el Sistema 2: más caro al principio, más barato en total).

---

### 8.4 ⭐⭐ Refactoring: qué es, por qué y cómo se hace

**Respuesta modelo:**
**Qué es:** una forma **disciplinada** de cambiar el código que **preserva el comportamiento externo y mejora la estructura interna**: elimina duplicación y complejidad innecesaria y deja el código más simple y fácil de cambiar.
**Por qué:** evita el deterioro del diseño (Lehman: cada cambio aumenta la complejidad), hace el código más legible, ayuda a encontrar errores, reduce el tiempo de depuración y la deuda técnica.
**Cómo (ciclo):** 1) encontrar código que "huele mal" → 2) asegurarse de que las pruebas pasen → 3) decidir cómo simplificar → 4) simplificar → 5) volver a correr las pruebas, y repetir.
**Contexto:** con **TDD**, las pruebas son la red de seguridad. Es **mantenimiento preventivo**.

---

### 8.5 ⭐⭐ Sistemas heredados: cómo se evalúan y qué estrategia aplicar

**Respuesta modelo:**
Un sistema heredado (legacy) es un sistema **viejo pero todavía útil o crítico**, con tecnología obsoleta, estructura degradada y poca documentación. Se evalúa con una **matriz de 2 ejes: valor de negocio y calidad del sistema**:
- **Valor alto + calidad baja → reingeniería** (vale mucho, se arregla por dentro).
- **Valor bajo + calidad baja → sustitución** (no conviene invertir).
- **Valor alto + calidad alta → mantenimiento normal.**
- **Valor bajo + calidad alta → mantenimiento** (y eventualmente sustitución si los costos suben).

---

### 8.6 ⭐⭐ Leyes de Lehman (o "¿por qué el software tiene que evolucionar?")

**Respuesta modelo:**
Son observaciones sobre cómo evolucionan los **sistemas grandes hechos a medida**. Las más importantes:
1. **Cambio continuo:** un sistema que se usa en el mundo real tiene que cambiar o se vuelve menos útil.
2. **Complejidad creciente:** cada cambio degrada la estructura; hay que invertir esfuerzo extra (refactoring) para controlarla.
6. **Crecimiento continuo:** la funcionalidad tiene que crecer para que el usuario siga conforme.
7. **Disminución de la calidad:** la calidad baja si el sistema no se adapta al entorno.
(Las otras: evolución autorregulada, estabilidad organizacional, conservación de la familiaridad y retroalimentación).
**Cierre:** no están del todo comprobadas para software enlatado, sistemas basados en COTS ni organizaciones chicas.

---

### 8.7 ⭐⭐ Mapeo de herencia a una base de datos relacional

**Respuesta modelo:**
El **problema de impedancia** es que los objetos (con herencia, referencias, comportamiento) y las tablas relacionales (filas, columnas, FK) son paradigmas distintos, y hay que traducir de uno a otro. Para la herencia hay 3 alternativas. *Ej.: `Persona` ← `Conductor`, `Autoridad`.*
1. **Simular la herencia:** una tabla por padre y una por cada hijo, con FK al padre. ✅ Sin redundancia, es la más fiel. ❌ Requiere JOINs.
2. **Eliminar el padre:** solo tablas de los hijos, con los atributos del padre copiados. ✅ Sin JOINs. ❌ Duplicación: un cambio en un atributo común se hace en varias tablas.
3. **Eliminar los hijos:** una única tabla con todos los atributos más una columna "tipo". ✅ Simple y rápida. ❌ Muchas columnas en NULL.
(Se puede sumar las **5 reglas de mapeo**: una tabla por clase, atributo → columna, PK = identificador del objeto, objeto → fila, asociación de cardinalidad mayor que 1 → tabla).

---

### 8.8 ⭐⭐ Súper Objeto Persistente vs. Esquema de Persistencia

**Respuesta modelo:**
Son los 2 modelos para darle persistencia a los objetos.
- **Súper Objeto Persistente:** todas las clases de negocio **heredan** de una superclase que sabe guardarse y recuperarse. ✅ Fácil de implementar, no agrega clases. ❌ **Viola SRP y SoC** (cada clase mezcla negocio con acceso a datos), aumenta el acoplamiento con la BD y rompe OCP.
- **Esquema de Persistencia:** la persistencia va en una **capa aparte** entre el negocio y la BD (típicamente un **framework de persistencia** / ORM). ✅ Mantiene la **cohesión**, es extensible y reutilizable, y respeta **SRP, OCP y DIP**. ❌ Más clases.
**Conclusión:** se prefiere el Esquema. Un framework de persistencia tiene que, como mínimo, **guardar y recuperar objetos** y manejar **transacciones (commit/rollback)**. Funciona con el **principio de Hollywood**: "no nos llame, nosotros lo llamaremos".

---

### 8.9 ⭐⭐ Análisis vs. Diseño (en el PUD)

**Respuesta modelo:**
El **análisis** modela el **problema** en términos **lógicos/conceptuales**; el **diseño** modela la **solución** en términos **físicos**, como plano de la implementación.
| Análisis | Diseño |
|---|---|
| Conceptual, genérico (sirve para varios diseños) | Físico, específico de una implementación |
| Menos formal | Más formal |
| Poco centrado en la secuencia | Muy centrado en la secuencia |
| Bosquejo de la arquitectura | Manifiesto de la arquitectura |
**Qué se diseña (6 aspectos):** con la **arquitectura en el centro** (definida por los requerimientos **no funcionales**), más **datos**, **procesos**, **experiencia de usuario**, **formas de entrada/salida** y **procedimientos manuales** (ver sección 1.4).
**Cierre:** hoy la frontera es difusa, porque se usan las mismas herramientas (UML) en todas las etapas.

---

### 8.10 ⭐⭐ Explicar SOLID (o 2 o 3 principios) con ejemplo

**Respuesta modelo (una línea por letra + ejemplo):**
- **S (responsabilidad única):** una clase tiene **una sola razón para cambiar**. *Ej.: `Factura` no debería imprimirse ni guardarse sola.*
- **O (abierto/cerrado):** **abierta a la extensión, cerrada a la modificación**. *Ej.: agregar un medio de pago nuevo creando una clase, sin tocar las que existen (Strategy).*
- **L (Liskov):** una subclase tiene que poder **reemplazar a su padre sin romper nada**. *Ej. clásico: `Cuadrado` hereda de `Rectángulo` y rompe `setAncho`.*
- **I (segregación de interfaces):** muchas interfaces chicas y específicas mejor que una gigante; **nadie implementa métodos que no usa**.
- **D (inversión de dependencias):** los módulos de alto nivel dependen de **abstracciones**, no de clases concretas.
**Cierre:** los patrones aplican estos principios (State y Strategy → OCP y SRP; Observer → bajo acoplamiento y DIP).

---

### 8.11 ⭐ ¿Qué es un patrón de diseño? Clasificación + comparar State y Strategy

**Respuesta modelo:**
Un patrón es una **solución probada a un problema de diseño que se repite en un contexto**. Tiene 4 elementos: **nombre, problema, solución y consecuencias**. GoF los clasifica según su **propósito** en **creacionales** (cómo se crean los objetos), **estructurales** (cómo se componen clases y objetos; ej.: Adapter) y **de comportamiento** (cómo interactúan y se reparten responsabilidades; ej.: State, Strategy, Observer, Iterator).
**State vs. Strategy:** tienen **la misma estructura** (un contexto delega en una interfaz con implementaciones concretas), pero distinta **intención**: en **Strategy** el **cliente elige el algoritmo** desde afuera y en general no cambia solo; en **State** el **objeto cambia su comportamiento solo cuando cambia su estado interno**, y los estados concretos deciden las transiciones.

---

### 8.12 ⭐ Composición vs. herencia / "programar hacia la interfaz"

**Respuesta modelo:**
La **herencia** es reutilización de "caja blanca": se define en compilación, la subclase depende de la implementación del padre (si el padre cambia, se rompe) y **rompe el encapsulamiento**. La **composición** es de "caja negra": los objetos se combinan en tiempo de ejecución a través de interfaces, el comportamiento **se puede cambiar dinámicamente** y el acoplamiento es menor. Se **prefiere la composición** (con **delegación**), y la herencia se reserva para relaciones "es-un" verdaderas.
**Programar hacia la interfaz:** el cliente conoce solo el **contrato** (la interfaz), no la clase concreta, así que se pueden cambiar implementaciones sin tocar al cliente. Es la base de Strategy, State, Observer y DIP.

---

> 🎯 **Si tenés poco tiempo para estudiar el desarrollo, priorizá:** 8.1 (reingeniería vs. refactoring), 8.3 (tipos de mantenimiento), 8.7 y 8.8 (persistencia) y 8.11 (State vs. Strategy).
