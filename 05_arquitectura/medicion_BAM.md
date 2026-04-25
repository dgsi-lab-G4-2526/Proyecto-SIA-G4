# 📊 Instrumentación de Eventos y Monitorización (BAM)

Para asegurar la operabilidad del Sistema de Información Automatizado (SIA) y la veracidad de los KPIs definidos en el Sprint 2, se establece la siguiente arquitectura de eventos para la Monitorización de Actividad de Negocio (BAM).

---

## 1. Localización Técnica de Eventos para KPIs
Este apartado identifica los puntos exactos del flujo del proceso donde el sistema captura los datos para alimentar los indicadores clave de desempeño.

| KPI | Evento de Disparo (Trigger) | Punto del Flujo (Pantalla / Acción) | Fuente del Dato (Registro) |
| :--- | :--- | :--- | :--- |
| **Tiempo de Reserva** | `RESERVA_INICIADA` | Pantalla 1: Selección de Pista y Horario | Log de Sesión (Frontend) |
| **Tiempo de Reserva** | `RESERVA_CONFIRMADA` | Pantalla 4: Confirmación Final y Envío QR | Tabla `Reservations` (BD) |
| **Tasa de Ocupación** | `PISTA_ACCESO_OPEN` | Apertura de puerta / Validación QR | Log de Dispositivo IoT |
| **Tasa de Cobro App** | `TX_PAYMENT_SUCCESS` | Pantalla 2: Validación Pasarela de Pago | API Gateway / Pasarela Stripe |

---

## 2. Lista de Eventos a Instrumentar (Mínimo 5)
Para una visibilidad completa del proceso "To-Be", se han definido los siguientes eventos técnicos que deben ser registrados por el sistema de forma obligatoria:

1.  **`EVT_01_SOLICITUD_CREADA`**: Registra el inicio de la interacción. Incluye el ID del socio y la pista solicitada.
2.  **`EVT_02_VALIDACION_CRM_OK`**: Confirma que el sistema ha verificado la identidad y el estado de pagos del socio en el SoR (System of Record).
3.  **`EVT_03_FALLO_PAGO_TECNICO`**: Se dispara ante errores de comunicación con la pasarela. Es crítico para la gestión de riesgos y excepciones.
4.  **`EVT_04_CHECK_DQ_EMAIL`**: Validación técnica del formato de correo electrónico antes del envío de la factura (Métrica de Calidad del Dato).
5.  **`EVT_05_TIMEOUT_PAGO`**: Evento de control que se activa si el socio permanece más de 5 minutos en la pasarela sin completar la transacción.

---

## 3. Soporte y Origen del Dato
La trazabilidad de estos eventos se apoya en tres capas de persistencia para garantizar que la especificación sea operable:

* **Logs de Aplicación**: Almacenan eventos de comportamiento (clicks, errores de carga, tiempos de respuesta) para el análisis de eficiencia.
* **Base de Datos Transaccional (PostgreSQL)**: Registro permanente de los cambios de estado del proceso (de "Pendiente" a "Confirmada").
* **Integración con Terceros (Webhooks)**: Los eventos de confirmación de pago proceden directamente de la API de la pasarela, asegurando la fiabilidad del dato financiero antes de impactar en el ERP.
