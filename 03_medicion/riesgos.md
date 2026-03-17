# Riesgos del Sistema de Información

Se han identificado los siguientes riesgos críticos que podrían afectar la operación digital del Club de Tenis:

## 1. Riesgo de Datos, Privacidad y Compliance (RGPD)
* **Descripción**: Posible acceso no autorizado o filtración de la base de datos de socios, afectando a datos sensibles como métodos de pago o contactos personales.
* **Impacto**: Sanciones legales graves por incumplimiento de la normativa de protección de datos y pérdida de confianza de los clientes.
* **Mitigación**: Implementación de cifrado en la base de datos, control de acceso por roles (RBAC) y auditorías de seguridad periódicas.

## 2. Riesgo Operativo (Continuidad del Servicio)
* **Descripción**: Fallo crítico en el servidor de aplicaciones o en la pasarela de pagos que impida realizar reservas o cobros automáticos en tiempo real.
* **Impacto**: Saturación del personal administrativo (vuelta al modelo manual), pérdida de ingresos inmediatos y posibles errores por solapamiento de reservas.
* **Mitigación**: Arquitectura con copias de seguridad automáticas diarias y un protocolo de pagos manuales de contingencia para evitar el bloqueo del servicio.

