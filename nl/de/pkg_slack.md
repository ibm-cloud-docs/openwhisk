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

Das vorinstallierte Paket `/whisk.system/slack` bietet eine komfortable Methode zur Verwendung der [Slack-APIs](https://api.slack.com/){: external}.
{: shortdesc}

Das Paket enthält die folgenden Aktionen:

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/slack` | Paket | `url`, `channel`, `username` | Mit der Slack-API interagieren. |
| `/whisk.system/slack/post` | Aktion | `text`, `url`, `channel`, `username` | Eine Nachricht an einen Slack-Kanal senden. |

Es wird empfohlen, eine Paketbindung mit den Werten `username`, `url` und `channel` zu erstellen. Mit der Bindung brauchen Sie die Werte nicht jedes Mal anzugeben, wenn Sie die Aktion im Paket aufrufen.

## Nachricht an einen Slack-Kanal senden

Die Aktion `/whisk.system/slack/post` sendet eine Nachricht an einen angegebenen Slack-Kanal. Die folgenden Parameter werden unterstützt. 

| Parameter | Beschreibung |
| --- | --- |
| `url` | Die URL für den Slack-Webhook. |
| `channel` | Der Slack-Kanal, an den die Nachricht zu senden ist. |
| `username` | Der Name, unter dem die Nachricht gesendet werden soll. |
| `text` | Eine zu sendende Nachricht. |
| `token` | (Optional) Ein Slack-[Zugriffstoken](https://api.slack.com/tokens){: external}. |

Im folgenden Beispiel wird gezeigt, wie Slack konfiguriert wird, eine Paketbindung erstellt wird und eine Nachricht an einen Kanal gesendet wird.

1. Konfigurieren Sie für Ihr Team einen [eingehenden Webhook](https://api.slack.com/incoming-webhooks){: external} für Slack.

  Nach der Konfiguration von Slack erhalten Sie eine Webhook-URL, die ungefähr wie folgt aussieht: `https://hooks.slack.com/services/aaaaaaaaa/bbbbbbbbb/cccccccccccccccccccccccc`. Der Webhook wird im nächsten Schritt benötigt.

2. Erstellen Sie eine Paketbindung mit Ihren Slack-Berechtigungsnachweisen, mit dem Kanal, an den gesendet werden soll, sowie mit dem Benutzernamen, unter dem gesendet werden soll.
  ```
  ibmcloud fn package bind /whisk.system/slack mySlack \
    --param url "https://hooks.slack.com/services/..." \
    --param username "Bob" \
    --param channel "#MySlackChannel"
  ```
  {: pre}

3. Rufen Sie die Aktion `post` in Ihrer Paketbindung auf, um eine Nachricht an Ihren Slack-Kanal zu senden.
  ```
  ibmcloud fn action invoke mySlack/post --blocking --result \
    --param text "Hello from OpenWhisk!"
  ```
  {: pre}

## Slack-Token-basierte API verwenden

Sie können außerdem auch die Slack-Token-basierte API anstelle der Webhook-API verwenden. Übergeben Sie in diesem Fall einen Parameter `token`, der Ihr [Slack-Zugriffstoken](https://api.slack.com/tokens){: external} enthält. Sie können dann eine beliebige [Slack-API-Methode](https://api.slack.com/methods){: external} als Parameter `url` verwenden. Um beispielsweise eine Nachricht zu senden, verwenden Sie den Parameterwert `url` von [<ph class="ignoreSpelling">slack.postMessage</ph>](https://api.slack.com/methods/chat.postMessage){: external}.



