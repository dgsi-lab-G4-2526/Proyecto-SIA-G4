# Definición de KPIs, métricas de control y calidad del dato

Este documento define los tres indicadores clave (KPIs) diseñados para monitorizar la eficiencia operativa y la fiabilidad de la información en el proceso E2E del Club de Tenis.

---

## 1. KPI: Tiempo medio de reserva (Lead Time)

Este indicador mide la agilidad del proceso administrativo desde que se inicia la interacción hasta que la reserva queda confirmada.

### Definición
Tiempo transcurrido entre el estado "Reserva Solicitada" y el estado "Reserva Confirmada".

### Detalles Técnicos
* **Eventos:**
  * Inicio: creación de la reserva web (timestamp del estado "Reserva Solicitdad").
  * Fin: confirmación automática (timestamp del estado "Reserva Confimada").
* **Fórmula:** $\sum (Hora\_Confirmación - Hora\_Solicitud) / Número\_Total\_Reservas$.
* **Fuente de datos:** log transaccional del sistema SIA (tabla de histórico de estados).
* **Baseline:** al ser actualmente un proceso manual (teléfono/WhatsApp), el baseline estimado es de 15 minutos (tiempo medio entre que el usuario escribe/llama, recepción lo lee, revisa el Excel y confirma).
* **Objetivo:** reducir el tiempo de espera del usuario y automatizar las validaciones para que este tiempo sea inferior a 2 minutos en el canal online.

---

## 2. KPI: Porcentaje de ocupación de pistas

Mide el nivel de aprovechamiento de los activos críticos del club (las pistas).

### Definición
Relación entre las franjas horarias realmente utilizadas y la capacidad total instalada.

### Detalles Técnicos
* **Eventos:**
  * Numerador: suma de franjas horarias que han alcanzado el estado "Reserva en uso" (Check-in real).
  * Denominador: total de franjas horarias disponibles de todas las pistas operativas.
* **Fórmula:** $(Franjas\_en\_estado\_"Uso" / Total\_franjas\_ofertadas) \times 100$.
* **Fuente de datos:** base de datos operativa del SIA (cruce entre entidad PISTAS y estado final de RESERVAS).
* **Baseline:** se estima un 60% de ocupación actual (basado en la intuición de dirección), aunque el dato real está distorsionado por la falta de registro de los "No-shows". Al implantar el SIA, el sistema lo medirá contando automáticamente los chack-ins físicos.
* **Objetivo:** superar el 75% de ocupación en franjas "Prime Time" y optimizar las horas valle mediante promociones.

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

### Detalles Tecnicos
* **Eventos:**
  * Numerador: reservas cerradas cuyo importe coincide exactamente con la tarifa teórica y usuarios con email/DNI en formato válido.
  * Demoninador: total de registros generados en el sistema en un periodo.
* **Fórmula:** $(Registros\_Correctos / Total\_Registros\_Creados) \times 100$
* **Fuente de datos:** motor de reglas y validaciones del SIA y logs de sincronización con el ERP.
* **Baseline:** se estima un 85% de calidad actual, ya que el registro manual en hojas de cálculo genera freceuntes errores tipográficos (emails incorrectos) y descuadres entre lo apuntado y lo cobrado.
* **Objetivo:** alcanzar un 99% de fiabilidad para evitar reclamaciones y procesos de corrección manual.

---

## 4. Métrica de Calidad del Dato - DQ (Responsable: SM)

Para asegurar que la automatización del proceso de reserva sea efectiva, se define el siguiente control de calidad sobre la información:

* **Dato Crítico:** Email del Socio.
* **Métrica de Calidad:** Completitud y Validez (formato estándar `usuario@dominio.com` y no nulo).
* **Fórmula:** (Nº de socios con email válido / Total de socios registrados) * 100.
* **Impacto en el Negocio:** Un email incorrecto impide que el sistema envíe automáticamente las facturas y los recordatorios de reserva. Esto obliga al personal de recepción a intervenir manualmente, lo que anula la eficiencia buscada y degrada directamente el KPI de **"Tiempo medio de reserva"**.

---

## 5. Criterios de aceptación (Definition of Done - DoD)

Esta tarea se considera finalizada cuando:

- [ ] Se han definido las fórmulas de cálculo para los 3 KPIs seleccionados.
- [ ] Se ha vinculado cada KPI con los perfiles de acceso (ejemplo: Dirección visualiza estos datos).
- [ ] Se ha establecido un objetivo (target) medible para cada indicador.
