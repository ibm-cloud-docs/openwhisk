---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 建立無伺服器 REST API
{: #openwhisk_apigateway}

使用 API 可直接管理 {{site.data.keyword.openwhisk}} 動作。「API 閘道」擔任 [Web 動作](./openwhisk_webactions.html)的 Proxy，並且提供 HTTP 方法遞送、用戶端 ID 與密碼、比率限制、CORS、檢視 API 用量、檢視回應日誌，以及 API 共用原則。
{: shortdesc}

如需 API 管理的相關資訊，您可以閱讀 [API Management 文件](/docs/api-management/manage_openwhisk_apis.html#manage_openwhisk_apis)。


## 建立第一個 API
{: #create_cli_api}

開始之前，請安裝 [{{site.data.keyword.openwhisk_short}} CLI 外掛程式](bluemix_cli.html)。

1. 將下列程式碼儲存至名為 `hello.js` 的 JavaScript 檔案中。
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. 使用您建立的檔案，建立名為 `hello` 的 Web 動作。**附註：**請務必新增 `--web true` 旗標。
  ```
  ibmcloud fn action create hello hello.js --web true
  ```
  {: pre}

  輸出範例：
  ```
  ok: created action hello
  ```
  {: screen}

3. 使用基本路徑 `/hello`、路徑 `/world`、方法 `get` 和回應類型 `json` 來建立 API。
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  輸出範例：
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  使用 HTTP GET 方法，產生新的 URL 來公開 `hello` 動作。

4. 使用 cURL 指令，將測試 HTTP 要求傳送至 URL。
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  輸出範例：
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

會呼叫 Web 動作 `hello`，它會傳回 JSON 物件，其中包括查詢參數中的 **name** 參數。您可以使用簡單查詢參數，或使用要求內文，將參數傳遞至動作。Web 動作可以公開呼叫動作，而不使用 {{site.data.keyword.openwhisk_short}} 授權 API 金鑰。

## 使用 HTTP 回應的完整控制
{: #full_control}

`--response-type` 旗標控制要透過「API 閘道」進行 Proxy 處理之 Web 動作的目標 URL。例如，當您使用 `--response-type json` 旗標，動作的完整結果會以 JSON 格式傳回，且 **Content-Type** 標頭會自動設為 `application/json`。

若要在內文傳回不同的內容類型，請使用 HTTP 回應的完整控制內容，例如 **statusCode** 和 **headers**。您可以使用 `--response-type http` 旗標，以使用 `http` 副檔名來配置 Web 動作的目標 URL。您可以使用 `http` 副檔名來變更動作的程式碼以符合 Web 動作的傳回值，或包括一連串的動作以將其結果傳遞給新動作。然後，新的動作可以將結果轉換為 HTTP 回應的正確格式。您可以在 [Web 動作](./openwhisk_webactions.html)文件中深入閱讀回應類型及 Web 動作副檔名。

1. 變更傳回 JSON 內容 `body`、`statusCode` 及 `headers` 之 `hello.js` 動作的程式碼。
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello world ${name}`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. 使用修改過的結果來更新動作。
  ```
  ibmcloud fn action update hello hello.js --web true
  ```
  {: pre}

3. 使用 `--response-type http` 旗標來更新 API 回應類型。
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

4. 使用下列 cURL 指令來呼叫更新過的 API。
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  輸出範例：
  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  {: screen}

## 公開多個 Web 動作
{: #multiple_web_actions}

您可以公開多個 Web 動作，以實作應用程式後端。例如，若要公開讀書會的一組動作，則可以使用一系列的動作來實作該讀書會後端：

|動作|HTTP 方法|說明|
| ----------- | ----------- | ------------ |
|getBooks|GET|取得書籍詳細資料|
|postBooks|POST|新增書籍|
|putBooks|PUT|更新書籍詳細資料|
|deleteBooks|DELETE|刪除書籍|

在此範例中，會使用路徑參數來定義 API。使用路徑參數時，必須使用 `http` 回應類型來定義 API。動作 JSON 參數的 `__ow_path` 欄位中提供路徑值，其開頭為基本路徑，並包括實際路徑參數值。如需 HTTP 環境定義欄位的詳細資料，請參閱 [Web 動作 HTTP 環境定義](./openwhisk_webactions.html#http-context)文件。

若要嘗試這個讀書會 Web 動作範例，請執行下列動作：

1. 使用 `/club` 作為其 HTTP URL 基本路徑、使用 `books` 作為其資源，並使用 `{isbn}` 作為用來利用「國際標準書碼 (ISBN)」識別特定書籍的路徑參數，以建立名為 `Book Club` 之讀書會的 API。
  ```
  ibmcloud fn api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud fn api create /club /books get getBooks                       --response-type http
  ibmcloud fn api create /club /books post postBooks                     --response-type http
  ibmcloud fn api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud fn api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: pre}

  以基本路徑 `/club` 公開的第一個動作，會以名稱 `Book Club` 標示。在 `/club` 下公開的任何其他動作現在都會與 `Book Club` 相關聯。

2. 列出所有公開的 `Book Club` 動作。
  ```
  ibmcloud fn api list /club -f
  ```
  {: pre}

  輸出範例：
  ```
  ok: APIs
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: postBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: post
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: putBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: put
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: deleteBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: delete
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

3. 使用 HTTP POST 新增標題為 `JavaScript: The Good Parts` 的書籍。
  ```
  curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' -H "Content-Type: application/json" https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  輸出範例：
  ```
  {
    "result": "success"
  }
  ```
  {: screen}

4. 使用 HTTP GET 呼叫 `getBooks` 動作，取得書籍清單。
  ```
  curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  輸出範例：
  ```
  {
    "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
  }
  ```
  {: screen}

5. 使用 HTTP DELETE 呼叫 `deleteBooks` 動作，刪除特定的書籍。在此範例中，`deleteBooks` 動作的 `__ow_path` 欄位值是 `/club/books/978-0596517748`，其中 `978-0596517748` 是路徑的 `{isbn}` 實際值。
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

## 匯出及匯入配置
{: #export_import_config}

若要匯出或匯入配置，您可以繼續使用讀書會範例。

1. 將 `Book Club` API 匯出至名為 `club-swagger.json` 的檔案。這個檔案可以用來作為基礎，使用檔案作為輸入來重建 API。
  ```
  ibmcloud fn api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. 先刪除一般基本路徑下的所有已公開 URL，來測試 Swagger 檔案。

  ```
  ibmcloud fn api delete /club
  ```
  {: pre}

  輸出範例：
  ```
  ok: deleted API /club
  ```
  {: screen}

  您可以使用基本路徑 `/club` 或 API 名稱標籤 `"Book Club"` 來刪除所有已公開 URL。
  {: tip}

3. 使用 `club-swagger.json` 檔案還原 `Book Club` API。
  ```
  ibmcloud fn api create --config-file club-swagger.json
  ```
  {: pre}

  輸出範例：
  ```
  ok: created api /club/books/{isbn} get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} put for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} delete for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books post for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: screen}

4. 驗證 `Book Club` API 已重建。
  ```
  ibmcloud fn api list /club
  ```
  {: pre}

  輸出範例：
  ```
  ok: apis
  Action                    Verb         API Name        URL
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

## 修改配置
{: #modify_config}

建立配置之後，您可以使用 {{site.data.keyword.openwhisk_short}} 儀表板中的 [**API 標籤**](https://console.bluemix.net/openwhisk/apimanagement)，以下列方式修改配置。

* [建立 {{site.data.keyword.openwhisk_short}} API](https://console.bluemix.net/openwhisk/apimanagement)，包裹一組 {{site.data.keyword.openwhisk_short}} 動作。
* 藉由套用 API 安全和限制原則的比率，以[保護 API](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api)。
* 藉由檢視 API 用量統計資料，及檢查回應日誌，以[管理資料流量](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api)。
* 與 {{site.data.keyword.Bluemix_notm}} 內外的開發人員[進行社交並共用](https://console.bluemix.net/docs/apis/management/manage_apis.html#share_api) API。

在完成配置的更新之後，您可以下載採用 JSON 格式的定義檔，然後使用 CLI 重新匯入該檔案。例如，在持續整合及部署 (CICD) 管線中進行自動式部署時，下載及匯入配置可能十分有用。您也可以選擇使用使用者介面來上傳及重新匯入 API 定義檔。
