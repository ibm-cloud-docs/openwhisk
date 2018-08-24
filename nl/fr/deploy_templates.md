---

copyright:
  years: 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Déploiement de modèles Quickstart
{: #serviceauth}

{{site.data.keyword.openwhisk}} offre un catalogue de modèles destinés à vous permettre de commencer rapidement à travailler sur votre nouveau projet. Les modèles représentent une combinaison d'actions, de déclencheurs et de séquences et peuvent également incorporer des instances de service dans {{site.data.keyword.Bluemix}}. Les modèles vous permettent de créer rapidement et facilement un projet et de commencer immédiatement le codage.

Ce tutoriel vous guide à travers les étapes de déploiement des modèle disponibles via {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

## Modèles Quickstart disponibles
{: #available-templates}

| Nom | Description | Environnements d'exécution pris en charge |
|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} Events](./deploy_templates.html#cloudant-template) | Lorsqu'un document {{site.data.keyword.cloudant}} est édité ou ajouté dans une base de données Cloudant, la modification est consignée dans la console. | Node.js, Swift, Python, PHP |
| [Upload Image](./deploy_templates.html#cos-upload-image) | Action Web qui vous permet d'envoyer par téléchargement une image vers le compartiment d'une instance {{site.data.keyword.cos_full}} puis d'extraire une miniature de cette image. | Node.js |
| [Get HTTP Resource](./deploy_templates.html#get-http-resource-template) | Action Web appelée en réponse à un événement HTTP et qui extrait des données de l'API Yahoo Weather. | Node.js, Python |
| [Hello World](./deploy_templates.html#hello-world-template) | Cette action n'accepte qu'un seul paramètre qui doit être un objet JSON. | Node.js, Swift, Python, PHP |
| [{{site.data.keyword.messagehub}} Events](./deploy_templates.html#messagehub-events-template) | Lorsque de nouvelles données sont ajoutées dans une rubrique {{site.data.keyword.messagehub_full}}, la modification est consignée dans la console. | Node.js, Swift, Python, PHP |
| [Periodic Slack Reminder](./deploy_templates.html#slack-reminder-template) |Action qui sera publiée dans Slack en fonction d'un déclencheur périodique. | Node.js, Swift, Python, PHP |

## Déploiement du modèle {{site.data.keyword.cloudant_short_notm}} Events
{: #cloudant-template}

Le modèle {{site.data.keyword.cloudant_short_notm}} crée une séquence d'actions et un déclencheur qui lancera cette séquence. Le déclencheur est lancé en cas de modification de la base de données {{site.data.keyword.cloudant_short_notm}} connectée, qui doit être une base de données de chats, avec un nom et une couleur. L'élément de données attendu est un chat portant le nom et la couleur définis. Lorsqu'un nouveau chat est ajouté à la base de données ou qu'un chat en cours est édité, les données sont consignées sur la console.

1. Pour créer un modèle, accédez à [{{site.data.keyword.openwhisk_short}} dans {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/), puis cliquez sur **Commencer la création**.

2. Cliquez sur **Modèles Quickstart**.

3. Cliquez sur **Cloudant Events**.

### Création de l'action {{site.data.keyword.cloudant_short_notm}}

1. Ensuite, entrez un nom pour votre package ou utiliser le nom fourni par défaut, `new-cloudant-item`.

2. Dans le menu déroulant **Actions**, sélectionnez l'environnement d'exécution des actions que vous posséderez (Node.js, Swift, Python ou PHP). Pour cet exemple, sélectionnez **Node.js**, puis cliquez sur **Suivant**.

### Création du déclencheur {{site.data.keyword.cloudant_short_notm}}

Les déclencheurs appellent des actions lorsqu'ils reçoivent des événements de la part de sources d'événements externes. Afin de créer un déclencheur pour un modèle {{site.data.keyword.cloudant_short_notm}}, indiquez au déclencheur les informations d'instance de service {{site.data.keyword.cloudant_short_notm}} requises.

#### Création d'une instance de service {{site.data.keyword.cloudant_short_notm}}

Les options disponibles sont les suivantes :
  * **Créez une instance**
  * **Entrez vos propres données d'identification**

1. Pour cet exemple, choisissez **Créez une instance**.

2. Une fenêtre contextuelle s'ouvre et vous dirige vers un nouvel onglet contenant la page de configuration {{site.data.keyword.cloudant_short_notm}}. Après avoir créé l'instance {{site.data.keyword.cloudant_short_notm}}, vous devez créer un ensemble de données d'identification de service, puis fermer l'onglet pour revenir à cette page en cliquant sur **Ok**.

3. A présent, choisissez **Entrez vos propres données d'identification** et indiquez les informations suivantes :
  * Utilisateur - _Votre nom d'utilisateur {{site.data.keyword.cloudant_short_notm}}_
  * Mot de passe - _Votre mot de passe {{site.data.keyword.cloudant_short_notm}}_
  * Hôte - _Généralement, `username.{{site.data.keyword.cloudant_short_notm}}.com`_
  * Base de données - _Nom de votre base de données {{site.data.keyword.cloudant_short_notm}}_

### Déploiement du modèle {{site.data.keyword.cloudant_short_notm}}

Cliquez sur **Déployer**.

Une fois le déploiement du modèle terminé, vous pouvez apporter d'autres modifications au code afin de le personnaliser en fonction de vos besoins, ou revenir en arrière et vérifier le catalogue de modèles disponibles.

## Déploiement du modèle Télécharger une image
{: #cos-upload-image}

Le modèle Télécharger une image crée une action Web qui vous permet d'envoyer par téléchargement une image à un compartiment {{site.data.keyword.cos_short_notm}} via une petite interface. Le modèle extrait ensuite l'image sous la forme d'une miniature et l'affiche dans l'interface de l'action Web.

Pour déployer le modèle :

1. Accédez à la console {{site.data.keyword.openwhisk_short}} dans [{{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/openwhisk/).

2. Cliquez sur **Commencer la création**.

2. Cliquez sur **Modèles Quickstart**.

3. Cliquez sur le modèle **Télécharger une image**.

4. Entrez un nom pour votre package ou utilisez le nom par défaut `upload-image`.

5. Cliquez sur **Suivant**.

6. Le modèle requiert les données d'identification du service d'une instance de service {{site.data.keyword.cos_full_notm}}. Dans la liste **{{site.data.keyword.cos_short}}**,
sélectionnez l'une des options suivantes : 
  * **Créez une nouvelle instance** : si vous n'avez pas d'instance de service, sélectionnez cette option pour en créer une.
      1. Dans la page de création d'instance de service de {{site.data.keyword.cos_full_notm}} qui s'affiche, créez une instance de service.
      2. [Créez un ensemble de données d'identification du service HMAC](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials).
      3. [Créez au moins un compartiment](/docs/services/cloud-object-storage/getting-started.html#create-buckets).
  * **Entrez vos propres données d'identification** : sélectionnez cette option pour saisir manuellement vos propres données d'identification pour une instance de service {{site.data.keyword.cos_short}}. Les données d'identification doivent contenir des clés HMAC et l'instance de service doit avoir au moins un compartiment.
  * **Instances existantes** : si vous avez des instances {{site.data.keyword.cos_short}}, sélectionnez-en une dans la liste. Les données d'identification doivent contenir des clés HMAC et l'instance de service doit avoir au moins un compartiment.

7. Cliquez sur **Déployer**.

8. Dans le volet de navigation gauche, cliquez sur **Noeuds finaux**. 

9. Dans la section Action Web, copiez le lien sans le suffixe .json et collez-le dans la barre d'adresse de votre navigateur. L'interface de l'action Web du modèle s'affiche.

10. Facultatif : une fois le modèle déployé, vous pouvez accéder au tableau de bord Actions afin de personnaliser le code dans les deux nouveaux packages :
    * Le package `cloud-object-storage`, qui contient les actions compatibles avec les instances {{site.data.keyword.cos_short}}
    * Le package de modèle (nom par défaut `upload-image`), qui contient l'action `app`

## Déploiement du modèle Get HTTP Resource
{: #get-http-resource-template}

Le modèle Get HTTP Resource crée une action permettant d'extraire une ressource externe, l'API Yahoo Weather, puis renvoie des données. Cette action est activée en tant qu'action web, ce qui lui permet d'être appelée avec une adresse URL compatible avec CORS sans avoir besoin de clé d'authentification, ce qui est pratique pour générer des système de back end pour les applications Web. **Remarque** : par défaut, le noeud final `get-http-resource` est disponible publiquement pour toutes les personnes qui souhaitent y faire appel.

1. Pour créer un modèle, accédez à [{{site.data.keyword.openwhisk_short}} dans {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/), puis cliquez sur **Commencer la création**.

2. Cliquez sur **Modèles Quickstart**.

3. Vérifiez la zone **Nom de package**, vous pouvez la mettre à jour si nécessaire. La valeur par défaut `get-http-resource` est définie pour vous.

4. Choisissez l'environnement d'exécution des actions que vous posséderez : Node.js 8, Node.js 6 ou Python 3.

5. Cliquez sur **Déployer**.

Une fois le déploiement du modèle terminé, vous pouvez apporter d'autres modifications au code afin de le personnaliser en fonction de vos besoins, ou revenir en arrière et vérifier le catalogue de modèles disponibles.

## Déploiement du modèle Hello World
{: #hello-world-template}

 Cette action n'accepte qu'un seul paramètre qui doit être un objet JSON. 

1. Pour créer un modèle, accédez à [{{site.data.keyword.openwhisk_short}} dans {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/), puis cliquez sur **Commencer la création**.

2. Cliquez sur **Modèles Quickstart**.

3. Vérifiez la zone **Nom de package**, vous pouvez la mettre à jour si nécessaire. La valeur par défaut `hello-world` est définie pour vous.

4. Choisissez l'environnement d'exécution des actions que vous posséderez : Node.js 8, Node.js 6, Python 3, Swift 4 ou PHP 7.1.

5. Cliquez sur **Déployer**.

Une fois le déploiement du modèle terminé, vous pouvez apporter d'autres modifications au code afin de le personnaliser en fonction de vos besoins, ou revenir en arrière et vérifier le catalogue de modèles disponibles.

## Déploiement du modèle {{site.data.keyword.messagehub}} Events
{: #messagehub-events-template}

Le modèle {{site.data.keyword.messagehub}} Events crée une action et un déclencheur pour la lancer. Le déclencheur s'exécute chaque fois qu'un nouvel élément est ajouté dans la rubrique {{site.data.keyword.messagehub}} sélectionnée lors de la création du modèle.

1. Pour créer un modèle, accédez à [{{site.data.keyword.openwhisk_short}} dans {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/), puis cliquez sur **Commencer la création**.

2. Cliquez sur **Modèles Quickstart**.

3. Vérifiez la zone **Nom de package**, vous pouvez la mettre à jour si nécessaire. La valeur par défaut `message-hub-events` est définie pour vous.

4. Choisissez l'environnement d'exécution des actions que vous posséderez : Node.js 8, Node.js 6, Python 3, Swift 4 ou PHP 7.1.

5. Cliquez sur **Suivant**.

### Création du déclencheur {{site.data.keyword.messagehub}}

Les déclencheurs appellent des actions lorsqu'ils reçoivent des événements de la part de sources d'événements externes. Afin de créer un déclencheur pour un modèle {{site.data.keyword.messagehub}}, indiquez au déclencheur les informations d'instance de service {{site.data.keyword.messagehub}} requises.

Vérifiez la zone **Nom du déclencheur**, vous pouvez la mettre à jour si nécessaire. La valeur par défaut `message-hub-events-trgr` est définie pour vous.

### Création de l'instance de service {{site.data.keyword.messagehub}}

Les options disponibles sont les suivantes :
  * **Créer une instance**
  * **Entrez vos propres données d'identification**

1. Pour cet exemple, choisissez **Créer une instance**.

2. Une fenêtre contextuelle s'ouvre et vous dirige vers un nouvel onglet contenant la page de configuration {{site.data.keyword.messagehub}}. Après avoir créé l'instance {{site.data.keyword.messagehub}}, vous devez créer un ensemble de données d'identification de service, puis fermer l'onglet pour revenir à cette page en cliquant sur **Ok**.

3. A présent, choisissez **Entrez vos propres données d'identification** et indiquez les informations suivantes :
  * Utilisateur - _Votre nom d'utilisateur {{site.data.keyword.messagehub}}_
  * Mot de passe - _Votre mot de passe {{site.data.keyword.messagehub}}_
  * kafka_admin_url - _Adresse URL REST d'administration de {{site.data.keyword.messagehub}}_
  * Base de données - _Nom de votre base de données {{site.data.keyword.messagehub}}_
  * Rubrique - _Rubrique à laquelle s'abonner_

### Déploiement du modèle {{site.data.keyword.messagehub}}

Cliquez sur **Déployer**.

Une fois le déploiement du modèle terminé, vous pouvez apporter d'autres modifications au code afin de le personnaliser en fonction de vos besoins, ou revenir en arrière et vérifier le catalogue de modèles disponibles.

## Déploiement du modèle Periodic Slack Reminder
{: #slack-reminder-template}

Le modèle Periodic Slack Reminder publie dans Slack en fonction d'un intervalle fourni par l'utilisateur lors de la création du déclencheur. Avant de créer ce modèle, accédez à l'adresse https://api.slack.com/incoming-webhooks pour configurer l'adresse URL des webhooks entrants.

1. Vérifiez la zone **Nom de package**, vous pouvez la mettre à jour si nécessaire. La valeur par défaut `periodic-slack-reminder` est définie pour vous.

2. Choisissez l'environnement d'exécution des actions que vous posséderez : Node.js 8, Node.js 6, Python 3, Swift 4 ou PHP 7.1.

3. Sous la section **Paramètres**, entrez l'URL du webhook dans la zone **Valeur du paramètre**, puis cliquez sur **Suivant**. (Exemple : https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX).

### Création du déclencheur Slack Reminder

Les déclencheurs appellent des actions lorsqu'ils reçoivent des événements de la part de sources d'événements externes. Afin de créer un déclencheur pour un modèle Slack Reminder, indiquez au déclencheur les informations d'instance de service {{site.data.keyword.messagehub}} requises.

1. Vérifiez la zone **Nom du déclencheur**, vous pouvez la mettre à jour si nécessaire. La valeur par défaut `periodic-slack-reminder-trgr` est définie pour vous.

2. Vous pouvez ensuite spécifier l'intervalle d'exécution du déclencheur en utilisant un modèle ou une expression cron. Vous pouvez sélectionner un temps TUC pour les jours de la semaine, les heures et les minutes. Sélectionnez l'intervalle désiré pour être prêt à déployer le modèle.

3. Cliquez sur **Déployer**.

Une fois le déploiement du modèle terminé, vous pouvez apporter d'autres modifications au code afin de le personnaliser en fonction de vos besoins, ou revenir en arrière et vérifier le catalogue de modèles disponibles.
