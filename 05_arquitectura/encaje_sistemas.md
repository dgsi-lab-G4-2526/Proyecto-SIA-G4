# Encaje de Sistemas Corporativos (CRM/ERP/SCM)

Para que el club de tenis opere de forma profesional y escalable, el SIA (Sistema de reservas) no debe trabajar aislado, sino que se integra con sistemas de gestión empresarial estándar.

## 1. Distribución de responsabilidades en el negocio

En el ecosistema tecnológico de nuestro club de tenis, cada sistema corporativo asume un rol específico para cubrir las distintas necesidades operativas y estratégicas:

**CRM (Customer Relationship Management)**
El CRM es el sistema responsable de gestionar toda la relación con el socio y su ciclo de vida dentro del club. Su principal objetivo abarca la fidelización, el marketing y la atención al cliente. En nuestro modelo de negocio, este sistema resulta fundamental para entender el comportamiento de los usuarios, como su nivel de juego, la frecuencia con la que reservan pistas o sus ratios de cancelación. Gracias a esta información centralizada, el club puede lanzar campañas promocionales automatizadas en horas valle, organizar torneos segmentados por categorías o diseñar estrategias para recuperar a socios que llevan meses inactivos.

**ERP (Enterprise Resource Planning)**
El ERP actúa como la columna vertebral financiera y administrativa de la organización. Se encarga de centralizar la contabilidad, la facturación, la gestión de recursos humanos (como las nóminas de los monitores) y el mantenimiento de los activos principales, que en este caso son las pistas. Su aplicación es crítica porque permite agrupar todos los flujos económicos del club en un solo lugar: desde los cobros automatizados de las cuotas mensaules de los socios, hasta los pagos puntuales por reservas diarias y los gastos de suminitros. Esto garantiza una trazabilidad financiera total y facilita enormemente tareas diarias como el cierre de caja en recepción.

## 2. Integraciones relevantes del proceso E2EE

Para que el proceso End-to-End (desde la solicitud de la pista hasta la facturación) fluya de manera automatizada y sin intervención manual, hemos diseñado dos integraciones fundamentales:

### Integración 1: Validación y perfilado de socio (SIA <-> CRM)
* **Qúe dato fluye:** ID_Socio, Estado de membresía y Nivel de juego.
* **Quién envía/Quién recibe:** el SIA envía la petición al CRM en el momento exacto en el que el usuario intenta confirmar una reserva. El CRM (como dueño del dato) evalúa el estado cliente y devuelve una respuesta indicando si tiene permisos para reservar o si debe ser bloqueado por impago de cuotas.

### Integración 2: Registro de facturación operativa (SIA <-> ERP)
* **Qúe dato fluye:** ID_Reserva, ID_Usuario, Importe (socio vs externo), Método de pago, Fecha.
* **Quién envía/Quién recibe:** una vez que la pista ha sido utilizada y la reserva pasa el estado final de "Cerrado", el SIA envía estos datos consolidados al ERP. El ERP lo procesa para generar el apunte contable y emite la factura electrónica correspondiente, garantizando el control de los ingresos por franja horaria (KPI clave del negocio).
