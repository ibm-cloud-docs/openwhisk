---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-01"

keywords: WebSocket, functions, actions, package

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


# WebSocket
{: #pkg_websocket}

{{site.data.keyword.openwhisk}} is deprecated. Existing Functions entities such as actions, triggers, or sequences will continue to run, but as of 28 December 2023, you can’t create new Functions entities. Existing Functions entities are supported until October 2024. Any Functions entities that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

The preinstalled `/whisk.system/websocket` package for {{site.data.keyword.openwhisk}} offers a convenient way to post messages to a WebSocket.
{: shortdesc}

The package includes the following actions.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/websocket` | Package | `uri` | Utilities for communicating with WebSockets |
| `/whisk.system/websocket/send` | Action | `uri`, `payload` | Send the payload to the WebSocket URI. |
{: caption="Table 1. WebSocket package entities" caption-side="bottom"}

If you plan to send many messages to the same WebSocket URI, creating a package binding with the `uri` value is suggested. With binding, you don't need to specify the value each time that you use the `send` action.

## Send a message to a WebSocket
{: #send-websocket}

The `/whisk.system/websocket/send` action sends a payload to a WebSocket URI. The parameters are as follows.

| Parameter | Description |
| --- | --- |
| `uri` | The URI of the WebSocket server. For example, `ws://mywebsockethost:80`. |
| `payload` | The message to send to the WebSocket. |
{: caption="Table 2. WebSocket send action parameters" caption-side="bottom"}

