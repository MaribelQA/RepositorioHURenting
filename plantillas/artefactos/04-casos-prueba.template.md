<!--
TEMPLATE — Casos de prueba (Paso 3, agente qa-test-design).
Cópialo a `resultado/HU-<id>/04-casos-prueba-HU-<id>.md` y reemplaza los <placeholders>.
Cada caso valida UN SOLO comportamiento. Sigue:
- Lineamientos QA: `docs/lineamientos-qa.md`
Incluye cobertura positiva, negativa y de borde. Salida en español.
-->

# Casos de prueba — HU-<id>

## 1. Resumen de cobertura
- **HU**: <id> — <título>
- **Total de casos**: <n>  (Positivos: <n> · Negativos: <n> · Borde: <n>)
- **Insumos usados**: clarificación (`02`)<, gaps (`03`) si aplica>
- **Técnicas aplicadas**: <partición de equivalencia / valores límite / tabla de decisión / transición de estados / error guessing>

## 2. Trazabilidad criterio ↔ caso
| Criterio de aceptación | Casos que lo cubren |
| --- | --- |
| <criterio> | <IDs de caso> |

## 3. Casos (formato Azure DevOps — Work Item Test Case)
> Cada `Step Action` debe tener su `Stept Expected Result`. EXCEPTO el paso 1 que es la Precondición, este no tiene `Stept Expected Result`.

## 🆔 Test Case Identification

**Test Case ID**: TC_[MODULE]*[FEATURE]*[###]
**Title**: [Action] should [expected outcome] when [condition]

## 🔢 Test Steps

| Step | Action                                                                                                      | Expected Result            |
| ---- | -------- |
| 1    | @Preconditions: * [System state required before] * [User must be logged in / data exists / etc.]           |                            |
| 2    | [Describe the action performed by the user]                                                                 | [Expected system behavior after this step] |
| 3    | [Next action]                                                                                               | [Expected result]          |
| N    | [...]                                                                                                       | [...]                      |


## 📝 Description

[Brief explanation of what is being validated and why]


---

**Notes (Optional)**
* [Any additional context or assumptions]

---
---

## 🧩 Example (Reference Only - Do NOT reuse)

**Test Case ID**: TC_AUTH_LOGIN_001
**Title**: User should log in successfully when valid credentials are provided

**Test Steps**:

| Step | Action | Expected Result |
| ---- | ---- | ----|
| 1    | @Preconditions: * User account exists |  |
| 2    | Navigate to login page         | Login page is displayed         |
| 3    | Enter valid email and password | Credentials are accepted        |
| 4    | Click login button             | User is redirected to dashboard |


**Description**: Validate that a registered user can log in with valid credentials

**Test Data**:
* Input: [valid_user@test.com](mailto:valid_user@test.com) / Password123
* Output: login exitoso

**Test Type**:
* Functional

**Priority**:
* High

**Environment**:
* QA


## 4. Cobertura pendiente
> Criterios que NO se cubrieron por depender de pendientes/bloqueantes de la clarificación
> (sugerencias del PO sin validar, datos faltantes, etc.). No inventar casos sobre supuestos.

- <criterio o escenario pendiente> — _depende de: <pendiente del `02`>_

---
## 🔗 Hand-off
- **Artefacto**: `resultado/HU-<id>/04-casos-prueba-HU-<id>.md`
- **Producido por**: qa-test-design
- **Estado**: Completado | Parcial | Bloqueado
- **Pendientes de validación**: <lista o "ninguno">
- **Siguiente agente sugerido**: @qa-ado-registration
- **Notas para el siguiente agente**: <destino ADO sugerido, casos prioritarios, cobertura pendiente>
---

## 🔗 Conexiones
- Siguiente artefacto: [[05-registro-ado.template]]
