---
name: qa-registrar
agent: qa-ado-registration
description: '(🟡) Paso 4: registra los casos como Test Case en Azure DevOps. Acción externa: confirma antes de escribir.'
argument-hint: Destino ADO — organización/proyecto/Test Plan/Suite. Te pido confirmación antes de crear nada.
---

# Paso 4 — Registrar Test Cases en Azure DevOps

Ejecuta el agente `qa-ado-registration`.

- **Entrada**: `qa-analisis-casos/HU-<id>/04-casos-prueba-HU-<id>.md` + destino ADO (organización, proyecto, Test Plan/Suite).
- **Salida**: Work Items (Test Case) en ADO + `qa-analisis-casos/HU-<id>/05-registro-ado-HU-<id>.md` + actualizar `00`.

⚠️ Acción externa e irreversible: muestra el resumen de lo que se creará y **pide confirmación
explícita** antes de escribir. No registres casos de un artefacto `Bloqueado`.

**Siguiente:** fin del flujo.

## 🔗 Conexiones
- Ejecuta: [[qa-ado-registration.agent|qa-ado-registration]]
