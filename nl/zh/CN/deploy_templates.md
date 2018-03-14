---

copyright:

  years: 2018

lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# 在 {{site.data.keyword.openwhisk_short}} 中部署模板
{: #serviceauth}

{{site.data.keyword.openwhisk}} 提供了模板目录来帮助您快速开始使用下一个项目。模板是操作、触发器和序列的组合，还可以包含 {{site.data.keyword.Bluemix}} 中的服务实例。通过使用模板，您可以快速、轻松地创建项目，然后立即开始编码。 

本教程将引导您逐渐部署 Cloudant 模板。
{: shortdesc}

## 可用模板
{: #available-templates}

| 名称| 描述
| 支持的运行时| 
|:-----------------|:-----------------|:-----------------|
| Cloudant Events| Cloudant DB 中编辑或添加了文档时，会在控制台中记录更改。| Node.js、Swift、Python 和 PHP|
| Get HTTP Resource| 此 Web 操作在响应 HTTP 事件时调用，然后通过 Yahoo Weather API 访存数据。| Node.js 和 Python|
| Hello World| 此操作将接受单个参数，此参数必须是 JSON 对象。| Node.js、Swift、Python 和 PHP|
| Message Hub Events| Message Hub 主题添加了新数据时，会在控制台中记录更改。| Node.js、Swift、Python 和 PHP| 
| Periodic Slack Reminder| 此操作将根据定期触发器发布到 Slack。| Node.js、Swift、Python 和 PHP|

## 部署 Cloudant Events 模板
{: #cloudant-template}

Cloudant 模板会创建一序列操作以及用于启动该序列的触发器。连接的 Cloudant 数据库（应该是 cat 的数据库，具有名称和颜色）中发生更改时，会触发此触发器。期望的数据项是定义了名称和颜色的 cat。向数据库添加新 cat 或编辑现有 cat 时，会将数据记录到控制台。

1. 要创建模板，请转至 [{{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.openwhisk_short}}](https://dev-console.stage1.bluemix.net/openwhisk/)，然后单击**开始创建**。 

2. 单击**部署模板**。

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

1. 单击**部署**。

部署模板后，您可以进一步对代码进行编辑以根据需要对其进行定制，或者返回并查看可用模板的目录。

