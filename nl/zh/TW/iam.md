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


# 管理存取權
{: #iam}

{{site.data.keyword.openwhisk}} 支援 Identity and Access Management (IAM)。您現在可以定義資源的 IAM 原則（例如名稱空間）。
{: shortdesc}

只有東京地區的 {{site.data.keyword.openwhisk_short}} 才能使用 IAM 原則。如果您在東京地區操作，必須使用 IAM 原則來控制存取。
{: tip}

</br>

## 將 IAM 角色對映至 {{site.data.keyword.openwhisk_short}}
{: #user-roles}

在 {{site.data.keyword.openwhisk_short}} 中，會將名稱空間視為 IBM Cloud 資源，容許您使用 IAM 角色及原則進行存取管理。您為名稱空間設定的所有原則，也會套用至名稱空間所包含的 {{site.data.keyword.openwhisk_short}} 實體（例如動作或觸發程式）。
{: shortdesc}

{{site.data.keyword.openwhisk_short}} 同時使用「平台」及「服務」管理角色。您可以設定有關誰可以在平台層次建立名稱空間的原則，同時使用服務角色來管理與名稱空間本身的互動。

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


## 設定 IAM 存取原則
{: #set-iam}

服務呼叫動作時，動作會有回應。因為回應是從名稱空間或動作傳送至服務，所以會被視為出埠資訊。如果要限制名稱空間對其他服務的影響程度，建議您建立存取原則。
{: shortdesc}

如需如何指派、編輯、檢閱或刪除資源存取原則的相關資訊，請參閱[管理 IAM 存取](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)。
{: tip}




</br>
</br>

## 存取名稱空間中的其他資源
{: #namespace-access}

您可以使用 IAM 記號，存取 IAM 受管理名稱空間中的其他資源。記號代表鑑別，並驗證資源的身分。存取 IAM 受管理服務或資源時，需要有 IAM 記號來進行鑑別。
{: shortdesc}

與使用者 ID 識別使用者的方式類似，服務 ID 代表特定資源。這表示 IAM 原則可以套用至管理存取權的資源。就像使用者一樣，資源必須鑑別才能驗證其身分。在 Functions 內，存取其他服務或資源時，動作的實作可以充分運用此功能。

當您建立新的 IAM 受管理名稱空間時，Functions 會自動建立對應的服務 ID，以識別名稱空間及 API 金鑰。在運行環境，Cloud Functions 會將 API 金鑰傳遞至動作碼，作為環境變數 `__OW_IAM_NAMESPACE_API_KEY` 的值。動作碼可以使用此 API 金鑰來產生 IAM 記號。大部分受支援的 SDK（例如 Cloudant、Watson）及 COS 都會使用 IAM 金鑰本身進行鑑別。使用 REST API 的其他 IAM 受管理服務或資源，會使用從 IAM 金鑰衍生的記號進行鑑別。

不確定 API 金鑰與記號是否符合？請在 [IAM 文件](/docs/iam?topic=iam-iamapikeysforservices)中進一步瞭解。
