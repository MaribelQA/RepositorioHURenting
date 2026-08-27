# Ejemplo Golden — Casos de prueba (referencia few-shot)

> **Para el agente `qa-test-design` (consulta on-demand, no auto-cargar).** Este es el
> **estándar de "cómo se ve un caso bien hecho"** en esta suite. Imita el **patrón y el nivel
> de detalle**, NO el contenido (la HU de ejemplo es ficticia). Leerlo **una vez** al inicio del
> diseño evita reinventar el formato en cada corrida (menos tokens, más consistencia).
>
> No copies estos casos a un artefacto real. Son plantilla mental, no contenido reutilizable.

---

## Qué hace "golden" a un caso (checklist rápido)

- [ ] **Un solo comportamiento por caso** — si el título tiene un "y", probablemente son dos casos.
- [ ] **Título = ID + patrón**: `TC-NN-HU-<id> - [Acción] debe [resultado] cuando [condición]`.
- [ ] **Oracle explícito**: el resultado esperado es **observable y verificable**, no "funciona bien".
- [ ] **Precondición separada** (fila con `Step Expected Result` vacío) del cuerpo ejecutable.
- [ ] **Mensajes/textos exactos entre comillas** cuando la HU los especifica (no parafrasear).
- [ ] **Validación de persistencia** cuando el flujo crea/actualiza/elimina datos.
- [ ] **Clasificación y criterio citados** en la precondición (Happy Path / Alterno / Negativo / Borde + CA-N).
- [ ] **Datos concretos**, no genéricos: placa `ABC-123`, monto `$0`, `101` caracteres — no "un valor inválido".

---

## Caso 1 — Happy Path con validación de persistencia (BD)

> Golden porque: valida el resultado **funcional visible** Y la **consistencia del dato persistido**
> en el mismo flujo. Toda operación que crea/actualiza/elimina datos necesita al menos un caso así.

| Title | Step Action | Step Expected Result |
| --- | --- | --- |
| TC-01-HU-99999 - Registrar solicitud debe persistir en estado "Pendiente" cuando los datos son válidos | Configurar entorno: usuario con rol Analista autenticado; formulario de solicitud vacío. Clasificación: Happy Path. Criterio: CA1. |  |
|  | 1. Diligenciar campos obligatorios con datos válidos (monto=$500.000, placa=ABC-123) | Campos aceptan los valores sin error de validación |
|  | 2. Presionar "Guardar solicitud" | Sistema muestra confirmación "Solicitud creada" sin errores |
|  | 3. Consultar la solicitud recién creada por su ID | Registro es recuperado del sistema |
|  | 4. Verificar persistencia: tabla `Solicitudes`, campo `estado` | `estado = "Pendiente"`; `monto = 500000`; `placa = "ABC-123"` |

## Caso 2 — Negativo con oracle de mensaje exacto

> Golden porque: usa un **dato de borde concreto** que dispara la regla, verifica que el sistema
> **bloquea** la acción, y valida el **texto exacto** del mensaje (no una paráfrasis). Un solo motivo
> de fallo por caso.

| Title | Step Action | Step Expected Result |
| --- | --- | --- |
| TC-02-HU-99999 - Rechazar solicitud debe mostrar alerta cuando el monto supera el tope permitido | Configurar entorno: usuario Analista autenticado; tope configurado = $1.000.000. Clasificación: Negativo. Criterio: CA2. |  |
|  | 1. Diligenciar monto = $1.000.001 (un peso sobre el tope) | Campo acepta el valor ingresado |
|  | 2. Presionar "Guardar solicitud" | Sistema bloquea el guardado (no crea registro) |
|  | 3. Leer el mensaje de alerta mostrado | Mensaje contiene exactamente: "El monto supera el tope permitido de $1.000.000." |

## Caso 3 — Borde (valor límite)

> Golden porque: prueba el **límite exacto** de la regla (el valor máximo aceptado), no un valor
> "cómodo" en el medio de la partición. Los defectos viven en los límites.

| Title | Step Action | Step Expected Result |
| --- | --- | --- |
| TC-03-HU-99999 - Aceptar solicitud debe persistir cuando el monto es exactamente el tope máximo | Configurar entorno: usuario Analista autenticado; tope configurado = $1.000.000. Clasificación: Borde. Criterio: CA2 (valor límite). |  |
|  | 1. Diligenciar monto = $1.000.000 (valor exacto del tope) | Campo acepta el valor sin alerta |
|  | 2. Presionar "Guardar solicitud" | Sistema guarda la solicitud sin mostrar alerta de tope |
|  | 3. Consultar la solicitud por su ID | `estado = "Pendiente"`; `monto = 1000000` |

---

## Antipatrones a evitar (ejemplos de casos NO golden)

| Antipatrón | Ejemplo malo | Por qué falla |
| --- | --- | --- |
| Oracle vago | "Verificar que funciona correctamente" | No es observable ni verificable |
| Multi-comportamiento | "Guardar y luego editar y eliminar la solicitud" | Debe ser 3 casos; si falla, no se sabe cuál |
| Dato genérico | "Ingresar un monto inválido" | No reproducible; ¿negativo? ¿texto? ¿sobre el tope? |
| Mensaje parafraseado | "Muestra un error de monto" | No detecta typos ni cambios de copy; usar texto exacto |
| Sin persistencia | Solo valida la pantalla tras un "Guardar" | No detecta que el dato no llegó a BD |
| Precondición con resultado | Fila de setup con `Step Expected Result` lleno | Rompe el formato ADO; setup no es un paso verificable |

## 🔗 Conexiones
- Plantilla base: [[04-casos-prueba.template]]
- Convenciones: [[lineamientos-qa]]
