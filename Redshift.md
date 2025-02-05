#redshift #data_warehouse #data_warehousing 
Es un servicio de almacenamiento y análisis de datos en la nube ofrecido por Amazon Web Services (AWS). Se trata de un servicio de [[Data warehousing]] totalmente administrado que permite almacenar y analizar grandes volúmenes de datos de manera rápida y eficiente.

ZpdJHazrgtnGSFU


con = latam-ap35552-prod-rshift-00-biba-58jpjcv7kr96.cu7jkfaatmsm.eu-central-1.redshift.amazonaws.com

  

|     | Desde POWER BI                                                                                                                                                                                 |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|     | Extension{"extensionDataSourceKind":"AmazonRedshift","extensionDataSourcePath":"latam-ap35552-prod-rshift-00-biba-58jpjcv7kr96.cu7jkfaatmsm.eu-central-1.redshift.amazonaws.com;dredprodbiba"} |

base= dredprodbiba

**SQL***:
ver #campos de tablas :
``` SQL
SELECT column_name, data_type, character_maximum_length, numeric_precision, numeric_scale FROM svv_columns WHERE table_schema = 'esquema_de_la_tabla' AND table_name = 'nombre_de_la_tabla';
```


Manejo fechas Redshift se puede tomar como un string
to_char o left funcionan.