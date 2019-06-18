---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-20"

keywords: runtimes, support

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

# 運行環境
{: #runtimes}
可以使用程式設計語言（如 JavaScript 或 Python）對 APP 進行編碼和執行。依預設，有許多運行環境可用於 {{site.data.keyword.openwhisk_short}}。
{: shortdesc}

檢視每個地區中 IBM Cloud Functions 的可用運行環境。下列鏈結會傳回 JSON 回應。

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [eu-de](https://eu-de.functions.cloud.ibm.com/)

回應的 `runtimes` 區段包含一組可用的運行環境。

`image` 區段包含 [Docker Hub](https://hub.docker.com/) 上運行環境映像檔的名稱以及使用的標籤。


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

JavaScript 動作可以在 Node.js 第 8 版或第 10 版中執行。依預設，所有 Node.js 動作都在 V10 環境中執行。



### 具有 IBM SDK 的 Node.js 第 10 版環境
{: #openwhisk_ref_javascript_environments_10}
如果在建立或更新動作時明確指定 `--kind` 旗標，且值為 `nodejs:10`，則會使用 Node.js 第 10 版環境。

#### 從 `nodejs:8` 移轉至 `nodejs:10`
- `nodejs:10` 中無法使用 `ibm_db` npm 套件。`ibm_db` 套件不支援 Node.js 10。您可以追蹤此問題 [ibmdb/node-ibm_db/issues/482](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541) 的進度。
- `nodejs:10` 中無法使用 `cloudant` npm 套件，此套件已被淘汰，您需要在匯入 nodejs 模組（即 `require('@cloudant/cloudant')`）時使用正式 npm 套件 [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) 3.0.0 版，而且 [3.x 版只會傳回 Promises](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x)。
- `nodejs:10` 中無法使用 `cradle` npm 套件。
- `nodejs:10` 中無法使用 `log4js` npm 套件。您可以在 [log4js-node/issues/805](https://github.com/log4js-node/log4js-node/issues/805) 追蹤此問題
- `nodejs:10` 中無法使用 `watson-developer-cloud` npm 套件。您可以追蹤此問題 [watson-developer-cloud/node-sdk/issues/780](https://github.com/watson-developer-cloud/node-sdk/issues/780) 中新版本的進度

nodejs 第 10 版運行環境的詳細資訊可以在 [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md) 中找到。

#### Node.js 10.15 套件

   - [10.15.0](https://nodejs.org/en/blog/release/v10.15.0/)

 NPM 套件：
   - [amqplib](https://www.npmjs.com/package/amqplib) - 用來製作 Node.JS 之 AMQP 0-9-1 用戶端的程式庫。
   - [apn](https://www.npmjs.com/package/apn) - 用來與 Apple Push Notification Service 互動的 Node.js 模組。
   - [async](https://www.npmjs.com/package/async) - 提供函數以便使用非同步函數。
   - [bent](https://www.npmjs.com/package/bent) - 適用於 async 和 await 的 Node.js 的功能性 HTTP 用戶端。
   - [bodyparser](https://www.npmjs.com/package/body-parser) - 在處理程式之前的中介軟體中剖析送入的要求內文，提供於 req.body 內容下。
   - [btoa](https://www.npmjs.com/package/btoa) - 瀏覽器 btoa 函數的埠。
   - [cassandra-driver](https://www.npmjs.com/package/cassandra-driver) - Apache Cassandra 的 DataStax Node.js 驅動程式。
   - [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) - 這是 Node.js 的正式 Cloudant 程式庫。
   - [commander](https://www.npmjs.com/package/commander) - node.js 指令行介面的完整解決方案。
   - [composeaddresstranslator](https://www.npmjs.com/package/composeaddresstranslator) - Scylla 資料庫之 Compose 使用者介面或 API 的位址轉換器。
   - [consul](https://www.npmjs.com/package/consul) - Consul 的用戶端，與服務探索及配置相關。
   - [cookie-parser](https://www.npmjs.com/package/cookie-parser) - 剖析 Cookie 標頭並將以 Cookie 名稱為索引鍵的物件移入 req.cookies。
   - [elasticsearch](https://www.npmjs.com/package/elasticsearch) - Node.js 的正式低階 Elasticsearch 用戶端。
   - [errorhandler](https://www.npmjs.com/package/errorhandler) - 僅限開發的錯誤處理常式中介軟體。
   - [etcd3](https://www.npmjs.com/package/etcd3) - 針對通訊協定緩衝區型 etcdv3 API 的高品質、可供正式作業使用的用戶端。
   - [formidable](https://www.npmjs.com/package/formidable) - 用來剖析表單資料的 Node.js 模組，尤其是針對檔案上傳。
   - [glob](https://www.npmjs.com/package/glob) - 使用 Shell 使用的型樣來比對檔案，例如 stars 和 stuff。
   - [gm](https://www.npmjs.com/package/gm) - 適用於 Node 的 GraphicsMagick 及 ImageMagick。
   - [ibm-cos-sdk](https://www.npmjs.com/package/ibm-cos-sdk) - {{site.data.keyword.cos_full}} SDK for Node.js
   - [ibm_db](https://www.npmjs.com/package/ibm_db) - node.js 對 IBM Db2 及 IBM Informix 的非同步/同步介面。
   - [ibmiotf](https://www.npmjs.com/package/ibmiotf) - node.js 用戶端用於簡化與 IBM Watson Internet of Things Platform 的互動。
   - [iconv-lite](https://www.npmjs.com/package/iconv-lite) - 純正的 JS 字元編碼轉換
   - [jsdom](https://www.npmjs.com/package/jsdom) - jsdom 是許多 Web 標準的純正 JavaScript 實作，特別值得一提的是 WHATWG DOM 和 HTML 標準。
   - [jsforce](https://www.npmjs.com/package/jsforce) - JavaScript 應用程式的 Salesforce API 程式庫。
   - [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) - JSON Web 記號的實作。
   - [lodash](https://www.npmjs.com/package/lodash) - 匯出為 Node.js 模組的 Lodash 程式庫。
   - [marked](https://www.npmjs.com/package/marked) - 全功能 Markdown 剖析器及編譯器，以 JavaScript 撰寫。為追求速度而建置。
   - [merge](https://www.npmjs.com/package/merge) - 將多個物件合併為一個，選擇性地建立新的複製物件。
   - [moment](https://www.npmjs.com/package/moment) - 輕量型 JavaScript 日期程式庫，用於剖析、驗證、操作及格式化日期。
   - [mongodb](https://www.npmjs.com/package/mongodb) - Node.js 的正式 MongoDB 驅動程式。
   - [mysql](https://www.npmjs.com/package/mysql) - 這是 mysql 的 node.js 驅動程式。
   - [mustache](https://www.npmjs.com/package/mustache) - mustache.js 是 mustache 範本系統的實作，以 JavaScript 撰寫。
   - [nano](https://www.npmjs.com/package/nano) - Node.js 的簡約 couchdb 驅動程式。
   - [nodemailer](https://www.npmjs.com/package/nodemailer) - 從 Node.js 傳送電子郵件 - 超簡單！
   - [oauth2-server](https://www.npmjs.com/package/oauth2-server) - 完整、符合規範且經過詳盡測試的模組，用來快速以 Node.js 實作 OAuth2 伺服器/提供者。
   - [openwhisk](https://www.npmjs.com/package/openwhisk) - OpenWhisk 平台的 JavaScript 用戶端程式庫。提供 OpenWhisk API 的封套。
   - [path-to-regex](https://www.npmjs.com/package/path-to-regexp) - 將路徑字串（例如 /user/:name）轉換成正規表示式，然後便可以用來比對 URL 路徑。
   - [pg](https://www.npmjs.com/package/pg) - 適用於 node.js 的非阻擋式 PostgreSQL 用戶端。純正的 JavaScript 及選用性原生 libpq 連結。
   - [process](https://www.npmjs.com/package/process) - require('process')；就像任何其他模組。
   - [pug](https://www.npmjs.com/package/pug) - 實作 Pug 範本語言。
   - [redis](https://www.npmjs.com/package/redis) - 這是完整且特性豐富的 Node.js Redis 用戶端。
   - [request](https://www.npmjs.com/package/request) - Request 設計要成為進行 HTTP 呼叫的最簡單可能方式。
   - [request-promise](https://www.npmjs.com/package/request-promise) - 簡化的 HTTP 要求用戶端 'request'，含 Promise 支援。採用 Bluebird 技術。
   - [rimraf](https://www.npmjs.com/package/rimraf) - 適用於 node 的 UNIX 指令 rm -rf。
   - [semver](https://www.npmjs.com/package/semver) - Nodejs 的語意版本化
   - [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail) - 透過 SendGrid API 提供電子郵件支援。
   - [serialize-error](https://www.npmjs.com/package/serialize-error) - 將錯誤序列化成為一般物件。
   - [serve-favicon](https://www.npmjs.com/package/serve-favicon) - Node.js 中介軟體，用來負責處理 favicon。
   - [socket.io](https://www.npmjs.com/package/socket.io) - Socket.IO 能促成即時、雙向、以事件為基礎的通訊。
   - [socket.io-client](https://www.npmjs.com/package/socket.io-client) - socket.io 的即時應用程式架構。
   - [superagent](https://www.npmjs.com/package/superagent) - SuperAgent 是小型的先進用戶端 HTTP 要求程式庫，以及具有相同 API 的 Node.js 模組，支援許多高階 HTTP 用戶端特性。
   - [swagger-tools](https://www.npmjs.com/package/swagger-tools) - 提供各種工具的套件，用來整合 Swagger 及與其互動。
   - [twilio](https://www.npmjs.com/package/twilio) - Twilio API 的封套，和語音、視訊及傳訊相關。
   - [underscore](https://www.npmjs.com/package/underscore) - Underscore.js 是 JavaScript 的 utility-belt 程式庫，提供對於常見功能可疑物件的支援（each、map、reduce、filter...）而不延伸任何核心 JavaScript 物件。
   - [url-pattern](https://www.npmjs.com/package/url-pattern) - 剖析 URL 中的路徑參數，比起使用正規表示式字串比對程式更輕鬆。
   - [uuid](https://www.npmjs.com/package/uuid) - 簡單、快速地產生 RFC4122 UUID。
   - [validator](https://www.npmjs.com/package/validator) - 字串驗證器及消毒器的程式庫。
   - [vcap_services](https://www.npmjs.com/package/vcap_services) - 剖析並傳回 VCAP_SERVICES 環境變數（由 IBM Cloud 提供）中的服務認證。
   - [when](https://www.npmjs.com/package/when) - When.js 是堅固、經過各式測試的 Promises/A+ 及 when() 實作，包括完整的 ES6 Promise shim。
   - [winston](https://www.npmjs.com/package/winston) - 適用於 node.js 的多傳輸非同步記載程式庫。"CHILL WINSTON! ... I put it in the logs."
   - [ws](https://www.npmjs.com/package/ws) - ws 是簡單易用、超快速且經過徹底測試的 WebSocket 用戶端與用戶端伺服器實作。
   - [xlsx](https://www.npmjs.com/package/xlsx) - 各種試算表格式的剖析器及撰寫器。
   - [xml2js](https://www.npmjs.com/package/xml2js) - 簡單的 XML 對 JavaScript 物件轉換器。它支援雙向轉換。
   - [xmlhttprequest](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest 是內建 http 用戶端的封套，可以模擬瀏覽器 XMLHttpRequest 物件。
   - [yauzl](https://www.npmjs.com/package/yauzl) - 適用於 Node 的另一個解壓縮程式庫。

### 具有 IBM SDK 的 Node.js 第 8 版環境
{: #openwhisk_ref_javascript_environments_8}
如果在建立或更新動作時明確指定 `--kind` 旗標，且值為 `nodejs:8`，則會使用 Node.js 第 8 版環境。

Node.js 第 8 版處於維護模式，且在 2019 年 12 月之前可供使用。請參閱 [Node.js 版本排程](https://github.com/nodejs/Release)。
{: deprecated}

#### Node.js 8.15 套件

 - [8.15.0](https://nodejs.org/en/blog/release/v8.15.0)

  - [amqplib](https://www.npmjs.com/package/amqplib) - 用來製作 Node.JS 之 AMQP 0-9-1 用戶端的程式庫。
   - [apn](https://www.npmjs.com/package/apn) - 用來與 Apple Push Notification Service 互動的 Node.js 模組。
   - [async](https://www.npmjs.com/package/async) - 提供函數以便使用非同步函數。
   - [bent](https://www.npmjs.com/package/bent) - 適用於 async/await 的 Node.js 的功能性 HTTP 用戶端。
   - [bodyparser](https://www.npmjs.com/package/body-parser) - 在處理程式之前的中介軟體中剖析送入的要求內文，提供於 req.body 內容下。
   - [btoa](https://www.npmjs.com/package/btoa) - 瀏覽器 btoa 函數的埠。
   - [cassandra-driver](https://www.npmjs.com/package/cassandra-driver) - Apache Cassandra 的 DataStax Node.js 驅動程式。
   - [cloudant](https://www.npmjs.com/package/cloudant) - 這是 Node.js 的正式 Cloudant 程式庫。
   - [@cloudant/cloudant](https://www.npmjs.com/package/cloudant) - 這是 Node.js 的正式 Cloudant 程式庫。
   - [commander](https://www.npmjs.com/package/commander) - node.js 指令行介面的完整解決方案。
   - [composeaddresstranslator](https://www.npmjs.com/package/composeaddresstranslator) - Scylla 資料庫之 Compose 使用者介面或 API 的位址轉換器。
   - [consul](https://www.npmjs.com/package/consul) - Consul 的用戶端，與服務探索及配置相關。
   - [cookie-parser](https://www.npmjs.com/package/cookie-parser) - 剖析 Cookie 標頭並將以 Cookie 名稱為索引鍵的物件移入 req.cookies。
   - [elasticsearch](https://www.npmjs.com/package/elasticsearch) - Node.js 的正式低階 Elasticsearch 用戶端。
   - [errorhandler](https://www.npmjs.com/package/errorhandler) - 僅限開發的錯誤處理常式中介軟體。
   - [etcd3](https://www.npmjs.com/package/etcd3) - 針對通訊協定緩衝區型 etcdv3 API 的高品質、可供正式作業使用的用戶端。
   - [formidable](https://www.npmjs.com/package/formidable) - 用來剖析表單資料的 Node.js 模組，尤其是針對檔案上傳。
   - [glob](https://www.npmjs.com/package/glob) - 使用 Shell 使用的型樣來比對檔案，例如 stars 和 stuff。
   - [gm](https://www.npmjs.com/package/gm) - 適用於 Node 的 GraphicsMagick 及 ImageMagick。
   - [ibm-cos-sdk](https://www.npmjs.com/package/ibm-cos-sdk) - {{site.data.keyword.cos_full}} SDK for Node.js
   - [ibm_db](https://www.npmjs.com/package/ibm_db) - node.js 對 IBM Db2 及 IBM Informix 的非同步/同步介面。
   - [ibmiotf](https://www.npmjs.com/package/ibmiotf) - node.js 用戶端用於簡化與 IBM Watson Internet of Things Platform 的互動。
   - [iconv-lite](https://www.npmjs.com/package/iconv-lite) - 純正的 JS 字元編碼轉換
   - [jsdom](https://www.npmjs.com/package/jsdom) - jsdom 是許多 Web 標準的純正 JavaScript 實作，特別值得一提的是 WHATWG DOM 和 HTML 標準。
   - [jsforce](https://www.npmjs.com/package/jsforce) - JavaScript 應用程式的 Salesforce API 程式庫。
   - [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) - JSON Web 記號的實作。
   - [lodash](https://www.npmjs.com/package/lodash) - 匯出為 Node.js 模組的 Lodash 程式庫。
   - [log4js](https://www.npmjs.com/package/log4js) - 用於轉換 log4js 架構以使用 Node。
   - [marked](https://www.npmjs.com/package/marked) - 全功能 Markdown 剖析器及編譯器，以 JavaScript 撰寫。為追求速度而建置。
   - [merge](https://www.npmjs.com/package/merge) - 將多個物件合併為一個，選擇性地建立新的複製物件。
   - [moment](https://www.npmjs.com/package/moment) - 輕量型 JavaScript 日期程式庫，用於剖析、驗證、操作及格式化日期。
   - [mongodb](https://www.npmjs.com/package/mongodb) - Node.js 的正式 MongoDB 驅動程式。
   - [mysql](https://www.npmjs.com/package/mysql) - 這是 mysql 的 node.js 驅動程式。
   - [mustache](https://www.npmjs.com/package/mustache) - mustache.js 是 mustache 範本系統的實作，以 JavaScript 撰寫。
   - [nano](https://www.npmjs.com/package/nano) - Node.js 的簡約 couchdb 驅動程式。
   - [nodemailer](https://www.npmjs.com/package/nodemailer) - 從 Node.js 傳送電子郵件 - 超簡單！
   - [oauth2-server](https://www.npmjs.com/package/oauth2-server) - 完整、符合規範且經過詳盡測試的模組，用來快速以 Node.js 實作 OAuth2 伺服器/提供者。
   - [openwhisk](https://www.npmjs.com/package/openwhisk) - OpenWhisk 平台的 JavaScript 用戶端程式庫。提供 OpenWhisk API 的封套。
   - [path-to-regex](https://www.npmjs.com/package/path-to-regexp) - 將路徑字串（例如 /user/:name）轉換成正規表示式，然後便可以用來比對 URL 路徑。
   - [pg](https://www.npmjs.com/package/pg) - 適用於 node.js 的非阻擋式 PostgreSQL 用戶端。純正的 JavaScript 及選用性原生 libpq 連結。
   - [process](https://www.npmjs.com/package/process) - require('process')；就像任何其他模組。
   - [pug](https://www.npmjs.com/package/pug) - 實作 Pug 範本語言。
   - [redis](https://www.npmjs.com/package/redis) - 這是完整且特性豐富的 Node.js Redis 用戶端。
   - [request](https://www.npmjs.com/package/request) - Request 設計要成為進行 HTTP 呼叫的最簡單可能方式。
   - [request-promise](https://www.npmjs.com/package/request-promise) - 簡化的 HTTP 要求用戶端 'request'，含 Promise 支援。採用 Bluebird 技術。
   - [rimraf](https://www.npmjs.com/package/rimraf) - 適用於 node 的 UNIX 指令 rm -rf。
   - [semver](https://www.npmjs.com/package/semver) - Nodejs 的語意版本化
   - [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail) - 透過 SendGrid API 提供電子郵件支援。
   - [serialize-error](https://www.npmjs.com/package/serialize-error) - 將錯誤序列化成為一般物件。
   - [serve-favicon](https://www.npmjs.com/package/serve-favicon) - Node.js 中介軟體，用來負責處理 favicon。
   - [socket.io](https://www.npmjs.com/package/socket.io) - Socket.IO 能促成即時、雙向、以事件為基礎的通訊。
   - [socket.io-client](https://www.npmjs.com/package/socket.io-client) - socket.io 的即時應用程式架構。
   - [superagent](https://www.npmjs.com/package/superagent) - SuperAgent 是小型的先進用戶端 HTTP 要求程式庫，以及具有相同 API 的 Node.js 模組，支援許多高階 HTTP 用戶端特性。
   - [swagger-tools](https://www.npmjs.com/package/swagger-tools) - 提供各種工具的套件，用來整合 Swagger 及與其互動。
   - [twilio](https://www.npmjs.com/package/twilio) - Twilio API 的封套，和語音、視訊及傳訊相關。
   - [underscore](https://www.npmjs.com/package/underscore) - Underscore.js 是 JavaScript 的 utility-belt 程式庫，提供對於常見功能可疑物件的支援（each、map、reduce、filter...）而不延伸任何核心 JavaScript 物件。
   - [url-pattern](https://www.npmjs.com/package/url-pattern) - 剖析 URL 中的路徑參數，比起使用正規表示式字串比對程式更輕鬆。
   - [uuid](https://www.npmjs.com/package/uuid) - 簡單、快速地產生 RFC4122 UUID。
   - [validator](https://www.npmjs.com/package/validator) - 字串驗證器及消毒器的程式庫。
   - [vcap_services](https://www.npmjs.com/package/vcap_services) - 剖析並傳回 VCAP_SERVICES 環境變數（由 IBM Cloud 提供）中的服務認證。
   - [watson-developer-cloud](https://www.npmjs.com/package/watson-developer-cloud) - 使用 Watson Developer Cloud 服務的 Node.js 用戶端程式庫，這是一組使用認知運算來解決複雜問題的 API。
   - [when](https://www.npmjs.com/package/when) - When.js 是堅固、經過各式測試的 Promises/A+ 及 when() 實作，包括完整的 ES6 Promise shim。
   - [winston](https://www.npmjs.com/package/winston) - 適用於 node.js 的多傳輸非同步記載程式庫。"CHILL WINSTON! ... I put it in the logs."
   - [ws](https://www.npmjs.com/package/ws) - ws 是簡單易用、超快速且經過徹底測試的 WebSocket 用戶端與用戶端伺服器實作。
   - [xml2js](https://www.npmjs.com/package/xml2js) - 簡單的 XML 對 JavaScript 物件轉換器。它支援雙向轉換。
   - [xmlhttprequest](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest 是內建 http 用戶端的封套，可以模擬瀏覽器 XMLHttpRequest 物件。
   - [yauzl](https://www.npmjs.com/package/yauzl) - 適用於 Node 的另一個解壓縮程式庫。

Node.js 第 8 版運行環境的詳細資訊可以在 [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md) 中找到。


## Python 運行環境
{: #openwhisk_ref_python_environments}

有兩個不同的運行環境版本可供選擇以執行 Python 動作。依預設，所有 Python 動作都在 V2 環境中執行。


### Python 3.7 動作（Debian Stretch 型）
{: #openwhisk_ref_python_environments_3.7}

Python 3.7 動作是使用 Python 3.7.x 來執行。若要使用此運行環境，請在建立或更新動作時，指定 CLI 參數 `--kind python:3.7`。

除了 Python 3.7 標準程式庫之外，運行環境還包含可供 Python 動作使用之 IBM Cloud 服務的 SDK 套件。

#### Python 3.7.2 套件

 - [3.7.2](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile)

 Python 套件：
 - asn1crypto
 - attrs
 - Automat
 - beautifulsoup4
 - botocore
 - cassandra-driver
 - certifi
 - cffi
 - chardet
 - Click
 - cloudant
 - constantly
 - cryptography
 - cssselect
 - docutils
 - elasticsearch
 - etcd3
 - Flask
 - gevent
 - greenlet
 - grpcio
 - httplib2
 - hyperlink
 - ibm-cos-sdk
 - ibm-cos-sdk-core
 - ibm-cos-sdk-s3transfer
 - ibm-db
 - ibmcloudsql
 - idna
 - incremental
 - itsdangerous
 - Jinja2
 - jmespath
 - kafka-python
 - lxml
 - MarkupSafe
 - numpy
 - pandas
 - parsel
 - pika
 - Pillow
 - protobuf
 - psycopg2
 - pyarrow
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pymongo
 - pyOpenSSL
 - python-dateutil
 - pytz
 - queuelib
 - redis
 - requests
 - scikit-learn
 - scipy
 - Scrapy
 - service-identity
 - simplejson
 - six
 - soupsieve
 - tenacity
 - tornado
 - Twisted
 - urllib3
 - virtualenv
 - w3lib
 - watson-developer-cloud
 - websocket-client
 - Werkzeug
 - zope.interface

Python 3.7 運行環境的詳細資訊可以在 [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md) 中找到。

### Python 3.6.8 動作（以 Debian Jessie 為基礎）
{: #openwhisk_ref_python_environments_3.6}

Python 3 動作是使用 Python 3.6.x 來執行。若要使用此運行環境，請在建立或更新動作時，指定 CLI 參數 `--kind python:3.6`。

除了 Python 3.6 標準程式庫之外，運行環境還包含可供 Python 動作使用之 IBM Cloud 服務的 SDK 套件。

#### Python 3.6.8 套件

Python 版本：
 - [3.6.8](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile)

 - Python 套件：
 - asn1crypto
 - attrs
 - autobahn
 - Automat
 - beautifulsoup4
 - botocore
 - cassandra-driver
 - certifi
 - cffi
 - chardet
 - Click
 - cloudant
 - constantly
 - cryptography
 - cssselect
 - docutils
 - elasticsearch
 - Flask
 - gevent
 - greenlet
 - httplib2
 - hyperlink
 - ibm-cos-sdk
 - ibm-cos-sdk-core
 - ibm-cos-sdk-s3transfer
 - ibm-db
 - ibmcloudsql
 - idna
 - incremental
 - itsdangerous
 - Jinja2
 - jmespath
 - kafka-python
 - lxml
 - MarkupSafe
 - numpy
 - pandas
 - parsel
 - pika
 - Pillow
 - psycopg2
 - pyarrow
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pymongo
 - pyOpenSSL
 - python-dateutil
 - pytz
 - queuelib
 - redis
 - requests
 - scikit-learn
 - scipy
 - Scrapy
 - service-identity
 - simplejson
 - six=
 - soupsieve
 - tornado
 - Twisted
 - txaio
 - urllib3
 - virtualenv
 - w3lib
 - watson-developer-cloud
 - Werkzeug
 - zope.interface

Python 3.6 運行環境的詳細資訊可以在 [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md) 中找到。

### Python 2 動作

除非您在建立或更新動作時指定 `--kind` 旗標，否則會使用 Python 2.7.15 來執行 Python 2 動作。

使用 virtualenv 建立 Python 動作時，請使用 Docker 映像檔 `openwhisk/python2action`。
除了 Python 2.7 標準程式庫之外，Python 2 動作還可以使用下列套件。

#### Python 2 套件

 - asn1crypto
 - attrs
 - Automat
 - beautifulsoup4
 - certifi
 - cffi
 - chardet
 - Click
 - constantly
 - cryptography
 - cssselect
 - enum34
 - Flask
 - functools32
 - gevent
 - greenlet
 - httplib2
 - hyperlink
 - idna
 - incremental
 - ipaddress
 - itsdangerous
 - Jinja2
 - kafka-python
 - lxml
 - MarkupSafe
 - parsel
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pyOpenSSL
 - python-dateutil
 - queuelib
 - requests
 - Scrapy
 - service-identity
 - simplejson
 - six
 - Twisted
 - urllib3
 - virtualenv=
 - w3lib
 - Werkzeug
 - zope.interface

Python 2 運行環境的詳細資訊可以在 [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md) 中找到。


## Swift 運行環境
{: #swift-actions}

依預設，所有 Swift 動作都在 V4.2 環境中執行。

Swift 4.x 動作運行環境未內嵌任何套件，請遵循[包裝的 swift 動作](/docs/openwhisk?topic=cloud-functions-prep#prep_swift42_single)的指示，以包括使用 Package.swift 的相依關係。

使用單一 Swift 原始檔時，Swift 4.2 動作可以使用下列套件：
- Watson Developer Cloud SDK 1.2.0 版：https://github.com/watson-developer-cloud/swift-sdk


### 使用單一來源動作檔的 SwiftyJSON
如果您具有未編譯的動作，並且使用的是 **SwftyJSON** 套件，您需要前置編譯動作，並指定要用於 `swift:4.2` kind 動作的 SwiftyJSON 版本。


## PHP 運行環境
{: #openwhisk_ref_php}

依預設，所有 PHP 動作都在 V7.3 環境中執行。

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

## Docker 運行環境
{: #openwhisk_ref_docker}

Docker 動作是在 Docker 容器中執行使用者提供的二進位檔。二進位檔是在根據 [python:3.6-alpine](https://hub.docker.com/r/library/python) 的 Docker 映像檔中執行，因此二進位檔必須與此發行套件相容。

Docker 架構是建置 OpenWhisk 相容 Docker 映像檔的便利方法。您可以使用 `ibmcloud fn sdk install docker` CLI 外掛程式指令來安裝架構。

主要二進位程式必須位在容器的 `/action/exec` 中。執行檔會從可解除序列化為 `JSON` 物件的單一指令行引數字串接收到輸入引數。它必須使用 `stdout`，以已序列化 `JSON` 的單行字串形式來傳回結果。

您可以修改 `dockerSkeleton` 中包含的 `Dockerfile`，來包含任何編譯步驟或相依關係。



## Go 運行環境
{: #runtimes_go}

依預設，所有 Go 動作都在 V1.11 環境中執行。



## Java 運行環境
{: #runtimes_java}

依預設，所有 Java 動作都在 V8 環境中執行。



## Ruby 運行環境
{: #runtimes_ruby}

依預設，所有 Ruby 動作都在 V2.5 環境中執行。



## .NET Core 運行環境
{: #runtimes_dotnet}

依預設，所有 .NET Core 動作都在 V2.2 環境中執行。
