---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: deploy, deployment templates, templates, example, quickstart

subcollection: cloud-functions

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 部署快速入门模板
{: #serviceauth}

{{site.data.keyword.openwhisk}} 提供了模板目录来帮助您快速开始使用下一个项目。模板是操作、触发器和序列的组合。某些模板还包含 {{site.data.keyword.Bluemix_notm}} 中的其他服务实例。通过使用模板，您可以快速、轻松地创建项目，然后立即开始编码。
{: shortdesc}

## 可用的快速入门模板
{: #available-templates}

|名称|描述|支持的运行时|
|:-----------------|:-----------------|:-----------------|
|[{{site.data.keyword.cloudant_short_notm}} Events](/docs/openwhisk?topic=cloud-functions-serviceauth#cloudant-template)|在 {{site.data.keyword.cloudantfull}} 数据库中编辑或添加了文档时，在控制台中记录此更改。|Node.js、Swift、Python 和 PHP|
|[Upload Image](/docs/openwhisk?topic=cloud-functions-serviceauth#cos-upload-image)|使用 Web 操作将图像上传到 {{site.data.keyword.cos_full}} 实例的存储区，然后检索该图像的缩略图。|Node.js|
|[Get HTTP Resource](/docs/openwhisk?topic=cloud-functions-serviceauth#get-http-resource-template)|使用 HTTP 事件调用 Web 操作，然后通过 Yahoo Weather API 获取数据。|Node.js 和 Python|
|[Hello World](/docs/openwhisk?topic=cloud-functions-serviceauth#hello-world-template)|创建接受 JSON 对象作为单个参数的基本操作。|Node.js、Swift、Python 和 PHP|
|[{{site.data.keyword.messagehub}} Events](/docs/openwhisk?topic=cloud-functions-serviceauth#messagehub-events-template)|在 {{site.data.keyword.messagehub_full}} 数据库中添加了新数据时，在控制台中记录此更改。|Node.js、Swift、Python 和 PHP|
|[Periodic Slack Reminder](/docs/openwhisk?topic=cloud-functions-serviceauth#slack-reminder-template)|根据定期触发器，使用 Webhook 访问 Slack。|Node.js、Swift、Python 和 PHP|

## 部署 {{site.data.keyword.cloudant_short_notm}} Events 模板
{: #cloudant-template}

{{site.data.keyword.cloudant_short_notm}} 模板会创建操作序列以及用于启动该序列的触发器。在已连接的 {{site.data.keyword.cloudant_short_notm}} 示例数据库 cats 中进行更改时，将触发触发器。期望的数据项是定义了名称和颜色的猫。向数据库添加新的猫或编辑当前的猫时，会将这些数据记录到控制台。

### 通过 UI 部署 {{site.data.keyword.cloudant_short_notm}} Events 模板

1. 转至 {{site.data.keyword.openwhisk_short}} 控制台中的[“创建”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/create)。

2. 单击**快速入门模板**。

3. 单击 **Cloudant Events**。

4. 创建 {{site.data.keyword.cloudant_short_notm}} 操作。
    1. 提供包的名称或使用提供的缺省名称 `new-cloudant-item`。
    2. 在**操作**列表中，为 cats 数据库示例选择 `Node.js 6`，然后单击**下一步**。

5. 创建 {{site.data.keyword.cloudant_short_notm}} 触发器。触发器从事件源收到事件时会调用操作。要创建触发器，必须提供 {{site.data.keyword.messagehub}} 服务实例和服务凭证。
    1. 在 **Cloudant 实例**列表中，选择**创建您自己的实例**。这将打开 {{site.data.keyword.cloudant_short_notm}} 设置页面。
    2. 创建 {{site.data.keyword.cloudant_short_notm}} 服务实例。
    3. 创建一组服务凭证。
    4. 单击**确定**以关闭 {{site.data.keyword.cloudant_short_notm}} 设置页面并返回到 {{site.data.keyword.openwhisk_short}} 控制台。
    5. 在 **Cloudant 实例**列表中，现在可以选择**输入您自己的凭证**，并提供以下信息：
      * 用户名：{{site.data.keyword.cloudant_short_notm}} 用户名
      * 密码：{{site.data.keyword.cloudant_short_notm}} 密码
      * 主机：`<username>.cloudant.com`
      * 数据库：{{site.data.keyword.cloudant_short_notm}} 数据库的名称

5. 单击**部署**。部署模板后，您可以进一步对代码进行编辑以根据需要对其进行定制，或者返回并查看可用模板的目录。

### 通过 CLI 部署 {{site.data.keyword.cloudant_short_notm}} Events 模板

1. 克隆模板存储库。
    ```
    git clone https://github.com/ibm-functions/template-cloudant-trigger.git
    ```
    {: pre}

2. 导航至要使用的操作运行时的目录：`nodejs-6`、`nodejs`、`php`、`python` 或 `swift`。
    ```
    cd template-cloudant-trigger/runtimes/nodejs
    ```
    {: pre}

3. 使用以下环境变量来部署模板。
    ```
    CLOUDANT_HOSTNAME=<host> CLOUDANT_USERNAME=<username> CLOUDANT_PASSWORD=<password> CLOUDANT_DATABASE=<database> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>了解环境变量</caption>
    <thead>
    <th colspan=2>了解环境变量</th>
    </thead>
    <tbody>
    <tr><td><code>CLOUDANT_HOSTNAME</code></td><td><code>&lt;username&gt;.cloudant.com</code></td></tr>
    <tr><td><code>CLOUDANT_USERNAME</code></td><td>{{site.data.keyword.cloudant_short_notm}} 用户名</td></tr>
    <tr><td><code>CLOUDANT_PASSWORD</code></td><td>{{site.data.keyword.cloudant_short_notm}} 密码</td></tr>
    <tr><td><code>CLOUDANT_DATABASE</code></td><td>{{site.data.keyword.cloudant_short_notm}} 数据库的名称</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>包的定制名称</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>规则的定制名称</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>触发器的定制名称</td></tr>
    </tbody></table>

## 部署 Upload Image 模板
{: #cos-upload-image}

Upload Image 模板会创建一个 Web 操作，允许您通过小型界面将图像上传到 {{site.data.keyword.cos_short}} 存储区。然后，该模板会将该图像作为缩略图进行检索，并在 Web 操作的界面上显示该图像。

### 通过 UI 部署 Upload Image 模板

1. 转至 {{site.data.keyword.openwhisk_short}} 控制台中的[“创建”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/create)。

2. 单击**快速入门模板**。

3. 单击**上传图像**。

4. 输入包的名称或使用缺省名称 `upload-image`，然后单击 **下一步**。

6. 模板需要来自 {{site.data.keyword.cos_full_notm}} 服务实例的服务凭证。在 **{{site.data.keyword.cos_short}}** 列表中，选择下列其中一个选项：
  * **创建新实例**：如果您没有现有服务实例，请选择此选项以创建服务实例。
      1. 在打开的 {{site.data.keyword.cos_full_notm}} 服务实例创建页面中，创建服务实例。
      2. [创建一组 HMAC 服务凭证](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials)。
      3. [至少创建一个存储区](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-create-buckets)。
  * **输入您自己的凭证**：选择此选项以手动输入您自己的 {{site.data.keyword.cos_short}} 服务实例凭证。凭证必须具有 HMAC 密钥，并且服务实例必须至少具有一个存储区。
  * **现有实例**：如果您有任何现有的 {{site.data.keyword.cos_short}} 实例，请从列表中选择其中一个实例。凭证必须具有 HMAC 密钥，并且服务实例必须至少具有一个存储区。

7. 单击**部署**。

8. 在左侧导航中，单击**端点**。

9. 在 **Web 操作**部分中，复制不带 .json 后缀的链接，并将其粘贴到浏览器的地址栏中。这将显示用于模板的 Web 操作的接口。

10. 可选：部署模板后，可以导航至**操作**仪表板来定制两个新包中的代码：
    * `cloud-object-storage` 包，其中包含使用 {{site.data.keyword.cos_short}} 实例的操作
    * 模板包（缺省名称 `upload-image`），其中包含 `app` 操作

### 通过 CLI 部署 Upload Image 模板

1. [安装 {{site.data.keyword.cos_full_notm}} 包](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions#cloud_object_storage_cli)。

2. 克隆模板存储库。
    ```
    git clone https://github.com/ibm-functions/template-cloud-object-storage.git
    ```
    {: pre}

3. 导航至 `nodejs` 运行时目录。
        ```
    cd template-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. 部署模板，并将定制包名和 {{site.data.keyword.cos_short}} 存储区的名称用作环境变量。
    ```
    PACKAGE_NAME=<name> BUCKET_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. 在 {{site.data.keyword.openwhisk_short}} 控制台的[“操作”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/actions) 中，单击 `process-change` 操作。

5. 在左侧导航中，单击**端点**。

6. 在 **Web 操作**部分中，复制不带 .json 后缀的链接，并将其粘贴到浏览器的地址栏中。这将显示用于模板的 Web 操作的接口。

## 部署 Get HTTP Resource 模板
{: #get-http-resource-template}

Get HTTP Resource 模板创建用于访存外部资源 Yahoo Weather API 的操作，然后返回数据。该操作作为 Web 操作启用，从而允许使用启用了 CORS 的 URL 来调用该操作，而无需认证密钥，这对于为 Web 应用程序构建后端非常有用。**注**：缺省情况下，`get-http-resource` 端点对于任何要调用该端点的用户都公开可用。

### 通过 UI 部署 Get HTTP Resource 模板

1. 转至 {{site.data.keyword.openwhisk_short}} 控制台中的[“创建”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/create)。

2. 单击**快速入门模板**。

3. 单击 **Get HTTP Resource**。

3. 输入包的名称或使用缺省名称 `get-http-resource`。

4. 为操作选择运行时：Node.js 8、Node.js 6 或 Python 3。

5. 单击**部署**。

6. 通过对以下 URL 运行 curl 命令来调用操作：`https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`。例如：
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

部署模板后，您可以进一步对代码进行编辑以根据需要对其进行定制，或者返回并查看可用模板的目录。

### 通过 CLI 部署 Get HTTP Resource 模板

1. 克隆模板存储库。
    ```
    git clone https://github.com/ibm-functions/template-get-external-resource.git
    ```
    {: pre}

2. 导航至要使用的操作运行时的目录：`nodejs-6`、`nodejs` 或 `python`。
    ```
    cd template-get-external-resource/runtimes/nodejs
    ```
    {: pre}

3. 部署模板，并将定制包名用作环境变量。
    ```
    PACKAGE_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. 通过对以下 URL 运行 curl 命令来调用操作：`https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`。例如：
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

## 部署 Hello World 模板
{: #hello-world-template}

可以部署此基本 Hello World 操作，以开始使用 {{site.data.keyword.openwhisk_short}} 或测试创建的其他实体，例如触发器和规则。

### 通过 UI 部署 Hello World 模板

1. 转至 {{site.data.keyword.openwhisk_short}} 控制台中的[“创建”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/create)。

2. 单击**快速入门模板**。

3. 单击 **Hello World**。

4. 输入包的名称或使用缺省名称 `hello-world`。

5. 为操作选择运行时：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。

6. 单击**部署**。

部署模板后，您可以进一步对代码进行编辑以根据需要对其进行定制，或者返回并查看可用模板的目录。

### 通过 CLI 部署 Hello World 模板

1. 克隆 Hello World 模板存储库。
    ```
    git clone https://github.com/ibm-functions/template-hello-world.git
    ```
    {: pre}

2. 导航至要使用的操作运行时的目录：`nodejs-6`、`nodejs`、`php`、`python` 或 `swift`。
    ```
    cd template-hello-world/runtimes/nodejs
    ```
    {: pre}

3. 部署模板。
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

## 部署 {{site.data.keyword.messagehub}} Events 模板
{: #messagehub-events-template}

{{site.data.keyword.messagehub}} Events 模板将创建操作以及用于触发该操作的触发器。只要向创建模板期间选择的 {{site.data.keyword.messagehub}} 主题添加了新项，就会触发该触发器。

### 通过 UI 部署 {{site.data.keyword.messagehub}} Events 模板

1. 转至 {{site.data.keyword.openwhisk_short}} 控制台中的[“创建”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/create)。

2. 单击**快速入门模板**。

3. 单击 **{{site.data.keyword.messagehub}} Events**。

4. 创建 {{site.data.keyword.messagehub}} 操作。
    1. 提供包的名称或使用提供的缺省名称 `message-hub-events`。
    2. 为操作选择运行时：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。
    3. 单击**下一步**。

5. 创建 {{site.data.keyword.messagehub}} 触发器。触发器从事件源收到事件时会调用操作。要创建触发器，必须提供 {{site.data.keyword.messagehub}} 服务实例和服务凭证。
    1. 在 **MessageHub 实例**列表中，选择**创建您自己的实例**。这将打开 {{site.data.keyword.messagehub}} 创建页面。
    2. 创建 {{site.data.keyword.messagehub}} 服务实例。
    3. 创建一组服务凭证。
    4. 单击**确定**以关闭 {{site.data.keyword.messagehub}} 创建页面并返回到 {{site.data.keyword.messagehub}} 控制台。
    5. 在 **MessageHub 实例**列表中，现在可以选择**输入您自己的凭证**，并提供以下信息：
      * 用户名：{{site.data.keyword.messagehub}} 用户名
      * 密码：{{site.data.keyword.messagehub}} 密码
      * kafka_admin_url：{{site.data.keyword.messagehub}} 管理 REST URL
      * 数据库：{{site.data.keyword.messagehub}} 数据库的名称
      * 主题：要预订的主题

5. 单击**部署**。

部署模板后，您可以进一步对代码进行编辑以根据需要对其进行定制，或者返回并查看可用模板的目录。

### 通过 CLI 部署 {{site.data.keyword.messagehub}} Events 模板

1. 克隆模板存储库。
    ```
    git clone https://github.com/ibm-functions/template-messagehub-trigger.git
    ```
    {: pre}

2. 导航至要使用的操作运行时的目录：`nodejs-6`、`nodejs`、`php`、`python` 或 `swift`。
    ```
    cd template-messagehub-trigger/runtimes/nodejs
    ```
    {: pre}

3. 使用以下环境变量来部署模板。
    ```
    KAFKA_BROKERS=<host> KAFKA_TOPIC=<topic> MESSAGEHUB_USER=<username> MESSAGEHUB_PASS=<password> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>了解环境变量</caption>
    <thead>
    <th colspan=2>了解环境变量</th>
    </thead>
    <tbody>
    <tr><td><code>KAFKA_BROKERS</code></td><td>{{site.data.keyword.messagehub}} 管理 REST URL</td></tr>
    <tr><td><code>KAFKA_TOPIC</code></td><td>要预订的主题</td></tr>
    <tr><td><code>MESSAGEHUB_USER</code></td><td>您的 {{site.data.keyword.messagehub}} 用户名</td></tr>
    <tr><td><code>MESSAGEHUB_PASS</code></td><td>您的 {{site.data.keyword.messagehub}} 密码</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>包的定制名称</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>规则的定制名称</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>触发器的定制名称</td></tr>
    </tbody></table>

## 部署 Periodic Slack Reminder 模板
{: #slack-reminder-template}

Periodic Slack Reminder 模板根据用户在触发器创建期间提供的时间间隔发布到 Slack。

### 通过 UI 部署 Periodic Slack Reminder 模板

1. 转至 https://api.slack.com/incoming-webhooks 以设置所需的入局 Webhook URL。

1. 转至 {{site.data.keyword.openwhisk_short}} 控制台中的[“创建”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/create)。

2. 单击**快速入门模板**。

3. 单击 **Periodic Slack Reminder Events**。

4. 创建 Periodic Slack Reminder 操作。
    1. 提供包的名称或使用提供的缺省名称 `periodic-slack-reminder`。
    2. 为操作选择运行时：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。
    3. 在“参数”部分中，在**参数值**字段中输入 Webhook URL，例如 `https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX`。
    4. 单击**下一步**。

5. 创建 Periodic Slack Reminder 触发器。触发器从事件源收到事件时会调用操作。
    1. 使用模式或 Cron 表达式来指定触发器的时间间隔。
        * 模式：为“周内日期”、“小时”和“分钟”选择 UTC 时间。
        * Cron：指定基于 <a href="http://crontab.org">UNIX crontab 语法</a>的 Cron 序列。使用的字段不超过 5 个，并且字段之间用空格分隔，格式为 `X X X X X`。
    2. 添加触发器 JSON 有效内容。

6. 单击**部署**。

部署模板后，您可以进一步对代码进行编辑以根据需要对其进行定制，或者返回并查看可用模板的目录。

### 通过 CLI 部署 Periodic Slack Reminder 模板

1. 转至 https://api.slack.com/incoming-webhooks 以设置所需的入局 Webhook URL。

1. 克隆模板存储库。
    ```
    git clone https://github.com/ibm-functions/template-reminder-slack.git
    ```
    {: pre}

2. 导航至要使用的操作运行时的目录：`nodejs-6`、`nodejs`、`php`、`python` 或 `swift`。
    ```
    cd template-reminder-slack/runtimes/nodejs
    ```
    {: pre}

3. 使用以下环境变量来部署模板。
    ```
    SLACK_WEBHOOK_URL=<url> ALARM_CRON=<cron> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>了解环境变量</caption>
    <thead>
    <th colspan=2>了解环境变量</th>
    </thead>
    <tbody>
    <tr><td><code>SLACK_WEBHOOK_URL</code></td><td>Webhook URL，例如 <code>https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX</code></td></tr>
    <tr><td><code>ALARM_CRON</code></td><td>基于 <a href="http://crontab.org">UNIX crontab 语法</a>的 Cron 序列。使用的字段不超过 5 个，并且字段之间用空格分隔，格式为 <code>X X X X X</code>。</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>包的定制名称</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>规则的定制名称</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>触发器的定制名称</td></tr>
    </tbody></table>
