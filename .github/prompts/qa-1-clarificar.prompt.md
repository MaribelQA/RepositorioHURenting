---
name: qa-clarificar
agent: qa-refinement
description: 'Paso 1: clarifica la HU pegada en el chat (matriz de hallazgos + preguntas). No reescribe la HU.'
---

# Paso 1 — Clarificar la HU

Ejecuta el agente `qa-refinement` sobre la HU **pegada o adjunta en este mismo mensaje**.
Sin HU disponible, pídela y detente. Sigue tu definición de agente y la constitución.

- **Entrada**: la HU en el chat.
- **Salida** (se guarda siempre en `resultado/HU-<id>/`): `01-HU-<id>.md` (copia literal),
  `02-reporte-clarificacion-HU-<id>.md` y `00-estado-HU-<id>.md`. El `<id>` se extrae de la HU;
  si no aparece, pregúntalo una vez.

**Siguiente:** sin bloqueantes → `/qa-gaps` o directo a `/qa-casos`. Los pendientes no bloqueantes
no detienen el flujo.
