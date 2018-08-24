---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-25"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 액션 관리
{: #openwhisk_managing}

액션 출력 모니터링, 액션에 대한 특정 정보 가져오기 또는 액션 삭제를 통해 액션을 관리합니다.
{: shortdec}

## 액션 가져오기
{: #getting-actions}

액션을 작성한 후에는 액션 세부사항에 대한 자세한 정보를 가져오고 네임스페이스의 액션을 나열할 수 있습니다.
{: shortdesc}

작성한 모든 액션을 나열하려면 다음을 실행하십시오.
```
ibmcloud fn action list
```
{: pre}

추가로 액션을 작성할 때 관련 액션을 [패키지](./openwhisk_packages.html)로 그룹화하면 유용할 수 있습니다. 특정 패키지 내의 액션으로만 액션의 목록을 필터링하려면 다음을 실행하십시오. 
```
ibmcloud fn action list [PACKAGE NAME]
```
{: pre}

특정 액션을 설명하는 메타데이터를 가져오려면 다음을 실행하십시오.

```
ibmcloud fn action get hello
```
{: pre}

출력 예:
```
ok: got action hello
{
    "namespace": "user@email.com",
    "name": "hello",
    "version": "0.0.1",
    "exec": {
        "kind": "nodejs:6",
        "binary": false
    },
    "annotations": [
        {
            "key": "exec",
            "value": "nodejs:6"
        }
    ],
    "limits": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}

<table>
<caption><code>action get</code> 명령 출력 이해하기</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> <code>action get</code> 명령 출력 이해하기</th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>이 액션이 있는 네임스페이스입니다. </td>
</tr>
<tr>
<td><code>name</code></td>
<td>액션의 이름입니다. </td>
</tr>
<tr>
<td><code>version</code></td>
<td>액션의 시맨틱 버전입니다. </td>
</tr>
<tr>
<td><code>exec</code></td>
<td><ul><li><code>kind</code>: 액션의 유형입니다. 가능한 값: nodejs:6, nodejs:8, php:7.1, python:3, python-jessie:3, swift:3.1.1, swift:4.1, java, blackbox 및 sequence.</li>
<li><code>code</code>: kind가 nodejs 또는 swift일 때 실행할 Javascript 또는 Swift 코드입니다. </li>
<li><code>components</code>: kind가 sequence일 때 시퀀스의 액션입니다. 액션은 순서대로 나열됩니다. </li>
<li><code>image</code>: kind가 blackbox일 때 컨테이너 이미지 이름입니다. </li>
<li><code>init</code>: kind가 nodejs일 때 선택적 zipfile 참조입니다. </li>
<li><code>binary</code>: 액션이 2진 실행 파일로 컴파일되는지 여부입니다. </li></ul></td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>이 액션의 어노테이션입니다. 가능한 어노테이션의 목록을 보려면 [액션 어노테이션](openwhisk_annotations.html#action) 및 [웹 액션 어노테이션](openwhisk_annotations.html#annotations-specific-to-web-actions) 참조 주제를 참조하십시오. </td>
</tr>
<tr>
<td><code>limits</code></td>
<td><ul><li><code>timeout</code>: 액션이 종료된 이후 액션에 대해 설정된 제한시간(밀리초)입니다. 기본값: 6000</li>
<li><code>memory</code>: 액션에 대해 설정된 최대 메모리 한계(MB)입니다. 기본값: 256</li>
<li><code>logs</code>: 액션에 대해 설정된 최대 로그 크기 한계(MB)입니다. 기본값: 10</li></ul></td>
</tr>
<tr>
<td><code>publish</code></td>
<td>액션이 공공연하게 공개되는지 여부입니다. </td>
</tr>
</tbody></table>

## 활성화 세부사항 보기
{: #activation}

{{site.data.keyword.openwhisk_short}} 액션은 기타 사용자에 의해, 다양한 이벤트에 대한 응답으로 또는 액션 시퀀스의 일부로서 호출될 수 있습니다. 액션이 호출될 때마다 해당 호출에 대해 활성화 레코드가 작성됩니다. 액션 호출의 결과에 대한 정보를 얻기 위해 활성화에 대한 세부사항을 가져올 수 있습니다. 

네임스페이스에서 모든 활성화 레코드 ID를 가져오려면 다음을 실행하십시오. 
```
ibmcloud fn activation list
```
{: pre}

액션 호출의 결과인 특정 활성화 레코드에 대한 세부사항을 가져오려면 다음을 실행하십시오. 
```
ibmcloud fn activation get <activation_ID>
```
{: pre}

출력 예:
```
ok: got activation c2b36969fbe94562b36969fbe9856215
{
    "namespace": "BobsOrg_dev",
    "name": "hello",
    "version": "0.0.1",
    "subject": "user@email.com",
    "activationId": "c2b36969fbe94562b36969fbe9856215",
    "start": 1532456307768,
    "end": 1532456309838,
    "duration": 2070,
    "response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
            "done": true
  }
    },
    "logs": [],
    "annotations": [
        {
            "key": "path",
            "value": "BobsOrg_dev/hello"
        },
        {
            "key": "waitTime",
            "value": 50
        },
        {
            "key": "kind",
    "value": "nodejs:6"
        },
        {
            "key": "limits",
    "value": {
                "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
        },
        {
            "key": "initTime",
            "value": 53
        }
    ],
    "publish": false
}
```
{: screen}

<table>
<caption><code>activation get</code> 명령 출력 이해하기</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> <code>activation get</code> 명령 출력 이해하기</th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>이 활성화가 있는 네임스페이스입니다. 이는 액션이 있는 네임스페이스와는 다를 수 있습니다. </td>
</tr>
<tr>
<td><code>name</code></td>
<td>액션의 이름입니다. </td>
</tr>
<tr>
<td><code>version</code></td>
<td>액션의 시맨틱 버전입니다. </td>
</tr>
<tr>
<td><code>subject</code></td>
<td>항목을 활성화할 사용자 계정입니다. </td>
</tr>
<tr>
<td><code>activationId</code></td>
<td>이 활성화 레코드의 ID입니다. </td>
</tr>
<tr>
<td><code>start</code></td>
<td>활성화가 시작된 시간입니다. </td>
</tr>
<tr>
<td><code>end</code></td>
<td>활성화가 완료된 시간입니다. </td>
</tr>
<tr>
<td><code>duration</code></td>
<td>활성화를 완료하는 데 걸린 시간(밀리초) 입니다.</td>
</tr>
<tr>
<td><code>response</code></td>
<td><ul><li><code>status</code>: 활성화의 종료 상태입니다. </li>
<li><code>statusCode</code>: 상태 코드입니다. 액션에서 오류가 발생한 경우에 HTTP 오류 코드입니다. </li>
<li><code>success</code>: 액션이 성공적으로 완료되었는지 여부입니다. </li>
<li><code>result</code>:활성화의 리턴값입니다. </li>
</ul></td>
</tr>
<tr>
<td><code>logs</code></td>
<td>이 활성화에 대한 로그입니다. </td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>이 액션의 어노테이션입니다. 가능한 활성화 어노테이션의 목록은 [어노테이션 참조 주제](openwhisk_annotations.html#activation)를 참조하십시오. </td>
</tr>
<tr>
<td><code>publish</code></td>
<td>액션이 공공연하게 공개되는지 여부입니다. </td>
</tr>
</tbody></table>

## 액션 본문 내의 액션 메타데이터 액세스
{: #accessing-action-metadata-within-the-action-body}

액션 환경에는 실행 중인 액션에 특정한 여러 특성이 포함되어 있습니다. 이러한 특성은 액션이 REST API를 통해 프로그래밍 방식으로 OpenWhisk 자산에 대해 작업하거나 액션이 할당된 시간 예산을 모두 소진하려는 시점에 내부 알람을 설정할 수 있도록 허용합니다. 특성은 지원되는 모든 런타임에 대해 시스템 환경에서 액세스가 가능합니다(OpenWhisk Docker 스켈레톤 사용 시의 Node.js, Python, Swift, Java 및 Docker). 

|특성|설명 |
| -------- | ----------- |
| `__OW_API_HOST` |이 액션을 실행 중인 OpenWhisk 배치를 위한 API 호스트입니다. |
| `__OW_API_KEY` |액션을 호출 중인 주제에 대한 API 키로서, 이 키는 제한된 API 키일 수 있습니다. |
| `__OW_NAMESPACE` |활성화에 대한 네임스페이스입니다. 이 네임스페이스는 액션데 대한 네임스페이스와 동일하지 않을 수 있습니다. |
| `__OW_ACTION_NAME` |실행 중인 액션의 완전한 이름입니다. |
| `__OW_ACTIVATION_ID` |실행 중인 이 액션 인스턴스에 대한 활성화 ID입니다. |
| `__OW_DEADLINE` |이 액션이 전체 기간 할당량을 소진할 개략적인 시간(에포크 밀리초)입니다. |

## 액션 URL 가져오기
{: #get-action-url}

액션은 HTTPS 요청을 통해 REST 인터페이스를 사용하여 호출될 수 있습니다.
{: shortdesc}

액션 URL을 가져오려면 다음을 실행하십시오.
```
ibmcloud fn action get actionName --url
```
{: pre}

표준 액션의 출력 예: 
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

[웹 액션](./openwhisk_webactions.html)의 출력 예: 
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**참고:** 표준 액션의 경우 인증은 HTTPS 요청을 통해 호출할 때 제공해야 합니다. REST 인터페이스를 사용하는 액션 호출에 대한 자세한 정보는 [REST API 참조](https://console.bluemix.net/apidocs/functions)를 참조하십시오.

## 액션 코드 저장
{: #save-action}

기존 액션과 연관된 코드를 가져와서 로컬로 저장할 수 있습니다. 시퀀스 및 Docker 액션을 제외한 모든 액션에 대해 코드를 저장할 수 있습니다.
{: shortdesc}

현재 작업 디렉토리에 기존 액션 이름과 일치하는 파일 이름으로 액션 코드를 저장하십시오.
```
ibmcloud fn action get actionName --save
```
{: pre}

액션 유형에 대응되는 파일 확장자가 사용됩니다. Zip 파일인 액션 코드의 경우에는 .zip 확장자가 사용됩니다. 출력 예:
```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
```
{: screen}

대신 `--save-as` 플래그를 사용하여 사용자 정의 파일 경로, 파일 이름 및 확장자를 제공할 수 있습니다. 
```
ibmcloud fn action get actionName --save-as codeFile.js
```
{: pre}

출력 예:
```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
```
{: screen}

## 액션 로그 모니터링
{: #monitor-action-output}

{{site.data.keyword.openwhisk_short}} 액션은 기타 사용자에 의해, 다양한 이벤트에 대한 응답으로 또는 액션 시퀀스의 일부로서 호출될 수 있습니다. 액션이 호출된 시간과 해당 출력에 대한 정보를 가져오기 위해 액션 로그의 모니터링이 유용할 수 있습니다. 

{{site.data.keyword.openwhisk_short}} CLI를 사용하여 액션이 호출될 때 액션의 출력을 감시할 수 있습니다.

1. 활성화의 로그를 지속적으로 검사하는 폴링 루프를 시작하십시오.
    ```
    ibmcloud fn activation poll
    ```
    {: pre}

2. 다른 창으로 전환하여 액션을 호출하십시오.
    ```
    ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
    ```
    {: pre}

    출력 예:
    ```
      ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
    ```
    {: screen}

3. 폴링 창에서 활성화 로그를 볼 수 있습니다.
    ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
    ```
    {: screen}
    실시간에 {{site.data.keyword.openwhisk_short}}에서 사용자 대신 실행되는 액션에 대한 로그를 보고자 할 수 도 있습니다. 

## 액션 삭제
{: #deleting-actions}

사용하지 않을 액션을 삭제하여 정리할 수 있습니다.

1. 액션을 삭제하십시오.
    ```
    ibmcloud fn action delete hello
    ```
    {: pre}

    출력 예:
    ```
      ok: deleted hello
    ```
    {: screen}

2. 액션이 더 이상 액션 목록에 표시되지 않는지 확인하십시오.
    ```
    ibmcloud fn action list
    ```
    {: pre}

    출력 예:
    ```
      actions
    ```
    {: screen}
