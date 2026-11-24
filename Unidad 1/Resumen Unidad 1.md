Responsabilidad: Contrato u obligación de un tipo o clase
Las responsabilidades se relacionan con las obligaciones de un objeto respecto a su comportamiento. 
Las responsabilidades pertenecen básicamente a dos categorías: el conocer y el hacer.
### EL HACER
	
* **Hacer algo en uno mismo:** El objeto realiza una operación internamente.
	Ejemplo: La clase Profesor tiene un método para setear el valor de su atributo apellido: setApellido()
	
* **Iniciar una acción en otros objetos:** El objeto delega o dispara comportamiento en otro. 
	Ejemplo: La clase Alumno tiene un método mostrarLocalidad() que invoca a un método getNombre() de la clase Localidad.
	
* **Controla y coordinar actividades en otros objetos:** El objeto actúa como coordinador o controlador de flujo
	Ejemplo: El Gestor de Venta coordina los pasos para Validar un pago: validarPago()
	
### EL CONOCER
	
* **Estar enterado de los datos privados encapsulados:** El objeto conoce sus propios atributos internos.
	Ejemplo: La clase Empleado conoce su sueldo, nombre y cargo (datos privados). Expone getSueldo() pero nadie más accede directamente.
	
* **Estar enterado de la existencia de objetos conexos:** El objeto sabe con qué otros objetos se relaciona.
	Ejemplo: La clase Factura conoce la lista de sus DetallesFactura, y puede preguntar cuántos ítems tiene => conocerDetalles()
	
* **Estar enterado de cosas que se pueden derivar o calcular:** El objeto puede deducir información a partir de lo que ya posee.
	Ejemplo: La clase Empleado calcula su antigüedad (calcularAntigüedad()) en base a la fecha de ingreso, sin necesitar de otro objeto externo.

## PATRONES GRASP

### 1. PATRÓN EXPERTO
La función del patrón experto es asignar una responsabilidad al experto en información: la clase que cuenta con la información necesaria para cumplir con la responsabilidad. (Lo hace quien conoce)

**El patrón Experto es el principio básico para asignar responsabilidades. Dictamina que una responsabilidad (un método o comportamiento) debe ser asignada a la clase que posee la información necesaria para llevarla a cabo. Su objetivo principal es mantener el encapsulamiento y evitar que las clases tengan que depender innecesariamente de los datos internos de otras**

![[Pasted image 20260604020821.png]]

### 2. PATRÓN CREADOR
La función del patrón creador es asignar a la clase **B** la responsabilidad de crear una instancia de la clase **A** en uno de los siguientes casos: 
* **B** agrega los objetos de **A**.
* **B** contiene los objetos de **A**.
* **B** tiene los datos de inicialización que serán transmitidos a **A** cuando sea creado (así **B** es un experto respecto de la creación de **A**).

**El patrón Creador busca reducir el acoplamiento en el sistema. Determina que una clase B debe ser la responsable de instanciar a una clase A si B contiene, agrega, registra o utiliza estrechamente a A, o si posee la información necesaria para inicializarla. Al hacer esto, evitamos que clases externas tengan que conocer los detalles de creación de A.**

![[Pasted image 20260604020848.png]]

### 3. BAJO ACOPLAMIENTO
Asignar una responsabilidad para mantener bajo el acoplamiento.

El acoplamiento es una medida de la fuerza con que un elemento está conectado a, tiene conocimiento de, confía en otros elementos.

Las clases con acoplamiento alto o fuerte presentan los siguientes problemas:
* Los cambios de las clases afines ocasionan cambios locales.
* Son más fáciles de entender cuando están aisladas.
* Son más difíciles de reutilizar porque se requiere la presencia de otras clases de las que depende.

* El bajo acoplamiento estimula a asignar una responsabilidad de modo tal que no incremente el acoplamiento.
* El patrón de Bajo Acoplamiento soporta el diseño de clases más independientes, que reducen el impacto de los cambios, y también más reutilizables, que aumenta la oportunidad de mayor productividad.
* Un grado moderado de acoplamiento es normal y necesario si quiere crearse un sistema orientado a objetos, donde los objetos colaboran entre sí.

**El Bajo Acoplamiento es un patrón evaluativo cuyo objetivo es mantener a las clases lo más independientes posible entre sí. El acoplamiento mide el grado de dependencia y conocimiento que tiene una clase sobre otra. Buscamos que sea bajo para que el sistema sea fácil de mantener (los cambios en una clase no salpiquen a otras), fácil de entender por separado y altamente reutilizable.**

![[Pasted image 20260604020910.png]]

![[Pasted image 20260604020923.png]]
### 4. ALTA COHESIÓN
Asignar una responsabilidad de modo que la cohesión siga siendo alta.

La cohesión funcional es la medida de cuan enfocadas o relacionadas están las responsabilidades de una clase.

Una alta cohesión caracteriza a las clases con responsabilidades muy relacionadas que no realicen un trabajo enorme. Las clases con baja cohesión presentan los siguientes problemas:
* Son difíciles de entender.
* Son difíciles de reutilizar.
* Son difíciles de conservar.
* Son delicadas: las afectan constantemente los cambios. 

Las clases con baja cohesión a menudo representan un grado de abstracción alto o han asumido responsabilidades que deberían haber delegado en otros objetos.

**La Alta Cohesión es un patrón evaluativo que mide el grado de enfoque y relación que tienen las responsabilidades de una clase. Dictamina que una clase debe hacer una sola cosa bien enfocada y que todas sus partes deben estar íntimamente relacionadas con ese objetivo. Si una clase empieza a hacer tareas que no le corresponden, debe delegarlas en otros objetos. Esto asegura que el código sea fácil de entender, mantener y reutilizar.**

![[Pasted image 20260604020944.png]]

### 5. PATRÓN CONTROLADOR
El patrón controlador debe asignar la responsabilidad del manejo de un mensaje de los eventos de un sistema a un controlador de caso.

* Un evento del sistema es un evento de alto nivel generado por el actor externo; se asocia con operaciones del sistema. (ej. Un usuario de Excel selecciona la opción “Guardar”, está generando un evento del sistema que indica que debe almacenarse el libro actual)

* Un controlador es un objeto que no pertenece a la interfaz de usuario, responsable de manejar un evento del sistema. Define además el método para la operación del sistema.

Garantiza que los procesos del dominio sean manejados por la capa del dominio y no por la de interfaz. Reflexionar sobre el estado de un caso de uso: asegurar que las operaciones ocurran en una cierta secuencia o saber el estado actual de las operaciones del caso de uso.

**El patrón Controlador actúa como el primer objeto de la capa de dominio que recibe y coordina los eventos del sistema generados por los actores externos. Su función principal es separar la interfaz de usuario de la lógica del negocio, garantizando que los procesos del dominio no queden acoplados a las pantallas. Además, es el encargado de mantener y controlar el estado y la secuencia correcta de las operaciones de un caso de uso.**

![[Pasted image 20260604021005.png]]

## PUD

### CLASES DE ANALISIS

* **Clase Boundary:** Una clase Boundary es aquella que se ubica en el límite del sistema para mediar la interacción con los actores externos. Su responsabilidad es capturar los datos que entran y mostrar las respuestas que salen, traduciéndose en la práctica como formularios, pantallas o componentes de comunicación.

![[Pasted image 20260604100701.png]]

* **Clase de Control:** Una clase de Control es el componente dinámico que encapsula la lógica y coordinación de un caso de uso específico. Su función es actuar como intermediario, recibiendo las acciones, controlando la secuencia del proceso y delegando el trabajo pesado a las interfaces y a las entidades, sin almacenar información persistente por sí misma.

![[Pasted image 20260604100740.png]]

* **Clase Entidad:** Una clase Entidad es aquella encargada de modelar la información persistente y de larga duración del sistema. Representa los conceptos clave del negocio y se deriva directamente del modelo del dominio, siendo el objeto que contiene los datos que necesitan ser almacenados y recuperados a lo largo del tiempo.

![[Pasted image 20260604100758.png]]