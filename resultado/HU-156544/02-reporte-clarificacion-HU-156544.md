# Reporte de Clarificación — HU-156544

## 1. Resumen del análisis
- **HU analizada**: 156544 — Alerta temprana de exceso de kilometraje al crear el servicio
- **Veredicto**: Sin bloqueantes, se puede avanzar
- **Preguntas hechas / respondidas**: 5 / 5

## 2. Matriz de hallazgos

| # | Categoría | Hallazgo | Tipo | Severidad | Estado | Pregunta |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Dominio y datos | No se define la "regla de negocio" de exceso de km. CA#1 dice "según la regla de negocio definida" pero no la especifica. | Omisión | Alta | Resuelto | P1 |
| 2 | Alcance funcional | CA#2 está incompleto: dice "Con el cambio:" y se corta. Los comentarios sueltos contienen la respuesta pero fuera del CA. Hay dos momentos de alerta con mensajes distintos. | Inconsistencia | Alta | Resuelto | P2 |
| 3 | Alcance funcional | CA#3 y CA#4 son casi idénticos (mismo Dado/Entonces/mensaje). La diferencia es el destino: Proveedor+ vs Órdenes. | Ambigüedad | Media | Resuelto | P3 |
| 4 | Alcance funcional | Comportamiento al Actualizar servicio: los comentarios lo mencionan pero no hay CA formal. | Omisión | Alta | Resuelto | P4 |
| 5 | Casos borde | Dos mensajes distintos en la HU sin aclarar cuál es definitivo ni cuándo aparece cada uno. | Inconsistencia | Media | Resuelto | P5 |
| 6 | Interacción y flujo | CA#5 mezcla tres comportamientos: no duplicidad de notas, momento de la alerta y carácter informativo. Estructuralmente confuso pero los comportamientos son identificables tras las aclaraciones. | Ambigüedad | Baja | Resuelto | — |
| 7 | Casos borde | Al actualizar km por debajo del contratado, los comentarios dicen que NO se modifica/elimina la observación en Órdenes. ¿Aplica igual para la nota en Proveedor+? | Omisión | Media | Resuelto | P6 |
| 8 | Dominio y datos | Nota por error de digitación (CA#5): ¿dónde se registra esa nota de corrección? ¿En Proveedor+, en observación de órdenes, o en ambos? | Ambigüedad | Baja | Resuelto | P7 |

## 3. Bitácora de aclaraciones
### Sesión 2026-07-01
- P1: ¿Cuál es la condición de exceso de km para CI y CI Express? → R: Exceso en CI = km_registrado > km_contratado + 500. Exceso en CI Express = km_registrado > km_contratado. Umbrales distintos por aplicación.
- P2: ¿Cuál es el nuevo comportamiento del momento de la alerta (CA#2 incompleto)? → R: Dos momentos: (1) al salir del campo Contador → mensaje informativo, no bloquea; (2) al dar clic en Guardar el Servicio → alerta, no bloquea. Ambos no restrictivos.
- P3: ¿CA#3 y CA#4 registran en destinos distintos o es duplicado? → R: CA#3 = nota automática en el módulo Notas de Proveedor+ (tipo proveedor). CA#4 = observación automática en Órdenes. Son dos registros distintos que se crean simultáneamente al detectar exceso.
- P4: ¿Qué debe pasar al actualizar un servicio existente? → R: Al actualizar: se evalúa exceso con la misma regla, se muestra alerta informativa, se registra nota en Proveedor+ con mensaje "crear/actualizar". NO se crea ni modifica observación en Órdenes (solo aplica al crear). Si el km actualizado baja del contratado, las observaciones existentes en Órdenes permanecen intactas.
- P5: ¿Cuál es el mensaje definitivo de la alerta? → R: Son dos mensajes distintos en dos momentos: (1) al salir del campo Contador: "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." (2) Al dar clic en Guardar el Servicio: "El vehículo [Placa] presenta exceso de kilometraje según las condiciones contractuales. Tenga en cuenta que el cliente puede no aceptar los valores asociados a este exceso."
- P6: Al actualizar km por debajo del contratado: ¿la nota en Proveedor+ permanece intacta? → R (PO, 2026-07-01): La nota previa permanece Y se adiciona nueva nota con texto: "Se actualiza lectura de kilometraje; tras validación de consistencia, se autoriza continuidad de la cotización."
- P7: Nota por error de digitación: ¿se registra en Proveedor+, en Órdenes, o en ambos? → R (PO, 2026-07-01): Solo en Proveedor+ (módulo Notas). No se modifica observación en Órdenes.

## 4. Pendientes de validación (no bloqueantes)

- ~~Al actualizar km por debajo del contratado: ¿la nota en Proveedor+ también permanece intacta?~~ → **Resuelto (PO, 2026-07-01)**: La nota previa de exceso en Proveedor+ permanece intacta Y se adiciona nueva nota: "Se actualiza lectura de kilometraje; tras validación de consistencia, se autoriza continuidad de la cotización."
- ~~Nota por error de digitación: ¿se registra en Proveedor+, en Órdenes, o en ambos?~~ → **Resuelto (PO, 2026-07-01)**: La nota de corrección se registra **únicamente en Proveedor+** (módulo Notas). No se modifica observación en Órdenes.

## 5. Bloqueantes (100% requeridos)

- ninguno

## 6. Sugerencias de criterios de aceptación para el PO _(opcional)_

- **SUGERENCIA — requiere validación del PO**: Formalizar un **CA#6 — Comportamiento al actualizar servicio**: "Dado que se actualiza un servicio existente y el km registrado excede el umbral, Entonces el sistema muestra alerta informativa y registra nota en Proveedor+ con mensaje de crear/actualizar. No se crea ni modifica observación en Órdenes."
- **SUGERENCIA — requiere validación del PO**: Separar CA#2 en dos momentos explícitos: (a) al salir del campo Contador → mensaje en pantalla; (b) al Guardar → alerta en pantalla. Ambos informativos, no bloqueantes.
- **SUGERENCIA — requiere validación del PO**: Especificar en CA#1 la regla de exceso explícitamente: CI = km_registrado > km_contratado + 500; CI Express = km_registrado > km_contratado.

---
## 🔗 Hand-off
- **Artefacto**: `resultado/HU-156544/02-reporte-clarificacion-HU-156544.md`
- **Producido por**: qa-refinement
- **Estado**: Completado
- **Pendientes de validación**: ninguno — resueltos por PO el 2026-07-01
- **Siguiente agente sugerido**: @qa-gap-analysis (o @qa-test-design)
- **Notas para el siguiente agente**: Umbrales de exceso aclarados (CI ≠ CI Express). Dos momentos de alerta con mensajes distintos. Al actualizar: solo nota en Proveedor+, no en Órdenes. Siniestro/Siniestro póliza excluidos. Al bajar km bajo contratado: nota de corrección solo en Proveedor+, nota previa permanece.
---

## 🔗 Conexiones
- Siguiente artefacto: [[03-reportes-gaps.template]]
