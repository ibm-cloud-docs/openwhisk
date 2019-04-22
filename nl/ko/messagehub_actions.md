---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: message hub, package, messages, events

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.messagehub}} 패키지 

{: #catalog_message_hub}

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

  이제 패키지 바인딩에 {{site.data.keyword.messagehub}} 인스턴스와 연관된 인증 정보가 포함됩니다.

## {{site.data.keyword.Bluemix_notm}} 외부에서 {{site.data.keyword.messagehub}} 패키지 설정

{{site.data.keyword.Bluemix_notm}} 외부에서 {{site.data.keyword.messagehub}}를 설정하려면 {{site.data.keyword.messagehub}} 서비스에 대한 패키지 바인딩을 수동으로 작성해야 합니다. {{site.data.keyword.messagehub}} 서비스 인증 정보 및 연결 정보가 필요합니다.

{{site.data.keyword.messagehub}} 서비스에 대해 구성된 패키지 바인딩을 작성하십시오.
```
ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

## 이벤트를 사용하여 메시지 청취

{{site.data.keyword.messagehub}}에서 트리거를 사용하여 메시지를 청취하는 방법에 대한 자세한 정보는 다음 태스크를 다루는
다음 [{{site.data.keyword.messagehub}} 이벤트 소스](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub) 주제를 참조하십시오.
* [{{site.data.keyword.messagehub}} 인스턴스를 청취하는 트리거 작성](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#create_message_hub_trigger)
* [{{site.data.keyword.Bluemix_notm}} 외부에서 {{site.data.keyword.messagehub}} 패키지에 대한 트리거 작성](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#create_message_hub_trigger_outside)
* [메시지 청취](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#message_hub_listen)
* [예제](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#examples)

## {{site.data.keyword.messagehub}}에 메시지 생성
{: #producing_messages}

`/messaging/messageHubProduce` 액션은 더 이상 사용되지 않으며 나중에 제거됩니다. 도쿄 지역에서 이미 제거되었습니다. 최적의 성능을 유지하려면 데이터가 {{site.data.keyword.messagehub}}/Kafka에 생성될 때 `/messaging/messageHubProduce` 액션의 사용을 마이그레이션하여 지속적 연결을 사용하십시오.
{: tip}

메시지 생성에 대한 자세한 정보를 보려면 [이벤트 스트림 문서](/docs/services/EventStreams?topic=eventstreams-producing_messages#producing_messages)를 체크아웃하십시오.

## 참조
- [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub)
- [Apache Kafka](https://kafka.apache.org)
