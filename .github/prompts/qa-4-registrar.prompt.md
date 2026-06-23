---
mode: agent
description: '(🟡 Esqueleto) Paso 4 del flujo QA: registra los casos de prueba como Work Items (Test Case) en Azure DevOps. Acción externa: confirma antes de escribir. A futuro vía MCP de Azure DevOps.'
tools: ['read', 'search', 'todo']
---

# Paso 4 — Registrar Test Cases en Azure DevOps

Adopta el rol y sigue las instrucciones del agente
[`qa-ado-registration`](../agents/qa-ado-registration.agent.md) (🟡 en refinamiento).
Respeta `.github/copilot-instructions.md`.

## Entrada
- Los **casos de prueba** del paso 3 (archivo en `casos-prueba/` o pegados en el chat).
- Destino en ADO: organización, proyecto, Test Plan / Suite.

## ⚠️ Precaución (acción externa e irreversible)
- **A futuro este paso se conectará al MCP de Azure DevOps** para crear los casos.
- **Nunca escribas en ADO sin confirmación explícita del usuario.** Antes de registrar:
  1. Muestra un **resumen de lo que se va a crear** (cuántos casos, títulos, destino).
  2. Pide confirmación clara ("sí, registrar").
  3. Solo entonces ejecuta el registro, idealmente caso por caso, reportando IDs/enlaces.
- No registres casos provenientes de un artefacto con estado `Bloqueado`.

## Qué debes hacer (cuando el MCP esté disponible)
1. Mapear `Title | Step Action | Stept Expected Result` a los campos del Test Case en ADO.
2. Vincular cada Test Case con la HU/PBI correspondiente (trazabilidad).
3. Generar un reporte de registro: IDs creados, enlaces y resultado por caso.

## Cierre del paso
> ✅ Registro completado. Fin del flujo QA. Resumen: casos creados, enlaces y pendientes (si los hay).
