# Especificación de Eventos de Sistema

## Objetivo

Estos eventos permiten registrar momentos clave del proceso de reserva, uso, pago y cierre, así como disparar acciones automáticas, controlar excepciones y asegurar la calidad del dato.

## Relación con BAM y con el proceso E2E

Los eventos aquí definidos se apoyan en el proceso principal del sistema:

**Reserva de pista → Uso → Pago → Facturación → Cierre**

Cada evento representa un punto de control dentro del flujo y permite:

- medir el rendimiento real del proceso,
- generar trazabilidad operativa,
- detectar incidencias,
- activar integraciones con CRM, ERP y pasarela de pago,
- y alimentar el panel de seguimiento.

---

## 1. Localización técnica de eventos para KPIs

Este apartado identifica los puntos exactos del flujo donde el sistema captura los datos que alimentan los indicadores clave.

| KPI | Evento de Disparo (Trigger) | Punto del Flujo (Pantalla / Acción) | Fuente del Dato (Registro) |
|---|---|---|---|
| Tiempo de Reserva | `RESERVA_INICIADA` | Pantalla 1: Selección de pista y horario | Log de sesión (Frontend) |
| Tiempo de Reserva | `RESERVA_CONFIRMADA` | Pantalla 4: Confirmación final y envío QR | Tabla `Reservations` (BD) |
| Tasa de Ocupación | `PISTA_ACCESO_OPEN` | Apertura de puerta / validación QR | Log de dispositivo IoT |
| Tasa de Cobro App | `TX_PAYMENT_SUCCESS` | Pantalla 2: Validación en pasarela de pago | API Gateway / Pasarela Stripe |

---

## 2. Lista de eventos a instrumentar

A continuación se definen los eventos mínimos que el sistema debe registrar de manera obligatoria.

### EVT_01_SOLICITUD_CREADA

**Descripción:** registra el inicio de la interacción cuando el usuario solicita una reserva.  
**Lo emite:** frontend / módulo de reservas del SIA.  
**Lo consume:** motor de validaciones, panel de seguimiento.  
**Acción que dispara:** comprobación inicial de disponibilidad y comienzo del seguimiento del tiempo de reserva.

**Payload mínimo:**

```json
{
  "event_id": "EVT-0001",
  "event_name": "EVT_01_SOLICITUD_CREADA",
  "timestamp": "2026-05-02T17:00:00Z",
  "id_socio": "SOC-000245",
  "id_pista": "P-03",
  "fecha_reserva": "2026-05-05",
  "hora_inicio": "18:00",
  "hora_fin": "19:30",
  "origen": "SIA-Frontend"
}
```

### 2.2 Evento: `EVT_02_VALIDACION_CRM_OK`

**Descripción:** confirma que el sistema ha validado correctamente al socio contra el CRM, verificando identidad, estado de membresía y situación de pago.
**Lo emite:** integración SIA ↔ CRM.
**Lo consume:** módulo de reservas del SIA.
**Acción que dispara:** continuación del flujo hacia la confirmación de la reserva.

**Payload mínimo:**

```json
{
  "event_id": "EVT-0002",
  "event_name": "EVT_02_VALIDACION_CRM_OK",
  "timestamp": "2026-05-02T17:00:02Z",
  "id_socio": "SOC-000245",
  "estado_membresia": "ACTIVA",
  "estado_pago_cuota": "AL_CORRIENTE",
  "permite_reserva": true,
  "origen": "SIA-CRM"
}
```

### 2.3 Evento: `RESERVA_INICIADA`

**Descripción:** evento técnico usado para marcar el comienzo del KPI de tiempo de reserva.
**Lo emite:** frontend del SIA.
**Lo consume:** módulo BAM / métricas.
**Acción que dispara:** inicio del cronómetro del proceso de reserva.

**Payload mínimo:**

```json
{
  "event_id": "EVT-0003",
  "event_name": "RESERVA_INICIADA",
  "timestamp": "2026-05-02T17:00:00Z",
  "id_socio": "SOC-000245",
  "id_pista": "P-03",
  "origen": "SIA-Frontend"
}
```

### 2.4 Evento: `RESERVA_CONFIRMADA`

**Descripción:** se genera cuando la reserva queda confirmada en el sistema.
**Lo emite:** módulo de reservas del SIA.
**Lo consume:** módulo de notificaciones, BAM, panel de seguimiento.
**Acción que dispara:** fin del tiempo de reserva, envío de confirmación y generación del QR.

**Payload mínimo:**

```json
{
  "event_id": "EVT-0004",
  "event_name": "RESERVA_CONFIRMADA",
  "timestamp": "2026-05-02T17:00:05Z",
  "id_reserva": "RES-001245",
  "id_socio": "SOC-000245",
  "id_pista": "P-03",
  "estado_reserva": "CONFIRMADA",
  "origen": "SIA-Reservas"
}
```

### 2.5 Evento: `PISTA_ACCESO_OPEN`

**Descripción:** evento que confirma el acceso físico del usuario a la instalación mediante validación QR o apertura de puerta.
**Lo emite:** dispositivo IoT / control de acceso.
**Lo consume:** módulo de ocupación, BAM, panel de seguimiento.
**Acción que dispara:** confirmación de uso real de pista y alimentación del KPI de ocupación.

**Payload mínimo:**

```json
{
  "event_id": "EVT-0005",
  "event_name": "PISTA_ACCESO_OPEN",
  "timestamp": "2026-05-05T17:58:00Z",
  "id_reserva": "RES-001245",
  "id_socio": "SOC-000245",
  "id_pista": "P-03",
  "origen": "IOT-Access"
}
```

### 2.6 Evento: `TX_PAYMENT_SUCCESS`

**Descripción:** confirma que la transacción de pago se ha completado correctamente en la pasarela.
**Lo emite:** pasarela de pago / API Gateway.
**Lo consume:** módulo de pagos del SIA, BAM, facturación.
**Acción que dispara:** actualización del estado de pago y preparación del envío al ERP.

**Payload mínimo:**

```json
{
  "event_id": "EVT-0006",
  "event_name": "TX_PAYMENT_SUCCESS",
  "timestamp": "2026-05-05T19:35:00Z",
  "id_reserva": "RES-001245",
  "id_socio": "SOC-000245",
  "importe_total": 12.00,
  "metodo_pago": "APP",
  "origen": "Stripe"
}
```

### 2.7 Evento: `EVT_03_FALLO_PAGO_TECNICO`

**Descripción:** se dispara cuando existe un error técnico de comunicación con la pasarela de pago.
**Lo emite:** API Gateway / integración de pagos.
**Lo consume:** sistema de alertas, panel de seguimiento, equipo de soporte.
**Acción que dispara:** generación de alerta operativa y registro de excepción.

**Payload mínimo:**

```json
{
  "event_id": "EVT-0007",
  "event_name": "EVT_03_FALLO_PAGO_TECNICO",
  "timestamp": "2026-05-05T19:31:00Z",
  "id_reserva": "RES-001245",
  "codigo_error": "PAY-502",
  "mensaje": "Error de comunicación con pasarela",
  "origen": "API-Gateway"
}
```

### 2.8 Evento: `EVT_04_CHECK_DQ_EMAIL`

**Descripción:** valida técnicamente el formato y presencia del email antes de enviar datos a facturación.
**Lo emite:** módulo de validación del SIA.
**Lo consume:** módulo de facturación / control de calidad del dato.
**Acción que dispara:** permitir o bloquear el envío a ERP si el dato no es válido.

**Payload mínimo:**

```json
{
  "event_id": "EVT-0008",
  "event_name": "EVT_04_CHECK_DQ_EMAIL",
  "timestamp": "2026-05-05T19:35:30Z",
  "id_socio": "SOC-000245",
  "email": "nombreapellido@gmail.es",
  "resultado_validacion": "OK",
  "origen": "SIA-Validacion"
}
```

### 2.9 Evento: `EVT_05_TIMEOUT_PAGO`

**Descripción:** evento de control que se activa cuando el usuario permanece demasiado tiempo en la pasarela sin completar la operación.
**Lo emite:** módulo de pagos / temporizador del SIA.
**Lo consume:** panel de seguimiento, alertas, métricas de abandono.
**Acción que dispara:** cierre del intento de pago y registro de incidencia operativa.

**Payload mínimo:**

```json
{
  "event_id": "EVT-0009",
  "event_name": "EVT_05_TIMEOUT_PAGO",
  "timestamp": "2026-05-05T19:10:00Z",
  "id_reserva": "RES-001245",
  "id_socio": "SOC-000245",
  "timeout_segundos": 300,
  "origen": "SIA-Pagos"
}
```

## 3. Soporte y origen del dato

### Logs de aplicación

Registran eventos de comportamiento y errores técnicos, como:

- inicio de interacción,
- tiempos de respuesta,
- fallos de carga,
- eventos de frontend.

### Base de datos transaccional

Registra cambios persistentes de estado del proceso, por ejemplo:

- reserva creada,
- reserva confirmada,
- pago asociado,
- cierre del proceso.

### Integraciones con terceros

Permiten recibir eventos externos fiables, como:

- confirmación de pago,
- errores de pasarela,
- validación de acceso IoT,
- respuesta de ERP o CRM.

