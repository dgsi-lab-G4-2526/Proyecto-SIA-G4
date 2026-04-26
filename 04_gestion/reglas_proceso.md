# Reglas de Proceso: Especificación Operable (Detalle E2E)

Este documento constituye el motor lógico del Sistema de Información Automatizado (SIA). Define cómo el sistema toma decisiones ante cada evento y qué reglas de negocio gobiernan la transición entre los estados del ciclo de vida de una reserva, garantizando que el proceso "To-Be" sea robusto, auditable y sin ambigüedades.

---

## 1. Diagrama de Estados y Ciclo de Vida del Proceso

El proceso "To-Be" se basa en una máquina de estados finitos. Cada transición es irreversible una vez validadas las condiciones, lo que garantiza que ninguna pista sea ocupada sin validación del perfil del socio y sin el pago confirmado. Esta estructura mitiga las ineficiencias y "reservas fantasma" detectadas en el modelo "As-Is".

### Tabla de Estados y Transiciones

| Estado | Descripción Técnica | Evento de Salida (Trigger) |
| :--- | :--- | :--- |
| **SOLICITADA** | El usuario ha seleccionado un slot de pista y hora. El sistema crea un registro temporal en la base de datos y bloquea el ID de pista para evitar colisiones. | `EVT_01_SOLICITUD_CREADA` |
| **VALIDANDO_SOCIO** | El sistema inicia una llamada asíncrona al CRM. El usuario visualiza una pantalla de carga ("Verificando perfil..."). | `API_RES_CRM_SUCCESS` |
| **PENDIENTE_PAGO** | El socio es apto. Se genera el ID de transacción y se invoca el SDK de la pasarela de pagos (Stripe). | `TX_PAYMENT_SUCCESS` |
| **CONFIRMADA** | El pago ha sido verificado. Se genera un Hash SHA-256 (ID_Socio + ID_Reserva) para crear el código QR. | `RESERVA_CONFIRMADA` |
| **EN_USO** | El sensor IoT de la pista ha validado el QR y ha enviado la señal de apertura de puerta/luces. | `PISTA_ACCESO_OPEN` |
| **FINALIZADA** | El tiempo de reserva ha expirado. El sistema libera el recurso físico. | `EVT_TIME_UP` |
| **CERRADA** | Se ha completado el envío de datos al ERP y se ha generado la factura legal de la operación. | `POST_FACTURA_201` |

### Lógica de Transición (Reglas de Disparo)
1. **Bloqueo de Seguridad:** Al pasar a "SOLICITADA", el sistema activa un *timer* de 300 segundos. Si no se alcanza el estado "CONFIRMADA" en ese tiempo, la reserva pasa automáticamente a "CANCELADA_TIMEOUT" para liberar la pista.
2. **Validación Cruzada:** El sistema impide programáticamente pasar a "CONFIRMADA" sin que el estado previo sea estrictamente "PENDIENTE_PAGO" y exista un Token de pago válido asociado a la sesión.

---

## 2. Reglas de Negocio, Validaciones y Calidad del Dato


---

## 3. Gestión de Excepciones y Resiliencia (Playbook de Errores)
