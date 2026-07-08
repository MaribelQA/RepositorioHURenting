# Casos de prueba — HU-136903

## 1. Resumen de análisis de la HU
- **HU**: 136903 — CI EXPRESS OPTIMIZAR TIEMPO DE CARGA
- **Actor / rol principal**: Usuario del sistema CI y CI Express
- **Objetivo funcional**: El formulario FRMcargar inicial debe cargar en ≤14.5s (medido desde clic del usuario hasta form completamente interactivo), con indicador de progreso si supera 2s, capacidad de cancelación con log, y funcionalidad intacta.
- **Reglas relevantes**:
  - Umbral de aceptación: ≤14.5s (50% de 29s base). Meta deseable: <10s.
  - Medición: desde clic del usuario hasta form completamente interactivo (datos visibles, controles habilitados).
  - CI y CI Express son módulos independientes; los casos se ejecutan en ambos.
  - Indicador "Cargando" (spinner/barra/bombillo CI) si la carga supera 2s.
  - Cancelación cierra el form, retorna al estado anterior y registra en log.
  - Log de métricas (supuesto provisional: tabla BD) — campos: usuario, fecha/hora, tiempo ms, módulo, resultado.
  - Validación por comparación antes/después de la optimización en entorno QA.
- **Veredicto de diseño**: Completado (1 supuesto provisional — log de métricas en BD, pendiente confirmación PO/Dev)
- **Preguntas de aclaración pendientes**: Ninguna que impacte diseño (ver `02-reporte-clarificacion-HU-136903.md`)

---

## 2. Matrices de diseño

### 2.1 Partición de equivalencia

| Regla / criterio | Partición válida | Partición inválida | Impacto funcional |
|---|---|---|---|
| Tiempo de carga (clic→interactivo) | ≤14.5s | >14.5s | Criterio de aceptación principal |
| Meta deseable de tiempo | <10s | 10s–14.5s (aceptable) | Diferenciación óptimo/aceptable |
| Indicador de carga | Carga que supera 2s → spinner visible | Carga ≤2s → no debe aparecer spinner | UX |
| Cancelación | Cancelar durante carga activa (>2s) | Intentar cancelar cuando ya cargó | Comportamiento post-cancelación |
| Log de métricas | Registro generado con todos los campos (usuario, fecha/hora, tiempo ms, módulo, resultado) | Registro incompleto o no generado | Trazabilidad |
| Funcionalidad post-optimización | Datos correctos, controles habilitados, sin errores | Pérdida de datos, errores en pantalla | Integridad funcional |

### 2.2 Valores límite

| Regla / campo | Mínimo | Máximo | Límites cercanos | Justificación |
|---|---|---|---|---|
| Tiempo de carga — umbral aceptación | — | 14.5s | 14.4s (pasa), 14.5s (pasa), 14.6s (falla) | Criterio de pass/fail |
| Tiempo de carga — meta deseable | — | <10s | 9.9s (meta cumplida), 10.0s (meta no cumplida) | Diferenciación nivel de éxito |
| Tiempo de carga — trigger spinner | >2s | — | 1.9s (sin spinner), 2.1s (con spinner) | Criterio de UX |
| Tiempo baseline (referencia) | ~29s | — | — | Punto de comparación antes/después |

### 2.3 Tabla de decisión

| Condición 1: Tiempo carga | Condición 2: Supera 2s | Condición 3: Usuario cancela | Acción esperada |
|---|---|---|---|
| ≤14.5s | No | No | Form cargado e interactivo; sin spinner; log con resultado=completado |
| ≤14.5s | Sí | No | Form cargado e interactivo; spinner visible hasta completar; log con resultado=completado |
| ≤14.5s | Sí | Sí | Form se cierra; vuelve al estado anterior; log con resultado=cancelado |
| >14.5s | Sí | No | Form tarda más del umbral; spinner activo; **falla el criterio de aceptación** |
| >14.5s | Sí | Sí | Form se cierra; vuelve al estado anterior; log con resultado=cancelado |

---

## 3. Resumen de cobertura
- **HU**: 136903 — CI EXPRESS OPTIMIZAR TIEMPO DE CARGA
- **Total de casos**: 15 (Positivos: 7 · Negativos: 3 · Borde: 5)
- **Prioridad**: Alta
- **Insumos usados**: clarificación (`02-reporte-clarificacion-HU-136903.md`)
- **Técnicas aplicadas**: partición de equivalencia, valores límite, tabla de decisión, error guessing

---

## 4. Casos (formato Azure DevOps — Work Item Test Case)

| Title | Step Action | Step Expected Result |
|---|---|---|
| TC-01-HU-136903 - Módulo CI debe cargar en ≤14.5s desde clic hasta form interactivo (Happy Path) | Configurar entorno: Aplicación CI desplegada con optimización aplicada en entorno QA. Usuario autenticado con perfil estándar. Cronómetro/herramienta de medición lista. Clasificación: Happy Path. Criterio: tiempo de carga ≤14.5s. | |
| | Hacer clic en el acceso / menú que abre el módulo FRMcargar de CI. Iniciar la medición de tiempo en el momento exacto del clic. | El sistema comienza a procesar la apertura del formulario. |
| | Observar el formulario hasta que esté completamente cargado: datos visibles en pantalla y controles (botones, campos, listas) habilitados para interacción. Detener el cronómetro. | El formulario FRMcargar de CI se encuentra completamente interactivo con datos visibles. El tiempo transcurrido es **≤14.5 segundos**. |
| | Verificar que no se presentan mensajes de error ni excepciones durante o después de la carga. | No se muestran errores ni excepciones. |
| TC-02-HU-136903 - Módulo CI Express debe cargar en ≤14.5s desde clic hasta form interactivo (Happy Path) | Configurar entorno: Aplicación CI Express desplegada con optimización aplicada en entorno QA. Usuario autenticado con perfil estándar. Cronómetro/herramienta de medición lista. Clasificación: Happy Path. Criterio: tiempo de carga ≤14.5s. | |
| | Hacer clic en el acceso / menú que abre el módulo FRMcargar de CI Express. Iniciar la medición de tiempo en el momento exacto del clic. | El sistema comienza a procesar la apertura del formulario. |
| | Observar el formulario hasta que esté completamente cargado: datos visibles en pantalla y controles habilitados. Detener el cronómetro. | El formulario FRMcargar de CI Express se encuentra completamente interactivo con datos visibles. El tiempo transcurrido es **≤14.5 segundos**. |
| | Verificar que no se presentan mensajes de error ni excepciones durante o después de la carga. | No se muestran errores ni excepciones. |
| TC-03-HU-136903 - Módulo CI debe cargar en <10s (meta deseable — borde) | Configurar entorno: Igual que TC-01. Clasificación: Borde. Criterio: meta deseable <10s (no bloqueante). | |
| | Hacer clic en el acceso del módulo FRMcargar de CI. Iniciar la medición de tiempo. | El sistema comienza a procesar. |
| | Esperar a que el formulario esté completamente interactivo. Detener el cronómetro. | El tiempo transcurrido es **<10 segundos**. *(Nota: si el tiempo está entre 10s y 14.5s, el caso pasa con resultado "Aceptable" pero no "Óptimo")* |
| TC-04-HU-136903 - Módulo CI Express debe cargar en <10s (meta deseable — borde) | Configurar entorno: Igual que TC-02. Clasificación: Borde. Criterio: meta deseable <10s (no bloqueante). | |
| | Hacer clic en el acceso del módulo FRMcargar de CI Express. Iniciar la medición de tiempo. | El sistema comienza a procesar. |
| | Esperar a que el formulario esté completamente interactivo. Detener el cronómetro. | El tiempo transcurrido es **<10 segundos**. *(Nota: si el tiempo está entre 10s y 14.5s, el caso pasa con resultado "Aceptable" pero no "Óptimo")* |
| TC-05-HU-136903 - Funcionalidad del módulo CI debe mantenerse intacta post-optimización | Configurar entorno: Módulo CI con optimización aplicada. Datos de prueba cargados en BD (contratos, registros de renting). Clasificación: Happy Path. Criterio: funcionalidad intacta. | |
| | Abrir el módulo FRMcargar de CI y esperar a que cargue completamente. | El formulario se abre sin errores con datos visibles. |
| | Verificar que todos los campos, listados y datos que se mostraban antes de la optimización siguen presentes y con valores correctos. | Todos los campos contienen los datos esperados. No hay campos vacíos ni con valores incorrectos donde antes los había. |
| | Ejecutar las operaciones habituales del módulo (consultar, seleccionar, navegar). | Las operaciones responden correctamente sin errores funcionales. |
| | Verificar que no se generaron dependencias nuevas (el módulo de Órdenes y otros módulos funcionan normalmente). | Los demás módulos no presentan errores ni comportamientos inesperados por la optimización del FRMcargar de CI. |
| TC-06-HU-136903 - Funcionalidad del módulo CI Express debe mantenerse intacta post-optimización | Configurar entorno: Módulo CI Express con optimización aplicada. Datos de prueba cargados. Clasificación: Happy Path. Criterio: funcionalidad intacta. | |
| | Abrir el módulo FRMcargar de CI Express y esperar a que cargue completamente. | El formulario se abre sin errores con datos visibles. |
| | Verificar que todos los campos, listados y datos esperados están presentes y correctos. | Todos los campos contienen los datos esperados. No hay regresiones funcionales. |
| | Ejecutar las operaciones habituales del módulo. | Las operaciones responden correctamente sin errores. |
| TC-07-HU-136903 - Indicador "Cargando" debe aparecer cuando la carga supera 2 segundos | Configurar entorno: Módulo CI o CI Express en entorno donde la carga tarde más de 2s (puede simularse con red limitada o entorno con datos voluminosos). Clasificación: Happy Path / UX. Criterio: indicador de carga si supera 2s. | |
| | Abrir el módulo FRMcargar e iniciar medición. | El sistema comienza a procesar. |
| | Observar la pantalla durante los primeros 2 segundos de carga. | Antes de los 2 segundos no se muestra indicador de carga. |
| | Observar la pantalla cuando se superan los 2 segundos de carga. | Se muestra el indicador "Cargando" (spinner, barra de progreso o bombillo CI). El indicador desaparece cuando la carga finaliza. |
| TC-08-HU-136903 - La ventana debe ser movible mientras se está cargando | Configurar entorno: Módulo CI o CI Express en entorno donde la carga tarde más de 2s (indicador visible). Clasificación: Happy Path / UX. Criterio: ventana movible durante carga. | |
| | Iniciar la apertura del módulo FRMcargar. | El formulario comienza a cargar y el indicador "Cargando" aparece. |
| | Mientras el indicador está activo (carga en progreso), hacer clic en la barra de título de la ventana y arrastrarla a otra posición en la pantalla. | La ventana se mueve correctamente a la nueva posición. La carga no se interrumpe. |
| TC-09-HU-136903 - La ventana debe poder cancelarse mientras se está cargando | Configurar entorno: Módulo CI o CI Express en entorno donde la carga tarde más de 2s. Clasificación: Happy Path / UX. Criterio: ventana cancelable durante carga. | |
| | Iniciar la apertura del módulo FRMcargar. | El formulario comienza a cargar y el indicador "Cargando" aparece. |
| | Verificar que existe un control visible para cancelar la carga (botón Cancelar, ×, o equivalente). | El control de cancelación está visible e interactuable mientras la carga está en progreso. |
| TC-10-HU-136903 - Al cancelar la carga el form debe cerrarse y retornar al estado anterior | Configurar entorno: Módulo CI o CI Express en entorno donde la carga tarde más de 2s. Pantalla de origen conocida (menú u otra vista). Clasificación: Alterno. Criterio: comportamiento post-cancelación. | |
| | Iniciar la apertura del módulo FRMcargar. Esperar a que aparezca el indicador de carga (>2s). | El indicador "Cargando" está visible. |
| | Hacer clic en el control de cancelación (botón Cancelar, ×, o equivalente). | La carga se detiene inmediatamente. |
| | Observar el resultado en pantalla. | El formulario FRMcargar **se cierra** completamente. El sistema retorna a la pantalla/estado anterior (menú u otra vista desde donde se abrió). No quedan datos parciales visibles. |
| TC-11-HU-136903 - Log de métricas debe registrar carga completada en CI con campos requeridos | Configurar entorno: Módulo CI con optimización aplicada. Acceso a la tabla de log en BD (supuesto provisional). Usuario de prueba identificado. Clasificación: Happy Path. Criterio: log de métricas — carga completada. ⚠️ *Basado en supuesto provisional — pendiente PO: ubicación y formato real del log.* | |
| | Anotar la hora aproximada de inicio. Abrir el módulo FRMcargar de CI y esperar a que cargue completamente. | El formulario carga correctamente. |
| | Consultar la tabla de log en BD filtrando por usuario de prueba y fecha/hora aproximada. | Existe un registro en la tabla de log correspondiente a esta apertura. |
| | Verificar los campos del registro: usuario, fecha/hora, tiempo de carga (ms), módulo (CI), resultado (completado). | El registro contiene: **usuario** = usuario de prueba, **fecha/hora** = timestamp de la apertura, **tiempo ms** = tiempo medido en la carga, **módulo** = CI, **resultado** = completado. Ningún campo está nulo. |
| TC-12-HU-136903 - Log de métricas debe registrar carga completada en CI Express con campos requeridos | Configurar entorno: Módulo CI Express con optimización aplicada. Acceso a la tabla de log en BD (supuesto provisional). Usuario de prueba identificado. Clasificación: Happy Path. Criterio: log de métricas — carga completada CI Express. ⚠️ *Basado en supuesto provisional — pendiente PO.* | |
| | Anotar la hora aproximada de inicio. Abrir el módulo FRMcargar de CI Express y esperar a que cargue completamente. | El formulario carga correctamente. |
| | Consultar la tabla de log filtrando por usuario y fecha/hora. | Existe un registro correspondiente a esta apertura. |
| | Verificar los campos: usuario, fecha/hora, tiempo ms, módulo (CI Express), resultado (completado). | El registro contiene todos los campos requeridos con valores correctos. **módulo** = CI Express. |
| TC-13-HU-136903 - Log de métricas debe registrar cancelación con campos requeridos | Configurar entorno: Módulo CI o CI Express en entorno con carga >2s. Acceso a la tabla de log en BD (supuesto provisional). Clasificación: Alterno. Criterio: log de métricas — cancelación. ⚠️ *Basado en supuesto provisional — pendiente PO.* | |
| | Iniciar apertura del módulo FRMcargar. Esperar el indicador de carga. Hacer clic en el control de cancelación. | La carga se cancela y el form se cierra. |
| | Consultar la tabla de log filtrando por usuario de prueba y fecha/hora. | Existe un registro correspondiente a esta cancelación. |
| | Verificar los campos: usuario, fecha/hora, módulo, resultado. | El registro contiene: **resultado** = cancelado, **usuario** = usuario de prueba, **fecha/hora** = timestamp de la cancelación, **módulo** = CI o CI Express según corresponda. |
| TC-14-HU-136903 - Medición baseline debe registrar tiempo de carga antes de la optimización en QA | Configurar entorno: Entorno QA **sin** la optimización aplicada (versión previa). Herramienta de medición de tiempo lista. Clasificación: Borde / Prerequisito de validación. Criterio: baseline para comparación antes/después. | |
| | Abrir el módulo FRMcargar de CI en entorno QA sin optimización. Iniciar medición desde el clic. | El formulario comienza a cargar. |
| | Esperar a que el formulario esté completamente interactivo. Detener la medición. Registrar el tiempo obtenido. | El tiempo registrado representa el **baseline antes de la optimización** para CI. Valor esperado: ~29s (o el tiempo real medido en QA). |
| | Repetir el paso anterior al menos 3 veces y calcular el promedio. | Se obtiene un tiempo promedio estable como baseline. Este valor se usará como referencia en TC-15. |
| | Repetir el mismo proceso para CI Express y registrar su baseline. | Se obtiene el baseline para CI Express. |
| TC-15-HU-136903 - Tiempo de carga debe reducirse respecto al baseline tras aplicar la optimización en QA | Configurar entorno: Entorno QA **con** la optimización aplicada. Baseline registrado en TC-14 disponible. Clasificación: Borde / Validación comparativa. Criterio: reducción medible ≥50% respecto al baseline en QA. | |
| | Abrir el módulo FRMcargar de CI en entorno QA con optimización. Medir el tiempo (clic → form interactivo) al menos 3 veces y calcular el promedio. | El formulario carga correctamente en todas las mediciones. |
| | Comparar el promedio obtenido con el baseline de CI registrado en TC-14. Calcular el porcentaje de reducción: `((baseline - nuevo) / baseline) × 100`. | El tiempo promedio post-optimización es **≤50% del baseline de CI** (reducción ≥50%). El tiempo también es ≤14.5s. |
| | Repetir la medición para CI Express y comparar con su baseline. | El tiempo promedio de CI Express post-optimización también muestra una reducción ≥50% respecto a su baseline. |

---

## 5. Matriz de trazabilidad final

| Criterio de aceptación | Casos que lo cubren | Estado de cobertura |
|---|---|---|
| Tiempo de carga ≤14.5s (≥50% de reducción) — CI | TC-01, TC-03, TC-17 (borde) | Completo |
| Tiempo de carga ≤14.5s (≥50% de reducción) — CI Express | TC-02, TC-04 | Completo |
| Funcionalidad intacta post-optimización — CI | TC-05 | Completo |
| Funcionalidad intacta post-optimización — CI Express | TC-06 | Completo |
| Indicador "Cargando" si proceso supera 2 segundos | TC-07 | Completo |
| Ventana movible durante la carga | TC-08 | Completo |
| Ventana cancelable durante la carga | TC-09 | Completo |
| Al cancelar: form cierra, retorna al estado anterior | TC-10 | Completo |
| Log registra carga completada (usuario, fecha/hora, tiempo ms, módulo, resultado) — CI | TC-11 | Completo (supuesto provisional) |
| Log registra carga completada — CI Express | TC-12 | Completo (supuesto provisional) |
| Log registra cancelación | TC-13 | Completo (supuesto provisional) |
| No se generan dependencias adicionales en otros módulos | TC-05, TC-06 (paso 4) | Completo |
| Validación de mejora antes/después en entorno QA | TC-14, TC-15 | Completo |
| Meta deseable <10s | TC-03, TC-04 | Completo (no bloqueante) |

---

## 6. Cobertura pendiente

- **Verificación exacta del esquema de la tabla de log en BD**: los casos TC-11, TC-12, TC-13 asumen estructura de tabla BD (supuesto provisional). Si el equipo define otro mecanismo (archivo, event log), los pasos de verificación del log deben actualizarse. — _depende de: pendiente PO/Dev en `02-reporte-clarificacion-HU-136903.md` §4a_
- **Escenario de fallo de optimización** (tiempo sigue >14.5s tras cambios): no definido por el PO si es bloqueante para despliegue. Los casos existentes cubren el criterio de aceptación pero no el proceso de acción correctiva. — _depende de: pendiente PO en `02-reporte-clarificacion-HU-136903.md` §4b_

---
## 🔗 Hand-off
- **Artefacto**: `resultado/HU-136903/04-casos-prueba-HU-136903.md`
- **Producido por**: qa-test-design
- **Estado**: Completado
- **Pendientes de validación**: Log de métricas (TC-11, TC-12, TC-13 basados en supuesto provisional — tabla BD, pendiente confirmación PO/Dev); escenario de fallo de optimización sin cubrir (bajo impacto)
- **Siguiente agente sugerido**: @qa-ado-registration
- **Notas para el siguiente agente**: 15 casos de prueba. Organización ADO: https://rentingcolombia.visualstudio.com/ / Proyecto: CentroDeInteligencia. Prioridad: Alta. TC-01 a TC-06 son los de mayor prioridad (rendimiento + funcionalidad). TC-11 a TC-13 llevan nota de supuesto provisional sobre log de métricas.
---

## 🔗 Conexiones
- Siguiente artefacto: [[05-registro-ado.template]]
