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

# {{site.data.keyword.openwhisk_short}} 系统详细信息
{: #openwhisk_reference}

以下各部分提供了有关 {{site.data.keyword.openwhisk}} 系统的更多详细信息。
{: shortdesc}

## {{site.data.keyword.openwhisk_short}} 实体
{: #openwhisk_entities}

### 名称空间和包
{: #openwhisk_entities_namespaces}

{{site.data.keyword.openwhisk_short}} 操作、触发器和规则属于名称空间，有时可属于包。

包可以包含操作和订阅源。一个包不能包含其他包，所以不允许包嵌套。此外，实体不必包含在包中。

在 {{site.data.keyword.Bluemix_notm}} 中，组织+空间对与 {{site.data.keyword.openwhisk_short}} 名称空间相对应。例如，组织 `BobsOrg` 和空间 `dev` 将对应于 {{site.data.keyword.openwhisk_short}} 名称空间 `/BobsOrg_dev`。

如果您有权创建名称空间，那么可以创建自己的名称空间。`/whisk.system` 名称空间保留用于使用 {{site.data.keyword.openwhisk_short}} 系统分发的实体。


### 标准名称
{: #openwhisk_entities_fullyqual}

实体的标准名称为
`/namespaceName[/packageName]/entityName`。请注意，`/` 用于对名称空间、包和实体定界。此外，名称空间必须带有前缀 `/`。

为了方便起见，如果名称空间是用户的*缺省名称空间*，那么可以保留不变。

例如，假设用户的缺省名称空间为 `/myOrg`。以下是一些实体的标准名称及其别名的示例。

| 标准名称| 别名| 名称空间| 包| 名称|
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read`|  | `/whisk.system`| `cloudant`| `read`|
| `/myOrg/video/transcode`| `video/transcode`| `/myOrg`| `video`| `transcode`|
| `/myOrg/filter`| `filter`| `/myOrg`|  | `filter`|

使用 {{site.data.keyword.openwhisk_short}} CLI 时，可以使用此命名方案，在其他位置也同样可使用此方案。

### 实体名称
{: #openwhisk_entities_names}

包括操作、触发器、规则、包和名称空间在内的所有实体的名称均为遵循以下格式的字符序列：

* 第一个字符必须为字母数字字符或下划线。
* 后续字符可以为字母数字、空格或以下任一值：`_`、`@`、`.` 和 `-`。
* 最后一个字符不能为空格。

更准确地说，名称必须匹配以下正则表达式（使用 Java 元字符语法表达）：`\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`。

## 操作语义
{: #openwhisk_semantics}

以下各部分描述了有关 {{site.data.keyword.openwhisk_short}} 操作的详细信息。

### 无状态
{: #openwhisk_semantics_stateless}

操作实施是无状态的，即*幂等*。虽然系统不会强制实施此属性，但不保证操作保持的任何状态在各调用中均可用。

此外，一个操作可能存在多个实例化，其中每个实例化都有其自己的状态。可能会将操作调用分派给其中任一实例化。

### 调用输入和输出
{: #openwhisk_semantics_invocationio}

操作的输入和输出是键/值对的字典。键为字符串，值为有效的 JSON 值。

### 操作的调用顺序
{: #openwhisk_ordering}

操作的调用并未排序。如果用户通过命令行或 REST API 调用一个操作两次，那么第二个调用可能会先于第一个调用运行。如果操作有副作用，那么可能会以任意顺序观察到这些副作用。

此外，无法保证操作自动执行。两个操作可以并行运行，其副作用可能会交错。对于副作用，OpenWhisk 并不能确保任何特定的并行一致性模型。任何并行副作用均依赖于实施。

### 操作执行保证
{: #openwhisk_atmostonce}

收到调用请求时，系统会记录该请求，并分派激活。

系统会返回激活标识（对于非阻塞性调用），以确认收到了调用。如果存在网络故障或其他在接收 HTTP 响应之前引起干预的故障，那么 {{site.data.keyword.openwhisk_short}} 有可能会收到并处理该请求。

系统尝试调用操作一次，会生成以下四个结果之一：
- *成功*：操作调用成功完成。
- *应用程序错误*：操作调用成功，但操作有意返回了错误值，例如由于不满足自变量上的前置条件。
- *操作开发者错误*：操作已调用，但以异常方式完成，例如操作未检测到异常或存在语法错误。
- *whisk 内部错误*：系统无法调用操作。
结果会记录在激活记录的 `status` 字段中，如以下部分中所述。

成功收到的每个调用以及可能对用户记帐的每个调用都将有激活记录。

结果为*操作开发者错误*时，操作可能已部分运行并生成外部可视的副作用。用户应负责检查是否实际发生了此类副作用并根据需要发出重试逻辑。某些 *whisk 内部错误*指示操作已开始运行，但在操作注册完成之前失败。

## 激活记录
{: #openwhisk_ref_activation}

每次操作调用和触发器触发都会生成一个激活记录。

激活记录包含以下字段：

- *activationId*：激活标识。
- *start* 和 *end*：记录激活开始时间和结束时间的时间戳记。值为 [UNIX 时间格式](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15)。
- *namespace* 和 `name`：实体的名称空间和名称。
- *logs*：字符串数组，其中包含在操作激活期间由操作生成的日志。每个数组元素对应于操作向 `stdout` 或 `stderr` 生成的一行输出，并且包含日志输出的时间和流。结构如下：`TIMESTAMP STREAM: LOG_OUTPUT`。
- *response*：用于定义 `success`、`status` 和 `result` 键的字典：
  - *status*：激活结果，可能为以下某个值：“success”、“application error”、“action developer error”和“whisk internal error”。
  - *success*：当且仅当 status 为“`success`”时，此项为 `true`
- *result*：包含激活结果的字典。如果激活成功，那么结果包含操作返回的值。如果激活不成功，那么 `result` 会包含 `error` 键，通常还会带有失败说明。


## JavaScript 操作
{: #openwhisk_ref_javascript}

### 函数原型
{: #openwhisk_ref_javascript_fnproto}

{{site.data.keyword.openwhisk_short}} JavaScript 操作在 Node.js 运行时中运行。

用 JavaScript 编写的操作必须限制为单个文件。该文件可以包含多个函数，但根据约定，必须存在名为 `main` 的函数，并且此函数是调用操作时调用的函数。例如，以下示例显示具有多个函数的操作。
```javascript
function main() {
    return { payload: helper() }
}

function helper() {
    return new Date();
}
```
{: codeblock}

操作输入参数会作为 JSON 对象传递到 `main` 函数。成功调用的结果也是 JSON 对象，但返回方式根据操作是同步还是异步而有所不同，如以下部分中所述。


### 同步和异步行为
{: #openwhisk_ref_javascript_synchasynch}

JavaScript 函数即便返回之后，仍在回调函数中继续执行是很常见的情况。要允许此操作，JavaScript 操作的激活可以是*同步*或*异步*的。

如果 main 函数在以下某个条件下退出，那么 JavaScript 操作的激活是**同步**的：

- main 函数在不执行 `return` 语句的情况下退出。
- main 函数通过执行 `return` 语句退出，此语句返回*除* Promise 以外的任何值。

请参阅以下同步操作示例：

```javascript
// 其中每条路径生成同步激活的操作
function main(params) {
  if (params.payload == 0) {
     return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
    return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}

如果 main 函数通过返回 Promise 退出，那么 JavaScript 操作的激活是**异步**的。在此情况下，系统假定操作仍在运行，直到履行或拒绝 Promise 为止。通过实例化新 Promise 对象并向其传递回调函数开始。回调函数采用两个自变量 resolve 和 reject，这两个自变量都是函数。所有异步代码都会进入该回调函数。

在以下示例中，可以查看如何通过调用 resolve 函数来履行 Promise。
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

以下示例显示如何通过调用 reject 函数来拒绝 Promise。
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

一个操作可以在某些输入上是同步的，而在其他输入上是异步的，如以下示例中所示。 
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

不管激活是同步还是异步的，操作的调用都可以是阻塞性或非阻塞性的。

### JavaScript 全局 whisk 对象已除去

全局对象 `whisk` 已除去；请迁移 nodejs 操作以使用替代方法。对于 `whisk.invoke()` 和 `whisk.trigger()` 函数，请使用已经安装的客户机库 [openwhisk](https://www.npmjs.com/package/openwhisk)。对于 `whisk.getAuthKey()`，可以从 `__OW_API_KEY` 环境变量中获取 API 密钥值。对于 `whisk.error()`，可以返回拒绝的 Promise（即 Promise.reject）。

### JavaScript 运行时环境
{: #openwhisk_ref_javascript_environments}

JavaScript 操作可以在 Node.js V6 或 Node.js V8 中执行。
目前，操作缺省情况下在 Node.js V6.11.4 环境中执行。  

### Node.js V6 环境
{: #openwhisk_ref_javascript_environments_6}
如果在创建/更新操作时将 `--kind` 标志的值显式指定为 `nodejs:6`，那么将使用 Node.js 6.12.2 环境。

以下包可在 Node.js 6.12.2 环境中使用：

- [apn v2.1.2](https://www.npmjs.com/package/apn) - 用于与 Apple 推送通知服务进行交互的 Node.js 模块。
- [async v2.1.4](https://www.npmjs.com/package/async) - 提供用于处理异步功能的函数。
- [btoa v1.1.2](https://www.npmjs.com/package/btoa) - 浏览器的 btoa 函数的端口。
- [cheerio v0.22.0](https://www.npmjs.com/package/cheerio) - 专为服务器设计的快速、灵活、精益的核心 jQuery 实现。
- [cloudant v1.6.2](https://www.npmjs.com/package/cloudant) - Node.js 的正式 Cloudant 库。
- [commander v2.9.0](https://www.npmjs.com/package/commander) - Node.js 命令行界面的完整解决方案。
- [consul v0.27.0](https://www.npmjs.com/package/consul) - Consul 的客户机，涉及服务发现和配置。
- [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - 解析 cookie 标头，并使用以 cookie 名称为键的对象填充 req.cookies。
- [cradle v0.7.1](https://www.npmjs.com/package/cradle) - Node.js 的高级别高速缓存 CouchDB 客户机。
- [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - 仅用于开发的错误处理程序中间件。
- [glob v7.1.1](https://www.npmjs.com/package/glob) - 通过 shell 使用的模式（如星号和内容）来匹配文件。
- [gm v1.23.0](https://www.npmjs.com/package/gm) - 用于 Node 的 GraphicsMagick 和 ImageMagick。
- [lodash v4.17.2](https://www.npmjs.com/package/lodash) - 导出为 Node.js 模块的 Lodash 库。
- [log4js v0.6.38](https://www.npmjs.com/package/log4js) - 转换设计为使用 Node 的 log4js 框架。 
- [iconv-lite v0.4.15](https://www.npmjs.com/package/iconv-lite) - 纯 JS 字符编码转换。
- [marked v0.3.6](https://www.npmjs.com/package/marked) - 使用 JavaScript 编写的功能齐全的 Markdown 解析器和编译器。专为速度而构建。
- [merge v1.2.0](https://www.npmjs.com/package/merge) - 将多个对象合并为一个对象，以创建新的克隆对象。 
- [moment v2.17.0](https://www.npmjs.com/package/moment) - 用于解析、验证、处理和格式化日期的轻量级 JavaScript 日期库。
- [mongodb v2.2.11](https://www.npmjs.com/package/mongodb) - Node.js 的正式 MongoDB 驱动程序。
- [mustache v2.3.0](https://www.npmjs.com/package/mustache) - mustache.js 是用 JavaScript 编写的 mustache 模板系统的实现。
- [nano v6.2.0](https://www.npmjs.com/package/nano) - Node.js 的极简 couchdb 驱动程序。
- [node-uuid v1.4.7](https://www.npmjs.com/package/node-uuid) - 不推荐使用的打包的 UUID。 
- [nodemailer v2.6.4](https://www.npmjs.com/package/nodemailer) - 通过 Node.js 发送电子邮件 - 轻而易举！
- [oauth2-server v2.4.1](https://www.npmjs.com/package/oauth2-server) - 用于通过 Node.js 中 Express 实现 OAuth2 服务器/提供程序的完整、合规且行之有效的模块。
- [openwhisk v3.11.0](https://www.npmjs.com/package/openwhisk) - OpenWhisk 平台的 JavaScript 客户机库。基于 OpenWhisk API 提供包装程序。
- [pkgcloud v1.4.0](https://www.npmjs.com/package/pkgcloud) - pkgcloud 是 Node.js 的标准库，可提取出多个云提供者之间的差异。
- [process v0.11.9](https://www.npmjs.com/package/process) - require('process')；就像任何其他模块一样。
- [pug v2.0.0-beta6](https://www.npmjs.com/package/pug) - 实现 Pug 模板语言。
- [redis v2.6.3](https://www.npmjs.com/package/redis) - Node.js 的完整且功能丰富的 Redis 客户机。 
- [request v2.79.0](https://www.npmjs.com/package/request) - 请求是发起 HTTP 调用的最简单方法。
- [request-promise v4.1.1](https://www.npmjs.com/package/request-promise) - 简化的 HTTP 请求客户机“请求”，支持使用 Promise。基于 Bluebird 技术。
- [rimraf v2.5.4](https://www.npmjs.com/package/rimraf) - 针对 Node 的 UNIX 命令 rm -rf。
- [semver v5.3.0](https://www.npmjs.com/package/semver) - 支持语义版本控制。
- [sendgrid v4.7.1](https://www.npmjs.com/package/sendgrid) - 通过 SendGrid API 提供电子邮件支持。
- [serve-favicon v2.3.2](https://www.npmjs.com/package/serve-favicon) - 用于提供 favicon 的 Node.js 中间件。
- [socket.io v1.6.0](https://www.npmjs.com/package/socket.io) - Socket.IO 支持基于事件的实时双向通信。
- [socket.io-client v1.6.0](https://www.npmjs.com/package/socket.io-client) - Socket.IO 的客户机端支持。
- [superagent v3.0.0](https://www.npmjs.com/package/superagent) - SuperAgent 是一种小型渐进式客户机端 HTTP 请求库，是具有相同 API 的 Node.js 模块，带有许多高级别的 HTTP 客户机功能。
- [swagger-tools v0.10.1](https://www.npmjs.com/package/swagger-tools) - 与使用 Swagger 相关的工具，是用于记录 API 的一种方法。
- [tmp v0.0.31](https://www.npmjs.com/package/tmp) - Node.js 的简单临时文件和目录创建程序。
- [twilio v2.11.1](https://www.npmjs.com/package/twilio) - Twilio API 的包装程序，与语音、视频和消息传递相关。
- [underscore v1.8.3](https://www.npmjs.com/package/underscore) - Underscore.js 是一种 JavaScript 实用工具集库，支持可能的常用函数（each、map、reduce、filter...），无需扩展任何核心 JavaScript 对象。
- [uuid v3.0.0](https://www.npmjs.com/package/uuid) - 简单、快速生成 RFC4122 UUID。
- [validator v6.1.0](https://www.npmjs.com/package/validator) - 字符串验证器和杀毒器的库。
- [watson-developer-cloud v2.29.0](https://www.npmjs.com/package/watson-developer-cloud) - 使用 Watson Developer Cloud 服务的 Node.js 客户机库，这是一组使用认知计算来解决复杂问题的 API。
- [when v3.7.7](https://www.npmjs.com/package/when) - When.js 是一种极其稳定、经过实际测试的 Promise/A+ 和 when() 实现，包含完整的 ES6 Promise 垫片。
- [winston v2.3.0](https://www.npmjs.com/package/winston) - Node.js 的多传输异步日志记录库。“别紧张 WINSTON！我把它放在日志里了。”
- [ws v1.1.1](https://www.npmjs.com/package/ws) - ws 是一种使用简单、速度超快且经过全面测试的 WebSocket 客户机和服务器实现。
- [xml2js v0.4.17](https://www.npmjs.com/package/xml2js) - 简单的 XML 到 JavaScript 对象转换器。支持双向转换。
- [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest 是内置 HTTP 客户机的包装程序，用于模拟浏览器 XMLHttpRequest 对象。
- [yauzl v2.7.0](https://www.npmjs.com/package/yauzl) - 是 Node 的另一个解压缩库。有关压缩。

### Node.js V8 环境
{: #openwhisk_ref_javascript_environments_8}
如果在创建/更新操作时将 `--kind` 标志的值显示指定为 `nodejs:8`，那么将使用 Node.js V8.9.3 环境。

Node.js V8.9.3 环境中预安装了以下包：

  - [apn v2.1.5](https://www.npmjs.com/package/apn) - 用于与 Apple 推送通知服务进行交互的 Node.js 模块。
  - [async v2.6.0](https://www.npmjs.com/package/async) - 提供用于处理异步功能的函数。
  - [bent v1.1.0](https://www.npmjs.com/package/btoa) - 浏览器的 btoa 函数的端口。
  - [btoa v1.1.2](https://www.npmjs.com/package/btoa) - 浏览器的 btoa 函数的端口。
  - [cloudant v1.10.0](https://www.npmjs.com/package/cloudant) - 这是 Node.js 的正式 Cloudant 库。
  - [commander v2.12.2](https://www.npmjs.com/package/commander) - Node.js 命令行界面的完整解决方案。
  - [consul v0.30.0](https://www.npmjs.com/package/consul) - Consul 的客户机，涉及服务发现和配置。
  - [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - 解析 cookie 标头，并使用以 cookie 名称为键的对象填充 req.cookies。
  - [cradle v0.7.1](https://www.npmjs.com/package/cradle) - Node.js 的高级别高速缓存 CouchDB 客户机。
  - [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - 仅用于开发的错误处理程序中间件。
  - [glob v7.1.2](https://www.npmjs.com/package/glob) - 通过 shell 使用的模式（如星号和内容）来匹配文件。
  - [gm v1.23.0](https://www.npmjs.com/package/gm) - 用于 Node 的 GraphicsMagick 和 ImageMagick。
  - [ibm-cos-sdk v1.1.1](https://www.npmjs.com/package/ibm-cos-sdk) - 用于 Node.js 的 IBM Cloud Object Storage SDK
  - [ibm_db v2.2.1](https://www.npmjs.com/package/ibm_db) - Node.js 与 IBM Db2 和 IBM Informix 连接的异步/同步接口。 
  - [lodash v4.17.4](https://www.npmjs.com/package/lodash) - 导出为 Node.js 模块的 Lodash 库。
  - [log4js v2.3.12](https://www.npmjs.com/package/log4js) - 转换设计为使用 Node 的 log4js 框架。
  - [iconv-lite v0.4.19](https://www.npmjs.com/package/iconv-lite) - 纯 JS 字符编码转换。
  - [marked v0.3.7](https://www.npmjs.com/package/marked) - 使用 JavaScript 编写的功能齐全的 Markdown 解析器和编译器。专为速度而构建。
  - [merge v1.2.0](https://www.npmjs.com/package/merge) - 将多个对象合并为一个对象，可选择创建新的克隆对象。
  - [moment v2.19.3](https://www.npmjs.com/package/moment) - 用于解析、验证、处理和格式化日期的轻量级 JavaScript 日期库。
  - [mongodb v2.2.33](https://www.npmjs.com/package/mongodb) - Node.js 的正式 MongoDB 驱动程序。
  - [mustache v2.3.0](https://www.npmjs.com/package/mustache) - mustache.js 是用 JavaScript 编写的 mustache 模板系统的实现。
  - [nano v6.4.2](https://www.npmjs.com/package/nano) - Node.js 的极简 couchdb 驱动程序。
  - [nodemailer v4.4.1](https://www.npmjs.com/package/nodemailer) - 通过 Node.js 发送电子邮件 - 轻而易举！
  - [oauth2-server v3.0.0](https://www.npmjs.com/package/oauth2-server) - 用于通过 Node.js 中 Express 实现 OAuth2 服务器/提供程序的完整、合规且行之有效的模块。
  - [openwhisk v3.11.0](https://www.npmjs.com/package/openwhisk) - OpenWhisk 平台的 JavaScript 客户机库。基于 OpenWhisk API 提供包装程序。
  - [process v0.11.10](https://www.npmjs.com/package/process) - require('process')；与任何其他模块一样。
  - [pug v2.0.0-rc.4](https://www.npmjs.com/package/pug) - 实现 Pug 模板语言。
  - [redis v2.8.0](https://www.npmjs.com/package/redis) - 这是 Node.js 的完整且功能丰富的 Redis 客户机。
  - [request v2.83.0](https://www.npmjs.com/package/request) - 请求是发起 HTTP 调用的最简单方法。
  - [request-promise v4.2.2](https://www.npmjs.com/package/request-promise) - 简化的 HTTP 请求客户机“请求”，支持使用 Promise。基于 Bluebird 技术。
  - [rimraf v2.6.2](https://www.npmjs.com/package/rimraf) - 针对 Node 的 UNIX 命令 rm -rf。
  - [semver v5.4.1](https://www.npmjs.com/package/semver) - 支持语义版本控制。
  - [@sendgrid/mail@6.1.4](https://www.npmjs.com/package/@sendgrid/mail) - 通过 SendGrid API 提供电子邮件支持。
  - [serve-favicon v2.4.5](https://www.npmjs.com/package/serve-favicon) - 用于提供 favicon 的 Node.js 中间件。
  - [socket.io v2.0.4](https://www.npmjs.com/package/socket.io) - Socket.IO 支持基于事件的实时双向通信。
  - [socket.io-client v2.0.4](https://www.npmjs.com/package/socket.io-client) - Socket.IO 的客户机端支持。
  - [superagent v3.8.2](https://www.npmjs.com/package/superagent) - SuperAgent 是一种小型渐进式客户机端 HTTP 请求库，是具有相同 API 的 Node.js 模块，带有许多高级别的 HTTP 客户机功能。
  - [swagger-tools v0.10.3](https://www.npmjs.com/package/swagger-tools) - 与使用 Swagger 相关的工具，是用于记录 API 的一种方法。
  - [tmp v0.0.33](https://www.npmjs.com/package/tmp) - Node.js 的简单临时文件和目录创建程序。
  - [twilio v3.10.1](https://www.npmjs.com/package/twilio) - Twilio API 的包装程序，与语音、视频和消息传递相关。
  - [underscore v1.8.3](https://www.npmjs.com/package/underscore) - Underscore.js 是一种 JavaScript 实用工具集库，支持可能的常用函数（each、map、reduce、filter...），无需扩展任何核心 JavaScript 对象。
  - [uuid v3.1.0](https://www.npmjs.com/package/uuid) - 简单、快速生成 RFC4122 UUID。
  - [validator v9.2.0](https://www.npmjs.com/package/validator) - 字符串验证器和杀毒器的库。
  - [watson-developer-cloud v3.0.2](https://www.npmjs.com/package/watson-developer-cloud) - 使用 Watson Developer Cloud 服务的 Node.js 客户机库，这是一组使用认知计算来解决复杂问题的 API。
  - [when v3.7.8](https://www.npmjs.com/package/when) - When.js 是一种极其稳定、经过实际测试的 Promise/A+ 和 when() 实现，包含完整的 ES6 Promise 垫片。
  - [winston v2.4.0](https://www.npmjs.com/package/winston) - Node.js 的多传输异步日志记录库。“别紧张 WINSTON！我把它放在日志里了。”
  - [ws v3.3.1](https://www.npmjs.com/package/ws) - ws 是一种使用简单、速度超快且经过全面测试的 WebSocket 客户机和服务器实现。
  - [xml2js v0.4.19](https://www.npmjs.com/package/xml2js) - 简单的 XML 到 JavaScript 对象转换器。支持双向转换。
  - [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest 是内置 HTTP 客户机的包装程序，用于模拟浏览器 XMLHttpRequest 对象。
  - [yauzl v2.9.1](https://www.npmjs.com/package/yauzl) - 是 Node 的另一个解压缩库。有关压缩

### 将 npm 包与操作打包在一起
对于任何未在 Node.js 环境中预安装的 `npm` 包，可以在创建或更新操作时将其捆绑为依赖项。

有关更多信息，请参阅[将操作打包为 Node.js 模块](./openwhisk_actions.html#openwhisk_js_packaged_action)或[将操作打包为单个捆绑软件](./openwhisk_actions.html#openwhisk_js_webpack_action)。


## Python 运行时环境
{: #openwhisk_ref_python_environments}

OpenWhisk 支持使用两种不同的运行时版本来运行 Python 操作。


### Python 3 操作（基于 Jessie）
{: #openwhisk_ref_python_environments_jessie}

Python 3 操作使用 Python 3.6.4 执行。要使用此运行时，请在创建或更新操作时，指定 `wsk` CLI 参数 `--kind python-jessie:3`。
使用 virtualenv 创建 Python 操作时，请使用 Docker 映像 `ibmfunctions/action-python-v3`。
除了 Python 3.6 标准库外，该运行时还包含 IBM Cloud 服务的 SDK 包，可供 Python 操作使用。

Python 版本：
- [3.6.4](https://github.com/docker-library/python/blob/a1aa406bfd8c7b129e6e0ee0ba972b863624ac0d/3.6/jessie/Dockerfile)

Python 包：
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


### Python 3 操作（基于 Alpine）
{: #openwhisk_ref_python_environments_alpine}

Python 3 操作通过 Python 3.6.1 执行。要使用此运行时，请在创建或更新操作时，指定 `wsk` CLI 参数 `--kind python:3`。
使用 virtualenv 创建 Python 操作时，请使用 Docker 映像 `openwhisk/python3action`。
除了 Python 3.6 标准库外，以下包也可供 Python 操作使用。

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


### Python 2 操作

Python 2 操作使用 Python 2.7.12 执行，这是 Python 操作的缺省运行时。除非在创建或更新操作时指定 `--kind` 标志。要显式选择此运行时，请使用 `--kind python:2`。使用 virtualenv 创建 Python 操作时，请使用 Docker 映像 `openwhisk/python2action`。
除了 Python 2.7 标准库外，以下包也可供 Python 2 操作使用。

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

## Swift 操作
{: #openwhisk_ref_swift3}

### Swift 3
Swift 3 操作使用 Swift 3.1.1 `--kind swift:3.1.1` 执行。请始终指定 kind `swift:3.1.1`，因为先前的 Swift 版本不受支持。


必须迁移所有 Swift 操作以使用 kind `swift:3.1.1`。最好始终在创建或更新操作时提供特定类型。
{: tip}

Swift 3.1.1 操作可以使用以下包：
- KituraNet V1.7.6：https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON V15.0.1：https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK V0.16.0：https://github.com/watson-developer-cloud/swift-sdk

## PHP 操作
{: #openwhisk_ref_php}

PHP 操作使用 PHP 7.1 执行。要使用此运行时，请在创建或更新操作时，指定 `wsk` CLI 参数 `--kind php:7.1`。这是使用扩展名为 `.php` 的文件创建操作时的缺省行为。

除了标准扩展名外，还可使用以下 PHP 扩展名：

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

### 组合器包
另外还提供了以下组合器包：

- guzzlehttp/guzzle       v6.3.0
- ramsey/uuid             v3.6.1

## Docker 操作
{: #openwhisk_ref_docker}

Docker 操作在 Docker 容器中运行用户提供的二进制文件。该二进制文件在基于 [python:2.7.12-alpine](https://hub.docker.com/r/library/python) 的 Docker 映像中运行，所以该二进制文件必须与此分发版兼容。

通过 Docker 框架，可以方便地构建兼容 OpenWhisk 的 Docker 映像。可以使用 `wsk sdk install docker` CLI 命令安装该框架。

主二进制程序必须位于容器内的 `/action/exec` 中。可执行文件通过可以反序列化为 `JSON` 对象的单个命令行自变量字符串来接收输入自变量。该文件必须通过 `stdout` 以单行序列化 `JSON` 字符串形式返回结果。

您可以通过修改 `dockerSkeleton` 中包含的 `Dockerfile` 来包含任何编译步骤或依赖关系。

## REST API
{: #openwhisk_ref_restapi}
有关 REST API 的信息位于[此处](openwhisk_rest_api.html)

## 系统限制
{: #openwhisk_syslimits}

### 操作
{{site.data.keyword.openwhisk_short}} 存在一些系统限制，包括一个操作可以使用的内存量和每分钟允许的操作调用数。

下表列出了操作的缺省限制。

| 限制| 描述
| 缺省值| 最小值| 最大值| 
| ----- | ----------- | :-------: | :---: | :---: |
| [codeSize](openwhisk_reference.html#openwhisk_syslimits_codesize)| 操作码的最大大小（以 MB 为单位）。| 48| 1| 48|
| [concurrent](openwhisk_reference.html#openwhisk_syslimits_concurrent)| 每个名称空间提交的正在执行或排队等待执行的激活数不得超过 N。| 1000| 1| 1000* |
| [logs](openwhisk_reference.html#openwhisk_syslimits_logs)| 不允许容器向 stdout 写入超过 N MB。| 10| 0| 10|
| [memory](openwhisk_reference.html#openwhisk_syslimits_memory)| 不允许容器分配的内存超过 N MB。| 256| 128| 512|
| [minuteRate](openwhisk_reference.html#openwhisk_syslimits_minuterate)| 每个名称空间每分钟提交的激活数不得超过 N。| 5000| 1| 5000*|
| [openulimit](openwhisk_reference.html#openwhisk_syslimits_openulimit)| Docker 操作的最大打开文件数。| 64| 0| 64|
| [parameters](openwhisk_reference.html#openwhisk_syslimits_parameters)| 可以附加的参数的最大大小（以 MB 为单位）。| 1| 0| 1|
| [proculimit](openwhisk_reference.html#openwhisk_syslimits_proculimit)| Docker 操作可用的最大进程数。| 512| 0| 512|
| [result](openwhisk_reference.html#openwhisk_syslimits_result)| 操作调用结果的最大大小（以 MB 为单位）。| 1| 0| 1|
| [timeout](openwhisk_reference.html#openwhisk_syslimits_timeout)| 不允许容器运行时间超过 N 毫秒。| 60000| 100| 300000|

### 增大固定限制
{: #increase_fixed_limit}

以 (*) 结尾的限制值是固定值，但如果业务用例可以证明使用更高的安全限制值是合理的，那么可以增大这些值。如果要增大限制值，请通过直接在 IBM [{{site.data.keyword.openwhisk_short}} Web 控制台](https://console.bluemix.net/openwhisk/)中开具凭单来联系 IBM 支持。
  1. 选择**支持**。
  2. 从下拉菜单中选择**添加凭单**。
  3. 对于凭单类型，选择**技术**。
  4. 对于“技术支持领域”，选择**函数**。

#### codeSize (MB)（固定值：48 MB）
{: #openwhisk_syslimits_codesize}
* 操作的最大代码大小为 48 MB。
* 建议 JavaScript 操作使用工具，将所有源代码（包括依赖关系）连接为单个捆绑文件。
* 此限制是固定值，不能更改。

#### concurrent（固定值：1000*）
{: #openwhisk_syslimits_concurrent}
* 针对一个名称空间正在执行或排队等待执行的激活数不能超过 1000。
* 此限制值是固定值，但如果业务用例可以证明使用更高的安全限制值是合理的，那么可以增大此值。请查看[增大固定限制](openwhisk_reference.html#increase_fixed_limit)部分，以获取有关如何增大此限制的详细指示信息。

#### logs (MB)（固定值：10 MB）
{: #openwhisk_syslimits_logs}
* 日志限制 N 在 [0 MB..10 MB] 范围内，并且按操作进行设置。
* 创建或更新操作时，用户可以更改操作日志限制。
* 超过所设置限制的日志会被截断，所以将忽略任何新的日志条目，并且在激活的最后输出中会添加警告，指出激活超出所设置的日志限制。

#### memory (MB)（固定值：256 MB）
{: #openwhisk_syslimits_memory}
* 内存限制 M 在 [128 MB..512 MB] 范围内，并且按操作进行设置，以 MB 为单位。
* 创建操作时，用户可以更改内存限制。
* 容器使用的内存不能超过此限制所分配的内存。

#### minuteRate（固定值：5000*）
{: #openwhisk_syslimits_minuterate}
* 速率限制 N 设置为 5000，用于限制 1 分钟时段中的操作调用数。
* 超过此限制的 CLI 或 API 调用将收到与 HTTP 状态码“`429：请求过多`”对应的错误代码。
* 此限制值是固定值，但如果业务用例可以证明使用更高的安全限制值是合理的，那么可以增大此值。请查看[增大固定限制](openwhisk_reference.html#increase_fixed_limit)部分，以获取有关如何增大此限制的详细指示信息。

#### openulimit（固定值：1024:1024）
{: #openwhisk_syslimits_openulimit}
* Docker 操作的最大打开文件数为 1024（同时适用于硬限制和软限制）。
* 此限制是固定值，不能更改。
* docker run 命令使用自变量 `--ulimit nofile=1024:1024`。
* 有关更多信息，请参阅 [docker run](https://docs.docker.com/engine/reference/commandline/run) 命令行参考文档。

#### parameters（固定值：1 MB）
{: #openwhisk_syslimits_parameters}
* 创建或更新操作/包/触发器的参数总大小限制为 1 MB。
* 尝试创建或更新具有过大参数的实体时，会遭到拒绝。
* 此限制是固定值，不能更改。

#### proculimit（固定值：1024:1024）
{: #openwhisk_syslimits_proculimit}
* Docker 操作容器可用的最大进程数为 1024。
* 此限制是固定值，不能更改。
* docker run 命令使用自变量 `--pids-limit 1024`。
* 有关更多信息，请参阅 [docker run](https://docs.docker.com/engine/reference/commandline/run) 命令行参考文档。

#### result（固定值：1 MB）
{: #openwhisk_syslimits_result}
* 操作调用结果的最大输出大小（以 MB 为单位）。
* 此限制是固定值，不能更改。

#### timeout（毫秒）（缺省值：60 秒）
{: #openwhisk_syslimits_timeout}
* 超时限制 N 在 [100 毫秒..300000 毫秒] 范围内，并且按操作进行设置，以毫秒为单位。
* 创建操作时，用户可以更改超时限制。
* 将终止运行时间超过 N 毫秒的容器。

### 触发器

触发器受每分钟触发率的影响，如下表中所述。

| 限制| 描述
| 缺省值| 最小值| 最大值| 
| ----- | ----------- | :-------: | :---: | :---: |
| [minuteRate](openwhisk_reference.html#openwhisk_syslimits_tminuterate)| 每个名称空间每分钟触发的触发器数不得超过 N。| 5000*| 5000*| 5000*|

### 增大固定限制
{: #increase_fixed_tlimit}

以 (*) 结尾的限制值是固定值，但如果业务用例可以证明使用更高的安全限制值是合理的，那么可以增大这些值。如果要增大限制值，请通过直接在 IBM [{{site.data.keyword.openwhisk_short}} Web 控制台](https://console.bluemix.net/openwhisk/)中开具凭单来联系 IBM 支持。
  1. 选择**支持**。
  2. 从下拉菜单中选择**添加凭单**。
  3. 对于凭单类型，选择**技术**。
  4. 对于“技术支持领域”，选择**函数**。

#### minuteRate（固定值：5000*）
{: #openwhisk_syslimits_tminuterate}

* 速率限制 N 设置为 5000，用于限制用户在 1 分钟时段中可以触发的触发器数。
* 创建触发器时，用户无法更改触发器限制。
* 超过此限制的 CLI 或 API 调用将收到与 HTTP 状态码“`429：请求过多`”对应的错误代码。
* 此限制值是固定值，但如果业务用例可以证明使用更高的安全限制值是合理的，那么可以增大此值。请查看[增大固定限制](openwhisk_reference.html#increase_fixed_tlimit)部分，以获取有关如何增大此限制的详细指示信息。
