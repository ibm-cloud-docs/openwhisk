---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# API 閘道
{: #openwhisk_apigateway}

透過 API Management 進行管理，有利於「OpenWhisk 動作」。

「API 閘道」作為 [Web 動作](./openwhisk_webactions.html)的 Proxy ，並提供它們其他特性。額外的特性包括：HTTP 方法遞送、用戶端 ID/密碼、比率限制、CORS、檢視 API 使用情形、檢視回應日誌，以及 API 共用原則。
如需 API 管理的相關資訊，您可以閱讀 [API Management 文件](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis)。
{: shortdesc}

## 使用「瀏覽器」從 OpenWhisk Web 動作建立 API。

使用「API 閘道」，您可以將「OpenWhisk 動作」公開為 API。定義 API 之後，您可以套用安全和比率限制原則、檢視 API 使用情形和回應日誌，以及定義 API 共用原則。
在「OpenWhisk 儀表板」中，按一下 [API](https://console.ng.bluemix.net/openwhisk/apimanagement) 標籤。


## 使用 CLI 從 OpenWhisk Web 動作建立 API

### OpenWhisk CLI 配置

使用 API 主機配置 OpenWhisk CLI。

有兩個可用的 {{site.data.keyword.Bluemix_notm}} 地區，其需要自己的專屬 API 主機和「授權」金鑰。

* 美國南部
  * API 主機：`openwhisk.ng.bluemix.net`

* 英國
  * API 主機：`openwhisk.eu-gb.bluemix.net`

執行下列指令，以設定您想要之 Bluemix 地區的 API 主機：

美國南部：
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

英國：
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

如果您需要切換地區，則必須使用 API 主機及授權金鑰來重新配置 CLI，因為授權金鑰是每個地區專用的。
{: tip}

「動作」、「規則」和套件之類的構件是地區特定的。因此，如果您在多個地區中使用相同的構件，您必須將它部署到每一個想要的地區。

若要可以使用 `wsk api` 指令，CLI 配置檔 `~/.wskprops` 需要包含「Bluemix 存取記號」。

若要取得存取記號，請使用下列 CLI 指令：
```
wsk bluemix login
```
{: pre}

如需這個指令的相關資訊，請執行：
```
wsk bluemix login -h
```
{: pre}

如果 `wsk bluemix login` 指令失敗，且發生錯誤 `BMXLS0202E: You are using a federated user ID, please use one time code to login with option --sso`，請使用 `bluemix login` 來登入 {{site.data.keyword.Bluemix_notm}} CLI，然後發出 `wsk bluemix login --sso`。
{: tip}

### 使用 CLI 建立第一個 API

1. 建立含有下列內容的 JavaScript 檔。在此範例中，檔名是 'hello.js'。
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}
  
2. 從下列 JavaScript 函數建立「Web 動作」。在此範例中，「動作」稱為 'hello'。請務必新增旗標 `--web true`。
  ```
  wsk action create hello hello.js --web true
  ```
  {: pre}

  ```
  ok: created action hello
  ```
  
3. 使用基本路徑 `/hello`、路徑 `/world` 和方法 `get` 來建立 API，回應類型 `json`：
  ```
  wsk api create /hello /world get hello --response-type json
  ```

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
會透過 __GET__ HTTP 方法，產生新的 URL 來公開 `hello` 動作。
  
4. 最後，將 HTTP 要求傳送至 URL。
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world?name=OpenWhisk
  ```

  ```json
  {
  "payload": "Hello world OpenWhisk"
  }
  ```

  會呼叫 Web 動作 `hello`，它會傳回 JSON 物件，包含透過查詢參數所傳送的 `name` 參數。您可以透過簡單查詢參數，或透過要求內文，將參數傳遞至「動作」。如果沒有 OpenWhisk 授權 API 金鑰，則「Web 動作」可以用公開的方式呼叫「動作」。
  
### HTTP 回應的完整控制
  
  `--response-type` 旗標控制要透過「API 閘道」進行 Proxy 處理之「Web 動作」的目標 URL。使用 `--response-type json` 會以 JSON 格式傳回「動作」的完整結果，並自動將 Content-Type 標頭設為 `application/json`。 
  
  開始使用之後，您要具有 HTTP 回應內容的完整控制（例如 `statusCode`、`headers`），並在 `body` 中傳回不同內容類型。旗標 `--response-type http` 可讓您使用 `http` 副檔名來配置「Web 動作」的目標 URL。

  您可以選擇使用 `http` 副檔名來變更動作的程式碼以符合「Web 動作」的傳回值，或包括一連串的「動作」可將其結果傳遞給新「動作」。然後，新的「動作」可以將結果轉換為 HTTP 回應的正確格式。您可以在 [Web 動作](./openwhisk_webactions.html)文件中深入閱讀回應類型及「Web 動作」副檔名。

  變更傳回 JSON 內容 `body`、`statusCode` 及 `headers` 之 `hello.js` 的程式碼
  ```javascript
  function main({name:name='Serverless API'}) {
      return { 
    body: new Buffer(JSON.stringify({payload:`Hello world ${name}`})).toString('base64'), 
        statusCode:200, 
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}
  請注意，需要傳回以 `base64` 編碼的內文，而非字串。

  
  使用修改過的結果來更新「動作」： 
  ```
  wsk action update hello hello.js --web true
  ```
  {: pre}

  使用 `--response-type http` 來更新 API： 
  ```
  wsk api create /hello /world get hello --response-type http
  ```
  {: pre}
  
  呼叫更新的 API： 
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  {: pre}

  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
現在，您已完全控制您的 API，而且可以控制內容。例如，傳回 HTML，或設定事項的狀態碼，例如「找不到 (404)」、「未獲授權 (401)」、甚至「內部錯誤 (500)」。


### 公開多個 Web 動作

例如，若您要公開讀書會的一組「動作」，則可以使用一系列的「動作」來實作該讀書會後端：

| 動作| HTTP 方法 (HTTP method)| 說明
|
| ----------- | ----------- | ------------ |
| getBooks| GET| 取得書籍詳細資料|
| postBooks| POST| 新增書籍|
| putBooks| PUT| 更新書籍詳細資料|
| deleteBooks| DELETE| 刪除書籍|

使用 `/club` 作為其 HTTP URL 基本路徑、`books` 作為其資源，建立讀書會的 API（名為 `Book Club`）。
```
wsk api create -n "Book Club" /club /books get getBooks --response-type http
wsk api create /club /books post postBooks              --response-type http
wsk api create /club /books put putBooks                --response-type http
wsk api create /club /books delete deleteBooks          --response-type http
```

請注意，以基本路徑 `/club` 公開的第一個「動作」會取得名為 `Book Club` 的 API 標籤。在 `/club` 下公開的任何其他「動作」都會與 `Book Club` 相關聯。

使用下列指令來列出所有公開的「動作」：
```
wsk api list -f
```
{: pre}

```
ok: APIs
Action: getBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: get
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: postBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: post
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: putBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: put
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: deleteBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: delete
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

為了好玩，您可以利用 HTTP __POST__ 來新增書籍 `JavaScript: The Good Parts`：
```
curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": "success"
}
```

透過 HTTP __GET__ 使用動作 `getBooks` 來取得清單
```
curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
}
```

### 匯出配置
將名為 `Book Club` 的 API 匯出至檔案，該檔案可作為基本，使用檔案作為輸入來重建 API。 
```
wsk api get "Book Club" > club-swagger.json
```
{: pre}

先刪除一般基本路徑下的所有已公開 URL，來測試 Swagger 檔案。
您可以使用基本路徑 `/club` 或 API 名稱標籤 `"Book Club"` 來刪除所有已公開者：
```
wsk api delete /club
```
```
ok: deleted API /club
```
### 變更配置

您可以在「OpenWhisk 儀表板」中編輯配置，按一下 [API](https://console.ng.bluemix.net/openwhisk/apimanagement) 標籤來設定安全、比率限制及其他特性。

### 匯入配置

現在，使用檔案 `club-swagger.json` 來還原名為 `Book Club` 的 API。
```
wsk api create --config-file club-swagger.json
```
{: pre}

```
ok: created api /books delete for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books get for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books post for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books put for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

驗證是否已重建 API：
```
wsk api list /club
```
{: pre}

```
ok: apis
Action                    Verb         API Name        URL
getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
