---
proyecto: "<nombre del proyecto>"
descripcion: "<una línea: qué hace el producto>"
repos:
  - nombre: "<repo principal>"
    ubicacion: "<url remota o ruta local del código>"
codigo_disponible: false   # true si hay código para analizar (repos con 'ubicacion' real) → el Paso 1 sugiere /qa-2-gaps; false → sugiere directo /qa-3-diseñar-casos-prueba
azure_devops:
  organizacion: "https://rentingcolombia.visualstudio.com/"
  proyecto: "CentroDeInteligencia"
  work_item_type: "Test Case"
  prioridad_defecto: "Media"
mcp:
  servidor_ado: ""   # ⚠️ PENDIENTE — Autorización de Infraestructura requerida
  estado: pendiente_autorizacion
  # Para activar: descomentar servidor_ado con el nombre real del servidor MCP
  # y cambiar estado a 'activo' una vez que Infraestructura apruebe la conexión
contexto:
  glosario: ".github/docs/glosario-renting.md"
  lineamientos: ".github/docs/lineamientos-qa.md"
estado_setup: "completado"   # pendiente | completado
---

# Configuración del proyecto

> **Único archivo a editar al clonar la suite para un proyecto nuevo.** El framework
> (agentes, comandos, plantillas) no se toca. Esto lo llena `/qa-setup` **una sola vez**
> —o tú a mano— y todos los agentes lo leen como **contexto persistente en disco**: no se
> vuelve a preguntar en cada sesión (constitución §3.2). Marca `estado_setup: completo`
> cuando termines.

## Proyecto
- **Nombre**: <…>
- **Descripción**: <una línea sobre qué hace el producto>

## Repositorio(s) de código
> Para el análisis de gaps (Paso 2): dónde vive el código que implementa la HU.
- <nombre> — <url remota o ruta local>

> **Ejemplos de cómo compartir la `ubicacion`** (en el frontmatter `repos:`). Descomenta y
> ajusta el que aplique; luego pon `codigo_disponible: true`:
>
> ```yaml
> repos:
>   # 1) URL remota (Git): Azure DevOps, GitHub, etc.
>   - nombre: "renting-web"
>     ubicacion: "https://rentingcolombia.visualstudio.com/CentroDeInteligencia/_git/renting-web"
>   # 2) Ruta local absoluta a la carpeta del código
>   - nombre: "renting-api"
>     ubicacion: "D:/TAS/TECH AND SOLVE/repos/renting-api"
>   # 3) Ruta relativa dentro del mismo workspace
>   - nombre: "renting-core"
>     ubicacion: "./src/renting-core"
> codigo_disponible: true
> ```

> **`codigo_disponible`** (frontmatter): indica si hay código para analizar. Si es `true`, el
> Paso 1 (`/qa-1-clarificar`) sugiere **`/qa-2-gaps`** como siguiente paso; si es `false`, sugiere
> ir **directo a `/qa-3-diseñar-casos-prueba`** (el análisis de gaps se omite). Ponlo en `true`
> cuando algún repo tenga una `ubicacion` real (no el placeholder `<…>`).

## Azure DevOps
> Para el registro de casos (Paso 4) y, si usas MCP, para **traer la HU por ID**.
- **Organización**: https://rentingcolombia.visualstudio.com/
- **Proyecto**: CentroDeInteligencia
- **Tipo de Work Item**: Test Case (registro directo, sin Test Plan ni Test Suite)
- **Prioridad por defecto**: <Alta / Media / Baja>

## MCP (servidores del entorno)
> El servidor MCP se registra **una sola vez** en `.vscode/mcp.json` o en los ajustes de
> Copilot, **no aquí**. En este archivo solo anotas **cuál** usar y para qué.
- **Azure DevOps MCP**: `<nombre del server>` — traer HUs por ID (Paso 1) y registrar Work Items (Paso 4).
  - Si está vacío: la HU se ingresa **pegándola en el chat** y el registro en ADO es manual.

## Contexto de dominio (consulta on-demand)
> Plantillas a completar con tu equipo. Los agentes solo las abren cuando una HU usa un
> término o regla que necesitan aclarar (constitución §3).
- **Glosario**: [[glosario-renting]]
- **Lineamientos QA**: [[lineamientos-qa]]
