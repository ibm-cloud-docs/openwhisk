---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Cloudant 事件源
{: #openwhisk_cloudant}

了解如何侦听对 {{site.data.keyword.cloudant}} 数据库的更改，过滤数据库更改事件，并使用操作序列来处理 {{site.data.keyword.cloudant_short_notm}} 数据库中的文档。`/whisk.system/cloudant` 包支持您使用 {{site.data.keyword.cloudant_short_notm}} 数据库，并包含以下操作和订阅源：

|实体|类型|参数|描述
|
| --- | --- | --- | --- |
|`/whisk.system/cloudant`|包|dbname、host、username 和 password|使用 Cloudant 数据库。|
|`/whisk.system/cloudant/read`|操作|dbname 和 id|从数据库中读取文档。|
|`/whisk.system/cloudant/write`|操作|dbname、overwrite 和 doc|将文档写入数据库。|
|`/whisk.system/cloudant/changes`|订阅源|dbname、filter、query_params、maxTriggers|对数据库进行更改时触发触发器事件。|
{: shortdesc}

以下各部分将逐步指导您配置关联的包以及如何使用 `/whisk.system/cloudant` 包中的操作和订阅源。有关设置 {{site.data.keyword.cloudant_short_notm}} 数据库以及对其进行读写操作的更多信息，请参阅 [{{site.data.keyword.cloudant_short_notm}} 操作](./cloudant_actions.html)。

## 使用过滤函数创建触发器

可以使用 `changes` 订阅源来配置服务，以在每次对 {{site.data.keyword.cloudant_short_notm}} 数据库进行更改时都触发触发器。

此示例中使用的参数如下所示：

**dbname**：{{site.data.keyword.cloudant_short_notm}} 数据库的名称_（必需）_。

**maxTriggers**：达到此限制时，停止触发触发器_（可选）_。缺省值为无限。

**filter**：在设计文档上定义的过滤函数_（可选）_。

**query_params**：过滤函数的额外查询参数_（可选）_。

1. 使用先前创建的包绑定中的 `changes` 订阅源来创建名为 **myCloudantTrigger** 的触发器。包含 `filter` 和 `query_params` 函数，以在添加（或修改）文档且状态为 `new` 时触发触发器。

  确保将 `/_/myCloudant` 替换为您的包名。
  ```
  ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes \
  --param dbname testdb \
  --param filter "mailbox/by_status" \
  --param query_params '{"status":"new"}'
  ```
  {: pre}

  示例输出：
  ```
ok: created trigger feed myCloudantTrigger
  ```
  {: screen}

2. 开始对激活进行轮询，以清楚地了解发生的情况。
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

3. 创建可用于观察更改订阅源的影响的操作。例如，创建名为 **showCloudantChange** 的操作，并包含以下 JavaScript 代码：
  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

4. 创建规则，用于将 **showCloudantChange** 操作连接到先前创建的触发器：
  ```
  ibmcloud fn rule update aCloudantRule myCloudantTrigger showCloudantChange
  ```
  {: pre}

5. 创建操作以及用于将操作与 **myCloudantTrigger** 触发器相关联的规则。

6. 在 {{site.data.keyword.cloudant_short_notm}} 仪表板中，修改现有文档或创建新文档。该文档应该具有 _status_ 字段且设置为 **new**。

7. 针对每次文档更改，仅在文档状态根据过滤函数和查询参数为 **new** 时，才会观察到 **myCloudantTrigger** 触发器的新激活。

如果无法观察到新的激活，请参阅 [{{site.data.keyword.cloudant_short_notm}}](./cloudant_actions.html) 主题，其中演示了如何对 {{site.data.keyword.cloudant_short_notm}} 数据库执行读写操作。测试读写步骤以帮助验证 {{site.data.keyword.cloudant_short_notm}} 凭证是否正确。
{: tip}

## 触发器事件的数据结构

生成的事件的内容具有以下参数：

  - `id`：文档标识。
  - `seq`：{{site.data.keyword.cloudant_short_notm}} 生成的序列标识。
  - `changes`：一组对象，其中每个对象都有 `rev` 字段，用于包含文档的修订版标识。

触发器事件的 JSON 表示如下所示：
```json
{
    "dbname": "testdb",
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

可以选择定义过滤函数，以避免不必要的更改事件触发触发器。

要创建新的过滤函数，可以使用操作。

创建 JSON 文档文件 `design_doc.json` 并包含以下过滤函数：
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

在数据库上创建设计文档并包含以下过滤函数：
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
```
{: pre}

新设计文档的信息会在显示在屏幕上：
```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}

## 使用操作序列和更改触发器处理 {{site.data.keyword.cloudant_short_notm}} 数据库中的文档
{: #openwhisk_catalog_cloudant_read_change notoc}

可以使用规则中的操作序列来访存和处理与 {{site.data.keyword.cloudant_short_notm}} 更改事件关联的文档。

下面是处理文档的操作的样本代码：
```javascript
function main(doc){
  return { "isWalter:" : doc.name === "Walter White"};
}
```
{: codeblock}

创建用于处理 {{site.data.keyword.cloudant_short_notm}} 中文档的操作：
```
ibmcloud fn action create myAction myAction.js
```
{: pre}

要从数据库中读取文档，可以使用 {{site.data.keyword.cloudant_short_notm}} 包中的 `read` 操作。
`read` 操作可与 `myAction` 一起编写以创建操作序列。
```
ibmcloud fn action create sequenceAction --sequence /_/myCloudant/read,myAction
```
{: pre}

`sequenceAction` 操作可以在对新 {{site.data.keyword.cloudant_short_notm}} 触发器事件激活该操作的规则中使用。
```
ibmcloud fn rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**注**：{{site.data.keyword.cloudant_short_notm}} `changes` 触发器以前支持 `includeDoc` 参数，但现在不再支持该参数。

您可以重新创建之前使用 `includeDoc` 创建的触发器。执行以下步骤来重新创建触发器：
```
ibmcloud fn trigger delete myCloudantTrigger
```
{: pre}

```
ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
```
{: pre}

此示例可用于创建操作序列，以读取更改后的文档并调用现有操作。请务必禁用不再有效的任何规则，然后使用操作序列模式创建新规则。
