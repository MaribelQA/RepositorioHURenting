---
name: qa-casos
agent: qa-test-design
description: 'Paso 3: diseña casos de prueba desde la HU + clarificación (+ gaps). Salida lista para ADO.'
argument-hint: (opcional) enfoque o alcance. Sin esto, diseño desde la clarificación (y gaps) del folder de la HU.
---

# Paso 3 — Diseño de Casos de Prueba

Ejecuta el agente `qa-test-design`. Lee primero `00-estado-HU-<id>.md`, `01`, `02` (y `03` si existe).

- **Entrada**: `resultado/HU-<id>/` (`01`, `02`, opcional `03`).
- **Salida**: `resultado/HU-<id>/04-casos-prueba-HU-<id>.md` (se guarda siempre) + `resultado/HU-<id>/04-casos-prueba-HU-<id>.csv` (tabular para Excel) + actualizar `00`.

Sin `02` disponible, vuelve a `/qa-clarificar`. Si el `02` está `Parcial`:
- Con pendientes `Impacta diseño de pruebas: Sí` sin resolver → el agente sugiere ejecutar otra ronda de `/qa-clarificar` primero; solo diseña con supuestos si el usuario confirma explícitamente.
- Con solo pendientes `Impacta diseño de pruebas: No` → no bloquea; diseña cobertura completa.
No generes casos sobre supuestos sin validar.

**Siguiente:** `/qa-registrar`.

## 🔗 Conexiones
- Ejecuta: [[qa-test-design.agent|qa-test-design]]
- Siguiente paso: [[qa-4-registrar.prompt|/qa-registrar]]
