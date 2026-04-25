# Definición de Contrato API (SIA a CRM)

## Objetivo de la integración

Esta integración permite que el Sistema de Información Automatizado (SIA) consulte al CRM para identificar al socio o usuario y validar si puede avanzar en el proceso de reserva.

## Endpoint propuesto

**Método:** `GET`
**Ruta:** `/socio/id`

## Finalidad funcional

El endpoint se utiliza en el momento en el que el usuario intenta comfirmar una reserva. Su obejtivo es devolver al SIA la imformacion minima necesaria para:

- identificar al socio de forma unívoca,
- conocer su estado de membresía,
- comprobar si existen bloqueos por impago,
- recuperar datos básicos de contacto,
- obtener atributos útiles de perfilado (por ejemplo, nivel de juego).


## Momento del proceso en el que se invoca

El SIA invoca esta API en la transición entre:

- **Reserva solicitada**
- **Reserva confirmada**

Es decir, después de que el usuario haya seleccionado fecha, hora, tipo de pista y modalidad, pero antes de bloquear definitivamente la pista y generar la confirmación.

## Request 

### Parámetros de entrada

El endpoint `GET /socio/id` recibirá como dato principal el identificador lógico del usuario.

#### Query params propuestos

- `id_socio` (string | obligatorio): identificador único del socio en el ecosistema del club.
- `include_profile` (boolean | opcional): indica si el SIA quiere recibir además datos de perfil, como nivel de juego o tipo de membresía.
- `include_contact` (boolean | opcional): indica si se deben devolver también los datos básicos de contacto para notificaciones.


## Response (respuesta)

### Datos mínimos que el CRM devuelve al SIA

El CRM devolverá los siguientes datos, porque son los estrictamente necesarios para la identificación y validación operativa del usuario en el proceso de reserva:

- id_socio: identificador único del socio.
- nombre_completo: nombre visible del usuario.
- email: dato de contacto principal.
- tipo_perfil: indica si es socio o usuario externo.
- estado_membresia: activa, suspendida, caducada.
- estado_pago_cuota: al corriente / impago / bloqueado.
- nivel_juego: dato de perfilado útil para servicios del club.
- permite_reserva: booleano calculado por el CRM según estado del cliente.
- motivo_bloqueo: texto explicativo si no puede reservar.


#### Ejemplo de respuesta exitosa

```json
{
  "id_socio": "SOC-000245",
  "nombre_completo": "nombre apellido1 apellido2",
  "email": "nombreapellido@gmail.es",
  "tipo_perfil": "SOCIO",
  "estado_membresia": "ACTIVA",
  "estado_pago_cuota": "AL_CORRIENTE",
  "nivel_juego": "NIVEL_2",
  "permite_reserva": true,
  "motivo_bloqueo": null
}
```

#### Ejemplo de respuesta con bloqueo

```json
{
  "id_socio": "SOC-000245",
  "nombre_completo": "nombre apellido1 apellido2",
  "email": "nombreapellido@gmail.es",
  "tipo_perfil": "SOCIO",
  "estado_membresia": "SUSPENDIDA",
  "estado_pago_cuota": "IMPAGO",
  "nivel_juego": "NIVEL_2",
  "permite_reserva": false,
  "motivo_bloqueo": "Socio con cuotas pendientes de pago"
}
```
## Reglas de negocio asociadas

Esta integración está directamente relacionada con reglas ya definidas en el proyecto:

### Validación de pagos pendientes

El sistema impedirá reservar a socios con recibos pendientes.

### CRM como dueño del dato de usuario
El SIA consume el dato, pero no es el sistema maestro del socio. Cualquier actualización de perfil debe sincronizarse con el CRM.

### Uso del email como dato crítico
El email es necesario porque afecta a notificaciones y facturación, y además ya se ha identificado como dato relevante para la calidad de la información.

### Códigos de respuesta propuestos
- 200 OK → socio identificado correctamente.
- 400 Bad Request → falta id_socio o formato inválido.
- 401 Unauthorized → token inválido o ausente.
- 404 Not Found → socio no encontrado en CRM.
- 409 Conflict → socio encontrado pero perfil inconsistente.
- 503 Service Unavailable → CRM no disponible temporalmente.

## Tratamiento en el SIA

Según la respuesta:

- Si `permite_reserva = true`, el SIA continúa con la confirmación de reserva.
- Si `permite_reserva = false`, el SIA bloquea la confirmación y muestra el motivo al usuario.
- Si el CRM no responde, el SIA no debe confirmar la reserva automáticamente, ya que estaría operando sin validar el estado maestro del socio.

