---

copyright:
  years: 2017
lastupdated: "2017-06-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connecting an external application
{: #connecting-external-app}

You can find the information you need to connect to {{site.data.keyword.composeForScyllaDB}} on the *Overview* page of your {{site.data.keyword.composeForPostgreSQL}} service.

You can connect directly to {{site.data.keyword.composeForScyllaDB}} using `cqlsh`. How you get this tool onto your local device depends on your local platform. The easiest is to install the latest Cassandra release using an appropriate method for your platform (the latest versions still support version 2.1.8 which is what Scylla is) and use the builtin `cqlsh` command.

On a Mac, for example:

1. Install Cassandra with `homebrew` by typing `brew install cassandra`.
2. Copy any of the commands from the Overview page:

  ![Example `cqlsh` connection string.](./cqlsh_connection_string "Example cqlsh connection string")

3. Paste the command into your shell to execute it:

  ![The `cqlsh` shell.](./cqlsh_shell.png "The cqlsh shell")

4. If you type `HELP` you can see that the shell has a lot of capability. What's even nicer is that all of those commands have `TAB` completion too.
5. Type `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. You should see the choices for the replication class.
6. You can choose `SimpleStrategy` here because the cluster won't be spanning multiple data centers.
7. Press `<TAB><TAB>` again and enter in 3 for the replication_factor. Then close the brace with `}` and finish the statement with `;<enter>`.

  You just created your first KEYSPACE and defaulted it to replicating your data to all three nodes in your cluster.

8. Now you can use the keyspace:

  ```sql
  USE my_new_keyspace;
  ```

  Your shell now shows that your command prompt is using your keyspace by default:

  ![Running `CREATE KEYSPACE` and `USE`.](./images/running_create_keyspace_use.png "Running `CREATE KEYSPACE` and `USE`")

  Every table has to have a keyspace. When you create one in the shell here it will default to `my_new_keyspace`.

  While Scylla/Cassandra has evolved into having a schema language that looks very similar to SQL. It's not really the case. Unlike an RDBMS, a row here is much more like a key value lookup. It just so happens that the value has a flexible schema which we are about to define:

9. Type the following `CREATE TABLE` command in the shell to create a place to populate with  examples.

  ```sql
  CREATE TABLE my_new_table (
    my_table_id uuid,
    last_name text,
    first_name text,
    PRIMARY KEY(my_table_id)
  );
  ```

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

Support for languages other than Java is very solid too. Python is a great example. `cqlsh` is even written in Python. So make no mistake the support here is more than up to date:

```shell
pip install cassandra-driver
```

This pulls in the driver with a python package manager `pip`. The following performs very similarly to the Java code of preparing a statement and executing an insert:

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
