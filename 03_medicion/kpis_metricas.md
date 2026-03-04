# Definición de KPIs y métricas de control

Este documento define los tres indicadores clave (KPIs) diseñados para monitorizar la eficiencia operativa y la fiabilidad de la información en el proceso E2E del Club de Tenis.

---

## 1. KPI: Tiempo medio de reserva (Lead Time)

Este indicador mide la agilidad del proceso administrativo desde que se inicia la interacción hasta que la reserva queda confirmada.

### Definición
Tiempo transcurrido entre el estado "Reserva Solicitada" y el estado "Reserva Confirmada".

### Fórmula
$\sum (Hora\_Confirmación - Hora\_Solicitud) / Número\_Total\_Reservas$.

### Objetivo
Reducir el tiempo de espera del usuario y automatizar las validaciones para que este tiempo sea inferior a 2 minutos en el canal online.

---

## 2. KPI: Porcentaje de ocupación de pistas

Mide el nivel de aprovechamiento de los activos críticos del club (las pistas).

### Definición
Relación entre las franjas horarias realmente utilizadas y la capacidad total instalada.

### Fórmula
$(Franjas\_en\_estado\_"Uso" / Total\_franjas\_ofertadas) \times 100$.

### Objetivo
Superar el 75% de ocupación en franjas "Prime Time" y optimizar las horas valle mediante promociones.

---

## 3. KPI: Índice de calidad del dato (Data Quality - DQ)

Mide la integridad y exactitud de la información registrada para evitar errores en la facturación o en el servicio.

### Definición
Porcentaje de registros de usuario y reserva que cumplen con todas las validaciones técnicas y de negocio sin intervención manual.

### Fórmula
$(Registros\_Correctos / Total\_Registros\_Creados) \times 100$

### Métricas de control

- Registros con DNI/Email validados correctamente.
- Reservas cerradas cuyo importe coincide exactamente con la tarifa aplicada.

### Objetivo
Alcanzar un 99% de fiabilidad para evitar reclamaciones y procesos de corrección manual.

---

## 4. Criterios de aceptación (Definition of Done - DoD)

Esta tarea se considera finalizada cuando:

- [ ] Se han definido las fórmulas de cálculo para los 3 KPIs seleccionados.
- [ ] Se ha vinculado cada KPI con los perfiles de acceso (ejemplo: Dirección visualiza estos datos).
- [ ] Se ha establecido un objetivo (target) medible para cada indicador.
