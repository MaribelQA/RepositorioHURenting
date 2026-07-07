---
name: qa-clarificar
agent: qa-refinement
description: 'Paso 1: clarifica la HU —pegada en el chat o traída por ID vía MCP— (matriz de hallazgos + preguntas). No reescribe la HU.'
argument-hint: Pega tu HU (texto o adjunto), o da solo el número de work item si tienes MCP de ADO configurado.
---

# Paso 1 — Clarificar la HU

Ejecuta el agente `qa-refinement` sobre la HU. Entra por una de dos vías (constitución §3.1):
**pegada/adjunta en este mensaje**, o —si configuraste el MCP de ADO en `/qa-setup`— dando
solo el **número de work item** para traerla por ID. Sin HU disponible, pídela y detente.

- **Entrada**: la HU en el chat, o un ID de work item (con MCP de ADO).
- **Salida** (se guarda siempre en `resultado/HU-<id>/`): `01-HU-<id>.md` (copia literal),
  `02-reporte-clarificacion-HU-<id>.md` y `00-estado-HU-<id>.md`. El `<id>` se extrae de la HU;
  si no aparece, pregúntalo una vez.

**Siguiente:** sin bloqueantes **y** sin pendientes con `Impacta diseño de pruebas: Sí` → `/qa-gaps` o directo a `/qa-casos`.
Si persisten pendientes con `Impacta diseño de pruebas: Sí`, `/qa-clarificar` puede **re-invocarse** sobre la misma HU: el agente lee el `02` existente y abre una nueva ronda exclusivamente sobre esos pendientes, sin reescribir lo ya resuelto.

## 🔗 Conexiones
- Ejecuta: [[qa-refinement.agent|qa-refinement]]
- Siguiente paso: [[qa-2-gaps.prompt|/qa-gaps]]
