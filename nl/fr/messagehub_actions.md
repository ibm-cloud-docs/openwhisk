---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: message hub, package, messages, events

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Package {{site.data.keyword.messagehub}} 

{: #catalog_message_hub}

Package qui permet de communiquer avec des instances [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub) pour publier et consommer des messages en utilisant l'API Kafka hautes performances native.
{: shortdesc}

## Configuration d'un package {{site.data.keyword.messagehub}} à l'aide d'{{site.data.keyword.Bluemix_notm}}
{: #create_message_hub_ibm}

1. Créez une instance de service {{site.data.keyword.messagehub}} sous l'organisation et l'espace en cours que vous utilisez pour {{site.data.keyword.openwhisk}}.

2. Vérifiez que la rubrique que vous souhaitez écouter est disponible dans {{site.data.keyword.messagehub}} ou créez une nouvelle rubrique, intitulée **mytopic**, par exemple.

3. Actualisez les packages dans votre espace de nom. Cette opération crée automatiquement une liaison de package pour l'instance de service {{site.data.keyword.messagehub}} que vous avez créée.
  ```
  ibmcloud fn package refresh
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
  ibmcloud fn package list
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
ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

## Ecoute de messages à l'aide d'événements

Pour obtenir des informations détaillées sur l'utilisation des déclencheurs dans {{site.data.keyword.messagehub}} pour l'écoute des messages, voir la rubrique ci-après
[Source d'événements {{site.data.keyword.messagehub}}](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub) où sont décrites les tâches suivantes :
* [Création d'un déclencheur à l'écoute d'une instance {{site.data.keyword.messagehub}}](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#create_message_hub_trigger)
* [Création d'un déclencheur pour un package {{site.data.keyword.messagehub}} en dehors d'{{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#create_message_hub_trigger_outside)
* [Ecoute de messages](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#message_hub_listen)
* [Exemples](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#examples)

## Production de messages vers {{site.data.keyword.messagehub}}
{: #producing_messages}

L'action `/messaging/messageHubProduce` est obsolète et sera supprimée prochainement. Il est déjà retiré de la région Tokyo. Pour conserver des performances optimales, au lieu d'utiliser l'action `/messaging/messageHubProduce`, passez par une connexion persistante lorsque des données sont produites dans {{site.data.keyword.messagehub}}/Kafka.
{: tip}

Pour en savoir plus sur la génération de messages, consultez la [documentation Event Streams](/docs/services/EventStreams?topic=eventstreams-producing_messages#producing_messages). 

## Références
- [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub)
- [Apache Kafka](https://kafka.apache.org)
