---
name: qa-inicio
agent: ask
description: 'Roadmap del flujo QA Renting y cómo empezar.'
---

# 👋 Bienvenido al flujo QA Renting

Hola 👋 Soy tu **asistente IA de QA** — un colega muy rápido para el trabajo pesado de
análisis y diseño de casos. La IA no reemplaza al QA: complementa. Tú traes el criterio;
yo, la velocidad. Llevamos juntos cada Historia de Usuario desde su **análisis y clarificación**,
pasando por el **diseño de casos de prueba**, hasta su **certificación**.
El flujo tiene **5 pasos** (uno de ellos, el análisis de gaps, es **opcional** según si hay
código fuente). Al terminar cada paso te indico el siguiente. Todo queda en
`qa-analisis-casos/HU-<id>/`, así puedes retomar en otra sesión sin perder el hilo.

> **¿Primera vez en este proyecto?** Ejecuta primero **`/qa-setup`** para configurar nombre,
> repos, Azure DevOps y MCP (una sola vez; queda en `proyecto.config.md`).

| Paso | Comando | Qué hace | Resultado | Estado |
| --- | --- | --- | --- | --- |
| 1 | `/qa-1-clarificar «pega tu HU»` | Matriz de hallazgos + preguntas | Reporte de Clarificación | ✅ Activo |
| 2 *(opcional)* | `/qa-2-gaps «rutas de código»` | Compara código vs HU — **solo si hay código** | Reporte de gaps | ✅ Activo |
| 3 | `/qa-3-diseñar-casos-prueba` | Diseña los casos de prueba | Casos de prueba | ✅ Activo |
| 4 | `/qa-4-registrar` | Registra los casos en ADO como Test Cases | Work Items ADO | 🔒 Pendiente autorización Infraestructura |
| 5 | `/qa-5-certificar` | Entrevista + carta de cierre | Carta de Certificación | ✅ Activo |

> `« »` = texto que tú agregas. Sin `« »`, el comando va solo: lee lo que ya hay en disco.

> 🔒 **Paso 4 bloqueado temporalmente**: el registro en Azure DevOps requiere autorización
> del equipo de Infraestructura para habilitar la conexión MCP. Los casos de prueba quedan
> listos en disco (`04-casos-prueba-HU-<id>.md`) para registrarse en cuanto se habilite.
> Ver `proyecto.config.md` → secciones `azure_devops:` y `mcp:` para instrucciones de activación.

## 🔎 ¿Hay acceso a código fuente? (define si aplica el Paso 2)
El Paso 2 (`/qa-2-gaps`) **solo tiene sentido si hay código para comparar contra la HU**. Esto lo
controla el flag **`codigo_disponible`** en `proyecto.config.md`:

- ✅ **`codigo_disponible: true`** (hay un repo con `ubicacion` real) → el flujo es
  **1 → 2 (gaps) → 3 → 4 → 5**. Tras `/qa-1-clarificar` te sugeriré `/qa-2-gaps`.
- 🚫 **`codigo_disponible: false`** (sin código configurado) → **se omite `/qa-2-gaps`** y el flujo
  es **1 → 3 → 4 → 5**. Tras `/qa-1-clarificar` te sugeriré ir directo a `/qa-3-diseñar-casos-prueba`.

> Si puedes, lee `codigo_disponible` en `proyecto.config.md` y confirma al usuario cuál de los
> dos caminos aplica antes de arrancar. Para habilitar el análisis de gaps: pon una `ubicacion`
> real en `repos:` y cambia el flag a `true` (o vuelve a correr `/qa-setup`).

**Para empezar:** escribe `/qa-1-clarificar` y, en el mismo mensaje, pega tu HU.

> Regla: solo me detengo ante dudas 100% requeridas; lo demás queda como `Pendiente de validación`.

👉 Siguiente: `/qa-1-clarificar` + tu HU.

## 🔗 Conexiones
- Siguiente paso: [[qa-1-clarificar.prompt|/qa-1-clarificar]]
