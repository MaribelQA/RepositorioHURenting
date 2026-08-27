<!--
TEMPLATE — Casos de prueba (Paso 3, agente qa-test-design).
Cópialo a `resultado/HU-<id>/04-casos-prueba-HU-<id>.md` y reemplaza los <placeholders>.
Cada caso valida UN SOLO comportamiento. Sigue:
- Lineamientos QA: `docs/lineamientos-qa.md`
Incluye cobertura positiva, negativa y de borde. Salida en español.
-->

# Casos de prueba — HU-<id>

## 1. Resumen de análisis de la HU
- **HU**: <id> — <título>
- **Actor / rol principal**: <actor>
- **Objetivo funcional**: <objetivo>
- **Reglas relevantes**: <reglas o "ninguna adicional">
- **Veredicto de diseño**: <Completado | Parcial | Bloqueado>
- **Preguntas de aclaración pendientes**: <preguntas o "ninguna" — ver `02-reporte-clarificacion-HU-<id>.md`>

## 2. Matrices de diseño

### 2.0 Priorización por riesgo (Testing Basado en Riesgo)
> Estima el riesgo de cada criterio para modular la profundidad de pruebas. Nivel de riesgo =
> Probabilidad de fallo × Impacto de negocio. A mayor riesgo, mayor profundidad y prioridad.

| Criterio de aceptación | Probabilidad (A/M/B) | Impacto (A/M/B) | Nivel de riesgo | Profundidad asignada |
| --- | --- | --- | --- | --- |
| <criterio> | <A/M/B> | <A/M/B> | Crítico / Alto / Medio / Bajo | <exhaustiva / estándar / mínima> |

### 2.1 Partición de equivalencia
| Regla / criterio | Partición válida | Partición inválida | Impacto funcional |
| --- | --- | --- | --- |
| <criterio> | <datos/condición válida> | <datos/condición inválida> | <impacto> |

### 2.2 Valores límite
| Regla / campo | Mínimo | Máximo | Límites cercanos | Justificación |
| --- | --- | --- | --- | --- |
| <regla/campo> | <mínimo> | <máximo> | <valores cercanos> | <por qué importa> |

### 2.3 Tabla de decisión
| Condiciones | Combinación | Acción esperada |
| --- | --- | --- |
| <condición o "No aplica"> | <combinación o "No aplica"> | <acción o justificación> |

### 2.4 Matriz combinatoria (pairwise)
> Úsala cuando 2+ parámetros/condiciones independientes interactúan. Selecciona el conjunto
> mínimo de combinaciones donde cada par de valores aparezca al menos una vez. Si no aplica
> (un solo parámetro variable o condiciones no independientes), escribe "No aplica" y justifica.

**Parámetros y valores**: <Param A: [v1, v2] · Param B: [v1, v2, v3] · … o "No aplica">

| # | <Param A> | <Param B> | <Param C> | Resultado esperado | Caso asociado |
| --- | --- | --- | --- | --- | --- |
| 1 | <valor> | <valor> | <valor> | <resultado> | TC-NN |

- **Reducción**: <N combinaciones seleccionadas> vs. <producto cartesiano completo>.
- **Combinaciones de alto riesgo/prohibidas añadidas aparte**: <lista o "ninguna">.

## 3. Resumen de cobertura
- **HU**: <id> — <título>
- **Total de casos**: <n>  (Positivos: <n> · Negativos: <n> · Borde: <n>)
- **Cobertura de criterios**: <criterios cubiertos> / <total de criterios> = <%> (meta: 100%)
- **Criterios por nivel de riesgo**: Crítico/Alto: <n> · Medio: <n> · Bajo: <n>
- **Prioridad**: <Alta | Media | Baja>
- **Insumos usados**: clarificación (`02`), gaps (`03`) si aplica
- **Técnicas aplicadas**: <partición de equivalencia / valores límite / tabla de decisión / pairwise / transición de estados / error guessing>

## 4. Casos (formato Azure DevOps — Work Item Test Case)

> **Estructura de la tabla:** `Title` aparece solo en la **primera fila** de cada caso. Las filas siguientes dejan `Title` vacío. La fila de precondición lleva `Step Action` (configurar entorno/datos) y `Step Expected Result` **vacío**. Solo los pasos ejecutables llevan resultado esperado.

| Title | Step Action | Step Expected Result |
| --- | --- | --- |
| TC-01-HU-<id> - [Acción] debe [resultado] cuando [condición] | Configurar entorno: <precondiciones y datos de prueba>. Clasificación: Happy Path / Alterno / Negativo / Borde. Criterio: <criterio>. |  |
|  | <acción del usuario/sistema> | <comportamiento esperado> |
|  | <acción> | <resultado> |
| TC-02-HU-<id> - [Acción] debe [resultado] cuando [condición] | Configurar entorno: <precondiciones y datos de prueba>. Clasificación: <clasificación>. Criterio: <criterio>. |  |
|  | <acción> | <resultado> |

## 5. Matriz de trazabilidad final
| Criterio de aceptación | Nivel de riesgo | Casos que lo cubren | Estado de cobertura |
| --- | --- | --- | --- |
| <criterio> | Crítico / Alto / Medio / Bajo | <IDs de caso> | Completo / Parcial |

## 6. Cobertura pendiente
> Criterios que NO se cubrieron por depender de pendientes/bloqueantes de la clarificación
> (sugerencias del PO sin validar, datos faltantes, etc.). No inventar casos sobre supuestos.

- <criterio o escenario pendiente> — _depende de: <pendiente del `02`>_

---
## 🔗 Hand-off
- **Artefacto**: `resultado/HU-<id>/04-casos-prueba-HU-<id>.md`
- **Producido por**: qa-test-design
- **Estado**: Completado | Parcial | Bloqueado
- **Pendientes de validación**: <lista o "ninguno">
- **Siguiente agente sugerido**: @qa-ado-registration
- **Notas para el siguiente agente**: <destino ADO sugerido, casos prioritarios, cobertura pendiente>
---

## 🔗 Conexiones
- Siguiente artefacto: [[05-registro-ado.template]]
