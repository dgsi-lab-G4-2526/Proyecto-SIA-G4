# Diagrama de Proceso As-Is (Issue #15)

Este diagrama representa el estado actual del club antes de la implementación del SIA. Se han detectado cuellos de botella en la gestión manual.

### Descripción del flujo actual:
1. El usuario llama o envía un WhatsApp para consultar disponibilidad.
2. El recepcionista anota en papel o Excel la reserva.
3. No hay validación de nivel ni de pagos pendientes.
4. El pago se realiza en efectivo al llegar, lo que genera colas y falta de registro.

```mermaid
graph TD
    A[Socio quiere jugar] --> B(Llamada/WhatsApp)
    B --> C{¿Hay pista?}
    C -- No --> D[Fin]
    C -- Sí --> E[Apunte manual en Excel]
    E --> F[Llegada al club]
    F --> G[Pago en efectivo]
    G --> H[Uso de pista]
