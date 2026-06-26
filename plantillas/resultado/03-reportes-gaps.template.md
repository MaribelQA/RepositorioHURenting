<!--
TEMPLATE — Reporte de Gaps Código vs HU (Paso 2, agente qa-gap-analysis).
Cópialo a `resultado/HU-<id>/03-reportes-gaps-HU-<id>.md` y reemplaza los <placeholders>.
Debe ser CLARO y presentable a la PO: empieza por el resumen ejecutivo.
No suponer implementación: si no hay evidencia en el código, marca el gap explícitamente.
-->

# Reporte de Gaps (Código vs HU) — HU-<id>

## 1. Resumen para la PO
> 3–6 líneas, sin tecnicismos: qué se revisó, cuántos gaps se hallaron y el riesgo principal.

- **HU**: <id> — <título>
- **Alcance revisado**: <módulos/rutas/endpoints inspeccionados>
- **Gaps HU→Código**: <n> · **Gaps Código→HU**: <n>
- **Riesgo principal**: <1 línea>

## 2. Trazabilidad criterio ↔ código
| # | Criterio de aceptación | Evidencia en código (archivo/función) | Estado |
| --- | --- | --- | --- |
| 1 | <criterio> | `<ruta:línea o función>` | Implementado / Parcial / Sin evidencia |

## 3. Gaps HU → Código
> Criterios de la HU sin implementación detectada.

| # | Criterio sin respaldo | Severidad | Recomendación |
| --- | --- | --- | --- |
| 1 | <criterio> | Alta / Media / Baja | <qué validar o reforzar> |

## 4. Gaps Código → HU
> Comportamientos/ramas del código no descritos en la HU.

| # | Comportamiento en código | Ubicación | Severidad | Recomendación |
| --- | --- | --- | --- | --- |
| 1 | <comportamiento> | `<ruta:línea>` | Alta / Media / Baja | <documentar / preguntar a la PO / cubrir con caso> |

## 5. Riesgos y recomendaciones
- <riesgo o recomendación para el refinamiento o el diseño de casos>

## 6. Pendientes / supuestos
- <lo que no se pudo verificar y por qué; marcar como `Pendiente de validación`>

---
## 🔗 Hand-off
- **Artefacto**: `resultado/HU-<id>/03-reportes-gaps-HU-<id>.md`
- **Producido por**: qa-gap-analysis
- **Estado**: Completado | Parcial | Bloqueado
- **Pendientes de validación**: <lista o "ninguno">
- **Siguiente agente sugerido**: @qa-test-design
- **Notas para el siguiente agente**: <gaps que deben reforzar la cobertura de casos>
---

## 🔗 Conexiones
- Siguiente artefacto: [[04-casos-prueba.template]]
