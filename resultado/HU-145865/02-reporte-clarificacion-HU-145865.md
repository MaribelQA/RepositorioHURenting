# Reporte de Clarificación — HU-145865

## 1. Resumen del análisis
- **HU analizada**: 145865 — Alerta temprana de exceso de kilometraje al crear el servicio
- **Veredicto**: Sin bloqueantes, se puede avanzar
- **Preguntas hechas / respondidas**: 5 / 5

## 2. Matriz de hallazgos

| # | Categoría | Hallazgo | Tipo | Severidad | Estado | Pregunta |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Dominio y datos | "Exceso de km según regla de negocio definida" — la regla no estaba documentada. Aclarado: CI = `km > km_contratado + 500`; CI Express = `km > km_contratado`. | Omisión | Alta | Resuelto | P1 |
| 2 | Interacción y flujo | 3 textos de mensaje distintos sin indicar cuál aplica dónde. Aclarado: alerta visual = "Contador supera…"; nota registrada = "Se detecta exceso de km al crear/actualizar…". | Inconsistencia | Alta | Resuelto | P2 |
| 3 | Integraciones | CA#3 "nota tipo proveedor" en Proveedor+ y CA#4 "observación órdenes" — no quedaba claro si eran el mismo registro. Aclarado: son dos registros independientes en módulos distintos. | Ambigüedad | Media | Resuelto | P3 |
| 4 | Alcance funcional | Los comentarios añaden "actualizar servicio" sin ser CA formal. Aclarado: es alcance confirmado (alerta + nota Proveedor+), pero observación órdenes NO se actualiza. | Omisión | Media | Resuelto | P4 |
| 5 | Casos borde | CA#5 "no duplicidad de notas" no aclaraba si la alerta visual se repite. Aclarado: la alerta siempre se muestra si hay exceso; la nota no se duplica si ya existe. | Ambigüedad | Media | Resuelto | P5 |
| 6 | Señales de completitud | CA#2 está truncado: "Con el cambio:" sin continuación. Las notas inline y P1/P2 cubren parcialmente la intención (al registrar km, no en cotización). | Omisión | Media | Pendiente de validación | — |
| 7 | Alcance funcional | El "Como" dice "auditor de taller" pero el usuario real es "proveedor / asesor de taller". El auditor es el beneficiario indirecto; el actor en sistema es el proveedor. | Inconsistencia | Baja | Pendiente de validación | — |
| 8 | Interacción y flujo | Mecanismo UX de la alerta no especificado (modal, banner, toast). Se difiere al diseño/implementación. | Omisión | Baja | Diferido | — |
| 9 | Casos borde | No se contempla qué ocurre si la placa no tiene contrato asociado o el servicio de km contratado no responde. | Escenario no contemplado | Baja | Pendiente de validación | — |

## 3. Bitácora de aclaraciones
### Sesión 2026-06-30
- P1: ¿Cuál es la regla de negocio para determinar "exceso de kilometraje"? → R: Dos reglas diferenciadas por app. CI: `km_registrado > km_contratado + 500 km` (tolerancia fija). CI Express: `km_registrado > km_contratado` (sin tolerancia).
- P2: ¿Cómo se distribuyen los mensajes (alerta visual vs nota registrada)? → R: Dos textos distintos. Alerta visual: "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." Nota registrada: "Se detecta exceso de km al crear/actualizar el servicio para el vehículo [Placa]. Notificación visible al proveedor."
- P3: ¿"Nota tipo proveedor" en Proveedor+ y "observación órdenes" son dos registros independientes? → R: Sí, son dos registros separados: uno en el módulo de notas de Proveedor+ y otro en el campo de observaciones de las órdenes asociadas al servicio.
- P4: ¿Actualizar servicio es alcance confirmado de esta HU? → R: Sí, al actualizar un servicio y cambiar el km se aplica la misma lógica (alerta + nota en Proveedor+), pero la observación de las órdenes NO se actualiza.
- P5: ¿La alerta visual se muestra de nuevo aunque ya exista nota? → R: Sí, la alerta siempre se muestra si hay exceso (informativa); la nota en Proveedor+ / observación órdenes no se duplica si ya existe.

## 4. Pendientes de validación (no bloqueantes)
- CA#2 truncado ("Con el cambio:" sin terminar). Se infiere del contexto que el momento es al registrar el km, no al cotizar. — _responsable sugerido: PO_
- Rol del "Como": auditor de taller vs proveedor/asesor de taller. El actor en el sistema es el proveedor; el auditor es beneficiario indirecto. — _responsable sugerido: PO_
- Comportamiento si la placa no tiene contrato asociado o el servicio de consulta de km contratado no está disponible. — _responsable sugerido: PO/Desarrollo_

## 5. Bloqueantes (100% requeridos)
- ninguno

## 6. Sugerencias de criterios de aceptación para el PO _(opcional)_

- **SUGERENCIA — requiere validación del PO**: **CA#6 — Actualizar servicio**: "Dado que el proveedor actualiza un servicio existente y modifica el km registrado, Y el nuevo km excede el km contratado (según regla por app), Entonces el sistema muestra la alerta visual Y registra/actualiza nota en Proveedor+ (con texto crear/actualizar), PERO NO modifica ni elimina la observación de las órdenes ya creadas."

- **SUGERENCIA — requiere validación del PO**: **CA#7 — Actualizar km por debajo del contratado**: "Dado que el proveedor actualiza el km del servicio a un valor por debajo del km contratado, Y ya existían órdenes con observación de exceso, Entonces las observaciones en las órdenes existentes NO se modifican ni eliminan."

- **SUGERENCIA — requiere validación del PO**: **CA#8 — Edición de km por error**: "Dado que el proveedor guardó un km incorrecto y edita el servicio para corregirlo, Entonces se debe registrar una nota: 'Se actualiza lectura de kilometraje; tras validación de consistencia, se autoriza continuidad de la cotización.'"

- **SUGERENCIA — requiere validación del PO**: **CA#9 — Exclusión de siniestro**: "Dado que el tipo de servicio es 'siniestro' o 'siniestro póliza', Entonces NO se evalúa exceso de kilometraje y NO se muestra alerta ni se registra nota."

---
## 🔗 Hand-off
- **Artefacto**: `resultado/HU-145865/02-reporte-clarificacion-HU-145865.md`
- **Producido por**: qa-refinement
- **Estado**: Completado
- **Pendientes de validación**: CA#2 truncado (momento inferido); rol auditor vs proveedor; comportamiento sin contrato asociado
- **Siguiente agente sugerido**: @qa-gap-analysis o @qa-test-design
- **Notas para el siguiente agente**: Reglas de exceso diferenciadas por app (CI: +500 km tolerancia, CI Express: sin tolerancia). Dos registros independientes (nota Proveedor+ y observación órdenes). Actualizar servicio es alcance (alerta + nota, pero observación órdenes no se toca). Siniestro y siniestro póliza excluidos.
---

## 🔗 Conexiones
- Siguiente artefacto: [[03-reportes-gaps-HU-145865]]
