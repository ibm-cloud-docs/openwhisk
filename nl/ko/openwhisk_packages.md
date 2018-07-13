---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 패키지에서 액션 구성
{: #openwhisk_packages}

{{site.data.keyword.openwhisk}}에서 패키지를 사용하여 일련의 관련된 액션을 번들화하고 이를 다른 사용자와 공유할 수 있습니다.
{: shortdesc}

패키지에는 *액션* 및 *피드*가 포함될 수 있습니다.
- 액션은 {{site.data.keyword.openwhisk_short}}에서 실행되는 코드 조각입니다. 예를 들어, {{site.data.keyword.cloudant}} 패키지에는 {{site.data.keyword.cloudant_short_notm}} 데이터베이스에서 레코드를 읽고 쓰는 액션이 포함되어 있습니다.
- 피드를 사용하여 트리거 이벤트를 실행하기 위한 외부 이벤트 소스를 구성합니다. 예를 들어, 알람 패키지에는 지정된 빈도로 트리거를 실행할 수 있는 피드가 포함되어 있습니다.

패키지를 포함하여 모든 {{site.data.keyword.openwhisk_short}} 엔티티는 *네임스페이스*에 속하며 엔티티의 완전한 이름은 `/namespaceName[/packageName]/entityName`입니다. 
자세한 정보는 [이름 지정 가이드라인](./openwhisk_reference.html#openwhisk_entities)을 참조하십시오.

다음 절에서는 패키지를 찾고 그 안에서 트리거 및 피드를 사용하는 방법에 대해 설명합니다. 또한 자체 패키지를 카탈로그에 컨트리뷰션하는 데 관심이 있는 경우에는 패키지 작성 및 공유에 대한 절을 읽으십시오.

## 패키지 찾아보기
{: #browse-packages}

여러 패키지가 {{site.data.keyword.openwhisk_short}}에 등록되어 있습니다. 네임스페이스에서 패키지의 목록을 가져오고, 패키지의 엔티티를 나열하며 패키지의 개별 엔티티에 대한 설명을 가져올 수 있습니다.

1. `/whisk.system` 네임스페이스에서 패키지의 목록을 가져오십시오.
  ```
  ibmcloud wsk package list /whisk.system
  ```
  {: pre}

  패키지 목록 출력:
  ```
  packages
  /whisk.system/cloudant                                                 shared
  /whisk.system/alarms                                                   shared
  /whisk.system/watson                                                   shared
  /whisk.system/websocket                                                shared
  /whisk.system/weather                                                  shared
  /whisk.system/system                                                   shared
  /whisk.system/utils                                                    shared
  /whisk.system/slack                                                    shared
  /whisk.system/samples                                                  shared
  /whisk.system/github                                                   shared
  /whisk.system/pushnotifications                                        shared
  ```
  {: screen}

2. `/whisk.system/cloudant` 패키지에서 엔티티의 목록을 가져오십시오.
  ```
  ibmcloud wsk package get --summary /whisk.system/cloudant
  ```
  {: pre}

  출력 예:
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.Bluemix_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  이 출력은 {{site.data.keyword.cloudant_short_notm}} 패키지가 두 개의 액션(`read` 및 `write`)과 하나의 트리거 피드(`changes`)를 제공함을 보여줍니다. `changes` 피드는 문서가 지정된 {{site.data.keyword.cloudant_short_notm}} 데이터베이스에 추가될 때 트리거가 실행되도록 합니다.

  또한 {{site.data.keyword.cloudant_short_notm}} 패키지는 `username`, `password`, `host` 및 `port` 매개변수도 정의합니다. 이러한 매개변수는 액션 및 피드가 유의미하도록 지정해야 합니다. 매개변수는 액션이 특정 {{site.data.keyword.cloudant_short_notm}} 계정에서 작동하도록 합니다. 예를 들어, 다음과 같습니다.

3. `/whisk.system/cloudant/read` 액션에 대한 설명을 가져오십시오.
  ```
  ibmcloud wsk action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  출력 예:
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```
  {: screen}

  이 출력은 검색할 데이터베이스 및 문서 ID를 포함하여 {{site.data.keyword.cloudant_short_notm}} `read` 액션에서 세 개의 매개변수가 필요함을 표시합니다.

## 패키지의 액션 호출
{: #openwhisk_package_invoke}

다른 액션에서와 같이 패키지에서 액션을 호출할 수 있습니다. 다음 몇 가지 단계는 다른 매개변수를 사용하여 `/whisk.system/samples` 패키지에서 `greeting` 액션을 호출하는 방법을 보여줍니다.

1. `/whisk.system/samples/greeting` 액션에 대한 설명을 가져오십시오.
  ```
  ibmcloud wsk action get --summary /whisk.system/samples/greeting
  ```
  {: pre}

  출력 예:
  ```
  action /whisk.system/samples/greeting: Print a friendly greeting
     (params: name place)
  ```
  {: screen}

  `greeting` 액션에 두 개의 매개변수(`name` 및 `place`)가 사용된다는 점에 유념하십시오.

2. 매개변수 없이 액션을 호출하십시오.
  ```
  ibmcloud wsk action invoke --blocking --result /whisk.system/samples/greeting
  ```
  {: pre}

  출력 예:
  ```
  {
      "payload": "Hello, stranger from somewhere!"
  }
  ```
  {: screen}

  매개변수가 지정되지 않았으므로 출력은 일반 메시지입니다.

3. 매개변수를 사용하여 액션을 호출하십시오.
  ```
  ibmcloud wsk action invoke --blocking --result /whisk.system/samples/greeting --param name Mork --param place Ork
  ```
  {: pre}

  출력 예:
  ```
  {
      "payload": "Hello, Mork from Ork!"
  }
  ```
  {: screen}

  출력이 액션에 전달된 `name` 및 `place` 매개변수를 사용한다는 점에 유념하십시오.

## 패키지 바인딩 작성 및 사용
{: #openwhisk_package_bind}

패키지의 엔티티를 직접 사용할 수 있지만, 사용자 자신이 동일한 매개변수를 매번 액션에 전달한다는 사실을 발견할 수 있습니다. 패키지를 바인딩하고 패키지의 액션에 의해 상속된 기본 매개변수를 지정하여 프로세스를 단순화할 수 있습니다.

예를 들어, `/whisk.system/cloudant` 패키지에서 기본 `username`, `password` 및 `dbname` 값을 패키지 바인딩에서 설정하면 이러한 값이 자동으로 패키지 내의 모든 액션에 전달됩니다.

다음의 단순한 예제에서 사용자는 `/whisk.system/samples` 패키지에 바인드합니다.

1. `/whisk.system/samples` 패키지에 바인드하고 기본 `place` 매개변수값을 설정하십시오.
  ```
  ibmcloud wsk package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  출력 예:
  ```
  ok: created binding valhallaSamples
  ```
  {: screen}

2. 패키지 바인딩의 설명을 가져오십시오.
  ```
  ibmcloud wsk package get --summary valhallaSamples
  ```
  {: pre}

  출력 예:
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Returns a friendly greeting
   action /myNamespace/valhallaSamples/wordCount: Count words in a string
   action /myNamespace/valhallaSamples/helloWorld: Demonstrates logging facilities
   action /myNamespace/valhallaSamples/curl: Curl a host url
  ```
  {: screen}

  `/whisk.system/samples` 패키지의 모든 액션을 `valhallaSamples` 패키지 바인딩에서 사용할 수 있다는 점에 유념하십시오.

3. 패키지 바인딩에서 액션을 호출하십시오.
  ```
  ibmcloud wsk action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  출력 예:
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  결과에서 `valhallaSamples` 패키지 바인딩을 작성할 때 설정된 `place` 매개변수를 액션이 상속한다는 점에 유념하십시오.

4. 액션을 호출하고 기본 매개변수값을 겹쳐쓰십시오.
  ```
  ibmcloud wsk action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  출력 예:
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  액션 호출에서 지정된 `place` 매개변수값이 `valhallaSamples` 패키지 바인딩에 설정된 기본값을 겹쳐쓴다는 점에 유념하십시오.

## 트리거 피드 작성 및 사용
{: #openwhisk_package_trigger}

피드는 {{site.data.keyword.openwhisk_short}} 트리거에 대해 이러한 이벤트를 실행하도록 외부 이벤트 소스를 구성하는 편리한 방법을 제공합니다. 이 예제는 알람 패키지의 피드를 사용하여 매초 트리거를 실행하는 방법과 룰을 사용하여 매초 액션을 호출하는 방법을 보여줍니다.

1. `/whisk.system/alarms` 패키지에서 피드의 설명을 가져오십시오.
  ```
  ibmcloud wsk package get --summary /whisk.system/alarms
  ```
  {: pre}

  출력 예:
  ```
  package /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
  ```
  {: screen}

  ```
  ibmcloud wsk action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  출력 예:
  ```
  action /whisk.system/alarms/alarm: Fire trigger when alarm occurs
     (params: cron trigger_payload)
  ```
  {: screen}

  `/whisk.system/alarms/alarm` 피드는 두 개의 매개변수를 취합니다.
  - `cron`: 트리거를 실행할 시기의 crontab 스펙입니다.
  - `trigger_payload`: 각 트리거 이벤트에서 설정할 페이로드 매개변수값입니다.

2. 8초마다 실행되는 트리거를 작성하십시오.
  ```
  ibmcloud wsk trigger create everyEightSeconds --feed /whisk.system/alarms/alarm -p cron "*/8 * * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  출력 예:
  ```
  ok: created trigger feed everyEightSeconds
  ```
  {: screen}

3. 다음 액션 코드를 사용하여 **hello.js** 파일을 작성하십시오.
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. 액션이 있는지 확인하십시오.
  ```
  ibmcloud wsk action update hello hello.js
  ```
  {: pre}

5. **everyEightSeconds** 트리거가 실행될 때마다 `hello` 액션을 호출하는 룰을 작성하십시오.
  ```
  ibmcloud wsk rule create myRule everyEightSeconds hello
  ```
  {: pre}

  출력 예:
  ```
  ok: created rule myRule
  ```
  {: screen}

6. 활성화 로그에 대한 폴링에 의해 액션이 호출되는지 확인하십시오.
  ```
  ibmcloud wsk activation poll
  ```
  {: pre}

  트리거, 룰 및 액션에 대해 활성화가 8초마다 관찰됨을 볼 수 있습니다. 액션은 각 호출에 대해 `{"name":"Mork", "place":"Ork"}` 매개변수를 수신합니다.

## 패키지 작성
{: #openwhisk_packages_create}

패키지를 사용하여 관련 액션 및 피드의 세트를 구성할 수 있습니다.
또한 이는 패키지의 모든 엔티티 간에 매개변수가 공유되도록 허용합니다.

내부에 단순 액션으로 사용자 정의 패키지를 작성하려면 다음 예제를 시도하십시오.

1. **custom**이라고 하는 패키지를 작성하십시오.
  ```
  ibmcloud wsk package create custom
  ```
  {: pre}

  출력 예:
  ```
  ok: created package custom
  ```
  {: screen}

2. 패키지의 요약을 가져오십시오.
  ```
  ibmcloud wsk package get --summary custom
  ```
  {: pre}

  출력 예:
  ```
  package /myNamespace/custom
  ```
  {: screen}

  패키지가 비어 있다는 점에 유념하십시오.

3. 다음 액션 코드가 포함된 `identity.js` 파일을 작성하십시오. 이 액션은 모든 입력 매개변수를 리턴합니다.
  ```javascript
  function main(args) { return args; }
  ```
  {: codeblock}

4. ``custom** 패키지에서 **identity** 액션을 작성하십시오.
  ```
  ibmcloud wsk action create custom/identity identity.js
  ```
  {: pre}

  출력 예:
  ```
  ok: created action custom/identity
  ```
  {: screen}

  패키지의 액션을 작성할 때 액션 이름 앞에 패키지 이름을 접두부로 지정해야 합니다. 패키지 중첩은 허용되지 않습니다. 패키지는 액션만 포함할 수 있으며 다른 패키지는 포함할 수 없습니다.

5. 패키지의 요약을 다시 가져오십시오.
  ```
  ibmcloud wsk package get --summary custom
  ```
  {: pre}

  출력 예:
  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```
  {: screen}

  이제 네임스페이스에서 **custom/identity** 액션을 볼 수 있습니다.

6. 패키지에서 액션을 호출하십시오.
  ```
  ibmcloud wsk action invoke --blocking --result custom/identity
  ```
  {: pre}

  출력 예:
  ```
  {}
  ```
  {: screen}

패키지의 모든 액션에 의해 상속되는 package-level 매개변수를 설정하여 패키지의 모든 엔티티에 기본 매개변수를 설정할 수 있습니다. 이 상속의 작동 방법을 알아보려면 다음 예제를 시도하십시오.

1. **custom** 패키지를 두 개의 매개변수 `city` 및 `country`로 업데이트하십시오.
  ```
  ibmcloud wsk package update custom --param city Austin --param country USA
  ```
  {: pre}

  출력 예:
  ```
  ok: updated package custom
  ```
  {: screen}

2. **custom** 패키지 및 **identidy** 액션에 매개변수를 표시하고 패키지의 **identity** 액션이 패키지에서 매개변수를 상속하는 방법을 확인하십시오.
  ```
  ibmcloud wsk package get custom parameters
  ```
  {: pre}

  출력 예:
  ```
  ok: got package custom, displaying field parameters

  [
      {
          "key": "city",
          "value": "Austin"
      },
      {
          "key": "country",
          "value": "USA"
      }
  ]
  ```
  {: screen}

  ```
  ibmcloud wsk action get custom/identity parameters
  ```
  {: pre}

  출력 예:
  ```
  ok: got action custom/identity, displaying field parameters

  [
      {
          "key": "city",
          "value": "Austin"
      },
      {
          "key": "country",
          "value": "USA"
      }
  ]
  ```
  {: screen}

3. 액션이 실제로 매개변수를 상속하는지 확인하려면 매개변수 없이 **identity** 액션을 호출하십시오.
  ```
  ibmcloud wsk action invoke --blocking --result custom/identity
  ```
  {: pre}

  출력 예:
  ```
  {
      "city": "Austin",
      "country": "USA"
  }
  ```
  {: screen}

4. 일부 매개변수를 사용하여 **identity** 액션을 호출하십시오. 호출 매개변수가 패키지 매개변수와 병합되며, 호출 매개변수가 패키지 매개변수를 대체합니다.
  ```
  ibmcloud wsk action invoke --blocking --result custom/identity --param city Dallas --param state Texas
  ```
  {: pre}

  출력 예:
  ```
  {
      "city": "Dallas",
      "country": "USA",
      "state": "Texas"
  }
  ```
  {: screen}

## 패키지 공유
{: #openwhisk_packages_share}

패키지를 구성하는 액션 및 피드가 디버그되고 테스트된 후에는 패키지를 모든 {{site.data.keyword.openwhisk_short}} 사용자와 공유할 수 있습니다. 패키지를 공유하면 사용자가 패키지를 바인드하고 패키지의 액션을 호출하며 {{site.data.keyword.openwhisk_short}} 룰 및 시퀀스 액션을 작성할 수 있습니다.

1. 모든 사용자와 패키지를 공유하십시오.
  ```
  ibmcloud wsk package update custom --shared yes
  ```
  {: pre}

  출력 예:
  ```
  ok: updated package custom
  ```
  {: screen}

2. 패키지의 `publish` 특성을 표시하여 현재 true인지 확인하십시오.
  ```
  ibmcloud wsk package get custom publish
  ```
  {: pre}

  출력 예:
  ```
  ok: got package custom, displaying field publish

    true
  ```
  {: screen}

다른 사용자는 이제 패키지에 바인딩 또는 패키지의 액션 직접 호출을 포함하여 **custom** 패키지를 사용할 수 있습니다. 다른 사용자는 패키지의 완전한 이름을 알아야 이를 바인드하거나 패키지의 액션을 호출할 수 있습니다. 공유 패키지 내 액션 및 피드는 _공용_입니다. 패키지가 개인용이면 모든 해당 컨텐츠 또한 개인용입니다.

1. 패키지 및 액션의 완전한 이름을 표시하려면 패키지의 설명을 가져오십시오.
  ```
  ibmcloud wsk package get --summary custom
  ```
  {: pre}

  출력 예:
  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```
  {: screen}

  이전 예제에서 사용자는 **myNamespace** 네임스페이스에 대해 작업하며 이 네임스페이스는 완전한 이름으로 표시됩니다.
