---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: push notifications, functions, webhooks

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

# Push Notifications
{: #pkg_push_notifications}


## Packages
{: #pkg_push_packages}

| Package | Disponibilité | Description |
| --- | --- | --- |
| [`/whisk.system/pushnotifications`](#pkg_push_send) | Préinstallé (non disponible à Tokyo) | Envoyer une notification push à un ou plusieurs périphériques spécifiés. |
| [`/whisk.system/pushnotifications/webhook`](#pkg_push_mobile) | Préinstallé (non disponible à Tokyo) | Envoyer une notification mobile push sur des événements de périphérique. |
| [`/push-notifications`](#pkg_push_mobile_send) | Installable | Interagir avec des instances de service {{site.data.keyword.mobilepushfull}}. Envoyer un message ou bien créer, mettre à jour ou supprimer un webhook. |

## Envoi de notifications push
{: #pkg_push_send}

Le package préinstallé n'est pas disponible dans la région Tokyo. Voir le package installable [Push Notifications](#pkg_push_mobile_send) pour obtenir l'action `sendMessage` utilisant l'authentification IAM
{: tip}

Découvrez comment créer une liaison de package Push Notifications et envoyer une notification push simple à l'aide du package `/whisk.system/pushnotifications`.
{: shortdesc}

Ce package comprend les actions et les flux suivants :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` | package | appId, appSecret, admin_url | Utiliser le service Push. |
| `/whisk.system/pushnotifications/sendMessage` | action | text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | Envoyer une notification push à un ou plusieurs périphériques spécifiés. |


Pour plus d'informations sur l'exécution d'événements déclencheurs lorsque le périphérique est actif, voir [Mobile Push sur des événements de périphérique](#pkg_push_mobile).

### Création d'une liaison de package Push
{: #pkg_push_create}

Pour créer une liaison de package Push Notifications, vous devez spécifier les paramètres suivants :

-  **appId** : **identificateur global unique de l'application** {{site.data.keyword.Bluemix}}.
-  **appSecret** : **valeur confidentielle de l'application** du service {{site.data.keyword.Bluemix_notm}} Push Notification.

Pour créer une liaison de package, procédez comme suit :

1. Créez une application {{site.data.keyword.Bluemix_notm}} dans le [tableau de bord {{site.data.keyword.Bluemix_notm}}](http://cloud.ibm.com).

2. Initialisez le service de notification push et liez ce service à l'application {{site.data.keyword.Bluemix_notm}}.

3. Configurez l'[application de notification push](/docs/services/mobilepush?topic=mobile-pushnotification-gettingstartedtemplate).

  Prenez soin de mémoriser l'**identificateur global unique de l'application** et la **valeur confidentielle de l'application** correspondant à l'application {{site.data.keyword.Bluemix_notm}} que vous avez créée.

4. Créez une liaison de package avec `/whisk.system/pushnotifications`.
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myPush -p appId myAppID -p appSecret myAppSecret
  ```
  {: pre}

5. Vérifiez que la liaison de package existe :
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Exemple de sortie :
  ```
  packages
  /myNamespace/myPush private binding
  ```
  {: screen}

### Paramètres de notification push
{: #pkg_push_params}

L'action `/whisk.system/pushnotifications/sendMessage` envoie des notifications push à des périphériques enregistrés. Les paramètres sont les suivants :
- `text` : message de notification à présenter à l'utilisateur. Par exemple, `-p text "Hi, OpenWhisk send a notification"`.
- `url` : URL qui peut être envoyée en même temps que l'alerte. Par exemple, `-p url "https:\\www.w3.ibm.com"`.
- `apiHost` : chaîne facultative qui spécifie l'hôte d'API. La valeur par défaut est `mobile.ng.bluemix.net`.  Par exemple : `-p apiHost "mobile.eu-gb.bluemix.net"`
- `deviceIds` : liste des périphériques spécifiés. Par exemple, `-p deviceIds ["deviceID1"]`.
- `platforms` : envoyez une notification aux périphériques des plateformes spécifiées. 'A' pour les périphériques Apple (iOS) et 'G' pour les périphériques Google (Android). Par exemple, `-p platforms ["A"]`.
- `userIds` : envoyez une notification aux périphériques des utilisateurs spécifiés. Par exemple, `-p userIds "[\"testUser\"]"`
- `tagNames` : envoyez une notification aux périphériques abonnés à l'une des étiquettes spécifiées. Par exemple, `-p tagNames "[\"tag1\"]"`.
- `gcmCollapseKey` : ce paramètre identifie un groupe de messages
- `gcmCategory` : identificateur de catégorie à utiliser pour les notifications push interactives.
- `gcmIcon` : spécifiez le nom de l'icône à afficher pour la notification. Assurez-vous que l'icône est déjà incluse dans le package de l'application client.
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
- `apnsBadge` : numéro à afficher en tant que badge de l'icône d'application.
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
- `fireFoxIconUrl` : URL de l'icône qui doit être définie pour WebPush Notification.
- `fireFoxTimeToLive` : ce paramètre spécifie la durée de conservation (exprimée en secondes) du message dans le stockage GCM si le périphérique est hors ligne.
- `fireFoxPayload` : contenu JSON personnalisé qui est envoyé dans le cadre du message de notification.
- `chromeTitle` : spécifie le titre qui doit être défini pour WebPush Notification.
- `chromeIconUrl` : URL de l'icône qui doit être définie pour WebPush Notification.
- `chromeTimeToLive` : ce paramètre spécifie la durée de conservation (exprimée en secondes) du message dans le stockage GCM si le périphérique est hors ligne.
- `chromePayload` : contenu JSON personnalisé qui est envoyé dans le cadre du message de notification.
- `safariTitle` : spécifie le titre à définir pour les notifications push Safari.
- `safariUrlArgs` : arguments d'URL qui doivent être utilisés avec cette notification. Ils doivent être fournis sous la forme d'un tableau JSON.
- `safariAction` : libellé du bouton d'action.
- `chromeAppExtTitle` : spécifie le titre qui doit être défini pour WebPush Notification.
- `chromeAppExtCollapseKey` : ce paramètre identifie un groupe de messages.
- `chromeAppExtDelayWhileIdle` : lorsque ce paramètre a pour valeur true, il indique que le message n'est pas envoyé tant que le périphérique n'est pas actif.
- `chromeAppExtIconUrl` : URL de l'icône qui doit être définie pour WebPush Notification.
- `chromeAppExtTimeToLive` : ce paramètre spécifie la durée de conservation (exprimée en secondes) du message dans le stockage GCM si le périphérique est hors ligne.
- `chromeAppExtPayload` : contenu JSON personnalisé qui est envoyé dans le cadre du message de notification.

### Exemple de notification push
{: #pkg_push_ex}

Voici un exemple d'envoi de notification push à partir du package Push Notification :

Envoyez une notification push à l'aide de l'action **sendMessage** dans la liaison de package que vous avez créée précédemment. Prenez soin de remplacer `/myNamespace/myPush` par votre nom de package.
```
ibmcloud fn action invoke /myNamespace/myPush/sendMessage --blocking --result -p url https://example.com -p text "this is my message" -p sound soundFileName -p deviceIds "[\"T1\",\"T2\"]"
```
{: pre}

Exemple de sortie :
```
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
{: screen}

## Envoi de notifications push sur des événements de périphérique mobile
{: #pkg_push_mobile}

Ce package préinstallé n'est pas disponible dans la région Tokyo.
{: tip}

Découvrez comment configurer le service Push Notifications pour qu'il exécute un déclencheur pour toute activité de périphérique, (par exemple, enregistrement/annulation d'enregistrement ou abonnement/annulation d'abonnement du périphérique) dans l'application spécifiée.
{: shortdesc}

### Paramètres
{: #pkg_push_mobile_params}

Les paramètres du package `/whisk.system/pushnotifications/webhook` sont les suivants :
- `appId` : identificateur global unique de l'application {{site.data.keyword.Bluemix_notm}}.
- `appSecret` : valeur confidentielle de l'application du service {{site.data.keyword.Bluemix_notm}} Push Notification. 
- `events` : `onDeviceRegister`, `onDeviceUnregister`, `onDeviceUpdate`, `onSubscribe`, `onUnsubscribe`

  Vous pouvez utiliser le caractère générique "`*`" pour être averti de tous les événements.

### Exécution d'un événement déclencheur sur une activité de service Push Notifications
{: #pkg_push_mobile_trigger}

Pour créer un déclencheur qui s'exécute chaque fois qu'un nouveau périphérique est enregistré auprès de l'application de service Push Notifications, voir l'exemple suivant :

1. Créez une liaison de package qui est configurée pour votre service Push Notifications avec vos paramètres `appId` et `appSecret`.
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. Créez un déclencheur pour le type d'événement `onDeviceRegister` du service Push Notifications en utilisant votre flux `myPush/webhook`.
  ```
  ibmcloud fn trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. Vous pouvez créer une règle qui envoie un message à chaque fois qu'un nouveau périphérique est enregistré. Créez une règle à l'aide de l'action et du déclencheur précédents.
  ```
  ibmcloud fn rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

4. Vérifiez les résultats en exécutant la commande `ibmcloud fn activation poll`.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

5. Enregistrez un périphérique dans votre application {{site.data.keyword.Bluemix_notm}}. Vous pouvez voir que la règle (`rule`), le déclencheur (`trigger`) et l'action (`action`) sont exécutés dans le [tableau de bord](https://cloud.ibm.com/openwhisk/dashboard) {{site.data.keyword.openwhisk}}.

  L'action envoie une notification push.


## Envoi de notifications push ou création, mise à jour et suppression de webhook
{: #pkg_push_mobile_send}

Le package installable {{site.data.keyword.mobilepushshort}} fournit un ensemble d'actions pour une interaction avec des instances de service {{site.data.keyword.mobilepushfull}}. Ces actions permettent d'envoyer un message ou de créer, mettre à jour ou supprimer un webhook.
{: shortdesc}

Le package {{site.data.keyword.mobilepushshort}} inclut les actions suivantes :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/push-notifications` | package | apikey, appGuid | Utilisation d'une instance {{site.data.keyword.mobilepushshort}}. |
| `/push-notifications/send-message` | action | text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | Envoyer une notification push à un ou plusieurs périphériques spécifiés. |
| `/push-notifications/webhook` | action | events | Exécuter des événements déclencheurs sur des activités de périphérique (enregistrement, annulation d'enregistrement, abonnement ou annulation d'abonnement du périphérique) sur le service Push. |

### Création d'une instance de service {{site.data.keyword.mobilepushshort}}
{: #service_instance_push}

Avant d'installer le package, vous devez créer une instance {{site.data.keyword.mobilepushshort}}.

1. [Créez une instance de service {{site.data.keyword.mobilepushshort}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a).

2. [Créez un ensemble de données d'identification de service ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1) pour l'instance du service Push Notifications. 

3. [Configurez l'instance de service {{site.data.keyword.mobilepushshort}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2).

### Installation du package {{site.data.keyword.mobilepushshort}}
{: #pkg_push_mobile_install}

Dès que vous disposez d'une instance de service {{site.data.keyword.mobilepushshort}}, utilisez l'interface de ligne de commande ou l'interface utilisateur {{site.data.keyword.openwhisk}} pour installer le package {{site.data.keyword.mobilepushshort}} dans votre espace de nom.

### Installation depuis l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #pkg_push_mobile_cli}

Avant de commencer :
  1. [Installez le plug-in {{site.data.keyword.openwhisk_short}} pour l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Pour installer le package {{site.data.keyword.mobilepushshort}} :

1. Clonez le référentiel de package {{site.data.keyword.mobilepushshort}}.
    ```
    git clone https://github.com/ibm-functions/package-push-notifications.git
    ```
    {: pre}

2. Accédez au répertoire `runtimes/nodejs`.
    ```
    cd package-push-notifications/runtimes/nodejs
    ```
    {: pre}

3. Déployez le package.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Vérifiez que le package `push-notifications` est ajouté à votre liste de packages.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Sortie :
    ```
    packages
    /myOrg_mySpace/push-notifications private
    ```
    {: screen}

5. Liez les données d'identification de l'instance de service {{site.data.keyword.mobilepushshort}} que vous avez créée au package.
    ```
    ibmcloud fn service bind imfpush push-notifications
    ```
    {: pre}

    Exemple de sortie :
    ```
    Credentials 'Credentials-1' from 'imfpush' service instance 'Push-Notifications-r1' bound to 'push-notifications'.
    ```
    {: screen}

6. Vérifiez que le package est configuré avec les données d'identification de votre instance de service {{site.data.keyword.mobilepushshort}}.
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/push-notifications parameters
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/push-notifications, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "imfpush": {
            "admin_url": "https://mobile.ng.bluemix.net/imfpushdashboard/?appGuid=12345a-a123-1234-ab12-1ba1234567",
            "apikey": "abcd1234abcd1234abcd1234",
            "appGuid": "12341-12345-1234-a1234-1abcd12345",
            "clientSecret": "1b1234ab-1234-1234-123a-ab12345abcd",
            "credentials": "Service credentials-1",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:imfpush:us-south:a/abcd1234abcd1234:abcd1234-abcd-1234-abcd1234::",
            "iam_apikey_name": "auto-generated-apikey-abcd1234abcd1234abcd1234",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/1234abcd1234abcd::serviceid:ServiceId-12345678-1234-12ab-abc1-1234abcd1234abcd",
            "instance": "Push Notifications-ab",
            "plan": "LITE",
            "url": "https://imfpush.ng.bluemix.net/imfpush/v1/apps/1234abcd-1234-abcd-1234"
          }
        }
      }
    ]
    ```
    {: screen}

### Installation depuis l'interface utilisateur {{site.data.keyword.openwhisk_short}}
{: #pkg_push_mobile_ui}

1. Sur la console {{site.data.keyword.openwhisk_short}}, accédez à la [page Créer ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/openwhisk/create).

2. A l'aide du commutateur d'espace de nom dans l'angle supérieur droit, sélectionnez l'espace de nom dans lequel vous voulez installer le package {{site.data.keyword.cos_full_notm}}.

3. Cliquez sur **Installer les packages**.

4. Cliquez sur le groupe de packages **IBM {{site.data.keyword.mobilepushshort}}**, puis cliquez sur le package **IBM {{site.data.keyword.mobilepushshort}}**.

5. Dans la section Contextes d'exécution disponibles, sélectionnez NodeJS dans la liste déroulante, puis cliquez sur **Installer**.

6. Une fois le package installé, vous êtes redirigé vers la page Actions et vous pouvez rechercher votre nouveau package, nommé **push-notifications**.

7. Pour utiliser les actions du package **push-notifications**, vous devez lier les données d'identification du service aux actions.
  * Pour lier les données d'identification du service à toutes les actions du package, suivez les étapes 5 et 6 dans les instructions de l'interface CLI ci-dessus.
  * Pour lier les données d'identification du service à des actions individuelles, suivez les étapes ci-après dans l'interface utilisateur. **Remarque **: Vous devez suivre les étapes ci-après pour chaque action que vous voulez utiliser.
    1. Cliquez sur une action du package **push-notifications** que vous voulez utiliser. La page des informations détaillées de cette action s'affiche.
    2. Dans le volet de navigation gauche, cliquez sur la section **Paramètres**.
    3. Entrez un nouveau **paramètre**. Pour le paramètre key, entrez `__bx_creds`. Comme valeur, collez l'objet JSON des données d'identification du service de l'instance de service que vous avez créée plus haut.

### Envoi d'une notification push
{: #pkg_push_mobile_sendmsg}

Pour envoyer un message via le service de notification push à l'aide de l'action `send-message` :
```
ibmcloud fn action invoke push-notifications/send-message --blocking --result --param messageText "Let's code something" --param messageUrl "http://developer.ibm.com"
```
{: pre}

Exemple de sortie :
```
{
  "response": {
      "result": {
          "message": {
              "alert": "let's code something",
              "url": "http://developer.ibm.com"
          },
          "messageId": "fLyql2tx"
      },
      "status": "success",
      "success": true
  },
}
```
{: screen}

### Création d'un webhook
{: #pkg_push_mobile_hook}

Pour créer un webhook pour le service {{site.data.keyword.mobilepushshort}} pour des événements onDeviceRegister :

```
 ibmcloud fn action invoke push-notifications/webhook --blocking --param triggerName "/myPackage/myTrigger" --param events onDeviceRegister
```
{: pre}

Exemple de sortie :
```
{
  "response": {
    "result": {
      "error": {}
    },
  "status": "application error",
  "success": false
  },
}
```
{: screen}

