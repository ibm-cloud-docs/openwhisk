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


# 创建 Web 操作
{: #actions_web}

创建 Web 操作时，获得的结果是可用于从任何 Web 应用程序触发操作的 URL。
{: shortdesc}

## 为什么要使用 Web 操作而不使用标准操作？

### 1. 以匿名方式运行 Web 操作

Web 操作激活与创建操作的用户（而不是与操作的调用者）相关联。通常，对于向 Github 等应用程序发出的 API 调用，将在 API 调用中包含用户名和令牌，以表示特定用户或功能标识。使用 Web 操作时，无需这些类型的凭证。Web 操作可通过 REST 接口进行访问，无需凭证。

虽然无需将凭证与 Web 操作配合使用，但您可以实现自己的认证和授权，即 OAuth 流程。要将 Web 操作配置为使用凭证，请参阅[保护 Web 操作](#actions_web_secure)。

### 2. 使用任何类型的 HTTP 请求

缺省情况下，操作仅接受 `POST` 请求，但 Web 操作可通过 HTTP 方法 `GET`、`POST`、`PUT`、`PATCH` 和 `DELETE` 中的任一种方法以及 `HEAD` 和 `OPTIONS` 进行调用。

### 3. 从任何位置触发 Web 操作

创建 {{site.data.keyword.openwhisk}} Web 操作时，会生成 URL，用于从任何基于 Web 的应用程序调用该操作。非 Web 操作需要认证，并且必须使用 JSON 对象进行响应。 

Web 操作 API 路径可用于 cURL 或 `wget`，甚至可以直接在浏览器中输入。可以使用如下所示构造的 URL 来调用 Web 操作：`https://<apihost>/api/v1/web/<namespace>/<packageName>/<actionName>.<ext>`。

### 4. 创建较少的 {{site.data.keyword.openwhisk_short}} 实体

由于可以从任何位置调用 Web 操作，因此无需创建其他 {{site.data.keyword.openwhisk_short}} 实体，如触发器或规则。

## Web 操作是如何运作的？

Web 操作可以在无需认证的情况下进行调用，也可用于实现通过不同类型的 `headers`、`statusCode` 和 `body` 内容进行响应的 HTTP 处理程序。


Web 操作必须返回 JSON 对象。但是，如果 Web 操作的结果包含以下一个或多个属性作为顶级 [JSON 属性](#web_action_properties)，那么控制器将以不同的方式处理 Web 操作。
{: shortdesc}

## Web 操作的可用功能
{: #actions_web_extra}

Web 操作支持以下功能：

|功能|描述|
| --- | --- |
|[内容扩展名](#extra_features)|可以为 HTTP 请求指定内容类型，例如 `.json`、`.html`、`.http`、`.svg` 或 `.text`。如果未指定内容类型，将采用 `.http` 扩展名。可以通过将扩展名添加到 URI 中的操作名称来指定内容类型，以便将 `demo/hello` 操作作为 `/demo/hello.svg` 引用。 `.json` 和 `.http` 扩展名无需投影路径，而 `.html`、`.svg` 和 `.text` 扩展名则需要投影路径。将采用缺省路径来与扩展名相匹配。要调用 Web 操作并接收 `.html` 响应，该操作必须使用包含顶级属性 `html` 的 JSON 对象进行响应（或者响应必须位于显式路径中）。换言之，`/<namespace>/demo/hello.html` 相当于显式对 `html` 属性进行投影，如 `/<namespace>/demo/hello.html/html` 中一样。操作的标准名称必须包含其包名，如果操作不在指定的包中，那么包名为 `default`。|
|[对结果中的字段投影](#projecting_fields)|操作名称后跟的路径用于投影到响应的一个或多个级别。例如，`/demo/hello.html/body`。此功能允许返回字典 `{body: "..." }` 的任何 Web 操作对 `body` 属性进行投影，并直接返回其字符串值，而不是其字典值。投影的路径将采用绝对路径模型（如在 XPath 中一样）。|
|[查询和主体参数作为输入](#query_test)|操作会接收查询参数以及请求主体中的参数。合并参数的优先顺序如下：包参数、操作参数、查询参数、主体参数。如果发生重叠，这些参数都会覆盖任何先前的值。例如，`/demo/hello.http?name=Jane` 可以将自变量 `{name: "Jane"}` 传递到操作。|
|[表单数据](#form_data)|除了标准 `application/json` 外，Web 操作还可以接收 URL 编码的表单数据 `application/x-www-form-urlencoded data` 作为输入。
|[通过多个 HTTP 动词激活](#actions_web_options)|Web 操作可通过 HTTP 方法 `GET`、`POST`、`PUT`、`PATCH` 和 `DELETE` 中的任一种方法以及 `HEAD` 和 `OPTIONS` 进行调用。|
|[非 JSON 主体和原始 HTTP 实体处理](#actions_web_raw_enable)|Web 操作可接受非 JSON 对象的 HTTP 请求主体，并可选择始终接收此类值作为不透明值（不是二进制文件时为明文，其他情况均为 Base64 编码的字符串）。|

## 创建 Web 操作
{: #actions_web_example}

要创建 Web 操作，请执行以下操作： 

1. 将以下 JavaScript 代码保存为 `hello.js`。

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

2. 创建 `demo` 包。除非显式指定包名，否则包名为 `default`。
  ```
ibmcloud fn package create demo
```
  {: pre}

3. 创建 `hello` 操作。
    在此示例中，`packageName/actionName` 为 `demo/hello`。将 `<filepath>` 变量替换为 `hello.js` 文件的文件路径，并将 `--web` 标志设置为 `true`。 

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

4. 在不使用任何参数的情况下调用或测试 `hello` Web 操作。替换 `<apihost>` 和 `<namespace>` 变量。要获取 `<apihost>`，请运行 `ibmcloud fn property get --apihost`。示例 `<apihost>`：`us-south.functions.cloud.ibm.com`。

  对于启用 IAM 的名称空间，请将 `<namespace>` 变量替换为名称空间标识。要获取该标识，请运行 `ibmcloud fn namespace get <namespace_name>`。
  {: note}

  a. 可以通过以下任一方式来测试 Web 操作： 
    * 在浏览器中使用以下结构打开 URL：`https://<apihost>/api/v1/web/<namespace>/demo/hello`。
    * 使用 cURL 命令测试操作。
      ```
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

    * 使用 `wget` 命令测试操作。  
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

  b. 操作代码会返回以下字典。
    ```
    {body: `<html><body><h3>${msg}</h3></body></html>`}
    ``` 
    {: screen}
    
  还可以通过使用以下命令仅返回 `body` 属性来测试操作：
  {: #projecting_fields}

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello.html/body
    ```
    {: pre}

    **示例输出**

    由于未指定 `<name>` 参数，因此会返回以下消息。
    ```
    <html><body><h3>You did not tell me who you are.</h3></body></html>
    ```
    {: screen}

5. 现在，请尝试定义 `<name>` 参数。通过以下任一方式来测试使用 `<name>` 参数的操作：
  * 在浏览器中打开 `https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane`。 
  * 使用 cURL 命令测试操作。

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}
  * 使用 `wget` 命令测试操作。  
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}

  **示例输出**
  ```
  <html><body><h3>Hello, Jane!</h3></body></html>
  ```
  {: screen}


**后续步骤**

将 `hello` Web 操作的 URL 添加到 Web 应用程序，并在其中对其进行测试。

### Web 操作 JSON 属性
{: #web_action_properties}

HTTP 响应的缺省 `Content-Type` 为 `application/json`，并且 body 可以是允许的任何 JSON 值。如果 `Content-Type` 不是 `application/json`，那么必须在操作代码的 `headers` 中指定 `Content-Type`。

如果达到操作的[结果大小限制](/docs/openwhisk?topic=cloud-functions-limits)，那么响应会失败。如果您知道操作结果大于 5 MB，请设置[对象存储](/docs/openwhisk?topic=cloud-functions-pkg_obstorage)。

|JSON 属性|描述|
| --- | --- |
|`headers`|JSON 对象，其中键是头名称，值是字符串值、数字值或布尔值。要为单个头发送多个值，该头的值为 JSON 多值数组。缺省情况下，不会设置任何头。|
|`statusCode`|有效的 HTTP 状态码。如果存在 body 内容，那么缺省值为 `200 OK`。如果不存在 body 内容，那么缺省值为 `204 No Content`。|
|`body`|字符串，为明文（JSON 对象或数组），或者为 Base64 编码的字符串（对于二进制数据）。如果 body 为 `null`、空字符串 `""` 或未定义，那么会将 body 视为空。缺省值为空主体。|

[控制器](/docs/openwhisk?topic=cloud-functions-about#about_controller)将任何操作指定的头、状态码或主体传递到 HTTP 客户机，后者将终止请求或响应。如果操作结果的 `headers` 中未声明 `Content-Type` 头，那么 body 将解释为 `application/json`（表示非字符串值），否则将解释为 `text/html`。 如果定义了 `Content-Type` 头，控制器将确定响应是二进制数据还是明文，然后根据需要使用 Base64 解码器对字符串进行解码。如果 body 未能正确解码，那么会向客户机返回错误。

Web 操作的所有者拥有所有激活记录，并且会产生在系统中运行该操作的开销，无论该操作的调用方式如何。
{: note}

#### 受保护的参数
操作参数受到保护，只能通过更新操作进行更改。参数会自动完成，以启用 Web 操作。

```
ibmcloud fn action create /<namespace>/demo/hello hello.js --parameter name Jane --web true
```
{: pre}


这些更改的结果是 `name` 绑定到 `Jane`，并且因为是最终注释，所以查询或主体参数都无法将其覆盖。此设计将保护操作不被意外或有意尝试更改此值的查询或主体参数覆盖。

### 使用 Web 操作执行 HTTP 重定向
{: #http_redirect}
您可以在 Web 应用程序中使用此功能来将用户重定向到站点的新版本。

**开始之前**
通过完成[创建 Web 操作](#actions_web_example)中的步骤来创建 `demo` 包和 `hello` Web 操作。

要创建用于执行 HTTP 重定向的 Web 操作，请执行以下操作：

1. 将以下代码保存为 `hello.js`。

  ```javascript
function main() {
  return {
      headers: { location: 'https://cloud.ibm.com/openwhisk/' },
      statusCode: 302
    }
  }
  ```
  {: codeblock}

2. 使用 `hello.js` 代码的新版本更新 `hello` Web 操作。将 `<filepath>` 替换为 `hello.js` 文件的文件路径。

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. 测试 `hello` Web 操作。替换 `<apihost>` 和 `<namespace>` 变量。可以通过以下任一方式来测试 Web 操作：

  * 在浏览器中打开 URL：`https://<apihost>/api/v1/web/<namespace>/demo/hello`。 
  * 运行以下 cURL 命令：
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * 运行以下 `wget` 命令：
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

  **示例结果** 
  
  此示例 Web 操作将浏览器重定向到 [{{site.data.keyword.openwhisk_short}} 仪表板](https://cloud.ibm.com/openwhisk/){: external}。

### 使用 Web 操作设置 cookie
{: #multiple_cookie}
成功登录后，您可以在 Web 应用程序中使用此功能将 JSON Web 令牌存储为会话 cookie。

要创建用于设置多个 cookie 的 Web 操作，请执行以下操作：

**开始之前**
通过完成[创建 Web 操作](#actions_web_example)中的步骤来创建 `demo` 包和 `hello` Web 操作。

1. 将以下代码保存为 `hello.js`。
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

2. 使用 `hello.js` 代码的新版本更新 `hello` Web 操作。将 `<filepath>` 替换为 `hello.js` 文件的文件路径。

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}
    
3. 测试操作之前，请清除浏览器的 cookie。

4. 通过在浏览器中打开 URL 来测试 `hello` Web 操作。替换 `<apihost>` 和 `<namespace>` 变量并打开 `https://<apihost>/api/v1/web/<namespace>/demo/hello`。示例 `<apihost>`：`us-south.functions.cloud.ibm.com`。

**结果**

cookie `UserID=Jane` 和 `SessionID=asdfgh123456` 已在浏览器的开发者工具中进行设置。


### 使用 Web 操作返回图像
{: #return_image}
您可以在 Web 应用程序中使用此功能来基于用户语言环境返回国家或地区标志的图像。

**开始之前** 

通过完成[创建 Web 操作](#actions_web_example)中的步骤来创建 `demo` 包和 `hello` Web 操作。

要创建用于返回 `image/png` 的 Web 操作，请执行以下操作： 

1. 将以下代码保存为 `hello.js`。

  ```javascript
  function main() {
    let png = '<base 64 encoded string';
      return { headers: { 'Content-Type': 'image/png' },
              statusCode: 200,
              body: png };
  }
  ```
  {: codeblock}

2. 使用 `hello.js` 代码的新版本更新 `hello` Web 操作。将 `<filepath>` 替换为 `hello.js` 文件的文件路径。

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. 在浏览器中或使用 cURL 命令来测试操作。替换 `<apihost>` 和 `<namespace>` 变量。可以通过以下任一方式来测试 Web 操作：

  * 在浏览器中打开 URL：`https://<apihost>/api/v1/web/<namespace>/demo/hello`。 
  * 运行以下 cURL 命令。
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * 运行以下 `wget` 命令。
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}



### 使用 Web 操作返回 JSON
{: #return_json}
您可以在 Web 应用程序中使用此功能来返回用户 IP 信息的 JSON 对象。

**开始之前** 

通过完成[创建 Web 操作](#actions_web_example)中的步骤来创建 `demo` 包和 `hello` Web 操作。

要创建用于返回 `application/json` 的 Web 操作，请执行以下操作：

1. 将以下代码保存为 `hello.js`。
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

2. 使用 `hello.js` 代码的新版本更新 `hello` Web 操作。将 `<filepath>` 替换为 `hello.js` 文件的文件路径。
  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. 在浏览器中或使用 cURL 命令来测试操作。替换 `<apihost>` 和 `<namespace>` 变量。可以通过以下任一方式来测试 Web 操作：
  * 在浏览器中打开 URL：`https://<apihost>/api/v1/web/<namespace>/demo/hello`。 
  * 运行以下 cURL 命令：
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * 运行以下 `wget` 命令：
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

    **示例输出**

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


### HTTP 上下文
{: #actions_web_context}

所有 Web 操作在调用时都会接收 HTTP 请求详细信息以作为操作自变量的输入参数。

|HTTP 参数|类型|描述|
| --- | --- | --- |
|`__ow_method`|字符串|请求的 HTTP 方法。|
|`__ow_headers`|将字符串映射到字符串|请求头。|
|`__ow_path`|字符串|请求的未匹配路径（使用操作扩展名后，匹配会停止）。|
|`__ow_user`|字符串|用于标识经 {{site.data.keyword.openwhisk_short}} 认证的主题的名称空间。|
|`__ow_body`|字符串|请求主体实体，内容为二进制文件时为 Base64 编码的字符串，其他情况均为明文字符串。|
|`__ow_query`|字符串|请求中的查询参数（以未解析字符串的形式提供）。|

请求不得覆盖任何指定的 `__ow_` 参数。加以覆盖会导致请求失败，并返回状态“400 错误请求”。

仅当 Web 操作[注释为需要认证](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions)并允许 Web 操作实现其自己的授权策略时，才会显示 `__ow_user`。仅当 Web 操作选择处理[“原始”HTTP 请求](#actions_web_raw_enable)时，`__ow_query` 才可用。`__ow_query` 是包含从 URI 中解析出的查询参数（用 `&` 分隔）的字符串。在原始 HTTP 请求中，或者当 HTTP 请求实体不是 JSON 对象或表单数据时，会显示 `__ow_body` 属性。在其他情况下，Web 操作接收查询和主体参数作为操作自变量中的第一类属性。主体参数优先于查询参数，而查询参数又优先于操作和包参数。

### HTTPS 端点支持
{: #actions_web_endpoint}

支持的 SSL 协议：TLS 1.2 和 TLS 1.3（[草稿版本 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18){: external}）

### 改变 Web 操作的响应内容
{: #extra_features}
您可以使用[内容扩展名](#actions_web_extra)来更改 Web 操作的响应内容，以返回不同的内容类型。
{: shortdesc}

**开始之前**

通过完成[创建 Web 操作](#actions_web_example)中的步骤来创建 `demo` 包和 `hello` Web 操作。

要改变响应 Web 操作，请执行以下操作：

1. 将以下代码保存为 `hello.js`。

  ```javascript
  function main(params) {
      return { response: params };
}
```
  {: codeblock}

2. 使用 `hello.js` 代码的新版本更新 `hello` Web 操作。将 `<filepath>` 替换为 `hello.js` 文件的文件路径。

  ```bash
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. 在浏览器中或使用 cURL 命令来测试操作。替换 `<apihost>` 和 `<namespace>` 变量。

  a. 通过以下任一方式来返回 JSON：
    * 在 Web 浏览器中打开 `https://<apihost>/api/v1/web/<namespace>/demo/hello.json`。 
    * 运行以下 cURL 命令。
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}
    * 运行以下 `wget` 命令。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}

      **示例输出**

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

  b. 使用查询参数来测试操作。可以通过以下任一方式来测试操作：
  {: #query_test}

    * 运行以下 cURL 命令。

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

    * 运行以下 `wget` 命令。

        ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

      **示例输出**
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

  c. 您还可以使用表单数据来测试 Web 操作。可以通过以下任一方式来测试 Web 操作：
  {: #form_data}
  
    * 运行以下 cURL 命令。

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}
      
    * 运行以下 `wget` 命令。
      ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}

      **示例输出**

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

  d. 可以通过运行以下命令来指定 JSON 对象。可以通过以下任一方式来测试 Web 操作：
    * 运行以下 cURL 命令。
      ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}
      
    * 运行以下 `wget` 命令。
      ```
        wget https://<apihost>/api/v1/web/{namespace/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}

      **示例输出**

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

  e. 还可以通过以下任一方式将 `name` 值作为文本返回：
  * 运行以下 cURL 命令。

      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}
  * 运行以下 `wget` 命令。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}

    **示例输出**

    ```
Jane
```
    {: screen}

    在标准操作中，查询参数、表单数据和 JSON 对象主体实体均视为字典，其值可以直接作为操作输入属性进行访问。但对于处理 HTTP 请求实体的 Web 操作，或者当 Web 操作接收到非 JSON 对象的实体时，此行为就不适用了。
    {: note}

  f. 可以通过以下任一方式来设置 `Content-Type`：
  * 运行以下 cURL 命令。  
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}
    
  * 运行以下 `wget` 命令。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}

    **示例输出**

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

## 保护 Web 操作
{: #actions_web_secure}

**开始之前**
通过完成[创建 Web 操作](#actions_web_example)中的步骤来创建 `demo` 包和 `hello` Web 操作。

缺省情况下，任何人都可以使用调用 URL 来调用 Web 操作。可以通过以下任一方式使用 `require-whisk-auth` [Web 操作注释](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions)来保护 Web 操作：
  1. 将 `require-whisk-auth` 注释设置为 `true`。`require-whisk-auth` 注释设置为 `true` 时，Web 操作将根据 Web 操作所有者的 Whisk 认证密钥来认证调用请求的基本授权凭证。设置为数字或区分大小写的字符串时，Web 操作的调用请求必须包含设置为此相同数字或区分大小写的字符串的 `X-Require-Whisk-Auth` 头。受保护的 Web 操作在凭证验证失败时，将返回消息`未授权`。

  2. 允许使用 `--web-secure` 标志自动设置 `require-whisk-auth` 注释。将 `--web-secure` 标志设置为 `true` 时，会生成随机数作为 `require-whisk-auth` 注释值。设置为 `false` 时，将除去 `require-whisk-auth` 注释。设置为其他任何值时，该值将用作 `require-whisk-auth` 注释值。

要测试安全 Web 操作，请执行以下操作：

1. 将以下 JavaScript 代码保存为 `hello.js`。
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

2. 使用 `hello.js` 代码的新版本更新 `hello` Web 操作，并将 `--web secure` 标志设置为 `true`。
  ```bash
  ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true
  ```
  {: pre}

3. 获取 `hello` Web 操作以查看随机生成的 `require-whisk-auth` 值。

  ```bash
  ibmcloud fn action get demo/hello
  ```
  {: pre}

    **示例输出**

    `require-whisk-auth` 值已设置为 `7819991076995522`。
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

要测试认证是否有效，请执行以下操作：

1. 在不设置 `X-Require-Whisk-Auth` 参数的情况下测试 `hello` Web 操作，以验证是否需要认证。此测试将生成错误。可以通过以下任一方式来测试 Web 操作：

  * 使用 cURL 命令测试 Web 操作。
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}
    
  * 使用 `wget` 命令测试 Web 操作。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}

   **示例输出**

    ```
      {
      "code": "4c4423556547f6ac764ee192d4ed27a6",
      "error": "Authentication is possible but has failed or not yet been provided."
    }
    ```
    {: screen}

    调用失败，因为未提供 `X-Require-Whisk-Auth` 值。
    {: note}

2. 现在，测试 `hello` Web 操作并提供随机生成的 `X-Require-Whisk-Auth` 值。替换 `<apihost>` 和 `<namespace>` 值。将 `<my-secret>` 值替换为在步骤 3 中创建的随机生成的数字。可以通过以下任一方式来测试 Web 操作：
  * 使用 cURL 命令测试 Web 操作。
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  * 使用 `wget` 命令测试 Web 操作。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  **示例输出**
    
    ```
    {
    "body": "<html><body><h3>Hello, Jane!</h3></body></html>"
    }
    ```
    {: screen}

要使用定制 `require-whisk-auth` 值来测试 Web 操作，请执行以下操作：

1. 使用您自己的 `require-whisk-auth` 值更新 `hello` Web 操作。然后，通过在调用期间指定 `X-Require-Whisk-Auth` 值来尝试测试 Web 操作。

  a. 设置 `require-whisk-auth` 值，其中 `<my-secret>` 是区分大小写的认证令牌。
    ```bash
    ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true --require-whisk-auth <mysecret>
    ```
    {: pre}
  
  b. 测试 Web 操作并包含 `<my-secret>` 值。可以通过以下任一方式来测试 Web 操作：
  * 使用 cURL 命令测试 Web 操作。
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}
  * 使用 `wget` 命令测试操作。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}


## 原始 HTTP 处理
{: #actions_web_raw}

Web 操作可选择直接解释和处理入局 HTTP 主体，而不将 JSON 对象提升为可用于 Web 操作输入的第一类属性（例如，`args.name`，而不是解析 `args.__ow_query`）。此过程通过 `raw-http` [注释](/docs/openwhisk?topic=cloud-functions-annotations)来执行。使用上文所示的相同示例，但现在作为将 `name` 同时接收为查询参数和 HTTP 请求主体中 JSON 值的“原始”HTTP Web 操作：
```bash
curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}


**示例输出**
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

{{site.data.keyword.openwhisk_short}} 使用 [Akka HTTP](https://doc.akka.io/docs/akka-http/current/?language=scala){: external} 框架来[确定哪些内容类型是二进制文件，哪些是明文](https://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html){: external}。

### 启用原始 HTTP 处理
{: #actions_web_raw_enable}

可以通过将 `--web` 设置为 `raw` 来创建原始 HTTP Web 操作。
```bash
ibmcloud fn action create demo/hello /<filepath>/hello.js --web raw
```
{: pre}

### 对基本 64 位编码的二进制主体内容解码
{: #actions_web_decode}

处理原始 HTTP 内容时，如果请求的 `Content-Type` 为 binary 类型，那么会对 `__ow_body` 内容进行 Base64 编码。
以下函数演示了如何对使用 Node、Python 和 Swift 编写的主体内容解码。

1. 将首选语言中的样本代码保存到名为 `decode.<ext>` 的文件中。将 `<ext>` 替换为首选语言样本代码的文件扩展名。

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

2. 通过运行以下命令，使用样本代码创建原始 HTTP Web 操作。在此示例中，Node 函数保存为 `decode.js`。将文件路径替换为 `decode` 文件的文件路径，并更新文件扩展名以匹配您使用的样本代码的扩展名。

  ```bash
  ibmcloud fn action create decode <filepath>/decode.js --web raw
  ```
  {: pre}

  **示例输出**
  ```
ok: created action decode
```
  {: screen}

3. 通过运行以下 cURL 命令来测试 `decode` 操作。
    ```bash
    curl -k -H "content-type: application" -X POST -d "Decoded body" https://<apihost>/api/v1/web/<namespace>/default/decode.json
    ```
    {: pre}

  **示例输出**
    ```
    {
      "body": "Decoded body"
    }
    ```
    {: screen}

## OPTIONS 请求
{: #actions_web_options}

缺省情况下，对 Web 操作发出的 `OPTIONS` 请求将生成自动添加到响应头的 CORS 头。这些头允许使用所有源以及 `OPTIONS`、`GET`、`DELETE`、`POST`、`PUT`、`HEAD` 和 `PATCH` HTTP 动词。
{: shortdesc}

请参阅以下头：
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

或者，`OPTIONS` 请求可以由 Web 操作手动处理。要启用此选项，请将值为 `true` 的 `web-custom-options` 注释添加到 Web 操作。启用了此功能时，CORS 头不会自动添加到请求响应。您必须改为以编程方式附加头。

要创建对 `OPTIONS` 请求的定制响应，请执行以下操作：

1. 将以下代码保存到 `custom-options.js` 文件中。

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

2. 创建 Web 操作。将 `require-whisk-auth` 设置为 `true`。

  ```bash
  ibmcloud fn action create custom-option <filepath>/custom-options.js --web true -a web-custom-options true
  ```
  {: pre}

3. 使用以下 cURL 命令来测试操作。

  ```bash
  $ curl https://<apihost>/api/v1/web/<namespace>/default/custom-option.http -kvX OPTIONS
  ```
  {: pre}

  **示例输出**
  ```
  < HTTP/1.1 200 OK
< Server: nginx/1.11.13
< Content-Length: 0
< Connection: keep-alive
< Access-Control-Allow-Methods: OPTIONS, GET
< Access-Control-Allow-Origin: example.com
```
  {: screen}

## 错误处理
{: #actions_web_errors}

{{site.data.keyword.openwhisk_short}} 操作以两种不同的可能失败方式失败。第一种称为_应用程序错误_，类似于捕获的异常：操作返回包含顶级 `error` 属性的 JSON 对象。第二种是_开发者错误_，当操作失败并且未生成响应时（这类似于未捕获的异常）发生。对于 Web 操作，控制器按如下所示处理应用程序错误：

- 将忽略任何指定的路径预测，控制器将改为预测 `error` 属性。
- 控制器将操作扩展名隐含的内容处理应用于 `error` 属性的值。

开发者必须了解可如何使用 Web 操作，并生成相应的错误响应。例如，与 `.http` 扩展名配合使用的 Web 操作会返回 HTTP 响应，例如 `{error: { statusCode: 400 }`。若未能返回 HTTP 响应，说明扩展名中隐含的 `Content-Type` 与错误响应中的 `Content-Type` 操作不匹配。必须格外注意作为序列的 Web 操作，以便构成序列的组成部分可以在需要时生成充分的错误。



## 禁用 Web 操作
{: #actions_web_disable}

您可以通过在 CLI 中将 `--web` 标志设置为 `false` 或 `no` 来禁用 Web 操作。将 `<packageName>/<actionName>` 和 `<filepath>/<filename>` 替换为包名称、Web 操作名称以及代码文件的文件路径和文件名。

```bash
ibmcloud fn action update <packageName>/<actionName> <filepath>/<filename> --web false
```
{: pre}



