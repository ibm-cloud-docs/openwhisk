---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: mobile, push notifications, binding, notifications

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Paquete Mobile Push
{: #openwhisk_catalog_pushnotifications}

Este paquete preinstalado no está disponible en la región de Tokio. Consulte el paquete [Push Notification](/docs/openwhisk?topic=cloud-functions-push-notifications-package) instalable para la acción
`sendMessage` que utiliza autenticación de IAM 
{: tip}

Aprenda a crear un enlace de paquete Push y a enviar una notificación Push simple con el paquete `/whisk.system/pushnotifications`, que le ofrece la posibilidad de trabajar con un servicio de envío por push.
{: shortdesc}

El paquete incluye las siguientes acciones y canales de información:

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` | paquete | appId, appSecret, admin_url | Trabajar con el servicio Push. |
| `/whisk.system/pushnotifications/sendMessage` | acción | text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | Enviar notificaciones push a uno o más dispositivos especificados. |


Para obtener información sobre cómo activar sucesos desencadenantes cuando hay actividad de dispositivo, consulte el tema [Mobile Push en sucesos de dispositivo](/docs/openwhisk?topic=cloud-functions-openwhisk_pushnotifications).

## Creación de un enlace de paquete Push
{: #create_push_binding}

Para crear un enlace de paquete de notificaciones push: debe especificar los parámetros siguientes:

-  **appId**: **GUID de la app** de {{site.data.keyword.Bluemix}}.
-  **appSecret**: **Secreto de la app** del servicio de notificaciones Push de {{site.data.keyword.Bluemix_notm}}.

Para crear un enlace de paquete, siga los siguientes pasos:

1. Cree una aplicación de {{site.data.keyword.Bluemix_notm}} en el [Panel de control de {{site.data.keyword.Bluemix_notm}}](http://cloud.ibm.com).

2. Inicialice el servicio de notificación push y enlace el servicio con la aplicación de {{site.data.keyword.Bluemix_notm}}.

3. Configure la [aplicación de notificación push](/docs/services/mobilepush?topic=mobile-pushnotification-gettingstartedtemplate).

  Asegúrese de recordar el **GUID de App** y el **Secreto de App** de la app de {{site.data.keyword.Bluemix_notm}} que ha creado.

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

  Salida de ejemplo:
  ```
  packages
  /myNamespace/myPush private binding
  ```
  {: screen}

## Parámetros de notificación push
{: #push_parameters}

La acción `/whisk.system/pushnotifications/sendMessage` envía notificaciones Push a los dispositivos registrados. Los parámetros son según se indica a continuación:
- `text`: el mensaje de notificación a mostrar al usuario. Por ejemplo, `-p text "Hi, OpenWhisk send a notification"`.
- `url`: un URL que se puede enviar junto con la alerta. Por ejemplo, `-p url "https:\\www.w3.ibm.com"`.
- `apiHost`: una serie opcional que especifica el host de la API. El valor predeterminado es `mobile.ng.bluemix.net`.  Por ejemplo: `-p apiHost "mobile.eu-gb.bluemix.net"`
- `deviceIds`: la lista de dispositivos especificados. Por ejemplo, `-p deviceIds ["deviceID1"]`.
- `plataformas`: enviar una notificación a los dispositivos de las plataformas especificadas. 'A' para dispositivos Apple (iOS) y 'G' para dispositivos Google (Android). Por ejemplo, `-p platforms ["A"]`.
- `userIds`: enviar notificaciones a dispositivos de los usuarios que especifique. Por ejemplo, `-p userIds "[\"testUser\"]"`
- `tagNames`: enviar una notificación a los dispositivos que están suscritos a cualquiera de estas etiquetas. Por ejemplo, `-p tagNames "[\"tag1\"]"`.
- `gcmCollapseKey`: este parámetro identifica un grupo de mensajes
- `gcmCategory`: identificador de categoría a utilizar para las notificaciones push interactivas.
- `gcmIcon`: especifica el nombre del icono a visualizar en la notificación. Asegúrese de que el icono ya está empaquetado con la aplicación de cliente.
- `gcmDelayWhileIdle`: cuando este parámetro se establece en true, el mensaje no se envía hasta que el dispositivo esté activo.
- `gcmSync`: la mensajería del grupo de dispositivos facilita que cada instancia de app de un grupo refleje el estado actualizado de mensajería.
- `gcmVisibility`: private/public - visibilidad de esta notificación, que afecta a cómo y cuándo se muestran las notificaciones en una pantalla bloqueada segura.
- `gcmPayload`: carga útil JSON personalizada que se envía como parte del mensaje de notificación. Por ejemplo, `-p gcmPayload "{\"hi\":\"hello\"}"`
- `gcmPriority`: establece la prioridad del mensaje.
- `gcmSound`: archivo de sonido (en el dispositivo) que se reproduce cuando la notificación llega al dispositivo.
- `gcmTimeToLive`: este parámetro especifica cuánto tiempo (en segundos) se conserva el mensaje en el almacenamiento GCM si el dispositivo está fuera de línea.
- `gcmStyleType`: especifica el tipo de notificaciones expandible. Los valores posibles son `bigtext_notification`, `picture_notification`, `inbox_notification`.
- `gcmStyleTitle`: especifica el título de la notificación. El título se muestra cuando se expande la notificación. El título se debe especificar para las tres notificaciones expandibles.
- `gcmStyleUrl`: URL del que se debe obtener la imagen para la notificación. Se debe especificar para `picture_notification`.
- `gcmStyleText`: texto detallado que se tiene que visualizar cuando se expande una `bigtext_notification`. Se debe especificar para `bigtext_notification`.
- `gcmStyleLines`: matriz de series que se debe visualizar en modalidad de bandeja de entrada para `inbox_notification`. Se debe especificar para `inbox_notification`.
- `gcmLightsLedArgb`: color del led. El hardware realiza la mejor aproximación posible.
- `gcmLightsLedOnMs`: número de milisegundos que el LED estará encendido mientras parpadea. El hardware realiza la mejor aproximación posible.
- `gcmLightsLedOffMs`: número de milisegundos que el LED estará apagado mientras parpadea. El hardware realiza la mejor aproximación posible.
- `apnsBadge`: número a mostrar como identificador del icono de aplicación.
- `apnsCategory`: identificador de categoría a utilizar para las notificaciones push interactivas.
- `apnsIosActionKey`: título de la clave de acción.
- `apnsPayload`: carga útil JSON personalizada que se envía como parte del mensaje de notificación.
- `apnsType`: ['DEFAULT', 'MIXED', 'SILENT'].
- `apnsSound`: nombre del archivo de sonido del paquete de aplicación. El sonido de este archivo se reproduce como una alerta.
- `apnsTitleLocKey`: clave de una serie de título en el archivo `Localizable.strings` con la traducción actual. La serie de la clave se puede formatear con especificadores %@ y %n$@ para ser sustituidos con las variables especificadas en la matriz `titleLocArgs`.
- `apnsLocKey`: clave de una serie de mensaje de alerta en el archivo `Localizable.strings` para la traducción actual (que se establece en las preferencias de idioma del usuario). La serie de la clave se puede formatear con especificadores %@ y %n$@ para ser sustituidos con las variables especificadas en la matriz locArgs.
- `apnsLaunchImage`: nombre de un archivo de imagen en el paquete de la app, con o sin la extensión del nombre de archivo. La imagen se utiliza como imagen de lanzamiento cuando los usuarios pulsan en el botón de acción o mueven el control deslizante de acción.
- `pnsTitleLocArgs`: valores de serie de variable que aparecerán en lugar de los especificadores de formato en `title-loc-key`.
- `apnsLocArgs`: valores de serie de variable que aparecerán en lugar de los especificadores de formato en `locKey`.
- `apnstitle`: título de las notificaciones push completas (soportado únicamente en iOS 10 y superior).
- `apnsSubtitle`: subtítulo de las notificaciones completas. (Soportado únicamente en iOS 10 y superior).
- `apnsAttachmentUrl`: enlace al medio de las notificaciones iOS (vídeo, audio, GIF, imágenes - soportado únicamente en iOS 10 y superior).
- `fireFoxTitle`: especifica el título que se debe definir para la notificación WebPush.
- `fireFoxIconUrl`: el URL del icono que se debe establecer para la notificación WebPush.
- `fireFoxTimeToLive`: este parámetro especifica cuánto tiempo (en segundos) se conserva el mensaje en el almacenamiento GCM si el dispositivo está fuera de línea.
- `fireFoxPayload`: carga útil JSON personalizada que se envía como parte del mensaje de notificación.
- `chromeTitle`: especifica el título que se debe definir para la notificación WebPush.
- `chromeIconUrl`: el URL del icono que se debe establecer para la notificación WebPush.
- `chromeTimeToLive`: este parámetro especifica cuánto tiempo (en segundos) se conserva el mensaje en el almacenamiento GCM si el dispositivo está fuera de línea.
- `chromePayload`: carga útil JSON personalizada que se envía como parte del mensaje de notificación.
- `safariTitle`: especifica el título a establecer para las notificaciones push de Safari.
- `safariUrlArgs`: argumentos de URL que se necesitan utilizar con esta notificación. Estos argumentos se deben proporcionar como una matriz JSON.
- `safariAction`: etiqueta del botón de acción.
- `chromeAppExtTitle`: especifica el título que se debe definir para la notificación WebPush.
- `chromeAppExtCollapseKey`: este parámetro identifica un grupo de mensajes.
- `chromeAppExtDelayWhileIdle`: cuando este parámetro se establece en true, indica que el mensaje no se envía hasta que el dispositivo esté activo.
- `chromeAppExtIconUrl`: el URL del icono que se debe establecer para la notificación WebPush.
- `chromeAppExtTimeToLive`: este parámetro especifica cuánto tiempo (en segundos) se conserva el mensaje en el almacenamiento GCM si el dispositivo está fuera de línea.
- `chromeAppExtPayload`: carga útil JSON personalizada que se envía como parte del mensaje de notificación.

## Envío de notificaciones push
{: #send_push_notifications}

Consulte el ejemplo siguiente para enviar una notificación push desde el paquete Notificaciones Push.

Enviar una notificación Push utilizando la acción **sendMessage** del enlace de paquete que ha creado anteriormente. Asegúrese de sustituir `/myNamespace/myPush` por su nombre de paquete.
```
ibmcloud fn action invoke /myNamespace/myPush/sendMessage --blocking --result -p url https://example.com -p text "this is my message" -p sound soundFileName -p deviceIds "[\"T1\",\"T2\"]"
```
{: pre}

Salida de ejemplo:
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
