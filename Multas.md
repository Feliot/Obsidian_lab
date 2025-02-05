

querys:
Área contable:

``` SQL
select cont.*, cod.descripcion
from bi_ar_coll.bt_ar_bi_contable cont
inner join dp_ar.bt_bi_ar_operaciones_multas_contabilidad cod 
on cod.cta_contable = cast(cont.cta_contable as int) 
where cont.fec_contable  between '2024-01-01' and '2024-05-30'
limit 20;
```