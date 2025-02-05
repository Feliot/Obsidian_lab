#shell #linux


proceso de Facturación en : usrtaskauto@srvpg01:/fs/datosmig/java17_jobs/Proceso_Facturacion> ./run_both.sh
``` shell
#!/bin/bash

# Run in the background
nohup bash -c '
    # Execute
    /fs/datosmig/java17_jobs/Proceso_Facturacion/Proceso_Facturacion_run.sh > /fs/datosmig/java17_jobs/Proceso_Facturacion/Proceso_Facturacion_run.log 2>&1
    exit_code=$?

    # Check successful execution
    if [ $exit_code -ne 0 ]; then
        exit $exit_code
    fi

    # Wait for 3 seconds
    sleep 3

    # Execute
    /fs/datosmig/java17_jobs/Proceso_Facturacion_2/Proceso_Facturacion_2_run.sh > /fs/datosmig/java17_jobs/Proceso_Facturacion_2/Proceso_Facturacion_2_run.log 2>&1
    exit_code=$?

    # Check successful execution
    if [ $exit_code -ne 0 ]; then
        exit $exit_code
    fi
' </dev/null >/dev/null 2>&1 &
~
```

#### **Guardar la hora y el minuto en un archivo**

``` shell
 date '+%H:%M' >> /ingesta/everis/scripts/argentina/talend_jobs/Trafico_0800/Trafico_0800/hora_y_minuto.txt
*/2 * * * * /ingesta/everis/scripts/argentina/talend_jobs/Trafico_0800/Trafico_0800/hola.sh
```
