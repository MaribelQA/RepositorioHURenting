# Casos de prueba — HU-136903

## 1. Resumen de análisis de la HU
- **HU**: 136903 — CI EXPRESS OPTIMIZAR TIEMPO DE CARGA
- **Actor / rol principal**: Usuario del sistema CI y CI Express
- **Objetivo funcional**: Reducir el tiempo de carga del formulario FRMcargar de carga inicial al menos un 50% respecto al baseline (~29s en producción), manteniendo la funcionalidad intacta, incorporando indicador visual de progreso, opción de cancelación con confirmación y registro de métricas en base de datos. La optimización aplica a dos módulos distintos: FRMcargar de CI y FRMcargar de CI Express.
- **Reglas relevantes**:
  - Baseline: ~29s medido en producción con datos reales y carga normal de usuarios.
  - Meta mínima: ≤ 14.5s (50% de reducción). Meta ideal: < 10s.
  - La optimización aplica por separado a FRMcargar de CI y FRMcargar de CI Express (módulos distintos con código similar).
  - NO trasladar la lógica de inicialización al módulo de órdenes.
  - Si el proceso supera 2 segundos: mostrar indicador visual de carga (spinner, barra de progreso o "bombillo CI") y permitir mover la ventana y cancelar.
  - Cancelación: diálogo de confirmación → si acepta → cierra el formulario; si rechaza → continúa la carga.
  - Logs de métricas (usuario, fecha/hora, tiempo de carga) almacenados en tabla de base de datos.
  - Sin dependencias adicionales que afecten otros módulos.
- **Veredicto de diseño**: Parcial
- **Preguntas de aclaración pendientes**: Ver sección 2.

## 2. Preguntas de aclaración pendientes

| # | Hallazgo | Impacto en casos de prueba | Responsable |
|---|---|---|---|
| #6 | Mezcla de criterios verificables con tareas de implementación | Los ítems "Analizar código", "Optimizar consultas a BD" y "Agregar logs" son tareas técnicas, no criterios observables. Solo se diseñan casos sobre resultados medibles. | PO |
| #7 | Comportamiento ante fallo de BD no definido en la HU | TC-10 cubre este escenario con supuesto razonable (mensaje de error, sin crash). Marcado como `Cobertura pendiente de validación`. | PO/QA |
| #8 | Alcance de regresión no especificado | TC-05 cubre un humo general. El equipo de desarrollo debe confirmar los flujos específicos de FRMcargar a regresar. Marcado como `Cobertura pendiente de validación`. | PO/Dev |
| #9 | "bombillo CI" sin definición formal | Tratado en los casos como indicador visual estándar (spinner o barra de progreso). Sin impacto bloqueante. | Dev/UX |

## 3. Matrices de diseño

### 3.1 Partición de equivalencia

| Regla / criterio | Partición válida | Partición inválida | Impacto funcional |
|---|---|---|---|
| CA-01: Tiempo de carga (meta mínima) | Tiempo total ≤ 14.5s | Tiempo total > 14.5s | Incumple criterio de aceptación de rendimiento |
| CA-01: Tiempo de carga (meta ideal) | Tiempo total < 10s | Tiempo total ≥ 10s | No alcanza meta óptima |
| CA-02: Funcionalidad intacta | Todos los flujos operan sin errores ni pérdida de datos | Un flujo presenta error o pérdida de dato | Regresión funcional |
| CA-03: Log en BD | Carga exitosa → INSERT en tabla BD | Carga exitosa sin registro en BD | Pérdida de trazabilidad de métricas |
| CA-04: Multi-entorno | Mejora verificada en dev, QA y producción | Mejora verificada solo en uno o dos entornos | Optimización no estable para promoción |
| CA-05: Sin dependencias adicionales | Otros módulos funcionan sin cambios | Otro módulo falla o cambia comportamiento | Regresión no planificada en otros módulos |
| CA-06: Umbral indicador visual | Carga > 2s → indicador visible | Carga ≤ 2s → sin indicador | UX incorrecto; criterio de presentación incumplido |
| CA-07: Ventana movible | Ventana puede arrastrarse durante la carga | Ventana bloqueada (no movible) | UX degradado |
| CA-08: Cancelación (confirmar) | Clic cancelar → diálogo → Aceptar → cierra formulario | Cierre sin diálogo previo | Flujo de cancelación incorrecto |
| CA-08: Cancelación (rechazar) | Clic cancelar → diálogo → Rechazar → carga continúa | Carga se detiene sin confirmación | Flujo de cancelación incorrecto |

### 3.2 Valores límite

| Regla / campo | Mínimo | Máximo | Límites cercanos | Justificación |
|---|---|---|---|---|
| Tiempo de carga (meta mínima CA-01) | 0s | 14.5s | 14.4s · 14.5s · 14.6s | 50% de reducción sobre baseline ~29s = 14.5s |
| Tiempo de carga (meta ideal CA-01) | 0s | 9.999s | 9.9s · 10.0s · 10.1s | Meta ideal explícita: < 10 segundos |
| Umbral indicador visual (CA-06) | 2.001s | Sin límite superior | 1.9s · 2.0s · 2.1s | El indicador aparece si el proceso **supera** 2s (no en 2.0s exacto) |

### 3.3 Tabla de decisión — Flujo de cancelación

| Carga activa (> 2s) | Usuario hace clic en cancelar | Usuario confirma diálogo | Acción esperada |
|---|---|---|---|
| Sí | Sí | Sí (Acepta) | Formulario se cierra. Sin datos parciales en pantalla. |
| Sí | Sí | No (Rechaza) | Diálogo cierra. Carga continúa hasta completarse. |
| Sí | No | — | Carga se completa normalmente sin intervención del usuario. |
| No (≤ 2s) | — | — | Carga directa, sin indicador ni opción de cancelar. |

## 4. Resumen de cobertura
- **HU**: 136903 — CI EXPRESS OPTIMIZAR TIEMPO DE CARGA
- **Total de casos**: 13 (Happy Path: 5 · Alternos: 4 · Negativos: 2 · Borde: 2)
- **Prioridad**: Alta
- **Insumos usados**: clarificación (`02`). Sin reporte de gaps (`03`).
- **Técnicas aplicadas**: Partición de equivalencia · Valores límite · Tabla de decisión · Error guessing (fallo de BD)

## 5. Casos (formato Azure DevOps — Work Item Test Case)

> **Estructura de la tabla:** `Title` aparece solo en la **primera fila** de cada caso. Las filas siguientes dejan `Title` vacío. La fila de precondición lleva `Step Action` (configurar entorno/datos) y `Step Expected Result` **vacío**. Solo los pasos ejecutables llevan resultado esperado.
>
> **CSV generado junto a este archivo:** `04-casos-prueba-HU-136903.csv` — separador `|`.

### Happy Path

| Title | Step Action | Step Expected Result |
|---|---|---|
| TC-01-HU-136903 - FRMcargar CI debe cargar en ≤ 14.5s cuando se ejecuta en producción | Configurar entorno: Acceso al módulo FRMcargar del sistema CI en producción. Usuario autenticado con perfil habitual. BD operativa con datos reales. Herramienta de medición de tiempo activa (cronómetro externo o log). Clasificación: Happy Path. Criterio: CA-01. |  |
|  | Registrar el tiempo exacto de inicio al ejecutar el módulo FRMcargar CI. | Se registra la marca de tiempo de inicio. |
|  | Ejecutar el módulo FRMcargar CI (apertura de carga inicial). | El formulario comienza a cargarse sin errores. |
|  | Registrar el tiempo exacto en que el formulario queda completamente disponible para interacción del usuario. | El tiempo total de carga (inicio a listo) es ≤ 14.5 segundos. |
| TC-02-HU-136903 - FRMcargar CI Express debe cargar en ≤ 14.5s cuando se ejecuta en producción | Configurar entorno: Acceso al módulo FRMcargar del sistema CI Express en producción. Usuario autenticado. BD operativa con datos reales. Herramienta de medición de tiempo activa. Clasificación: Happy Path. Criterio: CA-01. |  |
|  | Registrar el tiempo exacto de inicio al ejecutar el módulo FRMcargar CI Express. | Se registra la marca de tiempo de inicio. |
|  | Ejecutar el módulo FRMcargar CI Express (apertura de carga inicial). | El formulario comienza a cargarse sin errores. |
|  | Registrar el tiempo exacto en que el formulario queda completamente disponible para interacción. | El tiempo total de carga es ≤ 14.5 segundos. |
| TC-03-HU-136903 - El indicador de carga debe aparecer cuando el proceso supera los 2 segundos | Configurar entorno: Módulo FRMcargar CI accesible. Coordinar con el equipo de desarrollo un escenario controlado donde el tiempo de carga supere 2s (p. ej. datos voluminosos o limitación de red en QA). Clasificación: Happy Path. Criterio: CA-06. |  |
|  | Ejecutar el módulo FRMcargar CI en condición de carga > 2s. | El formulario inicia el proceso de carga. |
|  | Observar la pantalla durante los primeros 2 segundos del proceso. | No se muestra ningún indicador de carga antes de los 2 segundos. |
|  | Observar la pantalla al superar los 2 segundos con el proceso aún en curso. | Aparece el indicador de carga ("Cargando": spinner, barra de progreso o "bombillo CI"). El indicador es visible y reconocible. |
|  | Esperar a que la carga finalice. | El indicador de carga desaparece al completarse. El formulario queda listo para uso. |
| TC-04-HU-136903 - El log de métricas debe insertarse en la tabla BD cuando la carga finaliza exitosamente en CI | Configurar entorno: Módulo FRMcargar CI accesible en QA. Usuario autenticado como "usuarioQA01" (o usuario conocido). Acceso de lectura a la tabla de logs de métricas en BD (o API de consulta equivalente). Anotar fecha y hora de inicio de la prueba. Clasificación: Happy Path. Criterio: CA-03. Validación: punto a punto UI → BD. Nota: nombre de tabla y campos exactos deben confirmarse con el equipo de desarrollo (pendiente #8). |  |
|  | Ejecutar el módulo FRMcargar CI y esperar a que la carga finalice exitosamente. | El formulario carga correctamente y queda disponible para el usuario. |
|  | Consultar la tabla de logs de métricas en BD filtrando por usuario "usuarioQA01" y fecha/hora del evento registrado. | Se encuentra exactamente un nuevo registro insertado correspondiente a esta ejecución. |
|  | Verificar los campos del registro encontrado: usuario, fecha de la operación, hora de inicio, hora de fin y tiempo total de carga. | El registro contiene todos los campos requeridos con valores correctos y coherentes con la ejecución realizada. |
|  | Verificar el campo de resultado o estado del log. | El campo indica "Éxito" o equivalente definido por el equipo de desarrollo. |
| TC-05-HU-136903 - Las funcionalidades del módulo FRMcargar CI deben mantenerse intactas tras la optimización | Configurar entorno: Módulo FRMcargar CI accesible en QA post-optimización. Datos de prueba representativos disponibles. Alcance exacto de regresión debe confirmarse con Dev/PO (pendiente #8). Clasificación: Happy Path. Criterio: CA-02. |  |
|  | Ejecutar el módulo FRMcargar CI y esperar carga completa. | El formulario carga sin errores ni mensajes de excepción. |
|  | Verificar que todos los controles, campos y secciones del formulario están visibles y en su estado inicial esperado. | Los controles y secciones coinciden con el comportamiento previo a la optimización (sin pérdida de elementos visuales). |
|  | Interactuar con los flujos principales del módulo: realizar al menos una operación de cada tipo disponible (carga de datos, consultas, navegación). | Cada operación se ejecuta correctamente, sin errores ni pérdida de datos. |
|  | Verificar que no se presentan errores en consola, logs de aplicación ni mensajes en pantalla. | No hay errores ni excepciones no controladas en ninguna capa. |

### Alternos

| Title | Step Action | Step Expected Result |
|---|---|---|
| TC-06-HU-136903 - La ventana FRMcargar debe ser movible mientras la carga está en progreso | Configurar entorno: Módulo FRMcargar CI en condición de carga > 2s (ver TC-03). Indicador de carga visible en pantalla. Clasificación: Alterno. Criterio: CA-07. |  |
|  | Mientras el indicador de carga está activo, hacer clic sostenido en la barra de título y arrastrar la ventana a una nueva posición de la pantalla. | La ventana se desplaza a la posición indicada. La carga continúa sin interrupciones. |
|  | Soltar la ventana en la nueva posición y esperar a que la carga finalice. | La carga termina correctamente. El formulario queda en la nueva posición con todos sus elementos correctamente renderizados. |
| TC-07-HU-136903 - El formulario debe cerrarse cuando el usuario confirma la cancelación de la carga | Configurar entorno: Módulo FRMcargar CI en condición de carga > 2s. Indicador de carga y opción de cancelar visibles en pantalla. Clasificación: Alterno. Criterio: CA-08 (confirmar). |  |
|  | Hacer clic en el control de cancelar (botón o ícono X) mientras la carga está activa. | Aparece el diálogo de confirmación con mensaje "¿Desea cancelar la carga?" y opciones Aceptar / No. |
|  | Hacer clic en "Aceptar" para confirmar la cancelación. | El diálogo se cierra. El formulario FRMcargar se cierra completamente. No quedan datos parciales en pantalla. El usuario regresa a la pantalla o menú anterior. |
|  | Verificar que no se generó error en los logs de aplicación. | Los logs no registran excepción. El evento puede quedar registrado como "Cancelado" en la tabla de métricas. |
| TC-08-HU-136903 - La carga debe continuar cuando el usuario rechaza la cancelación en el diálogo | Configurar entorno: Mismo entorno que TC-07. Módulo FRMcargar CI en condición de carga > 2s. Clasificación: Alterno. Criterio: CA-08 (rechazar). |  |
|  | Hacer clic en el control de cancelar mientras la carga está activa. | Aparece el diálogo de confirmación de cancelación. |
|  | Hacer clic en "Cancelar" / "No" para rechazar la cancelación. | El diálogo se cierra. El indicador de carga permanece activo. La carga continúa. |
|  | Esperar a que la carga finalice completamente. | El formulario carga correctamente. No hay pérdida de datos ni errores. |
| TC-09-HU-136903 - FRMcargar CI debe cargar en < 10s en entornos de QA y desarrollo | Configurar entorno: Módulo FRMcargar CI accesible en entorno de QA y en entorno de desarrollo. Herramienta de medición de tiempo activa. Datos de prueba representativos. Clasificación: Alterno. Criterio: CA-01 (meta ideal) + CA-04 (multi-entorno). |  |
|  | Ejecutar el módulo FRMcargar CI en entorno de QA y registrar el tiempo total de carga. | El formulario carga correctamente. El tiempo total es < 10 segundos. |
|  | Ejecutar el módulo FRMcargar CI en entorno de desarrollo y registrar el tiempo total de carga. | El formulario carga correctamente. El tiempo total es < 10 segundos. |
|  | Comparar los tiempos de QA y desarrollo con el tiempo de producción registrado en TC-01. | La mejora es consistente en los tres entornos. No hay degradación significativa de rendimiento en ningún entorno. |

### Negativos

| Title | Step Action | Step Expected Result |
|---|---|---|
| TC-10-HU-136903 - La aplicación no debe bloquearse cuando la BD no está disponible al iniciar la carga | Configurar entorno: Módulo FRMcargar CI en QA o desarrollo. BD interrumpida o no disponible (coordinar con infraestructura para simular el fallo). Clasificación: Negativo. Criterio: CA-02 (robustez ante fallo). SUPUESTO — comportamiento ante fallo no definido en la HU; pendiente de validación del PO (hallazgo #7). |  |
|  | Con la BD no disponible, ejecutar el módulo FRMcargar CI. | El formulario intenta iniciar la carga. |
|  | Observar el comportamiento de la aplicación durante el intento de carga. | La aplicación no se bloquea (no queda "frozen"), no lanza excepción no controlada y no cierra abruptamente. |
|  | Verificar el mensaje que recibe el usuario. | Se muestra un mensaje de error comprensible (p. ej. "No se pudo conectar con la base de datos. Intente nuevamente."). |
|  | Verificar la estabilidad de los demás módulos de la aplicación CI. | Los otros módulos siguen siendo accesibles. El fallo queda contenido en el módulo FRMcargar. |
| TC-11-HU-136903 - FRMcargar no debe generar dependencias adicionales ni afectar el funcionamiento de otros módulos | Configurar entorno: Aplicación CI completa en QA post-optimización. Módulo de órdenes y demás módulos accesibles y en estado operativo previo a la prueba. Clasificación: Negativo. Criterio: CA-05. |  |
|  | Ejecutar el módulo FRMcargar CI con la optimización aplicada hasta completar la carga. | El módulo FRMcargar CI carga correctamente. |
|  | Navegar a cada módulo crítico de la aplicación CI (especialmente módulo de órdenes) y ejecutar sus flujos principales. | Cada módulo opera correctamente, sin cambios de comportamiento respecto al estado previo a la optimización. |
|  | Verificar que no se cargaron librerías, servicios o dependencias adicionales al iniciar FRMcargar (revisar procesos activos o DLLs cargadas si es posible). | No se detectan nuevas dependencias cargadas que no existían antes. |
|  | Ejecutar el módulo de órdenes específicamente y verificar que no recibe ni procesa datos de FRMcargar. | El módulo de órdenes funciona de forma completamente independiente de FRMcargar. |

### Borde

| Title | Step Action | Step Expected Result |
|---|---|---|
| TC-12-HU-136903 - El indicador de carga NO debe aparecer cuando el proceso dura exactamente 2.0s | Configurar entorno: Módulo FRMcargar CI accesible. Coordinar con el equipo de desarrollo un escenario controlado donde la carga dure exactamente 2.0 segundos (mock o configuración de prueba). Clasificación: Borde. Criterio: CA-06. Técnica: Valor límite (límite exacto). |  |
|  | Ejecutar el módulo FRMcargar CI en el escenario de 2.0 segundos de carga. | El formulario inicia y completa la carga en 2.0 segundos. |
|  | Observar la pantalla durante todo el proceso de carga. | No aparece ningún indicador de carga (spinner, barra de progreso ni "bombillo CI") durante los 2.0 segundos. |
|  | Verificar que el formulario quedó completamente cargado y operativo. | El formulario está listo para uso, sin indicadores activos. |
| TC-13-HU-136903 - El indicador de carga DEBE aparecer cuando el proceso supera los 2.0s | Configurar entorno: Módulo FRMcargar CI accesible. Coordinar con el equipo de desarrollo un escenario controlado donde la carga dure 2.1 segundos (mock o configuración de prueba). Clasificación: Borde. Criterio: CA-06. Técnica: Valor límite (límite superior inmediato). |  |
|  | Ejecutar el módulo FRMcargar CI en el escenario de 2.1 segundos de carga. | El formulario inicia la carga. |
|  | Observar la pantalla al transcurrir los 2 segundos con la carga aún en curso. | El indicador de carga aparece en pantalla ("Cargando": spinner, barra de progreso o "bombillo CI"). |
|  | Esperar a que la carga finalice (2.1s). | El indicador desaparece al completarse. El formulario queda completamente cargado y operativo. |

## 6. Matriz de trazabilidad final

| Criterio de aceptación | Casos que lo cubren | Estado de cobertura |
|---|---|---|
| CA-01: Tiempo de carga ≤ 14.5s — módulo CI | TC-01 | Completo |
| CA-01: Tiempo de carga ≤ 14.5s — módulo CI Express | TC-02 | Completo |
| CA-01: Meta ideal < 10s | TC-09 | Completo |
| CA-02: Funcionalidad del módulo intacta | TC-05 | Parcial (alcance de regresión pendiente, hallazgo #8) |
| CA-02 / Negativo: Robustez ante fallo de BD | TC-10 | Parcial (supuesto — pendiente validación PO, hallazgo #7) |
| CA-03: Log de métricas en tabla BD | TC-04 | Parcial (nombre de tabla y campos exactos a confirmar con Dev, hallazgo #8) |
| CA-04: Mejora sostenida en múltiples entornos | TC-01 (producción CI), TC-02 (producción CI Express), TC-09 (QA/dev) | Completo |
| CA-05: Sin dependencias adicionales | TC-11 | Completo |
| CA-06: Indicador visual si proceso > 2s | TC-03, TC-12, TC-13 | Completo |
| CA-07: Ventana movible durante carga | TC-06 | Completo |
| CA-08: Cancelación con diálogo — confirmar | TC-07 | Completo |
| CA-08: Cancelación con diálogo — rechazar | TC-08 | Completo |

## 7. Cobertura pendiente

- **CA-02 (regresión completa)**: El listado exacto de funcionalidades de FRMcargar a verificar en regresión no está definido. TC-05 cubre un humo general. _Depende de: hallazgo #8 — responsable: PO/Dev._
- **CA-03 (validación punto a punto BD)**: El nombre de la tabla de logs y los campos exactos no están disponibles sin el reporte de gaps. TC-04 describe la validación pero su ejecución queda condicionada a confirmar la estructura con el equipo de desarrollo. _Depende de: hallazgo #8 — responsable: Dev._
- **CA-02 / Negativo (fallo de BD)**: El comportamiento esperado ante BD no disponible no fue definido en la HU. TC-10 está diseñado con supuesto razonable pero debe validarse con el PO. _Depende de: hallazgo #7 — responsable: PO/QA._
- **Regresión FRMcargar CI Express**: TC-05 y TC-04 fueron diseñados solo para CI. Deben replicarse para CI Express una vez confirmado el alcance de regresión. _Depende de: hallazgo #8 — responsable: PO/Dev._

---
## 🔗 Hand-off
- **Artefacto**: `resultado/HU-136903/04-casos-prueba-HU-136903.md`
- **Producido por**: qa-test-design
- **Estado**: Parcial
- **Pendientes de validación**: Alcance de regresión FRMcargar (hallazgo #8), estructura tabla de logs en BD (hallazgo #8), comportamiento ante fallo de BD (hallazgo #7)
- **Siguiente agente sugerido**: @qa-ado-registration
- **Notas para el siguiente agente**: 13 casos diseñados (TC-01 a TC-13), 4 con cobertura parcial marcada. TC-04, TC-05 y TC-10 tienen notas de validación pendiente visibles en los pasos. Módulo CI Express requiere replicar TC-04 y TC-05 tras confirmar alcance con Dev. Destino ADO: organización rentingcolombia, proyecto CentroDeInteligencia, tipo Test Case.
---

## 🔗 Conexiones
- Siguiente artefacto: [[05-registro-ado.template]]
