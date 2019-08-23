---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: namespaces, iam, cloud foundry, classic namespaces, functions

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


# 管理名称空间
{: #namespaces}

通过 {{site.data.keyword.openwhisk}}，可以创建 Identity and Access (IAM) 管理的名称空间，以将实体（例如，操作或触发器）分组在一起。然后，可以为该名称空间创建 IAM 访问策略。有关 IAM 的概述，请参阅 [{{site.data.keyword.openwhisk_short}} 启用 IAM 的公告博客](https://www.ibm.com/cloud/blog/ibm-cloud-functions-is-now-identity-and-access-management-enabled){: external}。
{: shortdesc}

## 什么是名称空间？

名称空间包含 {{site.data.keyword.openwhisk_short}} 实体（例如，操作和触发器），并属于资源组。通过授予用户对名称空间的访问权，可以允许用户访问实体。

实体的标准名称为 `/namespaceName/packageName/entityName`。

### 创建名称空间时会发生什么情况？

在 {{site.data.keyword.openwhisk_short}} 中创建的名称空间时会识别为 IAM 服务实例。在创建名称空间期间，可以指定要在其中添加服务实例的[资源组](/docs/resources?topic=resources-rgs)。

创建名称空间时，将创建以下组件：

|组件|描述|
| --- | --- | 
|服务标识|在发出出站调用时，可将服务标识用作功能标识。在此名称空间中创建的所有操作都可以使用此服务标识来访问其他资源。缺省情况下，功能用户将获得“读取者”角色。“读取者”访问权表示它可以读取名称空间实体并调用操作。“读取者”角色由触发器用于调用操作。为了控制入站流量，您可能希望向其他用户授予访问权，例如分配“读取者”角色以调用操作。|
|API 密钥|可用于生成 IAM 令牌的服务标识的 API 密钥。可以使用这些令牌向其他 {{site.data.keyword.cloud_notm}} 服务认证名称空间。API 密钥作为环境变量 `__OW_IAM_NAMESPACE_API_KEY` 提供给操作。|

查看所有服务标识。
```
ibmcloud iam service-ids
```
{: pre}

查看与服务标识关联的 API 密钥。 
```
ibmcloud iam service-api-keys <ServiceID-12345678-1234-abcd-1234-123456789abc>
```
{: pre}

</br>

不要删除 API 密钥。
{: tip}

### 名称空间有任何限制吗？

IAM 管理的名称空间不支持[使用 API 网关创建 API](/docs/openwhisk?topic=cloud-functions-apigateway)，也不支持[移动 SDK](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk)。 

包括操作、触发器、规则、包和名称空间在内的所有实体的名称均为遵循以下格式的字符序列：
* 第一个字符必须为字母数字字符或下划线。
* 后续字符可以为字母数字、空格或以下任一值：`_`、`@`、`.` 和 `-`。
* 最后一个字符不能为空格。

### 如果我有基于 Cloud Foundry 的名称空间，该怎么办？

基于 Cloud Foundry 的名称空间将继续正常运行。但是，为了利用新功能，您必须创建启用 IAM 的名称空间。


## 在 UI 中创建基于 IAM 的名称空间
{: #create_iam_ui}

1. 在 [{{site.data.keyword.openwhisk_short}} 控制台](https://cloud.ibm.com/openwhisk){: external}中，单击“名称空间”下拉菜单。

2. 单击**创建名称空间**。

3. 输入名称空间的显示名称和简短描述，例如计划在此名称空间中创建的操作或包的类型。

4. 选择要在其中创建名称空间的资源组和要在其中部署名称空间资源的位置。

5. 单击**创建**。

6. 要查看名称空间资源的服务实例，请转至 [{{site.data.keyword.cloud_notm}} 仪表板](https://cloud.ibm.com/resources){: external}，然后在 **Functions 名称空间**窗格中查找名称空间名称。

如果需要，可以在 {{site.data.keyword.openwhisk_short}} 控制台中的**名称空间设置**页面上更新名称空间的名称或描述。

## 使用 CLI 创建基于 IAM 的名称空间
{: #namespaces_create}

可以创建 IAM 管理的名称空间以作为资源组的一部分，并通过在创建名称空间时将该资源组设定为目标，从而管理资源的访问策略。如果有其他用户需要访问您的名称空间，或者您希望通过名称空间的操作来访问其他资源，请确保在创建名称空间后设置 IAM 策略。
{: shortdesc}

1. 将要在其中创建名称空间的资源组设定为目标。如果尚未创建[资源组](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create)，可以将 `default` 组设定为目标。

  ```
    ibmcloud target -g default
    ```
  {: pre}

2. 创建启用 IAM 的名称空间。可选：使用 `-n` 或 `--description` 标志来包含名称空间的描述。如果描述长度多于一个词，那么必须用引号将描述括起。

  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description of your namespace">]
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
        <td>可选：向名称空间添加描述，例如计划创建哪种类型的操作或包。如果描述长度多于一个词，那么必须用引号将描述括起。</td>
      </tr>
      <tr>
        <td><code>--description &lt;description&gt;</code></td>
        <td>可选：向名称空间添加描述，例如计划创建哪种类型的操作或包。如果描述长度多于一个词，那么必须用引号将描述括起。</td>
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
  Details of namespace: myNamespace
  Description: short description
  Resource Plan Id: functions-base-plan
  Location: jp-tok
  ID: 05bae599-ead6-4ccb-9ca3-94ce8c8b3e43
  ```
  {: screen}

  此外，还可以列出所有名称空间，包括基于 IAM 的名称空间和基于 Cloud Foundry 的名称空间：
    

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. 在名称空间中创建实体之前，必须通过将该名称空间设定为目标，将 CLI 上下文设置为该名称空间。

  ```
    ibmcloud fn property set --namespace <namespace_name_or_id>
    ```
  {: pre}

设置属性（如 `--namespace` 属性）之后，将保留该属性，直到您手动将其取消设置为止。如果要在 IAM 名称空间之间或在 Cloud Foundry 与 IAM 之间进行切换，那么必须取消设置 namespace 属性并将其重置。有关更多信息，请参阅 [`ibmcloud fn property set`]
{: note}

## 使用 API 创建名称空间
{: #namespaces_create_api}

可以创建 IAM 管理的名称空间以作为资源组的一部分，并通过在创建名称空间时将该资源组设定为目标，从而管理资源的访问策略。如果有其他用户需要访问您的名称空间，或者您希望通过名称空间的操作来访问其他资源，请确保在创建名称空间后设置 IAM 策略。
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
        <td>{{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) 令牌。要检索 IAM 令牌，请运行 <code>ibmcloud iam oauth-tokens</code>。</td>
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

  **示例输出**

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

  **示例输出**

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

### 访问名称空间中的其他资源
{: #namespace-access}

操作通常会调用需要相应认证的其他 {{site.data.keyword.cloud_notm}} 资源和服务。如果这些服务已启用 IAM 并接受 IAM 令牌，那么可以利用名称空间的功能标识进行出站通信。
{: shortdesc}

如[管理 IAM 访问权](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)中所述，对于每个名称空间，都会创建一个服务标识来表示该名称空间。通过使用 IAM 策略管理来分配相应的角色，可以向此服务标识授予对其他服务和资源的访问权。有关创建服务标识以访问其他启用 IAM 的服务的更多信息，请参阅[创建和使用服务标识](/docs/iam?topic=iam-serviceids#serviceids)。

在运行时，{{site.data.keyword.openwhisk_short}} 会将名称空间服务标识的 API 密钥作为环境变量 `__OW_IAM_NAMESPACE_API_KEY` 传递到操作代码。操作代码可以使用此 API 密钥来生成 IAM 令牌。大多数支持的 {{site.data.keyword.openwhisk_short}} SDK（例如，Cloudant、{{site.data.keyword.watson}} 和 {{site.data.keyword.cos_full_notm}}）都使用 IAM API 密钥本身进行认证。对于其他使用 REST API 的 IAM 管理的服务或资源，可以使用从 IAM API 密钥派生的令牌进行认证。有关更多信息，请参阅[为用户或服务标识创建 IAM 访问令牌](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i)。

不确定 API 密钥和令牌如何匹配？请在 [IAM 文档](/docs/iam?topic=iam-iamapikeysforservices)中了解更多信息。

## 后续步骤
{: #namespaces_next}

既然您已创建了名称空间，现在可以创建 IAM 访问策略来帮助保护该名称空间。首先，请查看[管理访问权](/docs/openwhisk?topic=cloud-functions-iam)。 

有关如何管理基于 IAM 的名称空间的更多信息，请参阅 [{{site.data.keyword.openwhisk_short}} REST API 参考](/apidocs/functions)。








