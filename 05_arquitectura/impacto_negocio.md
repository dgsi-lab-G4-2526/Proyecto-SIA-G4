# 📈 Análisis de Valor, Impacto en KPIs y Trade-offs

La materialización del prototipo y la especificación operable del proceso "To-Be" demuestran un valor de negocio directo para el Club de Tenis. A continuación, se detalla el impacto esperado sobre los indicadores clave y los compromisos asumidos.

## 1. Impacto Esperado en KPIs (Mecanismo y Target)

El MVP diseñado mejora significativamente la eficiencia operativa impactando directamente en al menos dos de nuestros KPIs principales:

### KPI 1: Tiempo de Reserva (Lead Time)
* **Mecanismo de Mejora:** Al automatizar la consulta al CRM (estado del socio) y habilitar una pasarela de pago integrada, se elimina por completo la necesidad de validación manual y cobro en la recepción física.
* **Target:** Reducir el tiempo total de gestión por reserva de un baseline de **10-15 minutos** (incluyendo esperas en recepción) a **menos de 2 minutos** de forma autónoma desde la App.

### KPI 2: Tasa de Cobro por App (Adopción Digital)
* **Mecanismo de Mejora:** La nueva interfaz (prototipo) incluye el pago como un paso obligatorio y fluido (sin fricciones) para generar el código QR de acceso a la pista. El diseño intuitivo y la confirmación inmediata incentivan su uso.
* **Target:** Aumentar el porcentaje de pagos digitales anticipados desde el **30% actual** (baseline) a un **> 85%** (target), mejorando la liquidez del club y reduciendo el dinero en efectivo en caja.

---

## 2. Análisis de Trade-offs (Riesgos vs. Beneficios)

Toda decisión arquitectónica y de negocio implica compromisos. La implementación de este proceso To-Be introduce el siguiente *trade-off*:

* **El Trade-off:** Dependencia Tecnológica vs. Eficiencia Operativa.
* **Riesgo / Coste introducido:** Al automatizar el acceso mediante pago online y código QR, el club adquiere una alta dependencia de servicios de terceros (Pasarela de pago Stripe, disponibilidad de la nube). Si el proveedor falla, el proceso central de negocio se detiene. Además, se asume el riesgo de "brecha digital" para aquellos socios de mayor edad menos habituados a usar apps.
* **Mecanismo de Control:** 1. **Tecnológico:** Se ha instrumentado la alerta operativa `ALT_01_PAYMENT_DROP` (definida en el BAM) que avisa instantáneamente si la pasarela cae, habilitando un "modo degradado" (fallback) que permite pagar en recepción temporalmente.
  2. **Negocio:** Se mantendrá un terminal físico en recepción (Terminal de Punto de Venta) operado por un empleado para asistir a los socios con dificultades digitales, garantizando que el coste de la digitalización no suponga la pérdida de clientes.
