---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-29"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Slack-Paket
{: #openwhisk_catalog_slack}

Das Paket `/whisk.system/slack` bietet eine komfortable Methode zur Verwendung der [Slack-APIs](https://api.slack.com/).
{: shortdesc}

Das Paket enthält die folgenden Aktionen:

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/slack` | Paket | url, channel, username | Interaktion mit der Slack-API |
| `/whisk.system/slack/post` | Aktion | text, url, channel, username | Senden einer Nachricht an einen Slack-Kanal |

Es wird empfohlen, eine Paketbindung mit den Werten `username`, `url` und `channel` zu erstellen. Mit der Bindung brauchen Sie die Werte nicht jedes Mal anzugeben, wenn Sie die Aktion im Paket aufrufen.

## Nachricht an einen Slack-Kanal senden

Die Aktion `/whisk.system/slack/post` sendet eine Nachricht an einen angegebenen Slack-Kanal. Die folgenden Parameter sind verfügbar:

- `url`: Die URL für den Slack-Webhook.
- `channel`: Der Slack-Kanal, an den die Nachricht zu senden ist.
- `username`: Der Name, unter dem die Nachricht gesendet werden soll.
- `text`: Ein zu sendender Nachrichtentext.
- `token`: (optional) Ein Slack-[Zugriffstoken](https://api.slack.com/tokens).

Im folgenden Beispiel wird gezeigt, wie Slack konfiguriert wird, eine Paketbindung erstellt wird und eine Nachricht an einen Kanal gesendet wird.

1. Konfigurieren Sie für Ihr Team einen [eingehenden Webhook](https://api.slack.com/incoming-webhooks) für Slack.

  Nach der Konfiguration von Slack erhalten Sie eine Webhook-URL, die ungefähr wie folgt aussieht: `https://hooks.slack.com/services/aaaaaaaaa/bbbbbbbbb/cccccccccccccccccccccccc`. Der Webhook wird im nächsten Schritt benötigt.

2. Erstellen Sie eine Paketbindung mit Ihren Slack-Berechtigungsnachweisen, mit dem Kanal, an den gesendet werden soll, sowie mit dem Benutzernamen, unter dem gesendet werden soll.
  ```
  ibmcloud fn package bind /whisk.system/slack mySlack \
    --param url "https://hooks.slack.com/services/..." \
    --param username "Bob" \
    --param channel "#MySlackChannel"
  ```
  {: pre}

3. Rufen Sie die Aktion **post** in Ihrer Paketbindung auf, um eine Nachricht an Ihren Slack-Kanal zu senden.
  ```
  ibmcloud fn action invoke mySlack/post --blocking --result \
    --param text "Hello from OpenWhisk!"
  ```
  {: pre}

## Slack-Token-basierte API verwenden

Sie können außerdem auch die Slack-Token-basierte API anstelle der Webhook-API verwenden. Übergeben Sie in diesem Fall einen Parameter `token`, der Ihr [Slack-Zugriffstoken](https://api.slack.com/tokens) enthält. Sie können dann eine beliebige [Slack-API-Methode](https://api.slack.com/methods) als Parameter `url` verwenden. Um beispielsweise eine Nachricht zu senden, verwenden Sie den Parameterwert `url` von [slack.postMessage](https://api.slack.com/methods/chat.postMessage).
