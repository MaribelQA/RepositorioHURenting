---
mode: agent
description: '(🟡 Esqueleto) Paso 2 del flujo QA: compara el código fuente contra la HU refinada y detecta vacíos en ambas direcciones. Produce un reporte de gaps.'
tools: ['read', 'search', 'todo']
---

# Paso 2 — Análisis de Gaps (Código vs HU)

Adopta el rol y sigue las instrucciones del agente
[`qa-gap-analysis`](../agents/qa-gap-analysis.agent.md) (🟡 en refinamiento).
Respeta `.github/copilot-instructions.md`.

## Entrada
- La **HU refinada** del paso 1 (pegada en el chat, o en `hu-directory/*_refinement.md` si se guardó).
- Opcional: rutas/módulos del código a inspeccionar.

Si no hay HU refinada disponible, pídela o sugiere volver a `/qa-clarificar`.

## Qué debes hacer
1. Mapear cada criterio de aceptación de la HU contra evidencia en el código.
2. Detectar **Gaps HU→Código** (criterios sin implementación) y **Gaps Código→HU**
   (comportamientos del código no descritos en la HU).
3. No suponer implementación: si no hay evidencia, marca el gap explícitamente.

## Cierre del paso (encadenamiento)
> ✅ Gaps identificados. **Siguiente paso recomendado:** `/qa-casos` (diseñar casos de prueba),
> considerando los gaps detectados para reforzar la cobertura.
