# Inventario de Procesos del Club de Tenis

## 1. Lista de procesos del negocio

1. Registro y gestión de socios.
2. Registro y gestión de usuarios externos.
3. Gestión de reservas de pistas.
4. Gestión de cancelaciones y penalizaciones.
5. Gestión de abonos y membresías.
6. Gestión de clases individuales y grupales.
7. Organización y gestión de torneos.
8. Gestión de pagos y facturación.
9. Gestión de tienda deportiva.
10. Panel de seguimiento e indicadores del club.


# Proceso End-to-End seleccionado

## Proceso E2E: Gestión de Reserva de Pista (Reserva → Uso → Facturación)

### Justificación de la elección

Este proceso es el más relevante porque:

- Es el servicio principal del club.
- Impacta directamente en la ocupación de instalaciones.
- Genera ingresos directos.
- Afecta a la satisfacción del socio.
- Es medible mediante KPIs claros.
- Presenta margen real de mejora mediante digitalización.

Además, conecta directamente con la Promesa de Valor: trazabilidad completa desde la reserva hasta la facturación.




## Alcance del proceso

### Qué entra dentro del proceso:

- Creación de reserva.
- Confirmación automática.
- Gestión de modificaciones o cancelaciones.
- Uso efectivo de la pista.
- Confirmación de asistencia.
- Generación y registro del pago.
- Facturación.


### Qué no entra en esta fase:

- Gestión avanzada de torneos.
- Optimización predictiva de ocupación mediante IA.
- Gestión contable completa externa.
- Integración con sistemas bancarios reales (solo conceptual).


# Flujo completo del proceso E2E

## 1. Solicitud de reserva

El socio o usuario externo accede a la plataforma web y consulta disponibilidad de pistas por fecha y franja horaria.

Selecciona:
- Fecha
- Hora
- Tipo de pista
- Modalidad

El sistema valida:
- Disponibilidad real
- Límites de reserva por usuario
- Estado de pagos pendientes

Estado del proceso: **Reserva solicitada**



## 2. Confirmación automática

Si no existen conflictos, el sistema confirma la reserva automáticamente.

Se genera:
- Identificador único de reserva
- Confirmación vía email/notificación
- Registro en base de datos

Estado del proceso: **Reserva confirmada**



## 3. Posibles modificaciones o cancelaciones

El usuario puede:
- Modificar horario (si hay disponibilidad)
- Cancelar dentro del plazo permitido

El sistema:
- Aplica penalización automática si corresponde
- Registra historial del usuario

Estados posibles:
- **Reserva modificada**
- **Reserva cancelada**
- **Reserva cancelada con penalización**




## 4. Uso efectivo de la pista

En la franja horaria reservada:

- El usuario se presenta en recepción.
- El sistema valida asistencia.
- Si no se presenta y no canceló se marca como **no-show**.

Estados posibles:
- **Reserva utilizada**
- **Reserva no-show**



## 5. Gestión del pago

Según el modelo del club:

- Pago inmediato online.
- Pago en recepción.
- Cargo mensual a socios.

El sistema registra:
- Método de pago.
- Estado del pago.
- Relación con la reserva.

Estado del proceso:
- **Pago pendiente**
- **Pago confirmado**



## 6. Facturación y cierre

Una vez confirmado el pago:

- Se genera registro contable.
- Se actualizan métricas del sistema.
- Se cierra la reserva.

Estado final: **Reserva cerrada**


# Actores implicados

- Socio / Usuario externo
- Recepción / Administración
- Sistema SIA
- Dirección del club (para análisis de datos)


# KPIs asociados al proceso E2E

A continuación se identifican los principales indicadores que permitirán medir el rendimiento del proceso 


## KPI 1: Porcentaje de ocupación de pistas

Tipo: Eficiencia operativa

Fórmula:
(Ocupación real de franjas horarias / Total de franjas disponibles) × 100

Qué mide:
Nivel de aprovechamiento de las instalaciones.



## KPI 2: Tasa de cancelaciones

Tipo: Calidad del servicio

Fórmula:
(Número de reservas canceladas / Total de reservas) × 100

Qué mide:
Estabilidad del sistema de reservas y comportamiento de usuarios.



## KPI 3: Tasa de no-show

Tipo: Eficiencia económica

Fórmula:
(Número de reservas no utilizadas sin cancelación / Total de reservas confirmadas) × 100

Qué mide:
Pérdida de ingresos y mala planificación de uso de instalaciones.



## KPI 4: Lead Time administrativo (tiempo desde que se crea la reserva hasta que el pago queda confirmado y el proceso se cierra)

Tipo: Eficiencia temporal

Evento inicio:
Creación de reserva.

Evento fin:
Confirmación del pago.

Fórmula:
Tiempo medio transcurrido entre ambos eventos.

Qué mide:
Rapidez y eficiencia del proceso completo.



## KPI 5: Ingresos por franja horaria

Tipo: Rentabilidad

Fórmula:
Ingresos totales generados por franja / Número de franjas ocupadas

Qué mide:
Rentabilidad real de cada horario.