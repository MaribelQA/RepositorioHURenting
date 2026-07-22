---
name: qa-setup
agent: qa-setup
description: 'Setup inicial: configura la suite para tu proyecto (nombre, repos, Azure DevOps, MCP) y lo guarda en proyecto.config.md. Una sola vez por clon.'
argument-hint: Sin argumentos. Te hago las preguntas una a una. Escribe "reconfigurar" para volver a empezar.
---

# Setup — Configura la suite para tu proyecto

Ejecuta el agente `qa-setup`. Te pregunta **una sola vez** los datos del proyecto y los
escribe en `proyecto.config.md`, que el resto de agentes lee como contexto persistente
(no se vuelve a preguntar en cada sesión).

- **Cuándo**: justo después de clonar la suite para un proyecto nuevo, o si cambian los datos.
- **Salida**: `proyecto.config.md` completado y `estado_setup: completo`.

**Siguiente:** `/qa-clarificar` pegando tu HU, o —si configuraste el MCP de Azure DevOps—
`/qa-clarificar <id de work item>` para traerla por ID.

## 🔗 Conexiones
- Ejecuta: [[qa-setup.agent|qa-setup]]
