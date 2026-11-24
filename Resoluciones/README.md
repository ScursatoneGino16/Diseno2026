# Resoluciones – Tercer Parcial Córdoba (07/11/2026)

Dominio **CreditIn**, CU *Registrar pago de cuota de un préstamo*.

Para cada patrón se entregan los tres ítems de la consigna:

| Ítem | Archivo |
|---|---|
| A. Vista de diseño de la estructura (diagrama de clases) | `A_estructura.plantuml` |
| B. Vista dinámica (diagrama de secuencia) | `B_dinamica.plantuml` |
| C. Especificación del comportamiento (descripción + pseudocódigo) | `C_especificacion_comportamiento.md` |

## Tema 1 – Adapter (35 pts) + Strategy (25 pts)
Escenario: pago de una cuota vencida de un préstamo **en mora**, con recargo **porcentual** y cobro por ente externo **SENASA**.

| Patrón | Problema que resuelve |
|---|---|
| **Adapter** | Consultar la central de deudores (SENASA → JSON / AGIP → XML) y obtener el **código de pago**, sin importar el formato. |
| **Strategy** | Calcular el **recargo por mora** según la definición de mora del tipo de préstamo (porcentual o fijo). |

## Tema 2 – Observer (25 pts) + State (35 pts)
Escenario: pago de una cuota de un préstamo **al día**, con notificación vía **mail y SMS**.

| Patrón | Problema que resuelve |
|---|---|
| **Observer** | Informar de forma flexible el **pago exitoso** de la cuota y la **finalización del préstamo** (InterfazCorreo / InterfazSMS). |
| **State** | Comportamiento variable de **Préstamo** según su estado (Creado, AlDia, EnMora, Rechazado, Finalizado, ConAccionJudicial). |

## Convenciones utilizadas (tomadas de los prácticos de la cátedra)
- Solo se redibuja lo que modifica el patrón. El resto se indica con notas del tipo *"queda igual que en el análisis"*.
- La estructura lleva los atributos tipados y las signaturas completas de los métodos.
- Las interfaces llevan `<<interface>>` y las clases nuevas del patrón son de fabricación pura.
- En la secuencia, la creación de objetos se muestra con `create` + `new()` (UML 2.0) y los loops con `group Nombre [guarda]`.
- En la especificación se identifican el **método polimórfico** y el **método de enganche**, el que crea el objeto concreto.

## Supuestos de diseño
- **Adapter:** el préstamo guarda qué ente le corresponde (atributo `enteExterno`). El código de CreditIn ante AGIP es un atributo del `AdaptadorAGIP`. Se agrega `Prestamo.calcularMontoTotalADevolver()` para comparar contra el *monto final* que informa AGIP.
- **Strategy:** se agrega `DefinicionMora.esPorcentual()`. `obtenerParametrosMora()` pasa a ser `obtenerDefinicionMora()` y devuelve el objeto.
- **Observer:** `Cliente.formaNotificacion` es una colección (`["MAIL","SMS"]`), porque el cliente puede recibir notificaciones por mail y/o SMS.
- **State:** el patrón se aplica solo al Préstamo. Los estados de la Cuota se mantienen como en el análisis y se renombran a `EstadoCuota` para que no choquen con la clase abstracta `Estado`.

## Cómo ver los diagramas
Se pueden pegar en <https://www.plantuml.com/plantuml> o abrir con la extensión *PlantUML* de VS Code.
