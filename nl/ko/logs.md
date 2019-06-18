---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: logging, monitoring, viewing, logs, query, performance, dashboard, metrics, health

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

# 로그 보기
{: #logs}

문제를 해결하는 데 도움이 될 수 있도록 로깅은 {{site.data.keyword.openwhisk}}에서 자동으로 사용됩니다. 또한 {{site.data.keyword.cloudaccesstraillong}} 서비스를 사용하여 사용자와 애플리케이션이 {{site.data.keyword.openwhisk_short}} 서비스와 상호작용하는 방법을 추적할 수 있습니다.


## 발생하는 액션 로그 보기
{: #logs_poll}

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




## 활성화 세부사항 보기
{: #activation_details}

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
    "namespace": "myNamespace",
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
            "value": "myNamespace/hello"
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
<td>이 활성화가 있는 네임스페이스입니다. 이는 액션이 있는 네임스페이스와는 다를 수 있습니다.</td>
</tr>
<tr>
<td><code>이름</code></td>
<td>액션의 이름입니다.</td>
</tr>
<tr>
<td><code>version</code></td>
<td>액션의 시맨틱 버전입니다.</td>
</tr>
<tr>
<td><code>subject</code></td>
<td>항목을 활성화할 사용자 계정입니다.</td>
</tr>
<tr>
<td><code>activationId</code></td>
<td>이 활성화 레코드의 ID입니다.</td>
</tr>
<tr>
<td><code>start</code></td>
<td>활성화가 시작된 시간입니다.</td>
</tr>
<tr>
<td><code>end</code></td>
<td>활성화가 완료된 시간입니다.</td>
</tr>
<tr>
<td><code>duration</code></td>
<td>활성화를 완료하는 데 걸린 시간(밀리초) 입니다.</td>
</tr>
<tr>
<td><code>response</code></td>
<td><ul><li><code>status</code>: 활성화의 종료 상태입니다.</li>
<li><code>statusCode</code>: 상태 코드입니다. 액션에서 오류가 발생한 경우에 HTTP 오류 코드입니다.</li>
<li><code>success</code>: 액션이 성공적으로 완료되었는지 여부입니다.</li>
<li><code>result</code>:활성화의 리턴값입니다.</li>
</ul></td>
</tr>
<tr>
<td><code>logs</code></td>
<td>이 활성화에 대한 로그입니다.</td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>이 액션의 어노테이션입니다. 가능한 활성화 어노테이션의 목록은 [어노테이션 참조 주제](/docs/openwhisk?topic=cloud-functions-annotations#annotations_activation)를 참조하십시오.</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>액션이 공공연하게 공개되는지 여부입니다.</td>
</tr>
</tbody></table>



## {{site.data.keyword.loganalysisfull_notm}}에서 로그 보기
{: #logs_view}

{{site.data.keyword.openwhisk_short}} 모니터링 대시보드에서 직접 활성화 로그를 볼 수 있습니다. 또한 로그가 자신이 인덱스화되는 [{{site.data.keyword.loganalysisfull}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana)에 전달되므로, 생성된 모든 메시지를 통한 전체 텍스트 검색 및 특정 필드 기반의 편리한 조회가 가능합니다.
{:shortdesc}

**참고**: 미국 동부 지역에서는 로깅을 사용할 수 없습니다.

1. [{{site.data.keyword.openwhisk_short}} 모니터링 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/openwhisk/dashboard)를 여십시오.

2. 선택사항: 특정 액션에 대해서만 보기를 보려면 모니터링 요약을 해당 액션으로 제한하십시오. 필터링 옵션 섹션의 **제한 대상** 드롭 다운 목록에서 액션 이름을 선택하십시오.

3. 왼쪽 탐색 창에서 **로그**를 클릭하십시오. {{site.data.keyword.loganalysisshort_notm}} Kibana 페이지가 열립니다.

4. 선택사항: 이전 로그를 보려면 오른쪽 상단 모서리에서 **마지막 15분**을 클릭하고 다른 시간 범위를 선택하여 기본 시간 범위 값인 15분을 변경하십시오.

### 로그 조회
{: #logs_query}

Kibana의 조회 구문을 사용하여 [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana)에서 특정 활성화 로그를 찾을 수 있습니다.

다음의 조회 예는 오류를 디버그하는 데 도움이 될 수 있습니다.
  * 모든 오류 로그 찾기:
      ```
type: user_logs AND stream_str: stderr
      ```
      {: codeblock}

  * "myAction"으로 생성된 모든 오류 로그 찾기:
      ```
type: user_logs AND stream_str: stderr AND action_str: "*myAction"
      ```
      {: codeblock}

### 결과 조회
{: #logs_query_results}

로그 라인에 추가하여, [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana)에서는 {{site.data.keyword.openwhisk_short}}가 생성하는 결과 또는 활성화 레코드도 인덱스화합니다. 결과에는 활성화 지속 기간 또는 활성화 결과 코드 등의 활성화 메타데이터가 포함되어 있습니다. 결과 필드 조회는 {{site.data.keyword.openwhisk_short}} 액션의 작동 방식을 이해하는 데 도움이 될 수 있습니다.

Kibana의 조회 구문을 사용하여 특정 활성화 로그를 찾을 수 있습니다. 다음의 조회 예는 오류를 디버그하는 데 도움이 될 수 있습니다.

* 실패한 모든 활성화 찾기:
    ```
type: activation_record AND NOT status_str: 0
    ```
    {: codeblock}
    결과에서 `0`은 성공적으로 종료된 액션을 표시하며 기타 모든 값은 오류를 표시합니다.

* 특정 오류로 실패한 모든 활성화 찾기:
    ```
type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
    ```
    {: codeblock}

