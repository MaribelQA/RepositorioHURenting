<!--
TEMPLATE — Panel de estado e índice del flujo QA de una HU.
Cópialo a `resultado/HU-<id>/00-estado-HU-<id>.md`, reemplaza los <placeholders>
y actualízalo EN CADA paso del flujo. Es el punto de entrada para retomar el
trabajo en otra sesión: lo primero que debe leer cualquier agente o persona.
No borres las secciones; marca lo no aplicable como "—".
-->

# Estado del flujo QA — HU-<id>

- **HU / Work Item**: <id de Azure> — <título de la HU>
- **Carpeta**: `resultado/HU-<id>/`
- **Fecha de inicio**: <AAAA-MM-DD>
- **Última actualización**: <AAAA-MM-DD>
- **Veredicto global**: <En progreso | Bloqueado (100% requerido) | Listo para registrar en ADO | Cerrado>

## Pipeline

| Paso | Etapa | Artefacto | Estado |
| --- | --- | --- | --- |
| 1 | Captura de HU (backup) | [`01-HU-<id>.md`](01-HU-<id>.md) | ⏳ Pendiente / ✅ Completado |
| 1 | Clarificación | [`02-reporte-clarificacion-HU-<id>.md`](02-reporte-clarificacion-HU-<id>.md) | ⏳ / 🟡 Parcial / ✅ / ⛔ Bloqueado |
| 2 | Análisis de gaps | [`03-reportes-gaps-HU-<id>.md`](03-reportes-gaps-HU-<id>.md) | ⏳ / 🟡 / ✅ |
| 3 | Diseño de casos | [`04-casos-prueba-HU-<id>.md`](04-casos-prueba-HU-<id>.md) | ⏳ / 🟡 / ✅ |
| 4 | Registro en ADO | [`05-registro-ado-HU-<id>.md`](05-registro-ado-HU-<id>.md) | ⏳ / ✅ |

> Estados: ⏳ Pendiente · 🟡 Parcial (con pendientes no bloqueantes) · ✅ Completado · ⛔ Bloqueado (100% requerido)

## Pendientes de validación (no bloqueantes)
> Lo que quedó sin resolver pero **no impide avanzar**. No se pierde: se retoma luego.

- [ ] <pendiente> — _origen: paso <n> · responsable: <PO/QA/…>_
- [ ] <pendiente>

## Bloqueantes (100% requeridos)
> Solo aquí lo que **impide** producir un artefacto útil. Si esta sección está vacía, el flujo puede continuar.

- [ ] <bloqueante> — _origen: paso <n>_

## Bitácora de sesiones
- **<AAAA-MM-DD>**: <qué se avanzó en esta sesión, qué quedó abierto>

---
## 🔗 Conexiones
- Indexa los artefactos: [[02-reporte-clarificacion.template]] · [[03-reportes-gaps.template]] · [[04-casos-prueba.template]] · [[05-registro-ado.template]]
