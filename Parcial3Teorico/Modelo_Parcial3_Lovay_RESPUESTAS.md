	# Modelo de Parcial 3 (estilo Lovay): RESPUESTAS

> ⚠️ No abrir hasta terminar el modelo.

## Punto 1

| #   | Correctas      | Por qué son falsas las demás                                                                                                                                          |
| --- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **B, C, D**    | A: está invertida; el diseño es **más** formal que el análisis                                                                                                        |
| 2   | **B, D**       | A: eso es del **ingeniero de componentes** · C: también sale el **modelo de despliegue**                                                                              |
| 3   | **C**          | Secuencia, actividad y casos de uso son de **comportamiento**                                                                                                         |
| 4   | **B, C, D**    | A: es **funcional** (qué hace el sistema)                                                                                                                             |
| 5   | **A, C, D**    | B: Demeter permite llamar métodos **del parámetro**, no de los objetos que este devuelve                                                                              |
| 6   | **B, C**       | A: la herencia es reuso de caja **blanca** · D: la delegación es reuso a nivel de **objetos**                                                                         |
| 7   | **A, D**       | B: ISP propone interfaces **chicas y específicas** · C: una hija **no puede** violar las pre/postcondiciones del padre                                                |
| 8   | **A, B, C**    | D: tiene un absoluto ("siempre"); además, los `if` violan OCP                                                                                                         |
| 9   | **D**          | A: KISS pide el detalle **necesario**, no el máximo · B: YAGNI = no agregar; KISS = hacer simple lo necesario · C: DRY exige que cada regla esté en **un solo lugar** |
| 10  | **A, C, D**    | B: los patrones **no** garantizan persistencia                                                                                                                        |
| 11  | **A**          | B: Iterator es de **comportamiento** · C: Singleton es de **creación** · D: Observer y Strategy son de **comportamiento**                                             |
| 12  | **A, B, C**    | D: State **elimina** los CASE (los reemplaza por polimorfismo)                                                                                                        |
| 13  | **A, B, D**    | C: el tipo se elige una vez y no cambia solo, así que es Strategy                                                                                                     |
| 14  | **B, D**       | A: eso pasa en **State** (un estado crea al siguiente) · C: eso describe a State; en Strategy el algoritmo lo elige el cliente                                        |
| 15  | **A, C**       | B: con Observer **no** se modifica el sujeto (OCP) · D: los observadores implementan la interfaz **Observador**, no heredan del sujeto                                |
| 16  | **A, B, D**    | C: los observadores **no** heredan del sujeto, así que no aplica LSP                                                                                                  |
| 17  | **A, C, D**    | B: la clase de terceros es el **Adaptable (Adaptee)**, no el Adaptador                                                                                                |
| 18  | **A, B, C, D** | Todas son correctas (D es la desventaja del patrón)                                                                                                                   |
| 19  | **B, D**       | A: centralizar muchas responsabilidades es lo **contrario** de SRP · C: el sujeto depende de la **interfaz**, así que cumple DIP                                      |
| 20  | **A, C**       | B: eso **empeora** el problema · D: las BDOO **eliminan** la impedancia                                                                                               |
| 21  | **A, C, D**    | B: la FK va en la tabla del lado **"muchos"**                                                                                                                         |
| 22  | **B, C, D**    | A: simular la herencia es la opción con **menor** redundancia                                                                                                         |
| 23  | **A, B, D**    | C: el que viola SRP es el **Súper Objeto**; el Esquema lo respeta                                                                                                     |
| 24  | **A, D**       | B: materializar es **fila → objeto** · C: esa es la definición de **librería**                                                                                        |
| 25  | **A, C, D**    | B: la reparación de fallas es solo el **17%**                                                                                                                         |
| 26  | **B**          | A: alto valor y baja calidad → **reingeniería** · C: alta calidad → **mantenimiento** · D: bajo valor y alta calidad → **mantenimiento**                              |
| 27  | **A, B, D**    | C: el ritmo es **constante** y no depende de los recursos                                                                                                             |
| 28  | **A, B, C**    | D: la ingeniería reversa **no es un proceso de cambio**; solo analiza y extrae información                                                                            |
| 29  | **B, D**       | A: busca mejorar la **estructura**, no la velocidad · C: eso describe la **reingeniería**                                                                             |
| 30  | **A, D**       | B: en el retiro gradual **ya no** se hacen cambios · C: el proceso normal **sí** incluye análisis y actualización de requerimientos                                   |

**Distribución:** 4 preguntas con 1 correcta, 9 con 2, 16 con 3 y 1 con 4.

## Punto 2: qué tiene que tener la respuesta (guía de corrección)

1. **Definición de patrón**: solución probada y con nombre a un problema de diseño recurrente. Es independiente del lenguaje y de escala media. Sus 4 elementos son nombre, problema, solución y consecuencias.
2. **Clasificación GoF**:
   - Por propósito: **creación**, **estructura** y **comportamiento**.
   - Por ámbito: **de clase** (usa herencia) y **de objeto** (usa composición).
   - Conviene dar al menos un ejemplo de cada tipo.
3. **a) Diagrama de clases del ejemplo propio**. Por ejemplo, `Pedido` (Contexto) tiene una referencia a `EstadoPedido` (abstracta), con estados concretos como `Pendiente`, `Confirmado`, `Despachado` y `Cancelado`. Hay que indicar el rol de cada clase y que el contexto delega (`estado.cancelar(this)`).
4. **b) Transiciones**: el estado concreto crea el siguiente y lo asigna con `pedido.setEstado(new Confirmado())`. Los métodos tienen un comportamiento por defecto en la clase abstracta, y cada estado redefine solo lo que puede hacer.
5. **c) Principios, justificados en el ejemplo**:
   - **SRP**: cada estado encapsula el comportamiento de ese estado.
   - **OCP**: un estado nuevo es una clase nueva, y `Pedido` no se toca.
   - **DIP**: `Pedido` depende de la abstracción `EstadoPedido`.
   - Se puede mencionar también que **reemplaza los CASE por polimorfismo**.
6. **d) State vs. Strategy**: tienen la misma estructura y distinta **intención**.
   - En **State**, el contexto cambia de comportamiento automáticamente según su estado interno, y los estados se conocen entre sí.
   - En **Strategy**, el cliente configura el contexto con un algoritmo intercambiable, y las estrategias no se conocen entre sí.

> Errores que costaron puntos en parciales anteriores: **no responder alguna parte de la consigna** (por ejemplo, la "c" o la "d") y dar justificaciones genéricas que no se refieren al ejemplo propio.
