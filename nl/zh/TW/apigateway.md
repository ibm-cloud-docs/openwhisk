---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: serverless, rest api, gateway, web actions, functions

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


# 建立無伺服器 REST API
{: #apigateway}

您可以使用 API 來直接管理 {{site.data.keyword.openwhisk}} [Web 動作](/docs/openwhisk?topic=cloud-functions-actions_web)。
{: shortdesc}

以 IAM 為基礎的名稱空間不支援使用 API 閘道建立 API。請改為使用以 Cloud Foundry 為基礎的名稱空間。
{: important}

## 為什麼要將 REST API 用於 {{site.data.keyword.openwhisk_short}}？

您可以將 API 閘道用作 Web 動作的 Proxy。API 閘道提供 HTTP 方法遞送、用戶端 ID 和密碼、速率限制和 CORS，並可檢視 API 使用情況以及檢視回應日誌和 API 共用原則。

如需 API 管理的相關資訊，您可以閱讀 [API Management 文件](/docs/api-management?topic=api-management-manage_openwhisk_apis)。


## 建立第一個 API
{: #api_create}

您必須在 Cloud Foundry 空間中具有 `SpaceDeveloper` 許可權才能建立 REST API。可以透過執行 `ibmcloud account space-roles <org>` 來查看空間許可權。
{: note}

開始之前，請安裝 [{{site.data.keyword.openwhisk_short}} CLI 外掛程式](/docs/openwhisk?topic=cloud-functions-cli_install)。

1. 將下列程式碼儲存至名稱為 `hello.js` 的 JavaScript 檔案中。
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello, ${name}!`};
  }
  ```
  {: codeblock}

2. 透過使用已建立的檔案來建立名稱為 `hello` 的 Web 動作。確保新增 `--web true` 旗標。將 `<filepath>` 取代為您 `hello.js` 檔案的檔案路徑。

  ```
  ibmcloud fn action create hello <filepath>/hello.js --web true
  ```
  {: pre}

  **輸出範例**
  ```
  ok: created action hello
  ```
  {: screen}

3. 使用基本路徑 `/hello`、路徑 `/world`、方法 `get` 和回應類型 `json` 來建立 API。
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  **範例輸出**
  這將產生新 URL，用於使用 `GET` HTTP 方法公開 `hello` 動作。

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  
4. 使用下列 cURL 指令向 URL 傳送測試 HTTP 要求。
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=Jane
  ```
  {: pre}

  **範例輸出**
  這將呼叫 Web 動作 `hello`，它傳回的 JSON 物件包含查詢參數中的 `name` 參數。您可以使用簡單查詢參數，或使用要求內文，將參數傳遞至動作。Web 動作可以公開呼叫動作，而不使用鑑別。

  ```
  {
  "payload": "Hello, Jane!"
  }
  ```
  {: screen}



## 使用 HTTP 回應的完整控制
{: #api_control}

`--response-type` 旗標控制要透過「API 閘道」進行 Proxy 處理之 Web 動作的目標 URL。例如，當您使用 `--response-type json` 旗標，動作的完整結果會以 JSON 格式傳回，且 `Content-Type` 標頭會自動設為 `application/json`。

若要在內文傳回不同的內容類型，請使用 HTTP 回應的完整控制內容，例如 `statusCode` 和 `headers`。您可以使用 `--response-type http` 旗標，以使用 `http` 副檔名來配置 Web 動作的目標 URL。您可以使用 `http` 副檔名來變更動作的程式碼以符合 Web 動作的傳回值，或包括一連串的動作以將其結果傳遞給新動作。然後，新的動作可以將結果轉換為 HTTP 回應的正確格式。您可以在 [Web 動作](/docs/openwhisk?topic=cloud-functions-actions_web)文件中深入閱讀回應類型及 Web 動作副檔名。

1. 將下列程式碼儲存為 `hello.js`。
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello, ${name}!`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. 以新版的 `hello.js` 程式碼更新您的 `hello` Web 動作。
  ```
  ibmcloud fn action update hello <filepath>/hello.js --web true
  ```
  {: pre}

  **輸出**
  ```
  ok: updated action hello
  ```
  {: screen}

3. 使用 `--response-type http` 旗標來更新 API 回應類型。
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

  **輸出**
  ```
  ok: created API /hello/world GET for action /_/hello https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

4. 使用下列 cURL 指令來呼叫更新過的 API。
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  **輸出範例**
  ```
  {
  "payload": "Hello, Serverless API!"
  }
  ```
  {: screen}

## 修改配置
{: #api_modify_config}

建立配置後，可以使用 {{site.data.keyword.openwhisk_short}} 儀表板中的 [API 標籤](https://cloud.ibm.com/openwhisk/apimanagement){: external}，透過下列方式來修改配置。

* [建立 {{site.data.keyword.openwhisk_short}} API](/docs/services/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis)，包裹一組 {{site.data.keyword.openwhisk_short}} 動作。
* [保護 API](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis)，透過套用 API 安全和速率限制原則來實現。
* 藉由檢視 API 用量統計資料，及檢查回應日誌，以[管理資料流量](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis)。
* 與 {{site.data.keyword.cloud_notm}} 內外的開發人員[進行社交並共用](/docs/services/api-management?topic=api-management-manage_apis#share_api_manage_apis) API。

</br>
更新配置完成後，可以下載 JSON 格式的定義檔，然後使用 CLI 將其重新導入。例如，在持續整合及部署 (CICD) 管線中進行自動式部署時，下載及匯入配置可能十分有用。您還可以使用使用者介面來上傳和重新導入 API 定義檔。



