---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-04"

keywords: serverless, rest api, gateway, web actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 创建无服务器 REST API
{: #openwhisk_apigateway}

使用 API 来直接管理 {{site.data.keyword.openwhisk}} 操作。API 网关充当 [Web 操作](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions)的代理，并提供 HTTP 方法路由、客户机标识和密钥、速率限制和 CORS，并可查看 API 使用情况以及查看响应日志和 API 共享策略。
{: shortdesc}

有关 API Management 的更多信息，可以阅读 [API Management 文档](/docs/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis)。




## 创建第一个 API
{: #create_cli_api}

开始之前，请安装 [{{site.data.keyword.openwhisk_short}} CLI 插件](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli)。

1. 将以下代码保存在名为 `hello.js` 的 JavaScript 文件中。
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. 使用创建的文件创建名为 `hello` 的 Web 操作。**注：**请确保添加 `--web true` 标志。
  ```
  ibmcloud fn action create hello hello.js --web true
  ```
  {: pre}

  示例输出：
  ```
  ok: created action hello
  ```
  {: screen}

3. 创建基本路径为 `/hello`、路径为 `/world`、方法为 `get` 且响应类型为 `json` 的 API。
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  示例输出：
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  这将生成新 URL，用于使用 GET HTTP 方法公开 `hello` 操作。

4. 使用 cURL 命令向 URL 发送测试 HTTP 请求。
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  示例输出：
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

这将调用 Web 操作 `hello`，它返回的 JSON 对象包含查询参数中的 **name** 参数。可以通过简单的查询参数或使用请求主体将参数传递到操作。Web 操作可在不使用认证的情况下，公开调用操作。

## 使用对 HTTP 响应的完全控制
{: #full_control}

`--response-type` 标志用于控制将由 API 网关代理的 Web 操作的目标 URL。例如，使用 `--response-type json` 标志时，将以 JSON 格式返回操作的完整结果，并且 **Content-Type** 头会自动设置为 `application/json`。

要在主体中返回其他内容类型，请使用对 HTTP 响应的完全控制属性，例如 **statusCode** 和 **headers**。可以使用 `--response-type http` 标志来配置具有 `http` 扩展名的 Web 操作的目标 URL。您可以更改操作的代码，以符合返回具有 `http` 扩展名的 Web 操作的要求，或者将操作包含在序列中，以将其结果传递给新操作。然后，新的操作可以对结果进行变换，以便正确设置其格式以用于 HTTP 响应。您可以在 [Web 操作](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions)文档中阅读有关响应类型和 Web 操作扩展名的更多信息。

1. 更改 `hello.js` 操作的代码，以返回 JSON 属性 `body`、`statusCode` 和 `headers`。
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

2. 使用已修改的结果更新操作。
  ```
  ibmcloud fn action update hello hello.js --web true
  ```
  {: pre}

3. 使用 `--response-type http` 标志更新 API 响应类型。
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

4. 使用以下 cURL 命令来调用更新后的 API。
  ```
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

## 公开多个 Web 操作
{: #multiple_web_actions}

您可以公开多个 Web 操作来实现应用程序后端。例如，要为读书俱乐部公开一组操作，可以使用一系列操作来实现该读书俱乐部的后端：

|操作|HTTP 方法|描述
|
| ----------- | ----------- | ------------ |
|getBooks|GET|获取书籍详细信息|
|postBooks|POST|添加书籍|
|putBooks|PUT|更新书籍详细信息|
|deleteBooks|DELETE|删除书籍|

在此示例中，API 是使用路径参数定义的。使用路径参数时，必须使用响应类型 `http` 来定义 API。路径值（以基本路径开头，并包括实际路径参数值）在操作的 JSON 参数的 `__ow_path` 字段中提供。有关 HTTP 上下文字段的更多详细信息，请参阅 [Web 操作 HTTP 上下文](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#http-context)文档。

要试用此读书俱乐部 Web 操作示例，请执行以下操作：

1. 为读书俱乐部创建名为 `Book Club` 的 API，并将 `/club` 作为其 HTTP URL 基本路径，将 `books` 作为其资源，将 `{isbn}` 作为路径参数，用于通过国际标准书号 (ISBN) 来标识特定书籍。
  ```
  ibmcloud fn api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud fn api create /club /books get getBooks                       --response-type http
  ibmcloud fn api create /club /books post postBooks                     --response-type http
  ibmcloud fn api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud fn api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: pre}

  使用基本路径 `/club` 公开的第一个操作将标注有名称 `Book Club`。在 `/club` 下公开的其他所有操作现在都将与 `Book Club` 相关联。

2. 列出公开的所有 `Book Club` 操作。
  ```
  ibmcloud fn api list /club -f
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

3. 使用 HTTP POST 来添加名为 `JavaScript: The Good Parts` 的书籍。
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

4. 使用对 `getBooks` 操作的 HTTP GET 调用来获取书籍列表。
  ```
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

5. 使用对 `deleteBooks` 操作的 HTTP DELETE 调用来删除特定书籍。在此示例中，`deleteBooks` 操作的 `__ow_path` 字段值为 `/club/books/978-0596517748`，其中 `978-0596517748` 是路径的 `{isbn}` 实际值。
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

## 导出和导入配置
{: #export_import_config}

要导出或导入配置，可以继续使用读书俱乐部示例。

1. 将 `Book Club` API 导出到名为 `club-swagger.json` 的文件中。可以基于此文件将其用作输入来重新创建 API。
  ```
  ibmcloud fn api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. 通过先删除公共基本路径下所有公开的 URL 来测试 Swagger 文件。

  ```
  ibmcloud fn api delete /club
  ```
  {: pre}

  示例输出：
  ```
ok: deleted API /club
```
  {: screen}

  可以使用基本路径 `/club` 或 API 名称标签“`Book Club`”来删除所有公开的 URL。
  {: tip}

3. 使用 `club-swagger.json` 文件来复原 `Book Club` API。
  ```
  ibmcloud fn api create --config-file club-swagger.json
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

4. 验证是否已重新创建 `Book Club` API。
  ```
  ibmcloud fn api list /club
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

## 修改配置
{: #modify_config}

创建配置后，可以使用 {{site.data.keyword.openwhisk_short}} 仪表板中的 [**API 选项卡**](https://cloud.ibm.com/openwhisk/apimanagement)，通过以下方式来修改配置。

* [创建 {{site.data.keyword.openwhisk_short}} API](https://cloud.ibm.com/docs/services/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis)，用于包装一组 {{site.data.keyword.openwhisk_short}} 操作。
* [保护 API](https://cloud.ibm.com/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis)，通过应用 API 安全性和速率限制策略来实现。
* [管理流量](https://cloud.ibm.com/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis)，通过查看 API 使用情况统计信息并查看响应日志来实现。
* [社交化和共享](https://cloud.ibm.com/docs/services/api-management?topic=api-management-manage_apis#share_api_manage_apis)，与 {{site.data.keyword.Bluemix_notm}} 内部和外部的开发者社交化并共享您的 API。

完成更新配置后，可以下载 JSON 格式的定义文件，然后使用 CLI 将其重新导入。例如，对于持续集成和部署 (CICD) 管道中的无人照管部署，下载并导入配置会非常有用。您还可以选择使用 UI 来上传和重新导入 API 定义文件。
