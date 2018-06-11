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

# Conexión de una aplicación externa
{: #connecting-external-app}

Encontrará la información que necesita para establecer conexión con {{site.data.keyword.composeForScyllaDB_full}} en la página *Visión general* del servicio {{site.data.keyword.composeForPostgreSQL_full}}.

## Conexión desde JVM

Uno de los controladores más avanzados para Cassandra es el controlador Java. Esto tiene sentido teniendo en cuenta que Cassandra está escrito en Java. A continuación se muestra un script [Groovy](http://www.groovy-lang.org/documentation.html#gettingstarted). Para aquellos que utilicen cualquier lenguaje JVM, la conversión de Groovy al lenguaje que elija debe ser relativamente sencilla:

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

Para empezar, extraemos el último controlador de Cassandra:

```java
@Grab('com.datastax.cassandra:cassandra-driver-core:3.1.0')
```

Después de todas las importaciones, utilizamos `Cluster.builder()` para crear la configuración. Solo se utiliza un `ContactPoint` para establecer la conexión. Desde esta conexión se descubren los otros nodos del clúster. Si no se puede acceder a este `ContactPoint` en `connect`, se utiliza otro; por eso hemos añadido los tres.

Las sentencias `PreparedStatement` pueden parecerle familiares porque son análogas a otras características de las bases de datos del mismo nombre. La sentencia se analiza y se mantiene en el servidor, lista para ser utilizada varias veces. Las siguientes llamadas a `bind` y `execute` llenan y envían los datos al servidor para su ejecución real. Aunque hay métodos más sencillos para una ejecución puntual, vale la pena destacar esta característica tan útil.

Para comprobar que el script funciona, vuelva a `cqlsh` y consulte la tabla: ![Resultados de `SELECT` en `cqlsh`.](./images/results_select_java.png "Resultados de Select")

## Conexión desde Python

Para conectar la aplicación de Python, utilice el [controlador de Python DataStax](https://github.com/datastax/python-driver). La instalación puede realizarse mediante pip:

```shell
pip install cassandra-driver
```

Esto extrae el controlador con el gestor de paquetes de python `pip`. El controlador espera un certificado para utilizarlo cuando se habilita TLS/SSL. Las instrucciones para utilizar un certificado con el servicio están en la página [Utilización de certificados de LE](./scylla-certificates.html). El resto de la información que necesita el controlador está en la sección _Series de conexión_ de la _Visión general_ del servicio.

El siguiente código funciona de forma muy similar al código Java en cuanto a la preparación de una sentencia y a la ejecución de una inserción:

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

Para verificarlo nuevamente, vamos a ejecutar el mismo mandato `SELECT`:

![Resultados de `SELECT` en `cqlsh`.](./images/results_select_python.png "Resultados de Select")

## Conexión desde Node.js

Utilice el gestor de paquetes de nodo (npm) para instalar el controlador y la biblioteca `uuid` necesaria.

```shell
npm install cassandra-driver
npm install uuid
```

 El código es similar al de los ejemplos anteriores:

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

Una vez más, el código se conecta y prepara y ejecuta una sentencia insert. Verifique el código con el mandato SELECT:

![Resultados de `SELECT` en `cqlsh`.](./images/results_select_node.png "Resultados de Select")
