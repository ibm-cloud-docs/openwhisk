---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: namespaces, actions, create

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# 建立名稱空間
{: #openwhisk_namespaces}

在東京地區，{{site.data.keyword.openwhisk_short}} 會使用 Identity and Access (IAM) 受管理名稱空間將動作或觸發程式這類實體群組在一起。然後，您可以建立名稱空間的存取原則。
{: shortdesc}

當您建立 {{site.data.keyword.openwhisk_short}} 名稱空間時，會將它識別為 IAM 服務實例。IAM 受管理服務實例必須在[資源群組](/docs/resources?topic=resources-rgs)中建立。您可以建立自己的資源群組或將目標設為預設值。若要查看您帳戶中具有的 IAM 服務實例，您可以執行 `ibmcloud resource service-instances`。

下列構件會與您的名稱空間一起建立。請不要刪除它們。

* 建立「服務 ID」，以在進行出埠呼叫時用來作為功能 ID。在此名稱空間中建立的所有動作，都可以使用此「服務 ID」來存取其他資源。若要查看您的所有「服務 ID」，請執行 `ibmcloud iam service-ids`。

* 為上述「服務 ID」建立 API 金鑰，以用來產生 IAM 記號。您接著可以使用記號，向其他 IBM Cloud 服務鑑別名稱空間。API 金鑰會作為環境變數提供給動作。


## 限制
{: #limitations}

IAM 受管理名稱空間目前不支援[建立具有 API Gateway 的 API](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway) 以及使用[行動 SDK](/docs/openwhisk?topic=cloud-functions-openwhisk_mobile_sdk)。

</br>

若要將目標設為東京位置的 {{site.data.keyword.openwhisk_short}} 後端服務，您必須將 `apihost` 附加至所有 CLI 呼叫（例如 `ibmcloud fn namespace list --apihost jp-tok.functions.cloud.ibm.com`）。除非透過 `ibmcloud target -r jp-tok` 將目標設為該位置，否則這是暫時的。
{: tip}



</br>
</br>


## 使用 CLI 建立名稱空間
{: #create_iam_cli}

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
          <td>新 IAM 型名稱空間的顯示名稱。</td>
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
    ibmcloud fn namespace get <namespace_name_or_id> --no-entities
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
    ibmcloud fn namespace list --iam
    ```
    {: pre}

4. 在新的名稱空間中建立實體之前，請將 CLI 環境定義設為名稱空間，方法是將它設為目標。
    ```
    ibmcloud fn property set --namespace <namespace_name_or_id>
    ```
    {: pre}

</br>

## 使用 API 建立名稱空間
{: #create_iam_api}

您可以建立 IAM 受管理名稱空間作為資源群組的一部分，並在建立名稱空間時將目標設為資源群組，以管理資源的存取原則。如果您有其他使用者需要存取您的名稱空間，或您要透過名稱空間的動作來存取其他資源，請務必在建立名稱空間之後設定 IAM 原則。
{: shortdesc}



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
          <td>您的 IBM Cloud Identity and Access Management (IAM) 記號。若要擷取您的 IAM 記號，請執行 <code>ibmcloud iam oauth-tokens</code>。</td>
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


如需使用 HTTP REST 的相關資訊，請參閱 [Cloud Functions API 文件](https://cloud.ibm.com/apidocs/functions)。
{: tip}

</br>
</br>


## 後續步驟
{: #next}

既然您已建立名稱空間，就可以建立 IAM 存取原則來協助進行保護。若要開始，請參閱[管理存取](/docs/openwhisk?topic=cloud-functions-iam#iam)。如需如何管理 IAM 型名稱空間的相關資訊，請參閱 [{{site.data.keyword.openwhisk_short}} REST API 參照](https://cloud.ibm.com/apidocs/functions)。
