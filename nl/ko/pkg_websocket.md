---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: websocket, functions, actions, package

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}

# WebSocket
{: #pkg_websocket}

사전 설치된 `/whisk.system/websocket` 패키지는 메시지를 WebSocket에 게시하는 편리한 방법을 제공합니다.
{: shortdesc}

패키지에는 다음 액션이 포함됩니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/whisk.system/websocket` |패키지 |uri |WebSocket과 통신하기 위한 유틸리티 |
|`/whisk.system/websocket/send` |액션 |uri, payload |WebSocket URI에 페이로드 전송 |

동일한 WebSocket URI에 많은 메시지를 전송하려는 경우에는 `uri` 값으로 패키지 바인딩을 작성하도록 권장합니다. 바인딩을 사용하면 `send` 액션을 사용할 때마다 값을 지정할 필요가 없습니다.

## WebSocket에 메시지 전송

`/whisk.system/websocket/send` 액션은 WebSocket URI로 페이로드를 전송합니다. 매개변수는 다음과 같습니다.

- `uri`: WebSocket 서버의 URI(예: ws://mywebsockethost:80)입니다.
- `payload`: WebSocket에 전송할 메시지입니다.

