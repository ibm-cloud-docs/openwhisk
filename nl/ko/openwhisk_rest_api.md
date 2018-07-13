---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# OpenWhisk REST API 사용
{: #openwhisk_rest_api}

OpenWhisk 환경이 사용 가능하도록 설정된 후에는 REST API 호출로 웹 앱이나 모바일 앱에서 OpenWhisk를 사용할 수 있습니다.
{: shortdesc}

액션, 활성화, 패키지, 규칙 및 트리거에 대한 API 관련 상세 정보는 [OpenWhisk API 문서](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/openwhisk/openwhisk/master/core/controller/src/main/resources/apiv1swagger.json)를 참조하십시오. 


REST API를 통해 시스템의 모든 기능을 사용할 수 있습니다. 콜렉션 및 엔티티 엔드포인트를 액션, 트리거, 규칙, 패키지, 활성화 및 네임스페이스에 대해 사용할 수 있습니다. 

사용 가능한 콜렉션 엔드포인트:
- `https://{APIHOST}/api/v1/namespaces`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/actions`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/triggers`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/rules`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/packages`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/activations`

`{APIHOST}`는 OpenWhisk API 호스트 이름입니다(예: openwhisk.ng.bluemix.net, 172.17.0.1, 192.168.99.100, 192.168.33.13 등).
`{namespace}`의 경우, `_` 문자를 사용하여 사용자의 *default namespace*를 지정할 수 있습니다. 

콜렉션 엔드포인트에서 GET 요청을 수행하여 콜렉션에서 엔티티의 목록을 페치할 수 있습니다. 

다음 엔티티 엔드포인트를 각 유형의 엔티티에 대해 사용할 수 있습니다. 
- `https://{APIHOST}/api/v1/namespaces/{namespace}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/actions/[{packageName}/]{actionName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/triggers/{triggerName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/rules/{ruleName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/packages/{packageName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/activations/{activationName}`

네임스페이스 및 활성화 엔드포인트는 GET 요청을 지원합니다. 액션, 트리거, 규칙 및 패키지 엔드포인트는 GET, PUT 및 DELETE 요청을 지원합니다. 액션, 트리거 및 규칙의 엔드포인트는 액션과 트리거를 호출하고 규칙을 사용하거나 사용 중지하는 데 사용되는 POST 요청도 지원합니다.  

모든 API는 HTTP 기본 인증으로 보호됩니다.
[wskadmin ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/apache/incubator-openwhisk/tree/master/tools/admin) 도구를 사용하여 새 네임스페이스와 인증을 생성할 수 있습니다.
기본 인증 신임 정보는 `~/.wskprops` 파일의 `AUTH` 특성에 있으며, 이는 콜론으로 구분됩니다.
`wsk property get --auth`를 실행하는 CLI를 사용하여 이러한 신임 정보를 검색할 수도 있습니다. 


다음 예제에서는 [cURL](https://curl.haxx.se) 명령 도구를 사용하여 `whisk.system` 네임스페이스에서 모든 패키지의 목록을 가져옵니다. 
```bash
curl -u USERNAME:PASSWORD https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/packages
```
{: pre}

```json
[
  {
    "name": "slack",
    "binding": false,
    "publish": true,
    "annotations": [
      {
        "key": "description",
        "value": "Package that contains actions to interact with the Slack messaging service"
      }
    ],
    "version": "0.0.1",
    "namespace": "whisk.system"
  }
]
```

이 예제에서 인증은 `-u` 플래그를 사용하여 전달되었습니다. 이 값을 `https://$AUTH@{APIHOST}` 등과 같은 URL의 일부로서 전달할 수도 있습니다. 

OpenWhisk API는 웹 클라이언트의 요청-응답 호출을 지원합니다. OpenWhisk는 Cross-Origin 리소스 공유 헤더의 `OPTIONS` 요청에 응답합니다. 현재 모든 원본이 허용되며(즉, Access-Control-Allow-Origin이 "`*`"임), Access-Control-Allow-Headers는 Authorization 및 Content-Type을 생성합니다. 

**주의:** OpenWhisk가 현재 네임스페이스당 하나의 키만 지원하므로 단순한 실험을 넘어서 CORS를 사용하지 않도록 권장합니다. [웹 액션](./openwhisk_webactions.html) 또는 [API 게이트웨이](./openwhisk_apigateway.html)를 사용하여 액션을 공개적으로 노출시키고 CORS를 요구하는 클라이언트 애플리케이션에 대해 OpenWhisk 권한 부여 키를 사용하지 마십시오. 

## CLI 상세 모드 사용
{: #openwhisk_rest_api_cli_v}

OpenWhisk CLI는 OpenWhisk REST API에 대한 인터페이스입니다.
HTTP 요청 및 응답에 대한 모든 정보를 인쇄하는 `-v` 플래그로 상세 모드에서 CLI를 실행할 수 있습니다. 

다음 명령을 실행하여 현재 사용자에 대한 네임스페이스 값을 표시하십시오. 
```
wsk namespace list -v
```
{: pre}

```
REQUEST:
[GET]	https://openwhisk.ng.bluemix.net/api/v1/namespaces
Req Headers
{
  "Authorization": [
    "Basic XXXYYYY"
  ],
  "User-Agent": [
    "OpenWhisk-CLI/1.0 (2017-08-10T20:09:30+00:00)"
  ]
}
RESPONSE:Got response with code 200
Resp Headers
{
  "Content-Type": [
    "application/json; charset=UTF-8"
  ]
}
Response body size is 28 bytes
Response body received:
["john@example.com_dev"]
```

인쇄된 정보는 HTTP 요청의 특성을 제공하며 URL `https://openwhisk.ng.bluemix.net/api/v1/namespaces`에서 HTTP 메소드 `GET`을 수행합니다(User-Agent 헤더 `OpenWhisk-CLI/1.0(<CLI-Build-version>)` 및 기본 Authorization 헤더 `Basic XXXYYYY`를 사용함). 권한 부여 값이 base64 인코딩된 OpenWhisk 권한 부여 문자열인 점에 유념하십시오.
응답의 컨텐츠 유형은 `application/json`입니다. 

## 액션
{: #openwhisk_rest_api_actions}

액션을 작성하거나 업데이트하려면 액션 콜렉션에서 `PUT` 메소드로 HTTP 요청을 전송하십시오. 예를 들어, 단일 파일 컨텐츠를 사용하여 이름이 `hello`인 `nodejs:6` 액션을 작성하려면 다음 명령을 사용하십시오. 
```bash
curl -u $AUTH -d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"}}' -X PUT -H "Content-Type: application/json" https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true 
```
{: pre}

액션의 블로킹 호출을 수행하고 입력 매개변수 `name`이 포함된 본문 및 `POST` 메소드로 HTTP 요청을 전송하려면 다음 명령을 사용하십시오. 
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?blocking=true \
-X POST -H "Content-Type: application/json" \
-d '{"name":"John"}'  
```
{: pre}

다음과 같은 응답을 받습니다. 
```json
{
  "duration": 2,
  "name": "hello",
  "subject": "john@example.com_dev",
  "activationId": "c7bb1339cb4f40e3a6ccead6c99f804e",
  "publish": false,
  "annotations": [{
    "key": "limits",
    "value": {
      "timeout": 60000,
      "memory": 256,
      "logs": 10
    }
  }, {
    "key": "path",
    "value": "john@example.com_dev/hello"
  }],
  "version": "0.0.1",
  "response": {
    "result": {
      "payload": "Hello John"
    },
    "success": true,
    "status": "success"
  },
  "end": 1493327653769,
  "logs": [],
  "start": 1493327653767,
  "namespace": "john@example.com_dev"
}
```
`response.result`를 가져오려면 다음 예제에서와 같이 조회 매개변수 `result=true`로 명령을 다시 실행하십시오.
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?blocking=true&result=true" \
-X POST -H "Content-Type: application/json" \
-d '{"name":"John"}' 
```
{: pre}
다음과 같은 응답을 받습니다.
```json
{
  "payload": "hello John"
}
```

## 어노테이션 및 웹 액션
{: #openwhisk_rest_api_webactions}

웹 액션으로서 액션을 작성하려면 웹 액션에 대해 `web-export=true`의 [어노테이션](./openwhisk_annotations.html)을 추가해야 합니다. 웹 액션이 공용으로 액세스 가능하므로 `final=true` 어노테이션을 사용하여 사전 정의된 매개변수를 예측하고자 할 수 있습니다(즉, 이를 최종으로 처리함). CLI 플래그 `--web true`를 사용하는 액션을 작성하거나 업데이트하는 경우, 명령은 어노테이션 `web-export=true` 및 `final=true`를 둘 다 추가합니다. 

다음 curl 명령을 실행하여 액션에서 설정할 어노테이션의 전체 목록을 제공하십시오. 
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}'
```
{: pre}

이제 OpenWhisk 권한 없이 공용 URL로서 이 액션을 호출할 수 있습니다. 웹 액션 공용 URL을 사용하고 예를 들어 `.json` 또는 `.http` 등의 확장자를 URL의 끝에 포함하여 호출을 시도하십시오. 
```bash
curl https://openwhisk.ng.bluemix.net/api/v1/web/john@example.com_dev/default/hello.json?name=John
```
{: pre}

```json
{
  "payload": "Hello John"
}
```

이 예제 소스 코드는 `.http`로 작동되지 않습니다. 수정 방법은 [웹 액션](./openwhisk_webactions.html) 문서를 참조하십시오. 

## 시퀀스
{: #openwhisk_rest_api_sequences}

액션 시퀀스를 작성하려면 원하는 순서로 시퀀스를 구성하는 액션의 이름을 제공하십시오. 그러면 첫 번째 액션의 출력이 다음 액션에 입력으로 전달될 수 있습니다. 

$ wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort

`/whisk.system/utils/split` 및 `/whisk.system/utils/sort` 액션으로 시퀀스를 작성하십시오. 
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/sequenceAction?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"sequenceAction","exec":{"kind":"sequence","components":["/whisk.system/utils/split","/whisk.system/utils/sort"]},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}' 
```
{: pre}

액션의 이름을 지정하는 경우, 이는 완전한 이름이어야 합니다. 

## 트리거
{: #openwhisk_rest_api_triggers}

트리거를 작성하려는 경우, 필요한 최소 정보는 트리거의 이름입니다. 트리거가 실행될 때 규칙을 통해 액션에 전달되는 기본 매개변수를 포함할 수도 있습니다. 

`webhook` 값이 설정된 기본 매개변수 `type`으로 이름이 `events`인 트리거를 작성하십시오. 
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/events?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"events","parameters":[{"key":"type","value":"webhook"}]}' 
```
{: pre}

이제 이 트리거를 실행해야 하는 이벤트가 있을 때마다 이는 OpenWhisk 권한 부여 키를 사용하여 `POST` 메소드로 HTTP 요청을 가져옵니다. 

`temperature` 매개변수로 `events` 트리거를 실행하려면 다음 HTTP 요청을 전송하십시오. 
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/events \
-X POST -H "Content-Type: application/json" \
-d '{"temperature":60}' 
```
{: pre}

### 피드 액션의 트리거
{: #openwhisk_rest_api_triggers_feed}

피드 액션을 사용하여 특수 트리거를 작성할 수 있습니다. 피드 액션은 트리거에 대한 이벤트가 발생할 때마다 트리거 실행을 담당하는 피드 제공자의 구성에 도움을 주는 액션입니다. [feeds.md] 문서에서 이러한 피드 제공자에 대해 자세히 알아보십시오. 

피드 액션을 활용하는 사용 가능한 트리거 중 일부는 periodic/alarms, Slack, Github, Cloudant/Couchdb 및 messageHub/Kafka입니다. 자체 피드 액션 및 피드 제공자를 작성할 수도 있습니다. 

지정된 빈도인 2시간마다 실행되는 이름이 `periodic`인 트리거를 작성하십시오(예: 02:00:00, 04:00:00, ...).

CLI를 사용하여 다음의 트리거 작성 명령을 실행하십시오. 
```bash
wsk trigger create periodic --feed /whisk.system/alarms/alarm \
  --param cron "0 */2 * * *" -v
```
{: pre}

`-v` 플래그가 사용되므로 2개의 HTTP 요청이 전송됩니다. 하나는 `periodic`이라고 하는 트리거를 작성하는 요청이고, 다른 하나는 피드 액션을 호출하는 요청입니다. 피드 액션 `/whisk.system/alarms/alarm`에는 2시간마다 트리거를 실행하도록 피드 제공자를 구성하기 위한 매개변수가 전송됩니다. 

REST API로 이 활동을 달성하려면 우선 다음과 같이 트리거를 작성하십시오. 
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"periodic","annotations":[{"key":"feed","value":"/whisk.system/alarms/alarm"}]}'  
```
{: pre}

표시된 대로 어노테이션 `Feed`는 트리거에 저장되어 있습니다. 나중에 이 어노테이션을 사용하여 트리거 삭제에 사용할 피드 액션을 알 수 있습니다. 

이제 트리거가 작성되었으므로 피드 액션을 호출합니다. 
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"cron\":\"0 */2 * * *\",\"lifecycleEvent\":\"CREATE\",\"triggerName\":\"/_/periodic\"}" 
```
{: pre}

트리거 삭제는 트리거 작성과 유사합니다. 따라서 지금은 트리거에 대한 핸들러 삭제도 수행하도록 피드 제공자를 구성하기 위해 피드 액션을 사용하여 트리거를 삭제합니다. 

다음 명령을 실행하여 피드 제공자에서 트리거 핸들러를 삭제하는 피드 액션을 호출하십시오. 
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"lifecycleEvent\":\"DELETE\",\"triggerName\":\"/_/periodic\"}"  
```
{: pre}

이제는 `DELETE` 메소드를 사용하여 HTTP 요청으로 트리거를 삭제하십시오. 
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic \
-X DELETE -H "Content-Type: application/json" 
```
{: pre}

## 규칙
{: #openwhisk_rest_api_rules}

트리거를 액션과 연관시키는 규칙을 작성하려면 요청의 본문에 트리거와 액션을 제공하기 위한 `PUT` 메소드로 HTTP 요청을 전송하십시오. 
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"t2a","status":"","trigger":"/_/events","action":"/_/hello"}' 
```
{: pre}

규칙을 사용하거나 사용하지 않을 수 있으며, 해당 상태 특성을 업데이트하여 규칙의 상태를 변경할 수 있습니다. 예를 들어, `t2a` 규칙을 사용하지 않으려면 `POST` 메소드로 `status: "inactive"` 요청의 본문을 전송하십시오. 
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X POST -H "Content-Type: application/json" \
-d '{"status":"inactive","trigger":null,"action":null}'  
```
{: pre}

## 패키지
{: #openwhisk_rest_api_packages}

패키지의 액션을 작성하려면 우선 패키지를 작성해야 합니다. 다음 명령을 실행하여 이름이 `iot`인 패키지를 작성하고 `PUT` 메소드로 HTTP 요청을 전송하십시오. 
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/packages/iot?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"iot"}' 
```
{: pre}

## 활성화
{: #openwhisk_rest_api_activations}

마지막 3개 활성화의 목록을 가져오려면 다음과 같이 조회 매개변수 `limit=3`을 전달하여 `GET` 메소드의 HTTP 요청을 사용하십시오. 
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations?limit=3
```
{: pre}

결과 및 로그가 포함된 활성화의 모든 세부사항을 가져오려면 다음과 같이 활성화 ID를 경로 매개변수로 전달하여 `GET` 메소드로 HTTP 요청을 전송하십시오. 
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations/f81dfddd7156401a8a6497f2724fec7b
```
{: pre}
