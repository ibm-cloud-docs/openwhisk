---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# API 게이트웨이
{: #openwhisk_apigateway}

OpenWhisk 액션은 API 관리로 관리됨으로써 이점을 얻을 수 있습니다. 

API 게이트웨이는 [웹 조치](./openwhisk_webactions.html)에 대한 프록시의 역할을 하며 추가 기능을 이에 제공합니다. 추가 기능에는 HTTP 메소드 라우팅, 클라이언트 ID/시크릿, 속도 제한, CORS, API 사용법 보기, 응답 로그 보기 및 API 공유 정책 등이 포함됩니다.
API 관리에 대한 자세한 정보를 얻기 위해 [API 관리 문서](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis)를 읽을 수 있습니다.
{: shortdesc}

## 브라우저를 사용하여 OpenWhisk 웹 액션에서 API를 작성하십시오. 

API 게이트웨이로 OpenWhisk 액션을 API로서 노출시킬 수 있습니다. API를 정의한 후에는 보안 및 속도 제한 정책을 적용하고 API 사용법 및 응답 로그를 보며 API 공유 정책을 정의할 수 있습니다.
OpenWhisk 대시보드에서 [API 탭](https://console.ng.bluemix.net/openwhisk/apimanagement)을 클릭하십시오. 


## CLI를 사용하여 OpenWhisk 웹 액션에서 API 작성

### OpenWhisk CLI 구성

API 호스트에서 OpenWhisk CLI를 구성하십시오. 

자체 고유의 API 호스트 및 권한 부여 키가 필요한 2개의 {{site.data.keyword.Bluemix_notm}} 지역을 사용할 수 있습니다. 

* 미국 남부
  * API 호스트: `openwhisk.ng.bluemix.net`

* 영국
  * API 호스트: `openwhisk.eu-gb.bluemix.net`

다음 명령을 실행하여 원하는 Bluemix 지역에 대한 API 호스트를 설정하십시오. 

미국 남부:
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

영국:
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

권한 부여 키가 지역마다 특정하므로, 지역 전환이 필요한 경우에는 API 호스트 및 권한 부여 키를 둘 다 사용하여 CLI를 재구성해야 합니다.
{: tip}

패키지 및 조치, 규칙 등의 아티팩트는 지역에 특정합니다. 따라서 다중 지역에서 동일 아티팩트를 사용하는 경우에는 각각의 원하는 지역에 이를 배치해야 합니다. 

`wsk api` 명령을 사용할 수 있으려면 CLI 구성 파일 `~/.wskprops`에 Bluemix 액세스 토큰이 포함되어야 합니다. 

액세스 토큰을 가져오려면 다음의 CLI 명령을 사용하십시오. 
```
wsk bluemix login
```
{: pre}

이 명령에 대한 자세한 정보를 보려면 다음을 실행하십시오. 
```
wsk bluemix login -h
```
{: pre}

`wsk bluemix login` 명령이 `BMXLS0202E: You are using a federated user ID, please use one time code to login with option --sso` 오류로 실패하면 `bluemix login`을 사용하여 {{site.data.keyword.Bluemix_notm}} CLI로 로그인한 후에 `wsk bluemix login --sso`를 실행하십시오.
{: tip}

### CLI를 사용하여 첫 번째 API 작성

1. 다음 컨텐츠로 JavaScript 파일을 작성하십시오. 이 예제의 경우에 파일 이름은 'hello.js'입니다. 
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}
  
2. 다음의 Javascript 함수에서 웹 조치를 작성하십시오. 이 예제의 경우에는 조치를 'hello'라고 합니다. 반드시 `--web true` 플래그를 추가하십시오. 
  ```
  wsk action create hello hello.js --web true
  ```
  {: pre}

  ```
  ok: created action hello
  ```
  
3. 응답 유형 `json`으로 기본 경로 `/hello`, 경로 `/world` 및 메소드 `get`의 API를 작성하십시오. 
  ```
  wsk api create /hello /world get hello --response-type json
  ```

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
__GET__ HTTP 메소드를 통해 `hello` 조치를 노출하여 새 URL이 생성됩니다. 
  
4. 마지막으로 URL에 HTTP 요청을 전송하십시오. 
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world?name=OpenWhisk
  ```

  ```json
  {
  "payload": "Hello world OpenWhisk"
  }
  ```

  웹 조치 `hello`가 호출되었으며, 이는 조회 매개변수를 통해 전송된 `name` 매개변수가 포함된 JSON 오브젝트를 다시 리턴합니다. 단순 조회 매개변수 또는 요청 본문을 통해 조치에 매개변수를 전달할 수 있습니다. 웹 조치는 OpenWhisk 권한 부여 API 키 없이 공식으로 조치를 호출할 수 있습니다. 
  
### HTTP 응답에 대한 전체 제어
  
  `--response-type` 플래그는 API 게이트웨이가 프록싱하는 웹 조치의 대상 URL을 제어합니다. `--response-type json`을 사용하면 JSON 형식으로 조치의 전체 결과가 리턴되며 Content-Type 헤더가 자동으로 `application/json`으로 설정됩니다.  
  
  일단 시작된 경우, 사용자는 `statusCode`, `headers` 등의 HTTP 응답 특성에 대한 전체 제어 권한을 보유하고 `body`의 서로 다른 컨텐츠 유형을 리턴하고자 할 수 있습니다. `--response-type http` 플래그는 `http` 확장자로 웹 조치의 대상 URL을 구성하여 이를 가능하게 합니다. 

  `http` 확장자로 웹 조치의 리턴을 준수하도록 조치의 코드 변경 또는 결과를 새 조치에 전달하도록 시퀀스에 조치 포함을 선택할 수 있습니다. 그리고 새 조치는 HTTP 응답에 대해 적절히 형식화되도록 결과를 변환할 수 있습니다. [웹 조치](./openwhisk_webactions.html) 문서에서 응답 유형 및 웹 조치 확장자에 대해 자세히 알아볼 수 있습니다. 

  JSON 특성 `body`, `statusCode` 및 `headers`를 리턴하는 `hello.js`에 대한 코드 변경
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: new Buffer(JSON.stringify({payload:`Hello world ${name}`})).toString('base64'), 
        statusCode:200, 
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}
본문이 문자열이 아닌 `base64`로 인코딩되어 리턴되어야 한다는 점에 유념하십시오. 
  
  수정된 결과로 조치를 업데이트하십시오.  
  ```
  wsk action update hello hello.js --web true
  ```
  {: pre}

  `--response-type http`로 API를 업데이트하십시오.  
  ```
  wsk api create /hello /world get hello --response-type http
  ```
  {: pre}
  
  업데이트된 API를 호출하십시오.  
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  {: pre}

  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
이제 API의 전체 제어가 가능하며 컨텐츠를 제어할 수 있습니다. HTML 리턴을 원하거나, 찾을 수 없음(404), 권한 없음(401) 또는 심지어 내부 오류(500) 등에 대한 상태 코드를 설정합니다. 

### 다중 웹 조치 노출

예를 들어, 북 클럽에 대한 조치 세트를 노출하려면 일련의 조치를 사용하여 북 클럽에 대한 백엔드를 구현할 수 있습니다. 

| 조치 | HTTP 메소드 | 설명 |
| ----------- | ----------- | ------------ |
| getBooks    | GET | 서적 세부사항 가져오기 |
| postBooks   | POST | 서적 추가 |
| putBooks    | PUT | 서적 세부사항 업데이트 |
| deleteBooks | DELETE | 서적 삭제 |

`/club`을 HTTP URL 기본 경로로 사용하고 `books`를 해당 리소스로 사용하여 `Book Club`으로 이름 지정된 북 클럽에 대한 API를 작성하십시오. 
```
wsk api create -n "Book Club" /club /books get getBooks --response-type http
wsk api create /club /books post postBooks              --response-type http
wsk api create /club /books put putBooks                --response-type http
wsk api create /club /books delete deleteBooks          --response-type http
```

기본 경로 `/club`로 노출된 첫 번째 조치가 `Book Club`으로 이름 지정된 API 레이블을 가져온다는 점에 유념하십시오. `/club` 아래에서 노출된 기타 조치는 `Book Club`과 연관됩니다. 

다음 명령을 사용하여 노출된 모든 조치를 나열하십시오. 
```
wsk api list -f
```
{: pre}

```
ok: APIs
Action: getBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: get
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: postBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: post
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: putBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: put
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: deleteBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: delete
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

재미 삼아서 __POST__로 `JavaScript: The Good Parts` 서적을 추가할 수 있습니다. 
```
curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": "success"
}
```

HTTP __GET__을 통해 `getBooks` 조치를 사용하여 목록 가져오기
```
curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
}
```

### 구성 내보내기
입력으로서 파일을 사용하여 API를 재작성하기 위한 기반으로 사용될 수 있는 파일로 이름이 `Book Club`인 API를 내보내십시오.  
```
wsk api get "Book Club" > club-swagger.json
```
{: pre}

우선 공통 기본 경로 아래의 노출된 모든 URL을 삭제하여 Swagger 파일을 테스트하십시오.
기본 경로 `/club` 또는 API 이름 레이블 `"Book Club"`을 사용하여 노출된 모든 항목을 삭제할 수 있습니다. 
```
wsk api delete /club
```
```
ok: deleted API /club
```
### 구성 변경

OpenWhisk 대시보드에서 구성을 편집하고 [API 탭](https://console.ng.bluemix.net/openwhisk/apimanagement)을 클릭하여 보안, 속도 제한 및 기타 기능을 설정할 수 있습니다. 

### 구성 가져오기

이제 `club-swagger.json` 파일을 사용하여 `Book Club`으로 이름 지정된 API를 복원하십시오. 
```
wsk api create --config-file club-swagger.json
```
{: pre}

```
ok: created api /books delete for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books get for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books post for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books put for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

API가 재작성되었는지 확인하십시오. 
```
wsk api list /club
```
{: pre}

```
ok: apis
Action                    Verb         API Name        URL
getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
