---

copyright:
  years: 2018
lastupdated: "2018-07-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 로깅 및 모니터링 활동
{: #openwhisk_logs}

문제를 해결하고 액션의 상태와 성능을 개선하는데 도움이 될 수 있도록 로깅 및 모니터링은 {{site.data.keyword.openwhisk_short}}에서 자동으로 사용됩니다. 

## 로그 보기
{: #view-logs}

{{site.data.keyword.openwhisk_short}} 모니터링 대시보드에서 직접 활성화 로그를 볼 수 있습니다. 또한 로그가 자신이 인덱스화되는 [{{site.data.keyword.loganalysisfull}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana)에 전달되므로, 생성된 모든 메시지를 통한 전체 텍스트 검색 및 특정 필드 기반의 편리한 조회가 가능합니다.
{:shortdesc}

1. [{{site.data.keyword.openwhisk_short}} 모니터링 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/openwhisk/dashboard/)를 여십시오. 

2. 선택사항: 특정 액션에 대해서만 보기를 보려면 모니터링 요약을 해당 액션으로 제한하십시오. 필터링 옵션 섹션의 **제한 대상** 드롭 다운 목록에서 액션 이름을 선택하십시오. 

3. 왼쪽 탐색 창에서 **로그**를 클릭하십시오. {{site.data.keyword.loganalysisshort_notm}} Kibana 페이지가 열립니다. 

4. 선택사항: 이전 로그를 보려면 오른쪽 상단 모서리에서 **마지막 15분**을 클릭하고 다른 시간 범위를 선택하여 기본 시간 범위 값인 15분을 변경하십시오. 

### 로그 조회
{: #query-logs}

Kibana의 조회 구문을 사용하여 [{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana)에서 특정 활성화 로그를 찾을 수 있습니다. 

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
{: #query-results}

로그 라인에 추가하여, [{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana)에서는 {{site.data.keyword.openwhisk_short}}가 생성하는 결과 또는 활성화 레코드도 인덱스화합니다. 결과에는 활성화 지속 기간 또는 활성화 결과 코드 등의 활성화 메타데이터가 포함되어 있습니다. 결과 필드 조회는 {{site.data.keyword.openwhisk_short}} 액션의 작동 방식을 이해하는 데 도움이 될 수 있습니다.

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

## 활동 모니터링
{: #openwhisk_monitoring}

[{{site.data.keyword.openwhisk_short}} 대시보드](https://console.bluemix.net/openwhisk/dashboard/)는 사용자의 활동에 대한 그래픽 요약을 제공합니다. 대시보드를 사용하여 {{site.data.keyword.openwhisk_short}} 액션의 성능 및 상태를 판별하십시오.
{:shortdesc}

보고자 하는 액션 로그를 선택하여 로그를 필터링하고 로깅된 활동의 시간 범위를 선택할 수 있습니다. 이러한 필터는 대시보드의 모든 보기에 적용됩니다. 언제든지 **다시 로드**를 클릭하여 최신 활성화 로그 데이터로 대시보드를 업데이트할 수 있습니다.

### 활동 요약
{: #summary}

**활동 요약** 보기는 {{site.data.keyword.openwhisk_short}} 환경의 상위 레벨 요약을 제공합니다. 이 보기를 사용하면 {{site.data.keyword.openwhisk_short}} 사용 서비스의 전체 상태와 성능을 모니터할 수 있습니다. 이 보기의 메트릭에서 다음을 수행할 수 있습니다.
* 호출된 횟수를 보고 서비스의 {{site.data.keyword.openwhisk_short}} 사용 액션의 이용률을 판별합니다. 
* 모든 액션에서 실패의 전반적인 속도를 판별합니다. 오류를 발견하는 경우, 사용자는 **활동 히스토그램** 보기를 보고 오류가 있는 서비스 또는 액션을 격리할 수 있습니다. **활동 로그**를 보고 오류 자체를 격리하십시오.
* 각 액션과 연관된 평균 완료 시간을 보고 액션이 얼마나 잘 수행되는지 판별합니다.

### 활동 타임라인
{: #timeline}

**활동 타임라인** 보기는 과거 및 현재 액션의 활동을 보기 위한 세로 막대 그래프를 표시합니다. 빨간색은 특정 액션 내의 오류를 표시합니다. 이 보기를 **활동 로그**와 연관시키면 오류에 대한 상세 정보를 찾을 수 있습니다.

<!--
### Activity Histogram
{: #histogram}

The **Activity Histogram** view displays a horizontal bar graph for viewing the activity of past and present actions. Red bars indicate errors within specific actions. Correlate this view with the **Activity Log** to find more details about errors.
-->

### 활동 로그
{: #log}

이 **활동 로그** 보기는 활성화 로그의 형식화된 버전을 표시합니다. 이 보기는 모든 활성화의 세부사항을 표시하지만 새 활성화에 대해 1분마다 한 번씩 폴링합니다. 자세한 로그를 표시하려면 액션을 클릭하십시오.

CLI를 사용하여 활동 로그에 표시된 출력을 가져오려면 다음 명령을 사용하십시오. 
```
ibmcloud fn activation poll
```
{: pre}
