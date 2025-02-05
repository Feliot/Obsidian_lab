#cloudera #datalake #Cloudera_datalake 
Plataforma de datos que proporciona herramientas y servicios para almacenar, procesar, analizar y visualizar grandes volúmenes de datos.
[Impala]
[Hive]

Particularidades:

``` SQL
--CREAR TABLA DESDE SELECT
create table bi_argentina_uat.tabla as
select cliente FROM (VALUES (23 cliente) , (24) ) AS t ;

--ELIMINO LUEGO:
drop table bi_argentina_uat.bt_ar_bi_clientes_BORRAR;
```


``` SQL
-- ORDENAR VALORES BOOLEANOS
SELECT *
FROM (
    SELECT true AS value
    UNION ALL
    SELECT false AS value
    UNION ALL
    SELECT NULL AS value
) AS values_table
ORDER BY value desc;
/* 
Resultados:
[NULL]
true
false*/
``` 

ejemplo saber columnas tipo de datos y un ejemplo del contenido de la misma.
``` SQL
-- Obtener los nombres de columnas, tipos de datos y un dato de ejemplo por columna en una tabla
SELECT 
    column_name AS Campo, 
    data_type AS TipoDeDato,
    (SELECT column_name FROM nombre_de_tu_tabla LIMIT 1) AS Ejemplo
FROM 
    INFORMATION_SCHEMA.COLUMNS
WHERE 
    table_name = 'nombre_de_tu_tabla';
```

``` SQL 
-- Obtener los nombres de columnas, tipos de datos y un dato de ejemplo por columna en una tabla
DECLARE @table_name VARCHAR(255) = 'nombre_de_tu_tabla';

SELECT 
    column_name AS Campo, 
    data_type AS TipoDeDato,
    (SELECT TOP 1 * FROM @table_name) AS Ejemplo
FROM 
    INFORMATION_SCHEMA.COLUMNS
WHERE 
    table_name = @table_name;
```


#manejo_de_fechas en cloudera:
from_unixtime(unix_timestamp(**CURRENT_TIMESTAMP**()), 'yyyy-MM-dd HH:mm:ss');
conviene mas tratar como string ....

| Cast from   | Cast to     | Result                                                                                                                                                                                                                                                                                                            |
| ----------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `TIMESTAMP` | `DATE`      | The date component of the `TIMESTAMP` is returned, and the time of the day component of the `TIMESTAMP` is ignored.                                                                                                                                                                                               |
| `STRING`    | `DATE`      | The `DATE` value of `yyyy-MM-dd` is returned.<br><br>The `STRING` value must be in the `yyyy-MM-dd` or `yyyy-MM-dd HH:mm:ss.SSSSSSSSS` pattern.<br><br>If the time component is present in `STRING`, it is silently ignored.<br><br>If the `STRING` value does not match the above formats, an error is returned. |
| `DATE`      | `TIMESTAMP` | The year, month, and day of the `DATE` is returned along with the time of day component set to `00:00:00`.                                                                                                                                                                                                        |
| `DATE`      | `STRING`    | The `STRING` value, `'yyyy-MM-dd'`, is returned.                                                                                                                                                                                                                                                                  |


``` SQL
SELECT nro_cliente, nombre, trim(replace(zona, 'ZONA', '')) AS zona, partido, direccion, entre_calle, 
       y_calle, sistema_origen, tarifa, tel_cliente, 
       tel_caller_id, tel_casa, tel_celular, tel_vecino, count(distinct nro_reclamo) as cant_reclamos,
       sum(reiteraciones) as cant_reinteraciones, left(cast( recepcion as string) , 7) as mes_origen
  FROM bi_argentina.bt_ar_bi_reclamos_certa
WHERE nro_cliente is not null and nro_cliente <> ''
group by nro_cliente, nombre, zona, partido, direccion, entre_calle, 
  y_calle, sistema_origen, tarifa, tel_cliente, 
       tel_caller_id, tel_casa, tel_celular, tel_vecino, mes_origen
HAVING count(distinct nro_reclamo) >4;
```