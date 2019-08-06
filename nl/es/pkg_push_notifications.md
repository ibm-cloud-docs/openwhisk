---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: push notifications, functions, webhooks

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Notificaciones Push
{: #pkg_push_notifications}


## Paquetes
{: #pkg_push_packages}

| Paquete | Disponibilidad | Descripción |
| --- | --- | --- |
| [`/whisk.system/pushnotifications`](#pkg_push_send) | Preinstalado (No disponible en Tokio) | Enviar notificaciones push a uno o más dispositivos especificados. |
| [`/whisk.system/pushnotifications/webhook`](#pkg_push_mobile) | Preinstalado (No disponible en Tokio) | Enviar notificaciones push móviles en sucesos de dispositivo. |
| [`/push-notifications`](#pkg_push_mobile_send) | Instalable | Interaccionar con instancias de servicio de {{site.data.keyword.mobilepushfull}}. Enviar un mensaje, o crear, actualizar o suprimir un webhook. |

## Envío de notificaciones push
{: #pkg_push_send}

El paquete preinstalado no está disponible en la región de Tokio. Consulte el paquete [Notificación push](#pkg_push_mobile_send) instalable para la acción
`sendMessage` que utiliza autenticación de IAM
{: tip}

Aprenda a crear un enlace de paquete de Notificación push y a enviar una notificación push simple con el paquete `/whisk.system/pushnotifications`.
{: shortdesc}

El paquete incluye las siguientes acciones y canales de información:

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` | Paquete | `appId`, `appSecret`, `admin_url` | Trabajar con el servicio Push. |
| `/whisk.system/pushnotifications/sendMessage` | Acción | `text`, `url`, `deviceIds`, `platforms`, `userIds`, `tagNames`, `gcmCollapseKey`, `gcmCategory`, `gcmIcon`, `gcmDelayWhileIdle`, `gcmSync`, `gcmVisibility`, `gcmPayload`, `gcmPriority`, `gcmSound`, `gcmTimeToLive`, `gcmStyleType`, `gcmStyleTitle`, `gcmStyleUrl`, `gcmStyleText`, `gcmStyleLines`, `gcmLightsLedArgb`, `gcmLightsLedOnMs`, `gcmLightsLedOffMs`, `apnsBadge`, `apnsCategory`, `apnsIosActionKey`, `apnsPayload`, `apnsType`, `apnsSound`, `apnsTitleLocKey`, `apnsLocKey`, `apnsLaunchImage`, `apnsTitleLocArgs`, `apnsLocArgs`, `apnstitle`, `apnsSubtitle`, `apnsAttachmentUrl`, `fireFoxTitle`, `fireFoxIconUrl`, `fireFoxTimeToLive`, `fireFoxPayload`, `safariTitle`, `safariUrlArgs`, `safariAction`, `chromeTitle`, `chromeIconUrl`, `chromeTimeToLive`, `chromePayload`, `chromeAppExtTitle`, `chromeAppExtCollapseKey`, `chromeAppExtDelayWhileIdle`, `chromeAppExtIconUrl`, `chromeAppExtTimeToLive`, `chromeAppExtPayload` | Enviar notificaciones push a uno o más dispositivos especificados. |


Para obtener información sobre cómo activar sucesos desencadenantes cuando hay actividad de dispositivo, consulte [Mobile Push en sucesos de dispositivo](#pkg_push_mobile).

### Creación de un enlace de paquete Push
{: #pkg_push_create}

Para crear un enlace de paquete de notificaciones push. debe especificar los parámetros siguientes.

| Parámetro | Descripción |
| --- | --- |
| `appId` | El **GUID de la app** de {{site.data.keyword.cloud}}. |
| `appSecret` | El **secreto de app** del servicio de notificación push de {{site.data.keyword.cloud_notm}}. |

Para crear un enlace de paquete, siga los siguientes pasos:

1. Cree una aplicación de {{site.data.keyword.cloud_notm}} en el [panel de control de {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com){: external}.

2. Inicialice el servicio de notificación push y enlace el servicio con la aplicación de {{site.data.keyword.cloud_notm}}.

3. Configure la [aplicación de notificación push](/docs/services/mobilepush?topic=mobile-pushnotification-getting-started).

  Asegúrese de recordar el **GUID de App** y el **Secreto de App** de la app de {{site.data.keyword.cloud_notm}} que ha creado.

4. Cree un enlace de paquete con `/whisk.system/pushnotifications`.
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myPush -p appId myAppID -p appSecret myAppSecret
  ```
  {: pre}

5. Compruebe que el enlace de paquete existe:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Resultado de ejemplo**
  ```
  packages
  /myNamespace/myPush private binding
  ```
  {: screen}

### Parámetros de notificación push
{: #pkg_push_params}

La acción `/whisk.system/pushnotifications/sendMessage` envía notificaciones push a los dispositivos registrados. Los parámetros son los siguientes.

| Parámetro | Descripción |
| --- | --- |
| `text` | El mensaje de notificación a mostrar al usuario. Por ejemplo, `-p text "Hi, OpenWhisk send a notification"`. |
| `url` | Un URL que se puede enviar junto con la alerta. Por ejemplo, `-p url "https:\\www.w3.ibm.com"`. |
| `apiHost` | Una serie opcional que especifica el host de la API. El valor predeterminado es `mobile.ng.bluemix.net`.  Por ejemplo: `-p apiHost "mobile.eu-gb.bluemix.net"`. |
| `deviceIds` | La lista de dispositivos especificados. Por ejemplo, `-p deviceIds ["deviceID1"]`. |
| `platforms` | Enviar una notificación a los dispositivos de las plataformas especificadas. 'A' para dispositivos Apple (iOS) y 'G' para dispositivos Google (Android). Por ejemplo, `-p platforms ["A"]`. |
| `userIds` | Enviar notificaciones a dispositivos de los usuarios que especifique. Por ejemplo, `-p userIds "[\"testUser\"]"`. |
| `tagNames` | Enviar una notificación a los dispositivos que están suscritos a cualquiera de estas etiquetas. Por ejemplo, `-p tagNames "[\"tag1\"]"`. |
| `gcmCollapseKey` | Este parámetro identifica un grupo de mensajes. |
| `gcmCategory` | El identificador de categoría a utilizar para las notificaciones push interactivas. |
| `gcmIcon` | Especifica el nombre del icono a visualizar en la notificación. Asegúrese de que el icono ya está empaquetado con la aplicación de cliente. |
| `gcmDelayWhileIdle` | Cuando este parámetro se establece en true, el mensaje no se envía hasta que el dispositivo esté activo. |
| `gcmSync` | La mensajería del grupo de dispositivos facilita que cada instancia de app de un grupo refleje el estado actualizado de mensajería. |
| `gcmVisibility` | private/public - visibilidad de esta notificación, que afecta a cómo y cuándo se muestran las notificaciones en una pantalla bloqueada segura. |
| `gcmPayload` | Carga útil JSON personalizada que se envía como parte del mensaje de notificación. Por ejemplo, `-p gcmPayload "{\"hi\":\"hello\"}"`. |
| `gcmPriority` | Establece la prioridad del mensaje. |
| `gcmSound` | El archivo de sonido (en el dispositivo) que se reproduce cuando la notificación llega al dispositivo. |
| `gcmTimeToLive` | Este parámetro especifica cuánto tiempo (en segundos) se conserva el mensaje en el almacenamiento GCM si el dispositivo está fuera de línea. |
| `gcmStyleType` | Especifica el tipo de notificaciones expandible. Los valores posibles son `bigtext_notification`, `picture_notification`, `inbox_notification`. |
| `gcmStyleTitle` | Especifica el título de la notificación. El título se muestra cuando se expande la notificación. El título se debe especificar para las tres notificaciones expandibles. |
| `gcmStyleUrl` | Un URL del que se debe obtener la imagen para la notificación. Se debe especificar para `picture_notification`. |
| `gcmStyleText` | El texto detallado que se tiene que visualizar cuando se expande una `bigtext_notification`. Se debe especificar para `bigtext_notification`. |
| `gcmStyleLines` | Una matriz de series que se debe visualizar en modalidad de bandeja de entrada para `inbox_notification`. Se debe especificar para `inbox_notification`. |
| `gcmLightsLedArgb` | El color del led. El hardware realiza la mejor aproximación posible. |
| `gcmLightsLedOnMs` | El número de milisegundos que el LED estará encendido mientras parpadea. El hardware realiza la mejor aproximación posible. |
| `gcmLightsLedOffMs` | El número de milisegundos que el LED estará apagado mientras parpadea. El hardware realiza la mejor aproximación posible. |
| `apnsBadge` | El número a mostrar como identificador del icono de aplicación. |
| `apnsCategory` | El identificador de categoría a utilizar para las notificaciones push interactivas. |
| `apnsIosActionKey` | El título de la clave de acción. |
| `apnsPayload` | Carga útil JSON personalizada que se envía como parte del mensaje de notificación. |
| `apnsType` | ['DEFAULT', 'MIXED', 'SILENT']. |
| `apnsSound` | El nombre del archivo de sonido del paquete de aplicación. El sonido de este archivo se reproduce como una alerta. |
| `apnsTitleLocKey` | La clave de una serie de título en el archivo `Localizable.strings` del entorno local actual. La serie de la clave se puede formatear con especificadores %@ y %n$@ para ser sustituidos con las variables especificadas en la matriz `titleLocArgs`. |
| `apnsLocKey` | La clave de una serie de mensaje de alerta en el archivo `Localizable.strings` del entorno local actual (que se establece en las preferencias de idioma del usuario). La serie de la clave se puede formatear con especificadores %@ y %n$@ para ser sustituidos con las variables especificadas en la matriz `locArgs`. |
| `apnsLaunchImage` | El nombre de un archivo de imagen en el paquete de la app, con o sin la extensión del nombre de archivo. La imagen se utiliza como imagen de lanzamiento cuando los usuarios pulsan en el botón de acción o mueven el control deslizante de acción. |
| `pnsTitleLocArgs` | Valores de cadena variables que aparecen en el lugar de los especificadores de formato en `title-loc-key`. |
| `apnsLocArgs` | Valores de cadena variables que aparecen en el lugar de los especificadores de formato en `locKey`. |
| `apnstitle` | El título de las notificaciones push completas (soportado únicamente en iOS 10 y superior). |
| `apnsSubtitle` | El subtítulo de las notificaciones completas. (Soportado únicamente en iOS 10 y superior).
| `apnsAttachmentUrl` | El enlace al medio de las notificaciones iOS (vídeo, audio, GIF, imágenes - soportado únicamente en iOS 10 y superior). |
| `fireFoxTitle` | Especifica el título que se debe definir para la notificación push web. |
| `fireFoxIconUrl` | El URL del icono que se debe establecer para la notificación push web. |
| `fireFoxTimeToLive` | Este parámetro especifica cuánto tiempo (en segundos) se conserva el mensaje en el almacenamiento GCM si el dispositivo está fuera de línea. |
| `fireFoxPayload` | Carga útil JSON personalizada que se envía como parte del mensaje de notificación. |
| `chromeTitle` | Especifica el título que se debe definir para la notificación push web. |
| `chromeIconUrl` | El URL del icono que se debe establecer para la notificación push web. |
| `chromeTimeToLive` | Este parámetro especifica cuánto tiempo (en segundos) se conserva el mensaje en el almacenamiento GCM si el dispositivo está fuera de línea. |
| `chromePayload` | Carga útil JSON personalizada que se envía como parte del mensaje de notificación. |
| `safariTitle` | Especifica el título que se debe definir para las notificaciones push de Safari. |
| `safariUrlArgs` | Los argumentos de URL necesarios para utilizar con esta notificación. Estos argumentos se deben proporcionar como una matriz JSON. |
| `safariAction` | La etiqueta del botón de acción. |
| `chromeAppExtTitle` | Especifica el título que se debe definir para la notificación push web. |
| `chromeAppExtCollapseKey` | Este parámetro identifica un grupo de mensajes. |
| `chromeAppExtDelayWhileIdle` | Cuando este parámetro se establece en true, indica que el mensaje no se envía hasta que el dispositivo esté activo. |
| `chromeAppExtIconUrl` | El URL del icono que se debe establecer para la notificación push web. |
| `chromeAppExtTimeToLive` | Este parámetro especifica cuánto tiempo (en segundos) se conserva el mensaje en el almacenamiento GCM si el dispositivo está fuera de línea. |
| `chromeAppExtPayload` | Carga útil JSON personalizada que se envía como parte del mensaje de notificación. |

### Ejemplo de notificación push
{: #pkg_push_ex}

Consulte el ejemplo siguiente para enviar una notificación push desde el paquete Notificaciones Push.

Enviar una notificación push utilizando la acción **sendMessage** del enlace de paquete que ha creado anteriormente. Asegúrese de sustituir `/myNamespace/myPush` por su nombre de paquete.
```
ibmcloud fn action invoke /myNamespace/myPush/sendMessage --blocking --result -p url https://example.com -p text "this is my message" -p sound soundFileName -p deviceIds "[\"T1\",\"T2\"]"
```
{: pre}

**Resultado de ejemplo**
```
{
  "result":{
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

## Envío de notificaciones push en sucesos de dispositivos móviles
{: #pkg_push_mobile}

Este paquete preinstalado no está disponible en la región de Tokio.
{: tip}

Aprenda cómo configurar el servicio de Notificaciones push para activar un desencadenante cuando hay una actividad de dispositivo tal como un registro/anulación de registro o una suscripción/anulación de suscripción de un dispositivo en una aplicación especificada.
{: shortdesc}

### Parámetros
{: #pkg_push_mobile_params}

Los parámetros de `/whisk.system/pushnotifications/webhook` son según se indica a continuación:

| Parámetro | Descripción |
|--- | --- |
| `appId` | El GUID de la app de {{site.data.keyword.cloud_notm}}. |
| `appSecret` | El `appSecret` del servicio de notificación push de {{site.data.keyword.cloud_notm}}. |
| `events` | `onDeviceRegister`, `onDeviceUnregister`, `onDeviceUpdate`, `onSubscribe`, `onUnsubscribe`. Puede utilizar el carácter comodín "`*`" para obtener notificaciones de todos los sucesos. |

### Activación de un suceso desencadenante en la actividad del servicio de notificaciones push
{: #pkg_push_mobile_trigger}

Para crear un desencadenante que se active cada vez que se registre un nuevo dispositivo con la aplicación de servicio de notificaciones push, consulte el ejemplo siguiente:

1. Crear un enlace de paquete que está configurado para su servicio de notificaciones push utilizando su `appId` y `appSecret`.
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. Crear un desencadenante para el tipo de suceso `onDeviceRegister` del servicio de notificaciones push utilizando su canal de información `myPush/webhook`.
  ```
  ibmcloud fn trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. Puede crear una regla que envíe un mensaje cada vez que se registra un nuevo dispositivo. Cree una regla utilizando la acción y desencadenante anteriores.
  ```
  ibmcloud fn rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

4. Compruebe los resultados utilizando el mandato `ibmcloud fn activation poll`.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

5. Registre un dispositivo en la aplicación de {{site.data.keyword.cloud_notm}}. Puede ver que `rule`, `trigger` y `action` se ejecutan en el [panel de control](https://cloud.ibm.com/openwhisk/dashboard){: external} de {{site.data.keyword.openwhisk}}.

  **Resultado**
  
  La acción envía una notificación push.


## Envío de notificaciones push o creación, actualización y supresión de webhooks
{: #pkg_push_mobile_send}

El paquete instalable {{site.data.keyword.mobilepushshort}} proporciona un conjunto de acciones para interactuar con las instancias de servicio de {{site.data.keyword.mobilepushfull}}. Estas acciones le permiten enviar un mensaje o crear, actualizar o suprimir un webhook.
{: shortdesc}

El paquete {{site.data.keyword.mobilepushshort}} incluye las acciones siguientes:

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/push-notifications` | Paquete | `apikey`, `appGuid` | Trabajar con una instancia de {{site.data.keyword.mobilepushshort}}. |
| `/push-notifications/send-message` | Acción | `text`, `url`, `deviceIds`, `platforms`, `userIds`, `tagNames`, `gcmCollapseKey`, `gcmCategory`, `gcmIcon`, `gcmDelayWhileIdle`, `gcmSync`, `gcmVisibility`, `gcmPayload`, `gcmPriority`, `gcmSound`, `gcmTimeToLive`, `gcmStyleType`, `gcmStyleTitle`, `gcmStyleUrl`, `gcmStyleText`, `gcmStyleLines`, `gcmLightsLedArgb`, `gcmLightsLedOnMs`, `gcmLightsLedOffMs`, `apnsBadge`, `apnsCategory`, `apnsIosActionKey`, `apnsPayload`, `apnsType`, `apnsSound`, `apnsTitleLocKey`, `apnsLocKey`, `apnsLaunchImage`, `apnsTitleLocArgs`, `apnsLocArgs`, `apnstitle`, `apnsSubtitle`, `apnsAttachmentUrl`, `fireFoxTitle`, `fireFoxIconUrl`, `fireFoxTimeToLive`, `fireFoxPayload`, `safariTitle`, `safariUrlArgs`, `safariAction`, `chromeTitle`, `chromeIconUrl`, `chromeTimeToLive`, `chromePayload`, `chromeAppExtTitle`, `chromeAppExtCollapseKey`, `chromeAppExtDelayWhileIdle`, `chromeAppExtIconUrl`, `chromeAppExtTimeToLive`, `chromeAppExtPayload` | Enviar notificaciones push a uno o más dispositivos especificados. |
| `/push-notifications/webhook` | Acción | `events` | Activar sucesos desencadenantes en actividades de dispositivo (registro, anulación del registro, suscripción o anulación de suscripción de dispositivos) en el servicio Push. |

### Creación de una instancia de servicio de {{site.data.keyword.mobilepushshort}}
{: #service_instance_push}

Antes de instalar el paquete, debe crear una instancia de {{site.data.keyword.mobilepushshort}}.

1. [Cree una instancia de servicio de {{site.data.keyword.mobilepushshort}}](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a).

2. [Cree un conjunto de credenciales de servicio ](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1) para la instancia de servicio de Notificaciones push.

3. [Configure la instancia de servicio de {{site.data.keyword.mobilepushshort}}](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2).

### Instalación del paquete {{site.data.keyword.mobilepushshort}}
{: #pkg_push_mobile_install}

Una vez que tenga una instancia de servicio de {{site.data.keyword.mobilepushshort}}, utilice la CLI o la interfaz de usuario de {{site.data.keyword.openwhisk}} para instalar el paquete {{site.data.keyword.mobilepushshort}} en su espacio de nombres.

### Instalación desde la CLI de {{site.data.keyword.openwhisk_short}}
{: #pkg_push_mobile_cli}

Antes de empezar:
  1. [Instale el plugin de {{site.data.keyword.openwhisk_short}} para la CLI de {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

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

    **Resultado**
    ```
    packages
    /myOrg_mySpace/push-notifications private
    ```
    {: screen}

5. Enlace a las credenciales de la instancia de servicio de {{site.data.keyword.mobilepushshort}} que ha creado para el paquete.
    ```
    ibmcloud fn service bind imfpush push-notifications
    ```
    {: pre}

    **Resultado de ejemplo**
    ```
    Credentials 'Credentials-1' from 'imfpush' service instance 'Push-Notifications-r1' bound to 'push-notifications'.
    ```
    {: screen}

6. Verifique que el paquete esté configurado con sus credenciales de la instancia de servicio de {{site.data.keyword.mobilepushshort}}.
    ```
    ibmcloud fn package get /myOrg_mySpace/push-notifications parameters
    ```
    {: pre}

    **Resultado de ejemplo**
    ```
    ok: got package /myOrg_mySpace/push-notifications, displaying field parameters
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
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:ibmcloud:public:imfpush:us-south:a/abcd1234abcd1234:abcd1234-abcd-1234-abcd1234::",
            "iam_apikey_name": "auto-generated-apikey-abcd1234abcd1234abcd1234",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:ibmcloud:public:iam-identity::a/1234abcd1234abcd::serviceid:ServiceId-12345678-1234-12ab-abc1-1234abcd1234abcd",
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
{: #pkg_push_mobile_ui}

1. En la consola de {{site.data.keyword.openwhisk_short}}, vaya a la [página Crear](https://cloud.ibm.com/openwhisk/create){: external}.

2. Utilizando el menú desplegable de espacios de nombres de la esquina superior derecha, seleccione el espacio de nombres donde desea instalar el paquete de {{site.data.keyword.cos_full_notm}}.

3. Pulse **Instalar paquetes**.

4. Pulse el grupo de paquetes **IBM {{site.data.keyword.mobilepushshort}}** y, a continuación, pulse sobre el paquete **IBM {{site.data.keyword.mobilepushshort}}**.

5. En la sección Entornos de ejecución disponibles, seleccione nodeJS en la lista desplegable y, a continuación, pulse **Instalar**.

6. Una vez que se haya instalado el paquete, se le redirigirá a la página Acciones donde podrá buscar su nuevo paquete, que se denomina **push-notifications**.

7. Para utilizar las acciones del paquete **push-notifications**, debe enlazar las credenciales de servicio a la acción.
  * Para enlazar las credenciales de servicio con todas las acciones del paquete, siga los pasos 5 y 6 en las instrucciones de la CLI listadas más arriba.
  * Para enlazar las credenciales de servicio con acciones individuales, realice los pasos siguientes en la interfaz de usuario. **Nota**: Debe completar los pasos siguientes con cada acción que desee utilizar.
    1. Pulse sobre una acción del paquete **push-notifications** que desee utilizar. Se abrirá la página de detalles de dicha acción.
    2. En la navegación del lado izquierdo, pulse en la sección **Parámetros**.
    3. Especifique un nuevo **parámetro**. Para la clave, especifique `__bx_creds`. Para el valor, pegue en el objeto JSON de credenciales de servicio de la instancia de servicio que ha creado anteriormente.

### Enviar una notificación push
{: #pkg_push_mobile_sendmsg}

Para enviar un mensaje a través del servicio de notificaciones push utilizando la acción `send-message`:
```
ibmcloud fn action invoke push-notifications/send-message --blocking --result --param messageText "Let's code something" --param messageUrl "http://developer.ibm.com"
```
{: pre}

**Resultado de ejemplo**
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

### Crear un webhook
{: #pkg_push_mobile_hook}

Para crear un webhook para el servicio {{site.data.keyword.mobilepushshort}} para los sucesos `onDeviceRegister`:

```
 ibmcloud fn action invoke push-notifications/webhook --blocking --param triggerName "/myPackage/myTrigger" --param events onDeviceRegister
```
{: pre}

**Resultado de ejemplo**
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



