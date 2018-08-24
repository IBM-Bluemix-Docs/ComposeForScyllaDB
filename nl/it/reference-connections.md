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

# Configurazione della connessione
{: #connection-configuration}

Le connessioni database {{site.data.keyword.composeForScyllaDB_full}} sono gestite da 3 portali HAProxy. Ogni portale ha 64 MB di memoria.

Il failover al lato client è responsabilità del progettista dell'applicazione. La maggior parte dei driver Scylla sono compatibili con il cluster, in quanto puoi fornire una stringa di connessione, tutte e tre le stringhe di connessione, le associazioni di connessione o una loro qualsiasi combinazione per gestire più stringhe di connessione e un failover normale in modo automatico. A meno che tu non stia lavorando con un driver che gestisce completamente gli errori di failover, devi:

* Lavorare con un driver che accetta più endpoint nella sua configurazione della connessione.
* Assicurarti che le chiamate delle tue applicazioni per leggere o scrivere nel database reagiscano in modo appropriato agli errori. Le opzioni includono:
  + Registrazione dell'errore e riconnessione.
  + Riconnessione e nuovo tentativo dell'operazione che ha causato l'errore.
  + Accodamento dell'operazione non riuscita e pianificazione della riconnessione.

## Crittografia in transito

Tutti i portali HAProxy {{site.data.keyword.composeForScyllaDB}} hanno TLS/SSL abilitati e supportano TLS versione 1.2. I certificati per il servizio sono certificati di Let's Encrypt. Alcuni driver e l'interfaccia della riga di comando, cqlsh, hanno bisogno di una copia della catena di certificati per il collegamento al tuo servizio, per ulteriori informazioni consulta [Using Certifcates](https://console.{DomainName}/docs/services/ComposeForScyllaDB/scylla-certificates.html)

## Limiti connessioni

Le connessioni database hanno un limite di connessioni di 1000 connessioni per portale. Se si supera il limite di connessioni, il tuo servizio smetterà di rispondere. Puoi gestire le connessioni dalla tua applicazione tramite la funzione di pooling di connessioni del tuo driver.

## Timeout proxy

I portali HAProxy gestiscono il ciclo di vita delle connessioni tra il server e il client. Li descriviamo qui in dettaglio come guida di riferimento per gli scrittori di driver e per gli altri che hanno un interesse nell'attività di basso livello delle connessioni al database {{site.data.keyword.composeForScyllaDB}}.

Impostazione | Valore | Applicazione
----------|-----------|-----------
client | 5m | Quando si prevede che il client riconosca o invii i dati.
connect | 4s | Quando si sta stabilendo una connessione tra il proxy e il server.
server | 5m | Quando si prevede che il server riconosca o invii i dati.
tunnel | 1 giorno | Quando viene stabilita una connessione bidirezionale tra un client e un server e la connessione rimane inattiva in entrambe le direzioni.
client-fin | 1o | Quando si prevede che il client riconosca o invii dati mentre una direzione è già stata arrestata.
check | 5s | Come un controllo di timeout secondario quando si sta stabilendo una connessione tra il proxy e il server.

{: caption="Tabella 1. Timeout Scylla HAProxy" caption-side="top"}
