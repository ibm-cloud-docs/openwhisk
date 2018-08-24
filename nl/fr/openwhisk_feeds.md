---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Création de flux de fournisseur d'événement
{: #openwhisk_feeds}

{{site.data.keyword.openwhisk_short}} prend en charge une API ouverte, dans laquelle tous les utilisateurs peuvent exposer un service producteur d'événement sous forme de flux dans un package. La section ci-dessous décrit les options d'architecture et d'implémentation pour la mise à disposition de votre propre flux personnalisé.
{: shortdesc}

Ce document s'adresse aux utilisateurs avancés de {{site.data.keyword.openwhisk_short}} qui souhaitent publier leurs propres flux. La plupart des utilisateurs de {{site.data.keyword.openwhisk_short}} peuvent ignorer la section suivante relative à l'architecture de flux.

## architecture de flux

Il existe aux moins trois modèles d'architecture pour la création d'un flux : **Points d'ancrage**, **Interrogation** et **Connexions**.

### Points d'ancrage
Dans le modèle *Points d'ancrage*, un flux est configuré à l'aide d'une fonction de [webhook](https://en.wikipedia.org/wiki/Webhook) qui est exposée par un autre service.   Dans cette stratégie, un webhook est configuré sur un service externe afin d'envoyer des données directement à une adresse URL pour exécuter un déclencheur. Il s'agit de loin de la méthode la plus simple et la plus attrayante pour l'implémentation de flux à faible fréquence.

<!-- The github feed is implemented using webhooks.  Put a link here when we have the open repo ready -->

### Interrogation
Dans le modèle "Interrogation", une action {{site.data.keyword.openwhisk_short}} est définie de manière à interroger un noeud final régulièrement afin d'extraire de nouvelles données. Ce modèle est relativement facile à générer, mais la fréquence des événements est limitée par l'intervalle d'interrogation.

### Connexions
Dans le modèle "Connexions", un service distinct maintient une connexion permanente à une source de flux. L'implémentation reposant sur la connexion peut interagir avec un noeud final de service pour définir de longs intervalles d'interrogation ou configurer une notification push.

<!-- Our cloudant changes feed is connection based.  Put a link here to
an open repo -->

<!-- What is the foundation for the Message Hub feed? If it is "connections" then lets put a link here as well -->

## Différence entre un flux et un déclencheur

Les flux et les déclencheurs sont étroitement liés, mais ce sont des concepts techniquement distincts.   

- {{site.data.keyword.openwhisk_short}} traite des **événements** qui transitent dans le système.

- Techniquement, un **déclencheur** désigne une classe d'événements. Chaque événement appartient à un déclencheur et un seul ; par analogie, un déclencheur ressemble à une *rubrique* dans les systèmes de publication/abonnement reposant sur des rubriques. Une **règle** *T -> A* a la signification suivante : "à chaque fois qu'un événement provenant du déclencheur *T* arrive, appeler l'action *A* avec le contenu du déclencheur.

- Un **flux** est un flot d'événements qui appartiennent tous à un déclencheur *T*. Il est contrôlé par une **action de flux**, qui gère la création, la suppression, l'interruption et la reprise du flot d'événements constituant un flux. En règle générale, l'action de flux interagit avec des services externes qui produisent les événements, en utilisant une API REST qui gère les notifications.

##  Implémentation d'actions de flux

L'*action de flux* est une *action* {{site.data.keyword.openwhisk_short}} normale qui accepte les paramètres suivants :
* **lifecycleEvent** : 'CREATE', 'READ', 'UPDATE', 'DELETE', 'PAUSE' ou 'UNPAUSE'.
* **triggerName** : nom qualifié complet du déclencheur qui contient les événements produits depuis ce flux.
* **authKey** : données d'identification pour l'authentification de base de l'utilisateur {{site.data.keyword.openwhisk_short}} à qui appartient le déclencheur.

L'action de flux peut également accepter tout autre paramètre dont elle a besoin pour gérer le flux. Par exemple, l'action de flux {{site.data.keyword.cloudant}} "changes" s'attend à recevoir des paramètres, tels que *'dbname'*, *'username'*, etc.

Lorsque l'utilisateur crée un déclencheur depuis l'interface de ligne de commande avec le paramètre **--feed**, le système appelle
automatiquement l'action de flux avec les paramètres appropriés.

Par exemple, supposons que l'utilisateur crée une liaison **mycloudant** pour le package `cloudant` avec un nom d'utilisateur et un mot de passe comme paramètres liés. Lorsque l'utilisateur exécute la commande suivante depuis l'interface de ligne de commande :
```
ibmcloud fn trigger create T --feed mycloudant/changes -p dbName myTable
```
{: pre}

le système effectue "en coulisses" une opération équivalente à :
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

L'action de flux nommée *changes* admet ces paramètres et est censée effectuer toute action nécessaire à la configuration d'un flot d'événements à partir de {{site.data.keyword.cloudant_short_notm}}. L'action de flux est effectuée à l'aide de la configuration appropriée, dirigée vers le déclencheur *T*.

Pour le flux *changes* de {{site.data.keyword.cloudant_short_notm}}, l'action communique directement avec un service de *déclencheur {{site.data.keyword.cloudant_short_notm}}* qui est implémenté avec une architecture reposant sur des connexions.

Il existe un protocole d'action de flux similaire pour `ibmcloud fn trigger delete`, `ibmcloud fn trigger update` et `ibmcloud fn trigger get`.

## Implémentation de flux avec des points d'ancrage

Il est facile de configurer un flux en utilisant un point d'ancrage si le producteur d'événement prend en charge une fonction de webhook/rappel.

Avec cette méthode, il n'est _pas nécessaire_ de maintenir un service persistant hors de {{site.data.keyword.openwhisk_short}}. La gestion des flux s'exécute naturellement par le biais d'*actions de flux* {{site.data.keyword.openwhisk_short}} sans état, qui négocient directement avec une API de webhook tierce.

Lorsqu'elle est appelée avec `CREATE`, l'action de flux installe simplement un webhook pour un autre service et demande au service distant d'envoyer des notifications à l'URL `fireTrigger` appropriée dans {{site.data.keyword.openwhisk_short}}.

Le webhook reçoit l'instruction d'envoyer les notifications à une adresse URL, telle que :

`POST /namespaces/{namespace}/triggers/{triggerName}`

Le formulaire associé à la demande POST est interprété comme un document JSON qui définit des paramètres pour l'événement déclencheur. Les règles {{site.data.keyword.openwhisk_short}} transmettent ces paramètres de déclencheur pour que des actions soient déclenchées suite à l'événement.

## Implémentation de flux avec le modèle Interrogation

Il est possible de configurer une *action* {{site.data.keyword.openwhisk_short}} afin d'interroger une source de flux entièrement dans {{site.data.keyword.openwhisk_short}}, sans qu'il ne soit nécessaire de maintenir des connexions ou un service externe persistants.

Pour les flux pour lesquels aucun webhook n'est disponible mais qui n'ont pas besoin de gérer un volume élevé de demandes simultanées ou qui ne
requièrent pas de temps de réponse courts, l'interrogation est une option attrayante.

Pour configurer un flux reposant sur l'interrogation, l'action de flux effectue les opérations suivantes lorsqu'elle est appelée pour
`CREATE` :

1. L'action de flux configure un déclencheur périodique (*T*) avec la fréquence souhaitée, à l'aide du flux `whisk.system/alarms`.
2. Le développeur de flux crée une action `pollMyService` qui interroge simplement le service distant et renvoie les nouveaux événements.
3. L'action de flux configure une *règle* *T -> pollMyService*.

Cette procédure implémente un déclencheur qui repose sur l'interrogation en utilisant uniquement des actions {{site.data.keyword.openwhisk_short}}, sans avoir besoin de recourir à un service distinct.

## Implémentation de flux à l'aide du modèle Connexions

Les deux choix d'architecture précédents sont simples et faciles à implémenter. Cependant, si vous voulez créer un flux dont les performances sont élevées, il n'existe pas d'alternative aux connexions permanentes et à l'interrogation longue, ou à des techniques similaires.

Etant donné que les actions {{site.data.keyword.openwhisk_short}} doivent être de courte durée, une action ne peut pas maintenir de connexion permanente avec un tiers. En revanche, vous pouvez établir un service distinct, appelé *service fournisseur*, en dehors de {{site.data.keyword.openwhisk_short}}, qui fonctionne tout le temps. Un service fournisseur peut maintenir des connexions à des sources d'événement tierces qui prennent en charge l'interrogation longue ou d'autres notifications reposant sur les connexions.

Le service fournisseur possède une API REST qui permet à l'*action de flux* {{site.data.keyword.openwhisk_short}} de contrôler le flux. Le service fournisseur agit comme un proxy entre le fournisseur d'événements et {{site.data.keyword.openwhisk_short}}. Lorsqu'il reçoit des événements d'un tiers, il les envoie à {{site.data.keyword.openwhisk_short}} en exécutant un déclencheur.

Le flux *changes* de {{site.data.keyword.cloudant_short_notm}} en est un exemple canonique car il établit un service `cloudanttrigger`, qui fait office de médiateur entre les notifications {{site.data.keyword.cloudant_short_notm}} sur une connexion permanente et des déclencheurs {{site.data.keyword.openwhisk_short}}.
<!-- TODO: add a reference to the open source implementation -->

Le flux *alarm* est implémenté avec un modèle similaire.

L'architecture reposant sur les connexions est l'option qui permet d'obtenir les performances les plus élevées, mais elle impose une surcharge plus importante sur les opérations par rapport aux architectures reposant sur l'interrogation et les points d'ancrage.
