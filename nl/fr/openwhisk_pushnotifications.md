---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-02"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Utilisation du package Push Notifications
{: #openwhisk_catalog_pushnotifications}

Le package `/whisk.system/pushnotifications` vous permet d'utiliser un service push.
{: shortdesc}

Le package inclut l'action et le flux suivants :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` | package | appId, appSecret  | utilisation du service push |
| `/whisk.system/pushnotifications/sendMessage` | action | text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | Envoi de notification push à un ou plusieurs périphérique(s) spécifié(s) |
| `/whisk.system/pushnotifications/webhook` | flux | events | Exécution d'événements déclencheur sur des activités de périphérique (enregistrement, annulation d'enregistrement, abonnement ou annulation d'abonnement de périphérique) sur le service Push |
Il est recommandé de créer une liaison de package avec les valeurs `appId` et `appSecret`. Ainsi, il n'est pas nécessaire de spécifier ces données d'identification à chaque fois que vous appelez les actions du package.

## Création d'une liaison de package Push
{: #openwhisk_catalog_pushnotifications_create}

Pour créer une liaison de package Notifications push, vous devez spécifier les paramètres suivants :

-  `appId` : Identificateur global unique d'application {{site.data.keyword.Bluemix}}. 
-  `appSecret` : Valeur confidentielle d'application du service de notification push {{site.data.keyword.Bluemix_notm}}. 

Pour créer une liaison de package, reportez-vous à l'exemple suivant :

1. Créez une application {{site.data.keyword.Bluemix_notm}} dans le tableau de bord [{{site.data.keyword.Bluemix_notm}}](http://console.bluemix.net).

2. Initialisez le service de notification push et liez celui-ci à l'application {{site.data.keyword.Bluemix_notm}}. 

3. Configurez l'[application de notification push](https://console.bluemix.net/docs/services/mobilepush/index.html).

  Prenez soin de mémoriser l'`identificateur global unique d'application` et la `valeur confidentielle d'application` de l'application  {{site.data.keyword.Bluemix_notm}} que vous avez créée.

4. Créez une liaison de package avec `/whisk.system/pushnotifications`.

  ```
  wsk package bind /whisk.system/pushnotifications myPush -p appId myAppID -p appSecret myAppSecret
  ```
  {: pre}
  
5. Vérifiez que la liaison de package existe.

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myNamespace/myPush private binding
  ```


## Paramètres de notification push
{: #openwhisk_push_parameters}

L'action `/whisk.system/pushnotifications/sendMessage` envoie des notifications push à des périphériques enregistrés. Les paramètres sont les suivants :

- `text` : message de notification à présenter à l'utilisateur. Par exemple, `-p text "Hi, OpenWhisk send a notification"`.
- `url` : URL qui peut être envoyée en même temps que l'alerte. Par exemple, `-p url "https:\\www.w3.ibm.com"`.
- `deviceIds` : liste des périphériques spécifiés. Par exemple, `-p deviceIds ["deviceID1"]`.
- `platforms` : envoyez une notification aux périphériques des plateformes spécifiées. 'A' pour les périphériques Apple (iOS) et 'G' pour les périphériques Google (Android). Par exemple, `-p platforms ["A"]`.
- `userIds` : envoyez une notification aux périphériques des utilisateurs spécifiés. Par exemple, `-p userIds "[\"testUser\"]"`
- `tagNames` : envoyez une notification aux périphériques abonnés à l'une des étiquettes spécifiées. Par exemple, `-p tagNames "[\"tag1\"]"`.
- `gcmCollapseKey` : ce paramètre identifie un groupe de messages
- `gcmCategory` : identificateur de catégorie à utiliser pour les notifications push interactives.
- `gcmIcon` : spécifiez le nom de l'ibmcloudône à afficher pour la notification. Assurez-vous que l'ibmcloudône est déjà conditionnée avec l'application client.
- `gcmDelayWhileIdle` : lorsque ce paramètre a pour valeur true, le message n'est pas envoyé tant que le périphérique n'est pas actif.
- `gcmSync` : la messagerie de groupe de périphériques permet à chaque instance d'application d'un groupe de refléter le dernier état de messagerie.
- `gcmVisibility` : privé/public - visibilité de cette notification, qui affecte comment et quand les notifications sont révélées sur un écran verrouillé.
- `gcmPayload` : contenu JSON personnalisé qui est envoyé dans le cadre du message de notification. Par exemple, `-p gcmPayload "{\"hi\":\"hello\"}"`
- `gcmPriority` : définit la priorité du message.
- `gcmSound` : fichier son (sur le périphérique) qui est utilisé lorsque la notification arrive sur le périphérique.
- `gcmTimeToLive` : ce paramètre spécifie le nombre de secondes pendant lesquelles le message est conservé dans le stockage GCM si le périphérique est hors ligne.
- `gcmStyleType` : spécifie les types de notification pouvant être développés. Les valeurs possibles sont `bigtext_notification`, `picture_notification`, `inbox_notification`.
- `gcmStyleTitle` : spécifie le titre de la notification. Le titre s'affiche lorsque la notification est développée. Un titre doit être spécifié pour les trois notifications pouvant être développées.
- `gcmStyleUrl` : URL à partir de laquelle l'image doit être obtenue pour la notification. Elle doit être spécifiée pour `picture_notification`.
- `gcmStyleText` : grand texte qui doit être affiché sur une notification `bigtext_notification` pouvant être développée. Il doit être spécifié pour `bigtext_notification`.
- `gcmStyleLines` : tableau de chaînes qui doit être affiché dans le style boîte de réception pour `inbox_notification`. Il doit être spécifié pour `inbox_notification`.
- `gcmLightsLedArgb` : couleur du voyant. Le matériel utilise sa meilleure approximation.
- `gcmLightsLedOnMs` : durée, exprimée en millisecondes, pendant laquelle le voyant reste allumé lorsqu'il clignote. Le matériel utilise sa meilleure approximation.
- `gcmLightsLedOffMs` : durée, exprimée en millisecondes, pendant laquelle le voyant reste éteint lorsqu'il clignote. Le matériel utilise sa meilleure approximation.
- `apnsBadge` : numéro à afficher en tant que badge de l'ibmcloudône d'application.
- `apnsCategory` : identificateur de catégorie à utiliser pour les notifications push interactives.
- `apnsIosActionKey` : titre de la clé d'action.
- `apnsPayload` : contenu JSON personnalisé qui est envoyé dans le cadre du message de notification. 
- `apnsType` : ['DEFAULT', 'MIXED', 'SILENT'].
- `apnsSound` : nom du fichier son dans l'ensemble d'applications. Le son de ce fichier est utilisé pour une alerte.
- `apnsTitleLocKey` : clé d'une chaîne de titre dans le fichier `Localizable.strings` pour la localisation en cours. La chaîne de clé peut être formatée avec les spécificateurs %@ et %n$@ pour inclure les variables qui sont spécifiées dans le tableau `titleLocArgs`.
- `apnsLocKey` : clé d'une chaîne de message d'alerte dans un fichier `Localizable.strings` pour la localisation en cours (qui est définie par la préférence de langue de l'utilisateur). La chaîne de clé peut être formatée avec les spécificateurs %@ et %n$@ pour inclure les variables qui sont spécifiées dans le tableau locArgs.
- `apnsLaunchImage` : nom d'un fichier image dans le bundle d'application, avec ou sans extension de nom de fichier. L'image est utilisée comme image de lancement lorsque les utilisateurs appuient sur le bouton d'action ou déplacement le curseur d'action.
- `pnsTitleLocArgs` : valeurs de chaîne variables devant apparaître à la place des spécificateurs de format dans `title-loc-key`.
- `apnsLocArgs` : valeurs de chaîne variables devant apparaître à la place des spécificateurs de format dans `locKey`.
- `apnstitle` : titre des notifications push riches (pris en charge sous iOS 10 et versions ultérieures uniquement).
- `apnsSubtitle` : sous-titre des notifications push riches (pris en charge sous iOS 10 et versions ultérieures uniquement).
- `apnsAttachmentUrl` : lien vers le support des notifications iOS (vidéo, audio, GIF, images - pris en charge sous iOS 10 et versions ultérieures uniquement).
- `fireFoxTitle` : spécifie le titre qui doit être défini pour WebPush Notification.
- `fireFoxIconUrl` : URL de l'ibmcloudône qui doit être définie pour WebPush Notification.
- `fireFoxTimeToLive` : ce paramètre spécifie la durée de conservation (exprimée en secondes) du message dans le stockage GCM si le périphérique est hors ligne.
- `fireFoxPayload` : contenu JSON personnalisé qui est envoyé dans le cadre du message de notification. 
- `chromeTitle` : spécifie le titre qui doit être défini pour WebPush Notification.
- `chromeIconUrl` : URL de l'ibmcloudône qui doit être définie pour WebPush Notification.
- `chromeTimeToLive` : ce paramètre spécifie la durée de conservation (exprimée en secondes) du message dans le stockage GCM si le périphérique est hors ligne.
- `chromePayload` : contenu JSON personnalisé qui est envoyé dans le cadre du message de notification. 
- `safariTitle` : spécifie le titre à définir pour les notifications push Safari.
- `safariUrlArgs` : arguments d'URL qui doivent être utilisés avec cette notification. Ils doivent être fournis sous la forme d'un tableau JSON.
- `safariAction` : libellé du bouton d'action.
- `chromeAppExtTitle` : spécifie le titre qui doit être défini pour WebPush Notification.
- `chromeAppExtCollapseKey` : ce paramètre identifie un groupe de messages.
- `chromeAppExtDelayWhileIdle` : lorsque ce paramètre a pour valeur true, il indique que le message n'est pas envoyé tant que le périphérique n'est pas actif.
- `chromeAppExtIconUrl` : URL de l'ibmcloudône qui doit être définie pour WebPush Notification.
- `chromeAppExtTimeToLive` : ce paramètre spécifie la durée de conservation (exprimée en secondes) du message dans le stockage GCM si le périphérique est hors ligne.
- `chromeAppExtPayload` : contenu JSON personnalisé qui est envoyé dans le cadre du message de notification. 


## Envoi de notifications push
{: #openwhisk_send_push_notifications}

Voici un exemple qui illustre l'envoi d'une notification push à partir du package de notification push :

Envoyez une notification push en utilisant l'action `sendMessage` dans la liaison de package que vous avez créée précédemment. Prenez soin de remplacer `/myNamespace/myPush` par votre nom de package. 
```
wsk action invoke /myNamespace/myPush/sendMessage --blocking --result  -p url https://example.com -p text "this is my message"  -p sound soundFileName -p deviceIds "[\"T1\",\"T2\"]"
```
{: pre}

```json
{
  "result": {
  "pushResponse": 
      {
      "messageId":"11111H",
        "message":{
        "alert":"this is my message",
          "url":""
      },
        "settings":{
        "apns":{
          "sound":"default"
        },
          "gcm":{
          "sound":"default"
          },
          "target":{
          "deviceIds":["T1","T2"]
          }
      }
    }
  },
      "status": "success",
      "success": true
  }
```


## Exécution d'un événement déclencheur sur une activité de service Push Notifications
{: #openwhisk_catalog_pushnotifications_fire}

`/whisk.system/pushnotifications/webhook` configure le service Push pour qu'il exécute un déclencheur lorsqu'il existe une activité de périphérique, telle qu'un enregistrement ou une annulation d'enregistrement ou un abonnement ou une annulation d'abonnement de périphérique dans une application spécifiée.

Les paramètres sont les suivants :

- `appId :` identificateur global unique d'application {{site.data.keyword.Bluemix_notm}}. 
- `appSecret :` valeur confidentielle d'application du service de notification push {{site.data.keyword.Bluemix_notm}}. 
- `events :` les événements pris en charge sont `onDeviceRegister`, `onDeviceUnregister`, `onDeviceUpdate`, `onSubscribe` et `onUnsubscribe`. Vous pouvez utiliser le caractère générique `*` pour être averti de tous les événements. 

Pour créer un déclencheur qui s'exécute chaque fois qu'un nouveau périphérique est enregistré auprès de l'application de service Push Notifications, voir l'exemple suivant :

1. Créez une liaison de package qui est configurée pour votre service Push Notifications avec vos paramètres appId et appSecret.
  ```
  wsk package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. Créez un déclencheur pour le type d'événement `onDeviceRegister` du service Push Notifications en utilisant votre flux `myPush/webhook`.
  ```
  wsk trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. Vous pouvez créer une règle qui envoie un message à chaque fois qu'un nouveau périphérique est enregistré. Créez une règle à l'aide de l'action et du déclencheur précédents.
  ```
  wsk rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

  Vérifiez les résultats dans `wsk activation poll`.

  Enregistrez un périphérique dans votre application {{site.data.keyword.Bluemix_notm}}. Les éléments `Rule`, `Trigger` et `Action` qui sont exécutés sont visibles dans le [tableau de bord](https://console.{Domain}/openwhisk/dashboard) OpenWhisk.

  L'action envoie une notification push.

