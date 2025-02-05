#usuarios #users

En [[Cloudera - Datalake]] dentro del esquema [[bi_argentina]] se encuentra una tabla de usuarios RD:
``` SQL
select * from bi_argentina.rd_user_addr 
where bname = 'AR-DNI'
```

Desde [Consola] se puede ejecutar el siguiente script:

``` bash
net user /domain  ar92360035
```
