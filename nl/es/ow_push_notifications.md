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

# Paquete {{site.data.keyword.mobilepushshort}}

El paquete {{site.data.keyword.mobilepushshort}} proporciona un conjunto de acciones para interactuar con las instancias de servicio de {{site.data.keyword.mobilepushfull}}. Estas acciones le permiten enviar un mensaje o crear, actualizar o suprimir un webhook.
{: shortdesc}

El paquete {{site.data.keyword.mobilepushshort}} incluye las acciones siguientes:

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/push-notifications` | paquete | apikey, appGuid | Trabajar con una instancia de {{site.data.keyword.mobilepushshort}}. |
| `/push-notifications/send-message` | acción | text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | Enviar notificaciones push a uno o más dispositivos especificados. |
| `/push-notifications/webhook` | acción | events | Activar sucesos desencadenantes en actividades de dispositivo (registro, anulación del registro, suscripción o anulación de suscripción de dispositivos) en el servicio Push. |

## Creación de una instancia de servicio de {{site.data.keyword.mobilepushshort}}
{: #service_instance_push}

Antes de instalar el paquete, debe crear una instancia de {{site.data.keyword.mobilepushshort}}.

1. [Cree una instancia de servicio de {{site.data.keyword.mobilepushshort}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a).

2. [Cree un conjunto de credenciales de servicio ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1) para la instancia de servicio de notificaciones push.

3. [Configure la instancia de servicio de {{site.data.keyword.mobilepushshort}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2).

## Instalación del paquete {{site.data.keyword.mobilepushshort}}
{: #install_push}

Una vez que tenga una instancia de servicio de {{site.data.keyword.mobilepushshort}}, utilice la CLI o la interfaz de usuario de {{site.data.keyword.openwhisk}} para instalar el paquete {{site.data.keyword.mobilepushshort}} en su espacio de nombres.

### Instalación desde la CLI de {{site.data.keyword.openwhisk_short}}
{: #install_cli}

Antes de empezar:
  1. [Instale el plugin de {{site.data.keyword.openwhisk_short}} para la CLI de {{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

Para instalar el paquete {{site.data.keyword.mobilepushshort}}:

1. Clone el repositorio del paquete {{site.data.keyword.mobilepushshort}}.
    ```
    git clone https://github.com/ibm-functions/package-push-notifications.git
    ```
    {: pre}

2. Vaya al directorio `runtimes/nodejs`.
    ```
    cd package-push-notifications/runtimes/nodejs
    ```
    {: pre}

3. Despliegue el paquete.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Verifique que el paquete `push-notifications` se ha añadido a la lista de paquetes.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Salida:
    ```
    packages
    /myOrg_mySpace/push-notifications private
    ```
    {: screen}

5. Enlace a las credenciales de la instancia de servicio de {{site.data.keyword.mobilepushshort}} que creó para el paquete.
    ```
    ibmcloud fn service bind imfpush push-notifications
    ```
    {: pre}

    Salida de ejemplo:
    ```
    Credentials 'Credentials-1' from 'imfpush' service instance 'Push-Notifications-r1' bound to 'push-notifications'.
    ```
    {: screen}

6. Verifique que el paquete esté configurado con sus credenciales de la instancia de servicio de {{site.data.keyword.mobilepushshort}}.
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/push-notifications parameters
    ```
    {: pre}

    Salida de ejemplo:
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

### Instalación desde la interfaz de usuario de {{site.data.keyword.openwhisk_short}}
{: #install_ui}

1. En la consola de {{site.data.keyword.openwhisk_short}}, vaya a la [página Crear ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/create).

2. Utilizando el conmutador de espacios de nombres de la esquina superior derecha, seleccione el espacio de nombres en el que desee instalar el paquete de {{site.data.keyword.cos_short}}.

3. Pulse **Instalar paquetes**.

4. Pulse el grupo de paquetes **IBM {{site.data.keyword.mobilepushshort}}** y, a continuación, pulse sobre el paquete **IBM {{site.data.keyword.mobilepushshort}}**.

5. En la sección Entornos de ejecución disponibles, seleccione NodeJS en la lista desplegable y, a continuación, pulse **Instalar**.

6. Una vez que se haya instalado el paquete, se le redirigirá a la página Acciones donde podrá buscar su nuevo paquete, que se denomina **push-notifications**.

7. Para utilizar las acciones del paquete **push-notifications**, debe enlazar las credenciales de servicio a la acción.
  * Para enlazar las credenciales de servicio con todas las acciones del paquete, siga los pasos 5 y 6 en las instrucciones de la CLI listadas más arriba.
  * Para enlazar las credenciales de servicio con acciones individuales, realice los pasos siguientes en la interfaz de usuario. **Nota**: Debe completar los pasos siguientes con cada acción que desee utilizar.
    1. Pulse sobre una acción del paquete **push-notifications** que desee utilizar. Se abrirá la página de detalles de dicha acción.
    2. En la navegación del lado izquierdo, pulse en la sección **Parámetros**.
    3. Especifique un nuevo **parámetro**. Para la clave, especifique `__bx_creds`. Para el valor, pegue en el objeto JSON de credenciales de servicio de la instancia de servicio que ha creado anteriormente.

## Utilización del paquete {{site.data.keyword.mobilepushshort}}
{: #usage_push}

### Enviar una notificación push
{: #push_notif}

Para enviar un mensaje a través del servicio de notificaciones push utilizando la acción `send-message`:
```
ibmcloud fn action invoke push-notifications/send-message --blocking --result --param messageText "Let's code something" --param messageUrl "http://developer.ibm.com"
```
{: pre}

Salida de ejemplo:
```
{
  "response": {
      "result":{
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

## Crear un webhook
{: #webhook}

Para crear un webhook para el servicio {{site.data.keyword.mobilepushshort}} para los sucesos onDeviceRegister:

```
 ibmcloud fn action invoke push-notifications/webhook --blocking --param triggerName "/myPackage/myTrigger" --param events onDeviceRegister
```
{: pre}

Salida de ejemplo:
```
{
  "response": {
    "result":{
      "error": {}
    },
  "status": "application error",
  "success": false
  },
}
```
{: screen}
