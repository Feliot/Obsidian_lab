#Cloudera_datalake #cabecera_factura #bi_ar_bill 
Es la #cabecera_factura . En [[Cloudera - Datalake]]  se encuentra  [[bi_argentina]] . En [[Redshift]] se encuentra en el esquema [[bi_ar_bill]].


**==campos:==**
 1. nro_mandante =  defecto '100'
 2. doc_impresion = *#doc_impresion y se relaciona con [[bt_ar_bi_isu_yfac_invo_det (Detalle)]] y con [[bt_ar_bi_isu_yfac_billing_hdr (Cálculo)]]*
 3. nro_doc_oficial = #factura
 4. clase_doc = #clase_factura ejemplo 'FA'
 5. cta_cto = #cuenta_contrato o #cliente
 6. interlocutor = #interlocutor o #interlocutor_comercial de SAP ISU
 7. porcion = 
 8. fecha_conta = #fecha_conta Es la #fecha_de_emision de la                                                  factura
 9. fecha_impresion = 
 10. primer_vto = 
 11. segundo_vto = 
 12. via_de_pago = 
 13. fecha_anulacion = 
 14. creado_el = 
 15. creado_por = 
 16. sucursal = 
 17. periodo = 
 18. doc_ajusta = 
 19. motivo_ajuste = 
 20. abrvorg =   *Se puede interpretar como el tipo de Doc de Calculo.*
     1. { '01' = 'Cálculo periódico (01)',  
     2. '02' = 'Cálculo intermedio (02)' ,
     3. '03' = 'Cálculo final p.baja de instalación (03)' ,
     4. '04' = 'Cálculo final (04)' ,
     5. '05' = 'Cesión de área (05)' ,
     6. '06' = 'Abono/Recálculo manual (06)' ,
     7. '07' = 'Cambio de contrato (07)' ,
     8. '08' = 'Cambio de deudor (08)' }
     ![[Pasted image 20240619085507.png]]
 1. canal_zstampa = = {1 = 'Postal' ; 2= 'normal'; 3= 'Braile' ; 4= 'Digital'} 





