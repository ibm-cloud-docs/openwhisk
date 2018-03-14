---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Usando o pacote WebSocket
{: #openwhisk_catalog_websocket}

O pacote `/whisk.system/websocket` oferece uma maneira conveniente de postar mensagens para um WebSocket.
{: shortdesc}

O pacote inclui as Ações a seguir:

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/websocket` | Pacote | uri | Utilitários para comunicação com WebSockets |
| `/whisk.system/websocket/send` | Ação | uri, payload | Enviar a carga útil para o URI do WebSocket |

Se você planeja enviar muitas mensagens para o mesmo URI do WebSocket, sugere-se criar uma ligação de pacote com o valor `uri`.  Com a ligação, não será necessário especificar o valor cada vez que a ação `send` for usada.

## Enviar uma mensagem para um WebSocket

A ação `/whisk.system/websocket/send` envia uma carga útil para um URI do WebSocket. Os parâmetros são os seguintes:

- `uri`: o URI do servidor websocket (por exemplo, ws://mywebsockethost:80).
- `payload`: a mensagem que você deseja enviar ao WebSocket.
