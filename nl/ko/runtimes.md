---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-20"

keywords: runtimes, support

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

# 런타임
{: #runtimes}
앱은 프로그래밍 언어(예: Javascript 또는 Python)로 코딩되고 실행될 수 있습니다. 기본적으로 다수의 런타임은 {{site.data.keyword.openwhisk_short}}에서 사용 가능합니다.
{: shortdesc}

각 지역에서 IBM Cloud Functions에 사용할 수 있는 런타임을 보십시오. 다음 링크는 JSON 응답을 리턴합니다.

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [eu-de](https://eu-de.functions.cloud.ibm.com/)

응답의 `runtimes` 섹션에는 사용 가능한 런타임 세트가 포함되어 있습니다.

`image` 섹션에는 [Docker 허브](https://hub.docker.com/) 및 사용되는 태그에 대한 런타임 이미지 이름이 포함되어 있습니다. 


다음 예는 `ibmfunctions/action-nodejs-v10` 및 `openwhisk/nodejs8action` 이미지를 가리킵니다.
태그는 `1.9.0` 같은 버전 번호이거나 `b99d71e` 같은 짧은 형식의 GIT 커미트 해시일 수 있습니다.

예제 이미지 필드.
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs8action:b99d71e"
  ```

런타임은 정기적으로 업데이트됩니다. 이러한 업데이트에는 런타임 내의 패키지에 대한 보안 수정사항 및 부 버전 업데이트가 포함됩니다. 부 버전 업데이트 시에는 이전 버전과의 호환성이 중단될 수 있습니다. 런타임 업데이트는 사용자 액션에 영향을 줄 수 있습니다. 런타임을 업데이트하여 런타임을 실행 중인 액션을 최신 버전으로 마이그레이션해야 합니다. 

더 이상 사용되지 않는 런타임에 대한 앱은 런타임이 지원되는 런타임으로 업데이트될 때까지 성공적으로 완료할 수 없습니다. 실패한 액션에 대한 문제점 해결 시 런타임이 더 이상 사용되지 않는지 여부를 식별하려면 쿼리 응답에서 `deprecated=true`를 확인하십시오. 런타임을 업데이트하려면 [액션 런타임 변경](/docs/openwhisk?topic=cloud-functions-actions#actions_update)의 내용을 참조하십시오.

더 이상 사용되지 않는 런타임은 다음과 같습니다. 
<ul>
  <li><code>nodejs:6</code>(더 이상 사용되지 않음)</li>
  <li><code>php:7.1</code>(더 이상 사용되지 않음)</li>
  <li><code>php:7.2</code>(더 이상 사용되지 않음)</li>
  <li><code>swift:3</code>(더 이상 사용되지 않음)</li>
  <li><code>swift:3.1.1</code>(더 이상 사용되지 않음)</li>
  <li><code>swift:4.1</code>(더 이상 사용되지 않음)</li>
  <li><code>ballerina:0.990</code>(더 이상 사용되지 않음)</li>
</ul>






## JavaScript 런타임
{: #openwhisk_ref_javascript_environments}

JavaScript 액션은 Node.js 버전 8 또는 10에서 실행할 수 있습니다. 기본적으로 모든 Node.js 액션이 버전 10 환경에서 실행됩니다.



### IBM SDK의 Node.js 버전 10 환경
{: #openwhisk_ref_javascript_environments_10}
Node.js 버전 10 환경은 액션을 작성하거나 업데이트할 때 `--kind` 플래그가 `nodejs:10`의 값으로 명시적으로 지정된 경우에 사용됩니다.

#### `nodejs:8`에서 `nodejs:10`으로 마이그레이션
- `ibm_db` npm 패키지는 `nodejs:10`에서 사용할 수 없습니다. `ibm_db` 패키지는 Node.js 10을 지원하지 않습니다. [issue ibmdb/node-ibm_db/issues/482](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541)에서 이 문제의 진행상태를 추적할 수 있습니다.
- `cloudant` npm 패키지는 `nodejs:10`에서 사용할 수 없으며 더 이상 사용되지 않습니다. nodejs 모듈을 가져올 때 공식 npm 패키지 [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) v3.0.0을 사용해야 합니다(예를 들어 `require('@cloudant/cloudant')` [v3.x도 Promises만 리턴함](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x)).
- `cradle` npm 패키지는 `nodejs:10`에서 사용할 수 없습니다.
- `log4js` npm 패키지는 `nodejs:10`에서 사용할 수 없습니다. 이 문제는 [log4js-node/issues/805](https://github.com/log4js-node/log4js-node/issues/805)에서 추적할 수 있습니다.
- `watson-developer-cloud` npm 패키지는 `nodejs:10`에서 사용할 수 없습니다. 새 버전의 진행상태는 [watson-developer-cloud/node-sdk/issues/780](https://github.com/watson-developer-cloud/node-sdk/issues/780)에서 추적할 수 있습니다.

nodejs 버전 10 런타임 환경에 대한 자세한 정보는 [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md)에서 찾을 수 있습니다.

#### Node.js 10.15 패키지

   - [10.15.0](https://nodejs.org/en/blog/release/v10.15.0/)

 NPM 패키지:
   - [amqplib](https://www.npmjs.com/package/amqplib) - Node.JS용 AMQP 0-9-1 클라이언트를 작성하기 위한 라이브러리입니다.
   - [apn](https://www.npmjs.com/package/apn) - Apple Push Notification 서비스와 상호작용하기 위한 Node.js 모듈입니다.
   - [async](https://www.npmjs.com/package/async) - 비동기 함수 관련 작업을 수행하기 위한 함수를 제공합니다.
   - [bent](https://www.npmjs.com/package/bent) - async 및 await를 사용한 Node.js용 기능 HTTP 클라이언트입니다. 
   - [bodyparser](https://www.npmjs.com/package/body-parser) - 핸들러 전의 미들웨어에서 수신되는 요청 본문을 구문 분석하며, 이는 req.body 특성에서 사용 가능합니다. 
   - [btoa](https://www.npmjs.com/package/btoa) - 브라우저 btoa 함수의 포트입니다. 
   - [cassandra-driver](https://www.npmjs.com/package/cassandra-driver) - Apache Cassandra용 DataStax Node.js Driver입니다. 
   - [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) - Node.js용 공식 Cloudant 라이브러리입니다. 
   - [commander](https://www.npmjs.com/package/commander) - Node.js 명령행 인터페이스의 완전한 솔루션입니다.
   - [composeaddresstranslator](https://www.npmjs.com/package/composeaddresstranslator) - 작성 UI의 주소 변환기 또는 Scylla용 API 데이터베이스입니다.
   - [consul](https://www.npmjs.com/package/consul) - 서비스 검색 및 구성을 포함한 Consul용 클라이언트입니다. 
   - [cookie-parser](https://www.npmjs.com/package/cookie-parser) - 쿠키 헤더를 구문 분석하고 쿠키 이름으로 입력된 오브젝트로 req.cookies를 채웁니다. 
   - [elasticsearch](https://www.npmjs.com/package/elasticsearch) - Node.js용 공식 하위 레벨 Elasticsearch 클라이언트입니다.
   - [errorhandler](https://www.npmjs.com/package/errorhandler) - 개발 전용 오류 핸들러 미들웨어입니다.
   - [etcd3](https://www.npmjs.com/package/etcd3) - 프로토콜 버퍼 기반 etcdv3 API를 위한 고품질의 프로덕션 준비 클라이언트입니다. 
   - [formidable](https://www.npmjs.com/package/formidable) - 양식 데이터(특히 파일 업로드)를 구문 분석하기 위한 Node.js 모듈입니다. 
   - [glob](https://www.npmjs.com/package/glob) - 쉘에서 사용되는 패턴(예: 별과 주변 요소)을 사용하여 파일을 일치시킵니다.
   - [gm](https://www.npmjs.com/package/gm) - Node용 GraphicsMagick 및 ImageMagick입니다.
   - [ibm-cos-sdk](https://www.npmjs.com/package/ibm-cos-sdk) - Node.js용 {{site.data.keyword.cos_full}} SDK입니다.
   - [ibm_db](https://www.npmjs.com/package/ibm_db) - IBM DB2 및 IBM Informix에 대한 Node.js용 비동기/동기 인터페이스입니다.
   - [ibmiotf](https://www.npmjs.com/package/ibmiotf) - Node.js 클라이언트는 IBM Watson Internet of Things Platform과의 상호작용을 단순화하는 데 사용됩니다. 
   - [iconv-lite](https://www.npmjs.com/package/iconv-lite) - 퓨어 JS 문자 인코딩 변환입니다.
   - [jsdom](https://www.npmjs.com/package/jsdom) - jsdom은 많은 웹 표준의 퓨어 JavaScript 구현, 특히 WHATWG DOM 및 HTML 표준입니다. 
   - [jsforce](https://www.npmjs.com/package/jsforce) - Salesforce API Library for JavaScript 애플리케이션입니다.
   - [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) - JSON Web Token의 구현입니다. 
   - [lodash](https://www.npmjs.com/package/lodash) - Node.js 모듈로 내보낸 Lodash 라이브러리입니다. 
   - [marked](https://www.npmjs.com/package/marked) - 모든 기능을 갖춘 마크다운 구문 분석기 및 컴파일러이며, JavaScript에 작성됩니다. 속도에 맞게 빌드됩니다. 
   - [merge](https://www.npmjs.com/package/merge) - 여러 오브젝트를 하나로 병합하며, 선택적으로 새 복제 오브젝트를 작성합니다. 
   - [moment](https://www.npmjs.com/package/moment) - 날짜를 구분 분석하고, 유효성 검증하고, 조작하고, 형식화하기 위한 경량의 JavaScript 날짜 라이브러리입니다. 
   - [mongodb](https://www.npmjs.com/package/mongodb) - Node.js용 공식 MongoDB 드라이버입니다. 
   - [mysql](https://www.npmjs.com/package/mysql) - mysql용 Node.js 드라이버입니다.
   - [mustache](https://www.npmjs.com/package/mustache) - mustache.js는 JavaScript에 있는 mustache 템플리트 시스템의 구현입니다. 
   - [nano](https://www.npmjs.com/package/nano) - Node.js용 최소 couchdb 드라이버입니다.
   - [nodemailer](https://www.npmjs.com/package/nodemailer) - Node.js에서 이메일을 전송합니다. 매우 간단합니다!
   - [oauth2-server](https://www.npmjs.com/package/oauth2-server) - Node.js에서 표현식으로 OAuth2 서버/제공자를 구현하기 위한 완전하고, 호환되며, 성공적으로 테스트된 모듈입니다. 
   - [openwhisk](https://www.npmjs.com/package/openwhisk) - OpenWhisk 플랫폼용 JavaScript 클라이언트 라이브러리입니다. OpenWhisk API 주변에 랩퍼를 제공합니다. 
   - [path-to-regex](https://www.npmjs.com/package/path-to-regexp) - /user/:name과 같은 경로 문자열을 URL 경로에 대해 일치시키는 데 사용될 수 있는 정규식으로 변환합니다. 
   - [pg](https://www.npmjs.com/package/pg) - Node.js용 넌블로킹 PostgreSQL 클라이언트입니다. 퓨어 JavaScript 및 선택적인 기본 libpq 바인딩입니다.
   - [process](https://www.npmjs.com/package/process) - 다른 모듈과 마찬가지로 필수입니다('process').
   - [pug](https://www.npmjs.com/package/pug) - Pug 템플리트 언어를 구현합니다. 
   - [redis](https://www.npmjs.com/package/redis) - Node.js용 완전한 기능의 리치 Redis 클라이언트입니다.
   - [request](https://www.npmjs.com/package/request) - 요청은 HTTP를 호출할 수 있는 가장 간단한 방식입니다. 
   - [request-promise](https://www.npmjs.com/package/request-promise) - 단순화된 HTTP 요청 클라이언트 'request'입니다. Bluebird로 구현됩니다.
   - [rimraf](https://www.npmjs.com/package/rimraf) - 노드용 UNIX 명령 rm -rf입니다.
   - [semver](https://www.npmjs.com/package/semver) - Nodejs용 시맨틱 버전화입니다. 
   - [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail) - SendGrid API를 통해 이메일 지원을 제공합니다.
   - [serialize-error](https://www.npmjs.com/package/serialize-error) - 오류를 일반 오브젝트로 직렬화합니다. 
   - [serve-favicon](https://www.npmjs.com/package/serve-favicon) - favicon을 제공하기 위한 Node.js 미들웨어입니다.
   - [socket.io](https://www.npmjs.com/package/socket.io) - Socket.IO를 통해 실시간 양방향 이벤트 기반 통신이 가능합니다. 
   - [socket.io-client](https://www.npmjs.com/package/socket.io-client) - Socket.IO용 실시간 애플리케이션 프레임워크입니다.
   - [superagent](https://www.npmjs.com/package/superagent) - SuperAgent는 점진적인 소형 클라이언트 측 HTTP 요청 라이브러리 및 Node.js 모듈(동일한 API 사용)이며, 많은 상위 레벨 HTTP 클라이언트 기능의 이점이 있습니다. 
   - [swagger-tools](https://www.npmjs.com/package/swagger-tools) - Swagger와 통합하고 상호작용하기 위해 다양한 도구를 제공하는 패키지입니다.
   - [twilio](https://www.npmjs.com/package/twilio) - 음성, 비디오 및 메시징과 관련된 Twilio API용 랩퍼입니다. 
   - [underscore](https://www.npmjs.com/package/underscore) - Underscore.js는 코어 JavaScript 오브젝트를 확장하지 않고 일반적인 함수 suspect(each, map, reduce, filter...)에 대한 지원을 제공하는 유틸리티 벨트 라이브러리입니다. 
   - [url-pattern](https://www.npmjs.com/package/url-pattern) - regex 문자열 일치 프로그램을 사용하는 것 보다 더 쉽게 경로 매개변수에 대한 URL을 구문 분석합니다. 
   - [uuid](https://www.npmjs.com/package/uuid) - RFC4122 UUIDS를 간단하고 빠르게 생성합니다. 
   - [validator](https://www.npmjs.com/package/validator) - 문자열 유효성 검증기 및 무결 처리기의 라이브러리입니다. 
   - [vcap_services](https://www.npmjs.com/package/vcap_services) - IBM Cloud가 제공하는 VCAP_SERVICES 환경 변수에서 서비스 인증 정보를 구문 분석하고 리턴합니다. 
   - [when](https://www.npmjs.com/package/when) - When.js는 강력하며 검증된 Promises/A+ 및 when() 구현입니다(완전한 ES6 Promise shim 포함). 
   - [winston](https://www.npmjs.com/package/winston) - Node.js용 다중 전송 비동기 로깅 라이브러리입니다. "CHILL WINSTON! ... 로그에 저장합니다."
   - [ws](https://www.npmjs.com/package/ws) - ws는 사용하기 쉽고, 빠르고, 완전히 테스트된 WebSocket 클라이언트이며 서버 구현입니다. 
   - [xlsx](https://www.npmjs.com/package/xlsx) - 다양한 스프레드시트 형식에 맞는 구분 분석기 및 기록기입니다. 
   - [xml2js](https://www.npmjs.com/package/xml2js) - JavaScript 오브젝트 변환기에 대한 단순 XML입니다. 양방향 변환을 지원합니다. 
   - [xmlhttprequest](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest는 브라우저 XMLHttpRequest 오브젝트를 에뮬레이트하기 위한 기본 제공 http 클라이언트용 랩퍼입니다. 
   - [yauzl](https://www.npmjs.com/package/yauzl) - 노드를 위한 또 다른 추출 라이브러리입니다. 

### IBM SDK의 Node.js 버전 8 환경
{: #openwhisk_ref_javascript_environments_8}
Node.js 버전 8 환경은 액션을 작성하거나 업데이트할 때 `--kind` 플래그가 `nodejs:8`의 값으로 명시적으로 지정된 경우에 사용됩니다.

Node.js 버전 8은 유지보수 모드에 있으며 2019년 12월까지 사용 가능합니다. [Node.js 릴리스 스케줄](https://github.com/nodejs/Release)의 내용을 참조하십시오.
{: deprecated}

#### Node.js 8.15 패키지

 - [8.15.0](https://nodejs.org/en/blog/release/v8.15.0)

  - [amqplib](https://www.npmjs.com/package/amqplib) - Node.JS용 AMQP 0-9-1 클라이언트를 작성하기 위한 라이브러리입니다.
   - [apn](https://www.npmjs.com/package/apn) - Apple Push Notification 서비스와 상호작용하기 위한 Node.js 모듈입니다.
   - [async](https://www.npmjs.com/package/async) - 비동기 함수 관련 작업을 수행하기 위한 함수를 제공합니다.
   - [bent](https://www.npmjs.com/package/bent) - async/await를 사용한 Node.js용 기능 HTTP 클라이언트입니다. 
   - [bodyparser](https://www.npmjs.com/package/body-parser) - 핸들러 전의 미들웨어에서 수신되는 요청 본문을 구문 분석하며, 이는 req.body 특성에서 사용 가능합니다. 
   - [btoa](https://www.npmjs.com/package/btoa) - 브라우저 btoa 함수의 포트입니다. 
   - [cassandra-driver](https://www.npmjs.com/package/cassandra-driver) - Apache Cassandra용 DataStax Node.js Driver입니다. 
   - [cloudant](https://www.npmjs.com/package/cloudant) - Node.js용 공식 Cloudant 라이브러리입니다. 
   - [@cloudant/cloudant](https://www.npmjs.com/package/cloudant) - Node.js용 공식 Cloudant 라이브러리입니다. 
   - [commander](https://www.npmjs.com/package/commander) - Node.js 명령행 인터페이스의 완전한 솔루션입니다.
   - [composeaddresstranslator](https://www.npmjs.com/package/composeaddresstranslator) - 작성 UI의 주소 변환기 또는 Scylla용 API 데이터베이스입니다.
   - [consul](https://www.npmjs.com/package/consul) - 서비스 검색 및 구성을 포함한 Consul용 클라이언트입니다. 
   - [cookie-parser](https://www.npmjs.com/package/cookie-parser) - 쿠키 헤더를 구문 분석하고 쿠키 이름으로 입력된 오브젝트로 req.cookies를 채웁니다. 
   - [elasticsearch](https://www.npmjs.com/package/elasticsearch) - Node.js용 공식 하위 레벨 Elasticsearch 클라이언트입니다.
   - [errorhandler](https://www.npmjs.com/package/errorhandler) - 개발 전용 오류 핸들러 미들웨어입니다.
   - [etcd3](https://www.npmjs.com/package/etcd3) - 프로토콜 버퍼 기반 etcdv3 API를 위한 고품질의 프로덕션 준비 클라이언트입니다. 
   - [formidable](https://www.npmjs.com/package/formidable) - 양식 데이터(특히 파일 업로드)를 구문 분석하기 위한 Node.js 모듈입니다. 
   - [glob](https://www.npmjs.com/package/glob) - 쉘에서 사용되는 패턴(예: 별과 주변 요소)을 사용하여 파일을 일치시킵니다.
   - [gm](https://www.npmjs.com/package/gm) - Node용 GraphicsMagick 및 ImageMagick입니다.
   - [ibm-cos-sdk](https://www.npmjs.com/package/ibm-cos-sdk) - Node.js용 {{site.data.keyword.cos_full}} SDK입니다.
   - [ibm_db](https://www.npmjs.com/package/ibm_db) - IBM DB2 및 IBM Informix에 대한 Node.js용 비동기/동기 인터페이스입니다.
   - [ibmiotf](https://www.npmjs.com/package/ibmiotf) - Node.js 클라이언트는 IBM Watson Internet of Things Platform과의 상호작용을 단순화하는 데 사용됩니다. 
   - [iconv-lite](https://www.npmjs.com/package/iconv-lite) - 퓨어 JS 문자 인코딩 변환입니다.
   - [jsdom](https://www.npmjs.com/package/jsdom) - jsdom은 많은 웹 표준의 퓨어 JavaScript 구현, 특히 WHATWG DOM 및 HTML 표준입니다. 
   - [jsforce](https://www.npmjs.com/package/jsforce) - Salesforce API Library for JavaScript 애플리케이션입니다.
   - [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) - JSON Web Token의 구현입니다. 
   - [lodash](https://www.npmjs.com/package/lodash) - Node.js 모듈로 내보낸 Lodash 라이브러리입니다. 
   - [log4js](https://www.npmjs.com/package/log4js) - 노드 관련 작업을 수행할 log4js 프레임워크의 변환입니다. 
   - [marked](https://www.npmjs.com/package/marked) - 모든 기능을 갖춘 마크다운 구문 분석기 및 컴파일러이며, JavaScript에 작성됩니다. 속도에 맞게 빌드됩니다. 
   - [merge](https://www.npmjs.com/package/merge) - 여러 오브젝트를 하나로 병합하며, 선택적으로 새 복제 오브젝트를 작성합니다. 
   - [moment](https://www.npmjs.com/package/moment) - 날짜를 구분 분석하고, 유효성 검증하고, 조작하고, 형식화하기 위한 경량의 JavaScript 날짜 라이브러리입니다. 
   - [mongodb](https://www.npmjs.com/package/mongodb) - Node.js용 공식 MongoDB 드라이버입니다. 
   - [mysql](https://www.npmjs.com/package/mysql) - mysql용 Node.js 드라이버입니다.
   - [mustache](https://www.npmjs.com/package/mustache) - mustache.js는 JavaScript에 있는 mustache 템플리트 시스템의 구현입니다. 
   - [nano](https://www.npmjs.com/package/nano) - Node.js용 최소 couchdb 드라이버입니다.
   - [nodemailer](https://www.npmjs.com/package/nodemailer) - Node.js에서 이메일을 전송합니다. 매우 간단합니다!
   - [oauth2-server](https://www.npmjs.com/package/oauth2-server) - Node.js에서 표현식으로 OAuth2 서버/제공자를 구현하기 위한 완전하고, 호환되며, 성공적으로 테스트된 모듈입니다. 
   - [openwhisk](https://www.npmjs.com/package/openwhisk) - OpenWhisk 플랫폼용 JavaScript 클라이언트 라이브러리입니다. OpenWhisk API 주변에 랩퍼를 제공합니다. 
   - [path-to-regex](https://www.npmjs.com/package/path-to-regexp) - /user/:name과 같은 경로 문자열을 URL 경로에 대해 일치시키는 데 사용될 수 있는 정규식으로 변환합니다. 
   - [pg](https://www.npmjs.com/package/pg) - Node.js용 넌블로킹 PostgreSQL 클라이언트입니다. 퓨어 JavaScript 및 선택적인 기본 libpq 바인딩입니다.
   - [process](https://www.npmjs.com/package/process) - 다른 모듈과 마찬가지로 필수입니다('process').
   - [pug](https://www.npmjs.com/package/pug) - Pug 템플리트 언어를 구현합니다. 
   - [redis](https://www.npmjs.com/package/redis) - Node.js용 완전한 기능의 리치 Redis 클라이언트입니다.
   - [request](https://www.npmjs.com/package/request) - 요청은 HTTP를 호출할 수 있는 가장 간단한 방식입니다. 
   - [request-promise](https://www.npmjs.com/package/request-promise) - 단순화된 HTTP 요청 클라이언트 'request'입니다. Bluebird로 구현됩니다.
   - [rimraf](https://www.npmjs.com/package/rimraf) - 노드용 UNIX 명령 rm -rf입니다.
   - [semver](https://www.npmjs.com/package/semver) - Nodejs용 시맨틱 버전화입니다. 
   - [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail) - SendGrid API를 통해 이메일 지원을 제공합니다.
   - [serialize-error](https://www.npmjs.com/package/serialize-error) - 오류를 일반 오브젝트로 직렬화합니다. 
   - [serve-favicon](https://www.npmjs.com/package/serve-favicon) - favicon을 제공하기 위한 Node.js 미들웨어입니다.
   - [socket.io](https://www.npmjs.com/package/socket.io) - Socket.IO를 통해 실시간 양방향 이벤트 기반 통신이 가능합니다. 
   - [socket.io-client](https://www.npmjs.com/package/socket.io-client) - Socket.IO용 실시간 애플리케이션 프레임워크입니다.
   - [superagent](https://www.npmjs.com/package/superagent) - SuperAgent는 점진적인 소형 클라이언트 측 HTTP 요청 라이브러리 및 Node.js 모듈(동일한 API 사용)이며, 많은 상위 레벨 HTTP 클라이언트 기능의 이점이 있습니다. 
   - [swagger-tools](https://www.npmjs.com/package/swagger-tools) - Swagger와 통합하고 상호작용하기 위해 다양한 도구를 제공하는 패키지입니다.
   - [twilio](https://www.npmjs.com/package/twilio) - 음성, 비디오 및 메시징과 관련된 Twilio API용 랩퍼입니다. 
   - [underscore](https://www.npmjs.com/package/underscore) - Underscore.js는 코어 JavaScript 오브젝트를 확장하지 않고 일반적인 함수 suspect(each, map, reduce, filter...)에 대한 지원을 제공하는 유틸리티 벨트 라이브러리입니다. 
   - [url-pattern](https://www.npmjs.com/package/url-pattern) - regex 문자열 일치 프로그램을 사용하는 것 보다 더 쉽게 경로 매개변수에 대한 URL을 구문 분석합니다. 
   - [uuid](https://www.npmjs.com/package/uuid) - RFC4122 UUIDS를 간단하고 빠르게 생성합니다. 
   - [validator](https://www.npmjs.com/package/validator) - 문자열 유효성 검증기 및 무결 처리기의 라이브러리입니다. 
   - [vcap_services](https://www.npmjs.com/package/vcap_services) - IBM Cloud가 제공하는 VCAP_SERVICES 환경 변수에서 서비스 인증 정보를 구문 분석하고 리턴합니다. 
   - [watson-developer-cloud](https://www.npmjs.com/package/watson-developer-cloud) - 복잡한 문제를 해결하기 위해 코그너티브 컴퓨팅을 사용하는 API 콜렉션인 Watson Developer Cloud 서비스를 사용할 Node.js 클라이언트 라이브러리입니다. 
   - [when](https://www.npmjs.com/package/when) - When.js는 강력하며 검증된 Promises/A+ 및 when() 구현입니다(완전한 ES6 Promise shim 포함). 
   - [winston](https://www.npmjs.com/package/winston) - Node.js용 다중 전송 비동기 로깅 라이브러리입니다. "CHILL WINSTON! ... 로그에 저장합니다."
   - [ws](https://www.npmjs.com/package/ws) - ws는 사용하기 쉽고, 빠르고, 완전히 테스트된 WebSocket 클라이언트이며 서버 구현입니다. 
   - [xml2js](https://www.npmjs.com/package/xml2js) - JavaScript 오브젝트 변환기에 대한 단순 XML입니다. 양방향 변환을 지원합니다. 
   - [xmlhttprequest](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest는 브라우저 XMLHttpRequest 오브젝트를 에뮬레이트하기 위한 기본 제공 http 클라이언트용 랩퍼입니다. 
   - [yauzl](https://www.npmjs.com/package/yauzl) - 노드를 위한 또 다른 추출 라이브러리입니다. 

Node.js 버전 8 런타임 환경에 대한 자세한 정보는 [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md)에서 찾을 수 있습니다.


## Python 런타임
{: #openwhisk_ref_python_environments}

Python 액션의 경우 두 가지 다른 런타임 버전에서 선택할 수 있습니다. 기본적으로 모든 Python 액션이 버전 2 환경에서 실행됩니다.


### Python 3.7 액션(Debian Stretch 기반)
{: #openwhisk_ref_python_environments_3.7}

Python 3.7 액션은 Python 3.7.x에서 실행됩니다. 이 런타임을 사용하려면 액션을 작성하거나 업데이트할 때 CLI 매개변수 `--kind python:3.7`을 지정하십시오. 

런타임에는 Python 3.7 표준 라이브러리에 외에 Python 액션에서 사용할 수 있는 IBM Cloud 서비스에 대한 SDK 패키지가 포함되어 있습니다.

#### Python 3.7.2 패키지

 - [3.7.2](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile)

 Python 패키지:
 - asn1crypto
 - attrs
 - Automat
 - beautifulsoup4
 - botocore
 - cassandra-driver
 - certifi
 - cffi
 - chardet
 - Click
 - cloudant
 - constantly
 - cryptography
 - cssselect
 - docutils
 - elasticsearch
 - etcd3
 - Flask
 - gevent
 - greenlet
 - grpcio
 - httplib2
 - hyperlink
 - ibm-cos-sdk
 - ibm-cos-sdk-core
 - ibm-cos-sdk-s3transfer
 - ibm-db
 - ibmcloudsql
 - idna
 - incremental
 - itsdangerous
 - Jinja2
 - jmespath
 - kafka-python
 - lxml
 - MarkupSafe
 - numpy
 - pandas
 - parsel
 - pika
 - Pillow
 - protobuf
 - psycopg2
 - pyarrow
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pymongo
 - pyOpenSSL
 - python-dateutil
 - pytz
 - queuelib
 - redis
 - requests
 - scikit-learn
 - scipy
 - Scrapy
 - service-identity
 - simplejson
 - six
 - soupsieve
 - tenacity
 - tornado
 - Twisted
 - urllib3
 - virtualenv
 - w3lib
 - watson-developer-cloud
 - websocket-client
 - Werkzeug
 - zope.interface

Python 3.7 런타임 환경에 대한 자세한 정보는 [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md)에서 찾을 수 있습니다.

### Python 3.6.8 액션(Debian Jessie 기반)
{: #openwhisk_ref_python_environments_3.6}

Python 3 액션은 Python 3.6.x에서 실행됩니다. 이 런타임을 사용하려면 액션을 작성하거나 업데이트할 때 CLI 매개변수 `--kind python:3.6`을 지정하십시오. 

런타임에는 Python 3.6 표준 라이브러리에 외에 Python 액션에서 사용할 수 있는 IBM Cloud 서비스에 대한 SDK 패키지가 포함되어 있습니다.

#### Python 3.6.8 패키지

Python 버전:
 - [3.6.8](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile)

 - Python 패키지:
 - asn1crypto
 - attrs
 - autobahn
 - Automat
 - beautifulsoup4
 - botocore
 - cassandra-driver
 - certifi
 - cffi
 - chardet
 - Click
 - cloudant
 - constantly
 - cryptography
 - cssselect
 - docutils
 - elasticsearch
 - Flask
 - gevent
 - greenlet
 - httplib2
 - hyperlink
 - ibm-cos-sdk
 - ibm-cos-sdk-core
 - ibm-cos-sdk-s3transfer
 - ibm-db
 - ibmcloudsql
 - idna
 - incremental
 - itsdangerous
 - Jinja2
 - jmespath
 - kafka-python
 - lxml
 - MarkupSafe
 - numpy
 - pandas
 - parsel
 - pika
 - Pillow
 - psycopg2
 - pyarrow
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pymongo
 - pyOpenSSL
 - python-dateutil
 - pytz
 - queuelib
 - redis
 - requests
 - scikit-learn
 - scipy
 - Scrapy
 - service-identity
 - simplejson
 - six=
 - soupsieve
 - tornado
 - Twisted
 - txaio
 - urllib3
 - virtualenv
 - w3lib
 - watson-developer-cloud
 - Werkzeug
 - zope.interface

Python 3.6 런타임 환경에 대한 자세한 정보는 [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md)에서 찾을 수 있습니다.

### Python 2 액션

Python 2 액션은 액션을 작성하거나 업데이트할 때 `--kind` 플래그를 지정하지 않는 한 Python 2.7.15에서 실행됩니다.

virtualenv를 사용하여 python 액션을 작성할 때는 Docker 이미지 `openwhisk/python2action`을 사용하십시오.
Python 2.7 표준 라이브러리 외에 다음 패키지를 Python 2 액션에서 사용할 수 있습니다.

#### Python 2 패키지

 - asn1crypto
 - attrs
 - Automat
 - beautifulsoup4
 - certifi
 - cffi
 - chardet
 - Click
 - constantly
 - cryptography
 - cssselect
 - enum34
 - Flask
 - functools32
 - gevent
 - greenlet
 - httplib2
 - hyperlink
 - idna
 - incremental
 - ipaddress
 - itsdangerous
 - Jinja2
 - kafka-python
 - lxml
 - MarkupSafe
 - parsel
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pyOpenSSL
 - python-dateutil
 - queuelib
 - requests
 - Scrapy
 - service-identity
 - simplejson
 - six
 - Twisted
 - urllib3
 - virtualenv=
 - w3lib
 - Werkzeug
 - zope.interface

Python 2 런타임 환경에 대한 자세한 정보는 [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md)에서 찾을 수 있습니다.


## Swift 런타임
{: #swift-actions}

기본적으로 모든 Swift 액션이 버전 4.2 환경에서 실행됩니다.

Swift 4.x 액션 런타임에는 패키지가 포함되어 있지 않습니다. Package.swift를 사용하여 종속 항목을 포함하도록 [패키징된 swift 액션](/docs/openwhisk?topic=cloud-functions-prep#prep_swift42_single)에 대한 지시사항을 따르십시오.

Swift 4.2 액션은 단일 Swift 소스 파일을 사용할 때 다음 패키지를 사용할 수 있습니다.
- Watson Developer Cloud SDK 버전 1.2.0, https://github.com/watson-developer-cloud/swift-sdk


### 단일 소스 액션 파일을 사용한 SwiftyJSON
컴파일되지 않은 액션이 있고 `swift:3.1.1` 패키지를 사용하는 경우, 액션을 사전 컴파일하고 `swift:4.2` 유형의 액션에 사용하려는 SwiftyJSON의 버전을 지정하십시오. 


## PHP 런타임
{: #openwhisk_ref_php}

기본적으로 모든 PHP 액션이 버전 7.3 환경에서 실행됩니다.

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

## Docker 런타임
{: #openwhisk_ref_docker}

Docker 액션은 Docker 컨테이너에서 사용자 제공 2진을 실행합니다. 2진이 [python:3.6-alpine](https://hub.docker.com/r/library/python) 기반의 Docker 이미지에서 실행되므로, 2진은 이 배포와 호환 가능해야 합니다.

Docker 스켈레톤은 OpenWhisk 호환 가능 Docker 이미지를 빌드하는 편리한 방법입니다. `ibmcloud fn sdk install docker` CLI 플러그인 명령을 사용하여 스켈레톤을 설치할 수 있습니다.

기본 2진 프로그램은 컨테이너 내부의 `/action/exec`에 있어야 합니다. 실행 파일은 `JSON` 오브젝트로서 역직렬화될 수 있는 단일 명령행 인수 문자열을 통해 입력 인수를 수신합니다. 이는 직렬화된 `JSON`의 단일행 문자열로서 `stdout`을 사용하여 결과를 리턴해야 합니다.

`dockerSkeleton`에 포함된 `Dockerfile`을 수정하여 컴파일 단계나 종속 항목을 포함할 수 있습니다.



## Go 런타임
{: #runtimes_go}

기본적으로 모든 Go 액션이 버전 1.11 환경에서 실행됩니다.



## Java 런타임
{: #runtimes_java}

기본적으로 모든 Java 액션이 버전 8 환경에서 실행됩니다.



## Ruby 런타임
{: #runtimes_ruby}

기본적으로 모든 Ruby 액션이 버전 2.5 환경에서 실행됩니다.



## .NET Core 런타임
{: #runtimes_dotnet}

기본적으로 모든 .NET Core 액션이 버전 2.2 환경에서 실행됩니다.
