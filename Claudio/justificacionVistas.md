# Entrega 2 – Diseño arquitectónico (Bolsines – Grupo 1)

## 1. Requerimientos no funcionales significativos para la arquitectura

| N | Nombre | Descripción | ISO 25000 | SPA | Justificación |
|---|---|---|---|---|---|
| 1 | Tecnología web | El sistema se desarrolla completamente con tecnología web. | Portabilidad – Adaptabilidad | SI | Define el lenguaje y la estructura del front (WebUI con componentes PresWeb) y del WebAPIGateway. |
| 2 | Alta disponibilidad | Disponibilidad 7x24x365 mediante bases de datos espejadas (Database Mirroring) en 2 servidores diferentes. | Fiabilidad – Disponibilidad | SI | Cada base de datos de los servicios se despliega con un servidor espejo. |
| 3 | Despliegue espejado de la BD en la nube | La base de datos se aloja en servidores de Amazon en la nube. | Portabilidad – Adaptabilidad | SI | Todos los nodos de servicios y sus bases (y espejos) se despliegan en AWS. |
| 4 | Seguridad de usuarios | Autenticación de usuarios, control de sesiones y administración de permisos. | Seguridad – Autenticidad / Confidencialidad | SI | Requiere el microservicio genérico de autenticación de usuarios con su propia base de datos. |
| 5 | Envío de notificaciones por mail | Notificar por email la recepción de un bolsín y la ubicación de un bolsín, a través de la API de Outlook de Microsoft Graph. | Compatibilidad – Interoperabilidad | SI | Requiere el microservicio genérico ServicioNotificacionMail integrado al PublicAPIGateway. |
| 6 | Dispositivo GPS Tracker | Interface que resuelva la comunicación e interprete las coordenadas de los distintos modelos de GPS Tracker (XTR-4500L, NavTrack QX-7A, GeoPulse MTR-900). | Compatibilidad – Interoperabilidad | SI | Requiere el microservicio genérico ServicioIntegracionGPSTracker (allí se aplica el patrón Adapter en la Entrega 3). |
| 7 | Visualización de bolsines en el mapa | Librería de Google Maps que se comunica con el servidor de Google Maps. | Compatibilidad – Interoperabilidad | SI | Requiere el microservicio genérico ServicioVisualizacionEnMapa, que requiere la librería de Google Maps. |
| 8 | Generación de reportes y estadísticas | Los reportes y estadísticas se generan con Power BI. | Compatibilidad – Interoperabilidad | SI | Requiere el microservicio genérico ServicioIntegracionPowerBI. |

## 2. Vista arquitectónica de la funcionalidad – Justificación de los casos de uso

La vista es mínima: incluye la menor cantidad de casos de uso que cubren todos los RNF significativos para la arquitectura.

| Descripción | N° CU | Nombre CU | Justificación |
|---|---|---|---|
| ABMC entidad representativa | 19, 20, 21 y 22 | 19. Generar bolsín, 20. Modificar bolsín, 21. Eliminar bolsín, 22. Consultar bolsín | Elegido como representativo de las funciones ABMC del sistema sobre la entidad central del dominio. Es representativo para el desarrollo UX/UI, para resolver los algoritmos básicos de programación y el ABMC de las entidades. Accede a la base de datos para insertar y consultar entidades. Resuelve los RNF: 1. Tecnología web, 2. Alta disponibilidad y 3. Despliegue espejado de la BD en la nube. |
| Transacción compleja | 28 (incluye 29) | 28. Registrar recepción de bolsín, 29. Notificar recepción de bolsín | Transacción elegida como representativa del sistema. En una misma operación actualiza el estado del bolsín, de sus remitos y de la documentación según la situación de recepción, por lo que debe guardarse completa o no guardarse. Es representativa para el desarrollo UX/UI y para los algoritmos de manejo de transacciones. Por la inclusión del CU 29 resuelve el RNF 5. Envío de notificaciones por mail (notificación de recepción). |
| Seguridad | 5 | 5. Iniciar sesión | Se elige para resolver las decisiones de autenticación y autorización de los usuarios del sistema: definición de perfiles, manejo de sesiones y visualización de la información según permisos. Resuelve el RNF 4. Seguridad de usuarios. |
| Integración con sistemas externos | 36 (incluye 30) | 36. Consultar seguimiento de bolsines, 30. Notificar ubicación de bolsín | Caso de uso modelo para la integración del sistema con sistemas externos. Resuelve cómo se obtiene la ubicación de los bolsines desde los distintos modelos de GPS Tracker, cómo se muestran en un mapa con la librería de Google Maps y, por la inclusión del CU 30, cómo se envía el mail con la ubicación. Resuelve los RNF: 6. Dispositivo GPS Tracker, 7. Visualización de bolsines en el mapa y 5. Envío de notificaciones por mail (notificación de ubicación). |
| Reporte | 37 | 37. Generar reporte de bolsines en tránsito | Elegido como representativo de la generación de reportes del sistema. Define el formato general y el modelo base del que parten todos los reportes, con una vista UX/UI amigable. Requiere acceso a la base de datos y la comunicación con Power BI. Resuelve el RNF 8. Generación de reportes y estadísticas. |
| Estadística | 38 | 38. Generar estadística de movimiento de bolsines | Elegido como representativo de la visualización de datos procesados en gráficos. Resuelve el algoritmo de cálculo de las cantidades y la comunicación con Power BI. Resuelve el RNF 8. Generación de reportes y estadísticas. |

### Control de cobertura RNF → CU

| RNF | CU que lo resuelve |
|---|---|
| 1. Tecnología web | 19–22 |
| 2. Alta disponibilidad | 19–22 |
| 3. Despliegue espejado de la BD en la nube | 19–22 |
| 4. Seguridad de usuarios | 5 |
| 5. Envío de notificaciones por mail | 29 (incluido en 28) y 30 (incluido en 36) |
| 6. Dispositivo GPS Tracker | 36 |
| 7. Visualización de bolsines en el mapa | 36 |
| 8. Generación de reportes y estadísticas | 37 y 38 |

### Por qué no se incluyen los demás CU

- **27. Registrar el retiro de bolsines:** actualiza estados igual que el CU 28 y no agrega ningún RNF que no esté cubierto.
- **ABMC de usuario, documentación, tipo de documento, remito y comisión médica:** tienen la misma estructura que el ABMC de bolsín y no agregan RNF nuevos.
- **6. Cerrar sesión:** la gestión de sesiones ya se resuelve con el CU 5.
- **39. Cerrar bolsín y 40. Reabrir bolsín:** son cambios de estado simples ya contemplados en el ABMC de bolsín.

> Nota: en el listado de la ERS el CU 30 es "Notificar ubicación de bolsín" y el CU 31 es "Registrar revisión de documentación". La descripción del CU 36 dice "se incluye al CU 31 Notificar ubicación de bolsín", lo que es un error de numeración de la ERS. En la vista usamos el **CU 30**, que es el número que corresponde al nombre en el listado.

## 3. Descomposición en subdominios (patrón Decompose by subdomain)

- **Core:** Bolsines. Es el objetivo del sistema (gestión de envío, recepción y seguimiento de documentación en bolsines) y concentra las transacciones más importantes. Incluye el seguimiento del bolsín, los remitos y la documentación: según las reglas de negocio, cada cambio de estado del bolsín (generar, modificar, eliminar, retirar, recibir) actualiza en la misma operación el estado de sus remitos y de su documentación. Separarlos en servicios distintos generaría un alto acoplamiento entre servicios y transacciones distribuidas entre varias bases de datos, por eso forman un único dominio y un único servicio.
- **Soporte:** Comisiones Médicas (incluye a los empleados asignados a cada CM). Le dan sentido al core (CM origen/destino, gerente de la CM destino), pero no son el objetivo del sistema.
- **Genéricos:** Autenticación de Usuarios, Notificación por Mail, Visualización en Mapa, Integración GPS Tracker e Integración Power BI. Son comunes a cualquier negocio y podrían comprarse o subcontratarse.

Cada subdominio se convierte en un microservicio en la vista de diseño global.

## 4. Patrones de microservicios aplicados

- **Decompose by subdomain:** para identificar los servicios a partir de los subdominios core, de soporte y genéricos.
- **Database per Service:** cada servicio con datos propios (Bolsin, ComisionMedica, AutenticacionUsuarios) tiene su base de datos y un componente de persistencia que depende de los componentes que persiste y de la base de datos. Los servicios de integración no tienen base de datos.
- **API Gateway:** el WebAPIGateway dirige las solicitudes del front a los servicios y el PublicAPIGateway resuelve la comunicación con los servicios externos (GPS Tracker, Google Maps, Outlook Microsoft Graph y Power BI).
- **Single Service per Host:** cada servicio se despliega en su propio host en la nube de Amazon (RNF 3), lo que permite configurar una réplica espejada por servicio (RNF 2) sin depender de otro microservicio.
