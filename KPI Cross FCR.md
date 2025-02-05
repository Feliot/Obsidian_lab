#martin #KPI #FCR 


![[KPI FCR.png]]

Los números que debería obtener son:
- Q clientes con casos (denominador)
- Q clientes con recontacto dentro de los 7 días calendario (a contar desde el primer contacto)
- Q clientes con recontacto por el mismo submotivo dentro de los 7 días calendario (a contar desde el primer contacto)
- Q clientes activos
- Q contactos del período (casos)

Criterios:
- Desestimar casos cancelados
- Incluir criterios utilizados para tableros habituales

Te paso un primer intento de query que hice yo (no llegué a considerar el submotivo para el 2° KPI pero no llegué a buen puerto):

``` SQL

WITH _initial_contacts_ AS (

    SELECT

        cliente,

        MIN(cast(fechaapertura as DATE)) AS _fecha_inicial_

    FROM

        bi_ar_att.bt_ar_bi_casos_vw

    where cast(fechaapertura as date) between '2024-12-01' and '2024-12-31'

    GROUP BY

        cliente

),

_cross_channel_contacts_ AS (

    SELECT

        _c_.cliente,

        _c_.caso,

        cast(_c_.fechaapertura as DATE),

        _c_.origencaso,

        cast(_ic_._fecha_inicial_ as DATE)

    FROM

        bi_ar_att.bt_ar_bi_casos_vw as _c_

        JOIN

        _initial_contacts_ as _ic_

    ON

        _c_.cliente = _ic_.cliente

    where

               cast(_c_.fechaapertura as date) between '2024-12-01' and '2024-12-31'

        and cast(_c_.fechaapertura as DATE) BETWEEN cast(_ic_._fecha_inicial_ as DATE) AND cast(_ic_._fecha_inicial_ as DATE) + 7

        )

SELECT

               COUNT(DISTINCT _cross_channel_contacts_.cliente) * 100.0 / COUNT(distinct bi_ar_att.bt_ar_bi_casos_vw.cliente) AS _porcentaje_resolutivo_

FROM

    _cross_channel_contacts_

inner JOIN

    bi_ar_att.bt_ar_bi_casos_vw

ON

    _cross_channel_contacts_.cliente = bi_ar_att.bt_ar_bi_casos_vw.cliente

  where cast(bi_ar_att.bt_ar_bi_casos_vw.fechaapertura as date) between '2024-12-01' and '2024-12-31' ;
```


También te paso todos los case que hice en los motivos y submotivos para limpiar esa información que está “sucia” en la tabla:

``` SQL
    CASE

        when _a_.motivo = '10060' and _a_.desc_submotivocodigo = 'Baja Tensión' then 'Con Suministro - Habituales CS'

        when _a_.motivo = '10060' and _a_.desc_submotivocodigo in (

            'Buzón Puerta Faltante/Abierta/estruct deteriorada',

            'Buzón puerta intersticios',

            'Buzón puerta rota',

            'Caja esq tapa abierta/rota/deteriorada/con agua',

            'Caja esq tapa desnivel')

        then 'Con Suministro - SVP - Caja Dist. (Buzón o Caja Esq)'

        when _a_.motivo = '10060' and _a_.desc_submotivocodigo in (

            'Enre 24 hs',

            'Requiere Restitución x Arreglo',

            'Sin fase',

            'Sin Luz')

        then 'Sin Suministro - Habituales SS'

        when _a_.motivo = 'MOT003' and _a_.desc_submotivocodigo = 'ATAR121' then 'Atención Emergencia'

        when _a_.motivo = 'MOT003' then 'Comercial'

        when _a_.motivo = 'MOT008' then 'Pagos'

        when _a_.motivo = 'MOT015' then 'Recaudación'

        else _a_.motivo

    end as _motivo_,

              CASE _a_.desc_submotivocodigo

                            when 'ATAR237' then 'Consulta Consumos Facturados'

                            when 'ATAR239' then 'CT Digital - Simplificado'

                            when 'ATAR242' then 'Consulta de Convenio'

                            when 'ATAR243' then 'Desactivación de Convenio'

                            when 'ATAR245' then 'Consulta por cambio de Titularidad'

                            when 'ATAR249' then 'Baja Débito Automático'

                            else _a_.desc_submotivocodigo end,
```
                            