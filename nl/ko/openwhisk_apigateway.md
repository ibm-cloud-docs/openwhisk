---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 서버리스 REST API 작성
{: #openwhisk_apigateway}

{{site.data.keyword.openwhisk}} 액션은 API 게이트웨이를 도입하여 직접 API에서 관리함으로써 이점을 얻을 수 있으며, 이 API 게이트웨이는 [웹 액션](./openwhisk_webactions.html)에 대한 프록시 역할을 하고 이 웹 액션에 추가 기능을 제공합니다. 추가 기능에는 HTTP 메소드 라우팅, 클라이언트 ID/시크릿, 속도 제한, CORS, API 사용법 보기, 응답 로그 보기 및 API 공유 정책 등이 포함됩니다. API 관리에 대한 자세한 정보를 얻기 위해 [API 관리 문서](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis)를 읽을 수 있습니다.
{: shortdesc}

## 사용자 브라우저를 사용하여 OpenWhisk 웹 액션에서 API 작성
{: #create_api_browser}

[{{site.data.keyword.openwhisk_short}} 대시보드](https://console.bluemix.net/openwhisk/)에서 [**API 탭**](https://console.bluemix.net/openwhisk/apimanagement)을 사용하여 다음 태스크를 수행할 수 있습니다.

* [Cloud Functions API 작성](https://console.bluemix.net/openwhisk/apimanagement) - 일련의 OpenWhisk 액션을 랩핑하는 API를 작성합니다.
* [API 보안](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - API 보안 및 속도 제한 정책을 적용하여 API를 보호합니다.
* [트래픽 관리](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - API 사용 통계를 보고 응답 로그를 확인합니다.
* [사회화 및 공유](https://console.bluemix.net/docs/apis/management/manage_apis.html#share_api) - {{site.data.keyword.Bluemix_notm}} 내외부의 개발자와 API를 공유합니다.

## CLI 플러그인을 사용하여 OpenWhisk 웹 액션에서 API 작성
{: #create_api_cli}

다음 절에서는 {{site.data.keyword.openwhisk_short}} CLI 플러그인을 사용하는 API 관리 태스크를 안내합니다. CLI를 통해 API를 작성하고 관리하려면 먼저 {{site.data.keyword.Bluemix_notm}}에 대한 [{{site.data.keyword.openwhisk_short}} CLI 플러그인](https://console.bluemix.net/docs/openwhisk/bluemix_cli.html)을 설치해야 합니다.

편의상, 다음 API 태스크 목록을 사용하여 빨리 건너뛸 수 있는 작은 하위 주제로 주제를 나눌 수 있습니다.

* [첫 번째 API 작성](openwhisk_apigateway.html#create_cli_api)
* [HTTP 응답에 대한 전체 제어](openwhisk_apigateway.html#full_control)
* [다중 웹 액션 노출](openwhisk_apigateway.html#multiple_web_actions)
* [구성 내보내기](openwhisk_apigateway.html#export_config)
* [구성 가져오기](openwhisk_apigateway.html#import_config)
* [구성 수정](openwhisk_apigateway.html#modify_config)

### CLI를 사용하여 첫 번째 API 작성
{: #create_cli_api}

1. 다음 컨텐츠가 포함된 Javascript 파일 **hello.js**를 작성하십시오.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. 1단계에서 작성된 `hello.js` 파일을 사용하여 웹 액션 **hello**를 작성하십시오. **참고:** 플래그 `--web true`를 추가하십시오.
  ```
  ibmcloud wsk action create hello hello.js --web true
  ```
  {: pre}

  출력 예:
  ```
  ok: created action hello
  ```
  {: screen}

3. 응답 유형 `json`으로 기본 경로 `/hello`, 경로 `/world` 및 메소드 `get`의 API를 작성하십시오.
  ```
  ibmcloud wsk api create /hello /world get hello --response-type json
  ```
  {: pre}

  출력 예:
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  __GET__ HTTP 메소드를 통해 `hello` 액션을 노출하여 새 URL이 생성됩니다.

4. 마지막으로 **curl** 명령을 사용하여 URL에 HTTP 요청을 전송하십시오.
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  출력 예:
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

웹 액션 **hello**를 호출하면 조회 매개변수를 통해 전송된 매개변수 **이름**이 포함된 JSON 오브젝트가 다시 리턴됩니다. 단순 조회 매개변수 또는 요청 본문을 사용하여 액션에 매개변수를 전달할 수 있습니다. 웹 액션은 OpenWhisk 권한 부여 API 키를 사용하지 않고 공용 방식으로 액션을 호출할 수 있습니다.

### HTTP 응답에 대한 전체 제어
{: #full_control}

`--response-type` 플래그는 API 게이트웨이가 프록싱하는 웹 액션의 대상 URL을 제어합니다. `--response-type json`을 사용하면 JSON 형식으로 액션의 전체 결과가 리턴되며 Content-Type 헤더가 자동으로 `application/json`으로 설정됩니다.

`statusCode` 및 `headers` 등의 HTTP 응답 특성에 대한 전체 제어 권한을 가지려 하므로 `body`의 여러 컨텐츠 유형을 리턴할 수 있습니다. `--response-type http` 플래그는 `http` 확장자로 웹 액션의 대상 URL을 구성하여 이를 가능하게 합니다.

`http` 확장자로 웹 액션의 리턴을 준수하기 위해 액션의 코드를 변경하거나 결과를 새 액션에 전달하기 위해 시퀀스에 액션을 포함하도록 선택할 수 있습니다. 그리고 새 액션은 HTTP 응답에 대해 적절히 형식화되도록 결과를 변환할 수 있습니다. [웹 액션](./openwhisk_webactions.html) 문서에서 응답 유형 및 웹 액션 확장자에 대해 자세히 알아볼 수 있습니다.

1. JSON 특성 `body`, `statusCode` 및 `headers`를 리턴하는 `hello.js`에 대한 코드를 변경하십시오.
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
  ibmcloud wsk action update hello hello.js --web true
  ```
  {: pre}

3. `--response-type http` 플래그를 사용하여 API 응답 유형을 업데이트하십시오.
  ```
  ibmcloud wsk api create /hello /world get hello --response-type http
  ```
  {: pre}

4. 다음 **curl** 명령을 사용하여 업데이트된 API를 호출하십시오.
  ```bash
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

이제 API의 전체 제어가 가능하며 컨텐츠를 제어할 수 있습니다. HTML 리턴을 원하거나, 찾을 수 없음(404), 권한 없음(401) 또는 심지어 내부 오류(500) 등에 대한 상태 코드를 설정합니다.

### 다중 웹 액션 노출
{: #multiple_web_actions}

예를 들어, 북 클럽에 대한 액션 세트를 노출하려면 일련의 액션을 사용하여 북 클럽에 대한 백엔드를 구현할 수 있습니다.

|액션 |HTTP 메소드 |설명 |
| ----------- | ----------- | ------------ |
|getBooks    |GET |서적 세부사항 가져오기  |
|postBooks   |POST |서적 추가 |
|putBooks    |PUT |서적 세부사항 업데이트 |
|deleteBooks |DELETE |서적 삭제 |

이 예제에서 API는 **경로 매개변수**로 정의됩니다. 경로 매개변수를 사용하는 경우 API를 `http`의 응답 유형으로 정의해야 합니다. 기본 경로로 시작하며 실제 경로 매개변수값을 포함하는 경로 값은 액션의 JSON 매개변수의 `__ow_path` 필드에서 사용 가능합니다. `http` 응답 유형으로 호출된 웹 액션에서 사용 가능한 추가 HTTP 컨텍스트 필드에 대한 정보를 포함한 세부사항은 [웹 액션 HTTP 컨텍스트](./openwhisk_webactions.html#http-context) 문서를 참조하십시오.

1. `/club`을 HTTP URL 기본 경로로, `books`를 리소스로, `{isbn}`을 ISBN(International Standard Book Number)을 사용하여 특정 서적을 식별하는 데 사용되는 경로 매개변수로 사용하여 북 클럽 **Book Club**에 대한 API를 작성하십시오.
  ```bash
  ibmcloud wsk api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud wsk api create /club /books get getBooks                       --response-type http
  ibmcloud wsk api create /club /books post postBooks                     --response-type http
  ibmcloud wsk api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud wsk api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: codeblock}

  기본 경로 `/club`으로 노출된 첫 번째 액션이 **Book Club**으로 이름 지정된 API 레이블을 가져온다는 점에 유념하십시오. `/club` 아래에서 노출된 기타 액션은 이제 **Book Club**과 연관됩니다.

2. 다음 명령을 사용하여 노출된 모든 **Book Club** 액션을 나열하십시오.
  ```
  ibmcloud wsk api list /club -f
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

3. 재미 삼아서 HTTP __POST__로 **JavaScript: The Good Parts** 서적을 추가할 수 있습니다.
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

4. HTTP __GET__을 통해 **getBooks** 액션을 사용하여 서적 목록을 가져오십시오.
  ```bash
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

5. HTTP __DELETE__를 통해 **deleteBooks** 액션을 사용하여 특정 서적을 삭제할 수 있습니다. 이 예제에서 **deleteBooks** 액션의 `__ow_path` 필드 값은 `/club/books/978-0596517748`이며, 여기서 `978-0596517748`은 경로의 `{isbn}` 실제 값입니다.
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

### 구성 내보내기
{: #export_config}

1. 입력으로서 파일을 사용하여 API를 재작성하기 위한 기반으로 사용될 수 있는 파일로 이름이 **Book Club**인 API를 내보내십시오.
  ```
  ibmcloud wsk api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. 먼저 다음 명령으로 공통 기본 경로 아래의 노출된 모든 URL을 삭제하여 Swagger 파일을 테스트하십시오.
  ```
  ibmcloud wsk api delete /club
  ```
  {: pre}

  출력 예:
  ```
ok: deleted API /club
  ```
  {: screen}

  기본 경로 `/club` 또는 API 이름 레이블 **"Book Club"**을 사용하여 노출된 URL을 모두 삭제할 수 있습니다.
  {: tip}

### 구성 가져오기
{: #import_config}

1. 이제 **club-swagger.json** 파일을 사용하여 `Book Club`으로 이름 지정된 API를 복원하십시오.
  ```
  ibmcloud wsk api create --config-file club-swagger.json
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

2. **Book Club** API가 재작성되었는지 확인하십시오.
  ```
  ibmcloud wsk api list /club
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

### UI를 사용하여 구성 수정
{: #modify_config}

{{site.data.keyword.openwhisk_short}} 대시보드에서 구성을 편집하고 [API 탭](https://console.ng.bluemix.net/openwhisk/apimanagement)을 클릭하여 보안, 속도 제한 및 기타 기능을 설정할 수 있습니다. 구성을 업데이트하면 정의 파일을 JSON 형식으로 다운로드한 다음 CLI를 사용하여 다시 가져올 수 있습니다. 이는 예를 들어 지속적 통합 및 배치(CICD) 파이프라인의 무인 배치에서 유용할 수 있습니다. 또한 UI를 사용하여 API 정의 파일을 업로드하고 다시 가져오는 옵션이 있습니다.
