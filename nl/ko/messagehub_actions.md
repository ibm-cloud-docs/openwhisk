---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# {{site.data.keyword.messagehub}} 패키지 

{: #openwhisk_catalog_message_hub}

고유 고성능 Kafka API를 사용하여 메시지를 공개하고 이용하기 위한 [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub) 인스턴스와의 통신을 가능하게 하는 패키지입니다.
{: shortdesc}

## {{site.data.keyword.Bluemix_notm}}를 사용하여 {{site.data.keyword.messagehub}} 패키지 설정
{: #create_message_hub_ibm}

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

## {{site.data.keyword.Bluemix_notm}} 외부에서 {{site.data.keyword.messagehub}} 패키지 설정

{{site.data.keyword.Bluemix_notm}} 외부에서 {{site.data.keyword.messagehub}}를 설정하려면 {{site.data.keyword.messagehub}} 서비스에 대한 패키지 바인딩을 수동으로 작성해야 합니다. {{site.data.keyword.messagehub}} 서비스 신임 정보 및 연결 정보가 필요합니다.

{{site.data.keyword.messagehub}} 서비스에 대해 구성된 패키지 바인딩을 작성하십시오.
```
ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

## 이벤트를 사용하여 메시지 청취

{{site.data.keyword.messagehub}}에서 트리거를 사용하여 메시지를 청취하는 방법에 대한 자세한 정보는 다음 태스크를 다루는
다음 [{{site.data.keyword.messagehub}} 이벤트 소스](./openwhisk_messagehub.html) 주제를 참조하십시오.
* [{{site.data.keyword.messagehub}} 인스턴스를 청취하는 트리거 작성](./openwhisk_messagehub.html#create_message_hub_trigger)
* [{{site.data.keyword.Bluemix_notm}} 외부에서 {{site.data.keyword.messagehub}} 패키지에 대한 트리거 작성](./openwhisk_messagehub.html#create_message_hub_trigger_outside)
* [메시지 청취](./openwhisk_messagehub.html#message_hub_listen)
* [예제](./openwhisk_messagehub.html#examples)

## {{site.data.keyword.messagehub}}에 메시지 생성
{: #producing_messages}

`/messaging/messageHubProduce` 액션은 더 이상 사용되지 않으며 나중에 제거됩니다. 최적의 성능을 유지하려면 데이터가 {{site.data.keyword.messagehub}}/Kafka에 생성될 때 `/messaging/messageHubProduce` 액션의 사용을 마이그레이션하여 지속적 연결을 사용하십시오.
{: tip}

{{site.data.keyword.openwhisk_short}} 액션을 사용하여 {{site.data.keyword.messagehub}}에 메시지를 편리하게 생성하기 위해 `/messaging/messageHubProduce` 액션을 사용할 수 있습니다. 이 액션은 다음 매개변수를 사용합니다.

|이름|유형|설명|
|---|---|---|
|kafka_brokers_sasl|JSON 문자열 배열|이 매개변수는 {{site.data.keyword.messagehub}} 인스턴스에서 브로커를 구성하는 `<host>:<port>` 문자열의 배열입니다.|
|user|문자열|{{site.data.keyword.messagehub}} 사용자 이름입니다.|
|password|문자열|{{site.data.keyword.messagehub}} 비밀번호입니다.|
|topic|문자열|트리거가 청취하도록 할 주제입니다.|
|value|문자열|생성할 메시지에 대한 값입니다.|
|key|문자열(선택사항)|생성할 메시지에 대한 키입니다.|

처음 3개의 매개변수는 `ibmcloud fn package refresh`를 사용하여 자동으로 바인드될 수 있지만, 모든 필수 매개변수로 액션을 호출하는 다음 예제를 참조하십시오. 
```
ibmcloud fn action invoke /messaging/messageHubProduce -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p topic mytopic -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p value "This is the content of my message"
```
{: pre}

## 참조
- [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
