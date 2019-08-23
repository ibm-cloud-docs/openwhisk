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

Le package préinstallé `/whisk.system/websocket` offre un moyen très pratique de publier des messages sur un WebSocket.
{: shortdesc}

Le package inclut les actions ci-dessous.

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/websocket` | Package | `uri` | Utilitaires permettant de communiquer avec des WebSockets |
| `/whisk.system/websocket/send` | Action | `uri`, `payload` | Envoi du contenu à l'URI de WebSocket |

Si vous prévoyez d'envoyer un grand nombre de messages au même URI de WebSocket, il est recommandé de créer une liaison de package avec la valeur `uri`. Grâce à la liaison, vous n'avez pas besoin de spécifier la valeur à chaque fois que vous utilisez l'action `send`.

## Envoi d'un message à un WebSocket

L'action `/whisk.system/websocket/send` envoie un contenu à un URI de WebSocket. Les paramètres sont les suivants. 

| Paramètre | Description |
| --- | --- | 
| `uri` | Identificateur URI du serveur WebSocket. Exemple : `ws://mywebsockethost:80`. |
| `payload` | Message à envoyer au WebSocket. |



