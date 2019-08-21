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



# 設定存取原則
{: #iam}

## 將 IAM 角色對映至 {{site.data.keyword.openwhisk_short}}
{: #user-roles}

在 {{site.data.keyword.openwhisk_short}} 中，名稱空間是 {{site.data.keyword.cloud_notm}} 資源，可用於使用 IAM 角色和原則進行存取權管理。您為名稱空間設定的所有原則，也會套用至名稱空間所包含的 {{site.data.keyword.openwhisk_short}} 實體（例如動作或觸發程式）。
{: shortdesc}

{{site.data.keyword.openwhisk_short}} 同時使用「平台」及「服務」管理角色。您可以設定有關誰能在平台層次建立名稱空間的原則，並使用服務角色來管理與名稱空間本身的互動。

要進一步瞭解 IAM 主要概念嗎？請參閱 [IAM 文件](/docs/iam?topic=iam-iamconcepts#iamconcepts)。
{: tip}

</br>

### 平台管理角色

下表詳細說明對映至平台管理角色的動作。平台管理角色可讓使用者在平台層次對服務資源執行作業。例如，指派使用者對服務的存取權、建立或刪除服務 ID、建立實例，以及將實例連結至應用程式。
{: shortdesc}

如需如何指派、編輯、檢閱或刪除資源存取原則的相關資訊，請參閱[管理 IAM 存取權](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)。
{: tip}

<table>
  <thead>
    <tr>
      <th>平台角色</th>
      <th>說明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>管理者</td>
      <td>使用者可以建立名稱空間。在建立服務期間，需要「管理者」角色來執行 `service id` 和 `apikey lock` 作業。</td>
    </tr>
  </tbody>
</table>

由於不需要佈建服務，因此「編輯者」角色是您使用服務所需的唯一平台角色。如需其他角色的相關資訊，請參閱[平台管理角色](/docs/iam?topic=iam-userroles)。

</br>

### 服務特定的角色
{: #service_specific_roles}

服務特定的角色用於確定特定服務中存取原則的範圍。針對 {{site.data.keyword.openwhisk_short}}，角色可以套用至可使用服務的使用者，例如存取使用者介面，或執行 API 呼叫。
{: shortdesc}

許可權建置於彼此的基礎上。例如，`writer` 角色可以執行的任何作業，`manager` 角色也可以執行。不過，`manager` 角色會加上更多許可權。若要查看每個角色的一般許可權，請參閱[服務存取角色](/docs/iam?topic=iam-userroles)。

若要查看執行每項作業所需的角色，請參閱下表：

<table><caption>哪些角色可以執行哪些作業？</caption>
  <tr>
    <th style="width:150px">動作</th>
    <th style="width:2500px">說明</th>
    <th style="width:50px">讀者</th>
    <th style="width:50px">撰寫者</th>
    <th style="width:50px">管理員</th>
  </tr>
  <tr>
    <td><code>functions.namespaces.update</code></td>
    <td>更新名稱空間。</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.delete</code></td>
    <td>刪除名稱空間。</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.read</code></td>
    <td>檢視可用的名稱空間。</td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.create</code></td>
    <td>在 Functions 名稱空間內建立實體，例如動作。</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.update</code></td>
    <td>在 Functions 名稱空間內更新實體，例如套件。</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.delete</code></td>
    <td>從 Functions 名稱空間中刪除實體，例如觸發程式。</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.read</code></td>
    <td>檢視名稱空間中的可用實體（例如，規則）。</td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.activate</code></td>
    <td>在名稱空間內啟動實體，例如動作。</td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="可用的特性" style="width:32px;" /></td>
  </tr>
</table>

</br>

### 透過 CLI 設定原則
{: #cli-set}

若要授與以 IAM 為基礎的名稱空間中資源（例如動作），對以 IAM 為基礎之服務的存取權，您可以為該資源所在的名稱空間建立 IAM 存取原則。

```
ibmcloud iam service-policy-create <namespace_service_ID> --roles <IAM_role1,IAM_role2> --service-name <other_service_type> --service-instance <other_service_GUID>
```
{: pre}

<table>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="「構想」圖示"/> 瞭解 <code>ibmcloud iam service-policy-create</code> 指令的組成部分</th>
  </thead>
  <tbody>
    <tr>
      <td><code>&lt;namespace_service_ID&gt;</code></td>
      <td>名稱空間的服務 ID。若要查看所有服務 ID，請執行 <code>ibmcloud iam service-ids</code>。</td>
    </tr>
    <tr>
      <td>`--roles` <code>&lt;IAM_role&gt;</code></td>
      <td>動作要使用目標服務必須具有的 IAM 服務存取角色的類型。若要查看支援用於其他服務的角色，請執行 <code>ibmcloud iam roles --service SERVICE_NAME</code>。如需相關資訊，請參閱 [IAM 存取角色](/docs/iam?topic=iam-userroles#service-access-roles)。</td>
    </tr>
    <tr>
      <td>`--service-name` <code>&lt;other_service_type&gt;</code></td>
      <td>其他 {{site.data.keyword.cloud_notm}} 服務類型的名稱。</td>
    </tr>
    <tr>
      <td>`--service-instance` <code>&lt;other_service_GUID&gt;</code></td>
      <td>您希望動作有權存取的其他服務實例的 GUID。若要取得服務實例 GUID，請執行 <code>ibmcloud resource service-instance &lt;other_service_instance_name&gt;</code>。</td>
    </tr>
  </tbody>
</table>

</br>

**後續步驟**
如需管理服務認證的相關資訊，請參閱 [Manage service credentials for serverless applications](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external} 部落格。



