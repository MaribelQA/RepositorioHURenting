formato salida: Casos de prueba para Azure Devops (Test Plan)
La siguiente tabla es un ejemplo de como se espera el resultado, genera el resultado en un formato igual

| Title | Step Action | Stept Expected Result |
| --- | --- | --- |
| Placas devueltas o eliminadas en el centro de distribución para un CLIENTE |   |   |
|   | @Precondiciones: tener en la BD placas que se encuentran en estados devueltas o eliminadas (En estado Fin Renting) |   |
|   | consultar en las tablas de la BD correspondientes las placas que han sido devueltas o eliminadas (En estado Fin Renting) filtrando por un cliente | se deben visualizar los registros de las placas con el estado devuelta o eliminada (En estado Fin Renting) |
|   | Ingresar a la web antes de ejecutar el SP de eliminar las placas de las clasificaciones |  se debe visualizar la placa asociada a una clasificación del cliente |
|   |  Consultar el SP y visualizar la placa a eliminar, a continuación lanzar el SP |  se debe eliminar la placa de las clasificaciones del cliente |
|   | ingresar a la web y filtrar por el CLIENTE y desplegar las clasificaciones | no debe mostrar las placas que se encuentran en estado Fin renting |
