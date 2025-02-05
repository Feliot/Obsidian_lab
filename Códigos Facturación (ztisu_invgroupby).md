#codigos_facturacion #sap_isu #redshift 


Columnas que debe contener el File de [[SAP]] para importar en [[Redshift]] dentro del esquema [[dp_ar]] en la tabla [bt_bi_ar_ztisu_invgroupby] (se sacan espacios " ", acentos "´" y puntos "."):

| grupo_autorizaciones | tipo_de_tarifa | formulario_de_aplicacion | numero_de_box | operacion_principal | operacion_parcial | clase_pos_doc | indicador_impuestos | operacion | clase_de_condicion | rr_val_concr_tarifa | campo_cantidad | indicador | codigo_del_grupo | descripcion_del_grupo |     |
| -------------------- | -------------- | ------------------------ | ------------- | ------------------- | ----------------- | ------------- | ------------------- | --------- | ------------------ | ------------------- | -------------- | --------- | ---------------- | --------------------- | --- |

Como es la tabla [bt_bi_ar_ztisu_invgroupby] :

| **CAMPO**                | **TIPO**      |
| ------------------------ | ------------- |
| grupo_autorizaciones     | VARCHAR(6) ,  |
| tipo_de_tarifa           | VARCHAR(6) ,  |
| formulario_de_aplicacion | VARCHAR(20) , |
| numero_de_box            | VARCHAR(6) ,  |
| operacion_principal      | VARCHAR(6) ,  |
| operacion_parcial        | VARCHAR(6) ,  |
| clase_pos_doc            | VARCHAR(10) , |
| indicador_impuestos      | VARCHAR(6) ,  |
| operacion                | VARCHAR(6) ,  |
| clase_de_condicion       | VARCHAR(6) ,  |
| rr_val_concr_tarifa      | VARCHAR(20) , |
| campo_cantidad           | VARCHAR(20) , |
| indicador                | VARCHAR(2) ,  |
| codigo_del_grupo         | VARCHAR(6) ,  |
| descripcion_del_grupo    | VARCHAR(50)   |

