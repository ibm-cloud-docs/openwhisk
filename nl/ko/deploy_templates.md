---

copyright:
  years: 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# 빠른 시작 템플리트 배치
{: #serviceauth}

{{site.data.keyword.openwhisk}}는 다음 프로젝트에서 빠른 시작에 도움을 주는 템플리트의 카탈로그를 제공합니다. 템플리트는 액션, 트리거, 시퀀스의 조합이며 {{site.data.keyword.Bluemix}}의 서비스 인스턴스를 통합할 수도 있습니다. 템플리트를 사용하면 빠르고 손쉽게 프로젝트를 작성하고 즉시 코딩을 시작할 수 있습니다. 

이 튜토리얼에서는 {{site.data.keyword.openwhisk_short}}를 통해 사용할 수 있는 템플리트의 배치에 대해 안내합니다.
{: shortdesc}

## 사용 가능한 빠른 시작 템플리트
{: #available-templates}

|이름 |설명 |지원되는 런타임 |
|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} 이벤트](./deploy_templates.html#cloudant-template) |{{site.data.keyword.cloudant}}에 편집되거나 추가된 문서가 있으면 콘솔에서 변경사항을 로깅합니다. |Node.js, Swift, Python, PHP |
| [이미지 업로드](./deploy_templates.html#cos-upload-image) |{{site.data.keyword.cos_full}} 인스턴스의 버킷으로 이미지를 업로드한 후에 해당 이미지의 작은 그림을 검색할 수 있도록 허용하는 웹 액션입니다. | Node.js |
| [HTTP 리소스 가져오기](./deploy_templates.html#get-http-resource-template) |HTTP 이벤트에 대한 응답으로 호출된 후에 Yahoo Weather API에서 데이터를 페치하는 웹 액션입니다. |Node.js, Python |
| [Hello World](./deploy_templates.html#hello-world-template) |이 액션은 JSON 오브젝트여야 하는 단일 매개변수를 허용합니다. |Node.js, Swift, Python, PHP |
| [{{site.data.keyword.messagehub}} 이벤트](./deploy_templates.html#messagehub-events-template) |{{site.data.keyword.messagehub_full}} 주제에 새 데이터가 추가되면 콘솔에서 변경사항을 로깅합니다. |Node.js, Swift, Python, PHP |
| [주기적 Slack 리마인더](./deploy_templates.html#slack-reminder-template) |주기적 트리거를 기반으로 Slack에 게시할 액션입니다. |Node.js, Swift, Python, PHP |

## {{site.data.keyword.cloudant_short_notm}} 이벤트 템플리트 배치
{: #cloudant-template}

{{site.data.keyword.cloudant_short_notm}} 템플리트는 액션 시퀀스 및 해당 시퀀스를 시작할 트리거를 작성합니다. 트리거는 이름과 색상이 있는 고양이의 데이터베이스여야 하는 연결된 {{site.data.keyword.cloudant_short_notm}} 데이터베이스에 변경사항이 있을 때 실행됩니다. 예상되는 데이터 항목은 이름과 색상이 정의된 고양이입니다. 새 고양이가 데이터베이스에 추가되거나 현재 고양이가 편집되면 해당 데이터가 콘솔에 로깅됩니다.

1. 템플리트를 작성하려면 {{site.data.keyword.Bluemix_notm}}의 [{{site.data.keyword.openwhisk_short}}로 이동한 후에 **작성 시작**을 클릭하십시오. 

2. 빠른 시작 템플리트를 클릭하십시오. 

3. Cloudant 이벤트를 클릭하십시오. 

### {{site.data.keyword.cloudant_short_notm}} 액션 작성

1. 그 다음에는 패키지의 이름을 제공하거나 제공된 기본 이름 new-cloudant-item을 사용하십시오.

2. 액션 드롭 다운 아래에서 소유할 액션에 대한 런타임을 선택하십시오(nodejs, swift, python 또는 php). 이 예제의 경우에는 nodejs를 선택하고 다음을 클릭하십시오.

### {{site.data.keyword.cloudant_short_notm}} 트리거 작성

트리거는 이벤트 소스에서 이벤트를 수신할 때 액션을 호출합니다. {{site.data.keyword.cloudant_short_notm}} 템플리트에 대한 트리거를 작성하려면 필수 {{site.data.keyword.cloudant_short_notm}} 서비스 인스턴스 정보를 트리거에 제공하십시오. 

#### {{site.data.keyword.cloudant_short_notm}} 서비스 인스턴스 작성

다음 중 하나를 선택할 수 있습니다.
  * 자체 인스턴스 작성
  * 자체 신임 정보 입력

1. 이 예제의 경우에는 자체 인스턴스 작성을 선택하십시오.

2. {{site.data.keyword.cloudant_short_notm}} 설정 페이지의 새 탭으로 안내하기 위해 팝업이 열립니다. {{site.data.keyword.cloudant_short_notm}} 인스턴스가 작성되면 서비스 신임 정보 세트를 작성한 후에 확인을 클릭하여 탭을 닫고 이 페이지로 돌아와야 합니다. 

3. 이제 자체 신임 정보 입력을 선택하고 다음 정보를 제공하십시오.
  * 사용자 이름 - {{site.data.keyword.cloudant_short_notm}} 사용자 이름
  * 비밀번호 - {{site.data.keyword.cloudant_short_notm}} 비밀번호
  * 호스트 - 일반적으로 username.{{site.data.keyword.cloudant_short_notm}}.com
  * 데이터베이스 - {{site.data.keyword.cloudant_short_notm}} 데이터베이스의 이름

### {{site.data.keyword.cloudant_short_notm}} 템플리트 배치

배치를 클릭하십시오.

템플리트 배치 후에는 코드를 추가로 편집하여 필요에 따라 이를 사용자 정의하거나, 되돌아가서 사용 가능한 템플리트의 카탈로그를 체크아웃할 수 있습니다. 

## 이미지 업로드 템플리트 배치
{: #cos-upload-image}

이미지 업로드 템플리트는 소형 인터페이스를 통해 이미지를 {{site.data.keyword.cos_short_notm}} 버킷으로 업로드할 수 있도록 허용하는 웹 액션을 작성합니다. 그리고 템플리트는 작은 그림으로서 이미지를 검색하며  웹 액션의 인터페이스에 이를 표시합니다. 

템플리트를 배치하려면 다음을 수행하십시오. 

1. [{{site.data.keyword.Bluemix_notm}} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")의 {{site.data.keyword.openwhisk_short}} 콘솔로 이동하십시오](https://console.bluemix.net/openwhisk/)(https://console.bluemix.net/openwhisk/)].

2. **작성 시작**을 클릭하십시오. 

2. **빠른 시작 템플리트**를 클릭하십시오. 

3. **이미지 업로드** 템플리트를 클릭하십시오. 

4. 패키지의 이름을 입력하거나 제공된 기본 이름 `upload-image`를 사용하십시오. 

5. **다음**을 클릭하십시오.

6. 템플리트에서는 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스의 서비스 신임 정보가 필요합니다. **{{site.data.keyword.cos_short}}** 목록에서 다음 옵션 중 하나를 선택하십시오.  
  * **새 인스턴스 작성**: 기존 서비스 인스턴스가 없는 경우에는 이 옵션을 선택하여 하나를 작성하십시오. 
      1. 열려 있는 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스 작성 페이지에서 서비스 인스턴스를 작성하십시오. 
      2. [HMAC 서비스 신임 정보 세트를 작성하십시오](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials).
      3. [최소한 하나의 버킷을 작성하십시오](/docs/services/cloud-object-storage/getting-started.html#create-buckets).
  * **자체 신임 정보 입력**: {{site.data.keyword.cos_short}} 서비스 인스턴스에 대한 자체 신임 정보를 수동으로 입력하려면 이 옵션을 선택하십시오. 신임 정보에는 HMAC 키가 있어야 하며 서비스 인스턴스에는 최소한 하나의 버킷이 있어야 합니다. 
  * **기존 인스턴스**: 기존 {{site.data.keyword.cos_short}} 인스턴스가 있으면 목록에서 인스턴스 중 하나를 선택하십시오. 신임 정보에는 HMAC 키가 있어야 하며 서비스 인스턴스에는 최소한 하나의 버킷이 있어야 합니다. 

7. **배치**를 클릭하십시오.

8. 왼쪽 탐색 창에서 **엔드포인트**를 클릭하십시오. 

9. 웹 액션 섹션에서 .json 접미부 없이 링크를 복사한 후에 이를 브라우저의 주소 표시줄에 붙여넣으십시오. 템플리트의 웹 액션에 대한 인터페이스가 표시됩니다. 

10. 선택사항: 템플리트가 배치된 후에 사용자는 액션 대시보드로 이동하여 2개의 새 패키지의 코드를 사용자 정의할 수 있습니다. 
    * {{site.data.keyword.cos_short}} 인스턴스 관련 작업을 수행하는 액션이 포함된 `cloud-object-storage` 패키지
    * `app` 액션이 포함된 템플리트 패키지(기본 이름 `upload-image`)

## HTTP 리소스 가져오기 템플리트 배치
{: #get-http-resource-template}

HTTP 리소스 가져오기 템플리트는 외부 리소스, Yahoo Weather API를 페치하는 액션을 작성한 후에 데이터를 리턴합니다. 이 액션은 웹 애플리케이션에 대한 백엔드의 빌드에 유용한 인증 키가 필요 없고 CORS가 사용되는 URL로 호출될 수 있도록 허용하는 웹 액션으로서 사용됩니다. **참고**: 기본적으로 `get-http-resource` 엔드포인트는 호출하려는 모든 사용자가 공개적으로 사용할 수 있습니다.

1. 템플리트를 작성하려면 {{site.data.keyword.Bluemix_notm}}의 [{{site.data.keyword.openwhisk_short}}로 이동한 후에 **작성 시작**을 클릭하십시오. 

2. 빠른 시작 템플리트를 클릭하십시오. 

3. 패키지 이름 필드를 검토하십시오. 필요에 따라 이를 업데이트할 수 있습니다. 기본값은 get-http-resource로 설정됩니다.

4. 소유할 액션에 대한 런타임을 선택하십시오(Node.js 8, Node.js 6 또는 Python 3). 

5. 배치를 클릭하십시오.

템플리트 배치 후에는 코드를 추가로 편집하여 필요에 따라 이를 사용자 정의하거나, 되돌아가서 사용 가능한 템플리트의 카탈로그를 체크아웃할 수 있습니다. 

## Hello World 템플리트 배치
{: #hello-world-template}

이 액션은 JSON 오브젝트여야 하는 단일 매개변수를 허용합니다. 

1. 템플리트를 작성하려면 {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/)의 [{{site.data.keyword.openwhisk_short}}로 이동한 후에 작성 시작을 클릭하십시오. 

2. 빠른 시작 템플리트를 클릭하십시오. 

3. 패키지 이름 필드를 검토하십시오. 필요에 따라 이를 업데이트할 수 있습니다. 기본값은 hello-world로 설정됩니다.

4. 소유할 액션에 대한 런타임을 선택하십시오(Node.js 8, Node.js 6, Python 3, Swift 4 또는 PHP 7.1). 

5. 배치를 클릭하십시오.

템플리트 배치 후에는 코드를 추가로 편집하여 필요에 따라 이를 사용자 정의하거나, 되돌아가서 사용 가능한 템플리트의 카탈로그를 체크아웃할 수 있습니다. 

## {{site.data.keyword.messagehub}} 이벤트 템플리트 배치
{: #messagehub-events-template}

{{site.data.keyword.messagehub}} 이벤트 템플리트는 액션 및 해당 액션을 시작하는 트리거를 작성합니다. 트리거는 템플리트 작성 중에 선택한 {{site.data.keyword.messagehub}} 주제에 새 항목이 추가될 때마다 실행됩니다. 

1. 템플리트를 작성하려면 {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/)의 [{{site.data.keyword.openwhisk_short}}로 이동한 후에 작성 시작을 클릭하십시오. 

2. 빠른 시작 템플리트를 클릭하십시오. 

3. 패키지 이름 필드를 검토하십시오. 필요에 따라 이를 업데이트할 수 있습니다. 기본값은 message-hub-events로 설정됩니다.

4. 소유할 액션에 대한 런타임을 선택하십시오(Node.js 8, Node.js 6, Python 3, Swift 4 또는 PHP 7.1). 

5. 다음을 클릭하십시오.

### {{site.data.keyword.messagehub}} 트리거 작성

트리거는 이벤트 소스에서 이벤트를 수신할 때 액션을 호출합니다. {{site.data.keyword.messagehub}} 템플리트에 대한 트리거를 작성하려면 필수 {{site.data.keyword.messagehub}} 서비스 인스턴스 정보를 트리거에 제공하십시오. 

트리거 이름 필드를 검토하십시오. 필요에 따라 이를 업데이트할 수 있습니다. 기본값은 message-hub-events-trgr로 설정됩니다.

### {{site.data.keyword.messagehub}} 서비스 인스턴스 작성

다음 중 하나를 선택할 수 있습니다.
  * 자체 인스턴스 작성
  * 자체 신임 정보 입력

1. 이 예제의 경우에는 자체 인스턴스 작성을 선택하십시오.

2. {{site.data.keyword.messagehub}} 설정 페이지의 새 탭으로 안내하기 위해 팝업이 열립니다. {{site.data.keyword.messagehub}} 인스턴스가 작성되면 서비스 신임 정보 세트를 작성한 후에 확인을 클릭하여 탭을 닫고 이 페이지로 돌아와야 합니다. 

3. 이제 자체 신임 정보 입력을 선택하고 다음 정보를 제공하십시오.
  * 사용자 이름 - {{site.data.keyword.messagehub}} 사용자 이름
  * 비밀번호 - {{site.data.keyword.messagehub}} 비밀번호
  * kafka_admin_url - {{site.data.keyword.messagehub}} admin REST URL
  * 데이터베이스 - {{site.data.keyword.messagehub}} 데이터베이스의 이름
  * 주제 - 구독할 주제

### {{site.data.keyword.messagehub}} 템플리트 배치

배치를 클릭하십시오.

템플리트 배치 후에는 코드를 추가로 편집하여 필요에 따라 이를 사용자 정의하거나, 되돌아가서 사용 가능한 템플리트의 카탈로그를 체크아웃할 수 있습니다. 

## 주기적 Slack 리마인더 템플리트 배치
{: #slack-reminder-template}

주기적 Slack 리마인더 템플리트는 트리거 작성 중에 사용자가 제공한 간격으로 Slack에 게시합니다. 이 템플리트를 작성하기 전에 https://api.slack.com/incoming-webhooks로 이동하여 필요한 수신 웹훅 URL을 설정하십시오. 

1. 패키지 이름 필드를 검토하십시오. 필요에 따라 이를 업데이트할 수 있습니다. 기본값은 periodic-slack-reminder로 설정됩니다.

2. 소유할 액션에 대한 런타임을 선택하십시오(Node.js 8, Node.js 6, Python 3, Swift 4 또는 PHP 7.1). 

3. 매개변수 섹션에서 매개변수값 필드에 웹훅 URL을 입력한 후 다음을 클릭하십시오 (예: https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX).

### Slack 리마인더 트리거 작성

트리거는 이벤트 소스에서 이벤트를 수신할 때 액션을 호출합니다. Slack 리마인더 템플리트에 대한 트리거를 작성하려면 필수 {{site.data.keyword.messagehub}} 서비스 인스턴스 정보를 트리거에 제공하십시오. 

1. 트리거 이름 필드를 검토하십시오. 필요에 따라 이를 업데이트할 수 있습니다. 기본값은 periodic-slack-reminder-trgr로 설정됩니다.

2. 그 다음에는 Pattern 또는 Cron 표현식을 사용하여 트리거가 실행되는 간격을 지정할 수 있습니다. 요일, 시간 및 분에 대해 UTC 시간을 선택할 수 있습니다. 원하는 간격 옵션을 선택하면 템플리트 배치 준비가 됩니다.

3. 배치를 클릭하십시오.

템플리트 배치 후에는 코드를 추가로 편집하여 필요에 따라 이를 사용자 정의하거나, 되돌아가서 사용 가능한 템플리트의 카탈로그를 체크아웃할 수 있습니다. 
