# Casos de prueba — HU-145877

## 1. Resumen de análisis de la HU
- **HU**: 145877 — Alerta temprana de exceso de kilometraje al crear el servicio
- **Actor / rol principal**: Proveedor / Asesor de taller
- **Objetivo funcional**: Mostrar alerta de exceso de km al crear/actualizar servicio y registrar nota en Proveedor+ y observación en Ordenes; evitar reprocesos por exceso no informado oportunamente.
- **Reglas relevantes**:
  - CI: exceso si `km_ingresado > km_contratado + 500`
  - CI Express: exceso si `km_ingresado > km_contratado`
  - Exclusión: tipos Siniestro y Siniestro Póliza no aplican la regla
  - Mensaje en Contador (informativo): "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller."
  - Alerta al Guardar (informativa): "El vehículo [Placa] presenta exceso de kilometraje según las condiciones contractuales. Tenga en cuenta que el cliente puede no aceptar los valores asociados a este exceso."
  - Nota Proveedor+ al crear/actualizar con exceso: "Se detecta exceso de km al crear/actualizar el servicio para el vehículo [Placa]. Notificación visible al proveedor."
  - Observación en Ordenes (solo al crear con exceso): mismo texto que Proveedor+
  - Al actualizar, siempre agrega nota de lectura: "Se actualiza lectura de kilometraje; tras validación de consistencia." — sin importar si hay exceso o no
  - Notas y observaciones son históricas: no se eliminan ni modifican
  - No duplicar notas de exceso si ya existe una para ese servicio/vehículo
- **Veredicto de diseño**: Completado
- **Preguntas de aclaración pendientes**: ninguna — ver pendientes de validación PO en `02-reporte-clarificacion-HU-145877.md` (#7 CA#5 mezcla comportamientos; #8 actor inconsistente)

---

## 2. Matrices de diseño

### 2.1 Partición de equivalencia

| Regla / criterio | Partición válida | Partición inválida | Impacto funcional |
| --- | --- | --- | --- |
| CI: km > km_contratado + 500 | km = 101.000 (km_contratado=100.000; exceso) | km = 100.500 (sin exceso) | Dispara alerta, mensaje Contador, notas |
| CI Express: km > km_contratado | km = 85.000 (km_contratado=80.000; exceso) | km = 80.000 (sin exceso) | Dispara alerta, mensaje Contador, notas |
| Tipo de servicio | Normal / tipos no excluidos | Siniestro / Siniestro Póliza | Excluidos no disparan regla |
| Nota de exceso previa en Proveedor+ | Sin nota previa | Con nota de exceso ya registrada | No crear nota duplicada |
| Observación en Ordenes previa | Sin observación previa | Con observación ya registrada | No crear observación duplicada |
| Operación sobre el servicio | Crear servicio | Actualizar servicio | Al actualizar: nota lectura siempre; Ordenes no se tocan |

### 2.2 Valores límite

| Regla / campo | Mínimo (sin exceso) | Primer valor con exceso | Valor claramente excedido | Justificación |
| --- | --- | --- | --- | --- |
| CI — km_contratado = 100.000 | 100.500 (= km_contratado + 500; condición `>` no se cumple) | 100.501 | 101.000 | Umbral CI: km > km_contratado + 500 |
| CI Express — km_contratado = 80.000 | 80.000 (= km_contratado; condición `>` no se cumple) | 80.001 | 85.000 | Umbral CI Express: km > km_contratado |

### 2.3 Tabla de decisión

| App | Operación | Tipo servicio | km excedido | Nota exceso previa | Mensaje Contador | Alerta Guardar | Nota Proveedor+ (exceso) | Obs. Ordenes | Nota Proveedor+ (lectura) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| CI | Crear | Normal | Sí | No | Sí | Sí | Crear | Crear | No |
| CI | Crear | Normal | Sí | Sí | Sí | Sí | No duplicar | No duplicar | No |
| CI | Crear | Normal | No | — | No | No | No | No | No |
| CI | Crear | Siniestro | Sí | — | No | No | No | No | No |
| CI | Crear | Siniestro Póliza | Sí | — | No | No | No | No | No |
| CI Express | Crear | Normal | Sí | No | Sí | Sí | Crear | Crear | No |
| CI Express | Crear | Normal | No | — | No | No | No | No | No |
| CI | Actualizar | Normal | Sí | — | Sí | Sí | No (no duplicar) | No tocar | Sí (siempre) |
| CI | Actualizar | Normal | No | — | No | No | No | No tocar | Sí (siempre) |

---

## 3. Resumen de cobertura
- **HU**: 145877 — Alerta temprana de exceso de kilometraje al crear el servicio
- **Total de casos**: 12 (Happy Path: 3 · Alternos: 3 · Negativos: 4 · Borde: 2)
- **Prioridad**: Alta
- **Insumos usados**: clarificación (`02`) — sin gaps (`03`)
- **Técnicas aplicadas**: partición de equivalencia, valores límite, tabla de decisión, error guessing

---

## 4. Casos (formato Azure DevOps — Work Item Test Case)

> **Estructura:** La fila de precondición lleva `Step Action` (configurar entorno/datos) y `Step Expected Result` vacío. Solo los pasos ejecutables llevan resultado esperado.

| Title | Step Action | Step Expected Result |
| --- | --- | --- |
| TC-01-HU-145877 - Sistema debe mostrar mensaje en Contador y alerta al Guardar y registrar nota en Proveedor+ y observacion en Ordenes cuando km excede umbral al crear servicio en CI | Precondiciones: usuario proveedor/asesor autenticado en CI; vehiculo con km_contratado = 100.000 km tipo Normal disponible; sin nota de exceso previa. Dato de prueba: km a ingresar = 101.000 (101.000 > 100.500; exceso en CI). Clasificacion: Happy Path. Criterio: CA#1 CA#2 CA#3 CA#4. |  |
|  | Navegar a creacion de servicios en CI e ingresar la placa del vehiculo. | El sistema recupera los datos del vehiculo y muestra km_contratado = 100.000 km. |
|  | Ingresar km = 101.000 en el campo Contador y salir del campo (Tab o clic fuera). | El sistema muestra mensaje informativo: "Contador supera el kilometraje maximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." No bloquea la creacion. |
|  | Completar campos requeridos del servicio y dar clic en Guardar. | El sistema muestra alerta: "El vehiculo [Placa] presenta exceso de kilometraje segun las condiciones contractuales. Tenga en cuenta que el cliente puede no aceptar los valores asociados a este exceso." No bloquea; servicio creado exitosamente. |
|  | Navegar al modulo Notas de Proveedor+ y filtrar por el vehiculo/servicio creado. | Existe nota de tipo proveedor con texto: "Se detecta exceso de km al crear/actualizar el servicio para el vehiculo [Placa]. Notificacion visible al proveedor." |
|  | Navegar a las Ordenes asociadas al servicio y revisar observaciones. | Existe observacion con texto: "Se detecta exceso de km al crear/actualizar el servicio para el vehiculo [Placa]. Notificacion visible al proveedor." |
| TC-02-HU-145877 - Sistema debe mostrar mensaje en Contador y alerta al Guardar y registrar nota en Proveedor+ y observacion en Ordenes cuando km excede umbral al crear servicio en CI Express | Precondiciones: usuario autenticado en CI Express; vehiculo con km_contratado = 80.000 km tipo Normal disponible; sin nota de exceso previa. Dato de prueba: km a ingresar = 85.000 (85.000 > 80.000; exceso en CI Express). Clasificacion: Happy Path. Criterio: CA#1 CA#2 CA#3 CA#4. |  |
|  | Navegar a creacion de servicios en CI Express e ingresar la placa del vehiculo. | El sistema recupera datos; muestra km_contratado = 80.000 km. |
|  | Ingresar km = 85.000 en campo Contador y salir del campo. | El sistema muestra mensaje informativo: "Contador supera el kilometraje maximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." No bloquea. |
|  | Completar campos requeridos y dar clic en Guardar. | Alerta: "El vehiculo [Placa] presenta exceso de kilometraje segun las condiciones contractuales..." No bloquea; servicio creado. |
|  | Verificar nota en Proveedor+ (modulo Notas, tipo proveedor). | Nota registrada: "Se detecta exceso de km al crear/actualizar el servicio para el vehiculo [Placa]. Notificacion visible al proveedor." |
|  | Verificar observacion en Ordenes asociadas. | Observacion registrada con el mismo texto de exceso. |
| TC-03-HU-145877 - Sistema debe mostrar alertas y registrar nota de lectura en Proveedor+ sin modificar Ordenes al actualizar servicio con km excedido | Precondiciones: usuario autenticado en CI; servicio existente en modo edicion; km_contratado = 100.000 km tipo Normal; sin nota de lectura previa. Dato de prueba: km a actualizar = 102.000 (exceso). Clasificacion: Happy Path. Criterio: CA#6 (sugerido PO). |  |
|  | Abrir el servicio existente en modo edicion. | Formulario carga con los datos actuales del servicio. |
|  | Actualizar km a 102.000 en campo Contador y salir del campo. | El sistema muestra mensaje informativo de exceso: "Contador supera el kilometraje maximo contratado..." |
|  | Dar clic en Guardar. | Alerta de exceso mostrada. No bloquea; servicio actualizado correctamente. |
|  | Verificar modulo Notas de Proveedor+ para el vehiculo/servicio. | Nueva nota agregada: "Se actualiza lectura de kilometraje; tras validacion de consistencia." |
|  | Verificar observaciones en Ordenes asociadas al servicio. | Las observaciones en Ordenes permanecen intactas (mismo contenido y conteo que antes de la actualizacion). |
| TC-04-HU-145877 - Sistema NO debe duplicar nota en Proveedor+ ni observacion en Ordenes al volver a guardar servicio cuando ya existen registros de exceso | Precondiciones: usuario autenticado en CI; servicio existente con nota de exceso en Proveedor+ (conteo = N) y observacion en Ordenes (conteo = M). Clasificacion: Alterno. Criterio: CA#5. |  |
|  | Registrar el conteo actual de notas tipo proveedor en Proveedor+ (N) y de observaciones en Ordenes (M) para ese servicio/vehiculo. | Conteo N y M registrados como referencia. |
|  | Abrir el servicio y dar clic en Guardar sin modificar el km. | Sistema procesa sin errores; servicio guardado. |
|  | Verificar modulo Notas de Proveedor+ para el vehiculo/servicio. | El numero de notas tipo proveedor sigue siendo N. No se creo nota duplicada de exceso. |
|  | Verificar observaciones en Ordenes asociadas. | El numero de observaciones en Ordenes sigue siendo M. No se creo observacion duplicada. |
| TC-05-HU-145877 - Sistema NO debe duplicar nota de exceso en Proveedor+ al actualizar servicio cuando km sigue excedido y ya existe nota previa | Precondiciones: usuario autenticado en CI; servicio con nota de exceso en Proveedor+ (conteo = N). Dato de prueba: km a actualizar = 103.000 (sigue excediendo 103.000 > 100.500). Clasificacion: Alterno. Criterio: CA#5 CA#6 (sugerido). |  |
|  | Registrar el conteo actual de notas tipo proveedor en Proveedor+ (N). | Conteo N registrado como referencia. |
|  | Abrir el servicio en modo edicion, actualizar km a 103.000, salir del campo y Guardar. | Mensaje de exceso en Contador y alerta al Guardar visibles. Servicio actualizado. |
|  | Verificar modulo Notas de Proveedor+ para el vehiculo/servicio. | El conteo de notas es N+1: se agrego solo la nota de lectura "Se actualiza lectura de kilometraje...". No se creo nota duplicada de exceso. |
| TC-06-HU-145877 - Sistema debe agregar solo nota de lectura en Proveedor+ y conservar notas e historial al actualizar km por debajo del km contratado | Precondiciones: usuario autenticado en CI; servicio existente con nota de exceso en Proveedor+ (conteo = N) y observacion en Ordenes; km_contratado = 100.000. Dato de prueba: km a actualizar = 90.000 (por debajo del contratado). Clasificacion: Alterno. Criterio: CA#7 (sugerido PO). |  |
|  | Abrir el servicio en modo edicion, registrar conteo N de notas en Proveedor+ y estado de observaciones en Ordenes. | Formulario carga; conteo y estado registrados como referencia. |
|  | Actualizar km a 90.000 en campo Contador y salir del campo. | No se muestra mensaje informativo de exceso (km no supera el umbral). |
|  | Dar clic en Guardar. | No se muestra alerta de exceso. Servicio actualizado correctamente. |
|  | Verificar modulo Notas de Proveedor+ para el vehiculo/servicio. | Nueva nota agregada: "Se actualiza lectura de kilometraje; tras validacion de consistencia." Conteo = N+1. La nota de exceso previa permanece intacta. |
|  | Verificar observaciones en Ordenes asociadas. | Las observaciones en Ordenes permanecen intactas: mismo contenido y conteo que antes de la actualizacion. |
| TC-07-HU-145877 - Sistema NO debe mostrar alerta cuando km es exactamente igual a km_contratado mas 500 en CI (umbral exacto sin exceso) | Precondiciones: usuario autenticado en CI; vehiculo con km_contratado = 100.000 km tipo Normal disponible. Dato de prueba: km = 100.500 (100.500 > 100.500 es falso; sin exceso). Clasificacion: Negativo. Criterio: CA#1. |  |
|  | Crear servicio en CI. Ingresar km = 100.500 en campo Contador y salir del campo. | No se muestra mensaje informativo. El sistema no detecta exceso de km. |
|  | Completar formulario y dar clic en Guardar. | No se muestra alerta de exceso. Servicio creado normalmente. |
|  | Verificar Proveedor+ y Ordenes asociadas al servicio. | No se registro nota de exceso en Proveedor+ ni observacion en Ordenes por exceso de km. |
| TC-08-HU-145877 - Sistema NO debe mostrar alerta cuando km es exactamente igual a km_contratado en CI Express (umbral exacto sin exceso) | Precondiciones: usuario autenticado en CI Express; vehiculo con km_contratado = 80.000 km tipo Normal disponible. Dato de prueba: km = 80.000 (80.000 > 80.000 es falso; sin exceso). Clasificacion: Negativo. Criterio: CA#1. |  |
|  | Crear servicio en CI Express. Ingresar km = 80.000 en campo Contador y salir del campo. | No se muestra mensaje informativo. El sistema no detecta exceso. |
|  | Guardar el servicio. | No se muestra alerta. Servicio creado normalmente. |
|  | Verificar Proveedor+ y Ordenes. | No se registro nota ni observacion por exceso de km. |
| TC-09-HU-145877 - Sistema NO debe evaluar exceso ni mostrar alerta cuando tipo de servicio es Siniestro aunque km sea claramente excedido | Precondiciones: usuario autenticado en CI; vehiculo con km_contratado = 100.000 km disponible. Dato de prueba: tipo de servicio = Siniestro; km = 150.000 (claramente superior al umbral). Clasificacion: Negativo. Criterio: NOTA HU (exclusion Siniestro). |  |
|  | Crear servicio tipo Siniestro en CI. Ingresar km = 150.000 en campo Contador y salir del campo. | No se muestra mensaje informativo de exceso en campo Contador. |
|  | Guardar el servicio. | No se muestra alerta de exceso. Servicio creado normalmente. |
|  | Verificar Proveedor+ y Ordenes. | No se registro nota de exceso en Proveedor+ ni observacion en Ordenes. |
| TC-10-HU-145877 - Sistema NO debe evaluar exceso ni mostrar alerta cuando tipo de servicio es Siniestro Poliza aunque km sea claramente excedido | Precondiciones: usuario autenticado en CI; vehiculo con km_contratado = 100.000 km disponible. Dato de prueba: tipo de servicio = Siniestro Poliza; km = 150.000. Clasificacion: Negativo. Criterio: NOTA HU (exclusion Siniestro Poliza). |  |
|  | Crear servicio tipo Siniestro Poliza en CI. Ingresar km = 150.000 en Contador y salir del campo. | No se muestra mensaje informativo de exceso. |
|  | Guardar el servicio. | No se muestra alerta. Servicio creado normalmente. |
|  | Verificar Proveedor+ y Ordenes. | No se registro nota ni observacion por exceso de km. |
| TC-11-HU-145877 - Sistema debe detectar exceso y mostrar alerta cuando km es km_contratado mas 501 en CI (primer valor con exceso valido) | Precondiciones: usuario autenticado en CI; vehiculo con km_contratado = 100.000 km tipo Normal disponible; sin nota previa. Dato de prueba: km = 100.501 (100.501 > 100.500; primer valor que satisface la condicion). Clasificacion: Borde. Criterio: CA#1 CA#2. |  |
|  | Crear servicio en CI. Ingresar km = 100.501 en campo Contador y salir del campo. | El sistema detecta exceso y muestra mensaje: "Contador supera el kilometraje maximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." |
|  | Guardar el servicio. | Alerta de exceso mostrada. Servicio creado. Nota registrada en Proveedor+ y observacion en Ordenes. |
| TC-12-HU-145877 - Sistema debe detectar exceso y mostrar alerta cuando km es km_contratado mas 1 en CI Express (primer valor con exceso valido) | Precondiciones: usuario autenticado en CI Express; vehiculo con km_contratado = 80.000 km tipo Normal disponible; sin nota previa. Dato de prueba: km = 80.001 (80.001 > 80.000; primer valor que satisface la condicion). Clasificacion: Borde. Criterio: CA#1 CA#2. |  |
|  | Crear servicio en CI Express. Ingresar km = 80.001 en campo Contador y salir del campo. | El sistema detecta exceso y muestra mensaje: "Contador supera el kilometraje maximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." |
|  | Guardar el servicio. | Alerta de exceso mostrada. Servicio creado. Nota registrada en Proveedor+ y observacion en Ordenes. |

---

## 5. Matriz de trazabilidad final

| Criterio de aceptacion | Casos que lo cubren | Estado de cobertura |
| --- | --- | --- |
| CA#1 — Alerta de exceso al crear; regla de negocio | TC-01 TC-02 TC-07 TC-08 TC-11 TC-12 | Completo |
| CA#2 — Momento de la alerta (al crear, no al cotizar); dos mensajes distintos | TC-01 TC-02 TC-11 TC-12 | Completo |
| CA#3 — Registro nota Proveedor+ al crear con exceso | TC-01 TC-02 TC-11 TC-12 | Completo |
| CA#4 — Registro observacion en Ordenes al crear con exceso | TC-01 TC-02 TC-11 TC-12 | Completo |
| CA#5 — No duplicidad de notas y observaciones | TC-04 TC-05 | Completo |
| NOTA HU — Exclusion Siniestro y Siniestro Poliza | TC-09 TC-10 | Completo |
| CA#6 (sugerido) — Alerta y nota al actualizar con exceso | TC-03 TC-05 | Completo |
| CA#7 (sugerido) — Nota de lectura siempre al actualizar; historico intacto | TC-03 TC-05 TC-06 | Completo |
| Borde — Primer valor con exceso (limite exacto + 1) | TC-11 TC-12 | Completo |
| Borde — Valor exacto en umbral sin exceso | TC-07 TC-08 | Completo |

## 6. Cobertura pendiente
> Criterios que dependen de pendientes de validacion del PO (no bloqueantes).

- **CA#5 mezclado (hallazgo #7)** — Si el PO separa CA#5 en criterios independientes, los casos TC-04 y TC-05 podrian requerir ajuste de titulo y clasificacion. La cobertura funcional ya existe. — _depende de: respuesta del PO al hallazgo #7 del `02-reporte-clarificacion-HU-145877.md`_
- **Actor (#8)** — Si el PO define que el "auditor de taller" es un rol distinto al "proveedor/asesor", podria requerirse un caso adicional verificando que el rol correcto ve las alertas. — _depende de: confirmacion del PO al hallazgo #8_

---
## 🔗 Hand-off
- **Artefacto**: `resultado/HU-145877/04-casos-prueba-HU-145877.md`
- **Producido por**: qa-test-design
- **Estado**: Completado
- **Pendientes de validacion**: hallazgo #7 (CA#5 mezclado) y #8 (actor) — no bloquean; cobertura funcional completa
- **Siguiente agente sugerido**: @qa-ado-registration
- **Notas para el siguiente agente**: 12 casos listos. CA#6 y CA#7 son sugerencias al PO; registrar como Test Cases independientes. Siniestro y Siniestro Poliza son casos negativos criticos. Persistencia validada por UI (Proveedor+ y Ordenes).
---

## 🔗 Conexiones
- Siguiente artefacto: [[05-registro-ado-HU-145877]]
