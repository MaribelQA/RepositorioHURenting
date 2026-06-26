<!--
TEMPLATE — Casos de prueba (Paso 3, agente qa-test-design).
Cópialo a `resultado/HU-<id>/04-casos-prueba-HU-<id>.md` y reemplaza los <placeholders>.
Cada caso valida UN SOLO comportamiento. Sigue:
- Lineamientos QA: `docs/lineamientos-qa.md`
- Plantilla de caso: `plantillas/test-case-template.md`
- Formato ADO (Test Plan): `plantillas/formato-cp.md`
Incluye cobertura positiva, negativa y de borde. Salida en español.
-->

# Casos de prueba — HU-<id>

## 1. Resumen de cobertura
- **HU**: <id> — <título>
- **Total de casos**: <n>  (Positivos: <n> · Negativos: <n> · Borde: <n>)
- **Insumos usados**: clarificación (`02`)<, gaps (`03`) si aplica>
- **Técnicas aplicadas**: <partición de equivalencia / valores límite / tabla de decisión / transición de estados / error guessing>

## 2. Trazabilidad criterio ↔ caso
| Criterio de aceptación | Casos que lo cubren |
| --- | --- |
| <criterio> | <IDs de caso> |

## 3. Casos (formato Azure DevOps — Test Plan)
> Cada `Step Action` debe tener su `Stept Expected Result`. Ver `plantillas/formato-cp.md`.

| Title | Step Action | Stept Expected Result |
| --- | --- | --- |
| <Título: [Acción] debería [resultado esperado] cuando [condición]> |   |   |
|   | @Precondiciones: <estado/datos requeridos antes de ejecutar> |   |
|   | paso 1: <acción del usuario/sistema> | resultado paso 1: <comportamiento esperado> |
|   | paso 2: <acción> | resultado paso 2: <resultado> |
| <Título del siguiente caso> |   |   |
|   | @Precondiciones: <…> |   |
|   | paso 1: <…> | resultado paso 1: <…> |

## 4. Cobertura pendiente
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
