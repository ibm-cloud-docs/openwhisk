---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: websocket, functions, actions, package

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

# WebSocket
{: #pkg_websocket}

El paquete preinstalado `/whisk.system/websocket` proporciona un método cómodo para publicar mensajes en un WebSocket.
{: shortdesc}

El paquete incluye las acciones siguientes:

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/websocket` | paquete | uri | Programas de utilidad para comunicar con WebSockets |
| `/whisk.system/websocket/send` | acción | uri, payload | Enviar la carga útil al URI de WebSocket |

Si tiene intención de enviar varios mensajes al mismo URI de WebSocket, se recomienda crear un enlace de paquete con el valor `uri`. Con enlace, no necesita especificar el valor cada vez que utilice la acción `send`.

## Envío de un mensaje a un WebSocket

La acción `/whisk.system/websocket/send` envía una carga útil a un URI de WebSocket. Los parámetros son según se indica a continuación:

- `uri`: el URI del servidor websocket (por ejemplo, ws://mywebsockethost:80).
- `payload`: el mensaje que desea enviar a WebSocket.

