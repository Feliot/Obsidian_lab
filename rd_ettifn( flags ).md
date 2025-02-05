#flags #sap_isu #cloudera, #redshift, #rd_ettifn 


Tabla de [Flags] se encuentra en el esquema [[bi_argentina]] en [[Cloudera - Datalake]]. Y en [[Redshift]] se encuentra dentro del esquema [dp_ar] (migrado)

La tabla [bt_ar_bi_rd_ettifn] se encuentra en [[Redshift]]  dentro del esquema [bi_ar_bill].


Pero la tabla raiz se llama [rd_ettifn] (Tabla principal, donde están todos los flags) está en [[Cloudera - Datalake]] dentro del esquema [bi_argentina] 

``` SQL 
Electrodependiente -> operando "FLAGVIP" en la tabla ETTIFN con filtro STRING3 = 'X' y campo AB = Desde y BIS = Hasta la fecha que busques

Bomberos -> operando "FLAGBOMV" en la tabla ETTIFN con filtro STRING3 = 'X' y campo AB = Desde y BIS = Hasta la fecha que busques

Club de barrios-> operando "FLAGCLUB" en la tabla ETTIFN con filtro STRING3 = 'X' y campo AB = Desde y BIS = Hasta la fecha que busques

Entidad bien publico-> operando "FLAGEBP" en la tabla ETTIFN con filtro STRING3 = 'X' y campo AB = Desde y BIS = Hasta la fecha que busques
```


***Campos***:
mandt = '100' debe ser
anlage = [Numero de Suministro] #suministro o [Instalación] #instalacion String(10) (IMPORTANTE! cast(anlage as integer) para comparar)
operand = String con el nombre del Operando
ab = Desde vigencia "YYYYMMDD"
ab = Hasta vigencia "YYYYMMDD"
wert1 = Valor del Operando [Valor]
wert1 = '' String
belnr = [Documento de Calculo], se puede relacionar con #doc_calculo de la tabla [[bt_ar_bi_isu_yfac_billing_hdr (Cálculo)