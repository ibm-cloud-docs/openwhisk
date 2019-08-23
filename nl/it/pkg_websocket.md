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

Il pacchetto `/whisk.system/websocket` preinstallato offre una soluzione pratica per pubblicare i messaggi in un WebSocket.
{: shortdesc}

Il pacchetto include le seguenti azioni.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/websocket` | Pacchetto | `uri` | Programmi di utilità per comunicare con i WebSocket |
| `/whisk.system/websocket/send` | Azione | `uri`, `payload` | Inviare il payload all'URI WebSocket |

Se prevedi di inviare numerosi messaggi allo stesso URI WebSocket, si consiglia di creare un bind di pacchetto con il valore `uri`. Con il bind, non dovrai specificare il valore ogni volta che usi l'azione `send`.

## Invia un messaggio a un WebSocket

L'azione `/whisk.system/websocket/send` invia un payload a un URI WebSocket. I parametri sono i seguenti.

| Parametro | Descrizione |
| --- | --- | 
| `uri` | L'URI del server WebSocket. Ad esempio, `ws://mywebsockethost:80`. |
| `payload` | Il messaggio da inviare al WebSocket. |



