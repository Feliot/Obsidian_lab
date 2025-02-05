#ci #cloudera #redshift

# Mirroring Cloudera - Redshift

A veces, cuando las tablas que se quieren cargar a Redshift desde un Talend local son muy grandes (ej: tablas Enre 21 >600mb / 2.6M filas), es "mas rapido" usar un proceso de GDS que puede copiar una tabla gigante desde Cloudera hasta Redshift.

Este proceso se usa para las copias de las tablas de facturacion, de SAP ISU, de Salesforce... etc. Es un coso auditado y que GDS desarrolla junto con Operaciones, desde Chile.

El problema? Que ahora hay que cargar la tabla en Cloudera, en vez de Redshift, primero. Y despues llamar al proceso para hacer el mirror a Redshift.


### Credenciales
server = elaahmanla31.enelint.global [[elaahmanla31]]
user = ingesta
pwd = wCgTU!26
path = /ingesta/everis/scripts/argentina/


### Pasos (alto nivel)
1. tener el archivo origen pesado (csv, txt, etc)
	- notar el separador
	- formato = utf-8
2. [Cloudera] CREATE TABLE origen
	- schema = bi_argentina_uat
	- formato = todos STRINGs
	- path_HDFS = /data/user/hive/warehouse/{{schema}}.db/tabla
3. [Redshift] CREATE TABLE destino
	- schema = dp_ar
	- formato = todos VARCHAR(256), o los chars que necesite
	- misma cantidad + nombre de campos
4. [WinSCP] subir el .txt a elaahmanla31/{{path}}/input
5. [elaahmanla31] copiar el txt en la tabla de Cloudera
6. [Redshift] INSERT linea a config.redshift_ingest_process
7. [elaahmanla31] ejecutar script de mirroring
	- Main_Standard_Ingesta_Cloud_Redshift_run.sh


### 2. [Cloudera] CREATE TABLE origen
No se puede ejecutar DDL desde DBeaver en Cloudera.
Para ejecutar SQL en Cloudera, desde elaahmanla31 (en Putty):

```sh
hive -e "{{ sql_script }}"
```

El script SQL va adentro de las comillas, y no puede tener Enters

Ejemplo creando tabla 14 del ENRE:

```sh
hive -e "TRUNCATE TABLE bi_argentina_uat.test_st14"

hive -e "CREATE TABLE bi_argentina_uat.test_st14 (
id_usuar STRING,
nro_de_solicitud STRING,
...
pot_maxima_solicitada STRING
)
ROW FORMAT SERDE
  'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'
WITH SERDEPROPERTIES (
  'field.delim'='|',
  'serialization.format'='|')
STORED AS INPUTFORMAT
  'org.apache.hadoop.mapred.TextInputFormat'
OUTPUTFORMAT
  'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
LOCATION
  '/data/user/hive/warehouse/bi_argentina_uat.db/test_st14'
;"
```

Adentro del script:
- `field.delim=|` y `serialization.format=|`: aca va el delimitador del csv origen
- `/data/user/hive/warehouse/bi_argentina_uat.db/test_st14`: es el {{ path_HDFS }} real de la tabla creada. Usa el mismo nombre de la tabla. El nombre del schema lleva ".db" al final


### 3. [Redshift] CREATE TABLE destino
```sql
CREATE TABLE IF NOT EXISTS dp_ar.test_st14 (
id_usuar VARCHAR(255),
nro_de_solicitud VARCHAR(255),
--...
pot_maxima_solicitada VARCHAR(255)
)
DISTSTYLE AUTO;
```

Para checkear en DBeaver:

```sql
INVALIDATE METADATA bi_argentina_uat.test_st14;

SELECT COUNT(*) FROM bi_argentina_uat.test_st14;
```


### 4. [WinSCP] subir el .txt a elaahmanla31/{{path}}/input
Hay que usar el string de conexion que sale de Safeguard, S0007713 como URL para conectarse al server y poder mover el archivo.

Sin usuario, con el password del user "ingesta".

No existe forma de poder guardar la conexion, porque el string cambia cada vez que se copia =/.

A Filezilla no le gusta el string, al rato desconecta. WinSCP funciona bien.


### 5. [elaahmanla31] copiar el txt en la tabla de Cloudera
Esto se ejecuta desde elaahmanla31 (en Putty)

```sh
hdfs dfs -put /ingesta/everis/scripts/argentina/input/ST14.txt /data/user/hive/warehouse/bi_argentina_uat.db/test_st14/
```

Usas el {{ path_HDFS }} usado en el final del paso 2 (agregando la ultima / al final)


### 6. [Redshift] INSERT linea a config.redshift_ingest_process
Aca, el campo `process_name` no necesariamente tiene que ser igual al nombre de la tabla (aunque lo hace mas ordenado). Es solamente el nombre del proceso que va a usar el Talend para hacer el mirroring de la tabla.

No hace falta que se haga un INSERT por cada ejecucion, se define el trabajo la primera vez nomas.

```sql
INSERT INTO config.redshift_ingest_process (
process_name,       -- se puede modificar
dl_schema,          -- schema datalake
dl_table,           -- tabla datalake
dl_hdfs,            -- no es necesario, pero solia ser el path LOCATION
rds_schema,         -- schema redshift
rds_table,          -- tabla redshift
ing_type,
rds_table_type,     -- TEXT / ORC
is_table_partition, -- lo usa GDS para subir tablas grandes en particiones
pais                -- lo usa Operaciones (Global) para sus tableros
) VALUES (
'test_st14',
'bi_argentina_uat',
'test_st14',
'', --'/data/user/hive/warehouse/bi_argentina_uat.db/test_st14',
'dp_ar',
'test_st14',
'FULL',
'ORC',
false,
'arg'
);
```

Aca se necesita que GDS ejecute el INSERT. No tenemos permisos sobre el schema __config__

Para pedir permisos / pedir que Operaciones haga el INSERT, hablar con Rodrigo Morales.

La misma persona da el link para el pedido automático
( #oneclick **Solicitudes de soporte a Aplicaciones LATAM** ) :
https://enel.service-now.com/sp?id=new_global_sc_item_details&sys_id=3e6e766c4f686a0064c83c728110c707&clv=no 

Business Application = GLOBAL CUSTOMER DATA PLATFORM - LT BI
ENTONO = Producción
COMENTARIOS =
```
Buen día.
Para la creación de un tablero para Retail Argentina, necesitamos copiar una tabla consultada desde Datalake (en Cloudera) hacia Redshift (database = dredprodbiba)
Hemos creado la tabla destino en Redshift, y necesitamos ejecutar el archivo SQL adjunto, para crear un registro en la tabla "redshift_ingest_process", en el schema "config" de Redshift, que permite ejecutar el proceso.

He agregado la extension txt al archivo, ya que Service-Now no permite subir archivos sql.
Desde ya, muchas gracias.
```

ADJUNTOS = [redshift_ingest_process.sql] se cambia extensión a ".TXT" para que deje subir el file

### 7. [elaahmanla31] ejecutar script de mirroring
Esto lo pueden ejecutar desde AMS (Jen > Daniela Zamora Soto)

Mentira, lo podemos ejecutar nosotros, cuando muevan el SH al server

El comando real, ya fue movido al server:

```sh
sh /ingesta/everis/scripts/argentina/talend_jobs/Main_Standard_Ingesta_Cloud_Redshift_2_0/Main_Standard_Ingesta_Cloud_Redshift/Main_Standard_Ingesta_Cloud_Redshift_run.sh --context_param local_mode=true --context_param process_name="test_st21" --context_param queue="root.enel.critical.ar"
```

- parametro `process_name`: el nombre del proceso elegido en la tabla `config.redshift_ingest_process`
- parametro `queue`: critical = inmediato, sino normal

Los logs de las ejecuciones quedan en: `/ingesta/everis/scripts/argentina/talend_logs/yyyyMMdd`

Se crean automaticamente las carpetas y archivos logs por cada fecha/process_name

#### Main_Standard_Ingesta_Cloud_Redshift_run.sh
```sh
#!/bin/sh
export FECHA=`date '+%Y%m%d'`
export DIR_LOG=/ingesta/everis/scripts/argentina/talend_logs
export DIR_LOG_FECHA=$DIR_LOG/$FECHA
#Comprobar si existe el directorio con la fecha a la que se crean los logs
if [ -d $DIR_LOG/$FECHA ];
then
export DIR_LOG_FECHA=$DIR_LOG/$FECHA
else
`mkdir $DIR_LOG/$FECHA`
export DIR_LOG_FECHA=$DIR_LOG/$FECHA
fi
#obtiene process_name
for ((i=1;i<=$#;i++));
do
    if [[ ${!i} =~ process_name*. ]];
    then
        process_name=$(echo ${!i} | cut -d "=" -f 2)
    fi
done;
export LOG_FILE=$DIR_LOG_FECHA/$(echo ${0##*/} | cut -d '.' -f 1)_$process_name.log
cd `dirname $0`
ROOT_PATH=`pwd`

java -Dtalend.component.manager.m2.repository=$ROOT_PATH/../lib -Xms256M -Xmx1024M -cp .:
$ROOT_PATH:
$ROOT_PATH/../lib/routines.jar:
$ROOT_PATH/../lib/log4j-slf4j-impl-2.17.1.jar:
$ROOT_PATH/../lib/log4j-api-2.17.1.jar:
$ROOT_PATH/../lib/log4j-core-2.17.1.jar:
$ROOT_PATH/../lib/log4j-1.2-api-2.17.1.jar:
$ROOT_PATH/../lib/activation.jar:
$ROOT_PATH/../lib/fst-2.50.jar:
$ROOT_PATH/../lib/javax.servlet-api-3.1.0.jar:
$ROOT_PATH/../lib/twill-api-0.6.0-incubating.jar:
$ROOT_PATH/../lib/validation-api-1.1.0.Final.jar:
$ROOT_PATH/../lib/json-smart-2.4.11.jar:
$ROOT_PATH/../lib/jsch-0.1.54.jar:
$ROOT_PATH/../lib/metrics-json-3.1.5.jar:
$ROOT_PATH/../lib/okhttp-2.7.5.jar:
$ROOT_PATH/../lib/derby-10.14.1.0.jar:
$ROOT_PATH/../lib/snappy-java-1.1.7.1.jar:
$ROOT_PATH/../lib/parquet-hadoop-bundle-1.9.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/jackson-jaxrs-1.9.13.jar:
$ROOT_PATH/../lib/jetty-util-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/websocket-servlet-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/javax.ws.rs-api-2.0.1.jar:
$ROOT_PATH/../lib/htrace-core4-4.2.0-incubating.jar:
$ROOT_PATH/../lib/tephra-core-0.6.0.jar:
$ROOT_PATH/../lib/hive-llap-client-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/hive-shims-common-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/kerb-server-1.0.0.jar:
$ROOT_PATH/../lib/guice-4.0.jar:
$ROOT_PATH/../lib/netty-all-4.1.17.Final.jar:
$ROOT_PATH/../lib/hive-service-rpc-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/okio-1.6.0.jar:
$ROOT_PATH/../lib/hive-shims-0.23-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/jdo-api-3.0.1.jar:
$ROOT_PATH/../lib/paranamer-2.8.jar:
$ROOT_PATH/../lib/kerby-config-1.0.0.jar:
$ROOT_PATH/../lib/twill-core-0.6.0-incubating.jar:
$ROOT_PATH/../lib/websocket-api-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/audit-common-1.16.1.jar:
$ROOT_PATH/../lib/kerby-asn1-1.0.0.jar:
$ROOT_PATH/../lib/httpclient-4.5.3.jar:
$ROOT_PATH/../lib/curator-recipes-4.0.0.jar:
$ROOT_PATH/../lib/hive-llap-tez-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/hive-common-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/jetty-io-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/hadoop-yarn-server-web-proxy-3.0.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/hive-llap-server-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/twill-discovery-api-0.6.0-incubating.jar:
$ROOT_PATH/../lib/jasper-compiler-5.5.23.jar:
$ROOT_PATH/../lib/jackson-jaxrs-base-2.9.5.jar:
$ROOT_PATH/../lib/websocket-common-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/woodstox-core-5.0.3.jar:
$ROOT_PATH/../lib/commons-codec-1.11.jar:
$ROOT_PATH/../lib/kerb-identity-1.0.0.jar:
$ROOT_PATH/../lib/jersey-guava-2.25.1.jar:
$ROOT_PATH/../lib/ant-1.6.5.jar:
$ROOT_PATH/../lib/accessors-smart-2.4.11.jar:
$ROOT_PATH/../lib/javolution-5.5.1.jar:
$ROOT_PATH/../lib/hive-classification-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/slider-core-0.90.2-incubating.jar:
$ROOT_PATH/../lib/logging-event-layout-1.16.1.jar:
$ROOT_PATH/../lib/hbase-client-2.1.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/hive-service-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/jetty-util-6.1.26.cloudera.4.jar:
$ROOT_PATH/../lib/hadoop-yarn-common-3.0.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/kerb-client-1.0.0.jar:
$ROOT_PATH/../lib/hive-orc-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/hadoop-yarn-registry-2.7.1.jar:
$ROOT_PATH/../lib/datanucleus-api-jdo-4.2.1.jar:
$ROOT_PATH/../lib/hadoop-yarn-server-resourcemanager-3.0.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/hbase-shaded-protobuf-2.1.0.jar:
$ROOT_PATH/../lib/guice-assistedinject-3.0.jar:
$ROOT_PATH/../lib/jackson-core-2.9.5.jar:
$ROOT_PATH/../lib/jersey-common-2.25.1.jar:
$ROOT_PATH/../lib/jersey-client-2.25.1.jar:
$ROOT_PATH/../lib/hadoop-client-3.0.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/commons-pool2-2.9.0.jar:
$ROOT_PATH/../lib/hbase-http-2.1.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/jetty-runner-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/commons-math3-3.6.1.jar:
$ROOT_PATH/../lib/HikariCP-java7-2.4.12.jar:
$ROOT_PATH/../lib/jetty-rewrite-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/hbase-common-2.1.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/libthrift-0.9.3.jar:
$ROOT_PATH/../lib/ant-1.9.1.jar:
$ROOT_PATH/../lib/ant-launcher-1.9.1.jar:
$ROOT_PATH/../lib/nimbus-jose-jwt-4.41.1.jar:
$ROOT_PATH/../lib/guava-14.0.1.jar:
$ROOT_PATH/../lib/commons-lang3-3.10.jar:
$ROOT_PATH/../lib/geronimo-jcache_1.0_spec-1.0-alpha-1.jar:
$ROOT_PATH/../lib/hadoop-hdfs-client-3.0.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/jetty-jndi-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/jackson-mapper-asl-1.9.13-cloudera.1.jar:
$ROOT_PATH/../lib/javax.jdo-3.2.0-m3.jar:
$ROOT_PATH/../lib/jetty-http-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/kerby-util-1.0.0.jar:
$ROOT_PATH/../lib/jcommander-1.30.jar:
$ROOT_PATH/../lib/antlr-runtime-3.5.2.jar:
$ROOT_PATH/../lib/netty-3.10.6.Final.jar:
$ROOT_PATH/../lib/hive-metastore-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/HikariCP-2.6.1.jar:
$ROOT_PATH/../lib/junit-4.12.jar:
$ROOT_PATH/../lib/kerby-xdr-1.0.0.jar:
$ROOT_PATH/../lib/hbase-mapreduce-2.1.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/aopalliance-repackaged-2.5.0-b32.jar:
$ROOT_PATH/../lib/hadoop-auth-3.0.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/jamon-runtime-2.4.1.jar:
$ROOT_PATH/../lib/mssql-jdbc-6.2.1.jre7.jar:
$ROOT_PATH/../lib/dropwizard-metrics-hadoop-metrics2-reporter-0.1.2.jar:
$ROOT_PATH/../lib/hbase-shaded-netty-2.1.0.jar:
$ROOT_PATH/../lib/dom4j-2.1.3.jar:
$ROOT_PATH/../lib/hbase-hadoop-compat-2.1.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/jcodings-1.0.18.jar:
$ROOT_PATH/../lib/gson-2.2.4.jar:
$ROOT_PATH/../lib/hadoop-annotations-3.0.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/avro-1.8.2-cdh6.1.1.jar:
$ROOT_PATH/../lib/hamcrest-core-1.3.jar:
$ROOT_PATH/../lib/osgi-resource-locator-1.0.1.jar:
$ROOT_PATH/../lib/websocket-server-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/jackson-xc-1.9.13.jar:
$ROOT_PATH/../lib/jersey-guice-1.19.jar:
$ROOT_PATH/../lib/asm-9.5.jar:
$ROOT_PATH/../lib/commons-compress-1.19.jar:
$ROOT_PATH/../lib/tephra-api-0.6.0.jar:
$ROOT_PATH/../lib/transaction-api-1.1.jar:
$ROOT_PATH/../lib/hadoop-hdfs-3.0.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/jpam-1.1.jar:
$ROOT_PATH/../lib/jetty-annotations-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/javax.inject-1.jar:
$ROOT_PATH/../lib/hadoop-common-3.0.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/metrics-jvm-3.1.5.jar:
$ROOT_PATH/../lib/hive-shims-scheduler-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/hive-jdbc-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/jettison-1.1.jar:
$ROOT_PATH/../lib/twill-discovery-core-0.6.0-incubating.jar:
$ROOT_PATH/../lib/aopalliance-1.0.jar:
$ROOT_PATH/../lib/commons-logging-1.2.jar:
$ROOT_PATH/../lib/sshd-core-2.9.2.jar:
$ROOT_PATH/../lib/jetty-util-ajax-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/javassist-3.20.0-GA.jar:
$ROOT_PATH/../lib/hbase-procedure-2.1.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/httpcore-4.4.6.jar:
$ROOT_PATH/../lib/ehcache-3.3.1.jar:
$ROOT_PATH/../lib/hive-serde-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/jackson-annotations-2.9.5.jar:
$ROOT_PATH/../lib/re2j-1.1.jar:
$ROOT_PATH/../lib/hbase-protocol-shaded-2.1.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/job-audit-1.5.jar:
$ROOT_PATH/../lib/kerb-util-1.0.0.jar:
$ROOT_PATH/../lib/talend_file_enhanced-1.3.jar:
$ROOT_PATH/../lib/json-20090211.jar:
$ROOT_PATH/../lib/datanucleus-core-4.1.6.jar:
$ROOT_PATH/../lib/jetty-servlet-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/jersey-media-jaxb-2.25.1.jar:
$ROOT_PATH/../lib/libfb303-0.9.3.jar:
$ROOT_PATH/../lib/httpclient-4.5.3.jar:
$ROOT_PATH/../lib/commons-daemon-1.0.13.jar:
$ROOT_PATH/../lib/commons-httpclient-3.1.jar:
$ROOT_PATH/../lib/snappy-0.2.jar:
$ROOT_PATH/../lib/commons-configuration2-2.1.1.jar:
$ROOT_PATH/../lib/ecj-4.4.2.jar:
$ROOT_PATH/../lib/protobuf-java-2.5.0.jar:
$ROOT_PATH/../lib/jersey-server-2.25.1.jar:
$ROOT_PATH/../lib/hbase-metrics-api-2.1.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/hbase-zookeeper-2.1.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/hbase-hadoop2-compat-2.1.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/jackson-core-asl-1.9.13.jar:
$ROOT_PATH/../lib/leveldbjni-all-1.8.jar:
$ROOT_PATH/../lib/asm-commons-5.0.1.jar:
$ROOT_PATH/../lib/fastutil-7.2.1.jar:
$ROOT_PATH/../lib/hive-llap-common-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/findbugs-annotations-1.3.9-1.jar:
$ROOT_PATH/../lib/jasper-runtime-5.5.23.jar:
$ROOT_PATH/../lib/guice-servlet-4.0.jar:
$ROOT_PATH/../lib/jackson-module-jaxb-annotations-2.9.5.jar:
$ROOT_PATH/../lib/commons-lang-2.6.jar:
$ROOT_PATH/../lib/json-io-2.5.1.jar:
$ROOT_PATH/../lib/commons-net-3.1.jar:
$ROOT_PATH/../lib/hk2-utils-2.5.0-b32.jar:
$ROOT_PATH/../lib/hive-exec-shade-2.1.1-cdh6.1.1-talend-1.3.jar:
$ROOT_PATH/../lib/jetty-schemas-3.1.jar:
$ROOT_PATH/../lib/jaxb-api-2.2.12.jar:
$ROOT_PATH/../lib/commons-cli-1.4.jar:
$ROOT_PATH/../lib/slf4j-api-1.7.34.jar:
$ROOT_PATH/../lib/hbase-replication-2.1.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/hive-shims-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/zookeeper-3.4.6.jar:
$ROOT_PATH/../lib/websocket-client-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/redshift-jdbc42-no-awssdk-1.2.55.1083.jar:
$ROOT_PATH/../lib/javax.servlet.jsp-api-2.3.1.jar:
$ROOT_PATH/../lib/apache-jstl-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/hadoop-yarn-api-3.0.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/kerb-core-1.0.0.jar:
$ROOT_PATH/../lib/hadoop-mapreduce-client-jobclient-3.0.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/kerb-common-1.0.0.jar:
$ROOT_PATH/../lib/jetty-6.1.26.cloudera.4.jar:
$ROOT_PATH/../lib/jodd-core-3.5.2.jar:
$ROOT_PATH/../lib/hive-storage-api-2.1.1-cdh6.1.1.jar:
$ROOT_PATH/../lib/crypto-utils-7.0.5.jar:
$ROOT_PATH/../lib/jsr305-3.0.0.jar:
$ROOT_PATH/../lib/jsp-api-2.0.jar:
$ROOT_PATH/../lib/talendcsv-1.1.0.jar:
$ROOT_PATH/../lib/jetty-security-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/twill-common-0.6.0-incubating.jar:
$ROOT_PATH/../lib/hadoop-mapreduce-client-common-3.0.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/jetty-webapp-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/xz-1.6.jar:
$ROOT_PATH/../lib/commons-dbcp-1.4.jar:
$ROOT_PATH/../lib/kerby-pkix-1.0.0.jar:
$ROOT_PATH/../lib/kerb-simplekdc-1.0.0.jar:
$ROOT_PATH/../lib/jetty-jaas-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/java-util-1.9.0.jar:
$ROOT_PATH/../lib/opencsv-2.3.jar:
$ROOT_PATH/../lib/javax.inject-2.5.0-b32.jar:
$ROOT_PATH/../lib/jackson-jaxrs-json-provider-2.9.5.jar:
$ROOT_PATH/../lib/twill-zookeeper-0.6.0-incubating.jar:
$ROOT_PATH/../lib/jcip-annotations-1.0-1.jar:
$ROOT_PATH/../lib/asm-tree-5.0.4.jar:
$ROOT_PATH/../lib/hadoop-yarn-server-common-3.0.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/joni-2.1.11.jar:
$ROOT_PATH/../lib/hadoop-mapreduce-client-core-3.0.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/javax.servlet.jsp-2.3.2.jar:
$ROOT_PATH/../lib/joda-time-2.9.9.jar:
$ROOT_PATH/../lib/jetty-client-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/metrics-core-3.2.1.jar:
$ROOT_PATH/../lib/commons-collections-3.2.2.jar:
$ROOT_PATH/../lib/hbase-protocol-2.1.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/objenesis-2.5.1.jar:
$ROOT_PATH/../lib/disruptor-3.3.6.jar:
$ROOT_PATH/../lib/antlr4-runtime-4.8-1.jar:
$ROOT_PATH/../lib/hk2-api-2.5.0-b32.jar:
$ROOT_PATH/../lib/curator-client-4.0.0.jar:
$ROOT_PATH/../lib/jboss-marshalling-2.0.12.Final.jar:
$ROOT_PATH/../lib/sshd-common-2.9.2.jar:
$ROOT_PATH/../lib/datanucleus-rdbms-4.1.7.jar:
$ROOT_PATH/../lib/hbase-metrics-2.1.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/hadoop-yarn-client-3.0.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/commons-el-1.0.jar:
$ROOT_PATH/../lib/apache-jsp-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/jetty-xml-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/commons-beanutils-1.9.4.jar:
$ROOT_PATH/../lib/parquet-hadoop-bundle-1.6.0.jar:
$ROOT_PATH/../lib/hbase-shaded-miscellaneous-2.1.0.jar:
$ROOT_PATH/../lib/javax.annotation-api-1.2.jar:
$ROOT_PATH/../lib/commons-httpclient-3.1.jar:
$ROOT_PATH/../lib/bonecp-0.8.0.RELEASE.jar:
$ROOT_PATH/../lib/curator-framework-4.0.0.jar:
$ROOT_PATH/../lib/hk2-locator-2.5.0-b32.jar:
$ROOT_PATH/../lib/jsp-api-2.1.jar:
$ROOT_PATH/../lib/audit-log4j2-1.16.1.jar:
$ROOT_PATH/../lib/audience-annotations-0.8.0.jar:
$ROOT_PATH/../lib/jline-2.12.jar:
$ROOT_PATH/../lib/stax2-api-3.1.4.jar:
$ROOT_PATH/../lib/jackson-databind-2.9.5.jar:
$ROOT_PATH/../lib/jta-1.1.jar:
$ROOT_PATH/../lib/taglibs-standard-spec-1.2.5.jar:
$ROOT_PATH/../lib/jersey-container-servlet-core-2.25.1.jar:
$ROOT_PATH/../lib/jetty-plus-9.3.20.v20170531.jar:
$ROOT_PATH/../lib/httpcore-4.4.6.jar:
$ROOT_PATH/../lib/kerb-admin-1.0.0.jar:
$ROOT_PATH/../lib/tephra-hbase-compat-1.0-0.6.0.jar:
$ROOT_PATH/../lib/commons-io-2.6.jar:
$ROOT_PATH/../lib/hbase-server-2.1.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/jakarta.mail-1.6.7.jar:
$ROOT_PATH/../lib/commons-crypto-1.0.0.jar:
$ROOT_PATH/../lib/hadoop-yarn-server-applicationhistoryservice-3.0.0-cdh6.1.1.jar:
$ROOT_PATH/../lib/kerb-crypto-1.0.0.jar:
$ROOT_PATH/../lib/taglibs-standard-impl-1.2.5.jar:
$ROOT_PATH/../lib/jetty-server-9.3.20.v20170531.jar:
$ROOT_PATH/main_standard_ingesta_cloud_redshift_2_0.jar:
$ROOT_PATH/log_standard_ingesta_cloud_redshift_1_1.jar:
$ROOT_PATH/arq_ejec_cmd_1_0.jar:
$ROOT_PATH/standard_ingesta_cloud_redshift_2_0.jar:
gt_tld_automatizacion_ams_8.main_standard_ingesta_cloud_redshift_2_0.Main_Standard_Ingesta_Cloud_Redshift --context=PRD --log4jLevel=Info "$@" 2>> ${LOG_FILE} | tee -a ${LOG_FILE}
```