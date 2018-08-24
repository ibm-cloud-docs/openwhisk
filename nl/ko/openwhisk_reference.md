---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# 시스템 세부사항 및 한계
{: #openwhisk_reference}

다음 절에서는 {{site.data.keyword.openwhisk}} 시스템 및 한계 설정에 대한 기술 세부사항을 제공합니다.
{: shortdesc}

## {{site.data.keyword.openwhisk_short}} 엔티티
{: #openwhisk_entities}

### 네임스페이스 및 패키지
{: #openwhisk_entities_namespaces}

{{site.data.keyword.openwhisk_short}} 액션, 트리거 및 룰은 네임스페이스에, 그리고 가끔은 패키지에 속합니다.

패키지에는 액션 및 피드가 포함될 수 있습니다. 패키지에는 다른 패키지가 포함될 수 없으므로 패키지 중첩은 허용되지 않습니다. 또한 엔티티는 패키지에 포함될 필요가 없습니다.

{{site.data.keyword.Bluemix_notm}}에서 조직+영역 쌍은 {{site.data.keyword.openwhisk_short}} 네임스페이스에 대응됩니다. 예를 들어, 조직 `BobsOrg` 및 영역 `dev`는 {{site.data.keyword.openwhisk_short}} 네임스페이스 `/BobsOrg_dev`에 대응됩니다.

수행할 자격을 갖춘 경우에는 자체 네임스페이스를 작성할 수 있습니다. `/whisk.system` 네임스페이스는 {{site.data.keyword.openwhisk_short}} 시스템과 함께 배포되는 엔티티용으로 예약되어 있습니다.

### 완전한 이름
{: #openwhisk_entities_fullyqual}

엔티티의 완전한 이름은 `/namespaceName[/packageName]/entityName`입니다. `/`를 사용하여 네임스페이스, 패키지 및 엔티티를 구분한다는 점에 유념하십시오. 또한 네임스페이스 앞에는 `/`가 접두부로 지정되어야 합니다.

편의상, 사용자의 *기본 네임스페이스*인 경우에는 네임스페이스가 제외될 수 있습니다.

예를 들어, 기본 네임스페이스가 `/myOrg`인 사용자를 고려하십시오. 다음은 다수의 엔티티 및 이의 별명에 대한 완전한 이름의 예입니다.

|완전한 이름 |별명 |네임스페이스 |패키지 |이름 |
| --- | --- | --- | --- | --- |
|`/whisk.system/cloudant/read` |  |`/whisk.system` |`cloudant` |`read` |
|`/myOrg/video/transcode` |`video/transcode` |`/myOrg` |`video` |`transcode` |
|`/myOrg/filter` |`filter` |`/myOrg` |  |`filter` |

기타 위치 중에서 {{site.data.keyword.openwhisk_short}} CLI를 사용할 때 이 이름 지정 스킴을 사용할 수 있습니다.

### 엔티티 이름
{: #openwhisk_entities_names}

액션, 트리거, 룰, 패키지 및 네임스페이스를 포함하여 모든 엔티티의 이름은 다음 형식을 따르는 문자 시퀀스입니다.

* 첫 번째 문자는 영숫자 문자 또는 밑줄이어야 합니다.
* 후속 문자는 영숫자, 공백 또는 `_`, `@`, `.`, `-` 값일 수 있습니다.
* 마지막 문자는 공백일 수 없습니다.

더 정확히 말하면, 이름은 (Java 메타 문자 구문으로 표현된) 다음의 정규식과 일치해야 합니다. `\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`

## 액션 시맨틱
{: #openwhisk_semantics}

다음 절에서는 {{site.data.keyword.openwhisk_short}} 액션에 대한 세부사항을 설명합니다.

### Statelessness
{: #openwhisk_semantics_stateless}

액션 구현은 Stateless 또는 *idempotent*입니다. 시스템이 이 특성을 적용하지 않으면 액션에서 유지하는 상태를 호출에서 사용할 수 있다는 보장이 없습니다.

더구나 액션의 인스턴스화가 다수 존재할 수 있으며, 각각의 인스턴스화에는 고유 상태가 있습니다. 액션 호출은 이러한 인스턴스화에 디스패치될 수 있습니다.

### 호출 입력 및 출력
{: #openwhisk_semantics_invocationio}

액션에 대한 입력 및 출력은 키-값 쌍의 사전입니다. 키는 문자열이며 값은 유효한 JSON 값입니다.

### 액션의 호출 순서 지정
{: #openwhisk_ordering}

액션의 호출은 순서가 지정되어 있지 않습니다. 사용자가 REST API 또는 명령행에서 액션을 두 번 호출하는 경우, 두 번째 호출이 첫 번째 호출 이전에 실행될 수 있습니다. 액션에 부작용이 있는 경우, 이는 순서와 무관하게 관측될 수 있습니다.

또한 액션이 자동으로 실행된다는 보장은 없습니다. 두 개의 액션이 동시에 실행될 수 있으며 해당 부작용이 개입될 수 있습니다. OpenWhisk는 부작용과 관련하여 특정 동시 일관성 모델을 보장하지 않습니다. 동시성 부작용은 구현에 따라 다릅니다.

### 액션 실행 보증
{: #openwhisk_atmostonce}

호출 요청이 수신되면 시스템은 요청을 기록하고 활성화를 디스패치합니다.

시스템은 수신을 확인하는 (넌블로킹 호출의) 활성화 ID를 리턴합니다.
HTTP 응답을 수신하기 전에 네트워크 장애나 기타 장애가 개입되는 경우에는 {{site.data.keyword.openwhisk_short}}가 요청을 받아서 처리했을 수 있습니다.

시스템은 액션을 한 번만 호출하며, 최종적으로는 다음 4개의 결과 중 하나가 발생합니다.
- *성공*: 액션이 정상적으로 호출되었습니다.
- *애플리케이션 오류*: 액션이 정상적으로 호출되었지만 액션이 의도적으로 오류 값을 리턴했습니다(예: 인수의 전제조건이 충족되지 않았기 때문에).
- *액션 개발자 오류*: 액션이 호출되었지만 비정상적으로 완료되었습니다(예: 액션이 예외를 발견하지 못했거나 구문 오류가 존재함).
- *whisk 내부 오류*: 시스템이 액션을 호출할 수 없습니다.
결과는 다음 절에서 설명하는 대로 활성화 레코드의 `status` 필드에 기록됩니다.

정상적으로 수신되었으며 사용자가 비용 청구될 수 있는 모든 호출에 대해 활성화 레코드가 있습니다.

결과가 *액션 개발자 오류*인 경우에는 액션이 부분적으로 실행될 수 있으며 외견상의 부작용이 나타날 수 있습니다. 사용자는 이러한 부작용이 발생했는지 여부를 확인하고, 필요한 경우 재시도 로직을 실행해야 합니다. 특정 *whisk 내부 오류*는 액션이 실행을 시작하지만 액션이 완료를 알리기 전에 실패함을 표시합니다.

## 활성화 레코드
{: #openwhisk_ref_activation}

각 액션 호출 및 트리거 실행으로 인해 활성화 레코드가 생성됩니다.

활성화 레코드에는 다음과 같은 필드가 포함됩니다.

- *activationId*: 활성화 ID입니다.
- *start* 및 *end*: 활성화의 시작 및 종료를 기록하는 시간소인입니다. 값은 [UNIX 시간 형식](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15)입니다.
- *namespace* 및 `name`: 엔티티의 네임스페이스 및 이름입니다.
- *logs*: 해당 활성화 중에 액션에 의해 생성된 로그의 문자열 배열입니다. 각 배열 요소는 액션에 의한 `stdout` 또는 `stderr`로의 행 출력에 해당하며 로그 출력의 시간과 스트림을 포함합니다. 구조는 다음과 같습니다. `TIMESTAMP STREAM: LOG_OUTPUT`.
- *response*: `success`, `status` 및 `result` 키를 정의하는 사전입니다.
  - *status*: "성공", "애플리케이션 오류", "액션 개발자 오류" 및 "whisk 내부 오류" 값 중 하나일 수 있는 활성화 결과입니다.
  - *success*: 상태가 `"success"`인 경우에만 `true`입니다.
- *result*: 활성화 결과가 포함된 사전입니다. 활성화에 성공한 경우, 결과에는 액션에 의해 리턴된 값이 포함됩니다. 활성화에 실패한 경우에 `result`에는 일반적으로 실패에 대한 설명과 함께 `error` 키가 포함됩니다.

## JavaScript 액션
{: #openwhisk_ref_javascript}

### 함수 프로토타입
{: #openwhisk_ref_javascript_fnproto}

{{site.data.keyword.openwhisk_short}} JavaScript 액션은 Node.js 런타임에서 실행됩니다.

Javascript로 작성된 액션은 단일 파일로 제한되어야 합니다. 파일에는 다수의 함수가 포함될 수 있지만, 액션이 호출될 때 호출되는 함수인 `main` 함수가 관례상 존재해야 합니다. 예를 들어, 다음 예제는 다수의 함수가 있는 액션을 보여줍니다.
```javascript
function main() {
    return { payload: helper() }
}

function helper() {
    return new Date();
}
```
{: codeblock}

액션 입력 매개변수가 JSON 오브젝트 매개변수로 `main` 함수에 전달됩니다. 성공적인 활성화의 결과 또한 JSON 오브젝트이지만 다음 절에서 설명하는 대로 액션 동기 또는 비동기인지에 따라 다르게 리턴됩니다.

### 동기 및 비동기 작동
{: #openwhisk_ref_javascript_synchasynch}

일반적으로 JavaScript 함수는 리턴 후에도 콜백 함수에서 계속 실행됩니다. 이러한 동작을 허용하기 위해 Javascript 액션의 활성화가 *동기* 또는 *비동기*일 수 있습니다.

다음 조건 중 하나에서 main 함수가 종료되면 JavaScript 액션의 활성화는 **동기**입니다.

- main 함수가 `return` 명령문을 실행하지 않고 종료합니다.
- main 함수가 Promise를 *제외한* 값을 리턴하는 `return` 명령문을 실행하여 종료합니다.

동기 액션의 다음 예제를 참조하십시오.

```javascript
// an action in which each path results in a synchronous activation
function main(params) {
  if (params.payload == 0) {
     return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
     return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}

main 함수가 Promise를 리턴하여 종료되는 경우 JavaScript 액션의 활성화는 **비동기**입니다. 이 경우 시스템은 Promise가 이행되거나 거부될 때까지 액션이 계속 실행 중이라고 가정합니다.
새 Promise 오브젝트를 인스턴스화하고 이를 콜백 함수에 전달하여 시작하십시오. 콜백은 둘 다 함수인 두 개의 인수 resolve 및 reject를 취합니다. 모든 비동기 코드는 해당 콜백 내부로 이전됩니다.

다음 예제에서는 resolve 함수를 호출하여 Promise를 이행하는 방법을 볼 수 있습니다.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         resolve({ done: true });
                }, 100);
             })
}
```
{: codeblock}

이 예제는 reject 함수를 호출하여 Promise를 거부하는 방법을 보여줍니다.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 100);
    })
}
```
{: codeblock}

다음 예제에 표시된 대로 일부 입력에서는 액션이 동기이고 나머지 입력에서는 비동기일 수 있습니다.
```javascript
  function main(params) {
     if (params.payload) {
        // asynchronous activation
         return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
                }, 100);
             })
     }  else {
        // synchronous activation
         return {done: true};
      }
}
```
{: codeblock}

활성화가 동기인지 또는 비동기인지와 관계없이 액션의 호출은 블로킹 또는 넌블로킹일 수 있습니다.

### JavaScript 글로벌 Whisk 오브젝트가 제거됨

글로벌 오브젝트 `whisk`가 제거되었습니다. 대체 메소드를 사용하려면 nodejs 액션을 마이그레이션하십시오.
`whisk.invoke()` 및 `whisk.trigger()` 함수의 경우에는 이미 설치된 클라이언트 라이브러리 [openwhisk](https://www.npmjs.com/package/openwhisk)를 사용하십시오.
`whisk.getAuthKey()`의 경우에는 환경 변수 `__OW_API_KEY`에서 API 키 값을 가져올 수 있습니다.
`whisk.error()`의 경우에는 거부된 Promise(즉, Promise.reject)를 리턴할 수 있습니다.

### JavaScript 런타임 환경
{: #openwhisk_ref_javascript_environments}

JavaScript 액션은 Node.js 버전 6 또는 Node.js 버전 8에서 실행될 수 있습니다.
현재 액션은 기본적으로 Node.js 버전 6 환경에서 실행됩니다.
### 사용자 액션으로 npm 패키지를 패키징
Node.js 환경에 사전 설치되지 않은 `npm` 패키지의 경우에는 액션을 작성하거나 업데이트할 때 이를 종속 항목으로 번들링할 수 있습니다.

자세한 정보는 [Node.js 모듈로서 액션 패키징](./openwhisk_actions.html#openwhisk_js_packaged_action) 또는 [단일 번들로서 액션 패키징](./openwhisk_actions.html#openwhisk_js_webpack_action)을 참조하십시오.

### IBM SDK의 Node.js 버전 8 환경
{: #openwhisk_ref_javascript_environments_8}
Node.js 버전 8.11.3 환경은 액션을 작성하거나 업데이트할 때 `--kind` 플래그가 `nodejs:8`의 값으로 명시적으로 지정된 경우에 사용됩니다. 

다음과 같은 패키지가 Node.js 버전 8.11.3 환경에 사전 설치되어 있습니다. 
  - [amqplib v0.5.2](https://www.npmjs.com/package/amqplib) - Node.JS의 AMQP 0-9-1 클라이언트를 작성하기 위한 라이브러리입니다.
  - [apn v2.2.0](https://www.npmjs.com/package/apn) - Apple 푸시 알림 서비스와 인터페이스로 연결하기 위한 Node.js 모듈입니다.
  - [async v2.6.1](https://www.npmjs.com/package/async) - 비동기 함수 관련 작업을 위한 함수를 제공합니다.
  - [bent v1.1.0](https://www.npmjs.com/package/bent) - Node.js w/ async/await의 기능적 HTTP 클라이언트입니다.
  - [body-parser v1.18.3](https://www.npmjs.com/package/body-parser) - Node.js 본문 구문 분석 미들웨어입니다. 핸들러 전에 미들웨어에서 수신 요청 본문을 구문 분석하며 이 본문은 req.body 특성에 있습니다.
  - [btoa v1.2.1](https://www.npmjs.com/package/btoa) - 브라우저의 btoa 함수의 포트입니다.
  - [cassandra-driver v3.5.0](https://www.npmjs.com/package/cassandra-driver) - Apache Cassandra의 DataStax Node.js 드라이버입니다.
  - [cloudant v1.10.0](https://www.npmjs.com/package/cloudant) - 이는 Node.js에 대한 공식 Cloudant 라이브러리입니다.
  - [@cloudant/cloudant v2.3.0](https://www.npmjs.com/package/cloudant) - 이는 Node.js의 공식 Cloudant 라이브러리입니다. 
  - [commander v2.15.1](https://www.npmjs.com/package/commander) - node.js 명령행 인터페이스에 대한 완전한 솔루션입니다.
  - [composeaddresstranslator v1.0.4](https://www.npmjs.com/package/composeaddresstranslator) - Scylla 데이터베이스의 Compose UI 또는 API에서 변환기를 처리합니다.
  - [consul v0.33.1](https://www.npmjs.com/package/consul) - 서비스 검색 및 구성이 포함된 Consul에 대한 클라이언트입니다. 
  - [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - 쿠키 헤더를 구문 분석하고 쿠키 이름으로 키 지정된 오브젝트로 req.cookies를 채웁니다.
  - [cradle v0.7.1](https://www.npmjs.com/package/cradle) - Node.js에 대한 상위 레벨, 캐싱, CouchDB 클라이언트입니다.
  - [elasticsearch v15.0.0](https://www.npmjs.com/package/elasticsearch) - Node.js의 공식 하위 레벨 Elasticsearch 클라이언트입니다.
  - [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - 개발 전용 오류 핸들러 미들웨어입니다.
  - [etcd3 v0.2.11](https://www.npmjs.com/package/etcd3) - 프로토콜 버퍼 기반 etcdv3 API의 즉시 프로덕션 가능한 고품질 클라이언트입니다.
  - [express v4.16.3](https://www.npmjs.com/package/express) - 노드의 빠르고 관용적인 최소한의 웹 프레임워크입니다.
  - [express-session v1.15.6](https://www.npmjs.com/package/express-session) - Express 앱 내에서 세션 처리를 허용합니다.
  - [formidable v1.2.1](https://www.npmjs.com/package/formidable) - 양식 데이터, 특히 파일 업로드를 구문 분석하기 위한 Node.js 모듈입니다.
  - [glob v7.1.2](https://www.npmjs.com/package/glob) - 별표(*) 등과 같이 쉘이 사용하는 패턴을 사용하여 파일을 일치시킵니다.
  - [gm v1.23.1](https://www.npmjs.com/package/gm) - 노드의 GraphicsMagick 및 ImageMagick입니다.
  - [ibm-cos-sdk v1.2.1](https://www.npmjs.com/package/ibm-cos-sdk) - Node.js의 {{site.data.keyword.cos_full}} SDK입니다.
  - [ibm_db v2.4.0](https://www.npmjs.com/package/ibm_db) - IBM DB2 및 IBM Informix에 대한 node.js의 비동기/동기 인터페이스입니다.
  - [ibmiotf v0.2.41](https://www.npmjs.com/package/ibmiotf) - node.js 클라이언트는 IBM Watson Internet of Things Platform과의 상호작용을 단순화는 데 사용됩니다.
  - [iconv-lite v0.4.23](https://www.npmjs.com/package/iconv-lite) - Pure JS 문자 인코딩 변환입니다.
  - [jsdom v11.10.0](https://www.npmjs.com/package/jsdom) - jsdom은 많은 웹 표준, 특히 WHATWG DOM 및 HTML 표준의 순수 Javascript 구현입니다.
  - [jsonwebtoken v8.3.0](https://www.npmjs.com/package/jsonwebtoken) - JSON 웹 토큰의 구현입니다. 
  - [lodash v4.17.10](https://www.npmjs.com/package/lodash) - Node.js 모듈로서 노출된 Lodash 라이브러리입니다.
  - [log4js v2.9.0](https://www.npmjs.com/package/log4js) - 이는 노드 관련 작업을 위한 log4js 프레임워크의 변환입니다. 
  - [marked v0.4.0](https://www.npmjs.com/package/marked) - Javascript로 작성된, 모든 기능을 갖춘 마크다운 구문 분석기 및 컴파일러입니다. 속도를 위해 빌드되었습니다.
  - [merge v1.2.0](https://www.npmjs.com/package/merge) - 다중 오브젝트를 하나로 병합하며, 선택사항으로 복제된 새 오브젝트를 작성합니다.
  - [moment v2.22.2](https://www.npmjs.com/package/moment) - 날짜를 구문 분석, 유효성 검증, 조작 및 형식화하기 위한 경량 Javascript 날짜 라이브러리입니다.
  - [mongodb v3.0.10](https://www.npmjs.com/package/mongodb) - Node.js의 공식 MongoDB 드라이버입니다.
  - [mysql v2.15.0](https://www.npmjs.com/package/mysql) - 이는 mysql의 node.js 드라이버입니다.
  - [mustache v2.3.0](https://www.npmjs.com/package/mustache) - mustache.js는 Javascript로 된 mustache 템플리트 시스템의 구현입니다.
  - [nano v6.4.4](https://www.npmjs.com/package/nano) - Node.js의 최소한의 couchdb 드라이버입니다.
  - [nodemailer v4.6.7](https://www.npmjs.com/package/nodemailer) - Node.js에서 이메일을 발송합니다. 아주 쉽습니다! 
  - [oauth2-server v3.0.0](https://www.npmjs.com/package/oauth2-server) - Node.js로 표현하여 OAuth2 서버/제공자를 구현하기 위한 완벽하고 호환 가능하며 성공적으로 테스트된 모듈입니다.
  - [openwhisk v3.15.0](https://www.npmjs.com/package/openwhisk) - OpenWhisk 플랫폼에 대한 Javascript 클라이언트 라이브러리입니다. OpenWhisk API 관련 랩퍼를 제공합니다.
  - [path-to-regex v2.2.1](https://www.npmjs.com/package/path-to-regexp) - /user/:name과 같은 경로 문자열을 URL 경로에 대해 일치시키는 데 사용할 수 있는 정규식으로 변환합니다.
  - [pg v7.4.3](https://www.npmjs.com/package/pg) - node.js의 넌블로킹 PostgreSQL 클라이언트입니다. 순수 JavaScript 및 선택적 기본 libpq 바인딩입니다.
  - [process v0.11.10](https://www.npmjs.com/package/process) - require('process'). 기타 모듈과 같습니다.
  - [pug v2.0.3](https://www.npmjs.com/package/pug) - Pug 템플레이팅 언어를 구현합니다.
  - [redis v2.8.0](https://www.npmjs.com/package/redis) - 이는 Node.js에 대한 완전하고 기능이 풍부한 Redis 클라이언트입니다.
  - [request v2.87.0](https://www.npmjs.com/package/request) - 요청은 HTTP 호출을 작성하는 가장 단순한 방법이 되도록 설계되었습니다.
  - [request-promise v4.2.2](https://www.npmjs.com/package/request-promise) - Promise 지원의 단순화된 HTTP 요청 클라이언트 'request'입니다. Bluebird에 의해 구동됩니다.
  - [rimraf v2.6.2](https://www.npmjs.com/package/rimraf) - 노드에 대한 UNIX 명령 "rm -rf"입니다.
  - [semver v5.5.0](https://www.npmjs.com/package/semver) - Nodejs의 시맨틱 버전화입니다.
  - [@sendgrid/mail@6.3.1](https://www.npmjs.com/package/@sendgrid/mail) - SendGrid API를 통한 이메일 지원을 제공합니다. 
  - [serve-favicon v2.5.0](https://www.npmjs.com/package/serve-favicon) - 파비콘을 서비스하기 위한 Node.js 미들웨어입니다.
  - [superagent v3.8.3](https://www.npmjs.com/package/superagent) - SuperAgent는 소형의 혁신적 클라이언트 측 HTTP 요청 라이브러리 및 동일한 API의 Node.js 모듈이며, 다수의 상위 레벨 HTTP 클라이언트 기능을 과시합니다.
  - [twilio v3.17.3](https://www.npmjs.com/package/twilio) - 음성, 동영상 및 메시징과 관련된 Twilio API에 대한 랩퍼입니다. 
  - [underscore v1.9.1](https://www.npmjs.com/package/underscore) - Underscore.js는 유력한 기능상 용의자(각각, 맵, 축소, 필터...)를 지원하는 Javascript용 유틸리티 벨트 라이브러리입니다. 코어 Javascript 오브젝트의 확장은 없습니다.
  - [url-pattern v1.0.3](https://www.npmjs.com/package/url-pattern) - 매개변수의 URL을 구문 분석하며, 이는 정규식 문자열 매칭 프로그램을 사용하는 방식보다 더 쉽습니다.
  - [uuid v3.2.1](https://www.npmjs.com/package/uuid) - RFC4122 UUIDS의 단순하고 빠른 생성입니다.
  - [validator v10.3.0](https://www.npmjs.com/package/validator) - 문자열 유효성 검증기 및 무결 처리기의 라이브러리입니다.
  - [watson-developer-cloud v3.5.0](https://www.npmjs.com/package/watson-developer-cloud) - 코그너티브 컴퓨팅을 사용하여 복잡한 문제점을 해결하는 API의 콜렉션인 Watson Developer Cloud 서비스를 사용하기 위한 Node.js 클라이언트 라이브러리입니다. 
  - [when v3.7.8](https://www.npmjs.com/package/when) - When.js는 매우 탄탄하며 극한 테스트를 거친 Promises/A+ 및 when() 구현이며, 전체 ES6 Promise shim이 포함됩니다.
  - [winston v3.0.0](https://www.npmjs.com/package/winston) - node.js에 대한 다중 전송 비동기 로깅 라이브러리입니다. "CHILL WINSTON! ... I put it in the logs."
  - [ws v5.2.0](https://www.npmjs.com/package/ws) - ws는 사용이 단순하고 매우 빠르며 철저한 테스트를 거친 WebSocket 클라이언트 및 서버 구현입니다.
  - [xml2js v0.4.19](https://www.npmjs.com/package/xml2js) - 단순 XML 대 Javascript 오브젝트 변환기입니다. 이는 양방향 변환을 지원합니다.
  - [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest는 브라우저 XMLHttpRequest 오브젝트를 에뮬레이트하기 위한 기본 제공 http 클라이언트용 랩퍼입니다.
  - [yauzl v2.9.2](https://www.npmjs.com/package/yauzl) - 노드에 대한 또 다른 unzip 라이브러리입니다. 

### Node.js 버전 6 환경
{: #openwhisk_ref_javascript_environments_6}
Node.js 6.14.3 환경은 액션을 작성하거나 업데이트할 때 `--kind` 플래그가 `nodejs:6`의 값으로 명시적으로 지정된 경우에 사용됩니다. 

다음과 같은 패키지가 Node.js 6.14.3 환경에서 사용될 수 있습니다. 

- [apn v2.1.2](https://www.npmjs.com/package/apn) - Apple 푸시 알림 서비스와의 인터페이스를 위한 Node.js 모듈입니다.
- [async v2.1.4](https://www.npmjs.com/package/async) - 비동기 함수 관련 작업을 위한 함수를 제공합니다.
- [btoa v1.1.2](https://www.npmjs.com/package/btoa) - 브라우저의 btoa 함수의 포트입니다.
- [cheerio v0.22.0](https://www.npmjs.com/package/cheerio) - 서버에 맞게 별도로 디자인된 빠르고 유연한 코어 jQuery의 구현입니다.
- [cloudant v1.6.2](https://www.npmjs.com/package/cloudant) - Node.js에 대한 공식 Cloudant 라이브러리입니다.
- [commander v2.9.0](https://www.npmjs.com/package/commander) - Node.js 명령행 인터페이스에 대한 완전한 솔루션입니다.
- [consul v0.27.0](https://www.npmjs.com/package/consul) - 서비스 검색 및 구성을 포함하는 Consul에 대한 클라이언트입니다.
- [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - 쿠키 헤더를 구문 분석하고 쿠키 이름으로 키 지정된 오브젝트로 req.cookies를 채웁니다.
- [cradle v0.7.1](https://www.npmjs.com/package/cradle) - Node.js에 대한 상위 레벨, 캐싱, CouchDB 클라이언트입니다.
- [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - 개발 전용 오류 핸들러 미들웨어입니다.
- [glob v7.1.1](https://www.npmjs.com/package/glob) - 별표(*) 등과 같이 쉘이 사용하는 패턴을 사용하여 파일을 일치시킵니다.
- [gm v1.23.0](https://www.npmjs.com/package/gm) - 노드에 대한 GraphicsMagick 및 ImageMagick입니다.
- [lodash v4.17.2](https://www.npmjs.com/package/lodash) - Node.js 모듈로서 노출된 Lodash 라이브러리입니다.
- [log4js v0.6.38](https://www.npmjs.com/package/log4js) - 노드 관련 작업을 위해 디자인된 log4js 프레임워크의 변환입니다.
- [iconv-lite v0.4.15](https://www.npmjs.com/package/iconv-lite) - 순수 JS 문자 인코딩 변환입니다.
- [marked v0.3.6](https://www.npmjs.com/package/marked) - Javascript로 작성된 전체 기능의 마크다운 구문 분석기 및 컴파일러입니다. 속도를 위해 빌드되었습니다.
- [merge v1.2.0](https://www.npmjs.com/package/merge) - 복제된 새 오브젝트를 작성하기 위해 다중 오브젝트를 하나로 병합합니다.
- [moment v2.17.0](https://www.npmjs.com/package/moment) - 날짜를 구문 분석, 유효성 검증, 조작 및 형식화하기 위한 경량 Javascript 날짜 라이브러리입니다.
- [mongodb v2.2.11](https://www.npmjs.com/package/mongodb) - Node.js에 대한 공식 MongoDB 드라이버입니다.
- [mustache v2.3.0](https://www.npmjs.com/package/mustache) - Mustache.js는 Javascript로 된 mustache 템플리트 시스템의 구현입니다.
- [nano v6.2.0](https://www.npmjs.com/package/nano) - Node.js에 대한 최소한의 couchdb 드라이버입니다.
- [node-uuid v1.4.7](https://www.npmjs.com/package/node-uuid) - 더 이상 사용되지 않는 UUID 패키징입니다.
- [nodemailer v2.6.4](https://www.npmjs.com/package/nodemailer) - Node.js에서 이메일을 발송합니다. 아주 쉽습니다!
- [oauth2-server v2.4.1](https://www.npmjs.com/package/oauth2-server) - Node.js의 표현으로 OAuth2 서버/제공자를 구현하기 위한 완벽하고 호환 가능하며 성공적으로 테스트된 모듈입니다.
- [openwhisk v3.15.0](https://www.npmjs.com/package/openwhisk) - OpenWhisk 플랫폼에 대한 Javascript 클라이언트 라이브러리입니다. OpenWhisk API 관련 랩퍼를 제공합니다.
- [pkgcloud v1.4.0](https://www.npmjs.com/package/pkgcloud) - pkgcloud는 다중 클라우드 제공자 간의 차이를 도외시하는 Node.js용 표준 라이브러리입니다.
- [process v0.11.9](https://www.npmjs.com/package/process) - require('process'). 기타 모듈과 같습니다.
- [pug v2.0.0-beta6](https://www.npmjs.com/package/pug) - Pug 템플레이팅 언어를 구현합니다.
- [redis v2.6.3](https://www.npmjs.com/package/redis) - Node.js에 대한 완전하고 기능이 풍부한 Redis 클라이언트입니다.
- [request v2.79.0](https://www.npmjs.com/package/request) - 요청은 HTTP 호출 작성을 위한 가능한 가장 단순한 방법입니다.
- [request-promise v4.1.1](https://www.npmjs.com/package/request-promise) - Promise 지원의 단순화된 HTTP 요청 클라이언트 'request'입니다. Bluebird에 의해 구동됩니다.
- [rimraf v2.5.4](https://www.npmjs.com/package/rimraf) - 노드에 대한 UNIX 명령 "rm -rf"입니다.
- [semver v5.3.0](https://www.npmjs.com/package/semver) - 시맨틱 버전화를 지원합니다.
- [sendgrid v4.7.1](https://www.npmjs.com/package/sendgrid) - SendGrid API를 통한 이메일 지원을 제공합니다.
- [serve-favicon v2.3.2](https://www.npmjs.com/package/serve-favicon) - 파비콘을 서비스하기 위한 Node.js 미들웨어입니다.
- [socket.io v1.6.0](https://www.npmjs.com/package/socket.io) - Socket.IO는 실시간 양방향 이벤트 기반 통신을 가능하게 합니다.
- [socket.io-client v1.6.0](https://www.npmjs.com/package/socket.io-client) - Socket.IO에 대한 클라이언트 측 지원입니다.
- [superagent v3.0.0](https://www.npmjs.com/package/superagent) - SuperAgent는 소형의 혁신적 클라이언트 측 HTTP 요청 라이브러리 및 동일한 API의 Node.js 모듈이며, 다수의 상위 레벨 HTTP 클라이언트 기능을 과시합니다.
- [swagger-tools v0.10.1](https://www.npmjs.com/package/swagger-tools) - API를 문서화하는 방식인 Swagger 관련 작업과 연관된 도구입니다.
- [tmp v0.0.31](https://www.npmjs.com/package/tmp) - node.js에 대한 단순 임시 파일 및 디렉토리 작성자입니다.
- [twilio v2.11.1](https://www.npmjs.com/package/twilio) - 음성, 동영상 및 메시징과 관련된 Twilio API에 대한 랩퍼입니다.
- [underscore v1.8.3](https://www.npmjs.com/package/underscore) - Underscore.js는 유력한 기능상 용의자(각각, 맵, 축소, 필터...)를 지원하는 Javascript용 유틸리티 벨트 라이브러리입니다. 코어 Javascript 오브젝트의 확장은 없습니다.
- [uuid v3.0.0](https://www.npmjs.com/package/uuid) - RFC4122 UUIDS의 단순하고 신속한 생성입니다.
- [validator v6.1.0](https://www.npmjs.com/package/validator) - 문자열 유효성 검증기 및 무결 처리기의 라이브러리입니다.
- [watson-developer-cloud v2.29.0](https://www.npmjs.com/package/watson-developer-cloud) - 코그너티브 컴퓨팅을 사용하여 복잡한 문제점을 해결하는 API의 콜렉션인 Watson Developer Cloud 서비스를 사용하기 위한 Node.js 클라이언트 라이브러리입니다.
- [when v3.7.7](https://www.npmjs.com/package/when) - When.js는 매우 탄탄하며 극한 테스트를 거친 Promises/A+ 및 when() 구현이며, 전체 ES6 Promise shim이 포함됩니다.
- [winston v2.3.0](https://www.npmjs.com/package/winston) - node.js에 대한 다중 전송 비동기 로깅 라이브러리입니다. "CHILL WINSTON! ... I put it in the logs."
- [ws v1.1.1](https://www.npmjs.com/package/ws) - ws는 사용이 단순하고 매우 빠르며 철저한 테스트를 거친 WebSocket 클라이언트 및 서버 구현입니다.
- [xml2js v0.4.17](https://www.npmjs.com/package/xml2js) - Javascript 오브젝트 변환기에 대한 단순 XML입니다. 이는 양방향 변환을 지원합니다.
- [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest는 브라우저 XMLHttpRequest 오브젝트를 에뮬레이트하기 위한 기본 제공 http 클라이언트용 랩퍼입니다.
- [yauzl v2.7.0](https://www.npmjs.com/package/yauzl) - 노드에 대한 또 다른 unzip 라이브러리입니다. 


## Python 런타임 환경
{: #openwhisk_ref_python_environments}

OpenWhisk는 두 개의 서로 다른 런타임 버전을 사용하여 Python 액션의 실행을 지원합니다.

### Python 3 액션(Jessie 기반)
{: #openwhisk_ref_python_environments_jessie}

Python 3 액션은 Python 3.6.5에서 실행됩니다. 이 런타임을 사용하려면 액션을 작성하거나 업데이트할 때 `wsk` CLI 매개변수 `--kind python-jessie:3`을 지정하십시오.
virtualenv를 사용하여 python 액션을 작성할 때는 Docker 이미지 `ibmfunctions/action-python-v3`을 사용하십시오.
런타임에는 Python 3.6 표준 라이브러리에 외에 Python 액션에서 사용할 수 있는 IBM Cloud 서비스에 대한 SDK 패키지가 포함되어 있습니다.

Python 버전:
- [3.6.5](https://github.com/docker-library/python/blob/a1aa406bfd8c7b129e6e0ee0ba972b863624ac0d/3.6/jessie/Dockerfile)

Python 패키지:
- asn1crypto==0.24.0
- attrs==17.4.0
- Automat==0.6.0
- beautifulsoup4==4.6.0
- botocore==1.9.4
- cassandra-driver==3.14.0
- certifi==2018.1.18
- cffi==1.11.5
- chardet==3.0.4
- click==6.7
- cloudant==2.9.0
- constantly==15.1.0
- cryptography==2.1.4
- cssselect==1.0.3
- docutils==0.14
- elasticsearch==6.2.0
- Flask==1.0.2
- gevent==1.2.2
- greenlet==0.4.13
- httplib2==0.11.3
- hyperlink==18.0.0
- ibm-cos-sdk==2.1.1
- ibm-cos-sdk-core==2.1.1
- ibm-cos-sdk-s3transfer==2.1.1
- ibm-db==2.0.8a0
- ibmcloudsql==0.2.13
- idna==2.6
- incremental==17.5.0
- itsdangerous==0.24
- Jinja2==2.10
- jmespath==0.9.3
- kafka-python==1.4.3
- lxml==4.2.1
- MarkupSafe==1.0
- numpy==1.14.5
- pandas==0.23.1
- parsel==1.4.0
- pika==0.12.0
- Pillow==5.1.0
- psycopg2==2.7.5
- pyasn1==0.4.2
- pyasn1-modules==0.2.1
- pycparser==2.18
- PyDispatcher==2.0.5
- pymongo==3.6.1
- pyOpenSSL==17.5.0
- pysolr==3.7.0
- python-dateutil==2.7.3
- pytz==2018.3
- queuelib==1.4.2
- redis==2.10.6
- requests==2.19.1
- scikit-learn==0.19.1
- scipy==1.1.0
- Scrapy==1.5.0
- service-identity==17.0.0
- simplejson==3.15.0
- six==1.11.0
- tornado==5.0.2
- Twisted==18.4.0
- urllib3==1.22
- virtualenv==16.0.0
- w3lib==1.19.0
- watson-developer-cloud==1.4.0
- Werkzeug==0.14.1
- zope.interface==4.4.3

### Python 3 액션(Alpine 기반)
{: #openwhisk_ref_python_environments_alpine}

Python 3 액션은 Python 3.6.1에서 실행됩니다. 이 런타임을 사용하려면 액션을 작성하거나 업데이트할 때 `wsk` CLI 매개변수 `--kind python:3`을 지정하십시오.
virtualenv를 사용하여 python 액션을 작성할 때는 Docker 이미지 `openwhisk/python3action`을 사용하십시오.
Python 3.6 표준 라이브러리에 외에 다음 패키지를 Python 액션에서 사용할 수 있습니다.

Python 패키지:
- asn1crypto==0.23.0
- attrs==17.3.0
- Automat==0.6.0
- beautifulsoup4==4.5.3
- cffi==1.11.2
- click==6.7
- constantly==15.1.0
- cryptography==2.1.3
- cssselect==1.0.1
- Flask==0.12
- gevent==1.2.1
- greenlet==0.4.12
- httplib2==0.10.3
- idna==2.6
- incremental==17.5.0
- itsdangerous==0.24
- Jinja2==2.9.6
- kafka-python==1.3.4
- lxml==3.7.3
- MarkupSafe==1.0
- parsel==1.2.0
- pyasn1==0.3.7
- pyasn1-modules==0.1.5
- pycparser==2.18
- PyDispatcher==2.0.5
- pyOpenSSL==17.3.0
- python-dateutil==2.6.0
- queuelib==1.4.2
- requests==2.13.0
- Scrapy==1.3.3
- service-identity==17.0.0
- simplejson==3.10.0
- six==1.11.0
- Twisted==17.1.0
- virtualenv==15.1.0
- w3lib==1.18.0
- Werkzeug==0.12.2
- zope.interface==4.4.3

### Python 2 액션

Python 2 액션은 Python 액션에 대한 기본 런타임인 Python 2.7.12에서 실행됩니다. 액션을 작성하거나 업데이트할 때 `--kind` 플래그를 지정하지 않는 한, 이 런타임을 명시적으로 선택하려면 `--kind python:2`를 사용하십시오.
virtualenv를 사용하여 python 액션을 작성할 때는 Docker 이미지 `openwhisk/python2action`을 사용하십시오.
Python 2.7 표준 라이브러리 외에 다음 패키지를 Python 2 액션에서 사용할 수 있습니다.

Python 패키지:
- asn1crypto==0.23.0
- attrs==17.2.0
- beautifulsoup4==4.5.1
- cffi==1.11.1
- click==6.7
- cryptography==2.0.3
- cssselect==1.0.1
- enum34==1.1.6
- Flask==0.11.1
- gevent==1.1.2
- greenlet==0.4.12
- httplib2==0.9.2
- idna==2.6
- ipaddress==1.0.18
- itsdangerous==0.24
- Jinja2==2.9.6
- kafka-python==1.3.1
- lxml==3.6.4
- MarkupSafe==1.0
- parsel==1.2.0
- pyasn1==0.3.7
- pyasn1-modules==0.1.4
- pycparser==2.18
- PyDispatcher==2.0.5
- pyOpenSSL==17.3.0
- python-dateutil==2.5.3
- queuelib==1.4.2
- requests==2.11.1
- Scrapy==1.1.2
- service-identity==17.0.0
- simplejson==3.8.2
- six==1.11.0
- Twisted==16.4.0
- virtualenv==15.1.0
- w3lib==1.18.0
- Werkzeug==0.12.2
- zope.interface==4.4.3

## Swift 액션
{: #swift-actions}

### Swift 3
Swift 3 액션은 Swift 3.1.1 `--kind swift:3.1.1`을 사용하여 실행됩니다. 이전 Swift 버전이 지원되지 않으므로 항상 `swift:3.1.1` 유형을 지정하십시오.

`swift:3.1.1` 유형을 사용하려면 모든 Swift 액션을 마이그레이션해야 합니다. 액션을 작성하거나 업데이트할 때 항상 특정 유형을 제공하는 것이 가장 좋습니다.
{: tip}

Swift 3.1.1 액션은 단일 Swift 소스 파일을 사용할 때 다음 패키지를 사용할 수 있습니다.
- KituraNet 버전 1.7.6, https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON 버전 15.0.1, https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK 버전 0.16.0, https://github.com/watson-developer-cloud/swift-sdk

### Swift 4
Swift 4 액션은 Swift 4.1 `--kind swift:4.1`을 사용하여 실행됩니다.

[패키징된 swift 액션](./openwhisk_actions.html#packaging-an-action-as-a-swift-executable)에 대한 지시사항을 따라 Package.swift를 사용하여 종속 항목을 포함하십시오.

Swift 4.1 액션은 단일 Swift 소스 파일을 사용할 때 다음 패키지를 사용할 수 있습니다.
- Watson Developer Cloud SDK 버전 0.28.0, https://github.com/watson-developer-cloud/swift-sdk

### Swift 3.1.1을 Swift 4.1로 마이그레이션

#### 단일 소스 액션 파일을 사용한 SwiftyJSON
`swift:3.1.1` 액션이 컴파일되지 않은 경우 **SwiftyJSON** 패키지를 사용하는 소스 파일과 같이 액션을 미리 컴파일하고 `swift:4.1` 유형 액션에 사용할 SwiftyJSON 버전을 지정해야 합니다. Swift 4.1부터는 기본적인 JSON 데이터 관리에 대한 지원이 향상되었다는 점을 고려하십시오.

## PHP 액션
{: #openwhisk_ref_php}

PHP 액션은 PHP 7.1.18에서 실행됩니다. 이 런타임을 사용하려면 액션을 작성하거나 업데이트할 때 `wsk` CLI 매개변수 `--kind php:7.1`을 지정하십시오. `.php` 확장자가 있는 파일로 액션을 작성할 때는 이 작동이 기본값입니다.

표준 확장자에 추가하여 다음의 PHP 확장자를 사용할 수 있습니다.

- bcmath
- curl
- gd
- intl
- mbstring
- mysqli
- pdo_mysql
- pdo_pgsql
- pdo_sqlite
- soap
- zip

### 작성기 패키지
다음의 작성기 패키지 역시 사용 가능합니다.

- guzzlehttp/guzzle       v6.7.3
- ramsey/uuid             v3.7.3

## Docker 액션
{: #openwhisk_ref_docker}

Docker 액션은 Docker 컨테이너에서 사용자 제공 2진을 실행합니다. 2진이 [python:2.7.12-alpine](https://hub.docker.com/r/library/python) 기반의 Docker 이미지에서 실행되므로, 2진은 이 배포와 호환 가능해야 합니다.

Docker 스켈레톤은 OpenWhisk 호환 가능 Docker 이미지를 빌드하는 편리한 방법입니다. `ibmcloud wsk sdk install docker` CLI 플러그인 명령을 사용하여 스켈레톤을 설치할 수 있습니다.

기본 2진 프로그램은 컨테이너 내부의 `/action/exec`에 있어야 합니다. 실행 파일은 `JSON` 오브젝트로서 역직렬화될 수 있는 단일 명령행 인수 문자열을 통해 입력 인수를 수신합니다. 이는 직렬화된 `JSON`의 단일행 문자열로서 `stdout`을 사용하여 결과를 리턴해야 합니다.

`dockerSkeleton`에 포함된 `Dockerfile`을 수정하여 컴파일 단계나 종속 항목을 포함할 수 있습니다.

## REST API
{: #openwhisk_ref_restapi}
{{site.data.keyword.openwhisk_short}} REST API에 대한 정보는 [REST API 참조](https://console.bluemix.net/apidocs/openwhisk)에서 찾을 수 있습니다.

## 시스템 한계
{: #openwhisk_syslimits}

### 액션
{{site.data.keyword.openwhisk_short}}에는 액션이 사용할 수 있는 메모리 양 및 분당 허용되는 액션 호출 수 등을 포함한 시스템 한계가 있습니다.

다음 표에는 액션에 대한 기본 한계가 나열되어 있습니다.

|한계 |설명 |기본값 |최소값 |최대값 |
| ----- | ----------- | :-------: | :---: | :---: |
|[codeSize](openwhisk_reference.html#openwhisk_syslimits_codesize) | 액션 코드의 최대 크기(MB)입니다. |48 |1 |48 |
|[concurrent](openwhisk_reference.html#openwhisk_syslimits_concurrent) |실행 중인 또는 실행을 위해 큐에서 대기중인 네임스페이스당 최대 N개까지만 활성화가 제출될 수 있습니다. |1000 |1 |1000* |
|[logs](openwhisk_reference.html#openwhisk_syslimits_logs) |컨테이너가 N MB를 초과하여 stdout에 쓸 수 없습니다. |10 |0 |10 |
|[memory](openwhisk_reference.html#openwhisk_syslimits_memory) |컨테이너가 N MB를 초과하여 메모리를 할당할 수 없습니다. |256 |128 |512 |
|[minuteRate](openwhisk_reference.html#openwhisk_syslimits_minuterate) |N개를 초과하는 활성화가 분당 네임스페이스마다 제출될 수 없습니다. |5000 |1 |5000* |
|[openulimit](openwhisk_reference.html#openwhisk_syslimits_openulimit) | 액션에 대한 열린 파일의 최대 수입니다. | 1024 |0 | 1024 |
|[parameters](openwhisk_reference.html#openwhisk_syslimits_parameters) |첨부될 수 있는 매개변수의 최대 크기(MB)입니다. |1 |0 |1 |
|[proculimit](openwhisk_reference.html#openwhisk_syslimits_proculimit) | 액션에서 사용할 수 있는 최대 프로세스 수입니다. | 1024 |0 | 1024 |
|[result](openwhisk_reference.html#openwhisk_syslimits_result) | 액션 호출 결과의 최대 크기(MB)입니다. |1 |0 |1 |
| [sequenceMaxActions](openwhisk_reference.html#openwhisk_syslimits_sequencemax) | 지정된 시퀀스로 구성된 최대 액션 수입니다. |50 |0 | 50* |
|[timeout](openwhisk_reference.html#openwhisk_syslimits_timeout) |컨테이너가 N밀리초 넘게 실행될 수 없습니다. |60000 |100 | 600000 |

### 고정 한계 늘리기
{: #increase_fixed_limit}

(*)로 끝나는 한계 값은 고정되어 있지만, 경영 사례에서 보다 높은 안전 한계 값을 정당화할 수 있으면 이를 늘릴 수 있습니다. 한계 값을 늘리고자 하는 경우에는 IBM [{{site.data.keyword.openwhisk_short}} 웹 콘솔](https://console.bluemix.net/openwhisk/)에서 직접 티켓을 열어서 IBM 지원 센터에 문의하십시오.
  1. **지원**을 선택하십시오.
  2. 드롭 다운 메뉴에서 **티켓 추가**를 선택하십시오.
  3. 티켓 유형에 대해 **기술적**을 선택하십시오.
  4. 지원의 기술 영역에 대해 **함수**를 선택하십시오.

#### codeSize(MB)(고정됨: 48MB)
{: #openwhisk_syslimits_codesize}
* 액션에 대한 최대 코드 크기는 48MB입니다.
* Javascript 액션에서 종속 항목이 포함된 모든 소스 코드를 단일 번들링된 파일에 연결하는 도구를 사용하도록 권장합니다.
* 이 한계는 고정되어 있으며 변경될 수 없습니다.

#### concurrent(고정됨: 1000*)
{: #openwhisk_syslimits_concurrent}
* 네임스페이스에 대한 실행을 위해 큐에서 대기중이거나 실행 중인 활성화의 수는 1000개를 초과할 수 없습니다.
* 이 한계 값은 고정되어 있지만, 경영 사례에서 보다 높은 안전 한계 값을 정당화할 수 있으면 이를 늘릴 수 있습니다. 이 한계를 늘리는 방법에 대한 세부 지시사항은 [고정 한계 늘리기](openwhisk_reference.html#increase_fixed_limit) 절을 참조하십시오.

#### logs(MB)(기본값: 10MB)
{: #openwhisk_syslimits_logs}
* 로그 한계 N은 [0MB..10MB] 범위이며 액션마다 설정됩니다.
* 액션이 작성되거나 업데이트될 때 사용자는 액션 로그 한계를 변경할 수 있습니다.
* 설정 한계를 초과하는 로그는 잘립니다. 따라서 새 로그 항목은 무시되며 경고는 활성화가 설정된 로그 한계를 초과했음을 표시하기 위해 활성화의 마지막 출력으로서 추가됩니다.

#### memory(MB)(기본값: 256MB)
{: #openwhisk_syslimits_memory}
* 메모리 한계 M은 [128MB..512MB] 범위이며 MB 단위로 액션마다 설정됩니다.
* 사용자는 액션이 작성될 때 메모리 한계를 변경할 수 있습니다.
* 컨테이너는 한계에 의해 할당된 메모리를 초과하여 사용할 수 없습니다.

#### minuteRate(고정: 5000*)
{: #openwhisk_syslimits_minuterate}
* 속도 제한 N은 5000으로 설정되며 1분 간 액션 호출의 수를 제한합니다.
* 이 한계를 초과하는 CLI 또는 API 호출은 HTTP 상태 코드 `429: TOO MANY REQUESTS`에 대응하는 오류 코드를 수신합니다.
* 이 한계 값은 고정되어 있지만, 경영 사례에서 보다 높은 안전 한계 값을 정당화할 수 있으면 이를 늘릴 수 있습니다. 이 한계를 늘리는 방법에 대한 세부 지시사항은 [고정 한계 늘리기](openwhisk_reference.html#increase_fixed_limit) 절을 참조하십시오.

#### openulimit(고정: 1024:1024)
{: #openwhisk_syslimits_openulimit}
* 액션에 대한 열린 파일의 최대 수는 (하드 및 소프트 한계 모두에 대해) 1,024개입니다.
* 이 한계는 고정되어 있으며 변경될 수 없습니다.
* 액션이 호출되면 Docker 실행 명령이 `--ulimit nofile=1024:1024` 인수를 사용하여 `openulimit` 값을 설정합니다.
* 자세한 정보는 [docker run](https://docs.docker.com/engine/reference/commandline/run) 명령행 참조 문서를 참조하십시오.

#### parameters(고정: 1MB)
{: #openwhisk_syslimits_parameters}
* 액션/패키지/트리거의 작성 및 업데이트 시에 총 매개변수의 크기 한계는 1MB입니다.
* 너무 큰 매개변수의 엔티티는 이의 작성 또는 업데이트 시에 거부됩니다.
* 이 한계는 고정되어 있으며 변경될 수 없습니다.

#### proculimit(고정: 1024:1024)
{: #openwhisk_syslimits_proculimit}
* 액션 컨테이너에서 사용할 수 있는 최대 프로세스 수는 1,024입니다.
* 이 한계는 고정되어 있으며 변경될 수 없습니다.
* 액션이 호출되면 Docker 실행 명령이 `--pids-limit 1024` 인수를 사용하여 `proculimit` 값을 설정합니다.
* 자세한 정보는 [docker run](https://docs.docker.com/engine/reference/commandline/run) 명령행 참조 문서를 참조하십시오.

#### result(고정: 1MB)
{: #openwhisk_syslimits_result}
* 액션 호출 결과의 최대 출력 크기(MB)입니다.
* 이 한계는 고정되어 있으며 변경될 수 없습니다.

#### sequenceMaxActions(고정됨: 50*)
{: #openwhisk_syslimits_sequencemax}
* 지정된 시퀀스로 구성된 최대 액션 수입니다.
* 이 한계는 고정되어 있으며 변경될 수 없습니다.

#### timeout(ms)(기본값: 60s)
{: #openwhisk_syslimits_timeout}
* 제한시간 한계 N은 [100ms..600000ms] 범위이며 밀리초 단위로 액션마다 설정됩니다.
* 사용자는 액션이 작성될 때 제한시간 한계를 변경할 수 있습니다.
* N밀리초를 초과하여 실행되는 컨테이너는 종료됩니다.

### 트리거

트리거는 다음 표에서 설명하는 대로 분당 실행 속도에 종속됩니다.

|한계 |설명 |기본값 |최소값 |최대값 |
| ----- | ----------- | :-------: | :---: | :---: |
|[minuteRate](openwhisk_reference.html#openwhisk_syslimits_tminuterate) | 분당 네임스페이스마다 N개가 넘는 트리거는 실행될 수 없습니다. |5000* |5000* |5000* |

### 고정 한계 늘리기
{: #increase_fixed_tlimit}

(*)로 끝나는 한계 값은 고정되어 있지만, 경영 사례에서 보다 높은 안전 한계 값을 정당화할 수 있으면 이를 늘릴 수 있습니다. 한계 값을 늘리고자 하는 경우에는 IBM [{{site.data.keyword.openwhisk_short}} 웹 콘솔](https://console.bluemix.net/openwhisk/)에서 직접 티켓을 열어서 IBM 지원 센터에 문의하십시오.
  1. **지원**을 선택하십시오.
  2. 드롭 다운 메뉴에서 **티켓 추가**를 선택하십시오.
  3. 티켓 유형에 대해 **기술적**을 선택하십시오.
  4. 지원의 기술 영역에 대해 **함수**를 선택하십시오.

#### minuteRate(고정: 5000*)
{: #openwhisk_syslimits_tminuterate}

* 속도 제한 N은 5000으로 설정되며 1분간 사용자가 실행할 수 있는 트리거의 수를 제한합니다.
* 사용자는 트리거가 작성될 때 트리거 한계를 변경할 수 없습니다.
* 이 한계를 초과하는 CLI 또는 API 호출은 HTTP 상태 코드 `429: TOO MANY REQUESTS`에 대응하는 오류 코드를 수신합니다.
* 이 한계 값은 고정되어 있지만, 경영 사례에서 보다 높은 안전 한계 값을 정당화할 수 있으면 이를 늘릴 수 있습니다. 이 한계를 늘리는 방법에 대한 세부 지시사항은 [고정 한계 늘리기](openwhisk_reference.html#increase_fixed_tlimit) 절을 참조하십시오.
