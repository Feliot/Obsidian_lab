#talend #java #linux 

Se ejecuta en [[Linux]]

Cambiar archivos .sh que apunten al java portable:
#!/bin/sh
cd `dirname $0`
ROOT_PATH=`pwd`
/fs/datosmig/Jobs_Talend/jdk1.8.0_231/jre/bin/java -Dtalend.component.manager.m2.repository=$ROOT_PATH/../lib -Xmx4096M 