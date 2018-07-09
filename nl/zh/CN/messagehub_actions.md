---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# {{site.data.keyword.messagehub}}
{: #openwhisk_catalog_message_hub}

此包支持与 [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub) 实例进行通信，以通过本机高性能 Kafka API 来发布和使用消息。
{: shortdesc}

## 使用 {{site.data.keyword.Bluemix_notm}} 设置 {{site.data.keyword.messagehub}} 包
{: #create_message_hub_ibm}

1. 在正用于 {{site.data.keyword.openwhisk}} 的当前组织和空间下创建 {{site.data.keyword.messagehub}} 服务实例。

2. 验证要侦听的主题在 {{site.data.keyword.messagehub}} 中是否可用，或者创建新主题，例如名为 **mytopic** 的主题。

3. 刷新名称空间中的包。刷新操作将自动为已创建的 {{site.data.keyword.messagehub}} 服务实例创建包绑定。
  ```
  ibmcloud wsk package refresh
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
  ibmcloud wsk package list
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
ibmcloud wsk package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

## 使用事件侦听消息

有关如何使用 {{site.data.keyword.messagehub}} 中的触发器来侦听消息的详细信息，请参阅 [{{site.data.keyword.messagehub}} 事件源](./openwhisk_messagehub.html)主题，其中涵盖以下任务：
* [创建用于侦听 {{site.data.keyword.messagehub}} 实例的触发器](./openwhisk_messagehub.html#create_message_hub_trigger)
* [在 {{site.data.keyword.Bluemix_notm}} 外部创建 {{site.data.keyword.messagehub}} 包的触发器](./openwhisk_messagehub.html#create_message_hub_trigger_outside)
* [侦听消息](./openwhisk_messagehub.html#message_hub_listen)
* [示例](./openwhisk_messagehub.html#examples)

## 将消息生成到 {{site.data.keyword.messagehub}}
{: #producing_messages}

不推荐使用 `/messaging/messageHubProduce` 操作，该操作未来会除去。为了保持最佳性能，在将数据生成到 {{site.data.keyword.messagehub}}/Kafka 时，请将使用 `/messaging/messageHubProbate` 操作迁移为使用持久连接。
{: tip}

如果要使用 {{site.data.keyword.openwhisk_short}} 操作来方便地将消息生成到 {{site.data.keyword.messagehub}}，您可以使用 `/messaging/messageHubProduce` 操作。此操作将采用以下参数：

|名称|类型|描述|
|---|---|---|
|kafka_brokers_sasl|字符串的 JSON 数组|此参数是一组 `<host>:<port>` 字符串，包含 {{site.data.keyword.messagehub}} 实例中的代理程序。|
|user|字符串|您的 {{site.data.keyword.messagehub}} 用户名。|
|password|字符串|您的 {{site.data.keyword.messagehub}} 密码。|
|topic|字符串|希望触发器侦听的主题。|
|value|字符串|要生成的消息的值。|
|key|字符串（可选）|要生成的消息的键。|

前三个参数可使用 `ibmcloud wsk package refresh` 自动绑定，请参阅以下使用所有必需参数调用操作的示例：
```
ibmcloud wsk action invoke /messaging/messageHubProduce -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p topic mytopic -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p value "This is the content of my message"
```
{: pre}

## 参考
- [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
