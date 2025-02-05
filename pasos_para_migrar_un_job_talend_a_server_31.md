
Estos son los pasos acordados (normalizados) para migrar un job talend al [server31]

1. Primero copiar el .zip al directorio de [WinSCP] = `/ingesta/everis/scripts/argentina/talend_zips`
2.  hacer un `unzip` [namejob.zip] -d `/ingesta/everis/scripts/argentina/talend_jobs + [nombre job]` (ejemplo Trafico_0800).
*IMPORTANTE!*
 Al escribir el nombre del job en el parametro '-d' , se crea automáticamente dicha carpeta que contiene el todas las capetas internas de java. Esto se hace para que no se pisen en cada proyecto.

3. El job estaria listo para procesar.  Se puede configurar el crontab para automatizar su ejecución  (IMPORTANTE! tomar todos los recaudos para no afectar tareas ya automatizadas).
4. `chmod +x /ruta/al/archivo.sh`
   
