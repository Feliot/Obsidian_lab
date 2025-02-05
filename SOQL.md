#soql SF

Ingreso: chrome-extension://aodjmnfhjibkcdimpodiifdjnnncaafh/data-export.html?host=enelsud.my.salesforce.com


```SQL
select DATENAME('YYYY-MM-DD' , CreatedDate) as createdatee,  ChannelName, name, Id  from MessagingSession where ChannelName  like '%5491161876995%' limit 20 


```

``` SQL
--FINAL:
select  DAY_ONLY(CreatedDate) , COUNT_DISTINCT(name)  from MessagingSession where ChannelName  like '%5491161876995%' and Name in ('MS-16749398', 'MS-16749368' ) 
group by DAY_ONLY(CreatedDate) 
--DAY_ONLY(CreatedDate)>= 2024-08-01
--conversationEntry.EntryType
```

``` SQL
select CreatedDate, name, ChannelType, MessagingEndUser.Name
from MessagingSession where ChannelName  like '%5491161876995%' and DAY_ONLY(CreatedDate)= 2024-07-01 
limit 20
```

``` SQL
-- EntryTypes  de julio
Select EntryType, COUNT_DISTINCT(conversationID)  from conversationEntry 
where conversationID  in (select id from MessagingSession where ChannelName  like '%5491161876995%' and DAY_ONLY(CreatedDate)= 2024-07-01 ) and EntryType != 'Text'
group  by EntryType
LIMIT 10
```

``` SQL
Select id from MessagingSession where id in (SELECT conversationID FROM ConversationEntry WHERE EntryType = 'ChatbotEndedChatByAction' and ConversationId = '0Mwck000002qIED')
```

and  ConversationId  in (Select id from conversationEntry where EntryType = 'ChatbotEndedChatByAction' )

``` SQL 
--FINAL contar casos por fecha:
select  DAY_ONLY(CreatedDate) , COUNT_DISTINCT(name), COUNT(name)   from MessagingSession 
where ChannelName  like '%5491161876995%' and DAY_ONLY(CreatedDate)= 2024-07-01 and id in (Select conversationID from conversationEntry where EntryType in ('ChatbotEndedChatByAction' ,'BotEscalated'))
group by DAY_ONLY(CreatedDate) 
LIMIT 10
```
resultado: FISCAL_MONTH

| expr0      | expr1 | expr2 |
| ---------- | ----- | ----- |
| 2024-07-01 | 5502  | 5502  |
```SQL
-- ULTIMO FERNANDO GEIST:
--MessagingSession
select CreatedDate, name, ChannelName, MessagingEndUser.name from MessagingSession 
where ChannelName = 'whatsapp:+5491161876995' and (CreatedDate >= 2024-07-01T00:00:00.000-03:00 and CreatedDate < 2024-07-02T00:00:00.000-03:00) and id in (Select conversationID from conversationEntry where EntryType in ('ChatbotEndedChatByAction'))
```

``` SQL
--AGRUPADO MENSUAL
select  FISCAL_MONTH(CreatedDate) mes, COUNT_DISTINCT(name) cant_mensajes
from MessagingSession 
where ChannelName = 'whatsapp:+5491161876995' and (CreatedDate >= 2024-01-01T00:00:00.000-00:00 and CreatedDate < 2024-02-01T00:00:00.000-00:00) and id in (Select conversationID from conversationEntry where EntryType in ('ChatbotEndedChatByAction') )
group by FISCAL_MONTH(CreatedDate)
```

```  SQL
--DETALLE con DAY_ONLY
select  CreatedDate, name, ChannelName, MessagingEndUser.name from MessagingSession 
where ChannelName = 'whatsapp:+5491161876995' and DAY_ONLY(CreatedDate)= 2024-07-01 and id in (Select conversationID from conversationEntry where EntryType in ('ChatbotEndedChatByAction'))
```

``` SQL
-- MENSAJES
Select EntryType, conversationID ,ActorName, MessageIdentifier, Message, CreatedDate from conversationEntry 
where conversationID  in (select id from MessagingSession where ChannelName  like '%5491161876995%' and DAY_ONLY(CreatedDate)= 2024-07-01 ) and EntryType != 'Text'
limi 20
```

![[Pasted image 20240819142048.png]]

