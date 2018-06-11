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

# Externe Anwendung verbinden
{: #connecting-external-app}

Sie finden die Informationen, die Sie zum Herstellen einer Verbindung zu {{site.data.keyword.composeForScyllaDB_full}} benötigen, auf der Seite *Übersicht* Ihres {{site.data.keyword.composeForPostgreSQL_full}}-Service.

## Verbindung über die JVM herstellen

Einer der innovativsten Treiber für Cassandra ist der Java-Treiber. Das ist verständlich, wenn man bedenkt, dass Cassandra in Java geschrieben wurde. Nachstehend finden Sie ein [Groovy](http://www.groovy-lang.org/documentation.html#gettingstarted)-Script. Für jemanden, der fast alle JVM-Sprachen nutzt, sollte die Umsetzung von Groovy in die Sprache Ihrer Wahl relativ problemlos funktionieren:

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

Als Erstes binden Sie den neuesten Cassandra-Treiber ein:

```java
@Grab('com.datastax.cassandra:cassandra-driver-core:3.1.0')
```

Nach alle Importen erstellen Sie die Konfiguration mithilfe von `Cluster.builder()`. Für die Verbindung wird nur ein einziger Kontaktpunkt (`ContactPoint`) verwendet. Von dieser Verbindung aus werden die anderen Knoten im Cluster erkannt. Wenn dieser Kontaktpunkt (`ContactPoint`) bei einem `connect`-Befehl nicht erreichbar ist, wird ein anderer verwendet. Deshalb verwenden Sie alle drei.

Vorbereitete Anweisungen (`PreparedStatement`) könnten vertraut sein, da sie analog zu den anderen Datenbankfunktionen mit demselben Namen sind. Die Anweisung wird geparst und steht auf dem Server zur mehrfachen Verwendung bereit. Mit den folgenden Aufrufen von `bind` und `execute` werden die Daten gefüllt und zur eigentlichen Ausführung an den Server gesendet. Zwar gibt es einfachere Methoden für eine einmalige Ausführung, doch ist es gut, solch eine nützliche Funktion hervorzuheben.

Kehren Sie zu Ihrem Befehl `cqlsh` zurück, um zu beweisen, dass das Script funktioniert, und fragen Sie die Tabelle ab:
![Ergebnisse von 'SELECT' in 'cqlsh'.](./images/results_select_java.png "Ergebnisse des Befehls 'Select'")

## Verbindung über Python herstellen

Verwenden Sie den [DataStax Python-Treiber](https://github.com/datastax/python-driver), um eine Verbindung zu Ihrer Python-Anwendung herzustellen. Die Installation kann über pip erfolgen: 

```shell
pip install cassandra-driver
```

Dadurch wird der Treiber mit einem Python-Paketmanager namens `pip` integriert. Der Treiber erwartet die Verwendung eines Zertifikats, wenn TLS/SSL aktiviert ist. Anweisungen zur Verwendung eines Zertifikats mit Ihrem Service befinden sich auf der Seite [LE-Zertifikate verwenden](./scylla-certificates.html). Der Treiber benötigt außerdem nur noch Informationen, die sich in den _Verbindungszeichenfolgen_ der _Übersicht_ des Service befinden.

Der folgende Code funktioniert ähnlich wie der Java-Code zum Vorbereiten einer Anweisung und zum Ausführen einer Einfügung:

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

Zur Überprüfung müssen Sie denselben Befehl `SELECT` noch einmal ausführen:

![Ergebnisse von 'SELECT' in 'cqlsh'.](./images/results_select_python.png "Ergebnisse des Befehls 'Select'")

## Verbindung über Node.js herstellen

Installieren Sie den Treiber und die benötigte Bibliothek `uuid` mit Node Package Manager (npm).

```shell
npm install cassandra-driver
npm install uuid
```

 Der Code ähnelt dem der obigen Beispiele:

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

Der Code stellt noch einmal eine Verbindung her, bereitet eine Einfügeanweisung vor und führt diese aus. Überprüfen Sie den Code mit dem Befehl SELECT:

![Ergebnisse von 'SELECT' in 'cqlsh'.](./images/results_select_node.png "Ergebnisse des Befehls 'Select'")
