#crontab #produccion #automatitazación_de_tareas

``` bash
"Ver procesos cron"
Crontab -l 
```

Se presentaban (server viejo) los jobs programados:
`*/2 * * * * /ingesta/everis/scripts/argentina/talend_jobs/Trafico_0800/Trafico_0800/hola.sh`
`34 7 * * * /ingesta/everis/scripts/argentina/talend_jobs/Trafico_0800/Trafico_0800/Trafico_0800_run.sh > /ingesta/everis/scripts/argentina/talend_jobs/Trafico_0800/Trafico_0800/Trafico_0800_run.log`
`35 7 * * * /fs/datosmig/talend8_jobs/Trafico_0800/Trafico_0800_run.sh > /fs/datosmig/talend8_jobs/Trafico_0800/Trafico_0800_run.log &`
* `* * * * /fs/datosmig/jobs_productivos/prueba/hola.sh >> /fs/datosmig/jobs_productivos/prueba/log.log 2>&1`
`39 7 * * * /fs/datosmig/talend8_jobs/Call_Center_Genesys/Call_Center_Genesys_run.sh > /fs/datosmig/talend8_jobs/Call_Center_Genesys/Call_Center_Genesys_run.log` 