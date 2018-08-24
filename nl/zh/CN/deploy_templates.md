---

copyright:
  years: 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# 部署快速入门模板
{: #serviceauth}

{{site.data.keyword.openwhisk}} 提供了模板目录来帮助您快速开始使用下一个项目。模板是操作、触发器和序列的组合，还可以包含 {{site.data.keyword.Bluemix}} 中的服务实例。通过使用模板，您可以快速、轻松地创建项目，然后立即开始编码。

本教程将引导您逐步部署通过 {{site.data.keyword.openwhisk_short}} 提供的模板。
{: shortdesc}

## 可用的快速入门模板
{: #available-templates}

|名称|描述
|支持的运行时|
|:-----------------|:-----------------|:-----------------|
|[{{site.data.keyword.cloudant_short_notm}} Events](./deploy_templates.html#cloudant-template)|{{site.data.keyword.cloudant}} 编辑或添加了文档时，会在控制台中记录更改。|Node.js、Swift、Python 和 PHP|
|[Upload Image](./deploy_templates.html#cos-upload-image)|通过 Web 操作，可以将图像上传到 {{site.data.keyword.cos_full}} 实例的存储区，然后检索该图像的缩略图。|Node.js|
|[Get HTTP Resource](./deploy_templates.html#get-http-resource-template)|此 Web 操作在响应 HTTP 事件时调用，然后通过 Yahoo Weather API 访存数据。|Node.js 和 Python|
|[Hello World](./deploy_templates.html#hello-world-template)|此操作将接受单个参数，此参数必须是 JSON 对象。|Node.js、Swift、Python 和 PHP|
|[{{site.data.keyword.messagehub}} Events](./deploy_templates.html#messagehub-events-template)|{{site.data.keyword.messagehub_full}} 主题添加了新数据时，会在控制台中记录更改。|Node.js、Swift、Python 和 PHP|
|[Periodic Slack Reminder](./deploy_templates.html#slack-reminder-template)|此操作将根据定期触发器发布到 Slack。|Node.js、Swift、Python 和 PHP|

## 部署 {{site.data.keyword.cloudant_short_notm}} Events 模板
{: #cloudant-template}

{{site.data.keyword.cloudant_short_notm}} 模板会创建一序列操作以及用于启动该序列的触发器。连接的 {{site.data.keyword.cloudant_short_notm}} 数据库（应该是 cat 的数据库，具有名称和颜色）中发生更改时，会触发此触发器。期望的数据项是定义了名称和颜色的 cat。向数据库添加新 cat 或编辑现有 cat 时，会将数据记录到控制台。

1. 要创建模板，请转至 [{{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)，然后单击**开始创建**。

2. 单击**快速入门模板**。

3. 单击 **Cloudant Events**。

### 创建 {{site.data.keyword.cloudant_short_notm}} 操作

1. 接下来，提供包的名称或使用提供的缺省名称 `new-cloudant-item`。

2. 在**操作**下拉列表下，为您将拥有的操作选择运行时（nodejs、swift、python 或 php）。对于此示例，选择 **nodejs**，然后单击**下一步**。

### 创建 {{site.data.keyword.cloudant_short_notm}} 触发器

触发器从事件源收到事件时会调用操作。要创建 {{site.data.keyword.cloudant_short_notm}} 模板的触发器，请为触发器提供必需的 {{site.data.keyword.cloudant_short_notm}} 服务实例信息。

#### 创建 {{site.data.keyword.cloudant_short_notm}} 服务实例

可以选择以下任一项：
  * **创建自己的实例**
  * **输入自己的凭证**

1. 对于此示例，请选择**创建自己的实例**。

2. 这将打开一个弹出窗口，使您转至 {{site.data.keyword.cloudant_short_notm}} 设置页面的新选项卡。创建 {{site.data.keyword.cloudant_short_notm}} 实例后，必须创建一组服务凭证，然后通过单击**确定**关闭该选项卡以返回到此页面。

3. 现在，选择**输入自己的凭证**并提供以下信息：
  * 用户名 - _您的 {{site.data.keyword.cloudant_short_notm}} 用户名_
  * 密码 - _您的 {{site.data.keyword.cloudant_short_notm}} 密码_
  * 主机 - _这通常是您的 `username.{{site.data.keyword.cloudant_short_notm}}.com`_
  * 数据库 - _您的 {{site.data.keyword.cloudant_short_notm}} 数据库的名称_

### 部署 {{site.data.keyword.cloudant_short_notm}} 模板

单击**部署**。

部署模板后，您可以进一步对代码进行编辑以根据需要对其进行定制，或者返回并查看可用模板的目录。

## 部署 Upload Image 模板
{: #cos-upload-image}

Upload Image 模板会创建一个 Web 操作，允许您通过小型接口将图像上传到 {{site.data.keyword.cos_short_notm}} 存储区。然后，该模板会将该图像作为缩略图进行检索，并在 Web 操作的界面上显示该图像。

要部署模板，请执行以下操作：

1. 转至 [{{site.data.keyword.Bluemix_notm}} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/openwhisk/) 中的 {{site.data.keyword.openwhisk_short}} 控制台。

2. 单击**开始创建**。

2. 单击**快速入门模板**。

3. 单击 **Upload Image** 模板。

4. 输入包的名称或使用缺省名称 `upload-image`。

5. 单击**下一步**。

6. 模板需要来自 {{site.data.keyword.cos_full_notm}} 服务实例的服务凭证。在 **{{site.data.keyword.cos_short}}** 列表中，选择下列其中一个选项：
  * **创建新实例**：如果您没有现有服务实例，请选择此选项以创建服务实例。
      1. 在打开的 {{site.data.keyword.cos_full_notm}} 服务实例创建页面中，创建服务实例。
      2. [创建一组 HMAC 服务凭证](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials)。
      3. [至少创建一个存储区](/docs/services/cloud-object-storage/getting-started.html#create-buckets)。
  * **输入您自己的凭证**：选择此选项以手动输入您自己的 {{site.data.keyword.cos_short}} 服务实例凭证。凭证必须具有 HMAC 密钥，并且服务实例必须至少具有一个存储区。
  * **现有实例**：如果您有任何现有的 {{site.data.keyword.cos_short}} 实例，请从列表中选择其中一个实例。凭证必须具有 HMAC 密钥，并且服务实例必须至少具有一个存储区。

7. 单击**部署**。

8. 在左侧导航中，单击**端点**。

9. 在“Web 操作”部分中，复制不带 .json 后缀的链接，并将其粘贴到浏览器的地址栏中。这将显示用于模板的 Web 操作的接口。

10. 可选：部署模板后，可以浏览至“操作”仪表板来定制两个新包中的代码：
    * `cloud-object-storage` 包，其中包含使用 {{site.data.keyword.cos_short}} 实例的操作
    * 模板包（缺省名称 `upload-image`），其中包含 `app` 操作

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

## 部署 {{site.data.keyword.messagehub}} Events 模板
{: #messagehub-events-template}

{{site.data.keyword.messagehub}} Events 模板将创建操作以及用于触发该操作的触发器。只要向创建模板期间选择的 {{site.data.keyword.messagehub}} 主题添加了新项，就会触发该触发器。

1. 要创建模板，请转至 [{{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)，然后单击**开始创建**。

2. 单击**快速入门模板**。

3. 查看**包名称**字段，您可以根据需要对其进行更新。缺省值设置为 `message-hub-events`。

4. 为您将拥有的操作选择运行时：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。

5. 单击**下一步**。

### 创建 {{site.data.keyword.messagehub}} 触发器

触发器从事件源收到事件时会调用操作。要创建 {{site.data.keyword.messagehub}} 模板的触发器，请为触发器提供必需的 {{site.data.keyword.messagehub}} 服务实例信息。

查看**触发器名称**字段，您可以根据需要对其进行更新。缺省值设置为 `message-hub-events-trgr`。

### 创建 {{site.data.keyword.messagehub}} 服务实例

可以选择以下任一项：
  * **创建自己的实例**
  * **输入自己的凭证**

1. 对于此示例，请选择**创建自己的实例**。

2. 这将打开一个弹出窗口，使您转至具有 {{site.data.keyword.messagehub}} 设置页面的新选项卡。创建 {{site.data.keyword.messagehub}} 实例后，必须创建一组服务凭证，然后通过单击**确定**关闭该选项卡以返回到此页面。

3. 现在，选择**输入自己的凭证**并提供以下信息：
  * 用户名 - _您的 {{site.data.keyword.messagehub}} 用户名_
  * 密码 - _您的 {{site.data.keyword.messagehub}} 密码_
  * kafka_admin_url - _{{site.data.keyword.messagehub}} Admin REST URL_
  * 数据库 - _您的 {{site.data.keyword.messagehub}} 数据库的名称_
  * 主题 - _要预订的主题_

### 部署 {{site.data.keyword.messagehub}} 模板

单击**部署**。

部署模板后，您可以进一步对代码进行编辑以根据需要对其进行定制，或者返回并查看可用模板的目录。

## 部署 Periodic Slack Reminder 模板
{: #slack-reminder-template}

Periodic Slack Reminder 模板根据用户在触发器创建期间提供的时间间隔发布到 Slack。创建此模板之前，请转至 https://api.slack.com/incoming-webhooks 以设置所需的入局 Webhook URL。

1. 查看**包名称**字段，您可以根据需要对其进行更新。缺省值设置为 `periodic-slack-reminder`。

2. 为您将拥有的操作选择运行时：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。

3. 在**参数**部分下的**参数值**字段中，输入 Webhook URL，然后单击**下一步**。（示例：https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX）。

### 创建 Slack Reminder 触发器

触发器从事件源收到事件时会调用操作。要创建 Slack Reminder 模板的触发器，请为触发器提供必需的 {{site.data.keyword.messagehub}} 服务实例信息。

1. 查看**触发器名称**字段，您可以根据需要对其进行更新。缺省值设置为 `periodic-slack-reminder-trgr`。

2. 接下来，您可以使用模式或 Cron 表达式来指定触发器触发的时间间隔。可以为“周内日期”、“小时”和“分钟”选择 UTC 时间。选择所需的时间间隔选项后，您即准备就绪，可以进行模板部署。

3. 单击**部署**。

部署模板后，您可以进一步对代码进行编辑以根据需要对其进行定制，或者返回并查看可用模板的目录。
