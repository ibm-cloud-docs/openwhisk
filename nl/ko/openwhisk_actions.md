---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-23"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 액션 작성 및 호출
{: #openwhisk_actions}

액션은 {{site.data.keyword.openwhisk}} 플랫폼에서 실행되는 Stateless 코드 스니펫입니다. 예를 들어, 액션을 사용하여 이미지에서 얼굴 감지, 데이터베이스 변경에 응답, API 호출 세트의 집계 또는 트윗 게시 등을 수행할 수 있습니다.
{:shortdesc}

액션은 명시적으로 호출되거나 이벤트에 대한 응답으로 실행될 수 있습니다. 어떤 경우든지, 각 액션 실행의 결과로서 고유 활성화 ID로 식별되는 활성화 레코드가 생성됩니다. 액션에 대한 입력 및 액션의 결과는 키-값 쌍의 사전입니다. 여기서 키는 문자열이며 값은 유효한 JSON 값입니다. 또한 액션은 액션의 정의된 시퀀스 또는 기타 액션에 대한 호출로 구성될 수 있습니다.

액션은 JavaScript, Swift, Python, PHP 함수, Java 메소드 또는 임의의 2진 호환 가능 실행 파일(예: Go 프로그램 및 Docker 컨테이너로서 패키징된 사용자 정의 실행 파일)로서 작성될 수 있습니다. 선호하는 개발 환경에서 액션을 작성하고 호출하며 이를 디버그하는 방법을 알아보십시오. 

## Javascript 액션 작성
{: #creating-and-invoking-javascript-actions}

다음 절에서는 JavaScript에서 액션에 대해 작업하는 방법을 안내합니다. 단순 액션 작성 및 호출에서 시작하십시오. 그리고 액션에 매개변수를 추가하고 매개변수로 해당 액션을 호출하십시오. 그 다음에는 기본 매개변수를 설정하고 이를 호출하십시오. 마지막으로 비동기 액션을 작성하십시오. 

### 단순 JavaScript 액션 작성 및 호출
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

2. Javascript 함수를 사용하여 `hello`라고 하는 액션을 작성하십시오. 
  ```
  ibmcloud fn action create hello hello.js
  ```
  {: pre}

  출력 예:
  ```
  ok: created action hello
  ```
  {: screen}

  CLI는 소스 파일 확장자를 사용하여 액션의 유형을 자동으로 추론합니다. `.js` 소스 파일의 경우, 액션은 Node.js 6 런타임을 사용하여 실행됩니다. `--kind nodejs:8` 매개변수를 명시적으로 지정하여 Node.js 8으로 실행되는 액션을 작성할 수도 있습니다. 자세한 정보는 Node.js 6 대 8 [참조서](./openwhisk_reference.html#openwhisk_ref_javascript_environments)를 참조하십시오.

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

4. 블로킹 호출을 실행하여 클라우드에서 액션을 실행하십시오. 블로킹 호출은 요청/응답 스타일을 사용하며 활성화 결과가 사용 가능할 때까지 기다립니다. 대기 시간은 60초 또는 액션의 [시간 제한 값](./openwhisk_reference.html#openwhisk_syslimits) 중에서 더 작은 값입니다.
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
    활성화 모니터링에 대한 팁은 [액션 출력 모니터링](openwhisk_managing.html#monitor-action-output)을 참조하십시오.
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

### 비동기 액션 작성
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
    ibmcloud fn action create asyncAction asyncAction.js
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

3. 활성화 로그를 가져와서 활성화를 완료하는 데 걸린 시간을 확인하십시오. 

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

### 외부 API를 호출하기 위해 액션 사용
{: #openwhisk_apicall_action}

지금까지의 예제는 자체 포함된 Javascript 함수입니다. 외부 API를 호출하는 액션을 작성할 수도 있습니다.
{: shortdesc}

다음 예제에서는 매일 우주의 고유 이미지를 제공하는 NASA APOD(Astronomy Picture of the Day) 서비스를 호출합니다.

1. 이름이 `apod.js`인 파일에 다음 코드를 저장하십시오.
    ```javascript
    var url = "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo";

    $.ajax({
    url: url,
    success: function(result){
    if("copyright" in result) {
        $("#copyright").text("Image Credits: " + result.copyright);
    }
    else {
        $("#copyright").text("Image Credits: " + "Public Domain");
      }

      if(result.media_type == "video") {
        $("#apod_img_id").css("display", "none");
      $("#apod_vid_id").attr("src", result.url);
    }
    else {
        $("#apod_vid_id").css("display", "none");
      $("#apod_img_id").attr("src", result.url);
    }
      $("#reqObject").text(url);
    $("#returnObject").text(JSON.stringify(result, null, 4));
    $("#apod_explaination").text(result.explanation);
    $("#apod_title").text(result.title);
  }
    });
    ```
    {: codeblock}

    NASA APOD API에 대한 호출이 실행되며 JSON 결과에서 필드가 추출됩니다. 액션에서 사용할 수 있는 Node.js 패키지에 대한 자세한 정보는 [시스템 세부사항 및 한계](./openwhisk_reference.html#openwhisk_ref_javascript_environments)를 참조하십시오. 

2. 이름이 `apod`인 액션을 작성하십시오.
    ```
    ibmcloud fn action create apod apod.js
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

### Node.js 모듈로 액션 패키징
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
    * 이 파일의 이름을 **index.js**로 지정하거나 **package.json**의 `main` 특성으로서 선호하는 파일 이름을 지정해야 합니다. 

3. 모든 종속 항목을 로컬로 설치하십시오.
    ```
  npm install
    ```
    {: pre}
    **참고**: 대부분의 `npm` 패키지가 `npm install`에 JavaScript 소스를 설치하지만 일부는 2진 아티팩트의 설치와 컴파일도 수행합니다. 아카이브 파일 업로드는 현재 JavaScript 종속 항목만 지원합니다. 아카이브에 2진 종속 항목이 포함된 경우에는 액션 호출이 실패할 수 있습니다. 

4. 모든 종속 항목을 포함하여 모든 파일이 포함된 `.zip` 아카이브를 작성하십시오.
    ```
  zip -r action.zip *
    ```
    {: pre}

    Windows Explorer 액션을 zip 파일의 작성에 사용하면 결과적으로 잘못된 구조가 생성됩니다. {{site.data.keyword.openwhisk_short}} zip 액션에는 zip의 루트에 `package.json`이 있어야 하지만, Windows Explorer는 이를 중첩된 폴더 내에 둡니다. 가장 안전한 옵션은 명령행 `zip` 명령을 사용하는 것입니다.
    {: tip}

5. 액션을 작성하십시오. `.zip` 아카이브에서 액션을 작성하는 경우에는 `nodejs:6` 또는 `nodejs:8`을 사용하여 `--kind` 플래그에 대한 값을 명시적으로 제공해야 합니다.
    ```
    ibmcloud fn action create packageAction --kind nodejs:6 action.zip
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

### 액션을 단일 번들로 패키징
{: #openwhisk_js_webpack_action}

zip으로 액션 패키징 시에 불필요한 파일이 너무 많이 포함되거나 보다 빠른 배치를 원하는 경우, 종속 항목이 포함된 하나의 `.js` 파일에 최소한의 코드를 쓸 수 있습니다.
{: shortdesc}

[webpack ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://webpack.js.org/concepts/) 등의 Javascript 모듈 번들러를 사용하여 액션을 패키징할 수 있다. `webpack`에서 코드를 처리할 때 이는 액션에서 요구하는 모든 모듈이 포함된 종속 항목 그래프를 반복적으로 빌드합니다. 

1. 이름이 `package.json`인 파일에 다음 코드를 저장하십시오. `webpack`은 개발 종속 항목으로서 추가됩니다.
    ```json
    {
      "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
        "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:8"
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
        ibmcloud fn action update my-action dist/bundle.js
        ```
        {: pre}

    **참고**: `webpack`에서 빌드한 번들 파일은 Javascript 종속 항목만 지원합니다. `bundle.js` 파일에 포함되지 않으므로, 번들이 2진 종속 항목에 의존하는 경우에는 액션 호출이 실패할 수 있습니다. 

## Python 액션 작성
{: #creating-python-actions}

다음 절에서는 단일 Python 액션의 작성 및 호출과 해당 액션에 매개변수 추가에 대해 안내합니다. 

### Python 액션 작성 및 호출
{: #openwhisk_actions_python_invoke}

액션은 단순히 최상위 레벨의 Python 함수입니다. Python 액션을 작성하려면 다음을 수행하십시오. 

1. `hello.py`라고 하는 파일에 다음 코드를 저장하십시오.
    ```python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
        print(greeting)
    return {"greeting": greeting}
    ```
    {: codeblock}

  * Python 액션에서는 항상 사전을 이용하고 사전을 생성합니다.
  * 액션에 대한 진입 메소드는 기본적으로 `main`이지만, `--main` 플래그를 사용하여 `wsk` CLI에서 액션을 작성하도록 명시적으로 지정될 수 있습니다. 

2. `helloPython` 액션을 작성하십시오.
    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    출력 예:
    ```
    ok: created action helloPython
    ```
    {: screen}

    CLI는 소스 파일 확장자를 사용하여 액션의 유형을 자동으로 추론합니다. `.py` 소스 파일의 경우, 액션은 Python 2 런타임을 사용하여 실행됩니다. `--kind python:3` 매개변수를 명시적으로 지정하여 Python 3에서 실행되는 액션을 작성할 수도 있습니다. {{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.Db2_on_Cloud_long_notm}}, {{site.data.keyword.cos_full_notm}} 및 {{site.data.keyword.ibmwatson_notm}} 등의 IBM Cloud 서비스에 대한 추가 패키지가 포함된 `python-jessie:3` 유형의 Python 3 런타임을 사용할 수도 있습니다. 이 Python 3 런타임에 포함된 패키지에 대한 자세한 정보는 Python 런타임 [참조서](./openwhisk_reference.html#openwhisk_ref_python_environments)를 참조하십시오.

3. 액션을 호출하십시오.
    ```
    ibmcloud fn action invoke --result helloPython --param name World
    ```
    {: pre}

    출력 예:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### zip 파일로 Python 액션 패키징
{: #openwhisk_actions_python_zip}

Python 액션 및 종속 모듈을 Zip 파일에 패키징할 수 있습니다. 예를 들어, `helper.py`라고 하는 헬퍼 모듈로 액션을 작성하려면 다음을 수행하십시오. 

1. 소스 파일이 포함된 아카이브를 작성하십시오. **참고**: 시작점이 포함된 소스 파일의 이름은 `__main__.py`여야 합니다.
    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. 액션을 작성하십시오.
    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

### zip 파일에 가상 환경의 Python 액션 패키징
{: #openwhisk_actions_python_virtualenv}

가상 환경 `virtualenv`를 사용하여 Python 종속 항목을 패키징할 수 있습니다. 가상 환경은 예를 들어 [`pip` ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://packaging.python.org/installing/)를 사용하여 설치될 수 있는 추가 패키지를 링크할 수 있도록 허용합니다. 

종속 항목을 설치하려면 가상 환경에서 이를 패키징하고 호환 가능한 OpenWhisk 액션을 작성하십시오. 

1. 설치할 `pip` 모듈과 버전이 포함된 [requirements.txt ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) 파일을 작성하십시오. 

2. 종속 항목을 설치하고 가상 환경을 작성하십시오. 가상 환경 디렉토리의 이름은 `virtualenv`여야 합니다. OpenWhisk 런타임 컨테이너와의 호환성을 보장하려면 가상 환경 내의 패키지 설치에서 해당 유형에 대응되는 이미지를 사용해야 합니다. 
    - `python:2` 유형에는 Docker 이미지 `openwhisk/python2action`을 사용하십시오.
    - `python:3` 유형에는 Docker 이미지 `openwhisk/python3action`을 사용하십시오.
    - `python-jessie:3` 유형에는 Docker 이미지 `ibmfunctions/action-python-v3`을 사용하십시오.
    ```
docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp &&virtualenv virtualenv &&source virtualenv/bin/activate &&pip install -r requirements.txt"
    ```
    {: pre}

3. `virtualenv` 디렉토리 및 추가 Python 파일을 패키징하십시오. 시작점이 포함된 소스 파일의 이름은 `__main__.py`여야 합니다.
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. `helloPython` 액션을 작성하십시오.
    ```
    ibmcloud fn action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}

선택된 런타임 환경의 일부가 아닌 모듈만 `requirements.txt`에 추가하십시오. 그러면 `virtualenv`를 최소 크기로 유지하는 데 도움이 됩니다.
{: tip}

## PHP 액션 작성
{: #creating-php-actions}

다음 절에서는 단일 PHP 액션의 작성 및 호출과 해당 액션에 매개변수 추가에 대해 안내합니다. 

### PHP 액션 작성 및 호출
{: #openwhisk_actions_php_invoke}

액션은 단순히 최상위 레벨의 PHP 함수입니다. PHP 액션을 작성하려면 다음을 수행하십시오. 

1. `hello.php`라고 하는 파일에 다음 코드를 저장하십시오.
    ```
    <?php
function main(array $args) : array
{
        $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
    ```
    {: codeblock}

    * PHP 액션은 항상 연관 배열을 이용하고 연관 배열을 리턴합니다.
    * 액션에 대한 진입 메소드는 기본적으로 `main`이지만, `--main` 플래그를 사용하여 `ibmcloud fn` CLI에서 액션을 작성할 때 명시적으로 지정될 수 있습니다. 

2. `helloPHP`라고 하는 액션을 작성하십시오.
    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    CLI는 소스 파일 확장자에서 액션의 유형을 자동으로 추론합니다. `.php` 소스 파일의 경우, 액션은 PHP 7.1 런타임을 사용하여 실행됩니다. 자세한 정보는 PHP [참조서](./openwhisk_reference.html#openwhisk_ref_php)를 참조하십시오.

3. 액션을 호출하십시오.
    ```
    ibmcloud fn action invoke --result helloPHP --param name World
    ```
    {: pre}

    출력 예:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### zip 파일로 PHP 액션 패키징
{: #openwhisk_actions_php_zip}

PHP 액션 및 기타 파일 또는 종속 패키지를 Zip 파일에 패키징할 수 있습니다. 예를 들어, `helper.php`라고 하는 두 번째 파일로 액션을 패키징하려면 다음을 수행하십시오. 

1. 소스 파일이 포함된 아카이브를 작성하십시오. **참고**: 시작점이 포함된 소스 파일의 이름은 `index.php`여야 합니다.
    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. 액션을 작성하십시오.
    ```bash
    ibmcloud fn action create helloPHP --kind php:7.1 helloPHP.zip
    ```
    {: pre}

## Swift 액션 작성
{: #creating-swift-actions}

다음 절에서는 단일 Swift 액션 작성 및 호출과 zip 파일에 액션 패키징에 대해 안내합니다. 

**참고:** Swift 액션은 Linux 환경에서 실행됩니다. Swift on Linux는 아직 개발 중이며 {{site.data.keyword.openwhisk_short}}에서는 최신 사용 가능 릴리스를 사용합니다. 이 릴리스는 안정적이지 않을 수 있습니다. {{site.data.keyword.openwhisk_short}}에서 사용되는 Swift의 버전이 MacOS에서 안정적인 XCode 릴리스의 Swift 버전과 일치하지 않을 수 있습니다. 

Swift 런타임에 대한 자세한 정보는 Swift [참조](./openwhisk_reference.html#swift-actions)를 참조하십시오.
{: tip}

### 액션 작성 및 호출
{: #openwhisk_actions_swift_invoke}

#### Swift 3
{: #openwhisk_actions_swift3_invoke}

액션은 단순히 최상위 레벨의 Swift 함수입니다. Swift 3 액션을 작성하려면 다음을 수행하십시오. 

1. `hello.swift`라고 하는 파일에 다음 코드를 저장하십시오.
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

    이 예제에서 Swift 액션은 사전을 이용하고 사전을 생성합니다.

2. `helloSwift`라고 하는 액션을 작성하십시오.
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

    CLI는 소스 파일 확장자에서 액션의 유형을 자동으로 추론합니다. `.php` 소스 파일의 경우, 액션은 PHP 7.1 런타임을 사용하여 실행됩니다. 자세한 정보는 PHP [참조서](./openwhisk_reference.html#openwhisk_ref_php)를 참조하십시오.

3. 액션을 호출하십시오.
    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    출력 예:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

#### Swift 4
{: #openwhisk_actions_swift4_invoke}

기본 기능 시그니처에 추가하여, Swift 4에서는 [Codable ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.apple.com/documentation/swift/codable) 유형을 활용하는 2개의 추가 시그니처를 제공합니다. [여기서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types) JSON 등의 외부 표현과의 호환성을 위해 인코딩 및 디코딩 가능한 데이터 유형에 대해 자세히 알아볼 수 있습니다. 

1. `hello.swift`라고 하는 파일에 다음 코드를 저장하십시오.
    ```swift
    struct Input: Codable {
        let name: String?
    }
    struct Output: Codable {
        let greeting: String
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
        let result = Output(greeting: "Hello \(param.name ?? "stranger")!")
        print("Log greeting:\(result.greeting)")
        completion(result, nil)
    }
    ```
    {: codeblock}
    이 예제에서는 `name` 필드에서 `Codable Input`으로 입력 매개변수를 취하고 `greetings` 필드에서 `Codable output`을 리턴합니다. 

2. `helloSwift`라고 하는 액션을 작성하십시오.
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.1
    ```
    {: pre}

3. 액션을 호출하십시오.
    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    출력 예:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### 액션을 Swift 실행 파일로 패키징
{: #packaging-an-action-as-a-swift-executable}

Swift 소스 파일로 {{site.data.keyword.openwhisk_short}} Swift 액션을 작성하는 경우, 해당 파일은 액션이 실행되기 전에 2진으로 컴파일되어야 합니다. 이 지연을 콜드 스타트 지연이라고 합니다. 일단 2진이 작성되면, 액션을 보유한 컨테이너가 제거될 때까지 액션에 대한 후속 호출이 훨씬 더 빠릅니다. 콜드 스타트 지연을 피하기 위해 Swift 파일을 2진으로 컴파일한 후에 Zip 파일로 {{site.data.keyword.openwhisk_short}}에 2진을 업로드할 수 있습니다. 

스크립트를 사용하여 액션 패키징을 자동화할 수 있습니다.

**전제조건**: 다음 단계에서 사용되는 스크립트에서는 사용자에게 액션을 나타내는 각 최상위 레벨 디렉토리와 함께 `actions`라고 하는 디렉토리가 있다고 가정합니다. 
```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```

1. 이름이 `compile.sh`인 파일에 다음 코드를 저장하십시오.
    ```bash
    #!/bin/bash
    set -ex

    if [ -z "$1" ] ; then
    echo 'Error: Missing action name'
    exit 1
fi
if [ -z "$2" ] ; then
    echo 'Error: Missing kind, for example swift:4.1'
        exit 2
    fi
    OUTPUT_DIR="build"
    if [ ${2} == "swift:3.1.1" ]; then
      BASE_PATH="/swift3Action"
      DEST_SOURCE="$BASE_PATH/spm-build"
      RUNTIME="openwhisk/action-swift-v3.1.1"
    elif [ ${2} == "swift:4.1" ]; then
      RUNTIME="ibmfunctions/action-swift-v4.1"
      BASE_PATH="/swift4Action"
      DEST_SOURCE="/$BASE_PATH/spm-build/Sources/Action"
    else
      echo "Error: Kind $2 not recognize"
      exit 3
    fi
    DEST_PACKAGE_SWIFT="$BASE_PATH/spm-build/Package.swift"

    BUILD_FLAGS=""
    if [ -n "$3" ] ; then
        BUILD_FLAGS=${3}
    fi

    echo "Using runtime $RUNTIME to compile swift"
docker run --rm --name=compile-ow-swift -it -v "$(pwd):/owexec" $RUNTIME bash -ex -c "

    if [ -f \"/owexec/$OUTPUT_DIR/$1.zip\" ] ; then
    rm \"/owexec/$OUTPUT_DIR/$1.zip\"
fi

    echo 'Setting up build...'
    cp /owexec/actions/$1/Sources/*.swift $DEST_SOURCE/

    # action file can be either {action name}.swift or main.swift
if [ -f \"$DEST_SOURCE/$1.swift\" ] ; then
    echo 'renaming $DEST_SOURCE/$1.swift $DEST_SOURCE/main.swift'
    mv \"$DEST_SOURCE/$1.swift\" $DEST_SOURCE/main.swift
fi
# Add in the OW specific bits
cat $BASE_PATH/epilogue.swift >> $DEST_SOURCE/main.swift
echo '_run_main(mainFunction:main)' >> $DEST_SOURCE/main.swift

    # Only for Swift4
    if [ ${2} != "swift:3.1.1" ]; then
      echo 'Adding wait to deal with escaping'
      echo '_ = _whisk_semaphore.wait(timeout: .distantFuture)' >> $DEST_SOURCE/main.swift
    fi

    echo \"Compiling $1...\"
    cd /$BASE_PATH/spm-build
cp /owexec/actions/$1/Package.swift $DEST_PACKAGE_SWIFT
# we have our own Package.swift, do a full compile
swift build ${BUILD_FLAGS} -c release

    echo 'Creating archive $1.zip...'
    #.build/release/Action
mkdir -p /owexec/$OUTPUT_DIR
zip \"/owexec/$OUTPUT_DIR/$1.zip\" .build/release/Action
"
    ```
    {: codeblock}

2. 종속 항목을 추가하려면 `Package.swift` 파일을 작성하십시오. 다음 예제에서는 `example-package-deckofplayingcards`를 종속 항목으로 추가합니다. 
`CCurl`, `Kitura-net` 및 `SwiftyJSON`이 표준 Swift 액션에서 제공되므로, Swift 3 액션의 경우에만 이를 자체 `Package.swift`에 포함해야 합니다. 
    * Swift 3 구문 예:
        ```swift
        import PackageDescription

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
        {: codeblock}

    * Swift 4 구문 예:
        ```swift
        // swift-tools-version:4.0
        import PackageDescription

        let package = Package(
      name: "Action",
      products: [
              .executable(
          name: "Action",
          targets:  ["Action"]
        )
            ],
      dependencies: [
              .package(url: "https://github.com/apple/example-package-deckofplayingcards.git", .upToNextMajor(from: "3.0.0"))
            ],
      targets: [
              .target(
          name: "Action",
          dependencies: ["DeckOfPlayingCards"],
          path: "."
        )
            ]
        )
        ```
        {: codeblock}

3. `build`에서 `hello.zip`을 작성하려면 액션을 빌드하십시오. 
    * Swift 3:
      ```
  bash compile.sh hello swift:3.1.1
      ```
      {: pre}

    * Swift 4:
      ```
  bash compile.sh hello swift:4.1
      ```
      {: pre}

4. 액션 이름 `helloSwiftly`로 {{site.data.keyword.openwhisk_short}}에 zip을 업로드하십시오. 
    * Swift 3:
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:3.1.1
      ```
      {: pre}

    * Swift 4:
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:4.1
      ```
      {: pre}

5. 액션을 호출하십시오.
    ```
    ibmcloud fn action invoke helloSwiftly --blocking
    ```
    {: pre}

    액션을 실행하는 데 걸린 시간은 `duration1 특성에 있습니다. 

6. 사전 컴파일된 액션 호출의 지속 기간을 컴파일 단계에서 명령 호출의 지속 기간과 비교할 수 있습니다. 이전 섹션에서 액션을 호출하십시오.
    ```
    ibmcloud fn action invoke --result helloSwift --param name World --blocking
    ```
    {: pre}

### Swift 4의 오류 처리
{: #error-handling-swift4}

Codable 완료 핸들러를 사용하면 액션의 실패를 표시하는 오류를 전달할 수 있습니다. [Swift의 오류 처리 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)는 `try`, `catch` 및 `throw` 키워드의 사용으로 기타 언어의 예외 처리와 유사합니다. 

다음 스니펫은 오류 처리의 예제를 표시합니다. 
```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    // Return real error
    do{
        throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
    } catch {
        completion(nil, error)
    }
}
```
{: codeblock}

## Java 액션 작성
{: #creating-java-actions}

다음 절에서는 단일 Java 액션의 작성 및 호출과 해당 액션에 매개변수 추가에 대해 안내합니다. 

Java 파일을 컴파일, 테스트하고 아카이브하려면 [JDK 8 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://openjdk.java.net/install)이 로컬로 설치되어 있어야 합니다.

### Java 액션 작성 및 호출
{: #openwhisk_actions_java_invoke}

Java 액션은 `main`이라고 하는 메소드가 포함된 Java 프로그램입니다. `main`에는 다음과 정확히 동일한 시그니처가 있어야 합니다. 
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Java 액션을 작성하려면 다음을 수행하십시오. 

1. 이름이 `Hello.java`인 파일에 다음 코드를 저장하십시오.
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

2. `Hello.java`를 이름이 `hello.jar`인 JAR 파일로 컴파일하십시오. **참고**: [google-gson ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/google/gson)이 Java CLASSPATH에 존재해야 합니다.
    ```
javac Hello.java
    ```
    {: pre}
    ```
jar cvf hello.jar Hello.class
    ```
    {: pre}

3. 액션을 작성하십시오.
    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}
  * `--main`을 사용하여 main 클래스의 이름을 지정해야 합니다. 자격을 갖춘 main 클래스는 정적 `main` 메소드를 구현하는 클래스입니다. 클래스가 기본 패키지에 없는 경우에는 완전한 Java 클래스 이름(예: `--main com.example.MyMain`)을 사용하십시오.
  * Java 액션의 메소드 이름을 사용자 정의할 수 있습니다. 이는 액션의 완전한 메소드 이름(예: `--main com.example.MyMain#methodName`)을 지정하여 수행됩니다. 
  * CLI는 소스 파일 확장자에서 액션의 유형을 자동으로 추론합니다.

4. 액션을 호출하십시오.
    ```
    ibmcloud fn action invoke --result helloJava --param name World
    ```
    {: pre}

    출력 예:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Docker 액션 작성
{: #creating-docker-actions}

{{site.data.keyword.openwhisk_short}} Docker 액션을 사용하면 사용자의 액션을 어떤 언어로든 작성할 수 있습니다.
{: shortdesc}

사용자 코드는 실행 가능 2진으로 컴파일되며 Docker 이미지에 임베드됩니다. 2진 프로그램은 `stdin`에서 입력을 가져오고, `stdout`을 통해 응답하여 시스템과 상호작용합니다.   [참조](./openwhisk_reference.html#openwhisk_ref_docker) 절에서 Docker 액션 작성에 대한 자세한 정보를 찾을 수 있습니다.

전제조건: Docker 허브 계정이 있어야 합니다.  [Docker 허브 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://hub.docker.com)에서 무료 Docker ID 및 계정을 설정하십시오. 

사용자 정의 2진 설정 및 액션으로서 업로드된 Docker 이미지 사용: 

1. Docker 스켈레톤을 다운로드하고 설치하십시오. 스켈레톤은 사용자 정의 2진 양식으로 코드가 삽입될 수 있는 Docker 컨테이너 템플리트입니다.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Docker 스켈레톤에서 사용자 정의 2진을 설정하십시오. 스켈레톤에는 사용 가능한 C 프로그램이 포함되어 있습니다. `example.c` 파일의 일부가 Docker 이미지 빌드 프로세스의 일부로서 컴파일되므로, 시스템에서 컴파일된 C 프로그램은 필요하지 않습니다. 
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  출력 예:
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. 선택사항: 실행 파일 빌드를 위해 `Dockerfile`을 수정하여 Docker 이미지에 추가로 코드 및 종속 항목을 추가하십시오. 다음 요구사항과 제한사항을 참고하십시오. 
  * 2진은 `/action/exec`의 컨테이너 내부에 위치해야 합니다.
  * 실행 파일은 명령행에서 단일 인수를 수신합니다. 이 인수는 액션에 대한 인수를 표시하는 JSON 오브젝트의 문자열 직렬화입니다. 
  * 프로그램은 `stdout` 또는 `stderr`에 대해 로깅할 수 있습니다.
  * 관례상 출력의 마지막 행은 액션의 결과를 나타내는 문자열로 변환된 JSON 오브젝트여야 합니다. 

4. Docker 이미지를 빌드하고 제공된 스크립트를 사용하여 이를 업로드하십시오.
    1. Docker에 로그인하십시오.
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. `dockerSkeleton` 디렉토리로 이동하십시오.
        ```
  cd dockerSkeleton
        ```
        {: pre}

    3. 스크립트를 실행하십시오.
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}

5. Docker 컨테이너를 사용하여 액션을 작성하십시오.
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. 액션을 호출하십시오.
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    출력 예:
    ```
    {
        "args": {
            "payload": "Rey"
        },
      "msg": "Hello from arbitrary C program!"
  }
    ```
    {: screen}

7. Docker 액션을 업데이트하려면 최신 이미지를 Docker 허브로 업로드하십시오. 그러면 다음 번에 액션에 대한 코드를 실행할 때 시스템이 새 Docker 이미지를 가져올 수 있습니다.
    ```
    ./buildAndPush.sh <username>/blackboxdemo
    ```
    {: pre}

8. 이전 버전의 Docker 이미지를 사용하는 웜 컨테이너가 있는 경우, 새 호출은 해당 이미지를 계속 사용합니다. 새 호출이 새 이미지 사용을 시작하도록 액션을 업데이트하십시오.
    ```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. 선택사항: `--native` 인수를 `--docker openwhisk/dockerskeleton`에 대한 속기로서 사용할 수 있습니다. 이 인수를 사용하면 표준 Docker 액션 SDK 내에서 실행되는 실행 파일을 보다 손쉽게 작성하고 배치할 수 있습니다. 
    1. 위의 단계는 `/action/exec`에 있는 컨테이너 내의 2진 실행 파일을 작성합니다. `/action/exec` 파일을 로컬 파일 시스템으로 복사하고 이를 `exec.zip`으로 압축하십시오. 
    2. 초기화 데이터로서 실행 파일을 수신하는 Docker 액션을 작성하십시오. `--native` 인수는 `--docker openwhisk/dockerskeleton` 인수를 대체합니다.
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}

## Go 액션 작성
{: #creating-go-actions}

`--native` 인수를 사용하여 Go 실행 파일을 액션으로서 패키징할 수 있습니다. 

다음 요구사항과 제한사항을 참고하십시오. 
  * Go 실행 파일은 명령행에서 단일 인수를 수신합니다. 이 인수는 액션에 대한 인수를 표시하는 JSON 오브젝트의 문자열 직렬화입니다. 
  * 프로그램은 `stdout` 또는 `stderr`에 대해 로깅할 수 있습니다.
  * 관례상 출력의 마지막 행은 액션의 결과를 나타내는 문자열로 변환된 JSON 오브젝트여야 합니다. 

Go 액션을 작성하려면 다음을 수행하십시오. 

1. 이름이 `sample.go`인 파일에 다음 코드를 저장하십시오.
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
    {: codeblock}

2. {{site.data.keyword.openwhisk_short}}에 대해 `sample.go`를 교차 컴파일하십시오. 실행 파일의 이름은 `exec`여야 합니다.
    ```bash
    GOOS=linux GOARCH=amd64 go build -o exec
    zip exec.zip exec
    ibmcloud fn action create helloGo --native exec.zip
    ```
    {: codeblock}

3. 액션을 호출하십시오.
    ```bash
    ibmcloud fn action invoke helloGo -r -p name gopher
    {
        "msg": "Hello, gopher!"
    }
    ```
    {: pre}

## 액션 시퀀스 작성
{: #openwhisk_create_action_sequence}

일련의 액션을 연결하는 액션을 작성할 수 있습니다. 한 액션의 결과는 다음 액션에 대한 인수로 전달됩니다.
{: shortdesc}

여러 유틸리티 액션이 첫 번째 시퀀스 작성에 사용할 수 있는 `/whisk.system/utils` 패키지에서 제공됩니다. 

1. `/whisk.system/utils` 패키지의 액션을 나열하십시오.
    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    출력 예:
    ```
      package /whisk.system/utils: Building blocks that format and assemble data
   action /whisk.system/utils/head: Extract prefix of an array
   action /whisk.system/utils/split: Split a string into an array
   action /whisk.system/utils/sort: Sorts an array
   action /whisk.system/utils/echo: Returns the input
   action /whisk.system/utils/date: Current date and time
   action /whisk.system/utils/cat: Concatenates input into a string
    ```
    {: screen}

2. `split` 및 `sort` 액션을 사용하여 `split`의 결과가 `sort`에 대한 인수로 전달되도록 액션 시퀀스를 작성하십시오. 이 액션 시퀀스는 텍스트의 일부 행을 배열로 변환하고 해당 행을 정렬합니다.
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. 액션을 호출하십시오.
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    출력에서 행 나누기는 알파벳순으로 정렬됩니다.
    ```
    {
        "length": 3,
      "lines": [
            "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
    }
    ```
    {: screen}

**참고**:
* 기본 매개변수를 제외하면 시퀀스의 액션 간에 전달된 매개변수는 명시적입니다. 따라서 액션 시퀀스에 전달된 매개변수는 시퀀스의 첫 번째 액션에만 사용 가능합니다. 시퀀스에서 첫 번째 액션의 결과는 시퀀스에서 두 번째 액션에 대한 입력 JSON 오브젝트가 됩니다(계속 동일하게 반복됨). 첫 번째 액션이 결과에 이를 명시적으로 포함하지 않는 한, 이 오브젝트에는 시퀀스에 원래 전달된 매개변수가 포함되지 않습니다. 액션에 대한 입력 매개변수는 액션의 기본 매개변수와 병합되며, 전자가 우선순위를 갖고 일치하는 기본 매개변수를 대체합니다. 복수의 이름 지정된 매개변수로 액션 시퀀스 호출에 대한 자세한 정보는 [액션에 기본 매개변수 설정](./parameters.html#default-params-action)을 참조하십시오.
* 시퀀스에는 시퀀스 내의 각 액션의 제한시간과 별도인 전체 제한시간이 없습니다. 시퀀스가 오퍼레이션의 파이프라인이므로 한 액션의 장애는 파이프라인을 손상시킵니다. 하나의 액션의 제한시간이 초과되면 전체 시퀀스가 해당 장애로 종료됩니다. 

## 대형 액션 관리
{: #large-app-support}

액션에 대한 최대 코드 크기는 48MB입니다. 많은 써드파티 모듈, 기본 라이브러리 또는 외부 도구가 포함된 애플리케이션은 이 한계에 도달할 수 있습니다. 48MB를 초과하는.zip 또는 .jar 패키지 액션을 작성하는 경우, 사용자는 종속 항목의 런타임 이미지를 확장한 후에 48MB 미만의 소형 아카이브나 단일 소스 파일을 사용해야 합니다. 

예를 들어, 필수 공유 라이브러리가 포함된 사용자 정의 Docker 런타임을 빌드하면 종속 항목이 아카이브 파일에 존재할 필요가 없습니다. 개인용 소스 파일은 계속해서 아카이브에 번들링되고 런타임에 삽입될 수 있습니다.

### 액션 크기 줄이기
{: #large-app-reduce}

Python 앱의 코드 크기를 줄이려면 다음을 수행하십시오. 

1. `opencv-python` 라이브러리를 `opencv`에 배치하십시오. 
2. opencv 2진을 OS 이미지에 설치하십시오. 
3. `pip install requirements.txt`를 실행하여 추가 Python 라이브러리로 이미지를 늘리십시오. 
4. 새 이미지로 `action.py`를 사용하십시오. 

Node.js 앱의 코드 크기를 줄이려면 다음을 수행하십시오. 

1. `opencv`를 설치하십시오. 
   ```
npm install opencv
   ```
   {: pre}

2. `package.json`을 설치하십시오. 
   ```
npm install package.json
   ```
   {: pre}

3. 새 이미지로 `action.js`를 사용하십시오. 
