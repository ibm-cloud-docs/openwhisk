---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-29"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Slack
{: #openwhisk_catalog_slack}

`/whisk.system/slack` 套件提供一種簡便的方式來使用 [Slack API](https://api.slack.com/)。
{: shortdesc}

該套件包含下列動作：

|實體|類型|參數|說明
|
| --- | --- | --- | --- |
| `/whisk.system/slack` |套件|url、channel、username|與 Slack API 互動|
|`/whisk.system/slack/post` |動作|text、url、channel、username|將訊息張貼至 Slack 頻道|

建議使用 `username`、`url` 及 `channel` 值來建立套件連結。使用連結，您就不需要每次在呼叫套件中的動作時都指定值。

## 將訊息張貼至 Slack 頻道

`/whisk.system/slack/post` 動作會將訊息張貼至指定的 Slack 通道。參數如下所示：

- `url`：Slack Webhook URL。
- `channel`：要將訊息張貼至其中的 Slack 頻道。
- `username`：用來張貼訊息的名稱。
- `text`：要張貼的訊息。
- `token`：（選用）Slack [存取記號](https://api.slack.com/tokens)。

下列範例顯示如何配置 Slack、建立套件連結，以及將訊息張貼至頻道。

1. 針對您的團隊配置 Slack [送入的 Webhook](https://api.slack.com/incoming-webhooks)。

  配置 Slack 之後，您會得到與下列類似的 Webhook URL：`https://hooks.slack.com/services/aaaaaaaaa/bbbbbbbbb/cccccccccccccccccccccccc`。下一個步驟需要 Webhook。

2. 使用 Slack 認證、要張貼至其中的頻道，以及用來進行張貼的使用者名稱，來建立套件連結。
  ```
  ibmcloud fn package bind /whisk.system/slack mySlack \
    --param url "https://hooks.slack.com/services/..." \
    --param username "Bob" \
    --param channel "#MySlackChannel"
  ```
  {: pre}

3. 在套件連結中呼叫 **post** 動作，以將訊息張貼至 Slack 通道。
  ```
  ibmcloud fn action invoke mySlack/post --blocking --result \
    --param text "Hello from OpenWhisk!"
  ```
  {: pre}

## 使用以 Slack 記號為基礎的 API

如果您想要的話，可以選擇使用以 Slack 記號為基礎的 API，而不是 Webhook API。如果您選擇這麼做，則請傳入包含 Slack [存取記號](https://api.slack.com/tokens)的 `token` 參數。然後，您可以使用任何 [Slack API 方法](https://api.slack.com/methods)作為 `url` 參數。例如，若要張貼訊息，您將使用 `url` 參數值 [slack.postMessage](https://api.slack.com/methods/chat.postMessage)。
