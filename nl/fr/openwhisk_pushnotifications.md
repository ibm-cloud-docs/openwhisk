---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Mobile Push sur des événements de périphérique
{: #openwhisk_pushnotifications}

Découvrez comment configurer le service Push pour qu'il exécute un déclencheur pour toute activité de périphérique, (par exemple, enregistrement/annulation d'enregistrement ou abonnement/annulation d'abonnement du périphérique) dans l'application spécifiée.
{: shortdesc}

Pour plus d'informations sur le package `/whisk.system/pushnotifications` proprement dit, voir la rubrique [Mobile Push](./mobile_push_actions.html) qui traite de la création d'une liaison de package Push et de l'envoi d'une notification push.

## Paramètres Push
{: #push_parameters}

Les paramètres du package `/whisk.system/pushnotifications/webhook` sont les suivants :
- **appId :** identificateur global unique de l'application {{site.data.keyword.Bluemix_notm}}.
- **appSecret :** valeur confidentielle de l'application du service de notification push {{site.data.keyword.Bluemix_notm}}.
- **events :** _onDeviceRegister_, _onDeviceUnregister_, _onDeviceUpdate_, _onSubscribe_, _onUnsubscribe_

  Vous pouvez utiliser le caractère générique "`*`" pour être averti de tous les événements.

## Exécution d'un événement déclencheur sur une activité de service Push Notifications
{: #trigger_push_notify}

Pour créer un déclencheur qui s'exécute chaque fois qu'un nouveau périphérique est enregistré auprès de l'application de service Push Notifications, voir l'exemple suivant :

1. Créez une liaison de package qui est configurée pour votre service Push Notifications avec vos paramètres **appId** et **appSecret**.
  ```
  ibmcloud wsk package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. Créez un déclencheur pour le type d'événement `onDeviceRegister` du service Push Notifications en utilisant votre flux `myPush/webhook`.
  ```
  ibmcloud wsk trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. Vous pouvez créer une règle qui envoie un message à chaque fois qu'un nouveau périphérique est enregistré. Créez une règle à l'aide de l'action et du déclencheur précédents.
  ```
  ibmcloud wsk rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

4. Vérifiez les résultats en exécutant la commande `ibmcloud wsk activation poll`.
  ```
  ibmcloud wsk activation poll
  ```
  {: pre}

5. Enregistrez un périphérique dans votre application {{site.data.keyword.Bluemix_notm}}. Vous pouvez voir que la règle (`rule`), le déclencheur (`trigger`) et l'action (`action`) sont exécutés dans le [tableau de bord](https://console.bluemix.net/openwhisk/dashboard) {{site.data.keyword.openwhisk}}.

  L'action envoie une notification push.
