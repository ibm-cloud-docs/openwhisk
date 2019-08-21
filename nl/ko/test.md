---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, serverless, javascript, node, node.js, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}



# 서버리스 앱 테스트
{: #test}

서버리스 앱이 작동하는지 확인하거나 발생할 수 있는 문제를 해결하려면 CLI에서 작성하는 각 엔티티를 테스트하십시오.
{: shortdesc}


## 액션 테스트
{: #test-js}

`invoke` 명령을 실행하여 액션을 테스트할 수 있습니다. 매개변수를 사용하거나 사용하지 않고 액션을 테스트할 수 있습니다.
{: shortdesc}

```bash
ibmcloud fn action invoke --result ACTION_NAME --param PARAMETER VALUE
```
{: pre}

**Hello world 예**
```bash
ibmcloud fn action invoke --result myAction --param name stranger
```
{: pre}

**출력**
```json
  {
      "greeting": "Hello stranger!"
  }
```
{: screen}



### JSON 파일에 저장된 매개변수 테스트
{: #test_json_file}

JSON 형식으로 된 매개변수의 파일을 전달할 수 있습니다.
{: shortdesc}

```
ibmcloud fn action invoke --result ACTION_NAME --param-file JSON_FILE
```
{: pre}

**출력 예**
```
{
    "payload": "Hello, Dorothy from Kansas"
}
```
{: screen}


### JSON 형식으로 입력된 매개변수 테스트
{: #test_json}

호출을 사용하여 JSON 형식 매개변수를 전달할 수 있습니다.
{: shortdesc}


```
ibmcloud fn action invoke --result ACTION_NAME -p person '{"PARAM_NAME": "PARAM_VALUE", "PARAM_NAME": "PARAM_VALUE"}'
```
{: pre}

**출력 예**
```
{
    "payload": "Hello, Dorothy from Kansas"
}
```
{: screen}


### 블로킹 액션 테스트
{: #test-block}

액션의 호출은 블로킹 또는 넌블로킹일 수 있습니다. 기본적으로 호출은 넌블로킹입니다. 지금 당장 액션 결과가 필요하지 않으면 넌블로킹 호출을 사용하십시오.
{: shortdesc}

블로킹 호출은 요청-응답 스타일을 사용하며 활성화 결과가 사용 가능할 때까지 기다립니다. 대기 시간은 60초 또는 액션의 [시간 제한 값](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits) 중에서 더 작은 값입니다.

블로킹 호출을 실행하여 클라우드에서 액션을 실행하십시오.

```
ibmcloud fn action invoke --blocking ACTION_NAME
```
{: pre}


**출력 예**
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

명령에서 다음 정보를 출력합니다.
* 호출 결과(예상 대기 시간 내에 사용 가능한 경우)
* `--result` 옵션이 없으면 결과에 활성화 ID가 표시됩니다. 호출 결과 또는 로그를 검색하는 데 사용될 수 있는 활성화 ID는 `44794bd6aab74415b4e42a308d880e5b`입니다.


## 트리거 테스트
{: #test_triggers}

트리거는 키-값 쌍의 사전을 사용하여 실행되거나 활성화될 수 있습니다. 종종 이 사전을 이벤트라고 합니다. 트리거는 사용자에 의해 명시적으로 실행되거나 외부 이벤트 소스에 의해 사용자 대신 실행될 수 있습니다. 액션에서와 같이, 룰과 연관된 트리거의 개별 실행은 결과적으로 활성화 ID를 생성합니다.
{: shortdesc}

1. 트리거를 실행하십시오.

    ```
    ibmcloud fn trigger fire TRIGGER_NAME --param PARAM_NAME PARAM_VALUE --param PARAM_NAME PARAM_VALUE
    ```
    {: pre}

    룰과 연관되지 않은 트리거에는 실행 시에 가시적인 영향이 없습니다. 이 트리거와 연관된 룰이 없으므로 전달된 매개변수는 액션에 의해 입력으로 사용되지 않습니다.

    **출력 예**

    ```
    ok: triggered TRIGGER_NAME with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

2. 가장 최근의 활성화 레코드를 확인하여 액션이 호출되었는지 확인하십시오.
    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    **출력 예**
    ```
    activations
    fa495d1223a2408b999c3e0ca73b2677             ACTION_NAME
    ```
    {: screen}

3. 이전 명령 출력에서 활성화 ID에 대한 자세한 정보를 가져오십시오.
    ```
    ibmcloud fn activation result ACTIVATION_ID
    ```
    {: pre}

    **출력 예**
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}




## 활성화 기간 테스트
{: #test_time}

활성화 로그를 가져와서 활성화를 완료하는 데 걸린 시간을 확인하십시오. 기간이 너무 길거나 기능을 더 오래 실행할 수 있도록 기본 제한시간을 조정해야 하는 경우 제한시간으로 액션을 업데이트할 수 있습니다.
{: shortdesc}

1. 활성화 ID를 가져오십시오.

    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    출력 예:
    ```
    activations
    b066ca51e68c4d3382df2d8033265db0             ACTION_NAME
    ```
    {: screen}

2. 활성화 ID에 대한 로그를 가져오십시오.

    ```
      ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
    ```
    {: pre}

    `duration`는 시간(밀리초)를 표시합니다. 활성화는 다음을 완료하는 데 2초가 조금 넘게 걸렸습니다.

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

3. 제한시간(초)으로 액션을 업데이트하십시오.

    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME --timeout VALUE
    ```
    {: pre}

    예:
    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10 --timeout 1000
    ```
    {: pre}


## 메모리 사용량 테스트
{: #test_memory}

앱이 Docker 이미지에서 패키징된 경우 Docker 명령을 사용하여 앱의 메모리 사용량을 확인할 수 있습니다.
{: shortdesc}

1. Docker 이미지를 실행하는 컨테이너를 로컬로 작성하십시오.

    ```
    docker run IMAGE_NAME
    ```
    {: pre}

2. 컨테이너 ID를 가져오려면 컨테이너의 목록을 가져오십시오.

    ```
    docker ps
    ```
    {: pre}

3. 실행 중인 컨테이너의 통계를 확인하십시오.

    ```
    docker stats CONTAINER_ID
    ```
    {: pre}

4. 컨테이너에 대한 메모리 사용량 값을 검토하십시오. 값이 시스템 한계 내에 없으면 스크립트를 조정하십시오.

5. 정보를 검토한 후 실행 중인 컨테이너를 중지할 수 있습니다.

    ```
    docker stop CONTAINER_ID
    ```
    {: pre}

6. 컨테이너를 제거하십시오.

    ```
    docker rm CONTAINER_ID
    ```
    {: pre}








