#Cloudera_datalake  #bi_ar_bill #detalle_factura 

Tabla de [Detalle de la Facturación]. En [[Cloudera - Datalake]]  se encuentra  [[bi_argentina]] . En [[Redshift]] se encuentra en el esquema [[bi_ar_bill]].

*Para obtener el SQL del emulado de la factura ir a  - > [[facturación (emulación SQL de la factura)]].*

**==campos:==**
	1. nro_mandante =  
	2. doc_impresion = #doc_impresion
	3. pos_doc = 
	4. clase_pos_doc = se joinea con [dp_ar.bt_bi_ar_clase_pos_doc] o con [[bt_ar_belzart_cod_aux_mkt]] La clase del detalle ejemplo: 
		`sum(case when det.clase_pos_doc in ('ZPOTCV') THEN det.cantidad  ELSE 0 END) as potencia_contratada,` 
		`sum(case when det.clase_pos_doc in('ZPOTAD') THEN det.cantidad  ELSE 0 END) as potencia_adquirida`
	1. oper_principal =  con el campo [oper_principal] de la tabla [dp_ar.bt_bi_ar_op_principal_parcial]
	2. oper_parcial = con el campo [oper_parcial] de la tabla [dp_ar.bt_bi_ar_op_principal_parcial] (tienen que usarse ambos campos Si o SI)
	3. ind_imp = 
	4. operacion = 
	5. clase_condicion = 
	6. gr_val_conc_tarifa = 
	7. cod_equipo = material..
	8. cantidad = La cantidad de de la unidad en [clase_pos_doc]
	9. dias = cantidad de días de la unidad representada
	10. unidad_de_medida = 
	11. precio = $
	12. monto =  $
	13. base_recargo = 
	14. nro_cta_contrato = (12 caracteres fijos) [cuenta_contable_recaudacion]
	15. posicion = 
	16. tipo_de_doc = SDDCAMC






