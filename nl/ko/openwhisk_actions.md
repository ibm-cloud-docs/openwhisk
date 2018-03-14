---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 조치 작성 및 호출
{: #openwhisk_actions}

조치는 {{site.data.keyword.openwhisk}} 플랫폼에서 실행되는 Stateless 코드 스니펫입니다. 예를 들어, 조치를 사용하여 이미지에서 얼굴을 감지하고 데이터베이스 변경에 응답하며 API 호출 세트를 집계하거나 트윗을 게시할 수 있습니다. 조치는 JavaScript, Swift, Python, PHP 함수, Java 메소드 또는 임의의 2진 호환 가능 실행 파일(Docker 컨테이너로서 패키징된 사용자 정의 실행 파일 및 Go 프로그램 포함)로 작성될 수 있습니다.
{:shortdesc}

조치는 명시적으로 호출되거나 이벤트에 대한 응답으로 실행될 수 있습니다. 어떤 경우든지, 각 조치 실행의 결과로서 고유 활성화 ID로 식별되는 활성화 레코드가 생성됩니다. 조치에 대한 입력과 조치의 결과는 키-값 쌍의 사전입니다. 여기서 키는 문자열이며 값은 유효한 JSON 값입니다. 또한 조치는 조치의 정의된 시퀀스 또는 기타 조치에 대한 호출로 작성될 수도 있습니다. 

선호하는 개발 환경에서 조치를 작성, 호출 및 디버그하는 방법에 대해 알아보십시오. 
* [JavaScript](#creating-and-invoking-javascript-actions)
* [Swift](#creating-swift-actions)
* [Python](#creating-python-actions)
* [Java](#creating-java-actions)
* [PHP](#creating-php-actions)
* [Docker](#creating-docker-actions)
* [Go](#creating-go-actions)
* [임의의 실행 파일](#creating-actions-arbitrary)

또한 다음에 대해 알아보십시오. 
* [조치 출력 감시](#watching-action-output)
* [대형 애플리케이션 지원](#large-app-support)
* [조치 나열](#listing-actions)
* [조치 삭제](#deleting-actions)
* [조치 본문 내의 조치 메타데이터에 액세스](#accessing-action-metadata-within-the-action-body)


## JavaScript 조치 작성 및 호출
{: #creating-and-invoking-javascript-actions}

다음 절에서는 Javascript의 조치 관련 작업을 안내합니다. 우선 단순 조치의 작성 및 호출에서 시작합니다. 그리고 조치에 매개변수 추가 및 매개변수로 해당 조치 호출로 이동합니다. 그 다음은 기본 매개변수 설정 및 이의 호출입니다. 그리고 비동기 조치를 작성하고 최종적으로는 조치 시퀀스에 대해 작업합니다. 


### 단순 Javascript 조치 작성 및 호출
{: #openwhisk_single_action_js}

다음의 단계와 예제를 검토하여 첫 번째 JavaScript 조치를 작성하십시오.

1. 다음 컨텐츠로 JavaScript 파일을 작성하십시오. 이 예제의 경우에 파일 이름은 'hello.js'입니다. 

  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  JavaScript 파일에는 추가 기능이 포함될 수 있습니다. 그러나 편의상 `main`이라고 하는 함수가 조치에 대한 시작점을 제공하기 위해 존재해야 합니다. 

2. 다음 JavaScript 함수에서 조치를 작성하십시오. 이 예제의 경우에는 조치를 'hello'라고 합니다. 

  ```
  wsk action create hello hello.js
  ```
  {: pre}

  ```
  ok: created action hello
  ```
CLI는 소스 파일 확장자를 사용하여 조치의 유형을 자동으로 추론합니다. `.js` 소스 파일의 경우, 조치는 Node.js 6 런타임을 사용하여 실행됩니다. `--kind nodejs:8` 매개변수를 명시적으로 지정하여 Node.js 8으로 실행되는 조치를 작성할 수도 있습니다. 자세한 정보는 Node.js 6 대 8 [참조서](./openwhisk_reference.html#openwhisk_ref_javascript_environments)를 참조하십시오. 
  
3. 작성된 조치를 나열하십시오. 

  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  hello       private
  ```

  작성된 `hello` 조치를 볼 수 있습니다. 

4. 조치를 작성한 후에는 'invoke' 명령으로 OpenWhisk의 클라우드에서 이를 실행할 수 있습니다. 명령에 플래그를 지정하여 *블로킹* 호출(즉, 요청/응답 스타일) 또는 *넌블로킹* 호출로 조치를 호출할 수 있습니다. 블로킹 호출은 활성화 결과가 사용될 수 있도록 _대기_를 요청합니다. 대기 시간은 60초 또는 조치의 [시간 제한 값](./openwhisk_reference.html#openwhisk_syslimits) 중에서 더 작은 값입니다. 활성화의 결과는 이를 대기 시간 내에 사용할 수 있는 경우에 리턴됩니다. 그렇지 않으면, 활성화가 시스템에서 처리를 계속하고 활성화 ID가 리턴되며 이에 따라 넌블로킹 요청에서와 같이 나중에 결과에 대한 확인이 가능합니다. (활성화 모니터링에 대한 팁은 [여기](#watching-action-output)를 참조하십시오.) 

  이 예제에서는 블로킹 매개변수인 `--blocking`을 사용합니다. 

  ```
  wsk action invoke --blocking hello
  ```
  {: pre}

  ```
  ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b
  ```

  ```json
  {
      "result": {
          "payload": "Hello world"
      },
      "status": "success",
      "success": true
  }
  ```

  명령을 실행하면 2개의 중요한 정보 조각이 출력됩니다. 
  * 활성화 ID(`44794bd6aab74415b4e42a308d880e5b`)
  * 호출 결과(예상 대기 시간 내에 사용 가능한 경우)

  이 경우에 결과는 JavaScript 함수에 의해 리턴되는 `Hello world` 문자열입니다. 활성화 ID를 사용하여 향후에 활성화의 결과 또는 로그를 검색할 수 있습니다.   

5. 지금 당장 조치 결과가 필요하지 않은 경우, `--blocking` 플래그를 생략하여 넌블로킹 호출을 작성할 수 있습니다. 나중에 활성화 ID를 사용하여 결과를 가져올 수 있습니다. 다음 예제를 참조하십시오. 

  ```
  wsk action invoke hello
  ```
  {: pre}

  ```
  ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
  ```

  ```
  wsk activation result 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: pre}

  ```json
  {
      "payload": "Hello world"
  }
  ```

6. 활성화 ID 기록을 잊은 경우에는 최근부터 가장 오래된 순서로 정렬된 활성화의 목록을 가져올 수 있습니다. 다음 명령을 실행하여 활성화의 목록을 가져오십시오. 

  ```
  wsk activation list
  ```
  {: pre}

  ```
  activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
  ```

### 조치에 매개변수 전달
{: #openwhisk_pass_params}

매개변수는 호출 시에 조치에 전달될 수 있습니다. 

1. 조치에서 매개변수를 사용하십시오. 예를 들어, 다음 컨텐츠로 'hello.js' 파일을 업데이트하십시오. 

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  입력 매개변수가 JSON 오브젝트 매개변수로서 `main` 함수에 전달됩니다. 이 예제에서 `name` 및 `place` 매개변수가 `params` 오브젝트에서 검색되는 방법에 유념하십시오. 

2. `name` 및 `place` 매개변수값을 전달하면서 `hello` 조치를 업데이트하고 호출하십시오. 다음 예제를 참조하십시오. 

  ```
  wsk action update hello hello.js
  ```
  {: pre}

  non-service 신임 정보 매개변수를 수정해야 하는 경우, 새 매개변수로 `action update` 명령을 수행하면 현재 존재하지만 `action update` 명령에 지정되지 않은 매개변수가 제거된다는 점에 유념하십시오. 이름이 key1 및 key2인 키가 있는 `__bx_creds` 이외의 2개의 매개변수가 있는 경우를 예로 들 수 있습니다. `action update` 명령을 `-p key1 new-value -p key2 new-value`와 함께 실행하되 `__bx_creds` 매개변수를 생략하는 경우, `action update`의 정상적인 완료 후에 `__bx_creds` 매개변수는 더 이상 존재하지 않습니다. 그리고 사용자는 서비스 신임 정보를 리바인드해야 합니다. 이는 임시 해결책이 없는 알려진 제한사항입니다. {: tip}  

3.  매개변수는 명령행에서 명시적으로 제공되거나 원하는 매개변수가 포함된 파일을 제공하여 제공될 수 있습니다. 

  명령행을 통해 직접 매개변수를 전달하려면 `--param` 플래그에 키/값 쌍을 제공하십시오. 
  ```
  wsk action invoke --result hello --param name Bernie --param place Vermont
  ```
  {: pre}

  매개변수 컨텐츠가 포함된 파일을 사용하려면 JSON 형식의 매개변수가 포함된 파일을 작성하십시오. 그리고 파일 이름은 `param-file` 플래그에 전달되어야 합니다. 

  `parameters.json`이라고 하는 다음의 예제 매개변수 파일을 참조하십시오. 
  ```json
  {
      "name": "Bernie",
      "place": "Vermont"
  }
  ```

  ```
  wsk action invoke --result hello --param-file parameters.json
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

  `--result` 옵션의 사용에 유념하십시오. 이는 CLI가 활성화의 완료를 대기한 후에 결과만 표시하는 블로킹 호출을 의미합니다. 편의상 이 옵션은 자동으로 추론되는 `--blocking` 없이 사용될 수 있습니다. 

  또한 명령행에 지정된 매개변수값이 유효한 JSON인 경우, 이는 구문 분석되어 구조화된 오브젝트로서 조치에 전송됩니다. 예를 들어, 다음으로 hello 조치를 업데이트하십시오. 

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  이제 조치는 `name` 및 `place` 필드를 보유하도록 단일 `person` 매개변수를 예상합니다. 그리고 다음 예제에서와 같이 유효한 JSON인 `person` 매개변수로 조치를 호출하십시오. 

  ```
  wsk action invoke --result hello -p person '{"name": "Bernie", "place": "Vermont"}'
  ```
  {: pre}

  조치가 지금 예상하는 구조화된 오브젝트로 `person` 매개변수값을 CLI가 자동으로 구문 분석하므로 결과는 동일합니다. 
  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

### 기본 매개변수 설정
{: #openwhisk_binding_actions}

조치는 복수의 이름 지정된 매개변수로 호출될 수 있습니다. 이전 예제의 `hello` 조치가 두 개의 매개변수(개인의 *이름* 및 개인의 출신 *지역*)를 예상한다는 점을 상기하십시오. 

매번 모든 매개변수를 조치에 전달하는 대신에 특정 매개변수를 바인드할 수 있습니다. 다음 예제는 조치의 기본값이 "Vermont" 지역이 되도록 *place* 매개변수를 바인드합니다. 

1. `--param` 옵션을 사용하여 매개변수값을 바인드하거나 매개변수가 포함된 파일을 `--param-file`에 전달하여 조치를 업데이트하십시오. 

  명령행에서 명시적으로 기본 매개변수를 지정하려면 `param` 플래그에 키/값 쌍을 제공하십시오. 

  ```
  wsk action update hello --param place Vermont
  ```
  {: pre}

  파일에서 매개변수 전달은 JSON 형식의 원하는 컨텐츠가 포함된 파일 작성을 요구합니다. 그리고 파일 이름은 `-param-file` 플래그에 전달되어야 합니다. 

  `parameters.json`이라고 하는 다음의 예제 매개변수 파일을 참조하십시오. 
  ```json
  {
      "place": "Vermont"
  }
  ```
  {: codeblock}

  ```
  wsk action update hello --param-file parameters.json
  ```
  {: pre}

2. 이번에는 `name` 매개변수만 전달하여 조치를 호출하십시오. 

  ```
  wsk action invoke --result hello --param name Bernie
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

  조치를 호출할 때 "place" 매개변수를 지정할 필요가 없었다는 점에 유념하십시오. 호출 시에 매개변수값을 지정하여 바인드된 매개변수를 여전히 겹쳐쓸 수 있습니다. 

3. `name` 및 `place` 값을 모두 전달하여 조치를 호출하십시오. 후자는 조치에 바인드된 값을 겹쳐씁니다. 

  `--param` 플래그 사용:

  ```
  wsk action invoke --result hello --param name Bernie --param place "Washington, DC"
  ```
  {: pre}

  `--param-file` 플래그 사용:

  파일 parameters.json:
  ```json
  {
    "name": "Bernie",
    "place": "Vermont"
  }
  ```
  {: codeblock}
  ```
  wsk action invoke --result hello --param-file parameters.json
  ```
  {: pre}

  ```json
  {  
      "payload": "Hello, Bernie from Washington, DC"
  }
  ```

### 조치 URL 가져오기

조치는 HTTPS 요청을 통해 REST 인터페이스를 거쳐 호출될 수 있습니다. 조치 URL을 가져오려면 다음 명령을 실행하십시오. 

```
wsk action get actionName --url
```
{: pre}

```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```

인증은 HTTPS 요청을 통해 조치를 호출할 때 제공되어야 합니다. REST 인터페이스를 사용한 조치 호출에 관한 자세한 정보는 [OpenWhisk에서 REST API 사용](./openwhisk_rest_api.html#openwhisk_rest_api)을 참조하십시오.
{: tip}

### 조치 코드 저장

기존 조치와 연관된 코드는 페치되어 로컬로 저장될 수 있습니다. 저장은 시퀀스 및 Docker 조치를 제외한 모든 조치에서 수행됩니다. 조치 코드를 파일에 저장할 때 코드는 현재 작업 디렉토리에 저장되며 저장된 파일 경로가 표시됩니다. 

1. 기존 조치 이름에 대응되는 파일 이름에 조치 코드를 저장하십시오. 조치 유형에 대응되는 파일 확장자가 사용되거나, `.zip` 유형의 확장자가 Zip 파일인 조치 코드에 대해 사용됩니다. 
  ```
  wsk action get actionName --save
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```

2. CLI가 저장된 코드의 파일 이름과 확장자를 판별할 수 있도록 허용하는 대신, `--save-as` 플래그를 사용하여 사용자 정의 파일 이름과 확장자가 제공될 수 있습니다. 
  ```
  wsk action get actionName --save-as codeFile.js
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```

### 비동기 조치 작성
{: #openwhisk_asynchrony_js}

비동기로 실행되는 Javascript 함수는 조치에서 Promise를 리턴하여 `main` 함수의 리턴 후에 활성화 결과를 리턴할 수 있습니다. 

1. `asyncAction.js`라고 하는 파일에 다음 컨텐츠를 저장하십시오. 

  ```javascript
  function main(args) {
       return new Promise(function(resolve, reject) {
         setTimeout(function() {
           resolve({ done: true });
         }, 2000);
      })
   }
  ```
  {: codeblock}

  `main` 함수가 Promise를 리턴함에 유념하십시오. 이는 활성화가 아직 완료되지 않았지만 향후에 완료될 예정임을 표시합니다. 

  이 경우에 `setTimeout()` JavaScript 함수는 콜백 함수를 호출하기 전에 2초 동안 대기하며, 이는 비동기 코드를 표시하고 Promise의 콜백 함수 내부로 이동됩니다. 

  Promise의 콜백은 둘 다 함수인 두 개의 인수 "resolve" 및 "reject"를 취합니다. `resolve()`에 대한 호출은 Promise를 이행하며 활성화가 정상적으로 완료되었음을 표시합니다. 

  `reject()`에 대한 호출을 사용하면 Promise를 거부하고 활성화가 비정상적으로 완료되었음을 알릴 수 있습니다. 

2. 다음 명령을 실행하여 조치를 작성하고 이를 호출하십시오. 

  ```
  wsk action create asyncAction asyncAction.js
  ```
  {: pre}

  ```
  wsk action invoke --result asyncAction
  ```
  {: pre}

  ```json
  {
      "done": true
  }
  ```

  비동기 조치의 블로킹 호출을 수행했다는 점에 유념하십시오. 

3. 활성화 로그를 페치하여 활성화를 완료하는 데 걸린 시간을 확인하십시오. 

  ```
  wsk activation list --limit 1 asyncAction
  ```
  {: pre}
  
  ```
  activations
  b066ca51e68c4d3382df2d8033265db0             asyncAction
  ```

  ```
  wsk activation get b066ca51e68c4d3382df2d8033265db0
  ```
  {: pre}
 
  ```json
  {
      "start": 1455881628103,
      "end":   1455881648126,
      ...
  }
  ```

  활성화 레코드에서 `start` 및 `end` 시간소인을 비교하면 이 활성화를 완료하는 데 2초가 약간 넘는 시간이 소요되었음을 확인할 수 있습니다. 

### 조치를 사용하여 외부 API 호출
{: #openwhisk_apicall_action}

지금까지의 예제는 자체 포함된 Javascript 함수입니다. 외부 API를 호출하는 조치를 작성할 수도 있습니다. 

이 예제는 Yahoo Weather 서비스를 호출하여 특정 위치에서 현재 상태를 가져옵니다. 

1. `weather.js`라고 하는 파일에 다음 컨텐츠를 저장하십시오. 

  ```javascript
  var request = require('request');

  function main(params) {
      var location = params.location || 'Vermont';
      var url = 'https://query.yahooapis.com/v1/public/yql?q=select item.condition from weather.forecast where woeid in (select woeid from geo.places(1) where text="' + location + '")&format=json';

      return new Promise(function(resolve, reject) {
          request.get(url, function(error, response, body) {
              if (error) {
                  reject(error);
              }
              else {
                  var condition = JSON.parse(body).query.results.channel.item.condition;
                  var text = condition.text;
                  var temperature = condition.temp;
                  var output = 'It is ' + temperature + ' degrees in ' + location + ' and ' + text;
                  resolve({msg: output});
              }
          });
      });
  }
  ```
  {: codeblock}

 이 예제의 조치는 Javascript `request` 라이브러리를 사용하여 Yahoo Weather API에 대한 HTTP 요청을 작성하며 JSON 결과에서 필드를 추출합니다. [참조서](./openwhisk_reference.html#openwhisk_ref_javascript_environments)는 조치에서 사용할 수 있는 Node.js 패키지를 자세히 설명합니다. 

  이 예제는 비동기 조치에 대한 필요성도 보여줍니다. 조치는 Promise를 리턴하여 함수의 리턴 시에 이 조치의 결과를 아직 사용할 수 없음을 표시합니다. 그 대신에 결과는 HTTP 호출이 완료된 후에 `request` 콜백에서 사용이 가능하며 `resolve()` 함수에 인수로서 전달됩니다. 

2. 다음 명령을 실행하여 조치를 작성하고 이를 호출하십시오. 

  ```
  wsk action create weather weather.js
  ```
  {: pre}

  ```
  wsk action invoke --result weather --param location "Brooklyn, NY"
  ```
  {: pre}

  ```json
  {
      "msg": "It is 28 degrees in Brooklyn, NY and Cloudy"
  }
  ```

### Node.js 모듈로서 조치 패키징
{: #openwhisk_js_packaged_action}

단일 JavaScript 소스 파일에 모든 조치 코드를 작성하는 대안으로 조치를 `npm` 패키지로서 작성할 수 있습니다. 다음 파일의 디렉토리를 예제로서 고려하십시오. 

첫째로 `package.json`:

```json
{
  "name": "my-action",
  "main": "index.js",
  "dependencies" : {
    "left-pad" : "1.1.3"
  }
}
```
{: codeblock}

둘째로 `index.js`:

```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}

exports.main = myAction;
```
{: codeblock}

조치는 `exports.main`을 통해 노출됩니다. 오브젝트 수락과 오브젝트 리턴(또는 오브젝트의 `Promise`)의 일반 시그니처를 준수하는 한 조치 핸들러 자체는 임의의 이름을 보유할 수 있습니다. 각각의 Node.js 규약마다 이 파일의 이름을 `index.js`로 지정하거나 package.json의 `main` 특성으로 선호하는 파일 이름을 지정해야 합니다. 

이 패키지에서 OpenWhisk 조치를 작성하려면 다음을 수행하십시오. 

1. 우선 모든 종속 항목을 로컬로 설치하십시오. 

  ```
  npm install
  ```
  {: pre}

2. 모든 파일(모든 종속 항목 포함)이 포함된 `.zip` 아카이브를 작성하십시오. 

  ```
  zip -r action.zip *
  ```
  {: pre}

  Windows Explorer 조치를 zip 파일의 작성에 사용하면 결과적으로 잘못된 구조가 생성됩니다. OpenWhisk zip 조치에는 zip의 루트에 `package.json`이 있어야 하지만, Windows Explorer는 이를 중첩된 폴더 내에 둡니다. 가장 안전한 옵션은 명령행 `zip` 명령을 사용하는 것입니다. {: tip}

3. 조치를 작성하십시오. 

  ```
  wsk action create packageAction --kind nodejs:6 action.zip
  ```
  {: pre}

  CLI 도구로 `.zip` 아카이브에서 조치를 작성하는 경우에는 `nodejs:6` 또는 `nodejs:8`을 사용하여 `--kind` 플래그에 대한 값을 명시적으로 제공해야 합니다. 

4. 이 조치를 임의의 다른 조치처럼 호출할 수 있습니다. 

  ```
  wsk action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
  ```
  {: pre}
  
  ```json
  {
      "padded": [
          ".......................and now",
          "......for something completely",
          ".....................different"
      ]
  }
  ```

마지막으로, 대부분의 `npm` 패키지가 `npm install`에 JavaScript 소스를 설치하지만 일부는 2진 아티팩트의 설치와 컴파일도 수행한다는 점에 유념하십시오. 아카이브 파일 업로드는 현재 2진 종속 항목이 아닌 JavaScript 종속 항목만 지원합니다. 아카이브에 2진 종속 항목이 포함된 경우에는 조치 호출이 실패할 수 있습니다.

### 단일 번들로서 조치 패키징
{: #openwhisk_js_webpack_action}

종속 항목이 포함된 단일 `.js` 파일에 최소 코드만 포함하는 것이 편리합니다. 이 접근 방법을 사용하는 경우에는 불필요한 파일이 포함되어 zip으로 조치를 패키징하면 너무 커지는 일부 환경에서, 보다 빠른 배치가 가능합니다. 

[webpack](https://webpack.js.org/concepts/) 등의 Javascript 모듈 번들러를 사용할 수 있습니다. 코드를 처리할 때 webpack은 조치에서 요구하는 모든 모듈이 포함된 종속성 그래프를 반복적으로 빌드합니다. 

webpack을 사용한 빠른 예제는 다음과 같습니다. 

이전 예제 `package.json`을 가져와서 `webpack`을 개발 종속 항목으로 추가하고 일부 npm 스크립트 명령을 추가하십시오. 
```json
{
  "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
    "build": "webpack --config webpack.config.js",
    "deploy": "bx wsk action update my-action dist/bundle.js --kind nodejs:8"
  },
  "dependencies": {
    "left-pad": "1.1.3"
  },
  "devDependencies": {
    "webpack": "^3.8.1"
  }
}
```
{: codeblock}

webpack 구성 파일 `webpack.config.js`을 작성하십시오. 
```javascript
var path = require('path');
module.exports = {
  entry: './index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  target: 'node'
};
```
{: codeblock}

조치의 main 함수에 `global.main` 변수를 설정하십시오.
이전 예제에서: 
```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}
global.main = myAction;
```
{: codeblock}

함수 이름이 `main`이면 대신 이 구문을 사용하십시오. 
```javascript
global.main = main;
```
{: codeblock}


`npm` 및 `webpack`을 사용하여 OpenWhisk 액션을 빌드하고 배치하려면 다음을 수행하십시오. 

1. 우선 로컬로 종속 항목을 설치하십시오. 

  ```
  npm install
  ```
  {: pre}

2. webpack 번들을 빌드하십시오. 

  ```
  npm run build
  ```
  {: pre}

  `dist/bundle.js` 파일이 작성되며 조치 소스 코드로서 배치하는 데 사용됩니다. 

3. `npm` 스크립트 또는 CLI를 사용하여 조치를 작성하십시오.  `npm` 스크립트 사용: 
  ```
  npm run deploy
  ```
  {: pre}
  CLI 사용:
  ```
  bx wsk action update my-action dist/bundle.js
  ```
  {: pre}


마지막으로, `webpack`에서 빌드한 번들 파일은 2진 종속 항목을 지원하지 않는 대신에 Javascript 종속 항목은 지원합니다. 따라서 `bundle.js` 파일에 포함되지 않으므로 번들이 2진 종속 항목에 의존하는 경우에는 조치 호출이 실패합니다. 

## 조치 시퀀스 작성
{: #openwhisk_create_action_sequence}

조치 시퀀스를 함께 체인으로 연결하는 조치를 작성할 수 있습니다. 

첫 번째 시퀀스를 작성하는 데 사용할 수 있는 `/whisk.system/utils`라고 하는 패키지에서 여러 유틸리티 조치가 제공됩니다. [패키지](./openwhisk_packages.html) 절에서 패키지와 관련된 자세한 정보에 대해 알아볼 수 있습니다. 

1. `/whisk.system/utils` 패키지의 조치를 표시하십시오. 

  ```
  wsk package get --summary /whisk.system/utils
  ```
  {: pre}

  ```
  package /whisk.system/utils: Building blocks that format and assemble data
   action /whisk.system/utils/head: Extract prefix of an array
   action /whisk.system/utils/split: Split a string into an array
   action /whisk.system/utils/sort: Sorts an array
   action /whisk.system/utils/echo: Returns the input
   action /whisk.system/utils/date: Current date and time
   action /whisk.system/utils/cat: Concatenates input into a string
  ```

  이 예제에서는 `split` 및 `sort` 조치를 사용 중입니다. 

2. 한 조치의 결과가 다음 조치에 대한 인수로 전달되도록 조치 시퀀스를 작성하십시오. 

  ```
  wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

  이 조치 시퀀스는 텍스트의 일부 행을 배열로 변환하고 해당 행을 정렬합니다. 

3. 조치를 호출하십시오. 

  ```
  wsk action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
  ```
  {: pre}

  ```json
  {
      "length": 3,
      "lines": [
          "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
  }
  ```

  결과에서 정렬된 행을 볼 수 있습니다. 

**참고**: 기본 매개변수를 제외하면 시퀀스의 조치 간에 전달된 매개변수는 명시적입니다.
따라서 조치 시퀀스에 전달된 매개변수는 시퀀스의 첫 번째 조치에만 사용 가능합니다.
시퀀스에서 첫 번째 조치의 결과는 시퀀스에서 두 번째 조치에 대한 입력 JSON 오브젝트가 됩니다(계속해서 동일하게 반복됨).
첫 번째 조치가 결과에 이를 명시적으로 포함하지 않는 한, 이 오브젝트에는 시퀀스에 원래 전달된 매개변수가 포함되지 않습니다.
조치에 대한 입력 매개변수는 조치의 기본 매개변수와 병합되며, 전자가 우선순위를 갖고 일치하는 기본 매개변수를 대체합니다.
복수의 이름 지정된 매개변수로 조치 시퀀스 호출에 대한 자세한 정보는 [기본 매개변수 설정](./openwhisk_actions.html#openwhisk_binding_actions)을 참조하십시오. 

## Python 조치 작성
{: #creating-python-actions}

Python 조치를 작성하는 프로세스는 Javascript 조치의 경우와 유사합니다. 다음 절에서는 단일 Python 조치를 작성하고, 호출하며 해당 조치에 매개변수를 추가하는 방법에 대해 안내합니다. 

### Python 조치 작성 및 호출
{: #openwhisk_actions_python_invoke}

조치는 단순히 최상위 레벨의 Python 함수입니다. 예를 들어, 다음 소스 코드로 `hello.py`라고 하는 파일을 작성하십시오. 

```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
    print(greeting)
    return {"greeting": greeting}
```
{: codeblock}

Python 조치는 항상 사전을 이용하고 사전을 생성합니다. 조치에 대한 진입 메소드는 기본적으로 `main`이지만, 기타 조치 유형에서와 같이 `--main`을 사용하여 `wsk` CLI에서 조치를 작성하도록 명시적으로 지정될 수 있습니다. 

다음과 같이 이 함수에서 `helloPython`이라고 하는 OpenWhisk 조치를 작성할 수 있습니다. 
```
wsk action create helloPython hello.py
```
{: pre}

CLI는 소스 파일 확장자에서 조치의 유형을 자동으로 추론합니다. `.py` 소스 파일의 경우, 조치는 Python 2 런타임을 사용하여 실행됩니다. `--kind python:3` 매개변수를 명시적으로 지정하여 Python 3에서 실행되는 조치를 작성할 수도 있습니다.
또한 IBM Cloudant, IBM DB2, IBM COS 및 IBM Watson 등의 IBM Cloud 서비스에 대한 추가 패키지가 포함된 `python-jessie:3` 유형의 Python 3 런타임이 있습니다.
이 Python 3 런타임에 포함된 패키지에 대한 자세한 정보는 Python 런타임 [참조서](./openwhisk_reference.html#openwhisk_ref_python_environments)를 참조하십시오. 

조치 호출은 Javascript 조치애 대해서와 같이 Python 조치에 대해서도 동일합니다. 
```
wsk action invoke --result helloPython --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

### Zip 파일에 Python 조치 패키징
{: #openwhisk_actions_python_zip}

Python 조치 및 종속 모듈을 Zip 파일에 패키징할 수 있습니다.
시작점(예: `main`)이 포함된 소스 파일의 파일 이름은 `__main__.py`여야 합니다.
예를 들어, `helper.py`라고 하는 헬퍼 모듈로 조치를 작성하려면 우선 소스 파일이 포함된 아카이브를 작성하십시오. 

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}

그리고 조치를 작성하십시오. 

```bash
wsk action create helloPython --kind python:3 helloPython.zip
```
{: pre}

(`python:3` 유형의) Python 3에 대해 이러한 단계가 표시되면, 대체 Python 유형 `python:2` 또는 `python-jessie:3`으로 동일한 작업을 수행할 수 있습니다. 


### Zip 파일에 가상 환경의 Python 조치 패키징
{: #openwhisk_actions_python_virtualenv}

Python 종속 항목을 패키징하는 다른 방법은 가상 환경(`virtualenv`)을 사용하는 것입니다. 이를 사용하면 예를 들어 [`pip`](https://packaging.python.org/installing/)를 통해 설치될 수 있는 추가 패키지를 링크할 수 있습니다. 


기본 Zip 파일 지원에서와 같이, "main" 시작점이 포함된 소스 파일의 이름은 `__main__.py`여야 합니다. 명료함을 위해, `__main__.py`의 컨텐츠가 main 함수이므로 이 예제의 경우 `hello.py`의 이름을 이전 절의 `__main__.py`로 바꿀 수 있습니다. 또한 virtualenv 디렉토리의 이름은 `virtualenv`로 지정되어야 합니다. 종속 항목을 설치하고 이를 virtualenv에 패키징하며 호환 가능한 OpenWhisk 액션을 작성하기 위한 다음의 예제 시나리오를 참조하십시오. 

OpenWhisk 런타임 컨테이너와의 호환성을 보장하려면 virtualenv 내부의 패키지 설치가 유형에 대응하는 이미지를 사용하여 대상 환경에서 완료되어야 합니다. 
- `python:2` 유형에는 Docker 이미지 `openwhisk/python2action`을 사용하십시오. 
- `python:3` 유형에는 Docker 이미지 `openwhisk/python3action`을 사용하십시오. 
- `python-jessie:3` 유형에는 Docker 이미지 `ibmfunctions/action-python-v3`을 사용하십시오. 

1. 설치할 `pip` 모듈 및 버전이 포함된 [requirements.txt ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) 파일이 제공된 경우, 다음을 실행하여 종속 항목을 설치하고 호환 가능한 Docker 이미지를 사용하여 virtualenv를 작성하십시오. 
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
    ```
    {: pre}

2. virtualenv 디렉토리 및 추가적인 Python 파일을 아카이브하십시오. 
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

3. 조치를 작성하십시오. 
    ```
    wsk action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}


## PHP 조치 작성
{: #creating-php-actions}

PHP 조치를 작성하는 프로세스는 Javascript 조치의 경우와 유사합니다. 다음 절에서는 단일 PHP 조치를 작성하고, 호출하며 해당 조치에 매개변수를 추가하는 방법에 대해 안내합니다. 

### PHP 조치 작성 및 호출
{: #openwhisk_actions_php_invoke}

조치는 단순히 최상위 레벨의 PHP 함수입니다. 예를 들어, 다음 소스 코드로 `hello.php`라고 하는 파일을 작성하십시오. 

```php
<?php
function main(array $args) : array
{
    $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
```

PHP 조치는 항상 연관 배열을 이용하고 연관 배열을 리턴합니다. 조치에 대한 진입 메소드는 기본적으로 `main`이지만, 기타 조치 유형에서와 같이 `--main`을 사용하여 `wsk` CLI에서 조치를 작성할 때 명시적으로 지정될 수 있습니다. 

다음과 같이 이 함수에서 `helloPHP`라고 하는 OpenWhisk 조치를 작성할 수 있습니다. 

```
wsk action create helloPHP hello.php
```
{: pre}

CLI는 소스 파일 확장자에서 조치의 유형을 자동으로 추론합니다. `.php` 소스 파일의 경우, 조치는 PHP 7.1 런타임을 사용하여 실행됩니다. 자세한 정보는 PHP [참조서](./openwhisk_reference.html#openwhisk_ref_php)를 참조하십시오. 

조치 호출은 Javascript 조치에 대해서와 같이 PHP 조치에 대해서도 동일합니다. 

```
wsk action invoke --result helloPHP --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

### Zip 파일에 PHP 조치 패키징
{: #openwhisk_actions_php_zip}

기타 파일 및 종속 패키지와 함께 PHP 조치를 Zip 파일에 패키징할 수 있습니다.
시작점(예: `main`)이 포함된 소스 파일의 파일 이름은 `index.php`여야 합니다.
예를 들어, `helper.php`라고 하는 두 번째 파일이 포함된 조치를 작성하려면 우선 소스 파일이 포함된 아카이브를 작성하십시오. 

```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

그리고 조치를 작성하십시오. 

```bash
wsk action create helloPHP --kind php:7.1 helloPHP.zip
```
{: pre}

## Swift 조치 작성
{: #creating-swift-actions}

Swift 조치를 작성하는 프로세스는 Javascript 조치의 경우와 유사합니다. 다음 절에서는 단일 swift 조치를 작성하고, 호출하며 해당 조치에 매개변수를 추가하는 방법에 대해 안내합니다. 

또한 온라인 [Swift 샌드박스](https://swiftlang.ng.bluemix.net)를 사용하여 시스템에 Xcode를 설치하지 않고도 Swift 코드를 테스트할 수 있습니다. 

### 조치 작성 및 호출

조치는 단순히 최상위 레벨의 Swift 함수입니다. 예를 들어, 다음 컨텐츠로 `hello.swift`라고 하는 파일을 작성하십시오. 

```swift
func main(args: [String:Any]) -> [String:Any] {
    if let name = args["name"] as? String {
        return [ "greeting" : "Hello \(name)!" ]
    } else {
        return [ "greeting" : "Hello stranger!" ]
    }
}
```
{: codeblock}

Swift 조치는 항상 사전을 이용하고 사전을 생성합니다. 

다음과 같이 이 함수에서 `helloSwift`라고 하는 {{site.data.keyword.openwhisk_short}} 조치를 작성할 수 있습니다. 

```
wsk action create helloSwift hello.swift --kind swift:3.1.1
```
{: pre}
 

이전 Swift 버전이 지원되지 않으므로 항상 `swift:3.1.1`을 지정하십시오.
{: tip}

조치 호출은 Javascript 조치에 대해서와 같이 Swift 조치에 대해서도 동일합니다. 

```
wsk action invoke --result helloSwift --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

**주의:** Linux 환경에서 실행되는 Swift 조치는 아직 개발 중이며 {{site.data.keyword.openwhisk_short}}는 일반적으로 안정성이 보장되지 않는 최신의 사용 가능한 릴리스를 사용합니다. 또한 {{site.data.keyword.openwhisk_short}}에서 사용되는 Swift의 버전이 MacOS에서 안정적 XCode 릴리스의 Swift 버전과 일치하지 않을 수 있습니다. 

### Swift 실행 파일로서 조치 패키징
{: #openwhisk_actions_swift_zip}

Swift 소스 파일로 OpenWhisk Swift 조치를 작성하는 경우, 이는 조치가 실행되기 전에 2진으로 컴파일되어야 합니다. 일단 완료되면, 조치를 유지하는 컨테이너가 제거될 때까지 조치에 대한 후속 호출이 훨씬 더 빠릅니다. 이 지연을 콜드 스타트 지연이라고 합니다. 

콜드 스타트 지연을 피하기 위해 Swift 파일을 2진으로 컴파일한 후에 Zip 파일로 OpenWhisk에 업로드할 수 있습니다. OpenWhisk 테스트 발판이 필요하므로, 2진을 작성하는 가장 손쉬운 방법은 실행되는 동일한 환경 내에서 이를 빌드하는 것입니다. 다음 단계를 참조하십시오. 

- 다음 명령을 사용하여 대화식 Swift 조치 컨테이너를 실행하십시오. 
  ```
  docker run --rm -it -v "$(pwd):/owexec" openwhisk/action-swift-v3.1.1 bash
  ```
  {: pre}
  
- 소스 코드를 복사하고 이의 빌드를 준비하십시오. 
  ```
  cp /owexec/hello.swift /swift3Action/spm-build/main.swift 
  ```
  {: pre}

  ```
  cat /swift3Action/epilogue.swift >> /swift3Action/spm-build/main.swift
  ```
  {: pre}

  ```
  echo '_run_main(mainFunction:main)' >> /swift3Action/spm-build/main.swift
  ```
  {: pre}

- (선택사항) `Package.swift` 파일을 작성하여 종속 항목을 추가하십시오. 
   ```
   swift import PackageDescription
   
   let package = Package(
     name: "Action",
         dependencies: [
             .Package(url: "https://github.com/apple/example-package-deckofplayingcards.git", majorVersion: 3),
             .Package(url: "https://github.com/IBM-Swift/CCurl.git", "0.2.3"),
             .Package(url: "https://github.com/IBM-Swift/Kitura-net.git", "1.7.10"),
             .Package(url: "https://github.com/IBM-Swift/SwiftyJSON.git", "15.0.1"),
             .Package(url: "https://github.com/watson-developer-cloud/swift-sdk.git", "0.16.0")
         ]
   )
   ```
   {: pre}

  이 예제는 `swift-watson-sdk` 및 `example-package-deckofplayingcards` 종속 항목을 추가합니다. 자체 `Package.swift`에 포함할 수 있도록 `CCurl`, `Kitura-net` 및 `SwiftyJSON`이 표준 Swift 조치에서 제공된다는 점에 유념하십시오. 

- spm-build 디렉토리에 Package.swift 복사
  ```
  cp /owexec/Package.swift /swift3Action/spm-build/Package.swift
  ```
  {: pre}

- spm-build 디렉토리로 변경
  ```
  cd /swift3Action/spm-build
  ```
  {: pre}

- Swift 조치를 컴파일하십시오. 
  ```
  swift build -c release
  ```
  {: pre}

- zip 아카이브를 작성하십시오. 
  ```
  zip /owexec/hello.zip .build/release/Action
  ```
  {: pre}

- Docker 컨테이너를 종료하십시오. 
  ```
  exit
  ```
  {: pre}

hello.zip 파일이 hello.swift 파일과 동일한 디렉토리에 작성되었음을 볼 수 있습니다.  

- 조치 이름 helloSwifty로 OpenWhisk에 이를 업로드하십시오. 
  ```
  wsk action update helloSwiftly hello.zip --kind swift:3.1.1
  ```
  {: pre}

- 얼마나 더 빠른지 확인하려면 다음을 실행하십시오.  
  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

조치를 실행하는 데 걸리는 시간은 "duration" 특성에 있으며, 이는 hello 조치의 컴파일 단계에서 실행에 걸리는 시간과 비교됩니다. 

## Java 조치 작성
{: #creating-java-actions}

Java 조치를 작성하는 프로세스는 Javascript 및 Swift 조치의 경우와 유사합니다. 다음 절에서는 단일 Java 조치를 작성하고, 호출하며 해당 조치에 매개변수를 추가하는 방법에 대해 안내합니다. 

Java 파일을 컴파일하고 테스트하며 아카이브하려면 [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html)이 로컬로 설치되어 있어야 합니다. 

### 조치 작성 및 호출
{: #openwhisk_actions_java_invoke}

Java 조치는 다음과 같은 정확한 시그니처가 있는 `main`이라고 하는 메소드가 포함된 Java 프로그램입니다. 
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

예를 들어, 다음 컨텐츠로 `Hello.java`라고 하는 Java 파일을 작성하십시오. 

```java
import com.google.gson.JsonObject;
public class Hello {
    public static JsonObject main(JsonObject args) {
        String name = "stranger";
        if (args.has("name"))
            name = args.getAsJsonPrimitive("name").getAsString();
        JsonObject response = new JsonObject();
        response.addProperty("greeting", "Hello " + name + "!");
        return response;
    }
}
```
{: codeblock}

그리고 다음과 같이 JAR 파일 `hello.jar`로 `Hello.java`를 컴파일하십시오. 
```
javac Hello.java
```
{: pre}

```
jar cvf hello.jar Hello.class
```
{: pre}

Java 파일을 컴파일하려면 [google-gson](https://github.com/google/gson)이 Java CLASSPATH에 존재해야 합니다.
{: tip}

다음과 같이 이 JAR 파일에서 `helloJava`라고 하는 OpenWhisk 조치를 작성할 수 있습니다. 

```
wsk action create helloJava hello.jar --main Hello
```
{: pre}

명령행과 `.jar` 소스 파일을 사용하는 경우에는 Java 조치를 작성 중임을 지정할 필요가 없습니다. 도구가 파일 확장자에서 이를 판별합니다. 

`--main`을 사용하여 main 클래스의 이름을 지정해야 합니다. 자격을 갖춘 main 클래스는 정적 `main` 메소드를 구현하는 클래스입니다. 클래스가 기본 패키지에 없는 경우에는 완전한 Java 클래스 이름(예: `--main com.example.MyMain`)을 사용하십시오. 

필요하면 Java 조치의 메소드 이름을 사용자 정의할 수도 있습니다. 이는 조치의 완전한 Java 메소드 이름(예: `--main com.example.MyMain#methodName`)을 지정하여 수행됩니다. 

조치 호출은 Swift 및 Javascript 조치에 대해서와 같이 Java 조치에 대해서도 동일합니다. 

```
wsk action invoke --result helloJava --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

## Docker 조치 작성
{: #creating-docker-actions}

{{site.data.keyword.openwhisk_short}} Docker 조치에서 임의의 언어로 조치를 작성할 수 있습니다. 

사용자 코드는 실행 가능 2진으로 컴파일되며 Docker 이미지에 임베드됩니다. 2진 프로그램은 `stdin`에서 입력을 가져오고, `stdout`을 통해 응답하여 시스템과 상호작용합니다. 

전제조건으로 Docker 허브 계정이 있어야 합니다. 무료 Docker ID 및 계정을 설정하려면 [Docker 허브](https://hub.docker.com)로 이동하십시오.

다음 지시사항의 경우에는 Docker 사용자 ID가 `janesmith`이고 비밀번호가 `janes_password`라고 가정합니다. CLI가 설정되었다고 가정하면 {{site.data.keyword.openwhisk_short}}에서 사용할 사용자 정의 2진을 설정하기 위한 세 개의 단계가 남아 있습니다. 그 이후에, 업로드된 Docker 이미지는 조치로서 사용될 수 있습니다. 

1. Docker 스켈레톤을 다운로드하십시오. 다음과 같이 CLI를 사용하여 이를 다운로드하고 설치할 수 있습니다. 

  ```
  wsk sdk install docker
  ```
  {: pre}

  Docker 스켈레톤이 이제 현재 디렉토리에 설치되어 있습니다. 
  
  ```
  ls dockerSkeleton/
  ```
  {: pre}

  ```
  Dockerfile      README.md       buildAndPush.sh example.c
  ```

  스켈레톤은 사용자 정의 2진 양식으로 코드가 삽입될 수 있는 Docker 컨테이너 템플리트입니다. 

2. Docker 스켈레톤에서 사용자 정의 2진을 설정하십시오. 스켈레톤에는 사용 가능한 C 프로그램이 이미 포함되어 있습니다. 

  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

  필요에 따라 이 파일을 수정하거나 Docker 이미지에 추가 코드와 종속 항목을 추가할 수 있습니다. 후자의 경우, 실행 파일을 빌드하기 위해 필요하면 `Dockerfile`을 수정할 수 있습니다. 2진은 `/action/exec`의 컨테이너 내부에 위치해야 합니다. 

  실행 파일은 명령행에서 단일 인수를 수신합니다. 이는 조치에 대한 인수를 표시하는 JSON 오브젝트의 문자열 직렬화입니다. 프로그램은 `stdout` 또는 `stderr`에 대해 로깅할 수 있습니다. 관례상 출력의 마지막 행은 _반드시_ 조치의 결과를 나타내는 문자열로 변환된 JSON 오브젝트여야 합니다. 

3. Docker 이미지를 빌드하고 제공된 스크립트를 사용하여 이를 업로드하십시오. 우선 `docker login`을 실행하여 인증한 후에 선택된 이미지 이름으로 스크립트를 실행해야 합니다. 

  ```
  docker login -u janesmith -p janes_password
  ```
  {: pre}

  ```
  cd dockerSkeleton
  ```
  {: pre}

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  `example.c` 파일의 일부가 Docker 이미지 빌드 프로세스의 일부로서 컴파일되므로 시스템에 컴파일된 C 프로그램이 필요하지 않음에 유념하십시오. 실제로, 호환 가능한 호스트 시스템에서 2진을 컴파일하지 않는 한 형식이 불일치하므로 이를 컨테이너 내부에서 실행할 수 없습니다. 

  Docker 컨테이너를 이제 OpenWhisk 액션으로서 사용할 수 있습니다. 


  ```
  wsk action create example --docker janesmith/blackboxdemo
  ```
  {: pre}

  `--docker`를 사용한 조치의 작성에 유념하십시오. 모든 Docker 이미지가 Docker 허브에 호스팅되어 있다고 가정합니다. 이 조치는 임의의 기타 {{site.data.keyword.openwhisk_short}} 조치로서 호출될 수 있습니다.  

  ```
  wsk action invoke --result example --param payload Rey
  ```
  {: pre}

  ```json
  {
      "args": {
          "payload": "Rey"
      },
      "msg": "Hello from arbitrary C program!"
  }
  ```

  Docker 조치를 업데이트하려면 `buildAndPush.sh`를 실행하여 최신 이미지를 Docker 허브로 업로드하십시오. 그러면 다음 번에 조치에 대한 코드를 실행할 때 시스템이 새 Docker 이미지를 가져올 수 있습니다. 웜 컨테이너가 없는 경우, 새 호출은 새 Docker 이미지를 사용합니다. 그러나 이전 버전의 Docker 이미지를 사용하는 웝 컨테이너가 있는 경우, 새 호출은 `wsk action update`가 실행되지 않는 한 해당 이미지를 계속 사용합니다. 이는 새 호출에 대해 Docker 가져오기를 실행하여 새 Docker 이미지를 가져오도록 시스템에 지시합니다. 

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  ```
  wsk action update example --docker janesmith/blackboxdemo
  ```
  {: pre}

  [참조](./openwhisk_reference.html#openwhisk_ref_docker) 절에서 Docker 조치 작성에 대한 자세한 정보를 찾을 수 있습니다. 

  이전 버전의 CLI에서는 매개변수 없는 `--docker`를 지원했으며, 이미지 이름은 위치 인수였습니다. Docker 조치가 (zip) 파일을 통해 초기화 데이터를 수락할 수 있도록 허용하려면, 위치 인수(있는 경우)가 대신 파일(예: Zip 파일)이 되도록 Docker 조치에 대한 사용자 환경을 정규화하십시오. 이미지 이름은 `--docker` 옵션에 따라 지정되어야 합니다. 사용자 피드백 덕분에 `--native` 인수가 `--docker openwhisk/dockerskeleton`에 대한 약칭으로 포함되므로, 표준 Docker 조치 SDK 내부에서 실행되는 실행 파일은 작성하고 배치하기가 보다 편리합니다. 
  
  예를 들어, 이 튜토리얼에서는 `/action/exec`에 있는 컨테이너 내부에 2진 실행 파일을 작성합니다. 이 파일을 로컬 파일 시스템에 복사하고 이를 `exec.zip` 파일로 압축하는 경우, 다음 명령을 사용하여 초기화 데이터로서 실행 파일을 수신하는 Docker 조치를 작성할 수 있습니다.  

  ```
  wsk action create example exec.zip --native
  ```
  {: pre}

  다음 명령과 동등한 명령은 무엇입니까?  
  ```
  wsk action create example exec.zip --docker openwhisk/dockerskeleton
  ```
  {: pre}

## 임의의 실행 파일을 사용하여 조치 작성
{: #creating-actions-arbitrary}

`--native`를 사용하여 실행 파일이 OpenWhisk 액션으로서 실행될 수 있음을 볼 수 있습니다. 여기에는 `bash` 스크립트 또는 교차 컴파일된 2진이 포함됩니다. 후자의 경우에는 2진이 `openwhisk/dockerskeleton` 이미지와 호환 가능해야 한다는 제한조건이 있습니다. 

## Go 조치 작성
{: #creating-go-actions}

`--native` 옵션은 조치로서 실행 파일의 패키징을 허용합니다. 이는 예제로서 Go에 대해 작동됩니다.
Docker 조치에서와 같이, Go 실행 파일은 명령행에서 단일 인수를 수신합니다.
이는 조치에 대한 인수를 표시하는 JSON 오브젝트의 문자열 직렬화입니다.
프로그램은 `stdout` 또는 `stderr`에 대해 로깅할 수 있습니다. 관례상 출력의 마지막 행은 _반드시_ 조치의 결과를 나타내는 문자열로 변환된 JSON 오브젝트여야 합니다. 

Go 조치의 예제는 다음과 같습니다. 
```go
package main

import "encoding/json"
import "fmt"
import "os"

func main() {
    //program receives one argument: the JSON object as a string
    arg := os.Args[1]
   
    // unmarshal the string to a JSON object
    var obj map[string]interface{}
    json.Unmarshal([]byte(arg), &obj)

    // can optionally log to stdout (or stderr)
    fmt.Println("hello Go action")

    name, ok := obj["name"].(string)
    if !ok { name = "Stranger" }

    // last line of stdout is the result JSON object as a string
    msg := map[string]string{"msg": ("Hello, " + name + "!")}
    res, _ := json.Marshal(msg)
    fmt.Println(string(res))
}
```

위의 코드를 `sample.go` 파일에 저장하고 이를 OpenWhisk에 대해 교차 컴파일하십시오. 실행 파일의 이름은 `exec`여야 합니다. 
```bash
GOOS=linux GOARCH=amd64 go build -o exec
zip exec.zip exec
wsk action create helloGo --native exec.zip
```

이 조치는 임의의 기타 조치로서 실행될 수 있습니다. 
```bash
wsk action invoke helloGo -r -p name gopher
{
    "msg": "Hello, gopher!"
}
```

로그도 역시 유사한 방법으로 검색됩니다. 
```bash
wsk activation logs --last --strip
my first Go action.
```

## 조치 출력 모니터
{: #watching-action-output}

{{site.data.keyword.openwhisk_short}} 조치는 다양한 이벤트에 대한 응답으로 또는 조치 시퀀스의 일부로서 기타 사용자에 의해 호출될 수 있습니다. 해당 경우에는 호출을 모니터하는 것이 유용합니다. 

{{site.data.keyword.openwhisk_short}} CLI를 사용하여 호출 시에 조치의 출력을 모니터할 수 있습니다. 

1. 쉘에서 다음 명령을 실행하십시오. 
  ```
  wsk activation poll
  ```
  {: pre}

  이 명령은 활성화의 로그를 지속적으로 검사하는 폴링 루프를 시작합니다. 

2. 다른 창으로 전환하여 조치를 호출하십시오. 

  ```
  wsk action invoke /whisk.system/samples/helloWorld --param payload Bob
  ```
  {: pre}

  ```
  ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
  ```

3. 폴링 창에서 활성화 로그를 관찰하십시오. 

  ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
  ```

  이와 유사하게, 폴링 유틸리티를 실행할 때마다 OpenWhisk에서 사용자 대신 실행되는 조치에 대한 로그가 실시간으로 표시됩니다. 

## 대형 애플리케이션 지원
{: #large-app-support}

조치에 대한 최대 코드 크기는 48MB입니다. 많은 써드파티 모듈, 기본 라이브러리 또는 외부 도구가 포함된 애플리케이션은 이 한계에 도달할 수 있습니다. 

48MB를 초과하는 패키지 조치(zip 또는 jar)를 작성하는 경우, 솔루션은 종속 항목으로 런타임 이미지를 확장한 후에 48MB보다 작은 아카이브나 단일 소스 파일을 사용하는 것입니다. 

예를 들어, 필수 공유 라이브러리가 포함된 사용자 정의 Docker 런타임을 빌드하면 이러한 종속 항목이 아카이브 파일에 존재할 필요가 없습니다. 개인용 소스 파일은 계속해서 아카이브에 번들링되고 런타임에 삽입될 수 있습니다. 

아카이브 파일 크기를 줄이는 다른 이점은 배치 시간 역시 개선된다는 점입니다. 

### Python 예제

다음의 Python 예제에서, opencv는 `opencv-python` 라이브러리를 포함할 수 있으며 OS 이미지에 opencv 2진을 설치할 수 있습니다. 그리고 사용자는 `requirements.txt`를 사용하고 `pip install requirements.txt`를 실행하여 추가 Python 라이브러리로 이미지를 보강할 수 있습니다. 그리고 새 이미지에서 `action.py`를 사용할 수 있습니다. 

### Node.js 예제

다음 Node.js 예제에서 OS 이미지에 추가 패키지를 설치할 수 있습니다. 

`npm`를 사용하여 opencv를 설치하십시오. 
```
npm install opencv
```
{: pre}

이와 유사하게, `package.json`이 있으면 `npm`을 사용하여 이를 설치하십시오. 
```
npm install package.json
```
{: pre}

그리고 새 이미지로 `action.js` 사용을 진행하십시오. 

## 조치 나열
{: #listing-actions}

다음 명령을 사용하여 작성된 모든 조치를 나열할 수 있습니다. 

```
wsk action list
```
{: pre}

추가 조치를 작성하면 이 목록이 더 길어지며, 관련 조치를 [패키지](./openwhisk_packages.html)로 그룹화하는 것이 유용할 수 있습니다. 특정 패키지 내의 경우로만 조치의 목록을 필터링하려면 다음의 명령 구문을 사용할 수 있습니다.  

```
wsk action list [PACKAGE NAME]
```
{: pre}

## 조치 삭제
{: #deleting-actions}

사용하기를 원하지 않는 조치를 삭제하여 정리할 수 있습니다. 

1. 다음 명령을 실행하여 조치를 삭제하십시오.
  ```
  wsk action delete hello
  ```
  {: pre}

  ```
  ok: deleted hello
  ```

2. 조치가 더 이상 조치의 목록에 나타나지 않는지 확인하십시오. 
  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  ```

## 조치 본문 내의 조치 메타데이터에 액세스
{: #accessing-action-metadata-within-the-action-body}

조치 환경에는 실행 중인 조치에 특정한 여러 특성이 포함되어 있습니다. 이는 조치가 REST API를 통해 OpenWhisk 자산에 대한 작업을 프로그래밍 방식으로 수행할 수 있도록 합니다. 또는 조치가 할당된 시간 예산을 모두 다 사용하려는 경우에 내부 알람을 설정합니다.
특성은 지원되는 모든 런타임에 대해 시스템 환경을 통해 액세스가 가능합니다(OpenWhisk Docker 스켈레톤 사용 시의 Node.js, Python, Swift, Java 및 Docker 조치). 

* `__OW_API_HOST`: 이 조치를 실행 중인 OpenWhisk 배치에 대한 API 호스트
* `__OW_API_KEY`: 조치를 호출 중인 주제의 API 키이며, 이 키는 제한된 API 키일 수 있음
* `__OW_NAMESPACE`: _활성화_에 대한 네임스페이스(이는 조치에 대한 네임스페이스와 동일하지 않을 수 있음)
* `__OW_ACTION_NAME`: 실행 중인 조치의 완전한 이름
* `__OW_ACTIVATION_ID`: 실행 중인 이 조치 인스턴스에 대한 활성화 ID
* `__OW_DEADLINE`: 이 조치가 전체 기간 할당량을 이용하는 대략적인 시간(epoch 밀리초 단위로 측정됨)
