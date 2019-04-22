---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-29"

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

对于 {{site.data.keyword.openwhisk_short}}，IAM 策略仅在东京区域可用。如果您是在东京区域运行，那么必须使用 IAM 策略来控制访问权。
{: tip}

</br>

## 将 IAM 角色映射到 {{site.data.keyword.openwhisk_short}}
{: #user-roles}

在 {{site.data.keyword.openwhisk_short}} 中，名称空间被视为 IBM Cloud 资源，允许您使用 IAM 角色和策略进行访问权管理。为名称空间设置的所有策略会同时应用于该名称空间包含的 {{site.data.keyword.openwhisk_short}} 实体（例如，操作或触发器）。
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


## 设置 IAM 访问策略
{: #set-iam}

服务调用操作时，该操作会有响应。由于响应是从名称空间或操作发送到服务的，因此会将其视为出站信息。如果要限制名称空间对其他服务的影响程度，您可能希望创建访问策略。
{: shortdesc}

有关如何分配、编辑、查看或删除资源访问策略的信息，请参阅[管理 IAM 访问权](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)。
{: tip}




</br>
</br>

## 访问名称空间中的其他资源
{: #namespace-access}

可以使用 IAM 令牌访问 IAM 受管名称空间中的其他资源。令牌表示认证，并验证资源的身份。访问 IAM 受管服务或资源时，需要 IAM 令牌进行认证。
{: shortdesc}

与通过用户标识来识别用户类似，服务标识用于表示特定资源。这意味着可以将 IAM 策略应用于那些管理访问许可权的资源。就像用户一样，资源也必须进行认证来验证其身份。在 Functions 内，操作的实现可利用此功能来访问其他服务或资源。

创建新的 IAM 受管名称空间时，Functions 会自动创建对应的服务标识来识别名称空间和 API 密钥。在运行时，Cloud Functions 会将 API 密钥作为环境变量 `__OW_IAM_NAMESPACE_API_KEY` 的值传递到操作代码。操作代码可以使用此 API 密钥来生成 IAM 令牌。大多数支持的 SDK（例如，Cloudant、Watson 和 COS）都使用 IAM 密钥本身进行认证。其他使用 REST API 的 IAM 受管服务或资源将使用从 IAM 密钥派生的令牌进行认证。

不十分确定 API 密钥和令牌如何匹配？请在 [IAM 文档](/docs/iam?topic=iam-iamapikeysforservices)中了解更多信息。
