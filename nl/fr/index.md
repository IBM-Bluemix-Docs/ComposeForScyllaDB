---

copyright:
  years: 2016,2018
lastupdated: "2018-03-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# A propos de {{site.data.keyword.composeForScyllaDB}}
{: #about-compose-for-scylladb}

ScyllaDB est un remplacement interne pour la base de données répartie Cassandra à colonnes larges. ScyllaDB est écrit en C++, plutôt qu'en Java Cassandra, pour une meilleure utilisation des ressources pouvant conduire à des performances dix fois meilleures dans les tests de performances. En plus du maintien de la compatibilité avec les fichiers de données et l'outil Cassandra, ScyllaDB ajoute des fonctionnalités de réglage automatique. {{site.data.keyword.composeForScyllaDB_full}} étend les fonctionnalités de ScyllaDB en les gérant à votre place, vous offrant un système de déploiement à mise à l'échelle automatique facile à utiliser qui fournit des fonctions de haute disponibilité et de redondance ainsi que des sauvegardes automatisées.
{:shortdesc}

## Création d'une instance de service {{site.data.keyword.composeForScyllaDB}}

Vous pouvez créer un service {{site.data.keyword.composeForScyllaDB}} à partir de la [page {{site.data.keyword.composeForScyllaDB}}](https://console.{DomainName}/catalog/services/compose-for-scylladb/) du catalogue {{site.data.keyword.cloud_notm}}.

Choisissez un nom de service ainsi qu'une région, une organisation et un espace dans lesquels mettre le service à disposition. Vous pouvez utiliser la zone **Select a database version** pour sélectionner parmi les versions de base de données disponibles.

Lorsque vous mettez votre instance {{site.data.keyword.composeForScyllaDB}} à disposition, vous avez le choix entre le plan *Standard* et le plan *Entreprise*. Avec le plan *Entreprise*, vous pouvez mettre votre instance {{site.data.keyword.composeForScyllaDB}} à disposition dans un cluster {{site.data.keyword.composeEnterprise}} disponible. {{site.data.keyword.composeEnterprise}} offre la sécurité et l'isolement requis par les normes de conformité des entreprises et utilise un réseau dédié pour garantir les performances des bases de données déployées. Pour plus d'informations, voir la documentation [{{site.data.keyword.composeEnterprise}} documentation](/docs/services/ComposeEnterprise/index.html).

## Gestion de {{site.data.keyword.composeForScyllaDB}}

Vous pouvez gérer votre service depuis son tableau de bord. Vous y trouverez des informations concernant la base de données Compose d'{{site.data.keyword.cloud_notm}} et la manière de vous y connecter. Vous pouvez également :

- gérer vos sauvegardes ;
- allouer plus de ressources à votre service ; 
- constituer des listes blanches pour limiter l'accès à vos bases de données. 

Pour plus d'informations, voir [Paramètres](./dashboard-settings.html).

{{site.data.keyword.composeForScyllaDB}} repose sur les rôles Cloud Foundry pour la gestion de l'accès au service. Seuls les utilisateurs dotés du rôle Développeur peuvent voir ou utiliser le tableau de bord du service. Pour plus d'informations sur les rôles Cloud Foundry, voir les pages [Accès Cloud Foundry](https://console.{DomainName}/docs/iam/cfaccess.html#cfaccess) et [Gestion de l'accès Cloud Foundry](https://console.{DomainName}/docs/iam/mngcf.html#mngcf).
{: .tip}

## Connexion à {{site.data.keyword.composeForScyllaDB}}

Vous pouvez vous connecter à votre service avec les données d'identification créées en même temps que le service ou avec les chaînes de connexion et la ligne de commande fournies dans l'onglet *Vue d'ensemble* du tableau de bord de votre service.

## Connexion d'une application {{site.data.keyword.cloud_notm}} à {{site.data.keyword.composeForScyllaDB}}

Pour connecter une application {{site.data.keyword.cloud_notm}} à votre service, utilisez les données d'identification créées en même temps que le service. Pour plus d'informations sur la connexion d'une application {{site.data.keyword.cloud_notm}} à un service {{site.data.keyword.composeForScyllaDB}}, voir [Connexion d'une application {{site.data.keyword.cloud_notm}}](./connecting-bluemix-app.html).

## Connexion à {{site.data.keyword.composeForScyllaDB}} depuis l'extérieur d'{{site.data.keyword.cloud_notm}}

Pour vous connecter à {{site.data.keyword.composeForScyllaDB}} depuis l'extérieur d'{{site.data.keyword.cloud_notm}}, vous pouvez utiliser les chaînes de connexion ou la ligne de commande fournies. Pour plus d'informations sur ce type de connexion, voir [Connexion d'une application externe](./connecting-external.html).
