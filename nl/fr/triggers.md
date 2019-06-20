---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: triggers, serverless

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Création de déclencheurs d'événements
{: #triggers}

Un déclencheur (trigger) est une déclaration qui doit réagir à un certain type d'événement, provenant d'un utilisateur ou d'une source d'événements.
{: shortdesc}

Voici des exemples de déclencheur.
- Evénements de mise à jour d'emplacement
- Téléchargements de document sur un site Web
- Courriers électroniques entrants



## Création de déclencheurs depuis l'interface de ligne de commande
{: #triggers_create}


1. Créez le déclencheur. Les déclencheurs doivent être créés directement dans un espace de nom et ils ne peuvent pas être créés dans des packages.
    ```
    ibmcloud fn trigger create TRIGGER_NAME
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: created trigger TRIGGER_NAME
    ```
    {: screen}

2. Vérifiez que le déclencheur est créé.
    ```
    ibmcloud fn trigger list
    ```
    {: pre}

    Exemple de sortie :
    ```
    triggers
    /NAMESPACE/TRIGGER_NAME                            private
    ```
    {: screen}



Vous pouvez ensuite [tester le déclencheur](/docs/openwhisk?topic=cloud-functions-test#test_triggers) ou [créer une règle](/docs/openwhisk?topic=cloud-functions-rules) pour associer le déclencheur à une action.



## Différence entre un flux et un déclencheur
{: #triggers_difference}

Les flux et les déclencheurs sont étroitement liés, mais ce sont des concepts techniquement distincts.

- {{site.data.keyword.openwhisk_short}} traite des **événements** qui transitent dans le système.

- Un **déclencheur** (trigger) désigne une classe d'événements. Chaque événement appartient à un déclencheur et un seul ; par analogie, un déclencheur ressemble à une rubrique dans les systèmes de publication/abonnement reposant sur des rubriques. Une **règle** signifie qu'à chaque fois qu'un événement provenant du déclencheur arrive, il faut appeler l'action avec le contenu du déclencheur.

- Un **flux** (feed) est pratique pour configurer une source d'événements externe afin d'exécuter des événements déclencheurs qui peuvent être consommés par {{site.data.keyword.openwhisk_short}}. Un flux est un flot d'événements qui appartiennent tous à un déclencheur particulier. Les packages préinstallés, installables, ainsi que vos propres packages personnalisés peuvent contenir des flux.  Un flux est contrôlé par une **action de flux**, qui gère la création, la suppression, l'interruption et la reprise du flot d'événements inclus dans un flux. En règle générale, l'action de flux interagit avec des services externes qui produisent les événements, en utilisant une API REST qui gère les notifications.

Exemples de flux :
- Flux de modification de données {{site.data.keyword.cloudant}} qui exécute un déclencheur à chaque ajout ou modification d'un document dans une base de données.
- Flux Git qui exécute un déclencheur pour chaque validation dans un référentiel Git



## Création d'un déclencheur pour un flux
{: #triggers_feeds}

Cet exemple explique comment utiliser un flux dans le package Alarms afin d'exécuter un déclencheur toutes les minutes, et comment utiliser une règle permettant d'appeler une action toutes les minutes.

1. Obtenez une liste de descriptions des entités figurant dans le package `/whisk.system/alarms`.

    ```
    ibmcloud fn package get --summary /whisk.system/alarms
    ```
    {: pre}

    Exemple de sortie :
    ```
    package /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
    ```
    {: screen}
2. Obtenez une description du flux dans le package `/whisk.system/alarms` pour voir les paramètres que vous pouvez utiliser.

  ```
  ibmcloud fn action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  Exemple de sortie :
  ```
  action /whisk.system/alarms/alarm: Fire trigger when alarm occurs
     (params: cron trigger_payload)
  ```
  {: screen}

  Le flux `/whisk.system/alarms/alarm` admet deux paramètres :
  - `cron` : spécification crontab indiquant à quel moment exécuter le déclencheur.
  - `trigger_payload` : valeur de paramètre de contenu à définir dans chaque événement déclencheur.

2. Créez un déclencheur qui s'exécute toutes les minutes.
  ```
  ibmcloud fn trigger create everyOneMinute --feed /whisk.system/alarms/alarm -p cron "* * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: created trigger feed everyOneMinute
  ```
  {: screen}

3. Créez une application. Exemple `hello.js` :
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. Créez une action.
  ```
  ibmcloud fn action create hello hello.js
  ```
  {: pre}

5. Créez une règle qui appelle l'action `hello` chaque fois que le déclencheur `everyOneMinute` s'exécute.
  ```
  ibmcloud fn rule create myRule everyOneMinute hello
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: created rule myRule
  ```
  {: screen}

6. Vérifiez que l'action est appelée en interrogeant les journaux d'activation.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

  Vous pouvez voir que les activations ont lieu toutes les minutes pour le déclencheur, la règle et l'action. L'action reçoit les paramètres `{"name":"Mork", "place":"Ork"}` à chaque appel.


