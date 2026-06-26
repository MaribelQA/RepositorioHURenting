# Lineamientos QA — Renting

> Convenciones del equipo QA. Las consultan los QA humanos y los agentes (on-demand) para
> producir resultados consistentes. **Completa según los acuerdos de tu equipo.**

## Criterios de severidad (hallazgos y gaps)
| Severidad | Cuándo usarla | Efecto en el flujo |
| --- | --- | --- |
| Alta | <impide entender la HU o haría incorrectos los casos> | Suele ser **bloqueante** (100% requerido) |
| Media | <afecta cobertura o calidad, pero se puede avanzar> | `Pendiente de validación` |
| Baja | <detalle menor, cosmético o diferible> | Se registra y se continúa |

## ¿Qué cuenta como bloqueante?
> Recordatorio: se bloquea **solo** ante dudas 100% requeridas (constitución §3.3).

- ✅ Bloqueante: <ej.: falta el número de work item; una regla cuya ausencia invierte el resultado esperado>
- ❌ NO bloqueante: <ej.: falta un dato de UI no crítico; un escenario de borde poco probable> → `Pendiente de validación`

## Convenciones de casos de prueba
- Un caso valida **un solo comportamiento**.
- Cobertura mínima por criterio: <positivo + negativo + borde>.
- Naming del título: `[Acción] debería [resultado] cuando [condición]`.
- Cada `Step Action` debe tener su `Stept Expected Result` (formato ADO, ver `plantillas/formato-cp.md`).
- <otra convención del equipo>

## Definición de Hecho (DoD) de una HU lista para casos
- [ ] Criterios de aceptación testables (observables y medibles).
- [ ] Sin bloqueantes abiertos en el `02-reporte-clarificacion`.
- [ ] <otro criterio del equipo>

## Datos de prueba
- Entornos: <QA / Staging / Local — cuál es el por defecto>.
- Origen de datos: <BD, SP, fixtures…>.
- <reglas de datos sensibles / anonimización si aplica>

## Registro en Azure DevOps
- Organización / Proyecto: <valores>.
- Test Plan / Suite por defecto: <valor>.
- Prioridad por defecto: <Alta/Media/Baja>.
- <convención de vínculo HU ↔ Test Case>

## 🔗 Conexiones
- Formato de casos para ADO: [[formato-cp]]
