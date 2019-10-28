---

copyright:
  years: 2017, 2019
lastupdated: "2019-10-25"

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

The pre-installed `/whisk.system/websocket` package offers a convenient way to post messages to a WebSocket.
{: shortdesc}

The package includes the following actions.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/websocket` | Package | `uri` | Utilities for communicating with WebSockets |
| `/whisk.system/websocket/send` | Action | `uri`, `payload` | Send the payload to the WebSocket URI |

If you plan to send many messages to the same WebSocket URI, creating a package binding with the `uri` value is suggested. With binding, you don't need to specify the value each time that you use the `send` action.

## Send a message to a WebSocket

The `/whisk.system/websocket/send` action sends a payload to a WebSocket URI. The parameters are as follows.

| Parameter | Description |
| --- | --- |
| `uri` | The URI of the WebSocket server. For example, `ws://mywebsockethost:80`. |
| `payload` | The message to send to the WebSocket. |
