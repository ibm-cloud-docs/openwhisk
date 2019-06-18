---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: triggers, serverless

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


# 이벤트에 대한 트리거 작성
{: #triggers}

트리거는 사용자 또는 이벤트 소스에 의한 특정 유형의 이벤트에 반응하려는 선언입니다.
{: shortdesc}

다음은 트리거의 예입니다.
- 위치 업데이트 이벤트
- 웹 사이트로 문서 업로드
- 수신 이메일



## CLI에서 트리거 작성
{: #triggers_create}


1. 트리거를 작성하십시오. 트리거는 네임스페이스 내에서 직접 작성되어야 하며 패키지 내에서는 작성될 수 없습니다.
    ```
    ibmcloud fn trigger create TRIGGER_NAME
    ```
    {: pre}

    출력 예:
    ```
    ok: created trigger TRIGGER_NAME
    ```
    {: screen}

2. 트리거가 작성되었는지 확인하십시오.
    ```
    ibmcloud fn trigger list
    ```
    {: pre}

    출력 예:
    ```
    triggers
    /NAMESPACE/TRIGGER_NAME                            private
    ```
    {: screen}



그런 다음, [트리거를 테스트](/docs/openwhisk?topic=cloud-functions-test#test_triggers)하거나 [룰을 작성](/docs/openwhisk?topic=cloud-functions-rules)하여 트리거를 액션과 연관시킬 수 있습니다. 



## 피드 및 트리거 간의 차이점
{: #triggers_difference}

피드 및 트리거는 밀접하게 연관되어 있지만 기술적으로 다른 개념입니다.

- {{site.data.keyword.openwhisk_short}}는 시스템으로 유입되는 **이벤트**를 처리합니다.

- **트리거**는 이벤트 클래스의 이름입니다. 각 이벤트는 정확히 하나의 트리거에 속합니다. 유추하자면 트리거는 주제 기반 pub-sub 시스템의 주제와 유사합니다. **룰**은 트리거의 이벤트가 도착할 때마다 트리거 페이로드로 액션을 호출하는 것을 의미합니다. 

- **피드**는 {{site.data.keyword.openwhisk_short}}에서 이용할 수 있는 트리거 이벤트를 실행하기 위한 외부 이벤트 소스를 구성하는 편리한 방법입니다 피드는 모두가 일부 트리거에 속하는 이벤트의 스트림입니다. 사전 설치된 패키지, 설치 가능한 패키지 및 고유 사용자 정의 패키지에 피드가 포함될 수 있습니다. 피드는 피드를 구성하는 이벤트 스트림의 작성, 삭제, 일시정지 및 재개를 처리하는 **피드 액션**으로 제어됩니다. 일반적으로 피드 액션은 알림을 관리하는 REST API를 사용하여 이벤트를 생성하는 외부 서비스와 상호작용합니다.

피드 예:
- 데이터베이스의 문서가 추가되거나 수정될 때마다 트리거 이벤트를 실행하는 {{site.data.keyword.cloudant}} 데이터 변경 피드
- Git 저장소에 대한 모든 커미트에 대해 트리거 이벤트를 실행하는 Git 피드



## 피드에 대한 트리거 작성
{: #triggers_feeds}

이 예에서는 알람 패키지의 피드를 사용하여 매분마다 트리거를 실행하는 방법과, 매분마다 액션을 호출하기 위해 룰을 사용하는 방법을 보여줍니다.

1. `/whisk.system/alarms` 패키지에서 엔티티의 설명 목록을 가져오십시오.

    ```
  ibmcloud fn package get --summary /whisk.system/alarms
    ```
    {: pre}

출력 예:
    ```
      package /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
    ```
    {: screen}
2. 사용할 수 있는 매개변수를 보려면 `/whisk.system/alarms` 패키지에서 피드의 설명을 가져오십시오.

  ```
  ibmcloud fn action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  출력 예:
  ```
  action /whisk.system/alarms/alarm: Fire trigger when alarm occurs
     (params: cron trigger_payload)
  ```
  {: screen}

  `/whisk.system/alarms/alarm` 피드는 두 개의 매개변수를 취합니다.
  - `cron`: 트리거를 실행할 시기의 crontab 스펙입니다.
  - `trigger_payload`: 각 트리거 이벤트에서 설정할 페이로드 매개변수값입니다.

2. 매분마다 실행되는 트리거를 작성하십시오.
  ```
  ibmcloud fn trigger create everyOneMinute --feed /whisk.system/alarms/alarm -p cron "* * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  출력 예:
  ```
  ok: created trigger feed everyOneMinute
  ```
  {: screen}

3. 앱을 작성하십시오. `hello.js` 예는 다음과 같습니다. 
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. 액션을 작성하십시오.
  ```
    ibmcloud fn action create hello hello.js
  ```
  {: pre}

5. `everyOneMinute` 트리거가 실행될 때마다 `hello` 액션을 호출하는 룰을 작성하십시오.
  ```
  ibmcloud fn rule create myRule everyOneMinute hello
  ```
  {: pre}

  출력 예:
  ```
  ok: created rule myRule
  ```
  {: screen}

6. 활성화 로그에 대한 폴링에 의해 액션이 호출되는지 확인하십시오.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

  트리거, 룰 및 액션에 대해 활성화가 매분마다 발생함을 볼 수 있습니다. 액션은 각 호출에 대해 `{"name":"Mork", "place":"Ork"}` 매개변수를 수신합니다.


