---

copyright:
  years: 2018
lastupdated: "2018-05-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilisation de cqlsh
{: #using-cqlsh}

Vous pouvez utiliser `cqlsh` pour vous connecter directement à {{site.data.keyword.composeForScyllaDB_full}}.

`cqlsh` est un programme Python 2.7 et ne s'exécute pas avec Python 3.
{: .tip}

Etant donné que `cqlsh` est une application Python, vous pouvez exécuter `pip install cqlsh` pour installer la dernière version de la commande.

Vous pouvez également obtenir `cqlsh` à partir d'une distribution complète de Cassandra en la téléchargeant depuis le site [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/), qui propose le code binaire, le code source et des instructions d'installation de Cassandra sur des distributions Linux Debian (apt) et Red Hat (RPM). Téléchargez une version binaire et décompressez le fichier dans un répertoire. Accédez ensuite à ce répertoire (`cd`) pour rechercher `cqlsh` dans le répertoire `bin`.

La procédure d'obtention de cet outil sur votre unité locale dépend de votre système d'exploitation local. Le plus simple consiste à installer la dernière version de Cassandra en utilisant la méthode appropriée à votre système d'exploitation (les versions les plus récentes prennent toujours en charge la version 2.1.8, version actuelle de Scylla), puis d'utiliser la commande `cqlsh` intégrée. 

Sur Mac, par exemple, utilisez `homebrew` pour installer Cassandra en saisissant `brew install cassandra`.

## Fichiers cqlshrc

Lorsque la commande `cqlsh` s'exécute, elle charge le fichier `$HOME/.cassandra/cqlshrc` par défaut, qui peut définir de nombreux paramètres qu'il n'est pas possible de définir sur la ligne de commande. Vous pouvez éditer le fichier par défaut afin de définir des paramètres, et toutes les modifications que vous apportez s'appliquent à toutes les sessions à moins d'être remplacées à partir de la ligne de commande.

Si vous exécutez plusieurs instances de Scylla dans des environnements différents, vous pouvez créer un fichier `cqlshrc` distinct que vous utilisez en ajoutant `--cqlshrc=[filename]` à votre commande.


Pour un exemple de fichier cqlshrc complet, voir [Exemple Apache de cqlshrc](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample). 

### cqlshrc pour {{site.data.keyword.composeForScyllaDB}}

Un fichier `cqlshrc` minimal de connexion à votre service se présente comme suit :

```
[authentication]
username = scylla
password = [password]

[connection]
hostname = [hostname.composedb.com]
port = [portnumber]
factory = cqlshlib.ssl.ssl_transport_factory

[ssl]
version=SSLv23
certfile = ~/path_to_your/lechain.pem
validate = true
```

Entrez les informations fournies dans la section _Chaînes de connexion_ de votre service et sauvegardez le fichier sous le nom `example.cqlshrc`.

Vous pouvez à présent vous connecter à votre base de données Scylla en exécutant `cqlsh --ssl --cqlshrc=example.cqlshrc` depuis la ligne de commande. Même si `cqlshrc` a une option permettant d'indiquer `ssl=true`, elle semble être ignorée ; vous devez donc employer l'indicateur `--ssl`.

## TLSv1.2

`cqlsh` n'utilise pas par défaut TLSv1.2 lors de la négociation d'une connexion cryptée. Pour vous connecter à un déploiement Compose Scylla, vous devez indiquer la version TLS dans l'environnement qui exécute `cqlsh`. Pour une compatibilité maximale (et un peu de pérennité pour TLSv1.3), définissez `SSL_VERSION` dans l'environnement sur SSLv23 à l'aide de `export SSL_VERSION=SSLv23` ou en plaçant `SSL_VERSION=SSLv23` avant la commande de connexion `cqlsh`.

Vous pouvez également spécifiquement définir la version sur TLSv1.2 à l'aide de `export SSL_VERSION=TLSv1_2` ou en plaçant `SSL_VERSION=TLSv1_2` avant la commande de connexion `cqlsh`.

## Connexion sans validation

La validation est activée par défaut, mais vous pouvez vous connecter à Scylla avec TLS/SSL sans validation de l'hôte distant. Cette méthode n'est toutefois pas recommandée pour la production. La validation est contrôlée par la variable d'environnement `SSL_VALIDATE` ou par les paramètres SSL du fichier cqlshrc. Définissez `SSL_VALIDATE` sur false (`export SSL_VALIDATE=false`) dans votre environnement pour désactiver la validation.

Vous pouvez également désactiver la validation en éditant le fichier cqlshrc.

```
[ssl]
validate = false
```

Si vous voulez désactiver la validation pour une seule commande `cqlsh`, insérez `SSL_VALIDATE=false` avant la commande de connexion `cqlsh`. 

Vous devez toujours indiquer à `cqlsh` d'utiliser TLSv1.2, même lorsque la validation n'est pas activée. Si vous n'avez pas défini la version SSL dans l'environnement et que vous ne souhaitez procéder à la validation, la connexion doit être semblable à l'exemple suivant :

```
SSL_VERSION=SSLv23 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Obtention et utilisation du certificat

Si vous voulez utiliser cqlsh et vérifier l'hôte distant, vous devez obtenir le certificat, le sauvegarder dans un emplacement accessible, puis indiquer son chemin d'accès à cqlsh.

Indiquez dans la variable d'environnement `SSL_CERTFILE` le chemin et le nom de fichier de votre certificat sauvegardé afin de l'activer pour tous les appels ultérieurs de `cqlsh`. 

Vous pouvez également ajouter ce qui suit dans votre fichier cqlshrc.

```
[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

Si vous souhaitez uniquement utiliser un certificat particulier pour une seule commande `cqlsh`, placez `SSL_CERTFILE = ~/path_to_your/lechain.pem` au début de la commande de connexion à cqlsh. Par exemple :

```
SSL_VERSION=SSLv23 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Initiation à cqlsh

Si vous entrez `HELP` vous voyez que le shell dispose de nombreuses capacités. Toutes les commandes ont même une option de saisie semi-automatique via la touche `TAB`. Par exemple :

1. Entrez `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. Les choix disponibles pour la classe de réplication s'affichent.
2. Ici, sélectionnez `SimpleStrategy` car le cluster ne couvrira pas plusieurs centres de données.
3. Appuyez de nouveau sur `<TAB><TAB>` et entrez la valeur 3 dans la zone `replication_factor`. Fermez ensuite l'accolade avec `}` et terminez l'instruction avec le signe `;<enter>`.

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
