---

copyright:
  years: 2018
lastupdated: "2018-05-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 매개변수에 대한 작업

배치를 위해 액션 및 패키지에서 매개변수를 설정하는 방법 및 호출 중에 액션으로 매개변수를 전달하는 방법을 알아보십시오. 또한 명령행에서 개별적으로 각 매개변수를 제공하지 않고 파일을 사용하여 매개변수를 저장하고 파일 이름을 액션에 전달할 수 있습니다.
{: shortdesc}

서버리스 액션을 사용하면 액션에 매개변수를 추가하여 데이터가 제공되며, 이 액션은 main 서버리스 함수에 인수로 선언됩니다. 모든 데이터가 이러한 방식으로 제공되며 값은 서로 다른 몇 가지 방식으로 설정될 수 있습니다. 첫 번째 옵션은 액션 또는 패키지가 작성되거나 업데이트될 때 매개변수를 제공하는 것입니다. 이 옵션은 모든 실행에서 동일하게 유지되는 데이터(다른 플랫폼의 환경 변수에 해당) 또는 호출 시 대체할 수 있는 기본값에 대해 유용합니다. 두 번째 옵션은 이전에 설정된 매개변수를 대체하는 액션이 호출될 때 매개변수를 제공하는 것입니다.

## 호출 중에 액션에 매개변수 전달
{: #pass-params-action}

매개변수는 호출 시에 액션에 전달될 수 있습니다. 제공된 예제에서는 Javascript를 사용하지만 다른 모든 언어는 동일한 방식으로 작동합니다. 자세한 예제를 보려면 [Javascript 액션](./openwhisk_actions.html#creating-and-invoking-javascript-actions), [Swift 액션](./openwhisk_actions.html#creating-swift-actions), [Python 액션](./openwhisk_actions.html#creating-python-actions), [Java 액션](./openwhisk_actions.html#creating-java-actions), [PHP 액션](./openwhisk_actions.html#creating-php-actions), [Docker 액션](./openwhisk_actions.html#creating-docker-actions) 또는 [Go 액션](./openwhisk_actions.html#creating-go-actions)에서 다음 주제를 확인하십시오.

1. 액션에서 매개변수를 사용하십시오. 예를 들어, 다음 컨텐츠가 포함된 **hello.js** 파일을 작성하십시오.
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  입력 매개변수가 JSON 오브젝트 매개변수로서 **main** 함수에 전달됩니다. 이 예제에서 `name` 및 `place` 매개변수가 `params` 오브젝트에서 검색되는 방법에 유념하십시오.

2. 사용할 수 있도록 **hello** 액션을 업데이트하십시오.
  ```
  ibmcloud wsk action update hello hello.js
  ```
  {: pre}

  비서비스 신임 정보 매개변수를 수정하는 경우, 새 매개변수가 포함된 `action update` 명령을 실행하면 현재 존재하지만 `action update` 명령에 지정되지 않은 매개변수가 제거됩니다. 예를 들어, `action update -p key1 new-value -p key2 new-value`를 실행하지만 설정된 다른 매개변수를 생략하는 경우 액션이 업데이트되면 이 매개변수는 더 이상 존재하지 않습니다. 또한 액션에 바인드된 모든 서비스가 제거되므로 다른 매개변수를 업데이트한 후 다시 [액션에 서비스를 바인드](./binding_services.html)해야 합니다.
  {: tip}

3. 명령행을 사용하거나 원하는 매개변수가 포함된 [파일을 제공](./parameters.html#using-parameter-files)하여 매개변수를 명시적으로 제공할 수 있습니다.

  명령행을 통해 직접 매개변수를 전달하려면 `--param` 플래그에 키/값 쌍을 제공하십시오.
  ```
  ibmcloud wsk action invoke --result hello --param name Dorothy --param place Kansas
  ```
  {: pre}

  **응답:**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  `--result` 옵션의 사용에 유념하십시오. 이는 CLI가 활성화의 완료를 대기한 후에 결과만 표시하는 블로킹 호출을 의미합니다. 편의상 이 옵션은 자동으로 추론되는 `--blocking` 없이 사용될 수 있습니다.

  또한 명령행에 지정된 매개변수값이 유효한 JSON인 경우, 이는 구문 분석되어 구조화된 오브젝트로서 액션에 전송됩니다.

  예를 들어, **hello** 액션을 다음으로 업데이트하십시오.
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  이제 액션은 `name` 및 `place` 필드를 보유하도록 단일 `person` 매개변수를 예상합니다.

  그리고 다음 예제에서와 같이 유효한 JSON인 `person` 매개변수로 액션을 호출하십시오.
  ```
  ibmcloud wsk action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
  ```
  {: pre}

  **응답:**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  액션이 지금 예상하는 구조화된 오브젝트로 `person` 매개변수값을 CLI가 자동으로 구문 분석하므로 결과는 동일합니다.

## 액션에서 기본 매개변수 설정
{: #default-params-action}

액션은 복수의 이름 지정된 매개변수로 호출될 수 있습니다. 이전 예제의 **hello** 액션이 두 개의 매개변수(개인의 *이름* 및 개인의 출신 *지역*)를 예상한다는 점을 상기하십시오.

매번 모든 매개변수를 액션에 전달하는 대신에 특정 매개변수를 바인드할 수 있습니다. 다음 예제에서는 액션의 기본값이 "Kansas" 지역이 되도록 *place* 매개변수를 바인드합니다.

1. `--param` 옵션을 사용하여 매개변수값을 바인드하거나 매개변수가 포함된 파일을 `--param-file`에 전달하여 액션을 업데이트하십시오. (파일을 사용하는 예제는 [매개변수 파일 사용](./parameters.html#using-parameter-files)의 절을 참조하십시오.)

  명령행에서 명시적으로 기본 매개변수를 지정하려면 `param` 플래그에 키/값 쌍을 제공하십시오.
  ```
  ibmcloud wsk action update hello --param place Kansas
  ```
  {: pre}

2. 이번에는 `name` 매개변수만 전달하여 액션을 호출하십시오.
  ```
  ibmcloud wsk action invoke --result hello --param name Dorothy
  ```
  {: pre}

  출력 예:
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  액션을 호출할 때 "place" 매개변수를 지정할 필요가 없었다는 점에 유념하십시오. 호출 시에 매개변수값을 지정하여 바인드된 매개변수를 여전히 겹쳐쓸 수 있습니다.

3. `name` 및 `place` 값을 둘 다 전달하여 액션을 호출하고 출력을 확인하십시오.

  `--param` 플래그를 사용하여 액션을 호출하십시오.
  ```
  ibmcloud wsk action invoke --result hello --param name Dorothy --param place "Washington, DC"
  ```
  {: pre}

  출력 예:
  ```
  {  
      "payload": "Hello, Dorothy from Washington, DC"
  }
  ```
  {: screen}

  작성되거나 업데이트된 액션에 설정된 매개변수는 호출에 직접 제공된 매개변수로 항상 대체됩니다.
  {: tip}

## 패키지에서 기본 매개변수 설정
{: #default-params-package}

매개변수는 패키지 레벨에서 설정될 수 있으며 다음과 같은 _경우를 제외하고_ 액션에 대한 기본 매개변수의 역할을 합니다.

- 액션 자체에 기본 매개변수가 있습니다.
- 액션에 호출 시 제공된 매개변수가 있으며, 이 매개변수는 둘 이상의 매개변수가 사용 가능한 경우 항상 우위에 있습니다.

다음 예제에서는 **MyApp** 패키지에서 기본 매개변수 `name`을 설정하고 이를 사용하여 액션을 표시합니다.

1. 매개변수 세트로 패키지를 작성하십시오.

  ```
  ibmcloud wsk package update MyApp --param name World
  ```
  {: pre}

2. **MyApp** 패키지에서 액션을 작성하십시오.
  ```javascript
function main(params) {
         return {payload: "Hello, " + params.name};
     }
  ```
  {: codeblock}

  액션을 작성하십시오.
  ```
  ibmcloud wsk action update MyApp/hello hello.js
  ```
  {: pre}

3. 액션을 호출하고 사용 중인 기본 패키지 매개변수를 확인하십시오.
  ```
  ibmcloud wsk action invoke --result MyApp/hello
  ```
  {: pre}

  출력 예:
  ```
     {
         "payload": "Hello, World"
     }
  ```
  {: screen}

## 매개변수 파일 사용
{: #using-parameter-files}

JSON 형식의 파일에 매개변수를 저장한 다음, `--param-file` 플래그를 파일 이름에 제공하여 매개변수에 전달할 수 있습니다. 이 방법은 패키지 및 액션 작성(또는 업데이트)에 그리고 액션 호출 중에 사용할 수 있습니다.

1. 예를 들어, 다음 컨텐츠에서 `hello.js`를 사용하여 이전의 **hello** 예제를 고려하십시오.

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. `hello.js`의 업데이트된 컨텐츠로 액션을 업데이트하십시오.

  ```
  ibmcloud wsk action update hello hello.js
  ```
  {: pre}

3. JSON 형식 매개변수가 포함된 매개변수 파일 `parameters.json`을 작성하십시오.

  ```json
  {
      "name": "Dorothy",
      "place": "Kansas"
  }
  ```
  {: codeblock}

4. **hello** 액션 호출 시 `parameters.json` 파일 이름을 사용하고 출력을 확인하십시오.

  ```
  ibmcloud wsk action invoke --result hello --param-file parameters.json
  ```

  출력 예:
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}
