#cliente  #activo #bi_argentina #Cloudera_datalake 
Tabla de Clientes activos dentro del esquema [[bi_argentina]] en [[Cloudera - Datalake]]
Tiene que cumplir con todas las condiciones de activo.

### Campos
1. contrato = 
2. contrato_existente = #numero_cliente 
3. fecha_crea_contrato = 
4. fecha_alta = 
5. fecha_baja = 
6. instalacion = 
7. motivo_bloq_lect = 
8. pod = #point_of_delibery 
9. fecha_crea_instalacion = 
10. fecha_fin_eanlh = 
11. tipo_tarifa = 
12. unidad_lectura = 
13. plan_isu = 
14. fecha_fin_eastl = 
15. numero_equipo = 
16. fecha_fin_egerr = 
17. grupo_num = #grupo_num ,  se puede ver si tiene medidor electrónico,  se llama #grupo_numeradores en [[bt_ar_bi_isu_cliente_medidor_historico]]
    para ser un medidor electrónico : 
	    `grupo_numeradores in ('GRNUM01', 'GRNUM03', 'GRNUM04', 'GRNUM05', 'LECDER02')`
18. grupo_numeradores = #grupo_numeradores campo #repetido igual que el campo anterior #grupo_num (contiene la misma info)
19. material = 
20. numero_serie = 
21. fecha_crea_medidor = 
22. fecha_montaje = 
23. fecha_desmontaje = 
24. numerador = 
25. fecha_fin_etdz = 
26. identificar_num = 
27. factor_mult = 
28. enteros = 
29. decimales = 
30. grupo_aut_ever = 
31. bloqueo_calculo = 
32. i_marca_modelo = 
33. numero_contrato = 
34. ramo = 
35. identificado_debito_automatico = 
36. tipo_debito_aut = 
37. cod_estado_cobrabilidad = 
38. estado_cobrabilidad = 
39. tipo_reparto = #reparto  = {1 = 'Postal' ; 2= 'normal'; 3= 'Braile'; 4= 'Digital'} 
40. zz_fattura_email = 
41. coprc = 
42. zzcta_agr_res = 
43. email1 = 
44. email2 = 
45. email3 = 
