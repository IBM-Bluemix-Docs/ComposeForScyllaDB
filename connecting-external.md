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

# Connecting an external application
{: #connecting-external-app}

You can find the information you need to connect to {{site.data.keyword.composeForScyllaDB_full}} on the *Overview* page of your {{site.data.keyword.composeForPostgreSQL_full}} service.

## Connecting from the JVM

One of the most advanced drivers for Cassandra is the Java driver. This makes sense considering Cassandra is written in Java. What follows is a [Groovy](http://www.groovy-lang.org/documentation.html#gettingstarted) script. For those who utilize just about any JVM language translating from Groovy to your language of choice should be relatively straightforward:

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

To get started we pull in the latest Cassandra driver:

```java
@Grab('com.datastax.cassandra:cassandra-driver-core:3.1.0')
```

After all of the imports we use a `Cluster.builder()` to build up the configuration. Just one  of the `ContactPoint`s is used to connect. From that connection the other nodes in the cluster are discovered. If that `ContactPoint` is unreachable on `connect` then another is used which is why we add all three.

`PreparedStatement`s may be familiar since they are analogous to other DBs' features of the same name. The statement is parsed and held at the server ready to be used over and over again. The following calls to `bind` and `execute` populate and send the data over to the server for actual execution. While there are simpler methods for one off execution, it is good to highlight such a useful feature.

To prove that the script works go back to your `cqlsh` and query the table:
![Results from `SELECT` in `cqlsh`.](./images/results_select_java.png "Results from Select")

## Connecting from Python

To connect your python application, use the [DataStax Python Driver](https://github.com/datastax/python-driver). Installation can be done through pip:

```shell
pip install cassandra-driver
```

This pulls in the driver with a python package manager `pip`. The driver expects a cetificate to use when TLS/SSL is enabled, and instructions for using a certificate with your service are on the [Using LE Certificates](./scylla-certificates.html) page. All the other information the driver needs is in the _Connection Strings_ of the service _Overview_.

The following performs very similarly to the Java code of preparing a statement and executing an insert:

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

To verify again we'll run the same `SELECT` command:

![Results from `SELECT` in `cqlsh`.](./images/results_select_python.png "Results from Select")

## Connecting from Node.js

Use node package manager (npm) to install the driver and the needed `uuid` library.

```shell
npm install cassandra-driver
npm install uuid
```

 The code is similar to the previous examples:

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

Once again the code connects, and prepares and executes an insert statement. Verify the code with the SELECT command:

![Results from `SELECT` in `cqlsh`.](./images/results_select_node.png "Results from Select")
