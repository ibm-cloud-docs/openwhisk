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


# 管理存取權
{: #iam}

{{site.data.keyword.openwhisk}} 支援 Identity and Access Management (IAM)。您現在可以定義資源的 IAM 原則（例如名稱空間）。
{: shortdesc}

</br>

## 將 IAM 角色對映至 {{site.data.keyword.openwhisk_short}}
{: #user-roles}

在 {{site.data.keyword.openwhisk_short}} 中，名稱空間被視為 {{site.data.keyword.Bluemix_notm}} 資源，容許您使用 IAM 角色和原則進行存取權管理。您為名稱空間設定的所有原則，也會套用至名稱空間所包含的 {{site.data.keyword.openwhisk_short}} 實體（例如動作或觸發程式）。
{: shortdesc}

{{site.data.keyword.openwhisk_short}} 同時使用平台和服務管理角色。您可以設定有關誰可以在平台層次建立名稱空間的原則，同時使用服務角色來管理與名稱空間本身的互動。

要進一步瞭解 IAM 主要概念嗎？請參閱 [IAM 文件](/docs/iam?topic=iam-iamconcepts#iamconcepts)。
{: tip}

</br>

### 平台管理角色

下表詳細說明對映至平台管理角色的動作。平台管理角色可讓使用者在平台層次對服務資源執行作業。例如，指派使用者對服務的存取權、建立或刪除服務 ID、建立實例，以及將實例連結至應用程式。
{: shortdesc}

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
      <td>使用者可以建立名稱空間。</td>
    </tr>
  </tbody>
</table>

您需要具有平台管理的管理者角色，才能使用服務。如需角色的相關資訊，請參閱[平台管理角色](/docs/iam?topic=iam-userroles)。

</br>

### 服務特定角色
{: #service_specific_roles}

服務特定角色決定特定服務內的存取原則範圍。針對 {{site.data.keyword.openwhisk_short}}，角色可以套用至可使用服務的使用者，例如存取使用者介面，或執行 API 呼叫。
{: shortdesc}


請務必注意，許可權建置在彼此的基礎上。例如，`writer` 角色可以執行的任何作業，`manager` 角色也可以執行。不過，`manager` 角色會加上更多許可權。若要查看每個角色的一般許可權，請參閱[服務存取角色](/docs/iam?topic=iam-userroles)。

若要查看執行每項作業所需的角色，請參閱下表：

<table><caption>哪些角色可以執行哪些作業？</caption>
  <tr>
    <th style="width:150px">動作 (Action)</th>
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
    <td>檢視名稱空間內的可用實體，例如規則。</td>
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

如需在使用者介面中指派使用者角色的相關資訊，請參閱[管理 IAM 存取](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)。

</br>


## 為名稱空間設定 IAM 存取原則
{: #set-iam}

管理名稱空間或其中的實體時，可以使用上面列出的[特定於服務的角色](#service_specific_roles)向其他使用者授與存取權。在建立名稱空間期間，將建立服務 ID，該 ID 代表名稱空間以及功能使用者 ID。依預設，會為功能使用者 ID 指派「讀者」角色。「讀者」可以讀取名稱空間實體並呼叫動作。「讀者」角色由觸發程式用於呼叫動作。為了控制入埠資料流量，您可能希望向其他使用者授與存取權，例如指派「讀者」角色以呼叫動作。
{: shortdesc}

如需如何指派、編輯、檢閱或刪除資源存取原則的相關資訊，請參閱[管理 IAM 存取](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)。
{: tip}




</br>
</br>

## 存取名稱空間中的其他資源
{: #namespace-access}

動作通常會呼叫需要適當鑑別的其他 {{site.data.keyword.Bluemix_notm}} 資源和服務。如果這些服務已啟用 IAM 並接受 IAM 記號，則可以利用名稱空間的功能 ID 進行出埠通訊。如[管理 IAM 存取權](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)中所述，對於每個名稱空間，都會建立一個服務 ID 來代表該名稱空間。透過使用 IAM 原則管理來指派適當的角色，可以向此服務 ID 授與對其他服務和資源的存取權。

在運行環境，{{site.data.keyword.openwhisk_short}} 會將名稱空間服務 ID 的 API 金鑰作為環境變數 `__OW_IAM_NAMESPACE_API_KEY` 的值傳遞到動作碼。動作碼可以使用此 API 金鑰來產生 IAM 記號。大多數支援的 {{site.data.keyword.openwhisk_short}} SDK（例如，Cloudant、{{site.data.keyword.watson}} 和 {{site.data.keyword.cos_full_notm}}）都使用 IAM API 金鑰本身進行鑑別。對於其他使用 REST API 的 IAM 受管理服務或資源，可以使用從 IAM API 金鑰衍生的記號進行鑑別。如需相關資訊，請參閱[為使用者或服務 ID 建立 IAM 存取記號](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i)。

不確定 API 金鑰與記號是否符合？請在 [IAM 文件](/docs/iam?topic=iam-iamapikeysforservices)中進一步瞭解。

