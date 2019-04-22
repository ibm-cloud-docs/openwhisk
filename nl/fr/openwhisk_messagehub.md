---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-19"

keywords: message hub, event, trigger, messages, batch, listen

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Source d'événements Event Streams
{: #openwhisk_catalog_message_hub}

Vous pouvez créer un déclencheur qui réagit à la publication de messages dans une instance {{site.data.keyword.messagehub_full}} en utilisant des flux. Apprenez à créer des déclencheurs {{site.data.keyword.messagehub}} avec ou sans {{site.data.keyword.Bluemix}}, écouter des messages et traiter des messages par lots.
{: shortdesc}

## Package {{site.data.keyword.messagehub}} 

L'action `/messaging/messageHubProduce` est obsolète et sera supprimée prochainement. Il est déjà retiré de la région Tokyo. Pour conserver des performances optimales, au lieu d'utiliser l'action `/messaging/messageHubProduce`, passez par une connexion persistante lorsque les données sont produites dans Message Hub/Kafka.
{: deprecated}

Ce package permet de communiquer avec des instances [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub) pour publier et consommer des messages via l'API Kafka native à hautes performances. Pour plus d'informations sur le package {{site.data.keyword.messagehub}} et savoir comment le configurer et produire des messages, voir la rubrique [Package {{site.data.keyword.messagehub}}](/docs/openwhisk?topic=cloud-functions-catalog_message_hub).

## Création d'un déclencheur à l'écoute d'une instance {{site.data.keyword.messagehub}}
{: #create_message_hub_trigger}

Pour créer un déclencheur qui réagit lorsque des messages sont publiés dans une instance {{site.data.keyword.messagehub}}, vous devez utiliser le flux nommé `/messaging/messageHubFeed`. L'action de flux prend en charge les paramètres suivants :

|Nom|Type|Description|
|---|---|---|
|kafka_brokers_sasl|Tableau JSON de chaînes|Ce paramètre est un tableau de chaînes `<host>:<port>` composé des courtiers de votre instance {{site.data.keyword.messagehub}} instance|
|utilisateur|Chaîne|Votre nom d'utilisateur {{site.data.keyword.messagehub}}.|
|password|Chaîne|Votre mot de passe {{site.data.keyword.messagehub}}.|
|topic|Chaîne|Rubrique que le déclencheur doit écouter.|
|kafka_admin_url|Chaîne d'URL|URL de l'interface REST d'administration de {{site.data.keyword.messagehub}}.|
|isJSONData|Booléen (facultatif - par défaut=false)|Lorsque ce paramètre a pour valeur `true`, le fournisseur tente d'analyser la valeur du message en tant que JSON avant de le transmettre en tant que contenu du déclencheur.|
|isBinaryKey|Booléen (facultatif - par défaut=false)|Lorsque ce paramètre a pour valeur `true`, le fournisseur encode la valeur de la clé en Base64 avant de la transmettre en tant que contenu du déclencheur.|
|isBinaryValue|Booléen (facultatif - par défaut=false)|Lorsque ce paramètre a pour valeur `true`, le fournisseur encode la valeur du message en Base64 avant de la transmettre en tant que contenu du déclencheur.|

Cette liste de paramètres peut vous sembler impressionnante, mais ils peuvent être définis automatiquement à l'aide de la commande de plug-in de l'interface de ligne de commande `ibmcloud fn package refresh`.

1. Créez une instance de service {{site.data.keyword.messagehub}} sous l'organisation et l'espace en cours que vous utilisez pour {{site.data.keyword.openwhisk}}.

2. Vérifiez que la rubrique que vous souhaitez écouter est disponible dans {{site.data.keyword.messagehub}} ou créez une nouvelle rubrique, par exemple, **mytopic**.

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

5. A présent, il ne vous reste plus qu'à créer un déclencheur qui s'exécute lorsque de nouveaux messages sont publiés dans votre rubrique {{site.data.keyword.messagehub}}.
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## Création d'un déclencheur pour un package {{site.data.keyword.messagehub}} en dehors d'{{site.data.keyword.Bluemix_notm}}
{: #create_message_hub_trigger_outside}

Si vous souhaitez configurer votre package {{site.data.keyword.messagehub}} en dehors d'{{site.data.keyword.Bluemix_notm}}, vous devez créer manuellement une liaison de package pour votre service {{site.data.keyword.messagehub}}. Pour cela, vous avez besoin des données d'identification et des informations de connexion pour le service {{site.data.keyword.messagehub}}.

1. Créez une liaison de package configurée pour votre service {{site.data.keyword.messagehub}}.
  ```
  ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. A présent, vous pouvez créer un déclencheur à l'aide de votre nouveau package qui sera exécuté lorsque de nouveaux messages seront publiés dans votre rubrique {{site.data.keyword.messagehub}}.
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}

## Ecoute de messages
{: #message_hub_listen}

Dès qu'un déclencheur est créé, le système surveille la rubrique indiquée dans votre service de messagerie. Lorsque de nouveaux messages sont publiés, le déclencheur s'exécute.

Le contenu de ce déclencheur comporte une zone `messages`, qui est un tableau des messages publiés depuis la dernière exécution du déclencheur. Chaque objet de message figurant dans le tableau contient les zones suivantes :
- topic
- partition
- offset
- key
- value

En Kafka, les zones sont évidentes. Toutefois, `key` peut utiliser un paramètre `isBinaryKey` qui permet à `key` de transmettre des données binaires. Par ailleurs, `value` mérite une attention spéciale. Les zones `isJSONData` et `isBinaryValue` peuvent être utilisées pour le traitement des messages JSON et binaires. Les zones `isJSONData` et `isBinaryValue` ne peuvent pas être utilisées simultanément.

Par exemple, si le paramètre `isBinaryKey` avait pour valeur `true` lorsque le déclencheur a été créé, le paramètre `key` est encodé sous forme de chaîne en Base64 lorsqu'il est renvoyé à partir du contenu d'un déclencheur exécuté.

Si un élément `key` de `Some key` est publié avec la valeur `true` affectée au paramètre `isBinaryKey`, le contenu du déclencheur se présente comme suit :
```json
{
    "messages": [
    {
            "partition": 0,
            "key": "U29tZSBrZXk=",
            "offset": 421760,
            "topic": "mytopic",
            "value": "Some value"
        }
    ]
}
```
{: codeblock}

Si le paramètre `isJSONData` avait pour valeur `false` (ou n'était pas défini du tout) lors de la création du déclencheur, la zone `value` correspond à la valeur brute du message publié. Cependant, si `isJSONData` avait pour valeur `true` à la création du déclencheur, le système tente d'analyser cette valeur pour le mieux en tant qu'objet JSON. Si l'analyse aboutit, la zone `value` du contenu du déclencheur est l'objet JSON qui en résulte.

Si un message de type `{"title": "Some string", "amount": 5, "isAwesome": true}` est publié avec la valeur `true` affectée au paramètre `isJSONData`, le contenu du déclencheur peut se présenter comme suit :
```json
{
  "messages": [
    {
      "partition": 0,
        "key": null,
        "offset": 421760,
        "topic": "mytopic",
        "value": {
          "amount": 5,
            "isAwesome": true,
            "title": "Une chaîne"
        }
    }
  ]
}
```
{: codeblock}

Cependant, si le même contenu de message est publié avec la valeur `false` affectée au paramètre `isJSONData`, le contenu du déclencheur se présentera comme suit :
```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421761,
      "topic": "mytopic",
      "value": "{\"title\": \"Some string\", \"amount\": 5, \"isAwesome\": true}"
    }
  ]
}
```
{: codeblock}

A l'instar du paramètre `isJSONData`, si `isBinaryValue` avait pour valeur `true` lors de la création du déclencheur, l'élément `value` résultant dans le contenu du déclencheur est encodé sous forme de chaîne en Base64.

Si un élément `value` de `Some data` est transmis avec la valeur `true` affectée au paramètre `isBinaryValue`, le contenu du déclencheur se présente comme suit :
```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": "U29tZSBkYXRh"
    }
  ]
}
```
{: codeblock}

Si le même message est publié sans que la valeur `true` soit affectée au paramètre `isBinaryData`, le contenu du déclencheur se présente comme suit :
```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": "Some data"
    }
  ]
}
```
{: codeblock}

### Les messages sont traités par lots
Notez que le contenu du déclencheur comporte un tableau de messages. Si ces messages sont produits rapidement sur votre système de messagerie, le flux tente de générer un lot de messages publiés afin de les traiter en une seule exécution du déclencheur. Le traitement par lots permet de publier les messages dans le déclencheur de manière plus rapide et plus efficace.

Lorsque vous codez des actions exécutées par votre déclencheur, gardez à l'esprit que le nombre de messages du contenu est techniquement illimité, mais qu'il est toujours supérieur à 0. Voici un exemple de message par lots (notez la modification apportée à la valeur *offset*) :
```json
{
  "messages": [
    {
        "partition": 0,
         "key": null,
         "offset": 100,
         "topic": "mytopic",
         "value": {
            "amount": 5
         }
      },
      {
        "partition": 0,
         "key": null,
         "offset": 101,
         "topic": "mytopic",
         "value": {
            "amount": 1
         }
      },
      {
        "partition": 0,
         "key": null,
         "offset": 102,
         "topic": "mytopic",
         "value": {
            "amount": 999
         }
      }
  ]
}
```

## Exemples
{: #examples}

### Intégration d'OpenWhisk avec {{site.data.keyword.messagehub}}, Node Red, IBM Watson IoT, {{site.data.keyword.cos_full_notm}} et IBM Data Science Experience
Pour obtenir un exemple d'intégration d'OpenWhisk avec un service {{site.data.keyword.messagehub}}, Node Red, IBM Watson IoT, {{site.data.keyword.cos_full}}, IBM Data Science Experience (Spark), [cliquez ici](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0).

## Références
{: #message_references}
- [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org)
