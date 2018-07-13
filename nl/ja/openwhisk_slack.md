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

`/whisk.system/slack` パッケージを利用して、[Slack API](https://api.slack.com/) を簡単に使用することができます。
{: shortdesc}

このパッケージには、以下のアクションが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/slack` | パッケージ | url、channel、username | Slack API と対話 |
| `/whisk.system/slack/post` | アクション | text、url、channel、username | Slack チャネルへメッセージをポスト |

`username`、`url`、および `channel` の各値を使用してパッケージ・バインディングを作成することをお勧めします。 バインディングを使用すると、パッケージ内のアクションを起動するたびに値を指定する必要はありません。

## Slack チャネルへのメッセージのポスト

`/whisk.system/slack/post` アクションは、指定された Slack チャネルにメッセージをポストします。 パラメーターは次のとおりです。

- `url`: Slack の Webhook URL。
- `channel`: メッセージのポスト先の Slack チャネル。
- `username`: メッセージをポストするユーザーの名前。
- `text`: ポストするメッセージ。
- `token`: (オプション) Slack [アクセス・トークン](https://api.slack.com/tokens)。

以下の例は、Slack を構成し、パッケージ・バインディングを作成し、チャネルにメッセージをポストする方法を示しています。

1. チームに Slack の [Incoming Webhook](https://api.slack.com/incoming-webhooks) を構成します。

  Slack の構成後、`https://hooks.slack.com/services/aaaaaaaaa/bbbbbbbbb/cccccccccccccccccccccccc` のような Webhook URL を取得します。 この Webhook は次のステップで必要になります。

2. Slack の資格情報、ポスト先のチャネル、およびポストするユーザー名を指定して、パッケージ・バインディングを作成します。
  ```
  ibmcloud wsk package bind /whisk.system/slack mySlack \
    --param url "https://hooks.slack.com/services/..." \
    --param username "Bob" \
    --param channel "#MySlackChannel"
  ```
  {: pre}

3. パッケージ・バインディングで **post** アクションを呼び出して、メッセージを Slack チャネルにポストします。
  ```
  ibmcloud wsk action invoke mySlack/post --blocking --result \
    --param text "Hello from OpenWhisk!"
  ```
  {: pre}

## Slack トークン・ベースの API の使用

必要に応じて、Webhook API ではなく、Slack のトークン・ベース API を使用することを選択できます。 そうするよう選択した場合、Slack [アクセス・トークン](https://api.slack.com/tokens)を含んでいる `token` パラメーターを渡します。 次に、任意の [Slack API メソッド](https://api.slack.com/methods)を `url` パラメーターとして使用できます。 例えば、メッセージをポストするために、`url` パラメーター値 [slack.postMessage](https://api.slack.com/methods/chat.postMessage) を使用します。
