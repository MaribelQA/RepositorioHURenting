<!--
TEMPLATE — Reporte de Clarificación (Paso 1, agente qa-refinement).
Cópialo a `resultado/HU-<id>/02-reporte-clarificacion-HU-<id>.md` y reemplaza los <placeholders>.
NO es una HU reescrita: registra hallazgos, respuestas y pendientes. La HU original vive
intacta en `01-HU-<id>.md`. Mantén las secciones; usa "ninguno"/"—" donde no aplique.
-->

# Reporte de Clarificación — HU-<id>

## 1. Resumen del análisis
- **HU analizada**: <id> — <título, citado solo como referencia>
- **Veredicto**: <Sin bloqueantes, se puede avanzar | Con bloqueantes 100% requeridos>
- **Preguntas hechas / respondidas**: <n> / <n>

## 2. Matriz de hallazgos
> Fuente única. Se actualiza a medida que se responden las preguntas.

| # | Categoría | Hallazgo | Tipo | Severidad | Estado | Pregunta |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | <categoría> | <qué falta, es ambiguo o se contradice> | Ambigüedad / Omisión / Inconsistencia / No testable / Sin sentido / Alcance / Escenario | Alta / Media / Baja | Resuelto / Pendiente de validación / Bloqueante | P1 |

## 3. Bitácora de aclaraciones
### Sesión <AAAA-MM-DD>
- P: <pregunta> → R: <respuesta confirmada>
- P: <pregunta> → R: <respuesta confirmada>

## 4. Pendientes de validación (no bloqueantes)
> Filas `Pendiente de validación` de la matriz. No impiden avanzar; quedan registradas.

- <pendiente> — _responsable sugerido: <PO/QA/…>_

## 5. Bloqueantes (100% requeridos)
> Solo lo que **impide** entender la HU o haría incorrectos los casos. Si hay contenido aquí,
> el Hand-off va con `Estado: Bloqueado`. Si no, escribe "ninguno".

- <bloqueante o "ninguno">

## 6. Sugerencias de criterios de aceptación para el PO _(opcional)_
> Propuestas para validación del PO. **No son criterios cerrados.**

- **SUGERENCIA — requiere validación del PO**: <criterio propuesto>

---
## 🔗 Hand-off
- **Artefacto**: `resultado/HU-<id>/02-reporte-clarificacion-HU-<id>.md`
- **Producido por**: qa-refinement
- **Estado**: Completado | Parcial (pendientes no bloqueantes) | Bloqueado
- **Pendientes de validación**: <lista o "ninguno">
- **Siguiente agente sugerido**: @qa-gap-analysis (o @qa-test-design)
- **Notas para el siguiente agente**: <contexto relevante en 1-3 líneas>
---

## 🔗 Conexiones
- Siguiente artefacto: [[03-reportes-gaps.template]]
