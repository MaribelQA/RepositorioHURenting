# Suite de Agentes QA — Renting

Herramienta de QA asistida por **GitHub Copilot** para procesar Historias de Usuario (HU):
clarificarlas, analizar gaps código↔HU, diseñar casos de prueba y registrarlos en
**Azure DevOps (Test Plan)**. Los agentes se comunican por **artefactos en disco**, así el
trabajo se retoma en otra sesión sin depender del chat.

## Empezar
1. **Primera vez en el proyecto:** ejecuta `/qa-setup` para configurar nombre, repos,
   Azure DevOps y MCP. Queda en `proyecto.config.md` (una sola vez por clon).
2. Luego, en Copilot Chat, escribe `/qa-1-clarificar` y pega tu HU —o, si configuraste el
   MCP de Azure DevOps, da solo el número de work item. Cada paso te indica el siguiente.

| Paso | Comando | Resultado |
| --- | --- | --- |
| 1 | `/qa-1-clarificar` | Reporte de Clarificación |
| 2 | `/qa-2-gaps` | Reporte de gaps (opcional) |
| 3 | `/qa-3-diseñar-casos-prueba` | Casos de prueba |
| 4 | `/qa-4-registrar` | Work Items en Azure DevOps |

`/qa-inicio` muestra el roadmap completo.

## Dónde queda todo
Una carpeta por HU: `qa-analisis-casos/HU-<id>/`, con archivos numerados por orden del proceso
(`00-estado`, `01-HU`, `02-clarificacion`, `03-gaps`, `04-casos`, `05-registro-ado`).

## Estructura del repo
- `proyecto.config.md` — **configuración del proyecto** (único archivo a editar al clonar; lo llena `/qa-setup`).
- `.github/copilot-instructions.md` — constitución (reglas comunes; léela primero).
- `.github/agents/` — agentes especializados (`qa-*`).
- `.github/prompts/` — comandos `/` del flujo.
- `.github/plantillas/` — plantillas oficiales (incluye `.github/plantillas/artefactos/` con los esqueletos de salida).
- `.github/docs/` — contexto de dominio para QA: `glosario-renting.md` y `lineamientos-qa.md` (complétalos con tu equipo).
- `qa-analisis-casos/` — salidas por HU.


## 🔗 Conexiones
- 🧠 Grafo completo del repo: [[mapa-qa|Mapa de la Suite QA]]
- Reglas comunes: [[copilot-instructions|Constitución]]




#######

# Guía rápida: IA para QA — Sin tecnicismos

> Para compañeros QA que quieren entender cómo usamos Inteligencia Artificial
> en nuestro flujo de trabajo con Historias de Usuario.

---

## ¿De qué se trata todo esto?

Usamos una herramienta de IA llamada **GitHub Copilot** para ayudarnos a:

1. **Entender mejor las HU** — detectar partes ambiguas, incompletas o que no se pueden probar.
2. **Diseñar casos de prueba** — generarlos de forma estructurada y trazable.
3. **Registrarlos en Azure DevOps** — automáticamente, sin copiar y pegar.

La IA no reemplaza al QA. Lo asiste: hace el trabajo tedioso y repetitivo para que tú puedas enfocarte en analizar y validar.

---

## Los términos que vas a escuchar — explicados simple

### 🤖 GitHub Copilot
Es un **asistente de IA** integrado dentro de VS Code (el editor de código). Es como tener un colega muy rápido que lee documentos, detecta problemas y escribe borradores por ti. Tú revisas y apruebas.

---

### 🧠 Modelo de IA (Claude Sonnet, Claude Opus, Claude Haiku…)
Son los **"cerebros"** detrás de Copilot. Distintos modelos tienen distintas capacidades:

| Modelo | Analogía | Se usa para… |
|--------|----------|--------------|
| **Claude Opus** | El analista senior | Tareas complejas: clarificar HU |
| **Claude Sonnet** | El analista medio | Diseñar casos, analizar gaps |
| **Claude Haiku** | El ejecutivo rápido | Tareas mecánicas: registrar en ADO |

Tú no eliges el modelo manualmente — el sistema ya lo configura según la tarea.

---

### 🎭 Agente
Un **agente** es una IA configurada para un rol específico, igual que un especialista en tu equipo.

En este proyecto tenemos 7 agentes, cada uno con su función:

| Agente | Su rol |
|--------|--------|
| `@qa-setup` | El instalador: configura el proyecto una sola vez antes de empezar |
| `@qa-clarify` | Analista de HU: detecta ambigüedades y hace preguntas |
| `@qa-gap-analysis` | Revisor de código: compara lo que dice la HU con lo que hay en el sistema |
| `@qa-test-design` | Diseñador de casos de prueba |
| `@qa-ado-registration` | Secretario: registra todo en Azure DevOps |
| `@qa-certify` | Certifica que las pruebas se ejecutaron correctamente |
| `@qa-orchestrator` | El coordinador: valida que cada paso esté bien hecho antes de pasar al siguiente |

> **Analogía:** imagina que tienes un equipo de especialistas. Tú (el QA) eres el líder. Cada vez que necesitas algo, llamas al especialista correcto con un comando.

---

### ⚙️ Agente `@qa-setup` — El punto de partida del proyecto

Es el **primer agente que se usa**, pero **solo una vez** cuando el proyecto se configura por primera vez (o cuando cambian datos como la organización de ADO). Los QA del día a día normalmente no necesitan ejecutarlo.

**¿Qué hace exactamente?**

Te hace preguntas una por una para configurar el proyecto y guarda todo en un archivo llamado `proyecto.config.md`. Esa configuración es la que todos los demás agentes leen para saber con qué proyecto, qué repositorios y qué Azure DevOps están trabajando.

Las preguntas que hace son:

1. ¿Cuál es el nombre y descripción del proyecto?
2. ¿Cuál es el repositorio de código? (para poder analizar gaps)
3. Datos de Azure DevOps: organización, proyecto, Test Plan, prioridad por defecto
4. ¿Hay conexión MCP con ADO o se trabajará pegando la HU manualmente en el chat?

**¿Qué pasa si ya está configurado?**

Si `proyecto.config.md` ya está completo, el agente **no pregunta nada** — solo muestra un resumen de la configuración vigente. Es inteligente: no molesta si ya tiene todo lo que necesita.

**¿Qué NO hace?**

- No analiza HUs.
- No crea casos de prueba.
- No registra nada en ADO.
- Solo configura. Punto.

> **En resumen:** `/qa-setup` es como instalar un programa. Se hace una vez, queda configurado y luego todos usan el resultado sin tener que volver a configurar nada.

---

### ⌨️ Prompt / Comando
Un **prompt** es la instrucción que le das a la IA. En este proyecto, los prompts están empaquetados como **comandos** que empiezan con `/`:

| Comando | ¿Qué hace? |
|---------|-----------|
| `/qa-1-clarificar` | Analiza la HU y detecta puntos dudosos |
| `/qa-2-gaps` | Busca diferencias entre la HU y el código del sistema |
| `/qa-3-diseñar-casos-prueba` | Genera los casos de prueba |
| `/qa-4-registrar` | Crea los Work Items en Azure DevOps |
| `/qa-5-certificar` | Genera la Carta de Certificación |
| `/qa-setup` | Configura el proyecto (solo se hace una vez) |

> **Analogía:** un prompt es como un formulario ya llenado que envías a un especialista. En vez de explicarle todo desde cero, el formulario ya tiene las instrucciones. Tú solo pegas la HU y lanzas el comando.

---

### 📄 Artefacto
Un **artefacto** es simplemente un **archivo de salida** que produce cada paso. Quedan guardados en la carpeta `qa-analisis-casos/HU-<número>/`.

Cada HU tiene su carpeta con hasta 7 archivos, en orden:

```
HU-156235/
  00-estado-HU-156235.md        ← Panel de control: ¿en qué paso vamos?
  01-HU-156235.md               ← Copia de la HU original (nunca se toca)
  02-reporte-clarificacion...   ← Hallazgos: ambigüedades, preguntas, riesgos
  03-reportes-gaps...           ← Diferencias HU vs código (opcional)
  04-casos-prueba...            ← Los casos de prueba listos
  05-registro-ado...            ← Links a los Work Items en ADO
  06-carta-certificacion...     ← Carta final firmada
```

> **Analogía:** es como el expediente físico de una HU. Cada agente agrega su hoja al expediente.

---

### 🔗 Hand-off
Es el **"pase de turno"** entre agentes. Al terminar su trabajo, cada agente escribe al final de su artefacto un bloquecito que dice:
- Qué produjo
- En qué estado quedó (Completado / Parcial / Bloqueado)
- Qué falta por validar
- Quién sigue

Esto permite retomar el trabajo en otra sesión sin perder el hilo.

---

### 📁 `.github/` — La carpeta de configuración
Es donde vive toda la "inteligencia" del proyecto. Tú normalmente no necesitas tocarla. Contiene:

- `agents/` → Las instrucciones de cada agente (cómo piensa y actúa)
- `prompts/` → Los comandos `/` que tú usas en el chat
- `docs/` → El glosario de Renting y los lineamientos QA
- `plantillas/` → Las plantillas de cada artefacto

---

### 📋 `copilot-instructions.md` — Las reglas del juego

Es el **archivo más importante** de toda la suite. Copilot lo lee **automáticamente en cada conversación**, antes de hacer cualquier cosa.

Funciona como un **reglamento interno** que le dice a la IA:
- En qué idioma hablar (español)
- Dónde guardar cada archivo y con qué nombre
- Cuándo bloquearse y cuándo avanzar sola
- Qué agente hace qué cosa
- Qué principios QA debe respetar (trazabilidad, testabilidad, etc.)

> **Analogía:** imagina que contratas a un asistente nuevo. En vez de explicarle todo cada día, le das un manual de bienvenida que lee antes de empezar. Ese manual es `copilot-instructions.md`.

**¿Por qué existe?** Sin él, Copilot sería una IA genérica. Con él, se convierte en un QA especializado en tu proyecto y en tus reglas. Cada vez que abres el chat, ya "sabe" cómo trabaja tu equipo.

**¿Quién lo edita?** Solo quien administra la suite. Los QA que usan los comandos no necesitan tocarlo.

---

### 🗂️ MCP (Model Context Protocol)
Es la **conexión entre Copilot y Azure DevOps**. En teoría permite que la IA lea y escriba Work Items directamente, sin que tú copies y pegues nada.

> ⚠️ **Estado actual:** La conexión MCP existe técnicamente en el proyecto, pero **aún no está habilitada**. Está pendiente de aprobación. Por ahora, los casos de prueba y registros en Azure DevOps se gestionan de forma manual — la IA genera el contenido y tú lo cargas en ADO. Cuando se autorice la conexión, este paso se automatizará.

---

### 🎯 Agente `@qa-orchestrator` — El director del proceso

Es el agente **coordinador de toda la cadena**. Su trabajo no es hacer análisis ni escribir casos de prueba: es **asegurarse de que cada paso se haga en orden, con las entradas correctas, y que nadie avance si algo está mal**.

**¿Cuándo usarlo?**

- Cuando quieres ejecutar el proceso completo de una HU de principio a fin sin ir paso a paso.
- Cuando no sabes exactamente en qué punto va una HU y quieres que alguien lo evalúe.
- Cuando quieres una vista general del estado de todo el pipeline.

**¿Qué hace en concreto?**

1. Lee el panel `00-estado` de la HU para saber en qué etapa está.
2. Revisa que el artefacto anterior esté bien hecho (lee el bloque de **hand-off**).
3. Si todo está bien → llama al siguiente agente especializado y le pasa el contexto necesario.
4. Si algo está **Bloqueado o incompleto** → se detiene, avisa qué falta y no avanza.
5. Al terminar cada etapa, muestra una tabla con el estado de todo el pipeline:

| Etapa | Agente | Estado |
|-------|--------|--------|
| 1. Clarificación | `@qa-clarify` | ✅ Completado |
| 2. Gaps | `@qa-gap-analysis` | ✅ Completado |
| 3. Casos de prueba | `@qa-test-design` | 🟡 Parcial |
| 4. Registro ADO | `@qa-ado-registration` | ⏳ Pendiente |

**¿Qué NO hace?**

- No escribe el análisis ni los casos por sí mismo.
- No omite validaciones para ir más rápido.
- No avanza si la entrada de una etapa tiene errores críticos.

> **Analogía:** es como un jefe de proyecto que revisa que cada entregable esté aprobado antes de pasarlo al siguiente área. No hace el trabajo técnico, pero garantiza que la cadena de calidad no se rompa.

---

## El flujo completo — en un vistazo

```
Tú pegas la HU en el chat
        │
        ▼
/qa-1-clarificar  →  La IA analiza la HU, detecta dudas y genera preguntas para la PO
        │
        ▼
/qa-2-gaps  (opcional)  →  La IA compara la HU con el código del sistema
        │
        ▼
/qa-3-diseñar-casos-prueba  →  La IA genera los casos de prueba estructurados
        │
        ▼
/qa-4-registrar  →  La IA genera el contenido para ADO (carga manual por ahora — MCP pendiente de aprobación)
        │
        ▼
/qa-5-certificar  →  La IA genera la Carta de Certificación
```

**En cada paso tú revisas y apruebas.** La IA hace el borrador; el criterio QA es tuyo.

---

## ¿Qué NO hace la IA?

- No ejecuta pruebas en el sistema (eso sigue siendo tuyo).
- No toma decisiones de negocio.
- No inventa datos: si algo no está en la HU, lo marca como "Pendiente de validación".
- No borra ni sobreescribe sin avisar.

---

## Glosario rápido

| Término | Significado simple |
|---------|--------------------|
| **Copilot** | El asistente de IA en VS Code |
| **Agente** | IA con un rol específico (clarificador, diseñador, etc.) |
| **Prompt / Comando** | Instrucción que le das a la IA (ej: `/qa-1-clarificar`) |
| **Modelo** | El "cerebro" que usa la IA (Opus, Sonnet, Haiku) |
| **Artefacto** | Archivo de salida de cada paso |
| **Hand-off** | Pase de turno entre agentes |
| **MCP** | Conector con Azure DevOps (existe pero pendiente de aprobación) |
| **Gap** | Diferencia entre lo que dice la HU y lo que hace el sistema |
| **HU** | Historia de Usuario |
| **ADO** | Azure DevOps |

---

> Este proyecto fue construido para que **tú, como QA**, tengas un asistente inteligente
> que te ayude a llegar más rápido a mejores casos de prueba. La IA trabaja; tú decides.
