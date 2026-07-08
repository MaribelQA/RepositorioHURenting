# Reporte de Clarificación — HU-136903

## 1. Resumen del análisis
- **HU analizada**: 136903 — CI EXPRESS OPTIMIZAR TIEMPO DE CARGA
- **Veredicto**: Sin bloqueantes, se puede avanzar
- **Preguntas hechas / respondidas**: 8 / 8

## 2. Matriz de hallazgos

| # | Categoría | Hallazgo | Tipo | Severidad | Impacta diseño de pruebas | Estado | Pregunta |
|---|-----------|----------|------|-----------|--------------------------|--------|----------|
| 1 | Atributos de calidad | Criterio de rendimiento con dos umbrales: ≥50% vs <10s. ¿Cuál es pass/fail? | Ambigüedad | Alta | Sí | Resuelto | P1 |
| 2 | Atributos de calidad | Punto de medición del tiempo no definido (desde/hasta qué evento) | Omisión | Alta | Sí | Resuelto | P2 |
| 3 | Interacción y flujo (UX) | Comportamiento post-cancelación no definido | Omisión | Alta | Sí | Resuelto | P3 |
| 4 | Señales de completitud | Tareas de desarrollo mezcladas con criterios de aceptación | Inconsistencia | Media | Sí | Resuelto | P4 |
| 5 | Integraciones / Dependencias | Imagen referenciada no disponible | Omisión | Media | Sí | Resuelto | P5 |
| 6 | Alcance funcional | CI y CI Express: ¿módulos distintos? ¿Pruebas en ambos? | Ambigüedad | Media | Sí | Resuelto | P6 |
| 7 | Integraciones / Dependencias | Log de métricas: ubicación y formato no definidos | Omisión | Media | Sí | Resuelto (supuesto provisional) | P7 |
| 8 | Atributos de calidad | Umbral de tiempo: ¿mismo en todos los entornos? | Ambigüedad | Media | Sí | Resuelto | P8 |
| 9 | Dominio y datos | Entorno descrito como "probablemente" .NET/C# | Ambigüedad | Media | No | Pendiente de validación | — |
| 10 | Casos borde | Escenario si la optimización no alcanza el umbral | Escenario no contemplado | Baja | No | Pendiente de validación | — |

## 3. Bitácora de aclaraciones

### Sesión 2026-07-08
- P1: ¿Cuál es el umbral de pass/fail: ≤14.5s o <10s? → R: **≤14.5s** (50% de 29s). Menos de 10s es meta deseable, no bloqueante.
- P2: ¿Desde/hasta qué evento se mide el tiempo? → R: Desde **clic del usuario** hasta formulario **completamente cargado e interactivo** (datos visibles, controles habilitados).
- P3: ¿Qué pasa al cancelar la carga? → R: El formulario **se cierra**, vuelve al estado anterior y se **registra la cancelación en el log** (usuario, fecha/hora).
- P4: ¿Cómo tratar las tareas de desarrollo listadas como criterios? → R: **Excluidas** como criterios verificables. QA valida solo resultados observables (tiempo, funcionalidad, logs).
- P5: ¿La imagen aporta información adicional? → R: Solo muestra el formulario con ~29s ya descrito. **No aporta contexto adicional**.
- P6: ¿CI y CI Express son módulos distintos? → R: Sí, son **módulos/aplicaciones distintas** con formularios separados. Casos se ejecutan en **ambos**.
- P7: ¿Dónde y en qué formato se almacena el log? → R: Aún no definido por desarrollo. **Supuesto provisional**: tabla en BD (usuario, fecha/hora, tiempo ms, módulo, resultado). *Pendiente confirmación del PO/equipo.*
- P8: ¿Mismo umbral en todos los entornos? → R: Se hará **medición baseline en QA antes** de implementar; luego se valida que el tiempo **se reduce** con la solución. Comparación antes/después.

## 4. Pendientes de validación (no bloqueantes)

### 4a. Pendientes que impactan diseño de casos (Impacta diseño de pruebas: Sí)
- **Log de métricas — ubicación y formato**: supuesto provisional = tabla en BD. Los casos de verificación del log están diseñados sobre este supuesto. Si cambia la implementación, ajustar los casos. — _criterio afectado: verificación de log de métricas · responsable sugerido: PO/Dev_

### 4b. Pendientes cosméticos/administrativos (Impacta diseño de pruebas: No)
- **Plataforma tecnológica**: descrita como "probablemente" .NET/C#. No afecta pruebas funcionales. — _responsable sugerido: Dev_
- **Escenario de fallo de optimización**: no definido qué ocurre si no se alcanza el umbral tras los cambios. — _responsable sugerido: PO_

## 5. Bloqueantes (100% requeridos)
ninguno

## 6. Sugerencias de criterios de aceptación para el PO _(opcional)_

- **SUGERENCIA — requiere validación del PO**: Agregar criterio explícito sobre el comportamiento al cancelar: _"Si el usuario cancela la carga, el formulario se cierra, se retorna al estado anterior y se registra la cancelación en el log."_
- **SUGERENCIA — requiere validación del PO**: Separar las tareas de implementación (analizar código, optimizar consultas) de los criterios de aceptación verificables. Moverlas a la descripción técnica o tareas hijas.
- **SUGERENCIA — requiere validación del PO**: Especificar que los casos de prueba se ejecutan en **ambos módulos** (CI y CI Express) de forma independiente.

---
## 🔗 Hand-off
- **Artefacto**: `resultado/HU-136903/02-reporte-clarificacion-HU-136903.md`
- **Producido por**: qa-refinement
- **Estado**: Completado
- **Pendientes de validación**: Log de métricas (supuesto provisional — tabla BD); plataforma tecnológica; escenario de fallo de optimización
- **Siguiente agente sugerido**: @qa-gap-analysis o @qa-test-design
- **Notas para el siguiente agente**: HU de rendimiento. Umbral ≤14.5s, medición clic→interactivo, validación antes/después en QA. CI y CI Express son módulos separados, probar ambos. Log de métricas asumido en BD (supuesto provisional).
---
