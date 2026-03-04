# Riesgos del Proceso de Reservas
En el marco de la implantación del nuevo SIA para el club de tenis, se han identificado los siguientes riesgos operativos y tñecnicos que pueden afectar al proceso E2E, junto con sus respectivas estrategias de mitigación.

## Riesgo 1: Condiciones climáticas adversas
* **Descripción:** dado que muchas pistas son al aire libre, la lluvia impide el uso efectivo de la pista tras haber sido reservada y confirmada,lo que afecta a los ingresos y a la satisfaccion del socio.
* **Mitigación:** implementar en el SIA un estado de "Cancelación por fuerza mayor" que se active de forma masiva para las pistas exteriores afectadas. El sistema notificará automáticamente a los usuarios por SMS/Email y ofrecerá la reprogramación de la reserva o la devolución/saldo a favor sin aplicar la regla de penalización por cancelaciín tardía.

## Riesgo 2: Doble reserva por concurrencia
* **Descripción:** dos usuarios intentan reservar la misma pista y la misma franja horaria exactamente en el mismo segundo, generando un conflicto de datos y una reserva duplicada (algo muy común en horarios de máxima demanda).
* **Mitigación:** configurar validaciones a nivel de base de datos (control de concurrencia y transaccionalidad). Cuando un usuaroio selecciona una hora, se aplica un "bloqueo temporal" de esa franja mientras confirma la operación, impidiendo que a otros usuarios les aparezca como disponible.

## Riesgo 3: Caída del sistema del servidor
* **Descripción:** el servidor donde se aloja el SIA se cae o pierde conectividad, impidiendo que el personal de recepción consulte el panel de seguimiento para validar quién entra a las pistas, paralizando la operativa del club.
* **Mitigación:** desplegar la arquitectura del sistema en un entorno Cloud con alta disponibilidad. Además, habilitar la generación de reportes automáticos exportables como mecanismo de contingencia offline.

## Riesgo 4: Alto índice de reservas "No-Show"
* **Descripción:** los usuarios reservan pero no se presentan, dejando las pistas vacías y mermando tanto la ocupación real como la recaudación económica.
* **Mitigación:** implementar un sistema automatizado de recordatorios y aplicar de manera estricta las reglas de negocio: registrar penalizaciones económicas o bloquear la capacidad de reserva online durante 15 días a los usuarios reincidentes que acumulen 3 faltas.
