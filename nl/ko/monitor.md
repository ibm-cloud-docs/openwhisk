---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: monitoring, viewing, performance, dashboard, metrics, health, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# 활동 모니터링
{: #monitor}

{{site.data.keyword.openwhisk}}와 함께 배치된 액션의 성능에 대한 인사이트를 얻을 수 있습니다. 메트릭은 사용자가 병목 현상을 찾거나 액션 지속 기간, 액션 활성화 결과 또는 액션 활성화 한계에 도달한 시점을 기반으로 가능한 프로덕션 문제를 예측하도록 도울 수 있습니다.
{: shortdesc}

메트릭은 모든 엔티티에 대해 자동으로 수집됩니다. 액션이 IAM 기반 또는 Cloud Foundry 기반 네임스페이스에 있는지에 따라 메트릭이 IBM Cloud 계정 또는 영역에 있습니다. 이러한 메트릭은 {{site.data.keyword.monitoringlong}}에 전송되고 Grafana를 통해 사용할 수 있으며, 여기서는 대시보드를 구성하고 메트릭 이벤트 값을 기반으로 경보를 작성할 수 있습니다. 메트릭에 대한 자세한 정보는 [{{site.data.keyword.monitoringlong_notm}} 문서](/docs/services/cloud-monitoring?topic=cloud-monitoring-getting-started#getting-started)의 내용을 참조하십시오.

## 대시보드 작성
{: #monitor_dash}

Grafana 모니터링 대시보드를 작성하여 시작하십시오.

1. 다음 URL 중 하나로 이동하십시오.
  <table>
    <thead>
      <tr>
        <th>{{site.data.keyword.openwhisk_short}} 영역</th>
        <th>주소 모니터링</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>유럽 중부</td>
        <td>`metrics.eu-de.bluemix.net`</td>
      </tr>
      <tr>
        <td>영국 남부</td>
        <td>`metrics.eu-gb.bluemix.net`</td>
      </tr>
      <tr>
        <td>미국 남부</td>
        <td>`metrics.ng.bluemix.net`</td>
      </tr>
      <tr>
        <td>미국 동부</td>
        <td>사용할 수 없음</td>
      </tr>
    </tbody>
  </table>

2. 메트릭 도메인을 선택하십시오.
    * IAM 기반 네임스페이스:
        1. 사용자 이름을 클릭하십시오.
        2. **도메인** 드롭 다운 목록에서 **계정**을 선택하십시오.
        3. **계정** 드롭다운 목록에서 IAM 기반 네임스페이스가 있는 IBM Cloud 계정을 선택하십시오.
    * Cloud Foundry 기반 네임스페이스:
        1. 사용자 이름을 클릭하십시오.
        2. **도메인** 드롭 다운 목록에서 **영역**을 선택하십시오.
        3. Cloud Foundry 기반의 네임스페이스를 선택하려면 **조직** 및 **영역** 드롭 다운 목록을 사용하십시오.

3. 대시보드를 작성합니다.
    * 사전 작성 {{site.data.keyword.openwhisk_short}} 대시보드를 사용하려면 다음을 수행하십시오.
        1. **홈 > 가져오기**로 이동하십시오.
        3. 사전 작성 {{site.data.keyword.openwhisk_short}} 대시보드의 ID `8124`를 **Grafana.net 대시보드** 필드에 입력하십시오.
        4. **가져오기**를 클릭하십시오.
    * 사용자 정의 대시보드를 작성하려면 **홈 > 새로 작성**을 탐색하십시오.

액션이 실행된 후에 새 메트릭이 생성되고 Grafana에서 검색 가능합니다. 참고: 실행된 액션이 Grafana에 표시되는 데 최대 10분이 소요될 수 있습니다.




## 대시보드 사용
{: #monitor_dash_use}

[{{site.data.keyword.openwhisk_short}} 대시보드](https://cloud.ibm.com/openwhisk/dashboard){: external}는 사용자의 활동에 대한 그래픽 요약을 제공합니다. 대시보드를 사용하여 {{site.data.keyword.openwhisk_short}} 액션의 성능 및 상태를 판별하십시오.
{:shortdesc}

보고자 하는 액션 로그를 선택하여 로그를 필터링하고 로깅된 활동의 시간 범위를 선택할 수 있습니다. 이러한 필터는 대시보드의 모든 보기에 적용됩니다. 언제든지 **다시 로드**를 클릭하여 최신 활성화 로그 데이터로 대시보드를 업데이트할 수 있습니다.

### 활동 요약
{: #monitor_dash_sum}

**활동 요약** 보기는 {{site.data.keyword.openwhisk_short}} 환경의 상위 레벨 요약을 제공합니다. 이 보기를 사용하면 {{site.data.keyword.openwhisk_short}} 사용 서비스의 전체 상태와 성능을 모니터할 수 있습니다. 이 보기의 메트릭에서 다음을 수행할 수 있습니다.
* 호출된 횟수를 보고 서비스의 {{site.data.keyword.openwhisk_short}} 사용 액션의 이용률을 판별합니다.
* 모든 액션에서 실패의 전반적인 속도를 판별합니다. 오류를 발견하는 경우, 사용자는 **활동 히스토그램** 보기를 보고 오류가 있는 서비스 또는 액션을 격리할 수 있습니다. **활동 로그**를 보고 오류 자체를 격리하십시오.
* 각 액션과 연관된 평균 완료 시간을 보고 액션이 얼마나 잘 수행되는지 판별합니다.

### 활동 타임라인
{: #monitor_dash_time}

**활동 타임라인** 보기는 과거 및 현재 액션의 활동을 보기 위한 세로 막대 그래프를 표시합니다. 빨간색은 특정 액션 내의 오류를 표시합니다. 이 보기를 **활동 로그**와 연관시키면 오류에 대한 상세 정보를 찾을 수 있습니다.



### 활동 로그
{: #monitor_dash_log}

이 **활동 로그** 보기는 활성화 로그의 형식화된 버전을 표시합니다. 이 보기는 모든 활성화의 세부사항을 표시하지만 새 활성화에 대해 1분마다 한 번씩 폴링합니다. 자세한 로그를 표시하려면 액션을 클릭하십시오.

CLI를 사용하여 활동 로그에 표시된 출력을 가져오려면 다음 명령을 사용하십시오.
```
ibmcloud fn activation poll
```
{: pre}




## 메트릭 형식
{: #monitor_metric}

메트릭은 분 단위로 집계된 액션 활성화에서 수집된 데이터를 반영합니다. 메트릭은 액션 성능 또는 액션 동시성 레벨에서 검색 가능합니다.


### 액션 성능 메트릭
{: #monitor_metric_perf}

액션 성능 메트릭은 단일 액션에 대해 계산된 값입니다. 액션 성능 메트릭은 활성화의 상태 및 실행의 타이밍 특성을 둘 다 포함합니다. 참고: 작성 동안 패키지의 이름을 지정하지 않을 경우, 기본 패키지 이름이 사용됩니다. 이러한 메트릭은 다음 형식을 사용합니다.

```
ibmcloud.public.functions.<region>.action.namespace.<namespace>.<package>.<action>.<metric_name>
```
{: codeblock}

마침표(.), AT 부호(@), 공백( ), 앰퍼샌드(&), 밑줄(_), 콜론(:) 문자는 대시(`-`)로 변환됩니다.
{: tip}

예: 이름이 `hello-world`인 액션이 `us-south` 지역에서 Cloud Foundry 기반 네임스페이스 `user@email.com_dev`에 있는 경우, 액션 성능 메트릭이 다음과 유사할 수 있습니다.

```
ibmcloud.public.functions.us-south.action.namespace.user-ibm-com-dev.action-performance.default.hello-world.duration
```
{: screen}

</br>

### 액션 동시성 메트릭
{: #monitor_metric_con}

액션 동시성 메트릭은 네임스페이스의 모든 활성 액션에서 데이터를 기반으로 계산됩니다. 액션 동시성에는 동시성 한계가 초과될 때 잠재적으로 발생할 수 있는 시스템 조절 수와 동시 호출의 수가 포함됩니다. 이러한 메트릭은 다음 형식을 사용합니다.

```
ibmcloud.public.functions.<region>.action.namespace.<namespace>.action-performance.<package>.<action>.<metric_name>
```
{: codeblock}

예: 이름이 `myNamespace`인 IAM 기반 네임스페이스가 `us-south` 지역에 있는 경우, 액션 동시성 메트릭은 다음과 유사하게 보일 수 있습니다.

```
ibmcloud.public.functions.us-south.action.namespace.all.concurrent-invocations
```
{: screen}

</br>

### 사용 가능 메트릭
{: #monitor_metric_av}

수천 또는 수백만 개의 액션 활성화가 있을 수 있으므로, 많은 활성화에 의해 생성된 이벤트의 집계로 메트릭 값이 표시됩니다. 값은 다음과 같은 방식으로 집계됩니다.
* 합계: 모든 메트릭 값이 추가됩니다.
* 평균: 산술 평균이 계산됩니다.
* 합계된 평균: 산술 평균은 컴포넌트를 기반으로 계산되며 다른 컴포넌트를 함께 추가합니다.

사용자가 사용할 수 있는 메트릭을 보려면 다음 표를 확인하십시오.

<table>
  <thead>
    <tr>
      <th>메트릭 이름</th>
      <th>설명</th>
      <th>유형</th>
      <th>카테고리</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>duration</code></td>
      <td>평균 액션 지속 기간, 청구된 액션 실행 시간.</td>
      <td>평균</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>init-time</code></td>
      <td>액션 컨테이너를 초기화하는 데 소요된 시간입니다.</td>
      <td>평균</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>wait-time</code></td>
      <td>활성화가 스케줄링될 때까지 기다리는 큐에서 소요된 평균 시간입니다.</td>
      <td>평균</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>activation</code></td>
      <td>시스템에서 트리거된 전체 활성화 수입니다.</td>
      <td>합계</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.success</code></td>
      <td>액션 코드의 성공적인 활성화 수입니다.</td>
      <td>합계</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.application</code></td>
      <td>애플리케이션 오류로 인해 발생한 실패한 활성화 수입니다. 예를 들어, 액션에 오류가 있습니다. 액션 성능 메트릭이 파생되는 방법에 대한 자세한 정보는 [활성화 레코드 이해하기](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions.md#understanding-the-activation-record){: external}의 내용을 확인하십시오.</td>
      <td>합계</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.developer</code></td>
      <td>개발자로 인해 발생한 실패한 활성화 수입니다. 예를 들어, 액션 코드에서 처리되지 않은 예외에 의한 [액션 프록시 인터페이스](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions-new.md#action-interface){: external}의 위반이 있습니다.</td>
      <td>합계</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.internal</code></td>
      <td>{{site.data.keyword.openwhisk_short}} 내부 오류로 인해 실패한 활성화 수입니다.</td>
      <td>합계</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>concurrent-rate-limit</code></td>
      <td>동시성 비율 한계를 초과하여 조절된 활성화의 합계입니다. 한계에 도달하지 않은 경우 메트릭이 생성되지 않습니다.</td>
      <td>합계</td>
      <td><code>action-concurrency</code></td>
    </tr>
    <tr>
      <td><code>timed-rate-limit</code></td>
      <td>분당 한계를 초과하여 조절된 활성화의 합계입니다. 한계에 도달하지 않은 경우 메트릭이 생성되지 않습니다.</td>
      <td>합계</td>
      <td><code>action-concurrency</code></td>
    </tr>
    <tr>
      <td><code>concurrent-invocations</code></td>
      <td>시스템의 동시 호출 수입니다.</td>
      <td>합계된 평균</td>
      <td><code>action-concurrency</code></td>
    </tr>
  </tbody>
</table>

기본 네임스페이스의 일부로 존재하는 액션의 메트릭은 기본 카테고리에서 사용 가능합니다.
{: tip}




