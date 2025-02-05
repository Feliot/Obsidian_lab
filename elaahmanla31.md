#linux #server31 #elaahmanla31
Servidor Linux 
### Credenciales
server = elaahmanla31.enelint.global  [Server 31]
user = ingesta
pwd = wCgTU!26
path = /ingesta/everis/scripts/argentina/


30 23 * * 0 /ingesta/everis/scripts/argentina/mirror_rd_ettifn/mirroring.sh > /ingesta/everis/scripts/argentina/mirror_rd_ettifn/mirroring.log
31 11 * * * /ingesta/everis/scripts/argentina/talend_jobs/Trafico_0800/Trafico_0800/Trafico_0800_run.sh > /ingesta/everis/scripts/argentina/talend_jobs/Trafico_0800/Trafico_0800/Trafico_0800_run.log
* * * * * /ingesta/everis/scripts/argentina/talend_jobs/Trafico_0800/Trafico_0800/hola.sh

 ver [[Shells (linux)]]
#### **Guardar la hora y el minuto en un archivo**
 date '+%H:%M' >> /ingesta/everis/scripts/argentina/talend_jobs/Trafico_0800/Trafico_0800/hora_y_minuto.txt
*/2 * * * * /ingesta/everis/scripts/argentina/talend_jobs/Trafico_0800/Trafico_0800/hola.sh
