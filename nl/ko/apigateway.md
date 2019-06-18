---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: serverless, rest api, gateway, web actions

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

# 서버리스 REST API 작성
{: #apigateway}

API를 사용하여 {{site.data.keyword.openwhisk}} 액션을 직접 관리할 수 있습니다. API 게이트웨이는 [웹 액션](/docs/openwhisk?topic=cloud-functions-actions_web)에 대한 프록시 역할을 하며 HTTP 메소드 라우팅, 클라이언트 ID 및 시크릿, 속도 제한, CORS, API 사용 보기, 응답 로그 보기 및 API 공유 정책 등을 제공합니다.
{: shortdesc}

API 관리에 대한 자세한 정보를 얻기 위해 [API 관리 문서](/docs/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis)를 읽을 수 있습니다.



## 첫 번째 API 작성
{: #api_create}

시작하기 전에 [{{site.data.keyword.openwhisk_short}} CLI 플러그인](/docs/openwhisk?topic=cloud-functions-cli_install)을 설치하십시오.

1. 이름이 `hello.js`인 Javascript 파일에 다음 코드를 저장하십시오.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. 작성한 파일을 사용하여 이름이 `hello`인 웹 액션을 작성하십시오. **참고:** 플래그 `--web true`를 추가하십시오.
  ```
  ibmcloud fn action create hello hello.js --web true
  ```
  {: pre}

  출력 예:
  ```
  ok: created action hello
  ```
  {: screen}

3. 기본 경로 `/hello`, 경로 `/world`, 메소드 `get` 및 응답 유형 `json`의 API를 작성하십시오.
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  출력 예:
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  새 URL은 GET HTTP 메소드를 사용하여 `hello` 액션을 노출하여 생성됩니다.

4. cURL 명령을 사용하여 URL에 테스트 HTTP 요청을 전송하십시오.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  출력 예:
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

웹 액션 `hello`가 호출되며, 이는 조회 매개변수에 **name** 매개변수가 포함된 JSON 오브젝트를 리턴합니다. 사용자는 요청 본문을 사용하여 또는 단순 조회 매개변수로 액션에 매개변수를 전달할 수 있습니다. 웹 액션은 인증을 사용하지 않고 공개적으로 액션을 호출할 수 있습니다.

## HTTP 응답에 대한 전체 제어 사용
{: #api_control}

`--response-type` 플래그는 API 게이트웨이가 프록싱하는 웹 액션의 대상 URL을 제어합니다. 예를 들어, `--response-type json` 플래그를 사용하는 경우에는 액션의 전체 결과가 JSON 형식으로 리턴되고 **Content-Type** 헤더가 자동으로 `application/json`으로 설정됩니다.

본문에서 서로 다른 컨텐츠 유형을 리턴하려면 **statusCode** 및 **headers** 등의 HTTP 응답 특성에 대한 전체 제어를 사용하십시오. `--response-type http` 플래그를 사용하여 `http` 확장자로 웹 액션의 대상 URL을 구성할 수 있습니다. `http` 확장자로 웹 액션의 리턴을 준수하기 위해 액션의 코드를 변경하거나, 결과를 새 액션에 전달하기 위해 시퀀스에 액션을 포함할 수 있습니다. 그리고 새 액션은 HTTP 응답에 대해 적절히 형식화되도록 결과를 변환할 수 있습니다. [웹 액션](/docs/openwhisk?topic=cloud-functions-actions_web) 문서에서 응답 유형 및 웹 액션 확장자에 대해 자세히 알아볼 수 있습니다.

1. JSON 특성 `body`, `statusCode` 및 `headers`를 리턴하는 `hello.js` 액션에 대한 코드를 변경하십시오.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello world ${name}`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. 수정된 결과로 액션을 업데이트하십시오.
  ```
  ibmcloud fn action update hello hello.js --web true
  ```
  {: pre}

3. `--response-type http` 플래그를 사용하여 API 응답 유형을 업데이트하십시오.
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

4. 다음 cURL 명령을 사용하여 업데이트된 API를 호출하십시오.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  출력 예:
  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  {: screen}


<staging books>

## 다중 웹 액션 노출
{: #api_multiple_web}

여러 웹 액션을 노출하여 앱 백엔드를 구현할 수 있습니다. 예를 들어, 북 클럽에 대한 액션 세트를 노출하려면 일련의 액션을 사용하여 북 클럽에 대한 백엔드를 구현할 수 있습니다.

|액션 |HTTP 메소드 |설명 |
| ----------- | ----------- | ------------ |
|getBooks    |GET |서적 세부사항 가져오기  |
|postBooks   |POST |서적 추가 |
|putBooks    |PUT |서적 세부사항 업데이트 |
|deleteBooks |DELETE |서적 삭제 |

이 예제에서 API는 경로 매개변수로 정의됩니다. 경로 매개변수를 사용하는 경우에는 API를 `http`의 응답 유형으로 정의해야 합니다. 기본 경로로 시작하며 실제 경로 매개변수값을 포함하는 경로 값은 액션의 JSON 매개변수의 `__ow_path` 필드에서 사용 가능합니다. HTTP 컨텍스트 필드에 대한 세부사항은 [웹 액션 HTTP 컨텍스트](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_context) 문서를 참조하십시오.

이 북 클럽 웹 액션 예제를 사용해 보십시오.

1. `/club`을 HTTP URL 기본 경로로, `books`를 해당 리소스로, 그리고 `{isbn}`을 ISBN(International Standard Book Number)을 사용한 특정 서적의 식별에 사용되는 경로 매개변수로 사용하여 이름이 `Book Club`인 북 클럽에 대한 API를 작성하십시오.
  ```
  ibmcloud fn api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud fn api create /club /books get getBooks                       --response-type http
  ibmcloud fn api create /club /books post postBooks                     --response-type http
  ibmcloud fn api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud fn api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: pre}

  기본 경로 `/club`으로 노출된 첫 번째 액션은 이름 `Book Club`으로 레이블이 지정됩니다. `/club` 아래에서 노출된 기타 액션은 이제 `Book Club`과 연관됩니다.

2. 노출된 모든 `Book Club` 액션을 나열하십시오.
  ```
  ibmcloud fn api list /club -f
  ```
  {: pre}

  출력 예:
  ```
  ok: APIs
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: postBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: post
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: putBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: put
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: deleteBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: delete
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

3. HTTP POST를 사용하여 제목이 `JavaScript: The Good Parts`인 서적을 추가하십시오.
  ```
  curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' -H "Content-Type: application/json" https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  출력 예:
  ```
  {
    "result": "success"
  }
  ```
  {: screen}

4. `getBooks` 액션에 대한 HTTP GET 호출을 사용하여 서적의 목록을 가져오십시오.
  ```
  curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  출력 예:
  ```
  {
    "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
  }
  ```
  {: screen}

5. `deleteBooks` 액션에 대한 HTTP DELETE 호출을 사용하여 특정 서적을 삭제하십시오. 이 예제에서 `deleteBooks` 액션의 `__ow_path` 필드 값은 `/club/books/978-0596517748`이며, 여기서 `978-0596517748`은 경로의 `{isbn}` 실제 값입니다.
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

## 구성 내보내기 및 가져오기
{: #api_export_import}

구성을 내보내거나 가져오기 위해 북 클럽 예제를 계속 사용할 수 있습니다.

1. `Book Club` API를 이름이 `club-swagger.json`인 파일로 내보내십시오. 이 파일은 파일을 입력으로 사용하여 API를 재작성하기 위한 기반으로 사용될 수 있습니다.
  ```
  ibmcloud fn api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. 우선 공통 기본 경로 아래의 노출된 모든 URL을 삭제하여 Swagger 파일을 테스트하십시오.
  ```
  ibmcloud fn api delete /club
  ```
  {: pre}

  출력 예:
  ```
ok: deleted API /club
  ```
  {: screen}

  기본 경로 `/club` 또는 API 이름 레이블 `"Book Club"`을 사용하여 노출된 모든 URL을 삭제할 수 있습니다.
  {: tip}

3. `club-swagger.json` 파일을 사용하여 `Book Club` API를 복원하십시오.
  ```
  ibmcloud fn api create --config-file club-swagger.json
  ```
  {: pre}

  출력 예:
  ```
  ok: created api /club/books/{isbn} get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} put for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} delete for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books post for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: screen}

4. `Book Club` API가 재작성되었는지 확인하십시오.
  ```
  ibmcloud fn api list /club
  ```
  {: pre}

  출력 예:
  ```
  ok: apis
  Action                    Verb         API Name        URL
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}


</staging book>

## 구성 수정
{: #api_modify_config}

구성을 작성한 후에는 {{site.data.keyword.openwhisk_short}} 대시보드의 [**API 탭**](https://cloud.ibm.com/openwhisk/apimanagement)을 사용하여 다음 방법으로 구성을 수정할 수 있습니다.

* {{site.data.keyword.openwhisk_short}} 액션 세트를 랩핑하는 [{{site.data.keyword.openwhisk_short}} API](/docs/services/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis)를 작성하십시오. 
* API 보안 및 속도 제한 정책을 적용하여 [API를 보호](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis)합니다.
* API 사용 통계를 보고 응답 로그를 체크아웃하여 [트래픽을 관리](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis)합니다.
* {{site.data.keyword.Bluemix_notm}} 내부와 외부 모두에서 개발자와 함께 API를 [소셜화하고 공유](/docs/services/api-management?topic=api-management-manage_apis#share_api_manage_apis)합니다.

구성을 업데이트하면 정의 파일을 JSON 형식으로 다운로드한 다음 CLI를 사용하여 다시 가져올 수 있습니다. 구성 다운로드 및 가져오기는 예를 들어 지속적 통합 및 배치(CICD) 파이프라인에서 무인 배치에 유용할 수 있습니다. 또한 UI를 사용하여 API 정의 파일을 업로드하고 다시 가져오는 옵션이 있습니다.

