---

copyright:
  years: 2016,2018
lastupdated: "2018-05-09"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}

# Connexion d'une application {{site.data.keyword.cloud_notm}}

Pour connecter une application {{site.data.keyword.cloud}} à votre service, utilisez les données d'identification créées lors de la mise à disposition du service.

## Connexion à l'aide du modèle d'application 'Hello World'

Le modèle d'application [compose-scylladb-helloworld-nodejs](https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs) montre comment utiliser Node.js pour établir une connexion à un service {{site.data.keyword.composeForScyllaDB}} à l'aide des données d'identification fournies. L'application crée une base de données, y lit et y écrit.

Téléchargez le modèle d'application et suivez les instructions contenues dans le fichier Readme. Ensuite, sur la page des détails d'application d'{{site.data.keyword.cloud_notm}}, cliquez sur **Afficher l'application** pour afficher le contenu du tableau *Exemples*.

## Données d'identification disponibles

Nom de zone|Description
----------|-----------
`db_type`|Type de base de données fourni par le service, `scylla`, dans ce cas.
`uri_cli_1`|Seconde ligne de commande shell `cqlsh` qui permet d'établir la connexion à l'instance de base de données.
`maps`|Carte de connexion ScyllaDB qui fournit les informations requises par les différents pilotes pour associer les adresses IP internes aux noms DNS externes d'une base de données ScyllaDB.
`name`|Nom du déploiement de base de données.
`uri_cli`|Ligne de commande shell `cqlsh` qui permet d'établir la connexion à l'instance de base de données.
`uri_direct_2`|Troisième identificateur URI qui peut être utilisé pour se connecter au service. Le format est identique à `uri`.
`uri_direct_1`|Second identificateur URI qui peut être utilisé pour se connecter au service. Le format est identique à `uri`.
`ca_certificate_base64` `(facultatif)`|Certificat autosigné codé en base64 utilisé pour confirmer qu'une application se connecte au serveur approprié. Le certificat est présent uniquement sur les services ayant un certificat auto-signé au lieu d'un certificat Let's Encrypt. Vous devez décoder la clé avant de l'utiliser, comme illustré dans le modèle d'application.
`deployment_id`|Identificateur interne du service, créé dans Compose.
`uri_cli_2`|Troisième ligne de commande shell `cqlsh` qui permet d'établir la connexion à l'instance de base de données.
`uri`|Identificateur URI qui est utilisé pour la connexion au service et qui comprend le schéma (`scylla:`), le mot de passe, le nom d'hôte du serveur, le numéro de port auquel se connecter et le nom de la base de données.
{: caption="Tableau 1. Données d'identification Compose for ScyllaDB" caption-side="top"}

**Remarque :** trois portails `haproxy` permettent d'accéder au service Scylla. Les zones `uri`, `uri_direct_1` et `uri_direct_2` peuvent toutes être utilisées pour établir la connexion. Dans vos applications, utilisez `uri`, `uri_direct_1` et `uri_direct_2` pour gérer les réponses aux pannes de connexion.
