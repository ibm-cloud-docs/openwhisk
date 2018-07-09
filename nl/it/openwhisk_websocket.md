---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Utilizzo del pacchetto WebSocket
{: #openwhisk_catalog_websocket}

Il pacchetto `/whisk.system/websocket` offre una soluzione pratica per pubblicare i messaggi in un WebSocket.
{: shortdesc}

Il pacchetto include le seguenti azioni:

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/websocket` | Pacchetto | uri | Programmi di utilità per comunicare con i WebSocket |
| `/whisk.system/websocket/send` | Azione | uri, payload | Inviare il payload all'URI WebSocket |

Se prevedi di inviare numerosi messaggi allo stesso URI WebSocket, si consiglia di creare un bind di pacchetto con il valore `uri`.  Con il bind, non dovrai specificare il valore ogni volta che usi l'azione `send`.

## Invia un messaggio a un WebSocket

L'azione `/whisk.system/websocket/send` invia un payload a un URI WebSocket. I parametri sono i seguenti:

- `uri`: l'URI del server websocket (ad esempio, ws://mywebsockethost:80).
- `payload`: il messaggio che vuoi inviare a WebSocket.
