---

copyright:
  years: 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# IBM Cloud에서 활성화 로그 보기
{: #openwhisk_logs}

[{{site.data.keyword.openwhisk}} 모니터링 페이지](https://console.bluemix.net/openwhisk/dashboard/)에서 직접 활성화 로그를 볼 수 있습니다. 또한 로그가 인덱스화되는 [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana)로 로그가 전달되어, 생성된 모든 메시지를 통한 전체 텍스트 검색 및 특정 필드를 기반으로 한 편리한 조회가 가능합니다(예: 로그 레벨).
{:shortdesc}

## 로그 조회 
{: #query-logs}

[IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) 호스팅 Kibana를 사용하면 로그 조회가 간단합니다. Kibana의 조회 구문을 사용하여 검색하려는 로그를 찾으십시오.

{{site.data.keyword.openwhisk_short}} UI를 사용하면 Kibana의 액션에 대한 로그 및 결과로 직접 이동할 수 있습니다. **로그** 링크는 [{{site.data.keyword.openwhisk}} 모니터링 페이지](https://console.bluemix.net/openwhisk/dashboard/)의 내부 왼쪽 탐색에 있습니다. 특정 액션의 세부사항 페이지에 액세스하면 **로그** 링크를 통해 이 특정 액션의 결과(활성화 레코드)로 이동합니다. 로그를 표시할 시간 범위의 기본값은 15분으로 설정됩니다. 이전 레코드를 표시하려면 오른쪽 상단 구석의 Kibana에서 직접 이 값을 변경할 수 있습니다.

다음은 오류를 디버그하는 데 도움이 되는 몇 가지 조회 예가 있습니다.

### 오류 로그 모두 찾기:
```
type: user_logs AND stream_str: stderr
```
{: codeblock}

### "myAction"으로 생성된 오류 로그 모두 찾기:
```
type: user_logs AND stream_str: stderr AND action_str: "*myAction"
```
{: codeblock}

## 결과 조회
{: #query-results}

또한 [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana)는 로그 라인 외에 {{site.data.keyword.openwhisk_short}}에서 생성된 결과(활성화 레코드)를 인덱스화합니다. 결과에는 지속 기간 또는 결과 코드(성공, 오류)와 같이 활성화와 관련된 다양한 메타데이터가 있습니다. 모든 필드가 조회 가능하며 이는 {{site.data.keyword.openwhisk_short}} 액션 작동 방식을 이해하는 데 도움이 될 수 있습니다.

Kibana의 조회 구문을 사용하여 검색하는 활성화를 찾으십시오. 다음은 오류를 디버그하는 데 도움이 되는 몇 가지 조회 예가 있습니다.

### 실패한 활성화 모두 찾기:
```
type: activation_record AND NOT status_str: 0
```
{: codeblock}

Unix 명령과 같이 "`0`"은 모든 것이 오류로 간주될 때 성공적으로 종료된 액션을 표시합니다.

<!--
### Finding all activations that took longer than 30 seconds:

```
type: activation_record AND duration > 30000
```

Duration is in milliseconds.
-->

### 특정 오류와 함께 실패한 활성화 모두 찾기:
```
type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
```
{: codeblock}
