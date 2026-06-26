---
proyecto: "<nombre del proyecto>"
descripcion: "<una línea: qué hace el producto>"
repos:
  - nombre: "<repo principal>"
    ubicacion: "<url remota o ruta local del código>"
azure_devops:
  organizacion: "<org de Azure DevOps>"
  proyecto: "<proyecto de Azure DevOps>"
  test_plan: "<id o nombre del Test Plan por defecto>"
  test_suite: "<id o nombre de la Suite por defecto>"
  prioridad_defecto: "Media"
mcp:
  servidor_ado: "<nombre del server MCP de Azure DevOps registrado en el entorno, o vacío>"
contexto:
  glosario: "docs/glosario-renting.md"
  lineamientos: "docs/lineamientos-qa.md"
estado_setup: "pendiente"   # pendiente | completo
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

## Azure DevOps
> Para el registro de casos (Paso 4) y, si usas MCP, para **traer la HU por ID**.
- **Organización**: <…>
- **Proyecto**: <…>
- **Test Plan / Suite por defecto**: <…>
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
