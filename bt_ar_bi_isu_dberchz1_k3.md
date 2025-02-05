#energia_reactiva #bt_ar_bi_isu_dberchz1
[bt_ar_bi_isu_dberchz1_k3] se encuentra en el esquema [[bi_argentina]]  en [[Cloudera - Datalake]]. 
Y en [[Redshift]] se migró al esquema [[dp_ar]] .
Es una tabla creada a partir de [bt_ar_bi_isu_dberchz1] Se generó por la baja performance de dicha tabla que se genera por la cantidad de millones de registros.

Agosto 2024 se crea [bi_ar_bill.bt_ar_bi_isu_dberchz1_k3]

``` SQL
select belnr, sum((cast(v_abrmenge as decimal(20,2))  + cast(n_abrmenge as decimal(20,2)) )) as E_reactiva 
						from dp_ar.bt_ar_bi_isu_dberchz1_k3 --bi_argentina.rd_dberchz1 
						where mandt = '100' and  massbill = 'K3' 
						and tarifnr != 'T2-BFP' group by belnr
```

