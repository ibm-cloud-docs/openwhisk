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

# 创建无服务器 REST API
{: #openwhisk_apigateway}

通过引入 API 网关（充当 [Web 操作](./openwhisk_webactions.html)的代理并为这些操作提供更多功能），{{site.data.keyword.openwhisk}} 操作可受益于由 API 直接进行管理。这些额外的功能包括：HTTP 方法路由、客户机标识/私钥、速率限制、CORS、查看 API 使用情况和响应日志，以及 API 共享策略。
有关 API Management 的更多信息，可以阅读 [API Management 文档](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis)。
{: shortdesc}

## 使用浏览器通过 OpenWhisk Web 操作创建 API
{: #create_api_browser}

可以使用 [{{site.data.keyword.openwhisk_short}} 仪表板](https://console.bluemix.net/openwhisk/)中的 [**API 选项卡**](https://console.bluemix.net/openwhisk/apimanagement)来执行以下任务：

* [创建 Cloud Functions API](https://console.bluemix.net/openwhisk/apimanagement) - 创建用于包装一组 OpenWhisk 操作的 API。
* [保护 API](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - 应用 API 安全性和速率限制策略来保护 API。
* [管理流量](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - 查看 API 使用情况统计信息并查看响应日志。
* [社交化和共享](https://console.bluemix.net/docs/apis/management/manage_apis.html#share_api) - 与 {{site.data.keyword.Bluemix_notm}} 内部和外部开发者共享 API。

## 使用 CLI 插件通过 OpenWhisk Web 操作创建 API
{: #create_api_cli}

以下部分将逐步指导您使用 {{site.data.keyword.openwhisk_short}} CLI 插件完成 API Management 任务。要通过 CLI 创建和管理 API，必须首先为 {{site.data.keyword.Bluemix_notm}} 安装 [{{site.data.keyword.openwhisk_short}} CLI 插件](https://console.bluemix.net/docs/openwhisk/bluemix_cli.html)。

为了方便起见，步骤将分解为较小的子主题，您可以使用以下 API 任务列表快速跳至这些子主题：

* [创建第一个 API](openwhisk_apigateway.html#create_cli_api)
* [完全控制 HTTP 响应](openwhisk_apigateway.html#full_control)
* [公开多个 Web 操作](openwhisk_apigateway.html#multiple_web_actions)
* [导出配置](openwhisk_apigateway.html#export_config)
* [导入配置](openwhisk_apigateway.html#import_config)
* [修改配置](openwhisk_apigateway.html#modify_config)

### 使用 CLI 创建第一个 API
{: #create_cli_api}

1. 创建名为 **hello.js** 的 JavaScript 文件，该文件包含以下内容：
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. 使用在步骤 1 中创建的 `hello.js` 文件创建名为 **hello** 的 Web 操作。**注：**请确保添加 `--web true` 标志。
  ```
  ibmcloud wsk action create hello hello.js --web true
  ```
  {: pre}

  示例输出：
  ```
  ok: created action hello
  ```
  {: screen}

3. 创建基本路径为 `/hello`、路径为 `/world`、方法为 `get` 且响应类型为 `json` 的 API：
  ```
  ibmcloud wsk api create /hello /world get hello --response-type json
  ```
  {: pre}

  示例输出：
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  这将生成新 URL，用于使用 __GET__ HTTP 方法公开 `hello` 操作。

4. 最后，使用 **curl** 命令向 URL 发送 HTTP 请求：
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  示例输出：
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

这将调用 Web 操作 **hello**，返回的 JSON 对象包含通过查询参数发送的 **name** 参数。可以通过简单的查询参数或使用请求主体将参数传递到操作。Web 操作可以在不使用 OpenWhisk 授权 API 密钥的情况下，以公共方式调用操作。

### 完全控制 HTTP 响应
{: #full_control}

`--response-type` 标志用于控制将由 API 网关代理的 Web 操作的目标 URL。使用 `--response-type json` 将返回 JSON 格式的完整操作结果，并自动将 Content-Type 头设置为 `application/json`。

您希望对 HTTP 响应属性（如 `statusCode` 和 `headers`）具有完全控制权，以便可以在 `body` 中返回不同的内容类型。为此，可以使用 `--response-type http` 标志通过配置具有 `http` 扩展名的 Web 操作的目标 URL 来实现。

您可以选择更改操作码，以符合返回具有 `http` 扩展名的 Web 操作的要求，或者将操作包含在序列中，以将其结果传递给新操作。然后，新的操作可以对结果进行变换，以便正确设置其格式以用于 HTTP 响应。您可以在 [Web 操作](./openwhisk_webactions.html)文档中阅读有关响应类型和 Web 操作扩展名的更多信息。

1. 更改 `hello.js` 的代码，以返回 JSON 属性 `body`、`statusCode` 和 `headers`：
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

2. 使用已修改的结果更新操作：
  ```
  ibmcloud wsk action update hello hello.js --web true
  ```
  {: pre}

3. 使用 `--response-type http` 标志更新 API 响应类型：
  ```
  ibmcloud wsk api create /hello /world get hello --response-type http
  ```
  {: pre}

4. 使用以下 **curl** 命令来调用更新后的 API：
  ```bash
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  示例输出：
  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  {: screen}

  现在您可以完全控制 API，可以控制内容（如返回 HTML）或设置状态码，例如“找不到”(404)、“未授权”(401) 甚或“内部错误”(500)。

### 公开多个 Web 操作
{: #multiple_web_actions}

例如，如果要为读书俱乐部公开一组操作，可以使用一系列操作来实现该读书俱乐部的后端：

|操作|HTTP 方法|描述
|
| ----------- | ----------- | ------------ |
|getBooks|GET|获取书籍详细信息|
|postBooks|POST|添加书籍|
|putBooks|PUT|更新书籍详细信息|
|deleteBooks|DELETE|删除书籍|

在此示例中，API 是使用**路径参数**定义的。使用路径参数时，必须使用响应类型 `http` 来定义 API。路径值（以基本路径开头，并包括实际路径参数值）在操作的 JSON 参数的 `__ow_path` 字段中提供。请参阅 [Web 操作 HTTP 上下文](./openwhisk_webactions.html#http-context)文档，以获取更多详细信息，包括有关可用于使用 `http` 响应类型调用的 Web 操作的更多 HTTP 上下文字段的信息。

1. 为读书俱乐部创建名为 **Book Club** 的 API，并将 `/club` 作为其 HTTP URL 基本路径，将 `books` 作为其资源，将 `{isbn}` 作为路径参数，用于通过国际标准书号 (ISBN) 来标识特定书籍。
  ```bash
  ibmcloud wsk api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud wsk api create /club /books get getBooks                       --response-type http
  ibmcloud wsk api create /club /books post postBooks                     --response-type http
  ibmcloud wsk api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud wsk api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: codeblock}

  请注意，使用基本路径 `/club` 公开的第一个操作将获取名为 **Book Club** 的 API 标签。在 `/club` 下公开的其他所有操作现在都将与 **Book Club** 相关联。

2. 使用以下命令来列出公开的所有 **Book Club** 操作：
  ```
  ibmcloud wsk api list /club -f
  ```
  {: pre}

  示例输出：
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

3. 为了获得乐趣，您可以使用 HTTP __POST__ 来添加名为 **JavaScript: The Good Parts** 的书籍：
  ```
  curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' -H "Content-Type: application/json" https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  示例输出：
  ```
  {
    "result": "success"
  }
  ```
  {: screen}

4. 使用 **getBooks** 操作通过 HTTP __GET__ 获取书籍列表：
  ```bash
  curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  示例输出：
  ```
  {
    "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
  }
  ```
  {: screen}

5. 可以使用 **deleteBooks** 操作通过 HTTP __DELETE__ 来删除特定书籍。在此示例中，**deleteBooks** 操作的 `__ow_path` 字段值为 `/club/books/978-0596517748`，其中 `978-0596517748` 是路径的 `{isbn}` 实际值。
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

### 导出配置
{: #export_config}

1. 将名为 **Book Club** 的 API 导出到文件，可以基于此文件将其用作输入来重新创建 API。
  ```
  ibmcloud wsk api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. 使用以下命令通过先删除公共基本路径下所有公开的 URL 来测试 Swagger 文件：
  ```
  ibmcloud wsk api delete /club
  ```
  {: pre}

  示例输出：
  ```
ok: deleted API /club
```
  {: screen}

  可以使用基本路径 `/club` 或 API 名称标签“**Book Club**”来删除所有公开的 URL：
  {: tip}

### 导入配置
{: #import_config}

1. 现在，使用 **club-swagger.json** 文件名复原名为 `Book Club` 的 API：
  ```
  ibmcloud wsk api create --config-file club-swagger.json
  ```
  {: pre}

  示例输出：
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

2. 验证是否已重新创建 **Book Club** API：
  ```
  ibmcloud wsk api list /club
  ```
  {: pre}

  示例输出：
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

### 使用 UI 修改配置
{: #modify_config}

可以在 {{site.data.keyword.openwhisk_short}}“仪表板”中编辑配置，单击 [API 选项卡](https://console.ng.bluemix.net/openwhisk/apimanagement)来设置安全性、速率限制和其他功能。完成更新配置后，可以下载 JSON 格式的定义文件，然后使用 CLI 将其重新导入。例如，对于持续集成和部署 (CICD) 管道中的无人照管部署，这可能非常有用。您还可以选择使用 UI 来上传和重新导入 API 定义文件。
