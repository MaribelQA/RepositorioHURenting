# Reporte de Clarificación — HU-145877

## 1. Resumen del análisis
- **HU analizada**: 145877 — Alerta temprana de exceso de kilometraje al crear el servicio
- **Veredicto**: Sin bloqueantes, se puede avanzar
- **Preguntas hechas / respondidas**: 5 / 5

## 2. Matriz de hallazgos

| # | Categoría | Hallazgo | Tipo | Severidad | Estado | Pregunta |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Dominio y datos | CA#1 dice "según la regla de negocio definida" pero no documenta el umbral/fórmula de exceso de km. | Omisión | Alta | Resuelto | P1 |
| 2 | Interacción y flujo | Hay dos mensajes diferentes en la HU: uno para el campo Contador y otro al Guardar. No estaba claro si eran el mismo o dos alertas distintas. | Ambigüedad | Alta | Resuelto | P2 |
| 3 | Alcance funcional | CA#2 está truncado: dice "Con el cambio:" y no termina. El criterio queda formalmente incompleto. | Omisión | Alta | Resuelto | P2 |
| 4 | Escenario no contemplado | Los comentarios agregan el escenario de Actualizar servicio (no en CAs originales) con comportamiento específico: nota de actualización en Proveedor+ y observaciones de Ordenes intactas. No era un CA formal. | Omisión | Alta | Resuelto | P3 |
| 5 | Integraciones | CA#3 registra nota en Proveedor+ y CA#4 registra en observación de Ordenes. Son dos módulos distintos pero la HU no aclara la diferencia ni si el texto es idéntico en ambos. | Ambigüedad | Media | Resuelto | P4 |
| 6 | Dominio y datos | CI & CI Express: ¿la regla de exceso (umbral/fórmula) es la misma para ambas aplicaciones o difiere? | Omisión | Media | Resuelto | P1 |
| 7 | Terminología | CA#5 mezcla: no-duplicidad de notas, momento de la alerta, mensaje del Contador, y nota de corrección de km. Son al menos 3 comportamientos distintos en un solo criterio. | Inconsistencia | Media | Pendiente de validación | — |
| 8 | Alcance funcional | El actor en la historia dice "auditor de taller" pero los CAs hablan de "proveedor / asesor de taller". | Inconsistencia | Baja | Pendiente de validación | — |
| 9 | Casos borde | ¿Qué pasa con la nota de exceso previa en Proveedor+ si se actualiza km por debajo del contratado? | Escenario no contemplado | Media | Resuelto | P5 |

## 3. Bitácora de aclaraciones
### Sesión 2026-07-03
- P1: ¿Cuál es la regla de negocio para determinar exceso de km? ¿Es igual para CI y CI Express? → R: En CI: `km_ingresado > km_contratado + 500`; en CI Express: `km_ingresado > km_contratado` (umbral diferente por aplicación).
- P2: ¿Los dos mensajes mencionados son dos alertas en dos momentos diferentes? → R: Sí. (1) Al registrar km en campo Contador: "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." (2) Al dar clic en Guardar: "El vehículo [Placa] presenta exceso de kilometraje según las condiciones contractuales. Tenga en cuenta que el cliente puede no aceptar los valores asociados a este exceso." Ambos informativos, no bloquean. Esto también resuelve CA#2 (truncado): la alerta aparece al crear el servicio, no al cotizar.
- P3: ¿El comportamiento al Actualizar un servicio se formaliza? → R: Al actualizar un servicio, independientemente de si el contador supera o no el km contratado, se agrega una nota en Proveedor+: "Se actualiza lectura de kilometraje; tras validación de consistencia." Si tenía ordenes con observación, permanecen tal cual. NO se registra observación en Ordenes al actualizar.
- P4: CA#3 y CA#4: ¿son dos registros separados en módulos diferentes? → R: Sí. (1) Nota tipo proveedor en módulo Notas de Proveedor+, y (2) observación en módulo Ordenes asociadas al servicio. Mismo texto al crear: "Se detecta exceso de km al crear/actualizar el servicio para el vehículo [Placa]. Notificación visible al proveedor."
- P5: ¿Qué pasa con la nota de exceso previa en Proveedor+ cuando se actualiza km por debajo del contratado? → R: La nota de exceso previa permanece intacta. Solo se agrega la nueva nota de actualización. Las notas son históricas, no se eliminan.

## 4. Pendientes de validación (no bloqueantes)
- **#7** — CA#5 mezcla al menos 3 comportamientos distintos (no-duplicidad, momento de alerta, nota de corrección). Se recomienda al PO separar en criterios independientes para mayor claridad. — _responsable sugerido: PO_
- **#8** — Actor inconsistente: "auditor de taller" (historia) vs "proveedor / asesor de taller" (criterios). Confirmar quién es el actor principal y cuál es el rol correcto. — _responsable sugerido: PO_

## 5. Bloqueantes (100% requeridos)
- ninguno

## 6. Sugerencias de criterios de aceptación para el PO _(opcional)_

- **SUGERENCIA — requiere validación del PO**: **CA#6 — Alerta y nota al actualizar servicio con exceso de km.** Dado que se actualiza un servicio y el km sigue excediendo el umbral, Entonces el sistema muestra los mensajes de alerta (Contador + Guardar) y registra nota en Proveedor+ con texto "Se detecta exceso de km al crear/actualizar el servicio para el vehículo [Placa]. Notificación visible al proveedor." No se registra ni modifica observación en Ordenes.
- **SUGERENCIA — requiere validación del PO**: **CA#7 — Nota de actualización de lectura al actualizar servicio.** Dado que se actualiza un servicio (independientemente de si hay exceso o no), Entonces el sistema registra nota en Proveedor+: "Se actualiza lectura de kilometraje; tras validación de consistencia." Las notas de exceso previas y las observaciones en Ordenes permanecen intactas.

---
## 🔗 Hand-off
- **Artefacto**: `resultado/HU-145877/02-reporte-clarificacion-HU-145877.md`
- **Producido por**: qa-refinement
- **Estado**: Completado
- **Pendientes de validación**: #7 (CA#5 mezcla comportamientos), #8 (actor inconsistente)
- **Siguiente agente sugerido**: @qa-test-design
- **Notas para el siguiente agente**: Regla de exceso diferente por app (CI: km > km_contratado + 500; CI Express: km > km_contratado). Dos mensajes en dos momentos (Contador + Guardar). Al actualizar: siempre nota de lectura en Proveedor+, no se toca Ordenes. Notas históricas, no se eliminan. Siniestro y Siniestro Póliza excluidos.
---

## 🔗 Conexiones
- Siguiente artefacto: [[04-casos-prueba-HU-145877]]
