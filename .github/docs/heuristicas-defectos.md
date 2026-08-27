# Heurísticas de detección de defectos — Cheat Sheet QA

> **Para `qa-test-design` (consulta on-demand, no auto-cargar).** Es una **batería de disparadores**
> para "dónde suelen esconderse los fallos". Recórrela mentalmente durante el Paso 5 (generación de
> escenarios) para no dejar huecos negativos/borde. **No genera un caso por cada ítem**: úsala como
> filtro — para cada criterio de aceptación, pregúntate qué heurísticas aplican y crea casos solo
> donde haya riesgo real. Prioriza según el riesgo del criterio (ver capa RBT si está disponible).
>
> Basada en heurísticas ISTQB, error guessing y cheat sheets clásicas (Hendrickson, Bach), adaptada a Renting.

---

## 1. Entradas y datos (fronteras y particiones)

- **Vacío / nulo**: campo obligatorio vacío, lista sin elementos, archivo de 0 bytes.
- **Límites**: mínimo, mínimo−1, máximo, máximo+1, cero, negativo, el valor exacto del tope.
- **Longitud**: 0 caracteres, 1, el máximo permitido, máximo+1, texto muy largo.
- **Formato**: fecha inválida (31/02), email sin `@`, número con letras, decimales donde se espera entero.
- **Tipo**: texto en campo numérico, número negativo en cantidad, futuro/pasado en fecha.
- **Especiales/inyección**: `'`, `"`, `<script>`, `;`, emojis, acentos/ñ, espacios al inicio/fin.
- **Precisión**: redondeo de montos, centavos, división que no da exacto, moneda.

## 2. Estados y transiciones

- **Transición inválida**: intentar una acción no permitida en el estado actual (aprobar lo ya aprobado).
- **Estado inicial y final**: comportamiento en el primer registro y tras el último paso del flujo.
- **Reversión**: deshacer, cancelar a mitad, volver atrás en el navegador.
- **Idempotencia**: repetir la misma acción dos veces (doble clic en "Guardar"), reenvío.
- **Estado huérfano**: registro creado pero flujo abandonado; datos parciales.

## 3. Concurrencia y tiempo

- **Ediciones simultáneas**: dos usuarios editan el mismo registro; ¿quién gana? (last-write-wins, bloqueo).
- **Doble envío**: doble submit crea duplicados; timeouts que reintentan.
- **Orden**: eventos que llegan desordenados; dependencia temporal entre pasos.
- **Sesión**: expira a mitad de la operación; token vencido; re-login.
- **Zona horaria / fecha de corte**: operación justo en el cambio de día, fin de mes, año bisiesto.

## 4. Persistencia e integridad de datos

- **Consistencia UI ↔ BD**: lo que muestra la pantalla coincide con lo persistido (campos críticos).
- **Operación correcta**: insert vs update vs delete lógico; no sobreescribir lo que no se debe.
- **Cascada**: eliminar padre con hijos; ¿huérfanos? ¿bloqueo por integridad referencial?
- **Duplicados**: violar unicidad (misma placa, mismo documento); ¿lo detecta?
- **Rollback**: si un paso falla a mitad, ¿la transacción revierte todo o deja datos a medias?

## 5. Reglas de negocio y cálculos

- **Combinaciones de reglas**: cuando dos o más reglas aplican a la vez (usar tabla de decisión / pairwise).
- **Excepciones a la regla**: casos especiales, exenciones, valores por defecto.
- **Cálculos**: totales, porcentajes, descuentos acumulados, topes, prorrateo.
- **Regresión**: la funcionalidad previa sigue intacta cuando la nueva regla NO aplica (flag = 0/off).

## 6. Autorización, seguridad y privacidad

- **Rol/permiso**: usuario sin permiso intenta la acción; acceso directo por URL a algo no autorizado.
- **Datos sensibles**: ¿se muestran/loguean datos que no deberían? (documentos, montos, PII).
- **Escalamiento**: usuario de un cliente ve datos de otro cliente (aislamiento multi-tenant).

## 7. Integraciones y fallos externos

- **Servicio caído**: la API/integración no responde, responde lento o con error 500.
- **Respuesta inesperada**: payload vacío, campos faltantes, formato distinto al esperado.
- **Timeout / reintento**: ¿qué pasa si el servicio tarda? ¿reintenta? ¿duplica?
- **Fallback**: ¿hay comportamiento degradado definido cuando la dependencia falla?

## 8. Mensajes, UX y experiencia

- **Texto exacto**: el mensaje coincide **carácter por carácter** con lo especificado en la HU.
- **Contexto en el mensaje**: incluye datos variables correctos (placa, monto, nombre).
- **Estados vacío/carga/error**: pantalla sin datos, spinner, error recuperable.
- **Accesibilidad/localización**: idioma, formato de fecha/moneda local.

---

## Cómo usarla (guía rápida)

1. Por **cada criterio de aceptación**, recorre las 8 secciones y marca las heurísticas **aplicables**.
2. Cada heurística aplicable con riesgo real → **al menos un caso** (Negativo o Borde).
3. Si varias reglas interactúan → salta a **partición + valores límite + tabla de decisión / pairwise**.
4. Registra en la matriz de trazabilidad qué heurística originó cada caso negativo/borde (opcional pero recomendado).
5. No fuerces casos donde la heurística **no aplica** — documenta brevemente por qué se descarta si es relevante.

## 🔗 Conexiones
- Ejemplo de casos bien hechos: [[ejemplo-golden-casos]]
- Convenciones: [[lineamientos-qa]]
