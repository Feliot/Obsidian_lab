#ventas #EV #job #energia #estructura_de_ventas

Este proceso es utilizado para la compra de Energía a CAMMESA
El job de Estructura de ventas se corre dos veces al mes. 
La primer corrida a principio de mes, y luego después del 20 de cada mes con la demanda corregida.

Pasos:
1. Se carga altas y bajas en "dt_activosybajasn".
2. Se eliminan ultimos dos meses de "ventas.t1_reporteventas_historico" . Luego se insertan ultimos dos meses a "ventas.t1_reporteventas_historico" desde "ventas.t1_reporteventas ".
3. Se insertan "resumen_activosybajas" desde "dt_activosybajasn".
4. Se suben los Tarifa Social a "t1_cliente_tis".
5. Se suben Electrodependiente a "t1_cliente_eld".
6. Se sube la demanda en "demanda_diaria_carga".
7. Se actualiza o se inserta, según dependa "demanda_diaria" desde "demanda_diaria_carga".
8. Se trunca "tmp_t1_factura_res602".
9. Se carga FAC DIA en "tmp_t1_factura_res602".
10. DELETE FROM "parametro_periodo_actual".
11. INSERT INTO "parametro_periodo_actual" (periodo) SELECT MAX(periodo) FROM "tmp_t1_factura_res602".
12. Update "parametro_periodo_actual".
13. Se borra el ultimo periodo de "t1_factura_res602".
14. Se actualiza la subtarifa de la tabla "t1_factura_res602".
15. Insert "t1_factura_res602" desde "tmp_t1_factura_res602".
16. truncate table "tmp_t1_consumos_convenidos".
17. Insert "tmp_t1_consumos_convenidos" desde FACTURACIÓN.
18. truncate "tmp_t1_factura_res602" y Elimina el Indice.
19. Insert "tmp_t1_factura_res602" desde "t1_factura_res602". Y crea Indice.
20. truncate "t1_cliente_res602"  y drop table if exists "tmp_ultimo_consumo". Luego insert "tmp_ultimo_consumo" desde " tmp_t1_factura_res602". Despues Insert "t1_cliente_res602" desde "tmp_t1_factura_res602" inner "tmp_ultimo_consumo".
21. Elimina y luego inserta "t1_reporteventas_fotomes" desde "t1_reporteventas".
22. Genera "t1_reporteVentas" Facturado.
23. Genera "t1_reporteVentas" Pendiente.
24. Genera "t1_reporteVentas" Leido No Facturado.
25. Truncate "tmp_t1_cantidadclientes".
26.  insert "tmp_t1_cantidadclientes" Facturado
27.  insert "tmp_t1_cantidadclientes" Pendiente
28. insert "tmp_t1_cantidadclientes" Leido No Facturado.
29. Delete e Insert  "t1_cantidadclientes" desde "tmp_t1_cantidadclientes".
30. Insert "t1_resumen_clientes_facturados" desde "t1_factura_res602"
31. FIN.

Tramos:
1. Factura 1 , corresponde al Tramo 1 o "Real"= Se emite inmediatamente después de la lectura y corresponde al periodo  de lectura comprendido entre la "Fecha de lectura anterior" y la "Fecha media" del periodo de lectura.
2. Factura 2 , corresponde al Tramo 2 o "Ficticia" = Se emite 30 días después de la factura 1, y corresponde al periodo comprendido entre la "fecha media del periodo" y "fecha de lectura actual".

Estimación de Energía pendiente (EP) por tramo mensual (Calculo market)

Variación de energía pendiente: El desvió que se introduce en el modelo al estimar la EP.


Cuadro Tarifario Febrero 2024:

| Tarifa | Subtarifa | Categoría Consumo EDS (kWh/mes) | Rango | Cod.R93       | Consumo Res_ENRE 101-2024 | Bloques Res_ENRE 101-2024 |
| ------ | --------- | ------------------------------- | ----- | ------------- | ------------------------- | ------------------------- |
| T1     | R1        | 0-150                           | UR1   | Art 19 a) I   | R1                        | 0-150                     |
| T1     | R2        | 151-325                         | UR2   | Art 19 a) II  | R2                        | 151-400                   |
| T1     | R3        | 326-400                         | UR2   | Art 19 a) II  | R2                        | 151-400                   |
| T1     | R4        | 401-450                         | UR2   | Art 19 a) II  | R3                        | 401-600                   |
| T1     | R5        | 451-500                         | UR2   | Art 19 a) II  | R3                        | 401-600                   |
| T1     | R6        | 501-600                         | UR3   | Art 19 a) III | R3                        | 401-600                   |
| T1     | R7        | 601-700                         | UR3   | Art 19 a) III | R4                        | +600                      |
| T1     | R8        | 701-1400                        | UR4   | Art 19 a) IV  | R4                        | +600                      |
| T1     | R9        | +1400                           | UR5   | Art 19 a) V   | R4                        | +600                      |




A raiz de una necesidad , segmentos de demanda, 
reporte de ventas. EBP, electro dependiente, villas  (tienen alguna bonificación o servicio gratuito)
DIAS PER
Consumo
Cammesa pide el cliente a Cliente.
