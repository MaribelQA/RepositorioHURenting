---
name: qa-informe-semanal
agent: qa-weekly-report
description: 'Entrevista interactiva para tu Informe Semanal de Tareas QA. Campo a campo, pregunta por pregunta: semana, HU, estado, avance, bloqueos. Sin depender del disco - solo tus datos frescos.'
argument-hint: 'Opcional: semana a reportar (ej.: "13 al 17 de octubre" o "semana actual"). Sin argumento, pregunto la semana y luego te entrevisto campo a campo por cada HU.'
---

# Informe Semanal de Tareas — QA

Ejecuta el agente `qa-weekly-report`.

**Flujo — Entrevista interactiva y dinámica:**
1. **Pregunta la semana** (una vez).
2. **Por cada HU, pregunta campo a campo:**
   - ID y título de la HU
   - Estado actual (Activa / Testing QA / In Approved / In Approved PR / Production / Stabilization / Blocked QA / Closed)
   - **Preguntas dinámicas según el estado:**
     - **Activa**: ¿Qué actividad realizas? (análisis, diseño, etc.) + avance
     - **Testing QA**: Casos ejecutados, resultados (Pass/Fail), bugs reportados, fase (regresión o nuevos)
     - **In Approved**: ¿Se realizó review con usuario? + estado de Carta de Certificación
     - **Blocked QA**: Descripción del bloqueo + progreso alcanzado + expectativa de desbloqueo
     - **Production**: ¿Salida a producción? + validación de flujos happy path
     - **Stabilization**: ¿Incidentes reportados? + estado del monitoreo
     - **Closed**: Observación final de cierre
   - **Para todos**: Bloqueos o dependencias
   - ¿Otra HU más esta semana? → Sí/No
3. **Actividades adicionales**: ¿Realizaste refinamiento de HUs, análisis, soporte, documentación u otras actividades? 
4. **Construye y guarda** `qa-analisis-casos/informes/informe-semanal-<YYYY-MM-DD>.md` con los datos recolectados.

- **Plantilla**: `.github/plantillas/informe-semanal.template.md`.
- **Contextual**: las preguntas se adaptan al estado de cada HU.
- **Datos frescos**: todo viene de tu narrativa en tiempo real, no del disco → garantiza actualidad.
- **Modo incremental**: invócalo varias veces en la semana; agrega nuevas HUs, el informe se actualiza.

> Nada se inventa. La entrevista es inteligente: preguntas precisas según lo que realmente necesitas reportar de cada fase.

## 🔗 Conexiones
- Ejecuta: [[qa-weekly-report.agent|qa-weekly-report]]
- Plantilla: [[informe-semanal.template]]
