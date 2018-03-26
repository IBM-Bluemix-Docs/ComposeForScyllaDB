---

copyright:
  years: 2017,2018
lastupdated: "2017-06-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connessione a un'applicazione esterna
{: #connecting-external-app}

Puoi trovare le informazioni di cui hai bisogno per il collegamento a {{site.data.keyword.composeForScyllaDB_full}} nella pagina *Overview* del tuo servizio {{site.data.keyword.composeForPostgreSQL_full}}.

Non puoi collegarti direttamente a {{site.data.keyword.composeForScyllaDB}} utilizzando `cqlsh`. Come ottieni questo strumento nel tuo dispositivo locale dipende dalla tua piattaforma locale. Il modo più facile è di installare l'ultimo release Cassandra utilizzando un metodo appropriato per la tua piattaforma (le ultime versioni supportano ancora la versione 2.1.8 che è quella di Scylla) e di utilizzare il comando `cqlsh` integrato.

Su un Mac, ad esempio:

1. Installa Cassandra con `homebrew` immettendo `brew install cassandra`.
2. Copia i comandi dalla pagina della panoramica:

  ![Stringa di connessione `cqlsh` di esempio.](./cqlsh_connection_string "Stringa di connessione cqlsh di esempio")

3. Incolla il comando nella tua shell ed eseguilo:

  ![La shell `cqlsh`.](./cqlsh_shell.png "La shell cqlsh")

4. Se immetti `HELP` puoi visualizzare che la shell ha molte funzionalità. La cosa migliore è che tutti questi comandi hanno anche il completamento `TAB`.
5. Immetti `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. Dovresti visualizzare le scelte per la classe di replica.
6. Puoi scegliere `SimpleStrategy` perché il cluster non sarà esteso in più datacenter.
7. Premi `<TAB><TAB>` nuovamente e immetti 3 per replication_factor. Quindi chiudi la parentesi con `}` e termina l'istruzione con `;<enter>`.

  Hai appena creato il tuo primo KEYSPACE e lo hai reso predefinito per la replica dei tuoi dati in tutti e tre i nodi del tuo cluster.

8. Ora puoi utilizzare il keyspace:

  ```sql
  USE my_new_keyspace;
  ```

  La tua shell ora visualizza che il prompt dei comandi sta utilizzando il tuo keyspace per impostazione predefinita:

  ![Esecuzione di `CREATE KEYSPACE` e `USE`.](./images/running_create_keyspace_use.png "Esecuzione di `CREATE KEYSPACE` e `USE`")

  Ogni tabella ha un keyspace. Quando ne crei uno nella shell per impostazione predefinita sarà `my_new_keyspace`.

  Mentre Scylla/Cassandra si sono evoluti nell'utilizzare un linguaggio dello schema molto simile a SQL. Non è propriamente questo il caso. Diversamente da un RDBMS, una riga qui è molto più simile a una ricerca valore chiave. Succede così che il valore ha uno schema flessibile che stiamo per definire:

9. Immetti il seguente comando `CREATE TABLE` nella shell per creare una posizione da popolare con gli esempi.

  ```sql
  CREATE TABLE my_new_table (
    my_table_id uuid,
    last_name text,
    first_name text,
    PRIMARY KEY(my_table_id)
  );
  ```

## Connessione dalla JVM

Uno dei driver più avanzati per Cassandra è il driver Java. Questo ha senso considerando che Cassandra è scritto in Java. Il seguente è uno script [Groovy](http://www.groovy-lang.org/documentation.html#gettingstarted). Per coloro che utilizzano praticamente qualsiasi linguaggio JVM per la traduzione da Groovy al proprio linguaggio di scelta dovrebbe essere relativamente semplice:

```java
@Grab('com.datastax.cassandra:cassandra-driver-core:3.1.0')
@Grab('org.slf4j:slf4j-log4j12')

import com.datastax.driver.core.BoundStatement
import com.datastax.driver.core.Cluster
import com.datastax.driver.core.Host
import com.datastax.driver.core.PreparedStatement
import com.datastax.driver.core.Row
import com.datastax.driver.core.Session

import static java.util.UUID.randomUUID

Cluster cluster = Cluster.builder()
    .addContactPointsWithPorts(
        new InetSocketAddress("aws-us-east-1-portal9.dblayer.com", 15399 ),
        new InetSocketAddress("aws-us-east-1-portal9.dblayer.com", 15401 ),
        new InetSocketAddress("aws-us-east-1-portal6.dblayer.com", 15400 )
    )
    .withCredentials("scylla", "XOEDTTBPZGYAZIQD")
    .build()

Session session = cluster.connect("my_new_keyspace")

PreparedStatement myPreparedInsert = session.prepare(
  """INSERT INTO my_new_table(my_table_id, last_name, first_name)
     VALUES (?,?,?)""")

BoundStatement myInsert = myPreparedInsert
    .bind(randomUUID(), "Hutton", "Hays")

session.execute(myInsert)

session.close()
cluster.close()
```

Per iniziare inseriamo l'ultimo driver Cassandra:

```java
@Grab('com.datastax.cassandra:cassandra-driver-core:3.1.0')
```

Dopo aver importato tutto utilizziamo `Cluster.builder()` per creare la configurazione. Solo uno dei `ContactPoint` è utilizzato per il collegamento. Da tale connessione gli altri nodi nel cluster sono rilevati. Se questo `ContactPoint` non è raggiungibile su `connect` ne viene utilizzato un altro, questo è il motivo per cui li aggiungiamo tutti e tre.

I `PreparedStatement` potrebbe essere familiari poiché sono analoghi ad altre funzioni del DB con lo stesso nome. L'istruzione viene analizzata e conservata nel server pronta per essere utilizzata all'infinito. Le seguenti chiamate a `bind` e `execute` popolano e inviano i dati al server per l'esecuzione corrente. Mentre esistono metodi più semplici per un'unica esecuzione, è bene sottolineare che è una funzione utile.

Per dimostrare che lo script funziona, torna a `cqlsh` ed esegui la query della tabella:
![Risultati da `SELECT` in `cqlsh`.](./images/results_select_java.png "Risultati da Select")

## Connessione da Python

Anche il supporto per linguaggi diversi da Java è molto solido. Python è un ottimo esempio. `cqlsh` è ancora scritto in Python. Per cui non commettere errori, il supporto qui è più che aggiornato:

```shell
pip install cassandra-driver
```

Questo si inserisce nel driver con un gestore del pacchetto python `pip`. Quanto segue viene elaborato in modo molto simile al codice Java di preparazione di un'istruzione e di esecuzione di un inserimento:

```python
from cassandra.cluster import Cluster
from cassandra.auth import PlainTextAuthProvider
import uuid

auth_provider = PlainTextAuthProvider(
                  username='scylla',
                  password='XOEDTTBPZGYAZIQD')

cluster = Cluster(
            contact_points = ["aws-us-east-1-portal9.dblayer.com"],
            port = 15401,
            auth_provider = auth_provider)

session = cluster.connect('my_new_keyspace')

my_prepared_insert = session.prepare("""
    INSERT INTO my_new_table(my_table_id, first_name, last_name)
    VALUES (?, ?, ?)""")

session.execute(my_prepared_insert, [uuid.uuid4(), 'Snake', 'Hutton'])
```

Per verificare nuovamente rieseguiremo lo stesso comando `SELECT`:

![Risultati da `SELECT` in `cqlsh`.](./images/results_select_python.png "Risultati da Select")

## Connessione da Node.js

Utilizza il npm (node package manager) per installare il driver e la libreria `uuid` necessaria.

```shell
npm install cassandra-driver
npm install uuid
```

 Il codice è simile ai precedenti esempi:

```javascript
var cassandra = require('cassandra-driver')
var authProvider = new cassandra.auth.PlainTextAuthProvider('scylla', 'XOEDTTBPZGYAZIQD')
var uuid = require('uuid')

client = new cassandra.Client({
                        contactPoints: [
                          "aws-us-east-1-portal9.dblayer.com:15399",
                          "aws-us-east-1-portal9.dblayer.com:15401",
                          "aws-us-east-1-portal6.dblayer.com:15400"
                        ],
                        keyspace: 'my_new_keyspace',
                        authProvider: authProvider});

client.execute("INSERT INTO my_new_table(my_table_id, first_name, last_name) VALUES(?,?,?)",
               [uuid.v4(), "V8", "Hutton"],
               { prepare: true },
               function(err, result) {
                 if(err) { console.error(err); }
                 console.log("success")
               });

```

Ancora una volta il codice collega, prepara ed esegue un'istruzione di inserimento. Verifica il codice con il comando SELECT:

![Risultati da `SELECT` in `cqlsh`.](./images/results_select_node.png "Risultati da Select")
