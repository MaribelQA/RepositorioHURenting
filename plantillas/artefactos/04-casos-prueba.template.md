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

## 2. Preguntas de aclaración pendientes
- <pregunta o "ninguna">

## 3. Matrices de diseño

### 3.1 Partición de equivalencia
| Regla / criterio | Partición válida | Partición inválida | Impacto funcional |
| --- | --- | --- | --- |
| <criterio> | <datos/condición válida> | <datos/condición inválida> | <impacto> |

### 3.2 Valores límite
| Regla / campo | Mínimo | Máximo | Límites cercanos | Justificación |
| --- | --- | --- | --- | --- |
| <regla/campo> | <mínimo> | <máximo> | <valores cercanos> | <por qué importa> |

### 3.3 Tabla de decisión
| Condiciones | Combinación | Acción esperada |
| --- | --- | --- |
| <condición o "No aplica"> | <combinación o "No aplica"> | <acción o justificación> |

## 4. Resumen de cobertura
- **HU**: <id> — <título>
- **Total de casos**: <n>  (Positivos: <n> · Negativos: <n> · Borde: <n>)
- **Insumos usados**: clarificación (`02`)<, gaps (`03`) si aplica>
- **Técnicas aplicadas**: <partición de equivalencia / valores límite / tabla de decisión / transición de estados / error guessing>

## 5. Casos (formato Azure DevOps — Work Item Test Case)
> Cada `Step Action` ejecutable debe tener su `Stept Expected Result`. La fila `@Precondiciones` puede dejarlo vacío.

| Title | Step Action | Stept Expected Result |
| --- | --- | --- |
| TC-01-HU-<id> - [Acción] debe [resultado] cuando [condición] | Clasificación: Happy Path / Alterno / Negativo / Borde. Criterio cubierto: <criterio>. Técnica ISTQB: <técnica>. Datos de prueba: <datos>. | <resultado esperado general del caso> |
|  | @Precondiciones: <estado/datos requeridos antes de ejecutar> |  |
|  | paso 1: <acción del usuario/sistema> | resultado paso 1: <comportamiento esperado> |
|  | paso 2: <acción> | resultado paso 2: <resultado> |
| TC-02-HU-<id> - [Acción] debe [resultado] cuando [condición] | Clasificación: Happy Path / Alterno / Negativo / Borde. Criterio cubierto: <criterio>. Técnica ISTQB: <técnica>. Datos de prueba: <datos>. | <resultado esperado general del caso> |
|  | @Precondiciones: <estado/datos requeridos antes de ejecutar> |  |
|  | paso 1: <acción> | resultado paso 1: <resultado> |

## 6. Matriz de trazabilidad final
| Criterio de aceptación | Casos que lo cubren | Estado de cobertura |
| --- | --- | --- |
| <criterio> | <IDs de caso> | Completo / Parcial |

## 7. Cobertura pendiente
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
