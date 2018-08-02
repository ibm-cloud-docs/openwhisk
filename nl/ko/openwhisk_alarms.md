---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 알람
{: #openwhisk_catalog_alarm}

`/whisk.system/alarms` 패키지를 사용하여 지정된 빈도로 트리거를 실행할 수 있습니다. 알람은 반복 작업이나 태스크(예: 매시간 시스템 백업 액션 호출)를 설정하는 데 유용합니다.
{: shortdesc}

패키지에는 다음 피드가 포함됩니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
|`/whisk.system/alarms` |패키지 | - |알람 및 주기적 유틸리티입니다. |
|`/whisk.system/alarms/interval` |피드 |minutes, trigger_payload, startDate, stopDate | 간격 기반 스케줄에 따라 트리거 이벤트를 실행합니다. |
|`/whisk.system/alarms/once` |피드 | date, trigger_payload, deleteAfterFire | 특정 날짜에 한 번만 트리거 이벤트를 실행합니다. |
|`/whisk.system/alarms/alarm` |피드 |cron, trigger_payload, startDate, stopDate | cron을 사용하여 시간 기반 스케줄에 따라 트리거 이벤트를 실행합니다. |


## 간격 기반 스케줄에 따라 주기적으로 트리거 이벤트 실행
{: #openwhisk_catalog_alarm_fire}

`/whisk.system/alarms/interval` 피드는 간격 기반 스케줄에 따라 트리거 이벤트를 실행하도록 알람 서비스를 구성합니다. 매개변수는 다음과 같습니다.

- `minutes`(*필수*): 트리거 실행 간에 간격(분)의 길이를 표시하는 정수입니다.
- `trigger_payload`(*선택사항*): 이 매개변수의 값은 트리거가 실행될 때마다 트리거의 컨텐츠가 됩니다.
- `startDate`(*선택사항*): 첫 번째 트리거가 실행되는 날짜입니다. 후속 실행은 `minutes` 매개변수에서 지정하는 간격 길이를 기반으로 발생합니다.
- `stopDate`(*선택사항*): 트리거 실행이 중지되는 날짜입니다. 일단 이 날짜에 도달하면 트리거가 더 이상 실행되지 않습니다.

  **참고**: `startDate` 및 `stopDate` 매개변수는 정수 또는 문자열 값을 지원합니다. 정수 값은 1970년 1월 1일 00:00:00 UTC 이후의 시간(밀리초)을 표시하며, 문자열 값은 ISO 8601 형식이어야 합니다(http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15).

다음 예제는 매 2분마다 한 번씩 실행되는 트리거를 작성합니다. 트리거는 가급적 빨리 실행되며 2019년 1월 31일 23:59:00 UTC에 실행을 중지합니다.

  ```
  ibmcloud fn trigger create interval \
    --feed /whisk.system/alarms/interval \
    --param minutes 2 \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

생성된 각 이벤트에는 `trigger_payload` 값으로 지정된 특성인 매개변수가 포함됩니다. 이 경우에 각 트리거 이벤트에는 `name=Odin` 및 `place=Asgard` 매개변수가 있습니다.

## 트리거 이벤트를 한 번만 실행

`/whisk.system/alarms/once` 피드는 지정된 날짜에 트리거 이벤트를 실행하도록 알람 서비스를 구성합니다. 매개변수는 다음과 같습니다.

- `date`(*필수*): 트리거가 실행되는 날짜입니다. 트리거는 주어진 시간에 한 번만 실행됩니다.

  **참고**: `date` 매개변수는 정수 또는 문자열 값을 지원합니다. 정수 값은 1970년 1월 1일 00:00:00 UTC 이후의 시간(밀리초)을 표시하며 문자열은 ISO 8601 형식이어야 합니다(http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15).

- `trigger_payload`(*선택사항*): 이 매개변수의 값은 트리거가 실행될 때 트리거의 컨텐츠가 됩니다.

- `deleteAfterFire`(*선택사항*, 기본값:false): 이 매개변수의 값은 트리거 실행 후 트리거 및 잠재적으로 이와 연관된 모든 룰을 삭제할지 여부를 결정합니다.
  - `false`: 트리거 실행 후 액션이 수행되지 않습니다.
  - `true`: 트리거는 실행된 후 삭제됩니다.
  - `rules`: 트리거 및 이와 연관된 모든 룰은 트리거가 실행된 후 삭제됩니다.

다음은 2019년 12월 25일 12:30:00 UTC에 한 번만 실행되는 트리거를 작성하는 예제입니다. 트리거가 실행된 후 트리거 및 이와 연관된 모든 룰이 삭제됩니다.

  ```
  ibmcloud fn trigger create fireOnce \
    --feed /whisk.system/alarms/once \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param date "2019-12-25T12:30:00.000Z" \
    --param deleteAfterFire "rules"
  ```
  {: pre}

## cron을 사용하여 시간 기반 스케줄에 따라 트리거 실행

`/whisk.system/alarms/alarm` 피드는 지정된 빈도로 트리거 이벤트를 실행하도록 알람 서비스를 구성합니다. 매개변수는 다음과 같습니다.

- `cron`(*필수*): 협정 세계시(UTC)로 트리거 실행 시점을 표시하는 UNIX crontab 구문 기반의 문자열입니다. 문자열은 공백으로 분리된 5개 필드의 시퀀스입니다(`X X X X X`).
자세한 정보는 http://crontab.org를 참조하십시오. 다음 문자열은 빈도의 가변 기간을 사용하는 예제입니다.

  - `* * * * *`: 트리거가 매분 0초에 실행됩니다.
  - `0 * * * *`: 트리거가 매시 0분에 실행됩니다.
  - `0 */2 * * *`: 트리거가 2시간마다 실행됩니다(즉, 02:00:00, 04:00:00, ...).
  - `0 9 8 * *`: 트리거가 매월 8일 9:00:00AM(UTC)에 실행됩니다.

  **참고**: `cron` 매개변수는 5개 필드만 지원합니다.

- `trigger_payload`(*선택사항*): 이 매개변수의 값은 트리거가 실행될 때마다 트리거의 컨텐츠가 됩니다.

- `startDate`(*선택사항*): 트리거 실행이 시작되는 날짜입니다. 트리거는 cron 매개변수에서 지정하는 스케줄에 따라 실행됩니다.

- `stopDate`(*선택사항*): 트리거 실행이 중지되는 날짜입니다. 일단 이 날짜에 도달하면 트리거가 더 이상 실행되지 않습니다.

  **참고**: `startDate` 및 `stopDate` 매개변수는 정수 또는 문자열 값을 지원합니다. 정수 값은 1970년 1월 1일 00:00:00 UTC 이후의 시간(밀리초)을 표시하며, 문자열 값은 ISO 8601 형식이어야 합니다(http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15).

다음은 트리거 이벤트에서 `name` 및 `place` 값으로 매 2분마다 한 번씩 실행되는 트리거를 작성하는 예제입니다. 트리거는 2019년 1월 1일 00:00:00 UTC 이전에는 실행을 시작하지 않으며 2019년 1월 31일 23:59:00 UTC에는 실행을 중지합니다.

  ```
  ibmcloud fn trigger create periodic \
    --feed /whisk.system/alarms/alarm \
    --param cron "*/2 * * * *" \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param startDate "2019-01-01T00:00:00.000Z" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

 **참고**: `maxTriggers` 매개변수는 더 이상 사용되지 않으며 곧 제거될 예정입니다. 트리거를 중지하려면 `stopDate` 매개변수를 사용하십시오.
