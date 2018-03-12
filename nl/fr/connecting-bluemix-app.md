---

copyright:
  years: 2016,2018
lastupdated: "2017-06-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connexion d'une application {{site.data.keyword.cloud_notm}}

Pour connecter une application {{site.data.keyword.cloud}} à votre service, utilisez les données d'identification créées lors de la mise à disposition du service. Le modèle d'application montre comment utiliser
Node.js pour établir une connexion à un service {{site.data.keyword.composeForScyllaDB_full}} à l'aide des données d'identification fournies et comment créer une base de données, lire dans cette base de données et y écrire.

## Connexion à l'aide du modèle d'application 'Hello World'

Le modèle d'application [compose-scylladb-helloworld-nodejs](https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs) montre comment utiliser Node.js pour établir une connexion à un service {{site.data.keyword.composeForScyllaDB}} à l'aide des données d'identification fournies. L'application crée une base de données, y lit et y écrit.

Téléchargez le modèle d'application et suivez les instructions contenues dans le fichier Readme. Ensuite, sur la page des détails d'application d'{{site.data.keyword.cloud_notm}}, cliquez sur **Afficher l'application** pour afficher le contenu du tableau *Exemples*.

## Données d'identification disponibles

Nom de zone|Description
----------|-----------
`db_type`|Type de base de données fourni par le service, `scylla`, dans ce cas.
`uri_cli_1`|Ligne de commande shell `cqlsh` alternative qui permet d'établir la connexion à l'instance de base de données.
`maps`|Carte de connexion ScyllaDB qui fournit les informations requises par les différents pilotes pour associer les adresses IP internes aux noms DNS externes d'une base de données ScyllaDB.
`name`|Nom du déploiement de base de données.
`uri_cli`|Ligne de commande shell `cqlsh` qui permet d'établir la connexion à l'instance de base de données.
`uri_direct_2`|Identificateur URI alternatif qui peut être utilisé pour se connecter au service. Utilise le même format qu'`uri`.
`uri_direct_1`|Identificateur URI alternatif qui peut être utilisé pour se connecter au service. Utilise le même format qu'`uri`.
`ca_certificate_base64`|Certificat autosigné utilisé pour confirmer qu'une application se connecte au serveur approprié. Le certificat est codé en base64.
`deployment_id`|Identificateur interne du service, créé dans Compose.
`uri_cli_2`|Ligne de commande shell `cqlsh` alternative qui permet d'établir la connexion à l'instance de base de données.
`uri`|Identificateur URI qui est utilisé pour la connexion au service et qui comprend le schéma (`scylla:`), le mot de passe, le nom d'hôte du serveur, le numéro de port auquel se connecter et le nom de la base de données.
{: caption="Tableau 1. Données d'identification Compose for ScyllaDB" caption-side="top"}
