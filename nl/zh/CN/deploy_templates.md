---

copyright:

  years: 2018

lastupdated: "2018-05-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# 部署快速入门模板
{: #serviceauth}

{{site.data.keyword.openwhisk}} 提供了模板目录来帮助您快速开始使用下一个项目。模板是操作、触发器和序列的组合，还可以包含 {{site.data.keyword.Bluemix}} 中的服务实例。通过使用模板，您可以快速、轻松地创建项目，然后立即开始编码。

本教程将引导您逐渐部署 Cloudant 模板。
{: shortdesc}

## 可用的快速入门模板
{: #available-templates}

|名称|描述
|支持的运行时|
|:-----------------|:-----------------|:-----------------|
|[Cloudant Events](./deploy_templates.html#cloudant-template)|Cloudant DB 中编辑或添加了文档时，会在控制台中记录更改。|Node.js、Swift、Python 和 PHP|
|[Get HTTP Resource](./deploy_templates.html#get-http-resource-template)|此 Web 操作在响应 HTTP 事件时调用，然后通过 Yahoo Weather API 访存数据。|Node.js 和 Python|
|[Hello World](./deploy_templates.html#hello-world-template)|此操作将接受单个参数，此参数必须是 JSON 对象。|Node.js、Swift、Python 和 PHP|
|[Message Hub Events](./deploy_templates.html#messagehub-events-template)|Message Hub 主题添加了新数据时，会在控制台中记录更改。|Node.js、Swift、Python 和 PHP|
|[Periodic Slack Reminder](./deploy_templates.html#slack-reminder-template)|此操作将根据定期触发器发布到 Slack。|Node.js、Swift、Python 和 PHP|

## 部署 Cloudant Events 模板
{: #cloudant-template}

Cloudant 模板会创建一序列操作以及用于启动该序列的触发器。连接的 Cloudant 数据库（应该是 cat 的数据库，具有名称和颜色）中发生更改时，会触发此触发器。期望的数据项是定义了名称和颜色的 cat。向数据库添加新 cat 或编辑现有 cat 时，会将数据记录到控制台。

1. 要创建模板，请转至 [{{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)，然后单击**开始创建**。

2. 单击**快速入门模板**。

3. 单击**新建 Cloudant 项**。

### 创建 Cloudant 操作

1. 接下来，提供包的名称或使用提供的缺省名称 `new-cloudant-item`。

2. 在**操作**下拉列表下，为您将拥有的操作选择运行时（nodejs、swift、python 或 php）。对于此示例，选择 **nodejs**，然后单击**下一步**。

### 创建 Cloudant 触发器

触发器从事件源收到事件时会调用操作。要创建 Cloudant 模板的触发器，请为触发器提供必需的 Cloudant 服务实例信息。

#### 创建 Cloudant 服务实例

可以选择以下任一项：
  * **创建自己的实例**
  * **输入自己的凭证**

1. 对于此示例，请选择**创建自己的实例**。

2. 这将打开一个弹出窗口，使您转至具有 Cloudant 设置页面的新选项卡。创建 Cloudant 实例后，必须创建一组服务凭证，然后通过单击**确定**关闭该选项卡以返回到此页面。

3. 现在，选择**输入自己的凭证**并提供以下信息：
  * 用户名 - _您的 Cloudant 用户名_
  * 密码 - _您的 Cloudant 密码_
  * 主机 - _这通常是您的 `username.cloudant.com`_
  * 数据库 - _您的 Cloudant 数据库的名称_

### 部署 Cloudant 模板

单击**部署**。

部署模板后，您可以进一步对代码进行编辑以根据需要对其进行定制，或者返回并查看可用模板的目录。

## 部署 Get HTTP Resource 模板
{: #get-http-resource-template}

Get HTTP Resource 模板创建用于访存外部资源 Yahoo Weather API 的操作，然后返回数据。该操作作为 Web 操作启用，从而允许使用启用了 CORS 的 URL 来调用该操作，而无需认证密钥，这对于为 Web 应用程序构建后端非常有用。**注**：缺省情况下，`get-http-resource` 端点对于任何可能要调用该端点的用户都公开可用。

1. 要创建模板，请转至 [{{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)，然后单击**开始创建**。

2. 单击**快速入门模板**。

3. 查看**包名称**字段，您可以根据需要对其进行更新。缺省值设置为 `get-http-resource`。

4. 为您将拥有的操作选择运行时：Node.js 8、Node.js 6 或 Python 3。

5. 单击**部署**。

部署模板后，您可以进一步对代码进行编辑以根据需要对其进行定制，或者返回并查看可用模板的目录。

## 部署 Hello World 模板
{: #hello-world-template}

此操作将接受单个参数，此参数必须是 JSON 对象。

1. 要创建模板，请转至 [{{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)，然后单击**开始创建**。

2. 单击**快速入门模板**。

3. 查看**包名称**字段，您可以根据需要对其进行更新。缺省值设置为 `hello-world`。

4. 为您将拥有的操作选择运行时：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。

5. 单击**部署**。

部署模板后，您可以进一步对代码进行编辑以根据需要对其进行定制，或者返回并查看可用模板的目录。

## 部署 Message Hub Events 模板
{: #messagehub-events-template}

Message Hub Events 模板将创建操作以及用于触发该操作的触发器。只要向创建模板期间选择的 Message Hub 主题添加了新项，就会触发该触发器。

1. 要创建模板，请转至 [{{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)，然后单击**开始创建**。

2. 单击**快速入门模板**。

3. 查看**包名称**字段，您可以根据需要对其进行更新。缺省值设置为 `message-hub-events`。

4. 为您将拥有的操作选择运行时：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。

5. 单击**下一步**。

### 创建 Message Hub 触发器

触发器从事件源收到事件时会调用操作。要创建 Message Hub 模板的触发器，请为触发器提供必需的 Message Hub 服务实例信息。

查看**触发器名称**字段，您可以根据需要对其进行更新。缺省值设置为 `message-hub-events-trgr`。

### 创建 Message Hub 服务实例

可以选择以下任一项：
  * **创建自己的实例**
  * **输入自己的凭证**

1. 对于此示例，请选择**创建自己的实例**。

2. 这将打开一个弹出窗口，使您转至具有 Message Hub 设置页面的新选项卡。创建 Message Hub 实例后，必须创建一组服务凭证，然后通过单击**确定**关闭该选项卡以返回到此页面。

3. 现在，选择**输入自己的凭证**并提供以下信息：
  * 用户名 - _您的 Message Hub 用户名_
  * 密码 - _您的 Message Hub 密码_
  * kafka_admin_url - _Message Hub Admin REST URL_
  * 数据库 - _Message 数据库的名称_
  * 主题 - _要预订的主题_

### 部署 Message Hub 模板

单击**部署**。

部署模板后，您可以进一步对代码进行编辑以根据需要对其进行定制，或者返回并查看可用模板的目录。

## 部署 Periodic Slack Reminder 模板
{: #slack-reminder-template}

Periodic Slack Reminder 模板根据用户在触发器创建期间提供的时间间隔发布到 Slack。创建此模板之前，请转至 https://api.slack.com/incoming-webhooks 以设置所需的入局 Webhook URL。

1. 查看**包名称**字段，您可以根据需要对其进行更新。缺省值设置为 `periodic-slack-reminder`。

2. 为您将拥有的操作选择运行时：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。

3. 在**参数**部分下的**参数值**字段中，输入 Webhook URL，然后单击**下一步**。（示例：https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX）。

### 创建 Slack Reminder 触发器

触发器从事件源收到事件时会调用操作。要创建 Slack Reminder 模板的触发器，请为触发器提供必需的 Message Hub 服务实例信息。

1. 查看**触发器名称**字段，您可以根据需要对其进行更新。缺省值设置为 `periodic-slack-reminder-trgr`。

2. 接下来，您可以使用模式或 Cron 表达式来指定触发器触发的时间间隔。可以为“周内日期”、“小时”和“分钟”选择 UTC 时间。选择所需的时间间隔选项后，您即准备就绪，可以进行模板部署。

3. 单击**部署**。

部署模板后，您可以进一步对代码进行编辑以根据需要对其进行定制，或者返回并查看可用模板的目录。
