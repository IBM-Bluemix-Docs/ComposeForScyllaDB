---

copyright:
  years: 2017,2018
lastupdated: "2017-12-11"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilisation de nodetool
{: #using-nodetool}

Pour pouvoir utiliser nodetool afin d'administrer votre service {{site.data.keyword.composeForScyllaDB_full}}, vous devez :
1. Ajouter une clé SSH
2. Ouvrir un tunnel à l'aide de la commande SSH que contient l'onglet _Socks_ des chaînes de connexion.

Une fois le tunnel ouvert, vous pouvez utiliser des commandes `nodetool` au format indiqué dans l'onglet _Nodetool_ des chaînes de connexion. La commande entrée renvoie le statut du cluster Scylla.

## Commandes nodetool disponibles
Nodetool propose une large palette de commandes, mais nous nous limiteront à celles permettant d'éviter des situations susceptibles d'entraîner des dommages au niveau de la maintenance automatisée du cluster. Les commandes disponibles sont les suivantes :

Commande|Fonction
----------|-----------
cfhistograms|Fournit des statistiques concernant un tableau, y compris le nombre de tables SS, le temps d'attente de lecture/écriture, la taille des partitions et le nombre de colonnes.
cfstats|Fournit des diagnostics approfondis concernant une famille des colonne.
cleanup|Déclenche le nettoyage immédiat des clés qui n'appartiennent plus à un noeud.
compact|Force une compression (majeure) sur une ou plusieurs familles de colonne.
compactionhistory|Imprime l'historique de compression.
compactionstats|Imprime des statistiques sur les compressions.
describecluster|Imprime le nom, le "snitch", le programme de partitionnement et la version de schéma d'un cluster.
describering <keyspace>|Affiche des informations sur les plages de jetons d'un espace de clés donné.
flush|Vide une ou plusieurs familles de colonne.
help|Imprime l'aide.
getendpoints <keyspace> <cfname> <key>|Imprime les noeuds finaux qui possèdent la clé.
gossipinfo|Affiche des informations relatives au gossip du cluster.
info|Imprime des informations relatives aux noeuds.
move <new token>|Déplace un noeud de l'anneau à jeton vers un nouveau jeton.
netstats|Imprime des informations réseau sur l'hôte fourni (noeud de connexion par défaut).
proxyhistograms|Imprime des histogramme de statistiques concernant les opérations de réseau.
repair|Répare une ou plusieurs familles de colonne.
ring|Affiche des informations relatives à l'anneau à jeton.
status|Imprime des informations relative au cluster.
statusbackup|Imprime le statut de la sauvegarde ScyllaDB.
statusbinary|Imprime le statut du transport natif (protocole binaire).
statusgossip|Imprime le statut du gossip.
version|Imprime la version de la base de données.
{: caption="Tableau 1. Commandes nodetool disponibles" caption-side="top"}


## Commandes nodetool bloquées
Les commandes suivantes ne sont pas utilisables par nodetool dans le cadre d'un service {{site.data.keyword.composeForScyllaDB}} :

- clearsnapshot
- decommision
- disablebackup
- disablebinary
- disablegossip
- drain
- enablebackup
- enablebinary
- enablegossip
- getlogginglevels
- listsnapshots
- rebuild
- refresh
- removenode
- setlogginglevel
- settraceprobability
- snapshot
- stop
