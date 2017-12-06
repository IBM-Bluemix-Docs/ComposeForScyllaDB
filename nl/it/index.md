---

copyright:
  years: 2016,2017
lastupdated: "2017-07-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introduzione a Compose for ScyllaDB
{: #getting-started-with-compose-for-scylladb}

ScyllaDB è un sostituto del database distribuito su colonna ampia Cassandra. ScyllaDB è scritto in C++, piuttosto che il Java di Cassandra, per un migliore utilizzo della risorsa che può dare un risultato 10 volte migliore nelle pretazoni nei riferimenti. In aggiunta per mantenere la compatibilità con i file di dati e lo strumento Cassandra, ScyllaDB aggiunge le funzionalità di ottimizzazione automatica. {{site.data.keyword.composeForScyllaDB_full}} estende le funzionalità di ScyllaDB a gestirle al tuo posto, offrendo un sistema di distribuzione con ridimensionamento automatico facile da utilizzare che distribuisce l'elevata disponibilità e la ridondanza e i backup automatizzati.
{:shortdesc}

**Nota:** {{site.data.keyword.composeForScyllaDB_full}} non fornisce accesso alla IU di Compose. Consulta [Compose on {{site.data.keyword.cloud}} Support](https://help.compose.com/docs/bluemix-compose-support) per ulteriori dettagli.

## Creazione di un'istanza del servizio Compose per ScyllaDB

[Crea un'istanza {{site.data.keyword.composeForScyllaDB}}](https://console.ng.bluemix.net/catalog/services/compose-for-scylladb/).

Quando crei un'istanza del servizio, scegli un nome per il tuo servizio e un nome per la credenziale. Lascia il servizio senza bind; puoi collegare un'applicazione al tuo servizio successivamente utilizzando le credenziali fornite quando è stato eseguito il provisioning del servizio. I vari valori delle credenziali sono elencati nella sezione "Credenziali disponibili".

Quando esegui il provisioning della tua istanza {{site.data.keyword.composeForScyllaDB}} puoi scegliere i piani *Standard* o *Enterprise*. Con il piano *Enterprise*, puoi eseguire il provisioning della tua istanza {{site.data.keyword.composeForScyllaDB}} in un cluster {{site.data.keyword.composeEnterprise}} disponibile. {{site.data.keyword.composeEnterprise}} fornisce la sicurezza e l'isolamento necessari per la conformità aziendale e utilizza la rete dedicata per garantire le prestazioni dei database distribuiti. Per ulteriori dettagli, consulta la [Documentazione aziendale Compose](../ComposeEnterprise/index.html).

## Gestione di Compose for ScyllaDB

Puoi gestire il tuo servizio dal dashboard del servizio. Qui puoi trovare le informazioni sul tuo database {{site.data.keyword.cloud_notm}} Compose e su come collegarti ad esso. Puoi anche:

- gestire i tuoi backup
- assegnare ulteriori risorse al tuo servizio 
- utilizzare le whitelist per limitare l'accesso ai tuoi database 

Per ulteriori informazioni, consulta [Impostazioni](./dashboard-settings.html).

## Connessione a Compose for ScyllaDB

Puoi collegarti al tuo servizio utilizzando le credenziali create insieme al servizio o con le stringhe di connessione e la riga di comando forniti nella scheda *Overview* nel tuo dashboard del servizio.

## Connessione di un'applicazione {{site.data.keyword.cloud_notm}} a Compose for ScyllaDB

Per collegare un'applicazione {{site.data.keyword.cloud_notm}} al tuo servizio, utilizza le credenziali create insieme al servizio. Puoi trovare le informazioni su come collegare un'applicazione {{site.data.keyword.cloud_notm}} a un servizio {{site.data.keyword.composeForScyllaDB}} in [Connessione a un'applicazione {{site.data.keyword.cloud_notm}}](./connecting-bluemix-app.html).

## Connessione a Compose for ScyllaDB dall'esterno di {{site.data.keyword.cloud_notm}}

Se desideri collegarti a {{site.data.keyword.composeForScyllaDB}} dall'esterno di {{site.data.keyword.cloud_notm}}, puoi utilizzare la riga di comando o le stringhe di connessione fornite. Puoi trovare le informazioni su come collegarti in [Connessione a un'applicazione esterna](./connecting-external.html).
