---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: serverless, rest api, gateway, web actions, functions

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


# 创建无服务器 REST API
{: #apigateway}

您可以使用 API 来直接管理 {{site.data.keyword.openwhisk}} [Web 操作](/docs/openwhisk?topic=cloud-functions-actions_web)。
{: shortdesc}

基于 IAM 的名称空间不支持使用 API 网关创建 API。请改为使用基于 Cloud Foundry 的名称空间。
{: important}

## 为什么要将 REST API 用于 {{site.data.keyword.openwhisk_short}}？

您可以将 API 网关用作 Web 操作的代理。API 网关提供 HTTP 方法路由、客户机标识和密钥、速率限制和 CORS，并可查看 API 使用情况以及查看响应日志和 API 共享策略。

有关 API Management 的更多信息，可以阅读 [API Management 文档](/docs/api-management?topic=api-management-manage_openwhisk_apis)。

## 创建第一个 API
{: #api_create}

您必须在 Cloud Foundry 空间中具有 `SpaceDeveloper` 许可权才能创建 REST API。可以通过运行 `ibmcloud account space-roles <org>` 来查看空间许可权。
{: note}

开始之前，请安装 [{{site.data.keyword.openwhisk_short}} CLI 插件](/docs/openwhisk?topic=cloud-functions-cli_install)。

1. 将以下代码保存在名为 `hello.js` 的 JavaScript 文件中。
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello, ${name}!`};
  }
  ```
  {: codeblock}

2. 通过使用已创建的文件来创建名为 `hello` 的 Web 操作。确保添加 `--web true` 标志。将 `<filepath>` 替换为 `hello.js` 文件的文件路径。

  ```
  ibmcloud fn action create hello <filepath>/hello.js --web true
  ```
  {: pre}

  **示例输出**
  ```
  ok: created action hello
  ```
  {: screen}

3. 创建基本路径为 `/hello`、路径为 `/world`、方法为 `get` 且响应类型为 `json` 的 API。
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  **示例输出**
  这将生成新 URL，用于使用 `GET` HTTP 方法公开 `hello` 操作。

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  
4. 使用以下 cURL 命令向 URL 发送测试 HTTP 请求。
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=Jane
  ```
  {: pre}

  **示例输出**
  这将调用 Web 操作 `hello`，它返回的 JSON 对象包含查询参数中的 `name` 参数。可以通过简单的查询参数或使用请求主体将参数传递到操作。Web 操作可在不使用认证的情况下，公开调用操作。

  ```
  {
  "payload": "Hello, Jane!"
  }
  ```
  {: screen}



## 使用对 HTTP 响应的完全控制
{: #api_control}

`--response-type` 标志用于控制将由 API 网关代理的 Web 操作的目标 URL。例如，使用 `--response-type json` 标志时，将以 JSON 格式返回操作的完整结果，并且 `Content-Type` 头会自动设置为 `application/json`。

要在主体中返回其他内容类型，请使用对 HTTP 响应的完全控制属性，例如 `statusCode` 和 `headers`。可以使用 `--response-type http` 标志来配置具有 `http` 扩展名的 Web 操作的目标 URL。您可以更改操作的代码，以符合返回具有 `http` 扩展名的 Web 操作的要求，或者将操作包含在序列中，以将其结果传递给新操作。然后，新的操作可以对结果进行变换，以便正确设置其格式以用于 HTTP 响应。您可以在 [Web 操作](/docs/openwhisk?topic=cloud-functions-actions_web)文档中阅读有关响应类型和 Web 操作扩展名的更多信息。

1. 将以下代码保存为 `hello.js`。
  ```javascript
  function main({name:name='Serverless API'}) {
      return { 
    body: {payload:`Hello, ${name}!`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. 使用 `hello.js` 代码的新版本更新 `hello` Web 操作。
  ```
  ibmcloud fn action update hello <filepath>/hello.js --web true
  ```
  {: pre}

  **输出**
  ```
    ok: updated action hello
    ```
  {: screen}

3. 使用 `--response-type http` 标志更新 API 响应类型。
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

  **输出**
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

4. 使用以下 cURL 命令来调用更新后的 API。
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  **示例输出**
  ```
  {
  "payload": "Hello, Serverless API!"
  }
  ```
  {: screen}

## 修改配置
{: #api_modify_config}

创建配置后，可以使用 {{site.data.keyword.openwhisk_short}} 仪表板中的 [API 选项卡](https://cloud.ibm.com/openwhisk/apimanagement){: external}，通过以下方式来修改配置。

* [创建 {{site.data.keyword.openwhisk_short}} API](/docs/services/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis)，用于包装一组 {{site.data.keyword.openwhisk_short}} 操作。
* [保护 API](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis)，通过应用 API 安全性和速率限制策略来实现。
* [管理流量](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis)，通过查看 API 使用情况统计信息并查看响应日志来实现。
* [社交化和共享](/docs/services/api-management?topic=api-management-manage_apis#share_api_manage_apis)，与 {{site.data.keyword.cloud_notm}} 内部和外部的开发者社交化并共享您的 API。

</br>
更新配置完成后，可以下载 JSON 格式的定义文件，然后使用 CLI 将其重新导入。例如，对于持续集成和部署 (CICD) 管道中的无人照管部署，下载并导入配置会非常有用。您还可以使用 UI 来上传和重新导入 API 定义文件。



