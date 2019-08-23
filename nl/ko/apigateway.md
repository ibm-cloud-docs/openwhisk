---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: serverless, rest api, gateway, web actions, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# 서버리스 REST API 작성
{: #apigateway}

API를 사용하여 {{site.data.keyword.openwhisk}} [웹 액션](/docs/openwhisk?topic=cloud-functions-actions_web)을 직접 관리할 수 있습니다.
{: shortdesc}

IAM 기반 네임스페이스의 경우 API 게이트웨이를 사용하여 API를 작성하는 작업이 지원되지 않습니다. 대신 Cloud Foundry 기반 네임스페이스를 사용하십시오.
{: important}

## {{site.data.keyword.openwhisk_short}}에서 REST API를 사용하는 이유가 무엇입니까?

API 게이트웨이를 웹 액션에 대한 프록시로 사용할 수 있습니다. API 게이트웨이는 HTTP 메소드 라우팅, 클라이언트 ID 및 시크릿, 속도 제한, CORS, API 사용 보기, 응답 로그 보기 및 API 공유 정책을 제공합니다.


API 관리에 대한 자세한 정보를 얻기 위해 [API 관리 문서](/docs/api-management?topic=api-management-manage_openwhisk_apis)를 읽을 수 있습니다.

## 첫 번째 API 작성
{: #api_create}

REST API를 작성하려면 Cloud Foundry 영역에 `SpaceDeveloper` 권한이 있어야 합니다. 영역 권한은 `ibmcloud account space-roles<org>`를 실행하여 확인할 수 있습니다.
{: note}

시작하기 전에 [{{site.data.keyword.openwhisk_short}} CLI 플러그인](/docs/openwhisk?topic=cloud-functions-cli_install)을 설치하십시오.

1. 이름이 `hello.js`인 Javascript 파일에 다음 코드를 저장하십시오.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello, ${name}!`};
  }
  ```
  {: codeblock}

2. 작성된 파일을 사용하여 `hello`로 이름 지정된 웹 조치를 작성하십시오. `--web true` 플래그를 추가하십시오. `<filepath>`를 `hello.js` 파일의 파일 경로로 대체하십시오.

  ```
  ibmcloud fn action create hello <filepath>/hello.js --web true
  ```
  {: pre}

  **출력 예**
  ```
  ok: created action hello
  ```
  {: screen}

3. 기본 경로 `/hello`, 경로 `/world`, 메소드 `get` 및 응답 유형 `json`의 API를 작성하십시오.
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  **출력 예**
  새 URL은 `GET` HTTP 메소드를 사용하여 `hello` 액션을 노출하여 생성됩니다.

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  
4. 다음 cURL 명령을 사용하여 URL에 테스트 HTTP 요청을 전송하십시오.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=Jane
  ```
  {: pre}

  **출력 예**
  웹 액션 `hello`가 호출되며 조회 매개변수에 `name` 매개변수가 포함된 JSON 오브젝트를 리턴합니다. 사용자는 요청 본문을 사용하여 또는 단순 조회 매개변수로 액션에 매개변수를 전달할 수 있습니다. 웹 액션은 인증을 사용하지 않고 공개적으로 액션을 호출할 수 있습니다.

  ```
  {
  "payload": "Hello, Jane!"
  }
  ```
  {: screen}



## HTTP 응답에 대한 전체 제어 사용
{: #api_control}

`--response-type` 플래그는 API 게이트웨이가 프록싱하는 웹 액션의 대상 URL을 제어합니다. 예를 들어, `--response-type json` 플래그를 사용하는 경우에는 액션의 전체 결과가 JSON 형식으로 리턴되고 `Content-Type` 헤더가 자동으로 `application/json`으로 설정됩니다.

본문에서 서로 다른 컨텐츠 유형을 리턴하려면 `statusCode` 및 `headers` 등의 HTTP 응답 특성에 대한 전체 제어를 사용하십시오. `--response-type http` 플래그를 사용하여 `http` 확장자로 웹 액션의 대상 URL을 구성할 수 있습니다. `http` 확장자로 웹 액션의 리턴을 준수하기 위해 액션의 코드를 변경하거나, 결과를 새 액션에 전달하기 위해 시퀀스에 액션을 포함할 수 있습니다. 그리고 새 액션은 HTTP 응답에 대해 적절히 형식화되도록 결과를 변환할 수 있습니다. [웹 액션](/docs/openwhisk?topic=cloud-functions-actions_web) 문서에서 응답 유형 및 웹 액션 확장자에 대해 자세히 알아볼 수 있습니다.

1. 다음 코드를 `hello.js`로 저장하십시오.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello, ${name}!`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. `hello` 웹 액션을 `hello.js` 코드의 새 버전으로 업데이트하십시오.
  ```
  ibmcloud fn action update hello <filepath>/hello.js --web true
  ```
  {: pre}

  **출력**
  ```
    ok: updated action hello
  ```
  {: screen}

3. `--response-type http` 플래그를 사용하여 API 응답 유형을 업데이트하십시오.
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

  **출력**
  ```
  ok: created API /hello/world GET for action /_/hello https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

4. 다음 cURL 명령을 사용하여 업데이트된 API를 호출하십시오.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  **출력 예**
  ```
  {
  "payload": "Hello, Serverless API!"
  }
  ```
  {: screen}

## 구성 수정
{: #api_modify_config}

구성을 작성한 후에는 {{site.data.keyword.openwhisk_short}} 대시보드의 [API 탭](https://cloud.ibm.com/openwhisk/apimanagement){: external}을 사용하여 다음 방법으로 구성을 수정할 수 있습니다.

* {{site.data.keyword.openwhisk_short}} 액션 세트를 랩핑하는 [{{site.data.keyword.openwhisk_short}} API](/docs/services/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis)를 작성하십시오.
* API 보안 및 속도 제한 정책을 적용하여 [API를 보호](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis)합니다.
* API 사용 통계를 보고 응답 로그를 체크아웃하여 [트래픽을 관리](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis)합니다.
* {{site.data.keyword.cloud_notm}} 내부와 외부 모두에서 개발자와 함께 API를 [소셜화하고 공유](/docs/services/api-management?topic=api-management-manage_apis#share_api_manage_apis)합니다.

</br>
구성을 업데이트하면 정의 파일을 JSON 형식으로 다운로드한 다음 CLI를 사용하여 다시 가져올 수 있습니다. 구성 다운로드 및 가져오기는 예를 들어 지속적 통합 및 배치(CICD) 파이프라인에서 무인 배치에 유용할 수 있습니다. 또한 UI를 사용하여 API 정의 파일을 업로드한 후 다시 가져올 수도 있습니다.



