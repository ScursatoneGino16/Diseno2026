# Parcial Córdoba (26/09/2026) – Venta Cambalache Solidario

## Consigna 1 (10 pts)

> Para la vista Arquitectónica de la Funcionalidad dada, justifique la elección de los casos de uso incluidos en la vista.

### Vista dada

CU incluidos: **3. Confirmar venta de producto**, **6. Iniciar sesión con biometría**, **7. Enviar notificación de entrega**, **10. Consultar productos a vender**.

Actores: Participante, Vendedor (hereda de Participante), Biometría Android (AS del CU 6), FCM (AS del CU 7, que no tiene actor principal porque lo dispara el sistema), Google Maps (AS del CU 10).

### RNF significativos para la arquitectura (dados en el enunciado)

| N | Nombre |
|---|---|
| 1 | Aplicación Mobile para Participantes Vendedores (Android 15) |
| 2 | Servicio en la nube (Cloud, DevOps con AWS) |
| 3 | Seguridad de usuarios |
| 4 | Logueo con biometría |
| 5 | Notificaciones push (FCM) |
| 6 | Interfaz con Google Maps |

---

## Justificación de los casos de uso

| Descripción                              | N CU | Nombre CU                         | Justificación                                                                                                                                                                                                                                                                                                                                                                                       |
| ---------------------------------------- | ---- | --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Transacción                              | 3    | 3. Confirmar venta de producto    | Transacción elegida como representativa del sistema. Representativa para el desarrollo UX/UI y para resolver los algoritmos relacionados con el manejo de transacciones. Requiere acceso a la base de datos para actualizar el estado del producto y registrar la información de la venta y donación. Resuelve los RNF: 1. Aplicación Mobile para Participantes Vendedores, 2. Servicio en la nube. |
| Seguridad                                | 6    | 6. Iniciar sesión con biometría   | Caso de uso elegido como representativo de la seguridad del sistema. Representativo para resolver la autenticación, autorización y gestión de sesiones de los usuarios. Además, permite resolver la integración con la biometría del dispositivo. Resuelve los RNF: 3. Seguridad de usuarios, 4. Logueo con biometría.                                                                              |
| Notificaciones push / Proceso automático | 7    | 7. Enviar notificación de entrega | Caso de uso elegido como representativo del envío de notificaciones push. Permite resolver la comunicación entre eventos del sistema y el servicio de notificaciones. Es representativo de los demás CU de notificación. Resuelve el RNF: 5. Notificaciones push.                                                                                                                                   |
| Integración sistemas externos            | 10   | 10. Consultar productos a vender  | Caso de uso elegido como representativo de la integración con sistemas externos. Permite resolver la comunicación con Google Maps y el acceso a la base de datos para consultar los productos disponibles. Resuelve el RNF: 6. Interfaz con Google Maps.                                                                                                                                            |

### Control de cobertura RNF → CU

Cada RNF significativo queda cubierto por al menos un CU de la vista, y ninguno se repite:

| RNF | CU que lo resuelve |
|---|---|
| 1. Aplicación Mobile (Android 15) | 3 |
| 2. Servicio en la nube | 3 |
| 3. Seguridad de usuarios | 6 |
| 4. Logueo con biometría | 6 |
| 5. Notificaciones push | 7 |
| 6. Interfaz con Google Maps | 10 |

### Por qué no se incluyen los demás CU

La vista es mínima: lleva la menor cantidad de CU que cubren todos los RNF significativos.

- **1. Ofrecer producto, 2. Quitar producto ofrecido:** son operaciones de alta y baja de producto. Los aspectos de arquitectura que resolverían (UX/UI mobile, acceso a BD, subida de fotos) ya los cubre la transacción del CU 3, y no agregan ningún RNF nuevo.
- **4. Registrar usuario, 5. Cerrar sesión:** la decisión de seguridad y gestión de sesiones ya se resuelve en el CU 6.
- **8 y 9 (notificaciones push):** tienen la misma estructura que el CU 7 (AP N/A, AS FCM), así que con un solo CU representativo alcanza.

### Sobre los actores del diagrama

- **Vendedor → Participante** (generalización): el Vendedor es un Participante, así que hereda el CU 6 (iniciar sesión) y además ejecuta los CU 3 y 10.
- **Biometría Android, FCM y Google Maps** son actores secundarios: el CU los llama a ellos. Por eso aparecen después como servicios externos en el diseño global y en el despliegue (Biometría, FCM, Google Maps) y como microservicios genéricos en la descomposición por subdominios (Login biométrico, Notificaciones push, Visualización con Google Maps).
- **CU 7 sin actor principal (N/A):** lo dispara el sistema cuando el producto pasa a "entregado", y la flecha que va del CU al Vendedor indica quién recibe la notificación.
