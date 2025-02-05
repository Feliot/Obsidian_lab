#asset #salesforce #SF #activos
Entidad del sistema [Salesforce]  se migra a [[Cloudera - Datalake]] dentro de esquema [[ltmsalesforcemkt]].

Contiene los [Activos] de [[Salesforce]].

Recordar colocar en toda relación estos criterios para filtrar instalados y de ARGENTINA:
 `a.status  = 'Installed' and a.country__c = 'ARGENTINA'`

Se relaciona con :
	[[billing_profile (BP)]] : `asset.accountid = bp.account__c` , además , para una correcta Relación con el Cliente (CuentaContrato), si no se relaciona antes con el [[pointofdelivery (pod)]]  se debe agregar -> 
``` SQL
bp.accountcontract__c = replace(a.externalid__c, 'ARG', '')
```


[[pointofdelivery (pod)]] : `pod.id = a.pointofdelivery__c` 



