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

## 3. Resumen de cobertura
- **HU**: <id> — <título>
- **Total de casos**: <n>  (Positivos: <n> · Negativos: <n> · Borde: <n>)
- **Prioridad**: <Alta | Media | Baja>
- **Insumos usados**: clarificación (`02`), gaps (`03`) si aplica
- **Técnicas aplicadas**: <partición de equivalencia / valores límite / tabla de decisión / transición de estados / error guessing>

## 4. Casos (formato Azure DevOps — Work Item Test Case)

> **Estructura de la tabla:** `Title` aparece solo en la **primera fila** de cada caso. Las filas siguientes dejan `Title` vacío. La fila de precondición lleva `Step Action` (configurar entorno/datos) y `Step Expected Result` **vacío**. Solo los pasos ejecutables llevan resultado esperado.
>
> **CSV generado junto a este archivo:** `04-casos-prueba-HU-<id>.csv` — columnas `ID|Work_Item_Type|Title|Assigned_To|State|Step_Action|Step_Expected_Result`, separador `|`.

| Title | Step Action | Step Expected Result |
| --- | --- | --- |
| TC-01-HU-<id> - [Acción] debe [resultado] cuando [condición] | Configurar entorno: <precondiciones y datos de prueba>. Clasificación: Happy Path / Alterno / Negativo / Borde. Criterio: <criterio>. |  |
|  | <acción del usuario/sistema> | <comportamiento esperado> |
|  | <acción> | <resultado> |
| TC-02-HU-<id> - [Acción] debe [resultado] cuando [condición] | Configurar entorno: <precondiciones y datos de prueba>. Clasificación: <clasificación>. Criterio: <criterio>. |  |
|  | <acción> | <resultado> |

## 5. Matriz de trazabilidad final
| Criterio de aceptación | Casos que lo cubren | Estado de cobertura |
| --- | --- | --- |
| <criterio> | <IDs de caso> | Completo / Parcial |

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
