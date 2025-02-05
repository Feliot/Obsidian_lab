#categorias #cliente #cuenta_contrato #cuenta #CDC #rd_ev_account_category_aux_mkt #bt_ar_bi_account_category_aux_mkt

En [[Cloudera - Datalake]] la tabla [rd_ev_account_category_aux_mkt] están dentro del esquema [[bi_argentina]]
***Campos bi_argentina.rd_ev_account_category_aux_mkt:***
1. cod_categoria_cuenta = Se relaciona con [[bt_ar_bi_isu_cliente_medidor_historico]] campo [categoria_cuenta]
2. denominacion_categoria = ejemplos ( "Privado Residencial" , "Privado Comercial Peajes" ,  "Luz de obra / en construcción" , "Entidad Oficial Municipal")
4. segmento = #B2C o #B2B
5. fecha_ultima_actualizacion_tabla, 
6. rol_ultima_actualizacion_tabla, 
7. fecha_dl_alta, usuario_dl_alta

En [[Redshift]] la tabla se llama[bt_ar_bi_account_category_aux_mkt] y se encuentra dentro del esquema [[dp_ar]]







