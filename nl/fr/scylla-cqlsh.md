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

# Utilisation de cqlsh
{: #using-cqlsh}

Vous pouvez vous connecter directement à {{site.data.keyword.composeForScyllaDB_full}} à l'aide de `cqlsh`.
Vous pouvez l'installer de plusieurs manières. Etant donné qu'il s'agit d'une application Python, l'exécution de `pip install cqlsh` installe la dernière version de la commande. `cqlsh` est un programme Python 2.7 et ne s'exécute pas avec Python 3.

Il est également possible d'obtenir `cqlsh` à partir d'une distribution complète de Cassandra. Pour ce faire, accédez au site [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/) qui propose le code binaire, le code source et les instructions d'installation de Cassandra sur des distributions Linux Debian (apt) et Red Hat (RPM). Téléchargez une version binaire et décompressez le fichier dans un répertoire. Accédez ensuite à ce répertoire (`cd`) pour rechercher `cqlsh` dans le répertoire `bin`.

La procédure d'obtention de cet outil sur votre unité locale dépend de votre plateforme locale. Le plus simple consiste à installer la dernière version de Cassandra en utilisant la méthode appropriée à votre plateforme (les versions les plus récentes prennent toujours en charge la version 2.1.8, version actuelle de Scylla) et d'utiliser la commande `cqlsh` intégrée. 

Sur Mac, par exemple, installez Cassandra avec `homebrew` en saisissant `brew install cassandra`.

## A propos des fichiers cqlshrc
Lorsqu'elle est exécutée, la commande `cqlsh` charge un fichier, par défaut `$HOME/.cassandra/cqlshrc`, qui peut définir de nombreux paramètres qu'il n'est pas possible de définir sur la ligne de commande. [Exemple Apache de cqlshrc](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample). Vous pouvez éditer le fichier par défaut pour définir des paramètres. Ces derniers s'appliquent alors à toutes les sessions sauf s'ils sont remplacés sur la ligne de commande.

Si vous exécutez plusieurs instances de Scylla dans des environnements différents, vous pouvez créer un fichier `cqlshrc` distinct et faire en sorte que `cqlsh` l'utilise en ajoutant `--cqlshrc=[filename]` dans votre commande.

### cqlshrc pour {{site.data.keyword.composeForScyllaDB}}
Voici un exemple de fichier `cqlshrc` minimal pour la connexion à votre service :
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

Entrez les informations fournies dans la section _Connection Strings_ du service et sauvegardez ce fichier en tant que example.cqlshrc.
Exécutez `cqlsh --ssl --cqlshrc=example.cqlshrc` à partir de la ligne de commande pour vous connecter à la base de données Scylla. Même si `cqlshrc` a une option permettant d'indiquer `ssl=true`, elle semble être ignorée ; vous devez donc employer l'indicateur `--ssl`.

Il n'est pas possible de définir la version SSL dans le fichier `cqlshrc`. Vous devez donc soit faire en sorte que l'environnement utilise TLSv1.2, soit ajouter SSL_VERSION=TLSv1_2 au début de la commande de connexion `cqlsh`.

## TLSv1.2

`cqlsh` n'utilise pas par défaut TLSv1.2 lors de la négociation d'une connexion cryptée. Afin de vous connecter à un déploiement Compose Scylla, vous devez indiquer la version TLS dans l'environnement exécutant `cqlsh`. Définissez SSL_VERSION dans l'environnement en saisissant `export SSL_VERSION=TLSv1_2` ou placez `SSL_VERSION=TLSv1_2` avant la commande de connexion `cqlsh`.

## Connexion sans validation

La façon la plus simple de se connecter à Scylla avec TLS/SSL consiste à ne pas valider l'hôte distant. Cette méthode n'est pas recommandée pour la production. Par défaut, la validation est activée. Elle est contrôlée par la variable d'environnement `SSL_VALIDATE` ou par les paramètres SSL du fichier cqlshrc. Définissez `SSL_VALIDATE` sur false dans votre environnement pour désactiver la validation : `export SSL_VALIDATE=false`

Vous pouvez également ajouter ce qui suit dans votre fichier cqlshrc. 

```
[ssl]
validate = false
```

Si vous souhaitez la désactiver uniquement pour une seule commande `cqlsh`, placez `SSL_VALIDATE=false` avant la commande de connexion `cqlsh`. 

Vous devez toujours indiquer à `cqlsh` d'utiliser TLSv1.2, même si vous ne validez pas le certificat. Si vous n'avez pas défini la version SSL dans l'environnement et que vous ne souhaitez procéder à la validation, la connexion doit être semblable à l'exemple suivant : 

```
SSL_VERSION=TLSv1_2 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Obtention et utilisation du certificat

Si vous souhaitez utiliser cqlsh et vérifier l'hôte distant, vous devez obtenir le certificat, le sauvegarder dans un emplacement accessible, puis indiquer son chemin d'accès à cqlsh. Vous trouverez une copie du certificat sur la page [Scylla et certificats](doc:scylla-and-certificates), ainsi que des détails sur son contenu et sa source. Créez un fichier certificat ou sauvegardez une copie en local.  

Indiquez le chemin et le nom de fichier de votre certificat sauvegardé dans la variable d'environnement `SSL_CERTFILE` afin de l'activer pour les appels ultérieurs de `cqlsh`. 

Vous pouvez également ajouter ce qui suit dans votre fichier cqlshrc : 
```
[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

Si vous souhaitez uniquement utiliser un certificat particulier pour une seule commande `cqlsh`, placez `SSL_CERTFILE = ~/path_to_your/lechain.pem` au début de la commande de connexion à cqlsh. Par exemple : 

```
SSL_VERSION=TLSv1_2 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Utilisation de cqlsh

Si vous entrez `HELP` vous voyez que le shell dispose de nombreuses capacités. Le plus beau est que toutes ces commandes ont également une option de saisie semi-automatique via la touche `TAB`. Par exemple : 
1. Entrez `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. Les choix disponibles pour la classe de réplication s'affichent.
2. Ici, vous pouvez sélectionner `SimpleStrategy` car le cluster ne couvrira pas plusieurs centres de données.
3. Appuyez de nouveau sur `<TAB><TAB>` et entrez la valeur 3 dans la zone replication_factor. Fermez ensuite l'accolade avec `}` et terminez l'instruction avec le signe `;<enter>`.

Vous venez de créer votre premier espace de clés (KEYSPACE), défini par défaut sur la réplication de vos données dans les trois noeuds de votre cluster.
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

Vous pouvez maintenant utiliser votre espace de clés :
```sql 
USE my_new_keyspace;
```
Votre shell indique désormais que votre invite de commande utilise votre espace de clés par défaut. Chaque table doit disposer d'un espace de clés.

Saisissez la commande `CREATE TABLE` suivante dans le shell pour créer une table et la remplir avec des exemples.
```sql
CREATE TABLE my_new_table (
    my_table_id uuid,
    last_name text,
    first_name text,
    PRIMARY KEY(my_table_id)
);
```
