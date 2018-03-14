---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Message Hub 패키지 사용
{: #openwhisk_catalog_message_hub}

고유 고성능 Kafka API를 사용하여 메시지를 공개하고 이용하기 위한 [Message Hub](https://developer.ibm.com/messaging/message-hub) 인스턴스와의 통신을 가능하게 하는 패키지입니다.
{: shortdesc}

## IBM MessageHub 인스턴스를 청취하는 트리거 작성
{: #openwhisk_catalog_message_hub_trigger}

메시지가 Message Hub 인스턴스에 게시될 때 반응하는 트리거를 작성하려면 `/messaging/messageHubFeed`로 이름 지정된 피드를 사용해야 합니다. 이 피드 조치는 다음 매개변수를 지원합니다. 

|이름 |유형 |설명 |
|---|---|---|
|kafka_brokers_sasl|JSON 문자열 배열|이 매개변수는 Message Hub 인스턴스의 브로커를 구성하는 `<host>:<port>` 문자열의 배열입니다. |
|user|문자열|Message Hub 사용자 이름|
|password|문자열|Message Hub 비밀번호|
|topic|문자열|트리거가 청취하도록 할 주제|
|kafka_admin_url|URL 문자열|Message Hub 관리 REST 인터페이스의 URL|
|isJSONData|부울(선택사항 - 기본값=false)|`true`로 설정된 경우, 제공자는 메시지 값을 트리거 페이로드로서 전달하기 전에 이를 JSON으로서 구문 분석하려고 시도합니다.|
|isBinaryKey|부울(선택사항 - 기본값=false)|`true`로 설정된 경우, 제공자는 키 값을 트리거 페이로드로서 전달하기 전에 이를 Base64로서 인코딩하려고 시도합니다. |
|isBinaryValue|부울(선택사항 - 기본값=false)|`true`로 설정된 경우, 제공자는 메시지 값을 트리거 페이로드로서 전달하기 전에 이를 Base64로서 인코딩하려고 시도합니다. |

이 매개변수 목록이 어려워보일 수 있지만, 이는 패키지 새로 고치기 CLI 명령을 사용하여 사용자를 위해 자동으로 설정될 수 있습니다. 

1. OpenWhisk에 사용 중인 현재 조직 및 영역 아래에서 Message Hub 서비스의 인스턴스를 작성하십시오. 

2. 청취하고자 하는 주제가 Message Hub에서 사용 가능한지 확인하거나 새 주제(예: `mytopic`)를 작성하십시오. 

3. 네임스페이스의 패키지를 새로 고치십시오. 새로 고치기를 수행하면 작성된 Message Hub 서비스 인스턴스에 대한 패키지 바인딩이 자동으로 작성됩니다. 

  ```
  wsk package refresh
  ```
  {: pre}
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```

  이제 패키지 바인딩에 Message Hub 인스턴스와 연관된 신임 정보가 포함되어 있습니다. 

4. 이제 새 메시지가 Message Hub 주제에 게시될 때 실행되는 트리거를 작성하기만 하면 됩니다. 

  ```
  wsk trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## {{site.data.keyword.Bluemix_notm}} 외부에서 Message Hub 패키지 설정

{{site.data.keyword.Bluemix_notm}} 외부에서 Message Hub를 설정하려면 Message Hub 서비스에 대한 패키지 바인딩을 수동으로 작성해야 합니다. Message Hub 서비스 신임 정보 및 연결 정보가 필요합니다. 

1. Message Hub 서비스에 대해 구성된 패키지 바인딩을 작성하십시오. 

  ```
  wsk package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your Message Hub user> -p password <your Message Hub password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. 이제 새 메시지가 Message Hub 주제에 게시될 때 실행되는 새 패키지를 사용하여 트리거를 작성할 수 있습니다. 

  ```
  wsk trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}


## 메시지 청취
{: #openwhisk_catalog_message_hub_listen}

일단 트리거가 작성되면 시스템은 메시지 전달 서비스에서 지정된 주제를 모니터합니다. 새 메시지가 게시되면 트리거가 실행됩니다. 

해당 트리거의 페이로드에는 트리거가 최종 실행된 시간으로부터 게시된 메시지의 배열인 `messages` 필드가 포함되어 있습니다. 배열의 각 메시지 오브젝트에는 다음 필드가 포함되어 있습니다. 
- topic
- partition
- offset
- key
- value

Kafka 용어에서 필드는 자명합니다. 그러나 `key`에는 `key`가 2진 데이터를 전송할 수 있도록 허용하는 `isBinaryKey`라고 하는 기능이 있습니다. 또한 `value`에서는 특수 고려사항이 필요합니다. `isJSONData` 및 `isBinaryValue` 필드를 JSON 및 2진 메시지를 처리하는 데 사용할 수 있습니다. 이 `isJSONData` 및 `isBinaryValue` 필드는 함께 사용될 수 없습니다. 

예를 들어, 트리거가 작성될 때 `isBinaryKey`가 `true`로 설정된 경우에는 실행된 트리거의 페이로드로부터 리턴될 때 `key`가 Base64 문자열로서 인코딩됩니다. 

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

`isJSONData`와 유사하게 트리거 작성 중에 `isBinaryValue`가 `true`로 설정된 경우, 트리거 페이로드의 결과 `value`는 Base64 문자열로서 인코딩됩니다. 

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

`isBinaryData`가 `true`로 설정되지 않고 동일한 메시지가 게시된 경우, 트리거 페이로드는 다음 예제와 유사할 수 있습니다. 

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

### 메시지가 일괄처리됨
트리거 페이로드에 메시지의 배열이 포함되어 있음에 유념하십시오. 이러한 메시지가 메시징 시스템으로 빠르게 생성되는 경우, 피드는 트리거의 단일 실행으로 게시된 메시지를 일괄처리하려고 시도합니다. 일괄처리를 사용하면 메시지가 트리거에 보다 빠르고 효율적으로 게시될 수 있습니다. 

트리거에 의해 실행되는 조치를 코딩할 때 페이로드의 메시지 수에는 기술적으로 제한이 없지만 항상 0보다 크다는 점에 유념하십시오. 일괄처리된 메시지의 다음 예제를 참조하십시오(*offset* 값의 변경 참조). 
 
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

## Message Hub로 메시지 생성
OpenWhisk 액션을 사용하여 Message Hub로 메시지를 편리하게 생성하려면 `/messaging/messageHubProduce` 조치를 사용할 수 있습니다. 이 조치는 다음 매개변수를 취합니다. 

|이름 |유형 |설명 |
|---|---|---|
|kafka_brokers_sasl|JSON 문자열 배열|이 매개변수는 Message Hub 인스턴스의 브로커를 구성하는 `<host>:<port>` 문자열의 배열입니다. |
|user|문자열|Message Hub 사용자 이름|
|password|문자열|Message Hub 비밀번호|
|topic|문자열|트리거가 청취하도록 할 주제|
|value|문자열|생성하고자 하는 메시지에 대한 값|
|key|문자열(선택사항)|생성하고자 하는 메시지에 대한 키|

처음 3개의 매개변수가 `wsk package refresh`를 사용하여 자동으로 바인드될 수 있지만, 모든 필수 매개변수로 조치를 호출하는 다음 예제를 참조하십시오. 

```
wsk action invoke /messaging/messageHubProduce -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p topic mytopic -p user <your Message Hub user> -p password <your Message Hub password> -p value "This is the content of my message"
```
{: pre}

## 예제

### IBM Message Hub, Node Red, IBM Watson IoT, IBM Object Storage 및 IBM Data Science Experience와 OpenWhisk 통합
IBM Message Hub, Node Red, IBM Watson IoT, IBM Object Storage, IBM Data Science Experience(Spark) 서비스와 OpenWhisk를 통합하는 예제는 [여기서 찾을 수](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0) 있습니다. 

## 참조
- [IBM Message Hub](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
