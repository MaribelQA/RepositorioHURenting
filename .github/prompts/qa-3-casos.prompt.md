---
mode: agent
description: '(🟡 Esqueleto) Paso 3 del flujo QA: diseña casos de prueba a partir de la HU original + su Reporte de Clarificación (y los gaps), aplicando técnicas de diseño y las plantillas del repo. Salida lista para Azure DevOps.'
tools: ['read', 'edit', 'search', 'todo']
---

# Paso 3 — Diseño de Casos de Prueba

Adopta el rol y sigue las instrucciones del agente
[`qa-test-design`](../agents/qa-test-design.agent.md) (🟡 en refinamiento).
Respeta `.github/copilot-instructions.md` y usa estas referencias del repo:
- Principios: `promts/test-case-creation-principles.md`
- Plantilla: `plantillas/test-case-template.md`
- Formato ADO: `plantillas/formato-cp.md`

## Entrada
- La **HU original + su Reporte de Clarificación** (paso 1) y, opcionalmente, el **reporte de gaps** (paso 2).

Si no hay Reporte de Clarificación, pídelo o sugiere volver a `/qa-clarificar`. No avances si está `Bloqueado`.

## Qué debes hacer
1. Diseñar casos atómicos (positivo / negativo / borde) a partir de los criterios de la HU
   y las respuestas registradas en el Reporte de Clarificación.
2. Seguir la plantilla **sin omitir secciones**; cada caso valida un solo comportamiento.
3. No generar casos basados en suposiciones no confirmadas (respeta pendientes/bloqueantes del reporte).
4. Los criterios marcados como *sugerencia para el PO* solo se usan si fueron validados; si no, márcalos como cobertura pendiente.

## Cierre del paso (encadenamiento)
> ✅ Casos diseñados. **Siguiente paso recomendado:** `/qa-registrar` para crearlos en Azure DevOps.
