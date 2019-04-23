---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: websocket, functions, actions, package

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# WebSocket-Paket
{: #openwhisk_catalog_websocket}

Das Paket `/whisk.system/websocket` bietet eine bequeme Möglichkeit, Nachrichten an einen WebSocket zu senden (posten).
{: shortdesc}

Das Paket enthält die folgenden Aktionen:

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/websocket` | Paket | uri | Dienstprogramme zur Kommunikation mit WebSockets |
| `/whisk.system/websocket/send` | Aktion | uri, payload | Senden von Nutzdaten (payload) an den WebSocket-URI |

Wenn Sie planen, viele Nachrichten an denselben WebSocket-URI zu senden, wird empfohlen, eine Paketbindung mit dem Wert `uri` zu erstellen. Mit der Bindung brauchen Sie den Wert nicht jedes Mal anzugeben, wenn Sie die Aktion `send` verwenden.

## Nachricht an einen WebSocket senden

Die Aktion `/whisk.system/websocket/send` sendet Nutzdaten (payload) an einen WebSocket-URI. Die folgenden Parameter sind verfügbar:

- `uri`: Der URI des WebSocket-Servers (z. B.: ws://mywebsockethost:80).
- `payload`: Die Nachricht, die an den WebSocket gesendet werden soll.
