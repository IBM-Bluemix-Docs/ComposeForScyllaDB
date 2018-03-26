---

copyright:
  years: 2016,2018
lastupdated: "2017-07-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Informazioni su {{site.data.keyword.composeForScyllaDB}}
{: #about-compose-for-scylladb}

ScyllaDB è un sostituto del database distribuito su colonna ampia Cassandra. ScyllaDB è scritto in C++, piuttosto che il Java di Cassandra, per un migliore utilizzo della risorsa che può dare un risultato 10 volte migliore nelle prestazioni nei riferimenti. In aggiunta per mantenere la compatibilità con i file di dati e lo strumento Cassandra, ScyllaDB aggiunge le funzionalità di ottimizzazione automatica. {{site.data.keyword.composeForScyllaDB_full}} estende le funzionalità di ScyllaDB a gestirle al tuo posto, offrendo un sistema di distribuzione con ridimensionamento automatico facile da utilizzare che distribuisce l'elevata disponibilità e la ridondanza e i backup automatizzati.
{:shortdesc}

## Creazione di un'istanza del servizio {{site.data.keyword.composeForScyllaDB}}

Puoi creare un servizio {{site.data.keyword.composeForScyllaDB}} dalla pagina [{{site.data.keyword.composeForScyllaDB}}](https://console.{DomainName}/catalog/services/compose-for-scylladb/) nel catalogo {{site.data.keyword.cloud_notm}}.

Scegli un nome del servizio, una regione, un'organizzazione e uno spazio in cui eseguire il provisioning del servizio. Puoi utilizzare il campo **Select a database version** per scegliere tra le versioni del database disponibili.

Quando esegui il provisioning della tua istanza {{site.data.keyword.composeForScyllaDB}} puoi scegliere i piani *Standard* o *Enterprise*. Con il piano *Enterprise*, puoi eseguire il provisioning della tua istanza {{site.data.keyword.composeForScyllaDB}} in un cluster {{site.data.keyword.composeEnterprise}} disponibile. {{site.data.keyword.composeEnterprise}} fornisce la sicurezza e l'isolamento necessari per la conformità aziendale e utilizza la rete dedicata per garantire le prestazioni dei database distribuiti. Per ulteriori dettagli, consulta la [Documentazione aziendale Compose](../ComposeEnterprise/index.html).

## Gestione di {{site.data.keyword.composeForScyllaDB}}

Puoi gestire il tuo servizio dal dashboard del servizio. Qui puoi trovare le informazioni sul tuo database {{site.data.keyword.cloud_notm}} Compose e su come collegarti ad esso. Puoi anche:

- gestire i tuoi backup
- assegnare ulteriori risorse al tuo servizio 
- utilizzare le whitelist per limitare l'accesso ai tuoi database 

Per ulteriori informazioni, consulta [Impostazioni](./dashboard-settings.html).

## Connessione a {{site.data.keyword.composeForScyllaDB}}

Puoi collegarti al tuo servizio utilizzando le credenziali create insieme al servizio o con le stringhe di connessione e la riga di comando forniti nella scheda *Overview* nel tuo dashboard del servizio.

## Connessione di un'applicazione {{site.data.keyword.cloud_notm}} a {{site.data.keyword.composeForScyllaDB}}

Per collegare un'applicazione {{site.data.keyword.cloud_notm}} al tuo servizio, utilizza le credenziali create insieme al servizio. Puoi trovare le informazioni su come collegare un'applicazione {{site.data.keyword.cloud_notm}} a un servizio {{site.data.keyword.composeForScyllaDB}} in [Connessione a un'applicazione {{site.data.keyword.cloud_notm}}](./connecting-bluemix-app.html).

## Connessione a {{site.data.keyword.composeForScyllaDB}} dall'esterno di {{site.data.keyword.cloud_notm}}

Se desideri collegarti a {{site.data.keyword.composeForScyllaDB}} dall'esterno di {{site.data.keyword.cloud_notm}}, puoi utilizzare la riga di comando o le stringhe di connessione fornite. Puoi trovare le informazioni su come collegarti in [Connessione a un'applicazione esterna](./connecting-external.html).
