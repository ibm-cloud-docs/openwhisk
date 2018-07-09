---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Mobile Push en sucesos de dispositivo
{: #openwhisk_pushnotifications}

Aprenda cómo configurar el servicio Push para activar un desencadenante cuando hay una actividad de dispositivo tal como un registro/anulación de registro o una suscripción/anulación de suscripción de un dispositivo en una aplicación especificada.
{: shortdesc}

Para obtener información sobre el propio paquete `/whisk.system/pushnotifications`, consulte el tema [Mobile Push](./mobile_push_actions.html), que contiene información sobre cómo crear un enlace de paquete Push y cómo enviar una notificación Push.

## Parámetros de Push
{: #push_parameters}

Los parámetros de `/whisk.system/pushnotifications/webhook` son según se indica a continuación:
- **appId:** GUID de la app de {{site.data.keyword.Bluemix_notm}}.
- **appSecret:** appSecret del servicio de notificaciones push de {{site.data.keyword.Bluemix_notm}}.
- **events:** _onDeviceRegister_, _onDeviceUnregister_, _onDeviceUpdate_, _onSubscribe_, _onUnsubscribe_

  Puede utilizar el carácter comodín "`*`" para obtener notificaciones de todos los sucesos.

## Activación de un suceso desencadenante en la actividad del servicio de notificaciones push
{: #trigger_push_notify}

Para crear un desencadenante que se active cada vez que se registre un nuevo dispositivo con la aplicación de servicio de notificaciones push, consulte el ejemplo siguiente:

1. Crear un enlace de paquete que está configurado para su servicio de notificaciones push utilizando su **appId** y **appSecret**.
  ```
  ibmcloud wsk package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. Crear un desencadenante para el tipo de suceso `onDeviceRegister` del servicio de notificaciones push utilizando su canal de información `myPush/webhook`.
  ```
  ibmcloud wsk trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. Puede crear una regla que envíe un mensaje cada vez que se registra un nuevo dispositivo. Cree una regla utilizando la acción y desencadenante anteriores.
  ```
  ibmcloud wsk rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

4. Compruebe los resultados utilizando el mandato `ibmcloud wsk activation poll`.
  ```
  ibmcloud wsk activation poll
  ```
  {: pre}

5. Registre un dispositivo en la aplicación de {{site.data.keyword.Bluemix_notm}}. Puede ver que la `regla`, el `desencadenante` y la `acción` se ejecutan en el [panel de control](https://console.bluemix.net/openwhisk/dashboard) de {{site.data.keyword.openwhisk}}.

  La acción envía una notificación Push.
