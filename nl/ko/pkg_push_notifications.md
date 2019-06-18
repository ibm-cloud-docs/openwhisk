---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: push notifications, functions, webhooks

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

# 푸시 알림
{: #pkg_push_notifications}


## 패키지
{: #pkg_push_packages}

|패키지 |가용성 |설명 |
| --- | --- | --- |
| [`/whisk.system/pushnotifications`](#pkg_push_send) | 사전 설치됨(도쿄에서 사용할 수 없음) | 하나 이상의 지정된 디바이스에 푸시 알림을 전송합니다. |
| [`/whisk.system/pushnotifications/webhook`](#pkg_push_mobile) | 사전 설치됨(도쿄에서 사용할 수 없음) | 디바이스 이벤트의 모바일 푸시 알림을 전송합니다. |
| [`/push-notifications`](#pkg_push_mobile_send) | 설치 가능 | {{site.data.keyword.mobilepushfull}} 서비스 인스턴스와 상호작용합니다. 메시지를 보내거나 웹훅을 작성, 업데이트 또는 삭제할 수 있습니다.
|

## 푸시 알림 전송
{: #pkg_push_send}

사전 설치된 패키지는 도쿄 지역에서는 사용할 수 없습니다. IAM 인증을 사용하여 `sendMessage` 액션에 대해 설치 가능한 [푸시 알림](#pkg_push_mobile_send) 패키지를 참조하십시오.
{: tip}

푸시 알림 패키지 바인딩을 작성하는 방법을 알아보고 `/whisk.system/pushnotifications` 패키지를 사용하여 단순 푸시 알림을 전송하십시오.
{: shortdesc}

패키지에는 다음의 액션 및 피드가 포함됩니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` |패키지 | appId, appSecret, admin_url | 푸시 서비스 관련 작업을 수행합니다. |
|`/whisk.system/pushnotifications/sendMessage` |액션 |text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | 하나 이상의 지정된 디바이스에 푸시 알림을 전송합니다. |


디바이스 활동이 있는 경우 트리거 이벤트 실행에 대한 정보는 [디바이스 이벤트의 모바일 푸시](#pkg_push_mobile)를 참조하십시오.

### 푸시 패키지 바인딩 작성
{: #pkg_push_create}

푸시 알림 패키지 바인딩을 작성하려면 다음 매개변수를 지정해야 합니다.

-  **appId**: {{site.data.keyword.Bluemix}} **앱 GUID**.
-  **appSecret**: {{site.data.keyword.Bluemix_notm}} Push Notificiations 서비스 **앱 시크릿**.

패키지 바인딩을 작성하려면 다음 단계를 참조하십시오.

1. [{{site.data.keyword.Bluemix_notm}} 대시보드](http://cloud.ibm.com)에서 {{site.data.keyword.Bluemix_notm}} 애플리케이션을 작성하십시오.

2. Push Notificiations 서비스를 초기화하고 이 서비스를 {{site.data.keyword.Bluemix_notm}} 애플리케이션에 바인딩하십시오.

3. [푸시 알림 애플리케이션](/docs/services/mobilepush?topic=mobile-pushnotification-gettingstartedtemplate)을 구성하십시오.

  작성된 {{site.data.keyword.Bluemix_notm}} 앱의 **앱 GUID** 및 **앱 시크릿**을 반드시 기억하십시오.

4. `/whisk.system/pushnotifications`로 패키지 바인딩을 작성하십시오.
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myPush -p appId myAppID -p appSecret myAppSecret
  ```
  {: pre}

5. 패키지 바인딩이 있는지 확인하십시오.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  출력 예:
  ```
  packages
  /myNamespace/myPush private binding
  ```
  {: screen}

### 푸시 알림 매개변수
{: #pkg_push_params}

`/whisk.system/pushnotifications/sendMessage` 액션은 등록된 디바이스에 푸시 알림을 전송합니다. 매개변수는 다음과 같습니다.
- `text`: 사용자에게 표시될 알림 메시지입니다. 예: `-p text "Hi, OpenWhisk send a notification"`.
- `url`: 경보와 함께 전송될 수 있는 URL입니다. 예: `-p url "https:\\www.w3.ibm.com"`.
- `apiHost`: API 호스트를 지정하는 선택적 문자열입니다. 기본값은 `mobile.ng.bluemix.net`입니다.  예를 들어, `-p apiHost "mobile.eu-gb.bluemix.net"`입니다.
- `deviceIds`: 지정된 디바이스의 목록입니다. 예: `-p deviceIds ["deviceID1"]`.
- `platforms`: 지정된 플랫폼의 디바이스에 알림을 전송합니다. Apple(iOS) 디바이스의 경우에는 'A'이며, Google(Android) 디바이스의 경우에는 'G'입니다. 예: `-p platforms ["A"]`.
- `userIds`: 지정된 사용자의 디바이스에 알림을 전송합니다. 예: `-p userIds "[\"testUser\"]"`
- `tagNames`: 해당 태그를 구독하는 디바이스에 알림을 전송합니다. 예: `-p tagNames "[\"tag1\"]"`.
- `gcmCollapseKey`: 이 매개변수는 메시지의 그룹을 식별합니다.
- `gcmCategory`: 대화식 푸시 알림에 사용되는 카테고리 ID입니다.
- `gcmIcon`: 알림을 위해 표시되는 아이콘의 이름을 지정합니다. 아이콘이 클라이언트 애플리케이션에 이미 패키징되어 있는지 확인하십시오.
- `gcmDelayWhileIdle`: 이 매개변수가 true로 설정되면 디바이스가 활성화될 때까지 메시지가 전송됩니다.
- `gcmSync`: 디바이스 그룹 메시징은 그룹의 모든 앱 인스턴스가 최신 메시징 상태를 반영할 수 있도록 허용합니다.
- `gcmVisibility`: 사설/공용 - 보안 잠금 화면에서 알림이 나타나는 방법과 시점에 영향을 주는 이 알림의 가시성입니다.
- `gcmPayload`: 알림 메시지의 일부로서 전송된 사용자 정의 JSON 페이로드입니다. 예: `-p gcmPayload "{\"hi\":\"hello\"}"`
- `gcmPriority`: 메시지의 우선순위를 설정합니다.
- `gcmSound`: 알림이 디바이스에 도착하면 재생되는 (디바이스의) 사운드 파일입니다.
- `gcmTimeToLive`: 이 매개변수는 디바이스가 오프라인인 경우에 메시지가 GCM 스토리지에 보관되는 기간(초)을 지정합니다.
- `gcmStyleType`: 확장 가능한 알림의 유형을 지정합니다. 가능한 값은 `bigtext_notification`, `picture_notification` 및 `inbox_notification`입니다.
- `gcmStyleTitle`: 알림의 제목을 지정합니다. 제목은 알림이 확장될 때 표시됩니다. 3개의 모든 확장 가능 알림에 대해 제목을 지정해야 합니다.
- `gcmStyleUrl`: 알림에 대해 그림을 가져와야 하는 URL입니다. `picture_notification`에 대해 지정되어야 합니다.
- `gcmStyleText`: `bigtext_notification` 확장 시에 표시되어야 하는 대형 텍스트입니다. `bigtext_notification`에 대해 지정되어야 합니다.
- `gcmStyleLines`: `inbox_notification`에 대해 받은 편지함 스타일로 표시되는 문자열의 배열입니다. `inbox_notification`에 대해 지정되어야 합니다.
- `gcmLightsLedArgb`: LED 색상입니다. 하드웨어는 최상의 추정을 수행합니다.
- `gcmLightsLedOnMs`: 깜박이는 동안 LED가 켜지는 시간(밀리초)입니다. 하드웨어는 최상의 추정을 수행합니다.
- `gcmLightsLedOffMs`: 깜박이는 동안 LED가 꺼지는 시간(밀리초)입니다. 하드웨어는 최상의 추정을 수행합니다.
- `apnsBadge`: 애플리케이션 아이콘의 배지로서 표시할 숫자입니다.
- `apnsCategory`: 대화식 푸시 알림에 사용되는 카테고리 ID입니다.
- `apnsIosActionKey`: 액션 키의 제목입니다.
- `apnsPayload`: 알림 메시지의 일부로서 전송된 사용자 정의 JSON 페이로드입니다.
- `apnsType`: ['DEFAULT', 'MIXED', 'SILENT'].
- `apnsSound`: 애플리케이션 번들에서 사운드 파일의 이름입니다. 이 파일의 사운드는 경보로서 재생됩니다.
- `apnsTitleLocKey`: 현재 현지화에 대해 `Localizable.strings` 파일의 제목 문자열에 대한 키입니다. `titleLocArgs` 배열에 지정된 변수를 취하기 위해 키 문자열은 %@ 및 %n$@ 지정자로 형식화될 수 있습니다.
- `apnsLocKey`: (사용자의 언어 환경 설정에 의해 설정된) 현재 현지화에 대한 `Localizable.strings` 파일의 alert-message 문자열에 대한 키입니다. locArgs 배열에 지정된 변수를 취하기 위해 키 문자열은 %@ 및 %n$@ 지정자로 형식화될 수 있습니다.
- `apnsLaunchImage`: 파일 이름 확장자와 무관한 앱 번들에서 이미지 파일의 파일 이름입니다. 이 이미지는 사용자가 액션 단추를 탭하거나 액션 슬라이더를 이동할 때 실행 이미지로 사용됩니다.
- `pnsTitleLocArgs`: `title-loc-key`에서 형식 지정자 대신 나타나는 변수 문자열 값입니다.
- `apnsLocArgs`: `locKey`에서 형식 지정자 대신 나타나는 변수 문자열 값입니다.
- `apnstitle`: 리치 푸시 알림의 제목입니다(iOS 10 이상에서만 지원됨).
- `apnsSubtitle`: 리치 알림의 하위 제목입니다. (iOS 10 이상에서만 지원됨).
- `apnsAttachmentUrl`: iOS 알림 미디어에 대한 링크입니다(동영상, 오디오, GIF 및 이미지 - iOS 10 이상에서만 지원됨).
- `fireFoxTitle`: WebPush 알림에 대해 설정되는 제목을 지정합니다.
- `fireFoxIconUrl`: WebPush 알림에 대해 설정되는 아이콘의 URL입니다.
- `fireFoxTimeToLive`: 이 매개변수는 디바이스가 오프라인인 경우에 메시지가 GCM 스토리지에 보관되는 기간(초)을 지정합니다.
- `fireFoxPayload`: 알림 메시지의 일부로서 전송된 사용자 정의 JSON 페이로드입니다.
- `chromeTitle`: WebPush 알림에 대해 설정되는 제목을 지정합니다.
- `chromeIconUrl`: WebPush 알림에 대해 설정되는 아이콘의 URL입니다.
- `chromeTimeToLive`: 이 매개변수는 디바이스가 오프라인인 경우에 메시지가 GCM 스토리지에 보관되는 기간(초)을 지정합니다.
- `chromePayload`: 알림 메시지의 일부로서 전송된 사용자 정의 JSON 페이로드입니다.
- `safariTitle`: Safari 푸시 알림에 대해 설정되는 제목을 지정합니다.
- `safariUrlArgs`: 이 알림에서 사용되어야 하는 URL 인수입니다. 이러한 인수는 JSON 배열의 양식으로 제공됩니다.
- `safariAction`: 액션 단추의 레이블입니다.
- `chromeAppExtTitle`: WebPush 알림에 대해 설정되는 제목을 지정합니다.
- `chromeAppExtCollapseKey`: 이 매개변수는 메시지의 그룹을 식별합니다.
- `chromeAppExtDelayWhileIdle`: 이 매개변수가 true로 설정된 경우, 이는 디바이스가 활성화될 때까지 메시지가 전송되지 않음을 표시합니다.
- `chromeAppExtIconUrl`: WebPush 알림에 대해 설정되는 아이콘의 URL입니다.
- `chromeAppExtTimeToLive`: 이 매개변수는 디바이스가 오프라인인 경우에 메시지가 GCM 스토리지에 보관되는 기간(초)을 지정합니다.
- `chromeAppExtPayload`: 알림 메시지의 일부로서 전송된 사용자 정의 JSON 페이로드입니다.

### 푸시 알림 예
{: #pkg_push_ex}

푸시 알림 패키지에서 푸시 알림을 전송하려면 다음 예제를 참조하십시오.

이전에 작성한 패키지 바인딩의 **sendMessage** 액션을 사용하여 푸시 알림을 전송하십시오. 반드시 `/myNamespace/myPush`를 패키지 이름으로 대체하십시오.
```
ibmcloud fn action invoke /myNamespace/myPush/sendMessage --blocking --result -p url https://example.com -p text "this is my message" -p sound soundFileName -p deviceIds "[\"T1\",\"T2\"]"
```
{: pre}

출력 예:
```
{
  "result": {
  "pushResponse":
    {
      "messageId":"11111H",
      "message":{
        "alert":"this is my message",
        "url":""
      },
      "settings":{
        "apns":{
          "sound":"default"
        },
        "gcm":{
          "sound":"default"
          },
        "target":{
          "deviceIds":["T1","T2"]
        }
      }
    }
  },
  "status": "success",
  "success": true
}
```
{: screen}

## 모바일 디바이스 이벤트의 푸시 알림 전송
{: #pkg_push_mobile}

이 사전 설치된 패키지는 도쿄 지역에서는 사용할 수 없습니다.
{: tip}

지정된 애플리케이션에 디바이스 (등록/등록 해제) 또는 (구독/구독 해제)와 같은 디바이스 활동이 있는 경우 트리거를 실행하도록 푸시 알림 서비스를 구성하는 방법을 알아보십시오.
{: shortdesc}

### 매개변수
{: #pkg_push_mobile_params}

`/whisk.system/pushnotifications/webhook` 매개변수는 다음과 같습니다.
- `appId`: {{site.data.keyword.Bluemix_notm}} 앱 GUID입니다.
- `appSecret`: {{site.data.keyword.Bluemix_notm}} Push Notificiations 서비스 `appSecret`입니다.
- `events`: `onDeviceRegister`, `onDeviceUnregister`, `onDeviceUpdate`, `onSubscribe`, `onUnsubscribe`

  와일드카드 문자 "`*`"를 사용하여 모든 이벤트에 대해 알림을 받을 수 있습니다.

### Push Notificiations 서비스 활동에서 트리거 이벤트 실행
{: #pkg_push_mobile_trigger}

새 디바이스가 Push Notificiations 서비스 애플리케이션에 등록될 때마다 실행되는 트리거를 작성하려면 다음 예제를 참조하십시오.

1. `appId` 및 `appSecret`을 사용하여 Push Notificiations 서비스에 대해 구성된 패키지 바인딩을 작성하십시오.
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. `myPush/webhook` 피드를 사용하여 Push Notificiations 서비스 `onDeviceRegister` 이벤트 유형에 대한 트리거를 작성하십시오.
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

5. {{site.data.keyword.Bluemix_notm}} 애플리케이션에서 디바이스를 등록하십시오. `rule`, `trigger` 및 `action`이 {{site.data.keyword.openwhisk}} [대시보드](https://cloud.ibm.com/openwhisk/dashboard)에서 실행됩니다.

  액션이 푸시 알림을 전송합니다.


## 푸시 알림 전송 또는 웹훅의 작성, 업데이트 및 삭제
{: #pkg_push_mobile_send}

설치 가능한 {{site.data.keyword.mobilepushshort}} 패키지는 {{site.data.keyword.mobilepushfull}} 서비스 인스턴스와의 상호작용을 위한 액션 세트를 제공합니다. 이러한 액션을 통해 사용자는 메시지를 보내거나 웹훅을 작성, 업데이트 또는 삭제할 수 있습니다.
{: shortdesc}

{{site.data.keyword.mobilepushshort}} 패키지에는 다음 액션이 포함되어 있습니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/push-notifications` |패키지 | apikey, appGuid | {{site.data.keyword.mobilepushshort}} 인스턴스 관련 작업을 수행합니다. |
| `/push-notifications/send-message` |액션 |text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | 하나 이상의 지정된 디바이스에 푸시 알림을 전송합니다. |
| `/push-notifications/webhook` |액션 |events | 푸시 서비스에서 디바이스 활동(디바이스 등록, 등록 해제, 구독 또는 구독 해제)의 트리거 이벤트를 실행합니다. |

### {{site.data.keyword.mobilepushshort}} 서비스 인스턴스 작성
{: #service_instance_push}

패키지를 설치하기 전에 {{site.data.keyword.mobilepushshort}} 인스턴스를 작성해야 합니다.

1. [{{site.data.keyword.mobilepushshort}} 서비스 인스턴스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")을 작성하십시오](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a).

2. 푸시 알림 서비스 인스턴스에 대해 [서비스 인증 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 작성하십시오](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1).

3. [{{site.data.keyword.mobilepushshort}} 서비스 인스턴스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 구성하십시오](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2).

### {{site.data.keyword.mobilepushshort}} 패키지 설치
{: #pkg_push_mobile_install}

{{site.data.keyword.mobilepushshort}} 서비스 인스턴스가 보유되면 {{site.data.keyword.openwhisk}} CLI 또는 UI를 사용하여 네임스페이스에 {{site.data.keyword.mobilepushshort}} 패키지를 설치하십시오.

### {{site.data.keyword.openwhisk_short}} CLI에서 설치
{: #pkg_push_mobile_cli}

시작하기 전에 다음을 수행하십시오.
  1. [{{site.data.keyword.Bluemix_notm}} CLI용 {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오](/docs/openwhisk?topic=cloud-functions-cli_install).

{{site.data.keyword.mobilepushshort}} 패키지를 설치하려면 다음을 수행하십시오.

1. {{site.data.keyword.mobilepushshort}} 패키지 저장소를 복제하십시오.
    ```
    git clone https://github.com/ibm-functions/package-push-notifications.git
    ```
    {: pre}

2. `runtimes/nodejs` 디렉토리로 이동하십시오.
    ```
    cd package-push-notifications/runtimes/nodejs
    ```
    {: pre}

3. 패키지를 배치하십시오.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. `push-notifications` 패키지가 패키지 목록에 추가되었는지 확인하십시오.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    출력:
    ```
    packages
    /myOrg_mySpace/push-notifications private
    ```
    {: screen}

5. 작성한 {{site.data.keyword.mobilepushshort}} 서비스 인스턴스의 인증 정보를 패키지에 바인딩하십시오.
    ```
    ibmcloud fn service bind imfpush push-notifications
    ```
    {: pre}

    출력 예:
    ```
    Credentials 'Credentials-1' from 'imfpush' service instance 'Push-Notifications-r1' bound to 'push-notifications'.
    ```
    {: screen}

6. 패키지가 {{site.data.keyword.mobilepushshort}} 서비스 인스턴스 인증 정보로 구성되었는지 확인하십시오.
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/push-notifications parameters
    ```
    {: pre}

    출력 예:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/push-notifications, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "imfpush": {
            "admin_url": "https://mobile.ng.bluemix.net/imfpushdashboard/?appGuid=12345a-a123-1234-ab12-1ba1234567",
            "apikey": "abcd1234abcd1234abcd1234",
            "appGuid": "12341-12345-1234-a1234-1abcd12345",
            "clientSecret": "1b1234ab-1234-1234-123a-ab12345abcd",
            "credentials": "Service credentials-1",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:imfpush:us-south:a/abcd1234abcd1234:abcd1234-abcd-1234-abcd1234::",
            "iam_apikey_name": "auto-generated-apikey-abcd1234abcd1234abcd1234",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/1234abcd1234abcd::serviceid:ServiceId-12345678-1234-12ab-abc1-1234abcd1234abcd",
            "instance": "Push Notifications-ab",
            "plan": "LITE",
            "url": "https://imfpush.ng.bluemix.net/imfpush/v1/apps/1234abcd-1234-abcd-1234"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI에서 설치
{: #pkg_push_mobile_ui}

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [작성 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/openwhisk/create)로 이동하십시오.

2. 오른쪽 상단에 있는 네임스페이스 전환기를 사용하여 {{site.data.keyword.cos_full_notm}} 패키지를 설치할 네임스페이스를 선택하십시오.

3. **패키지 설치**를 클릭하십시오.

4. **IBM {{site.data.keyword.mobilepushshort}}** 패키지 그룹을 클릭한 후 **IBM {{site.data.keyword.mobilepushshort}}** 패키지를 클릭하십시오.

5. 사용 가능한 런타임 섹션의 드롭 다운 목록에서 NodeJS를 선택한 후에 **설치**를 클릭하십시오.

6. 일단 패키지가 설치되면 사용자는 액션 페이지로 경로 재지정되며, 이름이 **push-notifications**인 새 패키지를 검색할 수 있습니다.

7. **push-notifications** 패키지의 액션을 사용하려면 서비스 인증 정보를 액션에 바인딩해야 합니다.
  * 서비스 인증 정보를 패키지의 모든 액션에 바인딩하려면 위에 나열된 CLI 지시사항의 5 - 6단계를 수행하십시오.
  * 서비스 인증 정보를 개별 액션에 바인딩하려면 UI에서 다음 단계를 완료하십시오. **참고**: 사용할 각 액션마다 다음 단계를 완료해야 합니다.
    1. 사용할 **push-notifications** 패키지에서 액션을 클릭하십시오. 해당 액션에 대한 세부사항 페이지가 열립니다.
    2. 왼쪽 탐색 창에서 **매개변수** 섹션을 클릭하십시오.
    3. 새 **매개변수**를 입력하십시오. 키에 대해 `__bx_creds`를 입력하십시오. 값에 대해 이전에 작성한 서비스 인스턴스의 서비스 인증 정보 JSON 오브젝트를 붙여넣으십시오.

### 푸시 알림 보내기
{: #pkg_push_mobile_sendmsg}

`send-message` 액션을 사용하여 Push Notificiations 서비스를 통해 메시지를 보내려면 다음을 수행하십시오.
```
ibmcloud fn action invoke push-notifications/send-message --blocking --result --param messageText "Let's code something" --param messageUrl "http://developer.ibm.com"
```
{: pre}

출력 예:
```
{
  "response": {
      "result": {
          "message": {
              "alert": "let's code something",
              "url": "http://developer.ibm.com"
          },
          "messageId": "fLyql2tx"
      },
      "status": "success",
      "success": true
  },
}
```
{: screen}

### 웹훅 작성
{: #pkg_push_mobile_hook}

onDeviceRegister 이벤트에 대한 {{site.data.keyword.mobilepushshort}} 서비스의 웹훅을 작성하려면 다음을 수행하십시오.

```
 ibmcloud fn action invoke push-notifications/webhook --blocking --param triggerName "/myPackage/myTrigger" --param events onDeviceRegister
```
{: pre}

출력 예:
```
{
  "response": {
    "result": {
      "error": {}
    },
  "status": "application error",
  "success": false
  },
}
```
{: screen}

