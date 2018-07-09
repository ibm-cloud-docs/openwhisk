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

# Cloudant
{: #cloudant_actions}

`/whisk.system/cloudant` 包支持您使用 [{{site.data.keyword.cloudant}}](/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant) 数据库，并包含以下操作和订阅源：

|实体|类型|参数|描述
|
| --- | --- | --- | --- |
|`/whisk.system/cloudant`|包|dbname、host、username 和 password|使用 Cloudant 数据库。|
|`/whisk.system/cloudant/read`|操作|dbname 和 id|从数据库中读取文档。|
|`/whisk.system/cloudant/write`|操作|dbname、overwrite 和 doc|将文档写入数据库。|
|`/whisk.system/cloudant/changes`|订阅源|dbname、filter、query_params、maxTriggers|对数据库进行更改时触发触发器事件。|
{: shortdesc}

以下各部分将逐步指导您设置 {{site.data.keyword.cloudant_short_notm}} 数据库以及如何对其进行读写。有关如何将订阅源与 `/whisk.system/cloudant` 包配合使用的更多信息，请参阅 [{{site.data.keyword.cloudant_short_notm}} 事件源](./openwhisk_cloudant.html)。

## 在 {{site.data.keyword.Bluemix_notm}} 中设置 {{site.data.keyword.cloudant_short_notm}} 数据库
{: #cloudantdb_cloud}

如果是在 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk}}，那么 {{site.data.keyword.openwhisk_short}} 将为 {{site.data.keyword.cloudant_short_notm}} 服务实例自动创建包绑定。如果不是在 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk_short}} 和 {{site.data.keyword.cloudant_short_notm}}，请跳至下一部分。

1. 在 [{{site.data.keyword.Bluemix_notm}} 仪表板](http://console.bluemix.net)中创建 {{site.data.keyword.cloudant_short_notm}} 服务实例。

  请确保为每个新服务实例创建一个凭证密钥。

2. 刷新名称空间中的包。刷新操作将自动为已定义凭证密钥的每个 {{site.data.keyword.cloudant_short_notm}} 服务实例创建包绑定。
  ```
  ibmcloud wsk package refresh
  ```
  {: pre}

  示例输出：
  ```
  created bindings:
  Bluemix_testCloudant_Credentials-1
  ```
  {: screen}

  ```
  ibmcloud wsk package list
  ```
  {: pre}

  示例输出：
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 private binding
  ```
  {: screen}

  现在，包绑定包含与 {{site.data.keyword.cloudant_short_notm}} 服务实例关联的凭证。

3. 检查以确认先前创建的包绑定是否已配置为使用 {{site.data.keyword.cloudant_short_notm}} {{site.data.keyword.Bluemix_notm}} 服务实例主机和凭证。

  ```
  ibmcloud wsk package get /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 parameters
  ```
  {: pre}

  示例输出：
  ```
  ok: got package /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1, displaying field parameters

  [
      {
          "key": "username",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix"
      },
      {
          "key": "host",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix.cloudant.com"
      },
      {
          "key": "password",
          "value": "c9088667484a9ac827daf8884972737"
      }
      ]
  ```
  {: screen}

## 在 {{site.data.keyword.Bluemix_notm}} 外部设置 {{site.data.keyword.cloudant_short_notm}} 数据库
{: #cloudantdb_nocloud}

如果不是在 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk_short}}，或者如果要在 {{site.data.keyword.Bluemix_notm}} 外部设置 {{site.data.keyword.cloudant_short_notm}} 数据库，那么必须为 {{site.data.keyword.cloudant_short_notm}} 帐户手动创建包绑定。您需要 {{site.data.keyword.cloudant_short_notm}} 帐户主机名、用户名和密码。

1. 创建为 {{site.data.keyword.cloudant_short_notm}} 帐户配置的包绑定。
  ```
  wsk package bind /whisk.system/cloudant myCloudant -p username MYUSERNAME -p password MYPASSWORD -p host MYCLOUDANTACCOUNT.cloudant.com
  ```
  {: pre}


2. 验证包绑定是否存在。
  ```
  wsk package list
  ```
  {: pre}

  示例输出：
  ```
packages
  /myNamespace/myCloudant private binding
  ```
  {: screen}

## 从 {{site.data.keyword.cloudant_short_notm}} 数据库进行读取
{: #cloudant_read}

可以使用操作从名为 **testdb** 的 {{site.data.keyword.cloudant_short_notm}} 数据库中访存文档。确保此数据库在 {{site.data.keyword.cloudant_short_notm}} 帐户中存在。

- 使用先前创建的包绑定中的 **read** 操作来访存文档。确保将 `/_/myCloudant` 替换为您的包名。
  ```
  ibmcloud wsk action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}

  示例输出：
  ```
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```
  {: screen}

## 写入 {{site.data.keyword.cloudant_short_notm}} 数据库
{: #cloudant_write}

可以使用操作将文档存储在名为 **testdb** 的 {{site.data.keyword.cloudant_short_notm}} 数据库中。确保此数据库在 {{site.data.keyword.cloudant_short_notm}} 帐户中存在。

1. 使用先前创建的包绑定中的 **write** 操作来存储文档。确保将 `/_/myCloudant` 替换为您的包名。
  ```
  ibmcloud wsk action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  {: pre}

  示例输出：
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. 通过在 {{site.data.keyword.cloudant_short_notm}} 仪表板中浏览以查找该文档，验证该文档是否存在。

  **testdb** 数据库的仪表板 URL 类似于以下内容：`https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100`。
