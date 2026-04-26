# Modelo de Datos ER Refinado (Especificación Técnica)

Para que el SIA sea operable, se ha refinado el modelo conceptual a un modelo lógico detallando tipos de datos, claves y campos críticos para la calidad del proceso.

## 1. Entidades y Atributos

### Entidad: SOCIO (System of Record: CRM)
Representa al usuario del club. La información maestra reside en el CRM y el SIA consume una réplica/caché para validaciones.
* **id_socio** (PK): String (Ej: "SOC-001") - *Clave Primaria*.
* **nombre_completo**: String.
* **email**: String - **[DATO CRÍTICO DQ]**: Debe seguir el patrón `regex` de email válido para asegurar la facturación y el envío del QR.
* **estado_membresia**: Enum (ACTIVO, SUSPENDIDO, CADUCADO).
* **permite_reserva**: Boolean - *Campo calculado por lógica de negocio del CRM*.

### Entidad: RESERVA
Entidad central que gestiona el ciclo de vida del servicio.
* **id_reserva** (PK): String (Ej: "RES-2026-001").
* **id_socio** (FK): String - *Relación N:1 con Socio*.
* **id_pista** (FK): String - *Relación N:1 con Pista*.
* **fecha_reserva**: Date.
* **franja_horaria**: Enum (H1, H2, H3...).
* **estado_reserva**: Enum (SOLICITADA, PENDIENTE_PAGO, CONFIRMADA, CANCELADA, CERRADA).
* **id_factura_erp**: String (Null hasta cierre).

### Entidad: PISTA (System of Record: SCM/IoT)
* **id_pista** (PK): String.
* **tipo_pista**: Enum (TIERRA, QUICK, PADEL).
* **estado_pista**: Enum (DISPONIBLE, OCUPADA, MANTENIMIENTO).

---

## 2. Diagrama de Relaciones Lógicas
1. Un **Socio** puede tener muchas **Reservas** (1:N).
2. Una **Reserva** pertenece a una única **Pista** en un momento dado (N:1).
3. Una **Reserva** genera un único registro de pago y, posteriormente, una **Factura** en el ERP (1:1).

---

## 3. Diccionario de Datos Críticos (DQ)
| Campo | Regla de Calidad | Momento de Validación | Acción en Fallo |
| :--- | :--- | :--- | :--- |
| `Socio.email` | Formato RFC 5322 | `EVT_04_CHECK_DQ_EMAIL` | Bloqueo de facturación y alerta. |
| `Reserva.fecha` | No puede ser pasada | Al crear solicitud | Error en frontend. |
