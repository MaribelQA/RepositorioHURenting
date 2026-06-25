# Glosario de dominio — Renting

> Contexto de negocio del producto **Renting**. Lo consultan los QA humanos y los agentes
> (on-demand) para entender términos y estados que aparecen en las HU y los casos.
> **Completa y valida con negocio/PO.** Los términos marcados `(borrador)` se infirieron del
> repo y deben confirmarse.

## Cómo usar
- Una fila por término. Sé breve y concreto.
- Si un término tiene **estados**, enuméralos y di qué los dispara.
- Marca lo no confirmado como `(borrador)` hasta validarlo con el PO.

## Términos

| Término | Definición | Notas / sinónimos |
| --- | --- | --- |
| Placa | <vehículo/activo identificado por su placa> `(borrador)` | <sinónimos a evitar> |
| Clasificación | <agrupación de placas asociada a un cliente> `(borrador)` | |
| Cliente | <entidad a la que pertenecen las placas/clasificaciones> | |
| <término> | <definición> | |

## Estados

| Entidad | Estado | Qué significa / qué lo dispara |
| --- | --- | --- |
| Placa | Fin Renting | <fin del contrato de renting; agrupa "devuelta" y "eliminada"> `(borrador)` |
| Placa | Devuelta | <la placa fue devuelta> `(borrador)` |
| Placa | Eliminada | <la placa fue eliminada de las clasificaciones> `(borrador)` |
| <entidad> | <estado> | <descripción> |

## Reglas de negocio transversales
> Reglas que aplican a varias HU (no específicas de una sola). Útiles para detectar gaps.

- <regla> `(borrador)`
- <regla>

## Siglas
| Sigla | Significado |
| --- | --- |
| HU | Historia de Usuario |
| PO | Product Owner |
| ADO | Azure DevOps |
| SP | Stored Procedure (procedimiento almacenado) `(borrador)` |
| <sigla> | <significado> |
