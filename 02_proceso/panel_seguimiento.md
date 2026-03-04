# Inventario de procesos del club de tenis

## Lista de procesos del negocio

1. Lista de procesos del negocio.
2. Registro y gestión de socios.
3. Registro y gestión de usuarios externos.
4. Gestión de reservas de pistas.
5. Gestión de cancelaciones y penalizaciones.
6. Gestión de abonos y membresías.
7. Gestión de clases individuales y grupales.
8. Organización y gestión de torneos.
9. Gestión de pagos y facturación.
10. Gestión de tienda deportiva.
11. Panel de seguimiento e indicadores del club.

---

# Panel de seguimiento de estados del proceso

Este documento define la interfaz de control y los estados por los que transita el proceso End-to-End (E2E) de Gestión de Reserva de Pista.

El objetivo es garantizar la trazabilidad total y reducir las ineficiencias operativas detectadas.

---

## Visualización del dashboard operativo

El SIA dispondrá de un panel centralizado para el personal de Recepción y Administración que permitirá:

- Monitorización en tiempo real: ver el estado actual de todas las pistas y reservas del día.
- Alertas visuales: identificar rápidamente retrasos o faltas de asistencia (no-shows).
- Filtros avanzados: segmentar por tipo de usuario (Socio / Externo) y nivel de juego.

---

## Definición del ciclo de vida (Estados)

Para asegurar una gestión profesional, cada reserva debe pasar por los siguientes estados obligatorios:

### 1. Reserva solicitada
Estado inicial cuando el usuario realiza la petición web.  
El sistema valida automáticamente la disponibilidad técnica.

### 2. Reserva confirmada
La pista queda bloqueada.  
Se envía correo automático de confirmación al usuario.

### 3. Reserva en uso (Check-in)
El usuario se ha presentado en el club.  
El personal de recepción valida la llegada y actualiza el estado.

### 4. No-Show / Cancelada
Estado activado si:
- El usuario no se presenta tras 15 minutos de cortesía.
- Cancela fuera de plazo.

Este estado se vincula al sistema de penalizaciones.

### 5. Pago pendiente
La actividad ha finalizado, pero el cobro no se ha procesado (por ejemplo, socios con domiciliación mensual).

### 6. Reserva cerrada
Estado final del proceso.  
Se ha confirmado el ingreso económico y se generan los datos para los KPIs de rentabilidad.

---

## Indicadores de control en el panel

El panel mostrará de forma destacada los siguientes KPIs:

- Ocupación actual: porcentaje de pistas en estado "En Uso".
- Alertas de cobro: lista de reservas en "Pago Pendiente" con más de 24 horas de antigüedad.
- Contador de No-Shows: para identificar comportamientos recurrentes de usuarios.

---

## Criterios de aceptación (DoD)

Esta tarea de diseño se considera terminada cuando:

- [ ] Se han definido todos los estados de transición del proceso E2E.
- [ ] Existe un boceto conceptual de la interfaz del panel para Recepción.
- [ ] El flujo de estados permite medir el Lead Time administrativo (desde "Solicitada" hasta "Cerrada").
