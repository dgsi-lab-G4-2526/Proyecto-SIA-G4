# Backlog final priorizado y siguiente paso recomendado

## Enfoque

El objetivo de este documento es recoger el trabajo pendiente, priorizándolo de forma realista según valor de negocio, reducción de riesgo y viabilidad técnica.

El backlog final no se plantea como una selección priorizada de iniciativas que permitirían evolucionar el SIA del Club de Tenis desde el estado actual del MVP/prototipo hacia una solución más operable, robusta y útil para el negocio.

# 1. Criterio de priorización

La priorización del backlog se ha realizado combinando tres criterios principales:

1. **Valor de negocio inmediato**
   Se priorizan los ítems que impactan de forma directa en el proceso principal del club y en la experiencia del usuario.

2. **Reducción de riesgo operativo o financiero**
   Se priorizan los elementos que ayudan a evitar errores, impagos, incidencias técnicas o pérdidas de trazabilidad.

3. **Viabilidad realista**
   Se consideran dependencias, esfuerzo esperado y posibilidad de implantación progresiva sobre el MVP actual.

A partir de estos criterios, se da prioridad a los elementos que consolidan el flujo principal **Reserva → Uso → Pago → Facturación**, mejoran el control del proceso y generan valor visible a corto plazo.

# 2. Backlog final priorizado

| Título | Tipo | Prioridad / Weight | Valor esperado | Dependencia o restricción relevante | Criterio de terminado |
|---|---|---|---|---|---|
| Automatización de recordatorios y confirmaciones | HU / Operación | Alta / 5 | Reducir no-show y carga administrativa | Servicio de notificaciones disponible | El sistema envía confirmación al crear la reserva y recordatorio antes del uso |
| Bloqueo por impago en tiempo real | Integración / Control | Alta / 5 | Reducir morosidad y reservas inválidas | Integración CRM estable y contrato API operativo | Un socio con impago no puede confirmar reserva y recibe mensaje de bloqueo |
| Validación DQ de email antes de facturación | Dato / Control | Alta / 3 | Reducir errores de facturación y mejorar calidad del dato | Regla de validación activa y evento `EVT_04_CHECK_DQ_EMAIL` | No se envía ninguna factura al ERP con email inválido o ausente |
| Panel público de pistas libres | HU / UX | Media / 5 | Mejorar autoservicio y aumentar ocupación en horas valle | Estado de reservas actualizado desde backend | El usuario visualiza disponibilidad actualizada sin depender de recepción |
| Reintentos e idempotencia en la integración CRM | Integración / Técnica | Alta / 8 | Evitar errores y duplicidades en validación de socio | API Gateway y contrato CRM consolidados | El sistema no genera comportamientos inconsistentes ante reintentos |
| Reintentos e idempotencia en la integración ERP | Integración / Técnica | Alta / 8 | Reducir incidencias de facturación y cierre económico | Contrato ERP estable y estados finales de reserva definidos | Una reserva no se factura dos veces y los errores quedan registrados |
| Modo degradado del flujo de pago | Riesgo / Operación | Alta / 5 | Mantener continuidad operativa si falla la pasarela | Definición de fallback en recepción o pago alternativo | El proceso puede continuar con un mecanismo de contingencia ante fallo técnico |
| Integración de check-in QR / acceso físico | Funcionalidad / Integración | Media / 8 | Medir ocupación real y reforzar trazabilidad del uso | Lector QR o dispositivo de acceso disponible | El check-in cambia el estado de la reserva y queda registrado en BAM |
| Roles y permisos internos por perfil | Riesgo / Seguridad | Media / 3 | Reducir errores y reforzar control de accesos | Modelo de perfiles internos definido | Cada usuario interno accede solo a las funciones permitidas por su rol |
| Manual operativo y playbook de incidencias | Documentación / Operación | Baja / 3 | Facilitar uso real del sistema y respuesta ante fallos | Definición previa de alertas y procedimientos | Existe un documento breve con pasos de operación y gestión de incidencias |
| Revisión periódica de calidad del dato | Dato / Gobierno | Baja / 3 | Mantener consistencia del sistema tras la implantación | Definición de responsables y métricas DQ | Existe una rutina definida para revisar datos críticos como email, estado de socio y cobros |

# 3. Siguiente paso recomendado

## Iniciativa recomendada

La primera iniciativa que debería ejecutarse es **robustecer la integración con CRM y ERP**, ya que permite consolidar el flujo principal del sistema desde la validación del socio hasta el cierre y facturación de la reserva.

## Justificación

Esta iniciativa debe priorizarse porque actúa sobre el núcleo del proceso E2E del club y permite reforzar varios puntos críticos ya identificados en el proyecto:

- la validación del estado del socio antes de confirmar la reserva,
- el control de reservas con impago,
- la trazabilidad entre reserva, pago y factura,
- y la fiabilidad de las integraciones con sistemas externos.


## Motivo de prioridad

Se considera la siguiente mejor acción porque es la iniciativa que mejor equilibra:

- **valor inmediato para el negocio**,
- **reducción de riesgo operativo y financiero**,
- **impacto sobre el flujo principal del sistema**,
- y **viabilidad realista a corto plazo**.
