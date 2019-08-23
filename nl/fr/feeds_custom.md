---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: feeds, serverless, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}



# Création de flux de fournisseur d'événements personnalisés
{: #feeds_custom}

{{site.data.keyword.openwhisk_short}} prend en charge une API ouverte, dans laquelle tous les utilisateurs peuvent exposer un service producteur d'événement sous forme de flux dans un package.
{: shortdesc}


## Architecture de flux
{: #feeds_arch}

Vous pouvez créer un flux à l'aide de l'un des trois modèles d'architecture : **Points d'ancrage**, **Interrogation** et **Connexions**.

### Points d'ancrage

Avec le modèle Points d'ancrage (Hooks), un flux est configuré à l'aide d'un [webhook](https://en.wikipedia.org/wiki/Webhook){: external} exposé par un autre service. Dans cette stratégie, un webhook est configuré sur un service externe afin d'envoyer des données directement à une adresse URL pour exécuter un déclencheur. Il s'agit de loin de la méthode la plus simple et la plus attrayante pour l'implémentation de flux à faible fréquence.

Par exemple, le [package GitHub](/docs/openwhisk?topic=cloud-functions-pkg_github) et le [package Push Notification](/docs/openwhisk?topic=cloud-functions-pkg_push_notifications) utilisent un webhook.


### Interrogation

Avec le modèle Interrogation (Polling), une action {{site.data.keyword.openwhisk_short}} est définie de manière à interroger un noeud final régulièrement afin d'extraire de nouvelles données. Ce modèle est relativement facile à générer, mais la fréquence des événements est limitée par l'intervalle d'interrogation.

### Connexions

Avec le modèle Connexions (Connections), un service distinct gère une connexion permanente à une source de flux. L'implémentation reposant sur la connexion peut interagir avec un noeud final de service en utilisant de longs intervalles d'interrogation ou configurer une notification push.

Par exemple, le [package {{site.data.keyword.cloudant}}](/docs/openwhisk?topic=cloud-functions-pkg_cloudant) utilise le modèle Connexions.



##  Implémentation d'actions de flux
{: #feeds_actions}

L'action de flux (feed) est une action qui accepte les paramètres suivants. 

| Paramètre | Description |
| --- | --- |
| `lifecycleEvent` | `CREATE`, `READ`, `UPDATE`, `DELETE`, `PAUSE` ou `UNPAUSE`. |
| `triggerName` | Nom qualifié complet du déclencheur qui contient les événements produits depuis ce flux.|
| `authKey` | Données d'identification pour l'authentification de base de l'utilisateur {{site.data.keyword.openwhisk_short}}, propriétaire du déclencheur.|

L'action de flux peut également accepter tout autre paramètre dont elle a besoin pour gérer le flux. Par exemple, l'action de flux {{site.data.keyword.cloudant}} "changes" s'attend à recevoir des paramètres, tels que `dbname` et `username`.

Lorsque l'utilisateur crée un déclencheur depuis l'interface de ligne de commande avec le paramètre `--feed`, l'action de flux est automatiquement appelée avec les paramètres appropriés.

Par exemple, un utilisateur crée une liaison **mycloudant** pour le package `cloudant` avec un nom d'utilisateur et un mot de passe comme paramètres liés. Lorsque l'utilisateur exécute la commande suivante depuis l'interface de ligne de commande :
```
ibmcloud fn trigger create my_cloudant_trigger --feed mycloudant/changes -p dbName myTable
```
{: pre}

Ensuite, quelque chose d'équivalent à la commande suivante s'exécute :
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

L'action de flux nommée *changes* admet ces paramètres et est censée effectuer toute action nécessaire à la configuration d'un flot d'événements à partir de {{site.data.keyword.cloudant_short_notm}}. L'action de flux est effectuée à l'aide de la configuration appropriée, dirigée vers le déclencheur.

Pour le flux *changes* de {{site.data.keyword.cloudant_short_notm}}, l'action communique directement avec un service de *déclencheur {{site.data.keyword.cloudant_short_notm}}* qui est implémenté avec une architecture reposant sur des connexions.

Il existe un protocole d'action de flux similaire pour `ibmcloud fn trigger delete`, `ibmcloud fn trigger update` et `ibmcloud fn trigger get`. 

## Implémentation de flux à l'aide de points d'ancrage
{: #feeds_hooks}

Configurez un flux en utilisant un point d'ancrage (hook) lorsque le producteur d'événement prend en charge une fonction de rappel de webhook. 

Avec cette méthode, vous n'avez pas besoin de gérer un service persistant hors de {{site.data.keyword.openwhisk_short}}. La gestion des flux s'exécute naturellement par le biais d'**actions de flux** {{site.data.keyword.openwhisk_short}} sans état, qui négocient directement avec une API de webhook tierce.

Lorsqu'elle est appelée avec `CREATE`, l'action de flux installe simplement un webhook pour un autre service et demande au service distant d'envoyer des notifications à l'URL `fireTrigger` appropriée dans {{site.data.keyword.openwhisk_short}}.

Le webhook reçoit l'instruction d'envoyer les notifications à une adresse URL, telle que :

`POST /namespaces/{namespace}/triggers/{triggerName}`

Le formulaire associé à la demande POST est interprété comme un document JSON qui définit des paramètres pour l'événement déclencheur. Les règles {{site.data.keyword.openwhisk_short}} transmettent ces paramètres de déclencheur pour que des actions soient déclenchées suite à l'événement.

## Implémentation de flux à l'aide d'interrogation
{: #feeds_polling}

Vous pouvez configurer une action pour interroger une source de flux entièrement dans {{site.data.keyword.openwhisk_short}}, sans avoir à gérer des connexions permanentes ou un service externe.

Pour les flux pour lesquels aucun webhook n'est disponible mais qui n'ont pas besoin de gérer un volume élevé ou qui ne requièrent pas de temps de réponse courts, vous pouvez utiliser l'interrogation (Polling).

Pour configurer un flux reposant sur l'interrogation, l'action de flux effectue les opérations suivantes lorsqu'elle est appelée pour `CREATE` :

1. L'action de flux configure un déclencheur périodique avec une fréquence spécifique, à l'aide du flux `whisk.system/alarms`.
2. Le développeur de flux crée une action `pollMyService` qui interroge simplement le service distant et renvoie les nouveaux événements.
3. L'action de flux configure une *règle* *T -> pollMyService*.

Cette procédure implémente un déclencheur qui repose sur l'interrogation en utilisant uniquement des actions {{site.data.keyword.openwhisk_short}}, sans avoir besoin de recourir à un service distinct.

## Implémentation de flux à l'aide de connexions
{: #feeds_connections}

Les deux choix d'architecture précédents sont simples et faciles à implémenter. Cependant, si vous voulez créer un flux dont les performances sont élevées, vous pouvez utiliser des connexions persistantes et des techniques d'interrogation longue ou similaires. 

Etant donné que les actions {{site.data.keyword.openwhisk_short}} doivent être de courte durée, une action ne peut pas maintenir de connexion permanente avec un tiers. En revanche, vous pouvez établir un service distinct, appelé **service fournisseur**, en dehors de {{site.data.keyword.openwhisk_short}}, qui fonctionne tout le temps. Un service fournisseur peut maintenir des connexions à des sources d'événement tierces qui prennent en charge l'interrogation longue ou d'autres notifications reposant sur les connexions.

Le service fournisseur possède une API REST qui permet à l'**action de flux** {{site.data.keyword.openwhisk_short}} de contrôler le flux. Le service fournisseur agit comme un proxy entre le fournisseur d'événements et {{site.data.keyword.openwhisk_short}}. Lorsqu'il reçoit des événements d'un tiers, il les envoie à {{site.data.keyword.openwhisk_short}} en exécutant un déclencheur.

Le flux **changes** de {{site.data.keyword.cloudant_short_notm}} en est un exemple canonique car il établit un service `cloudanttrigger`, qui fait office de médiateur entre les notifications {{site.data.keyword.cloudant_short_notm}} sur une connexion permanente et des déclencheurs {{site.data.keyword.openwhisk_short}}.


Le flux **alarm** est implémenté avec un modèle similaire.

L'architecture reposant sur les connexions est l'option qui permet d'obtenir les performances les plus élevées, mais elle impose une surcharge plus importante sur les opérations que les architectures reposant sur l'interrogation et les points d'ancrage.






