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



# 管理名稱空間
{: #namespaces}

透過 {{site.data.keyword.openwhisk}}，可以建立 Identity and Access (IAM) 管理的名稱空間，以將實體（例如，動作或觸發程式）分組在一起。然後，可以為該名稱空間建立 IAM 存取原則。
{: shortdesc}


**什麼是名稱空間？**

名稱空間包含 {{site.data.keyword.openwhisk_short}} 實體（例如，動作和觸發程式），並屬於資源群組。透過授與使用者對名稱空間的存取權，可以允許使用者存取實體。

實體的完整名稱是 `/namespaceName/[packageName]/entityName`。


**建立名稱空間時會發生什麼情況？**

在 {{site.data.keyword.openwhisk_short}} 中建立的名稱空間時會識別為 IAM 服務實例。在建立名稱空間期間，可以指定要在其中新增服務實例的[資源群組](/docs/resources?topic=resources-rgs)。

建立名稱空間時，將同時建立下列構件：

* 在發出出埠呼叫時可用作功能 ID 的服務 ID。在此名稱空間中建立的所有動作都可以使用此服務 ID 來存取其他資源。若要查看所有服務 ID，請執行 `ibmcloud iam service-ids`。

* 用於服務 ID 的 API 金鑰，此金鑰可用於產生 IAM 記號。然後，可以使用這些記號向其他 {{site.data.keyword.Bluemix_notm}} 服務鑑別名稱空間。API 金鑰會作為環境變數提供給動作。

    不要刪除 API 金鑰。
    {: tip}

**名稱空間有任何限制嗎？**

IAM 管理的名稱空間不支援[使用 API 閘道建立 API](/docs/openwhisk?topic=cloud-functions-apigateway)，也不支援使用[行動 SDK](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk)。

{{site.data.keyword.openwhisk_short}} 對名稱空間的名稱有所限制。如需相關資訊，請參閱[系統詳細資料及限制](/docs/openwhisk?topic=cloud-functions-limits#limits_entities_ov)文件。
{: tip}



**如果我有以 Cloud Foundry 為基礎的名稱空間，該怎麼辦？**

以 Cloud Foundry 為基礎的名稱空間將繼續正常工作。但是，為了利用新特性，您必須[將名稱空間移轉到 IAM](/docs/resources?topic=resources-migrate)。

</br>


## 使用 CLI 建立名稱空間
{: #namespaces_create}

您可以建立 IAM 受管理名稱空間作為資源群組的一部分，並在建立名稱空間時將目標設為資源群組，以管理資源的存取原則。如果您有其他使用者需要存取您的名稱空間，或您要透過名稱空間的動作來存取其他資源，請務必在建立名稱空間之後設定 IAM 原則。
{: shortdesc}

1. 將目標設為您要在其中建立名稱空間的資源群組。如果您尚未建立[資源群組](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create)，則可以將目標設為 `default` 群組。

  ```
    ibmcloud target -g default
    ```
  {: pre}

2. 建立已啟用 IAM 功能的名稱空間。

  ```
    ibmcloud fn namespace create <namespace_name> [-n <description>]
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
        <td>選用項目：將說明新增至名稱空間，例如，它將包含的動作或套件類型。</td>
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
    Details of namespace: 'myNamespace'
    Description: 'short description'
    Resource Plan Id: 'functions-base-plan'
    Location: 'jp-tok'
    ID: '05bae599-ead6-4ccb-9ca3-94ce8c8b3e43'
    ```
  {: screen}

  您也可以列出所有名稱空間，包括 IAM 型及 Cloud Foundry 型名稱空間：
    

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. 在名稱空間中建立實體之前，請透過將該名稱空間設定為目標，將 CLI 環境定義設定為該名稱空間。
    

  ```
    ibmcloud fn property set --namespace <namespace_name_or_id>
    ```
  {: pre}

</br>

## 使用 API 建立名稱空間
{: #namespaces_create_api}

您可以建立 IAM 受管理名稱空間作為資源群組的一部分，並在建立名稱空間時將目標設為資源群組，以管理資源的存取原則。如果您有其他使用者需要存取您的名稱空間，或您要透過名稱空間的動作來存取其他資源，請務必在建立名稱空間之後設定 IAM 原則。
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
        <td>{{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) 記號。若要擷取您的 IAM 記號，請執行 <code>ibmcloud iam oauth-tokens</code>。</td>
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

  輸出範例：

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

  您也可以列出所有名稱空間，包括 IAM 型及 Cloud Foundry 型名稱空間：
    
  ```
  curl --request GET \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces?limit=0&offset=0' \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>'
  ```
  {: pre}

  輸出範例：
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


如需使用 HTTP REST 的相關資訊，請查看 [{{site.data.keyword.openwhisk_short}} API 文件](/apidocs/functions)。
{: tip}



## 後續步驟
{: #namespaces_next}

既然您已建立名稱空間，就可以建立 IAM 存取原則來協助進行保護。若要開始，請參閱[管理存取](/docs/openwhisk?topic=cloud-functions-iam)。如需如何管理 IAM 型名稱空間的相關資訊，請參閱 [{{site.data.keyword.openwhisk_short}} REST API 參考資料](/apidocs/functions)。


