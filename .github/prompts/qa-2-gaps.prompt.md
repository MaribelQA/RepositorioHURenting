---
name: qa-gaps
agent: qa-gap-analysis
description: 'Paso 2: compara el código vs la HU + su clarificación y detecta vacíos.'
argument-hint: (opcional) rutas o módulos de código a comparar. Sin esto, uso lo que haya en el folder de la HU.
---

# Paso 2 — Análisis de Gaps (Código vs HU)

Ejecuta el agente `qa-gap-analysis`. Lee primero `00-estado-HU-<id>.md`, `01` y `02` del folder.

- **Entrada**: `resultado/HU-<id>/` (`01`, `02`) + rutas/módulos de código (opcional).
- **Salida**: `resultado/HU-<id>/03-reportes-gaps-HU-<id>.md` (se guarda siempre) + actualizar `00`.

Sin `02` disponible, vuelve a `/qa-clarificar`. Un `02` Parcial no bloquea.

**Siguiente:** `/qa-casos`.

## 🔗 Conexiones
- Ejecuta: [[qa-gap-analysis.agent|qa-gap-analysis]]
- Siguiente paso: [[qa-3-casos.prompt|/qa-casos]]
