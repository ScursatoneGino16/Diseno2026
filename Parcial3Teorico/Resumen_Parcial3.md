> **Cómo está armado cada tema**
> 1. **La idea en una frase**: si te acordás solo de eso, ya tenés la base.
> 2. **Explicación**: el contenido completo, en orden.
> 3. **Ejemplo de clase**: el caso que usaron los profes (Venta Digital, Liga de Fútbol, Infracciones de Tránsito, etc.).
> 4. **⚠️ Ojo**: confusiones típicas.
> 5. **Autoevaluación**: preguntas con la respuesta oculta (hacé clic en "Ver respuesta"). Si no podés contestar una, volvé a leer la sección.

---

## Plan de estudio (05/11 → 12/11)

| Día | Qué leer | Cómo |
|---|---|---|
| **Jue 05/11** | 1. Proceso de Diseño + 2. Principios de Diseño (hasta Composición) | Leer entero una vez, sin apuro |
| **Vie 06/11** | 2. Principios: SOLID + 3. Patrones: conceptos y clasificación | Hacer las autoevaluaciones de lo del jueves |
| **Sáb 07/11** | 3. Patrones evaluables: **State, Strategy, Observer** | Dibujar a mano la estructura de cada uno sin mirar |
| **Dom 08/11** | 3. Patrones evaluables: **Adapter, Iterator** + resto de patrones (tabla) | Dibujar los 5 de memoria |
| **Lun 09/11** | 4. Diseño de Persistencia | Hacer el ejercicio de mapear la herencia Persona |
| **Mar 10/11** | 5. Evolución del Software (mantenimiento, Lehman, reingeniería, refactoring) | Autoevaluaciones |
| **Mié 11/11** | **Repaso general**: solo "La idea en una frase", las tablas y **todas** las autoevaluaciones | Lo que falles, releer |
| **Jue 12/11** | Antes del parcial: sección final **"Repaso de 10 minutos"** | — |

---

# 1. PROCESO DE DISEÑO (Workflow de Diseño)

### La idea en una frase
> Diseñar es **transformar el modelo lógico (análisis) en un modelo físico** con detalle suficiente para poder construirlo, teniendo en cuenta los requerimientos no funcionales y las restricciones del negocio y de la tecnología.

## 1.1 ¿Dónde está el diseño? (PUD / RUP)

El Proceso Unificado tiene estos workflows en cadena. Cada uno produce un modelo:

```
Modelado de Negocio  → Modelo de Negocio
Requerimientos       → Modelo de Requerimientos      ┐ Dominio del
Análisis             → Modelo de Análisis            ┘ PROBLEMA
Diseño               → Modelo de Diseño + Modelo de Despliegue   ┐ Dominio de la
Implementación       → Modelo de Implementación                  │ SOLUCIÓN
Prueba               → Modelo de Prueba                          ┘
Despliegue           → Versión del producto de software
```

- El **análisis** pertenece al **dominio del problema** (qué hay que resolver).
- El **diseño** pertenece al **dominio de la solución** (cómo lo vamos a construir).

## 1.2 ¿Qué es el diseño? (dos definiciones de la diapositiva)
1. Proceso en el que se aplican **técnicas y principios** para definir un dispositivo, un proceso o un sistema con suficiente nivel de detalle como para permitir su **realización física**.
2. Proceso **iterativo** de transformar un **modelo lógico en un modelo físico**, teniendo en cuenta las **restricciones del negocio**.

## 1.3 Análisis vs. Diseño
- En la metodología **estructurada** se separaban bien:
  - **Análisis** = *descripción del problema* → **modelo esencial/conceptual** → **rol del analista**.
  - **Diseño** = *descripción de la solución* → **modelo de implementación** → **rol del diseñador**.
- Regla corta: **el análisis modela la solución en términos lógicos; el diseño, en términos físicos.**
  - La diapositiva lo muestra con una **maqueta** (análisis) frente a un **plano de arquitecto** (diseño).

**Tendencias actuales de la ingeniería de software**
- Darle un papel **protagónico a los requerimientos**.
- Poner especial atención en la **arquitectura**.
- Usar **las mismas herramientas para modelar** (UML) en todas las etapas.
- **Usar un proceso**.
- Con los métodos nuevos, **la frontera entre análisis y diseño es cada vez más difusa**: es muy difícil plantear la solución sin pensar en cómo se va a implementar.

**Tabla del PUD (muy preguntable)**

| | Análisis | Diseño |
|---|---|---|
| Tipo de modelo | **Conceptual**: es una abstracción del sistema y **evita** los aspectos de implementación | **Físico**: es un **plano de la implementación** |
| Generalidad | **Genérico** respecto del diseño (aplicable a varios diseños) | **No genérico**: específico para una implementación |
| Formalidad | Menos formal | Más formal |
| Dinámica | Dinámico, **poco centrado en la secuencia** | Dinámico, **muy centrado en la secuencia** |
| Arquitectura | **Bosquejo** del diseño, incluida la arquitectura | **Manifiesto** del diseño, incluida la arquitectura |

## 1.4 ¿Qué aspectos hay que diseñar? (6, con la **Arquitectura en el centro**)

El dibujo de la diapositiva pone la **Arquitectura** en el medio, rodeada por **Datos, Procesos, UX, Formas de E/S y Procedimientos Manuales**.

### 1) Arquitectura
- **Arquitectura de software**: es el **conjunto de decisiones significativas** sobre **cómo resolver los requerimientos NO funcionales** afectando lo menos posible la funcionalidad del producto. Son decisiones **estratégicas**.
- **Diseño arquitectónico**: es el **proceso** que permite obtener una arquitectura, transformando un modelo lógico en una **tecnología específica**.

### 2) Datos
Es el conjunto de actividades para resolver **cómo organizar, almacenar y acceder** a los datos que maneja el software. Es una etapa **crítica**, porque asegura que los datos se representen de forma eficiente y coherente en toda la aplicación. Hay que considerar:
- manejo de **persistencia** (es la Unidad 4 de este resumen),
- cálculo de **volúmenes** de datos,
- mecanismos de **backup**,
- gestión de **transacciones**,
- control de **acceso y seguridad**.

### 3) Procesos
- Consiste en transformar las **realizaciones de CU de análisis** (descripciones conceptuales) en **realizaciones de CU de diseño**.
- En esa transformación se **agregan los aspectos no funcionales** (persistencia, concurrencia, seguridad, etc.). En el análisis se dejaron de lado a propósito, para **manejar la complejidad**.
- Estas realizaciones se hacen **usando patrones de diseño** (Unidad 3).

### 4) Experiencia de Usuario (UX)
- Es el proceso de **crear y mejorar la experiencia global** que tiene un usuario al interactuar con el producto.
- Objetivo: que la interacción sea **intuitiva, eficiente y satisfactoria**, desde que el usuario empieza a usar el sistema hasta que logra su objetivo.
- **Disciplinas relacionadas** (diapositiva): Diseño, Sociología, Programación, Inteligencia Artificial, Ingeniería de Software, Ergonomía, Psicología.
- El diagrama "Disciplinas" (Dan Saffer) incluye: UX (diseño de experiencias), AI (arquitectura de información), ARQ (arquitectura), DI (diseño industrial), DG (diseño gráfico), IxD (diseño de interacción), HCI (interacción humano-computador), producción de contenido y diseño de sonido.

### 5) Formas de Entrada/Salida
Describen cómo **entra** la información al software y cómo se **presentan las salidas**.
- **Formas de ENTRADA**: se necesitan cuando la información **entra al sistema en un momento distinto al que ocurre**, como en los sistemas **batch o por lotes**. Ejemplo: se completa un formulario en papel y después alguien lo carga.
- **Formas de SALIDA**: se necesitan en **todo tipo de aplicaciones** (en línea, batch y tiempo real).

### 6) Procedimientos Manuales
- Describen **cómo se integra el software al Sistema de Negocio**.
- Objetivo: que las **tareas manuales** asociadas al uso del software se hagan de forma **eficiente, coherente y precisa**, minimizando los errores humanos y cumpliendo los requisitos operativos y de negocio.

## 1.5 Guía para evaluar un buen diseño (3 características)
| Característica | Qué significa |
|---|---|
| **Cumple los requisitos** | Implementa todos los requisitos **explícitos** del modelo de análisis y se ajusta a los **implícitos** que desea el cliente |
| **Es legible y comprensible** | Sirve de **guía clara** para quienes generan el código y para quienes lo prueban y le dan soporte |
| **Da una imagen completa** | Cubre los dominios de **datos, funcional y de comportamiento** desde la perspectiva de la implementación |

## 1.6 Principios del proceso de diseño del software (5)
1. Debe poder **rastrearse hasta el modelo de análisis** (trazabilidad).
2. **No debe inventar nada que ya esté inventado** (reutilizar).
3. Debe **minimizar la "distancia intelectual"** entre el software y el problema, es decir, parecerse al mundo real que modela.
4. Debe presentar **uniformidad e integración**.
5. Debe **estructurarse para admitir cambios**.

## 1.7 Workflow de Diseño
**Propósito (5 puntos)**
1. Lograr una **comprensión profunda** de los **requerimientos no funcionales y las restricciones tecnológicas**.
2. Crear una **entrada para el workflow de implementación**.
3. **Descomponer** la implementación en partes manejables, lo que permite que **varios equipos trabajen en paralelo**.
4. **Capturar las interfaces entre subsistemas**.
5. Crear una **abstracción de la solución** sin estar atados a una tecnología.

**Rol en el ciclo iterativo e incremental del PUD**
- Fases: **Inicio, Elaboración, Construcción, Transición**. Cada fase tiene iteraciones y en cada iteración se recorren todos los workflows.
- **En Elaboración**: el diseño contribuye a una **arquitectura estable y robusta**.
- **En Construcción**: el diseño crea el **plano para la implementación**.

**Trabajadores y artefactos**

| Trabajador | Es responsable de |
|---|---|
| **Arquitecto** | Modelo de Diseño, Modelo de Despliegue, Descripción de la Arquitectura |
| **Ingeniero de Casos de Uso** | Realización de Casos de Uso – Diseño |
| **Ingeniero de Componentes** | Clases de Diseño, Subsistemas de Diseño, Interfaces |

**El workflow como proceso (entradas → salidas)**
```
ENTRADAS                                          SALIDAS
Modelo de Análisis:                               Modelo de Diseño:
  - Realizaciones de CU de Análisis                 - Realizaciones de CU de Diseño
  - Clases de Análisis          ──►  WORKFLOW  ──►  - Clases de Diseño
  - Paquetes de Análisis             DE DISEÑO      - Interfaces
  - Descripción de la Arquitectura                  - Descripción de la Arquitectura
Modelo de Requerimientos:                         Modelo de Despliegue:
  - Descripción de la Arquitectura                  - Descripción de la Arquitectura
     (vista arquitectónica de la funcionalidad
      + requerimientos no funcionales)
```

## 1.8 Diagramas UML 2.0 que se usan en Diseño
- **Modelan ESTRUCTURA**
  - de las **clases**: de Clases, de Paquetes, de Estructura Compuesta, de Componentes, de Despliegue;
  - de los **objetos**: de Objetos.
- **Modelan COMPORTAMIENTO**
  - de las **clases**: de Casos de Uso, de Actividad, de Máquina de Estados;
  - de los **objetos** (de interacción): de Comunicación, de Secuencia, de Timing, de Descripción de la Interacción.

### ⚠️ Ojo
- "Arquitectura" se define en función de los **requerimientos NO funcionales**, no de los funcionales.
- Las **formas de entrada** solo hacen falta cuando el dato entra **en otro momento** (batch). Las de **salida** se usan **siempre**.
- **Bosquejo** = análisis. **Manifiesto** = diseño.

### Autoevaluación
1. ¿Cuál es la diferencia entre análisis y diseño en una sola frase?
2. Nombrá los 6 aspectos que deben diseñarse. ¿Cuál está en el centro?
3. ¿Cuándo hace falta diseñar formas de entrada?
4. ¿Qué aporta el diseño en la fase de Elaboración y qué en la de Construcción?
5. ¿De qué es responsable el Ingeniero de Componentes?

<details><summary>Ver respuesta</summary>

1. El análisis modela la solución en términos lógicos (conceptuales, genéricos); el diseño, en términos físicos (específicos de una implementación).
2. Arquitectura (en el centro), Datos, Procesos, UX, Formas de E/S y Procedimientos Manuales.
3. Cuando la información entra al sistema en un momento distinto al que ocurre (sistemas batch o por lotes).
4. En Elaboración contribuye a una arquitectura estable y robusta; en Construcción crea el plano para la implementación.
5. De las clases de diseño, los subsistemas de diseño y las interfaces.
</details>

---

# 2. PRINCIPIOS DE DISEÑO ORIENTADO A OBJETOS

### La idea en una frase
> Todos los principios buscan lo mismo: **alta cohesión y bajo acoplamiento** (independencia de componentes). Son una **forma de pensar** que está por encima de los patrones; no son recetas concretas.

## 2.1 Qué debe considerar un buen diseño
- **Independencia de componentes**, que se logra con:
  - **Cohesión** (alta): cada módulo hace **una sola cosa** y todo lo que tiene está relacionado.
  - **Acoplamiento** (bajo): los módulos **dependen poco** unos de otros.
- **Identificación y tratamiento de excepciones**.
- **Prevención y tolerancia de defectos**.

> **Cohesión + Acoplamiento = Interdependencia Funcional.** Todos los principios son **corolarios** (consecuencias) de estos principios básicos.

## 2.2 DRY – Don't Repeat Yourself (No te repitas)
- **Idea**: no copiar y pegar código. El código duplicado "huele mal" (*something smells*).
- **Evitar duplicaciones + Crear abstracciones ⇒ Cada requerimiento en un único lugar.**
- ¿Por qué? Si una regla está repetida en 5 lugares y cambia, hay que cambiarla en los 5, y si te olvidás de uno aparece un bug.

## 2.3 SoC – Separación de Intereses (Separation of Concerns)
- **Definición**: aislar la aplicación en **secciones separadas**. Cada sección aborda una **preocupación distinta**, con **poca superposición** con las otras. La diapositiva lo compara con un cajón de cubiertos, con cada cosa en su compartimiento.
- **Se aplica en 2 niveles**:
  - **Arquitectónico**: separar en **capas** (interfaz, negocio, datos…).
  - **De programación**: los servicios se separan en distintos niveles (clases, métodos).
- **Ejemplo de clase**: una clase `Pago` que tenía `cliente, fechaPago, monto, nombre, numeroTarjeta, tipoTarjeta` y los métodos `borrarOrden()` y `procesarPago()` **mezclaba intereses**. La solución es separarla en **Pago** (fechaPago, monto, numeroTarjeta, tipoTarjeta), **Orden** (fecha, nroOrden, delete(), getDatosOrden()) y **Cliente** (nombre, numero), relacionadas por asociaciones.
- **Beneficios**: gestiona la complejidad y aumenta la mantenibilidad; mejora la cohesión y reduce el acoplamiento; facilita la extensión (OCP); permite la reutilización; reduce la fragilidad de los módulos.

## 2.4 YAGNI – You Ain't Gonna Need It (No lo vas a necesitar)
- **"Nunca desarrolles código que creas que va a servir en el futuro pero que ahora no tiene valor. El futuro siempre está cambiando. Es una pérdida de tiempo."**
- El dibujo: ✗ "creo que este código me va a servir en el futuro" frente a ✓ "programo solo lo que necesito ahora".

## 2.5 KISS – Keep It Simple, Stupid (Mantenelo simple)
El dibujo compara un camino enredado (estrategia equivocada) con una línea recta (estrategia correcta).

**¿Qué implica KISS?**
- **No aumentar infinitamente el nivel de abstracción**, porque eso multiplica la complejidad.
- **No incluir funciones "en reserva"** por si algún día alguien las necesita (para esto el enfoque de **YAGNI** es más correcto).
- **No incluir una librería enorme** si solo se necesitan un par de funciones.
- **Descomponer algo complejo en componentes simples** es arquitectónicamente correcto (acá KISS se hace eco de **DRY**).
- **No siempre hace falta precisión matemática absoluta** ni detalle extremo: alcanza con la precisión **suficiente** para una solución de calidad, y con los detalles que el usuario **necesita**, no con el máximo posible.

**Beneficios**: mayor calidad del código; código más simple de mantener; más flexible y fácil de ampliar, modificar y mejorar.

## 2.6 Ley de Demeter / Principio de Menor Conocimiento ("Don't talk to strangers")
- **Idea**: cada unidad debe tener **conocimiento limitado** sobre las demás y solo conocer a las que están **estrechamente relacionadas** con ella. **"Hablá con tus amigos inmediatos, no con extraños."**
- **Ejemplo**: `Employee → Enterprise → Address`. Si `Employee` quiere la ciudad de su empresa, **no** debería hacer `empleado.getEnterprise().getAddress().getCity()`, porque eso es hablar con un extraño (Address).
- **Reglas: un método M de un objeto O solo puede invocar métodos de:**
  1. **O mismo**,
  2. **los parámetros** de M,
  3. **los objetos creados dentro de** M,
  4. **los componentes directos** de O (sus atributos).
- **Propósito**: producir objetos **débilmente acoplados**.
  - *Fuertemente acoplados*: dependen mucho unos de otros y necesitan mucha información entre sí para funcionar.
  - *Débilmente acoplados*: tienen un número mínimo de dependencias y saben poco de los métodos de otras clases.
- **Ventajas**:
  - Mayor legibilidad y cohesión, y por lo tanto más **mantenibilidad y adaptabilidad**, porque se minimiza la cantidad de cambios cuando se modifica algo.
  - Reduce el riesgo de que métodos no revisados pasen desapercibidos en otra parte del código.
  - Si los objetos están muy acoplados y cambia el orden de los argumentos de un método, hay que cambiarlo en **todos** los lugares donde se lo llama.
- **Desventaja**: requiere **muchos métodos envoltorio (wrappers)** para llevar información entre objetos distantes.

## 2.7 Tell, Don't Ask (Decí, no preguntes)
- **✗ NO**: un objeto le **pregunta** datos a otro (ask, ask, ask…) y toma la decisión **afuera**, porque la lógica queda separada de los datos.
- **✓ SÍ**: le **dice** (tell) al otro objeto que haga algo, y la **lógica queda junto a los datos**.
- La secuencia de la diapositiva: **No usar los objetos para pedirles cosas → Decirles que hagan cosas → Un objeto se define por su comportamiento → Distribución de responsabilidades → POLIMORFISMO.**

## 2.8 Interfaz y Relación de Realización
- **Interfaz**: es un **contrato** que define **qué métodos debe tener una clase, pero no su implementación**. Además, es una clase abstracta.
  - Permite definir **comportamientos comunes sin conocer los detalles**.
  - Las clases que la implementan **deben cumplir el contrato**.
  - Hace que el software sea **más fácil de extender**.
- **Relación de Realización** (en UML, línea punteada con triángulo vacío):
  - Indica que una **clase concreta implementa una interfaz**; es la conexión entre ambas.
  - La clase concreta se compromete a implementar **TODOS** los métodos abstractos de la interfaz.
  - Es la formalización de que una clase **"cumple" el contrato** de la interfaz.

## 2.9 Program to Interface Principle (Programar hacia la interfaz, no hacia la implementación)
- **Origen del nombre (anécdota de clase)**: el **IMP (Interface Message Processor)**, uno de los primeros routers de ARPANET. Las computadoras no se conectaban entre sí directamente, sino que **todas hablaban con el IMP usando un protocolo común**. Cada una implementaba su parte, pero todas seguían **la misma "interfaz"**.
- **Ejemplo de clase (Equipo y Jugadores)**:
  - **Problema**: `Equipo.agregarJugador(???)`. ¿Qué tipo de parámetro le ponemos? Si `Equipo` depende de JugadorBasketball, JugadorTenis, JugadorBaseball, JugadorFootball y JugadorHockey, queda acoplado a **5 clases concretas**.
  - **Solución en 4 pasos**:
    1. **Determinar qué necesita exactamente** un objeto del otro: ¿qué métodos ejecuta? (`getDeporte()` y `jugar()`).
    2. **Describir esos métodos en una nueva interfaz** o clase abstracta: `<<interface>> Atleta`.
    3. **Hacer que las clases dependencia implementen esa interfaz** (realización).
    4. **Hacer que la otra clase (Equipo) dependa de la interfaz** y no de las clases concretas: `agregarJugador(Atleta)`.
  - **Conclusión: "Depender de abstracciones, no de clases concretas."**

```
 Equipo                          <<interface>> Atleta
 + agregarJugador(Atleta) ────►  + getDeporte(): String
                                 + jugar(): void
                                       △  (realización, punteada)
          ┌──────────────┬─────────────┼──────────────┬──────────────┐
  JugadorBasketball JugadorFootball JugadorTenis JugadorBaseball JugadorHockey
```

- **Trampas**:
  - **#1**: usar una interfaz pero **no programar para la interfaz** (seguir dependiendo de las clases concretas).
  - **#2**: **una interfaz para todo** (abusar de las interfaces).
- **Dos reglas para pensar cuándo usar interfaces**:
  1. Cuando hay **múltiples implementaciones concretas**.
  2. Para **desacoplar tu sistema de un sistema externo**.

## 2.10 Favorecer la composición sobre la herencia (Composite Reuse Principle)

| HERENCIA – reúso de **caja blanca** | COMPOSICIÓN (asociación, agregación, composición) – reúso de **caja negra** |
|---|---|
| Es fácil modificar lo heredado | El comportamiento depende de las relaciones |
| **Rompe el encapsulamiento** (la subclase ve los detalles del padre) | **No rompe el encapsulamiento** |
| Se define **estáticamente** (en compilación) | Se define **dinámicamente**; se puede cambiar **en tiempo de ejecución** |

- La balanza de la diapositiva se inclina hacia la **composición**.
- **Delegación**: es un **mecanismo para reutilizar usando composición**.
  - **Ejemplo**: `Window` tiene un atributo `rectangulo`. `Window.area()` hace `return rectangulo.area()`, es decir, **le delega el trabajo** al Rectángulo, que calcula `ancho * alto`. `Window` **no hereda** de `Rectángulo`.
- **Delegación + Agregación + Composición ⇒ Flexibilidad, Mantenibilidad, Reusabilidad y Extensibilidad.**
- **La delegación es reuso a nivel de OBJETOS; la herencia es reuso a nivel de CLASES. La herencia no es la única opción.**

## 2.11 Principios SOLID

> En la diapositiva cada principio viene con un dibujo de robots (✗ mal / ✓ bien) y con una "frase JENGA":

| Principio | Frase de la diapositiva |
|---|---|
| **S**ingle Responsibility | "Solo porque se pueda, no significa que debas" (navaja suiza con 100 herramientas) |
| **O**pen Closed | "La cirugía de cerebro no es necesaria para usar un sombrero" |
| **L**iskov Substitution | "Si luce como un pato, grazna como un pato, pero necesita baterías… tal vez es la abstracción equivocada" |
| **I**nterface Segregation | "¿Querés que coloque esto? ¿Dónde?" (un enchufe con demasiadas entradas) |
| **D**ependency Inversion | "¿Soldarías una lámpara directamente a los cables de la pared?" (no: usás un enchufe, que es una interfaz) |

### S – SRP: Principio de Responsabilidad Única
- **Enunciado**: una clase debe tener **una única responsabilidad = una única razón para cambiar**.
- **Única responsabilidad + Única razón para cambiar ⇒ ALTA COHESIÓN.**
- Robot: ✗ "Soy chef, jardinero, pintor y chofer"; ✓ 4 robots, uno por tarea.
- **Ejemplo Automóvil**: la clase tenía `cambiarNeumaticos(), conducir(), controlarAceite(), getAceite(), lavar(), start(), stop()`.

| Responsabilidad | ¿Cumple SRP? |
|---|---|
| El automóvil **se inicia** a sí mismo | ✓ Cumple |
| El automóvil **se detiene** a sí mismo | ✓ Cumple |
| El automóvil **se cambia el neumático** a sí mismo | ✗ Viola |
| El automóvil **se conduce** a sí mismo | ✗ Viola |
| El automóvil **se lava** a sí mismo | ✗ Viola |
| El automóvil **obtiene su nivel de aceite** | ✓ Cumple |

  **Solución**: `Automovil` (getAceite, start, stop), `LavaderoAuto` (lavar(Automóvil)), `Conductor` (conducir(Automóvil)), `Mecánico` (cambiarNeumáticos(Neumático[], Automóvil), controlarAceite(Automóvil)).
- **Ejemplo Cita–Evento ("¿se aplica SRP y DRY?")**:
  - **Mal diseño**: `Cita` tenía `enviarFlores(), irALaCita(), irARestaurante(), verPelicula(), validarEvento()`, y había subclases `PrimeraCita, SegundaCita, TerceraCita`, **cada una repitiendo** `validarEvento(evento)`. También había subclases de Evento (`EventoVerPelicula, EventoIrARestaurante, EventoOrdenarFlores`) que **solo cambiaban el nombre**.
  - **Buen diseño (SRP + DRY)**: `Cita` (numeroCita, agregarEvento(), irALaCita()) y `Evento` (descripcion, **permitidoEnCitas: int[]**, `permitidoEnCita(nroCita): boolean`). **Cada evento sabe en qué citas está permitido**, y se eliminan todas las subclases repetidas.

### O – OCP: Principio Abierto-Cerrado
- **Enunciado**: las clases deben estar **abiertas para la extensión y cerradas para la modificación**.
- **Permitir el cambio + No modificar lo existente ⇒ FLEXIBILIDAD.**
- Robot: ✗ para que pueda pintar hay que **abrirlo y modificarlo**; ✓ se le **agrega** la herramienta sin tocarlo.
- **Ejemplo Pintor**:
  - ✗ `Pintor` con `limpiarPinceles(), pintar(), prepararCaballete()`. Cada estilo nuevo obliga a **modificar** `pintar()`.
  - ✓ `Pintor` con `setEstiloPintar(EstiloPintura)` → `EstiloPintura` abstracta (`getEstilo(), pintar()`) ← `EstiloModerno, EstiloImpresionista, EstiloSurrealista, EstiloCubista`. Un estilo nuevo es **una clase nueva**, y el Pintor no se toca.
- **Corolario: Encapsular lo que varía**: **Separar lo que cambia + Ubicarlo en otra clase ⇒ Maneja la variabilidad.** Es la base de Strategy y State.

### L – LSP: Principio de Sustitución de Liskov
- **Enunciado**: los **subtipos deben poder sustituir a su tipo base** sin que el programa se rompa ni cambie su comportamiento esperado.
- **Tipos sustituibles por sus tipos base + Revela problemas de estructura si existieran ⇒ HERENCIA BIEN DISEÑADA.**
- Robot: Sam hace café y Eden es hija de Sam. ✗ Le piden café a Eden y dice "no sé hacer café, tomá agua". ✓ Eden hace un capuchino.
- **Ejemplo**:
  - ✗ **"Típico ejemplo inicial de herencia"**: `Persona (apellido, dni, fechaNacimiento, nombre)` ← `Empleado` y `Cliente`. Es **herencia de datos**: se hereda solo para reutilizar atributos, aunque Empleado y Cliente no se comportan como lo mismo.
  - ✓ **"Herencia de comportamiento"**: `Empleado (dni, nombre, sueldoFijo, calcularSueldo())` ← `EmpleadoProduccion (numeroPiezas, puntosPorPieza, calcularSueldo())` y `EmpleadoComercial (porcentajePorVentas, calcularSueldo())`. Cualquiera puede usarse donde se espera un Empleado, y `calcularSueldo()` se redefine.

### I – ISP: Principio de Segregación de Interfaces
- **Enunciado**: **no obligar a los clientes a depender de métodos que no usan** y **no obligar a las clases a implementar interfaces que no necesitan**.
- **⇒ Bajo acoplamiento, facilidad de implementación y prueba.**
- Robot: ✗ pizarrón "Todos los robots: girar, rotar brazos, mover antenas", y uno dice "¡Ups, no tengo antenas!"; ✓ separado en "robots que giran", "robots que rotan brazos", "robots que mueven antenas".
- **Ejemplo de clase**: una interfaz gorda `<<interface>> Vehiculo` con `acelerar, apagarRadio, cambiarMarcha, encenderMotor, frenar, giroDerecha, giroIzquierda, prenderLuces, sacarCD`, usada por `TerminalColectivos, Autopista, Estacionamiento, Conductor`. La `Autopista` no necesita `sacarCD()`.
  - **Solución**: segregar en interfaces chicas según cada cliente. `VehiculoA` para Autopista (`estanLucesPrendidas(), estaPagoImpuesto(), getTipoVehiculo(), getVelocidad()`) y `VehiculoE` para Estacionamiento (`estanLucesPrendidas(), getLargo(), getVelocidad(), tieneTicket()`). La imagen de los tachos de reciclaje (papel, plástico, vidrio) representa la idea: **cada cosa en su interfaz**.

### D – DIP: Principio de Inversión de Dependencias
- **Premisas**:
  - **A**: los módulos de **alto nivel no deben depender** de los de **bajo nivel**. **Ambos deben depender de abstracciones.**
  - **B**: las **abstracciones no deben depender de los detalles**. Son los **detalles** los que deben depender de las abstracciones.
- **⇒ Flexibilidad, Robustez, Movilidad.**
- **Qué se busca**: que los módulos no dependan unos de otros sino de **abstracciones**, para que sean **más fáciles de reutilizar**.
- Robot: ✗ "corto pizza con mi brazo cortapizzas" (la herramienta está soldada); ✓ "corto pizza con cualquier herramienta que me den".
- Imagen: esperar el colectivo (dependés de algo fijo) frente a pedir un taxi por app (dependés de un servicio y cualquier taxi sirve).
- **Ejemplo de componentes**:
  - ✗ `Componente A ⇢ Componente B ⇢ Componente C` (dependencia directa).
  - ✓ Cada componente define una **interfaz** en su paquete: `Componente A ⇢ <<interface>> ServicioComponenteA`, y **Componente B la implementa**. Lo mismo entre B y C.
- **"Negocio abstracto → Interfaz polimórfica ◁ Despliegue concreto"**: la **dependencia de ejecución** va del negocio al detalle, pero la **dependencia de compilación** se **invierte**: el detalle depende de la interfaz. Por eso se dice que la dependencia se "invierte".
- Se relaciona con el **Principio de Hollywood** ("No nos llames, nosotros te llamamos").

> ⚠️ **Error en las diapositivas**: dos diapositivas de DIP dicen "**ISP** – Principio de Inversión de Dependencias". Es un error de tipeo: es **DIP**.

## 2.12 Principios para la implementación en POO
- Notación **CamelCase**.
- **Código feliz** 😊.
- **Métodos cortos**: 7±2 líneas (siempre y cuando 3 sean comentarios).
- **Fácil lectura ⇒ fácil mantenimiento**.
- **Escribir código para quien lo va a mantener**.
- **Principio del menor asombro**: el nombre del método debe corresponder con lo que hace.
- **Abrazar el cambio**.
- **Reusar**.
- **Usar convenciones de código**.
- En síntesis: **"No me hagas pensar."**

## 2.13 Para tener en cuenta (diapositiva de cierre)
- Estos principios están en un **nivel más alto que los patrones de diseño**.
- **No son un ejemplo concreto** para resolver un problema.
- Son una **forma de pensar**: ideas para tener en mente al diseñar o programar, para crear **código limpio y mantenible**.

### ⚠️ Ojo
- **SRP** se relaciona con **cohesión**; **ISP** y **Demeter** con **acoplamiento**.
- **OCP** = "encapsular lo que varía". En la diapositiva aparece **entre paréntesis** junto a Open Closed.
- La **delegación** es la forma de reusar **sin herencia**.
- **YAGNI** ≠ **KISS**: YAGNI dice "no agregues lo que no necesitás"; KISS dice "hacé simple lo que sí necesitás". KISS cita a YAGNI y a DRY.

### Autoevaluación
1. ¿Cuáles son los 4 tipos de objetos a los que un método puede mandarle mensajes según la Ley de Demeter? ¿Cuál es su desventaja?
2. ¿Por qué la composición es "caja negra" y la herencia "caja blanca"?
3. Explicá los 4 pasos del Program to Interface con el ejemplo del Equipo.
4. ¿Qué principio viola un Automóvil que se lava solo? ¿Cómo se arregla?
5. Enunciá DIP (las dos premisas).
6. ¿Qué principio se viola si una interfaz obliga a Autopista a implementar `sacarCD()`?
7. ¿Por qué Persona ← Empleado, Cliente es un mal ejemplo de herencia según LSP?

<details><summary>Ver respuesta</summary>

1. Métodos del propio objeto O, de los parámetros de M, de los objetos creados dentro de M y de los componentes directos de O. Desventaja: hacen falta muchos métodos envoltorio (wrappers).
2. En la herencia la subclase **ve** la implementación del padre (rompe el encapsulamiento). En la composición el objeto contenido se usa solo por su interfaz y sus detalles quedan ocultos.
3. (1) Ver qué métodos necesita Equipo de los jugadores (getDeporte, jugar). (2) Crear la interfaz Atleta con esos métodos. (3) Que cada Jugador implemente Atleta. (4) Que Equipo dependa de Atleta: agregarJugador(Atleta).
4. SRP. Se crea la clase LavaderoAuto con lavar(Automóvil).
5. A: los módulos de alto nivel no deben depender de los de bajo nivel; ambos deben depender de abstracciones. B: las abstracciones no deben depender de los detalles; los detalles deben depender de las abstracciones.
6. ISP (Segregación de Interfaces).
7. Porque es herencia de **datos** (solo se reutilizan atributos), no de **comportamiento**. Un Cliente no es sustituible por un Empleado en lo que hace. La herencia correcta es la de comportamiento (Empleado ← EmpleadoProduccion/EmpleadoComercial redefiniendo calcularSueldo()).
</details>

---

# 3. PATRONES DE DISEÑO

### La idea en una frase
> Un patrón de diseño es una **solución probada, con nombre, a un problema de diseño que se repite**. Es independiente del lenguaje y de escala media: refina un subsistema sin cambiar la arquitectura.

> 🎯 **LOS 5 PATRONES QUE SE EVALÚAN** (diapositiva "Patrones que se pueden evaluar"): **1. State · 2. Strategy · 3. Observer · 4. Adapter · 5. Iterator**. En la tabla de clasificación de la diapositiva aparecen **resaltados en color**. Esos 5 hay que saberlos de memoria (propósito, estructura, participantes y consecuencias), y también saber dibujarlos.

## 3.1 Conceptos generales
**Los patrones de diseño:**
- **Refinan un subsistema o componente** de un sistema.
- Describen una estructura a la que **recurrimos muchas veces** para resolver problemas de diseño.
- Son **patrones de escala media**.
- **No afectan la arquitectura** del sistema.
- **No dependen del lenguaje** de implementación.
- Permiten resolver problemas complejos y **direccionan la cooperación efectiva entre componentes**.

**Patrón vs. Idioma**: la diferencia es el **alcance**. Los dos describen soluciones a problemas recurrentes, pero:
- en el **patrón** el problema y la solución son lo bastante **genéricos** como para ser **independientes del lenguaje**;
- el **idioma** es un patrón de **bajo nivel**, **específico de un lenguaje** de programación.

**Elementos principales de un patrón (4)**: **Nombre, Problema, Solución** (descripción abstracta) y **Consecuencias**.

**Plantilla de definición (GoF)**

| Sección | Qué contiene |
|---|---|
| Nombre | — |
| **Propósito** | ¿Qué hace? ¿Qué problema de diseño aborda? |
| Sinónimos | Otros nombres |
| **Motivación** | Escenario que ilustra el problema y cómo lo resuelve el patrón |
| **Aplicabilidad** | ¿En qué situaciones se aplica? ¿Cómo se reconocen? |
| **Estructura** | Diagrama de clases |
| **Participantes** | Clases que participan y sus responsabilidades |
| **Colaboraciones** | Diagramas de interacción |
| **Consecuencias** | ¿Cómo logra sus objetivos? Compromisos, costos, beneficios, alternativas |
| Implementación | Técnicas, consejos, cuestiones que dependen del lenguaje |
| Código de ejemplo | — |
| Usos conocidos | — |
| Patrones relacionados | — |

## 3.2 ¿A qué ayudan los patrones? (6)
1. **Encontrar los objetos**: es difícil, porque hay que pensar en encapsulamiento, flexibilidad, dependencias, reutilización, extensibilidad y rendimiento. Hay **clases de análisis** y **clases de diseño**. Las clases de diseño **no tienen contrapartida en el dominio** (son **Fabricación Pura**) y son clave para que el sistema sea flexible.
2. **Determinar la granularidad** de los objetos: Facade, Flyweight, Builder, Visitor.
3. **Especificar interfaces**: Memento, Decorator, Proxy.
4. **Especificar la implementación** de clases: muchos patrones dependen de la diferencia entre **herencia de implementación** y **herencia de interfaces** (Observer, State, Chain of Responsibility). **"Programar hacia la interfaz, no hacia la implementación"**: no declarar variables de clases concretas sino abstractas. Los patrones de creación permiten basar el sistema en interfaces.
5. **Favorecer la reutilización**: delegación; "**favorecer la composición sobre la herencia de clases**". "Se suele abusar de la herencia. Los diseños suelen ser más reutilizables si dependen de la composición." Herencia y composición trabajan juntas.
6. **Diseñar para el cambio**: la clave de la reutilización es **anticiparse a los cambios**. Cada patrón permite que **un aspecto de la estructura varíe independientemente** de los demás, y eso facilita el reuso interno, la extensibilidad y el mantenimiento.

**Herencia vs. Realización (diapositiva de patrones)**

| Herencia | Realización (interfaz) |
|---|---|
| Se define estáticamente | Se resuelve en tiempo de ejecución |
| Hay reuso de código (caja blanca) | No hay reuso de código |
| Rompe el encapsulamiento | No rompe el encapsulamiento |
| Es ampliamente aceptada la herencia simple | Una clase concreta puede implementar todas las interfaces que necesite |

> **Por qué cada patrón aparece con dos estructuras, "Herencia" y "Realización"**: el mismo patrón se puede dibujar con una **clase abstracta** (las subclases heredan, flecha continua con triángulo) o con una **interfaz** (las clases implementan, flecha punteada con triángulo). La idea es la misma.

## 3.3 Clasificación GoF
- **Creación**: **abstraen el proceso de creación** de objetos y hacen al sistema independiente de cómo se crean, componen y representan.
  - *De clase*: usan **herencia** para variar la clase que se instancia.
  - *De objeto*: **delegan** la instanciación a otro objeto.
  - **Encapsulan** qué clases concretas usa el sistema y cómo se crean sus instancias. El sistema **solo conoce las clases abstractas**. Dan flexibilidad en **qué** se crea, **quién** lo crea, **cómo** y **cuándo**.
- **Estructura**: determinan **cómo combinar objetos y clases** para formar estructuras complejas: **comunicar dos clases incompatibles** o **añadir funcionalidad** a objetos.
- **Comportamiento**: se ocupan de los **algoritmos y el reparto de responsabilidades**, y describen la **comunicación** entre objetos. Permiten:
  - definir abstracciones de algoritmos (Template Method),
  - cooperaciones entre objetos que **reducen dependencias** (Iterator, Observer),
  - asociar comportamiento a objetos e invocar su ejecución (Command, Strategy).

| Ámbito \ Propósito | Creación | Estructura | Comportamiento |
|---|---|---|---|
| **Clase** | Factory Method | **Adapter** | Interpreter, Template Method |
| **Objeto** | Abstract Factory, Builder, Prototype, Singleton | **Adapter**, Bridge, Composite, Decorator, Facade, Flyweight, Proxy | Chain of Responsibility, Command, **Iterator**, Mediator, Memento, **Observer**, **State**, **Strategy**, Visitor |

> Truco para recordar: **Adapter es el único que aparece en los dos ámbitos** (de clase y de objeto).

**Propósito de cada patrón GoF (una línea, según la diapositiva)**
- **Abstract Factory**: interfaz para crear **familias** de objetos relacionados sin especificar sus clases concretas.
- **Factory Method**: interfaz para crear un objeto, pero **las subclases deciden** qué clase instanciar.
- **Singleton**: **una única instancia** con un punto de acceso global.
- **Builder**: separa la **construcción** de un objeto complejo de su **representación**.
- **Prototype**: crea objetos **copiando** una instancia prototípica.
- **Adapter**: **convierte la interfaz** de una clase en la que esperan los clientes.
- **Bridge**: desacopla una **abstracción de su implementación** para que varíen de forma independiente.
- **Composite**: **árboles todo-parte**; trata uniformemente objetos individuales y compuestos.
- **Decorator**: **añade responsabilidades dinámicamente**; es una alternativa a la herencia.
- **Façade**: **interfaz unificada de alto nivel** para un subsistema.
- **Flyweight**: maneja eficientemente **muchos objetos de grano fino** compartiéndolos.
- **Proxy**: un **sustituto** que controla el acceso a otro objeto.
- **Chain of Responsibility**: pasa la petición por una **cadena** de receptores hasta que uno la atiende.
- **Command**: **encapsula una petición en un objeto** (encolar, registrar, deshacer).
- **Interpreter**: representa una **gramática** y un intérprete de sentencias.
- **Iterator**: **recorre** un agregado sin exponer su representación.
- **Mediator**: objeto que **encapsula cómo interactúan** otros objetos.
- **Memento**: guarda el **estado interno** para restaurarlo, sin violar el encapsulamiento.
- **Observer**: dependencia **uno a muchos**; cuando uno cambia, se **notifica** a todos.
- **State**: el objeto **cambia su comportamiento cuando cambia su estado**.
- **Strategy**: **familia de algoritmos intercambiables**.
- **Template Method**: **esqueleto de un algoritmo** en el que las subclases redefinen pasos.
- **Visitor**: **nueva operación** sobre una estructura sin cambiar las clases de sus elementos.

> **Caso de estudio de los ejemplos: "Venta Digital"**: un sitio que vende artículos digitales (Libro, Película, Música, JuegoVideo), con Catálogo, Pedido, DetallePedido, Cliente, TarjetaCrédito, Envío, etc.

---

## 3.4 ⭐ STATE (Estado) – Comportamiento, de objeto

### La idea en una frase
> Cuando el comportamiento de un objeto depende de su estado, **cada estado se convierte en una clase**. El objeto **delega** en su estado actual, y así se eliminan los `if/switch` gigantes.

**Problema que resuelve**
- Manejar los estados con **condicionales** se vuelve inmanejable cuando se agregan más estados y más comportamientos que dependen de ellos.
- Casi todos los métodos terminan con **condicionales monstruosos** que eligen qué hacer según el estado actual.
- Es muy difícil de mantener: **cualquier cambio en la lógica de transición puede obligar a cambiar los condicionales de cada método**.

**Propósito**: permite a un objeto **cambiar su comportamiento cuando cambia su estado interno**. **El objeto parece cambiar de clase.**

**Motivación (GoF)**: una **conexión TCP** puede estar en varios estados (establecida, escuchando, cerrada) y responde distinto a `abrir()`, `cerrar()` o `establecerConexión()` según el estado en que esté.

**Aplicabilidad**
- El comportamiento del objeto **depende de su estado** y debe cambiar **en tiempo de ejecución** según ese estado.
- Las operaciones tienen **grandes estructuras CASE** que dependen del estado del objeto, representado por una o más **constantes enumeradas**.

**Estructura (dibujala así)**
```
  ┌──────────────┐  +estado   ┌──────────────┐
  │  Contexto    │◇──────────►│   Estado     │  (abstracta o interfaz)
  │ solicitud()  │            │ operacion()  │
  └──────────────┘            └──────────────┘
   nota: solicitud(){               △
     estado.operacion() }     ┌─────┴──────────┐
                     ┌────────┴──────┐  ┌──────┴────────┐
                     │EstadoConcreto1│  │EstadoConcreto2│
                     │ operacion()   │  │ operacion()   │
                     └───────────────┘  └───────────────┘
```

**Participantes (genéricos y del ejemplo)**

| Participante | Responsabilidad | Ejemplo Venta Digital |
|---|---|---|
| **Contexto** | Recibe la petición del cliente y la **delega** en su estado actual; guarda la referencia al estado | **Pedido** (recibe `agregarArticulo()`) |
| **Estado (abstracto)** | Define los **métodos abstractos** para cada comportamiento que depende del estado | **EstadoPedido** («abstract») |
| **Estados concretos** | Cada uno implementa el comportamiento **propio de ese estado** | **EnCarroDeCompras, PendienteDePreparación, Confirmado, EnPreparación, Despachado, Cancelado, Finalizado, Preparado** |

**Ejemplo de clase: máquina de estados de Pedido**
```
●→ EnCarroDeCompras ──confirmarPedido()──► Confirmado ──preparar()──► EnPreparacion ──despachar()──► Despachado ──entregar()──► Finalizado →◉
        │                                  │  ▲ deshacerPreparacion()│ (preparar() se repite)
        └────cancelar()──► Cancelado ◄──cancelar()──────────────────┘
```
- En la estructura de diseño, **Pedido** tiene `estado: EstadoPedido` y métodos como `agregarArticulo(), cancelar(), confirmarPedido(), despachar(), entregar(), setEstado(nuevo)`.
- **EstadoPedido** (abstracta) declara **todos** los métodos.
- **Criterio de diseño de la diapositiva**: en EstadoPedido **todos los métodos tienen un comportamiento por defecto**. Los métodos que devuelven `boolean` devuelven `false` y los que devuelven una clase de Estado devuelven `NULL`. Cada estado concreto **redefine solo lo que puede hacer**. Por ejemplo, `EnCarroDeCompra.confirmarPedido(): Confirmado` y `Confirmado.cancelar(): Cancelado`.
- **Las transiciones**: el estado concreto **crea el siguiente estado** y se lo devuelve al Pedido, que hace `setEstado(nuevo)`.

**Dinámica (CU "Agregar artículo al carro de compras")**
1. El **GestorPedido** le pide al **Pedido** `agregarArticulo(art, cant, precio)`.
2. El **Pedido delega** la petición en su estado actual: `EnCarroDeCompra.agregarArticulo(art, c, precio, pedido)`.
3. El estado concreto resuelve: crea `new()` **PendienteDePreparacion** (el estado del detalle), `crearDetalle()` → `new DetallePedido(...)` y le devuelve el detalle al Pedido (`setDetallePedido(dt)`).

**Consecuencias**
- ✅ **Coloca todo el comportamiento de un estado en una clase**.
- ✅ **Subclases en lugar de sentencias CASE** (polimorfismo).
- ✅ **Ayuda a evitar estados inconsistentes**.
- ✅ Las **transiciones de estado son más explícitas**.
- ❌ **Aumenta el número de objetos/clases**. Posible solución: **los objetos estado pueden compartirse** (por ejemplo, con Flyweight o Singleton).

**Ventajas (según la aplicación)**: se **eliminan los CASE usando polimorfismo** y se **permite que crezca** la lógica de negocio que varía según los estados.

---

## 3.5 ⭐ STRATEGY / POLICY (Estrategia) – Comportamiento, de objeto

### La idea en una frase
> Hay **varias formas (algoritmos) de hacer lo mismo**. Cada una va a su propia clase con una interfaz común, y el contexto usa la que esté configurada. Se pueden **intercambiar** sin tocar al cliente.

**Analogía de la diapositiva**: el **Contexto** es el estadio/equipo; la **Estrategia** es el DT con su pizarra; las **estrategias concretas** son Ataque, Defensa, Tiro Libre y Contragolpe.

**Propósito**: define una **familia de algoritmos**, **encapsula cada uno** y los hace **intercambiables**. Permite que el algoritmo **varíe independientemente** de los clientes que lo usan.

**Motivación (GoF)**: hay muchos algoritmos para **justificar texto** (dividir en líneas). ¿Debe implementarlos todos el cliente que los necesita? No: cada uno va en una estrategia.

**Aplicabilidad**
- **Configurar una clase** con uno de **varios comportamientos** posibles.
- Se necesitan **distintas variantes de un algoritmo**.
- Una clase define muchos comportamientos que aparecen como **sentencias CASE** en sus métodos.

**Estructura**
```
  ┌───────────────────┐ +Estrategia ┌────────────────────┐
  │   Contexto        │◇───────────►│ Estrategia /       │
  │ interfaceContexto()│            │ IEstrategia        │
  └───────────────────┘             │ algoritmoInterface()│
                                    └────────────────────┘
                                         △ (herencia o realización)
                                ┌────────┴────────┐
                           EstrategiaA        EstrategiaB
```

**Ejemplo de clase: Liga de Fútbol, CU "Generar Diagramación del Campeonato"**
- **Problema**: la cantidad de **fechas** y de **partidos** se calcula distinto según el sistema de campeonato.
  - **Todos contra todos**: fechas = cantidad de clubes − 1; partidos = cantFechas × (clubes / 2).
  - **Eliminatorias**: se calculan las rondas, con otra fórmula.
  - Sin el patrón, eso son `if (sistema == "todos contra todos") … else …` dentro del Gestor.
- **Solución**:

| Participante | Responsabilidad | En el ejemplo |
|---|---|---|
| **Estrategia** (interfaz) | Declara la interfaz **común a todos los algoritmos** | **IEstrategiaSistemaDeCampeonato**: `calcularCantFechas(club[])`, `calcularCantPartidos(cantFechas, club[])`, `contarClubes(club[])` |
| **Estrategias concretas** | Implementan cada variante del algoritmo | **EstrategiaEliminatorias**, **EstrategiaTodosContraTodos** |
| **Contexto** | Usa las estrategias. **Mantiene la referencia** a una o más estrategias concretas, **configurables en tiempo de ejecución**. Es responsable de **establecer la estrategia** y de **ejecutar los métodos polimórficos**. La configuración debe hacerse **antes** de invocar el método | **SistemaCampeonato** (tiene `crearEstrategia(): IEstrategiaSistemaDeCampeonatos`) |

- **Dinámica**: `GestorDiagramacionCampeonato.determinarCantFechas()` → `Campeonato.calcularCantFechas(club[])` → `SistemaCampeonato.calcularCantFechas(club[])` → `crearEstrategia()` → `new EstrategiaTodosContraTodos` → `calcularCantFechas(club[])`, que devuelve `club.Size - 1`. Lo mismo pasa con `calcularCantPartidos`, que devuelve `cantFechas * (club.Size/2)`.

**Procedimiento para aplicar Strategy (diapositiva)**
1. Identificar los métodos cuyo algoritmo tiene **distintas variantes** (`calcularCantFechas()`, `calcularCantPartidos()`).
2. Definir la **interfaz Estrategia** con la interfaz común.
3. Poner en el **Contexto** la **referencia** a la estrategia abstracta.
4. Definir **cómo se configura** el Contexto con la estrategia deseada.
5. Implementar una **EstrategiaConcreta** por cada variante.

**Consecuencias**
- ✅ Define una **familia de algoritmos relacionados**.
- ✅ Es una **alternativa a crear subclases** de la clase Contexto.
- ✅ **Elimina sentencias CASE**.
- ⚠️ El **cliente puede elegir** entre distintas estrategias, pero **debe conocer los detalles** de cada una para elegir.
- ❌ **Aumenta el número de objetos**. Solución: usar **Flyweight**.
- ℹ️ **State y Strategy son similares; lo que cambia es el propósito.**

## 3.6 ⭐⭐ State vs. Strategy (lo pueden preguntar seguro)

| | **State** | **Strategy** |
|---|---|---|
| Diagrama de clases | **IGUAL** | **IGUAL** |
| Propósito | Cambiar el comportamiento **según el estado interno** | Elegir **un algoritmo** entre varios |
| ¿Quién elige la clase concreta? | Las transiciones, controladas por **los propios estados** o por el **contexto** | Normalmente **el cliente o el contexto la configura** una vez |
| ¿Cambia sola durante la vida del objeto? | **Sí**: el objeto pasa de un estado a otro | En general **no**: se configura y se usa |
| ¿Las clases concretas se conocen entre sí? | Sí: un estado crea el siguiente | No: son independientes |

Lo que dice la diapositiva "State/Strategy":
- State permite a un objeto tener **varios comportamientos** basados en su **estado interno**.
- A diferencia de una máquina de estados **procedural**, **State representa cada estado como una clase completa**.
- El **contexto obtiene su comportamiento delegando** en el estado actual, del que **está compuesto**.
- **Encapsular cada estado en una clase localiza los cambios**.
- **Tienen el mismo diagrama de clases, pero distinta intención.**
- **Strategy normalmente configura** la clase contexto con **un comportamiento o algoritmo**.
- Las **transiciones** pueden controlarlas las clases **estado** o la clase **contexto**.
- State **aumenta el número de clases**.
- Las clases estado **pueden compartirse** entre instancias de contexto.

---

## 3.7 ⭐ OBSERVER / PUBLISH-SUBSCRIBE – Comportamiento, de objeto

### La idea en una frase
> Un objeto (el **sujeto**) tiene una **lista de suscriptos** (los **observadores**). Cuando cambia, **les avisa a todos** sin saber quiénes son concretamente: solo conoce su interfaz.

**Analogía**: un escenario con un orador (sujeto) y un público con binoculares (observadores) que reacciona cuando el orador cambia algo.

**Propósito**: define una **dependencia uno-a-muchos** entre objetos, de modo que cuando **cambia el estado de un objeto**, **todos sus dependientes son notificados y actualizados automáticamente**.

**Motivación**
- ¿Cómo mantener la **consistencia** entre objetos relacionados **sin acoplamiento fuerte** entre sus clases?
- **Ejemplo: separación Modelo-Vista.** Una **tabla de datos** (el modelo: ventas Este/Oeste/Norte por trimestre) se muestra en **varias vistas** (gráfico de barras, gráfico de torta). Si cambia un dato (por ejemplo, t3 = 90), **todas las vistas se actualizan**.

**Aplicabilidad**
- Cuando un **cambio en un objeto requiere cambiar otros** y **no se sabe cuántos** son.
- Cuando un objeto debe **notificar a otros sin hacer suposiciones sobre quiénes son**.

**Estructura**
```
 ┌──────────────────┐ observadores  ┌──────────────────┐
 │ Sujeto / ISujeto │ 1 ─────── 1..*│Observador/IObserv│
 │ adjuntar(o)      │──────────────►│ actualizar()     │
 │ separar(o)       │               └──────────────────┘
 │ notificar()      │  nota: notificar(){ para todo o     △
 └──────────────────┘   en observadores: o.actualizar() } │
          △                                    ┌──────────┴─────────┐
 ┌──────────────────┐  +theSujetoConcreto      │ ObservadorConcreto │
 │ SujetoConcreto   │◄─────────────────────────│ estadoObservador   │
 │ estadoSujeto     │                          │ actualizar()       │
 │ getEstado()      │   nota: estadoObservador = sujeto.getEstado()
 │ setEstado()      │
 └──────────────────┘
```

**Colaboración (diagrama de secuencia genérico)**
```
:Otra ──1. setEstado()──► :Sujeto ──1.1 notificar() (a sí mismo)
                          :Sujeto ──1.1.1 actualizar()──► o1:Observador
                          :Sujeto ──1.1.2 actualizar()──► o2:Observador
```

**Ejemplo de clase (Venta Digital: notificar cambios de estado del Pedido por mail)**

| Participante | Responsabilidad | En el ejemplo |
|---|---|---|
| **Sujeto** (interfaz) | Interfaz para **suscribir, quitar y notificar** a los observadores | **ISujetoNotificacionPedido**: `observador: IObserverNotificacionPedido[]`, `notificar()`, `quitar(o)`, `suscribir(o)` |
| **Observador** (interfaz) | Interfaz para **actualizar** a los objetos que deben enterarse de los cambios | **IObserverNotificacionPedido**: `enviarMail(p: Pedido)` (PULL) o `actualizar(nroPed, fecha, email, detalle, texto)` (PUSH) |
| **Sujeto concreto** | **Guarda el estado** que les importa a los observadores y **les notifica** los cambios | **GestorPedido**, **GestorEnvio** |
| **Observadores concretos** | Mantienen su estado **consistente** con el del sujeto; implementan `actualizar()` | **NotificacionConfirmacionPedido**, **NotificacionDespachoRetrasado**, **NotificacionEnvioParcial** |

> Este ejemplo **combina Observer con State**: el cambio de estado del Pedido (EnCarroDeCompra → Confirmado) es lo que dispara la notificación.

### PULL vs. PUSH (las dos variantes)
| | **PULL ("tirar")** | **PUSH ("empujar")** |
|---|---|---|
| ¿Qué manda el sujeto? | Solo **avisa** que hubo un cambio (y pasa una referencia: `enviarMail(p: Pedido)`) | **Manda los datos** en la notificación: `actualizar(nroPed, fecha, email, detalle, texto)` |
| ¿Quién busca los datos? | El **observador va al sujeto o al pedido a pedir** lo que necesita (`getNumero()`, `listarArticulosPedido()`) | Nadie: el observador **ya los recibe** |
| Ventaja | El observador pide solo lo que necesita; interfaz más simple | El observador no tiene que conocer al sujeto |
| Desventaja | El observador tiene que deducir qué cambió | El sujeto tiene que saber qué datos necesitan los observadores |

**Dinámica PULL (diapositiva)**
1. `PantallaCrearPedido.tomarConfirmacion()` → `GestorPedido.confirmarPedido()` → `Pedido.confirmarPedido()` → se delega en el estado `EnCarroDeCompra.confirmarPedido(p)`, que hace `new()` **Confirmado** → `Pedido.setEstado(nuevo)`.
2. El **GestorPedido crea** un `NotificacionConfirmacionPedido` (`new()`) y lo **suscribe**: `suscribir(o)`.
3. `GestorPedido.notificar()` → `enviarMail(p: Pedido)` → el observador ejecuta `generarMail()` y **va al Pedido a pedir** `getNumero()` y `listarArticulosPedido()`.

> Nota de la diapositiva: *"El GestorPedido crea un objeto que implementa la InterfazNotificacion para notificar vía e-mail al cliente la confirmación del pedido, y lo suscribe. Luego el observador concreto de tipo NotificaciónConfirmaciónPedido, cuando lo desea, va al pedido a requerir su nuevo estado (sus datos)."*

**Dinámica PUSH**: es igual hasta `notificar()`, pero después el GestorPedido llama a `actualizar(nroPed, fecha, email, detalle, texto)`, y el observador solo hace `generarMail()`.
> Nota: *"Tras producirse el cambio de estado del Pedido, se notifica y se envían datos forzosamente a los observadores suscriptos al sujeto GestorPedido. Luego los observadores disponen de esos datos para realizar sus operaciones."*

**Consecuencias**
- ✅ Permite **modificar independientemente** sujetos y observadores.
- ✅ **Acoplamiento abstracto y mínimo** entre Sujeto y Observador:
  - se pueden **reutilizar por separado**;
  - se pueden **agregar observadores sin modificar el sujeto**;
  - el sujeto **no necesita conocer las clases concretas** de los observadores.
- ✅ **Comunicación broadcast**: el sujeto no especifica al receptor concreto, y se pueden **agregar o quitar observadores en cualquier momento**.
- ❌ **Actualizaciones inesperadas**: un cambio chico puede provocar una cascada de actualizaciones.
- ❌ Si la interfaz del observador es simple, **el observador tiene que deducir qué cambió**.

**Consecuencias de la aplicación (diapositiva)**: **Reusabilidad**, **Flexibilidad** (agregar o quitar observadores sin afectar al sujeto ni a los otros, porque el sujeto siempre ejecuta `notificar()` para todos), **menor acoplamiento** (la relación es abstracta, entre las clases "madre" o interfaces) y **consistencia** (aplicando **Template Method**, se puede hacer que `notificar()` se llame **al final** de la actualización del sujeto y nunca antes).

**GestorDeCambios** (para actualizaciones complejas)
- Cuando la semántica de las actualizaciones es compleja, un objeto mantiene las relaciones: el **GestorDeCambios**.
- Su propósito es **minimizar el trabajo** necesario para que los observadores reflejen el cambio del sujeto.
- Responsabilidades: **asociar cada sujeto con sus observadores**, **definir una estrategia de actualización** y **actualizar a todos los observadores** cuando lo pide el sujeto.
- **El GestorDeCambios es un Mediator**, y como hay **uno solo**, también es un **Singleton**.

---

## 3.8 ⭐ ADAPTER / WRAPPER (Adaptador / Envoltorio) – Estructura, de clase y de objeto

### La idea en una frase
> Es un **"enchufe adaptador"**: tenés una clase (muchas veces de terceros) que hace lo que necesitás pero **con una interfaz incompatible**, y ponés en el medio una clase que **traduce** de una interfaz a la otra.

**Dibujos de la diapositiva**: la valija del viajero lleva adaptadores de enchufe, y un auto se sube a las vías con un "car-to-rail adapter".

**¿Qué problema resuelve? (ejemplo de la diapositiva)**
- Una aplicación recibe datos de bolsa de un **Stock Data Provider** en **XML** y quiere usar una **Analytics Library** de terceros que **solo acepta JSON**. Directamente no se puede (✗).
- **Solución**: se agrega un **"XML to JSON Adapter"** en el medio. Las **Core Classes** siguen trabajando en XML, el adaptador convierte, y la librería recibe JSON.
- Con piezas de rompecabezas: "Your Existing System" y "Vendor Class" no encajan. El **Adapter** implementa la interfaz que esperan tus clases y le habla al vendor con la suya. Tu sistema: **sin cambios**; vendor: **sin cambios**; adapter: **código nuevo**.

**Propósito**: **convertir la interfaz** de un objeto para que otro pueda entenderla. El adaptador **"envuelve"** a uno de los objetos para **ocultar la complejidad de la conversión**, y el objeto envuelto **no sabe** que se está haciendo una conversión.

**Ámbito**: **de clases y de objetos** (es el único patrón con los dos).

**Aplicabilidad**
- Cuando queremos usar una **clase existente** pero **su interfaz no es compatible** con el resto del código.
- Cuando queremos crear una **clase reutilizable** que coopere con clases **no relacionadas o no previstas** (con interfaces no necesariamente compatibles).
- Cuando queremos usar **varias subclases existentes** pero **no es práctico adaptar su interfaz heredando de cada una**.
- Como patrón estructural, su objetivo tiene que ver con la **forma en que los objetos interactúan** entre sí.

**Implementación (4 pasos)**
1. **Identificar a los actores**: quién es el cliente y quiénes son las partes a adaptar.
2. **Diseñar una interfaz común**, que conozca el cliente y que todos los futuros adaptados puedan implementar.
3. **Crear un adaptador** por cada parte a adaptar, que implemente esa interfaz. Cada adaptador **contiene una instancia** de la clase adaptada y **"fuerza" su uso** para que se ajuste a la nueva interfaz.
4. **Desacoplar al cliente** de la clase concreta: ahora recibe cualquier objeto que implemente la nueva interfaz.

### Adaptador de OBJETO (usa **composición**; es el más usado)
```
 ┌─────────┐       ┌──────────────────────┐
 │ Cliente │──────►│ <<interface>>        │
 └─────────┘       │ Objetivo (Target)    │
                   │ metodo()             │
                   └──────────────────────┘
                              △ (realización)
                   ┌──────────────────────┐ adaptado  ┌────────────────────┐
                   │ Adaptador            │──────────►│ Adaptable(Adaptee) │
                   │ metodo(){            │           │ metodoEspecifico() │
                   │  adaptado.metodoEsp..}│           └────────────────────┘
                   └──────────────────────┘
```
- Implementa la interfaz de un objeto y **envuelve al otro**. Se puede implementar **en todos los lenguajes**.

**Ejemplo de clase: Venta Digital, CU "Autorizar Venta con Tarjeta de Crédito"**
- **Problema**: integrarse con los **sistemas de autorización de pago de cada marca de tarjeta** (Visa, Mastercard…). Hay que manejar los mensajes de **solicitud financiera (venta)** y de **reversa (anulación)** respetando el estándar **ISO 8583**. Cada marca tiene **su propia API**, distinta de las otras.
- **Arquitectura**: los componentes AutorizacionVenta y AnulacionVenta envían «transacción» al **Agente Intercambio Transacciones Financieras**, que manda `Reqto_Financiero` y `Reqto_Reversa` a MarcaTarjeta1…N.
- **CU**: Cliente → *05 Confirmar pedido* «include» *10 Autorizar Venta con Tarjeta de Crédito* → actor **Administradora de Tarjeta de Crédito (ATC)**.
- **En el análisis** había una boundary `AutorizadorTarjetaDeCredito` (autorizarVentaTC, iniciarPedidoAutorizacionVta, validarTC). **En el diseño** esa boundary se convierte en el patrón:

| Participante | Responsabilidad | En el ejemplo |
|---|---|---|
| **Cliente** | Contiene la **lógica de negocio** | **GestorAutorizaciónVenta** |
| **Objetivo** (interfaz) | La interfaz que el cliente sabe usar | **IAutorizadorTarjetaDeCrédito** (`validarTC(numeroTarjeta, fechaVto, control, montoVenta, cantidadCuotas)`, `convertirPedidoAutorizacion()`, `convertirRespuestaAutorizacion()`) |
| **Adaptable** | Clase, API o servicio **de terceros** con interfaz **incompatible** | **ServicioAutorizadorOperacionesTarjetaVisa** (`autorizarVenta(...)`), **ServicioAutorizacionTarjetaMastercard** (`autorizarVentaMastercard(...)`) |
| **Adaptador** | Implementa la interfaz del cliente **envolviendo al adaptable**; **traduce** la llamada | **AutorizadorTarjetaDeCreditoVisa**, **AutorizadorTarjetaDeCreditoMastercard** («adapter») |

- **Dinámica de diseño**: `GestorAutorizacionVenta.obtenerMontoVenta()` → `validarTC(...)` al **AutorizadorTarjetaDeCreditoVisa** → este hace `convertirPedidoAutorizacion()` → `autorizarVenta(...)` al **ServicioAutorizadorOperacionesTarjetaVisa** → `convertirRespuestaAutorizacion(): int`.

**Ventajas (adaptador de objeto)**
- ✅ **SRP**: separa la interfaz o el código de **conversión de datos** de la **lógica de negocio**.
- ✅ **OCP**: se pueden agregar **nuevos adaptadores** (una tarjeta nueva) **sin romper el código del cliente**, siempre que trabajen a través de la interfaz.

**Desventaja**
- ❌ Aumenta la **complejidad general** porque hay que agregar interfaces y clases nuevas. **A veces es más simple cambiar la clase de servicio** para que coincida con el resto del código.

### Adaptador de CLASE (usa **herencia múltiple**)
```
 Cliente ──► Objetivo        Adaptable
               △               △
               └──── Adaptador ┘   (hereda de los DOS a la vez)
```
- El adaptador **hereda las interfaces de ambos objetos al mismo tiempo** y **redefine** el comportamiento para que se entiendan.
- **Participantes**: Cliente, **Objetivo** (define las operaciones que necesita el cliente), Adaptable y **Adapter** (redefine los comportamientos de Objetivo y Adaptable para que se puedan entender).
- **Desventajas**: ❌ **solo es posible en lenguajes con herencia múltiple** (C++ sí; Java y C# no). ❌ La redefinición **solo sirve para un Adaptable concreto**.

---

## 3.9 ⭐ ITERATOR (Iterador) – Comportamiento, de objeto

### La idea en una frase
> Sacar **"cómo recorrer"** una colección **afuera** de la colección, en un objeto iterador que sabe **en qué posición va**. Así se recorre **sin conocer cómo está guardada** por dentro.

**Dibujo de la diapositiva**: un personaje con auriculares (el iterador) recorre uno por uno a personajes con letras unidos por un camino (los elementos).

📘 **Propósito (GoF)**: proporciona un modo de **acceder secuencialmente a los elementos de un objeto agregado sin exponer su representación interna**. *(La diapositiva de clasificación lo define con esas mismas palabras.)*

📘 **También conocido como**: **Cursor**.

📘 **Motivación (GoF)**
- Una **lista** debería permitir acceder a sus elementos **sin exponer su estructura interna**.
- Tal vez queramos **recorrerla de distintas formas**, pero **no queremos llenar la interfaz de Lista** con operaciones para cada recorrido.
- También puede hacer falta **más de un recorrido a la vez** sobre la misma lista.
- **Idea clave**: sacar la responsabilidad de **acceder y recorrer** la lista y ponerla en un **objeto iterador**. El iterador sabe **cuál es el elemento actual**, es decir, qué elementos ya recorrió.
- Operaciones: `Primero()` (pone el actual en el primero), `Siguiente()` (avanza), `HaTerminado()` (¿ya pasé el último?) y `ElementoActual()`.
- Se puede tener un `IteradorListaConFiltro` que solo devuelva los elementos que cumplen un filtro. En el ejemplo de clase, eso es `cumpleFiltro()`.
- **Iteración polimórfica**: con un **Agregado abstracto** y un **Iterador abstracto**, el cliente funciona igual con una Lista que con una ListaConSaltos. El agregado **crea su propio iterador** con `CrearIterador()`, que es un **Factory Method**. Así se conectan **dos jerarquías paralelas**: la de agregados y la de iteradores.

📘 **Aplicabilidad (GoF)**
- Para **acceder al contenido** de un agregado **sin exponer su representación interna**.
- Para permitir **varios recorridos** sobre agregados.
- Para dar una **interfaz uniforme** para recorrer **distintas estructuras agregadas** (**iteración polimórfica**).

**Estructura: "Iterador Externo Polimórfico" (diapositiva, en versión herencia y en versión realización)**
```
 ┌───────────────┐        ┌─────────┐        ┌──────────────────┐
 │ Agregado /    │◄───────│ Cliente │───────►│ Iterador /       │
 │ IAgregado     │        └─────────┘        │ IIterador        │
 │ crearIterador()│                          │ primero()        │
 └───────────────┘                           │ siguiente()      │
        △                                    │ haTerminado()    │
        │                                    │ elementoActual() │
 ┌──────┴─────────┐   crea (new)             └──────────────────┘
 │AgregadoConcreto│- - - - - - - - - - - - - - - - - - △
 │crearIterador(){ │                          ┌───────┴─────────┐
 │ return new      │◄─────────────────────────│IteradorConcreto │
 │ IteradorConcreto│     (conoce su agregado) │ (posición actual)│
 │ (this) }        │                          └─────────────────┘
```

**Participantes (diapositiva + GoF)**

| Participante | Responsabilidad | En el ejemplo |
|---|---|---|
| **Iterador** | Interfaz para **recorrer** los elementos y **acceder** a ellos | **IIterador**: `actual(): Object`, `cumpleFiltro(filtros: Object[]): boolean`, `haTerminado(): boolean`, `primero()`, `siguiente()` |
| **IteradorConcreto** | Implementa el Iterador y **mantiene la posición actual** del recorrido | **IteradorArticulos**, **IteradorSecciones** (atributos `actual: int`, `elementos: Object[]`) |
| **Agregado** | Interfaz para **crear un objeto Iterador** | **IAgregado**: `crearIterador(elementos: Object[]): IIterador` |
| **AgregadoConcreto** | Implementa la creación y **devuelve el iterador concreto adecuado** | **Catálogo**, **SeccionCatalogo** |

📘 **Colaboraciones**: el IteradorConcreto sabe cuál es el objeto actual del agregado y puede **calcular el siguiente**.

**Dinámica (ejemplo "listar nombres de secciones")**
1. `GestorCatalogo` → `Catalogo.listarNombreSecciones()`.
2. El Catálogo hace `crearIterador(elementos)` → `new IteradorSeccion(elem)`.
3. `primero()`.
4. **loop** `[haTerminado() = false]`: `actual()` → `getNombre()` a la `SeccionCatalogo` → `siguiente()`.

Para artículos activos (ejemplo con **JuegoVideo**): `SeccionCatalogo.listarArticulos()` → `crearIterador()` → `new IteradorArticulo(elem, filtros)` → loop: `haTerminado()`, `actual()`, `cumpleFiltro(filtros)` → `estaActivo()` al artículo → `getTitulo()` → `siguiente()`.

**Iterator + Composite**: el catálogo es un **árbol** (Catálogo → Secciones como "Música Internacional" → Subsecciones como "The Beatles" → Artículos como "Let It Be"). Se usan **IteradorCompuesto** (que recorre CompuestoCatalogo) e **IteradorArticulo**, con `GestorCatalogo` y `CompuestoCatalogo` como agregados concretos. El recorrido es **recursivo**: un iterador por nivel.

**Consecuencias (diapositiva)**
- ✅ **Simplifica la interfaz del contenedor**, porque se le sacan los métodos de recorrido.
- ✅ Permite **varios recorridos concurrentes**, ya que cada iterador guarda su propio estado.
- ✅ Soporta **variantes en las técnicas de recorrido**: basta con cambiar de iterador.

📘 **Implementación (GoF), lo más importante**
- **Iterador externo**: el **cliente controla** la iteración (pide `siguiente()` explícitamente). **Es el de la diapositiva.** Es más flexible; por ejemplo, permite comparar dos colecciones.
- **Iterador interno**: el **iterador controla** la iteración y le aplica una operación a cada elemento. Es más fácil de usar, pero menos flexible.
- **Cursor**: el **agregado** define el algoritmo de recorrido y el iterador solo **guarda la posición**.

---

## 3.10 Otros patrones vistos en clase (no evaluables, pero conviene conocerlos)

### Template Method (Método Plantilla) – Comportamiento, de **clase**
- **Propósito**: define el **esqueleto de un algoritmo** en una operación y **difiere algunos pasos a las subclases**. Las subclases redefinen ciertos pasos **sin cambiar la estructura** del algoritmo.
- **Motivación**: es **fundamental para escribir frameworks**. Ejemplo GoF: la clase `Aplicación`, con documentos, tiene `abrirDocumento()` con pasos fijos y pasos que define cada tipo de documento.
- **Estructura**:
  ```
  ClaseAbstracta                       nota: metodoPlantilla(){
  + metodoPlantilla()                     ... operacionPrimitiva1()
  + operacionPrimitiva1()  (abstracta)    ... operacionPrimitiva2() ... }
  + operacionPrimitiva2()  (abstracta)
          △
  ClaseConcreta
  + operacionPrimitiva1()
  + operacionPrimitiva2()
  ```
- **Aplicabilidad**: implementar **las partes fijas** de un algoritmo y dejar que las subclases implementen lo que varía; **factorizar el comportamiento común** en una superclase; **controlar las extensiones** de las subclases (puntos de extensión).
- **Consecuencias**: es una técnica fundamental de **reutilización**. Usa una **estructura de control invertida**, el **"Principio de Hollywood: no nos llames, nosotros te llamaremos"**. El método plantilla invoca: operaciones **concretas** (de la subclase, de clientes o de la clase abstracta), operaciones **abstractas (primitivas)**, **métodos fábrica** (combinado con Factory Method) y **métodos hook** (gancho: comportamiento **por defecto**, que la subclase puede usar o ignorar; pueden estar **vacíos**).
- **Implementación**: **minimizar** la cantidad de métodos a redefinir; **nombrarlos con un prefijo** (por ejemplo, "do"); en C++, los métodos a redefinir son protegidos y virtuales, y el método plantilla no es virtual.
- **Ejemplo (Venta Digital)**: **Articulo** (abstracta) con el método plantilla **`mostrarInfoProducto()`**; primitivas `conocerTraducciones()`, `mostrarInfoEspecifica()`; hooks `getTitulo()`, `conocerProductora()`, `conocerIdiomaOriginal()`. Las clases concretas **Pelicula, Musica, Libro y Juego** implementan las primitivas.

### Singleton (Único) – Creación, de objeto
- **Propósito**: asegurar que una clase tenga **una única instancia** y dar un **acceso global** a ella.
- **Motivación**: la clase se encarga de asegurar que exista una sola instancia. Casos típicos: **caches, cajas de diálogo, preferencias y configuración, logging, manejadores de dispositivos** (impresora, placa de video). Tener más de una instancia causaría funcionamiento incorrecto, sobreuso de recursos o resultados inconsistentes.
- **Estructura**: `Singleton` con atributo **estático** `instancia`, **constructor privado** y método **estático** `getInstancia()` (si no existe, la crea; después la devuelve).
- **Consecuencias**: **acceso controlado** a la única instancia; **evita variables globales**; se puede **generalizar a un número variable** de instancias; se pueden manejar **subclases** (validando que haya una única instancia y que sea la correcta).
- **Ejemplo**: **NumeroPedido**. El GestorPedido le pide al Singleton el próximo número, crea el pedido con `new()` pasándole ese número y después le pide al Singleton que actualice el último número asignado.

### Builder (Constructor) – Creación, de objeto
- **Propósito**: **separar la construcción** de un objeto complejo de **su representación**, para que el **mismo proceso** pueda crear **distintas representaciones**.
- **Motivación**: un traductor de **RTF** a otros formatos. ¿Se puede agregar una conversión nueva sin modificar el traductor?
- **Aplicabilidad**: el algoritmo de creación debe ser **independiente de las partes** y de cómo se ensamblan; el proceso debe permitir **distintas representaciones**.
- **Consecuencias**: permite **cambiar la representación interna** del producto; **separa** el código de construcción del de representación; da **control fino** del proceso.
- **Implementación**: la interfaz Builder debe ser **lo bastante general**; los métodos del Builder **no son abstractos sino vacíos**; los productos **no tienen una clase abstracta común**.
- **Participantes (ejemplo)**: **Constructor** [ConstructorCatalogo], **Constructores concretos** [ConstructorCatalogoWeb, ConstructorCatalogoResumenWeb, ConstructorCatalogoImpreso], **Director** [DirectorConstruccionCatalogo] (dirige la construcción usando la interfaz Constructor) y **Producto** [PantallaCatalogoWeb, PantallaCatalogoResumenWeb, CatalogoImpreso].

### Factory Method (Método de Fabricación / Constructor Virtual) – Creación, de **clase**
- **Propósito**: definir una **interfaz para crear objetos** y **delegar en las subclases la decisión de qué clase instanciar**. No se decide en compilación qué clase se instancia: se confía en que la fábrica devuelva la adecuada.
- **Aplicabilidad**: cuando **no se sabe en tiempo de diseño qué subtipo** se va a necesitar; cuando una clase quiere que **sus subclases especifiquen** los objetos que crea; para crear objetos relacionados por una base común **sin preocuparse por los detalles** de su creación.
- **Implementación**: (1) que todos los productos sigan **la misma interfaz**; (2) agregar un **método fábrica** vacío en la clase creadora, cuyo tipo de retorno sea la interfaz del producto; (3) **reemplazar** los `new` de productos por llamadas al método fábrica; (4) si el método base queda vacío, hacerlo **abstracto**; si no, queda como comportamiento **predeterminado**.
- **Ejemplo**: crear un artículo **sin condicionales** según su tipo. **Producto** [Articulo], **Productos concretos** [Libro, JuegoVideo, Pelicula, Musica], **Creador** [IGestorArticulo], **Creadores concretos** [GestorLibro, GestorPelicula, GestorMusica, GestorJuegoVideo].
- **Ventajas**: evita el acoplamiento entre el creador y los productos concretos; **SRP** (la creación queda en un solo lugar); **OCP** (se agregan productos nuevos sin romper el cliente). **Desventaja**: muchas subclases nuevas. El mejor caso es cuando ya existe una jerarquía de creadores.

### Abstract Factory (Fábrica Abstracta) – Creación, de objeto
- **Propósito**: dar una interfaz para crear **familias de objetos relacionados** o dependientes **sin especificar la clase concreta**.
- **Motivación**: un toolkit de interfaz de usuario que soporta varios "look & feel" (Windows, Motif, X-Windows).
- **Aplicabilidad**: el sistema debe ser **independiente** de cómo se crean sus productos; debe **configurarse para una familia** de productos; los productos de una familia **deben usarse juntos** (y hay que forzar esa restricción); se quiere ofrecer una librería **revelando solo la interfaz**.
- **Consecuencias**: **aísla** a los clientes de las clases concretas; **facilita cambiar de familia**; **favorece la consistencia** entre productos; ❌ es **difícil agregar nuevos tipos de productos**, porque hay que cambiar todas las fábricas.
- **Implementación**: las **fábricas suelen ser Singleton**; se necesita una subclase por familia; se puede usar **Prototype**.
- **Ejemplo**: **IFabricaPais** → **FabricaArgentina, FabricaChile**; productos abstractos **ICalendario, IInfoPersonal** → **CalendarioArg, CalendarioChile, InfoPersonalArg, InfoPersonalChi**; cliente **GestorCliente** (formato de fecha y máscara de teléfono según el país).

### Composite (Compuesto) – Estructura, de objeto
- **Propósito**: componer objetos en **estructuras de árbol** para representar jerarquías **todo/parte**, de modo que los clientes **traten igual** a los objetos simples y a los compuestos.
- **Motivación**: figuras compuestas por figuras; paquetes de valores (acciones y bonos). En clase también se dio un **ejemplo geográfico**: País → Provincias → Localidades.
- **Estructura**: **Componente** (interfaz común: `operacion()`, `agregar()`, `quitar()`, `obtenerHijo()`) ← **Hoja** (sin hijos) y **Compuesto** (tiene una colección de Componentes y delega en ellos recursivamente).
- **Aplicabilidad**: representar jerarquías todo/parte; que los clientes **ignoren la diferencia** entre compuestos e individuales.
- **Consecuencias**: composición **recursiva**; los clientes tratan **uniformemente** a todos; es **fácil agregar nuevos componentes**; ❌ **no se puede confiar en el sistema de tipos** para restringir qué contiene un compuesto, así que hacen falta comprobaciones en tiempo de ejecución.
- **Implementación**: referencias al **padre**; **maximizar la interfaz Componente**; declarar las operaciones de hijos implica un **equilibrio entre seguridad y transparencia**.
- **Ejemplo**: **IComponenteCatalogo**; **Hoja** = Articulo (Libro, Película, Música, JuegoDeVideo); **Compuesto** = CompuestoCatalogo; **Cliente** = GestorCatalogo. `getDescripcionJerarquia()` arma todo el árbol. En la hoja, `agregarComponente()`, `quitarComponente()` y `obtenerHijo()` están **VACÍOS**. Así el gestor obtiene cada elemento y su ruta **sin saber cuántos niveles tiene el catálogo**.

### Memento (Recuerdo / Token) – Comportamiento, de objeto
- **Problema**: antes de una operación, guardar una **instantánea** del estado para poder restaurarlo después.
- **Propósito**: **capturar y externalizar el estado interno** de un objeto **sin violar el encapsulamiento**, para poder restaurarlo más tarde.
- **Motivación**: checkpoints, **deshacer**, recuperación de errores, transacciones.
- **Aplicabilidad**: guardar parte del estado para restaurarlo; cuando una interfaz para obtener el estado **rompería el encapsulamiento**.
- **Implementación**: definir el **Creador**; crear el **Memento** con campos que reflejan los del Creador; hacerlo **inmutable** (recibe los datos **una sola vez**, por constructor, **sin setters**); el Creador tiene un método para **producir** mementos y otro para **restaurar**; el **Guardián** sabe cuándo pedirlos, cómo guardarlos y cuándo restaurar.
- **Ejemplo**: guardar las **preferencias del cliente** en el sitio (colores, links). **Creador** [PreferenciasCliente], **Memento** [MementoPreferencia], **Guardián** [Cliente] (contiene todas las instancias de Memento), **Cliente** [GestorPersonalizacionSitioWeb].
- **Ventajas**: instantáneas **sin violar el encapsulamiento**; simplifica al Creador, porque el Guardián mantiene el historial. **Desventajas**: puede consumir **mucha RAM**; el guardián debe rastrear el ciclo de vida del creador para borrar los mementos viejos; en algunos lenguajes es difícil asegurar que **solo el Creador** acceda al Memento.

### Façade (Fachada) – Estructura, de objeto
- **Propósito**: dar **una única interfaz** para un conjunto de interfaces de un subsistema, es decir, una interfaz **de más alto nivel** que facilita su uso.
- **Motivación**: **reducir las dependencias** entre subsistemas. Ejemplo: la clase **Compiler** es la fachada de Scanner, Parser, ProgramNode, ByteCodeStream y ProgramNodeBuilder.
- **Aplicabilidad**: dar una interfaz **simple** a un subsistema complejo; hay muchas dependencias entre los clientes y las clases que implementan una abstracción; arquitectura **en capas**, con una fachada como **punto de entrada a cada capa**.
- **Consecuencias**: facilita el uso del subsistema **ocultando sus componentes**; **acoplamiento débil** (los cambios internos no afectan a los clientes); **no impide** usar directamente las clases del subsistema si se necesita (se elige entre facilidad y funcionalidad).
- **Ejemplo**: **FachadaEnvio**, una **Fabricación Pura** que delega en las clases del subsistema de envíos.

## 3.11 Causas comunes de rediseño → qué patrón usar
| Causa | Patrones |
|---|---|
| Crear un objeto indicando la clase explícitamente | Abstract Factory, Factory Method, Prototype |
| Dependencia de operaciones específicas | Chain of Responsibility, Command |
| Dependencia de plataformas de HW/SW | Abstract Factory, Bridge |
| Dependencia de la representación de los objetos | Abstract Factory, Bridge, Memento, Proxy |
| Dependencia de algoritmos | Builder, **Iterator**, **Strategy**, Template Method, Visitor |
| Acoplamiento fuerte | Abstract Factory, Bridge, CoR, Command, Facade, Mediator, **Observer** |
| Extender funcionalidad con subclases | Bridge, CoR, Composite, Decorator, **Observer**, **Strategy**, **State** |
| No poder cambiar las clases convenientemente | **Adapter**, Decorator, Visitor |

### ⚠️ Ojo
- **Adapter** es **estructural**; los otros 4 evaluables son de **comportamiento**.
- **State y Strategy** tienen el **mismo diagrama**: la diferencia es el **propósito**.
- **Observer PULL**: el observador **va a buscar** los datos. **PUSH**: el sujeto **los manda**.
- **Adapter de objeto** = composición (todos los lenguajes). **Adapter de clase** = herencia múltiple (solo algunos lenguajes).
- En **Iterator**, quien **crea** el iterador es el **agregado** (`crearIterador()`), y eso es un **Factory Method**.

### Autoevaluación (patrones)
1. Dibujá de memoria la estructura de State y nombrá sus participantes con el ejemplo del Pedido.
2. ¿Qué diferencia hay entre State y Strategy si tienen el mismo diagrama?
3. En Strategy, ¿quién es responsable de establecer la estrategia y cuándo?
4. ¿Qué consecuencia negativa comparten State y Strategy? ¿Qué solución propone la cátedra?
5. Explicá la diferencia entre Observer PULL y PUSH con el ejemplo de la notificación de confirmación del pedido.
6. ¿Por qué el acoplamiento entre Sujeto y Observador es "abstracto y mínimo"?
7. ¿Qué es el GestorDeCambios y qué patrones aplica?
8. ¿Qué principios SOLID favorece Adapter y por qué?
9. ¿Por qué el adaptador de clase no se puede usar en Java?
10. Nombrá los 4 participantes de Iterator y sus responsabilidades.
11. ¿Qué tres consecuencias tiene Iterator?
12. ¿Cuál es la diferencia entre patrón e idioma?
13. ¿Qué es un método hook en Template Method?
14. Clasificá los 5 patrones evaluables según propósito y ámbito.

<details><summary>Ver respuesta</summary>

1. Contexto (Pedido) ◇→ Estado (EstadoPedido, abstracta) ← EstadosConcretos (EnCarroDeCompras, Confirmado, EnPreparación, Despachado, Cancelado, Finalizado…). El contexto delega: solicitud(){ estado.operacion() }.
2. El propósito. State cambia el comportamiento según el estado interno, y las transiciones las manejan los estados o el contexto. Strategy configura el contexto con un algoritmo intercambiable, normalmente elegido una vez.
3. El Contexto (SistemaCampeonato) establece la estrategia; debe configurarse antes de invocar el método de variación algorítmica.
4. Aumentan el número de objetos. State: compartir los objetos estado. Strategy: usar Flyweight.
5. PULL: el GestorPedido llama a enviarMail(p: Pedido) y el NotificacionConfirmacionPedido va al Pedido a pedir getNumero() y listarArticulosPedido(). PUSH: el GestorPedido llama a actualizar(nroPed, fecha, email, detalle, texto), le manda todos los datos, y el observador solo genera el mail.
6. Porque el sujeto solo conoce la interfaz Observador, no las clases concretas. Se pueden agregar observadores sin modificar el sujeto y reutilizar ambos por separado.
7. Es un objeto que mantiene la relación entre sujetos y observadores cuando las actualizaciones son complejas: asocia sujeto con observadores, define la estrategia de actualización y actualiza a todos. Es un Mediator y, como es único, un Singleton.
8. SRP: separa el código de conversión de la lógica de negocio. OCP: se agregan adaptadores nuevos sin modificar al cliente.
9. Porque necesita herencia múltiple (heredar de Objetivo y de Adaptable a la vez).
10. Iterador (interfaz para recorrer y acceder), IteradorConcreto (implementa y mantiene la posición actual), Agregado (interfaz para crear el iterador), AgregadoConcreto (crea y devuelve el iterador concreto adecuado).
11. Simplifica la interfaz del contenedor; permite varios recorridos concurrentes; soporta variantes en las técnicas de recorrido.
12. El alcance. El patrón es genérico e independiente del lenguaje; el idioma es de bajo nivel y específico de un lenguaje.
13. Un método con comportamiento por defecto (puede estar vacío) en la clase abstracta, que las subclases pueden redefinir o ignorar.
14. State, Strategy, Observer e Iterator: comportamiento, de objeto. Adapter: estructura, de clase y de objeto.
</details>

---

# 4. DISEÑO DE PERSISTENCIA

### La idea en una frase
> Los objetos viven en memoria **volátil**. Para que sobrevivan hay que guardarlos en memoria **persistente** (una BD), y como casi siempre es una **BD relacional (tablas)**, hay que resolver el **problema de impedancia**: **convertir objetos ⇄ tablas** sin que el resto del sistema se acople a la BD.

## 4.1 Paradigmas
- **Estructurado**: **separación función/dato** (el código está por un lado y los datos por otro, unidos por una "relación") y **bases de datos relacionales**. El dibujo muestra **Código** (módulos > procedimientos > control de flujo > acciones elementales) ⇅ **Datos**.
- **Orientado a Objetos**: **integración** (datos y comportamiento juntos), **reusabilidad** y **trazabilidad**.
- La balanza de la diapositiva muestra que **el diseño mezcla los dos mundos**: programamos en objetos pero guardamos en tablas. La imagen del **pato-conejo** dice lo mismo: es la misma realidad vista de dos formas.

**Repaso de objetos**: una **clase** define un conjunto de objetos con **estructura y comportamiento comunes**. Un **objeto** tiene:
- **Identidad**: ¿qué lo distingue de los demás? (el lugar que se le asigna al instanciarlo);
- **Estado**: ¿qué sabe o tiene? (los valores de sus atributos);
- **Comportamiento**: ¿qué puede hacer? (los métodos de su clase).

**Ejemplo**: la clase `Auto` (marca, modelo, color, dominio, añoFabricación; encenderMotor(), avanzar()…) y el objeto "mi auto" (Chevrolet, Classic, Blanco, MBO056, 2012).

📘 **Persistencia (Jacobson)**: es la capacidad de un objeto de **sobrevivir a la ejecución del programa que lo creó**. Se copia de la **memoria primaria** (rápida y volátil) a la **secundaria** (lenta y persistente). El programador solo quiere una **vista lógica** y deja que el DBMS resuelva el almacenamiento físico. Un DBMS además da **concurrencia**, **recuperación** ante fallas y **facilidad de consulta**.

📘 **¿Qué objetos se persisten?** Sobre todo los **objetos entidad**, porque su información **sobrevive a los casos de uso**. Señales de que hace falta un DBMS: información que debe ser persistente, **más de una aplicación compartiendo** datos, **gran número de instancias**, **búsquedas complejas**, **reportes avanzados**, **manejo de transacciones** de usuario y un registro para el reinicio del sistema.

## 4.2 El Problema de Impedancia
Viene del ejemplo de los transformadores en ingeniería eléctrica: dos mundos que "no encajan".
1. Las tablas usan casi solo **tipos primitivos** (enteros, caracteres…).
2. La información está **en los objetos** (tipos complejos y creados por el usuario), así que hay que **transformar la estructura de objetos en una estructura de tablas**.
3. 📘 En la tabla solo se guardan **datos, no comportamiento**.
4. **Genera un fuerte acoplamiento entre la aplicación y el DBMS**. Por eso, **la menor cantidad posible de partes del sistema debe conocer la interfaz del DBMS**.
5. 📘 Otros problemas: cómo representar la **herencia**, cómo guardar operaciones, la visión de **transacciones** (la de la aplicación frente a la del DBMS), los **bloqueos** en transacciones largas, la distribución y la **integridad** de las referencias entre memoria y BD.

## 4.3 Mapeo de Clases/Objetos a Tablas (5 reglas – MEMORIZAR)
1. **Asignar una tabla para la clase.**
2. **Cada atributo primitivo → una columna.** Si el atributo es **complejo**, se agrega **una tabla adicional** para él o se **distribuye en varias columnas** de la tabla de la clase.
3. **La clave primaria (PK) es el identificador único** de la instancia (el "identificador"). 📘 Conviene que lo **genere el sistema** y que sea **invisible** al usuario, para que los cambios administrativos de claves no lo afecten.
4. **Cada instancia → una fila** de la tabla.
5. **Cada asociación con cardinalidad mayor que 1** (por ejemplo, [0..N]) → **una nueva tabla** que conecta las tablas de los objetos asociados, usando sus PK. 📘 En algunos casos la relación alcanza con **una columna** (FK) en la tabla del objeto asociado.

📘 **Normalización**: un buen modelo de objetos (cada objeto con un identificador único y atributos independientes) suele quedar **en 3FN de entrada**, porque "la realidad ya está normalizada". Si la performance es mala, se puede usar **indexado** o **desnormalizar**. La ventaja OO es que **la redundancia queda encapsulada dentro del objeto**, en un solo lugar.

## 4.4 Relaciones entre clases que requieren persistencia
- **Asociación**: relación **estructural** que indica que los objetos de un elemento se **conectan** con los de otro. Tiene **multiplicidad** y **navegabilidad**. Ejemplo: `Gasto ──1──► TipoDeGasto`.
  - **Agregación**: tipo especial de asociación; relación **conceptual "todo/parte"**. Ejemplo: `Factura ◇──1..*── DetalleFactura`.
  - **Composición**: variante de la agregación con **fuerte pertenencia** y **vidas coincidentes** (si muere el todo, mueren las partes). Ejemplo: `Estantería ◆── Estante`.
- **Generalización**: relación entre un elemento **general** (superclase) y uno **más específico** (subclase). El hijo puede **agregar** estructura y comportamiento o **modificar** el del padre. Ejemplo: `CuentaBancaria ← CuentaCorriente, CuentaDeAhorro` (y CuentaBancaria puede heredar de ElementoConInterés y de ElementoAsegurable).

### Opciones para mapear la Asociación, Agregación y Composición
| Multiplicidad | Cómo se mapea |
|---|---|
| **1 a 1** | FK en una de las dos tablas |
| **1 a Muchos / Muchos a 1** | **FK en la tabla del lado "muchos"** (por ejemplo, cada `DetalleFactura` tiene la FK de su `Factura`) |
| **Muchos a Muchos** | **Tabla intermedia** con las PK de ambas tablas (regla 5) |

### Opciones para mapear la HERENCIA (3) ⭐
Ejemplo de clase (Infracciones de Tránsito): **Persona** (apellido, dni, fechaNac, nombre, sexo) ← **Conductor** y **AutoridadConstatacion** (legajo, nroPlaca).

| Opción | Cómo queda | Ventajas | Desventajas |
|---|---|---|---|
| **1. Simular la herencia** | **Una tabla para el padre y una para cada hijo.** Los hijos tienen una **FK al padre**. En el ejemplo: `Persona (*PK dni, apellido, nombre, fechaDeNacimiento, sexo)`, `AutoridadConstatación (*PK legajo, FK dni)`, `Conductor (*PK idConductor, FK dni)` | **Menos redundancia**. Un cambio en los atributos comunes se hace en un solo lugar. 📘 Es la que **recomienda Jacobson** | Para armar un objeto hace falta un **JOIN** (es más lenta). 📘 Si el identificador es común, puede haber problemas: con Persona ← Estudiante y Profesor, una persona no podría ser las dos cosas |
| **2. Eliminar el padre** | **Solo tablas de los hijos**, y cada una **copia los atributos heredados**. No hay tabla para la clase abstracta | 📘 **Más rápida**: no hay joins | 📘 La BD crece, las columnas heredadas se **duplican**, y si cambia un atributo heredado hay que tocar **todas** las tablas hijas. Hace falta otra tabla para saber qué tipos hay |
| **3. Eliminar los hijos** | **Una sola tabla del padre** con **todos** los atributos de todos los hijos, más una columna que indique el **tipo** | Simple, sin joins | Muchas columnas **nulas** (un Conductor no tiene nroPlaca); se pierde la semántica de los tipos |

> 📘 **Jacobson**: "No hay una mejor solución para todos los casos". Depende de **cómo se usan las tablas en los casos de uso** y de qué búsquedas se necesitan. En general se recomienda la de **tener tabla para la clase abstracta** (simular). Conviene **un identificador común** para todas las tablas o **claves generadas por el sistema**.

### Ejemplo integrador: Caso de Infracciones de Tránsito (Policía Caminera)
- **Modelo de Dominio (entidades)**: Conductor, Licencia, Vehículo, Modelo, Marca, OrganismoEmisor, Localidad, Provincia, País, ActaConstatacion, EstadoActa, Infraccion, InfraccionNomenclada, AutoridadConstatacion, Ruta, TipoDeRuta.
- **DER resultante del mapeo (cómo quedó)**:
  - Cada clase pasó a ser **una tabla** con **PK** (`idConductor`, `numero` de Licencia, `numeroActa`, `dominio` del Vehículo, `IdModelo`, `IdMarca`, `IdLocalidad`, `IdProvincia`, `IdPais`, `legajo`, `numeroRuta`, `ID_TipoRuta`, `codigoTipoInfraccion`, `IdEstado`, `IdInfraccion`).
  - Las asociaciones **muchos a 1** se resolvieron con **FK**: `Licencia` tiene `FK idConductor` y `FK IdOrganismoEmisor`; `ActaConstatacion` tiene `FK numero` (licencia), `FK IdEstado`, `FK legajo`, `FK dominio` y `FK numeroRuta`; `Vehiculo` tiene `FK IdModelo`; `Modelo` tiene `FK IdMarca`; `Localidad` tiene `FK IdProvincia`; `Provincia` tiene `FK IdPais`; `Infraccion` tiene `FK numeroActa` y `FK codigoTipoInfraccion`; `Ruta` tiene `FK IdTipoRuta` y `FK numeroActa`.
  - **Los métodos desaparecen**: en el DER solo quedan las columnas «column».

## 4.5 Modelos de Persistencia (2) ⭐

### A) Super Objeto Persistente (Jacobson, OOSE)
- **Idea**: todo objeto que deba persistirse **HEREDA** de un bloque abstracto **ObjectToStore** que ya trae la **funcionalidad de persistencia**. Es un **framework** que después se especializa en cada clase. (Por eso el "Super": como Superman, todos heredan sus poderes.)

```
   ObjectToStore (bloque abstracto / framework)          RDBPort
   ┌───────────────────────────────────────────┐        ┌──────┐
   │ Object                 ObjectState         │        │ Port │
   │ PersistentObjectAdmin  PersistentState     │        └──────┘
   └───────────────────────────────────────────┘
                     △ hereda
   Item de Depósito  (bloque concreto)
   ┌───────────────────────────────────────────┐
   │ Item                   ItemState           │
   │ PersistentItemAdmin    PersistentItemState │
   └───────────────────────────────────────────┘
```

| Clase | Qué hace |
|---|---|
| **Object** | Representa el objeto abstractamente (`Item` es el objeto real del sistema) |
| **ObjectState** | Los **atributos a guardar**, en la estructura de datos del objeto (`ItemState`) |
| **PersistentState** | Esos atributos, pero **en los tipos del DBMS** (`PersistentItemState`) |
| **PersistentObjectAdministrador** | **Convierte** ObjectState en PersistentState y lo guarda usando RDBPort (`PersistentItemAdministrador`) |
| **RDBPort** | **El único que conoce al RDBMS**: genera las sentencias **SQL**. **Encapsula** el DBMS |

📘 **Flujo** (ejemplo de la máquina recicladora, objeto `Lata`): se recibe `sumar` → se incrementa `CantidadDiaria` → se actualiza `CanState` → `Lata` le pide a `PersistentCanAdministrador` que almacene `(CanState)` → este lo **convierte** en `PersistentCanState` → `RDBPort.Print(Table="Lata", PersistentCanState)` → SQL. **Todo esto es invisible desde afuera de Lata.**

📘 **¿Qué hay que cambiar si cambia el DBMS?** Solo **RDBPort**. Si cambian las tablas o su estructura, también PersistentItemAdministrador (sabe los nombres de las tablas) y PersistentItemState (sabe la estructura).

- **Desde la arquitectura**: GUI → **Modelo de Objetos de Negocio** (los objetos heredan la persistencia) → BD Relacional. **La persistencia está dentro de las clases de negocio.**

### B) Esquema de Persistencia (Framework de persistencia)
- **Idea**: la persistencia **NO** está dentro de las clases de negocio. Va en una **capa separada** entre el Modelo de Negocio y la BD. Las clases de negocio **no saben** cómo se guardan.
- **Desde la arquitectura**: **GUI → Modelo de Negocio → [Esquema de Persistencia] → BD Relacional**.
- El dibujo: **Aplicación** (red de objetos Producto, Ítem, Tamaño, Color, Imagen) → **Mapeo** → **Servidor/RDBMS** (tablas Producto, Ítem, Tamaño, Imagen, Tamaño-Color, Color).

### Comparación (la balanza de la diapositiva) ⭐
| **Super Objeto Persistente** | **Esquema de Persistencia** |
|---|---|
| ✅ **Fácil de implementar** | ✅ **Mantiene la cohesión entre las clases** (las de negocio solo hacen negocio) |
| ✅ **No crece la estructura de clases** | ✅ **Soporta extensibilidad** |
| | ✅ **Mejor reusabilidad** |

La balanza se inclina hacia el **Esquema de Persistencia**.

> 💡 **Para entenderlo**: con el Super Objeto, cada clase de negocio **es** persistente (por herencia), así que mezcla dos responsabilidades (negocio y persistencia), lo que va contra **SRP** y **SoC**. Con el Esquema, esa responsabilidad está en otra capa, así que hay **SoC**, más cohesión y la capa es **reutilizable** en otros sistemas.

## 4.6 Bases de Datos Orientadas a Objetos (ODBMS)
**Características (Atkinson et al.)**
- **Objetos complejos**: debe soportar la noción de objetos complejos.
- **Identidad de objetos**: cada objeto tiene una identidad **independiente de sus valores**.
- **Encapsulamiento** de datos y comportamiento.
- **Tipos o Clases** como mecanismo de estructuración.
- **Jerarquía**: debe soportar **herencia**.
- **Ligadura tardía**: sobreescritura y ligadura tardía (polimorfismo).
- **Completitud**: el lenguaje de manipulación puede expresar cualquier función calculable.
- **Extensibilidad**: se pueden **agregar tipos nuevos**.

**Beneficios de usar ODBMS**
- Los **objetos se almacenan como tales** (en general las operaciones no se guardan: están en la librería de clases en memoria).
- **No hace falta convertir tipos**: las clases del usuario son tipos del DBMS.
- El lenguaje del DBMS se **integra con el lenguaje OO**; puede ser **el mismo** que el de la aplicación, así que el programador no tiene dos representaciones de sus objetos.
- 📘 ⇒ **Se elimina completamente el problema de impedancia.**

📘 **¿Cuándo conviene cada una? (Jacobson)**
- **ODBMS**: para **objetos complejos** (CAD, CASE, CAE), porque evita los joins lentos.
- **RDBMS**: para **estructuras simples que se expresan bien en tablas** (bancos con clientes y cuentas, reservas de vuelos), y porque son **más maduros**.
- **No hay una mejor elección general**: hacer **benchmarks** con volúmenes y usos reales.

## 4.7 Frameworks y Framework de Persistencia
**Framework**
- Es un **conjunto extensible de clases e interfaces que colaboran** para dar los servicios de la **parte central e invariable** de un subsistema lógico.
- Contiene clases **concretas** y, sobre todo, **abstractas**, que definen las interfaces a las que hay que ajustarse, las interacciones en las que participar y otras variantes.
- Puede requerir que el usuario defina **subclases** de sus clases para usar, adaptar y extender los servicios.
- Tiene clases abstractas con métodos **abstractos y concretos**.
- Se basa en el **Principio de Hollywood: "No nos llame, nosotros lo llamaremos"**: el framework llama a tu código, no al revés.
- Ofrece un **alto grado de reutilización**, mucho mayor que el de clases individuales.

**Framework de Persistencia: debe proporcionar funciones para**
1. **Almacenar y recuperar objetos** en un mecanismo de almacenamiento persistente.
2. **Confirmar y deshacer (commit y rollback) las transacciones.**

**Arquitectura (diagrama de despliegue de la diapositiva)**: Ventana → CoordinadorDeAplicaciones → Dominio → **EsquemaPersistenciaRelacionalObjetos** → Base de Datos Relacional.

📘 **Conceptos y patrones de un framework de persistencia (Larman, cap. 34)**

| Concepto | Qué es |
|---|---|
| **Representación de objetos como tablas** | Una tabla por clase persistente; atributos primitivos → columnas |
| **Identificador de objeto (OID)** | Clave única para relacionar registros con objetos y evitar duplicados |
| **Fachada de persistencia** | Una interfaz única para los servicios de persistencia (patrón Façade) |
| **Conversor (Mapper) de BD** | Una **Fabricación Pura** responsable de **materializar y desmaterializar** |
| **Materialización** | Transformar un **registro** del almacenamiento en un **objeto** |
| **Desmaterialización** | Lo opuesto: **objeto → registro** (también se le dice *passivation*) |
| **Caché** | Guardar los objetos materializados, para el rendimiento |
| **Materialización perezosa** | No traer un objeto de la BD hasta que se lo necesita, usando un **Proxy virtual** |
| **Estados transaccionales** | Los objetos persistentes pueden estar nuevos, limpios, modificados o eliminados, y la respuesta a `commit`/`rollback` depende del estado: patrón **State** |
| **Command** | Cada operación de la transacción como un objeto, para poder ejecutarla o deshacerla |

### ⚠️ Ojo
- **Super Objeto** = la persistencia **se hereda** y queda **dentro** del negocio. **Esquema** = la persistencia es una **capa aparte**.
- Las 3 opciones de herencia: **Simular** (padre + hijos con FK), **Eliminar el padre** (solo hijos), **Eliminar los hijos** (solo padre).
- **Muchos a muchos → tabla nueva.** Uno a muchos → FK.
- El **Problema de Impedancia** genera un **fuerte acoplamiento** entre la aplicación y el DBMS.
- Las ODBMS **eliminan** la impedancia.

### Autoevaluación (persistencia)
1. ¿Qué es el problema de impedancia? Nombrá 3 aspectos.
2. Enunciá las 5 reglas de mapeo de clases a tablas.
3. Mapeá la herencia Persona ← Conductor, AutoridadConstatación con las 3 opciones.
4. ¿Qué ventajas tiene el Super Objeto Persistente? ¿Y el Esquema de Persistencia?
5. ¿Qué clase del Super Objeto es la única que habla SQL?
6. ¿Qué funciones debe proporcionar un framework de persistencia?
7. ¿Qué es el principio de Hollywood?
8. Nombrá 4 características de una BDOO y 2 beneficios.

<details><summary>Ver respuesta</summary>

1. Es la diferencia entre el mundo de objetos y el mundo de tablas: (a) las tablas usan tipos primitivos; (b) la información está en objetos y hay que transformarla a tablas; (c) genera un fuerte acoplamiento aplicación–DBMS (y además: cómo representar la herencia y que solo se guardan datos, no comportamiento).
2. (1) Una tabla por clase; (2) cada atributo primitivo → columna (el complejo → tabla aparte o varias columnas); (3) la PK es el identificador; (4) cada instancia → fila; (5) cada asociación con cardinalidad > 1 → tabla nueva que conecta.
3. Simular: Persona(PK dni, …), Conductor(PK idConductor, FK dni), AutoridadConstatación(PK legajo, FK dni). Eliminar el padre: Conductor(idConductor, dni, apellido, nombre, fechaNac, sexo) y AutoridadConstatación(legajo, nroPlaca, dni, apellido, …). Eliminar los hijos: Persona(dni, apellido, nombre, fechaNac, sexo, idConductor, legajo, nroPlaca, tipo).
4. Super Objeto: fácil de implementar y no crece la estructura de clases. Esquema: mantiene la cohesión, soporta extensibilidad y tiene mejor reusabilidad.
5. RDBPort.
6. Almacenar y recuperar objetos en un almacenamiento persistente, y confirmar/deshacer (commit/rollback) transacciones.
7. "No nos llame, nosotros lo llamaremos": es el framework (o el método plantilla) el que llama al código del usuario.
8. Objetos complejos, identidad de objetos, encapsulamiento, clases, herencia, ligadura tardía, completitud, extensibilidad. Beneficios: se guardan los objetos como tales, no hay conversión de tipos y el lenguaje es el mismo que el de la aplicación.
</details>

---

# 5. EVOLUCIÓN DEL SOFTWARE

### La idea en una frase
> El software **no termina cuando se entrega**: tiene que **cambiar todo el tiempo** para seguir siendo útil. Ese cambio se hace con **mantenimiento** (cambios del día a día) o con **reingeniería** (rehacer la estructura de un sistema viejo). El **refactoring** es la forma disciplinada y continua de mejorar el código **sin cambiar lo que hace**.

## 5.1 Contexto
- **El enfoque cambió con el tiempo**: antes se hablaba de **"Mantenimiento del Software"**; ahora se habla de **"Evolución del Software"**.
- **Metáfora del iceberg**: se esperaba que el mantenimiento fuera solo lo que se veía, pero **debajo de la superficie hay una masa enorme de problemas y costos**.
  - "A principios de los **años 70** el iceberg era lo bastante grande como para **hundir un portaaviones**."
  - "**Hoy** podría **hundir a toda la armada**."
- 📘 Según Sommerville, entre el **85 y el 90%** de los costos del software organizacional son costos de **evolución** (otros estudios dicen alrededor de **dos tercios**).

**¿Por qué se necesita tanto mantenimiento?**
- Mucho del software del que dependemos tiene **20 años o más**.
- Aunque se haya construido con las mejores técnicas de análisis y diseño (**y en la mayoría no fue así**), se hizo cuando **el tamaño de los programas y el espacio de almacenamiento** eran la preocupación principal.
- Hubo **traslados a nuevas plataformas** y cambios de **hardware y de sistemas operativos**.
- Se incorporaron **mejoras** para satisfacer las necesidades de los usuarios.
- **¡Y todo eso sin tener en cuenta la arquitectura!**

**Razones de todo esto (4)**: **arquitecturas muy mal diseñadas**, **mala codificación**, **lógica inadecuada** y **documentación escasa o inexistente**.

## 5.2 Desarrollo y evolución
- **Modelo espiral de desarrollo y evolución**: Especificación → Implementación → Validación → Operación → (Release 1, Release 2, Release 3…). El desarrollo y la evolución son **un continuo**, no dos procesos separados.
- **Ciclo de vida en la evolución del software**: **Desarrollo Inicial → Evolución ↻ → Servicio ↻ → Retiro Gradual**.
  - 📘 En **Evolución** se hacen cambios importantes (arquitectura y funcionalidad); en **Servicio** solo cambios chicos y tácticos; en **Retiro Gradual** el sistema se sigue usando pero ya no se cambia.
- **Identificación del cambio y procesos de evolución** (ciclo): **Proceso de Identificación del Cambio → Propuestas de Cambio → Proceso de Evolución del Software → Nuevo Sistema →** (vuelve a empezar).

## 5.3 El proceso de evolución del software
```
Peticiones de Cambio → Análisis de Impacto → Planificación de Versiones → Implementación de Cambios → Liberación del Sistema
                                                  ├─ Reparación de Fallas
                                                  ├─ Adaptación de la Plataforma
                                                  └─ Perfeccionamiento del Sistema
```
**Procesos de cambio**
- **Implementación de cambios (normal)**: Peticiones de cambio → **Análisis de requerimientos → Actualización de requerimientos** → Desarrollo de software.
- **Reparación de fallas de emergencia**: Peticiones de cambio → **Análisis del código fuente → Modificación del código fuente** → Entregar el sistema modificado.
  - ⚠️ En la de emergencia **se saltean los requerimientos y el diseño**. Se va directo al código. 📘 Eso **degrada** el sistema y deja la documentación desactualizada.

## 5.4 Estrategias de Evolución del Software
Son 2: **MANTENIMIENTO** y **REINGENIERÍA**. 📘 Al valorar sistemas heredados se agregan **sustituir** y **desechar**.

## 5.5 Tipos de Mantenimiento ⭐

**Cambios en el software** (de dónde salen):
- **Hacer correcciones** → modifican **requerimientos existentes**.
- **Hacer mejoras** → crean **nuevos requerimientos**.
- Cambios que **modifican la instalación sin cambiar los requerimientos**.

**Los 3 tipos de mantenimiento (las 3 ruedas dentadas de la diapositiva)**

| Tipo (nombre de la cátedra / Sommerville) | Nombre "clásico" | Qué es | % del esfuerzo |
|---|---|---|---|
| **Reparación de Fallas** | Correctivo | **Corregir errores**. 📘 Los de **codificación** son baratos; los de **diseño**, más caros (pueden implicar reescribir componentes); los de **requerimientos**, **los más caros** (pueden exigir rediseñar) | **17%** |
| **Adaptación Ambiental** | Adaptativo | Adaptar el software a un **cambio del entorno**: **hardware, sistema operativo, plataforma** u otro software de soporte | **18%** |
| **Adición de Funcionalidad** (incorporación y modificación de funcionalidad) | Perfectivo | Cambios porque **cambian los requerimientos** (cambios organizacionales o del negocio). 📘 Son cambios **mucho más grandes** que los otros | **65%** ⭐ |

- **Preventivo**: es el **Refactoring** (5.10). Mejora la estructura **sin agregar funcionalidad**, para **prevenir** problemas futuros.
- 📘 **En la práctica no hay una distinción clara** entre los tipos: al adaptar a un entorno nuevo se suele agregar funcionalidad, etc. Además, **los nombres varían según el autor**. "Correctivo" es universal, pero "adaptativo" y "perfectivo" se usan con significados distintos. Por eso Sommerville (y la cátedra) prefiere **Reparación de fallas / Adaptación ambiental / Adición de funcionalidad**.

> 💡 **Para recordar el gráfico de torta**: **la mayor parte (65%) NO es arreglar bugs** sino **agregar o cambiar funcionalidad**. Los bugs son solo el 17%.

## 5.6 Costos de mantenimiento
**Gráfico "Costos de desarrollo / mantenimiento" (Sistema 1 vs. Sistema 2)**
- **Sistema 1**: costo de desarrollo ≈ 200 y costo de mantenimiento ≈ 200, total ≈ **400**.
- **Sistema 2**: costo de desarrollo ≈ 175 y costo de mantenimiento ≈ 325, total ≈ **500**.
- 📘 **Qué significa (Sommerville)**: en el Sistema 1 se invirtieron **$25.000 más en el desarrollo** para hacerlo **más mantenible**, y eso **ahorró $100.000** en mantenimiento durante su vida. **Invertir en mantenibilidad al desarrollar es rentable** si se miran todos los costos de vida del sistema (hay un efecto multiplicador). Esa es **la razón para refactorizar**.
- 📘 Igual, las empresas suelen no invertir: el costo extra se **mide hoy** y el ahorro futuro **no se puede medir** al mismo tiempo.

**Factores de costo de mantenimiento (4)**
📘 **Agregar funcionalidad cuando el sistema ya está en operación es más caro que hacerlo durante el desarrollo**, por estas razones:

| Factor | Explicación 📘 |
|---|---|
| **Estabilidad del equipo** | Después de la entrega el equipo **se disuelve**. Los nuevos **no conocen el sistema** ni por qué se tomaron las decisiones de diseño, y pierden tiempo entendiéndolo |
| **Prácticas de desarrollo deficientes** | El contrato de mantenimiento suele estar **separado** del de desarrollo (a veces es otra empresa). El desarrollador **no tiene incentivo** para hacer software mantenible y toma atajos |
| **Habilidades del personal** | El personal de mantenimiento suele ser **inexperto** y no conocer el dominio. El mantenimiento tiene **mala imagen** y se le asigna al personal más novato. Los sistemas viejos pueden estar en **lenguajes obsoletos** |
| **Antigüedad y estructura del programa** | Con los cambios **la estructura se degrada**: cuanto más viejo, más difícil de entender y cambiar. Puede no haberse hecho con técnicas modernas, estar optimizado para eficiencia y no para que se entienda, tener la documentación perdida o no haber tenido gestión de configuración |

📘 Los tres primeros vienen de que las organizaciones **ven el desarrollo y el mantenimiento como actividades separadas**.

**Motivación del personal (cómo mejorar el mantenimiento)**
- Relacionar el desarrollo de software con los **objetivos organizacionales** (mantenimiento racional).
- Relacionar las **recompensas** con la performance organizacional.
- **Integrar el mantenimiento con el desarrollo.**
- Crear un **presupuesto discrecional para mantenimiento preventivo**.
- **Planificar el mantenimiento en las etapas tempranas** del desarrollo.
- Planificar el gasto de esfuerzo en **programas de mantenibilidad**.

## 5.7 Predicción
**Predicción de cambios**
- Predecir cuántos pedidos de cambio habrá requiere **entender la relación entre el sistema y su entorno**.
- Los sistemas **fuertemente acoplados** con su entorno necesitan cambios **cada vez que el ambiente cambia**.
- **Factores** que influyen en esa relación:
  1. **Número y complejidad de las interfaces** del sistema.
  2. **Número de requerimientos volátiles** (los que cambian seguido).
  3. **Los procesos de negocio** que usan el sistema.

**Predicción del mantenimiento**: evaluar **qué partes del sistema pueden causar problemas y costos altos**.
- La **aceptación del cambio** depende de la **mantenibilidad** de los componentes afectados.
- **Implementar cambios degrada el sistema** y reduce su mantenibilidad.
- Los **costos de mantenimiento dependen del número de cambios**, y **los costos de cada cambio dependen de la mantenibilidad**.

**Las 3 predicciones y sus preguntas**

| Predicción | Preguntas |
|---|---|
| **Prediciendo mantenibilidad** | ¿Qué partes del sistema serán más afectadas por pedidos de cambio? ¿Qué partes serán más caras de mantener? |
| **Prediciendo cambios en el sistema** | ¿Cuántos pedidos de cambio pueden esperarse? |
| **Prediciendo costos de mantenimiento** | ¿Cuáles serán los costos de mantenimiento el próximo año? ¿Cuáles son los costos de mantenimiento del ciclo de vida? |

## 5.8 Leyes de Lehman ⭐
Lehman estudió cómo evolucionan los sistemas grandes. Son 8 leyes:

| # | Ley | Descripción | En palabras simples |
|---|---|---|---|
| 1 | **Cambio Continuo** | Un programa que se usa en el mundo real **debe cambiar** o se vuelve **cada vez menos útil** en ese entorno | Si no cambia, se vuelve obsoleto |
| 2 | **Incremento de Complejidad** | Al cambiar, la estructura se vuelve **más compleja**. Hay que **dedicar recursos extra** para preservarla y simplificarla | Cada cambio desordena: hay que ordenar a propósito (refactoring) |
| 3 | **Evolución de Grandes Programas** | Es un proceso **autorregulado**. El tamaño, el tiempo entre releases y el número de errores reportados son **aproximadamente invariantes** en cada release | Los sistemas grandes tienen "su propio ritmo" |
| 4 | **Estabilidad Organizacional** | Durante la vida del programa, su **ritmo de evolución es constante** e **independiente de los recursos** dedicados | Poner más gente no lo acelera |
| 5 | **Conservación de la Familiaridad** | El **cambio incremental en cada release es aproximadamente constante** | Cada versión cambia más o menos lo mismo |
| 6 | **Crecimiento Continuo** | La funcionalidad **tiene que crecer** continuamente para mantener satisfecho al usuario | Siempre hay que agregar cosas |
| 7 | **Disminución de la Calidad** | La calidad **va a disminuir** a menos que se modifique el sistema para reflejar los cambios de su entorno | Si no se adapta, la calidad baja |
| 8 | **Retroalimentación de Sistemas** | Los procesos de evolución son sistemas de **retroalimentación multi-agente y multi-ciclo**, y hay que tratarlos así para lograr una mejora significativa | La evolución es un sistema con realimentación |

**Aplicabilidad de las leyes de Lehman**
- **No está establecida completamente.**
- En general se aplican a **sistemas grandes, a medida, desarrollados para organizaciones grandes**.
- **No está claro** cómo aplicarlas a: **productos de software enlatados**, **sistemas con muchos componentes COTS** (comerciales), **organizaciones pequeñas** y **sistemas de tamaño medio**.

📘 *Explicación de la ley 3 (Sommerville)*: viene de factores **estructurales** (al cambiar y extenderse, la estructura se degrada) y **organizacionales**. Los cambios **chicos** reducen la degradación; los **grandes** tienden a introducir fallas nuevas.

## 5.9 Valoración de un sistema heredado ⭐
📘 **Sistema heredado (legacy)**: es un sistema **antiguo** que todavía es **útil** y a veces **crítico** para la empresa. Puede usar **lenguajes o tecnología obsoletos**, su **estructura está degradada** y su **documentación perdida o desactualizada**. Aun así, **sustituirlo** puede no ser rentable o ser muy riesgoso.

**Matriz Valor de Negocio × Calidad del Sistema**
```
                   Calidad BAJA                 Calidad ALTA
Valor ALTO   │  REINGENIERÍA              │  MANTENIMIENTO
             │  (vale mucho pero está     │  (vale y está bien:
             │   mal hecho → mejorarlo)   │   seguir manteniéndolo)
─────────────┼────────────────────────────┼──────────────────────────
Valor BAJO   │  SUSTITUCIÓN               │  MANTENIMIENTO
             │  (no vale y está mal:      │  (eventualmente SUSTITUCIÓN
             │   reemplazar / desechar)   │   si se elevan los costos)
```
> Truco: **valor alto → nunca se tira**. Si además la calidad es baja → **reingeniería**. **Calidad alta → mantenimiento**. **Valor bajo y calidad baja → sustituir**.

📘 **Las 4 opciones estratégicas (Sommerville)**
1. **Desechar completamente el sistema**: cuando ya **no aporta** a los procesos de negocio (porque esos procesos cambiaron).
2. **Dejar el sistema sin cambios y continuar con el mantenimiento regular**: cuando es **estable** y hay **pocos pedidos de cambio**.
3. **Someterlo a reingeniería para mejorar su mantenibilidad**: cuando la **calidad se degradó** por los cambios y **todavía se proponen cambios nuevos**.
4. **Sustituir todo o parte del sistema**: cuando, por ejemplo, el **hardware nuevo** no permite seguir operando el viejo o hay **sistemas comerciales** que lo reemplazan a un costo razonable. Muchas veces es una **sustitución evolutiva**, por partes.

## 5.10 Reingeniería de Software ⭐
**Fórmula de la diapositiva**
> **Ingeniería Reversa + Δ (cambios) + Ingeniería Delantera = REINGENIERÍA**

| Componente | Qué es |
|---|---|
| **Ingeniería Reversa** | **No es un proceso de cambio**. Solo **analiza** el programa y **extrae información** (documentación, diseño) |
| **Δ (delta)** | **Alteraciones al sistema original** |
| **Ingeniería Delantera (hacia adelante)** | Es el **proceso tradicional de refinamiento de abstracciones** (de la especificación al código) |

**Ingeniería Delantera vs. Reingeniería**
```
Ingeniería Delantera:  Especificación del Sistema ──► Diseño e Implementación ──► Nuevo Sistema
Reingeniería:          Sistema de Software Existente ──► Comprensión y transformación ──► Sistema Re-ingenierizado
```

📘 **Definición (Sommerville)**: someter un sistema heredado a reingeniería para **mejorar su estructura y que se entienda mejor**, y así hacerlo **más fácil de mantener**. Puede implicar **volver a documentarlo, refactorizar su arquitectura, traducirlo a un lenguaje moderno y modificar la estructura y los valores de sus datos**. **La funcionalidad NO cambia** y conviene evitar grandes cambios de arquitectura.

**Reingeniería de software: tópicos (5) y proceso**

| Actividad                                   | Qué hace                                                                                                                                                                 | 📘 Automatización                      |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------- |
| **1. Traducción de código fuente**          | Convierte el programa de un lenguaje viejo a una **versión moderna** del mismo lenguaje o a **otro lenguaje**                                                            | Automática (herramienta de traducción) |
| **2. Ingeniería Reversa**                   | **Analiza** el programa y **extrae información** para **documentar** su organización y funcionalidad                                                                     | Por lo general automática              |
| **3. Mejora de la estructura del programa** | Analiza y modifica la **estructura de control** para que sea más fácil de leer y entender                                                                                | Parcialmente automática                |
| **4. Modularización del programa**          | **Agrupa las partes relacionadas** y **elimina redundancia**; puede incluir **refactorización arquitectónica**                                                           | Manual                                 |
| **5. Re-Ingeniería de datos**               | Cambia los **datos** para reflejar los cambios del programa: **redefine esquemas** de BD, **convierte** las bases y las **limpia** (corrige errores, elimina duplicados) | Con herramientas de ayuda              |

**Diagrama del proceso**
```
 Programa Original ──► Ingeniería Reversa ──► Documentación del Programa
        │
        ├──► Traducción de Código Fuente ──► Mejora de la Estructura del Programa ──► Programa Re-estructurado
        │                                                        │
        │                                          Modularización del Programa ──► Programa con Reingeniería
 Datos Originales ──► Reingeniería de Datos ──► Datos con Reingeniería
```
📘 **No siempre hacen falta todos los pasos**: si se sigue usando el mismo lenguaje, no hace falta traducir; la reingeniería de datos solo hace falta si cambian las estructuras de datos. Para que el sistema interopere con software nuevo se pueden desarrollar **servicios adaptadores** que "envuelvan" al sistema heredado (es el patrón **Adapter** a gran escala).

📘 **Enfoques de reingeniería, de menor a mayor costo**: conversión automatizada del código fuente → reestructuración automatizada con cambios manuales → reestructuración del programa y de los datos → reestructuración más cambios arquitectónicos.

**Ventajas de la reingeniería (frente a desarrollar de cero)**

| Ventaja             | Por qué                                                                                                                                                                                                      |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Reducir riesgos** | Desarrollar software nuevo tiene **alto riesgo**: puede haber problemas de **desarrollo, de personal y de especificación**                                                                                   |
| **Reducir costos**  | El costo de la reingeniería es **significativamente menor** que el de un desarrollo nuevo. 📘 Ejemplo de Ulrich: reimplementar costaba **USD 50 millones** y la reingeniería se hizo con **USD 12 millones** |

**Factores de costo de la reingeniería (4)**
1. La **calidad del software** al que se le va a aplicar.
2. La **herramienta de soporte** disponible.
3. La **extensión de la conversión de datos** que se requiere.
4. La **disponibilidad de personal experto**.

**Factores de complejidad (para evaluar el programa)**
- **Complejidad de control**: se mide mirando las **sentencias condicionales**.
- **Complejidad de datos**: la de las **estructuras de datos** y los componentes de **interfaces**.
- **Tamaño de los módulos.**
- **Longitud de los identificadores**: **nombres más largos implican más legibilidad**.
- **Comentarios del programa**: tal vez **más comentarios** signifiquen un mantenimiento más fácil.

📘 **Modelo de reingeniería de Pressman (6 actividades, cíclico)**: **Análisis de inventario** (lista de aplicaciones por importancia, edad y mantenibilidad, para elegir candidatas) → **Reestructuración de documentos** → **Ingeniería inversa** → **Reestructuración de código** → **Reestructuración de datos** → **Ingeniería hacia adelante**. Consejo: aplicar **Pareto**, es decir, hacer reingeniería del **20%** del software que causa el **80%** de los problemas.

## 5.11 Refactoring (Refactorización) ⭐
**¿Qué es?**
- Es una **forma disciplinada de introducir cambios reduciendo la posibilidad de introducir defectos**.
- Es una transformación del software que:
  - **preserva la estructura externa** (el comportamiento observable), y
  - **mejora la estructura interna**.

**Son cambios que:**
- **No cambian el comportamiento observable**: **todas las pruebas siguen pasando**.
- **Eliminan la duplicación** o la **complejidad innecesaria** (DRY, KISS).
- **Mejoran la calidad** del software.
- Hacen que el código sea **más simple y más fácil de entender**.
- **Flexibilizan** el código.
- Hacen que el código sea **más fácil de cambiar**.

**¿Por qué es necesario?**
- Historieta de la diapositiva: el ingeniero arranca con "terreno limpio, bases sólidas, esta vez lo hago bien", y "mucho después" tiene una ciudad de casitas colgadas y caóticas: "¡Ay, lo hice de nuevo!".
- Para:
  - **Evitar el "deterioro del diseño"** (ley 2 de Lehman).
  - **Limpiar el desorden** del código.
  - **Simplificar** el código.
  - **Aumentar la legibilidad y la comprensibilidad.**
  - **Encontrar errores.**
  - **Reducir el tiempo de depuración.**
  - **Incorporar lo que aprendemos** sobre la aplicación.
  - Porque **rehacer las cosas es fundamental en todo proceso creativo**.

**¿Cómo hacer refactoring? (ciclo de 5 pasos)**
```
1. Encontrar código que "huela mal"
        ↓
2. Asegurarse de que las pruebas pasen
        ↓
3. Determinar cómo simplificar el código
        ↓
4. Hacer las simplificaciones
        ↓
5. Ejecutar las pruebas para asegurarse de que sigan pasando ──► (volver a 1)
```

📘 **Malos olores (bad smells, Fowler, citado en Sommerville)**
1. **Código duplicado** → se elimina o se pasa a un único método.
2. **Métodos largos** → se dividen en varios más cortos.
3. **Sentencias switch/case** → se reemplazan por **polimorfismo**. ¡Es justo lo que hacen **State y Strategy**!
4. **Aglomeración de datos** (el mismo grupo de datos repetido en muchos lugares) → se encapsula en un objeto.
5. **Generalidad especulativa** (código "por si algún día") → se elimina (**YAGNI**).

**Refactoring en el contexto del desarrollo y la evolución del software**
- **Refactoring y TDD**: 📘 el **Desarrollo Dirigido por Pruebas** (Test-Driven Development, de Beck y XP) **entrelaza las pruebas y el código**: se desarrolla en incrementos chicos, cada uno con su prueba, y no se avanza hasta que la prueba pasa. El ciclo es: **identificar la nueva funcionalidad → escribir la prueba → correrla (falla) → implementar la funcionalidad y refactorizar → correr la prueba (pasa) → siguiente**. **Las pruebas son la red de seguridad** que permite refactorizar sin miedo, porque si siguen pasando, no se rompió nada.
- **Refactoring como mantenimiento preventivo**: 📘 refactorizar es **mejorar un programa para frenar la degradación** que producen los cambios. **No se agrega funcionalidad**: uno se concentra en mejorar el programa. Por eso es **"mantenimiento preventivo"**: **reduce los problemas de los cambios futuros**. Es inherente a los métodos ágiles (XP).

## 5.12 ⭐⭐ Reingeniería vs. Refactoring (pregunta clásica)
| | **Reingeniería** | **Refactoring** |
|---|---|---|
| ¿Cuándo? | **Después** de mantener un sistema por un tiempo, cuando **los costos de mantenimiento aumentaron** | **Continuamente**, durante todo el desarrollo y la evolución |
| ¿Sobre qué? | **Sistemas heredados** completos | El código **actual**, en partes chicas |
| ¿Cómo? | Con **herramientas automatizadas** (traducción, ingeniería inversa…) | **Cambios pequeños** guiados por **pruebas** |
| Alcance | Grande: puede abarcar **lenguaje, datos y arquitectura** | Chico e **incremental** |
| Objetivo | **Crear un sistema nuevo más mantenible** a partir del viejo | **Evitar** que la estructura se degrade (**preventivo**) |
| ¿Cambia la funcionalidad? | **No** | **No** (el comportamiento observable se mantiene) |
| En común | Las dos buscan que el software sea **más fácil de entender y de cambiar** | ← |

### ⚠️ Ojo
- Los porcentajes: **17% fallas, 18% adaptación, 65% funcionalidad**.
- La **ingeniería reversa NO cambia** el sistema: solo extrae información.
- La **ingeniería delantera** es el proceso **tradicional** (especificación → diseño → código).
- La **reparación de emergencia** va **directo al código** y se saltea los requerimientos.
- **Valor alto + calidad baja = reingeniería.** **Valor bajo + calidad baja = sustitución.**
- **Refactoring = mantenimiento preventivo**, sin cambiar el comportamiento, con las pruebas pasando.

### Autoevaluación (evolución)
1. Nombrá los 3 tipos de mantenimiento con su porcentaje. ¿Cuál es el mayor?
2. ¿Cuáles son los 4 factores de costo de mantenimiento? Explicá uno.
3. ¿Qué factores influyen en la predicción de cambios?
4. Enunciá las leyes de Cambio Continuo, Incremento de Complejidad y Disminución de Calidad.
5. ¿A qué sistemas se aplican las leyes de Lehman y a cuáles no está claro?
6. Un sistema tiene alto valor de negocio pero baja calidad: ¿qué estrategia aplicás? ¿Y con bajo valor y baja calidad?
7. Completá: Ingeniería ______ + ______ + Ingeniería ______ = Reingeniería, y explicá cada parte.
8. Nombrá los 5 tópicos de la reingeniería.
9. ¿Cuáles son las 2 ventajas de la reingeniería?
10. Nombrá los 4 factores de costo de la reingeniería y 3 factores de complejidad.
11. Definí refactoring y nombrá los 5 pasos para hacerlo.
12. ¿Qué diferencia hay entre reingeniería y refactoring?
13. ¿Por qué el refactoring se considera mantenimiento preventivo?
14. ¿Qué muestra el gráfico Sistema 1 vs. Sistema 2?

<details><summary>Ver respuesta</summary>

1. Reparación de fallas (17%), Adaptación ambiental (18%), Adición/modificación de funcionalidad (65%, el mayor).
2. Estabilidad del equipo, Prácticas de desarrollo deficientes, Habilidades del personal, Antigüedad y estructura del programa. Ejemplo: estabilidad del equipo, porque cuando se entrega el sistema el equipo se disuelve y los nuevos tienen que gastar tiempo en entenderlo.
3. El número y la complejidad de las interfaces, el número de requerimientos volátiles y los procesos de negocio que usan el sistema.
4. Cambio continuo: un programa usado en el mundo real debe cambiar o se vuelve menos útil. Incremento de complejidad: al cambiar, la estructura se vuelve más compleja y hacen falta recursos extra para simplificarla. Disminución de calidad: la calidad baja salvo que se adapte a los cambios del entorno.
5. A sistemas grandes, a medida, de organizaciones grandes. No está claro para software enlatado, sistemas con muchos COTS, organizaciones pequeñas y sistemas medianos.
6. Alto valor y baja calidad: reingeniería. Bajo valor y baja calidad: sustitución.
7. Ingeniería Reversa (no es un proceso de cambio: analiza y extrae información) + Δ (alteraciones al sistema original) + Ingeniería Delantera (el proceso tradicional de refinamiento de abstracciones).
8. Traducción de código fuente, Ingeniería reversa, Mejora de la estructura del programa, Modularización del programa, Reingeniería de datos.
9. Reducir riesgos y reducir costos.
10. Costo: calidad del software, herramientas de soporte, extensión de la conversión de datos, personal experto. Complejidad: de control, de datos, tamaño de los módulos, longitud de los identificadores, comentarios.
11. Es una forma disciplinada de introducir cambios reduciendo la posibilidad de defectos, que preserva el comportamiento externo y mejora la estructura interna. Pasos: encontrar código que huela mal → asegurarse de que las pruebas pasen → determinar cómo simplificar → simplificar → volver a correr las pruebas.
12. La reingeniería es puntual, grande, sobre sistemas heredados, con herramientas automáticas, y se hace cuando los costos de mantenimiento ya subieron. El refactoring es continuo, chico, incremental y guiado por pruebas, y evita la degradación. Ninguno de los dos cambia la funcionalidad.
13. Porque no agrega funcionalidad: mejora la estructura para frenar la degradación y reducir los problemas de los cambios futuros.
14. Que invertir un poco más en el desarrollo para hacer el sistema mantenible (Sistema 1) reduce mucho el costo de mantenimiento y el costo total. En el ejemplo de Sommerville, 25.000 más en desarrollo ahorraron 100.000 en mantenimiento.
</details>

---

# 🔁 REPASO DE 10 MINUTOS (leer el día del parcial)

**Proceso de Diseño**
- Diseño = **modelo lógico → modelo físico**.
- **6 aspectos**: **Arquitectura** (en el centro; resuelve los **RNF**), Datos, Procesos (CU de análisis → CU de diseño con **patrones**), UX, Formas de E/S (la de entrada solo en **batch**) y Procedimientos manuales.
- **Elaboración** → arquitectura estable; **Construcción** → plano para implementar.

**Principios**
- Todos buscan **alta cohesión + bajo acoplamiento**.
- **DRY** (un único lugar), **SoC** (capas), **YAGNI** (no "por si acaso"), **KISS** (simple), **Demeter** (solo amigos: O, parámetros, creados en M, componentes de O), **Tell don't ask**, **Program to Interface** (depender de abstracciones), **Composición > Herencia** (delegación: reuso de objetos y caja negra).
- **SOLID**:
  - **S**: una razón para cambiar.
  - **O**: abierto a extensión, cerrado a modificación; encapsular lo que varía.
  - **L**: los subtipos sustituyen al tipo base.
  - **I**: no depender de métodos que no se usan.
  - **D**: depender de abstracciones (alto y bajo nivel); los detalles dependen de las abstracciones.

**Patrones evaluables**

| Patrón       | Tipo                    | En una línea                                                                                      | Ejemplo de clase                                    |
| ------------ | ----------------------- | ------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| **State**    | Comp./objeto            | El objeto cambia su comportamiento según su estado; cada estado es una clase y el contexto delega | Pedido / EstadoPedido                               |
| **Strategy** | Comp./objeto            | Familia de algoritmos intercambiables; el contexto usa la configurada                             | SistemaCampeonato / Eliminatorias, TodosContraTodos |
| **Observer** | Comp./objeto            | 1 a muchos: el sujeto notifica a los suscriptos (pull o push)                                     | GestorPedido / Notificaciones por mail              |
| **Adapter**  | Estruct./clase y objeto | Convierte una interfaz incompatible (de objeto = composición; de clase = herencia múltiple)       | AutorizadorTarjetaDeCreditoVisa                     |
| **Iterator** | Comp./objeto            | Recorre un agregado sin exponer su estructura; el agregado crea el iterador                       | Catálogo / IteradorSeccion, IteradorArticulo        |

**Persistencia**
- **Impedancia** = objetos vs. tablas, que genera acoplamiento con el DBMS.
- **5 reglas**: tabla por clase, atributo → columna, PK = identificador, instancia → fila, asociación > 1 → tabla nueva.
- **Herencia**: simular / eliminar el padre / eliminar los hijos.
- **Super Objeto** (se hereda la persistencia; fácil y no crece la estructura) vs. **Esquema** (capa aparte; cohesión, extensibilidad y reusabilidad).
- **Framework**: almacenar/recuperar + commit/rollback; principio de **Hollywood**.
- Las **ODBMS** eliminan la impedancia.

**Evolución**
- Mantenimiento → **Evolución**; iceberg.
- **Tipos**: fallas 17%, adaptación ambiental 18%, **funcionalidad 65%**.
- **Factores de costo**: estabilidad del equipo, malas prácticas, habilidades del personal, antigüedad y estructura.
- **Lehman**: cambio continuo, incremento de complejidad, autorregulación, estabilidad organizacional, familiaridad, crecimiento continuo, disminución de la calidad, retroalimentación.
- **Matriz**: valor alto + calidad baja → **reingeniería**; valor bajo + calidad baja → **sustitución**; calidad alta → **mantenimiento**.
- **Reingeniería** = Ing. Reversa + Δ + Ing. Delantera. Tópicos: traducción, ingeniería reversa, mejora de estructura, modularización, reingeniería de datos. Ventajas: **menos riesgo y menos costo**.
- **Refactoring** = cambiar la estructura interna **sin cambiar el comportamiento** (las pruebas pasan); es **mantenimiento preventivo**; va de la mano de **TDD**.
