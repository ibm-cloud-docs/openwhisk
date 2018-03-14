---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Utilisation du package Message Hub
{: #openwhisk_catalog_message_hub}

Package qui permet de comminiquer avec des instances [Message Hub](https://developer.ibm.com/messaging/message-hub) pour publier et consommer des messages via l'API Kafka hautes performances native.
{: shortdesc}

## Création d'un déclencheur qui écoute une instance IBM MessageHub
{: #openwhisk_catalog_message_hub_trigger}

Pour créer un déclencheur qui réagit lorsque des messages sont publiés dans une instance Message Hub, vous devez utiliser le flux nommé `/messaging/messageHubFeed`. Cette action de flux prend en charge les paramètres suivants :

|Nom|Type|Description|
|---|---|---|
|kafka_brokers_sasl|Tableau JSON de chaînes|Ce paramètre est un tableau de chaînes `<host>:<port>` composé des courtiers de votre instance Message Hub. |
|utilisateur|Chaîne|Votre nom d'utilisateur Message Hub|
|password|Chaîne|Votre mot de passe Message Hub|
|topic|Chaîne|Rubrique que le déclencheur doit écouter|
|kafka_admin_url|Chaîne d'URL|URL de l'interface REST d'administration de Message Hub|
|isJSONData|Booléen (facultatif - par défaut=false)|Lorsque ce paramètre a pour valeur `true`, le fournisseur tente de faire une analyse syntaxique de la valeur de message en tant que JSON avant de la transmettre en tant que contenu du déclencheur.|
|isBinaryKey|Booléen (facultatif - par défaut=false)|Lorsque ce paramètre a pour valeur `true`, le fournisseur encode la valeur de la clé en Base64 avant de la transmettre en tant que contenu du déclencheur.|
|isBinaryValue|Booléen (facultatif - par défaut=false)|Lorsque ce paramètre a pour valeur `true`, le fournisseur encode la valeur du message en Base64 avant de la transmettre en tant que contenu du déclencheur.|

Cette liste de paramètres peut vous sembler impressionnante, mais vous pouvez les définir automatiquement à l'aide de la commande d'interface de ligne de commande package refresh :

1. Créez une instance du service Message Hub sous l'organisation et l'espace en cours que vous utilisez pour OpenWhisk.

2. Vérifiez que la rubrique que vous souhaitez écouter existe dans Message Hub ou créez une rubrique, par exemple, `mytopic`.

3. Actualisez les packages dans votre espace de nom. L'actualisation crée automatiquement une liaison de package pour l'instance de service Message Hub que vous avez créée.

  ```
  wsk package refresh
  ```
  {: pre}
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```

  Votre liaison de package contient désormais les données d'identification associées à votre instance Message Hub.

4. A présent, il ne vous reste plus qu'à créer un déclencheur qui s'exécute lorsque de nouveaux messages sont publiés sur votre rubrique Message Hub.

  ```
  wsk trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## Configuration d'un package Message Hub en dehors de {{site.data.keyword.Bluemix_notm}}

Si vous voulez configurer votre package Message Hub en dehors de  {{site.data.keyword.Bluemix_notm}}, vous devez créer manuellement une liaison de package pour le service Message Hub. Pour ce faire, vous avez besoin des données d'identification et des informations de connexion du service Message Hub.

1. Créez une liaison de package configurée pour votre service Message Hub.

  ```
  wsk package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your Message Hub user> -p password <your Message Hub password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. A présent, vous pouvez créer un déclencheur à l'aide de votre nouveau package qui sera exécuté lorsque de nouveaux messages seront publiés sur votre rubrique Message Hub.

  ```
  wsk trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}


## Ecoute de messages
{: #openwhisk_catalog_message_hub_listen}

Une fois qu'un déclencheur est créé, le système surveille la rubrique indiquée dans votre service de messagerie. Lorsque de nouveaux messages sont publiés, le déclencheur s'exécute. 

Le contenu de ce déclencheur comporte une zone `messages`, qui est un tableau des messages publiés depuis la dernière exécution du déclencheur. Chaque objet de message figurant dans le tableau contient les zones suivantes :
- topic
- partition
- offset
- key
- value

En Kafka, les zones sont évidentes. Toutefois, `key` peut utiliser un paramètre `isBinaryKey` qui permet à `key` de transmettre des données binaires. Par ailleurs, `value` mérite une attention spéciale. Les zones `isJSONData` et `isBinaryValue` peuvent être utilisées pour le traitement des messages JSON et binaires. Les zones `isJSONData` et `isBinaryValue` ne peuvent pas être utilisées simultanément.

Par exemple, si `isBinaryKey` avait pour valeur `true` lors de la création du déclencheur, `key` est encodé sous forme de chaîne en Base64 lorsqu'il est renvoyé à partir du contenu d'un déclencheur exécuté.

Si un élément `key` de `Some key` est transmis avec la valeur `true` affectée au paramètre `isBinaryKey`, le contenu du déclencheur se présente comme suit :

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

Si le paramètre `isJSONData` avait pour valeur `false` (ou n'était pas défini du tout) lors de la création du déclencheur, la zone `value` correspond à la valeur brute du message publié. Cependant, si `isJSONData` avait pour valeur `true` à la création du déclencheur, le système tente d'analyser cette valeur pour le mieux en tant qu'objet JSON. Si l'analyse aboutit, la zone `value` du contenu du déclencheur est l'objet JSON qui en résulte.

Si un message `{"title": "Une chaîne", "amount": 5, "isAwesome": true}` est publié avec la valeur `true` affectée au paramètre `isJSONData`, le contenu du déclencheur peut se présenter comme suit :

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

Cependant, si le même contenu de message est publié avec la valeur `false` affectée au paramètre `isJSONData`, le contenu du déclencheur se présente comme suit : 

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

A l'instar d'`isJSONData`, si `isBinaryValue` avait pour valeur `true` lors de la création du déclencheur, l'élément `value` résultant dans le contenu du déclencheur est encodé sous forme de chaîne en Base64.

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

### Les messages sont traités par lots
Notez que le contenu du déclencheur comporte un tableau de messages. Si ces messages sont produits rapidement sur votre système de messagerie, le flux tente de générer un lot à partir des messages publiés afin de les traiter en une seule exécution du déclencheur. Le traitement par lots permet de publier les messages dans le déclencheur de manière plus rapide et plus efficace.

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

## Production de messages vers Message Hub
Si vous désirez utiliser une action OpenWhisk pour produire facilement un message vers Message Hub, vous pouvez utiliser l'action `/messaging/messageHubProduce`. Cette action utilise les paramètres suivants :

|Nom|Type|Description|
|---|---|---|
|kafka_brokers_sasl|Tableau JSON de chaînes|Ce paramètre est un tableau de chaînes `<host>:<port>` composé des courtiers de votre instance Message Hub. |
|utilisateur|Chaîne|Votre nom d'utilisateur Message Hub|
|password|Chaîne|Votre mot de passe Message Hub|
|topic|Chaîne|Rubrique que le déclencheur doit écouter|
|value|Chaîne|Valeur pour le message que vous désirez générer|
|key|Chaîne (facultatif)|Clé du message que vous désirez générer|

Tandis que les trois premiers paramètres peuvent être liés automatiquement à l'aide de `wsk package refresh`, ci-après figure un exemple d'appel de l'action avec tous les paramètres requis :

```
wsk action invoke /messaging/messageHubProduce -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p topic mytopic -p user <your Message Hub user> -p password <your Message Hub password> -p value "This is the content of my message"
```
{: pre}

## Exemples

### Intégration d'OpenWhisk avec IBM Message Hub, Node Red, IBM Watson IoT, IBM Object Storage et IBM Data Science Experience
[Cliquez ici](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0) pour consulter un exemple d'intégration OpenWhisk avec IBM Message Hub, Node Red, IBM Watson IoT, IBM Object Storage et le service IBM Data Science Experience (Spark).

## Références
- [IBM Message Hub](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
