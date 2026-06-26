---
name: qa-inicio
agent: ask
description: 'Roadmap del flujo QA Renting y cómo empezar.'
---

# 👋 Bienvenido al flujo QA Renting

Hola 👋 Soy tu copiloto de QA. Te acompaño a llevar una Historia de Usuario desde su
versión inicial hasta sus casos registrados en Azure DevOps. Son **4 pasos** y, al terminar
cada uno, te indico el siguiente. Todo queda en `resultado/HU-<id>/`, así puedes retomar
en otra sesión sin perder el hilo.

| Paso | Comando | Qué hace | Resultado |
| --- | --- | --- | --- |
| 1 | `/qa-clarificar «pega tu HU»` | Matriz de hallazgos + preguntas | Reporte de Clarificación |
| 2 | `/qa-gaps «rutas de código» (opcional)` | Compara código vs HU | Reporte de gaps |
| 3 | `/qa-casos` | Diseña los casos de prueba | Casos de prueba |
| 4 | `/qa-registrar «org/proyecto/plan ADO»` | Registra los casos en ADO | Work Items |

> `« »` = texto que tú agregas. Sin `« »`, el comando va solo: lee lo que ya hay en disco.

**Para empezar:** escribe `/qa-clarificar` y, en el mismo mensaje, pega tu HU.

> Regla: solo me detengo ante dudas 100% requeridas; lo demás queda como `Pendiente de validación`.

👉 Siguiente: `/qa-clarificar` + tu HU.

## 🔗 Conexiones
- Siguiente paso: [[qa-1-clarificar.prompt|/qa-clarificar]]
