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

# API 网关
{: #openwhisk_apigateway}

通过 API Management 来管理 OpenWhisk 操作对 OpenWhisk 操作十分有利。

API 网关充当 [Web 操作](./openwhisk_webactions.html)的代理，并为这些操作提供更多功能。这些额外的功能包括：HTTP 方法路由、客户机标识/私钥、速率限制、CORS、查看 API 使用情况和响应日志，以及 API 共享策略。
有关 API Management 的更多信息，可以阅读 [API Management 文档](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis)。
{: shortdesc}

## 使用浏览器通过 OpenWhisk Web 操作创建 API。

使用 API 网关，可以将 OpenWhisk 操作作为 API 公开。定义 API 后，可以应用安全性和速率限制策略，查看 API 使用情况和响应日志，以及定义 API 共享策略。在 OpenWhisk“仪表板”中，单击 [API 选项卡](https://console.ng.bluemix.net/openwhisk/apimanagement)。


## 使用 CLI 通过 OpenWhisk Web 操作创建 API

### OpenWhisk CLI 配置

使用 API 主机配置 OpenWhisk CLI。

有两个 {{site.data.keyword.Bluemix_notm}} 区域可用，它们需要自己的唯一 API 主机和授权密钥。

* 美国南部
  * API 主机：`openwhisk.ng.bluemix.net`

* 英国
  * API 主机：`openwhisk.eu-gb.bluemix.net`

运行以下命令来为所需 Bluemix 区域设置 API 主机：

美国南部：
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

英国：
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

如果需要切换区域，必须使用 API 主机和授权密钥来重新配置 CLI，因为授权密钥是特定于每个区域的。
{: tip}

工件（例如，操作、规则和包）是特定于区域的。所以，如果在多个区域中使用相同工件，必须将其部署到每个所需区域。

为了能够使用 `wsk api` 命令，CLI 配置文件 `~/.wskprops` 需要包含 Bluemix 访问令牌。

要获取访问令牌，请使用以下 CLI 命令：
```
wsk bluemix login
```
{: pre}

有关此命令的更多信息，请运行：
```
wsk bluemix login -h
```
{: pre}

如果 `wsk bluemix login` 命令失败，并返回错误 `BMXLS0202E: 您使用的是联合用户标识，请使用 --sso 选项通过一次性代码登录`，使用 `bluemix login` 通过 {{site.data.keyword.Bluemix_notm}} CLI 登录，然后发出 `wsk bluemix login --sso`。
{: tip}

### 使用 CLI 创建第一个 API

1. 创建具有以下内容的 JavaScript 文件。对于此示例，文件名为“hello.js”。
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}
  
2. 通过以下 JavaScript 函数创建 Web 操作。对于此示例，操作名为“hello”。确保添加 `--web true` 标志。
  ```
  wsk action create hello hello.js --web true
  ```
  {: pre}

  ```
  ok: created action hello
  ```
  
3. 创建基本路径为 `/hello`、路径为 `/world`、方法为 `get` 且响应类型为 `json` 的 API：
  ```
  wsk api create /hello /world get hello --response-type json
  ```

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  这将生成新 URL，用于通过 __GET__ HTTP 方法公开 `hello` 操作。
  
4. 最后，向 URL 发送 HTTP 请求。
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world?name=OpenWhisk
  ```

  ```json
  {
  "payload": "Hello world OpenWhisk"
  }
  ```

  这将调用 Web 操作 `hello`，返回的 JSON 对象包含通过查询参数发送的 `name` 参数。可以通过简单的查询参数或通过请求主体将参数传递到操作。Web 操作可以在没有 OpenWhisk 授权 API 密钥的情况下，以公共方式调用操作。
  
### 完全控制 HTTP 响应
  
  `--response-type` 标志用于控制将由 API 网关代理的 Web 操作的目标 URL。使用 `--response-type json` 将返回 JSON 格式的完整操作结果，并自动将 Content-Type 头设置为 `application/json`。 
  
  开始使用后，您希望对 HTTP 响应属性（如 `statusCode` 和 `headers`）具有完全控制权，并在 `body` 中返回不同的内容类型。为此，可以使用 `--response-type http` 标志来配置具有 `http` 扩展名的 Web 操作的目标 URL。

  您可以选择更改操作代码，以符合返回具有 `http` 扩展的 Web 操作的要求，或者将操作包含在序列中，将其结果传递给新操作。然后，新的操作可以对结果进行变换，以便对 HTTP 响应进行正确格式化。您可以在 [Web 操作](./openwhisk_webactions.html)文档中阅读有关响应类型和 Web 操作扩展的更多信息。

  更改 `hello.js` 的代码，以返回 JSON 属性 `body`、`statusCode` 和 `headers`
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
  请注意，body 需要返回的是 `base64` 编码的内容，而不是字符串。

  
  使用已修改的结果更新操作： 
  ```
  wsk action update hello hello.js --web true
  ```
  {: pre}

  使用 `--response-type http` 更新 API： 
  ```
  wsk api create /hello /world get hello --response-type http
  ```
  {: pre}
  
  调用已更新的 API： 
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  {: pre}

  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  现在您可以完全控制 API，可以控制内容（如返回 HTML）或设置状态码，例如“找不到”(404)、“未授权”(401) 甚或“内部错误”(500)。

### 公开多个 Web 操作

例如，如果要为读书俱乐部公开一组操作，可以使用一系列操作来实现该读书俱乐部的后端：

| 操作| HTTP 方法| 描述
|
| ----------- | ----------- | ------------ |
| getBooks| GET| 获取书籍详细信息|
| postBooks| POST| 添加书籍|
| putBooks| PUT| 更新书籍详细信息|
| deleteBooks| DELETE| 删除书籍|

为读书俱乐部创建名为 `Book Club` 的 API，并将 `/club` 作为其 HTTP URL 基本路径，将 `books` 作为其资源。
```
wsk api create -n "Book Club" /club /books get getBooks --response-type http
wsk api create /club /books post postBooks              --response-type http
wsk api create /club /books put putBooks                --response-type http
wsk api create /club /books delete deleteBooks          --response-type http
```

请注意，使用基本路径 `/club` 公开的第一个操作将获取名为 `Book Club` 的 API 标签。在 `/club` 下公开的其他所有操作都将与 `Book Club` 相关联。

使用以下命令来列出公开的所有操作：
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

为了获得乐趣，您可以使用 HTTP __POST__ 来添加书籍 `JavaScript: The Good Parts`：
```
curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": "success"
}
```

使用操作 `getBooks` 通过 HTTP __GET__ 获取书籍列表
```
curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
}
```

### 导出配置
将名为 `Book Club` 的 API 导出到文件，可以基于此文件将其用作输入来重新创建 API。 
```
wsk api get "Book Club" > club-swagger.json
```
{: pre}

通过先删除公共基本路径下所有公开的 URL 来测试 Swagger 文件。
可以使用基本路径 `/club` 或 API 名称标签“`Book Club`”来删除所有公开的 URL：
```
wsk api delete /club
```
```
ok: deleted API /club
```
### 更改配置

您可以在 OpenWhisk“仪表板”中编辑配置，单击 [API 选项卡](https://console.ng.bluemix.net/openwhisk/apimanagement)来设置安全性、速率限制和其他功能。

### 导入配置

现在，使用 `club-swagger.json` 文件复原名为 `Book Club` 的 API
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

验证是否已重新创建 API：
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
