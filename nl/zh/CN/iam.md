---
copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: access policies, iam, roles, functions

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



# 设置访问策略
{: #iam}

## 将 IAM 角色映射到 {{site.data.keyword.openwhisk_short}}
{: #user-roles}

在 {{site.data.keyword.openwhisk_short}} 中，名称空间是 {{site.data.keyword.cloud_notm}} 资源，可用于使用 IAM 角色和策略进行访问权管理。为名称空间设置的所有策略会同时应用于该名称空间包含的 {{site.data.keyword.openwhisk_short}} 实体（例如，操作或触发器）。
{: shortdesc}

{{site.data.keyword.openwhisk_short}} 同时使用平台和服务管理角色。您可以设置有关谁能在平台级别创建名称空间的策略，并使用服务角色来管理与名称空间本身的交互。

要了解有关 IAM 关键概念的更多信息吗？请查看 [IAM 文档](/docs/iam?topic=iam-iamconcepts#iamconcepts)。
{: tip}

</br>

### 平台管理角色

下表详细描述了与平台管理角色对应的操作。平台管理角色支持用户在平台级别对服务资源执行任务。例如，为用户分配对服务的访问权，创建或删除服务标识，创建实例以及将实例绑定到应用程序。
{: shortdesc}

有关如何分配、编辑、查看或删除资源访问策略的更多信息，请参阅[管理 IAM 访问权](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)。
{: tip}

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
      <td>用户可以创建名称空间。在创建服务期间，需要“管理员”角色来执行 `service id` 和 `apikey lock` 操作。</td>
    </tr>
  </tbody>
</table>

由于不需要供应服务，因此“编辑者”角色是您使用服务所需的唯一平台角色。有关其他角色的更多信息，请查看[平台管理角色](/docs/iam?topic=iam-userroles)。

</br>

### 特定于服务的角色
{: #service_specific_roles}

特定于服务的角色用于确定特定服务中访问策略的作用域。对于 {{site.data.keyword.openwhisk_short}}，角色可以应用于用户使用服务的能力，例如访问 UI 或执行 API 调用。
{: shortdesc}

许可权是基于彼此构建的。例如，`writer` 角色能够执行的任何操作，`manager` 角色也都能执行。但是，`manager` 角色将添加更多许可权。要查看每个角色的常规许可权，请查看[服务访问角色](/docs/iam?topic=iam-userroles)。

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

</br>

### 通过 CLI 设置策略
{: #cli-set}

要授予基于 IAM 的名称空间中资源（例如，操作）对基于 IAM 的服务的访问权，您可以为该资源所在的名称空间创建 IAM 访问策略。

```
ibmcloud iam service-policy-create <namespace_service_ID> --roles <IAM_role1,IAM_role2> --service-name <other_service_type> --service-instance <other_service_GUID>
```
{: pre}

<table>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 <code>ibmcloud iam service-policy-create</code> 命令的组成部分</th>
  </thead>
  <tbody>
    <tr>
      <td><code>&lt;namespace_service_ID&gt;</code></td>
      <td>名称空间的服务标识。要查看所有服务标识，请运行 <code>ibmcloud iam service-ids</code>。</td>
    </tr>
    <tr>
      <td>`--roles` <code>&lt;IAM_role&gt;</code></td>
      <td>操作要使用目标服务必须具有的 IAM 服务访问角色的类型。要查看支持用于其他服务的角色，请运行 <code>ibmcloud iam roles --service SERVICE_NAME</code>。有关更多信息，请参阅 [IAM 访问角色](/docs/iam?topic=iam-userroles#service-access-roles)。</td>
    </tr>
    <tr>
      <td>`--service-name` <code>&lt;other_service_type&gt;</code></td>
      <td>其他 {{site.data.keyword.cloud_notm}} 服务类型的名称。</td>
    </tr>
    <tr>
      <td>`--service-instance` <code>&lt;other_service_GUID&gt;</code></td>
      <td>您希望操作有权访问的其他服务实例的 GUID。要获取服务实例 GUID，请运行 <code>ibmcloud resource service-instance &lt;other_service_instance_name&gt;</code>。</td>
    </tr>
  </tbody>
</table>

</br>

**后续步骤**
有关管理服务凭证的更多信息，请参阅 [Manage service credentials for serverless applications](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external} 博客。



