---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: push notifications, functions, webhooks

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Package {{site.data.keyword.mobilepushshort}}

Le package {{site.data.keyword.mobilepushshort}} fournit un ensemble d'actions pour une interaction avec des instances de service {{site.data.keyword.mobilepushfull}}. Ces actions permettent d'envoyer un message ou de créer, mettre à jour, ou supprimer un webhook.
{: shortdesc}

Le package {{site.data.keyword.mobilepushshort}} inclut les actions suivantes :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/push-notifications` | package | apikey, appGuid | Utilisation d'une instance {{site.data.keyword.mobilepushshort}}. |
| `/push-notifications/send-message` | action | text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | Envoyer une notification push à un ou plusieurs périphériques spécifiés. |
| `/push-notifications/webhook` | action | events | Exécuter des événements déclencheurs sur des activités de périphérique (enregistrement, annulation d'enregistrement, abonnement ou annulation d'abonnement du périphérique) sur le service Push. |

## Création d'une instance de service {{site.data.keyword.mobilepushshort}}
{: #service_instance_push}

Avant d'installer le package, vous devez créer une instance {{site.data.keyword.mobilepushshort}}. 

1. [Créez une instance de service {{site.data.keyword.mobilepushshort}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a).

2. [Créez un ensemble de données d'identification de service ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1) pour l'instance de service Notifications push. 

3. [Configurez l'instance de service {{site.data.keyword.mobilepushshort}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2). 

## Installation du package {{site.data.keyword.mobilepushshort}}
{: #install_push}

Dès que vous disposez d'une instance de service {{site.data.keyword.mobilepushshort}}, utilisez l'interface de ligne de commande ou l'interface utilisateur {{site.data.keyword.openwhisk}} pour installer le package {{site.data.keyword.mobilepushshort}} dans votre espace de nom.


### Installation depuis l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #install_cli}

Avant de commencer :
  1. [Installez le plug-in {{site.data.keyword.openwhisk_short}} pour l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

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
{: #install_ui}

1. Sur la console {{site.data.keyword.openwhisk_short}}, accédez à [Create page ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/openwhisk/create).

2. A l'aide du commutateur d'espace de nom dans l'angle supérieur droit, sélectionnez l'espace de nom dans lequel vous voulez installer le package {{site.data.keyword.cos_short}}. 

3. Cliquez sur **Installer les packages**.

4. Cliquez sur le groupe de packages **IBM {{site.data.keyword.mobilepushshort}}**, puis cliquez sur le package **IBM {{site.data.keyword.mobilepushshort}}**. 

5. Dans la section Available Runtimes, sélectionnez NodeJS dans la liste déroulante, puis cliquez sur **Install**. 

6. Une fois le package installé, vous êtes redirigé vers la page Actions et vous pouvez rechercher votre nouveau package, nommé **push-notifications**.

7. Pour utiliser les actions du package **push-notifications**, vous devez lier les données d'identification du service aux actions.
  * Pour lier les données d'identification du service à toutes les actions du package, suivez les étapes 5 et 6 dans les instructions de l'interface CLI ci-dessus.
  * Pour lier les données d'identification du service à des actions individuelles, suivez les étapes ci-après dans l'interface utilisateur. **Remarque **: Vous devez suivre les étapes ci-après pour chaque action que vous voulez utiliser.
    1. Cliquez sur une action du package **push-notifications** que vous voulez utiliser. La page des informations détaillées de cette action s'affiche.
    2. Dans le volet de navigation gauche, cliquez sur la section **Parameters**.
    3. Entrez un nouveau **paramètre**. Pour le paramètre key, entrez `__bx_creds`. Comme valeur, collez l'objet JSON des données d'identification du service de l'instance de service que vous avez créée plus haut.

## Utilisation du package {{site.data.keyword.mobilepushshort}}
{: #usage_push}

### Envoi d'une notification push
{: #push_notif}

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

## Création d'un webhook
{: #webhook}

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
