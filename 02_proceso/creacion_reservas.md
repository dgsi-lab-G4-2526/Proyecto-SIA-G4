# Historia de Usuario (HU): Creación de Reserva de Pistas

**ID:** HU-01
**Título:** Seleccionar y confirmar pista y horario

## 1. Descripción
* **Como** socio o usuario externo del club de tenis.
* **Quiero** poder acceder a un calendario web para consultar la disponibilidad de las pistas y seleccionar una fecha, hora y tipo de pista.
* **Para** poder realizar una reserva de forma online, rápida y autónoma, sin dependeer del horario de atención de la recepción.

## 2. Criterios de Aceptación
Para considerar que esta funcionalidad está correctamente implementada, el sistema deberá comprobar automáticamente los siguientes puntos antes de dejar reservar al usuario:

1. **Filtro de búsqueda:** el usuario debe poder seleccionar fecha, hora, tipo de pista y modalidad. El sistema solo debe mostrarle las franjas horarias que realmente estén libres.
2. **Validación de pagos pendientes:** el sistema impedirá la reserva a socios que tengan recibos mensuales pendientes de pago.
3. **Límite de reservas por usuario:** para evitar el acaparamiento, el sistema validará que el usuario no supere el máximo de 2 reservas activas simultáneamente.
4. **Prevención de doble reserva:** el sistema debe cruzar datos en milisegundos para evitar dobles reservas en la misma pista y franja horaria por dos usuarios distintos a la vez.
5. **Confirmación final:** si se cumplen todas las reglas anteriores, el sistema confirmará la reserva automáticamente, generará un identificador único y enviará una confirmación al usuario (email o notificación).
6. 
