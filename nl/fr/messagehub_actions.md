---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# {{site.data.keyword.messagehub}}
{: #openwhisk_catalog_message_hub}

Package qui permet de communiquer avec des instances [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub) pour publier et consommer des messages en utilisant l'API Kafka hautes performances native.
{: shortdesc}

## Configuration d'un package {{site.data.keyword.messagehub}} à l'aide d'{{site.data.keyword.Bluemix_notm}}
{: #create_message_hub_ibm}

1. Créez une instance de service {{site.data.keyword.messagehub}} sous l'organisation et l'espace en cours que vous utilisez pour {{site.data.keyword.openwhisk}}.

2. Vérifiez que la rubrique que vous souhaitez écouter est disponible dans {{site.data.keyword.messagehub}} ou créez une nouvelle rubrique, intitulée **mytopic**, par exemple.

3. Actualisez les packages dans votre espace de nom. Cette opération crée automatiquement une liaison de package pour l'instance de service {{site.data.keyword.messagehub}} que vous avez créée.
  ```
  ibmcloud wsk package refresh
  ```
  {: pre}

  Exemple de sortie :
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```
  {: screen}

4. Affichez la liste des packages de votre espace de nom pour montrer que votre liaison de package est à présent disponible.
  ```
  ibmcloud wsk package list
  ```
  {: pre}

  Exemple de sortie :
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```
  {: screen}

  Votre liaison de package contient désormais les données d'identification associées à votre instance {{site.data.keyword.messagehub}}.

## Configuration d'un package {{site.data.keyword.messagehub}} en dehors d'{{site.data.keyword.Bluemix_notm}}

Si vous souhaitez configurer votre package {{site.data.keyword.messagehub}} en dehors d'{{site.data.keyword.Bluemix_notm}}, vous devez créer manuellement une liaison de package pour votre service {{site.data.keyword.messagehub}}. Pour cela, vous avez besoin des données d'identification et des informations de connexion pour le service {{site.data.keyword.messagehub}}.

Créez une liaison de package configurée pour votre service {{site.data.keyword.messagehub}}.
```
ibmcloud wsk package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

## Ecoute de messages à l'aide d'événements

Pour obtenir des informations détaillées sur l'utilisation des déclencheurs dans {{site.data.keyword.messagehub}} pour l'écoute des messages, voir la rubrique ci-après
[Source d'événements {{site.data.keyword.messagehub}}](./openwhisk_messagehub.html) où sont décrites les tâches suivantes :
* [Création d'un déclencheur à l'écoute d'une instance {{site.data.keyword.messagehub}}](./openwhisk_messagehub.html#create_message_hub_trigger)
* [Création d'un déclencheur pour un package {{site.data.keyword.messagehub}} en dehors d'{{site.data.keyword.Bluemix_notm}}](./openwhisk_messagehub.html#create_message_hub_trigger_outside)
* [Ecoute de messages](./openwhisk_messagehub.html#message_hub_listen)
* [Exemples](./openwhisk_messagehub.html#examples)

## Production de messages vers {{site.data.keyword.messagehub}}
{: #producing_messages}

L'action `/messaging/messageHubProduce` est obsolète et sera supprimée prochainement. Pour conserver des performances optimales, au lieu d'utiliser l'action `/messaging/messageHubProduce`, passez par une connexion persistante lorsque des données sont produites dans {{site.data.keyword.messagehub}}/Kafka.
{: tip}

Si vous désirez utiliser une action {{site.data.keyword.openwhisk_short}} pour produire facilement un message vers {{site.data.keyword.messagehub}}, vous pouvez utiliser l'action `/messaging/messageHubProduce`. Cette action reçoit les paramètres suivants :

|Nom|Type|Description|
|---|---|---|
|kafka_brokers_sasl|Tableau JSON de chaînes|Ce paramètre est un tableau de chaînes `<host>:<port>` composé des courtiers de votre instance {{site.data.keyword.messagehub}} instance.|
|utilisateur|Chaîne|Votre nom d'utilisateur {{site.data.keyword.messagehub}}.|
|password|Chaîne|Votre mot de passe {{site.data.keyword.messagehub}}.|
|topic|Chaîne|Rubrique que le déclencheur doit écouter.|
|value|Chaîne|Valeur du message que vous désirez générer.|
|key|Chaîne (facultatif)|Clé du message que vous désirez générer.|

Tandis que les trois premiers paramètres peuvent être liés automatiquement à l'aide de la commande `ibmcloud wsk package refresh`, consultez l'exemple d'appel d'action suivant intégrant tous les paramètres requis :
```
ibmcloud wsk action invoke /messaging/messageHubProduce -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p topic mytopic -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p value "This is the content of my message"
```
{: pre}

## Références
- [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
