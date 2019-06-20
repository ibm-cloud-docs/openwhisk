---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-17"

keywords: namespaces, iam, cloud foundry, classic namespaces

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



# 管理名称空间
{: #namespaces}

通过 {{site.data.keyword.openwhisk}}，可以创建 Identity and Access (IAM) 管理的名称空间，以将实体（例如，操作或触发器）分组在一起。然后，可以为该名称空间创建 IAM 访问策略。
{: shortdesc}


**什么是名称空间？**

名称空间包含 {{site.data.keyword.openwhisk_short}} 实体（例如，操作和触发器），并属于资源组。通过授予用户对名称空间的访问权，可以允许用户访问实体。

实体的标准名称为 `/namespaceName/[packageName]/entityName`。


**创建名称空间时会发生什么情况？**

在 {{site.data.keyword.openwhisk_short}} 中创建的名称空间时会识别为 IAM 服务实例。在创建名称空间期间，可以指定要在其中添加服务实例的[资源组](/docs/resources?topic=resources-rgs)。

创建名称空间时，将同时创建以下工件：

* 在发出出站调用时可用作功能标识的服务标识。在此名称空间中创建的所有操作都可以使用此服务标识来访问其他资源。要查看所有服务标识，请运行 `ibmcloud iam service-ids`。

* 用于服务标识的 API 密钥，此密钥可用于生成 IAM 令牌。然后，可以使用这些令牌向其他 {{site.data.keyword.Bluemix_notm}} 服务认证名称空间。API 密钥作为环境变量提供给操作。

    不要删除 API 密钥。
    {: tip}

**名称空间有任何限制吗？**

IAM 管理的名称空间不支持[使用 API 网关创建 API](/docs/openwhisk?topic=cloud-functions-apigateway)，也不支持使用[移动 SDK](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk)。

{{site.data.keyword.openwhisk_short}} 对名称空间的名称施加了限制。有关更多信息，请参阅[系统详细信息和限制](/docs/openwhisk?topic=cloud-functions-limits#limits_entities_ov)文档。
{: tip}



**如果我有基于 Cloud Foundry 的名称空间，该怎么办？**

基于 Cloud Foundry 的名称空间将继续正常运行。但是，为了利用新功能，您必须[将名称空间迁移到 IAM](/docs/resources?topic=resources-migrate)。

</br>


## 使用 CLI 创建名称空间
{: #namespaces_create}

可以创建 IAM 受管名称空间作为资源组的一部分，并通过在创建名称空间时将资源组设定为目标，从而管理资源的访问策略。如果有其他用户需要访问您的名称空间，或者您希望通过名称空间的操作来访问其他资源，请确保在创建名称空间后设置 IAM 策略。
{: shortdesc}

1. 将要在其中创建名称空间的资源组设定为目标。如果尚未创建[资源组](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create)，可以将 `default` 组设定为目标。

  ```
    ibmcloud target -g default
    ```
  {: pre}

2. 创建启用 IAM 的名称空间。

  ```
    ibmcloud fn namespace create <namespace_name> [-n <description>]
    ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;namespace_name&gt;</code></td>
        <td>基于 IAM 的名称空间的显示名称。</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>可选：向名称空间添加描述，例如将包含哪种类型的操作或包。</td>
      </tr>
    </tbody>
  </table>

  示例输出：

  ```
ok: created namespace myNamespace
    ```
  {: screen}

3. 验证新的名称空间是否已创建。

  ```
  ibmcloud fn namespace get <namespace_name_or_id> --properties
  ```
  {: pre}

  示例输出：

  ```
    Details of namespace: 'myNamespace'
    Description: 'short description'
    Resource Plan Id: 'functions-base-plan'
    Location: 'jp-tok'
    ID: '05bae599-ead6-4ccb-9ca3-94ce8c8b3e43'
    ```
  {: screen}

  此外，还可以列出所有名称空间，包括基于 IAM 的名称空间和基于 Cloud Foundry 的名称空间：
    

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. 在名称空间中创建实体之前，请通过将该名称空间设定为目标，将 CLI 上下文设置为该名称空间。
    

  ```
    ibmcloud fn property set --namespace <namespace_name_or_id>
    ```
  {: pre}

</br>

## 使用 API 创建名称空间
{: #namespaces_create_api}

可以创建 IAM 受管名称空间作为资源组的一部分，并通过在创建名称空间时将资源组设定为目标，从而管理资源的访问策略。如果有其他用户需要访问您的名称空间，或者您希望通过名称空间的操作来访问其他资源，请确保在创建名称空间后设置 IAM 策略。
{: shortdesc}


1. 将要在其中创建名称空间的资源组设定为目标。如果尚未创建[资源组](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create)，可以将 `default` 组设定为目标。

  ```
    ibmcloud target -g default
    ```
  {: pre}

2. 创建启用 IAM 的名称空间。

  ```
    curl --request POST \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>' \
    --data '{"description":"string","name":"string","resource_group_id":"string","resource_plan_id":"string"}'
    ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;IAM_token&gt;</code></td>
        <td>{{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) 令牌。要检索 IAM 令牌，请运行 <code>ibmcloud iam oauth-tokens</code>。</td>
      </tr>
      <tr>
        <td><code>-n &lt;name&gt;</code></td>
        <td>名称空间的名称。</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_group_id&gt;</code></td>
        <td>要在其中创建名称空间的资源组的标识。要查看资源组标识，请运行 <code>ibmcloud resource groups</code>。</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_plan_id&gt;</code></td>
        <td>资源计划的标识，例如 functions-base-plan。</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>可选：向名称空间添加描述，例如将包含哪种类型的操作或包。</td>
      </tr>
    </tbody>
  </table>

  示例输出：

  ```
  {
    "description": "My new namespace for packages X, Y, and Z.",
      "id": "12345678-1234-abcd-1234-123456789abc",
      "location": "jp-tok",
      "crn": "crn:v1:functions:jp-tok:a/1a22bb3c44dd1a22bb3c44dd1a22:12345678-1234-abcd-1234-123456789abc::",
      "name": "mynamespace",
      "resource_group_id": "1a22bb3c44dd1a22bb3c44dd1a22",
      "resource_plan_id": "functions-base-plan"
    }
    ```
  {: screen}

3. 验证新的名称空间是否已创建。

  ```
    curl --request GET \
      --url 'https://us-south.functions.cloud.ibm.com/api/servicebroker/api/v1/namespaces/{id} \
      --header 'accept: application/json' \
      --header 'authorization: <IAM_token>'
    ```
  {: pre}

  此外，还可以列出所有名称空间，包括基于 IAM 的名称空间和基于 Cloud Foundry 的名称空间：
    
  ```
  curl --request GET \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces?limit=0&offset=0' \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>'
  ```
  {: pre}

  示例输出：
  ```
  {
    "limit": 10,
      "offset": 0,
      "total_Count": 2,
      "namespaces": [
        {
          "id": "12345678-1234-abcd-1234-123456789abc",
          "location": "jp-tok"
        },
      {
        "id": "BobsOrg_dev",
          "classic_type": 1,
          "location": "jp-tok"
        }
      ]
  }
  ```
  {: screen}


有关使用 HTTP REST 的更多信息，请查看 [{{site.data.keyword.openwhisk_short}} API 文档](/apidocs/functions)。
{: tip}



## 后续步骤
{: #namespaces_next}

既然您已创建了名称空间，现在可以创建 IAM 访问策略来帮助保护该名称空间。首先，请查看[管理访问权](/docs/openwhisk?topic=cloud-functions-iam)。有关如何管理基于 IAM 的名称空间的更多信息，请参阅 [{{site.data.keyword.openwhisk_short}} REST API 参考](/apidocs/functions)。


