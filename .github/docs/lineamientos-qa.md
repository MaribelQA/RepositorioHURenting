# Lineamientos QA — Renting

> Convenciones del equipo QA. Las consultan los QA humanos y los agentes (on-demand) para
> producir resultados consistentes. **Borrador con valores por defecto sensatos** — ajusta las
> filas marcadas con 🔧 según los acuerdos reales de tu equipo.

## Criterios de severidad (hallazgos y gaps)
| Severidad | Cuándo usarla | Efecto en el flujo |
| --- | --- | --- |
| Alta | Impide entender la HU, contradice una regla clave, o haría **incorrectos** los casos (no solo incompletos). Ej.: falta la regla que define el resultado esperado. | Suele ser **bloqueante** (100% requerido). Detiene el avance. |
| Media | Afecta cobertura o calidad, pero se puede avanzar con un supuesto marcado. Ej.: falta un valor límite exacto, un mensaje sin texto confirmado. | `Pendiente de validación`. Se avanza con artefacto `Parcial`. |
| Baja | Detalle menor, cosmético o diferible que no afecta la cobertura de pruebas. Ej.: etiqueta de UI, orden de columnas. | Se registra y se continúa. No bloquea. |

## ¿Qué cuenta como bloqueante?
> Recordatorio: se bloquea **solo** ante dudas 100% requeridas (constitución §3.3).

- ✅ **Bloqueante**:
  - Falta el número de work item (la carpeta depende de él).
  - Una regla de negocio cuya ausencia **invierte o hace ambiguo** el resultado esperado.
  - Resultado esperado no observable ni medible para un criterio principal.
  - Regla de negocio internamente contradictoria sin forma de decidir cuál aplica.
- ❌ **NO bloqueante** → `Pendiente de validación`:
  - Falta un dato de UI no crítico (color, ubicación de un botón).
  - Un escenario de borde poco probable sin impacto alto.
  - Texto exacto de un mensaje secundario (se marca supuesto provisional).

## Testing Basado en Riesgo (nivel de riesgo → profundidad)
> Cada criterio recibe esfuerzo proporcional a su riesgo. Detalle en `qa-test-design` (Paso 2).

| Nivel de riesgo | Profundidad de pruebas | Prioridad del caso |
| --- | --- | --- |
| Crítico / Alto | Exhaustiva: happy path + todos los negativos relevantes + valores límite + pairwise si hay interacción + validación de persistencia | Alta |
| Medio | Estándar: happy path + negativos críticos + al menos un borde | Media |
| Bajo | Mínima: happy path + un negativo representativo | Baja |

- Nivel de riesgo = Probabilidad de fallo × Impacto de negocio (matriz en `qa-test-design`).
- Ante duda al estimar, usa criterio conservador (asume mayor riesgo) y registra el supuesto.

## Convenciones de casos de prueba
- Un caso valida **un solo comportamiento** (si el título tiene un "y", probablemente son dos casos).
- **Cobertura mínima por criterio**: 1 positivo (happy path) + 1 negativo por validación crítica + 1 borde por restricción relevante. Aumenta según el nivel de riesgo.
- **Naming del título**: `TC-[2 dígitos]-HU-<id> - [Acción] debe [resultado] cuando [condición]`.
- **ID**: secuencia por HU iniciando en `01`, incremento `+1`.
- **Oracle explícito**: el resultado esperado debe ser observable y verificable; nada de "funciona bien".
- **Mensajes/textos**: cuando la HU especifica un texto, cópialo **exacto entre comillas** (no parafrasear).
- **Datos concretos**: usa valores reproducibles (placa `ABC-123`, monto `$1.000.000`), no "un valor inválido".
- **Formato ADO**: cada `Step Action` ejecutable lleva su `Step Expected Result`. La fila de precondición deja `Step Expected Result` **vacío** (ver `plantillas/artefactos/04-casos-prueba.template.md`).
- **Persistencia**: todo flujo que crea/actualiza/elimina datos requiere ≥1 caso que valide el dato persistido (tabla, campos críticos, tipo de operación).
- **Referencias de diseño**: ejemplo golden en `ejemplo-golden-casos.md`; heurísticas de defectos en `heuristicas-defectos.md`.

## Definición de Hecho (DoD) de una HU lista para casos
- [ ] Criterios de aceptación testables (observables y medibles).
- [ ] Sin bloqueantes abiertos en el `02-reporte-clarificacion`.
- [ ] Actor/rol, objetivo y valor de negocio identificados.
- [ ] Reglas de negocio y validaciones críticas definidas o con supuesto marcado.
- [ ] Nivel de riesgo estimado por criterio.

## Definición de Hecho (DoD) de la suite de casos (artefacto 04)
- [ ] Cada criterio de aceptación tiene ≥1 caso que lo cubre (trazabilidad explícita).
- [ ] Cobertura mínima positivo/negativo/borde según riesgo.
- [ ] Casos de mensajes usan el texto exacto especificado.
- [ ] Flujos con persistencia incluyen validación de BD (o alternativa: API/logs/eventos).
- [ ] Sin casos duplicados o de bajo valor diagnóstico.
- [ ] Matriz de trazabilidad con estado de cobertura (Completo/Parcial) y nivel de riesgo.

## Métrica de cobertura (referencia)
- **Cobertura de criterios** = criterios con ≥1 caso `Completo` / total de criterios. Meta por defecto: **100%** de criterios cubiertos (los no cubiertos van a "Cobertura pendiente" con su motivo). 🔧
- Reporta en el resumen del `04`: total de casos, distribución (positivos/negativos/borde) y % de criterios cubiertos.

## Datos de prueba
- **Entorno por defecto**: QA. 🔧 (alternativas: Staging, UAT, Local).
- **Origen de datos**: base de datos del ambiente QA / procedimientos almacenados / fixtures. 🔧
- **Datos sensibles**: no usar datos reales de clientes; anonimizar documentos, placas y montos en los artefactos. 🔧
- **Reutilización**: preferir datos de prueba estables y reproducibles; documentar el dato en la precondición del caso.

## Registro en Azure DevOps
- **Organización**: https://rentingcolombia.visualstudio.com/
- **Proyecto**: CentroDeInteligencia
- **Tipo de Work Item**: Test Case (registro directo, sin Test Plan ni Test Suite).
- **Prioridad por defecto**: Media (se sobrescribe con la prioridad heredada del nivel de riesgo del caso). 🔧
- **Vínculo HU ↔ Test Case**: enlazar cada Test Case a la HU de origen con relación "Tested By" / "Tests". 🔧
- **Campos mínimos**: Title (con ID `TC-NN-HU-<id>`), Steps (Action/Expected), Priority, Area/Iteration del equipo. 🔧

## Presupuesto de contexto (disciplina de tokens)
> Cada agente lee un conjunto **acotado** de archivos. El objetivo es consumo predecible: leer
> solo lo necesario, en orden, y **no releer** lo ya resumido en `00-estado`. Complementa la
> constitución §7. Regla de oro: **`00-estado` es el índice y punto de entrada** — se lee primero
> siempre; si un dato ya está resumido ahí, no reabras el artefacto de origen.

| Agente (paso) | Leer siempre (orden) | Leer on-demand (solo si) | No leer / no releer |
| --- | --- | --- | --- |
| @qa-clarify (1) | HU pegada/adjunta en el chat → `00-estado` (si existe) → plantilla `02` | `glosario-renting.md` / `lineamientos-qa.md` solo ante un término o regla que necesites aclarar | El repo de código; artefactos de otras HU; no buscar la HU en disco (llega en el chat) |
| @qa-gap-analysis (2) | `00-estado` → `01` → `02` → plantilla `03` | Solo los **módulos de código relevantes** a la HU (nunca el repo completo) | `04`/`05`/`06`; código no relacionado; artefactos de otras HU |
| @qa-test-design (3) | `00-estado` → `01` → `02` → plantilla `04` → `ejemplo-golden-casos.md` (una vez) | `03` si existe; `heuristicas-defectos.md` en Paso 5; `glosario`/`lineamientos` ante duda puntual | Releer `01`/`02` completos si `00-estado` ya los resume; código; ejemplo golden más de una vez |
| @qa-ado-registration (4) | `00-estado` → `04` | `05` previo si se reanuda un registro parcial | `01`/`02`/`03`; código; docs de dominio |
| @qa-certify (5) | Entrevista (7 preguntas) → `00-estado` → `04` | `01`/`02` solo para datos puntuales del reporte | Releer artefactos completos si la entrevista + `00-estado` ya bastan |

**Reglas transversales**
- **Una etapa por invocación**: no encadenar pasos en una sola corrida.
- **Leer rangos amplios una vez** en lugar de muchas lecturas pequeñas del mismo archivo.
- **No repetir en el chat** lo que ya quedó en disco: resumir en 1-3 líneas y enlazar al artefacto.
- **Acotar el código** (paso 2) a los módulos que toca la HU; nunca escanear todo el repo.
- Docs de dominio (`glosario`, `lineamientos`) son **on-demand**: ábrelos solo ante el término/regla concreto, no por defecto.

## 🔗 Conexiones
- Formato de casos para ADO: [[04-casos-prueba.template]]
- Ejemplo golden: [[ejemplo-golden-casos]]
- Heurísticas de defectos: [[heuristicas-defectos]]
