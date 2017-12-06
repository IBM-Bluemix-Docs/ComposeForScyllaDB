---

Copyright:
  Years: 2017
lastupdated: "2017-09-07"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Présentation du service

La page _Vue d'ensemble_ affiche des informations concernant la base de données Compose d'{{site.data.keyword.cloud}}. La vue d'ensemble contient des informations d'identification essentielles et indique l'utilisation actuelle des ressources. Vous y trouverez également une section qui contient les chaînes de connexion que vous pouvez utiliser avec les outils pour vous connecter à votre base de données.

## Deployment Details

Le panneau _Deployment Details_ affiche des détails concernant votre déploiement.

![Deployment Details](./images/scylla-deployment-details.png "Vue du panneau Deployment Details")

### Type

Type de base de données fourni par le service et version de base de données qu'utilise votre service.

### Nom

Identificateur interne du service.

### Utilisation

Taille de votre base de données et quantité de stockage fournie par le plan de service.


## Chaînes de connexion

Chaque chaîne de connexion pour votre service se trouve dans un onglet distinct du panneau _Chaînes de connexion_.

### HTTPS

Chaîne de connexion au format URI, qui peut être utilisée par certaines bibliothèques client et qui contient toutes les informations requises pour la connexion d'autres bibliothèques. Pour savoir comment utiliser une chaîne de connexion pour vous connecter, voir [Connexion d'une application externe](./connecting-external.html).

### Ligne de commande

Une **ligne de commande** est une commande préformatée qui appellera `cqlsh` avec les paramètres appropriés. La commande affichée inclut des indicateurs obligatoires (`--ssl` et `--cqlversion`).  Pour l'utiliser, les outils du client PostgreSQL doivent être installés sur le système local. Pour plus d'informations sur la procédure à suivre, voir [Connexion d'une application externe](./connecting-external.html).

### Mappes
Ces mappes de conversion d'adresse peuvent être utilisées dans les applications qui nécessitent la haute disponibilité. Elles peuvent utiliser la reconnaissance automatique pour rechercher des noeuds dans le cluster. Elles convertissent les adresses de portail externes en adresses de cluster internes pour les pilotes de client qui utilisent cette fonctionnalité.

### Certificat SSL

Le service Compose {{site.data.keyword.cloud_notm}} vous fournit un certificat SSL qui vous permet de vous connecter à votre base de données.
