# Suite de Agentes QA — Renting

Herramienta de QA asistida por **GitHub Copilot** para procesar Historias de Usuario (HU):
clarificarlas, analizar gaps código↔HU, diseñar casos de prueba y registrarlos en
**Azure DevOps (Test Plan)**. Los agentes se comunican por **artefactos en disco**, así el
trabajo se retoma en otra sesión sin depender del chat.

## Empezar
En Copilot Chat, escribe `/qa-clarificar` y, en el mismo mensaje, pega tu HU.
Cada paso te indica el siguiente.

| Paso | Comando | Resultado |
| --- | --- | --- |
| 1 | `/qa-clarificar` | Reporte de Clarificación |
| 2 | `/qa-gaps` | Reporte de gaps (opcional) |
| 3 | `/qa-casos` | Casos de prueba |
| 4 | `/qa-registrar` | Work Items en Azure DevOps |

`/qa-inicio` muestra el roadmap completo.

## Dónde queda todo
Una carpeta por HU: `resultado/HU-<id>/`, con archivos numerados por orden del proceso
(`00-estado`, `01-HU`, `02-clarificacion`, `03-gaps`, `04-casos`, `05-registro-ado`).

## Estructura del repo
- `.github/copilot-instructions.md` — constitución (reglas comunes; léela primero).
- `.github/agents/` — agentes especializados (`qa-*`).
- `.github/prompts/` — comandos `/` del flujo.
- `plantillas/` — plantillas oficiales (incluye `plantillas/resultado/` con los esqueletos de salida).
- `promts/` — principios de diseño de casos.
- `docs/` — contexto de dominio para QA: `glosario-renting.md` y `lineamientos-qa.md` (complétalos con tu equipo).
- `resultado/` — salidas por HU.
