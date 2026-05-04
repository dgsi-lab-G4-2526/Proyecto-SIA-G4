# Roles y Responsabilidades del Equipo - Proyecto TCMS

En la organización **dgsi-lab-G4-2526**, hemos definido los siguientes roles siguiendo el marco de trabajo Scrum para asegurar el éxito del sistema de gestión de transporte (TCMS).

## 1. Scrum Master (SM)
**Responsable:** Jimena González Sánchez
* **Gestión del Tablero:** Mantener actualizado el Backlog en GitHub Projects.
* **Facilitador:** Eliminar bloqueos técnicos y asegurar que el equipo siga la metodología.
* **Calidad de Gestión:** Asegurar que cada Issue tenga su Weight (puntos de historia) y Sprint asignado.

## 2. Product Owner (PO)
**Responsable:** Jesús Pulido Hernández
* **Visión de Negocio:** Definir la Promesa de Valor y los objetivos del sistema.
* **Priorización:** Decidir qué tareas son críticas (High Priority) para el cliente.
* **Definición de Procesos:** Liderar la creación del inventario de procesos y el detalle E2E.

## 3. Equipo de Desarrollo (Analistas)
**Responsables:** Jennifer García Guba y Álvaro Mora Sánchez
* **Diseño Técnico:** Crear la arquitectura conceptual del SIA.
* **Análisis de Requisitos:** Redactar las Historias de Usuario (HU).
* **Calidad del Dato:** Definir las reglas de validación y métricas (DQ).

## 4. Modelo de Gobernanza y Control Operativo

Para asegurar que el sistema se mantiene bajo control una vez en funcionamiento, el equipo asume las siguientes funciones de gobernanza basadas en los KPIs definidos:

### Supervisión de Negocio (Responsable: Jesús Pulido - PO)
* **Control de Ocupación:** Monitorizar semanalmente que la ocupación de pistas no baje del target establecido.
* **Acción ante desviaciones:** Si la ocupación es baja, coordinar con el club promociones de última hora basadas en los datos del sistema.

### ⚙️ Supervisión de Metodología y Flujo (Responsable: Jimena González - SM)
* **Control de Tiempos:** Revisar que el **Tiempo Medio de Reserva** no aumente debido a cuellos de botella técnicos o administrativos.
* **Acción ante desviaciones:** Facilitar sesiones de revisión para optimizar el flujo si el sistema se vuelve lento o ineficiente.

### 🛡️ Supervisión de Integridad y Calidad (Responsables: Jennifer García y Álvaro Mora - Analistas)
* **Control de Calidad del Dato (DQ):** Auditar periódicamente que el índice de errores en registros y pagos sea inferior al 1%.
* **Acción ante desviaciones:** Ajustar las **Reglas de Validación** si se detectan entradas de datos incoherentes o fraudulentas.

## 5. Protocolo de Actuación ante Eventos Críticos
La gobernanza del sistema se activa automáticamente ante los siguientes eventos:
1. **Alerta de Impago:** El sistema bloquea el acceso; el equipo de Administración es el único que puede autorizar el desbloqueo manual.
2. **Alerta de No-Show Recurrente:** Al tercer aviso, se genera un reporte automático para que la Dirección aplique la normativa de suspensión temporal.
