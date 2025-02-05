#rase #enre #GAM50350 #RASE
La segmentación energética tiene por finalidad ordenar los subsidios a la electricidad y el gas según los aspectos socio-económicos de cada hogar, focalizando los subsidios en quienes más lo necesitan.

Luego se cruza el archivo ENRE_EDESUR.csv  (que comparte Mariela Barbota) con python con el SEGMENTO de cada cliente. 

##### 20241101 - VERIFICACIONES MANUALES :
1  -Todo lo que tenga flag T-Social vigente. ahora pasa como novedad si no es SEG2 en SAP.  
2 - Verificar segmento 2 VENCIDO SAP - Tsocial , también pasa a ser novedad si no es SEG2 en SAP. 
3- Verificar segmento SAP Tsocial, ahora ya no es un ERROR ahora va ser siempre así  
4- Verificar Tarifa GENERAL, sigue igual. no se toman.


Se ejecuta la siguiente SQL desde [[Cloudera - Datalake]]  para descargar el SEGMENTO de cada cliente:
	
```SQL
select cast(r.anlage as integer) suministro, cast(r.vkonto as integer) as cliente, begru, activos.tipo_tarifa, erdat
, n.ab, n.bis, concat(CASE  WHEN n.wert1 is not null THEN 'SEG' else null end, LEFT(n.wert1, 1) ) as segmento 
, case  when tsocial.operand = 'FLAGTIS' then (case when (tsocial.string3 = 'X' and tsocial.bis = '99991231')  then 'tsocial' else 'FLAG desactivado' end) else '' end as t_social --, n.fecha_alta, n.fecha_ult_mod 
, activos.estado_cliente , tsocial.bis
from bi_argentina.rd_ever r
left join
(select  anlage , operand, ab, bis, wert1, wert2, TRIM(string3) as string3, ROW_NUMBER() over(PARTITION by anlage order by bis desc) as fila
from  bi_argentina.rd_ettifn 
where  operand= 'SEGMENTO'  and mandt = '100') AS n
on n.anlage = r.anlage
left join (select  anlage , operand, ab, bis, TRIM(string3) as string3, ROW_NUMBER() over(PARTITION by anlage order by bis desc) as fila
from  bi_argentina.rd_ettifn 
where  operand= 'FLAGTIS' and mandt = '100') as tsocial
on tsocial.anlage = r.anlage and tsocial.fila= 1
left join (select estado_cliente, contrato_existente, tipo_tarifa, row_number() over(PARTITION by contrato_existente order by fecha_baja desc) as fila --estado_cliente, numero_contrato, fecha_modif_instalacion
from  bi_argentina.bt_ar_bi_isu_cliente_medidor_historico ) as activos 
on activos.contrato_existente = cast(r.vkonto as integer)  and activos.fila= 1
WHERE  r.auszdat = '99991231' 
and (n.fila = 1 or n.fila is null);
```