---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-27"

keywords: actions, serverless, javascript, node, node.js

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


# Javascript 액션 작성
{: #actions_javascript
{: #creating-and-invoking-javascript-actions}

다음 절에서는 JavaScript에서 액션에 대해 작업하는 방법을 안내합니다. 단순 액션 작성 및 호출에서 시작하십시오. 그리고 액션에 매개변수를 추가하고 매개변수로 해당 액션을 호출하십시오. 그 다음에는 기본 매개변수를 설정하고 이를 호출하십시오. 마지막으로 비동기 액션을 작성하십시오.
{: shortdesc}

## 단순 JavaScript 액션 작성 및 호출
{: #single_action_js}
{: #openwhisk_single_action_js}

다음의 단계와 예제를 검토하여 첫 번째 JavaScript 액션을 작성하십시오.

1. 이름이 `hello.js`인 파일에 다음 코드를 저장하십시오.

  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  JavaScript 파일에는 추가 기능이 포함될 수 있습니다. 그러나 편의상 `main`이라고 하는 함수가 액션에 대한 시작점을 제공하기 위해 존재해야 합니다.
  {: shortdesc}

2. Javascript 함수를 사용하여 `hello`라고 하는 액션을 작성하십시오.

  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  출력 예:
  
  ```
  ok: created action hello
  ```
  {: screen}

  소스 파일 확장자를 사용하여 액션의 유형이 판별됩니다. `.js` 소스 파일의 경우, 액션은 Node.js 런타임을 사용하여 실행됩니다. `--kind` 매개변수를 `nodejs:10` 또는 `nodejs:8`로 설정하여 JavaScript 액션에 대한 Node.js 런타임 버전을 지정할 수 있습니다. Node.js 런타임에 대한 자세한 정보는 [런타임](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments)을 참조하십시오. {: shortdesc}

3. `hello` 액션이 액션 목록에 있는지 확인하십시오.

  ```
  ibmcloud fn action list
  ```
  {: pre}

  출력 예:
  
  ```
  actions
  hello       private
  ```
  {: screen}

4. 블로킹 호출을 실행하여 클라우드에서 액션을 실행하십시오. 블로킹 호출은 요청/응답 스타일을 사용하며 활성화 결과가 사용 가능할 때까지 기다립니다. 대기 시간은 60초 또는 액션의 [시간 제한 값](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits) 중에서 더 작은 값입니다.

    ```
    ibmcloud fn action invoke --blocking hello
    ```
    {: pre}

    명령에서 다음 정보를 출력합니다.
        * 호출 결과 또는 로그를 검색하는 데 사용될 수 있는 활성화 ID(`44794bd6aab74415b4e42a308d880e5b`)
        * 호출 결과(예상 대기 시간 내에 사용 가능한 경우)

    ```
  ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

    {
        "result": {
            "payload": "Hello world"
        },
      "status": "success",
      "success": true
  }
    ```
    {: screen}
    
    활성화 모니터링에 대한 팁은 [액션 출력 모니터링](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#monitor-action-output)을 참조하십시오.
    {: tip}

5. 지금 당장 액션 결과가 필요하지 않으면 `--blocking` 플래그를 생략하여 넌블로킹 호출을 실행할 수 있습니다.

    1. 넌블로킹 호출을 실행하십시오.

        ```
        ibmcloud fn action invoke hello
        ```
        {: pre}

        출력 예:

        ```
  ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: screen}

    2. 활성화 ID를 사용하여 액션 결과를 가져오십시오.

        ```
        ibmcloud fn activation result 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: pre}

        액션 결과가 리턴됩니다.

        ```
        {
            "payload": "Hello world"
  }
        ```
        {: screen}

6. 활성화 ID 기록을 잊은 경우에는 가장 최근부터 가장 오래된 순서로 정렬된 활성화의 목록을 가져올 수 있습니다.

    ```
    ibmcloud fn activation list
    ```
    {: pre}

    출력:
    ```
      activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
    ```
    {: screen}
    
### Javascript 함수 프로토타입
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

## 비동기 액션 작성
{: #asynchronous_javascript}
{: #openwhisk_asynchrony_js}

비동기로 실행되는 Javascript 함수는 액션에서 Promise를 리턴하여 `main` 함수의 리턴 후에 활성화 결과를 리턴할 수 있습니다.

1. 이름이 `asyncAction.js`인 파일에 다음 코드를 저장하십시오.

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

    * `main` 함수는 Promise를 리턴합니다. Promise는 활성화가 아직 완료되지 않았지만 향후에 완료될 예정임을 표시합니다.

    * `setTimeout()` JavaScript 함수는 Promise의 콜백 함수를 호출하기 전에 2초 동안 대기하며, 이는 비동기 코드를 표시합니다.

    * Promise의 콜백은 둘 다 함수인 `resolve` 및 `reject` 인수를 허용합니다.

      * `resolve()`에 대한 호출은 Promise를 이행하며 활성화가 정상적으로 완료되었음을 표시합니다.
      * `reject()`에 대한 호출을 사용하면 Promise를 거부하고 활성화가 비정상적으로 완료되었음을 알릴 수 있습니다.

2. `asyncAction`이라고 하는 액션을 작성하십시오.
    ```
    ibmcloud fn action create asyncAction asyncAction.js --kind nodejs:10
    ```
    {: pre}

3. 액션을 호출하십시오.

    ```
    ibmcloud fn action invoke --result asyncAction
    ```
    {: pre}

    출력 예:

    ```
    {
        "done": true
  }
    ```
    {: screen}

4. 활성화 로그를 가져와서 활성화를 완료하는 데 걸린 시간을 확인하십시오.

  1. 활성화 ID를 가져오십시오.

      ```
      ibmcloud fn activation list --limit 1 asyncAction
      ```
      {: pre}

      출력 예:
      ```
        activations
  b066ca51e68c4d3382df2d8033265db0             asyncAction
      ```
      {: screen}

  2. 활성화 ID에 대한 로그를 가져오십시오.

      ```
      ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
      ```
      {: pre}

      `duration`은 이 활성화가 완료되는 데 2초가 조금 넘게 걸렸음을 표시합니다.

      ```
ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
          ...
          "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
          ...
      }
      ```
      {: screen}

## 외부 API를 호출하기 위해 액션 사용
{: #apicall_action}
{: #openwhisk_apicall_action}

지금까지의 예제는 자체 포함된 Javascript 함수입니다. 외부 API를 호출하는 액션을 작성할 수도 있습니다.
{: shortdesc}

다음 예제에서는 매일 우주의 고유 이미지를 제공하는 NASA APOD(Astronomy Picture of the Day) 서비스를 호출합니다.

1. 이름이 `apod.js`인 파일에 다음 코드를 저장하십시오.

    ```javascript
    let rp = require('request-promise')

    function main(params) {
        const options = {
            uri: "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo",
            json: true
        }
        return rp(options)
        .then(res => {
            return { response: res }
        })
    }
    ```
    {: codeblock}

2. NASA APOD API에 대한 호출이 실행되며 JSON 결과에서 필드가 추출됩니다. 액션에서 사용할 수 있는 Node.js 패키지에 대한 자세한 정보는 [런타임](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments)을 참조하십시오.

3. 이름이 `apod`인 액션을 작성하십시오.
    ```
    ibmcloud fn action create apod apod.js --kind nodejs:10
    ```
    {: pre}

3. `apod` 액션을 호출하십시오.
    ```
    ibmcloud fn action invoke --result apod
    ```
    {: pre}

    다음의 예제 오브젝트가 리턴됩니다.

    ```
    {
      "copyright": "Eric Houck",
    "date": "2018-03-28",
    "explanation": "Does an alignment like this occur only once in a blue moon? ...",
    "hdurl": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_1799.jpg",
    "media_type": "image",
    "service_version": "v1",
    "title": "Blue Moon Tree",
    "url": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_960.jpg"
  }
    ```
    {: screen}

## Node.js 모듈로 액션 패키징
{: #packaging_javascript_actions}
{: #openwhisk_js_packaged_action}

단일 JavaScript 소스 파일에 모든 액션 코드를 작성하는 대안으로 액션을 `npm` 패키지로서 작성할 수 있습니다.

예를 들어, 다음 파일이 있는 디렉토리를 고려하십시오.

1. 이름이 `package.json`인 파일에 다음 코드를 저장하십시오.

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

2. 이름이 `index.js`인 파일에 다음 코드를 저장하십시오.

    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    exports.main = myAction;
    ```
    {: codeblock}

    * 액션은 `exports.main`을 통해 노출됩니다.
    * 오브젝트 수락과 오브젝트 리턴(또는 오브젝트의 `Promise`)의 통상적인 시그니처를 준수하는 한 액션 핸들러는 임의의 이름을 보유할 수 있습니다.
    * 이 파일의 이름을 `index.js`로 지정하거나 `package.json`의 `main` 특성으로서 선호하는 파일 이름을 지정해야 합니다.

3. 모든 종속 항목을 로컬로 설치하십시오.

    ```
  npm install
    ```
    {: pre}

    **참고**: 대부분의 `npm` 패키지가 `npm install`에 JavaScript 소스를 설치하지만 일부는 2진 아티팩트의 설치와 컴파일도 수행합니다. 아카이브 파일 업로드는 JavaScript 종속 항목만 지원합니다. 아카이브에 2진 종속 항목이 포함된 경우에는 액션 호출이 실패할 수 있습니다.

4. 모든 종속 항목을 포함하여 모든 파일이 포함된 `.zip` 아카이브를 작성하십시오.

    ```
  zip -r action.zip *
    ```
    {: pre}

    Windows Explorer 액션을 zip 파일의 작성에 사용하면 결과적으로 잘못된 구조가 생성됩니다. {{site.data.keyword.openwhisk_short}} .zip 액션에는 zip의 루트에 `package.json`이 있어야 하지만, Windows Explorer는 이를 중첩된 폴더 내에 둡니다. 가장 안전한 옵션은 명령행 `zip` 명령을 사용하는 것입니다.
    {: tip}

5. 액션을 작성하십시오. `.zip` 아카이브에서 액션을 작성할 경우, `--kind` 매개변수에 대한 값을 설정하여 Node.js 런타임 버전을 지정해야 합니다. `nodejs:8` 또는 `nodejs:10` 사이에서 선택하십시오.

    ```
    ibmcloud fn action create packageAction action.zip --kind nodejs:10
    ```
    {: pre}

6. 액션을 호출하십시오.

    ```
    ibmcloud fn action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
    ```
    {: pre}

    출력 예:

    ```
    {
        "padded": [
            ".......................and now",
          "......for something completely",
          ".....................different"
      ]
    }
    ```
    {: screen}

## 액션을 단일 번들로 패키징
{: #webpack_javascript}
{: #openwhisk_js_webpack_action}

.zip으로 액션 패키징 시에 불필요한 파일이 너무 많이 포함되거나 보다 빠른 배치를 원하는 경우, 종속 항목이 포함된 하나의 `.js` 파일에 최소한의 코드를 쓸 수 있습니다.
{: shortdesc}

[webpack ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://webpack.js.org/concepts/) 등의 Javascript 모듈 번들러를 사용하여 액션을 패키징할 수 있다. `webpack`에서 코드를 처리할 때 이는 액션에서 요구하는 모든 모듈이 포함된 종속 항목 그래프를 반복적으로 빌드합니다.

1. 이름이 `package.json`인 파일에 다음 코드를 저장하십시오. `webpack`은 개발 종속 항목으로서 추가됩니다.
    

    ```json
    {
      "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
        "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10"
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

2. 이름이 `webpack.config.js`인 파일에 다음의 webpack 구성 코드를 저장하십시오.

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

3. 이름이 `index.js`인 파일에 다음 코드를 저장하십시오. 변수 `global.main`은 액션의 기본 함수로 설정됩니다.

    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    global.main = myAction;
    ```
    {: codeblock}

4. 모든 종속 항목을 로컬로 설치하십시오.

    ```
  npm install
    ```
    {: pre}

5. webpack 번들을 빌드하십시오.

    ```
  npm run build
    ```
    {: pre}

    `dist/bundle.js` 파일이 작성되며 액션 소스 코드로서 배치됩니다.

6. `npm` 스크립트 또는 CLI를 사용하여 액션을 작성하십시오.

    * `npm` 스크립트 사용:

        ```
  npm run deploy
        ```
        {: pre}

    *   CLI 사용:

        ```
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    `webpack`에서 빌드한 번들 파일은 JavaScript 종속 항목만 지원합니다. 번들은 `bundle.js` 파일과 함께 포함되어 있지 않으므로 이 번들이 2진 파일 종속 항목에 의존할 경우 액션 호출에 실패할 수 있습니다.
    {: tip}
    



