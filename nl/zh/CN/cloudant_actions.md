---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: cloudant, database, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Cloudant 包
{: #cloudant_actions}

`/whisk.system/cloudant` 包支持您使用 [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) 数据库，并包含以下操作和订阅源：

|实体|类型|参数|描述
|
| --- | --- | --- | --- |
|`/whisk.system/cloudant`|包|dbname、host、username 和 password|使用 Cloudant 数据库。|
|`/whisk.system/cloudant/read`|操作|dbname 和 id|从数据库中读取文档。|
|`/whisk.system/cloudant/write`|操作|dbname、overwrite 和 doc|将文档写入数据库。|
|`/whisk.system/cloudant/changes`|订阅源|dbname、iamApiKey、iamUrl、filter、query_params、maxTriggers|对数据库进行更改时触发触发器事件。|
{: shortdesc}

以下各部分将逐步指导您设置 {{site.data.keyword.cloudant_short_notm}} 数据库以及如何对其进行读写。有关如何将订阅源与 `/whisk.system/cloudant` 包配合使用的更多信息，请参阅 [{{site.data.keyword.cloudant_short_notm}} 事件源](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant)。

## 在 {{site.data.keyword.Bluemix_notm}} 中设置 {{site.data.keyword.cloudant_short_notm}} 数据库
{: #cloudantdb_cloud}

如果使用的是 {{site.data.keyword.Bluemix_notm}} 的 {{site.data.keyword.openwhisk}}，那么可以使用 [{{site.data.keyword.openwhisk}} CLI 插件](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli)将服务绑定到操作或包。

必须首先为 {{site.data.keyword.cloudant_short_notm}} 帐户手动创建包绑定。

1. 创建为 {{site.data.keyword.cloudant_short_notm}} 帐户配置的包绑定。
  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. 验证包绑定是否存在。
  ```
  ibmcloud fn package list
  ```
  {: pre}

  示例输出：
  ```
  packages
  /myNamespace/myCloudant private
  ```
  {: screen}
  
3. 获取要绑定到操作或包的服务实例的名称。
    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    示例输出：
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

    示例输出：
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

    示例输出：
    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
        {
            "key": "bluemixServiceName",
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

    在此示例中，Cloudant 服务的凭证属于名为 `__bx_creds` 的参数。
  

## 从 {{site.data.keyword.cloudant_short_notm}} 数据库进行读取
{: #cloudant_read}

可以使用操作从名为 **testdb** 的 {{site.data.keyword.cloudant_short_notm}} 数据库中访存文档。确保此数据库在 {{site.data.keyword.cloudant_short_notm}} 帐户中存在。

- 使用先前创建的包绑定中的 **read** 操作来访存文档。确保将 `/_/myCloudant` 替换为您的包名。
  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
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
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
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
