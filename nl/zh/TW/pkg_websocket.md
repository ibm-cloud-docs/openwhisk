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

透過預先安裝的 `/whisk.system/websocket` 套件，可方便地將訊息公佈到 WebSocket。
{: shortdesc}

該套件包含下列動作。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| `/whisk.system/websocket` |套件| `uri` |與 WebSockets 進行通訊的公用程式|
|`/whisk.system/websocket/send` |動作| `uri`、`payload` |將有效負載傳送至 WebSocket URI|

如果您計劃將許多訊息傳送至相同的 WebSocket URI，則建議使用 `uri` 值來建立套件連結。使用連結，您就不需要每次使用 `send` 動作時都指定值。

## 將訊息傳送至 WebSocket

`/whisk.system/websocket/send` 動作會將有效負載傳送至 WebSocket URI。參數如下。

| 參數 |說明|
| --- | --- | 
| `uri` |WebSocket 伺服器的 URI。例如，`ws://mywebsockethost:80`。|
| `payload` |要傳送至 WebSocket 的訊息。|



