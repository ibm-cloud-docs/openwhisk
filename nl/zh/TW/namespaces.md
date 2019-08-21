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


# 管理名稱空間
{: #namespaces}

透過 {{site.data.keyword.openwhisk}}，可以建立 Identity and Access (IAM) 管理的名稱空間，以將實體（例如，動作或觸發程式）分組在一起。然後，可以為該名稱空間建立 IAM 存取原則。如需 IAM 的概觀，請參閱 [{{site.data.keyword.openwhisk_short}} 啟用 IAM 的公告部落格](https://www.ibm.com/cloud/blog/ibm-cloud-functions-is-now-identity-and-access-management-enabled){: external}。
{: shortdesc}

## 什麼是名稱空間？

名稱空間包含 {{site.data.keyword.openwhisk_short}} 實體（例如，動作和觸發程式），並屬於資源群組。透過授與使用者對名稱空間的存取權，可以允許使用者存取實體。

實體的完整名稱為 `/namespaceName/packageName/entityName`。

### 建立名稱空間時會發生什麼情況？

在 {{site.data.keyword.openwhisk_short}} 中建立的名稱空間時會識別為 IAM 服務實例。在建立名稱空間期間，可以指定要在其中新增服務實例的[資源群組](/docs/resources?topic=resources-rgs)。

建立名稱空間時，將建立下列元件：

|元件|說明|
| --- | --- | 
|服務 ID|在發出出埠呼叫時，可將服務 ID 用作功能 ID。在此名稱空間中建立的所有動作都可以使用此服務 ID 來存取其他資源。依預設，功能使用者將取得「讀者」角色。「讀者」存取權表示它可以讀取名稱空間實體並呼叫動作。「讀者」角色由觸發程式用於呼叫動作。為了控制入埠資料流量，您可能希望向其他使用者授與存取權，例如指派「讀者」角色以呼叫動作。
|
|API 金鑰|可用於產生 IAM 記號的服務 ID 的 API 金鑰。可以使用這些記號向其他 {{site.data.keyword.cloud_notm}} 服務鑑別名稱空間。API 金鑰作為環境變數 `__OW_IAM_NAMESPACE_API_KEY` 提供給動作。|

檢視所有服務 ID。
```
ibmcloud iam service-ids
```
{: pre}

檢視與服務 ID 關聯的 API 金鑰。 
```
ibmcloud iam service-api-keys <ServiceID-12345678-1234-abcd-1234-123456789abc>
```
{: pre}

</br>

不要刪除 API 金鑰。
{: tip}

### 名稱空間有任何限制嗎？

IAM 管理的名稱空間不支援[使用 API 閘道建立 API](/docs/openwhisk?topic=cloud-functions-apigateway)，也不支援[行動 SDK](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk)。 

所有實體（包括動作、觸發程式、規則、套件及名稱空間）的名稱都是遵循下列格式的一串字元：
* 第一個字元必須是英數字元或底線。
* 後續字元可以是英數字元、空格或下列任何一值：`_`、`@`、`.`、`-`。
* 最後一個字元不能是空格。

### 如果我有以 Cloud Foundry 為基礎的名稱空間，該怎麼辦？

以 Cloud Foundry 為基礎的名稱空間仍可正常運作。但是，為了利用新特性，您必須建立啟用 IAM 功能的名稱空間。


## 在使用者介面中建立以 IAM 為基礎的名稱空間
{: #create_iam_ui}

1. 在 [{{site.data.keyword.openwhisk_short}} 主控台](https://cloud.ibm.com/openwhisk){: external}中，按一下「名稱空間」下拉功能表。

2. 按一下**建立名稱空間**。

3. 輸入名稱空間的顯示名稱和簡要說明，例如計劃在此名稱空間中建立的動作或套件的類型。

4. 選擇要在其中建立名稱空間的資源群組和要在其中部署名稱空間資源的位置。

5. 按一下**建立**。

6. 若要檢視名稱空間資源的服務實例，請移至 [{{site.data.keyword.cloud_notm}} 儀表板](https://cloud.ibm.com/resources){: external}，然後在 **Functions 名稱空間**窗格中尋找名稱空間名稱。

如果需要，可以在 {{site.data.keyword.openwhisk_short}} 主控台中的**名稱空間設定**頁面上更新名稱空間的名稱或說明。

## 使用 CLI 建立以 IAM 為基礎的名稱空間
{: #namespaces_create}

您可以建立 IAM 管理的名稱空間作為資源群組的一部分，並在建立名稱空間時將目標設為資源群組，以管理資源的存取原則。如果您有其他使用者需要存取您的名稱空間，或您要透過名稱空間的動作來存取其他資源，請務必在建立名稱空間之後設定 IAM 原則。
{: shortdesc}

1. 將目標設為您要在其中建立名稱空間的資源群組。如果您尚未建立[資源群組](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create)，則可以將目標設為 `default` 群組。

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. 建立已啟用 IAM 功能的名稱空間。選用：使用 `-n` 或 `--description` 旗標來包含名稱空間的說明。如果您的說明比一個字還長，則必須加上引號。

  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description of your namespace">]
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="「構想」圖示"/> 瞭解此指令的元件</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;namespace_name&gt;</code></td>
        <td>以 IAM 為基礎之名稱空間的顯示名稱。</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>選用項目：將說明新增至名稱空間，例如您計劃建立的動作或套件類型。如果您的說明比一個字還長，則必須加上引號。</td>
      </tr>
      <tr>
        <td><code>--description &lt;description&gt;</code></td>
        <td>選用項目：將說明新增至名稱空間，例如您計劃建立的動作或套件類型。如果您的說明比一個字還長，則必須加上引號。</td>
      </tr>
    </tbody>
  </table>

  輸出範例：

  ```
ok: created namespace myNamespace
    ```
  {: screen}

3. 驗證新的名稱空間已建立。

  ```
  ibmcloud fn namespace get <namespace_name_or_id> --properties
  ```
  {: pre}

  輸出範例：

  ```
  Details of namespace: myNamespace
  Description: short description
  Resource Plan Id: functions-base-plan
  Location: jp-tok
  ID: 05bae599-ead6-4ccb-9ca3-94ce8c8b3e43
  ```
  {: screen}

  您也可以列出所有名稱空間，包括以 IAM 及 Cloud Foundry 為基礎的名稱空間：
    

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. 在名稱空間中建立實體之前，必須透過將該名稱空間設定為目標，將 CLI 環境定義設定為該名稱空間。

  ```
  ibmcloud fn property set --namespace <namespace_name_or_id>
  ```
  {: pre}

設定內容（如 `--namespace` 內容）之後，將保留該內容，直到您手動將其取消設定為止。如果要在 IAM 名稱空間之間或在 Cloud Foundry 與 IAM 之間進行切換，則必須取消設置 namespace 內容並將其重設。如需相關資訊，請參閱 [`ibmcloud fn property set`]
{: note}

## 使用 API 建立名稱空間
{: #namespaces_create_api}

您可以建立 IAM 管理的名稱空間作為資源群組的一部分，並在建立名稱空間時將目標設為資源群組，以管理資源的存取原則。如果您有其他使用者需要存取您的名稱空間，或您要透過名稱空間的動作來存取其他資源，請務必在建立名稱空間之後設定 IAM 原則。
{: shortdesc}

1. 將目標設為您要在其中建立名稱空間的資源群組。如果您尚未建立[資源群組](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create)，則可以將目標設為 `default` 群組。

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. 建立已啟用 IAM 功能的名稱空間。

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
        <th colspan=2><img src="images/idea.png" alt="「構想」圖示"/> 瞭解此指令的元件</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;IAM_token&gt;</code></td>
        <td>{{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) 記號。若要擷取您的 IAM 記號，請執行 <code>ibmcloud iam oauth-tokens</code>。</td>
      </tr>
      <tr>
        <td><code>-n &lt;name&gt;</code></td>
        <td>名稱空間的名稱。</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_group_id&gt;</code></td>
        <td>您要在其中建立名稱空間的資源群組 ID。若要查看資源群組 ID，請執行 <code>ibmcloud resource groups</code>。</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_plan_id&gt;</code></td>
        <td>資源方案的 ID，例如 functions-base-plan</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>選用項目：將說明新增至名稱空間，例如，它將包含的動作或套件類型。</td>
      </tr>
    </tbody>
  </table>

  **輸出範例**

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

3. 驗證新的名稱空間已建立。

  ```
    curl --request GET \
      --url 'https://us-south.functions.cloud.ibm.com/api/servicebroker/api/v1/namespaces/{id} \
      --header 'accept: application/json' \
      --header 'authorization: <IAM_token>'
    ```
  {: pre}

  您也可以列出所有名稱空間，包括以 IAM 及 Cloud Foundry 為基礎的名稱空間：
    
  ```
  curl --request GET \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces?limit=0&offset=0' \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>'
  ```
  {: pre}

  **輸出範例**

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

如需使用 HTTP REST 的相關資訊，請參閱 [{{site.data.keyword.openwhisk_short}} API 文件](/apidocs/functions)。
{: tip}

### 存取名稱空間中的其他資源
{: #namespace-access}

動作通常會呼叫需要適當鑑別的其他 {{site.data.keyword.cloud_notm}} 資源和服務。如果這些服務已啟用 IAM 並接受 IAM 記號，則可以利用名稱空間的功能 ID 進行出埠通訊。
{: shortdesc}

如[管理 IAM 存取權](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)中所述，對於每個名稱空間，都會建立一個服務 ID 來代表該名稱空間。透過使用 IAM 原則管理來指派適當的角色，可以向此服務 ID 授權對其他服務和資源的存取權。如需建立服務 ID 以存取其他啟用 IAM 的服務的相關資訊，請參閱[建立和使用服務 ID](/docs/iam?topic=iam-serviceids#serviceids)。

在運行環境中，{{site.data.keyword.openwhisk_short}} 會將名稱空間服務 ID 的 API 金鑰作為環境變數 `__OW_IAM_NAMESPACE_API_KEY` 傳遞到動作碼。動作碼可以使用此 API 金鑰來產生 IAM 記號。大多數支援的 {{site.data.keyword.openwhisk_short}} SDK（例如，Cloudant、{{site.data.keyword.watson}} 和 {{site.data.keyword.cos_full_notm}}）都使用 IAM API 金鑰本身進行鑑別。對於其他使用 REST API 的 IAM 受管理服務或資源，可以使用從 IAM API 金鑰衍生的記號進行鑑別。如需相關資訊，請參閱[為使用者或服務 ID 建立 IAM 存取記號](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i)。

不確定 API 金鑰和記號是否符合？請在 [IAM 文件](/docs/iam?topic=iam-iamapikeysforservices)中進一步瞭解。

## 後續步驟
{: #namespaces_next}

既然您已建立了名稱空間，現在可以建立 IAM 存取原則來協助保護該名稱空間。若要開始，請參閱[管理存取](/docs/openwhisk?topic=cloud-functions-iam)。 

如需如何管理以 IAM 為基礎的名稱空間的相關資訊，請參閱 [{{site.data.keyword.openwhisk_short}} REST API 參考資料](/apidocs/functions)。








