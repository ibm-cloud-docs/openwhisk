---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 디바이스 이벤트의 모바일 푸시
{: #openwhisk_pushnotifications}

지정된 애플리케이션에 디바이스 (등록/등록 해제) 또는 (구독/구독 해제)와 같은 디바이스 활동이 있는 경우 트리거를 실행하도록 푸시 서비스를 구성하는 방법을 알아보십시오.
{: shortdesc}

`/whisk.system/pushnotifications` 패키지 자체에 대한 정보는 푸시 패키지 바인딩 작성 및 푸시 알림 전송을 다루는 [모바일 푸시](./mobile_push_actions.html) 주제를 참조하십시오.

## 푸시 매개변수
{: #push_parameters}

`/whisk.system/pushnotifications/webhook` 매개변수는 다음과 같습니다.
- **appId:** {{site.data.keyword.Bluemix_notm}} 앱 GUID.
- **appSecret:** {{site.data.keyword.Bluemix_notm}} 푸시 알림 서비스 appSecret.
- **events:** _onDeviceRegister_, _onDeviceUnregister_, _onDeviceUpdate_, _onSubscribe_, _onUnsubscribe_

  와일드카드 문자 "`*`"를 사용하여 모든 이벤트에 대해 알림을 받을 수 있습니다.

## 푸시 알림 서비스 활동에서 트리거 이벤트 실행
{: #trigger_push_notify}

새 디바이스가 푸시 알림 서비스 애플리케이션에 등록될 때마다 실행되는 트리거를 작성하려면 다음 예제를 참조하십시오.

1. **appId** 및 **appSecret**을 사용하여 푸시 알림 서비스에 대해 구성된 패키지 바인딩을 작성하십시오.
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. `myPush/webhook` 피드를 사용하여 푸시 알림 서비스 `onDeviceRegister` 이벤트 유형에 대한 트리거를 작성하십시오.
  ```
  ibmcloud fn trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. 새 디바이스가 등록될 때마다 메시지를 전송하는 룰을 작성할 수 있습니다. 이전 액션 및 트리거를 사용하여 규칙을 작성하십시오.
  ```
  ibmcloud fn rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

4. `ibmcloud fn activation poll` 명령을 사용하여 결과를 확인하십시오.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

5. {{site.data.keyword.Bluemix_notm}} 애플리케이션에서 디바이스를 등록하십시오. `rule`, `trigger` 및 `action`이 {{site.data.keyword.openwhisk}} [대시보드](https://console.bluemix.net/openwhisk/dashboard)에서 실행됩니다.

  액션이 푸시 알림을 전송합니다.
