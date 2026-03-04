# Definición de validaciones de datos y reglas de negocio

Este documento detalla las reglas de control y validaciones de negocio que el SIA debe ejecutar para garantizar la sostenibilidad económica del club y la optimización de las instalaciones.

---

## 1. Validaciones de negocio para reservas

Antes de confirmar cualquier reserva en el flujo E2E, el sistema debe validar:

### 1.1 Estado de pago de cuota
El sistema impedirá la reserva a socios que tengan recibos mensuales pendientes de pago.

### 1.2 Límite de reservas activas
Para evitar el acaparamiento de pistas, un socio solo puede tener un máximo de 2 reservas activas simultáneamente.

### 1.3 Validación de disponibilidad real
El sistema debe cruzar datos en milisegundos para evitar dobles reservas en la misma pista y franja horaria.

---

## 2. Reglas de cancelación y penalizaciones

Para reducir el ratio de pistas vacías y mejorar la ocupación (objetivo del negocio), se aplican las siguientes reglas:

### 2.1 Plazo de cancelación gratuita
Solo se permite la cancelación sin coste hasta 24 horas antes del evento.

### 2.2 Penalización por cancelación tardía
Si se cancela con menos de 24 horas de antelación, el sistema cargará automáticamente el 50% del coste de la pista.

### 2.3 Bloqueo por No-Show
Si un usuario acumula 3 estados de "No-Show" en un mes, el sistema bloqueará su capacidad de reservar online durante 15 días, obligándole a realizar la reserva presencialmente en recepción.

---

## 3. Validaciones de integridad del proceso (Data Quality - DQ)

Para asegurar la calidad del dato en la facturación:

### 3.1 Cierre de caja
No se puede marcar una reserva como "Cerrada" si el importe registrado es inferior al precio de la tarifa aplicada (Socio vs Externo).

### 3.2 Coherencia temporal
El sistema no permitirá registrar un Check-in ("Reserva en Uso") si la hora actual es posterior a la hora de finalización de la reserva.

---

## 4. Criterios de aceptación (Definition of Done - DoD)

Esta tarea se considera finalizada cuando:

- [ ] Se han definido al menos 3 reglas de negocio que afecten directamente a los ingresos o la ocupación.
- [ ] Se han vinculado estas validaciones con los estados del panel de seguimiento (Issue #7).
- [ ] El equipo ha validado que estas reglas no suponen una barrera excesiva para la experiencia del usuario.
