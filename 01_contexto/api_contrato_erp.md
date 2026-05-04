# Definición de Contrato API (SIA a ERP)

## Objetivo de la integración

Esta integracion permmite que el Sistema de Imformacion Automatizado (SIA) envíe al ERP la información necesaria para generar la factura legal asociada a una reserva ya completada.

## Endpoint propuesto

**Método:** `POST`
**Ruta:** `/factura`

## Finalidad funcional

El endpoint se utiliza cuando una reserva ha sido utilizada correctamente y ya dispone de la información necesaria para ser facturada.

Su objetivo es enviar al ERP los datos mínimos del servicio prestado, del cliente y del pago, para que el sistema financiero pueda:

- generar la factura legal,
- registrar el ingreso,
- mantener la trazabilidad económica de la operación,
- y consolidar la información contable asociada a la reserva.

## Momento del proceso en el que se invoca

El SIA invoca esta API en la transición entre:

- **Pago confirmado**
- **Reserva cerrada / Facturación**

la llamada al ERP no se realiza en la fase inicial de reserva, sino cuando el proceso ya ha alcanzado un estado suficientemente sólido como para justificar la emisión de una factura legal.



## Request (petición)

### Datos mínimos que el SIA envía al ERP

El endpoint `POST /factura` debe recibir los siguientes datos mínimos:

- `id_factura_origen_sia`: identificador interno de la solicitud de facturación generada por el SIA.
- `id_reserva`: identificador único de la reserva.
- `fecha_emision`: fecha en la que se solicita la factura.
- `id_socio`: identificador del cliente.
- `nombre_cliente`: nombre completo del cliente.
- `email_cliente`: email asociado al cliente.
- `tipo_cliente`: socio o usuario externo.
- `concepto`: descripción del servicio facturado.
- `fecha_reserva`: fecha en la que se utilizó la pista.
- `hora_inicio`: hora de inicio del servicio.
- `hora_fin`: hora de fin del servicio.
- `id_pista`: identificador de la pista.
- `tipo_pista`: tipo de pista reservada.
- `modalidad`: modalidad de juego o reserva.
- `importe_base`: importe base del servicio.
- `descuento_aplicado`: descuento aplicado.
- `importe_total`: importe final a facturar.
- `moneda`: divisa usada.
- `metodo_pago`: método de pago registrado.
- `estado_pago`: estado final del pago.
- `estado_reserva_final`: estado final de la reserva en el SIA.
- `timestamp_pago_confirmado`: instante en el que el pago fue confirmado.
- `origen`: sistema emisor del mensaje.
- `correlation_id`: identificador de trazabilidad para la integración.


### Ejemplo de petición

```json
{
  "id_factura_origen_sia": "FAC-SIA-2026-000145",
  "id_reserva": "RES-2026-000982",
  "fecha_emision": "2026-04-25",
  "id_socio": "SOC-000245",
  "nombre_cliente": "Nombre apellido1 apellido2",
  "email_cliente": "Nombreapellido@gmail.es",
  "tipo_cliente": "SOCIO",
  "concepto": "Reserva de pista de tenis",
  "fecha_reserva": "2026-04-24",
  "hora_inicio": "18:00",
  "hora_fin": "19:30",
  "id_pista": "P-03",
  "tipo_pista": "TIERRA",
  "modalidad": "DOBLES",
  "importe_base": 14.00,
  "descuento_aplicado": 2.00,
  "importe_total": 12.00,
  "moneda": "EUR",
  "metodo_pago": "APP",
  "estado_pago": "CONFIRMADO",
  "estado_reserva_final": "RESERVA_CERRADA",
  "timestamp_pago_confirmado": "2026-04-24T19:35:00Z",
  "origen": "SIA",
  "correlation_id": "req-2026-04-25-0104"
}
```




## Response (respuesta)

### Datos mínimos que el ERP devuelve al SIA

El ERP devolverá la información necesaria para confirmar si la factura ha sido generada correctamente o si se ha producido algún error durante la validación o registro contable.

#### Ejemplo de respuesta exitosa

```json
{
  "id_factura_erp": "ERP-FAC-00054321",
  "estado": "FACTURA_GENERADA",
  "fecha_registro": "2026-04-25T10:20:15Z",
  "mensaje": "Factura registrada correctamente en ERP"
}
```

#### Ejemplo de respuesta con error

```json
{
  "estado": "ERROR_VALIDACION",
  "codigo_error": "ERP-422",
  "mensaje": "El importe_total no coincide con el detalle económico"
}
```

### Relación con BAM y trazabilidad

Este contrato API está directamente relacionado con la arquitectura de eventos y monitorización del sistema.

La secuencia esperada es la siguiente:

El usuario completa el flujo de pago.
El sistema registra TX_PAYMENT_SUCCESS.
El SIA ejecuta la validación técnica de correo mediante EVT_04_CHECK_DQ_EMAIL.
Si ambas condiciones son satisfactorias, el SIA invoca POST /factura.
El ERP responde confirmando o rechazando la generación de la factura.
El SIA registra el resultado y continúa el cierre del proceso.


## Reglas de negocio asociadas

Esta integración está directamente relacionada con reglas ya definidas en el proyecto:

### 1. Solo se factura una reserva válida

El SIA no debe solicitar factura para una reserva cancelada o incompleta. La facturación solo debe producirse cuando la reserva ha sido utilizada o cuando el modelo de negocio del club justifique el cobro.

### 2. El ERP es el dueño del dato financiero

El SIA puede conocer el estado operativo de una reserva y registrar la confirmación del pago.

### 3. La calidad del dato condiciona la facturación

Para que la integración funcione correctamente, los datos enviados al ERP deben ser consistentes. Esto afecta a elementos como:

- la identificación del socio,
- el email de contacto,
- el importe total,
- la relación entre reserva, pista y franja horaria,
- y el estado final de pago.

### 4. Trazabilidad entre reserva y factura

La existencia de `id_reserva` e `id_factura_origen_sia` permite mantener la trazabilidad entre el servicio operativo gestionado por el SIA y la factura legal consolidada en el ERP.

### 5. Dependencia de eventos BAM previos

La solicitud de factura al ERP solo debe ejecutarse si previamente se ha registrado un evento TX_PAYMENT_SUCCESS y la validación EVT_04_CHECK_DQ_EMAIL ha sido satisfactoria.

## Códigos de respuesta propuestos

- 201 Created → factura generada correctamente en ERP.
- 400 Bad Request → faltan campos obligatorios o formato inválido.
- 401 Unauthorized → token inválido o ausente.
- 409 Conflict → la reserva ya fue facturada anteriormente.
- 422 Unprocessable Entity → inconsistencia económica o fiscal en los datos enviados.
- 503 Service Unavailable → ERP no disponible temporalmente.

## Tratamiento en el SIA

Según la respuesta del ERP:

- Si el ERP devuelve confirmación de factura generada, el SIA actualiza la reserva como cerrada y registra el identificador de factura.
- Si el ERP devuelve error de validación, el SIA no debe cerrar completamente la operación financiera y debe dejar constancia del fallo.
- Si el ERP no responde, el SIA puede dejar la reserva en un estado intermedio de pendiente de facturación hasta que la integración se recupere.

