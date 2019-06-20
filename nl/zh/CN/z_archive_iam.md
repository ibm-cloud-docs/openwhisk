---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-10"

keywords: iam, access managment, roles, service roles, policies, access

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
{:download: .download}


# 管理访问权
{: #iam}

{{site.data.keyword.openwhisk}} 支持 Identity and Access Management (IAM)。现在，可以为资源（例如，名称空间）定义 IAM 策略。
{: shortdesc}

</br>

## 将 IAM 角色映射到 {{site.data.keyword.openwhisk_short}}
{: #user-roles}

在 {{site.data.keyword.openwhisk_short}} 中，名称空间被视为 {{site.data.keyword.Bluemix_notm}} 资源，允许您使用 IAM 角色和策略进行访问权管理。为名称空间设置的所有策略会同时应用于该名称空间包含的 {{site.data.keyword.openwhisk_short}} 实体（例如，操作或触发器）。
{: shortdesc}

{{site.data.keyword.openwhisk_short}} 同时使用平台和服务管理角色。您可以设置有关谁能在平台级别创建名称空间的策略，同时使用服务角色来管理与名称空间本身的交互。

要了解有关 IAM 关键概念的更多信息吗？请查看 [IAM 文档](/docs/iam?topic=iam-iamconcepts#iamconcepts)。
{: tip}

</br>

### 平台管理角色

下表详细描述了与平台管理角色对应的操作。平台管理角色支持用户在平台级别对服务资源执行任务。例如，为用户分配对服务的访问权，创建或删除服务标识，创建实例以及将实例绑定到应用程序。
{: shortdesc}

<table>
  <thead>
    <tr>
      <th>平台角色</th>
      <th>描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>管理员</td>
      <td>用户能够创建名称空间。</td>
    </tr>
  </tbody>
</table>

您需要具有支持平台管理的管理员角色才能使用服务。有关角色的更多信息，请查看[平台管理角色](/docs/iam?topic=iam-userroles)。

</br>

### 特定于服务的角色
{: #service_specific_roles}

特定于服务的角色用于确定特定服务中访问策略的作用域。对于 {{site.data.keyword.openwhisk_short}}，角色可以应用于用户使用服务的能力，例如访问 UI 或执行 API 调用。
{: shortdesc}


请务必注意，这些许可权是基于彼此构建的。例如，`writer` 角色能够执行的任何操作，`manager` 角色也都能执行。但是，`manager` 角色将添加更多许可权。要查看每个角色的常规许可权，请查看[服务访问角色](/docs/iam?topic=iam-userroles)。

要了解执行每个操作所需的角色，请查看下表：

<table><caption>哪些角色可以执行哪些操作？</caption>
  <tr>
    <th style="width:150px">操作</th>
    <th style="width:2500px">描述</th>
    <th style="width:50px">读取者</th>
    <th style="width:50px">写入者</th>
    <th style="width:50px">管理者</th>
  </tr>
  <tr>
    <td><code>functions.namespaces.update</code></td>
    <td>更新名称空间。</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.delete</code></td>
    <td>删除名称空间。</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.read</code></td>
    <td>查看可用名称空间。</td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.create</code></td>
    <td>在 Functions 名称空间内创建实体（例如，操作）。</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.update</code></td>
    <td>更新 Functions 名称空间内的实体（例如，包）。</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.delete</code></td>
    <td>从 Functions 名称空间中删除实体（例如，触发器）。</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.read</code></td>
    <td>查看名称空间中的可用实体（例如，规则）。</td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.activate</code></td>
    <td>激活 Functions 名称空间内的实体（例如，操作）。</td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="功能可用" style="width:32px;" /></td>
  </tr>
</table>

有关在 UI 中分配用户角色的信息，请参阅[管理 IAM 访问权](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)。

</br>


## 为名称空间设置 IAM 访问策略
{: #set-iam}

管理名称空间或其中的实体时，可以使用上面列出的[特定于服务的角色](#service_specific_roles)向其他用户授予访问权。在创建名称空间期间，将创建服务标识，该标识表示名称空间以及功能用户标识。缺省情况下，会为功能用户标识分配“读取者”角色。“读取者”可以读取名称空间实体并调用操作。“读取者”角色由触发器用于调用操作。为了控制入站流量，您可能希望向其他用户授予访问权，例如分配“读取者”角色以调用操作。
{: shortdesc}

有关如何分配、编辑、查看或删除资源访问策略的信息，请参阅[管理 IAM 访问权](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)。
{: tip}




</br>
</br>

## 访问名称空间中的其他资源
{: #namespace-access}

操作通常会调用需要相应认证的其他 {{site.data.keyword.Bluemix_notm}} 资源和服务。如果这些服务已启用 IAM 并接受 IAM 令牌，那么可以利用名称空间的功能标识进行出站通信。如[管理 IAM 访问权](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)中所述，对于每个名称空间，都会创建一个服务标识来表示该名称空间。通过使用 IAM 策略管理来分配相应的角色，可以向此服务标识授予对其他服务和资源的访问权。

在运行时，{{site.data.keyword.openwhisk_short}} 会将名称空间服务标识的 API 密钥作为环境变量 `__OW_IAM_NAMESPACE_API_KEY` 的值传递到操作代码。操作代码可以使用此 API 密钥来生成 IAM 令牌。大多数支持的 {{site.data.keyword.openwhisk_short}} SDK（例如，Cloudant、{{site.data.keyword.watson}} 和 {{site.data.keyword.cos_full_notm}}）都使用 IAM API 密钥本身进行认证。对于其他使用 REST API 的 IAM 受管服务或资源，可以使用从 IAM API 密钥派生的令牌进行认证。有关更多信息，请参阅[为用户或服务标识创建 IAM 访问令牌](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i)。

不十分确定 API 密钥和令牌如何匹配？请在 [IAM 文档](/docs/iam?topic=iam-iamapikeysforservices)中了解更多信息。

