---
copyright:
  years: 2016,2018
lastupdated: "2018-06-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Architettura 
{: #architecture}

## Replica

Un servizio {{site.data.keyword.composeForScyllaDB_full}} contiene un cluster con tre repliche di dati. Tutte le repliche sono ugualmente importanti; non c'è alcuna replica principale o master e ogni nodo conosce lo stato degli altri nodi. Seleziona una regione in cui il servizio viene distribuito, le tre repliche di dati vengono distribuite nelle zone di disponibilità della regione e i tuoi dati vengono copiati in tutte e tre. Se un membro dei dati diventa non raggiungibile, il cluster continua a funzionare normalmente.

## Crittografia del disco

Tutti i servizi {{site.data.keyword.composeForScyllaDB}} hanno la crittografia a riposo. I tuoi dati si trovano su server che hanno la crittografia a livello di volume abilitata. 

Poiché {{site.data.keyword.composeForScyllaDB}} è un servizio gestito, gli operatori {{site.data.keyword.cloud_notm}} Compose possono visualizzare i dati. In aggiunta alla crittografia del disco, dovresti codificare le informazioni personali prima di archiviarle nel database o di utilizzare le estensioni o le funzioni per abilitare la crittografia nel database stesso. Anche se questi metodi possono influire sull'usabilità o sulle prestazioni, è buona norma assicurarsi che le informazioni personali siano protette con la crittografia.

## Scalabilità automatica

Le risorse vengono ridimensionate automaticamente in base all'utilizzo della memoria disco totale dei database Scylla. La memoria è basata su un rapporto del disco di cui viene eseguito il provisioning di 1:10. Queste risorse sono raccolte in bundle come unità.

Il ridimensionamento automatico è progettato per rispondere alle tendenze a medio e a lungo termine del tuo database. Il tuo servizio viene controllato ogni ora. Se sta esaurendo lo spazio su disco, delle ulteriori unità vengono assegnate alla distribuzione. 

Il ridimensionamento automatico non riduce le distribuzioni in cui l'utilizzo di disco/memoria si è ridotto. Le risorse di cui è stato eseguito il provisioning ai tuoi database rimangono per le tue future esigenze oppure finché non riduci manualmente la tua distribuzione.
{: .tip}


