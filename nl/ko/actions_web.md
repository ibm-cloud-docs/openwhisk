---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: web actions, serverless, functions

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


# 웹 액션 작성
{: #actions_web}

웹 조치를 작성할 때 결과는 웹 앱에서 조치를 트리거하기 위해 사용할 수 있는 URL입니다.
{: shortdesc}

## 표준 조치가 아닌 웹 조치를 사용하는 이유가 무엇입니까?

### 1. 익명으로 웹 조치 실행

웹 액션 활성화는 액션의 호출자가 아닌 액션을 작성한 사용자와 연관되어 있습니다. 일반적으로 API 호출에서 Github 등의 앱을 호출하려면 특정 사용자 또는 기능 ID에 대한 API 호출에 사용자 이름 및 토큰을 포함시켜야 합니다. 웹 조치를 사용하는 경우 이러한 유형의 신임 정보가 필요하지 않습니다. 인증 정보를 요구하지 않고도 REST 인터페이스를 통 웹 액션에 액세스할 수 있습니다.

웹 조치에서는 사용자 신임 정보를 사용할 필요가 없지만 자체 권한 부여 및 인증 또는 OAuth 플로우를 구현할 수 있습니다. 인증 정보를 사용하여 웹 액션을 구성하려면 [웹 액션 보안](#actions_web_secure)을 참조하십시오.

### 2. 임의 유형의 HTTP 요청 사용

기본적으로, 액션은 `POST` 요청만 허용하지만, 웹 액션이 `GET`, `POST`, `PUT`, `PATCH` 및 `DELETE`는 물론 `HEAD` 및 `OPTIONS` 등의 HTTP 메소드를 통해 호출될 수 있습니다.

### 3. 임의 위치에서 웹 조치 트리거

{{site.data.keyword.openwhisk}} 웹 조치를 작성하는 경우 임의의 웹 기반 앱에서 해당 조치를 호출하기 위한 URL이 생성됩니다. 웹 액션이 아닌 액션은 인증을 필요로 하며 JSON 오브젝트로 응답해야 합니다. 

웹 액션 API 경로는 cURL, `wget`에서 사용하거나 브라우저에 직접 입력할 수 있습니다. 웹 액션은 다음과 같이 구성된 URL을 사용하여 호출될 수 있습니다. `https://<apihost>/api/v1/web/<namespace>/<packageName>/<actionName><ext>`.

### 4. 더 적은 {{site.data.keyword.openwhisk_short}} 엔티티 작성

임의 위치에서 웹 조치를 호출할 수 있기 때문에 트리거 또는 규칙과 같은 다른 {{site.data.keyword.openwhisk_short}} 엔티티를 작성할 필요가 없습니다.

## 웹 액션은 어떻게 작동합니까?

웹 액션은 인증 없이 호출될 수 있으며, 다양한 유형의 `headers`, `statusCode` 및 `body` 컨텐츠로 응답하는 HTTP 핸들러를 구현하는 데 사용될 수 있습니다.

웹 액션은 JSON 오브젝트를 리턴해야 합니다. 그러나 해당 결과에 하나 이상의 다음 최상위 레벨 [JSON 특성](#web_action_properties)이 포함된 경우에는 제어기가 웹 액션을 다르게 처리합니다.
{: shortdesc}

## 사용 가능한 웹 조치의 기능
{: #actions_web_extra}

웹 액션은 다음을 기능을 지원합니다.

|기능 |설명 |
| --- | --- |
|[Content extensions](#extra_features) |HTML 요청에 대한 컨텐츠 유형(예: `.json`, `.html`, `.http`, `.svg` 또는 `.text`)을 지정할 수 있습니다. 컨텐츠 유형이 지정되지 않으면, `.http` 확장자가 가정됩니다. `demo/hello` 액션이 `/demo/hello.svg`로서 참조되도록 확장자를 URI의 액션 이름에 추가하여 컨텐츠 유형을 지정할 수 있습니다. `.json` 및 `.http` 확장자에서는 투영 경로가 필요하지 않지만, `.html`, `.svg` 및 `.text` 확장자에서는 필요합니다. 기본 경로는 확장자 이름과 일치한다고 가정합니다. 웹 액션을 호출하고 `.html` 응답을 수신하려면, 액션이 `html`이라고 하는 최상위 레벨 특성이 포함된 JSON 오브젝트로 응답해야 합니다(또는 응답이 명시적 경로에 있어야 함). 다시 말하면, `/<namespace>/demo/hello.html`은 `/<namespace>/demo/hello.html/html`에서와 같이 `html` 특성의 명시적 투영과 동등합니다. 액션의 완전한 이름에는 해당 패키지 이름이 포함되어야 합니다. 액션이 이름 지정된 패키지에 없는 경우, 이 이름은 `default`입니다. |
|[Projecting fields from the result](#projecting_fields) |액션 이름을 따르는 경로를 사용하여 응답의 레벨을 하나 이상 투영합니다(예: `/demo/hello.html/body`). 이 기능은 사전(예: `{body: "..." }`)을 리턴하는 웹 액션이 `body` 특성을 투영하고 해당 문자열 값을 사전 값 대신 직접 리턴하도록 허용합니다. 투영된 경로는 절대 경로 모델(XPath에서와 같이)을 따릅니다. |
|[Query and body parameters as input](#query_test) |액션이 요청 본문에서 매개변수는 물론 조회 매개변수를 수신합니다. 매개변수 병합의 우선순위는 패키지 매개변수, 액션 매개변수, 조회 매개변수 및 본문 매개변수입니다. 이러한 각 매개변수는 겹침이 발생할 때 이전 값을 대체할 수 있습니다. 예를 들어, `/demo/hello.http?name=Jane`은 `{name: "Jane"}` 인수를 액션에 전달할 수 있습니다. |
|[Form data](#form_data) |표준 `application/json`에 외에 웹 액션은 입력으로서 `application/x-www-form-urlencoded data` 데이터에서 인코딩된 URL을 수신할 수 있습니다. |[Activations using multiple HTTP verbs](#actions_web_options) |웹 액션이 `GET`, `POST`, `PUT`, `PATCH` 및 `DELETE`는 물론 `HEAD` 및 `OPTIONS` 등의 HTTP 메소드를 통해 호출될 수 있습니다. |
|[Non-JSON body and raw HTTP entity handling](#actions_web_raw_enable) |웹 액션은 JSON 오브젝트가 아닌 HTTP 요청 본문을 수락할 수 있으며, 항상 해당 값을 오파크 값으로 수신하도록 선택할 수 있습니다(2진 파일이 아닌 경우에는 일반 텍스트고 그 외의 경우에는 base64 인코딩된 문자열). |

## 웹 액션 작성
{: #actions_web_example}

웹 액션을 작성하려면 다음을 수행하십시오. 

1. 다음 JavaScript 코드를 `hello.js`로 저장하십시오.

  ```javascript
function main({name}) {
    var msg = 'You did not tell me who you are.';
  if (name) {
      msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
  }
  ```
  {: codeblock}

2. `demo` 패키지를 작성하십시오. 명시적으로 지정되지 않는 한 패키지 이름은 `default`입니다.
  ```
ibmcloud fn package create demo
  ```
  {: pre}

3. `hello` 액션을 작성하십시오. 이 예에서는 `packageName/actionName`이 `demo/hello`입니다. `<filepath>` 변수를 `hello.js` 파일의 파일 경로로 대체하고 `--web` 플래그를 `true`로 설정하십시오. 

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

4. 매개변수 없이 `hello` 웹 액션을 호출 또는 테스트하십시오. `<apihost>` 및 `<namespace>` 변수를 대체하십시오. `<apihost>`를 가져오려면 `ibmcloud fn property get --apihost`를 실행하십시오. `<apihost>` 예: `us-south.functions.cloud.ibm.com`

  IAM 사용 네임스페이스의 경우 `<namespace>` 변수를 네임스페이스 ID로 대체하십시오. ID를 가져오려면 `ibmcloud fn namespace get <namespace_name>`을 실행하십시오.
  {: note}

  a. 다음 중 하나를 수행하여 웹 액션을 테스트할 수 있습니다. 
    * 브라우저에서 `https://<apihost>/api/v1/web/<namespace>/demo/hello` 구조를 사용하여 URL을 여십시오.
    * cURL 명령을 사용하여 조치를 테스트하십시오.
      ```
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

    * `wget` 명령을 사용하여 조치를 테스트하십시오.  
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

  b. 액션 코드는 다음 사전을 리턴합니다.
    ```
    {body: `<html><body><h3>${msg}</h3></body></html>`}
    ``` 
    {: screen}
    
  또한 다음 명령을 사용한 `body` 특성만 리턴하여 액션을 테스트할 수도 있습니다.
  {: #projecting_fields}

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello.html/body
    ```
    {: pre}

    **출력 예**

    `<name>` 매개변수가 지정되지 않았으므로 다음 메시지가 리턴됩니다.
    ```
    <html><body><h3>You did not tell me who you are.</h3></body></html>
    ```
    {: screen}

5. 이제 `<name>` 매개변수를 정의하십시오. 다음 중 하나를 수행하여 `<name>` 매개변수로 액션을 테스트하십시오.
  * 브라우저에서 `https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane`을 여십시오. 
  * cURL 명령을 사용하여 조치를 테스트하십시오.

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}
  * `wget` 명령을 사용하여 조치를 테스트하십시오.  
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}

  **출력 예**
  ```
  <html><body><h3>Hello, Jane!</h3></body></html>
  ```
  {: screen}


**다음 단계**

웹 앱에 `hello` 웹 액션에 대한 URL을 추가한 후 테스트하십시오.

### 웹 액션 JSON 특성
{: #web_action_properties}

HTTP 응답에 대한 기본 `Content-Type`은 `application/json`이며, 본문은 허용되는 임의의 JSON 값이 될 수 있습니다. `Content-Type`이 `application/json`이 아닌 경우 조치 코드의 `headers`에 `Content-Type`을 지정해야 합니다.

조치에 대한 [결과 크기 한계](/docs/openwhisk?topic=cloud-functions-limits)에 도달하는 경우 응답이 실패합니다. 조치 결과가 5MB를 초과하는 경우 [오브젝트 저장소](/docs/openwhisk?topic=cloud-functions-pkg_obstorage)를 설정하십시오.

|JSON 특성 |설명 |
| --- | --- |
|`headers` |키가 헤더 이름이고 값이 문자열, 숫자 또는 부울 값인 JSON 오브젝트입니다. 단일 헤더에 대해 다중 값을 전송하려면 헤더의 값이 다중 값의 JSON 배열입니다. 기본적으로 헤더는 설정되지 않습니다. |
|`statusCode` |유효한 HTTP 상태 코드입니다. 본문 컨텐츠가 있는 경우, 기본값은 `200 OK`입니다. 본문 컨텐츠가 없는 경우, 기본값은 `204 No`입니다. |
|`body` |일반 텍스트, JSON 오브젝트나 배열, 또는 base64 인코딩된 문자열(2진 데이터의 경우)인 문자열입니다. `null`, 빈 문자열 `""` 또는 정의되지 않은 경우, 본문이 비어 있는 것으로 간주됩니다. 비어 있는 본문이 기본값입니다. |

[제어기](/docs/openwhisk?topic=cloud-functions-about#about_controller)는 요청 또는 응답을 종료하는 액션별 헤더, 상태 코드 또는 본문을 HTTP 클라이언트로 전달합니다. `Content-Type` 헤더가 액션 결과의 `headers`에서 선언되지 않은 경우, 본문은 빈 문자열 값인 경우 `application/json`로 해석되고 다른 경우 `text/html`로 해석됩니다. `Content-Type` 헤더가 정의된 경우, 제어기는 응답이 2진 데이터인지 또는 일반 텍스트인지 여부를 판별하며 필요에 따라 base64 디코더를 사용하여 문자열을 디코드합니다. 본문이 올바르게 디코딩되지 않은 경우, 클라이언트로 오류가 리턴됩니다.

웹 액션의 소유자가 모든 활성화 레코드를 소유하며 액션 호출 방법에 관계없이 시스템에서 액션을 실행하는 비용을 발생시킵니다.
{: note}

#### 보호된 매개변수
조치 매개변수는 보호되며 조치를 업데이트하는 경우에만 변경할 수 있습니다. 매개변수는 웹 액션을 사용하도록 자동으로 최종 처리됩니다.

```
ibmcloud fn action create /<namespace>/demo/hello hello.js --parameter name Jane --web true
```
{: pre}


이러한 변경의 결과로 `name`은 `Jane`에 바인딩되며. 이는 최종 어노테이션 때문에 조회 또는 본문 매개변수로 대체될 수 없습니다. 이 디자인은 우연하든 또는 의도적이든 여부와 무관하게 이 값의 변경을 시도하는 조회 또는 본문 매개변수에 대해 액션을 보호합니다.

### 웹 액션을 사용하여 HTTP 경로 재지정 수행
{: #http_redirect}
웹 애플리케이션에서 이 기능을 사용하여 사용자를 새 버전의 사이트로 경로 재지정할 수 있습니다.

**시작하기 전에**
[웹 조치 작성](#actions_web_example)의 단계를 완료하여 `demo` 패키지 및 `hello` 웹 조치를 작성하십시오.

HTTP 경로 재지정을 수행하는 웹 액션을 작성하려면 다음을 수행하십시오.

1. 코드를 `hello.js`로 저장하십시오.

  ```javascript
function main() {
    return {
      headers: { location: 'https://cloud.ibm.com/openwhisk/' },
      statusCode: 302
    }
  }
  ```
  {: codeblock}

2. `hello` 웹 액션을 `hello.js` 코드에 대한 새 버전으로 업데이트하십시오. `<filepath>`를 `hello.js` 파일의 파일 경로로 대체하십시오.

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. `hello` 웹 액션을 테스트하십시오. `<apihost>` 및 `<namespace>` 변수를 대체하십시오. 다음 중 하나를 수행하여 웹 액션을 테스트할 수 있습니다.

  * 브라우저에서 `https://<apihost>/api/v1/web/<namespace>/demo/hello` URL을 여십시오. 
  * 다음 cURL 명령을 실행하십시오.
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * 다음 `wget` 명령을 실행하십시오.
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

  **결과 예** 
  
  이 웹 조치 예제는 브라우저를 [{{site.data.keyword.openwhisk_short}} 대시보드](https://cloud.ibm.com/openwhisk/){: external}로 경로 재지정합니다.

### 웹 액션을 사용하여 쿠키 설정
{: #multiple_cookie}
정상적인 로그인 후 웹 애플리케이션에서 이 기능을 사용하여 JSON 웹 토큰을 세션 쿠키로 저장할 수 있습니다.

다중 쿠키를 설정하는 웹 액션을 작성하려면 다음을 수행하십시오.

**시작하기 전에**
[웹 조치 작성](#actions_web_example)의 단계를 완료하여 `demo` 패키지 및 `hello` 웹 조치를 작성하십시오.

1. 코드를 `hello.js`로 저장하십시오.
  ```javascript
function main() {
    return {
      headers: {
        'Set-Cookie': [
          'UserID=Jane; Max-Age=3600; Version=',
        'SessionID=asdfgh123456; Path = /'
        ],
      'Content-Type': 'text/html'
      }, 
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
  }
  ```
  {: codeblock}

2. `hello` 웹 액션을 `hello.js` 코드에 대한 새 버전으로 업데이트하십시오. `<filepath>`를 `hello.js` 파일의 파일 경로로 대체하십시오.

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}
    
3. 액션을 테스트하기 전에 브라우저의 쿠키를 지우십시오.

4. 브라우저에서 URL을 열어 `hello` 웹 액션을 테스트하십시오. `<apihost>` 및 `<namespace>` 변수를 대체하고 `https://<apihost>/api/v1/web/<namespace>/demo/hello`를 여십시오. `<apihost>` 예: `us-south.functions.cloud.ibm.com`

**결과**

브라우저의 개발자 도구에 `UserID=Jane` 및 `SessionID=asdfgh123456` 쿠키가 설정됩니다.


### 웹 액션을 사용하여 이미지 리턴
{: #return_image}
웹 애플리케이션에서 이 기능을 사용하여 사용자 로케일에 따라 국가 플래그의 이미지를 리턴할 수 있습니다.

**시작하기 전에** 

[웹 조치 작성](#actions_web_example)의 단계를 완료하여 `demo` 패키지 및 `hello` 웹 조치를 작성하십시오.

`image/png`를 리턴하는 웹 액션을 작성하려면 다음을 수행하십시오. 

1. 코드를 `hello.js`로 저장하십시오.

  ```javascript
  function main() {
      let png = '<base 64 encoded string';
      return { headers: { 'Content-Type': 'image/png' },
              statusCode: 200,
              body: png };
  }
  ```
  {: codeblock}

2. `hello` 웹 액션을 `hello.js` 코드에 대한 새 버전으로 업데이트하십시오. `<filepath>`를 `hello.js` 파일의 파일 경로로 대체하십시오.

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. cURL 명령을 사용하여 브라우저에서 액션을 테스트하십시오. `<apihost>` 및 `<namespace>` 변수를 대체하십시오. 다음 중 하나를 수행하여 웹 액션을 테스트할 수 있습니다.

  * 브라우저에서 `https://<apihost>/api/v1/web/<namespace>/demo/hello` URL을 여십시오. 
  * 다음 cURL 명령을 실행하십시오.
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * 다음 `wget` 명령을 실행하십시오.
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}



### 웹 조치를 사용하여 JSON 리턴
{: #return_json}
웹 애플리케이션에서 이 기능을 사용하여 사용자 IP 정보에 대한 JSON 오브젝트를 리턴할 수 있습니다.

**시작하기 전에** 

[웹 조치 작성](#actions_web_example)의 단계를 완료하여 `demo` 패키지 및 `hello` 웹 조치를 작성하십시오.

`application/json`을 리턴하는 웹 액션을 작성하려면 다음을 수행하십시오.

1. 코드를 `hello.js`로 저장하십시오.
  ```javascript
  function main(params) {
      return {
          statusCode: 200,
          headers: { 'Content-Type': 'application/json' },
          body: params
      };
  }
  ```
  {: codeblock}

2. `hello` 웹 액션을 `hello.js` 코드에 대한 새 버전으로 업데이트하십시오. `<filepath>`를 `hello.js` 파일의 파일 경로로 대체하십시오.
  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. cURL 명령을 사용하여 브라우저에서 액션을 테스트하십시오. `<apihost>` 및 `<namespace>` 변수를 대체하십시오. 다음 중 하나를 수행하여 웹 액션을 테스트할 수 있습니다.
  * 브라우저에서 `https://<apihost>/api/v1/web/<namespace>/demo/hello` URL을 여십시오. 
  * 다음 cURL 명령을 실행하십시오.
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * 다음 `wget` 명령을 실행하십시오.
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

    **출력 예**

    ```
    {
      "__ow_headers": {
        "accept": "*/*",
        "accept-encoding": "gzip",
        "cdn-loop": "cloudflare",
        "cf-connecting-ip": "XX.XXX.XXX.XXX",
        "cf-ipcountry": "US",
        "cf-ray": "4d9f3e442a86cf28-IAD",
        "cf-visitor": "{\"scheme\":\"https\"}",
        "host": "<apihost>",
        "user-agent": "curl/7.54.0",
        "x-forwarded-for": "XX.XXX.XX.XXX, XX.XXX.XX.XXX",
        "x-forwarded-host": "<apihost>",
        "x-forwarded-port": "443",
        "x-forwarded-proto": "https",
        "x-global-k8fdic-transaction-id": "11fd03071bd0841d3a00f52354ab880f",
        "x-real-ip": "XXX.XX.XX.XX",
        "x-request-id": "11fd03071bd0841d3a00f52354ab880f"
      },
      "__ow_method": "get",
      "__ow_path": ""
    }
    ```
    {: screen}


### HTTP 컨텍스트
{: #actions_web_context}

호출 시에 모든 웹 액션은 액션 매개변수에 대한 입력 매개변수로서 HTTP 요청 세부사항을 수신합니다.

|HTTP 매개변수 |유형 |설명 |
| --- | --- | --- |
|`__ow_method` |문자열 |요청의 HTTP 메소드입니다. |
|`__ow_headers` |문자열 대 문자열 맵핑 |요청 헤더입니다. |
|`__ow_path` |문자열 |요청의 일치하지 않는 경로입니다(일단 액션 확장자가 이용되면 일치가 중지됨). |
|`__ow_user` |문자열 |{{site.data.keyword.openwhisk_short}} 인증 제목을 식별하는 네임스페이스입니다. |
|`__ow_body` |문자열 |base64 인코딩된 문자열(컨텐츠가 2진 파일인 경우) 또는 일반 문자열(그 외의 경우)로서의 요청 본문 엔티티입니다. |
|`__ow_query` |문자열 |구문 분석되지 않은 문자열로서의 요청의 조회 매개변수입니다. |

요청은 이름 지정된 `__ow_` 매개변수를 대체할 수 없습니다. 이를 수행하면 결과적으로 "400 잘못된 요청"과 동일한 상태로 요청이 실패합니다.

`__ow_user`는 웹 액션에 [인증을 요구하는 어노테이션이 있는](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions) 경우에만 존재하며, 웹 액션이 자체 권한 부여 정책을 구현할 수 있도록 합니다. `__ow_query`는 웹 액션이 ["원시" HTTP 요청](#actions_web_raw_enable)을 처리하도록 선택한 경우에만 사용 가능합니다. `__ow_query`는 (`&`로 분리된) URI에서 구문 분석된 조회 매개변수가 포함된 문자열입니다. `__ow_body` 특성은 원시 HTTP 요청에 존재하거나 HTTP 요청 엔티티가 JSON 오브젝트 또는 양식 데이터가 아닌 경우에 존재합니다. 그렇지 않으면, 웹 액션이 액션 인수의 첫 번째 클래스 특성으로서 조회 및 본문 매개변수를 수신합니다. 본문 매개변수는 조회 매개변수에 우선하며, 조회 매개변수는 다시 액션 및 패키지 매개변수에 우선합니다.

### HTTPS 엔드포인트 지원
{: #actions_web_endpoint}

지원되는 SSL 프로토콜: TLS 1.2, TLS 1.3([드래프트 버전 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18){: external})

### 웹 액션의 응답 컨텐츠 변경
{: #extra_features}
[컨텐츠 확장기능](#actions_web_extra)을 사용하여 다른 컨텐츠 유형을 리턴하도록 웹 조치의 응답 컨텐츠를 변경할 수 있습니다.
{: shortdesc}

**시작하기 전에**

[웹 조치 작성](#actions_web_example)의 단계를 완료하여 `demo` 패키지 및 `hello` 웹 조치를 작성하십시오.

응답 웹 조치를 변경하려면 다음 작업을 수행하십시오.

1. 다음 코드를 `hello.js`로 저장하십시오.

  ```javascript
  function main(params) {
      return { response: params };
}
  ```
  {: codeblock}

2. `hello` 웹 액션을 `hello.js` 코드의 새 버전으로 업데이트하십시오. `<filepath>`를 `hello.js` 파일의 파일 경로로 대체하십시오.

  ```bash
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. cURL 명령을 사용하여 브라우저에서 액션을 테스트하십시오. `<apihost>` 및 `<namespace>` 변수를 대체하십시오.

  a. 다음 중 하나를 수행하여 JSON을 리턴하십시오.
    * 웹 브라우저에서 `https://<apihost>/api/v1/web/<namespace>/demo/hello.json`을 여십시오. 
    * 다음 cURL 명령을 실행하십시오.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}
    * 다음 `wget` 명령을 실행하십시오.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}

      **출력 예**

      ```
      {
        "response": {
          "__ow_method": "get",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  b. 조회 매개변수를 사용하여 액션을 테스트하십시오. 다음 중 하나를 수행하여 액션을 테스트할 수 있습니다.
  {: #query_test}

    * 다음 cURL 명령을 실행하십시오.

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

    * 다음 `wget` 명령을 실행하십시오.

        ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

      **출력 예**
      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "get",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  c. 또한 양식 데이터를 사용하여 웹 액션을 테스트할 수도 있습니다. 다음 중 하나를 수행하여 웹 액션을 테스트할 수 있습니다.
  {: #form_data}
  
    * 다음 cURL 명령을 실행하십시오.

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}
      
    * 다음 `wget` 명령을 실행하십시오.
      ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}

      **출력 예**

      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "content-length": "10",
            "content-type": "application/x-www-form-urlencoded",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  d. 다음 명령을 실행하여 JSON 오브젝트를 지정할 수 있습니다. 다음 중 하나를 수행하여 웹 액션을 테스트할 수 있습니다.
    * 다음 cURL 명령을 실행하십시오.
        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}
      
    * 다음 `wget` 명령을 실행하십시오.
        ```
        wget https://<apihost>/api/v1/web/{namespace/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}

      **출력 예**

      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "content-length": "15",
            "content-type": "application/json",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  e. 또한 다음 중 하나를 수행하여 `name` 값을 텍스트로 리턴할 수도 있습니다.
  * 다음 cURL 명령을 실행하십시오.

      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}
  * 다음 `wget` 명령을 실행하십시오.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}

    **출력 예**

    ```
    Jane
    ```
    {: screen}

    표준 액션에서 조회 매개변수, 양식 데이터 및 JSON 오브젝트 본문 엔티티는 모두 사전으로서 처리되며, 해당 값은 액션 입력 특성으로서 바로 액세스가 가능합니다. 이 동작은 HTTP 요청 엔티티를 처리하는 웹 액션의 경우 또는 웹 액션이 JSON 오브젝트가 아닌 엔티티를 수신하는 경우에는 해당되지 않습니다.
    {: note}

  f. 다음 중 하나를 수행하여 `Content-Type`을 설정할 수 있습니다.
  * 다음 cURL 명령을 실행하십시오.  
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}
    
  * 다음 `wget` 명령을 실행하십시오.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}

    **출력 예**

    ```
    {
      "response": {
        "__ow_method": "post",
    "__ow_headers": {
          "accept": "*/*",
          "connection": "close",
          "content-length": "4",
          "content-type": "text/plain",
          "host": "172.17.0.1",
          "user-agent": "curl/7.43.0"
        },
        "__ow_path": "",
        "__ow_body": "Jane"
      }
    }
    ```
    {: screen}

## 웹 액션 보안
{: #actions_web_secure}

**시작하기 전에**
[웹 조치 작성](#actions_web_example)의 단계를 완료하여 `demo` 패키지 및 `hello` 웹 조치를 작성하십시오.

기본적으로 누구나 호출 URL을 사용하여 웹 조치를 호출할 수 있습니다. `require-whisk-auth` [웹 액션 어노테이션](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions)을 사용하여 다음 중 하나를 통해 웹 액션을 보호할 수 있습니다.
  1. `require-whisk-auth` 어노테이션을 `true`로 설정하십시오. `require-whisk-auth` 어노테이션이 `true`로 설정된 경우 웹 액션이 웹 액션 소유자의 whisk 인증 키에 대해 호출 요청의 기본 권한 부여 인증 정보를 인증합니다. 숫자 또는 대소문자 구분 문자열로 설정된 경우, 웹 액션의 호출 요청에 동일한 숫자 또는 대소문자 구분 문자열로 설정된 `X-Require-Whisk-Auth` 헤더를 포함되어야 합니다. 인증 정보 유효성 검증이 실패하면 보안된 웹 액션이 메시지 `Not Authorized`를 리턴합니다.

  2. `--web-secure` 플래그를 사용하여 `require-whisk-auth` 어노테이션이 자동으로 설정되도록 허용하십시오. `--web-secure` 플래그가 `true`로 설정되면, 난수가 `require-whisk-auth` 어노테이션 값으로 생성됩니다. `false`로 설정된 경우에는 `require-whisk-auth` 어노테이션이 제거됩니다.  다른 값으로 설정되면 이 값은 `require-whisk-auth` 어노테이션 값으로 사용됩니다.

보안 웹 조치를 테스트하려면 다음 작업을 수행하십시오.

1. 다음 JavaScript 코드를 `hello.js`로 저장하십시오.
  ```javascript
function main({name}) {
    var msg = 'You did not tell me who you are.';
  if (name) {
      msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
  }
  ```
  {: codeblock}

2. `hello` 웹 액션을 `hello.js` 코드의 새 버전으로 업데이트하고 `--web secure` 플래그를 `true`로 설정하십시오.
  ```bash
  ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true
  ```
  {: pre}

3. `hello` 웹 조치를 가져와서 랜덤하게 생성된 `require-whisk-auth` 값을 확인하십시오.

  ```bash
  ibmcloud fn action get demo/hello
  ```
  {: pre}

    **출력 예**

    `require-whisk-auth` 값이 `7819991076995522`로 설정되었습니다.
    ```
    {
      "namespace": "<namespace>/demo",
      "name": "hello",
      "version": "0.0.34",
      "exec": {
          "kind": "nodejs:10",
          "binary": false
      },
      "annotations": [
          {
              "key": "web-export",
              "value": true
          },
          {
              "key": "raw-http",
              "value": false
          },
          {
              "key": "final",
              "value": true
          },
          {
              "key": "require-whisk-auth",
              "value": 7819991076995522
          },
          {
              "key": "exec",
              "value": "nodejs:10"
          }
      ],
    "limits": {
          "timeout": 60000,
          "memory": 256,
          "logs": 10,
          "concurrency": 1
      },
    "publish": false
}
    ```
    {: screen}

인증이 작동하는지 테스트하려면 다음을 수행하십시오.

1. `X-Require-Whisk-Auth` 매개변수를 설정하지 않고 `hello` 웹 조치를 테스트하여 인증이 필요한지 확인하십시오. 이 테스트에서는 오류가 발생합니다. 다음 중 하나를 수행하여 웹 액션을 테스트할 수 있습니다.

  * cURL 명령을 사용하여 웹 조치를 테스트하십시오.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}
    
  * `wget` 명령을 사용하여 웹 조치를 테스트하십시오.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}

   **출력 예**

    ```
      {
      "code": "4c4423556547f6ac764ee192d4ed27a6",
      "error": "Authentication is possible but has failed or not yet been provided."
    }
    ```
    {: screen}

    `X-Require-Whisk-Auth` 값이 제공되지 않았기 때문에 호출이 실패합니다.
    {: note}

2. 이제 `hello` 웹 조치를 테스트하고 랜덤하게 생성된 `X-Require-Whisk-Auth` 값을 제공하십시오. `<apihost>` 및 `<namespace>` 값을 대체하십시오. `<my-secret>` 값을 3단계에서 작성한 무작위로 생성된 숫자로 대체하십시오. 다음 작업 중 하나를 수행하여 웹 조치를 테스트할 수 있습니다. 
  * cURL 명령을 사용하여 웹 조치를 테스트하십시오.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  * `wget` 명령을 사용하여 웹 조치를 테스트하십시오.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  **출력 예**
    
    ```
    {
    "body": "<html><body><h3>Hello, Jane!</h3></body></html>"
    }
    ```
    {: screen}

사용자 정의 `require-whisk-auth` 값을 사용하여 웹 액션을 테스트하려면 다음을 수행하십시오.

1. `hello` 웹 액션을 자체 `require-whisk-auth` 값으로 업데이트하십시오. 그런 다음 호출 중에 `X-Require-Whisk-Auth` 값을 지정하여 웹 조치를 테스트하십시오.

  a. `require-whisk-auth` 값을 설정하십시오. 여기서 `<my-secret>`은 대소문자를 구분하는 인증 토큰입니다.
    ```bash
    ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true --require-whisk-auth <mysecret>
    ```
    {: pre}
  
  b. 웹 액션을 테스트하고 `<my-secret>` 값을 포함시키십시오. 다음 중 하나를 수행하여 웹 액션을 테스트할 수 있습니다.
  * cURL 명령을 사용하여 웹 조치를 테스트하십시오.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}
  * `wget` 명령을 사용하여 조치를 테스트하십시오.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}


## 원시 HTTP 처리
{: #actions_web_raw}

웹 액션은 웹 액션 입력에 사용 가능한 첫 번째 클래스 특성으로 JSON 오브젝트를 승격하지 않고 수신 HTTP 본문을 직접 해석하고 처리하도록 선택할 수 있습니다(예: `args.name` versus parsing `args.__ow_query`). 이 프로세스는 `raw-http` [어노테이션](/docs/openwhisk?topic=cloud-functions-annotations)을 통해 수행됩니다. 이전에 나타난 동일한 예제를 사용하되 이제는 `name`을 수신하는 "원시" HTTP 웹 액션으로서(둘 다 조회 매개변수로서), 그리고 HTTP 요청 본문의 JSON 값으로서 다음을 수행하십시오.
```bash
curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}


**출력 예**
```
{
  "response": {
    "__ow_method": "post",
    "__ow_query": "name=Jane",
    "__ow_body": "eyJuYW1lIjoiSmFuZSJ9",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "15",      
      "content-type": "application/json",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```
{: screen}

{{site.data.keyword.openwhisk_short}}는 [Akka HTTP](https://doc.akka.io/docs/akka-http/current/?language=scala){: external} 프레임워크를 사용하여 [2진 파일인 컨텐츠 유형과 일반 텍스트인 컨텐츠 유형을 판별합니다](https://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html){: external}.

### 원시 HTTP 처리 사용
{: #actions_web_raw_enable}

`--web`을 `raw`로 설정하여 원시 HTTP 웹 액션을 작성할 수 있습니다.
```bash
ibmcloud fn action create demo/hello /<filepath>/hello.js --web raw
```
{: pre}

### Base64에서 2진 본문 컨텐츠 디코드
{: #actions_web_decode}

원시 HTTP 컨텐츠가 처리되는 경우, 요청 `Content-Type`이 2진 유형이면 `__ow_body` 컨텐츠는 Base64로 인코딩됩니다. 다음 함수는 Node, Python 및 Swift에서 본문 컨텐츠를 디코드하는 방법을 보여줍니다.

1. 샘플 코드를 `decode.<ext>`로 이름 지정된 파일에 선호하는 언어로 저장하십시오. `<ext>`를 선호하는 언어의 샘플 코드에 대한 파일 확장자로 대체하십시오.

  **Node**
  {: #actions_web_decode_js}

  ```javascript
function main(args) {
      decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
      return {body: decoded}
  }
  ```
  {: codeblock}

  **Python**
  {: #actions_web_decode_python}

  ```python
def main(args):
    try:
        decoded = args['__ow_body'].decode('base64').strip()
        return {"body": decoded}
    except:
        return {"body": "Could not decode body from Base64."}
  ```
  {: codeblock}

  **Swift**
  {: #actions_web_decode_swift}

  ```swift
extension String {
      func base64Decode() -> String? {
          guard let data = Data(base64Encoded: self) else {
              return nil
          }

          return String(data: data, encoding: .utf8)
      }
  }

func main(args: [String:Any]) -> [String:Any] {
      if let body = args["__ow_body"] as? String {
          if let decoded = body.base64Decode() {
              return [ "body" : decoded ]
        }
      }

      return ["body": "Could not decode body from Base64."]
  }
  ```
  {: codeblock}

2. 다음 명령을 실행하여 샘플 코드로 원시 HTTP 웹 조치를 작성하십시오. 이 예에서는 Node 함수가 `decode.js`로 저장됩니다. 파일 경로를 `decode` 파일의 파일 경로로 대체하고 사용된 샘플 코드의 확장자와 일치하도록 파일 확장자를 업데이트하십시오.

  ```bash
  ibmcloud fn action create decode <filepath>/decode.js --web raw
  ```
  {: pre}

  **출력 예**
  ```
ok: created action decode
  ```
  {: screen}

3. 다음 cURL 명령을 실행하여 `decode` 액션을 테스트하십시오.
    ```bash
    curl -k -H "content-type: application" -X POST -d "Decoded body" https://<apihost>/api/v1/web/<namespace>/default/decode.json
    ```
    {: pre}

  **출력 예**
    ```
    {
      "body": "Decoded body"
    }
    ```
    {: screen}

## 옵션 요청
{: #actions_web_options}

기본적으로, 웹 액션에 대해 작성된 `OPTIONS` 요청의 결과로 응답 헤더에 자동으로 추가되는 CORS 헤더가 생성됩니다. 이러한 헤더는 모든 원본 및 `OPTIONS`, `GET`, `DELETE`, `POST`, `PUT`, `HEAD` 및 `PATCH` HTTP verb를 허용합니다.
{: shortdesc}

다음 헤더를 참조하십시오.
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

또는 `OPTIONS` 요청이 웹 액션에 의해 수동으로 처리될 수 있습니다. 이 옵션을 사용하려면 `true` 값의
`web-custom-options` 어노테이션을 웹 액션에 추가하십시오. 이 기능을 사용하면 CORS 헤더가 요청 응답에 자동으로 추가되지 않습니다. 대신, 헤더를 프로그래밍 방식으로 추가해야 합니다.

`OPTIONS` 요청에 대한 사용자 정의 응답을 작성하려면 다음을 수행하십시오.

1. `custom-options.js` 파일에 다음 코드를 저장하십시오.

  ```js
function main(params) {
    if (params.__ow_method == "options") {
      return {
        headers: {
          'Access-Control-Allow-Methods': 'OPTIONS, GET',
        'Access-Control-Allow-Origin': 'example.com'
        },
      statusCode: 200
    }
    }
  }
  ```
  {: codeblock}

2. 웹 액션을 작성하십시오. `--web-custom-options`를o `true`로 설정하십시오.

  ```bash
  ibmcloud fn action create custom-option <filepath>/custom-options.js --web true -a web-custom-options true
  ```
  {: pre}

3. 다음 cURL 명령을 사용하여 액션을 테스트하십시오.

  ```bash
  $ curl https://<apihost>/api/v1/web/<namespace>/default/custom-option.http -kvX OPTIONS
  ```
  {: pre}

  **출력 예**
  ```
  < HTTP/1.1 200 OK
< Server: nginx/1.11.13
< Content-Length: 0
< Connection: keep-alive
< Access-Control-Allow-Methods: OPTIONS, GET
< Access-Control-Allow-Origin: example.com
  ```
  {: screen}

## 오류 처리
{: #actions_web_errors}

{{site.data.keyword.openwhisk_short}} 액션은 두 개의 서로 다른 가능한 실패 모드로 실패합니다. 첫 번째는 _애플리케이션 오류_라고 하며, 이는 발견된 예외와 유사합니다. 액션에서 최상위 레벨 `error` 특성이 포함된 JSON 오브젝트를 리턴합니다. 두 번째는 _개발자 오류_입니다. 이는 액션 실패 시에 발생하며 응답을 생성하지 않습니다(미발견 예외와 유사함). 웹 액션의 경우, 제어기는 애플리케이션 오류를 다음과 같이 처리합니다.

- 지정된 경로 투영은 무시되며 제어기가 `error` 특성을 대신 투영합니다.
- 제어기가 액션 확장자에 내재된 컨텐츠 처리를 `error` 특성의 값에 적용합니다.

개발자는 웹 액션이 사용되고 적합한 오류 응답을 생성하는 방법을 알아야 합니다. 예를 들어, `.http` 확장자로 사용된 웹 액션은 `{error: { statusCode: 400 }`과 같은 HTTP 응답을 리턴합니다. 이렇게 되지 못하면, 확장자의 내재된 `Content-Type`과 오류 응답의 액션 `Content-Type` 간에 불일치가 발생합니다. 필요할 때 시퀀스를 구성하는 컴포넌트가 적절한 오류를 생성할 수 있도록 시퀀스인 웹 액션에 대해 특별한 고려가 필요합니다.



## 웹 액션 사용 안함
{: #actions_web_disable}

CLI에서 `--web` 플래그를 `false` 또는 `no`로 설정하여 웹 조치를 사용 안함으로 설정할 수 있습니다. `<packageName>/<actionName>` 및 `<filepath>/<filename>`를 코드 파일의 패키지 이름, 웹 조치 이름, 파일 경로 및 파일 이름으로 대체하십시오.

```bash
ibmcloud fn action update <packageName>/<actionName> <filepath>/<filename> --web false
```
{: pre}



