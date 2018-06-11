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

# Connexion d'une application externe
{: #connecting-external-app}

Vous trouverez les informations nécessaire pour vous connecter à {{site.data.keyword.composeForScyllaDB_full}} sur la page *Vue d'ensemble* du service {{site.data.keyword.composeForPostgreSQL_full}}.

## Connexion à partir de la machine virtuelle Java

L'un des pilotes les plus évolués pour Cassandra est le pilote Java. C'est une évidence sachant que Cassandra est écrit en Java. Ce qui suit est un script [Groovy](http://www.groovy-lang.org/documentation.html#gettingstarted). Ceux qui utilisent pratiquement tous les langages JVM traduits de Groovy dans le langage de leur choix trouveront ce script relativement simple :

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

Pour commencer nous recherchons le pilote Cassandra le plus récent :

```java
@Grab('com.datastax.cassandra:cassandra-driver-core:3.1.0')
```

Une fois toutes les importations effectuées, nous utilisons `Cluster.builder()` pour élaborer la configuration. Un seul `ContactPoint` est utilisé pour la connexion. A partir de cette connexion, les autres noeuds du cluster sont découverts. Si ce `ContactPoint` n'est pas accessible sur `connect`, un autre est utilisé. Voilà pourquoi nous avons ajouté les trois.

Les éléments `PreparedStatement` vous sont probablement familiers puisqu'ils sont similaires à d'autres fonctions de base de données de même nom. L'instruction est analysée et mise en attente jusqu'à ce que le serveur soit prêt pour l'utiliser ensuite à volonté. Les appels suivants de `bind` et `execute` désignent les données et les envoient au serveur pour exécution effective. Même s'il existe des méthodes plus simples pour une exécution ponctuelle, il est utile de mettre en évidence une fonction aussi intéressante.

Pour prouver que le script fonctionne, revenez à `cqlsh` et interrogez la table :
![Résultats de `SELECT` dans `cqlsh`.](./images/results_select_java.png "Résultats de Select")

## Connexion à partir de Python

Pour vous connecter à votre application python, faites appel à [DataStax Python Driver](https://github.com/datastax/python-driver). L'installation peut être effectuée via pip :

```shell
pip install cassandra-driver
```

Ce code extrait le pilote avec un gestionnaire de package python `pip`. Le pilote attend un certificat à employer lorsque la fonction TLS/SSL est activée ; vous trouverez des instructions relatives à l'utilisation d'un certificat avec votre service sur la page [Using LE Certificates](./scylla-certificates.html). Toutes les autres informations exigées par le pilote figurent dans la section _Connection Strings_ de la vue d'ensemble du service.

Le code qui suit fonctionne sensiblement de la même manière que le code Java de préparation d'une instruction et d'exécution d'une insertion :

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

Pour vérifier de nouveau, nous allons exécuter la même commande `SELECT` :

![Résultats de `SELECT` dans `cqlsh`.](./images/results_select_python.png "Résultats de Select")

## Connexion à partir de Node.js

Utilisez npm (node package manager) pour installer le pilote et la bibliothèque `uuid` requise.

```shell
npm install cassandra-driver
npm install uuid
```

 Le code est semblable à celui des exemples précédents :

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

Une fois encore, le code se connecte, prépare et exécute une instruction d'insertion. Vérifiez le code avec la commande SELECT :

![Résultats de `SELECT` dans `cqlsh`.](./images/results_select_node.png "Résultats de Select")
