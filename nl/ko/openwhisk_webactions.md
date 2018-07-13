---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 웹 액션
{: #openwhisk_webactions}

웹 액션은 빠르게 개발자가 웹 기반 애플리케이션을 빌드할 수 있도록 하기 위한 어노테이션이 작성된 OpenWhisk 액션입니다. 이러한 어노테이션이 있는 액션으로 개발자는 OpenWhisk 인증 키를 요구하지 않고도 웹 애플리케이션이 익명으로 액세스할 수 있는 백엔드 로직을 프로그래밍할 수 있습니다. 원하는 인증과 권한(즉, OAuth 플로우)을 구현하는 것은 액션 개발자의 역할입니다.
{: shortdesc}

웹 액션 활성화는 액션을 작성한 사용자와 연관되어 있습니다. 이 액션은 호출자의 액션 활성화 비용을 액션의 소유자에게 위임합니다.

다음 Javascript 액션 `hello.js`를 보십시오.
```javascript
function main({name}) {
  var msg = 'you did not tell me who you are.';
  if (name) {
    msg = `hello ${name}!`
  }
  return {body: `<html><body><h3>${msg}</h3></body></html>`}
}
```
{: codeblock}  

`true` 또는 `yes` 값으로 CLI `--web` 플래그를 사용하여 `guest` 네임스페이스에 대한 `demo` 패키지에서 _웹 액션_ **hello**를 작성할 수 있습니다.
```
ibmcloud wsk package create demo
```
{: pre}

```
ibmcloud wsk action create /guest/demo/hello hello.js --web true
```
{: pre}

`true` 또는 `yes` 값으로 `--web` 플래그를 사용하면 신임 정보를 요구하지 않고도 REST 인터페이스를 통해 액션에 액세스할 수 있습니다. 신임 정보를 사용하여 웹 액션을 구성하려면 [웹 액션 보안](./openwhisk_webactions.html#securing-web-actions) 절을 참조하십시오. 웹 액션은 다음과 같이 구성된 URL을 사용하여 호출될 수 있습니다.
`https://{APIHOST}/api/v1/web/{namespace}/{packageName}/{actionName}.{EXT}`.

액션이 이름 지정된 패키지에 없는 경우, 패키지 이름은 **default**입니다.

`guest/demo/hello`가 예입니다. 웹 액션 API 경로는 API 키 없이 `curl` 또는 `wget`에서 사용될 수 있습니다. 브라우저에서 이를 직접 입력할 수도 있습니다.

웹 브라우저에서 [https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane](https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane)을 열어 보십시오. 또는 `curl`을 사용하여 액션 호출을 시도하십시오.
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane
```
{: pre}

다음 예제에서 웹 액션은 HTTP 경로 재지정을 수행합니다.
```javascript
function main() {
  return {
    headers: { location: 'http://openwhisk.org' },
    statusCode: 302
  }
}
```
{: codeblock}    

다음 예제에서 웹 액션은 단일 쿠키를 설정합니다.
```javascript
function main() {
  return {
    headers: {
      'Set-Cookie': 'UserID=Jane; Max-Age=3600; Version=',
      'Content-Type': 'text/html'
    }, 
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
}
```
{: codeblock}  

다음 예제에서 웹 액션은 다중 쿠키를 설정합니다.
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

다음 예제는 `image/png`를 리턴합니다.
```javascript
function main() {
    let png = <base 64 encoded string>
    return { headers: { 'Content-Type': 'image/png' },
             statusCode: 200,
             body: png };
}
```
{: codeblock}  

The following example returns `application/json`:
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

HTTP 응답에 대한 기본 `Content-Type`은 `application/json`이며 본문에서는 JSON 값을 사용할 수 있습니다. 헤더에서는 기본 `Content-Type`을 생략할 수 있습니다.

[응답 크기 한계](./openwhisk_reference.html)(액션에 대한)를 알아야 합니다. 사전 정의된 시스템 한계를 초과하는 응답이 실패하기 때문입니다. 대형 오브젝트는 OpenWhisk를 통해 인라인으로 전송되지 않지만, 대신 오브젝트 저장소를 따릅니다. 예를 들면, 다음과 같습니다.

## 액션에서 HTTP 요청 처리
{: #openwhisk_webactions_http}

웹 액션이 아닌 OpenWhisk 액션은 두 인증을 모두 요구하며 JSON 오브젝트로 응답해야 합니다. 이와는 대조적으로 웹 액션은 인증 없이 호출될 수 있으며, 다양한 유형의 _headers_, _statusCode_ 및 _body_ 컨텐츠로 응답하는 HTTP 핸들러를 구현하는 데 사용될 수 있습니다. 웹 액션은 JSON 오브젝트를 리턴해야 합니다. 그러나 해당 결과에 하나 이상의 다음 최상위 레벨 JSON 특성이 포함된 경우에는 OpenWhisk 시스템(즉, `controller`)이 웹 액션을 다르게 처리합니다.

- `headers`: 키가 header-name이며 값이 해당 헤더(기본값은 헤더 없음)에 대해 문자열, 숫자 또는 부울 값인 JSON 오브젝트입니다. 단일 헤더에 대해 다중 값을 전송하려면 헤더의 값이 JSON 배열 값이어야 합니다.
- `statusCode`: 유효한 HTTP 상태 코드입니다(기본값: 200 OK).
- `body`: 일반 텍스트 또는 base64 인코딩된 문자열(2진 데이터의 경우)인 문자열입니다.

제어기는 요청/응답을 종료하는 HTTP 클라이언트에 액션 지정 헤더(있는 경우)를 전달합니다. 이와 유사하게 제어기는 상태 코드로 응답합니다(존재하는 경우). 마지막으로, 본문은 응답의 본문으로 전달됩니다. `Content-Type` 헤더가 액션 결과의 `headers`에 선언되지 않는 한, 문자열인 경우 본문은 있는 그대로 전달됩니다(그 외의 경우에는 결과적으로 오류가 발생함). `Content-Type`이 정의된 경우, 제어기는 응답이 2진 데이터인지 또는 일반 텍스트인지 여부를 판별하며 필요에 따라 base64 디코더를 사용하여 문자열을 디코드합니다. 본문의 올바른 디코드에 실패하면 오류가 호출자에게 리턴됩니다.

_참고_: JSON 오브젝트 또는 배열은 2진 데이터로서 처리되며 base64 인코딩되어야 합니다.

## HTTP 컨텍스트
{: #http-context}

호출 시에 모든 웹 액션은 액션 입력 매개변수에 대한 매개변수로서 HTTP 요청 세부사항을 수신합니다.

다음과 같은 HTTP 매개변수를 참조하십시오.

- `__ow_method`(유형: 문자열). 요청의 HTTP 메소드입니다.
- `__ow_headers`(유형: 문자열 대 문자열 맵핑): 요청 헤더입니다.
- `__ow_path`(유형: 문자열): 요청의 일치하지 않는 경로입니다(일단 액션 확장자가 이용되면 일치가 중지됨).
- `__ow_user`(유형: 문자열): OpenWhisk 인증 제목을 식별하는 네임스페이스입니다.
- `__ow_body`(유형: 문자열): base64 인코딩된 문자열(컨텐츠가 2진인 경우) 또는 일반 문자열(그 외의 경우)로서의 요청 본문 엔티티입니다.
- `__ow_query`(유형: 문자열): 구문 분석되지 않은 문자열로서의 요청의 조회 매개변수입니다.

요청은 이름 지정된 `__ow_` 매개변수를 대체할 수 없습니다. 이를 수행하면 결과적으로 "400 잘못된 요청"과 동일한 상태로 요청이 실패합니다.

`__ow_user`는 웹 액션에 [인증을 요구하는 어노테이션이 있는](./openwhisk_annotations.html#annotations-specific-to-web-actions) 경우에만 존재하며, 웹 액션이 자체 권한 부여 정책을 구현할 수 있도록 합니다. `__ow_query`는 웹 액션이 ["원시" HTTP 요청](#raw-http-handling)을 처리하도록 선택한 경우에만 사용 가능합니다. 이는 (`&`로 분리된) URI에서 구문 분석된 조회 매개변수가 포함된 문자열입니다. `__ow_body` 특성은 "원시" HTTP 요청에 존재하거나 HTTP 요청 엔티티가 JSON 오브젝트 또는 양식 데이터가 아닌 경우에 존재합니다. 그렇지 않으면, 웹 액션이 액션 인수의 첫 번째 클래스 특성으로서 조회 및 본문 매개변수를 수신합니다. 본문 매개변수는 조회 매개변수에 우선하며, 조회 매개변수는 다시 액션 및 패키지 매개변수에 우선합니다.

## HTTPS 엔드포인트 지원

지원되는 SSL 프로토콜: TLS 1.0, TLS 1.1, TLS 1.2, TLS 1.3([드래프트 버전 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18))

미지원 SSL 프로토콜: SSLv2, SSLv3

## 추가 기능
{: #extra-features}

웹 액션은 다음을 포함하는 추가 기능을 제공합니다.

- `Content extensions`: 요청이 원하는 컨텐츠 유형을 `.json`, `.html`, `.http`, `.svg` 또는 `.text`로 지정해야 합니다. `/guest/demo/hello` 액션이 `/guest/demo/hello.http`로서 참조되도록(예: HTTP 응답을 다시 수신하기 위해) 유형은 확장자를 URI의 액션 이름에 추가하여 지정됩니다. 편의상, 확장자를 찾을 수 없으면 `.http` 확장자가 가정됩니다.
- `Projecting fields from the result`: 액션 이름을 따르는 경로를 사용하여 응답의 레벨을 하나 이상 투영합니다.
`/guest/demo/hello.html/body`. 이 기능은 `{body: "..." }` 사전을 리턴하는 액션이 `body` 특성을 보호하고 해당 문자열 값을 대신 직접 리턴하도록 허용합니다. 투영된 경로는 절대 경로 모델(XPath에서와 같이)을 따릅니다.
- `Query and body parameters as input`: 액션이 요청 본문에서 매개변수는 물론 조회 매개변수를 수신합니다. 매개변수 병합의 우선순위는 패키지 매개변수, 액션 매개변수, 조회 매개변수 및 본문 매개변수입니다. 이러한 각 매개변수는 겹침이 발생할 때 이전 값을 대체할 수 있습니다. 예를 들어, `/guest/demo/hello.http?name=Jane`은 `{name: "Jane"}` 인수를 액션에 전달할 수 있습니다.
- `Form data`: 표준 `application/json`에 외에 웹 액션은 입력으로서 `application/x-www-form-urlencoded data` 데이터에서 인코딩된 URL을 수신할 수 있습니다.
- `Activation using multiple HTTP verbs`: 웹 액션이 `GET`, `POST`, `PUT`, `PATCH` 및 `DELETE`는 물론 `HEAD` 및 `OPTIONS` 등의 HTTP 메소드를 통해 호출될 수 있습니다.
- `Non JSON body and raw HTTP entity handling`: 웹 액션은 JSON 오브젝트가 아닌 HTTP 요청 본문을 수락할 수 있으며, 항상 해당 값을 오파크 값으로 수신하도록 선택할 수 있습니다(2진이 아닌 경우에는 일반 텍스트고 그 외의 경우에는 base64 인코딩된 문자열).

다음 예제는 웹 액션에서 이러한 기능을 사용할 수 있는 방법을 간략하게 보여줍니다. 다음 본문의 `/guest/demo/hello` 액션을 고려하십시오.
```javascript
function main(params) {
    return { response: params };
}
```

이 액션이 웹 액션으로서 호출된 경우, 결과에서 서로 다른 경로를 투영하여 웹 액션의 응답을 변경할 수 있습니다.

예를 들어, 전체 오브젝트를 리턴하고 액션이 수신하는 인수를 보려면 다음을 수행하십시오.
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json
 ```
{: pre}

출력 예:
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

조회 매개변수로 실행하려면 다음 예제 명령을 참조하십시오.
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json?name=Jane
 ```
{: pre}

출력 예:
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

양식 데이터로 실행할 수도 있습니다.
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -d "name":"Jane"
 ```
{: pre}

출력 예:
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

JSON 오브젝트에 대해 다음 명령을 실행하십시오.
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
```
{: pre}

출력 예:
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

다음 명령을 실행하여 (텍스트로서) 이름을 투영하십시오.
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.text/response/name?name=Jane
```
{: pre}

출력 예:
```
Jane
```
{: screen}

편의상 조회 매개변수, 양식 데이터 및 JSON 오브젝트 본문 엔티티는 모두 사전으로서 처리되며, 해당 값은 액션 입력 특성으로서 바로 액세스가 가능합니다. 이 동작은 HTTP 요청 엔티티의 보다 직접적인 처리를 선택하는 웹 액션의 경우 또는 웹 액션이 JSON 오브젝트가 아닌 엔티티를 수신하는 경우에는 해당되지 않습니다.

이전에 표시된 대로 "텍스트" 컨텐츠 유형을 사용하는 다음 예제를 참조하십시오.
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
```
{: pre}

출력 예:
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

## 컨텐츠 확장자
{: #openwhisk_webactions_extensions}

컨텐츠 확장자는 일반적으로 웹 액션을 호출하는 데 필요합니다. 확장자가 없으면 기본적으로 `.http`라고 가정합니다. `.json` 및 `.http` 확장자에서는 투영 경로가 필요하지 않지만, `.html`, `.svg` 및 `.text` 확장자에서는 필요합니다. 편의상 기본 경로는 확장자 이름과 일치한다고 가정합니다. 웹 액션을 호출하고 `.html` 응답을 수신하려면, 액션이 `html`이라고 하는 최상위 레벨 특성이 포함된 JSON 오브젝트로 응답해야 합니다(또는 응답이 명시적 경로에 있어야 함). 다시 말하면, `/guest/demo/hello.html`은 `/guest/demo/hello.html/html`에서와 같이 `html` 특성의 명시적 투영과 동등합니다. 액션의 완전한 이름에는 해당 패키지 이름이 포함되어야 합니다. 액션이 이름 지정된 패키지에 없는 경우, 이 이름은 `default`입니다.

## 보호된 매개변수
{: #openwhisk_webactions_protected}

액션 매개변수는 불변으로서 처리되고 보호됩니다. 매개변수는 웹 액션을 사용하도록 자동으로 최종 처리됩니다.
```
ibmcloud wsk action create /guest/demo/hello hello.js --parameter name Jane --web true
```
{: pre}

이러한 변경의 결과로 `name`은 `Jane`에 바인드되며. 이는 최종 어노테이션 때문에 조회 또는 본문 매개변수로 대체될 수 없습니다. 이 디자인은 우연하든 또는 의도적이든 여부와 무관하게 이 값의 변경을 시도하는 조회 또는 본문 매개변수에 대해 액션을 보호합니다.

## 웹 액션 보안
{: #securing-web-actions}

기본적으로 웹 액션은 웹 액션의 호출 URL을 가진 사용자가 호출할 수 있습니다. `require-whisk-auth` [웹 액션 어노테이션](./openwhisk_annotations.html#annotations-specific-to-web-actions)을 사용하여 웹 액션을 보안하십시오. `require-whisk-auth` 어노테이션이 `true`로 설정된 경우 액션이 액션 소유자의 whisk 인증 키에 대해 호출 요청의 기본 권한 부여 신임 정보를 인증합니다. 숫자 또는 대소문자 구분 문자열로 설정된 경우 액션의 호출 요청에 이 동일한 값을 갖는 `X-Require-Whisk-Auth` 헤더가 포함되어야 합니다. 신임 정보 유효성 검증이 실패하면 보안된 웹 액션이 메시지 `Not Authorized`를 리턴합니다.

또는 `--web-secure` 플래그를 사용하여 `require-whisk-auth` 어노테이션을 자동으로 설정하십시오. `true`로 설정되면 난수가 `require-whisk-auth` 어노테이션 값으로 생성됩니다. `false`로 설정된 경우에는 `require-whisk-auth` 어노테이션이 제거됩니다. 다른 값으로 설정되면 이 값은 `require-whisk-auth` 어노테이션 값으로 사용됩니다.

**--web-secure** 사용 예:
```bash
ibmcloud wsk action update /guest/demo/hello hello.js --web true --web-secure my-secret
```
{: pre}

**require-whisk-auth** 사용 예:
```bash
ibmcloud wsk action update /guest/demo/hello hello.js --web true -a require-whisk-auth my-secret
```
{: pre}

**X-Require-Whisk-Auth** 사용 예:
```bash
curl https://${APIHOST}/api/v1/web/guest/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: my-secret"
```
{: pre}

웹 액션의 소유자가 모든 활성화 레코드를 소유하며 액션 호출 방법에 관계없이 시스템에서 액션을 실행하는 비용을 발생시킨다는 점을 유념해야 합니다.

## 웹 액션 사용 안함

웹 API(`https://openwhisk.bluemix.net/api/v1/web/`)를 통해 앱 액션이 호출되지 않도록 하려면 `false` 또는 `no` 값을 `--web` 플래그에 전달하여 CLI에서 액션을 업데이트하십시오.
```
ibmcloud wsk action update /guest/demo/hello hello.js --web false
```
{: pre}

## 원시 HTTP 처리

웹 액션은 액션 입력에 사용 가능한 첫 번째 클래스 특성으로 JSON 오브젝트를 승격하지 않고 수신 HTTP 본문을 직접 해석하고 처리하도록 선택할 수 있습니다(예: `args.name` 대 `args.__ow_query` 구문 분석). 이 프로세스는 `raw-http` [어노테이션](./openwhisk_annotations.html)을 통해 수행됩니다. 이전에 나타난 동일한 예제를 사용하되 이제는 `name`을 수신하는 "원시" HTTP 웹 액션으로서(둘 다 조회 매개변수로서), 그리고 HTTP 요청 본문의 JSON 값으로서 다음을 수행하십시오.
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}

출력 예:
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

OpenWhisk는 [Akka Http](http://doc.akka.io/docs/akka-http/current/scala/http/) 프레임워크를 사용하여 2진인 컨텐츠 유형과 일반 텍스트인 컨텐츠 유형을 [판별](http://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html)합니다.

### 원시 HTTP 처리 사용

원시 HTTP 웹 액션은 `raw` 값을 사용하여 `--web` 플래그를 통해 사용됩니다.
```
ibmcloud wsk action create /guest/demo/hello hello.js --web raw
```
{: pre}

### 원시 HTTP 처리 사용 안함

원시 HTTP 사용 안함은 `false` 또는 `no` 값을 `--web` 플래그에 전달하여 완료됩니다.
```
ibmcloud wsk update create /guest/demo/hello hello.js --web false
```
{: pre}

### Base64에서 2진 본문 컨텐츠 디코드

원시 HTTP 컨텐츠가 처리되는 경우, 요청 `Content-Type`이 2진이면 `__ow_body` 컨텐츠는 Base64로 인코딩됩니다. 다음 함수는 Node, Python 및 Swift에서 본문 컨텐츠를 디코드하는 방법을 보여줍니다. 단순히 메소드를 파일에 저장하고 저장된 아티팩트를 활용하는 원시 HTTP 웹 액션을 작성한 후에 웹 액션을 호출하십시오.

#### 노드

```javascript
function main(args) {
    decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
    return {body: decoded}
}
```
{: codeblock}

#### Python

```python
def main(args):
    try:
        decoded = args['__ow_body'].decode('base64').strip()
        return {"body": decoded}
    except:
        return {"body": "Could not decode body from Base64."}
```
{: codeblock}

#### Swift

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

예를 들어, Node 함수를 `decode.js`로서 저장하고 다음 명령을 실행하십시오.
```
ibmcloud wsk action create decode decode.js --web raw
```
{: pre}

출력 예:
```
ok: created action decode
```
{: screen}

```
curl -k -H "content-type: application" -X POST -d "Decoded body" https:// openwhisk.ng.bluemix.net/api/v1/web/guest/default/decodeNode.json
```
{: pre}

출력 예:
```
{
  "body": "Decoded body"
}
```
{: screen}

## 옵션 요청
{: #options-requests}

기본적으로, 웹 액션에 대해 작성된 OPTIONS 요청의 결과로 응답 헤더에 자동으로 추가되는 CORS 헤더가 생성됩니다. 이러한 헤더는 모든 원본 및 options, get, delete, post, put, head 및 patch HTTP verb를 허용합니다.

다음 헤더를 참조하십시오.
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

또는 OPTIONS 요청이 웹 액션에 의해 수동으로 처리될 수 있습니다. 이 옵션을 사용하려면 `true` 값의
`web-custom-options` 어노테이션을 웹 액션에 추가하십시오. 이 기능을 사용하면 CORS 헤더가 요청 응답에 자동으로 추가되지 않습니다. 그 대신, 원하는 헤더를 프로그래밍 방식으로 추가하는 것은 개발자의 책임입니다.

다음 예제를 참조하여 OPTIONS 요청에 대한 사용자 정의 응답을 작성하십시오.
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

함수를 `custom-options.js`에 저장하고 다음 명령을 실행하십시오.
```
ibmcloud wsk action create custom-option custom-options.js --web true -a web-custom-options true
```
{: pre}

```
$ curl https://${APIHOST}/api/v1/web/guest/default/custom-options.http -kvX OPTIONS
```
{: pre}

출력 예:
```
< HTTP/1.1 200 OK
< Server: nginx/1.11.13
< Content-Length: 0
< Connection: keep-alive
< Access-Control-Allow-Methods: OPTIONS, GET
< Access-Control-Allow-Origin: example.com
```
{: screen}

## 오류 핸들링
{: #openwhisk_webactions_errors}

{{site.data.keyword.openwhisk_short}} 액션은 두 개의 서로 다른 가능한 실패 모드로 실패합니다. 첫 번째는 _애플리케이션 오류_라고 하며, 이는 발견된 예외와 유사합니다. 액션에서 최상위 레벨 `error` 특성이 포함된 JSON 오브젝트를 리턴합니다. 두 번째는 _개발자 오류_입니다. 이는 액션의 갑작스런 실패 시에 발생하며 응답을 생성하지 않습니다(미발견 예외와 유사함). 웹 액션의 경우, 제어기는 애플리케이션 오류를 다음과 같이 처리합니다.

- 지정된 경로 투영은 무시되며 제어기가 `error` 특성을 대신 투영합니다.
- 제어기가 액션 확장자에 내재된 컨텐츠 처리를 `error` 특성의 값에 적용합니다.

개발자는 웹 액션이 사용되고 적합한 오류 응답을 생성하는 방법을 알아야 합니다. 예를 들어, `.http` 확장자로 사용된 웹 액션은 `{error: { statusCode: 400 }`과 같은 HTTP 응답을 리턴합니다. 이렇게 되지 못하면, 확장자의 내재된 `Content-Type`과 오류 응답의 액션 `Content-Type` 간에 불일치가 발생합니다. 필요할 때 시퀀스를 구성하는 컴포넌트가 적절한 오류를 생성할 수 있도록 시퀀스인 웹 액션에 대해 특별한 고려가 필요합니다.
