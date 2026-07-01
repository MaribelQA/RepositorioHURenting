# Casos de prueba — HU-145865

## 1. Resumen de análisis de la HU
- **HU**: 145865 — Alerta temprana de exceso de kilometraje al crear el servicio
- **Actor / rol principal**: Proveedor / Asesor de taller
- **Objetivo funcional**: Mostrar alerta informativa de exceso de km al momento de registrar el kilometraje durante la creación/actualización de un servicio (antes de la cotización), y registrar notas automáticas en Proveedor+ y en observación de órdenes para trazabilidad.
- **Aplicaciones impactadas**: CI y CI Express
- **Reglas de exceso por app**:
  - CI: `km_registrado > km_contratado + 500` (tolerancia fija de 500 km)
  - CI Express: `km_registrado > km_contratado` (sin tolerancia)
- **Exclusiones de alcance**: Servicios tipo Siniestro y Siniestro Póliza no evalúan exceso de km.
- **Registros de persistencia involucrados**:
  1. Nota tipo proveedor en módulo **Proveedor+**: texto "Se detecta exceso de km al crear/actualizar el servicio para el vehículo [Placa]. Notificación visible al proveedor."
  2. Observación en **órdenes** del servicio: mismo texto (solo al crear, NO al actualizar).
- **Texto alerta visual confirmado**: "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller."
- **Naturaleza de la alerta**: informativa; no restringe continuar con la creación del servicio.
- **Alcance adicional confirmado**: flujo de actualización de servicio (comentarios HU, validados en clarificación P4).
- **Veredicto de diseño**: Completado

## 2. Preguntas de aclaración pendientes
- CA#2 está truncado ("Con el cambio:" sin continuación). El momento del trigger se infiere del contexto como "al registrar el km durante la creación", no "antes de la cotización". — _Responsable: PO_
- Destino exacto de la nota de corrección de km por error (CA comentarios): ¿se registra en Proveedor+, en órdenes o en ambos? — _Responsable: PO/Desarrollo_

## 3. Matrices de diseño

### 3.1 Partición de equivalencia

| Regla / Criterio | Partición válida (dispara alerta) | Partición inválida (no dispara alerta) | Impacto funcional |
| --- | --- | --- | --- |
| Exceso km — CI | `km_registrado > km_contratado + 500` (ej. km_c=100.000, km_r=100.502) | `km_registrado ≤ km_contratado + 500` (ej. km_r=100.500) | Alerta visual + nota Proveedor+ + obs órdenes |
| Exceso km — CI Express | `km_registrado > km_contratado` (ej. km_c=80.000, km_r=80.001) | `km_registrado ≤ km_contratado` (ej. km_r=80.000) | Alerta visual + nota Proveedor+ + obs órdenes |
| Tipo de servicio | Normal (cualquier tipo salvo siniestro) | Siniestro / Siniestro Póliza | Siniestro excluye toda la lógica de exceso de km |
| Existencia de nota previa | Sin nota previa → se crea nota nueva | Con nota previa → NO se crea nota nueva (no duplicidad) | Controla duplicación en Proveedor+ y órdenes |
| Operación sobre el servicio | Actualizar servicio → solo nota Proveedor+ (obs órdenes NO se toca) | Crear servicio → nota Proveedor+ + obs órdenes | Determina qué registros se insertan |

### 3.2 Valores límite

| App / Campo | No exceso — límite superior sin alerta | Primer valor con exceso | Límite cercano inferior | Justificación |
| --- | --- | --- | --- | --- |
| CI: km_registrado vs (km_contratado + 500) | km_contratado + 500 (ej. 100.500 km) | km_contratado + 501 (ej. 100.501 km) | km_contratado + 499 | Validar frontera exacta de la tolerancia de 500 km |
| CI Express: km_registrado vs km_contratado | km_contratado (ej. 80.000 km) | km_contratado + 1 (ej. 80.001 km) | km_contratado - 1 | Sin tolerancia; cualquier km mayor dispara alerta |

### 3.3 Tabla de decisión

| App | Tipo servicio | km excede umbral | Nota preexistente | Operación | Alerta visual | Nota Proveedor+ | Obs. Órdenes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| CI / CI Express | Normal | Sí | No | Crear | Sí | Nueva nota | Nueva obs |
| CI / CI Express | Normal | Sí | Sí | Crear / Reabrir | Sí | No nueva nota | No nueva obs |
| CI / CI Express | Normal | No | — | Crear | No | No | No |
| CI / CI Express | Siniestro | — | — | Crear | No | No | No |
| CI / CI Express | Siniestro Póliza | — | — | Crear | No | No | No |
| CI / CI Express | Normal | Sí | No | Actualizar | Sí | Nueva nota | NO se toca |
| CI / CI Express | Normal | Sí | Sí | Actualizar | Sí | No nueva nota | NO se toca |
| CI / CI Express | Normal | Km baja ≤ umbral | Sí (obs existente) | Actualizar | No | No | NO se elimina obs |
| CI / CI Express | Normal | Error entrada km | — | Actualizar | Según exceso | Nota corrección | Pendiente PO |

## 4. Resumen de cobertura
- **HU**: 145865 — Alerta temprana de exceso de kilometraje al crear el servicio
- **Total de casos**: 12 (Happy Path: 4 · Alternos: 2 · Negativos: 4 · Borde: 2)
- **Insumos usados**: HU (`01-HU-145865.md`), Reporte de Clarificación (`02-reporte-clarificacion-HU-145865.md`)
- **Técnicas ISTQB aplicadas**: Partición de equivalencia, Valores límite, Tabla de decisión, Error guessing

## 5. Casos (formato Azure DevOps — Work Item Test Case)

> Cada `Step Action` ejecutable lleva su `Stept Expected Result`. La fila `@Precondiciones` puede tener el resultado vacío.

| Title | Step Action | Stept Expected Result |
| --- | --- | --- |
| TC-01-HU-145865 - Sistema debe mostrar alerta visual y registrar nota en Proveedor+ y observación en órdenes cuando km excede umbral CI al crear servicio | Clasificación: Happy Path. Criterio cubierto: CA#1, CA#2, CA#3, CA#4. Técnica ISTQB: Partición de equivalencia. Datos de prueba: App CI. Vehículo con placa válida, contrato activo, km_contratado = 100.000 km. Tipo servicio = Normal. Sin nota previa de exceso para este servicio/vehículo. | El sistema muestra alerta informativa, registra nota en Proveedor+ y observación en órdenes. La alerta no bloquea la creación. |
|  | @Precondiciones: Usuario con perfil proveedor/asesor de taller autenticado en CI. Vehículo con contrato activo, km_contratado = 100.000 km. Módulo de creación de servicios disponible. Tipo de servicio = Normal. No existe nota de exceso de km previa para este servicio/vehículo. |  |
|  | Paso 1: Navegar al módulo de creación de servicios en CI. | El formulario de creación de servicio se carga correctamente. |
|  | Paso 2: Ingresar la placa del vehículo de prueba. | El sistema recupera los datos del vehículo y muestra km_contratado = 100.000 km. |
|  | Paso 3: Ingresar km = 100.502 en el campo de kilometraje (100.502 > km_contratado + 500 = 100.500). | El sistema detecta exceso inmediatamente. Se muestra alerta visual con mensaje: "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." |
|  | Paso 4: Intentar continuar con la creación del servicio sin descartar la operación. | La alerta es informativa. El sistema permite continuar. No genera error ni bloqueo que impida avanzar. |
|  | Paso 5: Completar los demás campos requeridos del servicio y guardar. | El servicio es creado exitosamente. |
|  | Paso 6: Navegar al módulo Proveedor+ y verificar las notas del proveedor para el servicio creado. | Existe exactamente una nota de tipo proveedor con el texto: "Se detecta exceso de km al crear/actualizar el servicio para el vehículo [Placa]. Notificación visible al proveedor." (verificación de persistencia: INSERT en Proveedor+). |
|  | Paso 7: Navegar a las órdenes asociadas al servicio y revisar el campo de observaciones. | Existe la observación con el texto: "Se detecta exceso de km al crear/actualizar el servicio para el vehículo [Placa]. Notificación visible al proveedor." (verificación de persistencia: INSERT en observación de órdenes). |
| TC-02-HU-145865 - Sistema debe mostrar alerta visual y registrar nota en Proveedor+ y observación en órdenes cuando km excede umbral CI Express al crear servicio | Clasificación: Happy Path. Criterio cubierto: CA#1, CA#2, CA#3, CA#4. Técnica ISTQB: Partición de equivalencia. Datos de prueba: App CI Express. Vehículo con placa válida, contrato activo, km_contratado = 80.000 km. Tipo servicio = Normal. Sin nota previa de exceso. | El sistema muestra alerta informativa, registra nota en Proveedor+ y observación en órdenes. La alerta no bloquea la creación. |
|  | @Precondiciones: Usuario con perfil proveedor/asesor de taller autenticado en CI Express. Vehículo con contrato activo, km_contratado = 80.000 km. Tipo servicio = Normal. No existe nota de exceso previa para este servicio/vehículo. |  |
|  | Paso 1: Navegar al módulo de creación de servicios en CI Express. | El formulario de creación de servicio se carga correctamente. |
|  | Paso 2: Ingresar la placa del vehículo de prueba. | El sistema recupera los datos del vehículo y muestra km_contratado = 80.000 km. |
|  | Paso 3: Ingresar km = 80.500 (80.500 > 80.000, umbral CI Express = km_contratado sin tolerancia). | El sistema detecta exceso inmediatamente. Se muestra alerta: "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." |
|  | Paso 4: Verificar que la alerta no bloquea el flujo. Continuar con la creación. | El proveedor puede continuar con la creación del servicio sin restricción. |
|  | Paso 5: Completar los demás campos y guardar el servicio. | Servicio creado exitosamente. |
|  | Paso 6: Verificar la nota en Proveedor+ para el servicio creado. | Existe nota tipo proveedor: "Se detecta exceso de km al crear/actualizar el servicio para el vehículo [Placa]. Notificación visible al proveedor." |
|  | Paso 7: Verificar la observación en las órdenes del servicio. | Existe observación: "Se detecta exceso de km al crear/actualizar el servicio para el vehículo [Placa]. Notificación visible al proveedor." |
| TC-03-HU-145865 - Sistema NO debe mostrar alerta cuando km es exactamente igual al umbral máximo sin exceso en CI (km_contratado + 500) | Clasificación: Negativo. Criterio cubierto: CA#1 (condición de frontera — sin exceso). Técnica ISTQB: Valores límite. Datos de prueba: App CI. km_contratado = 100.000. km_registrado = 100.500 (= km_contratado + 500, no supera el umbral). | El sistema no muestra alerta de exceso. No se registra nota en Proveedor+ ni observación en órdenes. |
|  | @Precondiciones: App CI. Vehículo con contrato activo, km_contratado = 100.000 km. Tipo servicio = Normal. |  |
|  | Paso 1: Navegar a creación de servicios en CI. Ingresar la placa del vehículo. | Datos del vehículo recuperados, km_contratado = 100.000 km. |
|  | Paso 2: Ingresar km = 100.500 (exactamente km_contratado + 500; no supera el umbral). | El sistema no detecta exceso de km. No se muestra ninguna alerta. El flujo continúa normalmente. |
|  | Paso 3: Completar y guardar el servicio. | Servicio creado exitosamente sin alerta de exceso. |
|  | Paso 4: Verificar Proveedor+ para el servicio creado. | No existe ninguna nota de exceso de km para este servicio. |
|  | Paso 5: Verificar observaciones en órdenes del servicio. | No existe ninguna observación de exceso de km. |
| TC-04-HU-145865 - Sistema NO debe mostrar alerta cuando km es exactamente igual al km contratado en CI Express | Clasificación: Negativo. Criterio cubierto: CA#1 (condición de frontera — sin exceso). Técnica ISTQB: Valores límite. Datos de prueba: App CI Express. km_contratado = 80.000. km_registrado = 80.000 (= km_contratado, no lo supera). | El sistema no muestra alerta de exceso. No se registran notas ni observaciones. |
|  | @Precondiciones: App CI Express. Vehículo con contrato activo, km_contratado = 80.000 km. Tipo servicio = Normal. |  |
|  | Paso 1: Navegar a creación de servicios en CI Express. Ingresar la placa del vehículo. | Datos del vehículo recuperados, km_contratado = 80.000 km. |
|  | Paso 2: Ingresar km = 80.000 (igual al km_contratado, no lo supera). | El sistema no detecta exceso. No se muestra alerta. |
|  | Paso 3: Completar y guardar el servicio. | Servicio creado exitosamente sin alerta. |
|  | Paso 4: Verificar Proveedor+ y observaciones en órdenes. | No existe nota ni observación de exceso de km para este servicio. |
| TC-05-HU-145865 - Sistema NO debe mostrar alerta ni registrar nota cuando el tipo de servicio es Siniestro | Clasificación: Negativo. Criterio cubierto: NOTA de HU (exclusión por tipo Siniestro). Técnica ISTQB: Tabla de decisión. Datos de prueba: App CI. km_contratado = 100.000. km_registrado = 150.000 (claramente excesivo). Tipo servicio = Siniestro. | El sistema no evalúa exceso de km. No se muestra alerta. No se registra nota en Proveedor+ ni observación en órdenes. |
|  | @Precondiciones: App CI. Vehículo con contrato activo, km_contratado = 100.000 km. Tipo de servicio = Siniestro. |  |
|  | Paso 1: Navegar a creación de servicios en CI. Seleccionar tipo = Siniestro. | Formulario de creación disponible con tipo Siniestro seleccionado. |
|  | Paso 2: Ingresar la placa del vehículo. | Datos del vehículo recuperados. |
|  | Paso 3: Ingresar km = 150.000 (muy superior a km_contratado + 500 = 100.500). | El sistema no evalúa exceso de km para tipo Siniestro. No se muestra ninguna alerta de km. |
|  | Paso 4: Completar y guardar el servicio. | Servicio de tipo Siniestro creado exitosamente, sin alerta ni mensaje de exceso. |
|  | Paso 5: Verificar Proveedor+ para el servicio. | No existe nota de exceso de km para este servicio. |
|  | Paso 6: Verificar observaciones en las órdenes del servicio. | No existe observación de exceso de km. |
| TC-06-HU-145865 - Sistema NO debe mostrar alerta ni registrar nota cuando el tipo de servicio es Siniestro Póliza | Clasificación: Negativo. Criterio cubierto: NOTA de HU (exclusión por tipo Siniestro Póliza). Técnica ISTQB: Tabla de decisión. Datos de prueba: App CI. km_registrado claramente excesivo. Tipo servicio = Siniestro Póliza. | El sistema no evalúa exceso de km. No se muestra alerta. No se registra nota ni observación. |
|  | @Precondiciones: App CI. Vehículo con contrato activo. Tipo de servicio = Siniestro Póliza. |  |
|  | Paso 1: Navegar a creación de servicios en CI. Seleccionar tipo = Siniestro Póliza. | Formulario con tipo Siniestro Póliza disponible. |
|  | Paso 2: Ingresar placa del vehículo y km claramente excesivo (ej. 150.000). | El sistema no evalúa exceso para tipo Siniestro Póliza. No se muestra alerta. |
|  | Paso 3: Completar y guardar el servicio. | Servicio Siniestro Póliza creado exitosamente sin alerta. |
|  | Paso 4: Verificar Proveedor+ y observaciones en órdenes. | No existen nota ni observación de exceso de km para este servicio. |
| TC-07-HU-145865 - Sistema debe mostrar alerta visual pero NO debe crear nota duplicada al reabrir o editar un servicio con nota de exceso ya registrada | Clasificación: Alterno. Criterio cubierto: CA#5 (no duplicidad). Técnica ISTQB: Tabla de decisión / Error guessing. Datos de prueba: Servicio existente con km excedido, con nota tipo proveedor en Proveedor+ y observación en órdenes ya registradas. | La alerta se muestra al reabrir. No se crea nota nueva en Proveedor+. No se crea observación nueva en órdenes. |
|  | @Precondiciones: App CI o CI Express. Existe un servicio con km excedido. Ya existe exactamente 1 nota tipo proveedor en Proveedor+ y 1 observación en órdenes para este servicio (resultado esperado de TC-01 o TC-02). |  |
|  | Paso 1: Abrir/reabrir el servicio existente que ya tiene nota de exceso registrada. | El sistema carga el servicio en modo de edición o visualización. |
|  | Paso 2: Verificar que se muestra la alerta de exceso de km de inmediato. | La alerta visual "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." se muestra al reabrir/editar el servicio. |
|  | Paso 3: Registrar el número exacto de notas de exceso en Proveedor+ para este servicio antes de cualquier acción adicional. | Existe exactamente 1 nota de exceso en Proveedor+. |
|  | Paso 4: Navegar o guardar el servicio sin modificar el km. | El servicio se cierra o guarda sin cambios en km. |
|  | Paso 5: Verificar las notas en Proveedor+ después de la operación. | Sigue existiendo exactamente 1 nota de exceso en Proveedor+. No se generó nota duplicada. |
|  | Paso 6: Verificar las observaciones en las órdenes del servicio. | Las observaciones permanecen igual (1 observación de exceso). No se generó una nueva observación duplicada. |
| TC-08-HU-145865 - Sistema debe mostrar alerta y registrar nota en Proveedor+ pero NO debe crear ni modificar observación en órdenes al actualizar un servicio con km excedido | Clasificación: Happy Path. Criterio cubierto: CA comentarios — actualizar servicio (alcance confirmado). Técnica ISTQB: Partición de equivalencia / Tabla de decisión. Datos de prueba: App CI. Servicio existente, km_contratado = 100.000 km. Sin nota previa de exceso. km actualizado = 101.000 (excede umbral). | Alerta visual mostrada. Nota creada en Proveedor+. Observación en órdenes NO creada ni modificada. |
|  | @Precondiciones: App CI. Existe un servicio con km guardado ≤ 100.500 (sin exceso previo). Sin nota de exceso en Proveedor+ ni en órdenes. km_contratado = 100.000 km. |  |
|  | Paso 1: Abrir el servicio existente y navegar a la edición del campo km. | Formulario de edición disponible con km actual ≤ 100.500. |
|  | Paso 2: Actualizar el km a 101.000 (101.000 > 100.000 + 500 = 100.500). | El sistema detecta exceso al procesar el nuevo km. Muestra alerta: "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." |
|  | Paso 3: Verificar que la alerta no bloquea el guardado. Guardar el servicio. | El servicio se guarda exitosamente con el km actualizado. |
|  | Paso 4: Verificar las notas en Proveedor+ para el servicio actualizado. | Existe una nota tipo proveedor con texto: "Se detecta exceso de km al crear/actualizar el servicio para el vehículo [Placa]. Notificación visible al proveedor." (INSERT en Proveedor+). |
|  | Paso 5: Verificar las observaciones en las órdenes asociadas al servicio. | Las observaciones en órdenes NO fueron modificadas ni se agregó una nueva observación de exceso. El campo de observaciones queda idéntico al estado previo a la actualización. |
| TC-09-HU-145865 - Sistema NO debe modificar ni eliminar la observación de las órdenes al actualizar el km del servicio por debajo del km contratado | Clasificación: Borde. Criterio cubierto: CA comentarios — km por debajo de contratado no elimina obs en órdenes. Técnica ISTQB: Valores límite / Error guessing. Datos de prueba: Servicio existente con km excedido y observación registrada en órdenes. km actualizado por debajo del km_contratado. | Las observaciones en órdenes permanecen intactas después de la actualización. |
|  | @Precondiciones: App CI o CI Express. Existe un servicio con km excedido. Existe observación de exceso registrada en las órdenes del servicio (consecuencia de TC-01 o TC-02). Registrar contenido exacto de la observación antes de ejecutar. |  |
|  | Paso 1: Abrir el servicio con exceso registrado y verificar la observación actual en sus órdenes. | La observación de exceso en órdenes es visible y tiene el texto esperado. Registrar el contenido exacto. |
|  | Paso 2: Editar el km del servicio a un valor por debajo del km_contratado (ej. si km_contratado = 100.000, ingresar km = 90.000). | El sistema acepta el nuevo km. No se muestra alerta de exceso porque no hay exceso. |
|  | Paso 3: Guardar el servicio con el km actualizado. | Servicio guardado exitosamente con km = 90.000. |
|  | Paso 4: Verificar las observaciones en las órdenes del servicio. | La observación de exceso de km registrada previamente permanece sin cambios. No fue modificada ni eliminada. Su texto es idéntico al registrado en el Paso 1. |
|  | Paso 5: Verificar las notas en Proveedor+. | La nota de Proveedor+ también permanece sin cambios. No fue eliminada ni modificada. |
| TC-10-HU-145865 - Sistema debe registrar nota de corrección de lectura de km al editar un km ingresado por error | Clasificación: Alterno. Criterio cubierto: CA#5 comentarios (edición de km por error de digitación). Técnica ISTQB: Error guessing. Datos de prueba: Servicio existente con km guardado de forma incorrecta. El proveedor corrige el km. | Se registra nota: "Se actualiza lectura de kilometraje; tras validación de consistencia, se autoriza continuidad de la cotización." Nota: destino exacto (Proveedor+ y/u órdenes) pendiente de confirmación PO. |
|  | @Precondiciones: App CI o CI Express. Existe un servicio con un km guardado que el proveedor identifica como error de digitación. El servicio fue previamente creado y guardado. |  |
|  | Paso 1: Abrir el servicio con el km incorrecto y navegar a la edición del campo km. | El servicio se carga en modo de edición. El km incorrecto es visible. |
|  | Paso 2: Modificar el campo de km al valor correcto (diferente al guardado anteriormente). | El campo acepta el nuevo valor de km. |
|  | Paso 3: Guardar el servicio con el km corregido. | El servicio se guarda con el nuevo km. |
|  | Paso 4: Verificar las notas en Proveedor+ y/o las observaciones en órdenes del servicio. | Existe la nota: "Se actualiza lectura de kilometraje; tras validación de consistencia, se autoriza continuidad de la cotización." — Cobertura parcial: validar el destino exacto (Proveedor+ y/u órdenes) pendiente de confirmación del PO. |
| TC-11-HU-145865 - Sistema debe mostrar alerta cuando km es exactamente km_contratado + 501 en CI (primer valor con exceso) | Clasificación: Borde. Criterio cubierto: CA#1 (límite inferior de exceso en CI). Técnica ISTQB: Valores límite. Datos de prueba: App CI. km_contratado = 100.000. km_registrado = 100.501 (primer valor que supera el umbral km_contratado + 500). | El sistema detecta exceso y muestra la alerta. |
|  | @Precondiciones: App CI. Vehículo con contrato activo, km_contratado = 100.000 km. Tipo servicio = Normal. |  |
|  | Paso 1: Iniciar creación de servicio en CI. Ingresar la placa del vehículo. | Datos del vehículo recuperados, km_contratado = 100.000 km. |
|  | Paso 2: Ingresar km = 100.501 (100.501 > 100.000 + 500; es el primer valor que supera el umbral). | El sistema detecta exceso. Se muestra alerta visual de exceso de km. |
|  | Paso 3: Verificar el texto exacto de la alerta. | La alerta muestra: "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." |
| TC-12-HU-145865 - Sistema debe mostrar alerta cuando km es exactamente km_contratado + 1 en CI Express (primer valor con exceso) | Clasificación: Borde. Criterio cubierto: CA#1 (límite inferior de exceso en CI Express). Técnica ISTQB: Valores límite. Datos de prueba: App CI Express. km_contratado = 80.000. km_registrado = 80.001 (primer valor que supera el km contratado). | El sistema detecta exceso y muestra la alerta. |
|  | @Precondiciones: App CI Express. Vehículo con contrato activo, km_contratado = 80.000 km. Tipo servicio = Normal. |  |
|  | Paso 1: Iniciar creación de servicio en CI Express. Ingresar la placa del vehículo. | Datos del vehículo recuperados, km_contratado = 80.000 km. |
|  | Paso 2: Ingresar km = 80.001 (80.001 > 80.000; primer valor que supera el km contratado sin tolerancia). | El sistema detecta exceso. Se muestra alerta visual de exceso de km. |
|  | Paso 3: Verificar el texto exacto de la alerta. | La alerta muestra: "Contador supera el kilometraje máximo contratado. Los costos asociados los debe asumir el cliente directamente en taller." |

## 6. Matriz de trazabilidad final

| Criterio de aceptación | Casos que lo cubren | Estado de cobertura |
| --- | --- | --- |
| CA#1 — Alerta de exceso al crear servicio (mensaje visible de inmediato) | TC-01, TC-02, TC-11, TC-12 | Completo |
| CA#2 — Momento de la alerta: al registrar km, no al cotizar | TC-01, TC-02 | Completo (CA truncado; comportamiento inferido y cubierto en pasos) |
| CA#3 — Registro nota tipo proveedor en Proveedor+ | TC-01, TC-02, TC-07 (no duplicidad), TC-08 | Completo |
| CA#4 — Registro observación en órdenes del servicio | TC-01, TC-02, TC-07 (no duplicidad), TC-09 | Completo |
| CA#5 — No duplicidad de notas al reabrir/editar servicio | TC-07 | Completo |
| CA#5 comentarios — Alerta informativa, no restringe continuar | TC-01, TC-02, TC-08 | Completo |
| CA#5 comentarios — Edición de km por error: nota de corrección | TC-10 | Parcial (destino de la nota pendiente PO) |
| NOTA HU — Exclusión tipo Siniestro | TC-05 | Completo |
| NOTA HU — Exclusión tipo Siniestro Póliza | TC-06 | Completo |
| Comentarios HU — Actualizar servicio: alerta + nota Proveedor+ (obs órdenes no se toca) | TC-08 | Completo |
| Comentarios HU — Actualizar km bajo contratado: obs órdenes permanecen intactas | TC-09 | Completo |
| Frontera CI — sin exceso: km = km_contratado + 500 | TC-03 | Completo |
| Frontera CI Express — sin exceso: km = km_contratado | TC-04 | Completo |
| Frontera CI — primer exceso: km = km_contratado + 501 | TC-11 | Completo |
| Frontera CI Express — primer exceso: km = km_contratado + 1 | TC-12 | Completo |

## 7. Cobertura pendiente

- **TC-10 — Destino nota de corrección por error**: el campo de la HU indica el texto de la nota ("Se actualiza lectura de kilometraje; tras validación de consistencia...") pero no especifica si se registra en Proveedor+, en observación de órdenes o en ambos. El caso está diseñado pero la validación de persistencia queda parcial hasta confirmar con PO. — _depende de: pendiente CA#5 comentarios / PO_
- **Escenario placa sin contrato o servicio de km no disponible**: comportamiento del sistema cuando no se encuentra contrato activo para el vehículo o el servicio de consulta de km contratado no responde. No se cubrió para evitar supuestos sin validar. — _depende de: pendiente técnico hallazgo #9 / PO + Desarrollo_

---
## 🔗 Hand-off
- **Artefacto**: `resultado/HU-145865/04-casos-prueba-HU-145865.md`
- **Producido por**: qa-test-design
- **Estado**: Completado
- **Pendientes de validación**: Destino nota de corrección TC-10 (Proveedor+ y/u órdenes — pendiente PO); comportamiento ante placa sin contrato o servicio km no disponible (pendiente PO/Desarrollo).
- **Siguiente agente sugerido**: @qa-ado-registration
- **Notas para el siguiente agente**: 12 casos listos para registro en ADO. Dos apps con reglas distintas: CI (tolerancia +500 km) y CI Express (sin tolerancia). Verificar Test Plan/Suite de destino en `proyecto.config.md`. TC-10 tiene cobertura parcial; priorizar validación del PO sobre el destino de la nota antes de cerrar el registro.
---

## 🔗 Conexiones
- Siguiente artefacto: [[05-registro-ado-HU-145865]]
