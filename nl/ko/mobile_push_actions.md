---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: mobile, push notifications, binding, notifications

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 모바일 푸시 패키지
{: #openwhisk_catalog_pushnotifications}

이 사전 설치된 패키지는 도쿄 지역에서는 사용할 수 없습니다. IAM 인증을 사용하여 `sendMessage` 액션에 대해 설치 가능한 [푸시 알림](/docs/openwhisk?topic=cloud-functions-push-notifications-package) 패키지를 참조하십시오.
{: tip}

푸시 패키지 바인딩을 작성하는 방법을 알아보고 푸시 서비스에 대한 작업을 수행하는 기능을 제공하는 `/whisk.system/pushnotifications` 패키지를 사용하여 단순 푸시 알림을 전송하십시오.
{: shortdesc}

패키지에는 다음의 액션 및 피드가 포함됩니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` |패키지 | appId, appSecret, admin_url | 푸시 서비스 관련 작업을 수행합니다. |
|`/whisk.system/pushnotifications/sendMessage` |액션 |text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | 하나 이상의 지정된 디바이스에 푸시 알림을 전송합니다. |


디바이스 활동이 있는 경우 트리거 이벤트 실행에 대한 정보는 [디바이스 이벤트의 모바일 푸시](/docs/openwhisk?topic=cloud-functions-openwhisk_pushnotifications) 주제를 참조하십시오.

## 푸시 패키지 바인딩 작성
{: #create_push_binding}

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

## 푸시 알림 매개변수
{: #push_parameters}

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

## 푸시 알림 전송
{: #send_push_notifications}

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
