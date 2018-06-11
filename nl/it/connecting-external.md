---

copyright:
  years: 2017,2018
lastupdated: "2018-02-28"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connessione a un'applicazione esterna
{: #connecting-external-app}

Puoi trovare le informazioni di cui hai bisogno per il collegamento a {{site.data.keyword.composeForScyllaDB_full}} nella pagina *Overview* del tuo servizio {{site.data.keyword.composeForPostgreSQL_full}}.

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

Per connettere la tua applicazione Python, utilizza il [driver DataStax Python](https://github.com/datastax/python-driver). L'installazione può essere eseguita tramite pip:

```shell
pip install cassandra-driver
```

Questo si inserisce nel driver con un gestore del pacchetto python `pip`. Il driver prevede un certificato da utilizzare quando TLS/SSL è abilitato; le istruzioni per l'utilizzo di un certificato con il tuo servizio sono disponibili nella pagina relativa all'[utilizzo dei certificati LE](./scylla-certificates.html). Tutte le altre informazioni di cui ha bisogno il driver sono disponibili nelle _Stringhe di connessione_ della _Panoramica_ del servizio.

Quanto segue viene elaborato in modo molto simile al codice Java di preparazione di un'istruzione e di esecuzione di un inserimento:

```python
import ssl
import uuid
from cassandra.cluster import Cluster
from cassandra.auth import PlainTextAuthProvider

ssl_options = {
    "ca_certs":"/path_to_cert/lecert.pem",
    "ssl_version":ssl.PROTOCOL_TLSv1_2
}

auth_provider = PlainTextAuthProvider(
                  username='scylla',
                  password='your_password')

cluster = Cluster(
            contact_points = [
                "portal-59b813def16d6e0014003b45224-8.bmix-dal-ys1-fd6a5b7e-e120-43f3-95ea-e40028e540a8.composeci-us-ibm-com.composedb.com",
                "portal-59b813def16d6e0014003b47186-6.bmix-dal-ys1-fd6a5b7e-e120-43f3-95ea-e40028e540a8.composeci-us-ibm-com.composedb.com",
                "portal-59b813def16d6e0014003b49208-7.bmix-dal-ys1-fd6a5b7e-e120-43f3-95ea-e40028e540a8.composeci-us-ibm-com.composedb.com"
                ],
            port = 15401,
            auth_provider = auth_provider,
            ssl_options=ssl_options)

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
