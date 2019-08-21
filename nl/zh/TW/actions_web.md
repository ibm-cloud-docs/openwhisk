---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: web actions, serverless, functions

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


# 建立 Web 動作
{: #actions_web}

建立 Web 動作時，獲得的結果是可用於從任何 Web 應用程式觸發動作的 URL。
{: shortdesc}

## 為什麼要使用 Web 動作而不使用標準動作？

### 1. 以匿名方式執行 Web 動作

Web 動作啟動與建立動作的使用者（而不是與動作的呼叫程式）相關聯。通常，對於向 Github 等 APP 發出的 API 呼叫，將在 API 呼叫中包含使用者名稱和記號，以表示特定使用者或功能 ID。使用 Web 動作時，無需這些類型的認證。Web 動作可透過 REST 介面進行存取，無需認證。

雖然無需將認證與 Web 動作配合使用，但您可以實作自己的鑑別和授權，即 OAuth 流程。若要將 Web 動作配置為使用認證，請參閱[保護 Web 動作](#actions_web_secure)。

### 2. 使用任何類型的 HTTP 要求

依預設，動作僅接受 `POST` 要求，但 Web 動作可透過 HTTP 方法 `GET`、`POST`、`PUT`、`PATCH` 和 `DELETE` 中的任一種方法以及 `HEAD` 和 `OPTIONS` 進行呼叫。

### 3. 從任何位置觸發 Web 動作

建立 {{site.data.keyword.openwhisk}} Web 動作時，會產生 URL，用於從任何 Web 型應用程式呼叫該動作。非 Web 動作需要鑑別，並且必須使用 JSON 物件進行回應。 

Web 動作 API 路徑可用於 cURL 或 `wget`，甚至可以直接在瀏覽器中輸入。可以使用結構如下的 URL 來呼叫 Web 動作：`https://<apihost>/api/v1/web/<namespace>/<packagename>/<actionname>.<ext>`.

### 4. 建立較少的 {{site.data.keyword.openwhisk_short}} 實體

由於可以從任何位置呼叫 Web 動作，因此無需建立其他 {{site.data.keyword.openwhisk_short}} 實體，如觸發程式或規則。

## Web 動作是如何運作的？

Web 動作可在未鑑別的情況下呼叫，而且可用來實作回應不同類型的 `headers`、`statusCode` 及 `body` 內容的 HTTP 處理程式。

Web 動作必須傳回 JSON 物件。但是，如果 Web 動作的結果包含下列一個以上內容作為最上層 [JSON 內容](#web_action_properties)，則控制器將以不同的方式處理 Web 動作。
{: shortdesc}

## Web 動作的可用特性
{: #actions_web_extra}

Web 動作支援下列特性：

|特性|說明|
| --- | --- |
|[內容副檔名](#extra_features)|可以為 HTTP 要求指定內容類型，例如 `.json`、`.html`、`.http`、`.svg` 或 `.text`。如果未指定內容類型，將採用 `.http` 副檔名。可以透過將副檔名新增到 URI 中的動作名稱來指定內容類型，以便將 `demo/hello` 動作作為 `/demo/hello.svg` 參照。 `.json` 及 `.http` 副檔名不需要投射路徑，而 `.html`、`.svg` 及 `.text` 副檔名則需要。將採用預設路徑來與副檔名相符合。若要呼叫 Web 動作並接收 `.html` 回應，動作必須回應包含稱為 `html` 的最上層內容的 JSON 物件（或者回應必須位於明確的路徑中）。換言之，`/<namespace>/demo/hello.html` 相當於明確地投射 `html` 內容，如 `/<namespace>/demo/hello.html/html` 中一樣。動作的完整名稱必須包括其套件名稱，如果動作不在具名套件中，其為 `default`。|
|[對結果中的欄位投射](#projecting_fields)|接在動作名稱後面的路徑是用來投射回應的一個以上層次。例如，`/demo/hello.html/body`。此特性容許傳回字典 `{body: "..." }` 的任何 Web 操作對 `body` 內容進行投射，並直接傳回其字串值，而不是其字典值。投射路徑遵循絕對路徑模型（如 XPath）。|
|[查詢和內文參數作為輸入](#query_test)|動作會接收查詢參數以及要求內文中的參數。參數的合併優先順序是：套件參數、動作參數、查詢參數及內文參數。這些參數每一個都會在重疊時覆寫任何先前的值。例如，`/demo/hello.http?name=Jane` 可以將引數 `{name: "Jane"}` 傳遞到動作。|
|[表單資料](#form_data)|除了標準 `application/json` 外，Web 動作還可以接收 URL 編碼的表單資料 `application/x-www-form-urlencoded data` 作為輸入。
|[使用多個 HTTP 動詞啟動](#actions_web_options)|Web 動作可透過 HTTP 方法 `GET`、`POST`、`PUT`、`PATCH` 和 `DELETE` 中的任一種方法以及 `HEAD` 和 `OPTIONS` 進行呼叫。|
|[非 JSON 內文和原始 HTTP 實體處理](#actions_web_raw_enable)|Web 動作可接受非 JSON 物件的 HTTP 要求內文，並可選擇一律接收此類值作為不透明值（不是二進位檔時為純文字，其他情況均為 Base64 編碼的字串）。|

## 建立 Web 動作
{: #actions_web_example}

若要建立 Web 動作，請執行以下動作： 

1. 將下列 JavaScript 程式碼儲存為 `hello.js`。

  ```javascript
  function main({name}) {
    var msg = 'You did not tell me who you are.';
    if (name) {
      msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
  }
  ```
  {: codeblock}

2. 建立 `demo` 套件。除非明確指定套件名稱，否則套件名稱為 `default`。
  ```
  ibmcloud fn package create demo
  ```
  {: pre}

3. 建立 `hello` 動作。
    在此範例中，`packageName/actionName` 為 `demo/hello`。將 `<filepath>` 變數取代為 `hello.js` 檔案的檔案路徑，並將 `--web` 旗標設定為 `true`。 

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

4. 在不使用任何參數的情況下呼叫或測試 `hello` Web 動作。取代 `<apihost>` 及 `<namespace>` 變數。若要取得 `<apihost>`，請執行 `ibmcloud fn property get --apihost`。`<apihost>` 範例：`us-south.functions.cloud.ibm.com`。

  對於啟用 IAM 的名稱空間，請將 `<namespace>` 變數取代為名稱空間 ID。若要取得該 ID，請執行 `ibmcloud fn namespace get <namespace_name>`。
  {: note}

  a. 您可以藉由下列任一方法測試 Web 動作： 
    * 在瀏覽器中使用下列結構開啟 URL：`https://<apihost>/api/v1/web/<namespace>/demo/hello`。
    * 使用 cURL 指令來測試動作。
      ```
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

    * 使用 `wget` 指令來測試動作。  
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

  b. 動作碼會傳回下列字典。
    ```
    {body: `<html><body><h3>${msg}</h3></body></html>`}
    ``` 
    {: screen}
    
  還可以透過使用下列指令僅傳回 `body` 內容來測試動作：
  {: #projecting_fields}

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello.html/body
    ```
    {: pre}

    **輸出範例**

    由於未指定 `<name>` 參數，因此會傳回下列訊息。
    ```
    <html><body><h3>You did not tell me who you are.</h3></body></html>
    ```
    {: screen}

5. 現在，請嘗試定義 `<name>` 參數。透過以下任一方式來測試使用 `<name>` 參數的動作：
  * 在瀏覽器中開啟 `https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane`。 
  * 使用 cURL 指令來測試動作。

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}
  * 使用 `wget` 指令來測試動作。  
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}

  **輸出範例**
  ```
  <html><body><h3>Hello, Jane!</h3></body></html>
  ```
  {: screen}


**後續步驟**

將 `hello` Web 動作的 URL 新增到 Web 應用程式，並在其中對其測試。

### Web 動作 JSON 內容
{: #web_action_properties}

HTTP 回應的預設 `Content-Type` 為 `application/json`，並且 body 可以是容許的任何 JSON 值。如果 `Content-Type` 不是 `application/json`，則必須在動作碼的 `headers` 中指定 `Content-Type`。

如果達到動作的[結果大小限制](/docs/openwhisk?topic=cloud-functions-limits)，則回應會失敗。如果您知道動作結果大於 5 MB，請設定[物件儲存庫](/docs/openwhisk?topic=cloud-functions-pkg_obstorage)。

|JSON 內容|說明|
| --- | --- |
|`headers`|JSON 物件，其中鍵是標頭名稱，值是字串、數字或布林值。若要傳送單一標頭的多個值，則標頭的值是多個值的 JSON 陣列。依預設，不會設定任何標頭。|
|`statusCode`|有效的 HTTP 狀態碼。如果存在內文內容，則預設值為 `200 OK`。如果不存在任何內文內容，則預設值為 `204 No Content`。|
|`body`|字串，為純文字（JSON 物件或陣列），或者為 Base64 編碼的字串（對於二進位資料）。如果內文為 `null`、空字串 `""` 或未定義，則會將其視為空白。預設值是空的內文。|

[控制器](/docs/openwhisk?topic=cloud-functions-about#about_controller)將任何操作指定的標頭、狀態碼或內文傳遞到 HTTP 用戶端，後者將終止要求或回應。如果未在動作結果的 `headers` 中宣告 `Content-Type` 標頭，則會將內文解譯為非字串值的 `application/json`，否則會解譯為 `text/html`。如果已定義 `Content-Type` 標頭，則控制器會判斷回應是二進位資料還是純文字，並視需要使用 base64 解碼器來解碼字串。如果內文未正確解碼，則會將錯誤傳回給用戶端。

Web 動作的擁有者擁有所有啟動記錄，並且會產生在系統中執行該動作的成本，無論該動作的呼叫動作的方式為何。
{: note}

#### 受保護的參數
動作參數是受保護的，只能透過更新動作進行變更。系統會自動完成參數，以啟用 Web 動作。

```
ibmcloud fn action create /<namespace>/demo/hello hello.js --parameter name Jane --web true
```
{: pre}


這些變更的結果為 `name` 已連結至 `Jane`，而且無法因最終註釋而置換為查詢或內文參數。此設計可保護對意外或有意嘗試變更此值的查詢或內文參數所執行的動作。

### 使用 Web 動作執行 HTTP 重新導向
{: #http_redirect}
您可以在 Web 應用程式中使用此特性來將使用者重新導向到網站的新版本。

**開始之前**
請完成[建立 Web 動作](#actions_web_example)中的步驟，建立 `demo` 套件及 `hello` Web 動作。

建立執行 HTTP 重新導向的 Web 動作：

1. 將程式碼儲存為 `hello.js`。

  ```javascript
  function main() {
    return {
      headers: { location: 'https://cloud.ibm.com/openwhisk/' },
      statusCode: 302
    }
  }
  ```
  {: codeblock}

2. 以新版的 `hello.js` 程式碼更新您的 `hello` Web 動作。將 `<filepath>` 取代為您 `hello.js` 檔案的檔案路徑。

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. 測試 `hello` Web 動作。取代 `<apihost>` 及 `<namespace>` 變數。您可以藉由下列任一方法測試 Web 動作：

  * 在瀏覽器中開啟 URL `https://<apihost>/api/v1/web/<namespace>/demo/hello`。 
  * 執行下列 cURL 指令：
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * 執行下列 `wget` 指令：
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

  **範例結果** 
  
  此範例 Web 動作將瀏覽器重新導向到 [{{site.data.keyword.openwhisk_short}} 儀表板](https://cloud.ibm.com/openwhisk/){: external}。

### 使用 Web 動作設定 Cookie
{: #multiple_cookie}
成功登入後，您可以在 Web 應用程式中使用此特性將 JSON Web 記號儲存為階段作業 Cookie。

若要建立用於設定多個 Cookie 的 Web 動作，請執行以下動作：

**開始之前**
請完成[建立 Web 動作](#actions_web_example)中的步驟，建立 `demo` 套件及 `hello` Web 動作。

1. 將程式碼儲存為 `hello.js`。
  ```javascript
  function main() {
    return {
      headers: {
        'Set-Cookie': [
        'UserID=Jane; Max-Age=3600; Version=',
        'SessionID=asdfgh123456; Path = /'
      ],
      'Content-Type': 'text/html'
    }, 
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
  }
  ```
  {: codeblock}

2. 以新版的 `hello.js` 程式碼更新您的 `hello` Web 動作。將 `<filepath>` 取代為您 `hello.js` 檔案的檔案路徑。

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}
    
3. 測試動作之前，請清除瀏覽器的 Cookie。

4. 透過在瀏覽器中開啟 URL 來測試 `hello` Web 動作。取代 `<apihost>` 和 `<namespace>` 變數並開啟 `https://<apihost>/api/v1/web/<namespace>/demo/hello`。`<apihost>` 範例：`us-south.functions.cloud.ibm.com`。

**結果**

Cookie `UserID=Jane` 和 `SessionID=asdfgh123456` 已在瀏覽器的開發人員工具中進行設定。


### 使用 Web 動作傳回影像
{: #return_image}
您可以在 Web 應用程式中使用此特性來根據使用者語言環境傳回國家旗標的影像。

**開始之前** 

請完成[建立 Web 動作](#actions_web_example)中的步驟，建立 `demo` 套件及 `hello` Web 動作。

若要建立用於傳回 `image/png` 的 Web 動作，請執行以下動作： 

1. 將程式碼儲存為 `hello.js`。

  ```javascript
  function main() {
      let png = '<base 64 encoded string';
      return { headers: { 'Content-Type': 'image/png' },
              statusCode: 200,
              body: png };
  }
  ```
  {: codeblock}

2. 以新版的 `hello.js` 程式碼更新您的 `hello` Web 動作。將 `<filepath>` 取代為您 `hello.js` 檔案的檔案路徑。

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. 在瀏覽器中測試動作，或是使用 cURL 指令測試動作。取代 `<apihost>` 及 `<namespace>` 變數。您可以藉由下列任一方法測試 Web 動作：

  * 在瀏覽器中開啟 URL `https://<apihost>/api/v1/web/<namespace>/demo/hello`。 
  * 執行下列 cURL 指令。
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * 執行下列 `wget` 指令。
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}



### 使用 Web 動作傳回 JSON
{: #return_json}
您可以在 Web 應用程式中使用此特性來傳回使用者 IP 資訊的 JSON 物件。

**開始之前** 

請完成[建立 Web 動作](#actions_web_example)中的步驟，建立 `demo` 套件及 `hello` Web 動作。

若要建立用於傳回 `application/json` 的 Web 動作，請執行以下動作：

1. 將程式碼儲存為 `hello.js`。
  ```javascript
  function main(params) {
      return {
          statusCode: 200,
          headers: { 'Content-Type': 'application/json' },
          body: params
      };
  }
  ```
  {: codeblock}

2. 以新版的 `hello.js` 程式碼更新您的 `hello` Web 動作。將 `<filepath>` 取代為您 `hello.js` 檔案的檔案路徑。
  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. 在瀏覽器中測試動作，或是使用 cURL 指令測試動作。取代 `<apihost>` 及 `<namespace>` 變數。您可以藉由下列任一方法測試 Web 動作：
  * 在瀏覽器中開啟 URL `https://<apihost>/api/v1/web/<namespace>/demo/hello`。 
  * 執行下列 cURL 指令：
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * 執行下列 `wget` 指令：
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

    **輸出範例**

    ```
    {
      "__ow_headers": {
        "accept": "*/*",
        "accept-encoding": "gzip",
        "cdn-loop": "cloudflare",
        "cf-connecting-ip": "XX.XXX.XXX.XXX",
        "cf-ipcountry": "US",
        "cf-ray": "4d9f3e442a86cf28-IAD",
        "cf-visitor": "{\"scheme\":\"https\"}",
        "host": "<apihost>",
        "user-agent": "curl/7.54.0",
        "x-forwarded-for": "XX.XXX.XX.XXX, XX.XXX.XX.XXX",
        "x-forwarded-host": "<apihost>",
        "x-forwarded-port": "443",
        "x-forwarded-proto": "https",
        "x-global-k8fdic-transaction-id": "11fd03071bd0841d3a00f52354ab880f",
        "x-real-ip": "XXX.XX.XX.XX",
        "x-request-id": "11fd03071bd0841d3a00f52354ab880f"
      },
      "__ow_method": "get",
      "__ow_path": ""
    }
    ```
    {: screen}


### HTTP 環境定義
{: #actions_web_context}

所有 Web 動作在呼叫時都會接收 HTTP 要求詳細資料以作為動作引數的輸入參數。

| HTTP 參數|類型|說明|
| --- | --- | --- |
| `__ow_method` |字串|要求的 HTTP 方法。|
| `__ow_headers` |將字串對映到字串|要求標頭。|
| `__ow_path` |字串|要求的未相符路徑（比對會在使用動作副檔名之後停止）。|
| `__ow_user` |字串|用於識別經 {{site.data.keyword.openwhisk_short}} 鑑別的主旨的名稱空間。|
| `__ow_body` |字串|要求內文實體，內容為二進位檔時為 Base64 編碼的字串，其他情況均為純文字字串。|
| `__ow_query` |字串|要求中的查詢參數（以未解析字串的形式提供）。|

要求無法置換任何具名的 `__ow_` 參數。這麼做會導致要求失敗，其狀態等於「400 不正確的要求」。

只有在 Web 動作[已註釋為需要鑑別](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions)，並容許 Web 動作實作自己的授權原則時，`__ow_user` 才會存在。只有在 Web 動作選擇要處理[「原始」HTTP 要求](#actions_web_raw_enable)時，才能使用 `__ow_query`。`__ow_query` 是包含從 URI 中剖析出的查詢參數（用 `&` 分隔）的字串。在原始 HTTP 要求中，或者當 HTTP 要求實體不是 JSON 物件或表單資料時，會顯示 `__ow_body` 內容。否則，Web 動作會接收查詢和內文參數，作為動作引數中的第一個類別內容。內文參數的優先順序高於查詢參數，而查詢參數的優先順序高於動作和套件參數。

### HTTPS 端點支援
{: #actions_web_endpoint}

支援的 SSL 通訊協定：TLS 1.2、TLS 1.3（[初版 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18){: external}）

### 改變 Web 動作的回應內容
{: #extra_features}
您可以使用[內容副檔名](#actions_web_extra)來變更 Web 動作的回應內容，以傳回不同的內容類型。
{: shortdesc}

**開始之前**

請完成[建立 Web 動作](#actions_web_example)中的步驟，建立 `demo` 套件及 `hello` Web 動作。

若要變更回應 Web 動作，請執行以下動作：

1. 將下列程式碼儲存為 `hello.js`。

  ```javascript
  function main(params) {
      return { response: params };
  }
  ```
  {: codeblock}

2. 以新版的 `hello.js` 程式碼更新您的 `hello` Web 動作。將 `<filepath>` 取代為您 `hello.js` 檔案的檔案路徑。

  ```bash
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. 在瀏覽器中測試動作，或是使用 cURL 指令測試動作。取代 `<apihost>` 及 `<namespace>` 變數。

  a. 透過以下任一方式來傳回 JSON：
    * 在 Web 瀏覽器中開啟 `https://<apihost>/api/v1/web/<namespace>/demo/hello.json`。 
    * 執行下列 cURL 指令。
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}
    * 執行下列 `wget` 指令。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}

      **輸出範例**

      ```
      {
        "response": {
          "__ow_method": "get",
          "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  b. 使用查詢參數來測試動作。可以透過以下任一方式來測試動作：
  {: #query_test}

    * 執行下列 cURL 指令。

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

    * 執行下列 `wget` 指令。

        ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

      **輸出範例**
      ```
      {
        "response": {
          "name": "Jane",
          "__ow_method": "get",
          "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  c. 您還可以使用表單資料來測試 Web 動作。您可以藉由下列任一方法測試 Web 動作：
  {: #form_data}
  
    * 執行下列 cURL 指令。

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}
      
    * 執行下列 `wget` 指令。
        ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}

      **輸出範例**

      ```
      {
        "response": {
          "name": "Jane",
          "__ow_method": "post",
          "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "content-length": "10",
            "content-type": "application/x-www-form-urlencoded",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  d. 可以透過執行下列指令來指定 JSON 物件。您可以藉由下列任一方法測試 Web 動作：
    * 執行下列 cURL 指令。
        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}
      
    * 執行下列 `wget` 指令。
        ```
        wget https://<apihost>/api/v1/web/{namespace/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}

      **輸出範例**

      ```
      {
        "response": {
          "name": "Jane",
          "__ow_method": "post",
          "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "content-length": "15",
            "content-type": "application/json",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  e. 還可以透過以下任一方式將 `name` 值作為文字傳回：
  * 執行下列 cURL 指令。

      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}
  * 執行下列 `wget` 指令。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}

    **輸出範例**

    ```
    Jane
    ```
    {: screen}

    在標準動作中，查詢參數、表單資料和 JSON 物件內文實體均視為字典，其值可以直接作為動作輸入內容進行存取。但對於處理 HTTP 要求實體的 Web 動作，或者當 Web 動作接收到非 JSON 物件的實體時，此行為就不適用了。
    {: note}

  f. 可以透過以下任一方式來設定 `Content-Type`：
  * 執行下列 cURL 指令。  
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}
    
  * 執行下列 `wget` 指令。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}

    **輸出範例**

    ```
    {
      "response": {
        "__ow_method": "post",
        "__ow_headers": {
          "accept": "*/*",
          "connection": "close",
          "content-length": "4",
          "content-type": "text/plain",
          "host": "172.17.0.1",
          "user-agent": "curl/7.43.0"
        },
        "__ow_path": "",
        "__ow_body": "Jane"
      }
    }
    ```
    {: screen}

## 保護 Web 動作
{: #actions_web_secure}

**開始之前**
請完成[建立 Web 動作](#actions_web_example)中的步驟，建立 `demo` 套件及 `hello` Web 動作。

依預設，任何人都可以使用呼叫 URL 來呼叫 Web 動作。可以透過以下任一方式使用 `require-whisk-auth` [Web 動作註釋](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions)來保護 Web 動作：
  1. 將 `require-whisk-auth` 註釋設定為 `true`。`require-whisk-auth` 註釋設定為 `true` 時，Web 動作將根據 Web 動作擁有者的 Whisk 鑑別金鑰來鑑別呼叫要求的基本授權認證。設定為數字或區分大小寫的字串時，Web 動作的呼叫要求必須包含設定為此相同數字或區分大小寫的字串的 `X-Require-Whisk-Auth` 標頭。受保護的 Web 動作在認證驗證失敗時，將傳回訊息`未獲授權`。

  2. 容許使用 `--web-secure` 旗標自動設定 `require-whisk-auth` 註釋。將 `--web-secure` 旗標設定為 `true` 時，會產生亂數作為 `require-whisk-auth` 註釋值。設為 `false` 時，會移除 `require-whisk-auth` 註釋。設為任何其他值時，會使用該值作為 `require-whisk-auth` 註釋值。

若要測試安全 Web 動作，請執行以下動作：

1. 將下列 JavaScript 程式碼儲存為 `hello.js`。
  ```javascript
  function main({name}) {
    var msg = 'You did not tell me who you are.';
    if (name) {
      msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
  }
  ```
  {: codeblock}

2. 使用 `hello.js` 程式碼的新版本更新 `hello` Web 動作，並將 `--web secure` 旗標設定為 `true`。
  ```bash
  ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true
  ```
  {: pre}

3. 取得 `hello` Web 動作以檢視隨機產生的 `require-whisk-auth` 值。

  ```bash
  ibmcloud fn action get demo/hello
  ```
  {: pre}

    **輸出範例**

    `require-whisk-auth` 值已設定為 `7819991076995522`。
    ```
    {
      "namespace": "<namespace>/demo",
      "name": "hello",
      "version": "0.0.34",
      "exec": {
          "kind": "nodejs:10",
          "binary": false
      },
      "annotations": [
          {
              "key": "web-export",
              "value": true
          },
          {
              "key": "raw-http",
              "value": false
          },
          {
              "key": "final",
              "value": true
          },
          {
              "key": "require-whisk-auth",
              "value": 7819991076995522
          },
          {
              "key": "exec",
              "value": "nodejs:10"
          }
      ],
      "limits": {
          "timeout": 60000,
          "memory": 256,
          "logs": 10,
          "concurrency": 1
      },
      "publish": false
    }
    ```
    {: screen}

若要測試鑑別是否有效，請執行以下操作：

1. 在不設定 `X-Require-Whisk-Auth` 參數的情況下測試 `hello` Web 動作，以驗證是否需要鑑別。此測試將導致錯誤。您可以藉由下列任一方法測試 Web 動作：

  * 使用 cURL 指令來測試 Web 動作。
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}
    
  * 使用 `wget` 指令來測試 Web 動作。
      ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
      {: pre}

   **輸出範例**

    ```
      {
      "code": "4c4423556547f6ac764ee192d4ed27a6",
      "error": "Authentication is possible but has failed or not yet been provided."
    }
    ```
    {: screen}

    呼叫失敗，因為未提供 `X-Require-Whisk-Auth` 值。
    {: note}

2. 現在，測試 `hello` Web 動作並提供隨機產生的 `X-Require-Whisk-Auth` 值。取代 `<apihost>` 和 `<namespace>` 值。將 `<my-secret>` 值取代為在步驟 3 中建立的隨機產生的數字。可以透過以下任一方式來測試 Web 動作：
  * 使用 cURL 指令來測試 Web 動作。
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  * 使用 `wget` 指令來測試 Web 動作。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  **輸出範例**
    
    ```
    {
    "body": "<html><body><h3>Hello, Jane!</h3></body></html>"
    }
    ```
    {: screen}

若要使用自訂 `require-whisk-auth` 值來測試 Web 動作，請執行以下動作：

1. 使用您自己的 `require-whisk-auth` 值更新 `hello` Web 動作。然後，透過在呼叫期間指定 `X-Require-Whisk-Auth` 值來嘗試測試 Web 動作。

  a. 設定 `require-whisk-auth` 值，其中 `<my-secret>` 是區分大小寫的鑑別記號。
    ```bash
    ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true --require-whisk-auth <mysecret>
    ```
    {: pre}
  
  b. 測試 Web 動作並包含 `<my-secret>` 值。您可以藉由下列任一方法測試 Web 動作：
  * 使用 cURL 指令來測試 Web 動作。
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}
  * 使用 `wget` 指令來測試動作。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}


## 原始 HTTP 處理
{: #actions_web_raw}

Web 動作可選擇直接解譯和處理送入 HTTP 內文，而不將 JSON 物件提升為可用於 Web 動作輸入的第一類別內容（例如，`args.name`，而不是剖析 `args.__ow_query`）。透過 `raw-http` [註釋](/docs/openwhisk?topic=cloud-functions-annotations)即可完成此處理程序。使用稍早顯示的相同範例，但現在是作為「原始」HTTP Web 動作，它會接收 `name` 同時作為查詢參數以及 HTTP 要求內文中的 JSON 值：
```bash
curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}


**輸出範例**
```
{
  "response": {
    "__ow_method": "post",
    "__ow_query": "name=Jane",
    "__ow_body": "eyJuYW1lIjoiSmFuZSJ9",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "15",      
      "content-type": "application/json",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```
{: screen}

{{site.data.keyword.openwhisk_short}} 使用 [Akka HTTP](https://doc.akka.io/docs/akka-http/current/?language=scala){: external} 架構來[確定哪些內容類型是二進位檔，哪些是純文字](https://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html){: external}。

### 啟用原始 HTTP 處理
{: #actions_web_raw_enable}

可以透過將 `--web` 設定為 `raw` 來建立原始 HTTP Web 動作。
```bash
ibmcloud fn action create demo/hello /<filepath>/hello.js --web raw
```
{: pre}

### 從 Base64 解碼二進位內文內容
{: #actions_web_decode}

處理原始 HTTP 內容時，如果要求的 `Content-Type` 為 binary 類型，則會對 `__ow_body` 內容進行 Base64 編碼。
下列函數示範如何解碼 Node、Python 及 Swift 中的內文內容。

1. 將偏好語言中的範例程式碼儲存到名為 `decode.<ext>` 的檔案中。將 `<ext>` 取代為偏好語言範例程式碼的副檔名。

  **Node**
  {: #actions_web_decode_js}

  ```javascript
function main(args) {
     decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
    return {body: decoded}
  }
  ```
  {: codeblock}

  **Python**
  {: #actions_web_decode_python}

  ```python
def main(args):
    try:
        decoded = args['__ow_body'].decode('base64').strip()
        return {"body": decoded}
    except:
        return {"body": "Could not decode body from Base64."}
```
  {: codeblock}

  **Swift**
  {: #actions_web_decode_swift}

  ```swift
  extension String {
      func base64Decode() -> String? {
          guard let data = Data(base64Encoded: self) else {
              return nil
          }

          return String(data: data, encoding: .utf8)
      }
  }

  func main(args: [String:Any]) -> [String:Any] {
      if let body = args["__ow_body"] as? String {
          if let decoded = body.base64Decode() {
              return [ "body" : decoded ]
          }
      }

      return ["body": "Could not decode body from Base64."]
  }
  ```
  {: codeblock}

2. 透過執行下列指令，使用範例程式碼建立原始 HTTP Web 動作。在此範例中，Node 函數儲存為 `decode.js`。將檔案路徑取代為 `decode` 檔案的檔案路徑，並更新檔案副檔名以符合您使用的範例程式碼的副檔名。

  ```bash
  ibmcloud fn action create decode <filepath>/decode.js --web raw
  ```
  {: pre}

  **輸出範例**
  ```
  ok: created action decode
  ```
  {: screen}

3. 透過執行下列 cURL 指令來測試 `decode` 動作。
    ```bash
    curl -k -H "content-type: application" -X POST -d "Decoded body" https://<apihost>/api/v1/web/<namespace>/default/decode.json
    ```
    {: pre}

  **輸出範例**
    ```
    {
      "body": "Decoded body"
    }
    ```
    {: screen}

## OPTIONS 要求
{: #actions_web_options}

依預設，對 Web 動作發出的 `OPTIONS` 要求將產生自動新增到回應標頭的 CORS 標頭。這些標頭容許使用所有原點以及 `OPTIONS`、`GET`、`DELETE`、`POST`、`PUT`、`HEAD` 和 `PATCH` HTTP 動詞。
{: shortdesc}

請參閱下列標頭：
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

或者，`OPTIONS` 要求可以由 Web 動作手動處理。若要啟用此選項，請在 Web 動作中新增 `web-custom-options` 註釋及值 `true`。啟用此特性時，不會自動將 CORS 標頭新增至要求回應。您必須改為以程式化方式附加標頭。

若要建立對 `OPTIONS` 要求的自訂回應，請執行以下操作：

1. 將下列程式碼儲存到 `custom-options.js` 檔案中。

  ```js
  function main(params) {
    if (params.__ow_method == "options") {
      return {
        headers: {
          'Access-Control-Allow-Methods': 'OPTIONS, GET',
          'Access-Control-Allow-Origin': 'example.com'
        },
        statusCode: 200
      }
    }
  }
  ```
  {: codeblock}

2. 建立 Web 動作。將 `require-whisk-auth` 設定為 `true`。

  ```bash
  ibmcloud fn action create custom-option <filepath>/custom-options.js --web true -a web-custom-options true
  ```
  {: pre}

3. 使用下列 cURL 指令來測試動作。

  ```bash
  $ curl https://<apihost>/api/v1/web/<namespace>/default/custom-option.http -kvX OPTIONS
  ```
  {: pre}

  **輸出範例**
  ```
  < HTTP/1.1 200 OK
  < Server: nginx/1.11.13
  < Content-Length: 0
  < Connection: keep-alive
  < Access-Control-Allow-Methods: OPTIONS, GET
  < Access-Control-Allow-Origin: example.com
  ```
  {: screen}

## 錯誤處理
{: #actions_web_errors}

{{site.data.keyword.openwhisk_short}} 動作在兩種不同的可能失敗模式下失敗。第一個稱為_應用程式錯誤_，類似於捕捉的異常狀況：動作會傳回包含最上層 `error` 內容的 JSON 物件。第二種是動作失敗且未產生回應時所發生的_開發人員錯誤_（這類似於未捕捉到的異常狀況）。對於 Web 動作，控制器會如下處理應用程式錯誤：

- 忽略所有指定的路徑投射，控制器會改為投射 `error` 內容。
- 控制器會將依動作副檔名所隱含的內容處理套用至 `error` 內容的值。

開發人員必須知道 Web 動作的使用方式，以及如何產生適當的錯誤回應。例如，與 `.http` 副檔名搭配使用的 Web 動作傳回 HTTP 回應，類似於 `{error: { statusCode: 400 }`。無法這麼做時，會導致副檔名中所隱含的 `Content-Type` 與錯誤回應中的動作 `Content-Type` 不相符。必須對具有序列的 Web 動作進行特殊考量，才能讓構成序列的元件可以在必要時產生足夠的錯誤。



## 停用 Web 動作
{: #actions_web_disable}

您可以透過在 CLI 中將 `--web` 旗標設定為 `false` 或 `no` 來停用 Web 動作。將 `<packagename>/<actionname>` 和 `<filepath>/<filename>` 取代為套件名稱、Web 動作名稱以及程式碼檔案的檔案路徑和檔名。

```bash
ibmcloud fn action update <packageName>/<actionName> <filepath>/<filename> --web false
```
{: pre}



