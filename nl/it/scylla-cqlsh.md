---

copyright:
  years: 2018
lastupdated: "2018-03-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilizzo di cqlsh
{: #using-cqlsh}

Non puoi collegarti direttamente a {{site.data.keyword.composeForScyllaDB_full}} utilizzando `cqlsh`.
Ci sono diversi modi per eseguirne l'installazione. Poiché si tratterà di un'applicazione Python, l'esecuzione di `pip install cqlsh` installerà la versione più recente del comando. Al momento della compilazione di questo documento, `cqlsh` è un programma Python 2.7 e non viene eseguito con Python 3.

È anche possibile ottenere `cqlsh` da una distribuzione completa di Cassandra. Può essere ottenuta da [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/) che offre file binari, sorgente e istruzioni su come installare Cassandra su distribuzioni Debian (apt) e Red Hat (RPM) Linux. Scarica una release binaria e decomprimi il file in una directory. Passa quindi con `cd` a tale directory, dove troverai `cqlsh` nella directory `bin`.

Come ottieni questo strumento nel tuo dispositivo locale dipende dalla tua piattaforma locale. Il modo più facile è di installare l'ultimo release Cassandra utilizzando un metodo appropriato per la tua piattaforma (le ultime versioni supportano ancora la versione 2.1.8 che è quella di Scylla) e di utilizzare il comando `cqlsh` integrato. 

Su un Mac, ad esempio, installa Cassandra con `homebrew` immettendo `brew install cassandra`.

## Informazioni sui file cqlshrc
Quando viene eseguito, il comando `cqlsh` carica un file, `$HOME/.cassandra/cqlshrc` per impostazione predefinita, che può impostare molti parametri che non possono essere impostati nella riga di comando. Un [esempio Apache di cqlshrc](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample). Puoi modificare il file predefinito per impostare i parametri. Si applicheranno a tutte le sessioni a meno che non vengano sovrascritti nella riga di comando.

Se si stanno eseguendo più istanze di Scylla in diversi ambienti, puoi creare un file `cqlshrc` separato e fare in modo che `cqlsh` lo utilizzi aggiungendo `--cqlshrc=[filename]` al tuo comando.

### cqlshrc per {{site.data.keyword.composeForScyllaDB}}
Questo è un file `cqlshrc` minimo di esempio per stabilire una connessione al tuo servizio:
```
[authentication]
username = scylla
password = [password]

[connection]
hostname = [hostname.composedb.com]
port = [portnumber]
factory = cqlshlib.ssl.ssl_transport_factory

[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

Immetti le informazioni fornite dalle _stringhe di connessione_ del tuo servizio e salva questo file come example.cqlshrc. Esegui `cqlsh --ssl --cqlshrc=example.cqlshrc` dalla riga di comando ed eseguirai l'accesso al tuo database Scylla. Nota: anche se `cqlshrc` ha un'opzione per specificare `ssl=true` sembra che venga ignorata; occorre pertanto utilizzare l'indicatore `--ssl`.

La versione ssl non può essere impostata nel file `cqlshrc`; dovrai pertanto impostare l'ambiente per utilizzare TLSv1.2  oppure accodare SSL_VERSION=TLSv1_2 davanti al comando di connessione `cqlsh`.

## TLSv1.2

`cqlsh` non viene automaticamente impostato su TLSv1.2 quando negozia una connessione crittografata. Per stabilire correttamente una connessione a una distribuzione Compose Scylla, la versione TLS deve essere specificata nell'ambiente che esegue `cqlsh`. Imposta SSL_VERSION nell'ambiente utilizzando:
`export SSL_VERSION=TLSv1_2`
oppure inserisci `SSL_VERSION=TLSv1_2` prima del comando di connessione `cqlsh`.

## Connessione senza convalida

Il modo più semplice per stabilire una connessione a Scylla con TLS/SSL è senza la convalida dell'host remoto. Ciò non è consigliato per la produzione. Per impostazione predefinita, la convalida è abilitata. La convalida è controllata dalla variabile di ambiente `SSL_VALIDATE` o dalle impostazioni SSL nel file cqlshrc. Imposta `SSL_VALIDATE` su false nel tuo ambiente per disabilitare la convalida: `export SSL_VALIDATE=false`

In alternativa, puoi aggiungere quanto segue al tuo file cqlshrc.

```
[ssl]
validate = false
```

Se vuoi eseguirne la disabilitazione solo per un singolo comando `cqlsh`, inserisci `SSL_VALIDATE=false` prima del comando di connessione `cqlsh`. 

Devi comunque indicare a `cqlsh` di utilizzare TLSv1.2, anche quando non si convalida il certificato. Se non hai impostato la versione SSL nell'ambiente e non vuoi eseguire la convalida, la connessione dovrebbe essere simile a questo esempio:

```
SSL_VERSION=TLSv1_2 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Ottenimento e utilizzo del certificato

Se vuoi utilizzare cqlsh e verificare l'host remoto, dovrai ottenere il certificato, salvarlo in un'ubicazione accessibile e fornire quindi a cqlsh il percorso al certificato. Una copia del certificato è disponibile nella pagina [Scylla and certificates](doc:scylla-and-certificates), insieme ai dettagli relativi al suo contenuto e alla sua origine. Crea un file certificato oppure salva una copia in locale. 

Imposta la variabile di ambiente `SSL_CERTFILE` sul percorso e il nome file del tuo certificato salvato per abilitarlo per tutti i successivi richiami di `cqlsh`. 

In alternativa, puoi aggiungere quanto segue al tuo file cqlshrc.
```
[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

Se vuoi solo utilizzare un certificato specifico per un singolo comando `cqlsh`, inserisci `SSL_CERTFILE = ~/path_to_your/lechain.pem` all'inizio del tuo comando di connessione cqlsh. Ad esempio:

```
SSL_VERSION=TLSv1_2 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Utilizzo di cqlsh

Se immetti `HELP` puoi visualizzare che la shell ha molte funzionalità. La cosa migliore è che tutti questi comandi hanno anche il completamento `TAB`. Ad esempio:
1. Immetti `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. Dovresti visualizzare le scelte per la classe di replica.
2. Puoi scegliere `SimpleStrategy` perché il cluster non sarà esteso in più datacenter.
3. Premi `<TAB><TAB>` nuovamente e immetti 3 per replication_factor. Quindi chiudi la parentesi con `}` e termina l'istruzione con `;<enter>`.

Hai appena creato il tuo primo KEYSPACE e lo hai reso predefinito per la replica dei tuoi dati in tutti e tre i nodi del tuo cluster.
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
