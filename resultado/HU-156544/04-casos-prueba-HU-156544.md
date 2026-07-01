# Casos de prueba — HU-156544

## 1. Resumen de análisis de la HU
- **HU**: 156544 — Alerta temprana de exceso de kilometraje al crear el servicio
- **Actor / rol principal**: Proveedor / Asesor de taller
- **Objetivo funcional**: Mostrar alerta informativa de exceso de km en dos momentos durante la creación/actualización del servicio, y registrar automáticamente nota en Proveedor+ y observación en Órdenes, sin bloquear el flujo.
- **Reglas relevantes**:
  - CI: exceso = km_registrado **>** km_contratado + 500
  - CI Express: exceso = km_registrado **>** km_contratado
  - Siniestro y Siniestro Póliza: **excluidos**, no evalúan exceso
  - Momento 1 (campo Contador): mensaje "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller."
  - Momento 2 (Guardar): alerta "El vehículo [Placa] presenta exceso de kilometraje según las condiciones contractuales. Tenga en cuenta que el cliente puede no aceptar los valores asociados a este exceso."
  - Al crear: nota en Proveedor+ (tipo proveedor) + observación en Órdenes
  - Al actualizar: nota en Proveedor+ únicamente. Observaciones en Órdenes NO se crean ni modifican
  - No duplicidad: si ya existe nota de exceso para ese servicio/vehículo, no se duplica
  - Mensaje de nota (crear/actualizar): "Se detecta exceso de km al crear/actualizar el servicio para el vehículo [Placa]. Notificación visible al proveedor."
  - Al bajar km bajo contratado (actualizar): nota previa en Proveedor+ permanece + se adiciona nueva nota: "Se actualiza lectura de kilometraje; tras validación de consistencia, se autoriza continuidad de la cotización." Solo en Proveedor+, no en Órdenes.
- **Veredicto de diseño**: Completado
- **Preguntas de aclaración pendientes**: ninguna — ver `02-reporte-clarificacion-HU-156544.md`

---

## 2. Matrices de diseño

### 2.1 Partición de equivalencia

| Regla / criterio | Partición válida (activa alerta) | Partición inválida (sin alerta) | Impacto funcional |
| --- | --- | --- | --- |
| CA#1 — Exceso km en CI | km_registrado > km_contratado + 500 | km_registrado ≤ km_contratado + 500 | Activa ambos mensajes y registra notas |
| CA#1 — Exceso km en CI Express | km_registrado > km_contratado | km_registrado ≤ km_contratado | Activa ambos mensajes y registra notas |
| NOTA — Tipo de servicio | Normal (y otros no excluidos) | Siniestro, Siniestro Póliza | Tipos excluidos: no se evalúa exceso |
| CA#5 — Nota previa existente | Sin nota de exceso previa para ese servicio/vehículo | Con nota de exceso ya registrada | Sin nota: crea nueva; con nota: no duplica |
| CA#6 — Operación | Crear servicio | Actualizar servicio | Crear: nota Proveedor+ + obs Órdenes; Actualizar: solo nota Proveedor+ |

### 2.2 Valores límite

| Regla / campo | Límite sin exceso | Primer valor con exceso | Justificación |
| --- | --- | --- | --- |
| km_registrado — CI (km_contratado = 100.000) | 100.500 (= km_contratado + 500, condición > es estricta) | 100.501 | El umbral usa > no ≥; 100.500 no activa la alerta |
| km_registrado — CI Express (km_contratado = 80.000) | 80.000 (= km_contratado, condición > es estricta) | 80.001 | El umbral usa > no ≥; 80.000 no activa la alerta |

### 2.3 Tabla de decisión

| App | km excedido | Tipo servicio | Operación | Nota previa | Msj. Contador | Alerta Guardar | Nota Proveedor+ | Obs. Órdenes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| CI | Sí | Normal | Crear | No | ✅ | ✅ | ✅ (crear) | ✅ |
| CI | Sí | Normal | Crear | Sí | ✅ | ✅ | ❌ no duplica | ❌ no duplica |
| CI | No | Normal | Crear | N/A | ❌ | ❌ | ❌ | ❌ |
| CI | Sí | Siniestro / Siniestro Póliza | Crear | N/A | ❌ | ❌ | ❌ | ❌ |
| CI | Sí | Normal | Actualizar | No | ✅ | ✅ | ✅ (crear/actualizar) | ❌ |
| CI | Sí | Normal | Actualizar | Sí | ✅ | ✅ | ❌ no duplica | ❌ |
| CI | Baja km < contratado | Normal | Actualizar | Sí | ❌ | ❌ | ✅ nueva nota corrección en Proveedor+ (nota previa permanece intacta) | Intacta (sin cambio) |
| CI Express | Sí | Normal | Crear | No | ✅ | ✅ | ✅ (crear) | ✅ |
| CI Express | No | Normal | Crear | N/A | ❌ | ❌ | ❌ | ❌ |

---

## 3. Resumen de cobertura
- **HU**: 156544 — Alerta temprana de exceso de kilometraje al crear el servicio
- **Total de casos**: 13 (Happy Path: 3 · Alternos: 4 · Negativos: 4 · Borde: 2)
- **Prioridad**: Alta
- **Insumos usados**: clarificación (`02`)
- **Técnicas aplicadas**: Partición de equivalencia, Valores límite, Tabla de decisión, Error guessing

---

## 4. Casos (formato Azure DevOps — Work Item Test Case)
> Cada `Step Action` ejecutable debe tener su `Stept Expected Result`. La fila `Precondiciones` puede dejarlo vacío.

| Title | Step Action | Stept Expected Result |
| --- | --- | --- |
| TC-01-HU-156544 - Sistema debe mostrar mensaje informativo en Contador y alerta al Guardar, y registrar nota en Proveedor+ y observación en Órdenes cuando km excede umbral al crear servicio en CI | Precondiciones: Usuario con perfil proveedor/asesor autenticado en CI. Vehículo con placa válida, contrato activo y km_contratado = 100.000 km. Tipo servicio = Normal. Sin nota de exceso previa para este servicio/vehículo. Datos de prueba: km a ingresar en Contador = 101.000 (101.000 > 100.500 → exceso en CI). Clasificación: Happy Path. Criterio cubierto: CA#1, CA#2, CA#3, CA#4. |  |
|  | Navegar al módulo de creación de servicios en CI e ingresar la placa del vehículo. | El sistema recupera los datos del vehículo y muestra km_contratado = 100.000 km. |
|  | Ingresar km = 101.000 en el campo Contador y salir del campo (Tab o clic fuera). | El sistema detecta exceso de km de inmediato y muestra mensaje informativo: "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." El mensaje no bloquea la creación. |
|  | Completar los demás campos requeridos del formulario y dar clic en Guardar el Servicio. | Se muestra alerta: "El vehículo [Placa] presenta exceso de kilometraje según las condiciones contractuales. Tenga en cuenta que el cliente puede no aceptar los valores asociados a este exceso." La alerta no bloquea; el servicio queda creado. |
|  | Navegar al módulo Notas de Proveedor+ y filtrar por el vehículo/servicio creado. | Existe nota de tipo proveedor con texto: "Se detecta exceso de km al crear el servicio para el vehículo [Placa]. Notificación visible al proveedor." |
|  | Navegar a las Órdenes asociadas al servicio y revisar observaciones. | Existe observación con el mismo texto: "Se detecta exceso de km al crear el servicio para el vehículo [Placa]. Notificación visible al proveedor." |
| TC-02-HU-156544 - Sistema debe mostrar mensaje informativo en Contador y alerta al Guardar, y registrar nota en Proveedor+ y observación en Órdenes cuando km excede umbral al crear servicio en CI Express | Precondiciones: Usuario autenticado en CI Express. Vehículo con contrato activo, km_contratado = 80.000 km. Tipo servicio = Normal. Sin nota de exceso previa. Datos de prueba: km a ingresar = 85.000 (85.000 > 80.000 → exceso en CI Express). Clasificación: Happy Path. Criterio cubierto: CA#1, CA#2, CA#3, CA#4. |  |
|  | Navegar a creación de servicios en CI Express e ingresar placa del vehículo. | Sistema recupera datos; km_contratado = 80.000 km. |
|  | Ingresar km = 85.000 en campo Contador y salir del campo. | Mensaje informativo: "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." No bloquea. |
|  | Completar datos y dar clic en Guardar. | Alerta: "El vehículo [Placa] presenta exceso de kilometraje según las condiciones contractuales..." No bloquea; servicio creado. |
|  | Verificar nota en Proveedor+ (módulo Notas, tipo proveedor). | Nota registrada: "Se detecta exceso de km al crear el servicio para el vehículo [Placa]. Notificación visible al proveedor." |
|  | Verificar observación en Órdenes asociadas. | Observación registrada con el mismo texto. |
| TC-03-HU-156544 - Sistema debe mostrar alerta y registrar nota en Proveedor+ pero NO crear ni modificar observación en Órdenes al actualizar servicio con km excedido en CI | Precondiciones: Usuario autenticado en CI. Servicio existente en estado editable, km_contratado = 100.000 km. Sin nota de exceso previa. Tipo = Normal. Órdenes asociadas al servicio ya creadas. Datos de prueba: km a actualizar = 101.000 (exceso en CI). Clasificación: Happy Path. Criterio cubierto: CA#6 (actualizar), CA#3, CA#4 (negativo en actualizar). |  |
|  | Abrir el servicio existente en modo edición. | Formulario de edición carga con datos actuales del servicio. |
|  | Actualizar km a 101.000 en campo Contador y salir del campo. | Mensaje informativo: "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." No bloquea. |
|  | Dar clic en Guardar / Actualizar el Servicio. | Alerta: "El vehículo [Placa] presenta exceso de kilometraje según las condiciones contractuales..." No bloquea; servicio actualizado. |
|  | Verificar nota en Proveedor+ (módulo Notas, tipo proveedor). | Nota registrada: "Se detecta exceso de km al crear/actualizar el servicio para el vehículo [Placa]. Notificación visible al proveedor." |
|  | Verificar observaciones en Órdenes asociadas al servicio. | No se crea ni modifica ninguna observación en Órdenes. El conteo y contenido de observaciones permanece igual al estado previo. |
| TC-04-HU-156544 - Sistema NO debe duplicar nota en Proveedor+ ni observación en Órdenes al reabrir servicio cuando ya existe nota de exceso registrada en CI | Precondiciones: Usuario autenticado en CI. Servicio existente con km excedido. Ya existe nota tipo proveedor en Proveedor+ y observación en Órdenes para este servicio/vehículo. Datos de prueba: conteo actual de notas tipo proveedor = N; conteo de observaciones en Órdenes = M. Clasificación: Alterno. Criterio cubierto: CA#5 (no duplicidad). |  |
|  | Abrir el servicio que ya tiene nota de exceso registrada. | Servicio carga. La alerta o mensaje puede mostrarse al cargar. |
|  | Sin modificar datos, dar clic en Guardar. | Sistema procesa sin errores. Servicio guardado. |
|  | Verificar módulo Notas de Proveedor+ para el vehículo/servicio. | El número de notas tipo proveedor sigue siendo N. No se creó nota duplicada. |
|  | Verificar observaciones en Órdenes asociadas. | El número de observaciones sigue siendo M. No se creó observación duplicada. |
| TC-05-HU-156544 - Sistema NO debe duplicar nota en Proveedor+ al actualizar servicio cuando ya existe nota de exceso y el km sigue excedido | Precondiciones: Usuario autenticado en CI. Servicio con nota de exceso ya registrada en Proveedor+. Al actualizar, km sigue excediendo el umbral. Datos de prueba: conteo de notas tipo proveedor = N; km actualizado = 102.000 (sigue excediendo). Clasificación: Alterno. Criterio cubierto: CA#5 (no duplicidad al actualizar). |  |
|  | Abrir el servicio con nota previa de exceso en modo edición. | Formulario carga con datos actuales. |
|  | Actualizar km a 102.000, salir del campo y dar clic en Guardar. | Mensaje en Contador y alerta al guardar se muestran. Servicio actualizado. |
|  | Verificar módulo Notas de Proveedor+ para el vehículo/servicio. | El número de notas tipo proveedor sigue siendo N. No se creó nota duplicada. |
| TC-06-HU-156544 - Sistema NO debe modificar ni eliminar la observación en Órdenes al actualizar km por debajo del km contratado cuando ya existe observación registrada | Precondiciones: Usuario autenticado en CI. Servicio existente con km excedido y observación de exceso registrada en Órdenes. Órdenes asociadas creadas. Datos de prueba: km_contratado = 100.000. km a actualizar = 90.000 (por debajo del contratado). Clasificación: Alterno. Criterio cubierto: CA#4 / comentarios HU (actualizar km bajo contratado). |  |
|  | Abrir el servicio en modo edición. | Formulario carga con km excedido previo y observación visible en Órdenes. |
|  | Actualizar km a 90.000 en campo Contador, salir del campo. | No se muestra mensaje informativo de exceso (km no supera el umbral). |
|  | Dar clic en Guardar. | No se muestra alerta de exceso. Servicio actualizado correctamente. |
|  | Verificar observaciones en Órdenes asociadas al servicio. | Las observaciones en Órdenes permanecen intactas: mismo contenido, mismo conteo. No se eliminó ni modificó ninguna observación. |
| TC-07-HU-156544 - Sistema NO debe mostrar alerta cuando km es exactamente km_contratado + 500 en CI (umbral exacto sin exceso) | Precondiciones: Usuario autenticado en CI. Vehículo con km_contratado = 100.000 km. Tipo = Normal. Datos de prueba: km a ingresar = 100.500 (= km_contratado + 500; condición > es estricta, 100.500 > 100.500 es falso → sin exceso). Clasificación: Negativo. Criterio cubierto: CA#1 (frontera sin exceso en CI). |  |
|  | Crear servicio en CI. Ingresar km = 100.500 en campo Contador y salir del campo. | No se muestra mensaje informativo. El sistema no detecta exceso. |
|  | Completar formulario y dar clic en Guardar. | No se muestra alerta de exceso. Servicio creado normalmente. |
|  | Verificar Proveedor+ y Órdenes. | No se registra nota en Proveedor+ ni observación en Órdenes por exceso de km. |
| TC-08-HU-156544 - Sistema NO debe mostrar alerta cuando km es exactamente igual al km contratado en CI Express (umbral exacto sin exceso) | Precondiciones: Usuario autenticado en CI Express. Vehículo con km_contratado = 80.000 km. Tipo = Normal. Datos de prueba: km a ingresar = 80.000 (= km_contratado; 80.000 > 80.000 es falso → sin exceso). Clasificación: Negativo. Criterio cubierto: CA#1 (frontera sin exceso en CI Express). |  |
|  | Crear servicio en CI Express. Ingresar km = 80.000 en Contador y salir del campo. | No se muestra mensaje informativo. |
|  | Guardar el servicio. | No se muestra alerta. Servicio creado normalmente. |
|  | Verificar Proveedor+ y Órdenes. | No se registra nota ni observación por exceso de km. |
| TC-09-HU-156544 - Sistema NO debe evaluar exceso ni mostrar alerta cuando tipo de servicio es Siniestro, aunque km sea claramente excedido | Precondiciones: Usuario autenticado en CI. Vehículo con km_contratado = 100.000 km. Tipo servicio = Siniestro. Datos de prueba: km a ingresar = 150.000 (claramente superior al umbral de exceso). Clasificación: Negativo. Criterio cubierto: NOTA de HU (exclusión tipo Siniestro). |  |
|  | Crear servicio tipo Siniestro en CI. Ingresar km = 150.000 en Contador y salir del campo. | No se muestra mensaje informativo de exceso en campo Contador. |
|  | Guardar el servicio. | No se muestra alerta de exceso. Servicio creado normalmente. |
|  | Verificar Proveedor+ y Órdenes. | No se registra nota de exceso en Proveedor+ ni observación en Órdenes. |
| TC-10-HU-156544 - Sistema NO debe evaluar exceso ni mostrar alerta cuando tipo de servicio es Siniestro Póliza, aunque km sea claramente excedido | Precondiciones: Usuario autenticado en CI. Vehículo con km_contratado = 100.000 km. Tipo servicio = Siniestro Póliza. Datos de prueba: km = 150.000. Clasificación: Negativo. Criterio cubierto: NOTA de HU (exclusión tipo Siniestro Póliza). |  |
|  | Crear servicio tipo Siniestro Póliza en CI. Ingresar km = 150.000 en Contador y salir del campo. | No se muestra mensaje informativo de exceso. |
|  | Guardar el servicio. | No se muestra alerta. Servicio creado normalmente. |
|  | Verificar Proveedor+ y Órdenes. | No se registra nota ni observación por exceso de km. |
| TC-11-HU-156544 - Sistema debe mostrar mensaje en Contador y alerta al Guardar cuando km es km_contratado + 501 en CI (primer valor con exceso) | Precondiciones: Usuario autenticado en CI. Vehículo con km_contratado = 100.000 km. Tipo = Normal. Sin nota previa. Datos de prueba: km = 100.501 (primer valor que satisface 100.501 > 100.500 → exceso activo). Clasificación: Borde. Criterio cubierto: CA#1, CA#2 (límite inferior de exceso en CI). |  |
|  | Crear servicio en CI. Ingresar km = 100.501 en Contador y salir del campo. | El sistema detecta exceso y muestra mensaje informativo: "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." |
|  | Guardar el servicio. | Alerta de exceso mostrada. Servicio creado. Nota registrada en Proveedor+ y observación en Órdenes. |
| TC-12-HU-156544 - Sistema debe mostrar mensaje en Contador y alerta al Guardar cuando km es km_contratado + 1 en CI Express (primer valor con exceso) | Precondiciones: Usuario autenticado en CI Express. Vehículo con km_contratado = 80.000 km. Tipo = Normal. Sin nota previa. Datos de prueba: km = 80.001 (primer valor que satisface 80.001 > 80.000 → exceso activo). Clasificación: Borde. Criterio cubierto: CA#1, CA#2 (límite inferior de exceso en CI Express). |  |
|  | Crear servicio en CI Express. Ingresar km = 80.001 en Contador y salir del campo. | El sistema detecta exceso y muestra mensaje informativo: "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." |
|  | Guardar el servicio. | Alerta de exceso mostrada. Servicio creado. Nota registrada en Proveedor+ y observación en Órdenes. |
| TC-13-HU-156544 - Sistema debe agregar nota de actualización de lectura en Proveedor+ y conservar nota previa de exceso cuando km se actualiza por debajo del km contratado | Precondiciones: Usuario autenticado en CI. Servicio existente con km excedido (nota de exceso registrada en Proveedor+, observación de exceso en Órdenes). km_contratado = 100.000. Datos de prueba: conteo de notas en Proveedor+ = N (≥ 1 nota de exceso). km a actualizar = 90.000 (por debajo del contratado). Clasificación: Alterno. Criterio cubierto: PO#1 + PO#2 — comportamiento Proveedor+ al actualizar km bajo contratado; nota de corrección solo en Proveedor+. |  |
|  | Abrir el servicio en modo edición e ingresar km = 90.000 en campo Contador, salir del campo. | No se muestra mensaje informativo de exceso (km no supera el umbral). |
|  | Dar clic en Guardar. | No se muestra alerta de exceso. Servicio actualizado correctamente. |
|  | Verificar módulo Notas de Proveedor+ para el vehículo/servicio. | Se registra nueva nota con texto: "Se actualiza lectura de kilometraje; tras validación de consistencia, se autoriza continuidad de la cotización." El conteo de notas pasa de N a N+1. La nota de exceso previa permanece intacta. |
|  | Verificar observaciones en Órdenes asociadas al servicio. | No se agrega ni modifica ninguna observación en Órdenes. Conteo y contenido permanecen iguales al estado previo. |

---

## 5. Matriz de trazabilidad final

| Criterio de aceptación | Casos que lo cubren | Estado de cobertura |
| --- | --- | --- |
| CA#1 — Alerta de exceso al crear servicio (CI y CI Express) | TC-01, TC-02, TC-07, TC-08, TC-11, TC-12 | Completo |
| CA#2 — Momento de la alerta: campo Contador + Guardar | TC-01, TC-02, TC-03, TC-11, TC-12 | Completo |
| CA#3 — Registro automático de nota en Proveedor+ (tipo proveedor) | TC-01, TC-02, TC-03, TC-04, TC-05 | Completo |
| CA#4 — Registro automático de observación en Órdenes (solo al crear) | TC-01, TC-02, TC-03, TC-04, TC-06 | Completo |
| CA#5 — No duplicidad de notas por el mismo servicio/vehículo | TC-04, TC-05 | Completo |
| CA#6 — Comportamiento al actualizar servicio (comentarios HU) | TC-03, TC-05, TC-06 | Completo |
| NOTA — Exclusión tipo Siniestro / Siniestro Póliza | TC-09, TC-10 | Completo |
| PO — Nota corrección Proveedor+ al actualizar km bajo contratado (nota previa permanece + nueva nota corrección; Órdenes sin cambios) | TC-06, TC-13 | Completo |

---

## 6. Cobertura pendiente

- ninguno — ambos pendientes del PO fueron resueltos el 2026-07-01. TC-13 cubre el escenario confirmado.

---
## 🔗 Hand-off
- **Artefacto**: `resultado/HU-156544/04-casos-prueba-HU-156544.md`
- **Producido por**: qa-test-design
- **Estado**: Completado
- **Pendientes de validación**: ninguno
- **Siguiente agente sugerido**: @qa-ado-registration
- **Notas para el siguiente agente**: 13 casos diseñados (3 HP · 4 Alt · 4 Neg · 2 Borde). Prioridad Alta. Apps: CI y CI Express. TC-13 agregado tras confirmación del PO (2026-07-01). Los TC-01 y TC-02 son los más críticos para regresión.
---

## 🔗 Conexiones
- Siguiente artefacto: [[05-registro-ado.template]]
