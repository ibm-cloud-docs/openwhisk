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

O pacote `/whisk.system/websocket` pré-instalado oferece uma maneira conveniente de postar mensagens em um WebSocket.
{: shortdesc}

O pacote inclui as ações a seguir.

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/websocket` | Pacote | `uri` | Utilitários para comunicação com WebSockets |
| `/whisk.system/websocket/send` | Ação | `uri`, `payload` | Enviar a carga útil para o URI do WebSocket |

Se você planeja enviar muitas mensagens para o mesmo URI do WebSocket, sugere-se criar uma ligação de pacote com o valor `uri`. Com a ligação, não será necessário especificar o valor cada vez que a ação `send` for usada.

## Enviar uma mensagem para um WebSocket

A ação `/whisk.system/websocket/send` envia uma carga útil para um URI do WebSocket. Os parâmetros são os seguintes.

| Parâmetro | Descrição |
| --- | --- | 
| `uri` | O URI do servidor WebSocket. Por exemplo, `ws://mywebsockethost:80`. |
| `payload` | A mensagem a ser enviada para o WebSocket. |



