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

Para asegurar la operabilidad, el SIA aplica un motor de reglas (Business Rules Engine) que filtra la información antes de persistir cualquier dato en los sistemas maestros (SoR).

### 2.1. Reglas de Validación de Negocio
* **Regla de Simultaneidad:** Un socio no puede poseer más de una reserva activa en el mismo bloque horario. Esto evita el acaparamiento indebido de pistas y maximiza la disponibilidad.
* **Regla de Antelación:** Las reservas solo pueden realizarse con un máximo de 7 días de antelación y un mínimo de 30 minutos antes del inicio del bloque seleccionado.
* **Regla de Saldo Pendiente:** Si la consulta al CRM devuelve una deuda mayor a 0€, el sistema bloquea el flujo y redirige al socio a la pasarela de "Pago de Cuotas" antes de permitir la selección de una nueva pista.

### 2.2. Reglas de Calidad del Dato (DQ - Requisito Crítico)
El campo **Email** se ha identificado como el "Atributo Crítico para la Operación", dado que impacta en la facturación y en la entrega del código QR de acceso.
* **Validación Sintáctica:** El sistema aplica una validación mediante expresión regular (`^[a-zA-Z0-9+_.-]+@[a-zA-Z0-9.-]+$`) en el evento `EVT_04_CHECK_DQ_EMAIL`.
* **Validación de Existencia:** Si el CRM devuelve un perfil con el campo email vacío o nulo, el sistema lanza una excepción inmediata.
* **Impacto en Proceso:** Ante un fallo de DQ, el sistema bloquea el paso al estado "CERRADA", ya que el ERP rechazaría la factura por falta de receptor legal. La reserva se marca con el flag `PENDIENTE_CORRECCION_DQ` para "Intervención Manual de Recepción" a la llegada del socio.

---

## 3. Gestión de Excepciones y Resiliencia (Playbook de Errores)

Se han definido protocolos automáticos de actuación para que el proceso no se detenga por completo ante fallos técnicos de terceros o errores humanos, garantizando la continuidad del negocio.

### 3.1. Excepciones de Integración (Fallos Tecnológicos)
* **Timeout de Pasarela (Error 504):** Si la pasarela de pago (Stripe) no responde en 10 segundos, el sistema registra el evento `EVT_03_FALLO_PAGO_TECNICO`.
    * **Plan de Mitigación:** Se activa el "Modo Contingencia". El sistema confirma la reserva con el estado `PAGO_EN_RECEPCION` y envía una alerta al terminal del empleado de pista para realizar el cobro manual mediante TPV físico.
* **Caída del CRM (Error 503):** Si el CRM está inoperativo, el SIA aplica la "Regla de Confianza". Si el socio tiene el código QR de su última reserva guardado en caché en la app, se le permite el acceso temporal, marcando la transacción como `PENDIENTE_SINCRO`.
* **Error de Facturación ERP (Error 422/500):** Si el ERP rechaza el *payload* de la factura, el SIA no interrumpe el uso de la pista. Encola la petición en un sistema de reintentos automáticos (Retry Policy) que volverá a invocar la API cada 30 minutos.

### 3.2. Excepciones de Usuario y Operación Físicas
* **Fallo de Hardware (QR No Válido / Error de Escaneo):** Si el sensor IoT de la pista falla al leer el código QR en el dispositivo móvil del socio, el sistema permite la apertura manual introduciendo en el teclado numérico de la puerta un PIN de 4 dígitos enviado por SMS como *backup*.
* **Abandono en Checkout:** Si el usuario cierra la aplicación durante el proceso de pago, se activa el evento `EVT_05_TIMEOUT_PAGO`. El sistema libera la pista en menos de 60 segundos para evitar bloqueos prolongados, optimizando la métrica del KPI de "Tasa de Ocupación".
