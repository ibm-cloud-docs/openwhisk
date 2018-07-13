---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Organisation d'actions en packages
{: #openwhisk_packages}

Dans {{site.data.keyword.openwhisk}}, vous pouvez utiliser des packages afin de regrouper des actions connexes et de les partager.
{: shortdesc}

Un package peut inclure des *actions* et des *flux*.
- Une action est un élément de code qui s'exécute sur {{site.data.keyword.openwhisk_short}}. Par exemple, le package {{site.data.keyword.cloudant}} contient des actions permettant de lire et d'écrire des enregistrements dans une base de données {{site.data.keyword.cloudant_short_notm}}.
- Un flux est utilisé pour configurer une source d'événements externe afin d'exécuter des événements déclencheurs. Par exemple, le package Alarm comprend un flux pouvant exécuter un déclencheur à la fréquence spécifiée.

Chaque entité {{site.data.keyword.openwhisk_short}}, notamment les packages, appartient à un *espace de nom*, et le nom qualifié complet d'une entité est `/nom_espace_nom[/nom_package]/nom_entité`. Pour plus d'informations, voir les [instructions de désignation](./openwhisk_reference.html#openwhisk_entities).

Les sections ci-après expliquent comment parcourir les packages et utiliser les déclencheurs et les flux qu'ils contiennent. De plus, si vous souhaitez ajouter vos propres packages au catalogue, lisez les sections relatives à la création et au partage de packages.

## Exploration des packages
{: #browse-packages}

Plusieurs packages sont enregistrés dans {{site.data.keyword.openwhisk_short}}. Vous pouvez obtenir la liste des packages qui se trouvent dans un espace de nom, répertorier les entités qui se trouvent dans un package, et obtenir la description de chaque entité d'un package.

1. Obtenez la liste des packages qui se trouvent dans l'espace de nom `/whisk.system`.
  ```
  ibmcloud wsk package list /whisk.system
  ```
  {: pre}

  Liste de packages obtenue :
  ```
  packages
  /whisk.system/cloudant                                                 shared
  /whisk.system/alarms                                                   shared
  /whisk.system/watson                                                   shared
  /whisk.system/websocket                                                shared
  /whisk.system/weather                                                  shared
  /whisk.system/system                                                   shared
  /whisk.system/utils                                                    shared
  /whisk.system/slack                                                    shared
  /whisk.system/samples                                                  shared
  /whisk.system/github                                                   shared
  /whisk.system/pushnotifications                                        shared
  ```
  {: screen}

2. Obtenez la liste des entités qui se trouvent dans le package `/whisk.system/cloudant`.
  ```
  ibmcloud wsk package get --summary /whisk.system/cloudant
  ```
  {: pre}

  Exemple de sortie :
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.Bluemix_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  Cette sortie indique que le package {{site.data.keyword.cloudant_short_notm}} fournit deux actions, `read` et `write`, ainsi qu'un flux de déclencheur nommé `changes`. Le flux `changes` entraîne l'exécution de déclencheurs en cas d'ajout de documents dans la base de données {{site.data.keyword.cloudant_short_notm}} spécifiée.

  Le package {{site.data.keyword.cloudant_short_notm}} définit également les paramètres `username`, `password`, `host` et `port`. Ceux-ci doivent être spécifiés pour que les actions et les flux aient un sens. Par exemple, les paramètres autorisent les actions sur un compte {{site.data.keyword.cloudant_short_notm}} spécifique.

3. Obtenez une description de l'action `/whisk.system/cloudant/read`.
  ```
  ibmcloud wsk action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  Exemple de sortie :
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```
  {: screen}

  Cette sortie indique que l'action {{site.data.keyword.cloudant_short_notm}} `read` requiert trois paramètres, y compris l'ID de la base de données et du document à récupérer.

## Appel d'actions dans un package
{: #openwhisk_package_invoke}

Vous pouvez appeler des actions dans un package, à l'instar des autres actions. Les quelques étapes suivantes expliquent comment appeler l'action `greeting` dans le package `/whisk.system/samples` avec différents paramètres.

1. Obtenez une description de l'action `/whisk.system/samples/greeting`.
  ```
  ibmcloud wsk action get --summary /whisk.system/samples/greeting
  ```
  {: pre}

  Exemple de sortie :
  ```
  action /whisk.system/samples/greeting: Print a friendly greeting
     (params: name place)
  ```
  {: screen}

  Remarquez que l'action `greeting` admet deux paramètres : `name` et `place`.

2. Appelez l'action sans paramètre.
  ```
  ibmcloud wsk action invoke --blocking --result /whisk.system/samples/greeting
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
      "payload": "Hello, stranger from somewhere!"
  }
  ```
  {: screen}

  La sortie est un message générique car aucun paramètre n'a été spécifié.

3. Appelez l'action avec des paramètres.
  ```
  ibmcloud wsk action invoke --blocking --result /whisk.system/samples/greeting --param name Mork --param place Ork
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
      "payload": "Hello, Mork from Ork!"
  }
  ```
  {: screen}

  Remarquez que la sortie utilise les paramètres `name` et `place` qui ont été transmis à l'action.

## Création et utilisation de liaisons de package
{: #openwhisk_package_bind}

Vous pouvez utiliser les entités d'un package directement, mais il se peut que vous transmettiez les mêmes paramètres à l'action à chaque fois. Vous pouvez simplifier le processus en effectuant la liaison à un package et en indiquant les paramètres par défaut, qui sont hérités par les actions dans le package.

Par exemple, dans le package `/whisk.system/cloudant`, vous pouvez définir les valeurs par défaut `username`, `password` et `dbname` dans une liaison de package ; elles seront transmises automatiquement aux actions du package.

Dans l'exemple simple ci-dessous, vous établissez une liaison au package `/whisk.system/samples`.

1. Etablissez la liaison au package `/whisk.system/samples` et définissez une valeur de paramètre `place` par défaut.
  ```
  ibmcloud wsk package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: created binding valhallaSamples
  ```
  {: screen}

2. Obtenez une description de la liaison de package.
  ```
  ibmcloud wsk package get --summary valhallaSamples
  ```
  {: pre}

  Exemple de sortie :
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Returns a friendly greeting
   action /myNamespace/valhallaSamples/wordCount: Count words in a string
   action /myNamespace/valhallaSamples/helloWorld: Demonstrates logging facilities
   action /myNamespace/valhallaSamples/curl: Curl a host url
  ```
  {: screen}

  Remarquez que toutes les actions dans le package `/whisk.system/samples` sont disponibles dans la liaison de package `valhallaSamples`.

3. Appelez une action dans la liaison de package.
  ```
  ibmcloud wsk action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  Dans le résultat, vous constatez que l'action hérite du paramètre `place` que vous avez défini lorsque vous avez créé la liaison de package `valhallaSamples`.

4. Appelez une action et remplacez la valeur de paramètre par défaut.
  ```
  ibmcloud wsk action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  Remarquez que la valeur de paramètre `place` qui est spécifiée avec l'appel d'action remplace la valeur par défaut définie dans la liaison de package `valhallaSamples`.

## Création et utilisation de flux de déclencheurs
{: #openwhisk_package_trigger}

Les flux sont pratiques pour configurer une source d'événements externe afin de déclencher ces événements dans un déclencheur {{site.data.keyword.openwhisk_short}}. Cet exemple montre comment utiliser un flux dans le package Alarms afin d'exécuter un déclencheur toutes les secondes, et comment utiliser une règle permettant d'appeler une action toutes les secondes.

1. Obtenez une description du flux dans le package `/whisk.system/alarms`.
  ```
  ibmcloud wsk package get --summary /whisk.system/alarms
  ```
  {: pre}

  Exemple de sortie :
  ```
  package /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
  ```
  {: screen}

  ```
  ibmcloud wsk action get --summary /whisk.system/alarms/alarm
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

2. Créez un déclencheur qui s'exécute toutes les huit secondes.
  ```
  ibmcloud wsk trigger create everyEightSeconds --feed /whisk.system/alarms/alarm -p cron "*/8 * * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: created trigger feed everyEightSeconds
  ```
  {: screen}

3. Créez un fichier nommé **hello.js** avec le code d'action suivant :
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. Vérifiez que l'action existe.
  ```
  ibmcloud wsk action update hello hello.js
  ```
  {: pre}

5. Créez une règle qui appelle l'action **hello** à chaque fois que le déclencheur `everyEightSeconds` s'exécute.
  ```
  ibmcloud wsk rule create myRule everyEightSeconds hello
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: created rule myRule
  ```
  {: screen}

6. Vérifiez que l'action est appelée en interrogeant les journaux d'activation.
  ```
  ibmcloud wsk activation poll
  ```
  {: pre}

  Comme vous pouvez le constater, les activations sont observées toutes les huit secondes pour le déclencheur, la règle et l'action. L'action reçoit les paramètres `{"name":"Mork", "place":"Ork"}` à chaque appel.

## Création d'un package
{: #openwhisk_packages_create}

Un package est utilisé pour organiser un ensemble d'actions et de flux connexes.
Il permet également de partager des paramètres entre toutes les entités du package.

Pour créer un package personnalisé contenant une action simple, procédez comme suit.

1. Créez un package appelé **custom**.
  ```
  ibmcloud wsk package create custom
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: created package custom
  ```
  {: screen}

2. Obtenez un récapitulatif du package.
  ```
  ibmcloud wsk package get --summary custom
  ```
  {: pre}

  Exemple de sortie :
  ```
  package /myNamespace/custom
  ```
  {: screen}

  Remarquez que le package est vide.

3. Créez un fichier appelé `identity.js` contenant le code d'action suivant. Cette action renvoie tous les paramètres d'entrée.
  ```javascript
  function main(args) { return args; }
  ```
  {: codeblock}

4. Créez une action appelée **identity** dans le package `custom`.
  ```
  ibmcloud wsk action create custom/identity identity.js
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: created action custom/identity
  ```
  {: screen}

  La création d'une action dans un package requiert que vous ajoutiez le nom de package comme préfixe au nom d'action. L'imbrication de packages n'est pas autorisée. Un package ne peut contenir que des actions et ne peut pas comporter d'autres packages.

5. Obtenez à nouveau un récapitulatif du package.
  ```
  ibmcloud wsk package get --summary custom
  ```
  {: pre}

  Exemple de sortie :
  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```
  {: screen}

  A présent, vous pouvez voir l'action **custom/identity** dans votre espace de nom.

6. Appelez l'action dans le package.
  ```
  ibmcloud wsk action invoke --blocking --result custom/identity
  ```
  {: pre}

  Exemple de sortie :
  ```
  {}
  ```
  {: screen}

Vous pouvez configurer des paramètres par défaut pour toutes les entités d'un package en définissant des paramètres au niveau du package qui sont hérités par toutes les actions du package. L'exemple suivant montre comment cet héritage fonctionne :

1. Mettez à jour le package **custom** avec deux paramètres : `city` et `country`.
  ```
  ibmcloud wsk package update custom --param city Austin --param country USA
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: updated package custom
  ```
  {: screen}

2. Affichez les paramètres dans le package **custom** et l'action **identity**, et observez comment l'action **identity** dans le package hérite des paramètres du package.
  ```
  ibmcloud wsk package get custom parameters
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: got package custom, displaying field parameters

  [
      {
          "key": "city",
          "value": "Austin"
      },
      {
          "key": "country",
          "value": "USA"
      }
  ]
  ```
  {: screen}

  ```
  ibmcloud wsk action get custom/identity parameters
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: got action custom/identity, displaying field parameters

  [
      {
          "key": "city",
          "value": "Austin"
      },
      {
          "key": "country",
          "value": "USA"
      }
  ]
  ```
  {: screen}

3. Appelez l'action **identity** sans paramètre afin de vérifier qu'elle hérite effectivement des paramètres.
  ```
  ibmcloud wsk action invoke --blocking --result custom/identity
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
      "city": "Austin",
      "country": "USA"
  }
  ```
  {: screen}

4. Appelez l'action **identity** avec quelques paramètres. Les paramètres d'appel sont fusionnés avec les paramètres du package et les remplacent.
  ```
  ibmcloud wsk action invoke --blocking --result custom/identity --param city Dallas --param state Texas
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
      "city": "Dallas",
      "country": "USA",
      "state": "Texas"
  }
  ```
  {: screen}

## Partage d'un package
{: #openwhisk_packages_share}

Une fois que les actions et les flux qui constituent un package ont été débogués et testés, le package peut être partagé avec tous les utilisateurs de {{site.data.keyword.openwhisk_short}}. Le partage du package permet aux utilisateurs de lier le package, d'appeler des actions du package et de créer des règles et des actions de séquence {{site.data.keyword.openwhisk_short}}.

1. Partagez le package avec tous les utilisateurs :
  ```
  ibmcloud wsk package update custom --shared yes
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: updated package custom
  ```
  {: screen}

2. Affichez la propriété `publish` du package pour vérifier qu'elle est désormais associée à la valeur true.
  ```
  ibmcloud wsk package get custom publish
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: got package custom, displaying field publish

  true
  ```
  {: screen}

Désormais, d'autres utilisateurs peuvent se servir de votre package **custom**, et notamment établir une liaison au package ou appeler directement une action qu'il contient. Pour ce faire, ils doivent connaître les noms qualifiés complets du package. Les actions et les flux qui se trouvent dans un package partagé sont _publics_. Si le package est privé, son contenu est également privé.

1. Obtenez une description du package pour afficher les noms qualifiés complets du package et de l'action.
  ```
  ibmcloud wsk package get --summary custom
  ```
  {: pre}

  Exemple de sortie :
  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```
  {: screen}

  Dans l'exemple précédent, vous utilisiez l'espace de nom **myNamespace** et cet espace de nom figure dans le nom qualifié complet.
