---

copyright:

  years: 2018

lastupdated: "2018-05-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Déploiement de modèles Quickstart
{: #serviceauth}

{{site.data.keyword.openwhisk}} offre un catalogue de modèles destinés à vous permettre de commencer rapidement à travailler sur votre nouveau projet. Les modèles représentent une combinaison d'actions, de déclencheurs et de séquences et peuvent également incorporer des instances de service dans {{site.data.keyword.Bluemix}}. Les modèles vous permettent de créer rapidement et facilement un projet et de commencer immédiatement le codage.

Ce tutoriel vous guide à travers les étapes de déploiement du modèle Cloudant.
{: shortdesc}

## Modèles Quickstart disponibles
{: #available-templates}

| Nom | Description | Environnements d'exécution pris en charge |
|:-----------------|:-----------------|:-----------------|
| [Cloudant Events](./deploy_templates.html#cloudant-template) | Lorsqu'un document est édité ou ajouté dans une base de données Cloudant, la modification est consignée dans la console. | Node.js, Swift, Python, PHP |
| [Get HTTP Resource](./deploy_templates.html#get-http-resource-template) | Action Web appelée en réponse à un événement HTTP et qui extrait des données de l'API Yahoo Weather. | Node.js, Python |
| [Hello World](./deploy_templates.html#hello-world-template) | Cette action n'accepte qu'un seul paramètre qui doit être un objet JSON. | Node.js, Swift, Python, PHP |
| [Message Hub Events](./deploy_templates.html#messagehub-events-template) | Lorsque de nouvelles données sont ajoutées dans une rubrique Message Hub, la modification est consignée dans la console. | Node.js, Swift, Python, PHP |
| [Periodic Slack Reminder](./deploy_templates.html#slack-reminder-template) |Action qui sera publiée dans Slack en fonction d'un déclencheur périodique. | Node.js, Swift, Python, PHP |

## Déploiement du modèle Cloudant Events
{: #cloudant-template}

Le modèle Cloudant crée une séquence d'actions et un déclencheur qui lancera cette séquence. Le déclencheur est lancé en cas de modification de la base de données Cloudant connectée, qui doit être une base de données de chats, avec un nom et une couleur. L'élément de données attendu est un chat portant le nom et la couleur définis. Lorsqu'un nouveau chat est ajouté à la base de données ou qu'un chat en cours est édité, les données sont consignées sur la console.

1. Pour créer un modèle, accédez à [{{site.data.keyword.openwhisk_short}} dans {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/), puis cliquez sur **Commencer la création**.

2. Cliquez sur **Modèles Quickstart**.

3. Cliquez sur **Nouvel élément Cloudant**.

### Création de l'action Cloudant

1. Ensuite, entrez un nom pour votre package ou utiliser le nom fourni par défaut, `new-cloudant-item`.

2. Dans le menu déroulant **Actions**, sélectionnez l'environnement d'exécution des actions que vous posséderez (Node.js, Swift, Python ou PHP). Pour cet exemple, sélectionnez **Node.js**, puis cliquez sur **Suivant**.

### Création du déclencheur Cloudant

Les déclencheurs appellent des actions lorsqu'ils reçoivent des événements de la part de sources d'événements externes. Afin de créer un déclencheur pour un modèle Cloudant, indiquez au déclencheur les informations d'instance de service Cloudant requises.

#### Création d'une instance de service Cloudant

Les options disponibles sont les suivantes :
  * **Créer une instance**
  * **Entrez vos propres données d'identification**

1. Pour cet exemple, choisissez **Créer une instance**.

2. Une fenêtre contextuelle s'ouvre et vous dirige vers un nouvel onglet contenant la page de configuration Cloudant. Après avoir créé l'instance Cloudant, vous devez créer un ensemble de données d'identification de service, puis fermer l'onglet pour revenir à cette page en cliquant sur **Ok**.

3. A présent, choisissez **Entrez vos propres données d'identification** et indiquez les informations suivantes :
  * Utilisateur - _Votre nom d'utilisateur Cloudant_
  * Mot de passe - _Votre mot de passe Cloudant_
  * Hôte - _Généralement, `username.cloudant.com`_
  * Base de données - _Nom de votre base de données Cloudant_

### Déploiement du modèle Cloudant

Cliquez sur **Déployer**.

Une fois le déploiement du modèle terminé, vous pouvez apporter d'autres modifications au code afin de le personnaliser en fonction de vos besoins, ou revenir en arrière et vérifier le catalogue de modèles disponibles.

## Déploiement du modèle Get HTTP Resource
{: #get-http-resource-template}

Le modèle Get HTTP Resource crée une action permettant d'extraire une ressource externe, l'API Yahoo Weather, puis renvoie des données. Cette action est activée en tant qu'action Web, ce qui lui permet d'être appelée avec une adresse URL compatible avec CORS sans avoir besoin de clé d'authentification, ce qui est pratique pour générer des système de back end pour les applications Web. **Remarque** : par défaut, le noeud final `get-http-resource` est disponible publiquement pour toutes les personnes qui souhaitent y faire appel.

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

## Déploiement du modèle Message Hub Events
{: #messagehub-events-template}

Le modèle Message Hub Events crée une action et un déclencheur pour la lancer. Le déclencheur s'exécute chaque fois qu'un nouvel élément est ajouté dans la rubrique Message Hub sélectionnée lors de la création du modèle.

1. Pour créer un modèle, accédez à [{{site.data.keyword.openwhisk_short}} dans {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/), puis cliquez sur **Commencer la création**.

2. Cliquez sur **Modèles Quickstart**.

3. Vérifiez la zone **Nom de package**, vous pouvez la mettre à jour si nécessaire. La valeur par défaut `message-hub-events` est définie pour vous.

4. Choisissez l'environnement d'exécution des actions que vous posséderez : Node.js 8, Node.js 6, Python 3, Swift 4 ou PHP 7.1.

5. Cliquez sur **Suivant**.

### Création du déclencheur Message Hub

Les déclencheurs appellent des actions lorsqu'ils reçoivent des événements de la part de sources d'événements externes. Afin de créer un déclencheur pour le modèle Message Hub, indiquez au déclencheur les informations d'instance de service Message Hub requises.

Vérifiez la zone **Nom du déclencheur**, vous pouvez la mettre à jour si nécessaire. La valeur par défaut `message-hub-events-trgr` est définie pour vous.

### Création de l'instance de service Message Hub

Les options disponibles sont les suivantes :
  * **Créer une instance**
  * **Entrez vos propres données d'identification**

1. Pour cet exemple, choisissez **Créer une instance**.

2. Une fenêtre contextuelle s'ouvre et vous dirige vers un nouvel onglet contenant la page de configuration de Message Hub. Après avoir créé l'instance Message Hub, vous devez créer un ensemble de données d'identification de service, puis fermer l'onglet pour revenir à cette page en cliquant sur **Ok**.

3. A présent, choisissez **Entrez vos propres données d'identification** et indiquez les informations suivantes :
  * Utilisateur - _Votre nom d'utilisateur Message Hub_
  * Mot de passe - _Votre mot de passe Message Hub_
  * kafka_admin_url - _Adresse URL REST d'administration de Message Hub_
  * Base de données - _Nom de votre base de données Message Hub_
  * Rubrique - _Rubrique à laquelle s'abonner_

### Déploiement du modèle Message Hub

Cliquez sur **Déployer**.

Une fois le déploiement du modèle terminé, vous pouvez apporter d'autres modifications au code afin de le personnaliser en fonction de vos besoins, ou revenir en arrière et vérifier le catalogue de modèles disponibles.

## Déploiement du modèle Periodic Slack Reminder
{: #slack-reminder-template}

Le modèle Periodic Slack Reminder publie dans Slack en fonction d'un intervalle fourni par l'utilisateur lors de la création du déclencheur. Avant de créer ce modèle, accédez à https://api.slack.com/incoming-webhooks pour configurer l'adresse URL des webhooks entrants.

1. Vérifiez la zone **Nom de package**, vous pouvez la mettre à jour si nécessaire. La valeur par défaut `periodic-slack-reminder` est définie pour vous.

2. Choisissez l'environnement d'exécution des actions que vous posséderez : Node.js 8, Node.js 6, Python 3, Swift 4 ou PHP 7.1.

3. Sous la section **Paramètres**, entrez l'URL du webhook dans la zone **Valeur du paramètre**, puis cliquez sur **Suivant**. (Exemple : https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX).

### Création du déclencheur Slack Reminder

Les déclencheurs appellent des actions lorsqu'ils reçoivent des événements de la part de sources d'événements externes. Afin de créer un déclencheur pour le modèle Slack Reminder, indiquez au déclencheur les informations d'instance de service Slack Reminder requises.

1. Vérifiez la zone **Nom du déclencheur**, vous pouvez la mettre à jour si nécessaire. La valeur par défaut `periodic-slack-reminder-trgr` est définie pour vous.

2. Vous pouvez ensuite spécifier l'intervalle d'exécution du déclencheur en utilisant un modèle ou une expression cron. Vous pouvez sélectionner un temps TUC pour les jours de la semaine, les heures et les minutes. Sélectionnez l'intervalle désiré pour être prêt à déployer le modèle.

3. Cliquez sur **Déployer**.

Une fois le déploiement du modèle terminé, vous pouvez apporter d'autres modifications au code afin de le personnaliser en fonction de vos besoins, ou revenir en arrière et vérifier le catalogue de modèles disponibles.
