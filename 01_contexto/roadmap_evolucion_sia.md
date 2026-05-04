# Roadmap de evolución del SIA (6–12 semanas)

## Enfoque general

El objetivo de este roadmap es definir una evolución realista del Sistema de Información Automatizado (SIA) del Club de Tenis.

La planificación se estructura en dos bloques:

- **Quick Wins**, orientados a mejoras rápidas, de bajo o medio esfuerzo y con impacto visible en operación, experiencia de usuario, calidad del dato o control.
- **Iniciativas estructurales**, orientadas a mejoras de mayores, dependientes de integración, robustez técnica, gobierno del dato, control o evolución de arquitectura.


# 1. Quick Wins (Semanas 1–4)

## 1.1 Automatización de recordatorios y confirmaciones

**Objetivo:**
Automatizar el envío de confirmaciones y recordatorios al usuario una vez realizada la reserva y antes del inicio del servicio.

**Valor esperado:**
Reducir cancelaciones de última hora, disminuir no-show y reducir carga operativa en recepción.

**Dependencia principal:**
Disponibilidad del módulo de notificaciones y de los datos de contacto validados del usuario.

**Impacto sobre KPI, riesgo o calidad del dato:**
- Impacta en la tasa de cancelaciones y no-show.
- Mejora la experiencia de usuario.
- Refuerza el uso del email como dato crítico.

**Prioridad / horizonte temporal:**
Alta prioridad. Semana 1–2.

## 1.2 Bloqueo automático por impago con mensaje claro

**Objetivo:**
Impedir que socios con cuotas pendientes puedan confirmar reservas, mostrando de forma clara el motivo del bloqueo.

**Valor esperado:**
Reducir impagos, evitar reservas inválidas y mejorar el control económico del proceso.

**Dependencia principal:**
Integración activa con CRM y validación del estado de pago del socio.

**Impacto sobre KPI, riesgo o calidad del dato:**
- Mejora el control de ingresos.
- Reduce riesgo de impago.
- Refuerza la coherencia entre reglas de negocio y operación.

**Prioridad / horizonte temporal:**
Alta prioridad. Semana 1–2.


## 1.3 Validación del email antes de facturación

**Objetivo:**
Validar de forma automática el email antes de lanzar la facturación al ERP.

**Valor esperado:**
Reducir errores de facturación, evitar incidencias administrativas y mejorar la calidad del dato.

**Dependencia principal:**
Regla de validación técnica y activación del evento `EVT_04_CHECK_DQ_EMAIL`.

**Impacto sobre KPI, riesgo o calidad del dato:**
- Mejora la calidad del dato.
- Reduce errores en el tramo final del proceso.
- Refuerza la trazabilidad del flujo económico.

**Prioridad / horizonte temporal:**
Alta prioridad. Semana 2–3.


## 1.4 Visualización pública de pistas libres en tiempo real

**Objetivo:**
Ofrecer al usuario una vista más clara de disponibilidad actualizada para facilitar la reserva y favorecer la ocupación.

**Valor esperado:**
Aumentar la ocupación de pistas en franjas menos demandadas y mejorar la experiencia de uso del sistema.

**Dependencia principal:**
Actualización consistente del estado de reservas y disponibilidad de lectura desde frontend.

**Impacto sobre KPI, riesgo o calidad del dato:**
- Impacta en el porcentaje de ocupación.
- Mejora la usabilidad del sistema.
- Reduce fricción en el proceso de reserva.

**Prioridad / horizonte temporal:**
Media prioridad. Semana 3–4.


# 2. Iniciativas estructurales (Semanas 5–12)

## 2.1 Robustecer la integración con CRM y ERP


**Objetivo:**
Mejorar la fiabilidad de las integraciones entre SIA, CRM y ERP, incorporando control de errores, reintentos, trazabilidad e idempotencia.

**Valor esperado:**
Reducir incidencias de integración, evitar duplicidades y aumentar la robustez del flujo completo de reserva a facturación.

**Dependencia principal:**
Contratos API.

**Impacto sobre KPI, riesgo o calidad del dato:**
- Reduce riesgo operativo y de integración.
- Mejora la consistencia del dato entre sistemas.
- Refuerza el control del flujo E2E.

**Prioridad / horizonte temporal:**
Alta prioridad. Semana 5–7.



## 2.2 Consolidación del flujo de pago con modo un alternativo

**Objetivo:**
Permitir continuidad operativa del proceso incluso ante fallos de pago, incorporando un modo alternativo.

**Valor esperado:**
Evitar bloqueos del proceso principal, mantener la operabilidad del club y reducir el impacto de incidencias externas.

**Dependencia principal:**
Definición de fallback operativo y coordinación entre módulo de pagos, recepción y control BAM.

**Impacto sobre KPI, riesgo o calidad del dato:**
- Impacta en la tasa de cobro.
- Reduce riesgo tecnológico.
- Mejora la continuidad del servicio.

**Prioridad / horizonte temporal:**
Alta prioridad. Semana 5–8.



## 2.3 Integración del check-in QR con control de acceso real

**Objetivo:**
Vincular la reserva confirmada con la ocupación física real mediante QR y evento de acceso.

**Valor esperado:**
Medir la ocupación real, reforzar la trazabilidad del uso de pistas y mejorar la fiabilidad del KPI de ocupación.

**Dependencia principal:**
Dispositivo de acceso o lector QR y conexión con evento `PISTA_ACCESO_OPEN`.

**Impacto sobre KPI, riesgo o calidad del dato:**
- Mejora la medición de ocupación.
- Reduce discrepancias entre reserva y uso efectivo.
- Refuerza la evidencia del servicio prestado.

**Prioridad / horizonte temporal:**
Media prioridad. Semana 6–9.



## 2.4 Definición de roles y permisos internos

**Objetivo:**
Separa qué puede hacer cada perfil interno del club dentro del sistema.

**Valor esperado:**
Reducir errores operativos, limitar accesos innecesarios y mejorar el control interno.

**Dependencia principal:**
Modelo de usuarios internos y definición funcional de permisos.

**Impacto sobre KPI, riesgo o calidad del dato:**
- Reduce riesgo operativo.
- Mejora seguridad y control.
- Evita actuaciones fuera de rol.

**Prioridad / horizonte temporal:**
Media prioridad. Semana 8–10.



## 2.5 Gobierno mínimo del dato y revisión periódica

**Objetivo:**
Definir un mecanismo básico de revisión de calidad del dato y responsabilidad sobre información crítica del sistema.

**Valor esperado:**
Aumentar la sostenibilidad del sistema a medio plazo y evitar que la calidad del dato se degrade con el uso.

**Dependencia principal:**
Reglas de validación, definición de responsables y métricas de calidad del dato ya identificadas.

**Impacto sobre KPI, riesgo o calidad del dato:**
- Mejora DQ.
- Reduce errores en facturación, reservas y notificaciones.
- Refuerza fiabilidad del sistema.

**Prioridad / horizonte temporal:**
Media prioridad. Semana 9–12.
