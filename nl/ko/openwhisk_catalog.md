---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 사전 설치된 패키지
{: #openwhisk_ecosystem}

{{site.data.keyword.openwhisk}}에서 패키지의 카탈로그는 유용한 기능으로 앱을 개선하고 에코시스템의 외부 서비스에 액세스하기 위한 손쉬운 방법을 제공합니다. {{site.data.keyword.openwhisk_short}} 사용 외부 서비스의 예에는 Cloudant, Message Hub, Watson, The Weather Company, Slack 및 GitHub 등이 포함됩니다.
{: shortdesc}

카탈로그는 `/whisk.system` 및 `/watson-iot` 네임스페이스에서 패키지로서 사용될 수 있습니다. 자세한 정보는 [패키지 찾아보기](openwhisk_packages.html#browse-packages)를 참조하십시오. 

## 카탈로그 패키지
{: notoc}

| 패키지 | 설명 |
| --- | --- |
| [/whisk.system/alarms](./openwhisk_alarms.html) | 주기적 트리거를 작성하기 위한 패키지 |
| [/whisk.system/cloudant](./openwhisk_cloudant.html) | [Cloudant noSQL DB](https://console.ng.bluemix.net/docs/services/Cloudant/index.html) 서비스 관련 작업을 위한 패키지 |
| [/whisk.system/github](./openwhisk_github.html) | [GitHub](https://developer.github.com/)의 웹훅 트리거를 작성하기 위한 패키지 |
| [/whisk.system/messaging](./openwhisk_messagehub.html) | [Message Hub](https://console.ng.bluemix.net/docs/services/MessageHub/index.html) 서비스 관련 작업을 위한 패키지 |
| [/whisk.system/pushnotifications](./openwhisk_pushnotifications.html) | [푸시 알림](https://console.ng.bluemix.net/docs/services/mobilepush/index.html) 서비스 관련 작업을 위한 패키지  |
| [/whisk.system/slack](./openwhisk_slack.html) | [Slack API](https://api.slack.com/)에 게시하기 위한 패키지 |
| [/whisk.system/watson-translator](./openwhisk_watson_translator.html) | [텍스트 변환 및 언어 식별](https://www.ibm.com/watson/developercloud/language-translator.html)을 위한 패키지 |
| [/whisk.system/watson-speechToText](./openwhisk_watson_speechtotext.html) | [음성을 텍스트로](https://www.ibm.com/watson/developercloud/speech-to-text.html) 변환하기 위한 패키지 |
| [/whisk.system/watson-textToSpeech](./openwhisk_watson_texttospeech.html) | [텍스트를 음성으로](https://www.ibm.com/watson/developercloud/text-to-speech.html) 변환하기 위한 패키지 |
| [/watson-iot/iot-gateway](https://console.stage1.bluemix.net/docs/services/IoT/gateways/iotgw.html) | [Watson IoT 플랫폼 게이트웨이](https://console.stage1.bluemix.net/docs/services/IoT/index.html) 서비스 관련 작업을 위한 패키지 |
| [/whisk.system/weather](./openwhisk_weather.html) | [Weather Company Data](https://console.ng.bluemix.net/docs/services/Weather/index.html) 서비스 관련 작업을 위한 패키지 |
| [/whisk.system/websocket](./openwhisk_websocket.html) | [웹 소켓](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API) 서버 관련 작업을 위한 패키지 |
