#cammesa

En Cloudera se usa [[rd_ettifn( flags )]]

``` SQL
--CAMMESA
SELECT
mh.contrato_existente as codigo_suministro, 
case when length(mh.name1_text) < 3 then mh.mc_name1 else mh.name1_text end  as descripcion , 
case when operand = 'FLAGVIP' then '6000'
 when operand = 'FLAGBOMV' then '6000'
 when operand = 'FLAGCLUB' then '6500'
when operand = 'FLAGEBP' then '6500' else '' end as categoria_de_usuario,  
geo.partido , geo.localidad , case when geo.piso = '' then concat(geo.calle, ', ' , geo.numero)
else concat(geo.calle, ', ' , geo.numero, ', ' ,geo.piso, ', ' ,geo.depto) end as  direccion
FROM bi_argentina.rd_ettifn fl --bt_ar_bi_rd_ettifn fl
inner join (SELECT contrato_existente, instalacion , name1_text, mc_name1
			FROM bi_argentina.bt_ar_bi_isu_cliente_medidor_historico 
			where fecha_baja >'20240801' ) mh
on mh.instalacion  = cast(fl.anlage as int)
left join  bi_argentina.bt_ar_bi_clientes_dis_geografica geo
on geo.numero_cliente = mh.contrato_existente
where  operand  in ('FLAGVIP', 'FLAGBOMV', 'FLAGCLUB', 'FLAGEBP')
AND BIS between '20240801' and  '99991231' AND STRING3 = 'X'
and AB <  '20240901' ;
```

