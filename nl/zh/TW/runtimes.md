---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: runtimes, support, functions

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


# 運行環境
{: #runtimes}
可以使用程式設計語言（如 JavaScript 或 Python）對 APP 進行編碼和執行。依預設，有許多運行環境可用於 {{site.data.keyword.openwhisk_short}}。
{: shortdesc}

檢視每個地區中 IBM Cloud Functions 的可用運行環境。下列鏈結會傳回 JSON 回應。回應的 `runtimes` 區段包含一組可用的運行環境。`image` 區段包含 [Docker Hub](https://hub.docker.com/){: external} 上運行環境映像檔的名稱以及使用的標籤。

  - [`us-south`](https://us-south.functions.cloud.ibm.com/){: external}
  - [`us-east`](https://us-east.functions.cloud.ibm.com/){: external}
  - [`eu-gb`](https://eu-gb.functions.cloud.ibm.com/){: external}
  - [`eu-de`](https://eu-de.functions.cloud.ibm.com/){: external}


下列範例指向映像檔 `ibmfunctions/action-nodejs-v10` 和 `openwhisk/nodejs8action`。標籤可以是 `1.9.0` 這類版本號碼或 git commit 雜湊的簡短格式（例如 `b99d71e`）。

映像檔欄位範例。
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs8action:b99d71e"
  ```

運行環境會定期更新。這些更新項目包括安全修正程式及運行環境內套件的次要版本更新項目。次要版本更新項目可能會造成舊版相容性岔斷。運行環境更新項目可能會影響您的動作。您必須透過更新運行環境，將執行該運行環境的動作移轉到更新的版本。

在已淘汰的運行環境上執行的 APP 無法順利完成，直到將相應運行環境更新為支援的運行環境為止。對失敗動作進行疑難排解時，若要識別運行環境是否已被淘汰，請檢查查詢回應中的 `deprecated=true`。若要更新運行環境，請參閱[變更動作運行環境](/docs/openwhisk?topic=cloud-functions-actions#actions_update)

已淘汰的運行環境如下：
<ul>
  <li><code>nodejs:6</code>（已淘汰）</li>
  <li><code>php:7.1</code>（已淘汰）</li>
  <li><code>php:7.2</code>（已淘汰）</li>
  <li><code>swift:3</code>（已淘汰）</li>
  <li><code>swift:3.1.1</code>（已淘汰）</li>
  <li><code>swift:4.1</code>（已淘汰）</li>
  <li><code>ballerina:0.990</code>（已淘汰）</li>
</ul>



## JavaScript 運行環境
{: #openwhisk_ref_javascript_environments}

依預設，所有 Node.js 動作都在第 10 版環境中執行。
{: note}

JavaScript 動作可以在 Node.js 第 8 版或第 10 版中執行。Node.js 第 8 版處於維護模式，且在 2019 年 12 月之前可供使用。請參閱 [Node.js 版本排程](https://github.com/nodejs/Release){: external}。
{: deprecated}

|運行環境|說明|變更日誌|
| --- | --- | --- |
| [10.15.0](https://nodejs.org/en/blog/release/v10.15.0/){: external} |依預設，所有 Node.js 動作都在第 10 版環境中執行。| [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md){: external}。|
| [8.15.0](https://nodejs.org/en/blog/release/v8.15.0/){: external} |如果在建立或更新動作時明確指定 `--kind` 旗標，且值為 `nodejs:8`，則會使用 Node.js 8.15.0 版。| [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md){: external}。|


### 從 nodeJS 8 移轉至 nodeJS 10

|套件|詳細資料|
| --- | --- |
| `ibm_db` |`nodejs:10` 中無法使用 `ibm_db` NPM 套件。`ibm_db` 套件不支援 Node.js 10。您可以在[此問題](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541){: external}中追蹤進度。|
|`cloudant` |`nodejs:10` 中無法使用 `cloudant` NPM 套件。套件已淘汰。在匯入 Node.js 模組時，需要使用正式 NPM 套件 [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant){: external} 3.0.0 版（即 `require('@cloudant/cloudant')`），此外 [ 3.x 版僅傳回 Promise](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x){: external}。|
| `cradle` |`nodejs:10` 中無法使用 `cradle` NPM 套件。|
| `log4js` |`nodejs:10` 中無法使用 `log4js` NPM 套件。您可以追蹤[此問題](https://github.com/log4js-node/log4js-node/issues/805){: external}。|
| `watson-developer-cloud` |`nodejs:10` 中無法使用 `watson-developer-cloud` NPM 套件。您可以追蹤[此問題](https://github.com/watson-developer-cloud/node-sdk/issues/780){: external}在新版本上的進度。|


### Node.js 套件

|Node.js 10.15 套件|說明|
|:-----------------|:-----------------|
| [`amqplib`](https://www.npmjs.com/package/amqplib){: external} |用來製作 Node.JS 之進階傳訊佇列作業通訊協定 0-9-1 用戶端的程式庫。|
| [`apn`](https://www.npmjs.com/package/apn){: external} |用來與 Apple Push Notification Service 互動的 Node.js 模組。|
| [`async`](https://www.npmjs.com/package/async){: external} |提供函數以便使用非同步函數。|
| [`bent`](https://www.npmjs.com/package/bent){: external} |適用於 async 和 await 的 Node.js 的功能性 HTTP 用戶端。|
| [`bodyparser`](https://www.npmjs.com/package/body-parser){: external} |在處理程式之前的中介軟體中剖析送入的要求內文，提供於 req.body 內容下。|
| [`btoa`](https://www.npmjs.com/package/btoa){: external} |瀏覽器 `btoa` 函數的埠。|
| [`cassandra-driver`](https://www.npmjs.com/package/cassandra-driver){: external} |Apache Cassandra 的 DataStax Node.js 驅動程式。|
| [`@cloudant/cloudant`](https://www.npmjs.com/package/@cloudant/cloudant){: external} |Node.js 的正式 Cloudant 程式庫。|
| [`commander`](https://www.npmjs.com/package/commander){: external} |Node.js 指令行介面的完整解決方案。|
| [`composeaddresstranslator`](https://www.npmjs.com/package/composeaddresstranslator){: external} |Scylla 資料庫之 Compose 使用者介面或 API 的位址轉換器。|
| [`consul`](https://www.npmjs.com/package/consul){: external} |Consul 的用戶端，與服務探索及配置相關。|
| [`cookie-parser`](https://www.npmjs.com/package/cookie-parser){: external} |剖析 Cookie 標頭並將以 Cookie 名稱為索引鍵的物件移入 req.cookies。|
| [`elasticsearch`](https://www.npmjs.com/package/elasticsearch){: external} |Node.js 的正式低階 ElasticSearch 用戶端。|
| [`errorhandler`](https://www.npmjs.com/package/errorhandler){: external} |僅限開發的錯誤處理常式中介軟體。|
| [`etcd3`](https://www.npmjs.com/package/etcd3){: external} |針對通訊協定緩衝區型 etcdv3 API 的高品質、可供正式作業使用的用戶端。|
| [`formidable`](https://www.npmjs.com/package/formidable){: external} |用來剖析表單資料的 Node.js 模組，尤其是針對檔案上傳。|
| [`glob`](https://www.npmjs.com/package/glob){: external} |使用 Shell 使用的型樣來比對檔案，例如 stars 和 stuff。|
| [`gm`](https://www.npmjs.com/package/gm){: external} |適用於 Node 的 GraphicsMagick 及 ImageMagick。|
| [`ibm-cos-sdk`](https://www.npmjs.com/package/ibm-cos-sdk){: external} | {{site.data.keyword.cos_full}} SDK for Node.js |
| [`ibm_db`](https://www.npmjs.com/package/ibm_db){: external} |Node.js 對 IBM Db2 及 IBM Informix 的非同步/同步介面。|
| [`ibmiotf`](https://www.npmjs.com/package/ibmiotf){: external} |用於簡化與 IBM Watson Internet of Things 平台的互動的 Node.js 用戶端。|
| [`iconv-lite`](https://www.npmjs.com/package/iconv-lite){: external} |純正 JS 字元編碼轉換。|
| [`jsdom`](https://www.npmjs.com/package/jsdom){: external} | `jsdom` 是許多 Web 標準的純正 JavaScript 實作，特別值得一提的是 Web Hypertext Application Technology Working Group DOM 和 HTML 標準。|
| [`jsforce`](https://www.npmjs.com/package/jsforce){: external} |JavaScript 應用程式的 Salesforce API 程式庫。|
| [`jsonwebtoken`](https://www.npmjs.com/package/jsonwebtoken){: external} |JSON Web 記號的實作。|
| [`lodash`](https://www.npmjs.com/package/lodash){: external} |匯出為 Node.js 模組的 `lodash` 程式庫。|
| [`marked`](https://www.npmjs.com/package/marked){: external} |全功能 Markdown 剖析器及編譯器，以 JavaScript 撰寫。為追求速度而建置。|
| [`merge`](https://www.npmjs.com/package/merge){: external} |將多個物件合併為一個，選擇性地建立新的複製物件。
| [`moment`](https://www.npmjs.com/package/moment){: external} |輕量型 JavaScript 日期程式庫，用於剖析、驗證、操作及格式化日期。|
| [`mongodb`](https://www.npmjs.com/package/mongodb){: external} |Node.js 的正式 MongoDB 驅動程式。|
| [`mysql`](https://www.npmjs.com/package/mysql){: external} |MySQL 的 Node.js 驅動程式。|
| [`mustache`](https://www.npmjs.com/package/mustache){: external} |Mustache.js 是 mustache 範本系統的實作，以 JavaScript 撰寫。|
| [`nano`](https://www.npmjs.com/package/nano){: external} |Node.js 的 Minimalistic CouchDB 驅動程式。|
| [`nodemailer`](https://www.npmjs.com/package/nodemailer){: external} |從 Node.js 傳送電子郵件 - 超簡單！|
| [`oauth2-server`](https://www.npmjs.com/package/oauth2-server){: external} |完整、符合規範且經過測試的模組，用來快速以 Node.js 實作 OAuth2 伺服器/提供者。|
| [`openwhisk`](https://www.npmjs.com/package/openwhisk){: external} |OpenWhisk 平台的 JavaScript 用戶端程式庫。提供 OpenWhisk API 的封套。|
| [`path-to-regex`](https://www.npmjs.com/package/path-to-regexp){: external} |將路徑字串（例如 `/user/:name`）轉換成正規表示式，然後便可以用來比對 URL 路徑。|
| [`pg`](https://www.npmjs.com/package/pg){: external} |適用於 Node.js 的非阻擋式 PostgreSQL 用戶端。純正的 JavaScript 及選用性原生 `libpq` 連結。|
| [`process`](https://www.npmjs.com/package/process){: external} | `require('process')`；就像任何其他模組。|
| [`pug`](https://www.npmjs.com/package/pug){: external} |實作 Pug 範本語言。|
| [`redis`](https://www.npmjs.com/package/redis){: external} |完整且特性豐富的 Node.js Redis 用戶端。|
| [`request`](https://www.npmjs.com/package/request){: external} |進行 HTTP 呼叫。|
| [`request-promise`](https://www.npmjs.com/package/request-promise){: external} |簡化的 HTTP 要求用戶端 'request'，含 Promise 支援。採用 Bluebird 技術。|
| [`rimraf`](https://www.npmjs.com/package/rimraf){: external} |適用於 node 的 UNIX 指令 rm -rf。|
| [`semver`](https://www.npmjs.com/package/semver){: external} |適用於 nodeJS 的語意版本化。|
| [`@sendgrid/mail`](https://www.npmjs.com/package/@sendgrid/mail){: external} |透過 SendGrid API 提供電子郵件支援。|
| [`serialize-error`](https://www.npmjs.com/package/serialize-error){: external} |將錯誤序列化成為一般物件。|
| [`serve-favicon`](https://www.npmjs.com/package/serve-favicon){: external} |Node.js 中介軟體，用來負責處理 favicon。
| [`socket.io`](https://www.npmjs.com/package/socket.io){: external} | `socket.io` 能促成即時、雙向、以事件為基礎的通訊。|
| [`socket.io-client`](https://www.npmjs.com/package/socket.io-client){: external} |`socket.io` 的即時應用程式架構。|
| [`superagent`](https://www.npmjs.com/package/superagent){: external} | `superagent` 是小型的先進用戶端 HTTP 要求程式庫，以及具有相同 API 的 Node.js 模組，支援許多高階 HTTP 用戶端特性。|
| [`swagger-tools`](https://www.npmjs.com/package/swagger-tools){: external} |提供各種工具的套件，用來整合 Swagger 及與其互動。|
| [`twilio`](https://www.npmjs.com/package/twilio){: external} |Twilio API 的封套，和語音、視訊及傳訊相關。|
| [`underscore`](https://www.npmjs.com/package/underscore){: external} |Underscore.js 是 JavaScript 的 utility-belt 程式庫，可支援常見功能可疑物件（each、map、reduce、filter...）而不延伸任何核心 JavaScript 物件。|
| [`url-pattern`](https://www.npmjs.com/package/url-pattern){: external} |剖析 URL 中的路徑參數，比起使用正規表示式字串比對程式更輕鬆。|
| [`uuid`](https://www.npmjs.com/package/uuid){: external} |簡單、快速地產生 RFC4122 UUID。|
| [`validator`](https://www.npmjs.com/package/validator){: external} |字串驗證器及消毒器的程式庫。|
| [`vcap_services`](https://www.npmjs.com/package/vcap_services){: external} |剖析並傳回 VCAP_SERVICES 環境變數（由 IBM Cloud 提供）中的服務認證。|
| [`when`](https://www.npmjs.com/package/when){: external} |When.js 是堅固、經過各式測試的 `Promises/A+` 及 `when()` 實作，包括完整的 ES6 Promise shim。|
| [`winston`](https://www.npmjs.com/package/winston){: external} |適用於 Node.js 的多傳輸非同步記載程式庫。"CHILL WINSTON! ... I put it in the logs."|
| [`ws`](https://www.npmjs.com/package/ws){: external} | `ws` 是簡單易用、超快速且經過徹底測試的 WebSocket 用戶端與用戶端伺服器實作。|
| [`xlsx`](https://www.npmjs.com/package/xlsx){: external} |各種試算表格式的剖析器及撰寫器。|
| [`xml2js`](https://www.npmjs.com/package/xml2js){: external} |簡單的 XML 對 JavaScript 物件轉換器。它支援雙向轉換。|
| [`xmlhttprequest`](https://www.npmjs.com/package/xmlhttprequest){: external} |node-XMLHttpRequest 是內建 http 用戶端的封套，可以模擬瀏覽器 XMLHttpRequest 物件。|
| [`yauzl`](https://www.npmjs.com/package/yauzl){: external} |適用於 Node 的另一個解壓縮程式庫。|
{: caption="表 1. Node.js 10.15 套件。" caption-side="top"}
{: #javascript-1}
{: tab-title="Node.js 10.15 packages"}
{: tab-group="node"}
{: class="simple-tab-table"}

|Node.js 8.15 套件|說明|
|:-----------------|:-----------------|
| [`amqplib`](https://www.npmjs.com/package/amqplib){: external} |用來製作 Node.JS 之進階傳訊佇列作業通訊協定 0-9-1 用戶端的程式庫。|
| [`apn`](https://www.npmjs.com/package/apn){: external} |用來與 Apple Push Notification Service 互動的 Node.js 模組。| [`async`](https://www.npmjs.com/package/async){: external} |提供函數以便使用非同步函數。|
| [`bent`](https://www.npmjs.com/package/bent){: external} |具有 async-await 且適用於 Node.js 的功能性 HTTP 用戶端。|
| [`bodyparser`](https://www.npmjs.com/package/body-parser){: external} |在處理程式之前的中介軟體中剖析送入的要求內文，提供於 req.body 內容下。|
| [`btoa`](https://www.npmjs.com/package/btoa){: external} |瀏覽器 `btoa` 函數的埠。|
| [`cassandra-driver`](https://www.npmjs.com/package/cassandra-driver){: external} |Apache Cassandra 的 DataStax Node.js 驅動程式。|
|[`cloudant`](https://www.npmjs.com/package/cloudant){: external} |Node.js 的正式 Cloudant 程式庫。|
| [`@cloudant/cloudant`](https://www.npmjs.com/package/cloudant){: external} |Node.js 的正式 Cloudant 程式庫。|
| [`commander`](https://www.npmjs.com/package/commander){: external} |Node.js 指令行介面的完整解決方案。|
| [`composeaddresstranslator`](https://www.npmjs.com/package/composeaddresstranslator){: external} |Scylla 資料庫之 Compose 使用者介面或 API 的位址轉換器。|
| [`consul`](https://www.npmjs.com/package/consul){: external} |Consul 的用戶端，與服務探索及配置相關。|
| [`cookie-parser`](https://www.npmjs.com/package/cookie-parser){: external} |剖析 Cookie 標頭並將以 Cookie 名稱為索引鍵的物件移入 req.cookies。|
| [`elasticsearch`](https://www.npmjs.com/package/elasticsearch){: external} |Node.js 的正式低階 ElasticSearch 用戶端。|
| [`errorhandler`](https://www.npmjs.com/package/errorhandler){: external} |僅限開發的錯誤處理常式中介軟體。|
| [`etcd3`](https://www.npmjs.com/package/etcd3){: external} |針對通訊協定緩衝區型 etcdv3 API 的高品質、可供正式作業使用的用戶端。|
| [`formidable`](https://www.npmjs.com/package/formidable){: external} |用來剖析表單資料的 Node.js 模組，尤其是針對檔案上傳。|
| [`glob`](https://www.npmjs.com/package/glob){: external} |使用 Shell 使用的型樣來比對檔案，例如 stars 和 stuff。|
| [`gm`](https://www.npmjs.com/package/gm){: external} |適用於 Node 的 GraphicsMagick 及 ImageMagick。|
| [`ibm-cos-sdk`](https://www.npmjs.com/package/ibm-cos-sdk){: external} | {{site.data.keyword.cos_full}} SDK for Node.js。|
| [`ibm_db`](https://www.npmjs.com/package/ibm_db){: external} |Node.js 對 IBM Db2 及 IBM Informix 的非同步/同步介面。|
| [`ibmiotf`](https://www.npmjs.com/package/ibmiotf){: external} |Node.js 用戶端用於簡化與 IBM Watson Internet of Things Platform 的互動。|
| [`iconv-lite`](https://www.npmjs.com/package/iconv-lite){: external} |純正 JS 字元編碼轉換。|
| [`jsdom`](https://www.npmjs.com/package/jsdom){: external} | `jsdom` 是許多 Web 標準的純正 JavaScript 實作，特別值得一提的是 Web Hypertext Application Technology Working Group DOM 和 HTML 標準。|
| [`jsforce`](https://www.npmjs.com/package/jsforce){: external} |JavaScript 應用程式的 Salesforce API 程式庫。|
| [`jsonwebtoken`](https://www.npmjs.com/package/jsonwebtoken){: external} |JSON Web 記號的實作。|
| [`lodash`](https://www.npmjs.com/package/lodash){: external} |匯出為 Node.js 模組的 `lodash` 程式庫。|
| [`log4js`](https://www.npmjs.com/package/log4js){: external} |用於轉換 log4js 架構以使用 Node。|
| [`marked`](https://www.npmjs.com/package/marked){: external} |全功能 Markdown 剖析器及編譯器，以 JavaScript 撰寫。為追求速度而建置。|
| [`merge`](https://www.npmjs.com/package/merge){: external} |將多個物件合併為一個，選擇性地建立新的複製物件。|
| [`moment`](https://www.npmjs.com/package/moment){: external} |輕量型 JavaScript 日期程式庫，用於剖析、驗證、操作及格式化日期。|
| [`mongodb`](https://www.npmjs.com/package/mongodb){: external} |Node.js 的正式 MongoDB 驅動程式。|
| [`mysql`](https://www.npmjs.com/package/mysql){: external} |MySQL 的 Node.js 驅動程式。|
| [`mustache`](https://www.npmjs.com/package/mustache){: external} |mustache.js 是 mustache 範本系統的實作，以 JavaScript 撰寫。|
| [`nano`](https://www.npmjs.com/package/nano){: external} |Node.js 的 Minimalistic CouchDB 驅動程式。|
| [`nodemailer`](https://www.npmjs.com/package/nodemailer){: external} |從 Node.js 傳送電子郵件 - 超簡單！|
| [`oauth2-server`](https://www.npmjs.com/package/oauth2-server){: external} |完整、符合規範且經過測試的模組，用來快速以 Node.js 實作 OAuth2 伺服器/提供者。|
| [`openwhisk`](https://www.npmjs.com/package/openwhisk){: external} |OpenWhisk 平台的 JavaScript 用戶端程式庫。提供 OpenWhisk API 的封套。|
| [`path-to-regex`](https://www.npmjs.com/package/path-to-regexp){: external} |將路徑字串（例如 `/user/:name`）轉換成正規表示式，然後便可以用來比對 URL 路徑。
| [`pg`](https://www.npmjs.com/package/pg){: external} |適用於 Node.js 的非阻擋式 PostgreSQL 用戶端。純正的 JavaScript 及選用性原生 `libpq` 連結。|
| [`process`](https://www.npmjs.com/package/process){: external} | `require('process')`；就像任何其他模組。|
| [`pug`](https://www.npmjs.com/package/pug){: external} |實作 Pug 範本語言。|
| [`redis`](https://www.npmjs.com/package/redis){: external} |完整且特性豐富的 Node.js Redis 用戶端。|
| [`request`](https://www.npmjs.com/package/request){: external} |進行 HTTP 呼叫。|
| [`request-promise`](https://www.npmjs.com/package/request-promise){: external} |簡化的 HTTP 要求用戶端 'request'，含 Promise 支援。採用 Bluebird 技術。|
| [`rimraf`](https://www.npmjs.com/package/rimraf){: external} |適用於 node 的 UNIX 指令 rm -rf。|
| [`semver`](https://www.npmjs.com/package/semver){: external} |適用於 nodeJS 的語意版本化。|
| [`@sendgrid/mail`](https://www.npmjs.com/package/@sendgrid/mail){: external} |透過 SendGrid API 提供電子郵件支援。|
| [`serialize-error`](https://www.npmjs.com/package/serialize-error){: external} |將錯誤序列化成為一般物件。|
| [`serve-favicon`](https://www.npmjs.com/package/serve-favicon){: external} |Node.js 中介軟體，用來負責處理 favicon。
|
| [`socket.io`](https://www.npmjs.com/package/socket.io){: external} | `socket.io` 能促成即時、雙向、以事件為基礎的通訊。|
| [`socket.io-client`](https://www.npmjs.com/package/socket.io-client){: external} |`socket.io` 的即時應用程式架構。|
| [`superagent`](https://www.npmjs.com/package/superagent){: external} | `superagent` 是小型的先進用戶端 HTTP 要求程式庫，以及具有相同 API 的 Node.js 模組，支援許多高階 HTTP 用戶端特性。|
| [`swagger-tools`](https://www.npmjs.com/package/swagger-tools){: external} |提供各種工具的套件，用來整合 Swagger 及與其互動。|
| [`twilio`](https://www.npmjs.com/package/twilio){: external} |Twilio API 的封套，和語音、視訊及傳訊相關。|
| [`underscore`](https://www.npmjs.com/package/underscore){: external} |Underscore.js 是 JavaScript 的 utility-belt 程式庫，可支援常見功能可疑物件（each、map、reduce、filter...）而不延伸任何核心 JavaScript 物件。|
| [`url-pattern`](https://www.npmjs.com/package/url-pattern){: external} |剖析 URL 中的路徑參數，比起使用正規表示式字串比對程式更輕鬆。|
| [`uuid`](https://www.npmjs.com/package/uuid){: external} |簡單、快速地產生 RFC4122 UUID。|
| [`validator`](https://www.npmjs.com/package/validator){: external} |字串驗證器及消毒器的程式庫。|
| [`vcap_services`](https://www.npmjs.com/package/vcap_services){: external} |剖析並傳回 VCAP_SERVICES 環境變數（由 IBM Cloud 提供）中的服務認證。|
| [`watson-developer-cloud`](https://www.npmjs.com/package/watson-developer-cloud){: external} |使用 Watson Developer Cloud 服務的 Node.js 用戶端程式庫，這是一組使用認知運算來解決複雜問題的 API。|
| [`when`](https://www.npmjs.com/package/when){: external} |When.js 是堅固、經過各式測試的 `Promises/A+` 及 `when()` 實作，包括完整的 ES6 Promise shim。|
| [`winston`](https://www.npmjs.com/package/winston){: external} |適用於 Node.js 的多傳輸非同步記載程式庫。"CHILL WINSTON! ... I put it in the logs."|
| [`ws`](https://www.npmjs.com/package/ws){: external} | `ws` 是簡單易用、超快速且經過徹底測試的 WebSocket 用戶端與用戶端伺服器實作。|
| [`xml2js`](https://www.npmjs.com/package/xml2js){: external} |簡單的 XML 對 JavaScript 物件轉換器。它支援雙向轉換。|
| [`xmlhttprequest`](https://www.npmjs.com/package/xmlhttprequest){: external} | `node-XMLHttpRequest` 是內建 http 用戶端的封套，可以模擬瀏覽器 XMLHttpRequest 物件。|
| [`yauzl`](https://www.npmjs.com/package/yauzl){: external} |適用於 Node 的另一個解壓縮程式庫。|
{: caption="表 2. Node.js 8.15 套件。" caption-side="top"}
{: #javascript-2}
{: tab-title="Node.js 8.15 packages"}
{: tab-group="node"}
{: class="simple-tab-table"}


## Python 運行環境
{: #openwhisk_ref_python_environments}

依預設，所有 Python 動作都在 2.7.15 版環境中執行。
{: note}

|Python 版本|說明|變更日誌|
| --- | --- | --- |
|2.7.15|依預設，所有 Python 動作都在 2.7.15 版環境中執行，除非在建立或更新動作時指定 `--kind` 旗標。使用 `virtualenv` 建立 Python 動作時，請使用 Docker 映像檔 `openwhisk/python2action`。除了 Python 2.7 標準程式庫之外，Python 2 動作還可以使用下列套件。| [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md){: external}。|
|[3.6.8](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile){: external}|Python 3 動作是使用 Python 3.6.x 來執行。若要使用此運行環境，請在建立或更新動作時，指定 CLI 參數 `--kind python:3.6`。除了 Python 3.6 標準程式庫之外，運行環境還包含可供 Python 動作使用之 IBM Cloud 服務的 SDK 套件。| [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md){: external}。|
|[3.7.2](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile){: external}|Python 3.7 動作（基於 Debian Stretch）使用 Python 3.7.x 執行。若要使用此運行環境，請在建立或更新動作時，指定 CLI 參數 `--kind python:3.7`。除了 Python 3.7 標準程式庫之外，運行環境還包含可供 Python 動作使用之 IBM Cloud 服務的 SDK 套件。| [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md){: external}。|


### Python 套件

|Python 2.7.15 套件| 
|:-----------------|
| `asn1crypto` |
| `attrs` |
| `Automat` |
| `beautifulsoup4` |
| `certifi` |
| `cffi` |
| `chardet` |
| `Click` |
| `constantly` |
| `cryptography` |
| `cssselect` |
| `enum34` |
| `Flask` |
| `functools32` |
| `gevent` |
| `greenlet` |
| `httplib2` |
| `hyperlink` |
| `idna` |
| `incremental` |
| `ipaddress` |
| `itsdangerous` |
| `Jinja2` |
| `kafka-python` |
| `lxml` |
| `MarkupSafe` |
| `parsel` |
| `pyasn1` |
| `pyasn1-modules` |
| `pycparser` |
| `PyDispatcher` |
| `PyHamcrest` |
| `pyOpenSSL` |
| `python-dateutil` |
| `queuelib` |
| `requests` |
| `Scrapy` |
| `service-identity` |
| `simplejson` |
| `six` |
| `Twisted` |
| `urllib3` |
| `virtualenv=` |
| `w3lib` |
| `Werkzeug` |
| `zope.interface` |
{: caption="表 1. Python 2.7.15 套件。" caption-side="top"}
{: #python-1}
{: tab-title="Python 2.7.15 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}

|Python 3.6.8 套件| 
|:-----------------|
| `asn1crypto` |
| `attrs` |
| `autobahn` |
| `Automat` |
| `beautifulsoup4` |
| `botocore` |
| `cassandra-driver` |
| `certifi` |
| `cffi` |
| `chardet` |
| `Click` |
|`cloudant` |
| `constantly` |
| `cryptography` |
| `cssselect` |
| `docutils` |
| `elasticsearch` |
| `Flask` |
| `gevent` |
| `greenlet` |
| `httplib2` |
| `hyperlink` |
| `ibm-cos-sdk` |
| `ibm-cos-sdk-core` |
| `ibm-cos-sdk-s3transfer` |
| `ibm-db` |
| `ibmcloudsql` |
| `idna` |
| `incremental` |
| `itsdangerous` |
| `Jinja2` |
| `jmespath` |
| `kafka-python` |
| `lxml` |
| `MarkupSafe` |
| `numpy` |
| `pandas` |
| `parsel` |
| `pika` |
| `Pillow` |
| `psycopg2` |
| `pyarrow` |
| `pyasn1` |
| `pyasn1-modules` |
| `pycparser` |
| `PyDispatcher` |
| `PyHamcrest` |
| `pymongo` |
| `pyOpenSSL` |
| `python-dateutil` |
| `pytz` |
| `queuelib` |
| `redis` |
| `requests` |
| `scikit-learn` |
| `scipy` |
| `Scrapy` |
| `service-identity` |
| `simplejson` |
| `six=` |
| `soupsieve` |
| `tornado` |
| `Twisted` |
| `txaio` |
| `urllib3` |
| `virtualenv` |
| `w3lib` |
| `watson-developer-cloud` |
| `Werkzeug` |
| `zope.interface` |
{: caption="表 2. Python 3.6.8 套件。" caption-side="top"}
{: #python-2}
{: tab-title="Python 3.6.8 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}

|Python 3.7.2 套件| 
|:-----------------|
| `asn1crypto` |
| `attrs` |
| `Automat` |
| `beautifulsoup4` |
| `botocore` |
| `cassandra-driver` |
| `certifi` |
| `cffi` |
| `chardet` |
| `Click` |
|`cloudant` |
| `constantly` |
| `cryptography` |
| `cssselect` |
| `docutils` |
| `elasticsearch` |
| `etcd3` |
| `Flask` |
| `gevent` |
| `greenlet` |
| `grpcio` |
| `httplib2` |
| `hyperlink` |
| `ibm-cos-sdk` |
| `ibm-cos-sdk-core` |
| `ibm-cos-sdk-s3transfer` |
| `ibm-db` |
| `ibmcloudsql` |
| `idna` |
| `incremental` |
| `itsdangerous` |
| `Jinja2` |
| `jmespath` |
| `kafka-python` |
| `lxml` |
| `MarkupSafe` |
| `numpy` |
| `pandas` |
| `parsel` |
| `pika` |
| `Pillow` |
| `protobuf` |
| `psycopg2` |
| `pyarrow` |
| `pyasn1` |
| `pyasn1-modules` |
| `pycparser` |
| `PyDispatcher` |
| `PyHamcrest` |
| `pymongo` |
| `pyOpenSSL` |
| `python-dateutil` |
| `pytz` |
| `queuelib` |
| `redis` |
| `requests` |
| `scikit-learn` |
| `scipy` |
| `Scrapy` |
| `service-identity` |
| `simplejson` |
| `six` |
| `soupsieve` |
| `tenacity` |
| `tornado` |
| `Twisted` |
| `urllib3` |
| `virtualenv` |
| `w3lib` |
| `watson-developer-cloud` |
| `websocket-client` |
| `Werkzeug` |
| `zope.interface` |
{: caption="表 3. Python 3.7.2 套件。" caption-side="top"}
{: #python-3}
{: tab-title="Python 3.7.2 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}


## Swift 運行環境
{: #swift-actions}

依預設，所有 Swift 動作都在 4.2 版環境中執行。
{: note}

Swift 4.x 動作運行環境未內嵌任何套件，請遵循[包裝 Swift 動作](/docs/openwhisk?topic=cloud-functions-prep#prep_swift42_single)的指示以使用 Package.swift 來包含相依關係。

在使用單一 Swift 原始檔時，Swift 4.2 動作可以使用下列套件：
- Watson Developer Cloud SDK 1.2.0 版：https://github.com/watson-developer-cloud/swift-sdk


### 使用單一來源動作檔案的 <ph class="ignoreSpelling">SwiftyJSON</ph>
如果您具有未編譯的動作，並且使用的是 `SwftyJSON` 套件，則您需要預先編譯動作，並指定要用於 `swift:4.2` kind 動作的 `SwiftyJSON` 版本。


## PHP 運行環境
{: #openwhisk_ref_php}

依預設，所有 PHP 動作都在 7.3 版環境中執行。
{: note}

除了標準副檔名之外，也能使用下列 PHP 副檔名：

- `bcmath`
- `curl`
- `gd`
- `intl`
- `mbstring`
- `mysqli`
- `pdo_mysql`
- `pdo_pgsql`
- `pdo_sqlite`
- `soap`
- `zip`

## Docker 運行環境
{: #openwhisk_ref_docker}

Docker 動作在 Docker 容器中執行使用者提供的執行檔。該執行檔是在根據 [python:3.6-alpine](https://hub.docker.com/_/python){: external} 的 Docker 映像檔中執行，因此該執行檔必須與此發行套件相容。

Docker 架構是建置 OpenWhisk 相容 Docker 映像檔的便利方法。您可以使用 `ibmcloud fn sdk install docker` CLI 外掛程式指令來安裝架構。

主要的可執行程式必須位於容器內的 `/action/exec` 中。執行檔會從可解除序列化為 `JSON` 物件的單一指令行引數字串接收到輸入引數。它必須使用 `stdout`，以已序列化 `JSON` 的單行字串形式來傳回結果。

您可以修改 `dockerSkeleton` 中包含的 `Dockerfile`，來包含任何編譯步驟或相依關係。

## 更多運行環境支援

|運行環境|
| --- | --- | 
|依預設，所有 Go 動作都在 1.11 版環境中執行。|
{: caption="表 1. Go。" caption-side="top"}
{: #runtimes-1}
{: tab-title="Go"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

|運行環境|
| --- | --- | 
|依預設，所有 Java 動作都在第 8 版環境中執行。|
{: caption="表 2. Java。" caption-side="top"}
{: #runtimes-2}
{: tab-title="Java"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

|運行環境|
| --- |
|依預設，所有 Ruby 動作都在 2.5 版環境中執行。|
{: caption="表 3. Ruby。" caption-side="top"}
{: #runtimes-3}
{: tab-title="Ruby"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

|運行環境|
| --- |
|依預設，所有 .NET Core 動作都在 2.2 版環境中執行。|
{: caption="表 4. .NET Core。" caption-side="top"}
{: #runtimes-4}
{: tab-title=".NET"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}




