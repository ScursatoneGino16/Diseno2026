# Modelo de Parcial 3 N.º 2 (estilo Lovay): RESPUESTAS

> ⚠️ No abrir hasta terminar el modelo.

## Punto 1

| # | Correctas | Por qué son falsas las demás |
|---|---|---|
| 1 | **A, C** | B: eso describe el **análisis** · D: el diseño pertenece al dominio de la **solución** |
| 2 | **B, C, D** | A: las formas de entrada solo hacen falta en sistemas **batch** |
| 3 | **B, C** | A: también hay que cumplir los requisitos **implícitos** · D: el diseño busca una abstracción sin atarse a una tecnología |
| 4 | **A, B, D** | C: hay que **minimizar** la distancia intelectual |
| 5 | **C** | A: el arquitecto se ocupa de los modelos de diseño y despliegue y de la arquitectura · B: el ingeniero de componentes se ocupa de clases, subsistemas e interfaces · D: la descripción de la arquitectura es del **arquitecto** |
| 6 | **A, C, D** | B: el de objetos es de **estructura** |
| 7 | **A, C** | B: es el contraejemplo de la clase (mezcla temas) · D: SoC se aplica en **los dos** niveles |
| 8 | **A, B, D** | C: concentrar la lógica afuera del objeto es justamente "Ask" |
| 9 | **B, C** | A: depende de clases concretas · D: abusar de las interfaces es una trampa; se usan cuando hay varias implementaciones o para desacoplarse de un sistema externo |
| 10 | **B, C, D** | A: arrancar y detener son responsabilidades propias del auto |
| 11 | **B** | A: es herencia de **datos**, el contraejemplo · C: una hija no puede violar el contrato del padre · D: heredar atributos no garantiza la sustitución del comportamiento |
| 12 | **B, C, D** | A: ISP propone interfaces chicas, no "implementar la mayor cantidad posible" |
| 13 | **A, B** | C: los **detalles** dependen de las abstracciones · D: es un absoluto; las clases concretas existen, solo que no se depende de ellas |
| 14 | **A, B, C** | D: los patrones **no** reemplazan a SOLID; se apoyan en esos principios |
| 15 | **D** | A: el idioma es **específico** de un lenguaje · B: los patrones son de **escala media** · C: los 4 elementos son nombre, **problema, solución y consecuencias** |
| 16 | **A, B, D** | C: según el criterio de la cátedra, la clase abstracta da un comportamiento **por defecto** y cada estado redefine **solo lo que puede hacer** |
| 17 | **A, C** | B y D: son las afirmaciones falsas típicas sobre Strategy |
| 18 | **A, C, D** | B: la política la **elige el administrador** (el cliente), no cambia sola; por eso es Strategy |
| 19 | **B, C** | A: eso describe **PUSH** · D: la dependencia es **uno a muchos**, y es el sujeto el que notifica |
| 20 | **A, B, C** | D: el sujeto conoce a los observadores **solo por la interfaz** |
| 21 | **A, D** | B: el Adapter **no modifica** la clase de terceros · C: el adaptador de clase requiere **herencia múltiple** (Java y C# no la tienen) |
| 22 | **B, C, D** | A: el patrón existe justamente para **no exponer** la estructura interna |
| 23 | **A, C, D** | B: eso describe **Command**; Memento guarda una "foto" del estado para poder deshacer |
| 24 | **A, B** | C: en una BD relacional se usan **claves foráneas** (las referencias en memoria son de los objetos) · D: el paradigma estructurado separa datos y funciones |
| 25 | **A, D** | B: simular la herencia **sí** requiere JOINs · C: eliminar el padre **duplica** los atributos comunes |
| 26 | **A** | B: el Súper Objeto **viola** SRP · C: eso describe el **Esquema** de Persistencia · D: la cátedra se inclina por el **Esquema** |
| 27 | **A, B, D** | C: al mantenimiento suele ir el personal **menos** experimentado |
| 28 | **A, B** | C: el ritmo es **constante**; más recursos no lo aceleran · D: para esos casos **no está claro** que se apliquen |
| 29 | **B, C, D** | A: el sistema (3) tiene bajo valor y baja calidad, así que corresponde **sustitución** |
| 30 | **A, C, D** | B: son cambios chicos y continuos con pruebas; eso es refactoring, no una reestructuración profunda de un sistema heredado |

**Distribución:** 4 preguntas con 1 correcta, 11 con 2 y 15 con 3.

---

## Punto 2: qué tiene que tener la respuesta (guía de corrección)

1. **Impedancia, definición:** es la dificultad de compatibilizar el modelo de objetos con las estructuras tabulares de una BD relacional.
2. **Impedancia, causas:**
   - Las tablas solo manejan tipos primitivos.
   - No guardan comportamiento.
   - No admiten multivalores ni herencia.
   - Los objetos se relacionan por referencias y las tablas por FK.
   - **Consecuencia:** genera un fuerte acoplamiento entre la aplicación y el DBMS. Se reduce con un ORM y con interfaces entre dominio y persistencia (DIP).
3. **Ejemplo propio:** un modelo de clases con herencia y con una asociación 1 a N. Por ejemplo:
   - `Cliente` 1 —— * `Cuenta`
   - `Cuenta` (nro, saldo) ← `CuentaCorriente` (limiteDescubierto) y `CajaAhorro` (tasaInteres)
4. **a) Asociación 1 a N:** la **FK va en la tabla del lado "muchos"**. Por ejemplo, `Cuenta(nro PK, saldo, idCliente FK)`. Conviene mencionar las reglas de mapeo: una tabla por clase, atributo → columna, objeto → fila y PK = identificador.
5. **b) Las tres alternativas para la herencia, con sus tablas:**
   - **Simular la herencia:** `Cuenta(nro PK, saldo)`, `CuentaCorriente(nro PK/FK, limiteDescubierto)` y `CajaAhorro(nro PK/FK, tasaInteres)`. 👍 Menor redundancia y la más fiel al modelo. 👎 Requiere JOINs.
   - **Eliminar el padre:** `CuentaCorriente(nro, saldo, limiteDescubierto)` y `CajaAhorro(nro, saldo, tasaInteres)`. 👍 Sin JOINs. 👎 Atributos duplicados: un cambio en un atributo común obliga a tocar todas las tablas.
   - **Eliminar los hijos:** `Cuenta(nro, saldo, limiteDescubierto, tasaInteres, tipo)`. 👍 Simple y rápida. 👎 Muchas columnas en NULL.
6. **c) Modelo de persistencia:** conviene elegir el **Esquema de Persistencia**, porque:
   - La persistencia queda en una capa aparte y las clases de negocio no saben cómo se guardan. Respeta **SRP/SoC** y mantiene la cohesión.
   - Se pueden agregar clases sin modificar las de dominio, gracias al framework de persistencia como capa de abstracción. Respeta **OCP**.
   - El dominio depende de abstracciones y no del DBMS. Respeta **DIP**.
   - Hay que contrastarlo con el Súper Objeto: es fácil de implementar, pero mezcla negocio con acceso a datos, viola SRP y aumenta el acoplamiento.
   - Se puede mencionar que el framework debe proveer **guardar/recuperar y commit/rollback**, y que aplica **Hollywood**.

> Errores que restan puntos: no mostrar las tablas (la consigna dice "mostrando"), olvidar la asociación 1 a N, no dar ventajas **y** desventajas de cada alternativa, y justificar el modelo sin nombrar principios.
