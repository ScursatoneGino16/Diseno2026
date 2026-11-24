## Un poco de teoría

#### Vistas arquitectónicas

Muestra el proyecto desde diferentes perspectivas, dependiendo a quién va dirigido. Están dirigidas por casos de uso. Cada vista tiene su parte dinámica y estática. No todos los sistemas requieren hacer todas las vistas por la complejidad del sistema.

#### Arquitectura microservicios: Patrones

Decompose by subdomain: Identificación de servicios

Database per Service: Cada microservicio tiene su propia base de datos. Por eso tienen un componente de persistencia que va a conocer a todos los demás componentes del microservicio (menos los que son algoritmos y no se guardan), y que se va a conectar con la base de datos.

API gateway: Dirige solicitudes y respuestas

Backends for frontends: Define cómo interactúan los microservicios con cada tipo de cliente (web/mobile)

Single service per host: Despliega cada instancia de servicio en un host. El servidor web y las API están en el mismo componente. También puedo replicar el servidor web en los componentes genéricos. Sirve para hacer el despliegue en la nube.

Ventajas: Instancias de servicio aisladas, pueden consumir sólo sus propios recursos

Desventajas: Uso no eficiente de recursos

![[Pasted image 20261003172223.png]]

Multiple Services per host: El servidor web se desplegará en un único componente.

![[Pasted image 20261003172234.png]]

## Resumen práctico

Enunciado ──► (1) RNF significativos (SPA = Sí) 
		├─► (2) Vista de funcionalidad = pocos CU que "cubren" todos los RNF SPA 
		├─► (3) Descomposición en subdominios = Core / Soporte / Genéricos(patrón Decompose by subdomain) 
		├─► (4) Diseño global = 1 dominio → 1 microservicio (+ BD) + gateways + externos 
		├─► (5) Diseño detallado = adentro de cada microservicio: componentes + persistencia + interfaces 
		└─► (6) Despliegue = nodos de hardware con TODOS los componentes del global


Cada vista sale de la anterior. Lo que aparece en el global tiene que estar en el detallado y en el despliegue (mismo nombre, mismo color).

## 1) Vista de la funcionalidad

Todos los RNF significativos para la arquitectura deben estar contemplados en un CU, y se usa la menor cantidad de CU posible porque es una vista mínima. No repetir los RNF.

1 CU Inicio sesión, si hay manejo de usuarios

1 CU ABMC significativo para el dominio. El que tenga más argumentos y no sea una transacción.

Ejemplo: En administración de campos, ABM de campos. Lotes no porque la creación de Lotes es una responsabilidad de campos.

1 CU Transacción compleja web y mobile. Puede tener más de un actor y se coloca la que tiene más argumentos. No hay que confundir una transacción con un ABMC, aunque tenga crear modificar etc etc al final si es el caso de uso más importante del dominio lo más probable es que sea una transacción.

1 CU proceso automático. Actor Tiempo. Representar manejo de procesos automáticos en el sistema, acceso a base de datos, ejecución

1 CU Reporte: Listado con informacion escrita al que se le pueden aplicar filtros. Cosas que no digan explícitamente que son reportes pero que permitan filtrar, lo toman como reporte. Ejemplo: Visualizar en el mapa eventos generados para la organización, aplicando filtros de estado, fechas, tipos de eventos y ubicació

1 CU estadísticas: Informacion procesada en graficos obtenida con un algoritmo

#### Sobre los actores

Cuando un CU tiene actor secundario, significa que el actor primario llama al CU, y que el CU llama al actor secundario.

La herencia dentro del diagrama CU se realiza de la siguiente manera: El hijo apunta al padre con la flecha de generalización.

Hay que poner a todos los actores de los CU en el diagrama CU

#### Justificaciones clave

![[Pasted image 20261003172327.png]]

ABMC: "Elegido como representativo de las funciones ABMC del sistema. Representativo para el desarrollo UX/UI, para resolver los algoritmos básicos de programación y para el ABMC de las entidades. Acceso a la BD por inserción y consulta de entidades.”

Transacción: "Transacción elegida como representativa del sistema. Representativa para el desarrollo UX/UI y para resolver los algoritmos de manejo de transacciones (se guarda completa o no se guarda).”

Seguridad: "Se elige para resolver las decisiones de autenticación y autorización de los usuarios, definición de perfiles y visualización de la información según permisos.”

Reportes: "Representativo de la generación de reportes: define el formato general y el modelo base del que parten todos los reportes, con vista UX/UI amigable. Requiere acceso a BD.”

Estadísticas: "Representativo de la visualización de datos procesados en gráficos; resuelve el algoritmo de cálculo.”

Proceso masivo y/o automático: "Debe asegurar buena performance al recorrer [los registros que corresponda]; minimizar accesos a BD. Desarrolla un componente para [el envío de notificaciones / lo que haga]."

Integración externa: "Caso de uso modelo para la integración con [sistema externo]: resuelve cómo se comunica el sistema con él mediante [librería/API]."

## 2) Descomposición del sistema en subdominios

Dominios Core: Centrales, relacionados con el objetivo del sistema, con las transacciones más importantes. Distingue a la organización de la competencia. Ejemplo: Socio, beneficio, campeonato, cobro, notifica

Dominios de Soporte: Ayudan a Core. Ejemplo: Sorteo, evento

Dominios genéricos: Para cualquier aplicación, sin importar su dominio. Podrían ser externos y subcontratados. Ejemplo: Integración con mercado pago, notificaciones por correo, notificaciones push, integración con instagram, usuarios y accesos

#### Validación de subdominios

Si no sé cómo hacer la división, empezar con microservicios generales y luego ir separándolos según requerimientos. Si son muy atómicos, se va a generar acoplamiento.

Cada servicio tiene una responsabilidad y un tamaño manejable que puede ser desarrollado por un equipo de programación

Los servicios están débilmente acoplados. Si dos servicios se comunican mucho entre sí, capaz que podrían ser uno solo

## 3) Vista de diseño global

Los componentes relacionados van con el mismo color entre sí.

Diagrama de componentes. Todos los dominios que encontramos en paso 1 se colocan como microservicios, los cuales pueden tener base de datos asociadas. Los que son sobre integraciones con servicios externos no tienen base de datos. El de usuarios tiene base de datos.

Si hay una parte web y otra mobile, se separa en dos componentes.

API Gateway es un componente que funciona como una puerta de entrada para los microservicios, la misma direcciona y da la respuesta. Permite comunicación con el sistema.

Componentes general: Cada subsistema tiene un componente de persistencia que se relaciona con la base de datos.

Componente de usuarios: Se suele repetir en los dominios porque es un componente genérico que sigue siempre la misma lógica.

Notificaciones push: Este servicio no va con las cosas externas, sino que el MobileAPIGateway va a buscar al ServicioNotificacionPush, que no tiene base de datos.

Componentes externos: Si tengo servicios externos, tengo que hacer un rectángulo de borde negro y sin relleno y ponerlos adentro de forma individual. Los conecto a “PublicApiGateway”, y esta API se comunica con las integraciones. Ejemplo: ServicioDispositivoGPS—> PublicAPIGateway —> IntegracionDispositivoGPS.

Una integración que no es común como un tracker de comidas no se considera un sistema externo y no se pone en la vista global como genérico, se muestra recién en la vista de despliegue

![[Pasted image 20261003172350.png]]


## 4) Vista de diseño detallada

Los componentes relacionados van con el mismo color entre sí, correspondiente al paso anterior.

Los nodos deben tener profundidad 3D para que no se confundan con componentes

A los componentes hay que dibujarles un cuadrado en la esquina superior derecha

Interfaces: Coloco componentes para cada funcionalidad correspondiente a los microservicios. Nombre es [funcionalidad]PresWeb. (ejemplo de funcionalidades web).
![[Pasted image 20261003172406.png]]


El círculo significa “interfaz provista”. El medio círculo significa “interfaz requerida”. Si queda cómodo se enganchan ambos símbolos, sino se ponen los nombres tanto a la provista como a la requerida.

El componente que lleva a cabo un proceso automático, debe estar en algún microservicio pintado de otro color, porque no necesita persistencia ya que es un algoritmo en este caso (CalcularSorteo). Por eso la persistencia no se relaciona con el proceso automático

Si un microservicio se relaciona a más de un componente de la api, hay que ponerlo las dos veces, como pasa con “NoticiaRestApi”.

Si puedo iniciar sesión por una aplicación del celu o por el navegador, tengo que recibir a la IAutenticacionUsuarioRestAPI en ambas Apis, y el microservicio de usuarios lo tiene que proveer una sola vez.

#### Apigateway

Componente de software que recibe peticiones y las encamina a los microservicios que deben responderle. Cuando le responden, le pasa la respuesta al frontend. Resuelve comunicación entre front y back. RESUELVE: Dirigir solicitudes y respuestas, no debería tener lógica de negocio, la cual tiene que estar encapsulada en los microservicios.

DESVENTAJA: Aumenta complejidad y aumenta tiempos de respuesta. VENTAJA: Se elimina dependencia entre front y back

No diagramo relaciones entre microservicios sino que simplifico haciendo que se comuniquen mediante api gateway. Los microservicios que se conectan con sistemas externos o con core, deben estar relacionados. Pero no core entre sí, o core entre soporte.

Public api gateway se conecta con los servicios externos, por ello tiene interfaces requeridas (palito abierto) y la provista no se diagrama porque es algo externo, acá se diagraman los componentes de la aplicación.

Ejemplo: Tengo un microservicio para generar una factura, y quiero mandarlo por mail. El microservicio Factura va a relacionarse con otro microservicio “NotificaciónMail”. Y este notificaciónMail se va a relacionar con PublicApiGateway para comunicarse con el servidor de correo.

![[Pasted image 20261003172417.png]]

#### Persistencia

El microservicio necesita un componente de persistencia siempre que tenga una base de datos, el cual se va a relacionar con todos los otros componentes menos con el de algoritmos.

![[Pasted image 20261003172424.png]]

Principio “Inversión de dependencia”: La persistencia va desde el componente de persistencia hacia el elemento que quiere persistir. Ejemplo: El importador no tiene persistencia porque nomás ejecuta una función y nada de eso va a ir a una base de datos, es un algoritmo. Por eso tiene color distinto. “Generador”, “Importador” “Calculador”. La flecha significa “quién necesita a quién”.

![[Pasted image 20261003172428.png]]

#### Patrón una base de datos por servicio

Cada microservicio administra sus datos de forma privada y sólo puede acceder a ellos a través de una API.

#### Estadísticas

Todos los componentes relacionados con las estadísticas deben proveer una interfaz y conectarla con la que corresponda (IEstadisticasMeta)

## 5) Vista de despliegue

Tengo que ver si en el dominio algún RNF me pide seguir una arquitectura en específico. Si lo hace, debo hacer el diagrama respetándola (de alguna forma que desconozco). Y si no lo hace lo completo como yo quiera.

#### Diagrama de despliegue

Es estático y físico porque muestra distribución del software en el hardware. Los cubos son nodos de hardware y dentro tienen los componentes. Cada componente del diagrama de componentes tiene que aparecer.

Tipo implementación: Debe tener nodos relacionados a cada una de las implementaciones que tiene. Ejemplo: MobileAppAndroid, MobileAppIOS, Browser (cliente web). En el web no vamos a desplegar nada pero necesito un navegador, por ello pongo “Browser”.

![[Pasted image 20261003172442.png]]

Los servidores de integración tienen el componente “WebServer” y hay que ponerlo sí o sí.

#### Implementación Base de datos espejada

Cada servicio debe tener una réplica para garantizar disponibilidad. Sólo se espejan las que pide el requerimiento, y si no pide ninguna en específico, se hace con todas.

#### Implementación Base de datos sombra

Una base de datos copia para almacenar copia sincronizada de las demás bases de datos de los servicios que se necesita replicar.

![[Pasted image 20261003172452.png]]
## Ejercicios resueltos

### Caso: Administración de campos

![[Pasted image 20261003172457.png]]

Usabilidad > Accesibilidad: Personas con discapacidad

Usabilidad > Operabilidad: Que sea facil de usar

#### Requerimientos no Funcionales

Para establecer la comunicación entre el servidor y las estaciones de trabajo de deberá realizar una red privada virtual (VPN). [ Seguridad - integridad ]

Aplicación desarrollada con tecnología Web [ Portabilidad - adaptabilidad ]

Aplicación pueda operar bajo cualquiera de los siguientes navegadores: Internet Explorer 11, Mozilla Firefox 51.0.1 y Google Chrome v56.0 (y versiones superiores). [ Portabilidad - adaptabilidad ]

Para todos los reportes e informes estadísticos que emitirá el sistema, se requiere que sean visualizados tanto en formato de grilla como en formato gráfico, y que puedan ser exportarlos a Excel y Archivo. [ Usabilidad - reconocimiento de correccion ]

generar mapas de rendimiento de cada lote, en los cuales se resaltan con diferentes colores las zonas que más y menos producen [ Usabilidad - reconocimiento de correccion ]

El sistema deberá contemplar que cada usuario acceda mediante una clave, la misma deberá contener como mínimo 6 caracteres alfanuméricos y deberá estar encriptada. [ Seguridad - autenticidad ]

A los usuarios se les asignarán perfiles que determinarán a qué funcionalidades tendrá acceso y esto debe verificarse en el momento del logueo. [ Seguridad - confidencialidad ]

El Módulo Mobile deberá comunicarse con el Servidor Web de las Administración Central mediante el protocolo de comunicación HTTP [ compatibilidad - interoperatividad ]

El sistema debe ser capaz de interpretar datos de monitores de rendimiento ubicados en las cosechadoras. [ compatibilidad - interoperatividad ]

El sistema debe ser capaz de interpretar datos recibidos en tiempo real de distintos fabricantes y modelos de monitores de rendimiento ubicados en las cosechadoras. [ desempenio - comportamiento temporal ]

Agro SRL ha requerido que la base de datos de la aplicación sea desarrollada en Oracle 12c debido a que cuentan con licencias de ese motor de base de datos. [ Portabilidad - adaptabilidad ]

que el sistema funcione correctamente aun con un solo tipo de conexión limitada, sin depender de otra infraestructura [ Portabilidad - adaptabilidad ]

Para representarlo: Nombre, descripción (cuando voy a tener contacto con este requerimiento), categoría ISO 25000, SPA (Si/No) - significativo para la arquitectura.

Ejemplo: Conexión con ARCA, Al momento de facturar el pedido del cliente el sistema se comunicará con el sistema de ARCA para obtener el número de CAE, Compatibilidad - interoperatividad, Si

Justificación: No debe dar espacio a dudas, debe ser concreta. Nada de podrías o debería.

Descripción: Medibles, no rápido, sino debe responder a una velocidad de 1 segundo.

Significativos para la arquitectura:

|   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|
|N|Nombre|Descripcion|ISO 25000|SPA|Justificación|CU|
|1|Uso de red VPN|Para establecer la comunicación entre el servidor y las estaciones de trabajo de deberá realizar una red privada virtual (VPN).|Seguridad - integridad|NO|Medio de comunicación|-|
|2|Requerimientos para puesto de trabajo|Debe ser posible acceder al sistema desde cualquier dispositivo contando únicamente con conexión a internet.|Portabilidad - adaptabilidad|SI|Se debe desarrollar un producto web|-|
|3|Lenguaje de programación|El lenguaje de programación utilizado para el sistema debe ser con tecnologías web.|Portabilidad - adaptabilidad|SI|Lenguaje web|-|
|5|Clave de acceso al sistema|El sistema debe contar con encriptación para la clave de acceso.|Seguridad - autenticidad|SI|Uso de algoritmo de encriptación para que contemple las condiciones mencionadas.|-|
|6|Utilización de dispositivos móviles|El sistema debe contar con un módulo mobile compatible con dispositivos móviles.|compatibilidad - interoperatividad|SI|Lenguaje web responsive|-|
|7|Importación de datos|El sistema debe permitir el recupero de datos a través de sensores ubicados en maquinaria agrícola|compatibilidad - interoperatividad|SI|Se debe construir un componente de software para gestionar la importación de los datos desde monitores de cosechadoras.|72|
|8|Base de datos (Oracle 12c)|La base de datos debe ser Oracle 12c debido a que ya está presente en la infraestructura de la empresa.|Portabilidad - adaptabilidad|SI|La base de datos debe ser relacional y resolver al esquema de encriptación y auditorías de transacciones|-|
|9|Generación de reportes|El sistema debe permitir la realización de reportes.|Usabilidad - reconocimiento de correccion|NO|El framework utilizado ya lo resuelve|74|
|11|Despliegue en la nube|El sistema debe estar desplegado en la nube.|Portabilidad - adaptabilidad|SI|Se deberá hacer uso del patrón múltiple service per host, para gestionar el despliegue de los servicios|-|
|12|Navegadores permitidos|Internet Explorer 11, Mozilla Firefox 51.0.1 y Google Chrome v56.0 (y versiones superiores).|Portabilidad - adaptabilidad|NO|El sistema no requiere muchos recursos ni realiza una validación importante, por lo cual funciona correctamente en los navegadores mencionados.|-|

#### 1. Vista de la funcionalidad

|                               |                 |                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ----------------------------- | --------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|                               | Nro CU          | Nombre CU                                                                      | Justificación                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ABMC Entidad (4 CU)           | 12, 13, 14 y 15 | 12. Registrar campo 13. Modificar campo 14. Consultar campo 15. Eliminar campo | Elegido como representativo de las funciones ABMC del sistema. Representativo para el desarrollo UX/UI, además para resolver los algoritmos básicos de programación y para ABMC de las entidades. Acceso a la base de datos por inserción y consulta de entidades. Resuelve los RNF 2. Requerimientos para puesto de trabajo 3. Lenguaje de programación 6. Utilización de dispositivos móviles 8. Base de datos 11. Despliegue en la nube |
| Transaccion                   | 1, 6 y 8        | 1. Registrar laboreos en lotes 6. Registrar usuario 8. Cerrar sesión           | Transacciones elegidas como representativas del sistema. Representativo para el desarrollo UX/UI, además para resolver los algoritmos relacionados con manejo de transacciones. (lo escribí yo)                                                                                                                                                                                                                                            |
| Seguridad                     | 7               | 7. Iniciar sesion                                                              | -                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Proceso automatico y/o masivo | -               | -                                                                              | -                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Reportes y/o estadisticas     | 62 y 73         | 62. Generar historico por lote 73. Generar estadistica de rendimiento de lotes | Elegido como representativo de la generación de reportes y estadísticas del sistema. Representativo para la visualización de datos generados por el sistema. Resuelve los RNF: 4. Visualización de Reportes e Informes Estadísticos                                                                                                                                                                                                        |
| Resto RNF                     | 72              | 72. Importar datos relevados por monitor                                       | Elegido como representativo de la comunicación entre datos externos y el sistema. Resuelve los RNF: 7. Importación de datos                                                                                                                                                                                                                                                                                                                |

@startuml usecaseDiagram left to right direction actor "Administrador de campos" as AC actor "Encargado de laboreos" as EL actor "Usuario" as US usecase "12.Registrar campo" as U12 usecase "14.Consultar campo" as U14 usecase "13.Modificar campo" as U13 usecase "15.Eliminar campo" as U15 usecase "1. Registrar laboreos en lotes" as U1 usecase "7. Iniciar sesion" as U7 usecase "73. Generar estadisticas de rendimiento de lotes" as U73 usecase "62. Generar historico de proyectos de cultivo por campo o lote" as U62 AC --> U12 AC --> U14 AC --> U13 AC --> U15 AC --> U73 AC --> U62 EL --> U1 EL --> US AC --> US US --> U7 @enduml


​![[Pasted image 20261003172547.png]]



### Caso: Consultorio de salud mental

### Vista arquitectónica de la funcionalidad

|   |   |   |   |
|---|---|---|---|
||Nro CU|Nombre CU|Justificación|
|ABMC Entidad|6, 7, 8 y 9|6: Registrar un profesional 7: Dar de baja un profesional. 8: Modificar profesional 9: Consultar profesional|Elegido como representativo de las funciones ABMC del sistema. Representativo para el desarrollo UX/UI, además para resolver los algoritmos básicos de programación y para ABMC de las entidades. Acceso a la base de datos por inserción y consulta de entidades. Resuelve los RNF: 1: Tecnología web 6: Web-responsive 12: Disponibilidad de los datos del sistema|
|Transacción|2, 3 y 4|2. Solicitud de turno 3. Registrar aceptacion solicitud ausencia profesional 4. Generar agenda de profesional|Transacciones elegidas como representativas del sistema. Representativo para el desarrollo UX/UI, además para resolver los algoritmos relacionados con manejo de transacciones. (lo escribí yo)|
|Seguridad|16|16. Iniciar sesión|Se elige este CU para resolver las decisiones respecto a la autenticacion y autorizacion de los usuarios del sistema, como definicion de perfiles y visualizacion de la informacion dependiendo de sus permisos. RNF asociados: • 7: reCAPTCHA para inicio de sesión • 8: Seguridad de usuarios • 9: OAuth con Google • 10: Certificado SSL|
|Proceso automático y/o masivo|5 o 10|5. Habilitar agenda de profesional. 10. Solicitar confirmacion de turnos.||

@startuml usecaseDiagram left to right direction skinparam style strictuml actor "Encargado de Personal" as EP actor "Usuario" as US actor "Secretaria" as SE actor "Servidor de correo" as SC usecase "6. Registrar un profesional" as U6 usecase "7. Dar de baja un profesional" as U7 usecase "8. Modificar profesional" as U8 usecase "9. Consultar profesional" as U9 usecase "4. Generar agenda de profesional" as U4 usecase "2. Solicitud de turno" as U2 usecase "26. Generar informe de turnos asistidos por profesional" as U26 usecase "28. Generar estadística de turnos por estado" as U28 usecase "16. Iniciar sesion" as U16 usecase "3. Registrar aceptación de solicitud de ausencia de profesional." as U3 EP --> U6 EP --> U7 EP --> U8 EP --> U9 EP --> U4 EP --> U26 EP --> U28 EP--> US US --> U16 SE --|> US SC --> U3 SC --> U2 @enduml

​
![[Pasted image 20261003172554.png]]


### Caso: Reclamos

### Vista funcionalidad

|   |   |   |   |
|---|---|---|---|
||Nro CU|Nombre CU|Justificación|
|ABMC Entidad|1, 2, 3 y 4|1. Registrar evento 2. Modificar evento 3. Consultar evento 4. Eliminar evento|Elegido como representativo de las funciones ABMC del sistema. Representativo para el desarrollo UX/UI, además para resolver los algoritmos básicos de programación y para ABMC de las entidades. Acceso a la base de datos por inserción y consulta de entidades. Resuelve los RNF: 1. Aplicacion mobile 3. Sistema web 5. Servicio en la nube 10. Campos personalizables (cada empresa puede poner sus propios campos)|
|Transacción|1.|1. Registrar evento|Transacciones elegidas como representativas del sistema. Representativo para el desarrollo UX/UI, además para resolver los algoritmos relacionados con manejo de transacciones. Resuelve los RNF: 7. Ubicación del evento 11. Formato de fotos y videos 12. Interface con GPS|
|Seguridad|28|28. Iniciar sesión|Se elige este CU para resolver las decisiones respecto a la autenticacion y autorizacion de los usuarios del sistema, como definicion de perfiles y visualizacion de la informacion dependiendo de sus permisos. RNF asociados: 8. Seguridad de usuarios|
|Proceso automático y/o masivo|38, 43|-|-|
|Transaccion compleja|34||12|
|||||

### Caso: Fórmula 1

Sistema: Mover escuderías de un país a otro usando órdenes de traslado.

#### Vista de la funcionalidad

|Descripción|N CU|Nombre CU|Justificación|
|---|---|---|---|
|ABMC Entidad representativa|5, 6, 7, y 8|5. Registrar escudería 6. Modificar escudería 7. Consultar escudería 8. Eliminar escudería|Elegido como representativo de las funciones ABMC del sistema. Representativo para el desarrollo UX/UI, además para resolver los algoritmos básicos de programación y para ABMC de las entidades. Acceso a la base de datos por inserción y consulta de entidades. Resuelve los RNF: 1. Sistema web 2. Alta disponibilidad del servicio 3. Servicio en la nube|
|Transacción|14|14. Generar orden de traslado|Se eligió como representativo para dar soporte a las transacciones del sistema, las cuales requieren acceso a la base de datos. Resuelve los RNF: 5. Notificaciones por correo|
|Seguridad|22|22. Iniciar sesión|Se elige este CU para resolver las decisiones respecto a la autenticacion y autorizacion de los usuarios del sistema, como definicion de perfiles y visualizacion de la informacion dependiendo de sus permisos. Resuelve los RNF: 4. Seguridad de usuarios|
|Integracion sistemas externos|19|19. Monitorear ubicación de contenedores|Caso de uso modelo para la integracion del sistema con un mapa en tiempo real haciendo uso de su integracion con GPS. Resuelve los RNF: 6. Interfaz con GPS 7. Interfaz con Google Maps|
|Proceso masivo y/o automático|16|16. Procesar OT no analizadas|Debe asegurar una buena performance al momento de recorrer las OT creadas. Minimizar acceso a base de datos. Desarrolla un componente en el envio de notificaciones por correo.|
|Reportes y/o estadísticas|24|24. Emitir reporte de traslados|Elegido como representativo de la generación de reportes del sistema definiendo su formato general. Define un modelo base del cual parten todos los reportes del sistema, ofreciendo una vista UX/UI amigable para el usuario. Requiere de acceso a base de datos. Representativo para la visualización de datos generados.|

![[Pasted image 20261003172603.png]]

#### Patrón descomposición por subdominios

![[Pasted image 20261003172607.png]]

#### Vista arquitectónica del diseño global

![[Pasted image 20261003172616.png]]

## Caso: Entrenamientos

#### Vista de la funcionalidad

|Descripción|N CU|Nombre CU|Justificación|
|---|---|---|---|
|ABMC Entidad representativa|8, 9, 10 y 11|8. Registrar tipo de actividad 9. Eliminar tipo de actividad 10. Consultar tipo de actividad 11. Eliminar tipo de actividad|Elegido como representativo de las funciones ABMC del sistema. Representativo para el desarrollo UX/UI, además para resolver los algoritmos básicos de programación y para ABMC de las entidades. Acceso a la base de datos por inserción y consulta de entidades. Resuelve los RNF: 3. Sistema web 5. Servicio en la nube|
|Transacción compleja|4|14. Registrar entrenamiento|Se eligió como representativo para dar soporte a las transacciones complejas del sistema, las cuales requieren acceso a la base de datos. Resuelve los RNF: 1.Aplicación mobile 2.Sistemas operativos móviles 4.Disponibilidad de servicio de entrenamiento 9.Interfaz con GPS 10. Interfaz con GoogleMaps|
|Seguridad|20|20. Iniciar sesión|Se elige este CU para resolver las decisiones respecto a la autenticacion y autorizacion de los usuarios del sistema, como definicion de perfiles y visualizacion de la informacion dependiendo de sus permisos. Resuelve los RNF: 7. Seguridad de usuarios|
|Integracion sistemas externos|7|7. Compartir entrenamiento en Instagram|Caso de uso modelo para la integracion del sistema con un mapa en tiempo real haciendo uso de su integracion con GPS. Resuelve los RNF: 6.Publicación en Instagram|
|Notificaciones push|26|26. Enviar notificación de avance de la meta de entrenamiento|Caso de uso modelo para resolver la implementación de notificaciones push a aquellos usuarios que hayan decidido recibir notificaciones. Relacionado con los RNF: 8.Notificaciones push|
|Subida de archivos|27|27. Registrar ingesta de alimentos|Debe asegurar una buena performance al momento de recorrer las OT creadas. Minimizar acceso a base de datos. Desarrolla un componente en el envio de notificaciones por correo. 11.Interfaz con LogMeal|
|Reportes y/o estadísticas|23|23. Generar reporte de indicadores de entrenamiento|Elegido como representativo de la generación de reportes del sistema definiendo su formato general. Define un modelo base del cual parten todos los reportes del sistema, ofreciendo una vista UX/UI amigable para el usuario. Requiere de acceso a base de datos. Representativo para la visualización de datos generados.|
|Reportes y/o estadísticas|24|24. Generar estadística del progreso del deportista|Elegido como representativo para la consulta a base de datos y generación de estadísticas siguiendo un formato UX/UI amigable.|
|Proceso automático|25|25. Verificar el avance de la meta de entrenamiento|Debe resolver asegurar una buena performance, minimizar el acceso a la base de datos y afectación de usuarios que estén utilizando la aplicación.|