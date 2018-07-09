---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 패키지 작성 및 사용
{: #openwhisk_packages}

{{site.data.keyword.openwhisk}}에서는 패키지를 사용하여 관련 조치 세트를 번들링하고 이를 다른 사용자와 공유할 수 있습니다.
{: shortdesc}

패키지에는 *조치* 및 *피드*가 포함될 수 있습니다. 
- 조치는 {{site.data.keyword.openwhisk_short}}에서 실행되는 코드 조각입니다. 예를 들어, Cloudant 패키지에는 Cloudant 데이터베이스에 대해 레코드를 읽고 쓰는 조치가 포함되어 있습니다. 
- 피드를 사용하여 트리거 이벤트를 실행하기 위한 외부 이벤트 소스를 구성할 수 있습니다. 예를 들어, 알람 패키지에는 지정된 빈도로 트리거를 실행할 수 있는 피드가 포함되어 있습니다. 

패키지를 포함하여 모든 {{site.data.keyword.openwhisk_short}} 엔티티는 *네임스페이스*에 속하며 엔티티의 완전한 이름은 `/namespaceName[/packageName]/entityName`입니다. 
자세한 정보는 [이름 지정 가이드라인](./openwhisk_reference.html#openwhisk_entities)을 참조하십시오. 

다음 절에서는 패키지를 찾아보고 그 내부의 트리거 및 피드를 사용하는 방법에 대해 설명합니다. 또한 자체 패키지를 카탈로그에 컨트리뷰션하는 데 관심이 있는 경우에는 패키지 작성 및 공유에 대한 절을 읽으십시오. 

## 패키지 찾아보기
{: #browse-packages}

여러 패키지가 {{site.data.keyword.openwhisk_short}}에 등록되어 있습니다. 네임스페이스에서 패키지의 목록을 가져오고, 패키지의 엔티티를 나열하며 패키지의 개별 엔티티에 대한 설명을 가져올 수 있습니다. 

1. `/whisk.system` 네임스페이스에서 패키지의 목록을 가져오십시오. 

  ```
  wsk package list /whisk.system
  ```
  {: pre}
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

2. `/whisk.system/cloudant` 패키지에서 엔티티의 목록을 가져오십시오. 

  ```
  wsk package get --summary /whisk.system/cloudant
  ```
  {: pre}
  ```
  package /whisk.system/cloudant: Cloudant database service
     (params: {{site.data.keyword.Bluemix_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```

  이 출력은 Cloudant 패키지가 두 개의 조치(`read` 및 `write`)와 하나의 트리거 피드(`changes`)를 제공함을 보여줍니다. 
`changes` 피드는 문서가 지정된 Cloudant 데이터베이스에 추가될 때 트리거가 실행되도록 합니다. 

  또한 Cloudant 패키지는 `username`, `password`, `host` 및 `port` 매개변수도 정의합니다. 이러한 매개변수는 의미가 있도록 조치 및 피드에 대해 지정되어야 합니다. 매개변수는 조치가 특정 Cloudant 계정에서 작동하도록 허용합니다. 예를 들어, 다음과 같습니다. 

3. `/whisk.system/cloudant/read` 조치의 설명을 가져오십시오. 
  ```
  wsk action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```

  이 출력은 검색할 데이터베이스 및 문서 ID를 포함하여 `read` 조치에서 세 개의 매개변수가 필요함을 표시합니다. 


## 패키지의 조치 호출
{: #openwhisk_package_invoke}

기타 조치에서와 같이 패키지의 조치를 호출할 수 있습니다. 다음의 일부 단계는 서로 다른 매개변수로 `/whisk.system/samples` 패키지의 `greeting` 조치를 호출하는 방법을 보여줍니다. 

1. `/whisk.system/samples/greeting` 조치의 설명을 가져오십시오. 
  ```
  wsk action get --summary /whisk.system/samples/greeting
  ```
  {: pre}
  
  ```
  action /whisk.system/samples/greeting: Print a friendly greeting
     (params: name place)
  ```

  `greeting` 조치가 두 개의 매개변수 `name` 및 `place`를 취한다는 점에 유념하십시오. 

2. 매개변수 없이 이 조치를 호출하십시오. 
  ```
  wsk action invoke --blocking --result /whisk.system/samples/greeting
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, stranger from somewhere!"
  }
  ```

  매개변수가 지정되지 않았으므로 출력은 일반 메시지입니다. 

3. 매개변수를 사용하여 조치를 호출하십시오. 
  ```
  wsk action invoke --blocking --result /whisk.system/samples/greeting --param name Mork --param place Ork
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Mork from Ork!"
  }
  ```

  출력이 조치에 전달된 `name` 및 `place` 매개변수를 사용한다는 점에 유념하십시오. 


## 패키지 바인딩 작성 및 사용
{: #openwhisk_package_bind}

패키지의 엔티티를 직접 사용할 수 있지만, 사용자 자신이 동일한 매개변수를 매번 조치에 전달한다는 사실을 발견할 수 있습니다. 패키지를 바인딩하고 패키지의 조치에 의해 상속된 기본 매개변수를 지정하여 프로세스를 단순화할 수 있습니다. 

예를 들어, `/whisk.system/cloudant` 패키지에서 사용자는 패키지 바인딩에 `username`, `password` 및 `dbname` 값을 설정할 수 있으며 이러한 값은 패키지의 조치에 자동으로 전달됩니다. 

다음의 단순한 예제에서 사용자는 `/whisk.system/samples` 패키지에 바인드합니다. 

1. `/whisk.system/samples` 패키지에 바인드하고 기본 `place` 매개변수값을 설정하십시오. 

  ```
  wsk package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}
  ```
  ok: created binding valhallaSamples
  ```

2. 패키지 바인딩의 설명을 가져오십시오. 

  ```
  wsk package get --summary valhallaSamples
  ```
  {: pre}
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Returns a friendly greeting
   action /myNamespace/valhallaSamples/wordCount: Count words in a string
   action /myNamespace/valhallaSamples/helloWorld: Demonstrates logging facilities
   action /myNamespace/valhallaSamples/curl: Curl a host url
  ```

  `/whisk.system/samples` 패키지의 모든 조치를 `valhallaSamples` 패키지 바인딩에서 사용할 수 있다는 점에 유념하십시오. 

3. 패키지 바인딩의 조치를 호출하십시오. 

  ```
  wsk action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```

  결과에서 `valhallaSamples` 패키지 바인딩을 작성할 때 설정된 `place` 매개변수를 조치가 상속한다는 점에 유념하십시오. 

4. 조치를 호출하고 기본 매개변수 값을 겹쳐쓰십시오. 
  ```
  wsk action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```

  조치 호출에서 지정된 `place` 매개변수값이 `valhallaSamples` 패키지 바인딩에 설정된 기본값을 겹쳐쓴다는 점에 유념하십시오. 


## 트리거 피드 작성 및 사용
{: #openwhisk_package_trigger}

피드는 {{site.data.keyword.openwhisk_short}} 트리거에 대해 이러한 이벤트를 실행하도록 외부 이벤트 소스를 구성하는 편리한 방법입니다. 이 예제는 알람 패키지의 피드를 사용하여 매초마다 트리거를 실행하는 방법과 규칙을 사용하여 매초마다 조치를 호출하는 방법을 보여줍니다. 

1. `/whisk.system/alarms` 패키지에서 피드의 설명을 가져오십시오. 
  ```
  wsk package get --summary /whisk.system/alarms
  ```
  {: pre}

  ```
  package /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
  ```

  ```
  wsk action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  ```
  action /whisk.system/alarms/alarm: Fire Trigger when alarm occurs
     (params: cron trigger_payload)
  ```

  `/whisk.system/alarms/alarm` 피드는 두 개의 매개변수를 취합니다. 
  - `cron`: 트리거 실행 시점의 crontab 스펙입니다. 
  - `trigger_payload`: 각 트리거 이벤트에서 설정할 페이로드 매개변수값입니다. 

2. 8초마다 실행되는 트리거를 작성하십시오. 
  ```
  wsk trigger create everyEightSeconds --feed /whisk.system/alarms/alarm -p cron "*/8 * * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  ```
  ok: created trigger feed everyEightSeconds
  ```

3. 다음과 같은 조치 코드로 'hello.js' 파일을 작성하십시오. 
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. 조치가 존재하는지 확인하십시오. 
  ```
  wsk action update hello hello.js
  ```
  {: pre}

5. `everyEightSeconds` 트리거가 실행될 때마다 `hello` 조치를 호출하는 규칙을 작성하십시오. 
  ```
  wsk rule create myRule everyEightSeconds hello
  ```
  {: pre}
  ```
  ok: created rule myRule
  ```

6. 활성화 로그에 대해 폴링하여 조치가 호출되는지 확인하십시오. 

  ```
  wsk activation poll
  ```
  {: pre}

  트리거, 규칙 및 조치에 대해 활성화가 8초마다 관찰됨을 볼 수 있습니다. 조치는 호출 시마다 `{"name":"Mork", "place":"Ork"}` 매개변수를 수신합니다. 


## 패키지 작성
{: #openwhisk_packages_create}

패키지를 사용하여 관련 조치 및 피드의 세트를 구성할 수 있습니다. 또한 이는 패키지의 모든 엔티티 간에 매개변수가 공유되도록 허용합니다.

내부의 단순 조치로 사용자 정의 패키지를 작성하려면 다음 예제를 시도하십시오. 

1. "custom"이라고 하는 패키지를 작성하십시오. 
  ```
  wsk package create custom
  ```
  {: pre}

  ```
  ok: created package custom
  ```

2. 패키지의 요약을 가져오십시오. 
  ```
  wsk package get --summary custom
  ```
  {: pre}

  ```
  package /myNamespace/custom
  ```

  패키지가 비어 있다는 점에 유념하십시오. 

3. 다음 조치 코드가 포함된 `identity.js`라고 하는 파일을 작성하십시오. 이 조치는 모든 입력 매개변수를 리턴합니다. 
  ```javascript
  function main(args) { return args; }
  ```
  {: codeblock}

4. `custom` 패키지에서 `identity` 조치를 작성하십시오. 
  ```
  wsk action create custom/identity identity.js
  ```
  {: pre}
  
  ```
  ok: created action custom/identity
  ```

  패키지의 조치 작성에서는 조치 이름 앞에 패키지 이름을 접두부로 지정해야 합니다. 패키지 중첩은 허용되지 않습니다. 패키지에는 조치만 포함될 수 있으며 다른 패키지는 포함될 수 없습니다. 

5. 패키지의 요약을 다시 가져오십시오. 
  ```
  wsk package get --summary custom
  ```
  {: pre}

  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```

  이제 네임스페이스의 `custom/identity` 조치를 볼 수 있습니다. 

6. 패키지의 조치를 호출하십시오. 
  ```
  wsk action invoke --blocking --result custom/identity
  ```
  {: pre}

  ```json
  {}
  ```


패키지의 모든 조치에 의해 상속되는 package-level 매개변수를 설정하여 패키지의 모든 엔티티에 대해 기본 매개변수를 설정할 수 있습니다. 이 상속의 작동 방법을 알아보려면 다음 예제를 시도하십시오. 

1. `custom` 패키지를 두 개의 매개변수 `city` 및 `country`로 업데이트하십시오. 
  ```
  wsk package update custom --param city Austin --param country USA
  ```
  {: pre}

  ```
  ok: updated package custom
  ```

2. 조치 및 패키지의 매개변수를 표시하고 패키지의 `identity` 조치가 패키지에서 매개변수를 상속하는 방법을 확인하십시오. 
  ```
  wsk package get custom parameters
  ```
  {: pre}

  ```
  ok: got package custom, displaying field parameters
  ```

  ```json
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

  ```
  wsk action get custom/identity parameters
  ```
  {: pre}

  ```
  ok: got action custom/identity, , displaying field parameters
  ```

  ```json
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

3. 매개변수 없이 ID 조치를 호출하여 조치가 실제로 매개변수를 상속하는지 확인하십시오. 

  ```
  wsk action invoke --blocking --result custom/identity
  ```
  {: pre}
  ```json
  {
      "city": "Austin",
      "country": "USA"
  }
  ```

4. 일부 매개변수로 ID 조치를 호출하십시오. 호출 매개변수가 패키지 매개변수와 병합되며, 호출 매개변수가 패키지 매개변수를 대체합니다. 
  ```
  wsk action invoke --blocking --result custom/identity --param city Dallas --param state Texas
  ```
  {: pre}

  ```json
  {
      "city": "Dallas",
      "country": "USA",
      "state": "Texas"
  }
  ```

## 패키지 공유
{: #openwhisk_packages_share}

패키지를 구성하는 조치 및 피드가 디버그되고 테스트된 후에는 패키지를 모든 {{site.data.keyword.openwhisk_short}} 사용자와 공유할 수 있습니다. 패키지를 공유하면 사용자가 패키지를 바인드하고 패키지의 조치를 호출하며 {{site.data.keyword.openwhisk_short}} 규칙 및 시퀀스 조치를 작성할 수 있습니다. 

1. 모든 사용자와 패키지를 공유하십시오. 
  ```
  wsk package update custom --shared yes
  ```
  {: pre}

  ```
  ok: updated package custom
  ```

2. 패키지의 `publish` 특성을 표시하여 현재 true인지 확인하십시오. 
  ```
  wsk package get custom publish
  ```
  {: pre}

  ```
  ok: got package custom, displaying field publish
  ```

  ```json
  true
  ```


기타 사용자는 이제 패키지에 바인딩 또는 패키지의 조치에 대한 직접 호출 등을 포함하여 `사용자 정의` 패키지 사용을 수행할 수 있습니다. 기타 사용자는 패키지의 완전한 이름을 알아야 이를 바인드하거나 패키지의 조치를 호출할 수 있습니다. 공유 패키지 내의 조치 및 피드는 _공용_입니다. 패키지가 개인용이면 모든 해당 컨텐츠 또한 개인용입니다. 

1. 패키지의 설명을 가져와서 조치 및 패키지의 완전한 이름을 표시하십시오. 

  ```
  wsk package get --summary custom
  ```
  {: pre}
  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```

  이전 예제에서 사용자는 `myNamespace` 네임스페이스에 대해 작업하며 이 네임스페이스는 완전한 이름으로 표시됩니다. 
