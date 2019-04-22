---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: annotations, annotate, package, parameters, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 어노테이션
{: #openwhisk_annotations}

{{site.data.keyword.openwhisk}} 액션, 트리거, 룰 및 패키지(집합적으로 엔티티라고 함)에 어노테이션이 포함될 수 있습니다.
{: shortdesc}

어노테이션은 매개변수와 같은 엔티티에 연결됩니다. 어노테이션은 이름을 정의하는 `key`와 값을 정의하는 `value`로 구성되어 있습니다. 어노테이션은 가장 흔하게 액션 및 패키지의 문서화에 사용됩니다. {{site.data.keyword.openwhisk_short}} 카탈로그의 패키지 중 다수는 어노테이션을 전달합니다(예: 해당 액션이 제공하는 기능의 설명, 패키지 바인딩 시간에 사용할 매개변수, invoke-time 매개변수 또는 매개변수가 시크릿인지 여부). 어노테이션은 필요에 따라 작성됩니다(예: UI 통합을 허용하기 위해).

`--annotation` 또는 `-a` 플래그를 사용하여 CLI의 엔티티를 문서화할 수 있다.

## 액션 어노테이션
{: #action}

액션을 설명하는 어노테이션에는 다음이 포함됩니다.

- `description`: 액션의 간결하고 함축적인 설명입니다.
- `parameters`: 액션 실행에 필요한 액션을 설명하는 배열입니다.
- `sampleInput`: 일반 값의 입력 스키마를 표시하는 예제입니다.
- `sampleOutput`: (일반적으로 `sampleInput`에 대한) 출력 스키마를 표시하는 예제입니다.



다음 코드는 `echo` 액션에 대한 예제 어노테이션 세트이며, 이는 입력 인수를 수정하지 않고 리턴합니다. 이 액션은 입력 매개변수의 로깅에 유용합니다(예: 시퀀스 또는 룰의 일부로서).

```
ibmcloud fn action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

## 웹 액션 어노테이션
{: #annotations-specific-to-web-actions}

다음의 웹 액션 어노테이션은 대화식으로 API를 사용할 수 있도록 명시적으로 `true`로 설정되어야 합니다.

- `web-export`: 액션에 적용될 때 액션은 [웹 액션](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions)이 됩니다. 사용자가 브라우저에서 액션에 액세스할 수 있도록 액션은 인증 없이 REST 호출에서 액세스 가능하게 됩니다.
    * **참고**: 웹 액션의 소유자는 시스템에서 이의 실행 비용을 유발합니다. 다시 말하면, 액션의 소유자는 활성화 레코드도 소유합니다.
- `final`: 액션에 적용될 때 이전에 정의된 액션 매개변수는 불변이 됩니다. 매개변수는 호출 중에 제공된 매개변수로 대체될 수 없습니다.
- `raw-http`: `web-export` 어노테이션이 있는 액션에 적용되는 경우, HTTP 요청 조회 및 본문 매개변수는 예약된 특성으로서 액션에 전달됩니다.
- `web-custom-options`: 사용자 정의된 헤더의 OPTIONS 요청에 응답하기 위해 웹 액션을 사용합니다. 그렇지 않으면, [기본 CORS 응답](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#options-requests)이 적용됩니다.
- `require-whisk-auth`: 웹 액션은 적합한 인증 인증 정보를 제공하는 요청에 의해서만 호출될 수 있습니다.
    * 부울 값으로 설정된 경우, 이는 요청의 기본 인증 값이 인증되는지 여부를 제어합니다. `true` 값은 인증 정보를 인증하고 `false` 값은 인증 없이 액션을 호출합니다.
    * 정수 또는 문자열로 설정된 경우, 이 값은 요청의 `X-Require-Whisk-Auth` 헤더 값과 일치해야 합니다.
    * **참고**: 웹 액션의 소유자는 시스템에서 이의 실행 비용을 유발합니다. 다시 말하면, 액션의 소유자는 활성화 레코드도 소유합니다.

## 패키지 어노테이션
{: #package}

패키지를 설명하는 어노테이션에는 다음이 포함됩니다.

- `description`: 패키지의 간결하고 함축적인 설명입니다.
- `parameters`: 패키지로 범위가 한정된 매개변수를 설명하는 배열입니다.

## 매개변수 어노테이션
{: #parameter}

매개변수를 설명하는 어노테이션에는 다음이 포함됩니다.

- `name`: 매개변수의 이름입니다.
- `description`: 매개변수의 간결하고 함축적인 설명입니다.
- `doclink`: (OAuth 토큰에 유용한) 매개변수의 추가 문서에 대한 링크입니다.
- `required`: 필수 매개변수의 경우에는 true이며, 선택적 매개변수의 경우에는 false입니다.
- `bindTime`: 패키지가 바인딩될 때 매개변수가 지정되어 있으면 true입니다.
- `type`: 매개변수의 유형이며 `password` 또는 `array` 중 하나입니다(단, 보다 광범위하게 사용될 수 있음).

## 활성화 어노테이션
{: #activation}

사용자는 다음 어노테이션으로 활성화 레코드를 문서화할 수 있습니다.

- `path`: 활성화를 생성한 액션의 완전한 경로 이름입니다. 이 활성화가 패키지 바인딩의 액션의 결과인 경우에 경로가 상위 패키지를 참조한다는 점에 유념하십시오.
- `kind`: 실행된 액션의 유형이며, 지원 OpenWhisk 런타임 유형 중 하나입니다.
- `limits`: 이 활성화가 종속된 시간, 메모리 및 로그 한계입니다.

시퀀스 관련 활성화의 경우, 시스템은 다음 어노테이션을 생성합니다.

- `topmost`: 이는 가장 외부의 시퀀스 액션에 대해서만 존재합니다.
- `causedBy`: 이는 시퀀스에 포함된 액션에 대해서만 존재합니다.

성능 투명성을 제공하기 위해 활성화는 다음도 기록합니다.

- `waitTime`: 내부 OpenWhisk 시스템에서 대기 중일 때 사용한 시간입니다. 이는 활성화 요청을 수신하는 제어기 및 액션에 대해 호출기가 컨테이너를 프로비저닝하는 시점 간에 소요된 개략적인 시간입니다. 이 값은 현재 비-시퀀스 관련 활성화에 대해서만 존재합니다. 시퀀스의 경우, 이 정보는 `topmost` 시퀀스 활성화 레코드에서 파생될 수 있습니다.
- `initTime`: 함수 초기화 시에 사용된 시간입니다. 이 값이 존재하는 경우, 액션은 초기화를 필요로 하며 콜드 스타트를 표시합니다. 웜 활성화는 초기화를 건너뛰며, 이 경우에는 어노테이션이 생성되지 않습니다.

활성화 레코드에 나타날 수 있으므로 다음 예에서는 이러한 어노테이션을 표시합니다.

```javascript
"annotations": [
  {
    "key": "path",
    "value": "guest/echo"
  },
  {
    "key": "waitTime",
    "value": 66
  },
  {
    "key": "kind",
    "value": "nodejs:6"
  },
  {
    "key": "initTime",
    "value": 50
  },
  {
    "key": "limits",
    "value": {
      "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
  }
]
```
{: codeblock}
