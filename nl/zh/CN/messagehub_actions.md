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

# {{site.data.keyword.messagehub}} 包

{: #catalog_message_hub}

此包支持与 [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub) 实例进行通信，以通过本机高性能 Kafka API 来发布和使用消息。
{: shortdesc}

## 使用 {{site.data.keyword.Bluemix_notm}} 设置 {{site.data.keyword.messagehub}} 包
{: #create_message_hub_ibm}

1. 在正用于 {{site.data.keyword.openwhisk}} 的当前组织和空间下创建 {{site.data.keyword.messagehub}} 服务实例。

2. 验证要侦听的主题在 {{site.data.keyword.messagehub}} 中是否可用，或者创建新主题，例如名为 **mytopic** 的主题。

3. 刷新名称空间中的包。刷新操作将自动为已创建的 {{site.data.keyword.messagehub}} 服务实例创建包绑定。
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  示例输出：
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```
  {: screen}

4. 列出名称空间中的包，以显示包绑定是否现在可用。
  ```
  ibmcloud fn package list
  ```
  {: pre}

  示例输出：
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```
  {: screen}

  现在，包绑定包含与 {{site.data.keyword.messagehub}} 实例关联的凭证。

## 在 {{site.data.keyword.Bluemix_notm}} 外部设置 {{site.data.keyword.messagehub}} 包

如果要在 {{site.data.keyword.Bluemix_notm}} 外部设置 {{site.data.keyword.messagehub}}，那么必须为 {{site.data.keyword.messagehub}} 服务手动创建包绑定。您需要 {{site.data.keyword.messagehub}} 服务凭证和连接信息。

创建为 {{site.data.keyword.messagehub}} 服务配置的包绑定。
```
ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

## 使用事件侦听消息

有关如何使用 {{site.data.keyword.messagehub}} 中的触发器来侦听消息的详细信息，请参阅 [{{site.data.keyword.messagehub}} 事件源](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub)主题，其中涵盖以下任务：
* [创建用于侦听 {{site.data.keyword.messagehub}} 实例的触发器](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#create_message_hub_trigger)
* [在 {{site.data.keyword.Bluemix_notm}} 外部创建 {{site.data.keyword.messagehub}} 包的触发器](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#create_message_hub_trigger_outside)
* [侦听消息](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#message_hub_listen)
* [示例](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#examples)

## 将消息生成到 {{site.data.keyword.messagehub}}
{: #producing_messages}

不推荐使用 `/messaging/messageHubProduce` 操作，该操作未来会除去。东京区域中已除去该操作。为了保持最佳性能，在将数据生成到 {{site.data.keyword.messagehub}}/Kafka 时，请将使用 `/messaging/messageHubProbate` 操作迁移为使用持久连接。
{: tip}

要了解有关生成消息的更多信息，请查看 [Event Streams 文档](/docs/services/EventStreams?topic=eventstreams-producing_messages#producing_messages)。

## 参考
- [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub)
- [Apache Kafka](https://kafka.apache.org)
