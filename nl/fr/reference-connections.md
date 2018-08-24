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

# Configuration de connexion
{: #connection-configuration}

Les connexions à la base de données {{site.data.keyword.composeForScyllaDB_full}} sont gérées par 3 portails HAProxy. Chaque portail dispose de 64 Mo de mémoire.

Le basculement côté client relève de la responsabilité du concepteur d'applications. La plupart des pilotes Scylla sont adaptés aux clusters en ce sens que vous pouvez fournir une chaîne de connexion, les trois chaînes de connexion, les cartes de connexion ou une combinaison de ces éléments pour gérer plusieurs chaînes de connexion et effectuer un basculement automatique sans perte de données. A moins que vous n'utilisiez un pilote qui traite entièrement les erreurs de basculement, il est conseillé de procéder comme suit :

* Utilisez un pilote qui accepte plusieurs noeuds finaux dans sa configuration de connexion.
* Vérifiez que les appels de vos applications en vue d'une lecture ou d'une écriture dans la base de données réagissent aux erreurs de manière appropriée. Ces opérations incluent :
  + la journalisation de l'erreur et la reconnexion,
  + la reconnexion et une nouvelle tentative de l'opération à l'origine de l'erreur,
  + la mise en file d'attente de l'opération ayant échoué et la planification de la reconnexion.

## Chiffrement du transport

Tous les portails {{site.data.keyword.composeForScyllaDB}} HAProxy disposent de TLS/SSL et prennent en charge TLS version 1.2. Les certificats du service sont des certificats Let's Encrypt. Certains pilotes et l'interface de ligne de commande cqlsh nécessitent une copie de la chaîne de certificats pour se connecter à votre service (pour plus d'informations voir [Utilisation des certificats](https://console.{DomainName}/docs/services/ComposeForScyllaDB/scylla-certificates.html)

## Limites de connexion

Les connexions de base de données sont limitées à 1000 connexions par portail. Lorsque la limite de connexion est dépassée, le service ne répond plus. Vous pouvez gérer les connexions à partir de votre application à l'aide de la fonction de regroupement de connexions de votre pilote.

## Délais d'attente de proxy

Les portails HAProxy gèrent le cycle de vie des connexions entre le serveur et le client. Nous en fournissons le détail à titre de référence pour les éditeurs de routeurs et les personnes intéressées par les activités de bas niveau des connexions de base de données {{site.data.keyword.composeForScyllaDB}}.

Paramètre | Valeur | S'applique
----------|-----------|-----------
client | 5m | Lorsque le client doit accuser réception de données ou en envoyer.
connect | 4s | Lorsqu'une connexion est établie entre le proxy et le serveur.
server | 5m | Lorsque le serveur doit accuser réception de données ou en envoyer.
tunnel | 1 jour | Lorsqu'une connexion bidirectionnelle est établie entre un client et un serveur et que la connexion reste inactive dans les deux sens.
client-fin | 1h | Lorsque le client doit accuser réception de données ou en envoyer alors qu'une direction est déjà fermée.
check | 5s | En tant que vérification secondaire du délai d'attente lorsqu'une connexion est établie entre le proxy et le serveur.

{: caption="Tableau 1. Délais d'attente des portails Scylla" caption-side="top"}
