---
copyright:
  years: 2016,2018
lastupdated: "2018-06-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Architecture 
{: #architecture}

## Réplication

Un service {{site.data.keyword.composeForScyllaDB_full}} contient un cluster avec trois répliques de données. Toutes les répliques ont la même importance ; il n'y pas de réplique principale ou maître et chaque noeud connaît l'état des autres noeuds. Vous sélectionnez une région où le service est déployé, les trois répliques de données sont propagées dans les zones de disponibilité de la région, puis vos données sont copiées dans les trois répliques. Lorsqu'un membre de données n'est plus accessible, votre cluster continue à fonctionner normalement.

## Chiffrement de disque

Tous les services {{site.data.keyword.composeForScyllaDB}} disposent du chiffrement au repos. Vos données résident sur des serveurs où le chiffrement au niveau volume est activé. 

Etant donné que {{site.data.keyword.composeForScyllaDB}} est un service géré, les opérateurs {{site.data.keyword.cloud_notm}} Compose peuvent voir les données. Outre le chiffrement du disque, vous devez chiffrer vos informations personnelles avant de les stocker dans la base de données ou utiliser des extensions ou des fonctions permettant d'activer le chiffrement sur la base de données elle-même. Même si ces méthodes risquent d'avoir un impact sur la facilité d'utilisation ou les performances, il est conseillé de s'assurer que les informations personnelles sont protégées par un chiffrement.

## Mise à l'échelle automatique

Les ressources sont automatiquement mises à l'échelle en fonction de l'utilisation totale du stockage sur disque des bases de données Scylla. La mémoire est basée sur un rapport d'espace disque mis à disposition de 1 sur 10. Ces ressources sont intégrées en tant qu'unités.

La mise à l'échelle automatique est conçue pour répondre aux tendances à court et moyen termes de votre base de données. Votre service est contrôlé toutes les heures. S'il est à court d'espace disque, des unités supplémentaires sont attribuées au déploiement. 

Une mise à l'échelle automatique ne réduit pas la taille des déploiements où l'utilisation du disque/de la mémoire à diminué. Les ressources mises à disposition de vos bases de données sont conservées pour les besoins ultérieurs ou jusqu'à ce que vous réduisiez manuellement votre déploiement.
{: .tip}


