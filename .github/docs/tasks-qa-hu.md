# Tasks QA por Historia de Usuario

> Guía de referencia para crear las **Tasks QA** de cada HU comprometida en el sprint.
> Contexto: equipo de **mejoras sobre aplicaciones en producción**; la HU llega **refinada**.
> Los tiempos son una **línea base** — calibrarlos con horas reales tras 3-4 sprints.

## Convención de nombres en ADO

```
[QA] <Nombre de la task> - HU <id>
```

Ejemplo: `[QA] Análisis estático de la HU - HU 158938`

## Resumen de tiempos

| # | Task | HU 8 pts | HU 13 pts |
| --- | --- | --- | --- |
| 1 | [QA] Análisis estático de la HU | 2 h | 3 h |
| 2 | [QA] Exploración funcional y análisis de impacto | 3 h | 4 h |
| 3 | [QA] Diseño de casos de prueba | 4 h | 6 h |
| 4 | [QA] Preparación de datos y ejecución de pruebas con evidencias | 8 h | 12 h |
| 5 | [QA] Reporte y seguimiento de defectos | 3 h | 5 h |
| 6 | [QA] Pruebas de regresión | 3 h | 5 h |
| 7 | [QA] Review / UAT con usuario final | 1,5 h | 2 h |
| 8 | [QA] Carta de certificación | 1 h | 1,5 h |
| 9 | [QA] Smoke test post-producción | 1 h | 1,5 h |
| | **Total** | **26,5 h** | **40 h** |

Distribución: análisis y diseño (1-3) ≈ 33 % · ejecución (4-6) ≈ 54 % · cierre (7-9) ≈ 13 %.

---

## Detalle de cada task

### 1. [QA] Análisis estático de la HU
**Tiempo:** 8 pts → 2 h · 13 pts → 3 h

**Descripción para ADO:**
> Revisar la HU y sus criterios de aceptación aplicando pruebas estáticas (ISTQB) para detectar ambigüedades, omisiones, inconsistencias, CA no testables y escenarios no contemplados, con el fin de ajustarlos o adicionarlos antes de la implementación.

**Actividades:**
- Leer HU, CA, reglas de negocio y anexos.
- Identificar CA mal redactados, incompletos o no verificables.
- Proponer CA faltantes (negativos, bordes, permisos, mensajes).
- Socializar hallazgos con PO/equipo y registrar respuestas.

**Entregable:** reporte de clarificación (`02-reporte-clarificacion-HU-<id>.md`).

---

### 2. [QA] Exploración funcional y análisis de impacto
**Tiempo:** 8 pts → 3 h · 13 pts → 4 h

**Descripción para ADO:**
> Recorrer el flujo actual en la aplicación para entender pantallas, estados, reglas y datos involucrados, e identificar módulos, integraciones o procesos que puedan verse impactados por la mejora y escenarios no contemplados en la HU.

**Actividades:**
- Recorrer el flujo actual end-to-end en el ambiente.
- Identificar pantallas, estados, roles y datos requeridos.
- Validar con desarrollo qué componentes se modificarán.
- Listar áreas impactadas (insumo para la regresión).
- Reportar escenarios no contemplados detectados.

**Entregable:** notas de exploración + lista de áreas impactadas.

---

### 3. [QA] Diseño de casos de prueba
**Tiempo:** 8 pts → 4 h · 13 pts → 6 h

**Descripción para ADO:**
> Diseñar los casos de prueba de la HU (positivos, negativos y de borde) con trazabilidad a los criterios de aceptación, y seleccionar los casos de regresión aplicables según el análisis de impacto.

**Actividades:**
- Aplicar técnicas de diseño (particiones, valores límite, tablas de decisión, transición de estados).
- Garantizar trazabilidad CA ↔ caso.
- Seleccionar casos de regresión existentes.
- Registrar los casos en el Test Plan de ADO.

**Entregable:** `04-casos-prueba-HU-<id>.md` + casos en Test Plan.

---

### 4. [QA] Preparación de datos y ejecución de pruebas con evidencias
**Tiempo:** 8 pts → 8 h · 13 pts → 12 h

**Descripción para ADO:**
> Verificar que el ambiente de QA cuenta con la versión desplegada correcta y ejecutar los casos de prueba diseñados para la HU, preparando de forma incremental los datos de prueba, usuarios y configuraciones que requiere cada escenario, registrando el resultado de cada caso y adjuntando evidencias (capturas, videos, logs).

**Actividades:**
- Confirmar versión desplegada, configuraciones y usuarios/roles base (reportar bloqueos de ambiente).
- Crear o identificar los datos de prueba por escenario a medida que se ejecuta.
- Ejecutar casos en el Test Plan de ADO.
- Registrar resultado (Passed / Failed / Blocked).
- Adjuntar evidencias por caso y documentar los datos usados.

**Entregable:** ejecución registrada en ADO con evidencias + set de datos documentado.

---

### 5. [QA] Reporte y seguimiento de defectos
**Tiempo:** 8 pts → 3 h · 13 pts → 5 h

**Descripción para ADO:**
> Reportar los defectos encontrados con trazabilidad a la HU y al caso de prueba, hacer seguimiento a su corrección y ejecutar el re-test correspondiente.

**Actividades:**
- Registrar bugs (pasos, resultado esperado/obtenido, evidencias, severidad).
- Vincular bug ↔ caso ↔ HU.
- Priorizar con el equipo.
- Re-test y cierre de defectos.

**Entregable:** bugs registrados y cerrados/aceptados en ADO.

---

### 6. [QA] Pruebas de regresión
**Tiempo:** 8 pts → 3 h · 13 pts → 5 h

**Descripción para ADO:**
> Ejecutar los casos de regresión seleccionados sobre las funcionalidades impactadas para verificar que la mejora no afectó comportamientos existentes en producción.

**Actividades:**
- Ejecutar suite de regresión acotada al impacto.
- Registrar resultados y evidencias.
- Reportar defectos de regresión.

**Entregable:** resultados de regresión en ADO.

---

### 7. [QA] Review / UAT con usuario final
**Tiempo:** 8 pts → 1,5 h · 13 pts → 2 h

**Descripción para ADO:**
> Presentar la funcionalidad al usuario final para su validación de negocio, registrando observaciones y la aprobación de la HU.

**Actividades:**
- Preparar el escenario de demostración y los datos.
- Ejecutar la sesión de review con el usuario.
- Registrar observaciones y aprobación.

**Entregable:** acta / registro de aprobación del usuario.

---

### 8. [QA] Carta de certificación
**Tiempo:** 8 pts → 1 h · 13 pts → 1,5 h

**Descripción para ADO:**
> Elaborar la carta de certificación de pruebas de la HU con el resumen de la ejecución, defectos, riesgos aceptados y recomendación de salida a producción.

**Actividades:**
- Consolidar resultados de ejecución y regresión.
- Listar defectos abiertos y riesgos aceptados.
- Emitir recomendación de paso a producción.

**Entregable:** `06-carta-certificacion-HU-<id>.md`.

---

### 9. [QA] Smoke test post-producción
**Tiempo:** 8 pts → 1 h · 13 pts → 1,5 h

**Descripción para ADO:**
> Hacer seguimiento a la salida a producción de la HU y ejecutar un smoke test sobre el flujo principal para confirmar el correcto funcionamiento en producción.

**Actividades:**
- Confirmar despliegue en producción.
- Validar el flujo principal (sin afectar datos reales críticos).
- Reportar incidentes y cerrar la HU.

**Entregable:** confirmación de validación en producción.

---

## Factores de ajuste

| Situación | Ajuste |
| --- | --- |
| Muchas integraciones / módulos impactados | +30-50 % en tasks 2 y 6 |
| Datos difíciles de obtener (terceros, batch) | +2-3 h en task 4 |
| Desarrollo con historial de muchos defectos | +50-100 % en task 5 |
| Suite de regresión ya documentada/automatizada | −50 % en task 6 |
| HU muy similar a otra ya probada | −30-40 % en tasks 2 y 3 |
| Ambiente de QA inestable | +10 % de buffer general |

## Tasks opcionales (según la HU)

| Task | Cuándo crearla |
| --- | --- |
| [QA] Pruebas no funcionales | Mejora con impacto en rendimiento, seguridad o accesibilidad. |
| [QA] Automatización de casos de regresión | El equipo cuenta con framework de automatización. |
