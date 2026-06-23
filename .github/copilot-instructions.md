# Instrucciones del repositorio — Suite de Agentes QA Renting

> Este archivo es la **constitución compartida** del repositorio. GitHub Copilot lo
> inyecta automáticamente en **todos** los chats y agentes. Define el lenguaje,
> las convenciones de nombres, la estructura de carpetas y el contrato de
> orquestación que **todos** los agentes QA deben respetar.
>
> No describe el comportamiento detallado de cada agente: eso vive en
> `.github/agents/*.agent.md`. Aquí está lo que es común a todos.

---

## 1. Contexto del proyecto

Repositorio de QA para el producto **Renting**. Su propósito es procesar Historias
de Usuario (HU) con IA para: refinarlas, diseñar casos de prueba, detectar vacíos
entre el código y la HU, y registrar los casos en **Azure DevOps (Test Plan)**.

El trabajo se realiza mediante una **suite de agentes especializados de Copilot**
coordinados por un **orquestador**. Cada agente hace una sola cosa muy bien y se
comunica con los demás a través de **artefactos en disco** (archivos Markdown) con
un contrato de hand-off común (ver sección 5).

---

## 2. Idioma y estilo

- **Toda interacción conversacional con el usuario es en español** (claro y profesional).
- **Los artefactos generados se redactan en el mismo idioma que el documento de origen.**
  Si el idioma del origen es ambiguo o mezcla idiomas, usar **español**.
- Usar encabezados, listas con viñetas y tablas. Respuestas directas y accionables.
- No inventar información. Cuando falte un dato crítico, **preguntar** o marcarlo
  explícitamente como `Pendiente de validación`.

---

## 3. Estructura de carpetas (contrato de ubicaciones)

| Carpeta | Contenido | Producido por |
| --- | --- | --- |
| `hu-directory/` | HU refinadas (`*_refinement.md`) cuando el usuario pide guardarlas | Agente de Refinamiento (la HU de entrada se pega/adjunta en el chat, no requiere archivo) |
| `casos-prueba/` | Casos de prueba diseñados (formato plantilla y/o ADO) | Agente de Diseño de Casos |
| `reportes-gaps/` | Reportes de análisis Código vs HU | Agente de Análisis de Gaps |
| `plantillas/` | Plantillas oficiales (NO modificar sin acuerdo del equipo) | Equipo QA |
| `promts/` | Principios y prompts de referencia | Equipo QA |
| `.github/agents/` | Definiciones de los agentes Copilot | Equipo QA |

Plantillas y referencias clave:
- `plantillas/test-case-template.md` — estructura estándar de un caso de prueba.
- `plantillas/formato-cp.md` — formato de salida para Azure DevOps (Test Plan).
- `promts/test-case-creation-principles.md` — principios de diseño de casos.

---

## 4. La suite de agentes (mapa)

Todos los agentes viven en `.github/agents/` y usan la nomenclatura `qa-<rol>`.

| # | Agente (archivo) | Invocación | Rol | Estado |
| --- | --- | --- | --- | --- |
| 0 | `qa-orchestrator.agent.md` | `@qa-orchestrator` | **Orquestador**: enruta el flujo, valida artefactos y delega en los demás | ✅ Activo |
| 1 | `qa-refinement.agent.md` | `@qa-refinement` | **Refinamiento de HU**: análisis estático ISTQB, preguntas de aclaración y HU refinada | ✅ Activo |
| 2 | `qa-test-design.agent.md` | `@qa-test-design` | **Diseño de Casos**: genera casos de prueba desde la HU refinada | 🟡 Esqueleto (a refinar) |
| 3 | `qa-gap-analysis.agent.md` | `@qa-gap-analysis` | **Análisis de Gaps**: compara código vs HU y detecta vacíos | 🟡 Esqueleto (a refinar) |
| 4 | `qa-ado-registration.agent.md` | `@qa-ado-registration` | **Registro en ADO**: crea Work Items (Test Case) en Azure DevOps | 🟡 Esqueleto (a refinar) |

**Flujo por defecto:**

```
@qa-refinement  ──►  @qa-gap-analysis  ──►  @qa-test-design  ──►  @qa-ado-registration
   (HU refinada)      (reporte gaps)         (casos prueba)         (Work Items ADO)
        ▲                                                                  
        └──────────────── @qa-orchestrator coordina y valida ─────────────┘
```

El usuario puede invocar a un agente directamente o pedirle al orquestador que
conduzca el flujo completo de principio a fin.

### Flujo guiado por comandos (estilo Spec-Kit)

Además de los agentes, hay **prompt files** en `.github/prompts/` que funcionan como
**comandos `/`** y guían el proceso paso a paso, encadenándose entre sí (cada comando
termina recomendando el siguiente):

| Comando | Paso | Detrás está el agente |
| --- | --- | --- |
| `/qa-inicio` | Muestra el roadmap y cómo empezar | — |
| `/qa-clarificar` | 1. Matriz de Ambigüedades + preguntas + HU refinada | `qa-refinement` |
| `/qa-gaps` | 2. Análisis código vs HU | `qa-gap-analysis` |
| `/qa-casos` | 3. Diseño de casos de prueba | `qa-test-design` |
| `/qa-registrar` | 4. Registro en Azure DevOps (a futuro vía MCP) | `qa-ado-registration` |

**Cómo empezar:** pega la HU en el chat y ejecuta `/qa-inicio` (o directamente `/qa-clarificar`).
Los prompt files son la "barra de navegación" del flujo; los agentes aportan el comportamiento experto.

---

## 5. Contrato de hand-off entre agentes

Para que los agentes "se hablen entre sí" sin perder contexto, **cada artefacto
generado termina con un bloque de hand-off estandarizado**. Todos los agentes
deben **escribirlo** al producir su salida y **leerlo** al recibir una entrada.

```markdown
---
## 🔗 Hand-off
- **Artefacto**: <ruta relativa del archivo>
- **Producido por**: <nombre del agente, p.ej. qa-refinement>
- **Estado**: Completado | Parcial (bloqueos pendientes) | Bloqueado
- **Pendientes de validación**: <lista o "ninguno">
- **Siguiente agente sugerido**: <p.ej. @qa-gap-analysis>
- **Notas para el siguiente agente**: <contexto relevante en 1-3 líneas>
---
```

Reglas:
- Un agente **no debe avanzar** si el artefacto de entrada tiene `Estado: Bloqueado`
  o pendientes que invaliden su trabajo: debe avisar al usuario y/o devolver al
  orquestador.
- El orquestador es el responsable de validar la cadena y decidir el siguiente paso.

---

## 6. Principios QA transversales (aplican a todos los agentes)

- **Pruebas estáticas (ISTQB Foundation Level):** detectar defectos temprano en los
  requisitos (ambigüedad, omisión, inconsistencia, no testabilidad, escenarios no
  contemplados) antes de cualquier prueba dinámica.
- **No suposiciones silenciosas:** todo supuesto se hace explícito y se marca como
  `Pendiente de validación`.
- **Testabilidad:** todo criterio de aceptación debe ser observable, medible y verificable.
- **Trazabilidad:** del valor de negocio → HU → criterios → casos de prueba → Work Item ADO.
- **Preservar lo correcto:** conservar literalmente el texto de origen que sea claro
  y correcto; reescribir solo lo ambiguo, incompleto o inconsistente.
