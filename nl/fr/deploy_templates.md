---

copyright:

  years: 2018

lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Déploiement de modèles dans {{site.data.keyword.openwhisk_short}}
{: #serviceauth}

{{site.data.keyword.openwhisk}} offre un catalogue de modèles destinés à vous permettre de commencer rapidement à travailler sur votre nouveau projet. Les modèles représentent une combinaison d'actions, de déclencheurs et de séquences et peuvent également incorporer des instances de service dans {{site.data.keyword.Bluemix}}. Les modèles vous permettent de créer rapidement et facilement un projet et de commencer immédiatement le codage.  

Ce tutoriel vous guide à travers les étapes de déploiement de modèle cloudant.
{: shortdesc}

## Modèles disponibles
{: #available-templates}

| Nom | Description | Environnements d'exécution pris en charge| 
|:-----------------|:-----------------|:-----------------|
| Cloudant Events | Lorsqu'un document est édité ou ajouté dans une base de données Cloudant, la modification est consignée dans la console. | Node.js, Swift, Python, PHP |
| Get HTTP Resource | Action Web qui est appelée dans la réponse à un événement HTTP et qui extrait les données de l'API Yahoo Weather. | Node.js, Python |
| Hello World | Cette action n'accepte qu'un seul paramètre et celui-ci doit être un objet JSON.| Node.js, Swift, Python, PHP |
| Message Hub Events | Lorsque de nouvelles données sont ajoutées dans une rubrique Message Hub, la modification est consignée dans la console. | Node.js, Swift, Python, PHP | 
| Periodic Slack Reminder | Action qui sera publiée sur Slack en fonction d'un déclencheur périodique. | Node.js, Swift, Python, PHP |

## Déploiement du modèle Cloudant Events
{: #cloudant-template}

Le modèle Cloudant crée une séquence d'actions et un déclencheur qui lancera cette séquence. Le déclencheur est lancé en cas de modification de la base de données Cloudant connectée, qui doit être une base de données de chats, avec un nom et une couleur. L'élément de données attendu est un chat portant le nom et la couleur définis. Lorsqu'un nouveau chat est ajouté à la base de données ou qu'un chat en cours est édité, les données sont consignées sur la console. 

1. Pour créer un modèle, accédez à [{{site.data.keyword.openwhisk_short}} dans {{site.data.keyword.Bluemix_notm}}](https://dev-console.stage1.bluemix.net/openwhisk/), puis cliquez sur **Commencer la création**. 

2. Cliquez sur **Deploy Template**.

3. Cliquez sur **New Cloudant Item**.

### Création de l'action Cloudant

1. Ensuite, entrez un nom pour votre package ou utiliser le nom fourni par défaut, `new-cloudant-item`. 

2. Dans le menu déroulant **Actions**, sélectionnez l'environnement d'exécution des actions que vous posséderez (nodejs, swift, python ou php). Pour cet exemple, sélectionnez **nodejs**, puis cliquez sur **Next**.

### Création du déclencheur Cloudant

Les déclencheurs appellent des actions lorsqu'ils reçoivent des événements de la part de sources d'événements externes. Afin de créer un déclencheur pour un modèle Cloudant, indiquez au déclencheur les informations d'instance de service Cloudant requises. 

#### Création d'une instance de service Cloudant

Les options disponibles sont les suivantes :
  * **Create your own instance**
  * **Input your own credentials** 

1. Pour cet exemple, choisissez **Create your own instance**.

2. Une fenêtre contextuelle s'ouvre et vous dirige vers un nouvel onglet contenant la page de configuration Cloudant. Après avoir créé l'instance Cloudant, vous devez créer un ensemble de données d'identification de service, puis fermer l'onglet pour revenir à cette page en cliquant sur **Ok**.

3. A présent, choisissez **Input your own credentials** et indiquez les informations suivantes :
  * Username - _Votre nom d'utilisateur Cloudant_
  * Password - _Votre mot de passe Cloudant_
  * Host - _Généralement, `username.cloudant.com`_
  * Database - _Nom de votre base de données Cloudant_

### Déploiement de votre modèle Cloudant

1. Cliquez sur **Deploy**.

Une fois le déploiement du modèle terminé, vous pouvez apporter d'autres modifications au code afin de le personnaliser en fonction de vos besoins, ou revenir en arrière et vérifier le catalogue de modèles disponibles. 

