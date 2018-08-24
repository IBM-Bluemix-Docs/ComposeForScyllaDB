---

copyright:
  years: 2018
lastupdated: "2018-05-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilizzo di cqlsh
{: #using-cqlsh}

Puoi utilizzare `cqlsh` per il collegamento diretto a {{site.data.keyword.composeForScyllaDB_full}}.

Al momento della compilazione di questo documento, `cqlsh` è un programma Python 2.7 e non viene eseguito con Python 3.
{: .tip}

Poiché `cqlsh` è un'applicazione Python, puoi eseguire `pip install cqlsh` per installare la versione più recente del comando.

Puoi anche ottenere `cqlsh` da una distribuzione completa di Cassandra scaricandolo da [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/) che offre file binari, sorgente e istruzioni su come installare Cassandra su distribuzioni Debian (apt) e Red Hat (RPM) Linux. Scarica una release binaria e decomprimi il file in una directory. Passa quindi con `cd` a tale directory, dove troverai `cqlsh` nella directory `bin`.

Come ottieni questo strumento nel tuo dispositivo locale dipende dal sistema operativo locale. Il modo più facile è di installare l'ultimo release Cassandra utilizzando un metodo appropriato per il tuo sistema operativo (le ultime versioni supportano ancora la versione 2.1.8, che è quella di Scylla) e poi utilizzare il comando `cqlsh` integrato. 

Su un Mac, ad esempio, puoi utilizzare `homebrew` per installare Cassandra immettendo `brew install cassandra`.

## File cqlshrc

Quando viene eseguito il comando `cqlsh`, carica `$HOME/.cassandra/cqlshrc` per impostazione predefinita, che può impostare molti parametri che non possono essere impostati nella riga di comando. Puoi modificare il file predefinito per impostare i parametri e tutte le modifiche che applichi alle sessioni a meno che non vengono sovrascritte dalla riga di comando.

Se si stanno eseguendo più istanze di Scylla in diversi ambienti, puoi creare un file `cqlshrc` separato e utilizzarlo aggiungendo `--cqlshrc=[filename]` al tuo comando.


Per un esempio di un file cqlshrc completo, consulta [Apache sample of cqlshrc](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample). 

### cqlshrc per {{site.data.keyword.composeForScyllaDB}}

Un file `cqlshrc` minimo per stabilire una connessione al tuo servizio è simile al seguente:

```
[authentication]
username = scylla
password = [password]

[connection]
hostname = [hostname.composedb.com]
port = [portnumber]
factory = cqlshlib.ssl.ssl_transport_factory

[ssl]
version=SSLv23
certfile = ~/path_to_your/lechain.pem
validate = true
```

Immetti le informazioni fornite nelle _stringhe di connessione_ del tuo servizio e salva il file come `example.cqlshrc`.

Puoi ora accedere al tuo database Scylla eseguendo `cqlsh --ssl --cqlshrc=example.cqlshrc` dalla riga di comando. Tieni presente che anche se `cqlshrc` ha un'opzione per specificare `ssl=true` sembra che venga ignorata, per cui devi utilizzare l'indicatore `--ssl`.

## TLSv1.2

`cqlsh` non viene automaticamente impostato su TLSv1.2 quando negozia una connessione crittografata. Per stabilire correttamente una connessione a una distribuzione Compose Scylla, la versione TLS deve essere specificata nell'ambiente che esegue `cqlsh`. Per ottenere la maggiore compatibilità (e per alcune correzioni future di TLSv1.3), imposta `SSL_VERSION` nell'ambiente su SSLv23 utilizzando `export SSL_VERSION=SSLv23` o posizionando `SSL_VERSION=SSLv23` prima del comando di connessione `cqlsh`.

Puoi anche impostare la versione specificatamente su TLSv1.2 utilizzando `export SSL_VERSION=TLSv1_2` o posizionando `SSL_VERSION=TLSv1_2` prima del comando di connessione `cqlsh`.

## Connessione senza convalida

La convalida viene abilitata per impostazione predefinita, ma puoi collegarti a Scylla con TLS/SSL senza la convalida dell'host remoto. Tuttavia questo non è consigliato per la produzione. La convalida è controllata dalla variabile di ambiente `SSL_VALIDATE` o dalle impostazioni SSL nel file cqlshrc. Imposta `SSL_VALIDATE` su false (`export SSL_VALIDATE=false`) nel tuo ambiente per disabilitare la convalida.

In alternativa, puoi modificare il file cqlshrc per disabilitare la convalida.

```
[ssl]
validate = false
```

Se vuoi disabilitare la convalida per un solo comando `cqlsh`, includi `SSL_VALIDATE=false` prima del comando di connessione `cqlsh`. 

Devi comunque indicare a `cqlsh` di utilizzare TLSv1.2, anche quando la convalida non è abilitata. Se non hai impostato la versione SSL nell'ambiente e non vuoi eseguire la convalida, la connessione dovrebbe essere simile a questo esempio:

```
SSL_VERSION=SSLv23 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Ottenimento e utilizzo del certificato

Se vuoi utilizzare cqlsh e verificare l'host remoto, devi ottenere il certificato, salvarlo in un'ubicazione accessibile e fornire quindi a cqlsh il percorso al certificato.

Imposta la variabile di ambiente `SSL_CERTFILE` sul percorso e il nome file del tuo certificato salvato per abilitarlo per tutti i successivi richiami di `cqlsh`. 

In alternativa, puoi aggiungere quanto segue al tuo file cqlshrc.

```
[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

Se vuoi solo utilizzare un certificato specifico per un singolo comando `cqlsh`, inserisci `SSL_CERTFILE = ~/path_to_your/lechain.pem` all'inizio del tuo comando di connessione cqlsh. Ad esempio:

```
SSL_VERSION=SSLv23 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Introduzione a cqlsh

Se immetti `HELP` puoi visualizzare che la shell ha molte funzionalità. Anche se tutti i comandi `TAB` hanno il completamento. Ad esempio:

1. Immetti `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. Dovresti visualizzare le scelte per la classe di replica.
2. Scegli `SimpleStrategy` perché il cluster non sarà esteso in più datacenter.
3. Premi `<TAB><TAB>` nuovamente e immetti 3 per il valore `replication_factor`. Quindi chiudi la parentesi con `}` e termina l'istruzione con `;<enter>`.

Hai creato il tuo primo KEYSPACE e lo hai reso predefinito per la replica dei tuoi dati in tutti e tre i nodi del tuo cluster.

```sql
scylla@cqlsh> CREATE KEYSPACE example_keyspace WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 3 };
scylla@cqlsh> use example_keyspace ;
scylla@cqlsh:example_keyspace> create table names (
                    ... name_id uuid,
                    ... last_name text,
                    ... first_name text,
                    ... PRIMARY KEY(name_id)
                    ... );
scylla@cqlsh:example_keyspace> 
```

Ora puoi utilizzare il keyspace:

```sql 
USE my_new_keyspace;
```

La tua shell ora visualizza che il prompt dei comandi sta utilizzando il tuo keyspace per impostazione predefinita. Ogni tabella ha un keyspace.

Immetti il seguente comando `CREATE TABLE` nella shell per creare una tabella da compilare con gli esempi.

```sql
CREATE TABLE my_new_table (
    my_table_id uuid,
    last_name text,
    first_name text,
    PRIMARY KEY(my_table_id)
);
```
