# Sprint Review y Evidencias Agile (Sprint 3)

Este documento resume los resultados del tercer Sprint, detallando el cumplimiento de los objetivos y proporcionando los enlaces de trazabilidad que validan la operabilidad del MVP.

---

## 1. Sprint Goal (Meta del Sprint)
**"Diseñar un MVP interactivo del proceso To-Be y definir su especificación operable E2E, garantizando la viabilidad técnica y la monitorización de KPIs."**

Al cierre del Sprint, el equipo ha materializado este objetivo mediante la entrega de un prototipo funcional, un modelo de datos refinado, contratos de integración y un sistema de control de calidad (BAM).

---

## 2. Sprint Review: Resultados del Incremento
A continuación, se detalla lo logrado por cada rol bajo la supervisión de la Scrum Master:

* **Producto (PO):** Se han especificado los contratos API síncronos con el CRM y el ERP, además de una arquitectura de eventos asíncrona para asegurar que el sistema es "operable" y no solo un diseño visual.
* **Diseño (UX/UI):** Se ha entregado un MVP interactivo en Figma que cubre el flujo feliz del socio y, críticamente, el flujo de error en el pago para demostrar la resiliencia del sistema.
* **Desarrollo/Lógica:** Se ha refinado el modelo de datos (ER) y se han redactado las reglas de negocio, estados y excepciones, permitiendo que cualquier equipo de desarrollo pueda implementar la solución sin ambigüedades.
* **Control (SM):** Se ha instrumentado el proceso para capturar KPIs en tiempo real (BAM) y se ha definido una matriz RACI para la gestión de alertas operativas.

---

## 3. Evidencias de Gestión en GitHub (Issue #43)

La gestión del proyecto se ha realizado siguiendo ceremonias Scrum, dejando constancia de la trazabilidad en los siguientes puntos:

### 3.1. Gestión de Tareas (Board & Issues)
* **Tablero Kanban:** Se ha mantenido un flujo de trabajo dinámico. Todas las tareas (Issues) han pasado por los estados de `To Do`, `In Progress` y `Done`.
* **Enlace al Tablero:** https://github.com/orgs/dgsi-lab-G4-2526/projects/2/views/1
* **Issues Cerradas:** Se han resuelto y documentado 13 issues técnicas vinculadas a este Sprint.

### 3.2. Control de Versiones y Colaboración
* **Commits y Trazabilidad:** Cada pieza de documentación y código se ha subido mediante commits vinculados a sus respectivas Issues (usando `closes #XX`).
* **Enlace al Repositorio:** https://github.com/dgsi-lab-G4-2526/Proyecto-SIA-G4

### 3.3. Prototipo Demostrable
* **Enlace a Figma:** https://www.figma.com/make/4NGAQNmrLMYUQAFc4qWJTk/Pantalla-de-Reserva-Deportiva?p=f&fullscreen=1 
* **Capturas de Evidencia:** Disponibles en la carpeta `/06_prototipo/` del repositorio, incluyendo la pantalla de "Modo Degradado" ante fallos de pasarela.

---

## 4. Conclusiones y Retrospectiva Rápida
El equipo ha superado el reto de integrar sistemas externos (CRM/ERP) en la lógica del proceso. Como mejora para futuros Sprints, se identifica la necesidad de estandarizar los formatos JSON desde el inicio para acelerar la integración entre el modelo de datos y los contratos API.
