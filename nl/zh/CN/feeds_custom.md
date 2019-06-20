---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: feeds, serverless

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# 创建定制事件提供程序订阅源
{: #feeds_custom}

{{site.data.keyword.openwhisk_short}} 支持开放 API，通过此 API，任何用户都可以将事件发起者服务公开为包中的订阅源。
{: shortdesc}


## 订阅源体系结构
{: #feeds_arch}

有 3 种体系结构模式可用于创建订阅源：**Hook**、**轮询**和**连接**。

### Hook

在 Hook 模式下，使用由其他服务公开的 [Webhook](https://en.wikipedia.org/wiki/Webhook) 来设置订阅源。在此策略中，Webhook 在外部服务上进行配置，用于直接向 URL 执行 POST 操作来触发触发器。此方法是目前为止实现低频率订阅源最简单、最有吸引力的选项。

例如，[Github 包](/docs/openwhisk?topic=cloud-functions-pkg_github)和 [Push Notification 包](/docs/openwhisk?topic=cloud-functions-pkg_push_notifications)使用 Webhook。


### 轮询

使用“轮询”模式时，{{site.data.keyword.openwhisk_short}} 操作安排为定期轮询端点以访存新数据。此模式构建起来相对容易，但事件频率会受到轮询时间间隔的限制。

### 连接

使用“连接”模式时，单独的服务用于保持与源订阅源的持续连接。基于连接的实现可能会通过使用较长的轮询时间间隔与服务端点进行交互，或者设置推送通知。

例如，[{{site.data.keyword.cloudant}} 包](/docs/openwhisk?topic=cloud-functions-pkg_cloudant)使用“连接”模式。



##  实现订阅源操作
{: #feeds_actions}

订阅源操作是接受以下参数的一种操作：
* **lifecycleEvent**：“CREATE”、“READ”、“UPDATE”、“DELETE”、“PAUSE”或“UNPAUSE”。
* **triggerName**：包含从此订阅源所生成事件的触发器的标准名称。
* **authKey**：拥有触发器的 {{site.data.keyword.openwhisk_short}} 用户的基本认证凭证。

订阅源操作还可以接受管理订阅源所需的其他任何参数。例如，{{site.data.keyword.cloudant}} changes 订阅源操作期望接收多个参数，包括 `dbname` 和 `username`。

用户使用 `--feed` 参数通过 CLI 创建触发器时，系统会自动使用相应参数调用订阅源操作。

例如，用户通过将其用户名和密码作为绑定参数，为 **cloudant** 包创建了 `mycloudant` 绑定。用户通过 CLI 发出以下命令时：
```
ibmcloud fn trigger create my_cloudant_trigger --feed mycloudant/changes -p dbName myTable
```
{: pre}

然后，将运行等效于以下命令的内容：
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

名为 *changes* 的订阅源操作将采用这些参数，并且系统期望该操作执行设置来自 {{site.data.keyword.cloudant_short_notm}} 的事件流所需的任何操作。订阅源操作是使用相应的配置来执行的，此配置将定向到触发器。

对于 {{site.data.keyword.cloudant_short_notm}} *changes* 订阅源，该操作与通过基于连接的体系结构实现的 *{{site.data.keyword.cloudant_short_notm}} trigger* 服务直接对话。

对于 `ibmcloud fn trigger delete`、`ibmcloud fn trigger update` 和 `ibmcloud fn trigger get`，会执行类似的订阅源操作协议。

## 通过 Hook 实现订阅源
{: #feeds_hooks}

事件发起者支持 Webhook/回调工具时，可通过 Hook 设置订阅源。

通过此方法，无需在 {{site.data.keyword.openwhisk_short}} 外部维持任何持久服务。所有订阅源管理工作均通过无状态的 {{site.data.keyword.openwhisk_short}} *订阅源操作*正常执行，这些操作直接与第三方 Webhook API 进行协商。

使用 `CREATE` 进行调用时，订阅源操作只会为其他某个服务安装 Webhook，并请求远程服务执行 POST 操作将通知发布到 {{site.data.keyword.openwhisk_short}} 中的相应 `fireTrigger` URL。

这将对 Webhook 进行定向以向 URL 发送通知，例如：

`POST /namespaces/{namespace}/triggers/{triggerName}`

具有 POST 请求的表单将解释为 JSON 文档，用于定义有关触发器事件的参数。{{site.data.keyword.openwhisk_short}} 规则会将这些触发器参数传递到要作为事件结果进行触发的任何操作。

## 通过轮询实现订阅源
{: #feeds_polling}

可以设置操作以完全在 {{site.data.keyword.openwhisk_short}} 中轮询源订阅源，而无需维持任何持续连接或外部服务。

对于订阅源，如果其中 Webhook 不可用，但无需大容量或等待响应时间短，那么可以使用轮询。

要设置基于轮询的订阅源，订阅源操作在针对 `CREATE` 进行调用时执行以下步骤：

1. 订阅源操作使用 `whisk.system/alarms` 订阅源将定期触发器设置为所需频率。
2. 订阅源开发者创建 `pollMyService` 操作，以轮询远程服务并返回任何新事件。
3. 订阅源操作设置*规则* *T -> pollMyService*。

此过程完全使用 {{site.data.keyword.openwhisk_short}} 操作来实现基于轮询的触发器，而无需任何单独的服务。

## 使用连接实现订阅源
{: #feeds_connections}

前两个体系结构选项实现起来既简单又轻松。但是，如果需要高性能订阅源，那么没有持续连接和长时间轮询的替代方法或类似方法。

由于 {{site.data.keyword.openwhisk_short}} 操作必须是短时间运行的，因此操作不能保持与第三方的持续连接。可以改为在 {{site.data.keyword.openwhisk_short}} 外部维持一直运行的名为*提供者服务*的单独服务。提供者服务可以保持与第三方事件源的连接，这些事件源支持长时间轮询或其他基于连接的通知。

提供者服务的 REST API 允许 {{site.data.keyword.openwhisk_short}} *订阅源操作*控制订阅源。提供者服务充当事件提供程序和 {{site.data.keyword.openwhisk_short}} 之间的代理。当提供者服务收到来自第三方的事件时，会通过触发触发器来将其发送给 {{site.data.keyword.openwhisk_short}}。

{{site.data.keyword.cloudant_short_notm}} *changes* 订阅源是典型示例，因为它维持 `cloudanttrigger` 服务，该服务用于调解通过持续连接发送的 {{site.data.keyword.cloudant_short_notm}} 通知和 {{site.data.keyword.openwhisk_short}} 触发器。


*alarm* 订阅源通过类似模式实现。

基于连接的体系结构是性能最高的选项，但相对于轮询和 Hook 体系结构，它在操作方面的开销更大。


