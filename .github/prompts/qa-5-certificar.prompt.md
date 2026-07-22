---
name: qa-certificar
agent: qa-certify
description: 'Paso 5: genera la Carta de Certificación de Pruebas. Inicia con una entrevista interactiva (ID de HU + 7 preguntas de ejecución) y luego produce el artefacto 06.'
argument-hint: 'Número de work item de la HU a certificar (ej.: 158938). Si no lo indicas, el agente te lo pedirá como primer paso.'
---

# Paso 5 — Generar Carta de Certificación de Pruebas

Ejecuta el agente `qa-certify`.

**El agente sigue este flujo en cada invocación:**
1. Pide el **ID de la HU** si no fue incluido en este mensaje.
2. Realiza una **entrevista interactiva** (7 preguntas sobre la ejecución: analista, ambiente, período, bugs, escenarios bloqueados, validación del PO y recomendaciones).
3. Lee los artefactos del folder `qa-analisis-casos/HU-<id>/`.
4. Genera y guarda `qa-analisis-casos/HU-<id>/06-carta-certificacion-HU-<id>.md`.
5. Actualiza `00-estado-HU-<id>.md`.

- **Entradas leídas**: `00-estado`, `01-HU`, `04-casos-prueba` (obligatorios) + `02-reporte-clarificacion` (recomendado).
- **Plantilla**: `.github/plantillas/artefactos/06-carta-certificacion.template.md`.

Si `04-casos-prueba` no existe o está `Bloqueado`, el agente no genera la carta: informa y sugiere ejecutar primero `/qa-diseñar-casos-prueba`.

**Siguiente:** artefacto de cierre del flujo. Sin pasos posteriores obligatorios.

## 🔗 Conexiones
- Ejecuta: [[qa-certify.agent|qa-certify]]
- Paso previo: [[qa-3-casos.prompt|/qa-diseñar-casos-prueba]]
