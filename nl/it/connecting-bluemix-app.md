---

copyright:
  years: 2016,2018
lastupdated: "2018-05-09"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}

# Connessione a un'applicazione {{site.data.keyword.cloud_notm}}

Per collegare un'applicazione {{site.data.keyword.cloud}} al tuo servizio, utilizza le credenziali create quando è stato eseguito il provisioning del servizio.

## Collegamento con l'applicazione di esempio 'Hello World'

L'applicazione di esempio [compose-scylladb-helloworld-nodejs](https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs) illustra come utilizzare Node.js per il collegamento a un servizio {{site.data.keyword.composeForScyllaDB}} utilizzando le credenziali fornite. L'applicazione crea, legge da, e scrive su un database.

Scarica l'applicazione di esempio e segui le istruzioni nel file readme. In seguito, nella pagina dei dettagli della tua applicazione in {{site.data.keyword.cloud_notm}}, fai clic su **Visualizza applicazione** per visualizzare il contenuto della tabella *esempi*.

## Credenziali disponibili

Nome campo|Descrizione
----------|-----------
`db_type`|Il tipo di database offerto dal servizio, in questo caso `scylla`.
`uri_cli_1`|Una riga di comando shell `cqlsh` secondario che esegue il collegamento all'istanza del database.
`maps`|Un'associazione di connessione ScyllaDB che fornisce le informazioni necessarie ai vari driver per associare gli indirizzi IP interni con i nomi DNS esterni di un database ScyllaDB.
`name`|Il nome di distribuzione del database.
`uri_cli`|Una riga di comando shell `cqlsh` che esegue il collegamento all'istanza del database.
`uri_direct_2`|Un terzo URI che può essere utilizzato per il collegamento al servizio. Il formato è lo stesso di `uri`.
`uri_direct_1`|Un URI secondario che può essere utilizzato per il collegamento al servizio. Il formato è lo stesso di `uri`.
`ca_certificate_base64` `(facoltativo)`|Un certificato autofirmato con codifica base64 che viene utilizzato per confermare che un'applicazione sia collegata al server corretto. Il certificato è presente solo sui servizi che hanno un certificato autofirmato invece di un certificato di Let's Encrypt. Devi decodificare la chiave prima di utilizzarla, come mostrato nell'applicazione di esempio.
`deployment_id`|Un identificativo interno per il servizio come creato in Compose.
`uri_cli_2`|Una terza riga di comando shell `cqlsh` che esegue il collegamento all'istanza del database.
`uri`|L'URI da utilizzare quando esegui il collegamento al servizio, include lo schema (`scylla:`), la password, il nome host del server, il numero di porta a cui collegarsi e il nome del database.
{: caption="Tabella 1. Credenziali di Compose for ScyllaDB" caption-side="top"}

**Nota:** tre portali `haproxy` forniscono l'accesso al servizio Scylla. Per il collegamento possono essere utilizzati `uri`, `uri_direct_1` e `uri_direct_2`. Nelle tue applicazioni, passa da `uri`, `uri_direct_1` e `uri_direct_2` per gestire le risposte agli errori di connessione.
