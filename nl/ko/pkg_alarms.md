---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: alarms, serverless, triggers, functions

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



# 알람
{: #pkg_alarms}

`/whisk.system/alarms` 패키지를 사용하여 지정된 빈도로 트리거를 실행할 수 있습니다. 알람은 반복 작업 또는 태스크(예: 매시간 시스템 백업 호출)를 설정하는 데 유용합니다.
{: shortdesc}

패키지에는 다음 피드가 포함됩니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
|`/whisk.system/alarms` |패키지 | - |알람 및 주기적 유틸리티입니다. |
|`/whisk.system/alarms/once` |피드 |`date`, `trigger_payload`, `deleteAfterFire` | 특정 날짜에 한 번만 트리거 이벤트를 실행합니다. |
|`/whisk.system/alarms/interval` |피드 |`minutes`, `trigger_payload`, `startDate`, `stopDate` | 간격 기반 스케줄에 따라 트리거 이벤트를 실행합니다. |
|`/whisk.system/alarms/alarm` |피드 |`cron`, `timezone`, `trigger_payload`, `startDate`, `stopDate` |cron을 사용하여 시간 기반 스케줄에 따라 트리거 이벤트를 실행합니다. |



## 트리거 이벤트를 한 번만 실행
{: #pkg_alarms_one}

`/whisk.system/alarms/once` 피드는 지정된 날짜에 트리거 이벤트를 한 번 실행하도록 알람 서비스를 구성합니다. 한 번 실행 알람을 작성하려면 다음 명령을 실행하십시오.

```
ibmcloud fn trigger create fireOnce --feed /whisk.system/alarms/once --param date "<date>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param deleteAfterFire "<delete_option>"
```
{: pre}
</br>

<table>
<caption><code>trigger create fireOnce</code> 명령 컴포넌트 이해하기</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> <code>trigger create fireOnce</code> 명령 컴포넌트 이해하기</th>
</thead>
<tbody>
<tr>
<td><code>fireOnce</code></td>
<td>작성 중인 알람 트리거의 유형입니다.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/once</code></td>
<td>fireOnce 피드에 대한 알람 패키지 파일 경로입니다.</td>
</tr>
<tr>
<td><code>--param date</code></td>
<td><code>&lt;date&gt;</code>를 트리거를 실행할 날짜로 대체하십시오. 트리거는 지정된 시간에 한 번 실행됩니다. `date` 매개변수는 정수 또는 문자열 값을 지원합니다. 정수 값은 `1970년 1월 1일 00:00:00` UTC 이후의 시간(밀리초)을 표시하며, 문자열 값은 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 형식</a>형식이어야 합니다.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>(선택사항) <code>&lt;key&gt;</code> 및 <code>&lt;value&gt;</code>를 트리거 실행 시의 트리거 매개변수로 대체하십시오.</td>
</tr>
<tr>
<td><code>--param deleteAfterFire</code></td>
<td>(선택사항) 트리거 실행 이후에 트리거 및 연관된 룰이 삭제되는지 여부입니다. <code>&lt;delete_option&gt;</code>을 다음 중 하나로 대체하십시오.<ul><li><code>false</code> - (기본값) 트리거 실행 이후에 액션이 수행되지 않습니다.</li><li><code>true</code> - 실행 이후에 트리거가 삭제됩니다.</li><li><code>rules</code> - 실행 이후에 트리거 및 이와 연관된 모든 룰이 삭제됩니다.</li></ul></td>
</tr>
</tbody></table>

다음 명령은 2019년 12월 25일 12:30:00 UTC에 한 번 실행되는 트리거를 작성하는 예입니다. 각 트리거 이벤트에는 `name=Odin` 및 `place=Asgard` 매개변수가 있습니다. 트리거 실행 이후에 트리거 및 연관된 모든 룰이 삭제됩니다.

```
ibmcloud fn trigger create fireOnce \
  --feed /whisk.system/alarms/once \
  --param date "2019-12-25T12:30:00.000Z" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param deleteAfterFire "rules"
```
{: pre}


## 간격 기반 스케줄에 따라 주기적으로 트리거 이벤트 실행
{: #pkg_alarms_int}

`/whisk.system/alarms/interval` 피드는 간격 기반 스케줄에 따라 트리거 이벤트를 실행하도록 알람 서비스를 구성합니다. 간격 기반 알람을 작성하려면 다음 명령을 실행하십시오.
```
ibmcloud fn trigger create interval --feed /whisk.system/alarms/interval --param minutes "<minutes>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}
</br>

<table>
<caption><code>trigger create interval</code> 명령 컴포넌트 이해하기</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> <code>trigger create interval</code> 명령 컴포넌트 이해하기</th>
</thead>
<tbody>
<tr>
<td><code>interval</code></td>
<td>작성 중인 알람 트리거의 유형입니다.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/interval</code></td>
<td>간격 피드에 대한 알람 패키지 파일 경로입니다.</td>
</tr>
<tr>
<td><code>--param minutes</code></td>
<td><code>&lt;minutes&gt;</code>를 트리거 실행 간의 시간 간격(분)을 표시하는 정수로 대체하십시오.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>(선택사항) <code>&lt;key&gt;</code> 및 <code>&lt;value&gt;</code>를 트리거 실행 시의 트리거 매개변수로 대체하십시오.</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>(선택사항) <code>&lt;startDate&gt;</code>를 첫 번째 트리거를 실행할 날짜로 대체하십시오. 후속 실행은 분 매개변수에서 지정하는 시간 간격을 기반으로 발생합니다. 이 매개변수는 정수 또는 문자열 값을 지원합니다. 정수 값은 `1970년 1월 1일 00:00:00` UTC 이후의 시간(밀리초)을 표시하며, 문자열 값은 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 형식</a>형식이어야 합니다.</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>(선택사항) <code>&lt;stopDate&gt;</code>를 트리거를 중지할 날짜로 대체하십시오. 일단 이 날짜에 도달하면 트리거가 실행되지 않습니다. 이 매개변수는 정수 또는 문자열 값을 지원합니다. 정수 값은 `1970년 1월 1일 00:00:00` UTC 이후의 시간(밀리초)을 표시하며, 문자열 값은 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 형식</a>형식이어야 합니다.</td>
</tr>
</tbody></table>

다음 예제는 매 2분마다 한 번씩 실행되는 트리거를 작성합니다. 트리거는 가급적 빨리 실행되며 `2019년 1월 31일 23:59:00` UT에 실행을 중지합니다. 각 트리거 이벤트에는 `name=Odin` 및 `place=Asgard` 매개변수가 있습니다.

```
ibmcloud fn trigger create interval \
  --feed /whisk.system/alarms/interval \
  --param minutes 2 \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}



## cron을 사용하여 시간 기반 스케줄에 따라 트리거 실행
{: #pkg_alarms_cron}

`/whisk.system/alarms/alarm` 피드는 지정된 빈도로 트리거 이벤트를 실행하도록 알람 서비스를 구성합니다. 시간 기반 알람을 작성하려면 다음 명령을 실행하십시오.
```
ibmcloud fn trigger create periodic --feed /whisk.system/alarms/alarm --param cron "<cron>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}
</br>

<table>
<caption><code>trigger create periodic</code> 명령 컴포넌트 이해하기</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> <code>trigger create periodic</code> 명령 컴포넌트 이해하기</th>
</thead>
<tbody>
<tr>
<td><code>periodic</code></td>
<td>작성 중인 알람 트리거의 유형입니다.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/alarm</code></td>
<td>주기적 알람 피드에 대한 알람 패키지 파일 경로입니다.</td>
</tr>
<tr>
<td><code>--param cron</code></td>
<td><code>&lt;cron&gt;</code>을 트리거를 실행할 시간대를 협정 세계시(UTC)로 표시하는 문자열로 대체하십시오. 문자열은 <a href="http://crontab.org">UNIX crontab 구문</a>을 기반으로 하며 최대 5개 필드의 시퀀스입니다. 필드는 <code>X X X X X</code> 형식으로 공백에 의해 구분됩니다. 다음 문자열은 빈도의 가변 지속 기간을 사용하는 예입니다.<ul><li><code>\* \* \* \* \*</code> - 매분 시작 시 트리거가 실행됩니다.</li><li><code>0 \* \* \* \*</code> - 매시간 시작 시 트리거가 실행됩니다.</li><li><code>0 \*/2 \* \* \*</code> - 트리거가 2시간마다 실행됩니다(예: 02:00:00, 04:00:00, ...).</li><li><code>0 9 8 \* \*</code> - 트리거가 매월 8일 9:00:00AM(UTC)에 실행됩니다.</li></ul></td>
</tr>
<tr>
<tr>
<td><code>--param timezone</code></td>
<td>(선택사항) <code>&lt;timezone&gt;</code>을 시간대를 지정하는 문자열로 대체하십시오. 트리거를 실행하는 실제 시간은 지정된 시간대에 상대적으로 수정됩니다. 시간대가 올바르지 않으면 오류가 발생합니다. [Moment Timezone 웹 사이트](http://momentjs.com/timezone/docs/#/data-loading/getting-zone-names){: external}에서 사용 가능한 모든 시간대를 확인할 수 있습니다.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>(선택사항) <code>&lt;key&gt;</code> 및 <code>&lt;value&gt;</code>를 트리거 실행 시의 트리거 매개변수로 대체하십시오.</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>(선택사항) <code>&lt;startDate&gt;</code>를 첫 번째 트리거를 실행할 날짜로 대체하십시오. 후속 실행은 분 매개변수에서 지정하는 시간 간격을 기반으로 발생합니다. 이 매개변수는 정수 또는 문자열 값을 지원합니다. 정수 값은 `1970년 1월 1일 00:00:00` UTC 이후의 시간(밀리초)을 표시하며, 문자열 값은 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 형식</a>형식이어야 합니다.</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>(선택사항) <code>&lt;stopDate&gt;</code>를 트리거가 실행을 중지할 날짜로 대체하십시오. 일단 이 날짜에 도달하면 트리거가 실행되지 않습니다. 이 매개변수는 정수 또는 문자열 값을 지원합니다. 정수 값은 `1970년 1월 1일 00:00:00` UTC 이후의 시간(밀리초)을 표시하며, 문자열 값은 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 형식</a>형식이어야 합니다.</td>
</tr>
</tbody></table>

다음 명령은 매 2분마다 한 번씩 실행되는 트리거를 작성하는 예입니다. 트리거는 `2019년 1월 1일 00:00:00` UTC 이전에는 실행을 시작하지 않으며 `2019년 1월 31일 23:59:00` UTC에는 실행을 중지합니다. 각 트리거 이벤트에는 `name=Odin` 및 `place=Asgard` 매개변수가 있습니다.

```
ibmcloud fn trigger create periodic \
  --feed /whisk.system/alarms/alarm \
  --param cron "*/2 * * * *" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param startDate "2019-01-01T00:00:00.000Z" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}




