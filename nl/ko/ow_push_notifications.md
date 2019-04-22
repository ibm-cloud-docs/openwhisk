---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: push notifications, functions, webhooks

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.mobilepushshort}} 패키지 

{{site.data.keyword.mobilepushshort}} 패키지는 {{site.data.keyword.mobilepushfull}} 서비스 인스턴스와의 상호작용을 위한 액션 세트를 제공합니다. 이러한 액션을 통해 사용자는 메시지를 보내거나 웹훅을 작성, 업데이트 또는 삭제할 수 있습니다.
{: shortdesc}

{{site.data.keyword.mobilepushshort}} 패키지에는 다음 액션이 포함되어 있습니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/push-notifications` |패키지 | apikey, appGuid | {{site.data.keyword.mobilepushshort}} 인스턴스 관련 작업을 수행합니다. |
| `/push-notifications/send-message` |액션 |text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | 하나 이상의 지정된 디바이스에 푸시 알림을 전송합니다. |
| `/push-notifications/webhook` |액션 |events | 푸시 서비스에서 디바이스 활동(디바이스 등록, 등록 해제, 구독 또는 구독 해제)의 트리거 이벤트를 실행합니다. |

## {{site.data.keyword.mobilepushshort}} 서비스 인스턴스 작성
{: #service_instance_push}

패키지를 설치하기 전에 {{site.data.keyword.mobilepushshort}} 인스턴스를 작성해야 합니다. 

1. [{{site.data.keyword.mobilepushshort}} 서비스 인스턴스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")을 작성하십시오](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a).

2. Push Notificiations 서비스 인스턴스에 대해 [서비스 인증 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 작성하십시오](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1).

3. [{{site.data.keyword.mobilepushshort}} 서비스 인스턴스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 구성하십시오](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2).

## {{site.data.keyword.mobilepushshort}} 패키지 설치
{: #install_push}

{{site.data.keyword.mobilepushshort}} 서비스 인스턴스가 보유되면 {{site.data.keyword.openwhisk}} CLI 또는 UI를 사용하여 네임스페이스에 {{site.data.keyword.mobilepushshort}} 패키지를 설치하십시오. 

### {{site.data.keyword.openwhisk_short}} CLI에서 설치
{: #install_cli}

시작하기 전에 다음을 수행하십시오.
  1. [{{site.data.keyword.Bluemix_notm}} CLI용 {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

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
{: #install_ui}

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [작성 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/openwhisk/create)로 이동하십시오.

2. 오른쪽 상단에 있는 네임스페이스 전환기를 사용하여 {{site.data.keyword.cos_short}} 패키지를 설치할 네임스페이스를 선택하십시오.

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

## {{site.data.keyword.mobilepushshort}} 패키지 사용
{: #usage_push}

### 푸시 알림 보내기
{: #push_notif}

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

## 웹훅 작성
{: #webhook}

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
