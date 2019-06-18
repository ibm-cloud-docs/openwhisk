---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

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
{:gif: data-image-type='gif'}


# 액션을 위한 앱 준비
{: #prep}

앱을 서버리스로 설정하거나 특별히 이벤트에 응답하도록 스크립트를 작성하는 경우 모두, 코드에서 액션을 작성하려면 코드에는 특정 요구사항이 충족되어야 합니다.
{: shortdesc}

각 프로그래밍 언어에는 실행하는 데 필요한 위한 특정 요구사항이 있으나 대부분 다음과 같은 일반 요구사항이 있습니다. 
- 기본적으로 코드에 대한 시작점의 예상 이름은 `main`입니다. 시작점이 `main`이 아닌 경우 액션 작성 시 사용자 정의 이름을 지정할 수 있으므로 해당 이름을 기록해 두십시오. 
- 앱에 대한 입력 매개변수 및 앱의 출력 결과는 엔티티 간에 전달될 수 있도록 특정 구조로 형식화되어야 합니다. 구조는 코드 언어에 따라 달라집니다. 예를 들어, Python 앱을 사용하면 사전으로서 매개변수는 앱에 대한 입력이어야 하며 사전으로서 앱의 결과는 구조화되어야 합니다. 콘솔이 JSON과 같이 구조화된 오브젝트의 매개변수를 액션에도 전달할 수 있으므로, 특정 필드에서 JSON 값(예: `name` 및 `place`)이 사용된 입력 매개변수를 예상하도록 코드를 구조화할 수 있습니다.

    JSON input example:
    ```json
    {"name": "Dorothy", "place": "Kansas"}
    ```
    {: codeblock}

    JavaScript example:
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
    ```
    {: codeblock}
- 앱에 여러 파일이 포함되어 있으면 액션에 사용되도록 하나의 파일로 결합되어야 합니다. 코드를 하나의 파일로 다시 작성하거나 파일 및 종속 항목을 하나의 아카이브 파일로 패키지할 수 있습니다. 런타임이 지원되지 않으면 Docker 이미지에서 앱을 패키지할 수 있습니다. 
- 종속 항목은 앱으로 패키지될 수 있습니다. 사용 가능한 런타임에는 몇 개의 사전 설치된 패키지 및 확장기능이 함께 제공됩니다. 앱의 종속성이 이미 런타임에 포함된 경우 [런타임에 대한 참조 정보를 검토](/docs/openwhisk?topic=cloud-functions-runtimes)하십시오. 종속성이 포함된 경우 앱으로 종속성을 패키지할 필요가 없습니다. 

    코드 컴파일은 필수가 아니지만 런타임에 사용 가능한 경우 코드를 미리 컴파일하면 성능을 향상시킬 수 있습니다.
    {: tip}

## Docker 이미지에서 앱 준비
{: #prep_docker}

{{site.data.keyword.openwhisk_short}}를 사용하면 어떠한 언어로도 앱을 작성하고 Docker 이미지로 앱을 패키지할 수 있습니다.
{: shortdesc}

Docker Hub에서 공개적으로 사용할 수 있는 이미지와 같이 공용 레지스트리에서만 이미지를 사용할 수 있습니다. 개인용 레지스트리는 지원되지 않습니다.
{: important}

### Docker 이미지에서 코드 준비
{: #prep_docker_pkg}

사용자 코드는 실행 가능 2진으로 컴파일되며 Docker 이미지에 임베드됩니다. 2진 프로그램은 `stdin`에서 입력을 가져오고, `stdout`을 통해 응답하여 시스템과 상호작용합니다.
{: shortdesc}

시작하기 전에 다음을 수행하십시오.
- Docker 허브 계정이 있어야 합니다. [Docker 허브 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://hub.docker.com)에서 무료 Docker ID 및 계정을 설정할 수 있습니다.
- [Docker를 설치](https://hub.docker.com/search?offering=community&type=edition)하십시오.
- [Docker 런타임에 대한 요구사항을 검토](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker)하십시오.

앱을 패키징하려면 다음을 수행하십시오.

코드를 Docker 이미지로 패키징하려면 다음을 수행하십시오. 
1. Docker 스켈레톤을 다운로드하고 설치하십시오. 스켈레톤은 사용자 정의 2진 양식으로 코드가 삽입될 수 있는 Docker 컨테이너 템플리트입니다.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. 블랙박스 스켈레톤에서 사용자 정의 2진을 설정하십시오. 스켈레톤에는 사용 가능한 C 프로그램이 포함되어 있습니다. `example.c` 파일의 일부가 Docker 이미지 빌드 프로세스의 일부로서 컴파일되므로, 시스템에서 컴파일된 C 프로그램은 필요하지 않습니다.
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

3. 선택사항: 실행 파일 빌드를 위해 `Dockerfile`을 수정하여 Docker 이미지에 추가로 코드 및 종속 항목을 추가하십시오. 다음 요구사항에 유의하십시오. 
  * 2진은 `/action/exec`의 컨테이너 내부에 위치해야 합니다.
  * 실행 파일은 명령행에서 단일 인수를 수신합니다. 이 인수는 액션에 대한 인수를 표시하는 JSON 오브젝트의 문자열 직렬화입니다.
  * 프로그램은 `stdout` 또는 `stderr`에 대해 로깅할 수 있습니다.
  * 관례상 출력의 마지막 행은 액션의 결과를 나타내는 문자열로 변환된 JSON 오브젝트여야 합니다.
  Dockerfiles 구성에 대한 자세한 정보는 [Dockerfile 참조](https://docs.docker.com/engine/reference/builder/)를 참조하십시오.

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




## JavaScript 앱 준비
{: #prep_js}

액션을 작성하기 전에 JavaScript 코드를 준비하십시오. 코드가 올바르게 구조화되었는지 확인한 후 패키지되어야 하는지 여부를 판별하십시오.
{: shortdesc}

### JavaScript 코드 구조화
{: #prep_js_struct}

- 시작점 함수의 예상 이름은 `main`입니다. 코드의 함수가 `main`이 아닌 경우 액션 작성 시 지정할 이름을 기록해 두십시오.
- 입력 매개변수는 JSON 오브젝트로 전달됩니다.
- 성공적인 활성화의 결과 또한 JSON 오브젝트이지만 액션 [동기](#prep_js_sync) 또는 [비동기](#prep_js_async)인지에 따라 다르게 리턴됩니다.



예:
```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```

여러 함수가 사용된 예:

  ```javascript
  function main() {
      return { payload: helper() }
  }

  function helper() {
      return new Date();
}
  ```
  {: codeblock}


### 동기 동작으로 JavaScript 코드 구조화
{: #prep_js_sync}

JavaScript 활성화는 기본 함수가 `return`문을 실행하지 않고 존재하거나 Promise 없이 값을 리턴하는 `return`문을 실행하여 존재하는 경우 동기입니다.
{: shortdesc}

동기 코드의 예:

```javascript
// each path results in a synchronous activation
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




### 비동기 동작으로 JavaScript 코드 구조화
{: #prep_js_async}

JavaScript 함수는 리턴 후에도 콜백 함수에서 계속해서 실행될 수 있습니다. main 함수가 Promise를 리턴하여 종료되는 경우 JavaScript 활성화는 비동기입니다. 이 경우 시스템은 Promise가 이행되거나 거부될 때까지 액션이 계속 실행 중이라고 가정합니다. 비동기로 실행되는 Javascript 함수는 액션에서 Promise를 리턴하여 `main` 함수의 리턴 후에 활성화 결과를 리턴할 수 있습니다.
{: shortdesc}

새 Promise 오브젝트를 인스턴스화하고 콜백 함수를 전달하여 시작하십시오. 콜백은 둘 다 함수인 두 개의 인수 resolve 및 reject를 취합니다. 모든 비동기 코드는 해당 콜백 내부로 이전됩니다. 오브젝트 수락과 오브젝트 리턴(또는 오브젝트의 `Promise`)의 통상적인 시그니처를 준수하는 한 액션 핸들러는 임의의 이름을 보유할 수 있습니다.

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

위의 예에서는 다음 세부사항이 실행됩니다. 
* `main` 함수가 Promise를 리턴합니다. Promise는 활성화가 아직 완료되지 않았지만 향후에 완료될 예정임을 표시합니다.
* `setTimeout()` JavaScript 함수는 Promise의 콜백 함수를 호출하기 전에 2초 동안 대기하며, 이는 비동기 코드를 표시합니다.
* Promise의 콜백은 둘 다 함수인 `resolve` 및 `reject` 인수를 허용합니다.
  * `resolve()`에 대한 호출은 Promise를 이행하며 활성화가 정상적으로 완료되었음을 표시합니다.
  * `reject()`에 대한 호출을 사용하면 Promise를 거부하고 활성화가 비정상적으로 완료되었음을 알릴 수 있습니다.


### 동기 및 비동기 동작으로 JavaScript 코드 구조화
{: #prep_js_both}

다음 예제에 표시된 대로 일부 입력에서는 액션이 동기이고 나머지 입력에서는 비동기일 수 있습니다.
{: shortdesc}

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





### 예: JavaScript를 사용한 외부 API 호출
{: #prep_js_api}

다음 예제에서는 매일 우주의 고유 이미지를 제공하는 NASA APOD(Astronomy Picture of the Day) 서비스의 외부 API를 호출합니다.
{: shortdesc}


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

NASA APOD API에 대한 호출이 실행되며 JSON 결과에서 필드가 추출됩니다.

그런 다음 [액션을 작성](/docs/openwhisk?topic=cloud-functions-actions)하고 [액션을 호출](/docs/openwhisk?topic=cloud-functions-test)하여 테스트합니다. 다음의 예제 오브젝트가 리턴됩니다.

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






### webpack 모듈로 JavaScript 코드 패키지
{: #prep_js_pkg}

[webpack ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://webpack.js.org/concepts/) 등의 Javascript 모듈 번들러를 사용하여 앱을 패키지할 수 있습니다. `webpack`에서 코드를 처리할 때 이는 액션에서 요구하는 모든 모듈이 포함된 종속 항목 그래프를 반복적으로 빌드합니다.
{: shortdesc}

시작하기 전에 앱의 종속성이 이미 런타임에 포함되어 있는지 확인하려면 [JavaScript 런타임에 포함된 패키지를 검토](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments)하십시오. 종속성이 포함되지 않은 경우 앱으로 종속성을 패키지해야 합니다.

1. `package.json` 파일을 작성하십시오. 개발 종속 항목으로서 `webpack`을 추가하십시오. 

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

3. 앱 코드를 준비하십시오. 이 예제에서 `index.js`라고 하는 변수 `global.main`은 앱의 기본 함수로 설정됩니다.

    예:
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



### NPM 파일로 JavaScript 코드 패키지
{: #prep_js_npm}

단일 JavaScript 소스 파일에 모든 액션 코드를 작성하는 대안으로 코드를 .zip 파일에서 `npm` 패키지로서 작성할 수 있습니다.
{: shortdesc}

시작하기 전에 앱의 종속성이 이미 런타임에 포함되어 있는지 확인하려면 [JavaScript 런타임에 포함된 패키지를 검토](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments)하십시오. 종속성이 포함되지 않은 경우 앱으로 종속성을 패키지해야 합니다.

1. 루트 디렉토리에서 `package.json` 파일을 작성하십시오. 예를 들면, 다음과 같습니다.

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

2. 모든 종속 항목을 로컬로 설치하십시오.

    ```
    npm install DEPENDENCY
    ```
    {: pre}

    **참고**: 대부분의 `npm` 패키지가 `npm install`에 JavaScript 소스를 설치하지만 일부는 2진 아티팩트의 설치와 컴파일도 수행합니다. 아카이브 파일 업로드는 JavaScript 종속 항목만 지원합니다. 아카이브에 2진 종속 항목이 포함된 경우에는 액션 호출이 성공하지 못할 수 있습니다. 

3. 모든 종속 항목을 포함하여 모든 파일이 포함된 `.zip` 아카이브를 작성하십시오.

    ```
  zip -r action.zip *
    ```
    {: pre}

    Windows 사용자: Windows Explorer 액션을 .zip 파일의 작성에 사용하면 결과적으로 잘못된 파일 구조가 생성됩니다. {{site.data.keyword.openwhisk_short}} .zip 액션에는 아카이브의 루트에 `package.json`이 있어야 하지만, Windows Explorer는 이를 중첩된 폴더 내에 둡니다. 대신 `zip` 명령을 사용하십시오.
    {: tip}





## Go 앱 준비
{: #prep_go}

빠른 테스트 또는 개발 목적인 경우 단일 파일을 사용하십시오. 프로덕션 앱의 경우 벤더 라이브러리를 포함하여 보다 나은 성능 또는 다중 소스 파일 지원을 위해 Go 액션을 실행 파일에 미리 컴파일하십시오.
{: shortdesc}

`GOOS=Linux` 및 `GOARCH=amd64`로 교차 컴파일하여 Go 플랫폼에서 2진을 작성할 수 있더라도 런타임 컨테이너 이미지에 포함된 사전 컴파일 기능을 사용하십시오. [다중 소스 파일](#prep_go_multi) 또는 [벤더 라이브러리](#prep_go_vendor)를 패키징할 수 있습니다.
{: tip}


### Go 코드 구조화
{: #prep_go_struct}

- 시작점 패키지의 예상 이름은 `main`입니다. 코드의 패키지가 `main`이 아닌 경우 액션 작성 시 지정할 이름을 기록해 두십시오.
- 패키지는 공용이어야 합니다.

예:
```go
package main

    import "fmt"

    // Main is the function implementing the action
    func Main(params map[string]interface{}) map[string]interface{} {
        // parse the input JSON
        name, ok := params["name"].(string)
        if !ok {
            name = "World"
        }
        msg := make(map[string]interface{})
        msg["body"] = "Hello " + name + "!"
        // can optionally log to stdout (or stderr)
        fmt.Println("hello Go action")
        // return the output JSON
        return msg
    }
    ```
    {: codeblock}

### 여러 Go 소스 파일 패키지
{: #prep_go_multi}

1. 최상위 레벨 `src` 디렉토리를 작성하십시오. 최상위 `src` 디렉토리를 사용하고 기본 패키지에 속한 소스 파일을 `src`의 루트 또는 `main` 디렉토리 내부에 배치하고 다른 패키지용 하위 디렉토리를 작성하십시오. 예를 들어 `hello` 패키지는 `src/hello` 디렉토리가 됩니다.
  ```
  go-action-hello/
  └── src
      ├── hello
      │   └── hello.go
      └── main
          └── main.go
  ```
  {: screen}

2. 하위 패키지를 가져오십시오. hello 하위 패키지를 가져오는 `main/main.go`의 예는 다음과 같습니다.

  ```go
  package main

  import (
  	"fmt"
  	"hello"
  )

  // Main forwading to Hello
  func Main(args map[string]interface{}) map[string]interface{} {
  	fmt.Println("This is main.Main")
  	greetings := "World"
  	name, ok := args["name"].(string)
  	if ok {
  		greetings = name
  	}
  	return hello.Hello(greetings)
  }
  ```
  {: codeblock}

  `hello/hello.go`의 예:

  ```go
  package hello

  import "fmt"

  // Hello return a greeting message
  func Hello(name string) map[string]interface{} {
  	fmt.Println("This is hello.Hello")
  	res := make(map[string]interface{})
  	res["body"] = "Hello " + name
  	return res
  }
  ```
  {: codeblock}

3. 코드를 컴파일하십시오. `src` 디렉토리의 .zip 아카이브를 작성하십시오. 최상위 프로젝트 디렉토리 `go-action-project/`는 포함하지 **마십시오**.

  ```bash
  cd src
  zip -r ../hello-src.zip *
  cd ..
  ```
  {: pre}

  `GOPATH`를 `src` 디렉토리의 상위로 설정하여 로컬로 컴파일할 수 있습니다. VSCode를 사용하는 경우, `go.inferGopath` 설정을 `true`로 변경해야 합니다.
  {: note}

4. .zip 아카이브의 루트에서 `exec`로 Go 실행 파일을 컴파일 및 패키징하십시오. 다음 명령을 사용하여 `hello-bin.zip` 아카이브를 빌드하십시오. 사용자 워크스테이션에 Docker CLI가 설치되어 있고 `PATH`에 `docker`가 있어야 합니다. 

  ```bash
  docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
  ```
  {: pre}

  이 예에서는 기본 함수가 `-compile main`입니다. 다른 함수를 기본으로 사용하려면 `-compile`의 값을 변경하십시오. 기본 함수는 컴파일 시에 선택됩니다. 사전 컴파일하는 경우, `ibmcloud fn action [update | create]`는 `--main`을 무시합니다.

  컨테이너는 `stdin`에서 소스 .zip의 컨텐츠를 얻고, 컨텐츠를 컴파일하고 루트에서 `exec` 실행 파일로 새 .zip 아카이브를 작성합니다. .zip 아카이브 컨텐츠는 `stdout`으로 스트림되며, 이렇게 되면 Go 액션으로 배치되도록 `hello-bin.zip` 아카이브로 경로 재지정됩니다.




### 벤더 라이브러리로 Go 코드 패키지
{: #prep_go_vendor}

Go 파일을 컴파일할 때 `vendor` 디렉토리를 소스 `zip` 아카이브에 채움으로써 종속 항목을 포함시킬 수 있습니다. `vendor` 디렉토리는 최상위 레벨에 두지 못합니다. `vendor` 디렉토리는 `src/` 내에 패키지 디렉토리 내부에 배치해야 합니다.
{: shortdesc}

`hello.go` 앱에 있는 로그 패키지 `logrus`의 예:

```go
package hello

import (
	"os"

	"github.com/Sirupsen/logrus"
)

var log = logrus.New()

func init() {
	log.Out = os.Stdout
}

// Hello return a greeting message
func Hello(name string) map[string]interface{} {
	log.WithFields(logrus.Fields{"greetings": name}).Info("Hello")
	res := make(map[string]interface{})
	res["body"] = "Hello, " + name
	return res
}
```
{: codeblock}

</br>
이 예에서는 `vendor` 디렉토리가 `src/hello/vendor`에 위치합니다. `hello` 패키지에 사용되는 서드파티 라이브러리를 추가할 수 있습니다. [dep ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://golang.github.io/dep/docs/installation.html) 등의 다중 도구를 사용하여 종속 항목을 채우고 관리할 수 있습니다.

라이브러리의 버전 및 위치를 설명하는 `src/main/Gopkg.toml` 파일을 작성하여 `dep`를 사용할 수 있습니다.

```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

`vendor` 디렉토리를 채우고 `dep ensure`를 실행하십시오.







## Swift 앱 준비
{: #prep_swift}

Swift 파일은 액션이 실행되기 전에 2진으로 컴파일되어야 합니다. 이 지연을 콜드 스타트 지연이라고 합니다. 콜드 스타트 지연을 피하기 위해 Swift 파일을 2진으로 컴파일한 후에 .zip 파일로 {{site.data.keyword.openwhisk_short}}에 2진을 업로드할 수 있습니다. Docker 런타임에는 사용자가 Swift 4.2 액션을 컴파일 및 패키징하는 데 도움이 되는 컴파일러가 포함되어 있습니다. 액션을 보유한 컨테이너가 제거될 때까지 액션에 대한 후속 호출이 훨씬 더 빠릅니다. 

Swift 액션은 Linux 환경에서 실행됩니다. Swift on Linux는 아직 개발 중이며 {{site.data.keyword.openwhisk_short}}에서는 최신 사용 가능 릴리스를 사용합니다. 이 릴리스는 안정적이지 않을 수 있습니다. {{site.data.keyword.openwhisk_short}}에서 사용되는 Swift의 버전이 MacOS에서 안정적인 XCode 릴리스의 Swift 버전과 일치하지 않을 수 있습니다.
{: important}



### Swift 코드 구조화
{: #prep_swift_struc}

시작점 함수의 예상 이름은 `main`입니다. 코드의 함수가 `main`이 아닌 경우 액션 작성 시 지정할 이름을 기록해 두십시오.

기본 기능 시그니처에 추가하여, Swift 4에서는 [Codable ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.apple.com/documentation/swift/codable) 유형을 활용하는 2개의 추가 시그니처를 제공합니다. [JSON ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 등의 외부 표현과의 호환성을 위해 인코딩 및 디코딩 가능한](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types) 데이터 유형에 대해 자세히 알아볼 수 있습니다.

예:
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


#### Swift에서의 오류 처리
{: #prep_swift_error}

Codable 완료 핸들러를 사용하면 액션의 실패를 표시하는 오류를 전달할 수 있습니다. [Swift의 오류 처리 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)는 `try`, `catch` 및 `throw` 키워드의 사용으로 기타 언어의 예외 처리와 유사합니다.
{: shortdesc}

다음 스니펫은 오류 처리의 예를 표시합니다.

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


### Swift 4.2 파일을 2진으로 패키징
{: #prep_swift42_single}

외부 라이브러리에 의존하지 않는 단일 소스 파일을 컴파일하십시오. 기본 메소드의 이름으로 `-compile` 플래그를 사용하십시오.

시작하기 전에 다음을 수행하십시오.
- [Docker를 설치](https://hub.docker.com/search?offering=community&type=edition)하십시오.
- 앱의 종속성이 이미 런타임에 포함되어 있는지 확인하려면 [Swift 런타임에 포함된 패키지를 검토](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions)하십시오. 종속성이 포함되지 않은 경우 앱으로 종속성을 패키지해야 합니다.

앱을 패키징하려면 다음을 수행하십시오.

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

Docker 컨테이너는 `stdin`에서 파일의 컨텐츠를 읽고, 컴파일된 swift 실행 파일로 .zip 아카이브를 `stdout`에 기록합니다.



### Swift 4.2 다중 파일 프로젝트 및 종속 항목 패키지
{: #prep_swift42_multi}

시작하기 전에 다음을 수행하십시오.
- [Docker를 설치](https://hub.docker.com/search?offering=community&type=edition)하십시오.
- 앱의 종속성이 이미 런타임에 포함되어 있는지 확인하려면 [Swift 런타임에 포함된 패키지를 검토](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions)하십시오. 종속성이 포함되지 않은 경우 앱으로 종속성을 패키지해야 합니다.

앱을 패키징하려면 다음을 수행하십시오.

1. 다중 파일을 컴파일하고 외부 종속 항목을 포함시키려면 다음 디렉토리 구조를 작성하십시오. 

  ```
  .
  ├── Package.swift
  └── Sources
      └── main.swift
  ```
  {: codeblock}

  `Sources/` 디렉토리에 이름이 `main.swift`인 파일이 포함됩니다.

  `Package.swift`는 Swift 도구용으로 버전 `4.2`를 지정하는 주석으로 시작해야 합니다.

  ```swift
  // swift-tools-version:4.2
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
      .package(url: "https://github.com/IBM-Swift/SwiftyRequest.git", .upToNextMajor(from: "1.0.0"))
      ],
      targets: [
      .target(
          name: "Action",
          dependencies: ["SwiftyRequest"],
          path: "."
      )
      ]
  )
  ```
  {: codeblock}

2. 디렉토리의 컨텐츠를 사용하여 .zip 아카이브를 작성하십시오. 

  ```bash
  zip ../action-src.zip -r *
  ```
  {: codeblock}

3. `stdin`을 통해 .zip 아카이브를 Docker 컨테이너에 전달하십시오. `stdout`는 컴파일된 실행 파일이 있는 새 .zip 아카이브입니다. Docker 컨테이너는 .zip 아카이브의 컨텐츠를 `stdin`에서 읽고 새 .zip 아카이브를 컴파일된 Swift 실행 파일로 `stdout`에 기록합니다.

  ```
  docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
  ```
  {: codeblock}

  Linux 기반 시스템에서는 `zip` 및 `docker run` 단계를 단일 명령으로 결합할 수 있습니다. 

  ```
  zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
  ```
  {: codeblock}





## Python 앱 패키징
{: #prep_python}


### Python 코드 구조화
{: #prep_python_struct}

- Python 앱에서는 사전을 이용하고 사전을 생성해야 합니다. 
- 시작점 메소드의 예상 이름은 `main`입니다. 코드의 함수가 `main`이 아닌 경우 액션 작성 시 지정할 이름을 기록해 두십시오.
{: shortdesc}

예:
```python
def main(args):
	name = args.get("name", "stranger")
	greeting = "Hello " + name + "!"
	print(greeting)
	return {"greeting": greeting}
```

### Python 코드 패키징
{: #prep_python_pkg}

.zip 파일에서 Python 코드 및 종속 모듈을 패키징하십시오. 이 예에서 시작점이 포함된 소스 파일은 `__main__.py`이고 헬퍼 모듈은 `helper.py` 파일에 있습니다. 

시작하기 전에 앱의 종속성이 이미 런타임에 포함되어 있는지 확인하려면 [Python 런타임에 포함된 패키지를 검토](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments)하십시오. 종속성이 포함되지 않은 경우 앱으로 종속성을 패키지해야 합니다.

앱을 패키징하려면 다음을 수행하십시오.

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}


### zip 파일에 가상 환경의 Python 코드 패키징
{: #prep_python_virtenv}

가상 환경 `virtualenv`를 사용하여 Python 종속 항목을 패키징할 수 있습니다. 가상 환경은 [`pip` ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://packaging.python.org/installing/)를 사용하여 설치될 수 있는 추가 패키지를 링크할 수 있도록 허용합니다.

시작하기 전에 앱의 종속성이 이미 런타임에 포함되어 있는지 확인하려면 [Python 런타임에 포함된 패키지를 검토](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments)하십시오. 종속성이 포함되지 않은 경우 앱으로 종속성을 패키지해야 합니다.

앱을 패키징하려면 다음을 수행하십시오.

1. 설치할 `pip` 모듈과 버전이 포함된 [requirements.txt ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) 파일을 작성하십시오.

  `virtualenv`를 최소 크기로 유지하려면 선택된 런타임 환경의 일부가 아닌 모듈만 `requirements.txt`에 추가하십시오. Python 런타임에 포함된 패키지에 대한 자세한 정보는 Python [런타임 참조](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments)를 참조하십시오.
  {: tip}

2. 사용자 런타임에 적합한 다음 이미지 중 하나를 가져오십시오. 런타임 컨테이너와의 호환성을 보장하려면 가상 환경 내의 패키지된 설치에서 해당 유형에 대응되는 이미지를 사용해야 합니다.
    * `python:3.7`의 경우, Docker 이미지 `ibmfunctions/action-python-v3.7`을 사용하십시오.
    * `python:3.6`의 경우, Docker 이미지 `ibmfunctions/action-python-v3.6`을 사용하십시오.
    * `python:2`의 경우, Docker 이미지 `openwhisk/python2action`을 사용하십시오.

   예:
   ```
   docker pull ibmfunctions/action-python-v3.7
   ```
   {: pre}

2. 종속 항목을 설치하고 가상 환경을 작성하십시오. 가상 환경 디렉토리의 이름은 `virtualenv`여야 합니다.

   ```
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. `virtualenv` 디렉토리 및 추가 Python 파일을 패키징하십시오. 시작점이 포함된 소스 파일의 이름은 `__main__.py`여야 합니다.

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}





## Ruby 앱 준비
{: #prep_ruby}

액션을 작성하기 전에 Ruby 코드를 준비하십시오. 

### Ruby 코드 구조화
{: #prep_ruby_struct}

* Ruby 액션은 항상 Hash(디렉토리 같은 콜렉션)를 이용하고 Hash를 리턴합니다.
* 시작점 함수의 예상 이름은 `main`입니다. 코드의 함수가 `main`이 아닌 경우 액션 작성 시 지정할 이름을 기록해 두십시오.


예:
```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
    ```
    {: codeblock}

### Ruby 코드 패키징
{: #prep_ruby_pkg}

Ruby 앱 및 종속 패키지를 .zip 파일에 패키징할 수 있습니다. 예를 들어 `helper.rb`라는 두 번째 파일로 액션을 패키징할 수 있습니다.

소스 파일이 포함된 아카이브를 작성하십시오. 시작점이 포함된 소스 파일의 이름은 `main.rb`여야 합니다.

```bash
    zip -r helloRuby.zip main.rb helper.rb
```
{: pre}

기본 및 번들링된 gems 외에도 gems `mechanize` 및 `jwt`를 사용 가능합니다. 압축된 액션을 사용하여 모든 종속 항목을 패키징할 때마다 임의의 gems를 사용할 수 있습니다.



## PHP 앱 준비
{: #prep_php}

액션을 작성하기 전에 PHP 코드를 준비하십시오. 

### PHP 코드 구조화
{: #prep_php_struct}

- PHP 액션은 항상 연관 배열을 이용하고 연관 배열을 리턴합니다.
- 시작점 함수의 예상 이름은 `main`입니다. 코드의 함수가 `main`이 아닌 경우 액션 작성 시 지정할 이름을 기록해 두십시오.

예:
```php
<?php
function main(array $args) : array
{
    $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
?>
```
{: codeblock}

### PHP 코드 패키징
{: #prep_php_pkg}

PHP 파일 또는 종속 패키지를 .zip 파일에 패키징할 수 있습니다. 

시작하기 전에 앱의 종속성이 이미 런타임에 포함되어 있는지 확인하려면 [PHP 런타임에 포함된 패키지를 검토](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php)하십시오. 종속성이 포함되지 않은 경우 앱으로 종속성을 패키지해야 합니다.

앱을 패키징하려면 다음을 수행하십시오.

```bash
zip -r ARCHIVE_NAME.zip FILE_1.php FILE_2.php
```
{: pre}

예:
```bash
    zip -r helloPHP.zip index.php helper.php
```
{: pre}







## Java 앱 준비
{: #prep_java}

액션을 작성하기 전에 Java 코드를 준비하십시오. 

### Java 코드 구조화
{: #prep_java_struct}

Java 액션은 `main`이라고 하는 메소드가 포함된 Java 프로그램입니다. `main`에는 다음 서명이 있어야 합니다.


```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}


* `--main`을 사용하여 main 클래스의 이름을 지정해야 합니다. 자격을 갖춘 main 클래스는 정적 `main` 메소드를 구현하는 클래스입니다. 클래스가 기본 패키지에 없는 경우에는 완전한 Java 클래스 이름(예: `--main com.example.MyMain`)을 사용하십시오.
* Java 액션의 메소드 이름을 사용자 정의할 수 있습니다. 이는 액션의 완전한 메소드 이름(예: `--main com.example.MyMain#methodName`)을 지정하여 수행됩니다.
* 소스 파일 확장자를 사용하여 액션의 유형이 판별됩니다.

예:
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


### Java 코드 패키징
{: #prep_java_pkg}

Java 파일을 컴파일, 테스트하고 아카이브하려면 [JDK 8 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://openjdk.java.net/install)이 로컬로 설치되어 있어야 합니다.

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

2. `Hello.java` 파일을 클래스 파일로 컴파일하십시오. 

    ```
javac Hello.java
    ```
    {: pre}

2. 클래스 파일을 이름이 `hello.jar`인 JAR 파일로 압축하십시오. **참고**: [google-gson ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/google/gson)이 Java CLASSPATH에 존재해야 합니다.
3.
    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}


### Gradle로 Java 코드 패키징
{: #prep_java_gradle}

[Gradle](https://gradle.org) 같은 빌드 도구를 사용하여 Maven Central 같은 저장소에서 라이브러리를 페치하고 코드 및 모든 종속 항목이 포함된 최종 JAR 아카이브를 빌드할 수 있습니다.

다음은 Gradle을 사용하여 QR 코드 이미지 생성 기능을 제공하는 `com.google.zxing` 라이브러리를 활용하는 Java 액션을 빌드하는 예입니다.

1. 이름이 `build.gradle`인 파일을 작성하고 종속 항목을 지정하십시오. 

  ```gradle
  apply plugin: 'java'

  version = '1.0'

  repositories {
     mavenCentral()
  }

  configurations {
      provided
      compile.extendsFrom provided
  }

  dependencies {
       provided 'com.google.code.gson:gson:2.6.2'
      compile 'com.google.zxing:core:3.3.0'
       compile 'com.google.zxing:javase:3.3.0'
  }

  jar {
      dependsOn configurations.runtime

     from {
          (configurations.runtime - configurations.provided).collect {
              it.isDirectory() ? it : zipTree(it)
          }
      }
  }
  ```
  {: codeblock}

2. `build/libs/` 디렉토리에 JAR 아카이브를 생성하는 `gradle jar` 명령을 실행하십시오.

  자세한 정보는 Gradle 문서 [종속 항목 선언](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies)을 읽어보십시오.





## .NET Core 앱 준비
{: #prep_dotnet}

액션을 작성하기 전에 .NET Core 코드를 준비하십시오. 

### .NET Core 코드 구조화
{: #prep_dotnet_struct}

.NET Core 액션은 이름이 `Main`으로 지정될 것으로 예상되는 메소드가 사용되는 .NET Core 클래스 라이브러리입니다. 코드의 메소드가 `Main`이 아닌 경우 액션이 `--main {Assembly}::{Class Full Name}::{Method}` 형식으로 작성될 때 지정할 이름을 기록해 두십시오.

예:
```
Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main
```

### .NET Core 코드 패키징
{: #prep_dotnet_pkg}

시작하기 전에:
.NET Core 프로젝트를 컴파일, 테스트 및 아카이브하려면 다음을 수행해야 합니다. 
- [.NET Core SDK](https://dotnet.microsoft.com/download)를 로컬로 설치하십시오. 
- `dotnet` 실행 파일을 찾을 수 있는 위치로 `DOTNET_HOME` 환경 변수를 설정하십시오. 



코드를 패키징하려면 다음을 수행하십시오.

  1. `Apache.OpenWhisk.Example.Dotnet`이라는 C# 프로젝트를 작성하십시오.

      ```bash
      dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
      ```
      {: pre}

  2. `Apache.OpenWhisk.Example.Dotnet` 디렉토리로 이동하십시오. 

      ```bash
      cd Apache.OpenWhisk.Example.Dotnet
      ```
      {: pre}

  3. [Newtonsoft.Json](https://www.newtonsoft.com/json) NuGet 패키지를 다음과 같이 설치하십시오.

      ```bash
    dotnet add package Newtonsoft.Json -v 12.0.1
      ```
      {: pre}

  4. 이름이 `Hello.cs`인 파일에 다음 코드를 저장하십시오.

      ```csharp
    using System;
    using Newtonsoft.Json.Linq;

      namespace Apache.OpenWhisk.Example.Dotnet
    {
          public class Hello
        {
              public JObject Main(JObject args)
            {
                  string name = "stranger";
                if (args.ContainsKey("name")) {
                      name = args["name"].ToString();
                }
                JObject message = new JObject();
                message.Add("greeting", new JValue($"Hello, {name}!"));
                return (message);
            }
          }
      }
      ```
      {: codeblock}

  5. `Hello.cs` 및 기타 파일과 출력을 `out` 디렉토리로 컴파일하십시오.

      ```bash
    dotnet publish -c Release -o out
      ```
      {: pre}

  6. out 디렉토리로 이동하십시오.

      ```bash
      cd out
      ```
      {: pre}

  7. 게시된 파일을 압축하십시오.

      ```bash
      zip -r -0 ../helloDotNet.zip *
      ```
      {: pre}


