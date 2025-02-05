#Cloudera_datalake  
# Contactabilidad

SQL en Serviceproduct:
``` SQL
not EXISTS (select bpc.contacto__c from ltmsalesforcemkt.billingprofilecontact bpc inner join ltmsalesforcemkt.billing_profile bp_2 
			on bp_2.id = bpc.billing_profile__c --bp.contact__c =c.id
			inner join ltmsalesforcemkt.pointofdelivery pod 
			on pod.id = bp_2.pointofdelivery__c 
			inner join ltmsalesforcemkt.asset asse
			on asse.pointofdelivery__c = pod.id and asse.status = 'Installed' and asse.country__c = 'ARGENTINA' 
			and bp_2.accountcontract__c = substring(asse.externalid__c, 1, length(asse.externalid__c)-3) 
			where bpc.contacto__c = s.contact__c and asse.id = a.id)
and not EXISTS (select sd.id  from ltmsalesforcemkt.rd_serviceproduct__delete sd  where sd.id  = s.id  ) --SP Eliminados
```

**Código - Estado**
c.verification_principal_email__c
1. VER000 - Inicial
2. VER001 - No Validado
3. VER002 - Rechazado
4. VER003 - Validado
5. VER004 - Entregado
6. VER005 - Pendiente