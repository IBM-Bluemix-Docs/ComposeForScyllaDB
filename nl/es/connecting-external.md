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

# Conexión de una aplicación externa
{: #connecting-external-app}

Encontrará la información que necesita para establecer conexión con {{site.data.keyword.composeForScyllaDB_full}} en la página *Visión general* del servicio {{site.data.keyword.composeForPostgreSQL_full}}.

Puede conectarse directamente a {{site.data.keyword.composeForScyllaDB}} utilizando `cqlsh`. Cómo se obtiene esta herramienta en el dispositivo local depende de la plataforma local. Lo más sencilla es instalar el último release de Cassandra utilizando un método adecuado para su plataforma (la última versión todavía da soporte a la versión 2.1.8, que es la de Scylla) y utilizar el mandato incorporado `cqlsh`.

En un Mac, por ejemplo:

1. Instale Cassandra con `homebrew` escribiendo `brew install cassandra`.
2. Copie cualquiera de los mandatos de la página Visión general:

  ![Ejemplo de serie de conexión `cqlsh`](./cqlsh_connection_string "Ejemplo de serie de conexión cqlsh")

3. Pegue el mandato en el shell para ejecutarlo:

  ![El shell de `cqlsh`.](./cqlsh_shell.png "El shell de cqlsh")

4. Si escribe `HELP`, verá que el shell tiene mucha capacidad. Y lo mejor es que todos estos mandatos también tienen la función `TAB`.
5. Escriba `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. Debería ver las opciones para la clase de réplica.
6. Aquí puede elegir `SimpleStrategy` porque el clúster no abarcará varios centros de datos.
7. Pulse `<TAB><TAB>` de nuevo y escriba 3 para replication_factor. Cierre las llaves con `}` y finalice la sentencia con `;<enter>`.

  Acaba de crear el primer espacio de clave, que de forma predeterminada realiza una réplica de los datos en los tres nodos del clúster.

8. Ahora puede utilizar el espacio de clave:

  ```sql
  USE my_new_keyspace;
  ```

  Ahora el shell muestra que el indicador de mandatos utiliza el espacio de clave de forma predeterminada:

  ![Ejecución de `CREATE KEYSPACE` y `USE`.](./images/running_create_keyspace_use.png "Ejecución de `CREATE KEYSPACE` y `USE`")

  Cada tabla debe tener un espacio de clave. Cuando cree uno aquí en el shell, tomará como valor predeterminado `my_new_keyspace`.

  Aunque Scylla/Cassandra ha evolucionado para tener un lenguaje de esquema muy parecido a SQL, este no es realmente el caso. A diferencia de RDBMS, aquí una fila se parece más a una búsqueda de valor de clave. Simplemente ocurre que el valor tiene un esquema flexible que vamos a definir:

9. Escriba el siguiente mandato `CREATE TABLE` en el shell para crear un lugar que llenar con ejemplos.

  ```sql
  CREATE TABLE my_new_table (
    my_table_id uuid,
    last_name text,
    first_name text,
    PRIMARY KEY(my_table_id)
  );
  ```

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

El soporte de otros lenguajes, además de Java, también es muy completo. Python es un buen ejemplo. `cqlsh` está incluso escrito en Python. Así que no dude de que este soporte está más que actualizado:

```shell
pip install cassandra-driver
```

Esto extrae el controlador con el gestor de paquetes de python `pip`. El siguiente código funciona de forma muy similar al código Java en cuanto a la preparación de una sentencia y a la ejecución de una inserción:

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
