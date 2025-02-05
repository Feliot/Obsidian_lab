#Cloudera_datalake #bi_argentina #calculo_factura #sap_isu #doc_calculo 
Contiene el Cálculo de una factura, se encuentra en el esquema [[bi_argentina]] dentro de [[Cloudera - Datalake]].  En [[Redshift]] se encuentra en el esquema [[bi_ar_bill]] .

> [!sql] Cloudera
> select * from bi_argentina.bt_ar_bi_isu_yfac_billing_hdr
--Potencia en KW
--Energia en KWH

> [!sql] Redshift
> select * from bi_ar_bill.bt_ar_bi_isu_yfac_billing_hdr
--Potencia en KW
--Energia en KWH

Capos:
 1. nro_mandante = 
 2. doc_calculo = #doc_calculo 
 3. doc_impresion = #doc_impresion
 4. contrato = Contrato SAP
 5. cdc = categoria_cuenta , ejemplo 'T1'
 6. categoria_cuenta =  se relaciona como el CDC con la tabla [[Categorias de Cuentas - Clientes - rd_ev_account_category_aux_mkt]]  (depende si es Redshift o Cloudera er que la tabla cambia de nombre.)
 7. inicio_periodo_calculo = 
 8. fin_periodo_calculo = 
 9. fecha_calculo = #fecha_calculo
 10. adatsoll = 
 11. unidad_de_lectura = #unidad_de_lectura
 12. creado_el = 
 13. creado_por = 
 14. istablart = 
 15. tarifa = La #tarifa del cliente
 16. nro_instalacion = Suministro
 17. zescalon_tarifa = #escalon_tarifa
 18. z_nivel_tarifario = #segmento
 19. ablesart = Tramo facturado
 20. ablhinw = 


