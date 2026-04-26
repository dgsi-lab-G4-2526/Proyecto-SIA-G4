# Diseño de Alertas y Notificaciones de Control

Para asegurar que el proceso To-Be se mantiene dentro de los parámetros de calidad esperados y mitigar los riesgos operativos, se ha definido el siguiente protocolo de alerta automatizada (BAM mínimo).

## Ficha de Alerta Operativa: Caída de Tasa de Cobro

Se establece una alerta crítica vinculada al KPI de "Tasa de Cobro por App" para detectar de forma temprana problemas en la pasarela de pagos o en la conexión con el ERP.

* **ID de Alerta:** `ALT_01_PAYMENT_DROP`
* **Métrica Monitorizada:** Tasa de cobro a través de la aplicación.
* **Umbral de Activación (Threshold):** Más de 3 pagos rechazados o abandonados por error técnico (evento `EVT_03_FALLO_PAGO_TECNICO`) en un periodo de **15 minutos**.

### Matriz de Responsabilidades (RACI Operativo)
Define quién debe actuar y quién debe estar informado cuando se dispara el umbral:

* **[R] Responsible (Quien ejecuta la acción):** Soporte Técnico / Administrador del Sistema.
* **[A] Accountable (Responsable final):** Gerente del Club de Tenis.
* **[C] Consulted (A quien se consulta):** Proveedor de la pasarela de pagos (ej. Soporte de Stripe).
* **[I] Informed (A quien se informa):** Personal de Recepción (para que sepan que los socios no pueden pagar por la app).

### Acción Esperada (Playbook de Mitigación)
Cuando la alerta `ALT_01_PAYMENT_DROP` se dispara, el sistema y el equipo deben ejecutar automáticamente el siguiente protocolo:

1. **Acción del Sistema:** Desactivar temporalmente la obligatoriedad del pago online para reservar y mostrar el aviso en pantalla: *"Servicio de pago temporalmente no disponible. Su reserva está confirmada, por favor abone la pista en recepción"*.
2. **Acción Humana (Soporte Técnico):** Revisar los logs de la pasarela de pago para identificar el código de error y abrir un ticket de incidencia con el proveedor externo.
