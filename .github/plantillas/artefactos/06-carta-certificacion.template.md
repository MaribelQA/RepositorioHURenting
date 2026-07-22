<!--
TEMPLATE — Carta de Certificación de Pruebas.
Cópialo a `resultado/HU-<id>/06-carta-certificacion-HU-<id>.md` y reemplaza los <placeholders>.
Esta carta certifica la ejecución de pruebas funcionales sobre una HU validada.
Elimina las secciones que no apliquen (ej. "Riesgos Identificados" si no hubo riesgos).
Salida en español, tono formal.
-->

# Carta de Certificación de Pruebas — HU-<id>

---

| Campo | Detalle |
| --- | --- |
| **Historia de Usuario** | HU-<id> — <título de la HU> |
| **Fecha de certificación** | <YYYY-MM-DD> |
| **Período de ejecución** | <fecha inicio> al <fecha fin> |
| **Analista QA** | <nombre del analista> |
| **Ambiente de pruebas** | <QA / Staging / UAT> |
| **Estado de certificación** | <Certificada ✅ \| Certificada con riesgos ⚠️ \| No certificada ❌> |

---

## 1. Introducción

Por medio de la presente se certifica la ejecución de pruebas funcionales realizadas sobre la Historia de Usuario **"<título de la HU>"**, cuyo objetivo es <descripción del objetivo funcional de la HU>.

<Descripción de la iniciativa asociada: qué problema de negocio resuelve, qué ajuste o mejora introduce y qué restricciones existentes no se ven afectadas.>

Durante la ejecución se validaron los escenarios definidos en los criterios de aceptación de la historia de usuario.

---

## 2. Alcance de las Pruebas

Las pruebas funcionales realizadas contemplaron los siguientes aspectos:

1. **<Título del aspecto validado 1>** — <Descripción breve: qué se verificó, en qué aplicación/módulo y bajo qué regla o condición.>
2. **<Título del aspecto validado 2>** — <Descripción breve.>
3. **<Título del aspecto validado 3>** — <Descripción breve.>
4. **<Título del aspecto validado 4>** — <Descripción breve.>
<!-- Agrega o elimina ítems según los aspectos reales validados -->

---

## 3. Resumen de Resultados

| Métrica | Valor |
| --- | --- |
| Total de casos de prueba ejecutados | <n> |
| Casos exitosos (Pass) | <n> |
| Casos fallidos (Fail) | <n> |
| Casos bloqueados | <n> |
| Defectos reportados | <n> |
| Defectos cerrados / resueltos | <n> |
| Defectos pendientes | <n> |

<!-- Si no aplica tabla de defectos, elimínala y reemplaza por un párrafo de "Sin defectos identificados". -->

### Defectos registrados

| ID Defecto | Descripción | Severidad | Estado |
| --- | --- | --- | --- |
| <id> | <descripción breve> | Alta / Media / Baja | Abierto / Cerrado / Diferido |
<!-- Elimina esta tabla si no hay defectos -->

---

## 4. Riesgos Identificados

<!-- Elimina esta sección si no se identificaron riesgos durante las pruebas -->

Durante la ejecución de las pruebas se identificó el siguiente riesgo funcional:

### 4.1 Descripción del riesgo

<Describir el riesgo: en qué escenario se identificó, qué comportamiento diferencial o inesperado se observó y cuál es su origen (regla heredada, integración externa, configuración de ambiente, etc.).>

### 4.2 Impacto del riesgo

Los principales impactos identificados son los siguientes:

1. **<Impacto 1>** — <Descripción breve del impacto sobre la interpretación funcional, la operación o la estabilidad en producción.>
2. **<Impacto 2>** — <Descripción breve.>
3. **<Impacto 3>** — <Descripción breve.>
<!-- Agrega o elimina ítems según corresponda -->

### 4.3 Mitigación

<Describir cómo fue mitigado o acotado el riesgo: pruebas adicionales ejecutadas, acuerdo con el PO, documentación generada, decisión tomada y por quién. Si el PO autorizó la salida, registrar su declaración textual.>


---

## 5. Escenarios No Ejecutados o Simulados

<!-- Elimina esta sección si todos los casos de prueba fueron ejecutados directamente en el ambiente -->

Durante la ejecución se presentaron los siguientes casos que no pudieron ejecutarse de forma directa o que requirieron simulación:

| Caso de prueba | Motivo de no ejecución o simulación | ¿Resultado concluyente? |
| --- | --- | --- |
| <ID / Título del caso> | <Descripción: ambiente no disponible / funcionalidad no desplegada / integración externa no disponible / uso de mock o simulación> | Sí / No — <observación> |
<!-- Añade o elimina filas según los casos reales -->

<Párrafo explicativo si aplica: impacto de los casos no ejecutados sobre la cobertura de la certificación y acción recomendada (re-ejecución, pendiente de ambiente, diferido para próximo sprint, etc.).>

---

## 6. Conclusión y Recomendaciones

<Párrafo de cierre que resume el resultado general de las pruebas, el estado de la HU desde la perspectiva QA y el veredicto final de certificación.>

<Si hay riesgos: mencionar la mitigación aplicada y el acuerdo alcanzado con el PO.>

<Recomendaciones concretas para el equipo: nuevas HU sugeridas, ajustes técnicos pendientes, seguimiento post-despliegue, documentación que falta, o cualquier observación relevante que deba atenderse antes o después del despliegue a producción.>

---

## 🔗 Hand-off
- **Artefacto**: `resultado/HU-<id>/06-carta-certificacion-HU-<id>.md`
- **Producido por**: <analista QA / agente>
- **Estado**: Completado | Parcial | Bloqueado
- **Pendientes de validación**: <lista o "ninguno">
- **Siguiente agente sugerido**: — (artefacto final de certificación)
- **Notas**: <observaciones relevantes para el equipo o para el registro histórico>
