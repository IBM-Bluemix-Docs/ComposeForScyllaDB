---

Copyright:
  Years: 2017
lastupdated: "2017-09-07"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Panoramica sul servizio

La pagina _Overview_ ti mostra le informazioni sul tuo database {{site.data.keyword.cloud}} Compose. La panoramica include le informazioni di identificazione essenziali e l'utilizzo della risorsa corrente. Troverai inoltre una sezione per le stringhe di connessione che puoi utilizzare con gli strumenti o utilizzare gli strumenti per il collegamento al tuo database.

## Dettagli della distribuzione

Il pannello _Deployment Details_ mostra i dettagli del tuo servizio.

![Dettagli della distribuzione](./images/scylla-deployment-details.png "Una vista del pannello dei dettagli della distribuzione")

### Tipo

Il tipo di database offerto dal servizio e la versione che il servizio utilizza.

### Nome

Un identificativo interno per il servizio.

### Utilizzo

La dimensione del tuo database e la quantità di memoria fornita dal tuo piano di servizio.


## Stringhe di connessione

Troverai ogni stringa di connessione per il tuo servizio in una scheda differente nel pannello _Connection Strings_.

### HTTPS

Una stringa di connessione formattata URI che può essere utilizzata da alcune librerie client e che contiene tutte le informazioni necessarie al collegamento di altre librerie. Puoi trovare come utilizzare la stringa di connessione per il collegamento in [Connessione a un'applicazione esterna](./connecting-external.html).

### Riga di comando

La **Riga di comando** è un comando pre-formattato che richiamerà `cqlsh` con i parametri corretti. Il comando visualizzato include gli indicatori obbligatori (`--ssl` e `--cqlversion`).  Per utilizzarla, dovrai aver installato gli strumenti client PostgreSQL nel sistema locale. Puoi trovare ulteriori informazioni su come farlo in [Connessione a un'applicazione esterna](./connecting-external.html).

### Associazioni
Queste associazioni di traduzione dell'indirizzo possono essere utilizzate nelle applicazioni che richiedono l'elevata disponibilità e il rilevamento automatico per trovare i nodi nel cluster. Traducono gli indirizzi del portale esterni in interni per i driver del client che utilizzano questa funzione.

### Certificato SSL

Il tuo servizio Compose {{site.data.keyword.cloud_notm}} ti fornisce un certificato SSL che puoi utilizzare per collegarti al tuo database.
