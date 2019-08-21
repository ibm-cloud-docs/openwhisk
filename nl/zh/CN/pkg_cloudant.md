---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: cloudant, event, action, trigger, sequence, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Cloudant
{: #pkg_cloudant}

通过预安装的 `/whisk.system/cloudant` 包，您可以使用 [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) 数据库。使用此包不需要任何服务绑定。
{: shortdesc}


## 可用实体
{: #cloudant_available}
下表显示了 `whisk.system/cloudant` 包中的一些可用实体。您可以使用 `whisk.system/cloudant` 包来读取、写入、更新或删除文档。还可以使用 `changes` 订阅源来侦听对 {{site.data.keyword.cloudant_short_notm}} 数据库的更改。
{: shortdesc}

有关 `/whisk.system/cloudant` 包中可用实体的完整列表，请运行 `ibmcloud fn package get /whisk.system/cloudant`。
{: note}

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|`/whisk.system/cloudant`|包|`dbname`、`host`、`username`、`password`|使用 Cloudant 数据库。|
|`/whisk.system/cloudant/read`|操作|`dbname`、`id`|从数据库中读取文档。|
|`/whisk.system/cloudant/write`|操作|`dbname`、`overwrite`、`doc`|将文档写入数据库。|
|`/whisk.system/cloudant/update-document`|操作|`dbname`、`doc`|更新数据库中的文档。|
|`/whisk.system/cloudant/changes`|订阅源|`dbname`、`iamApiKey`、`iamUrl`、`filter`、`query_params`、`maxTriggers`|对数据库进行更改时触发触发器事件。|

不再支持 `includeDoc` 参数用于 `/whisk.system/cloudant/changes` 订阅源。如果已创建使用此参数的触发器，那么必须在不使用 `includeDoc` 参数的情况下重新创建这些触发器。
{: deprecated}

## 将 `/whisk.system/cloudant` 包绑定到 {{site.data.keyword.cloudant_short_notm}} 数据库。
如果使用的是 {{site.data.keyword.cloud_notm}} 的 {{site.data.keyword.openwhisk}}，那么可以使用 {{site.data.keyword.openwhisk}} CLI 插件将服务绑定到操作或包。
{: #cloudant_db}

**开始之前**
您必须具有 {{site.data.keyword.cloudant_short_notm}} 实例。要创建实例，请参阅 [{{site.data.keyword.cloudant_short_notm}} 入门](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)。

1. 创建为 {{site.data.keyword.cloudant_short_notm}} 帐户配置的 `/whisk.system/cloudant` 包绑定。在此示例中，包名为 `myCloudant`。

  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. 验证包绑定是否存在。

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **示例输出**

  ```
  packages
  /<namespace>/myCloudant private
  ```
  {: screen}

3. 获取要绑定到操作或包的服务实例的名称。
    

    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    **示例输出**
    ```
    Name          Location   State    Type
    Cloudant-gm   us-south   active   service_instance
    ```
    {: screen}

4. 获取为您在上一步中获取的服务实例所定义的凭证的名称。
    

    ```
    ibmcloud resource service-keys --instance-name Cloudant-gm
    ```
    {: pre}

    **示例输出**

    ```
Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. 将服务绑定到步骤 1 中创建的包。

    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1' 
    ```
    {: pre}

6. 验证凭证是否已成功绑定。
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    **示例输出**

    ```
ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
        {
            "key": "serviceName",
                "value": "cloudantNoSQLDB"
            },
            {
                "key": "apihost",
                "value": "us-south.functions.cloud.ibm.com"
            },
            {
                "key": "__bx_creds",
            "value": {
                "cloudantnosqldb": {
                        "apikey": "[Service apikey]",
                        "credentials": "Service credentials-1",
                        "iam_apikey_description": "[Service description]",
                        "iam_apikey_name": "[Service apikey name]",
                        "iam_role_crn": "[Service role crn]",
                        "iam_serviceid_crn": "[Service id crn]",
                        "instance": "Cloudant-gm",
                        "url": "[Service url]",
                        "username": "[Service username]"
                    }
                }
            }
        ],
    }
    ```
    {: screen}

在此示例中，{{site.data.keyword.cloudant_short_notm}} 服务的凭证属于名为 `__bx_creds` 的参数。

## 使用 {{site.data.keyword.cloudant_short_notm}} 数据库中的文档
{: #cloudant_read}

可以使用操作来读取、写入、更新和删除 {{site.data.keyword.cloudant_short_notm}} 数据库中的文档。
{: shortdesc}

### 读取文档
可以使用 `/whisk.system/cloudant/read` 操作读取 {{site.data.keyword.cloudant_short_notm}} 数据库中的文档。

**开始之前**
如果 {{site.data.keyword.cloudant_short_notm}} 数据库中没有文档，那么可以使用 {{site.data.keyword.cloudant_short_notm}} 仪表板来创建文档。仪表板的 URL 为 `https://<mycloudantaccount>.cloudant.com/dashboard.html#database/<database_name>/_all_docs?limit=100`。

使用 `read` 操作访存文档。将 `/_/myCloudant` 替换为包名，将 `<database_name>` 替换为数据库名称，并将 `<document_id>` 替换为文件标识。调用操作以测试访存文档。

**命令语法**

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id <document_id>
  ```
  {: pre}

**对 `test` 数据库执行的示例读操作**
调用操作以测试读取文件。此示例读取 `id` 为 `9f86f4955e7a38ab0169462e6ac0f476` 的文件，这是一个空文件。

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id 9f86f4955e7a38ab0169462e6ac0f476
  ```
  {:pre}

**示例输出**
  ```
  {
      "_id": "9f86f4955e7a38ab0169462e6ac0f476",
      "_rev": "1-967a00dff5e02add41819138abb3284d"
  }
  ```
  {: screen}

### 将文档写入 {{site.data.keyword.cloudant_short_notm}} 数据库
{: #cloudant_write}

可以使用操作在 {{site.data.keyword.cloudant_short_notm}} 数据库中创建或更新文档。
{: shortdesc}

**开始之前**
  创建为 {{site.data.keyword.cloudant_short_notm}} 帐户配置的 `/whisk.system/cloudant` [包绑定](#cloudant_db)。

1. 使用已创建的包绑定中的 `write` 操作来存储文档。将 `/_/myCloudant` 替换为包名，将 `<database_name>` 替换为数据库名称，将 `<document_id>` 替换为文件标识，并将 `<test_name>` 替换为名称。

  **命令语法**
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param <database_name> test --param doc "{\"_id\":\"<document_id>\",\"name\":\"<test_name>\"}"
  ```
  {: pre}

  **对 `test` 数据库执行的示例写操作**

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\"}"
  ```
  {: pre}

  **示例输出**

  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "color",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. 验证文档在 {{site.data.keyword.cloudant_short_notm}} 仪表板中是否存在。`test` 数据库的仪表板 URL 的格式如下：`https://<mycloudantaccount>.cloudant.com/dashboard.html#database/test/_all_docs?limit=100`。

  **{{site.data.keyword.cloudant_short_notm}} 仪表板中的示例文档**
  ```
  {
  "_id": "color",
  "_rev": "1-f413f4b74a724e391fa5dd2e9c8e9d3f",
  "name": "blue"
  }
  ```
  {: screen}

### 更新文档
可以使用 `/update-document` 操作更新 {{site.data.keyword.cloudant_short_notm}} 数据库中的文档。
{: short desc}

**开始之前**
  创建为 {{site.data.keyword.cloudant_short_notm}} 帐户配置的 `/whisk.system/cloudant` [包绑定](#cloudant_db)。

以下示例更新在[将文档写入 {{site.data.keyword.cloudant_short_notm}} 数据库](#cloudant_write)部分中创建的文档。
{: note}

可以通过将 `<test>` 替换为数据库名称，并将 `--param doc` 标志替换为要更新的数据库中文档的 `id` 和内容，更新数据库中的文档。


1. 可以通过运行以下命令来更新 `test` 数据库中的文档。此示例将 `shade` 值添加到 `color` 文档。 

  ```
  ibmcloud fn action invoke /_/myCloudant/update-document --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}"
  ```
  {: pre}

  **输出**
  ```
  {
    "id": "color",
    "ok": true,
    "rev": "2-8b904347bfe52e0f388ef6f39d6ba84f"
    }
  ```
  {: screen}

2. 要查看更新，请重新访存该文档。

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id color
  ```
  {: pre}

  **示例文档**
  ```
  {
    "_id": "color",
    "_rev": "2-8b904347bfe52e0f388ef6f39d6ba84f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

## 使用过滤函数创建触发器
{: #cloudant_trigger}

可以使用 `changes` 订阅源来配置服务，以在每次对 {{site.data.keyword.cloudant_short_notm}} 数据库进行更改时都触发触发器。

**开始之前**
  创建为 {{site.data.keyword.cloudant_short_notm}} 帐户配置的 `/whisk.system/cloudant` [包绑定](#cloudant_db)。

此示例中使用的参数。

|参数|描述|
| --- | --- |
|`dbname`|（必需）{{site.data.keyword.cloudant_short_notm}} 数据库的名称。|
|`iamApiKey`|（可选）Cloudant 数据库的 IAM API 密钥。如果指定，此值将用作凭证以取代用户名和密码。|
|`iamUrl`|（可选）指定 `iamApiKey` 时使用的 IAM 令牌服务 URL。缺省值为 `https://iam.cloud.ibm.com/identity/token`。| 
|`maxTriggers`|（可选）达到此限制时，停止触发触发器。缺省值为无限。|
|`filter`|（可选）在设计文档中定义的过滤函数。|
|`query_params`|（可选）过滤函数可能需要的任何其他查询参数。|
|`includeDoc`|（不推荐）不再支持 `includeDoc` 参数用于 `/whisk.system/cloudant/changes` 订阅源。|

</br>

1. 使用先前创建的包绑定中的 `changes` 订阅源来创建名为 `cloudantTrigger` 的触发器。包含 `filter` 和 `query_params` 函数，以在添加（或修改）文档且状态为 `new` 时触发触发器。

  将 `/_/myCloudant` 替换为包的名称。此示例使用的是名为 `test` 的数据库。
  ```
  ibmcloud fn trigger create cloudantTrigger --feed /_/myCloudant/changes \ --param dbname test
  ```
  {: pre}

  **示例输出**

  ```
  ok: created trigger feed cloudantTrigger
  ```
  {: screen}

2. 将以下 JavaScript 代码保存为 `cloudantChange.js`。

  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

3. 创建名为 `cloudantChange` 的操作，可用于观察 changes 订阅源。将 `<file_path>` 替换为计算机上 `cloudantChange.js` 文件的文件路径。

  ```
  ibmcloud fn action create cloudantChange <file_path>/cloudantChange.js
  ```
  {: pre}

4. 创建名为 `cloudantRule` 的规则，用于将 `cloudantChange` 操作连接到先前创建的 `cloudantTrigger`。

  ```
  ibmcloud fn rule create cloudantRule cloudantTrigger cloudantChange
  ```
  {: pre}

5. 在另一个终端窗口中，启动轮询，以便您可以看到激活何时发生。

  ```
  ibmcloud fn activation poll
  ```
  {: pre}

6. 在 {{site.data.keyword.cloudant_short_notm}} 仪表板中，修改现有文档或创建文档。

7. 观察针对每个文档更改的 `cloudantTrigger` 触发器的激活。

**`cloudantTrigger` 的示例激活**

```
Activation: 'cloudantTrigger' (ef6605cc05e04589a605cc05e04589d8)
[
    "{\"statusCode\":0,\"success\":true,\"activationId\":\"6067ed0d28774a68a7ed0d28771a684d\",\"rule\":\"<namespace>/cloudantRule\",\"action\":\"<namespace>/cloudantChange\"}"
]

Activation: 'cloudantChange' (6067ed0d28774a68a7ed0d28771a684d)
[
    "2019-06-24T16:46:10.428643Z    stdout: { changes: [ { rev: '19-f7f6d8607d6381d224321acfcfb8887e' } ],",
    "2019-06-24T16:46:10.428693Z    stdout: dbname: 'test',",
    "2019-06-24T16:46:10.428697Z    stdout: id: '6ca436c44074c4c2aa6a40c9a188b348',",
    "2019-06-24T16:46:10.428700Z    stdout: seq: '103-g1AAAAeLeJy91M9NwzAUBnCrrVQqDvTKCa4gpcT5YycnugFsAH5' }"
```
{: screen}

### 触发器激活的数据结构
{: #cloudant_struct}

生成的事件的内容具有以下参数。

|参数|描述|
| --- | --- |
|`id`|文档标识。|
|`seq`|由 {{site.data.keyword.cloudant_short_notm}} 生成的序列标识。|
|`changes`|对象数组，其中每个对象都有 `rev` 字段，用于包含文档的修订版标识。|

**触发器激活的 JSON 表示**

```json
{
    "dbname": "test",
    "id": "6ca436c44074c4c2aa6a40c9a188b348",
    "seq": "2-g1AAAAL9aJyV-GJCaEuqx4-BktQkYp_dmIfC",
    "changes": [
        {
            "rev": "2-da3f80848a480379486fb4a2ad98fa16"
          }
      ]
}
```
{: codeblock}

## 过滤数据库更改事件
{: #cloudant_filter}

可以定义过滤函数，以避免不必要的更改事件触发触发器。

**开始之前**
  创建为 {{site.data.keyword.cloudant_short_notm}} 帐户配置的 `/whisk.system/cloudant` [包绑定](#cloudant_db)。

要创建新的过滤函数，可以使用操作。

1. 将以下 JSON 过滤器保存在名为 `design_doc.json` 的文件中。

  ```json
  {
    "doc": {
    "_id": "_design/mailbox",
    "filters": {
      "by_status": "function(doc, req){if (doc.status != req.query.status){return false;} return true;}"
    }
  }
  }
  ```
  {: codeblock}

2. 在数据库中创建设计文档并包含以下过滤函数。将 `<database_name>` 替换为数据库的名称，并将 `<file_path>` 替换为 `design_doc.json` 的文件路径。调用 `write` 操作以测试创建设计文档。

**命令语法**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname <database_name> -p overwrite true -P <file_path>/design_doc.json
```
{: pre}

**用于将 `design_doc.json` 文件写入 `test` 数据库的示例命令**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname test -p overwrite true -P <file_path>/design_doc.json -r
```
{: pre}

**示例输出**

```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}


有关 {{site.data.keyword.cloudant_short_notm}} 设计文档的更多信息，请参阅[设计文档](/docs/services/Cloudant?topic=cloudant-design-documents)

## 使用操作序列处理单个文档
{: #cloudant_seq}

可以使用规则中的操作序列来访存并处理与 {{site.data.keyword.cloudant_short_notm}} 更改事件关联的文档。

**开始之前**
  创建为 {{site.data.keyword.cloudant_short_notm}} 帐户配置的 `/whisk.system/cloudant` [包绑定](#cloudant_db)。 

此示例更新在[将文档写入 {{site.data.keyword.cloudant_short_notm}} 数据库](#cloudant_write)部分中创建的文档。
{: note}

### 创建用于处理单个文档的操作

要创建用于处理对单个文档的更改的操作，请运行以下命令。
{: shortdesc}

1. 将以下代码保存为 `docChange.js`。

  ```javascript
  function main(doc){
    return { "isBlue:" : doc.name === "blue"};
  }
  ```
  {: codeblock}

2. 创建名为 `docChange` 的操作，用于处理先前创建的名称为 `blue` 的文档。将 `<file_path>` 替换为 `docChange.js` 的文件路径。

  ```
  ibmcloud fn action create docChange <file_path>/docChange.js
  ```
  {: pre}

  **输出**
  ```
  ok: created action docChange
  ```
  {: screen}

### 创建具有 `read` 操作的序列 

`read` 操作可与 `docChange` 一起编写以创建操作序列。
{: shortdesc}

  ```
  ibmcloud fn action create docSequence --sequence /_/myCloudant/read,docChange
  ```
  {: pre}

  **输出**
  ```
  ok: created action docSequence
  ```
  {: screen}

### 创建用于 `changes` 订阅源的触发器

  ```
  ibmcloud fn trigger create docTrigger --feed /_/myCloudant/changes \
  --param dbname test
  ```
  {: pre}

### 创建规则以将触发器与序列相关联

`docSequence` 操作可以在对新 {{site.data.keyword.cloudant_short_notm}} 触发器事件激活该操作的规则中使用。

  ```
  ibmcloud fn rule create docRule docTrigger docSequence
  ```
  {: pre}

  **输出**
  ```
  ok: created rule docRule
  ```

  **示例激活**
  ```
  "{\"statusCode\":0,\"success\":true,\"activationId\":\"144a4f95198a49ec8a4f95198a79ecc8\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ```
  {: screen}

### 测试序列

1. 通过对先前创建的 `blue` 文件进行更改来测试 `docSequence`。在此示例中，`shade` 值更改为 `indigo`。

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}" -p overwrite true
  ```
  {: pre}

  **示例激活**

  ```
  Activation: 'docChange' (aa3e8fc3030446b2be8fc3030406b2eb)
  []

  Activation: 'docSequence' (23e0a17bebd3486ca0a17bebd3186c8d)
  [
      "8d42679127f3400382679127f300039d",
      "aa3e8fc3030446b2be8fc3030406b2eb"
  ]

  Activation: 'docTrigger' (db6de778bb084366ade778bb08036685)
  [
      "{\"statusCode\":0,\"success\":true,\"activationId\":\"23e0a17bebd3486ca0a17bebd3186c8d\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ]
  ```
  {: screen}

2. 通过调用 `read` 操作来验证文件是否已更新为包含 `shade` 值。将 `<database>` 名称替换为数据库的名称。

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id color
  ```
  {: pre}

  **输出**
  ```
  {
    "_id": "color",
    "_rev": "3-6845b04618338f717676f16edf32a78f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

### 后续步骤
既然您在侦听对 {{site.data.keyword.cloudant_short_notm}} 数据库中文档的更改，现在可以使用 [`/whisk.system/slack` 包](/docs/openwhisk?topic=cloud-functions-pkg_slack)来触发与更改相关的 Slack 通知。


