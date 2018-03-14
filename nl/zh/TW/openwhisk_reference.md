---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-15"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.openwhisk_short}} 系統詳細資料
{: #openwhisk_reference}

下列各節提供有關 {{site.data.keyword.openwhisk}} 系統的其他詳細資料。
{: shortdesc}

## {{site.data.keyword.openwhisk_short}} 實體
{: #openwhisk_entities}

### 名稱空間及套件
{: #openwhisk_entities_namespaces}

{{site.data.keyword.openwhisk_short}} 的「動作」、「觸發程式」及「規則」屬於「名稱空間」，有時屬於「套件」。

套件可以包含「動作」及「資訊來源」。套件不能包含另一個套件，因此不容許巢狀套件。實體也不需要包含在套件中。

在 {{site.data.keyword.Bluemix_notm}} 中，組織+空間配對對應至 {{site.data.keyword.openwhisk_short}} 名稱空間。例如，組織 `BobsOrg` 及空間 `dev` 將對應至 {{site.data.keyword.openwhisk_short}} 名稱空間 `/BobsOrg_dev`。

如果您已獲授權，請自行建立名稱空間。`/whisk.system` 名稱空間是保留供隨 {{site.data.keyword.openwhisk_short}} 系統一起配送的實體使用。


### 完整名稱
{: #openwhisk_entities_fullyqual}

實體的完整名稱是 `/namespaceName[/packageName]/entityName`。請注意，`/` 是用來區隔「名稱空間」、「套件」及實體。「名稱空間」的前面也必須加上 `/`。

為了方便起見，如果是使用者的*預設名稱空間*，則「名稱空間」可以保留。

例如，假設有一個使用者的預設「名稱空間」是 `/myOrg`。下列範例說明一些實體的完整名稱及其別名。

| 完整名稱| 別名| 名稱空間| 套件| 名稱|
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read` |  | `/whisk.system` | `cloudant` | `read` |
| `/myOrg/video/transcode` | `video/transcode` | `/myOrg` | `video` | `transcode` |
| `/myOrg/filter` | `filter` | `/myOrg` |  | `filter` |

在其他位置中使用 {{site.data.keyword.openwhisk_short}} CLI 時，您可以使用此命名方法。

### 實體名稱
{: #openwhisk_entities_names}

所有實體（包括「動作」、「觸發程式」、「規則」、「套件」及「名稱空間」）的名稱都是遵循下列格式的一連串字元：

* 第一個字元必須是英數字元或底線。
* 後續字元可以是英數字元、空格或下列任何一值：`_`、`@`、`.`、`-`。
* 最後一個字元不能是空格。

更精確地來說，名稱必須符合下列正規表示式（以 Java meta 字元語法表示）：`\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`。

## 動作語意
{: #openwhisk_semantics}

下列各節說明「{{site.data.keyword.openwhisk_short}} 動作」的詳細資料。

### 無狀態
{: #openwhisk_semantics_stateless}

動作實作是 Stateless 或 *idmpotent*。雖然系統不會強制執行此內容，但是不保證透過「動作」所維護的任何狀態都可在呼叫時使用。

此外，「動作」可能會有多個說明實例，每個說明實例都有自己的狀態。「動作」呼叫可能會分派至其中的任何實例。

### 呼叫輸入及輸出
{: #openwhisk_semantics_invocationio}

「動作」的輸入及輸出是鍵值組的字典。索引鍵是字串，而值是有效的 JSON 值。

### 動作的呼叫順序
{: #openwhisk_ordering}

「動作」不是依序進行呼叫。如果使用者從指令行或 REST API 呼叫「動作」兩次，則可能會先執行第二次呼叫，再執行第一次呼叫。如果「動作」有負面影響，則可能會依任意順序觀察到它們。

此外，不保證「動作」會自動執行。兩個「動作」可以同時執行，且其負面影響可能會交錯。OpenWhisk 無法確保任何特定並行一致性模型是否有負面影響。任何並行性負面影響都是根據實作。

### 動作執行保證
{: #openwhisk_atmostonce}

收到呼叫要求時，系統會記錄要求，並分派啟動。

系統會傳回啟動 ID（使用非區塊處理呼叫）確認已接收到該啟動 ID。
如果在收到 HTTP 回應之前發生網路失敗或其他干擾失敗，則可能是 {{site.data.keyword.openwhisk_short}} 已接收並處理該要求。

系統嘗試呼叫「動作」一次，導致下列四種結果的其中一種：
- *成功*：已順利完成「動作」呼叫。
- *應用程式錯誤*：「動作」呼叫成功，但「動作」故意傳回錯誤值（例如，因為引數的前置條件不相符）。
- *動作開發人員錯誤*：已呼叫「動作」，但異常完成（例如，「動作」偵測不到異常狀況，或有語法錯誤）。
- *Whisk 內部錯誤*：系統無法呼叫「動作」。結果會記錄在啟動記錄的 `status` 欄位中（如下節所記載）。

每個成功收到的呼叫以及每個可能向使用者收費的呼叫，都有一筆啟動記錄。

當結果是*動作開發人員錯誤* 時，可能會局部執行「動作」，並產生外部可見的負面影響。使用者必須負責檢查是否發生這類負面影響，如有需要，請發出重試邏輯。某些 *Whisk 內部錯誤* 指出「動作」開始執行，但在「動作」完成登錄之前就失敗了。

## 啟動記錄
{: #openwhisk_ref_activation}

每一個「動作」呼叫和「觸發程式」發動都會產生一筆啟動記錄。

啟動記錄包含下列欄位：

- *activationId*：啟動 ID。
- *start* 及 *end*：記錄啟動開始及結束的時間戳記。值為 [UNIX 時間格式](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15)。
- *namespace* 及 `name`：實體的名稱空間及名稱。
- *logs*：字串陣列，內含「動作」在其啟動期間所產生的日誌。每一個陣列元素都對應至「動作」之 `stdout` 或 `stderr` 的行輸出，並且包含日誌輸出的時間及串流。結構如下：`TIMESTAMP STREAM: LOG_OUTPUT`。
- *response*：定義索引鍵 `success`、`status` 及 `result` 的字典：
  - *status*：啟動結果，可能是下列其中一個值："success"、"application error"、"action developer error"、"whisk internal error"。
  - *success*：假設並且只有在狀態為 `"success"` 時，才會為 `true`
- *result*：包含啟動結果的字典。如果順利啟動，則結果會包含「動作」所傳回的值。如果啟動失敗，`result` 會包含 `error` 索引鍵，通常還會有失敗的說明。


## JavaScript 動作
{: #openwhisk_ref_javascript}

### 函數原型
{: #openwhisk_ref_javascript_fnproto}

{{site.data.keyword.openwhisk_short}} JavaScript 動作在 Node.js 執行時期中執行。

以 JavaScript 撰寫的動作必須限制於單一檔案。此檔案可以包含多個函數，但依慣例，必須要有一個稱為 `main` 的函數，並且在呼叫「動作」時呼叫此函數。例如，下列範例顯示具有多個函數的「動作」。
```javascript
function main() {
    return { payload: helper() }
}

function helper() {
    return new Date();
}
```
{: codeblock}

「動作」輸入參數作為 JSON 物件，當作參數傳遞至 `main` 函數。順利啟動的結果也是 JSON 物件，但會根據「動作」是同步或非同步（如下列一節所述），以不同的方式傳回。


### 同步及非同步行為
{: #openwhisk_ref_javascript_synchasynch}

JavaScript 函數很常會在回呼函數中繼續執行，即使是函數返回之後也是一樣。為了適應此行為，「JavaScript 動作」的啟動可以是*同步* 或*非同步*。

如果 main 函數在下列其中一種狀況下結束，則「JavaScript 動作」的啟動是**同步**：

- main 函數結束，而未執行 `return` 陳述式。
- main 函數結束的原因為執行會傳回任何值（Promise *除外*）的 `return` 陳述式。

請參閱下列同步「動作」範例：

```javascript
// an Action in which each path results in a synchronous activation
function main(params) {
  if (params.payload == 0) {return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
    return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}

如果 main 函數因為傳回 Promise 而結束，則「JavaScript 動作」的啟動為**非同步**。在此情況下，系統會假設「動作」仍在執行中，直到履行或拒絕 Promise。
首先，開始實例化新 Promise 物件，並將回呼函數傳遞給它。此回呼接受兩個引數（resolve 及 reject），而這兩者同時也是函數。所有非同步程式碼都在該回呼內。

在下列範例中，您可以呼叫 resolve 函數來查看如何完成 Promise。
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
            resolve({ done: true });
       }, 100);
    })
 }
```
{: codeblock}

這個範例顯示如何呼叫 reject 函數來拒絕 Promise。
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
            reject({ done: true });
       }, 100);
    })
 }
```
{: codeblock}

在某些輸入上，「動作」可能會同步，如下列範例所示。 
```javascript
  function main(params) {
      if (params.payload) {
         // asynchronous activation
         return new Promise(function(resolve, reject) {
                setTimeout(function() {
            resolve({ done: true });
                }, 100);
             })
      } else {
// synchronous activation
         return {done: true};
      }
  }
```
{: codeblock}

不論啟動是同步還是非同步，「動作」的呼叫都可以是區塊處理或非區塊處理。

### 已移除 JavaScript 廣域 whisk 物件

已移除廣域物件 `whisk`；請移轉您的「nodejs 動作」以使用替代方法。
對於 `whisk.invoke()` 和 `whisk.trigger()` 函數，請使用已安裝的用戶端程式庫 [openwhisk](https://www.npmjs.com/package/openwhisk)。
對於 `whisk.getAuthKey()`，您可以從環境變數 `__OW_API_KEY` 取得 API 金鑰值。
對於 `whisk.error()`，您可以傳回拒絕的 Promise（即，Promise.reject）。

### JavaScript 執行時期
{: #openwhisk_ref_javascript_environments}

「JavaScript 動作」可以在 Node.js 第 6 版或 Node.js 第 8 版中執行。
目前「動作」依預設在 Node.js 6.11.4 版環境中執行。  

### Node.js 第 6 版環境
{: #openwhisk_ref_javascript_environments_6}
如果在建立或更新「動作」時明確指定 `--kind` 旗標，且值為 `nodejs:6`，則會使用 Node.js 6.12.2 環境。

下列套件適用於 Node.js 6.12.2 環境：

- [apn 2.1.2 版](https://www.npmjs.com/package/apn)- Node.js 模組，以與 Apple Push Notification 服務互動。
- [async 2.1.4 版](https://www.npmjs.com/package/async) - 提供函數以與非同步函數搭配使用。
- [btoa 1.1.2 版](https://www.npmjs.com/package/btoa) - 瀏覽器 btoa 功能的埠。
- [cheerio 0.22.0 版](https://www.npmjs.com/package/cheerio) - 針對伺服器而特別設計的核心 jQuery 實作，快速精簡且有彈性。
- [cloudant 1.6.2 版](https://www.npmjs.com/package/cloudant) - Node.js 的官方 Cloudant 程式庫。
- [commander 2.9.0 版](https://www.npmjs.com/package/commander) - Node.js 指令行介面的完整解決方案。
- [consul 0.27.0 版](https://www.npmjs.com/package/consul) - Consul 的用戶端，包含服務探索及配置。
- [cookie-parser 1.4.3 版](https://www.npmjs.com/package/cookie-parser) - 剖析 Cookie 標頭，並在 req.cookies 中移入 Cookie 名稱所含索引鍵的物件。
- [cradle 0.7.1 版](https://www.npmjs.com/package/cradle) - 適用於 Node.js 的高階、快取、CouchDB 用戶端。
- [errorhandler 1.5.0 版](https://www.npmjs.com/package/errorhandler) - 僅限開發錯誤處理常式中介軟體。
- [glob 7.1.1 版](https://www.npmjs.com/package/glob) - 使用 Shell 所用的型樣來比對檔案，例如，星號及其他字元。
- [gm 1.23.0 版](https://www.npmjs.com/package/gm) - 適用於 Node 的 GraphicsMagick 及 ImageMagick。
- [lodash 4.17.2 版](https://www.npmjs.com/package/lodash) - 匯出為 Node.js 模組的 Lodash 程式庫。
- [log4js 0.6.38 版](https://www.npmjs.com/package/log4js) - 設計為使用 Node 的 log4js 架構的轉換。 
- [iconv-lite 0.4.15 版](https://www.npmjs.com/package/iconv-lite) - Pure JS 字元編碼轉換
- [marked 0.3.6 版](https://www.npmjs.com/package/marked) - 以 JavaScript 撰寫的全功能 Markdown 剖析器和編譯器。專為提供速度而打造。
- [merge 1.2.0 版](https://www.npmjs.com/package/merge) - 將多個物件合併成一個，以建立新的複製物件。 
- [moment 2.17.0 版](https://www.npmjs.com/package/moment) - 用於剖析、驗證、操作及格式化日期的輕量型 JavaScript 日期程式庫。
- [mongodb 2.2.11 版](https://www.npmjs.com/package/mongodb) - 適用於 Node.js 的正式 MongoDB 驅動程式。
- [mustache 2.3.0 版](https://www.npmjs.com/package/mustache) - Mustache.js 是 JavaScript 中 mustache 範本系統的實作。
- [nano 6.2.0 版](https://www.npmjs.com/package/nano)- Node.js 的簡約 couchdb 驅動程式。
- [node-uuid 1.4.7 版](https://www.npmjs.com/package/node-uuid) - 已淘汰的 UUID 套件。 
- [nodemailer 2.6.4 版](https://www.npmjs.com/package/nodemailer) - 從 Node.js 傳送電子郵件 - 容易使用！
- [oauth2-server 2.4.1 版](https://www.npmjs.com/package/oauth2-server) - 完整、相容及完整測試的模組，用於實作以 Node.js 表示的 OAuth2 Server/Provider。
- [openwhisk 3.11.0 版](https://www.npmjs.com/package/openwhisk) - OpenWhisk 平台的 JavaScript 用戶端程式庫。在 OpenWhisk API 附近提供封套。
- [pkgcloud 1.4.0 版](https://www.npmjs.com/package/pkgcloud) - pkgcloud 是一個 Node.js 標準程式庫，可抽象化多個雲端提供者之間的差異。
- [process 0.11.9 版](https://www.npmjs.com/package/process) - require('process')；就像任何其他模組。
- [pug 2.0.0-beta6 版](https://www.npmjs.com/package/pug) - 實作 Pug 範本語言。
- [redis 2.6.3 版](https://www.npmjs.com/package/redis) - 適用於 Node.js 的完整且功能豐富的 Redis 用戶端。 
- [request 2.79.0 版](https://www.npmjs.com/package/request) - 要求是發出 HTTP 呼叫的最簡單方式。
- [request-promise 4.1.1 版](https://www.npmjs.com/package/request-promise) - 具有 Promise 支援的簡化 HTTP 要求用戶端 'request'。採用 Bluebird 技術。
- [rimraf 2.5.4 版](https://www.npmjs.com/package/rimraf) - 適用於 Node 的 UNIX 指令 rm -rf。
- [semver 5.3.0 版](https://www.npmjs.com/package/semver) - 支援語意版本化。
- [sendgrid 4.7.1 版](https://www.npmjs.com/package/sendgrid) - 透過 SendGrid API 提供電子郵件支援。
- [serve-favicon 2.3.2 版](https://www.npmjs.com/package/serve-favicon) - 負責處理 Favicon 的 Node.js 中介軟體。
- [socket.io 1.6.0 版](https://www.npmjs.com/package/socket.io) - Socket.IO 可啟用即時雙向事件型通訊。
- [socket.io-client 1.6.0 版](https://www.npmjs.com/package/socket.io-client) - Socket.IO 的用戶端支援。
- [superagent 3.0.0 版](https://www.npmjs.com/package/superagent) - SuperAgent 是小型的漸進式用戶端 HTTP 要求程式庫及具有相同 API 的 Node.js 模組，其中包含許多高階 HTTP 用戶端特性。
- [swagger-tools 0.10.1 版](https://www.npmjs.com/package/swagger-tools) - 與使用 Swagger 相關的工具，用來記載 API 的一種方式。
- [tmp 0.0.31 版](https://www.npmjs.com/package/tmp) - node.js 的簡式暫存檔及目錄建立者。
- [twilio 2.11.1 版](https://www.npmjs.com/package/twilio) - 用於語音、視訊及傳訊的相關 Twilio API 的封套。
- [underscore 1.8.3 版](https://www.npmjs.com/package/underscore) - Underscore.js 是 JavaScript 的公用程式程式庫，支援一般的功能可疑物件（每個、對映、減少、過濾器...），而不延伸任何核心 JavaScript 物件。
- [uuid 3.0.0 版](https://www.npmjs.com/package/uuid) - 簡單、快速產生 RFC4122 UUID。
- [validator 6.1.0 版](https://www.npmjs.com/package/validator) - 字串驗證器和消毒器的程式庫。
- [watson-developer-cloud 2.29.0 版](https://www.npmjs.com/package/watson-developer-cloud) - Node.js 用戶端程式庫要使用 Watson Developer Cloud 服務，這是一組使用認知運算來解決複雜問題的 API 集合。
- [when 3.7.7 版](https://www.npmjs.com/package/when) - When.js 是一種穩固的實線、未經測試的 Promisses/A+ 和 when() 實作，包括完整的 ES6 Promise shim。
- [winston 2.3.0 版](https://www.npmjs.com/package/winston) - node.js 的多重傳輸非同步記載程式庫。"CHILL WINSTON! ... 我把它放在日誌裡。"
- [ws 1.1.1 版](https://www.npmjs.com/package/ws) - ws 為使用簡單、超快、完整測試的 WebSocket 用戶端及伺服器實作。
- [xml2js 0.4.17 版](https://www.npmjs.com/package/xml2js) - JavaScript 物件轉換器的簡單 XML。它支援雙向轉換。
- [xmlhttprequest 1.8.0 版](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest 是內建 HTTP 用戶端用來模擬瀏覽器 XMLHttpRequest 物件的封套。
- [yauzl 2.7.0 版](https://www.npmjs.com/package/yauzl) - 還有另一個適用於 Node 的解壓縮程式庫。用於壓縮。

### Node.js 第 8 版環境
{: #openwhisk_ref_javascript_environments_8}
如果在建立或更新「動作」時明確指定 `--kind` 旗標，且值為 `nodejs:8`，則會使用 Node.js 8.9.3 版環境。

下列套件已預先安裝在 Node.js 8.9.3 版環境中：

  - [apn 2.1.5 版](https://www.npmjs.com/package/apn)- Node.js 模組，以與 Apple Push Notification 服務互動。
  - [async 2.6.0 版](https://www.npmjs.com/package/async) - 提供函數以與非同步函數搭配使用。
  - [bent 1.1.0 版](https://www.npmjs.com/package/btoa) - 瀏覽器 btoa 功能的埠。
  - [btoa 1.1.2 版](https://www.npmjs.com/package/btoa) - 瀏覽器 btoa 功能的埠。
  - [cloudant 1.10.0 版](https://www.npmjs.com/package/cloudant) - 這是 Node.js 的官方 Cloudant 程式庫。
  - [commander 2.12.2 版](https://www.npmjs.com/package/commander) - Node.js 指令行介面的完整解決方案。
  - [consul 0.30.0 版](https://www.npmjs.com/package/consul) - Consul 的用戶端，包含服務探索及配置。
  - [cookie-parser 1.4.3 版](https://www.npmjs.com/package/cookie-parser) - 剖析 Cookie 標頭，並在 req.cookies 中移入 Cookie 名稱所含索引鍵的物件。
  - [cradle 0.7.1 版](https://www.npmjs.com/package/cradle) - 適用於 Node.js 的高階、快取、CouchDB 用戶端。
  - [errorhandler 1.5.0 版](https://www.npmjs.com/package/errorhandler) - 僅限開發錯誤處理常式中介軟體。
  - [glob 7.1.2 版](https://www.npmjs.com/package/glob) - 使用 Shell 所用的型樣來比對檔案，例如，星號及其他字元。
  - [gm 1.23.0 版](https://www.npmjs.com/package/gm) - 適用於 Node 的 GraphicsMagick 及 ImageMagick。
  - [ibm-cos-sdk 1.1.1 版](https://www.npmjs.com/package/ibm-cos-sdk) - IBM Cloud Object Storage SDK for Node.js
  - [ibm_db 2.2.1 版](https://www.npmjs.com/package/ibm_db) - node.js 至 IBM DB2 和 IBM Informix 的非同步/同步介面。 
  - [lodash 4.17.4 版](https://www.npmjs.com/package/lodash) - 匯出為 Node.js 模組的 Lodash 程式庫。
  - [log4js 2.3.12 版](https://www.npmjs.com/package/log4js) - 設計為使用 Node 的 log4js 架構的轉換。
  - [iconv-lite 0.4.19 版](https://www.npmjs.com/package/iconv-lite) - Pure JS 字元編碼轉換
  - [marked 0.3.7 版](https://www.npmjs.com/package/marked) - 以 JavaScript 撰寫的全功能 Markdown 剖析器和編譯器。專為提供速度而打造。
  - [merge 1.2.0 版](https://www.npmjs.com/package/merge) - 將多個物件合併成一個，選擇性地建立新的複製物件。
  - [moment 2.19.3 版](https://www.npmjs.com/package/moment) - 用於剖析、驗證、操作及格式化日期的輕量型 JavaScript 日期程式庫。
  - [mongodb 2.2.33 版](https://www.npmjs.com/package/mongodb) - 適用於 Node.js 的正式 MongoDB 驅動程式。
  - [mustache 2.3.0 版](https://www.npmjs.com/package/mustache) - Mustache.js 是 JavaScript 中 mustache 範本系統的實作。
  - [nano 6.4.2 版](https://www.npmjs.com/package/nano)- Node.js 的簡約 couchdb 驅動程式。
  - [nodemailer 4.4.1 版](https://www.npmjs.com/package/nodemailer) - 從 Node.js 傳送電子郵件 - 容易使用！
  - [oauth2-server 3.0.0 版](https://www.npmjs.com/package/oauth2-server) - 完整、相容及完整測試的模組，用於實作以 Node.js 表示的 OAuth2 Server/Provider。
  - [openwhisk 3.11.0 版](https://www.npmjs.com/package/openwhisk) - OpenWhisk 平台的 JavaScript 用戶端程式庫。在 OpenWhisk API 附近提供封套。
  - [process 0.11.10 版](https://www.npmjs.com/package/process) - require('process')；就像任何其他模組。
  - [pug 2.0.0-rc.4 版](https://www.npmjs.com/package/pug) - 實作 Pug 範本語言。
  - [redis 2.8.0 版](https://www.npmjs.com/package/redis) - 這是適用於 Node.js 的完整且功能豐富的 Redis 用戶端。
  - [request 2.83.0 版](https://www.npmjs.com/package/request) - 要求是發出 HTTP 呼叫的最簡單方式。
  - [request-promise 4.2.2 版](https://www.npmjs.com/package/request-promise) - 具有 Promise 支援的簡化 HTTP 要求用戶端 'request'。採用 Bluebird 技術。
  - [rimraf 2.6.2 版](https://www.npmjs.com/package/rimraf) - 適用於 Node 的 UNIX 指令 rm -rf。
  - [semver 5.4.1 版](https://www.npmjs.com/package/semver) - 支援語意版本化。
  - [@sendgrid/mail@6.1.4](https://www.npmjs.com/package/@sendgrid/mail) - 透過 SendGrid API 提供電子郵件支援。
  - [serve-favicon 2.4.5 版](https://www.npmjs.com/package/serve-favicon) - 負責處理 Favicon 的 Node.js 中介軟體。
  - [socket.io 2.0.4 版](https://www.npmjs.com/package/socket.io) - Socket.IO 可啟用即時雙向事件型通訊。
  - [socket.io-client 2.0.4 版](https://www.npmjs.com/package/socket.io-client) - Socket.IO 的用戶端支援。
  - [superagent 3.8.2 版](https://www.npmjs.com/package/superagent) - SuperAgent 是小型的漸進式用戶端 HTTP 要求程式庫及具有相同 API 的 Node.js 模組，其中包含許多高階 HTTP 用戶端特性。
  - [swagger-tools 0.10.3 版](https://www.npmjs.com/package/swagger-tools) - 與使用 Swagger 相關的工具，用來記載 API 的一種方式。
  - [tmp 0.0.33 版](https://www.npmjs.com/package/tmp) - node.js 的簡式暫存檔及目錄建立者。
  - [twilio 3.10.1 版](https://www.npmjs.com/package/twilio) - 用於語音、視訊及傳訊的相關 Twilio API 的封套。
  - [underscore 1.8.3 版](https://www.npmjs.com/package/underscore) - Underscore.js 是 JavaScript 的公用程式程式庫，支援一般的功能可疑物件（每個、對映、減少、過濾器...），而不延伸任何核心 JavaScript 物件。
  - [uuid 3.1.0 版](https://www.npmjs.com/package/uuid) - 簡單、快速產生 RFC4122 UUID。
  - [validator 9.2.0 版](https://www.npmjs.com/package/validator) - 字串驗證器和消毒器的程式庫。
  - [watson-developer-cloud 3.0.2 版](https://www.npmjs.com/package/watson-developer-cloud) - Node.js 用戶端程式庫要使用 Watson Developer Cloud 服務，這是一組使用認知運算來解決複雜問題的 API 集合。
  - [when 3.7.8 版](https://www.npmjs.com/package/when) - When.js 是一種穩固的實線、未經測試的 Promisses/A+ 和 when() 實作，包括完整的 ES6 Promise shim。
  - [winston 2.4.0 版](https://www.npmjs.com/package/winston) - node.js 的多重傳輸非同步記載程式庫。"CHILL WINSTON! ... 我把它放在日誌裡。"
  - [ws 3.3.1 版](https://www.npmjs.com/package/ws) - ws 為使用簡單、超快、完整測試的 WebSocket 用戶端及伺服器實作。
  - [xml2js 0.4.19 版](https://www.npmjs.com/package/xml2js) - JavaScript 物件轉換器的簡單 XML。它支援雙向轉換。
  - [xmlhttprequest 1.8.0 版](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest 是內建 HTTP 用戶端用來模擬瀏覽器 XMLHttpRequest 物件的封套。
  - [yauzl 2.9.1 版](https://www.npmjs.com/package/yauzl) - 還有另一個適用於 Node 的解壓縮程式庫。適用於壓縮

### 使用您的動作包裝 npm 套件
對於未預先安裝在 Node.js 環境中的任何 `npm` 套件，您可以在建立或更新動作時將它們組合成相依關係。

如需相關資訊，請參閱[將動作包裝為 Node.js 模組](./openwhisk_actions.html#openwhisk_js_packaged_action)或[以單一組合包裝動作](./openwhisk_actions.html#openwhisk_js_webpack_action)。


## Python 執行時期
{: #openwhisk_ref_python_environments}

OpenWhisk 支援使用兩個不同的執行時期版本來執行「Python 動作」。


### Python 3 動作（Jessie 型）
{: #openwhisk_ref_python_environments_jessie}

「Python 3 動作」是使用 Python 3.6.4 來執行。若要使用此執行時期，請在建立或更新「動作」時指定 `wsk` CLI 參數 `--kind python-jessie:3`。
使用 virtualenv 建立 Python 動作時，請使用 Docker 映像檔 `ibmfunctions/action-python-v3`。
除了 Python 3.6 標準程式庫之外，執行時期包含可供「Python 動作」使用之 IBM Cloud 服務的 SDK 套件。

Python 版本：
- [3.6.4](https://github.com/docker-library/python/blob/a1aa406bfd8c7b129e6e0ee0ba972b863624ac0d/3.6/jessie/Dockerfile)

Python 套件：
- asn1crypto (0.24.0)
- attrs (17.4.0)
- Automat (0.6.0)
- beautifulsoup4 (4.6.0)
- certifi (2017.11.5)
- cffi (1.11.4)
- chardet (3.0.4)
- click (6.7)
- cloudant (2.7.0)
- constantly (15.1.0)
- cryptography (2.1.4)
- cssselect (1.0.3)
- docutils (0.14)
- Flask (0.12.2)
- gevent (1.2.2)
- greenlet (0.4.12)
- httplib2 (0.10.3)
- hyperlink (17.3.1)
- ibm-cos-sdk (2.0.0)
- ibm-db (2.0.8a0)
- idna (2.6)
- incremental (17.5.0)
- itsdangerous (0.24)
- Jinja2 (2.10)
- jmespath (0.9.3)
- kafka-python (1.3.5)
- lxml (4.1.1)
- MarkupSafe (1.0)
- numpy (1.14.0)
- pandas (0.22.0)
- parsel (1.3.1)
- pip (9.0.1)
- pyasn1 (0.4.2)
- pyasn1-modules (0.2.1)
- pycparser (2.18)
- PyDispatcher (2.0.5)
- pyOpenSSL (17.5.0)
- pysolr (3.7.0)
- python-dateutil (2.6.1)
- pytz (2017.3)
- queuelib (1.4.2)
- requests (2.18.4)
- scikit-learn (0.19.1)
- scipy (1.0.0)
- Scrapy (1.5.0)
- service-identity (17.0.0)
- setuptools (38.4.0)
- simplejson (3.13.2)
- six (1.11.0)
- Twisted (17.9.0)
- urllib3 (1.22)
- virtualenv (15.1.0)
- w3lib (1.18.0)
- watson-developer-cloud (1.0.2)
- Werkzeug (0.14.1)
- wheel (0.30.0)
- zope.interface (4.4.3)


### Python 3 動作（Alpine 型）
{: #openwhisk_ref_python_environments_alpine}

「Python 3 動作」是使用 Python 3.6.1 來執行。若要使用此執行時期，請在建立或更新「動作」時指定 `wsk` CLI 參數 `--kind python:3`。
使用 virtualenv 建立 Python 動作時，請使用 Docker 映像檔 `openwhisk/python3action`。
除了 Python 3.6 標準程式庫之外，「Python 動作」還可以使用下列套件。

- asn1crypto (0.23.0)
- attrs (17.3.0)
- Automat (0.6.0)
- beautifulsoup4 (4.5.3)
- cffi (1.11.2)
- click (6.7)
- constantly (15.1.0)
- cryptography (2.1.3)
- cssselect (1.0.1)
- Flask (0.12)
- gevent (1.2.1)
- greenlet (0.4.12)
- httplib2 (0.10.3)
- idna (2.6)
- incremental (17.5.0)
- itsdangerous (0.24)
- Jinja2 (2.9.6)
- kafka-python (1.3.4)
- lxml (3.7.3)
- MarkupSafe (1.0)
- parsel (1.2.0)
- pip (9.0.1)
- pyasn1 (0.3.7)
- pyasn1-modules (0.1.5)
- pycparser (2.18)
- PyDispatcher (2.0.5)
- pyOpenSSL (17.3.0)
- python-dateutil (2.6.0)
- queuelib (1.4.2)
- requests (2.13.0)
- Scrapy (1.3.3)
- service-identity (17.0.0)
- setuptools (36.5.0)
- simplejson (3.10.0)
- six (1.11.0)
- Twisted (17.1.0)
- virtualenv (15.1.0)
- w3lib (1.18.0)
- Werkzeug (0.12.2)
- wheel (0.29.0)
- zope.interface (4.4.3)


### Python 2 動作

「Python 2 動作」是使用 Python 2.7.12 來執行，而 Python 2.7.12 是「Python 動作」的預設執行時期。除非您在建立或更新「動作」時指定 `--kind` 旗標。若要明確地選取此執行時期，請使用 `--kind python:2`。使用 virtualenv 建立 Python 動作時，請使用 Docker 映像檔 `openwhisk/python2action`。
除了 Python 2.7 標準程式庫之外，「Python 2 動作」還可以使用下列套件。

- asn1crypto (0.23.0)
- attrs (17.2.0)
- beautifulsoup4 (4.5.1)
- cffi (1.11.1)
- click (6.7)
- cryptography (2.0.3)
- cssselect (1.0.1)
- enum34 (1.1.6)
- Flask (0.11.1)
- gevent (1.1.2)
- greenlet (0.4.12)
- httplib2 (0.9.2)
- idna (2.6)
- ipaddress (1.0.18)
- itsdangerous (0.24)
- Jinja2 (2.9.6)
- kafka-python (1.3.1)
- lxml (3.6.4)
- MarkupSafe (1.0)
- parsel (1.2.0)
- pip (9.0.1)
- pyasn1 (0.3.7)
- pyasn1-modules (0.1.4)
- pycparser (2.18)
- PyDispatcher (2.0.5)
- pyOpenSSL (17.3.0)
- python-dateutil (2.5.3)
- queuelib (1.4.2)
- requests (2.11.1)
- Scrapy (1.1.2)
- service-identity (17.0.0)
- setuptools (36.5.0)
- simplejson (3.8.2)
- six (1.11.0)
- Twisted (16.4.0)
- virtualenv (15.1.0)
- w3lib (1.18.0)
- Werkzeug (0.12.2)
- wheel (0.29.0)
- zope.interface (4.4.3)

## Swift 動作
{: #openwhisk_ref_swift3}

### Swift 3
「Swift 3 動作」是使用 Swift 3.1.1 `--kind swift:3.1.1` 來執行。由於不支援舊版 Swift，請一律指定 `swift:3.1.1` 類型。

您必須移轉所有「Swift 動作」，以使用 `swift:3.1.1` 類型。最佳作法是在建立或更新「動作」時，一律提供特定類型。
{: tip}

「Swift 3.1.1 動作」可以使用下列套件：
- KituraNet 1.7.6 版：https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON 15.0.1 版：https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK 0.16.0 版：https://github.com/watson-developer-cloud/swift-sdk

## PHP 動作
{: #openwhisk_ref_php}

「PHP 動作」是使用 PHP 7.1 來執行。若要使用此執行時期，請在建立或更新「動作」時指定 `wsk` CLI 參數 `--kind php:7.1`。
當您使用副檔名為 `.php` 的檔案來建立「動作」時，這個行為是預設值。

除了標準副檔名之外，也能使用下列 PHP 副檔名：

- bcmath
- curl
- gd
- intl
- mbstring
- mysqli
- pdo_mysql
- pdo_pgsql
- pdo_sqlite
- soap
- zip

### Composer 套件
也可以使用下列 Composer 套件：

- guzzlehttp/guzzle       v6.3.0
- ramsey/uuid             v3.6.1

## Docker 動作
{: #openwhisk_ref_docker}

「Docker 動作」在 Docker 容器中執行使用者提供的二進位檔。二進位檔是在根據 [python:2.7.12-alpine](https://hub.docker.com/r/library/python) 的 Docker 映像檔中執行，因此二進位檔必須與此發行套件相容。

Docker 架構是建置 OpenWhisk 相容 Docker 映像檔的便利方法。您可以使用 `wsk sdk install docker` CLI 指令來安裝架構。

主要二進位程式必須位在容器的 `/action/exec` 中。執行檔會透過可解除序列化為 `JSON` 物件的單一指令行引數字串來收到輸入引數。它必須透過 `stdout`，以已序列化 `JSON` 的單行字串形式來傳回結果。

您可以修改 `dockerSkeleton` 中包含的 `Dockerfile`，來包含任何編譯步驟或相依關係。

## REST API
{: #openwhisk_ref_restapi}
在[這裡](openwhisk_rest_api.html)可以找到 REST API 的相關資訊

## 系統限制
{: #openwhisk_syslimits}

### 動作
{{site.data.keyword.openwhisk_short}} 有一些系統限制，包括「動作」可以使用的記憶體數量，以及每分鐘容許的「動作」呼叫數。

下表列出「動作」的預設限制。

| 限制 | 說明
| 預設值 | 最小值 | 最大值
| 
| ----- | ----------- | :-------: | :---: | :---: |
| [codeSize](openwhisk_reference.html#openwhisk_syslimits_codesize) | 「動作」碼的大小上限 (MB)。| 48| 1| 48|
| [concurrent](openwhisk_reference.html#openwhisk_syslimits_concurrent) | 每個「名稱空間」的執行中或置入佇列等待執行的活動次數不可提交超過 N 次。| 1000| 1| 1000* |
| [logs](openwhisk_reference.html#openwhisk_syslimits_logs) | 不容許容器寫入 stdout 的內容超過 N MB。| 10| 0 | 10|
| [memory](openwhisk_reference.html#openwhisk_syslimits_memory)	| 不容許容器配置超過 N MB 的記憶體。| 256| 128 | 512 |
| [minuteRate](openwhisk_reference.html#openwhisk_syslimits_minuterate) | 每個「名稱空間」每分鐘的活動次數不可提交超過 N 次。| 5000| 1| 5000* |
| [openulimit](openwhisk_reference.html#openwhisk_syslimits_openulimit) | 「Docker 動作」的開啟檔案數上限。| 64 | 0 | 64 |
| [parameters](openwhisk_reference.html#openwhisk_syslimits_parameters) | 可以附加的參數大小上限 (MB)。| 1| 0 | 1|
| [proculimit](openwhisk_reference.html#openwhisk_syslimits_proculimit) | 「Docker 動作」可用的程序數目上限。| 512 | 0 | 512 |
| [result](openwhisk_reference.html#openwhisk_syslimits_result) | 「動作」呼叫結果的大小上限 (MB)。| 1| 0 | 1|
| [timeout](openwhisk_reference.html#openwhisk_syslimits_timeout)	| 不容許容器執行超過 N 毫秒的時間。| 60000| 100 | 300000 |

### 增加固定限制
{: #increase_fixed_limit}

以 (*) 結尾的限制值是固定的，但如果業務案例可以調整較高的安全限制值，則可以增加值。如果您想要增加限制值，請直接從 IBM [{{site.data.keyword.openwhisk_short}} Web 主控台](https://console.bluemix.net/openwhisk/)來開啟問題單，以聯絡 IBM 支援中心。
  1. 選取**支援**
  2. 從下拉功能表中選取**新增問題單**。
  3. 針對問題單類型，請選取**技術**。
  4. 針對支援的技術領域，請選取**函數**。

#### codeSize (MB)（固定：48 MB）
{: #openwhisk_syslimits_codesize}
* 「動作」的程式碼大小上限為 48 MB。
* 建議「JavaScript 動作」使用工具將所有原始碼（包括相依關係）連結至單一組合檔。
* 此限制是固定的，無法變更。

#### concurrent（固定：1000*）
{: #openwhisk_syslimits_concurrent}
* 「名稱空間」的執行中或置入佇列等待執行的活動次數不能超過 1000。
* 此限制值是固定的，但如果業務案例可以調整較高的安全限制值，則可以增加此值。請查閱[增加固定限制](openwhisk_reference.html#increase_fixed_limit)一節，以取得如何增加此限制的詳細指示。

#### 日誌 (MB)（預設值：10 MB）
{: #openwhisk_syslimits_logs}
* 日誌限制 N 在範圍 [0 MB..10 MB] 內，並根據每個「動作」來設定。
* 建立或更新「動作」時，使用者可以變更「動作」日誌限制。
* 超過設定限制的日誌會被截斷，因此會忽略任何新的日誌項目，且會新增一則警告作為最後一個啟動輸出，指出啟動已超出設定的日誌限制。

#### 記憶體 (MB)（預設值：256 MB）
{: #openwhisk_syslimits_memory}
* 記憶體限制 M 在範圍 [128 MB..512 MB] 內，並根據每個「動作」來設定（以 MB 為單位）。
* 建立「動作」時，使用者可以變更記憶體限制。
* 容器使用的記憶體數量不能超過限制所配置的記憶體數量。

#### minuteRate（固定：5000*）
{: #openwhisk_syslimits_minuterate}
* 比率限制 N 設定為 5000，並限制一分鐘時間範圍內的「動作」呼叫次數。
* 超過此限制的 CLI 或 API 呼叫會收到與 HTTP 狀態碼 `429: TOO MANY REQUESTS` 對應的錯誤碼。
* 此限制值是固定的，但如果業務案例可以調整較高的安全限制值，則可以增加此值。請查閱[增加固定限制](openwhisk_reference.html#increase_fixed_limit)一節，以取得如何增加此限制的詳細指示。

#### openulimit（固定：1024:1024）
{: #openwhisk_syslimits_openulimit}
* 「Docker 動作」的開啟檔案數上限為 1024（同時適用於硬性和軟性限制）。
* 此限制是固定的，無法變更。
* docker run 指令使用 `--ulimit nofile=1024:1024` 引數。
* 如需相關資訊，請參閱 [docker run](https://docs.docker.com/engine/reference/commandline/run) 指令行參考文件。

#### 參數（固定：1 MB）
{: #openwhisk_syslimits_parameters}
* 在建立或更新「動作/套件/觸發程式」時，總參數的大小限制是 1 MB。
* 嘗試建立或更新參數太大的實體時，會拒絕該實體。
* 此限制是固定的，無法變更。

#### proculimit（固定：1024:1024）
{: #openwhisk_syslimits_proculimit}
* 「Docker 動作」容器可用的處理程序數目上限為 1024。
* 此限制是固定的，無法變更。
* docker run 指令使用 `--pids-limit 1024` 引數。
* 如需相關資訊，請參閱 [docker run](https://docs.docker.com/engine/reference/commandline/run) 指令行參考文件。

#### 結果（固定：1 MB）
{: #openwhisk_syslimits_result}
* 「動作」呼叫結果的輸出大小上限 (MB)。
* 此限制是固定的，無法變更。

#### 逾時（毫秒）（預設值：60 秒）
{: #openwhisk_syslimits_timeout}
* 逾時限制 N 在範圍 [100 毫秒..300000 毫秒] 內，並根據每個「動作」來設定（以毫秒為單位）。
* 建立「動作」時，使用者可以變更逾時限制。
* 終止執行時間超過 N 毫秒的容器。

### 觸發程式

觸發程式受限於每分鐘的發動率，如下表所記載。

| 限制 | 說明
| 預設值 | 最小值 | 最大值
| 
| ----- | ----------- | :-------: | :---: | :---: |
| [minuteRate](openwhisk_reference.html#openwhisk_syslimits_tminuterate) | 每個「名稱空間」每分鐘的「觸發程式」發動次數不可超過 N 次。| 5000* | 5000* | 5000* |

### 增加固定限制
{: #increase_fixed_tlimit}

以 (*) 結尾的限制值是固定的，但如果業務案例可以調整較高的安全限制值，則可以增加值。如果您想要增加限制值，請直接從 IBM [{{site.data.keyword.openwhisk_short}} Web 主控台](https://console.bluemix.net/openwhisk/)來開啟問題單，以聯絡 IBM 支援中心。
  1. 選取**支援**
  2. 從下拉功能表中選取**新增問題單**。
  3. 針對問題單類型，請選取**技術**。
  4. 針對支援的技術領域，請選取**函數**。

#### minuteRate（固定：5000*）
{: #openwhisk_syslimits_tminuterate}

* 比率限制 N 設定為 5000，並限制使用者在一分鐘時間範圍內可發動的「觸發程式」數目。
* 建立「觸發程式」時，使用者無法變更「觸發程式」限制。
* 超過此限制的 CLI 或 API 呼叫會收到與 HTTP 狀態碼 `429: TOO MANY REQUESTS` 對應的錯誤碼。
* 此限制值是固定的，但如果業務案例可以調整較高的安全限制值，則可以增加此值。請查閱[增加固定限制](openwhisk_reference.html#increase_fixed_tlimit)一節，以取得如何增加此限制的詳細指示。
