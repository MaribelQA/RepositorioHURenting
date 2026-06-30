# 🧠 Mapa de la Suite QA Renting

Nodo central del repositorio para visualizar el **grafo** de todos los `.md` en
**Foam** u **Obsidian**. Cada enlace `[[...]]` es una arista; abre la vista de grafo
("brain"/"graph view") para ver cómo se conectan agentes, comandos, plantillas y dominio.

> Punto de partida humano: [[README]] · Reglas comunes: [[copilot-instructions|Constitución]]

## Pipeline (flujo de extremo a extremo)

```
/qa-inicio
   │
   ▼
/qa-clarificar ──► /qa-gaps (opcional) ──► /qa-casos ──► /qa-registrar
@qa-refinement     @qa-gap-analysis        @qa-test-design   @qa-ado-registration
   │                   │                       │                  │
 01,02                03                      04             05 + Work Items ADO
```

Coordinado y validado por [[qa-orchestrator.agent|qa-orchestrator]] (Agente 0).

## Configuración
- [[proyecto.config]] — datos del proyecto (nombre, repos, ADO, MCP); único archivo a editar al clonar

## Comandos (`/`)
- [[qa-setup.prompt|/qa-setup]] → ejecuta [[qa-setup.agent|qa-setup]] (configura el proyecto)
- [[qa-0-inicio.prompt|/qa-inicio]] — roadmap y bienvenida
- [[qa-1-clarificar.prompt|/qa-clarificar]] → ejecuta [[qa-refinement.agent|qa-refinement]]
- [[qa-2-gaps.prompt|/qa-gaps]] → ejecuta [[qa-gap-analysis.agent|qa-gap-analysis]]
- [[qa-3-casos.prompt|/qa-casos]] → ejecuta [[qa-test-design.agent|qa-test-design]]
- [[qa-4-registrar.prompt|/qa-registrar]] → ejecuta [[qa-ado-registration.agent|qa-ado-registration]]

## Agentes
- [[qa-setup.agent|qa-setup]] — configura el proyecto (Paso 0)
- [[qa-orchestrator.agent|qa-orchestrator]] — director del proceso
- [[qa-refinement.agent|qa-refinement]] — clarifica la HU (Paso 1)
- [[qa-gap-analysis.agent|qa-gap-analysis]] — gaps código vs HU (Paso 2)
- [[qa-test-design.agent|qa-test-design]] — diseño de casos (Paso 3)
- [[qa-ado-registration.agent|qa-ado-registration]] — registro en ADO (Paso 4)

## Plantillas de salida (una por paso)
- [[00-estado.template]] — panel de estado e índice
- [[02-reporte-clarificacion.template]] — Reporte de Clarificación
- [[03-reportes-gaps.template]] — Reporte de gaps
- [[04-casos-prueba.template]] — Casos de prueba
- [[05-registro-ado.template]] — Registro en ADO

## Referencias de diseño de casos
- [[test-case-creation-principles]] — principios ISTQB
- [[04-casos-prueba.template]] — plantilla de caso y formato ADO (Work Item Test Case)

## Contexto de dominio (on-demand)
- [[glosario-renting]] — términos y estados del negocio
- [[lineamientos-qa]] — convenciones del equipo QA

## Instancias de ejemplo (HU-202368)
- [[01-HU-202368]] → [[02-reporte-clarificacion-HU-202368]] → [[03-reportes-gaps-HU-202368]] → [[04-casos-prueba-HU-202368]]

## Convenciones de enlaces (para no sobre-relacionar)
- **Este mapa es el único hub:** enlaza hacia todos los nodos; **ningún otro archivo enlaza de vuelta al mapa** (evita el grafo en estrella).
- **Cadena hacia adelante:** cada archivo enlaza solo a su **vecino inmediato** (su paso siguiente), nunca a pasos lejanos. Ej.: `/qa-inicio` → `/qa-clarificar` (no a `/qa-gaps`).
- **Enlaces verticales:** prompt → agente ("ejecuta") y agente → plantilla ("rellena").
- **Una relación = una arista:** sin recíprocas (si A enlaza a B, B no reenlaza a A).
- **Nodos terminales** (`/qa-registrar`, `05`, último de cada cadena) **no llevan pie de Conexiones**; se alcanzan desde este mapa.

---
> **Nota Foam/Obsidian:** los agentes y comandos viven en `.github/`. Foam los indexa;
> **Obsidian ignora por defecto las carpetas que empiezan con `.`**, así que aparecerán como
> nodos "no resueltos" en su grafo. Para verlos como archivos reales en Obsidian, copia/mueve
> el contenido fuera de `.github/` o usa Foam (VS Code).
