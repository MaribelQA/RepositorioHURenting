---
name: qa-casos
agent: qa-test-design
description: '(🟡) Paso 3: diseña casos de prueba desde la HU + clarificación (+ gaps). Salida lista para ADO.'
---

# Paso 3 — Diseño de Casos de Prueba

Ejecuta el agente `qa-test-design`. Lee primero `00-estado-HU-<id>.md`, `01`, `02` (y `03` si existe).

- **Entrada**: `resultado/HU-<id>/` (`01`, `02`, opcional `03`).
- **Salida**: `resultado/HU-<id>/04-casos-prueba-HU-<id>.md` (se guarda siempre) + actualizar `00`.

Sin `02` disponible, vuelve a `/qa-clarificar`. Un `02` Parcial no bloquea: diseña lo posible y
marca la cobertura pendiente. No generes casos sobre supuestos sin validar.

**Siguiente:** `/qa-registrar`.
