---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Message Hub 事件源
{: #openwhisk_catalog_message_hub}

您可以创建用于在消息使用订阅源发布到 {{site.data.keyword.messagehub_full}} 实例时做出反应的触发器。了解如何使用或不使用 {{site.data.keyword.Bluemix}} 创建 {{site.data.keyword.messagehub}}，侦听消息以及处理批量消息。
{: shortdesc}

## {{site.data.keyword.messagehub}} 包

不推荐使用 `/messaging/messageHubProduce` 操作，该操作未来会除去。为了保持最佳性能，在将数据生成到 Message Hub/Kafka 时，请将使用 `/messaging/messageHubProbate` 操作迁移为使用持久连接。
{: tip}

此包支持与 [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub) 实例进行通信，以通过本机高性能 Kafka API 来发布和使用消息。有关 {{site.data.keyword.messagehub}} 包以及如何设置包和生成消息的更多信息，请参阅 [{{site.data.keyword.messagehub}} 包](./messagehub_actions.html)主题。

## 创建用于侦听 {{site.data.keyword.messagehub}} 实例的触发器
{: #create_message_hub_trigger}

为了创建用于在消息发布到 {{site.data.keyword.messagehub}} 实例时做出反应的触发器，您需要使用名为 `/messaging/messageHubFeed` 的订阅源。此订阅源操作支持以下参数：

|名称|类型|描述|
|---|---|---|
|kafka_brokers_sasl|字符串的 JSON 数组|此参数是一组 `<host>:<port>` 字符串，包含 {{site.data.keyword.messagehub}} 实例中的代理程序。|
|user|字符串|您的 {{site.data.keyword.messagehub}} 用户名。|
|password|字符串|您的 {{site.data.keyword.messagehub}} 密码。|
|topic|字符串|希望触发器侦听的主题。|
|kafka_admin_url|URL 字符串|{{site.data.keyword.messagehub}} Admin REST 接口的 URL。|
|isJSONData|布尔值（可选 - 缺省值为 false）|设置为 `true` 时，提供程序在将消息值作为触发器有效内容传递之前，会先尝试将其解析为 JSON。|
|isBinaryKey|布尔值（可选 - 缺省值为 false）|设置为 `true` 时，提供程序在将键值作为触发器有效内容传递之前，会先将其编码为基本 64 位。|
|isBinaryValue|布尔值（可选 - 缺省值为 false）|设置为 `true` 时，提供程序在将消息值作为触发器有效内容传递之前，会先将其编码为基本 64 位。|

此参数列表可能看起来让人望而生畏，但可以使用 `ibmcloud fn package refresh` CLI 插件命令自动设置这些参数：

1. 在正用于 {{site.data.keyword.openwhisk}} 的当前组织和空间下创建 {{site.data.keyword.messagehub}} 服务实例。

2. 验证要侦听的主题在 {{site.data.keyword.messagehub}} 中是否可用，或者创建新主题，例如 **mytopic**。

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

5. 现在，您只需创建将在新消息发布到 {{site.data.keyword.messagehub}} 主题时触发的触发器即可。
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## 在 {{site.data.keyword.Bluemix_notm}} 外部创建 {{site.data.keyword.messagehub}} 包的触发器
{: #create_message_hub_trigger_outside}

如果要在 {{site.data.keyword.Bluemix_notm}} 外部设置 {{site.data.keyword.messagehub}}，那么必须为 {{site.data.keyword.messagehub}} 服务手动创建包绑定。您需要 {{site.data.keyword.messagehub}} 服务凭证和连接信息。

1. 创建为 {{site.data.keyword.messagehub}} 服务配置的包绑定。
  ```
  ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. 现在，您可以使用新包创建将在新消息发布到 {{site.data.keyword.messagehub}} 主题时触发的触发器。
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}

## 侦听消息
{: #message_hub_listen}

一旦创建触发器后，系统将监视消息传递服务中指定的主题。发布新消息时，将触发触发器。

该触发器的有效内容将包含 `messages` 字段，此字段是自上次触发触发器以来已发布的消息的数组。数组中的每个消息对象都包含以下字段：
- topic
- partition
- offset
- key
- value

在 Kafka 术语中，这些字段可顾名思义。但是，`key` 具有名为 `isBinaryKey` 的功能，允许 `key` 传输二进制数据。此外，需要特别注意 `value`。`isJSONData` 和 `isBinaryValue` 字段可用于处理 JSON 和二进制消息。但 `isJSONData` 和 `isBinaryValue` 字段不能一起使用。

例如，如果创建触发器时将 `isBinaryKey` 设置为 `true`，那么在从触发的触发器的有效内容返回 `key` 时，会先将其编码为基本 64 位字符串。

如果在 `isBinaryKey` 设置为 `true` 时发布值为 `Some key` 的 `key`，那么触发器有效内容将类似于以下示例：
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

如果创建触发器时 `isJSONData` 参数设置为 `false`（或根本未设置），那么 `value` 字段将为所发布消息的原始值。然而，如果创建触发器时将 `isJSONData` 设置为 `true`，那么系统将尽力尝试将此值解析为 JSON 对象。如果解析成功，那么触发器有效内容中的 `value` 将为生成的 JSON 对象。

如果在 `isJSONData` 设置为 `true` 的情况下发布消息 `{"title": "Some string", "amount": 5, "isAwesome": true}`，触发器有效内容可能类似于以下示例：
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

但是，如果在 `isJSONData` 设置为 `false` 的情况下发布相同的消息内容，触发器有效内容将类似于以下示例：
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

与 `isJSONData` 类似，如果触发器创建期间 `isBinaryValue` 设置为 `true`，那么触发器有效内容中生成的 `value` 将编码为基本 64 位字符串。

如果在 `isBinaryKey` 设置为 `true` 时发布的 `value` 为 `Some data`，那么触发器有效内容可能类似于以下示例：
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

如果在 `isBinaryData` 未设置为 `true` 时发布相同的消息，那么触发器有效内容将类似于以下示例：
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

### 消息将批量处理
请注意，触发器有效内容包含消息数组。如果向消息传递系统生成这些消息的速度非常快，订阅源将尝试在单次触发触发器时对发布的消息进行批量处理。通过批量处理，消息可以更快速、更高效地发布到触发器。

请记住，对触发器所触发的操作编写代码时，有效内容中的消息数在技术上是无限的，但始终会大于 0。请参阅以下批量消息的示例（请注意 *offset* 值的变化）：
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

## 示例
{: #examples}

### 将 OpenWhisk 与 {{site.data.keyword.messagehub}}、Node Red、IBM Watson IoT、{{site.data.keyword.cos_full_notm}} 和 IBM Data Science Experience 集成
有关用于将 OpenWhisk 与 {{site.data.keyword.messagehub}}、Node Red、IBM Watson IoT、{{site.data.keyword.cos_full}} 和 IBM Data Science Experience (Spark) 服务集成的示例[位于此处](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0)。

## 参考
- [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
