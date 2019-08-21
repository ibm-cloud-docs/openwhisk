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


# 运行时
{: #runtimes}
可以使用编程语言（如 JavaScript 或 Python）对应用程序进行编码和执行。缺省情况下，有许多运行时可用于 {{site.data.keyword.openwhisk_short}}。
{: shortdesc}

查看每个区域中 IBM Cloud Functions 的可用运行时。以下链接会返回 JSON 响应。响应的 `runtimes` 部分包含可用运行时集。`image` 部分包含 [Docker Hub](https://hub.docker.com/){: external} 上运行时映像的名称以及使用的标记。

  - [`us-south`](https://us-south.functions.cloud.ibm.com/){: external}
  - [`us-east`](https://us-east.functions.cloud.ibm.com/){: external}
  - [`eu-gb`](https://eu-gb.functions.cloud.ibm.com/){: external}
  - [`eu-de`](https://eu-de.functions.cloud.ibm.com/){: external}


以下示例指向映像 `ibmfunctions/action-nodejs-v10` 和 `openwhisk/nodejs8action`。标记可以是版本号（如 `1.9.0`），也可以是简短形式的 git 落实散列（如 `b99d71e`）。

示例 image 字段。
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs8action:b99d71e"
  ```

运行时会定期更新。这些更新包括对运行时内的包的安全修订和次版本更新。次版本更新可能会导致向后兼容性中断。运行时更新可能会影响操作。您必须通过更新运行时，将运行该运行时的操作迁移到更新的版本。

在不推荐使用的运行时上运行的应用程序无法成功完成，直到将相应运行时更新为支持的运行时为止。对失败的操作进行故障诊断时，要确定是否不推荐使用某个运行时，请在查询响应中检查是否有 `deprecated=true`。要更新运行时，请参阅[更改操作运行时](/docs/openwhisk?topic=cloud-functions-actions#actions_update)。

不推荐使用的运行时如下：
<ul>
  <li><code>nodejs:6</code>（不推荐）</li>
  <li><code>php:7.1</code>（不推荐）</li>
  <li><code>php:7.2</code>（不推荐）</li>
  <li><code>swift:3</code>（不推荐）</li>
  <li><code>swift:3.1.1</code>（不推荐）</li>
  <li><code>swift:4.1</code>（不推荐）</li>
  <li><code>ballerina:0.990</code>（不推荐）</li>
</ul>



## JavaScript 运行时
{: #openwhisk_ref_javascript_environments}

缺省情况下，所有 Node.js 操作都在 V10 环境中执行。
{: note}

JavaScript 操作可以使用 Node.js V8 或 V10 执行。Node.js V8 现在处于维护方式，要到 2019 年 12 月才可用。请参阅 [Node.js 发行版计划](https://github.com/nodejs/Release){: external}。
{: deprecated}

|运行时|描述|更改日志|
| --- | --- | --- |
|[10.15.0](https://nodejs.org/en/blog/release/v10.15.0/){: external}|缺省情况下，所有 Node.js 操作都在 V10 环境中执行。|[CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md){: external}。|
|[8.15.0](https://nodejs.org/en/blog/release/v8.15.0/){: external}|如果在创建或更新操作时使用值 `nodejs:8` 显式指定 `--kind` 标志，那么将使用 Node.js V8.15.0。|[CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md){: external}。|


### 从 nodeJS 8 迁移到 nodeJS 10

|包|详细信息|
| --- | --- |
|`ibm_db`|`ibm_db` NPM 包在 `nodejs:10` 中不可用。`ibm_db` 包不支持 Node.js 10。您可以在[此问题](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541){: external}中跟踪进度。|
|`cloudant`|`cloudant` NPM 包在 `nodejs:10` 中不可用。不推荐使用该包。在导入 Node.js 模块时，需要使用正式 NPM 包 [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant){: external} V3.0.0（即 `require('@cloudant/cloudant')`），此外 [V3.x 仅返回 Promise](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x){: external}。|
|`cradle`|`cradle` NPM 包在 `nodejs:10` 中不可用。|
|`log4js`|`log4js` NPM 包在 `nodejs:10` 中不可用。您可以跟踪[此问题](https://github.com/log4js-node/log4js-node/issues/805){: external}。|
|`watson-developer-cloud`|`watson-developer-cloud` NPM 包在 `nodejs:10` 中不可用。您可以在[此问题](https://github.com/watson-developer-cloud/node-sdk/issues/780){: external}中跟踪新版本的进度。|


### Node.js 包

|Node.js 10.15 包|描述|
|:-----------------|:-----------------|
|[`amqplib`](https://www.npmjs.com/package/amqplib){: external}|用于使高级消息传递排队协议 0-9-1 客户机可用于 Node.JS 的库。|
|[`apn`](https://www.npmjs.com/package/apn){: external}|用于与 Apple 推送通知服务进行交互的 Node.js 模块。|
|[`async`](https://www.npmjs.com/package/async){: external}|提供用于处理异步功能的函数。|
|[`bent`](https://www.npmjs.com/package/bent){: external}|用于支持 async 和 await 的 Node.js 的功能性 HTTP 客户机。|
|[`bodyparser`](https://www.npmjs.com/package/body-parser){: external}|解析中间件中处理程序（位于 req.body 属性下）之前的入局请求主体。|
|[`btoa`](https://www.npmjs.com/package/btoa){: external}|浏览器的 `btoa` 函数的端口。|
|[`cassandra-driver`](https://www.npmjs.com/package/cassandra-driver){: external}|用于 Apache Cassandra 的 DataStax Node.js 驱动程序。|
|[`@cloudant/cloudant`](https://www.npmjs.com/package/@cloudant/cloudant){: external}|用于 Node.js 的正式 Cloudant 库。|
|[`commander`](https://www.npmjs.com/package/commander){: external}|Node.js 命令行界面的完整解决方案。|
|[`composeaddresstranslator`](https://www.npmjs.com/package/composeaddresstranslator){: external}|Compose UI 或 API 中用于 Scylla 数据库的地址转换程序。|
|[`consul`](https://www.npmjs.com/package/consul){: external}|Consul 的客户机，涉及服务发现和配置。|
|[`cookie-parser`](https://www.npmjs.com/package/cookie-parser){: external}|解析 cookie 标头，并使用以 cookie 名称为键的对象填充 req.cookies。|
|[`elasticsearch`](https://www.npmjs.com/package/elasticsearch){: external}|Node.js 的正式低级别 ElasticSearch 客户机。|
|[`errorhandler`](https://www.npmjs.com/package/errorhandler){: external}|仅用于开发的错误处理程序中间件。|
|[`etcd3`](https://www.npmjs.com/package/etcd3){: external}|用于基于协议缓冲区的 etcdv3 API 的高质量生产就绪型客户机。|
|[`formidable`](https://www.npmjs.com/package/formidable){: external}|用于解析表单数据（尤其是文件上传）的 Node.js 模块。|
|[`glob`](https://www.npmjs.com/package/glob){: external}|通过 shell 使用的模式（如星号和内容）来匹配文件。|
|[`gm`](https://www.npmjs.com/package/gm){: external}|用于 Node 的 GraphicsMagick 和 ImageMagick。|
|[`ibm-cos-sdk`](https://www.npmjs.com/package/ibm-cos-sdk){: external}|用于 Node.js 的 {{site.data.keyword.cos_full}} SDK|
|[`ibm_db`](https://www.npmjs.com/package/ibm_db){: external}|Node.js 用于与 IBM Db2 和 IBM Informix 连接的异步/同步接口。|
|[`ibmiotf`](https://www.npmjs.com/package/ibmiotf){: external}|用于简化与 IBM Watson Internet of Things Platform 的交互的 Node.js 客户机。|
|[`iconv-lite`](https://www.npmjs.com/package/iconv-lite){: external}|纯 JS 字符编码转换。|
|[`jsdom`](https://www.npmjs.com/package/jsdom){: external}|`jsdom` 是许多 Web 标准（特别是 Web 超文本应用程序技术工作组 DOM 和 HTML 标准）的纯 JavaScript 实现。|
|[`jsforce`](https://www.npmjs.com/package/jsforce){: external}|用于 JavaScript 应用程序的 Salesforce API 库。|
|[`jsonwebtoken`](https://www.npmjs.com/package/jsonwebtoken){: external}|JSON Web 令牌的实现。|
|[`lodash`](https://www.npmjs.com/package/lodash){: external}|`lodash` 库导出为 Node.js 模块。|
|[`marked`](https://www.npmjs.com/package/marked){: external}|使用 JavaScript 编写的功能齐全的 Markdown 解析器和编译器。专为速度而构建。|
|[`merge`](https://www.npmjs.com/package/merge){: external}|将多个对象合并为一个对象，可选择创建新的克隆对象。
|[`moment`](https://www.npmjs.com/package/moment){: external}|用于解析、验证、处理和格式化日期的轻量级 JavaScript 日期库。|
|[`mongodb`](https://www.npmjs.com/package/mongodb){: external}|用于 Node.js 的正式 MongoDB 驱动程序。|
|[`mysql`](https://www.npmjs.com/package/mysql){: external}|用于 MySQL 的 Node.js 驱动程序。|
|[`mustache`](https://www.npmjs.com/package/mustache){: external}|Mustache.js 是用 JavaScript 编写的 mustache 模板系统的实现。|
|[`nano`](https://www.npmjs.com/package/nano){: external}|用于 Node.js 的极简 CouchDB 驱动程序。|
|[`nodemailer`](https://www.npmjs.com/package/nodemailer){: external}|通过 Node.js 发送电子邮件 - 轻而易举！|
|[`oauth2-server`](https://www.npmjs.com/package/oauth2-server){: external}|用于通过 Node.js 中 Express 实现 OAuth2 服务器/提供程序的完整、合规且经过测试的模块。|
|[`openwhisk`](https://www.npmjs.com/package/openwhisk){: external}|用于 OpenWhisk 平台的 JavaScript 客户机库。基于 OpenWhisk API 提供包装程序。|
|[`path-to-regex`](https://www.npmjs.com/package/path-to-regexp){: external}|将路径字符串（例如 `/user/:name`）转换为正则表达式，然后该表达式可用于与 URL 路径进行匹配。|
|[`pg`](https://www.npmjs.com/package/pg){: external}|用于 Node.js 的非阻塞性 PostgreSQL 客户机。纯 JavaScript 和可选的本机 `libpq` 绑定。|
|[`process`](https://www.npmjs.com/package/process){: external}|`require('process')`；就像任何其他模块一样。|
|[`pug`](https://www.npmjs.com/package/pug){: external}|实现 Pug 模板语言。|
|[`redis`](https://www.npmjs.com/package/redis){: external}|用于 Node.js 的完整且功能丰富的 Redis 客户机。|
|[`request`](https://www.npmjs.com/package/request){: external}|发出 HTTP 调用。|
|[`request-promise`](https://www.npmjs.com/package/request-promise){: external}|简化的 HTTP 请求客户机“request”，支持使用 Promise。基于 Bluebird 技术。|
|[`rimraf`](https://www.npmjs.com/package/rimraf){: external}|针对 Node 的 UNIX 命令 rm -rf。|
|[`semver`](https://www.npmjs.com/package/semver){: external}|针对 nodeJS 的语义版本控制。|
|[`@sendgrid/mail`](https://www.npmjs.com/package/@sendgrid/mail){: external}|通过 SendGrid API 提供电子邮件支持。|
|[`serialize-error`](https://www.npmjs.com/package/serialize-error){: external}|将错误序列化为普通对象。|
|[`serve-favicon`](https://www.npmjs.com/package/serve-favicon){: external}|用于提供收藏图标的 Node.js 中间件。
|[`socket.io`](https://www.npmjs.com/package/socket.io){: external}|`socket.io` 支持基于事件的实时双向通信。|
|[`socket.io-client`](https://www.npmjs.com/package/socket.io-client){: external}|用于 `socket.io` 的实时应用程序框架。|
|[`superagent`](https://www.npmjs.com/package/superagent){: external}|`superagent` 是一种小型渐进式客户机端 HTTP 请求库，是具有相同 API 的 Node.js 模块，带有许多高级别的 HTTP 客户机功能。|
|[`swagger-tools`](https://www.npmjs.com/package/swagger-tools){: external}|提供用于与 Swagger 集成和交互的各种工具的包。|
|[`twilio`](https://www.npmjs.com/package/twilio){: external}|Twilio API 的包装程序，与语音、视频和消息传递相关。|
|[`underscore`](https://www.npmjs.com/package/underscore){: external}|Underscore.js 是一种 JavaScript 实用工具集库，支持可能的常用函数（each、map、reduce、filter...），无需扩展任何核心 JavaScript 对象。|
|[`url-pattern`](https://www.npmjs.com/package/url-pattern){: external}|相比使用正则表达式字符串匹配器，可更轻松地解析路径参数的 URL。|
|[`uuid`](https://www.npmjs.com/package/uuid){: external}|简单、快速生成 RFC4122 UUID。|
|[`validator`](https://www.npmjs.com/package/validator){: external}|字符串验证器和杀毒器的库。|
|[`vcap_services`](https://www.npmjs.com/package/vcap_services){: external}|解析并返回 IBM Cloud 提供的 VCAP_SERVICES 环境变量中的服务凭证。|
|[`when`](https://www.npmjs.com/package/when){: external}|When.js 是一种极其稳固、经过实际测试的 `Promise/A+` 和 `when()` 实现，包含完整的 ES6 Promise 垫片。|
|[`winston`](https://www.npmjs.com/package/winston){: external}|Node.js 的多传输异步日志记录库。“别紧张 WINSTON！我把它放在日志里了。”|
|[`ws`](https://www.npmjs.com/package/ws){: external}|`ws` 是一种使用简单、速度超快且经过全面测试的 WebSocket 客户机和服务器实现。|
|[`xlsx`](https://www.npmjs.com/package/xlsx){: external}|用于各种电子表格格式的解析器和写程序。|
|[`xml2js`](https://www.npmjs.com/package/xml2js){: external}|简单的 XML 到 JavaScript 对象转换器。支持双向转换。|
|[`xmlhttprequest`](https://www.npmjs.com/package/xmlhttprequest){: external}|node-XMLHttpRequest 是内置 HTTP 客户机的包装程序，用于模拟浏览器 XMLHttpRequest 对象。|
|[`yauzl`](https://www.npmjs.com/package/yauzl){: external}|用于 Node 的另一个解压缩库。|
{: caption="表 1. Node.js 10.15 包。" caption-side="top"}
{: #javascript-1}
{: tab-title="Node.js 10.15 packages"}
{: tab-group="node"}
{: class="simple-tab-table"}

|Node.js 8.15 包|描述|
|:-----------------|:-----------------|
|[`amqplib`](https://www.npmjs.com/package/amqplib){: external}|用于使高级消息传递排队协议 0-9-1 客户机可用于 Node.JS 的库。|
|[`apn`](https://www.npmjs.com/package/apn){: external}|用于与 Apple 推送通知服务进行交互的 Node.js 模块。|[`async`](https://www.npmjs.com/package/async){: external}|提供用于处理异步功能的函数。|
|[`bent`](https://www.npmjs.com/package/bent){: external}|用于支持 async-await 的 Node.js 的功能性 HTTP 客户机。|
|[`bodyparser`](https://www.npmjs.com/package/body-parser){: external}|解析中间件中处理程序（位于 req.body 属性下）之前的入局请求主体。|
|[`btoa`](https://www.npmjs.com/package/btoa){: external}|浏览器的 `btoa` 函数的端口。|
|[`cassandra-driver`](https://www.npmjs.com/package/cassandra-driver){: external}|用于 Apache Cassandra 的 DataStax Node.js 驱动程序。|
|[`cloudant`](https://www.npmjs.com/package/cloudant){: external}|用于 Node.js 的正式 Cloudant 库。|
|[`@cloudant/cloudant`](https://www.npmjs.com/package/cloudant){: external}|用于 Node.js 的正式 Cloudant 库。|
|[`commander`](https://www.npmjs.com/package/commander){: external}|Node.js 命令行界面的完整解决方案。|
|[`composeaddresstranslator`](https://www.npmjs.com/package/composeaddresstranslator){: external}|Compose UI 或 API 中用于 Scylla 数据库的地址转换程序。|
|[`consul`](https://www.npmjs.com/package/consul){: external}|Consul 的客户机，涉及服务发现和配置。|
|[`cookie-parser`](https://www.npmjs.com/package/cookie-parser){: external}|解析 cookie 标头，并使用以 cookie 名称为键的对象填充 req.cookies。|
|[`elasticsearch`](https://www.npmjs.com/package/elasticsearch){: external}|Node.js 的正式低级别 ElasticSearch 客户机。|
|[`errorhandler`](https://www.npmjs.com/package/errorhandler){: external}|仅用于开发的错误处理程序中间件。|
|[`etcd3`](https://www.npmjs.com/package/etcd3){: external}|用于基于协议缓冲区的 etcdv3 API 的高质量生产就绪型客户机。|
|[`formidable`](https://www.npmjs.com/package/formidable){: external}|用于解析表单数据（尤其是文件上传）的 Node.js 模块。|
|[`glob`](https://www.npmjs.com/package/glob){: external}|通过 shell 使用的模式（如星号和内容）来匹配文件。|
|[`gm`](https://www.npmjs.com/package/gm){: external}|用于 Node 的 GraphicsMagick 和 ImageMagick。|
|[`ibm-cos-sdk`](https://www.npmjs.com/package/ibm-cos-sdk){: external}|用于 Node.js 的 {{site.data.keyword.cos_full}} SDK。|
|[`ibm_db`](https://www.npmjs.com/package/ibm_db){: external}|Node.js 用于与 IBM Db2 和 IBM Informix 连接的异步/同步接口。|
|[`ibmiotf`](https://www.npmjs.com/package/ibmiotf){: external}|Node.js 客户机用于简化与 IBM Watson Internet of Things Platform 的交互。|
|[`iconv-lite`](https://www.npmjs.com/package/iconv-lite){: external}|纯 JS 字符编码转换。|
|[`jsdom`](https://www.npmjs.com/package/jsdom){: external}|`jsdom` 是许多 Web 标准（特别是 Web 超文本应用程序技术工作组 DOM 和 HTML 标准）的纯 JavaScript 实现。|
|[`jsforce`](https://www.npmjs.com/package/jsforce){: external}|用于 JavaScript 应用程序的 Salesforce API 库。|
|[`jsonwebtoken`](https://www.npmjs.com/package/jsonwebtoken){: external}|JSON Web 令牌的实现。|
|[`lodash`](https://www.npmjs.com/package/lodash){: external}|导出为 Node.js 模块的 `lodash` 库。|
|[`log4js`](https://www.npmjs.com/package/log4js){: external}|用于转换 log4js 框架以使用 Node。|
|[`marked`](https://www.npmjs.com/package/marked){: external}|使用 JavaScript 编写的功能齐全的 Markdown 解析器和编译器。专为速度而构建。|
|[`merge`](https://www.npmjs.com/package/merge){: external}|将多个对象合并为一个对象，可选择创建新的克隆对象。
|
|[`moment`](https://www.npmjs.com/package/moment){: external}|用于解析、验证、处理和格式化日期的轻量级 JavaScript 日期库。|
|[`mongodb`](https://www.npmjs.com/package/mongodb){: external}|用于 Node.js 的正式 MongoDB 驱动程序。|
|[`mysql`](https://www.npmjs.com/package/mysql){: external}|用于 MySQL 的 Node.js 驱动程序。|
|[`mustache`](https://www.npmjs.com/package/mustache){: external}|mustache.js 是用 JavaScript 编写的 mustache 模板系统的实现。|
|[`nano`](https://www.npmjs.com/package/nano){: external}|用于 Node.js 的极简 CouchDB 驱动程序。|
|[`nodemailer`](https://www.npmjs.com/package/nodemailer){: external}|通过 Node.js 发送电子邮件 - 轻而易举！|
|[`oauth2-server`](https://www.npmjs.com/package/oauth2-server){: external}|用于通过 Node.js 中 Express 实现 OAuth2 服务器/提供程序的完整、合规且经过测试的模块。|
|[`openwhisk`](https://www.npmjs.com/package/openwhisk){: external}|用于 OpenWhisk 平台的 JavaScript 客户机库。基于 OpenWhisk API 提供包装程序。|
|[`path-to-regex`](https://www.npmjs.com/package/path-to-regexp){: external}|将路径字符串（例如 `/user/:name`）转换为正则表达式，然后该表达式可用于与 URL 路径进行匹配。|[`pg`](https://www.npmjs.com/package/pg){: external}|用于 Node.js 的非阻塞性 PostgreSQL 客户机。纯 JavaScript 和可选的本机 `libpq` 绑定。|
|[`process`](https://www.npmjs.com/package/process){: external}|`require('process')`；就像任何其他模块一样。|
|[`pug`](https://www.npmjs.com/package/pug){: external}|实现 Pug 模板语言。|
|[`redis`](https://www.npmjs.com/package/redis){: external}|用于 Node.js 的完整且功能丰富的 Redis 客户机。|
|[`request`](https://www.npmjs.com/package/request){: external}|发出 HTTP 调用。|
|[`request-promise`](https://www.npmjs.com/package/request-promise){: external}|简化的 HTTP 请求客户机“request”，支持使用 Promise。基于 Bluebird 技术。|
|[`rimraf`](https://www.npmjs.com/package/rimraf){: external}|针对 Node 的 UNIX 命令 rm -rf。|
|[`semver`](https://www.npmjs.com/package/semver){: external}|针对 nodeJS 的语义版本控制。|
|[`@sendgrid/mail`](https://www.npmjs.com/package/@sendgrid/mail){: external}|通过 SendGrid API 提供电子邮件支持。|
|[`serialize-error`](https://www.npmjs.com/package/serialize-error){: external}|将错误序列化为普通对象。|
|[`serve-favicon`](https://www.npmjs.com/package/serve-favicon){: external}|用于提供收藏图标的 Node.js 中间件。
|
|[`socket.io`](https://www.npmjs.com/package/socket.io){: external}|`socket.io` 支持基于事件的实时双向通信。|
|[`socket.io-client`](https://www.npmjs.com/package/socket.io-client){: external}|用于 `socket.io` 的实时应用程序框架。|
|[`superagent`](https://www.npmjs.com/package/superagent){: external}|`superagent` 是一种小型渐进式客户机端 HTTP 请求库，是具有相同 API 的 Node.js 模块，带有许多高级别的 HTTP 客户机功能。|
|[`swagger-tools`](https://www.npmjs.com/package/swagger-tools){: external}|提供用于与 Swagger 集成和交互的各种工具的包。|
|[`twilio`](https://www.npmjs.com/package/twilio){: external}|Twilio API 的包装程序，与语音、视频和消息传递相关。|
|[`underscore`](https://www.npmjs.com/package/underscore){: external}|Underscore.js 是一种 JavaScript 实用工具集库，支持可能的常用函数（each、map、reduce、filter...），无需扩展任何核心 JavaScript 对象。|
|[`url-pattern`](https://www.npmjs.com/package/url-pattern){: external}|相比使用正则表达式字符串匹配器，可更轻松地解析路径参数的 URL。|
|[`uuid`](https://www.npmjs.com/package/uuid){: external}|简单、快速生成 RFC4122 UUID。|
|[`validator`](https://www.npmjs.com/package/validator){: external}|字符串验证器和杀毒器的库。|
|[`vcap_services`](https://www.npmjs.com/package/vcap_services){: external}|解析并返回 IBM Cloud 提供的 VCAP_SERVICES 环境变量中的服务凭证。|
|[`watson-developer-cloud`](https://www.npmjs.com/package/watson-developer-cloud){: external}|使用 Watson Developer Cloud 服务的 Node.js 客户机库，这是一组使用认知计算来解决复杂问题的 API。|
|[`when`](https://www.npmjs.com/package/when){: external}|When.js 是一种极其稳固、经过实际测试的 `Promise/A+` 和 `when()` 实现，包含完整的 ES6 Promise 垫片。|
|[`winston`](https://www.npmjs.com/package/winston){: external}|Node.js 的多传输异步日志记录库。“别紧张 WINSTON！我把它放在日志里了。”|
|[`ws`](https://www.npmjs.com/package/ws){: external}|`ws` 是一种使用简单、速度超快且经过全面测试的 WebSocket 客户机和服务器实现。|
|[`xml2js`](https://www.npmjs.com/package/xml2js){: external}|简单的 XML 到 JavaScript 对象转换器。支持双向转换。|
|[`xmlhttprequest`](https://www.npmjs.com/package/xmlhttprequest){: external}|`node-XMLHttpRequest` 是内置 HTTP 客户机的包装程序，用于模拟浏览器 XMLHttpRequest 对象。|
|[`yauzl`](https://www.npmjs.com/package/yauzl){: external}|用于 Node 的另一个解压缩库。|
{: caption="表 2. Node.js 8.15 包。" caption-side="top"}
{: #javascript-2}
{: tab-title="Node.js 8.15 packages"}
{: tab-group="node"}
{: class="simple-tab-table"}


## Python 运行时
{: #openwhisk_ref_python_environments}

缺省情况下，所有 Python 操作都在 V2.7.15 环境中执行。
{: note}

|Python 版本|描述|更改日志|
| --- | --- | --- |
|2.7.15|缺省情况下，所有 Python 操作都在 V2.7.15 环境中执行，除非在创建或更新操作时指定 `--kind` 标志。使用 `virtualenv` 创建 Python 操作时，请使用 Docker 映像 `openwhisk/python2action`。除了 Python 2.7 标准库外，以下包也可供 Python 2 操作使用。|[CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md){: external}。|
|[3.6.8](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile){: external}|Python 3 操作使用 Python 3.6.x 执行。要使用此运行时，请在创建或更新操作时，指定 CLI 参数 `--kind python:3.6`。除了 Python 3.6 标准库外，该运行时还包含 IBM Cloud 服务的 SDK 包，可供 Python 操作使用。|[CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md){: external}。|
|[3.7.2](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile){: external}|Python 3.7 操作（基于 Debian Stretch）使用 Python 3.7.x 执行。要使用此运行时，请在创建或更新操作时，指定 CLI 参数 `--kind python:3.7`。除了 Python 3.7 标准库外，该运行时还包含 IBM Cloud 服务的 SDK 包，可供 Python 操作使用。|[CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md){: external}。|


### Python 包

|Python 2.7.15 包| 
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
{: caption="表 1. Python 2.7.15 包。" caption-side="top"}
{: #python-1}
{: tab-title="Python 2.7.15 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}

|Python 3.6.8 包| 
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
|`cloudant`|
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
{: caption="表 2. Python 3.6.8 包。" caption-side="top"}
{: #python-2}
{: tab-title="Python 3.6.8 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}

|Python 3.7.2 包| 
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
|`cloudant`|
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
{: caption="表 3. Python 3.7.2 包。" caption-side="top"}
{: #python-3}
{: tab-title="Python 3.7.2 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}


## Swift 运行时
{: #swift-actions}

缺省情况下，所有 Swift 操作都在 V4.2 环境中执行。
{: note}

Swift 4.x 操作运行时未嵌入任何包，请遵循[打包的 Swift 操作](/docs/openwhisk?topic=cloud-functions-prep#prep_swift42_single)的指示信息以使用 Package.swift 来包含依赖项。

在使用单个 Swift 源文件时，Swift 4.2 操作可以使用以下包：
- Watson Developer Cloud SDK V1.2.0：https://github.com/watson-developer-cloud/swift-sdk


### 使用单个源操作文件的 <ph class="ignoreSpelling">SwiftyJSON</ph>
如果您具有未编译的操作，并且使用的是 `SwftyJSON` 包，那么您需要预编译操作，并指定要用于 `swift:4.2` kind 操作的 `SwiftyJSON` 版本。


## PHP 运行时
{: #openwhisk_ref_php}

缺省情况下，所有 PHP 操作都在 V7.3 环境中执行。
{: note}

除了标准扩展名外，还可使用以下 PHP 扩展名：

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

## Docker 运行时
{: #openwhisk_ref_docker}

Docker 操作在 Docker 容器中运行用户提供的可执行文件。该可执行文件在基于 [python:3.6-alpine](https://hub.docker.com/_/python){: external} 的 Docker 映像中运行，所以该可执行文件必须与此分发版兼容。

通过 Docker 框架，可以方便地构建兼容 OpenWhisk 的 Docker 映像。可以使用 `ibmcloud fn sdk install docker` CLI 插件命令来安装该框架。

主可执行程序必须位于容器内的 `/action/exec` 中。可执行文件通过可以反序列化为 `JSON` 对象的单个命令行自变量字符串来接收输入自变量。该文件必须使用 `stdout` 以单行序列化 `JSON` 字符串形式返回结果。

您可以通过修改 `dockerSkeleton` 中包含的 `Dockerfile` 来包含任何编译步骤或依赖关系。

## 更多运行时支持

|运行时|
| --- | --- | 
|缺省情况下，所有 Go 操作都在 V1.11 环境中执行。|
{: caption="表 1. Go。" caption-side="top"}
{: #runtimes-1}
{: tab-title="Go"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

|运行时|
| --- | --- | 
|缺省情况下，所有 Java 操作都在 V8 环境中执行。|
{: caption="表 2. Java。" caption-side="top"}
{: #runtimes-2}
{: tab-title="Java"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

|运行时|
| --- |
|缺省情况下，所有 Ruby 操作都在 V2.5 环境中执行。|
{: caption="表 3. Ruby。" caption-side="top"}
{: #runtimes-3}
{: tab-title="Ruby"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

|运行时|
| --- |
|缺省情况下，所有 .NET Core 操作都在 V2.2 环境中执行。|
{: caption="表 4. .NET Core。" caption-side="top"}
{: #runtimes-4}
{: tab-title=".NET"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}




