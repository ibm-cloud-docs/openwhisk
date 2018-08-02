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

`/whisk.system/slack` 패키지는 [Slack API](https://api.slack.com/)를 사용하는 편리한 방법을 제공합니다.
{: shortdesc}

패키지에는 다음 액션이 포함됩니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/whisk.system/slack` |패키지 |url, channel, username |Slack API와 상호작용 |
|`/whisk.system/slack/post` |액션 |text, url, channel, username |Slack 채널에 메시지 게시 |

`username`, `url` 및 `channel` 값으로 패키지 바인딩을 작성하도록 권장합니다. 바인딩을 사용하면, 패키지에서 액션을 호출할 때마다 값을 지정할 필요가 없습니다.

## Slack 채널에 메시지 게시

`/whisk.system/slack/post` 액션은 메시지를 지정된 Slack 채널에 게시합니다. 매개변수는 다음과 같습니다.

- `url`: Slack 웹훅 URL입니다.
- `channel`: 메시지가 게시되는 Slack 채널입니다.
- `username`: 메시지가 게시되는 이름입니다.
- `text`: 게시할 메시지입니다.
- `token`: (선택사항) Slack [액세스 토큰](https://api.slack.com/tokens)입니다.

다음 예제는 Slack을 구성하고 패키지 바인딩을 작성하며 메시지를 채널에 게시하는 방법을 보여줍니다.

1. 팀에 대한 Slack [수신 웹훅](https://api.slack.com/incoming-webhooks)을 구성하십시오.

  Slack이 구성된 후에는 `https://hooks.slack.com/services/aaaaaaaaa/bbbbbbbbb/cccccccccccccccccccccccc` 같이 표시되는 웹훅 URL을 가져옵니다. 웹훅은 다음 단계에서 필요합니다.

2. Slack 신임 정보, 게시되는 채널 및 게시자의 사용자 이름을 사용하여 패키지 바인딩을 작성하십시오.
  ```
  ibmcloud fn package bind /whisk.system/slack mySlack \
    --param url "https://hooks.slack.com/services/..." \
    --param username "Bob" \
    --param channel "#MySlackChannel"
  ```
  {: pre}

3. 패키지 바인딩에서 **post** 액션을 호출하여 사용자의 Slack 채널에 메시지를 게시하십시오.
  ```
  ibmcloud fn action invoke mySlack/post --blocking --result \
    --param text "Hello from OpenWhisk!"
  ```
  {: pre}

## Slack 토큰 기반 API 사용

원하는 경우에는 웹훅 API 대신 Slack의 토큰 기반 API를 사용하도록 선택할 수 있습니다. 이를 선택하는 경우, Slack [액세스 토큰](https://api.slack.com/tokens)이 포함된 `token` 매개변수를 전달하십시오. 그리고 [Slack API 메소드](https://api.slack.com/methods)를 `url` 매개변수로서 사용할 수 있습니다. 예를 들어, 메시지를 게시하기 위해 [slack.postMessage](https://api.slack.com/methods/chat.postMessage)의 `url` 매개변수값을 사용할 수 있습니다.
