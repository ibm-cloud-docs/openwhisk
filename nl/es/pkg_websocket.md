---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: WebSocket, functions, actions, package

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


# WebSocket
{: #pkg_websocket}

El paquete preinstalado `/whisk.system/websocket` proporciona un método cómodo para publicar mensajes en un WebSocket.
{: shortdesc}

El paquete incluye las acciones siguientes.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/websocket` | Paquete | `uri` | Programas de utilidad para comunicar con WebSockets |
| `/whisk.system/websocket/send` | Acción | `uri`, `payload` | Enviar la carga útil al URI de WebSocket |

Si tiene intención de enviar varios mensajes al mismo URI de WebSocket, se recomienda crear un enlace de paquete con el valor `uri`. Con enlace, no necesita especificar el valor cada vez que utilice la acción `send`.

## Envío de un mensaje a un WebSocket

La acción `/whisk.system/websocket/send` envía una carga útil a un URI de WebSocket. Los parámetros son los siguientes.

| Parámetro | Descripción |
| --- | --- | 
| `uri` | El URI del servidor WebSocket. Por ejemplo, `ws://mywebsockethost:80`. |
| `payload` | El mensaje a enviar a WebSocket. |



