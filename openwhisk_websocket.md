---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# WebSocket
{: #openwhisk_catalog_websocket}

The `/whisk.system/websocket` package offers a convenient way to post messages to a WebSocket.
{: shortdesc}

The package includes the following actions:

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/websocket` | package | uri | Utilities for communicating with WebSockets |
| `/whisk.system/websocket/send` | action | uri, payload | Send the payload to the WebSocket URI |

If you plan to send many messages to the same WebSocket URI, creating a package binding with the `uri` value is suggested. With binding, you don't need to specify the value each time that you use the `send` action.

## Send a message to a WebSocket

The `/whisk.system/websocket/send` action sends a payload to a WebSocket URI. The parameters are as follows:

- `uri`: The URI of the websocket server (for example, ws://mywebsockethost:80).
- `payload`: The message that you wish to send to the WebSocket.
