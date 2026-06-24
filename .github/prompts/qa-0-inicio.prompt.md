---
name: qa-inicio
agent: ask
description: 'Roadmap del flujo QA Renting y cómo empezar.'
---

# 🚦 Flujo QA Renting

Acompaño una HU desde su versión inicial hasta sus casos registrados en Azure DevOps.
Son 4 pasos; al terminar cada uno te indico el siguiente. Todo se guarda en
`resultado/HU-<id>/`, así puedes retomar en otra sesión.

| Paso | Comando | Qué hace | Resultado |
| --- | --- | --- | --- |
| 1 | `/qa-clarificar` | Matriz de hallazgos + preguntas | Reporte de Clarificación |
| 2 | `/qa-gaps` | Compara código vs HU | Reporte de gaps |
| 3 | `/qa-casos` | Diseña los casos de prueba | Casos de prueba |
| 4 | `/qa-registrar` | Registra los casos en ADO | Work Items |

**Empezar:** escribe `/qa-clarificar` y, en el mismo mensaje, pega tu HU.

> Regla: se bloquea solo ante dudas 100% requeridas; lo demás queda como `Pendiente de validación`.

👉 Siguiente: `/qa-clarificar` + tu HU.
