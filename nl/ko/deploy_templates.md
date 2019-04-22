---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: deploy, deployment templates, templates, example, quickstart

subcollection: cloud-functions

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 빠른 시작 템플리트 배치
{: #serviceauth}

{{site.data.keyword.openwhisk}}는 다음 프로젝트에서 빠른 시작에 도움을 주는 템플리트의 카탈로그를 제공합니다. 템플리트는 액션, 트리거, 시퀀스의 조합입니다. 일부 템플리트는 또한 {{site.data.keyword.Bluemix_notm}}의 다른 서비스 인스턴스를 통합합니다. 템플리트를 사용하면 빠르고 손쉽게 프로젝트를 작성하고 즉시 코딩을 시작할 수 있습니다.
{: shortdesc}

## 사용 가능한 빠른 시작 템플리트
{: #available-templates}

|이름 |설명 |지원되는 런타임 |
|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} 이벤트](/docs/openwhisk?topic=cloud-functions-serviceauth#cloudant-template) | 문서가 {{site.data.keyword.cloudantfull}} 데이터베이스에서 편집 또는 추가된 경우, 콘솔에 변경사항을 로그하십시오. |Node.js, Swift, Python, PHP |
| [이미지 업로드](/docs/openwhisk?topic=cloud-functions-serviceauth#cos-upload-image) | 웹 액션을 사용하여 이미지를 {{site.data.keyword.cos_full}} 인스턴스의 버킷에 업로드한 후 해당 이미지의 썸네일을 검색하십시오. | Node.js |
| [HTTP 리소스 가져오기](/docs/openwhisk?topic=cloud-functions-serviceauth#get-http-resource-template) | HTTP 이벤트를 사용하여 웹 액션을 호출하고 Yahoo Weather API에서 데이터를 가져오십시오. |Node.js, Python |
| [Hello World](/docs/openwhisk?topic=cloud-functions-serviceauth#hello-world-template) | 단일 매개변수로 JSON 오브젝트를 허용하는 기본 액션을 작성하십시오. |Node.js, Swift, Python, PHP |
| [{{site.data.keyword.messagehub}} 이벤트](/docs/openwhisk?topic=cloud-functions-serviceauth#messagehub-events-template) |새 데이터가 {{site.data.keyword.messagehub_full}} 주제에 추가되면, 콘솔에 변경사항을 로그하십시오. |Node.js, Swift, Python, PHP |
| [주기적 Slack 리마인더](/docs/openwhisk?topic=cloud-functions-serviceauth#slack-reminder-template) | 주기적 트리거를 기반으로 웹 후크를 Slack에 사용하십시오. |Node.js, Swift, Python, PHP |

## {{site.data.keyword.cloudant_short_notm}} 이벤트 템플리트 배치
{: #cloudant-template}

{{site.data.keyword.cloudant_short_notm}} 템플리트는 액션 시퀀스 및 해당 시퀀스를 시작할 트리거를 작성합니다. 변경사항이 cat의 연결된 {{site.data.keyword.cloudant_short_notm}} 예제 데이터베이스에서 작성되면 트리거가 실행됩니다. 예상되는 데이터 항목은 이름과 색상이 정의된 cat입니다. 새 cat가 데이터베이스에 추가되거나 현재 cat이 편집되면 데이터가 콘솔에 로깅됩니다. 

### UI에서 {{site.data.keyword.cloudant_short_notm}} 이벤트 템플리트 배치

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 작성](https://cloud.ibm.com/openwhisk/create)으로 이동하십시오. 

2. **빠른 시작 템플리트**를 클릭하십시오.

3. **Cloudant 이벤트**를 클릭하십시오.

4. {{site.data.keyword.cloudant_short_notm}} 액션을 작성하십시오. 
    1. 패키지의 이름을 제공하거나 제공된 기본 이름인 `new-cloudant-item`을 사용하십시오.
    2. **액션** 목록에서 cats 데이터베이스 예제의 경우 `Node.js 6`을 선택하고 **다음**을 클릭하십시오.

5. {{site.data.keyword.cloudant_short_notm}} 트리거를 작성하십시오. 트리거는 이벤트 소스에서 이벤트를 수신할 때 액션을 호출합니다. 트리거를 작성하려면 {{site.data.keyword.messagehub}} 서비스 인스턴스 및 서비스 인증 정보를 제공해야 합니다. 
    1. **Cloudant 인스턴스** 목록에서 **고유 인스턴스 작성**을 선택하십시오. {{site.data.keyword.cloudant_short_notm}} 설정 페이지가 열립니다. 
    2. {{site.data.keyword.cloudant_short_notm}} 서비스 인스턴스를 작성하십시오. 
    3. 서비스 인증 정보 세트를 작성하십시오. 
    4. **확인**을 클릭하여 {{site.data.keyword.cloudant_short_notm}} 설정 페이지를 닫고 {{site.data.keyword.openwhisk_short}} 콘솔로 돌아가십시오.
    5. 이제 **Cloudant 인스턴스** 목록에서 **자체 인증 정보 입력**을 선택하고 다음 정보를 제공할 수 있습니다. 
      * 사용자 이름: 사용자의 {{site.data.keyword.cloudant_short_notm}} 사용자 이름
      * 비밀번호: 사용자의 {{site.data.keyword.cloudant_short_notm}} 비밀번호
      * 호스트: `<username>.cloudant.com`
      * 데이터베이스: {{site.data.keyword.cloudant_short_notm}} 데이터베이스의 이름

5. **배치**를 클릭하십시오. 템플리트 배치 후에는 코드를 추가로 편집하여 필요에 따라 이를 사용자 정의하거나, 되돌아가서 사용 가능한 템플리트의 카탈로그를 체크아웃할 수 있습니다.

### CLI에서 {{site.data.keyword.cloudant_short_notm}} 이벤트 템플리트 배치

1. 템플리트 저장소를 복제하십시오.
    ```
    git clone https://github.com/ibm-functions/template-cloudant-trigger.git
    ```
    {: pre}

2. `nodejs-6`, `nodejs`, `php`, `python` 또는 `swift` 중에서 사용하려는 액션 런타임에 대한 디렉토리를 탐색하십시오.
    ```
    cd template-cloudant-trigger/runtimes/nodejs
    ```
    {: pre}

3. 다음 환경 변수를 사용하여 템플리트를 배치하십시오.
    ```
    CLOUDANT_HOSTNAME=<host> CLOUDANT_USERNAME=<username> CLOUDANT_PASSWORD=<password> CLOUDANT_DATABASE=<database> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>환경 변수 이해하기</caption>
    <thead>
    <th colspan=2>환경 변수 이해하기</th>
    </thead>
    <tbody>
    <tr><td><code>CLOUDANT_HOSTNAME</code></td><td><code>&lt;username&gt;.cloudant.com</code></td></tr>
    <tr><td><code>CLOUDANT_USERNAME</code></td><td>사용자의 {{site.data.keyword.cloudant_short_notm}} 사용자 이름</td></tr>
    <tr><td><code>CLOUDANT_PASSWORD</code></td><td>사용자의 {{site.data.keyword.cloudant_short_notm}} 비밀번호</td></tr>
    <tr><td><code>CLOUDANT_DATABASE</code></td><td>{{site.data.keyword.cloudant_short_notm}} 데이터베이스의 이름</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>패키지의 사용자 정의 이름</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>규칙의 사용자 정의 이름</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>트리거의 사용자 정의 이름</td></tr>
    </tbody></table>

## 이미지 업로드 템플리트 배치
{: #cos-upload-image}

이미지 업로드 템플리트는 소형 인터페이스를 통해 이미지를 {{site.data.keyword.cos_short}} 버킷으로 업로드할 수 있도록 허용하는 웹 액션을 작성합니다. 그리고 템플리트는 작은 그림으로서 이미지를 검색하며 웹 액션의 인터페이스에 이를 표시합니다.

### UI에서 이미지 업로드 템플리트 배치

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 작성](https://cloud.ibm.com/openwhisk/create)으로 이동하십시오. 

2. **빠른 시작 템플리트**를 클릭하십시오.

3. **이미지 업로드**를 클릭하십시오. 

4. 패키지의 이름을 입력하거나 기본 이름 `upload-image`를 사용하고 **다음**을 클릭하십시오.

6. 템플리트에서는 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스의 서비스 인증 정보가 필요합니다. **{{site.data.keyword.cos_short}}** 목록에서 다음 옵션 중 하나를 선택하십시오.
  * **새 인스턴스 작성**: 기존 서비스 인스턴스가 없는 경우에는 이 옵션을 선택하여 하나를 작성하십시오.
      1. 열려 있는 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스 작성 페이지에서 서비스 인스턴스를 작성하십시오.
      2. [HMAC 서비스 인증 정보 세트를 작성하십시오](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials).
      3. [최소한 하나의 버킷을 작성하십시오](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-create-buckets).
  * **자체 인증 정보 입력**: {{site.data.keyword.cos_short}} 서비스 인스턴스에 대한 자체 인증 정보를 수동으로 입력하려면 이 옵션을 선택하십시오. 인증 정보에는 HMAC 키가 있어야 하며 서비스 인스턴스에는 최소한 하나의 버킷이 있어야 합니다.
  * **기존 인스턴스**: 기존 {{site.data.keyword.cos_short}} 인스턴스가 있으면 목록에서 인스턴스 중 하나를 선택하십시오. 인증 정보에는 HMAC 키가 있어야 하며 서비스 인스턴스에는 최소한 하나의 버킷이 있어야 합니다.

7. **배치**를 클릭하십시오.

8. 왼쪽 탐색 창에서 **엔드포인트**를 클릭하십시오.

9. **웹 액션** 섹션에서 .json 접미부 없이 링크를 복사하고 이를 브라우저의 주소 표시줄에 붙여 넣으십시오. 템플리트의 웹 액션에 대한 인터페이스가 표시됩니다.

10. 선택사항: 템플리트가 배치된 후에 사용자는 **액션** 대시보드로 이동하여 2개의 새 패키지의 코드를 사용자 정의할 수 있습니다. 
    * {{site.data.keyword.cos_short}} 인스턴스 관련 작업을 수행하는 액션이 포함된 `cloud-object-storage` 패키지
    * `app` 액션이 포함된 템플리트 패키지(기본 이름 `upload-image`)

### CLI에서 이미지 업로드 템플리트 배치

1. [{{site.data.keyword.cos_full_notm}} 패키지를 설치하십시오](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions#cloud_object_storage_cli).

2. 템플리트 저장소를 복제하십시오.
    ```
    git clone https://github.com/ibm-functions/template-cloud-object-storage.git
    ```
    {: pre}

3. `nodejs` 런타임 디렉토리로 이동하십시오.
    ```
    cd template-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. 사용자 정의 패키지 이름 및 {{site.data.keyword.cos_short}} 버킷의 이름을 환경 변수로 사용하여 템플리트를 배치하십시오.
    ```
    PACKAGE_NAME=<name> BUCKET_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. {{site.data.keyword.openwhisk_short}} 콘솔의 [액션 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/openwhisk/actions)에서 `process-change` 액션을 클릭하십시오. 

5. 왼쪽 탐색 창에서 **엔드포인트**를 클릭하십시오.

6. **웹 액션** 섹션에서 .json 접미부 없이 링크를 복사하고 이를 브라우저의 주소 표시줄에 붙여 넣으십시오. 템플리트의 웹 액션에 대한 인터페이스가 표시됩니다.

## HTTP 리소스 가져오기 템플리트 배치
{: #get-http-resource-template}

HTTP 리소스 가져오기 템플리트는 외부 리소스, Yahoo Weather API를 페치하는 액션을 작성한 후에 데이터를 리턴합니다. 이 액션은 웹 애플리케이션에 대한 백엔드의 빌드에 유용한 인증 키가 필요 없고 CORS가 사용되는 URL로 호출될 수 있도록 허용하는 웹 액션으로서 사용됩니다. **참고**: 기본적으로 `get-http-resource` 엔드포인트는 이를 호출하는 모든 사용자가 공개적으로 사용할 수 있습니다. 

### UI에서 HTTP 리소스 가져오기 템플리트 배치 

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 작성](https://cloud.ibm.com/openwhisk/create)으로 이동하십시오. 

2. **빠른 시작 템플리트**를 클릭하십시오.

3. **HTTP 리소스 가져오기**를 가져오십시오.

3. 패키지의 이름을 입력하거나 기본 이름 `get-http-resource`를 사용하십시오.

4. Node.js 8, Node.js 6 또는 Python 3 중에서 액션에 대한 런타임을 선택하십시오.

5. **배치**를 클릭하십시오.

6. 다음 URL을 컬링하여 액션을 호출하십시오. `https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`. 예:
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

템플리트 배치 후에는 코드를 추가로 편집하여 필요에 따라 이를 사용자 정의하거나, 되돌아가서 사용 가능한 템플리트의 카탈로그를 체크아웃할 수 있습니다.

### CLI에서 HTTP 리소스 가져오기 템플리트 배치 

1. 템플리트 저장소를 복제하십시오.
    ```
    git clone https://github.com/ibm-functions/template-get-external-resource.git
    ```
    {: pre}

2. `nodejs-6`, `nodejs` 또는 `python` 중에서 사용하려는 액션 런타임에 대한 디렉토리를 탐색하십시오.
    ```
    cd template-get-external-resource/runtimes/nodejs
    ```
    {: pre}

3. 사용자 정의 패키지 이름을 환경 변수로 사용하여 템플리트를 배치하십시오.
    ```
    PACKAGE_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. 다음 URL을 컬링하여 액션을 호출하십시오. `https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`. 예:
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

## Hello World 템플리트 배치
{: #hello-world-template}

이 기본 Hello World 액션을 배치하여 {{site.data.keyword.openwhisk_short}}를 시작하거나 트리거 및 규칙 같이 사용자가 작성하는 다른 엔티티를 테스트할 수 있습니다. 

### UI에서 Hello World 템플리트 배치

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 작성](https://cloud.ibm.com/openwhisk/create)으로 이동하십시오. 

2. **빠른 시작 템플리트**를 클릭하십시오.

3. **Hello World**를 클릭하십시오.

4. 패키지의 이름을 입력하거나 기본 이름 `hello-world`를 사용하십시오.

5. Node.js 8, Node.js 6, Python 3, Swift 4 또는 PHP 7.1 중에서 액션에 대한 런타임을 선택하십시오.

6. **배치**를 클릭하십시오.

템플리트 배치 후에는 코드를 추가로 편집하여 필요에 따라 이를 사용자 정의하거나, 되돌아가서 사용 가능한 템플리트의 카탈로그를 체크아웃할 수 있습니다.

### CLI에서 Hello World 템플리트 배치

1. Hello World 템플리트 저장소를 복제하십시오.
    ```
    git clone https://github.com/ibm-functions/template-hello-world.git
    ```
    {: pre}

2. `nodejs-6`, `nodejs`, `php`, `python` 또는 `swift` 중에서 사용하려는 액션 런타임에 대한 디렉토리를 탐색하십시오.
    ```
    cd template-hello-world/runtimes/nodejs
    ```
    {: pre}

3. 템플리트를 배치하십시오.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

## {{site.data.keyword.messagehub}} 이벤트 템플리트 배치
{: #messagehub-events-template}

{{site.data.keyword.messagehub}} 이벤트 템플리트는 액션 및 해당 액션을 시작하는 트리거를 작성합니다. 트리거는 템플리트 작성 중에 선택한 {{site.data.keyword.messagehub}} 주제에 새 항목이 추가될 때마다 실행됩니다.

### UI에서 {{site.data.keyword.messagehub}} 이벤트 템플리트 배치

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 작성](https://cloud.ibm.com/openwhisk/create)으로 이동하십시오. 

2. **빠른 시작 템플리트**를 클릭하십시오.

3. **{{site.data.keyword.messagehub}} 이벤트**를 클릭하십시오. 

4. {{site.data.keyword.messagehub}} 액션을 작성하십시오. 
    1. 패키지의 이름을 제공하거나 제공된 기본 이름인 `message-hub-events`를 사용하십시오.
    2. Node.js 8, Node.js 6, Python 3, Swift 4 또는 PHP 7.1 중에서 액션에 대한 런타임을 선택하십시오.
    3. **다음**을 클릭하십시오.

5. {{site.data.keyword.messagehub}} 트리거를 작성하십시오. 트리거는 이벤트 소스에서 이벤트를 수신할 때 액션을 호출합니다. 트리거를 작성하려면 {{site.data.keyword.messagehub}} 서비스 인스턴스 및 서비스 인증 정보를 제공해야 합니다. 
    1. **MessageHub 인스턴스** 목록에서 **자체 인스턴스 작성**을 선택하십시오. {{site.data.keyword.messagehub}} 작성 페이지가 열립니다. 
    2. {{site.data.keyword.messagehub}} 서비스 인스턴스를 작성하십시오. 
    3. 서비스 인증 정보 세트를 작성하십시오. 
    4. **확인**을 클릭하여 {{site.data.keyword.messagehub}} 작성 페이지를 닫고 {{site.data.keyword.messagehub}} 콘솔로 돌아가십시오.
    5. 이제 **MessageHub 인스턴스** 목록에서 **자체 인증 정보 입력**을 선택하고 다음 정보를 제공할 수 있습니다. 
      * 사용자 이름: 사용자의 {{site.data.keyword.messagehub}} 사용자 이름
      * 비밀번호: 사용자의 {{site.data.keyword.messagehub}} 비밀번호
      * kafka_admin_url: {{site.data.keyword.messagehub}} 관리 REST URL
      * 데이터베이스: {{site.data.keyword.messagehub}} 데이터베이스의 이름
      * 주제: 구독할 주제

5. **배치**를 클릭하십시오.

템플리트 배치 후에는 코드를 추가로 편집하여 필요에 따라 이를 사용자 정의하거나, 되돌아가서 사용 가능한 템플리트의 카탈로그를 체크아웃할 수 있습니다.

### CLI에서 {{site.data.keyword.messagehub}} 이벤트 템플리트 배치

1. 템플리트 저장소를 복제하십시오.
    ```
    git clone https://github.com/ibm-functions/template-messagehub-trigger.git
    ```
    {: pre}

2. `nodejs-6`, `nodejs`, `php`, `python` 또는 `swift` 중에서 사용하려는 액션 런타임에 대한 디렉토리를 탐색하십시오.
    ```
    cd template-messagehub-trigger/runtimes/nodejs
    ```
    {: pre}

3. 다음 환경 변수를 사용하여 템플리트를 배치하십시오.
    ```
    KAFKA_BROKERS=<host> KAFKA_TOPIC=<topic> MESSAGEHUB_USER=<username> MESSAGEHUB_PASS=<password> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>환경 변수 이해하기</caption>
    <thead>
    <th colspan=2>환경 변수 이해하기</th>
    </thead>
    <tbody>
    <tr><td><code>KAFKA_BROKERS</code></td><td>{{site.data.keyword.messagehub}} 관리 REST URL</td></tr>
    <tr><td><code>KAFKA_TOPIC</code></td><td>구독할 주제</td></tr>
    <tr><td><code>MESSAGEHUB_USER</code></td><td>사용자의 {{site.data.keyword.messagehub}} 사용자 이름</td></tr>
    <tr><td><code>MESSAGEHUB_PASS</code></td><td>사용자의 {{site.data.keyword.messagehub}} 비밀번호</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>패키지의 사용자 정의 이름</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>규칙의 사용자 정의 이름</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>트리거의 사용자 정의 이름</td></tr>
    </tbody></table>

## 주기적 Slack 리마인더 템플리트 배치
{: #slack-reminder-template}

주기적 Slack 리마인더 템플리트는 트리거 작성 중에 사용자가 제공한 간격으로 Slack에 게시합니다.

### UI에서 주기적 Slack 리마인더 템플리트 배치

1. https://api.slack.com/incoming-webhooks로 이동하여 필요한 수신 웹훅 URL을 설정하십시오.

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 작성](https://cloud.ibm.com/openwhisk/create)으로 이동하십시오. 

2. **빠른 시작 템플리트**를 클릭하십시오.

3. **주기적 Slack 리마인더 이벤트**를 클릭하십시오.

4. 주기적 Slack 리마인더 액션을 작성하십시오. 
    1. 패키지의 이름을 제공하거나 제공된 기본 이름인 `periodic-slack-reminder`를 사용하십시오.
    2. Node.js 8, Node.js 6, Python 3, Swift 4 또는 PHP 7.1 중에서 액션에 대한 런타임을 선택하십시오.
    3. 매개변수 섹션에서 웹훅 URL을 **매개변수값** 필드에 입력하십시오(예: `https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX`).
    4. **다음**을 클릭하십시오.

5. 주기적 Slack 리마인더 트리거를 작성하십시오. 트리거는 이벤트 소스에서 이벤트를 수신할 때 액션을 호출합니다. 
    1. 패턴 또는 복제 표현식을 사용하여 트리거의 시간 간격을 지정하십시오.
        * 패턴: 요일, 시간 및 분에 대해 UTC 시간을 선택하십시오. 
        * 복제: <a href="http://crontab.org">UNIX crontab 구문</a>을 기반으로 복제 시퀀스를 지정하십시오. `X X X X X` 형식으로 공백으로 구분된 5개 이하를 사용하십시오.
    2. 트리거 JSON 페이로드를 추가하십시오.

6. **배치**를 클릭하십시오.

템플리트 배치 후에는 코드를 추가로 편집하여 필요에 따라 이를 사용자 정의하거나, 되돌아가서 사용 가능한 템플리트의 카탈로그를 체크아웃할 수 있습니다.

### CLI에서 주기적 Slack 리마인더 템플리트 배치

1. https://api.slack.com/incoming-webhooks로 이동하여 필요한 수신 웹훅 URL을 설정하십시오.

1. 템플리트 저장소를 복제하십시오.
    ```
    git clone https://github.com/ibm-functions/template-reminder-slack.git
    ```
    {: pre}

2. `nodejs-6`, `nodejs`, `php`, `python` 또는 `swift` 중에서 사용하려는 액션 런타임에 대한 디렉토리를 탐색하십시오.
    ```
    cd template-reminder-slack/runtimes/nodejs
    ```
    {: pre}

3. 다음 환경 변수를 사용하여 템플리트를 배치하십시오.
    ```
    SLACK_WEBHOOK_URL=<url> ALARM_CRON=<cron> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>환경 변수 이해하기</caption>
    <thead>
    <th colspan=2>환경 변수 이해하기</th>
    </thead>
    <tbody>
    <tr><td><code>SLACK_WEBHOOK_URL</code></td><td>웹훅 URL(예: <code>https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX</code>) </td></tr>
    <tr><td><code>ALARM_CRON</code></td><td><a href="http://crontab.org">UNIX crontab 구문</a>을 기반으로 하는 복제 시퀀스. <code>X X X X X</code> 형식으로 공백으로 구분된 5개 이하의 필드를 사용하십시오.</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>패키지의 사용자 정의 이름</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>규칙의 사용자 정의 이름</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>트리거의 사용자 정의 이름</td></tr>
    </tbody></table>
