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

# Connexion d'une application externe
{: #connecting-external-app}

Vous trouverez les informations nécessaire pour vous connecter à {{site.data.keyword.composeForScyllaDB_full}} sur la page *Vue d'ensemble* du service {{site.data.keyword.composeForPostgreSQL_full}}.

Vous pouvez vous connecter directement à {{site.data.keyword.composeForScyllaDB}} à l'aide de `cqlsh`. La procédure d'obtention de cet outil sur votre unité locale dépend de votre plateforme locale. Le plus simple consiste à installer la dernière version de Cassandra en utilisant la méthode appropriée à votre plateforme (les versions les plus récentes prennent toujours en charge la version 2.1.8, version actuelle de Scylla) et d'utiliser la commande `cqlsh` intégrée.

Sur un Mac, par exemple :

1. Installez Cassandra avec `homebrew` en entrant `brew install cassandra`.
2. Copiez toutes les commandes de la page de présentation :

  ![Example `cqlsh` connection string.](./cqlsh_connection_string "Example cqlsh connection string")

3. Collez la commande dans votre shell pour exécution :

  ![The `cqlsh` shell.](./cqlsh_shell.png "The cqlsh shell")

4. Si vous entrez `HELP` vous voyez que le shell dispose de nombreuses capacités. Le plus beau est que toutes ces commandes ont également une option de saisie semi-automatique via la touche `TAB`.
5. Entrez `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. Les choix disponibles pour la classe de réplication s'affichent.
6. Ici, vous pouvez sélectionner `SimpleStrategy` car le cluster ne couvrira pas plusieurs centres de données.
7. Appuyez de nouveau sur `<TAB><TAB>` et entrez la valeur 3 dans la zone replication_factor. Fermez ensuite l'accolade avec `}` et terminez l'instruction avec le signe `;<enter>`.

  Vous venez de créer votre premier espace de clés (KEYSPACE), défini par défaut sur la réplication de vos données dans les trois noeuds de votre cluster.

8. Vous pouvez maintenant utiliser votre espace de clés :

  ```sql
  USE my_new_keyspace;
  ```

  Votre shell indique désormais que votre invite de commande utilise votre espace de clés par défaut :

  ![Exécution de `CREATE KEYSPACE` et `USE`.](./images/running_create_keyspace_use.png "Exécution de`CREATE KEYSPACE` et `USE`")

  Chaque table doit disposer d'un espace de clés. Lorsque vous en créez un ici dans le shell, il est par défaut nommé `my_new_keyspace`.

  Scylla/Cassandra a évolué de manière à s'approcher du schéma de langage SQL. Mais ce n'est pas réellement le cas. A la différence de SGBDR (système de gestion de base de données relationnelle), une ligne ici correspond plus à une recherche de valeur de clé. Il se trouve simplement que la valeur dispose d'un schéma flexible que nous allons définir :

9. Entrez la commande `CREATE TABLE` suivante dans le shell afin de créer un espace à remplir avec des exemples.

  ```sql
  CREATE TABLE my_new_table (
    my_table_id uuid,
    last_name text,
    first_name text,
    PRIMARY KEY(my_table_id)
  );
  ```

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

Une fois toutes les importations effectuées, nous utilisons `Cluster.builder()` pour élaborer la configuration. Une seul `ContactPoint` est utilisé pour la connexion. A partir de cette connexion, les autres noeuds du cluster sont découverts. Si ce `ContactPoint` n'est pas accessible sur `connect`, un autre est utilisé. Voilà pourquoi nous avons ajouté les trois.

Les éléments `PreparedStatement` vous sont probablement familiers puisqu'ils sont similaires à d'autres fonctions de base de données de même nom. L'instruction est analysée et mise en attente jusqu'à ce que le serveur soit prêt pour l'utiliser ensuite à volonté. Les appels suivants de `bind` et `execute` désignent les données et les envoient au serveur pour exécution effective. Même s'il existe des méthodes plus simples pour une exécution ponctuelle, il est utile de mettre en évidence une fonction aussi intéressante.

our prouver que le script fonctionne, revenez à `cqlsh` et interrogez la table :
![Résultats de `SELECT` dans `cqlsh`.](./images/results_select_java.png "Résultats de Select")

## Connexion à partir de Python

La prise en charge d'autres langages Java est également très performante. Python en est un bon exemple. `cqlsh` est souvent écrit en Python. Mais pas d'erreur, la prise en charge est ici plus que réelle :

```shell
pip install cassandra-driver
```

Ce code extrait le pilote avec un gestionnaire de package python `pip`. Le code qui suit fonctionne sensiblement de la même manière que le code Java de préparation d'une instruction et d'exécution d'une insertion :

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
