---

copyright:

  years: 2018

lastupdated: "2018-05-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# 빠른 시작 템플리트 배치
{: #serviceauth}

{{site.data.keyword.openwhisk}}는 다음 프로젝트에서 빠른 시작에 도움을 주는 템플리트의 카탈로그를 제공합니다. 템플리트는 액션, 트리거, 시퀀스의 조합이며 {{site.data.keyword.Bluemix}}의 서비스 인스턴스를 통합할 수도 있습니다. 템플리트를 사용하면 빠르고 손쉽게 프로젝트를 작성하고 즉시 코딩을 시작할 수 있습니다.

이 튜토리얼은 Cloudant 템플리트 배치를 안내합니다.
{: shortdesc}

## 사용 가능한 빠른 시작 템플리트
{: #available-templates}

|이름 |설명 |지원되는 런타임 |
|:-----------------|:-----------------|:-----------------|
| [Cloudant 이벤트](./deploy_templates.html#cloudant-template) |Cloudant DB에 편집되거나 추가된 문서가 있으면 콘솔에서 변경사항을 로깅합니다. |Node.js, Swift, Python, PHP |
| [HTTP 리소스 가져오기](./deploy_templates.html#get-http-resource-template) | HTTP 이벤트에 대한 응답으로 호출된 후에 Yahoo Weather API에서 데이터를 페치하는 웹 액션입니다. |Node.js, Python |
| [Hello World](./deploy_templates.html#hello-world-template) | 이 액션은 단일 매개변수(JSON 오브젝트여야 함)를 허용합니다. |Node.js, Swift, Python, PHP |
| [Message Hub 이벤트](./deploy_templates.html#messagehub-events-template) |Message Hub 주제에 새 데이터가 추가되면 콘솔에서 변경사항을 로깅합니다. |Node.js, Swift, Python, PHP |
| [주기적 Slack 리마인더](./deploy_templates.html#slack-reminder-template) | 주기적 트리거를 기반으로 Slack에 게시할 액션입니다. |Node.js, Swift, Python, PHP |

## Cloudant 이벤트 템플리트 배치
{: #cloudant-template}

Cloudant 템플리트는 액션 시퀀스 및 해당 시퀀스를 시작하는 트리거를 작성합니다. 트리거는 이름과 색상이 있는 고양이의 데이터베이스여야 하는 연결된 Cloudant DB에 변경사항이 있을 때 실행됩니다. 예상되는 데이터 항목은 이름과 색상이 정의된 고양이입니다. 새 고양이가 데이터베이스에 추가되거나 현재 고양이가 편집되면 해당 데이터가 콘솔에 로깅됩니다.

1. 템플리트를 작성하려면 [{{site.data.keyword.Bluemix_notm}}의 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)로 이동한 후에 **작성 시작**을 클릭하십시오.

2. **빠른 시작 템플리트**를 클릭하십시오.

3. **새 Cloudant 항목**을 클릭하십시오.

### Cloudant 액션 작성

1. 그 다음에는 패키지의 이름을 제공하거나 제공된 기본 이름 `new-cloudant-item`을 사용하십시오.

2. **액션** 드롭 다운 아래에서 소유할 액션에 대한 런타임을 선택하십시오(nodejs, swift, python 또는 php). 이 예제의 경우에는 **nodejs**를 선택하고 **다음**을 클릭하십시오.

### Cloudant 트리거 작성

트리거는 이벤트 소스에서 이벤트를 수신할 때 액션을 호출합니다. Cloudant 템플리트에 대한 트리거를 작성하려면 필수 Cloudant 서비스 인스턴스 정보와 함께 트리거를 제공하십시오.

#### Cloudant 서비스 인스턴스 작성

다음 중 하나를 선택할 수 있습니다.
  * **자체 인스턴스 작성**
  * **자체 신임 정보 입력**

1. 이 예제의 경우에는 **자체 인스턴스 작성**을 선택하십시오.

2. Cloudant 설정 페이지의 새 탭으로 안내하기 위해 팝업이 열립니다. 일단 Cloudant 인스턴스가 작성되면 서비스 신임 정보 세트를 작성한 후에 **확인**을 클릭하여 탭을 닫고 이 페이지로 돌아와야 합니다.

3. 이제 **자체 신임 정보 입력**을 선택하고 다음 정보를 제공하십시오.
  * 사용자 이름 - _Cloudant 사용자 이름_
  * 비밀번호 - _Cloudant 비밀번호_
  * 호스트 - _일반적으로 `username.cloudant.com`_
  * 데이터베이스 - _Cloudant 데이터베이스의 이름_

### Cloudant 템플리트 배치

**배치**를 클릭하십시오.

템플리트 배치 이후에는 코드를 추가로 편집하여 필요한 대로 이를 사용자 정의하거나 되돌아가서 사용 가능한 템플리트의 카탈로그를 확인할 수 있습니다.

## HTTP 리소스 가져오기 템플리트 배치
{: #get-http-resource-template}

HTTP 리소스 가져오기 템플리트는 외부 리소스인 Yahoo Weather API를 페치하는 액션을 작성한 다음 데이터를 리턴합니다. 액션은 CORS가 사용되고 인증 키가 필요 없는 URL로 호출할 수 있게 하는 웹 액션으로 사용되며 웹 애플리케이션의 백엔드 빌드에 유용합니다. **참고**: 기본적으로 `get-http-resource` 엔드포인트는 호출하려는 모든 사용자가 공개적으로 사용할 수 있습니다.

1. 템플리트를 작성하려면 [{{site.data.keyword.Bluemix_notm}}의 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)로 이동한 후에 **작성 시작**을 클릭하십시오.

2. **빠른 시작 템플리트**를 클릭하십시오.

3. **패키지 이름** 필드를 검토하십시오. 필요에 따라 이를 업데이트할 수 있습니다. 기본값은 `get-http-resource`로 설정됩니다.

4. 소유할 액션에 대한 런타임을 선택하십시오(Node.js 8, Node.js 6 또는 Python 3).

5. **배치**를 클릭하십시오.

템플리트 배치 이후에는 코드를 추가로 편집하여 필요한 대로 이를 사용자 정의하거나 되돌아가서 사용 가능한 템플리트의 카탈로그를 확인할 수 있습니다.

## Hello World 템플리트 배치
{: #hello-world-template}

이 액션은 단일 매개변수(JSON 오브젝트여야 함)를 허용합니다.

1. 템플리트를 작성하려면 [{{site.data.keyword.Bluemix_notm}}의 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)로 이동한 후에 **작성 시작**을 클릭하십시오.

2. **빠른 시작 템플리트**를 클릭하십시오.

3. **패키지 이름** 필드를 검토하십시오. 필요에 따라 이를 업데이트할 수 있습니다. 기본값은 `hello-world`로 설정됩니다.

4. 소유할 액션에 대한 런타임을 선택하십시오(Node.js 8, Node.js 6, Python 3, Swift 4 또는 PHP 7.1).

5. **배치**를 클릭하십시오.

템플리트 배치 이후에는 코드를 추가로 편집하여 필요한 대로 이를 사용자 정의하거나 되돌아가서 사용 가능한 템플리트의 카탈로그를 확인할 수 있습니다.

## Message Hub 이벤트 템플리트 배치
{: #messagehub-events-template}

Message Hub 이벤트 템플리트는 액션 및 이 액션을 시작하는 트리거를 작성합니다. 트리거는 템플리트 작성 중에 선택한 Message Hub 주제에 추가된 새 항목이 있을 때마다 실행됩니다.

1. 템플리트를 작성하려면 [{{site.data.keyword.Bluemix_notm}}의 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)로 이동한 후에 **작성 시작**을 클릭하십시오.

2. **빠른 시작 템플리트**를 클릭하십시오.

3. **패키지 이름** 필드를 검토하십시오. 필요에 따라 이를 업데이트할 수 있습니다. 기본값은 `message-hub-events`로 설정됩니다.

4. 소유할 액션에 대한 런타임을 선택하십시오(Node.js 8, Node.js 6, Python 3, Swift 4 또는 PHP 7.1).

5. **다음**을 클릭하십시오.

### Message Hub 트리거 작성

트리거는 이벤트 소스에서 이벤트를 수신할 때 액션을 호출합니다. Message Hub 템플리트에 대한 트리거를 작성하려면 필수 Message Hub 서비스 인스턴스 정보와 함께 트리거를 제공하십시오.

**트리거 이름** 필드를 검토하십시오. 필요에 따라 이를 업데이트할 수 있습니다. 기본값은 `message-hub-events-trgr`로 설정됩니다.

### Message Hub 서비스 인스턴스 작성

다음 중 하나를 선택할 수 있습니다.
  * **자체 인스턴스 작성**
  * **자체 신임 정보 입력**

1. 이 예제의 경우에는 **자체 인스턴스 작성**을 선택하십시오.

2. Message Hub 설정 페이지의 새 탭으로 안내하기 위해 팝업이 열립니다. 일단 Message Hub 인스턴스를 작성하면 서비스 신임 정보 세트를 작성한 다음 **확인**을 클릭하여 탭을 닫고 이 페이지로 돌아와야 합니다.

3. 이제 **자체 신임 정보 입력**을 선택하고 다음 정보를 제공하십시오.
  * 사용자 이름 - _Message Hub 사용자 이름_
  * 비밀번호 - _Message Hub 비밀번호_
  * kafka_admin_url - _Message Hub 관리 REST URL_
  * 데이터베이스 - _Message Hub 데이터베이스의 이름_
  * 주제 - _구독할 주제_

### Message Hub 템플리트 배치

**배치**를 클릭하십시오.

템플리트 배치 이후에는 코드를 추가로 편집하여 필요한 대로 이를 사용자 정의하거나 되돌아가서 사용 가능한 템플리트의 카탈로그를 확인할 수 있습니다.

## 주기적 Slack 리마인더 템플리트 배치
{: #slack-reminder-template}

주기적 Slack 리마인더 템플리트는 트리거 작성 중에 사용자가 제공한 간격으로 Slack에 게시합니다. 이 템플리트를 작성하기 전에 https://api.slack.com/incoming-webhooks로 이동하여 필요한 수신 웹훅 URL을 설정하십시오.

1. **패키지 이름** 필드를 검토하십시오. 필요에 따라 이를 업데이트할 수 있습니다. 기본값은 `periodic-slack-reminder`로 설정됩니다.

2. 소유할 액션에 대한 런타임을 선택하십시오(Node.js 8, Node.js 6, Python 3, Swift 4 또는 PHP 7.1).

3. **매개변수** 섹션에서 **매개변수값** 필드에 웹훅 URL을 입력한 후 **다음**을 클릭하십시오(예: https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX).

### Slack 리마인더 트리거 작성

트리거는 이벤트 소스에서 이벤트를 수신할 때 액션을 호출합니다. Slack 리마인더 템플리트에 대한 트리거를 작성하려면 필수 Message Hub 서비스 인스턴스 정보와 함께 트리거를 제공하십시오.

1. **트리거 이름** 필드를 검토하십시오. 필요에 따라 이를 업데이트할 수 있습니다. 기본값은 `periodic-slack-reminder-trgr`로 설정됩니다.

2. 그런 다음 Pattern 또는 Cron 표현식을 사용하여 트리거를 실행하는 간격을 지정할 수 있습니다. 요일, 시간 및 분에 대해 UTC 시간을 선택할 수 있습니다. 원하는 간격 옵션을 선택하면 템플리트 배치 준비가 됩니다.

3. **배치**를 클릭하십시오.

템플리트 배치 이후에는 코드를 추가로 편집하여 필요한 대로 이를 사용자 정의하거나 되돌아가서 사용 가능한 템플리트의 카탈로그를 확인할 수 있습니다.
