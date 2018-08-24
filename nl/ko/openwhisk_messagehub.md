---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Message Hub 이벤트 소스
{: #openwhisk_catalog_message_hub}

피드를 사용하여 메시지가 {{site.data.keyword.messagehub_full}} 인스턴스에 게시될 때 반응하는 트리거를 작성할 수 있습니다. {{site.data.keyword.Bluemix}}를 사용하거나 사용하지 않고 {{site.data.keyword.messagehub}} 트리거를 작성하는 방법을 알아보고 메시지를 청취하고 일괄처리된 메시지를 처리하십시오.
{: shortdesc}

## {{site.data.keyword.messagehub}} 패키지 

`/messaging/messageHubProduce` 액션은 더 이상 사용되지 않으며 나중에 제거됩니다. 최적의 성능을 유지하려면 데이터가 Message Hub/Kafka에 생성될 때 `/messaging/messageHubProduce` 액션의 사용을 마이그레이션하여 지속적 연결을 사용하십시오.
{: tip}

이 패키지는 기본 고성능 Kafka API를 사용하여 메시지를 공개하고 이용하기 위한 [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub) 인스턴스와의 통신을 가능하게 합니다. {{site.data.keyword.messagehub}} 패키지, 설정 방법 및 메시지 생성 방법에 대한 자세한 정보는 [{{site.data.keyword.messagehub}} 패키지](./messagehub_actions.html) 주제를 참조하십시오.

## {{site.data.keyword.messagehub}} 인스턴스를 청취하는 트리거 작성
{: #create_message_hub_trigger}

메시지가 {{site.data.keyword.messagehub}} 인스턴스에 게시될 때 반응하는 트리거를 작성하려면 이름이 `/messaging/messageHubFeed`인 피드를 사용해야 합니다. 피드 액션은 다음 매개변수를 지원합니다. 

|이름|유형|설명|
|---|---|---|
|kafka_brokers_sasl|JSON 문자열 배열|이 매개변수는 {{site.data.keyword.messagehub}} 인스턴스에서 브로커를 구성하는 `<host>:<port>` 문자열의 배열입니다.|
|user|문자열|{{site.data.keyword.messagehub}} 사용자 이름입니다.|
|password|문자열|{{site.data.keyword.messagehub}} 비밀번호입니다.|
|topic|문자열|트리거가 청취하도록 할 주제입니다.|
|kafka_admin_url|URL 문자열|{{site.data.keyword.messagehub}} 관리 REST 인터페이스의 URL입니다.|
|isJSONData|부울(선택사항 - 기본값=false)|`true`로 설정된 경우, 제공자는 메시지 값을 트리거 페이로드로서 전달하기 전에 JSON으로 구문 분석하려고 시도합니다.|
|isBinaryKey|부울(선택사항 - 기본값=false)|`true`로 설정된 경우, 제공자는 키 값을 트리거 페이로드로서 전달하기 전에 Base64로 인코딩합니다.|
|isBinaryValue|부울(선택사항 - 기본값=false)|`true`로 설정된 경우, 제공자는 메시지 값을 트리거 페이로드로서 전달하기 전에 Base64로 인코딩합니다.|

이 매개변수의 목록이 어려워보일 수 있지만, 이는 `ibmcloud fn package refresh` CLI 플러그인 명령을 사용하여 자동으로 설정될 수 있습니다. 

1. {{site.data.keyword.openwhisk}}에 사용 중인 현재 조직 및 영역 아래에서 {{site.data.keyword.messagehub}} 서비스의 인스턴스를 작성하십시오.

2. 청취할 주제가 {{site.data.keyword.messagehub}}에서 사용 가능한지 확인하거나 새 주제(예: **mytopic**)를 작성하십시오.

3. 네임스페이스의 패키지를 새로 고치십시오. 새로 고치기를 수행하면 작성된 {{site.data.keyword.messagehub}} 서비스 인스턴스에 대한 패키지 바인딩이 자동으로 작성됩니다.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  출력 예:
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```
  {: screen}

4. 네임스페이스에 패키지를 나열하여 패키지 바인딩이 이제 사용 가능함을 표시하십시오.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  출력 예:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```
  {: screen}

  이제 패키지 바인딩에 {{site.data.keyword.messagehub}} 인스턴스와 연관된 신임 정보가 포함됩니다.

5. 이제 새 메시지가 {{site.data.keyword.messagehub}} 주제에 게시될 때 실행되는 트리거를 작성하기만 하면 됩니다.
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## {{site.data.keyword.Bluemix_notm}} 외부에서 {{site.data.keyword.messagehub}} 패키지에 대한 트리거 작성
{: #create_message_hub_trigger_outside}

{{site.data.keyword.Bluemix_notm}} 외부에서 {{site.data.keyword.messagehub}}를 설정하려면 {{site.data.keyword.messagehub}} 서비스에 대한 패키지 바인딩을 수동으로 작성해야 합니다. {{site.data.keyword.messagehub}} 서비스 신임 정보 및 연결 정보가 필요합니다.

1. {{site.data.keyword.messagehub}} 서비스에 대해 구성된 패키지 바인딩을 작성하십시오.
  ```
  ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. 이제 새 메시지가 {{site.data.keyword.messagehub}} 주제에 게시될 때 실행되는 새 패키지를 사용하여 트리거를 작성할 수 있습니다.
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}

## 메시지 청취
{: #message_hub_listen}

일단 트리거가 작성되면 시스템은 메시징 전달 서비스에서 지정된 주제를 모니터합니다. 새 메시지가 게시되면 트리거가 실행됩니다.

이 트리거의 페이로드에는 트리거가 최종 실행된 시간으로부터 게시된 메시지의 배열인 `messages` 필드가 포함되어 있습니다. 배열의 각 메시지 오브젝트에는 다음 필드가 포함되어 있습니다.
- topic
- partition
- offset
- key
- value

Kafka 용어에서 필드는 자명합니다. 그러나 `key`에는 `key`가 2진 데이터를 전송할 수 있도록 허용하는 `isBinaryKey`라고 하는 기능이 있습니다. 또한 `value`에서는 특수 고려사항이 필요합니다. `isJSONData` 및 `isBinaryValue` 필드를 JSON 및 2진 메시지를 처리하는 데 사용할 수 있습니다. 이 `isJSONData` 및 `isBinaryValue` 필드는 함께 사용될 수 없습니다.

예를 들어, 트리거가 작성될 때 `isBinaryKey`가 `true`로 설정된 경우에는 실행된 트리거의 페이로드로부터 리턴될 때 `key`가 Base64 문자열로 인코딩됩니다.

`Some key`의 `key`가 `true`로 설정된 `isBinaryKey`로 게시되는 경우, 트리거 페이로드는 다음 예제와 유사합니다.
```json
{
    "messages": [
        {
            "partition": 0,
            "key": "U29tZSBrZXk=",
            "offset": 421760,
            "topic": "mytopic",
            "value": "Some value"
        }
    ]
}
```
{: codeblock}

트리거가 작성될 때 `isJSONData` 매개변수가 `false`로 설정된 경우(또는 전혀 설정되지 않은 경우), `value` 필드는 게시된 메시지의 원시 값입니다. 그러나 트리거가 작성될 때 `isJSONData`가 `true`로 설정된 경우, 시스템은 이 값을 최상의 방식으로 JSON 오브젝트로서 구문 분석하려고 시도합니다. 구문 분석에 성공하는 경우, 트리거 페이로드의 `value`는 결과 JSON 오브젝트입니다.

`{"title": "Some string", "amount": 5, "isAwesome": true}`의 메시지가 `true`로 설정된 `isJSONData`와 함께 게시되는 경우, 트리거 페이로드는 다음 예제와 다소 유사할 수 있습니다.
```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": {
          "amount": 5,
          "isAwesome": true,
          "title": "Some string"
      }
    }
  ]
}
```
{: codeblock}

그러나 동일한 메시지 컨텐츠가 `false`로 설정된 `isJSONData`와 함께 게시된 경우, 트리거 페이로드는 다음 예제와 유사할 수 있습니다.
```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421761,
      "topic": "mytopic",
      "value": "{\"title\": \"Some string\", \"amount\": 5, \"isAwesome\": true}"
    }
  ]
}
```
{: codeblock}

`isJSONData`와 유사하게 트리거 작성 중에 `isBinaryValue`가 `true`로 설정된 경우, 트리거 페이로드의 결과 `value`는 Base64 문자열로 인코딩됩니다.

`Some data`의 `value`가 `true`로 설정된 `isBinaryValue`와 함께 게시된 경우, 트리거 페이로드는 다음 예제와 다소 유사할 수 있습니다.
```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": "U29tZSBkYXRh"
    }
  ]
}
```
{: codeblock}

`isBinaryData`가 `true`로 설정되지 않고 동일한 메시지가 게시된 경우, 트리거 페이로드는 다음 예제와 유사합니다.
```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": "Some data"
    }
  ]
}
```
{: codeblock}

### 메시지가 일괄처리됨
트리거 페이로드에 메시지의 배열이 포함되어 있음에 유념하십시오. 이러한 메시지가 메시징 시스템으로 빠르게 생성되는 경우, 피드는 트리거의 단일 실행으로 게시된 메시지를 일괄처리하려고 시도합니다. 일괄처리를 사용하면 메시지가 트리거에 보다 빠르고 효율적으로 게시될 수 있습니다.

트리거에 의해 실행되는 액션을 코딩할 때 페이로드의 메시지 수에는 기술적으로 제한이 없지만 항상 0보다 크다는 점에 유념하십시오. 일괄처리된 메시지의 다음 예제를 참조하십시오(*offset* 값의 변경 참조).
```json
{
  "messages": [
       {
        "partition": 0,
         "key": null,
         "offset": 100,
         "topic": "mytopic",
         "value": {
            "amount": 5
         }
      },
      {
        "partition": 0,
         "key": null,
         "offset": 101,
         "topic": "mytopic",
         "value": {
            "amount": 1
         }
      },
      {
        "partition": 0,
         "key": null,
         "offset": 102,
         "topic": "mytopic",
         "value": {
            "amount": 999
         }
      }
  ]
}
```

## 예제
{: #examples}

### {{site.data.keyword.messagehub}}, Node Red, IBM Watson IoT, {{site.data.keyword.cos_full_notm}} 및 IBM Data Science Experience와 OpenWhisk 통합
{{site.data.keyword.messagehub}}, Node Red, IBM Watson IoT, {{site.data.keyword.cos_full}}, IBM Data Science Experience(Spark) 서비스와 OpenWhisk를 통합하는 예제는 [여기서 찾을 수](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0) 있습니다.

## 참조
- [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
