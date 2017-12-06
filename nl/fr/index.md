---

copyright:
  years: 2016,2017
lastupdated: "2017-07-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Initiation à Compose for ScyllaDB
{: #getting-started-with-compose-for-scylladb}

ScyllaDB est un remplacement interne pour la base de données répartie Cassandra à colonnes larges. ScyllaDB est écrit en C++, plutôt qu'en Java Cassandra, pour une meilleure utilisation des ressources pouvant conduire à des performances dix fois meilleures dans les tests de performances. En plus du maintien de la compatibilité avec les fichiers de données et l'outil Cassandra, ScyllaDB ajoute des fonctionnalités de réglage automatique. {{site.data.keyword.composeForScyllaDB_full}} étend les fonctionnalités de ScyllaDB en les gérant à votre place, vous offrant un système de déploiement à dimensionnement automatique facile à utiliser qui fournit des fonctions de haute disponibilité et de redondance ainsi des sauvegardes automatisées.
{:shortdesc}

**Remarque: ** {{site.data.keyword.composeForScyllaDB_full}} ne donne pas l'accès à l'interface utilisateur Compose. Pour plus d'informations, voir [Compose on {{site.data.keyword.cloud}} Support](https://help.compose.com/docs/bluemix-compose-support).

## Création d'une instance de service Compose for ScyllaDB

[Créez une instance {{site.data.keyword.composeForScyllaDB}}](https://console.ng.bluemix.net/catalog/services/compose-for-scylladb/).

Quand vous créez une instance du service, choisissez un nom pour votre service et un nom de données d'identification. Laissez le service non lié ; vous pourrez connecter une application à votre service ultérieurement en utilisant les données d'identification fournies lors de la mise à disposition du service. La section "Données d'identification disponibles" répertorie les différentes valeurs de données d'identification.

Lorsque vous mettez votre instance {{site.data.keyword.composeForScyllaDB}} à disposition, vous avez le choix entre le plan *Standard* et le plan *Entreprise*. Avec le plan *Entreprise*, vous pouvez mettre votre instance {{site.data.keyword.composeForScyllaDB}} à disposition dans un cluster {{site.data.keyword.composeEnterprise}} disponible. {{site.data.keyword.composeEnterprise}} fournit la sécurité et l'isolement requis par les normes de conformité des entreprises et utilise un réseau dédié pour garantir les performances des bases de données déployées. Pour plus d'informations, voir la [documentation Compose - Entreprise](../ComposeEnterprise/index.html).

## Gestion de Compose for ScyllaDB

Vous pouvez gérer votre service depuis son tableau de bord. Vous y trouverez des informations concernant la base de données Compose d'{{site.data.keyword.cloud_notm}} et la manière de vous y connecter. Vous pouvez également :

- gérer vos sauvegardes ;
- allouer plus de ressources à votre service ; 
- constituer des listes blanches pour limiter l'accès à vos bases de données. 

Pour plus d'informations, voir [Paramètres](./dashboard-settings.html).

## Connexion à Compose for ScyllaDB

Vous pouvez vous connecter à votre service avec les données d'identification créées en même temps que le service ou avec les chaînes de connexion et la ligne de commande fournies dans l'onglet *Vue d'ensemble* du tableau de bord de votre service.

## Connexion d'une application {{site.data.keyword.cloud_notm}} à Compose for ScyllaDB

Pour connecter une application {{site.data.keyword.cloud_notm}} à votre service, utilisez les données d'identification créées en même temps que le service. Pour plus d'informations sur la connexion d'une application {{site.data.keyword.cloud_notm}} à un service {{site.data.keyword.composeForScyllaDB}}, voir [Connexion d'une application {{site.data.keyword.cloud_notm}}](./connecting-bluemix-app.html).

## Connexion à Compose for ScyllaDB depuis l'extérieur d'{{site.data.keyword.cloud_notm}}

Pour vous connecter à {{site.data.keyword.composeForScyllaDB}} depuis l'extérieur d'{{site.data.keyword.cloud_notm}}, vous pouvez utiliser les chaînes de connexion ou la ligne de commande fournies. Pour plus d'informations sur ce type de connexion, voir [Connexion d'une application externe](./connecting-external.html).
