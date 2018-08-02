---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 建立無伺服器 REST API
{: #openwhisk_apigateway}

藉由引進「API 閘道」，直接透過 API 進行管理，有利於 {{site.data.keyword.openwhisk}} 動作，而「API 閘道」作為 [Web 動作](./openwhisk_webactions.html)的 Proxy，並將其他特性提供給它們。額外的特性包括：HTTP 方法遞送、用戶端 ID/密碼、比率限制、CORS、檢視 API 使用情形、檢視回應日誌，以及 API 共用原則。
如需 API 管理的相關資訊，您可以閱讀 [API Management 文件](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis)。
{: shortdesc}

## 使用瀏覽器從 OpenWhisk Web 動作建立 API
{: #create_api_browser}

您可以使用 [{{site.data.keyword.openwhisk_short}} 儀表板](https://console.bluemix.net/openwhisk/)中的 [**API 標籤**](https://console.bluemix.net/openwhisk/apimanagement)來執行下列作業：

* [建立 Cloud Functions API](https://console.bluemix.net/openwhisk/apimanagement) - 建立包裝一組 OpenWhisk 動作的 API。
* [保護 API](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - 套用 API 安全及速率限制原則以保護 API。
* [管理資料流量](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - 檢視 API 用量統計資料，並移出回應日誌。
* [社交化及共用](https://console.bluemix.net/docs/apis/management/manage_apis.html#share_api) - 與 {{site.data.keyword.Bluemix_notm}} 內外部的開發人員共用 API。

## 使用 CLI 外掛程式從 OpenWhisk Web 動作建立 API
{: #create_api_cli}

下節逐步引導您使用 {{site.data.keyword.openwhisk_short}} CLI 外掛程式來進行 API 管理作業。若要透過 CLI 建立及管理 API，您必須先安裝適用於 {{site.data.keyword.Bluemix_notm}} 的 [{{site.data.keyword.openwhisk_short}} CLI 外掛程式](https://console.bluemix.net/docs/openwhisk/bluemix_cli.html)。

為了方便起見，步驟分成幾個較小的子主題，而您可以使用下列 API 作業清單快速跳至這些子主題：

* [建立第一個 API](openwhisk_apigateway.html#create_cli_api)
* [HTTP 回應的完整控制](openwhisk_apigateway.html#full_control)
* [公開多個 Web 動作](openwhisk_apigateway.html#multiple_web_actions)
* [匯出配置](openwhisk_apigateway.html#export_config)
* [匯入配置](openwhisk_apigateway.html#import_config)
* [修改配置](openwhisk_apigateway.html#modify_config)

### 使用 CLI 建立第一個 API
{: #create_cli_api}

1. 建立名為 **hello.js** 的 JavaScript 檔案，其中包含下列內容：
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. 使用步驟 1 中建立的 `hello.js` 檔案，建立名為 **hello** 的 Web 動作。**附註：**請務必新增 `--web true` 旗標。
  ```
  ibmcloud fn action create hello hello.js --web true
  ```
  {: pre}

  輸出範例：
  ```
  ok: created action hello
  ```
  {: screen}

3. 使用基本路徑 `/hello`、路徑 `/world` 和方法 `get` 來建立 API，回應類型 `json`：
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

  使用 __GET__ HTTP 方法，產生新的 URL 來公開 `hello` 動作。

4. 最後，使用 **curl** 指令，將 HTTP 要求傳送至 URL：
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  輸出範例：
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

會呼叫 Web 動作 **hello**，它會傳回 JSON 物件，其中包括透過查詢參數所傳送的 **name** 參數。您可以使用簡單查詢參數，或使用要求內文，將參數傳遞至動作。如果未使用 OpenWhisk 授權 API 金鑰，則 Web 動作可以透過公開的方式呼叫動作。

### HTTP 回應的完整控制
{: #full_control}

`--response-type` 旗標控制要透過「API 閘道」進行 Proxy 處理之 Web 動作的目標 URL。使用 `--response-type json` 會以 JSON 格式傳回動作的完整結果，並自動將 Content-Type 標頭設為 `application/json`。

您要具有 HTTP 回應內容的完整控制（例如 `statusCode` 及 `headers`），因此可以在 `body` 中傳回不同的內容類型。`--response-type http` 旗標可讓您使用 `http` 副檔名來配置 Web 動作的目標 URL。

您可以選擇使用 `http` 副檔名來變更動作的程式碼以符合 Web 動作的傳回值，或包括一連串的動作以將其結果傳遞給新動作。然後，新的動作可以將結果轉換為 HTTP 回應的正確格式。您可以在 [Web 動作](./openwhisk_webactions.html)文件中深入閱讀回應類型及 Web 動作副檔名。

1. 變更傳回 JSON 內容 `body`、`statusCode` 及 `headers` 之 `hello.js` 的程式碼：
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

2. 使用修改過的結果來更新動作：
  ```
  ibmcloud fn action update hello hello.js --web true
  ```
  {: pre}

3. 使用 `--response-type http` 旗標來更新 API 回應類型：
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

4. 使用下列 **curl** 指令來呼叫更新過的 API：
  ```bash
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

現在，您已完全控制您的 API，而且可以控制內容。例如，傳回 HTML，或設定事項的狀態碼，例如「找不到 (404)」、「未獲授權 (401)」、甚至「內部錯誤 (500)」。


### 公開多個 Web 動作
{: #multiple_web_actions}

例如，如果您要公開讀書會的一組動作，則可以使用一系列的動作來實作該讀書會後端：

|動作|HTTP 方法 (HTTP method)|說明
|
| ----------- | ----------- | ------------ |
|getBooks|GET|取得書籍詳細資料|
|postBooks|POST|新增書籍|
|putBooks|PUT|更新書籍詳細資料|
|deleteBooks|DELETE|刪除書籍|

在此範例中，會使用**路徑參數**來定義 API。使用路徑參數時，必須使用 `http` 回應類型來定義 API。動作 JSON 參數的 `__ow_path` 欄位中提供路徑值，其開頭為基本路徑，並包括實際路徑參數值。如需詳細資料，請參閱 [Web 動作 HTTP 環境定義](./openwhisk_webactions.html#http-context)文件，包括其他 HTTP 環境定義欄位的相關資訊，而這些欄位可用於使用 `http` 回應類型所呼叫的 Web 動作。

1. 使用 `/club` 作為其 HTTP URL 基本路徑、使用 `books` 作為其資源，並使用 `{isbn}` 作為用來利用「國際標準書碼 (ISBN)」識別特定書籍的路徑參數，以建立名為 **Book Club** 之讀書會的 API。
  ```bash
  ibmcloud fn api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud fn api create /club /books get getBooks                       --response-type http
  ibmcloud fn api create /club /books post postBooks                     --response-type http
  ibmcloud fn api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud fn api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: codeblock}

  請注意，以基本路徑 `/club` 公開的第一個動作會取得名為 **Book Club** 的 API 標籤。在 `/club` 下公開的任何其他動作現在都會與 **Book Club** 相關聯。

2. 使用下列指令來列出所有公開的 **Book Club** 動作：
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

3. 為了好玩，您可以使用 HTTP __POST__ 來新增標題為 **JavaScript: The Good Parts** 的書籍：
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

4. 利用 HTTP __GET__，以使用動作 **getBooks** 來取得書籍清單：
  ```bash
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

5. 您可以使用動作 **deleteBooks** 與 HTTP __DELETE__ 搭配，以刪除特定書籍。在此範例中，**deleteBooks** 動作的 `__ow_path` 欄位值是 `/club/books/978-0596517748`，其中 `978-0596517748` 是路徑的 `{isbn}` 實際值。
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

### 匯出配置
{: #export_config}

1. 將名為 **Book Club** 的 API 匯出至檔案，該檔案可作為基本，使用檔案作為輸入來重建 API。
  ```
  ibmcloud fn api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. 使用下列指令，先刪除一般基本路徑下的所有已公開 URL，來測試 Swagger 檔案：
  ```
  ibmcloud fn api delete /club
  ```
  {: pre}

  輸出範例：
  ```
ok: deleted API /club
```
  {: screen}

  您可以使用基本路徑 `/club` 或 API 名稱標籤 **"Book Club"** 來刪除所有已公開 URL：
  {: tip}

### 匯入配置
{: #import_config}

1. 現在，使用檔名 `club-swagger.json` 來還原名為 **Book Club** 的 API：
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

2. 驗證 **Book Club** API 已重建：
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

### 使用使用者介面來修改配置
{: #modify_config}

您可以在「{{site.data.keyword.openwhisk_short}} 儀表板」中編輯配置，按一下 [API 標籤](https://console.ng.bluemix.net/openwhisk/apimanagement)來設定安全、速率限制及其他特性。在完成配置的更新之後，您可以下載採用 JSON 格式的定義檔，然後使用 CLI 重新匯入該檔案。例如，在持續整合及部署 (CICD) 管線中進行自動式部署時，這可能十分有用。您也可以選擇使用使用者介面來上傳及重新匯入 API 定義檔。
