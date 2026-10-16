<!--
TEMPLATE — Informe Semanal de Tareas QA.
Lo usa el agente `qa-weekly-report` (comando /qa-informe-semanal): copia este archivo a
`qa-analisis-casos/informes/informe-semanal-<YYYY-MM-DD>.md` (fecha = lunes de la semana)
y reemplaza los <placeholders>. Duplica el bloque "## HU <n>" por cada Historia reportada.
Salida en español. Elimina este comentario al generar el artefacto final.
-->

# Informe Semanal de Tareas — QA

| Campo | Detalle |
| --- | --- |
| **Semana** | <YYYY-MM-DD> al <YYYY-MM-DD> |
| **Responsable** | <Nombre del analista QA> |
| **Fecha de corte** | <YYYY-MM-DD> |
| **HUs reportadas** | <n> |

---

## HU <n> — <título breve (opcional)>

- **HU:** <Link de la Historia de Usuario en Azure DevOps>
- **Estado:** <Activa | Testing QA | In Approved | In Approved PR | Blocked QA | ...>
- **Avance de la semana:** <Breve descripción de las actividades realizadas y logros alcanzados durante la semana.>
- **Bloqueos o dependencias:** <Impedimento, o espera de entrega por parte del equipo de desarrollo. Si no hay, indicar "Ninguno".>

---

<!-- Duplica el bloque "## HU <n>" tantas veces como HUs necesites reportar. -->

---

## Actividades adicionales (opcional)

<Refinamiento de HUs, análisis, soporte a otras áreas, documentación, capacitación, revisión de procesos, o cualquier otra actividad no directamente vinculada a HUs. Si no hay, elimina esta sección.>

---

## Resumen de la semana (opcional)

- **Total de HUs:** <n>
- **En Testing QA:** <n> · **Bloqueadas:** <n> · **Aprobadas:** <n>
- **Logros destacados:** <1-3 líneas>
- **Riesgos / dependencias abiertas:** <lista o "ninguno">

---

## Ejemplo diligenciado

- **HU:** [ID HU](https://rentingcolombia.visualstudio.com/CentroDeInteligencia/_workitems/edit/XXXXX)
- **Estado:** Testing QA
- **Avance de la semana:** Se ejecutaron los casos de prueba planificados, se reportaron 3 defectos y 2 fueron corregidos por desarrollo.
- **Bloqueos o dependencias:** Pendiente entrega de corrección del último defecto por parte de Dev.
