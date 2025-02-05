#cloudera #bi_argentina #tabla
Tabla de Clientes activos Histórico dentro del esquema [[bi_argentina]] en [[Cloudera - Datalake]]
 y Dentro del esquema [[bi_ar_cus]] en [[Redshift]].
Se ingesta a la madrugada todos los días.
#### código para que no se repitan clientes:
` row_number() over(partition by contrato_existente order by fecha_crea_medidor desc) as fila `
Luego filtrar por el campo "fila = 1".

VER = [Declaración de Conformidad de Instalación eléctrica] o #DCI

==**Campos:**==
1. estado_cliente = Estado del #cliente , string : ('Activo' , 'Inactivo')
2. contrato = su contrato
3. contrato_existente = Numero de cliente #cliente
4. fecha_alta_sistema_existente = Fecha alta
5. fecha_crea_contrato = Fecha
6. fecha_alta = Fecha
7. fecha_baja = Fecha
8. grupo_aut_ever = #tipo_cliente (T1, '' ,T3, T2) . El ('') solo tiene 1 cliente 
9. bloqueo_calculo = 
10. grverifaparcal = (ZT1RESID, ZT1RESID, ZT2, ..)
11. imputacion_co = 
12. lugar_comercial = 
13. numero_contrato = 
14. denomctacontr = 
15. clave_intereses = 
16. procedimiento_reclamacion = 
17. clase_impuesto = 
18. region_cuenta_contrato = 
19. codigo_condado = 
20. categoria_cuenta =  se relaciona con [bt_ar_bi_account_category_aux_mkt]
    `AP  T1 | Alumbrado Publico`  
	`C1  T1 | Privado Comercial`  
	`C2  Privado Comercial`  
	`C3  Privado Comercial`  
	`CO  T1 | Cons.Prop.`  
	`E1  T1 | Clientes Eventuales`  
	`EX  Clientes Eventuales`  
	`I1  T1 | Privado Industrial`  
	`I2  Privado Industrial`  
	`I3  Privado Industrial`  
	`LO  T1 | Luz de Obra`  
	`LR  T1 | Luz de Riego`  
	`LS  T1 | PIMT ENTIDADES S/FINES LUCRO`  
	`M1  T1 | Entidad Oficial Municipal`  
	`M2  Entidad Oficial Municipal`  
	`M3  Entidad Oficial Municipal`  
	`N1  T1 | Entidades Oficial Nacional`  
	`N2  Entidad Oficial Nacional`  
	`N3  Entidad Oficial Nacional`  
	`P1  T1 | Entidad Oficial Provincial`  
	`P2  Entidad Oficial Provincial`  
	`P3  Entidad Oficial Provincial`  
	`PC  T1 | PIMT PRIVADO COMERCIAL`  
	`PI  T1 | PIMT PRIVADO INDUSTRIAL`  
	`PJ  T1 | PIMT JUBILADO`  
	`R1  T1 | Privado Residencial`  
	`R2  Privado Residencial`  
	`R3  Privado Residencial`  
	`RP  T1 | Residencial Acuerdo Marco`  
	`S1  T1 | Entidades Sin Fines de Lucro`  
	`S2  Entidades Sin Fines de Lucro`  
	`S3  Entidades Sin FInes de Lucro`
1. grverificapfact =  se puede usar como #tarifa
2. condicion_pago = 
3. clase_determinacion_cuenta = 
4. division = 
5. identificado_debito_automatico = 
6. tipo_debito_aut = 
7. cod_estado_cobrabilidad = 
8. estado_cobrabilidad = 
9. tipo_reparto  #reparto  = {1 = 'Postal' ; 2= 'normal'; 3= 'Braile'; 4= 'Digital'} 
10. zz_fattura_email = 
11. coprc = 
12. bu_sort1 = 
13. bu_sort2 = 
14. name_last = 
15. name_first = 
16. name_org1 =  
17. name_org2 = 
18. name1_text = Este campo tiene el dato completo de #nombre Pero tiene vacíos
19. mc_name1 = tiene #nombre Incompleto pero casi no hay vacíos.
``` SQL	
case when mh.name1_text = '' then mc_name1 else name1_text end nombre_cliente
```
1. interlocutor_comercial = 
2. ic_numero_externo = 
3. dir_poblacion = Es análogo al #partido (con algunos errores) 
4. dir_distrito = 
5. dir_codigo_postal = 
6. dir_calle = 
7. dir_altura = 
8. dir_entrecalle_1 = 
9. dir_entrecalle_2 = 
10. dir_sigla_edificio = 
11. dir_piso = 
12. dir_habitacion = 
13. dir_pais = 
14. dir_region = 
15. dir_poblacion_2 = 
16. dir_mc_calle = 
17. email1 = 
18. email2 = 
19. email3 = 
20. instalacion = Suministro
21. motivo_bloq_lect = 
22. pod = 
23. fecha_crea_instalacion = 
24. punto_suministro = 
25. nivel_tension = 
26. motgarantsumin = 
27. tipo_conexion = 
28. fecha_modif_instalacion = 
29. pod_instalacion = 
30. subestacion = 
31. nombre_subestacion = 
32. alimentador = 
33. centro_transformacion =  #centro_transformacion es la primera parte , luego están las salidas del transformador.
34. fase = 
35. acometida = 
36. tipo_instalacion = 
37. calle_instalacion = 
38. numero_calle_instalacion = 
39. piso_calle_instalacion = 
40. dpto_calle_instalacion = 
41. entrecalle1_instalacion = 
42. entrecalle2_instalacion = 
43. manzana_instalacion = 
44. barrio_instalacion = 
45. localidad_instalacion = 
46. partido_instalacion = 
47. sucursal_instalacion = 
48. coordenada_x = 
49. coordenada_y = 
50. latitud = 
51. longitud = 
52. secuencia_lectura = 
53. fecha_inicio_eanlh = 
54. fecha_fin_eanlh = 
55. tipo_tarifa = (T1-AP-NOM, T2,'',T3AT,T1-GEN-MED, T1-GEN-NOM, T3BT, T3-VILLAS, T3MT, T1-RESIDEN, T1-AP-MED)
56. unidad_lectura = 
57. plan_isu = 
58. codigo_ramo = El código del #ramo
59. numero_logico_aparato = 
60. fecha_inicio_eastl = 
61. fecha_fin_eastl = 
62. numero_equipo = 
63. fecha_inicio_egerr = 
64. fecha_fin_egerr = 
65. grupo_numeradores = #grupo_numeradores , se puede ver si tiene medidor electrónico,  se llama #grupo_num en [[bt_ar_bi_isu_cliente_medidor]]
    para ser un medidor electrónico : 
	    `grupo_numeradores in ('GRNUM01', 'GRNUM03', 'GRNUM04', 'GRNUM05', 'LECDER02')`
1. material = 
2. numero_serie = 
3. fecha_crea_medidor = 
4. fecha_modif_medidor = 
5. fecha_montaje = 
6. fecha_desmontaje = 
7. i_marca_modelo = 
8. numerador = 
9. fecha_inicio_etdz = 
10. fecha_fin_etdz = 
11. factor_mult = 
12. enteros = 
13. decimales = 
14. identificador_numerador = 
15. clase_numerador = 
16. fecha_crea_etdz = [Fecha de creación del contrato] o #fecha_crea_contrato
17. fecha_modif_etdz = 
18. ramo = String del campo [codigo_ramo] o nombre de la [Actividad económica] #ramo 
19. zzcta_agr_res = 
