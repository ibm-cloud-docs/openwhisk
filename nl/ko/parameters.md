---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: parameters, passing, invocation, binding

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 매개변수에 대한 작업

서버가 없는 액션에서는 데이터가 액션에 매개변수를 추가하여 제공됩니다. 매개변수는 서버가 없는 기본 함수에 대한 인수로 선언됩니다.
{: shortdesc}

다음 두 가지 방법으로 매개변수의 값을 제공할 수 있습니다.
* **호출 동안 액션에 매개변수 전달**:액션이 CLI 플래그 또는 파일을 통해 호출될 때 매개변수를 제공하십시오. 호출에 제공된 매개변수는 이전에 설정된 모든 기본 매개변수를 대체합니다. 
* **액션 또는 패키지에 매개변수 바인딩**: 액션 또는 패키지가 작성 또는 업데이트될 때 기본 매개변수를 설정하십시오. 이 옵션은 모든 실행에서 동일하게 유지되는 데이터(다른 플랫폼의 환경 변수에 해당) 또는 호출 시 대체할 수 있는 기본값에 대해 유용합니다.

## 호출 중에 액션에 매개변수 전달
{: #pass-params-action}

매개변수는 호출 시에 액션에 전달될 수 있습니다.

1. `hello.js` 파일에 다음 코드를 저장하십시오.

    ```javascript
function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: codeblock}

2. `hello` 액션을 작성하십시오.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. 이 액션을 이전에 사용한 경우, 이를 업데이트하여 액션이 이전에 설정된 매개변수를 지웠는지 확인하십시오.
    ```
  ibmcloud fn action update hello hello.js
    ```
    {: pre}

4. `name` 및 `place` 매개변수를 전달하여 액션을 호출하십시오.
    ```
  ibmcloud fn action invoke --result hello --param name Dorothy --param place Kansas
    ```
    {: pre}

    **참고**: JSON 형식의 매개변수의 파일을 대신 전달할 수 있습니다.
    ```
      ibmcloud fn action invoke --result hello --param-file parameters.json
    ```
    {: pre}

    출력 예:
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
    ```
    {: screen}

5. 또한 구조화된 오브젝트의 매개변수를 액션에 전달할 수도 있습니다. 예를 들어, `hello` 액션을 다음으로 업데이트하십시오.
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
    ```
    {: codeblock}

    이제 액션은 `name` 및 `place` 필드를 보유하도록 단일 `person` 매개변수를 예상합니다.

6. 유효한 JSON 오브젝트인 단일 `person` 매개변수로 액션을 호출하십시오.
    ```
  ibmcloud fn action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
    ```
    {: pre}

    출력 예:
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
    ```
    {: screen}

## 액션에 매개변수 바인딩
{: #default-params-action}

다중으로 이름 지정된 매개변수를 사용하여 액션을 호출할 수 있습니다. 예를 들어 기본 `hello` 액션은 두 개의 매개변수(개인의 `name`, 개인의 출신 `place`)를 예상합니다. 

```javascript
function main(params) {
   return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
```
{: screen}

매번 액션에 모든 매개변수를 전달하는 대신 기본 매개변수를 액션에 바인딩할 수 있습니다. 다음 단계에서는 액션이 "Kansas" 위치에 기본값을 지정하도록 `place` 매개변수를 기본 `hello` 액션에 바인딩하는 방법을 보여줍니다. 

1. `hello.js` 파일에 다음 코드를 저장하십시오.

    ```javascript
function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: codeblock}

2. `hello` 액션을 작성하십시오.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. `--param` 플래그 및 키/값 쌍을 사용하여 매개변수 값을 바인딩하도록 액션을 업데이트하십시오. 

    ```
  ibmcloud fn action update hello --param place Kansas
    ```
    {: pre}

    **참고**: JSON 형식의 매개변수의 파일을 대신 전달할 수 있습니다.
    ```
    ibmcloud fn action update hello --param-file parameters.json
    ```
    {: pre}

    비서비스 인증 정보 매개변수를 수정하는 경우, 새 매개변수가 포함된 `action update` 명령을 실행하면 현재 존재하지만 `action update` 명령에 지정되지 않은 매개변수가 제거됩니다. 예를 들어, `action update -p key1 new-value -p key2 new-value`를 실행하지만 설정된 다른 매개변수를 생략하는 경우 액션이 업데이트되면 이 매개변수는 더 이상 존재하지 않습니다. 또한 액션에 바인딩된 모든 서비스가 제거되므로 다른 매개변수를 업데이트한 후 다시 [액션에 서비스를 바인딩](/docs/openwhisk?topic=cloud-functions-binding_services)해야 합니다.
    {: tip}

4. `name` 매개변수만 전달하여 액션을 호출하십시오.
    ```
  ibmcloud fn action invoke --result hello --param name Dorothy
    ```
    {: pre}

    출력 예:
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
    ```
    {: screen}

    액션을 호출할 때 `place` 매개변수를 지정하지 않았으므로 바인딩된 기본 매개변수의 값 `Kansas`가 사용됩니다. 

5. 호출 시에 매개변수값을 지정하여 바인드된 매개변수를 겹쳐쓸 수 있습니다. `name` 및 `place`를 둘 다 전달하여 액션을 호출하십시오.
    ```
  ibmcloud fn action invoke --result hello --param name Dorothy --param place "Washington, DC"
    ```
    {: pre}

    출력 예:
    ```
    {  
        "payload": "Hello, Dorothy from Washington, DC"
    }
    ```
    {: screen}

## 패키지에 매개변수 바인딩
{: #default-params-package}

기본 매개변수는 또한 패키지 레벨에서도 설정할 수 있습니다. 바인딩된 매개변수는 다음의 경우를 제외하고 패키지에서 액션에 대한 기본 매개변수로 제공됩니다. 

- 액션 자체에 기본 매개변수가 있는 경우
- 액션에 호출 시 제공되는 매개변수가 있는 경우

다음 예제에서는 `MyApp` 패키지에서 기본 매개변수 `name`을 설정하고 이를 사용하여 액션을 표시합니다.

1. 기본 매개변수 `name`을 이에 바인딩하여 패키지를 작성하십시오.
```
  ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

2. 이름이 `helloworld.js`인 파일에 다음 코드를 저장하십시오.

    ```javascript
function main(params) {
           return {payload: "Hello, " + params.name};
     }
    ```
    {: codeblock}

3. `MyApp` 패키지에 액션을 작성하십시오.
    ```
    ibmcloud fn action update MyApp/hello helloworld.js
    ```
    {: pre}

    비서비스 인증 정보 매개변수를 수정하는 경우, 새 매개변수가 포함된 `action update` 명령을 실행하면 현재 존재하지만 `action update` 명령에 지정되지 않은 매개변수가 제거됩니다. 예를 들어, `action update -p key1 new-value -p key2 new-value`를 실행하지만 설정된 다른 매개변수를 생략하는 경우 액션이 업데이트되면 이 매개변수는 더 이상 존재하지 않습니다. 또한 액션에 바인딩된 모든 서비스가 제거되므로 다른 매개변수를 업데이트한 후 다시 [액션에 서비스를 바인딩](/docs/openwhisk?topic=cloud-functions-binding_services)해야 합니다.
    {: tip}

3. 매개변수를 전달하지 않고 액션을 호출하십시오.
    ```
  ibmcloud fn action invoke --result MyApp/hello
    ```
    {: pre}

    출력 예:
    ```
       {
           "payload": "Hello, World"
     }
    ```
    {: screen}

    패키지에 바인딩된 기본 매개변수가 사용됩니다. 
