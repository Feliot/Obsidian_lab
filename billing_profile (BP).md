
#bp #salesforce #SF #billing_profile #perfi_facturacion
Entidad del sistema [[Salesforce]]  se migra a [[Cloudera - Datalake]] dentro de esquema [[ltmsalesforcemkt]].
El tipo de Reparto se hace por la ContractLine #contract_line

Se relaciona con :
	[[asset]] : `billingprofile.account__c  = asset.accountid` 
		`and bp.accountcontract__c = substring(a.externalid__c, 1, length(a.externalid__c)-3) ` -> (Aunque esta ultima LINEA se puede evitar si el ASSET se Relaciona con el POD [a.pointofdelivery__c] = [p.id] )
	[[billingprofilecontact (bpc)]] : `bp.id = bpc.billing_profile__c` 

campo: 
[EDEEnrolment__c] ( #edeenrolment__c) = dice si tiene o no suscripción #e-factura 
edeenrolment__c = 1 --Adheridos a [E-Factura]
edeenrolment__c = 0 --NO Adheridos a [E-Factura]


[Delivery_Type__c] ( #delivery_type__c )=  Tipo de Reparto :
- 'S' = Formato Digital
- 'F' = Braile
- 'P' = Postal
- 'N' = Normal

SQL
Cloudera Prod - Cantidad de BP totales con al menos un envio digital.sql


