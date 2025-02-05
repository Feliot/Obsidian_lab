~~#bi_argentina~~  #bi_ar_bill  #tabla21 #clientes_activos #bt_ar_bi_clientes_dis_geografica #cloudera [`bi_ar_bill.bt_ar_enre_periodo_clientes_activos`]

Tabla de #clientes_activos dentro del esquema [[bi_ar_bill]]  (no tomar la tabla en el esquema ~~[[bi_argentina]]~~ , dado que está generada por error) en [[Cloudera - Datalake]] .
En [[Redshift]] está dentro del esquema [bi_ar_bill]
Es la tabla 21 que va al ENRE. se ejecuta dos veces al mes.

Contiene internamente la dirección normalizada que viene de (`bi_ar_bill.bt_ar_bi_clientes_dis_geografica` [[bt_ar_bi_clientes_dis_geografica]] ).



==**Campos en [Cloudera]:**==
1. id_usuar= #cliente o #numero_cliente
2. tarifa= #tarifa categorizadas en [[Tarifas]]
3. sucursal= #sucursal
4. nro_medidor= #numero_medidor
5. concatenado= #marca_medidor + "-" + #tipo_medidor 
6. marca_medidor= #marca_medidor
7. tipo_medidor= #tipo_medidor
8. f_colocacion= 
9. nombre= #nombre_clientes
10. calle= #calle
11. numero= #altura
12. piso= #piso
13. depto= #departamento
14. telefono= 
15. cod_postal= #codigo_postal
16. localidad= #localidad
17. tension= 
18. potencia_fuera_de_punta= 
19. potencia_de_punta_contratada= 
20. plan_fact= #plan
21. suministro= #nro_suministro
22. partido= #partid
23. barrio= #barrio
24. comuna= 
25. electro_sensible= 
26. coord_x= #coordenada_x en [[Coordenadas_x_y]]
27. coord_y= #coordenada_y en [[Coordenadas_x_y]]
28. alim_mt= 
29. cen_mtbt= 
30. tarifa_social= 
31. ebp= 
32. suministro_comunitario= 
33. tipo_doc= 
34. nro_doc= #DNI
35. cuit_cuil= 
36. prepago= 
37. generacion_distribuida= 
38. latitud= #latitud en [[Latitud y Longitud]]
39. longitud= #longitud  en [[Latitud y Longitud]]
40. actividad_economica= #ramo
41. usuario_alta= 
42. fecha_alta= 
43. periodo_reporte




