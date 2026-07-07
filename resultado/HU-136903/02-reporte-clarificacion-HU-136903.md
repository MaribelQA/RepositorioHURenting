# Reporte de Clarificación — HU-136903

## 1. Resumen del análisis
- **HU analizada**: 136903 — CI EXPRESS OPTIMIZAR TIEMPO DE CARGA
- **Veredicto**: Sin bloqueantes, se puede avanzar
- **Preguntas hechas / respondidas**: 5 / 5

## 2. Matriz de hallazgos

| # | Categoría | Hallazgo | Tipo | Severidad | Estado | Pregunta |
|---|---|---|---|---|---|---|
| 1 | Interacción y flujo (UX) | La HU indica que la ventana de carga debe ser "cancelable" pero no define qué ocurre al cancelar: ¿se cierra? ¿datos parciales? ¿volver atrás? | Omisión | **Alta** | ✅ Resuelto | P1 |
| 2 | Terminología y consistencia | El rol es "Usuario del sistema CI y CI Express". No queda claro si ambos comparten el mismo formulario `FRMcargar` o son módulos independientes. | Ambigüedad | **Alta** | ✅ Resuelto | P2 |
| 3 | Atributos de calidad (no funcionales) | Baseline de ~29s sin formalizar: entorno de medición, volumen de datos, condiciones de red. Sin baseline controlado, el criterio del 50% no es verificable objetivamente. | Omisión | **Media** | ✅ Resuelto | P3 |
| 4 | Dominio y datos | Se requiere "log de métricas (usuario, fecha y hora)" pero no se indica dónde se almacena: ¿tabla BD? ¿archivo? ¿event log? | Omisión | **Media** | ✅ Resuelto | P4 |
| 5 | Misc / Placeholders | Se referencia "Captura de pantalla 2026-05-05 095857.png" como evidencia del cuello de botella, pero no fue adjunta ni está disponible. | Omisión | **Media** | ✅ Resuelto | P5 |
| 6 | Señales de completitud | Varios ítems son **tareas de implementación** ("Analizar el código actual", "Optimizar consultas a BD", "Agregar logs") mezclados con criterios de aceptación. Dificulta distinguir qué se valida vs. qué se implementa. | Ambigüedad | **Media** | 🟡 Pendiente de validación | — |
| 7 | Casos borde y manejo de fallos | No se contemplan escenarios de fallo durante la carga: BD sin respuesta, timeout de conexión, datos incompletos. | Omisión | **Media** | 🟡 Pendiente de validación | — |
| 8 | Alcance funcional | "La funcionalidad del módulo debe mantenerse intacta" sin especificar cuáles funcionalidades o flujos deben probarse en regresión. | No testable | **Media** | 🟡 Pendiente de validación | — |
| 9 | Terminología | "bombillo CI": término entre paréntesis sin definición formal. ¿Es un componente visual estándar del sistema CI? | Ambigüedad | **Baja** | 🟡 Pendiente de validación | — |
| 10 | Misc / Placeholders | Dos secciones tituladas "Criterios de Aceptación". La primera parece ser contexto/problema y la segunda los criterios reales. Estructura confusa. | Inconsistencia | **Baja** | 🟡 Pendiente de validación | — |

## 3. Bitácora de aclaraciones
### Sesión 2026-07-07
- P1: ¿Qué ocurre cuando el usuario cancela la carga del formulario? → R: Se muestra un **diálogo de confirmación** ("¿Desea cancelar la carga?") y, si el usuario acepta, se cierra el formulario.
- P2: ¿CI y CI Express comparten el mismo formulario `FRMcargar` o son módulos distintos? → R: Son **formularios distintos con código similar**. La optimización debe aplicarse y probarse en ambos.
- P3: ¿En qué entorno y condiciones se midió el baseline de ~29 segundos? → R: Medido en **producción** con datos reales y carga normal de usuarios.
- P4: ¿Dónde se almacenan los logs de métricas de tiempo de carga? → R: En una **tabla en base de datos**.
- P5: ¿Está disponible la imagen referenciada del cuello de botella? → R: La imagen **ya no está disponible**; el equipo de desarrollo conoce el cuello de botella.

## 4. Pendientes de validación (no bloqueantes)
> No impiden avanzar; quedan registrados para validación futura.

- **Mezcla criterios/tareas**: Separar formalmente los criterios de aceptación verificables de las tareas de implementación técnica. — _responsable sugerido: PO_
- **Escenarios de fallo**: Definir comportamiento ante fallos durante la carga (BD no responde, timeout, datos incompletos). Se abordará en el diseño de casos de prueba (escenarios negativos). — _responsable sugerido: PO/QA_
- **Alcance de regresión**: Listar las funcionalidades específicas del módulo `FRMcargar` que deben mantenerse intactas. Se cubrirá en diseño de casos. — _responsable sugerido: PO/Dev_
- **"bombillo CI"**: Confirmar si es un componente visual estándar del sistema CI (ícono de bombillo/indicador de carga propio). — _responsable sugerido: Dev/UX_
- **Secciones duplicadas**: La HU tiene dos bloques "Criterios de Aceptación"; el primero es contexto del problema. Considerar reorganizar en la HU original. — _responsable sugerido: PO_

## 5. Bloqueantes (100% requeridos)

- ninguno

## 6. Sugerencias de criterios de aceptación para el PO _(opcional)_

- **SUGERENCIA — requiere validación del PO**: Al cancelar la carga (vía botón cancelar o X), el sistema debe mostrar un diálogo de confirmación. Si el usuario confirma, el formulario se cierra sin guardar datos parciales. Si rechaza, la carga continúa.
- **SUGERENCIA — requiere validación del PO**: La optimización y las pruebas de rendimiento deben ejecutarse tanto en el formulario `FRMcargar` de CI como en el de CI Express, dado que son módulos distintos con código similar.
- **SUGERENCIA — requiere validación del PO**: El baseline oficial para medir la mejora del 50% debe tomarse en el entorno de producción (donde se registraron los ~29s). Las pruebas en QA y desarrollo validan estabilidad, pero la métrica de aceptación se mide en producción.
- **SUGERENCIA — requiere validación del PO**: La tabla de logs de métricas debe registrar al menos: usuario, fecha/hora de inicio, fecha/hora de fin, tiempo total de carga (ms), módulo (CI o CI Express), entorno, y resultado (éxito/cancelado/error).

---
## 🔗 Hand-off
- **Artefacto**: `resultado/HU-136903/02-reporte-clarificacion-HU-136903.md`
- **Producido por**: qa-refinement
- **Estado**: Completado
- **Pendientes de validación**: 5 pendientes no bloqueantes (mezcla criterios/tareas, escenarios de fallo, alcance regresión, "bombillo CI", secciones duplicadas)
- **Siguiente agente sugerido**: @qa-gap-analysis (o @qa-test-design)
- **Notas para el siguiente agente**: CI y CI Express tienen formularios FRMcargar separados (probar ambos). Cancelación con diálogo de confirmación. Baseline en producción (~29s). Logs en tabla BD. Considerar escenarios negativos (fallo BD, timeout) en diseño de casos.
---

## 🔗 Conexiones
- Siguiente artefacto: [[03-reportes-gaps.template]]
