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

Das vorinstallierte Paket `/whisk.system/websocket` bietet eine bequeme Möglichkeit, Nachrichten an ein WebSocket zu senden (posten).
{: shortdesc}

Das Paket enthält die folgenden Aktionen.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/websocket` | Paket | `uri` | Dienstprogramme zur Kommunikation mit WebSockets |
| `/whisk.system/websocket/send` | Aktion | `uri`, `payload` | Senden von Nutzdaten (payload) an den WebSocket-URI |

Wenn Sie planen, viele Nachrichten an denselben WebSocket-URI zu senden, wird empfohlen, eine Paketbindung mit dem Wert `uri` zu erstellen. Mit der Bindung brauchen Sie den Wert nicht jedes Mal anzugeben, wenn Sie die Aktion `send` verwenden.

## Nachricht an ein WebSocket senden

Die Aktion `/whisk.system/websocket/send` sendet Nutzdaten (payload) an einen WebSocket-URI. Die Parameter lauten wie folgt. 

| Parameter | Beschreibung |
| --- | --- | 
| `uri` | Der URI des WebSocket-Servers. Beispiel: `ws://mywebsockethost:80`. |
| `payload` | Die Nachricht, die an das WebSocket gesendet werden soll. |



