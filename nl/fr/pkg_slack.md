---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: slack package, token-based, api, functions

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


# Slack
{: #pkg_slack}

Le package préinstallé `/whisk.system/slack` offre un moyen très pratique d'utiliser les [API Slack](https://api.slack.com/){: external}.
{: shortdesc}

Le package inclut les actions suivantes :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/slack` | Package | `url`, `channel`, `username` | Interagir avec l'API Slack. |
| `/whisk.system/slack/post` | Action | `text`, `url`, `channel`, `username` | Envoie un message à un canal Slack. |

Il est recommandé de créer une liaison de package avec les valeurs `username`, `url` et `channel`. Grâce à la liaison, il n'est pas nécessaire de spécifier les valeurs à chaque fois que vous appelez l'action dans le package.

## Publication d'un message dans un canal Slack

L'action `/whisk.system/slack/post` publie un message dans un canal Slack spécifié. Les paramètres ci-après sont pris en charge.

| Paramètre | Description |
| --- | --- |
| `url` | URL du webhook Slack. |
| `channel` | Canal Slack dans lequel publier le message. |
| `username` | Nom sous lequel publier le message. |
| `text` | Message à publier. |
| `token` | (facultatif) [Jeton d'accès](https://api.slack.com/tokens){: external} Slack. |

L'exemple suivant montre comment configurer Slack, créer une liaison de package et publier un message sur un canal :

1. Configurez un [webhook entrant](https://api.slack.com/incoming-webhooks){: external} Slack pour votre équipe.

  Une fois Slack configuré, vous obtenez une adresse URL de webhook similaire à `https://hooks.slack.com/services/aaaaaaaaa/bbbbbbbbb/cccccccccccccccccccccccc`. Vous aurez besoin de cette adresse URL de webhook à l'étape suivante.

2. Créez une liaison de package avec vos données d'identification Slack, le canal dans lequel publier le message, et le nom d'utilisateur sous lequel publier le message.
  ```
  ibmcloud fn package bind /whisk.system/slack mySlack \
    --param url "https://hooks.slack.com/services/..." \
    --param username "Bob" \
    --param channel "#MySlackChannel"
  ```
  {: pre}

3. Appelez l'action `post` dans votre liaison de package pour publier un message dans votre canal Slack.
  ```
  ibmcloud fn action invoke mySlack/post --blocking --result \
    --param text "Hello from OpenWhisk!"
  ```
  {: pre}

## Utilisation de l'API reposant sur le jeton Slack

Si vous préférez, vous pouvez choisir d'utiliser l'API reposant sur le jeton Slack plutôt que l'API de webhook. Dans ce cas, transmettez un paramètre `token` contenant votre [jeton d'accès](https://api.slack.com/tokens){: external} Slack. Vous pourrez ensuite utiliser l'une des [méthodes d'API Slack](https://api.slack.com/methods){: external} comme paramètre `url`. Par exemple, pour envoyer un message, utilisez [<ph class="ignoreSpelling">slack.postMessage</ph>](https://api.slack.com/methods/chat.postMessage){: external} comme valeur de paramètre `url`.



