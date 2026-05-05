# Evolución del Prototipo

En este Sprint hemos iterado sobre el prototipo interactivo inicial para incorporar las reglas de control y validaciones de negocio definidas por el equipo para garantizar la rentabilidad y optimización del club.

No se ha rediseñado el producto desde cero, sino que se han aplicado "Quicks Wins" visuales que aportan gran valor:

### 1. Control de Límite de Reservas
* **Regla aplicada:** Límite de reservas activas.
* **Implementación en Figma:** Se ha añadido un indicador visual en la pantalla de búsqueda ("Tienes 1 de 2 reservas activas permitidas") para que el sistema prevenga el acaparamiento de pistas de forma transparente para el socio.

**Captura de la mejora:**
<img width="464" height="793" alt="image" src="https://github.com/user-attachments/assets/f162635d-16a2-4ba3-91ad-b807a3ffd19a" />

---

### 2. Transparencia en la Política de Cancelaciones
* **Reglas aplicadas:** Plazo de cancelación gratuita y Penalización por cancelación tardía.
* **Implementación en Figma:** Para reducir el ratio de pistas vacías y los No-Shows, se ha introducido un Checkbox obligatorio en la pasarela de pago. El usuario debe aceptar explícitamente la política de cancelación (gratuita hasta 24h antes y penalización del 50% por cancelación tardía) antes de poder abonar la reserva.

**Captura de la mejora:**
<img width="455" height="689" alt="image" src="https://github.com/user-attachments/assets/ba43f21a-c585-4151-8e2e-097d6a2206f3" />

---

### 3. Validación de integridad de datos (Data Quality)
* **Reglas aplicadas:** Validaciones de integridad del proceso (Data Quality - DQ) para asegurar la calidad del dato en la facturación.
* **Implementación en Figma:** Se ha diseñado un estado de error en tiempo real en la pasarela de pago. Si el usuario introduce una tarjeta no válida o incompleta, el sistema muestra una alerta visual en rojo ("Número de tarjeta incompleto") que bloquea el botón de confirmación. Esto previene errores de cobro y asegura la calidad de los datos antes de intentar procesar la transacción.

**Captura de la mejora:**
<img width="456" height="470" alt="image" src="https://github.com/user-attachments/assets/609d60f2-6f16-40b9-b74c-e1faff2ece8e" />


**Enlace al prototipo interactivo actualizado:**
https://www.figma.com/make/4NGAQNmrLMYUQAFc4qWJTk/Pantalla-de-Reserva-Deportiva?t=vw9c95wIuoZoKS70-1
