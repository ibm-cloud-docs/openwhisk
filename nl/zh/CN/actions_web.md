---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: web actions, serverless

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


# 创建 Web 操作
{: #actions_web}

Web 操作是 {{site.data.keyword.openwhisk}} 操作，这些操作经过注释，可快速支持开发者构建基于 Web 的应用程序。这些带注释的操作允许开发者对 Web 应用程序可以匿名访问的后端逻辑进行编程，而无需 {{site.data.keyword.openwhisk_short}} 认证密钥。由操作开发者来实现其自己所需的认证和授权（即 OAuth 流）。
{: shortdesc}

Web 操作激活将与创建操作的用户相关联。此操作会将操作激活的成本从调用者转移到操作所有者。

查看以下 JavaScript 操作 `hello.js`：
```javascript
function main({name}) {
  var msg = 'you did not tell me who you are.';
  if (name) {
    msg = `hello ${name}!`
  }
  return {body: `<html><body><h3>${msg}</h3></body></html>`}
}
```
{: codeblock}

您可在 CLI 中使用值为 `true` 或 `yes` 的 `--web` 标志在包 `demo` 中为名称空间 `guest` 创建 _Web 操作_ **hello**：
```
ibmcloud fn package create demo
```
{: pre}

```
ibmcloud fn action create /guest/demo/hello hello.js --web true
```
{: pre}

使用值为 `true` 或 `yes` 的 `--web` 标志时，将允许通过 REST 接口访问操作，而无需凭证。要使用凭证配置 Web 操作，请参阅[保护 Web 操作](#actions_web_secure)部分。可以使用如下所示构造的 URL 来调用 Web 操作：`https://{APIHOST}/api/v1/web/{namespace}/{packageName}/{actionName}.{EXT}`。

如果操作不在指定的包中，那么包名为 **default**。

例如，`guest/demo/hello`。Web 操作 API 路径可与 `curl` 或 `wget` 配合使用，而不使用 API 密钥。甚至可以直接在浏览器中进行输入。

尝试在 Web 浏览器中打开 `https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello?name=Jane`。或者，尝试使用 `curl` 来调用该操作：
```
curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello?name=Jane
```
{: pre}

在以下示例中，Web 操作将执行 HTTP 重定向：
```javascript
function main() {
  return { 
    headers: { location: 'http://openwhisk.org' },
    statusCode: 302
  }
}
```
{: codeblock}

在以下示例中，Web 操作将设置单个 cookie：
```javascript
function main() {
  return { 
    headers: { 
      'Set-Cookie': 'UserID=Jane; Max-Age=3600; Version=',
      'Content-Type': 'text/html'
    }, 
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
}
```
{: codeblock}

在以下示例中，Web 操作将设置多个 cookie：
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

以下示例将返回 `image/png`：
```javascript
function main() {
  let png = <base 64 encoded string>
    return { headers: { 'Content-Type': 'image/png' },
             statusCode: 200,
             body: png };
}
```
{: codeblock}

The following example returns `application/json`:
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

The default `Content-Type` for an HTTP response is `application/json`, and the body can be any allowed JSON value. The default `Content-Type` can be omitted from the headers.

It is important to be aware of the [响应大小限制](/docs/openwhisk?topic=cloud-functions-limits)，因为超过预定义系统限制的响应将失败。例如，大对象不会通过 {{site.data.keyword.openwhisk_short}} 内嵌发送，而是会转移到对象存储。

## 使用操作处理 HTTP 请求
{: #actions_web_http}

不是 Web 操作的 {{site.data.keyword.openwhisk_short}} 操作需要认证，并且必须使用 JSON 对象进行响应。

Web 操作可以在无需认证的情况下进行调用，也可用于实现通过不同类型的 `headers`、`statusCode` 和 `body` 内容进行响应的 HTTP 处理程序。
Web 操作必须返回 JSON 对象。但是，控制器在其结果包含以下一个或多个属性作为顶级 JSON 属性的情况下，将以不同的方式处理 Web 操作：

- `headers`：JSON 对象，其中键是头名称，值是字符串值、数字值或布尔值。要为单个头发送多个值，该头的值为 JSON 多值数组。缺省情况下，不会设置任何头。
- `statusCode`：有效的 HTTP 状态码。如果存在 body 内容，那么缺省值为 `200 OK`。如果不存在 body 内容，那么缺省值为 `204 No Content`。
- `body`：字符串，为明文、JSON 对象或数组，或者为 Base64 编码的字符串（对于二进制数据）。如果 body 为 `null`、空字符串 `""` 或未定义，那么会将 body 视为空。缺省值为空主体。

控制器将任何操作指定的头、状态码或主体传递到 HTTP 客户机，后者将终止请求或响应。如果操作结果的 `headers` 中未声明 `Content-Type` 头，那么 body 将解释为 `application/json`（表示非字符串值），否则将解释为 `text/html`。 如果定义了 `Content-Type` 头，控制器将确定响应是二进制数据还是明文，然后根据需要使用 Base64 解码器对字符串进行解码。如果 body 未能正确解码，那么会向客户机返回错误。


## HTTP 上下文
{: #actions_web_context}

所有 Web 操作在调用时都会接收 HTTP 请求详细信息以作为操作输入自变量的参数。

请参阅以下 HTTP 参数：

- `__ow_method`（类型：字符串）：请求的 HTTP 方法。
- `__ow_headers`（类型：字符串到字符串的映射）：请求头。
- `__ow_path`（类型：字符串）：请求的未匹配路径（使用操作扩展名后，匹配会停止）。
- `__ow_user`（类型：字符串）：用于标识经 {{site.data.keyword.openwhisk_short}} 认证的主题的名称空间。
- `__ow_body`（类型：字符串）：请求主体实体，内容为二进制时为基本 64 位编码字符串，其他情况均为明文字符串。
- `__ow_query`（类型：字符串）：请求中的查询参数（以未解析字符串的形式提供）。

请求不得覆盖任何指定的 `__ow_` 参数。加以覆盖会导致请求失败，并返回状态“400 错误请求”。

仅当 Web 操作[注释为需要认证](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions)并允许 Web 操作实现其自己的授权策略时，才会显示 `__ow_user`。仅当 Web 操作选择处理[“原始”HTTP 请求](#actions_web_raw_enable)时，`__ow_query` 才可用。这是包含从 URI 中解析出的查询参数（用 `&` 分隔）的字符串。处理“原始”HTTP 请求时或者 HTTP 请求实体不是 JSON 对象或表单数据时，会显示 `__ow_body` 属性。在其他情况下，Web 操作接收查询和主体参数作为操作自变量中的第一类属性。主体参数优先于查询参数，而查询参数又优先于操作和包参数。

## HTTPS 端点支持
{: #actions_web_endpoint}

支持的 SSL 协议：TLS 1.2 和 TLS 1.3（[草稿版本 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18)）

不支持的 SSL 协议：SSLv2、SSLv3、TLS 1.0 和 TLS 1.1

## 额外功能
{: #actions_web_extra}

Web 操作额外提供了一些功能，包括：

- `内容扩展名`：请求必须将其所需的内容类型指定为 `.json`、`.html`、`.http`、`.svg` 或 `.text`。通过在 URI 中向操作名称添加扩展名来指定类型，以便 `/guest/demo/hello` 操作作为 `/guest/demo/hello.http`（举例而言）引用以接收返回的 HTTP 响应。为了方便起见，检测不到扩展名时将采用 `.http` 扩展名。
- `对结果中的字段投影`：操作名称后跟的路径用于投影到响应的一个或多个级别。
`/guest/demo/hello.html/body`。此功能允许返回字典 `{body: "..." }` 的操作对 `body` 属性进行投影，并直接返回其字符串值。投影的路径将采用绝对路径模型（如在 XPath 中一样）。
- `查询和主体参数作为输入`：操作会接收查询参数以及请求主体中的参数。合并参数的优先顺序如下：包参数、操作参数、查询参数、主体参数。如果发生重叠，这些参数都会覆盖任何先前的值。例如，`/guest/demo/hello.http?name=Jane` 可以将自变量 `{name: "Jane"}` 传递到操作。
- `表单数据`：除了标准 `application/json` 外，Web 操作还可以接收 URL 编码的表单数据 `application/x-www-form-urlencoded data` 作为输入。
- `通过多个 HTTP 动词激活`：Web 操作可通过 HTTP 方法 `GET`、`POST`、`PUT`、`PATCH` 和 `DELETE` 中的任一种方法以及 `HEAD` 和 `OPTIONS` 进行调用。
- `非 JSON 主体和原始 HTTP 实体处理`：Web 操作可接受非 JSON 对象的 HTTP 请求主体，并可选择始终接收此类值作为不透明值（不是二进制时为明文，其他情况均为基本 64 位编码的字符串）。

以下示例简要概括了在 Web 操作中可如何使用这些功能。假设 `/guest/demo/hello` 操作具有以下主体：
```javascript
  function main(params) {
      return { response: params };
}
```

将此操作作为 Web 操作调用时，可以通过对结果中的不同路径进行投影来变更该 Web 操作的响应。

例如，要返回整个对象，并查看操作接收的自变量：
```
 curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json
 ```
{: pre}

示例输出：
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

要使用查询参数运行，请参阅以下示例命令：
```
 curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json?name=Jane
 ```
{: pre}

示例输出：
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

还可以使用表单数据来运行：
```
 curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json -d "name":"Jane"
 ```
{: pre}

示例输出：
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

对 JSON 对象运行以下命令：
```
 curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
```
{: pre}

示例输出：
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

运行以下命令以对名称（作为文本）进行投影：
```
curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.text/response/name?name=Jane
```
{: pre}

示例输出：
```
Jane
```
{: screen}

为了方便起见，查询参数、表单数据和 JSON 对象主体实体均视为字典，其值可以直接作为操作输入属性进行访问。但对于选择以更直接的方式处理 HTTP 请求实体的 Web 操作，或者当 Web 操作接收到非 JSON 对象的实体时，此行为就不是这样了。

请参阅以下示例，其中使用的 content-type 是“text”，如先前所示。
```
curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
```
{: pre}

示例输出：
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

## 内容扩展名
{: #actions_web_ext}

调用 Web 操作时通常需要内容扩展名。缺少扩展名时将采用 `.http` 作为缺省值。`.json` 和 `.http` 扩展名无需投影路径，而 `.html`、`.svg` 和 `.text` 扩展名则需要投影路径。为了方便起见，将采用缺省路径来与扩展名相匹配。要调用 Web 操作并接收 `.html` 响应，该操作必须使用包含顶级属性 `html` 的 JSON 对象进行响应（或者响应必须位于显式路径中）。换言之，`/guest/demo/hello.html` 相当于显式对 `html` 属性进行投影，如 `/guest/demo/hello.html/html` 中一样。操作的标准名称必须包含其包名，如果操作不在指定的包中，那么包名为 `default`。

## 受保护的参数
{: #actions_web_protect}

操作参数受到保护并且被视为不可改变。参数会自动完成，以启用 Web 操作。
```
ibmcloud fn action create /guest/demo/hello hello.js --parameter name Jane --web true
```
{: pre}

这些更改的结果是 `name` 绑定到 `Jane`，并且因为是最终注释，所以查询或主体参数都无法将其覆盖。此设计将保护操作不被意外或有意尝试更改此值的查询或主体参数覆盖。

## 保护 Web 操作
{: #actions_web_secure}

缺省情况下，任何具有 Web 操作调用 URL 的人员都可以调用 Web 操作。使用 `require-whisk-auth` [Web 操作注释](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions)可保护 Web 操作。`require-whisk-auth` 注释设置为 `true` 时，操作将根据操作所有者的 Whisk 认证密钥来认证调用请求的基本授权凭证。设置为数字或区分大小写的字符串时，操作的调用请求必须包含具有此相同值的 `X-Require-Whisk-Auth` 头。受保护的 Web 操作在凭证验证失败时，将返回消息`未授权`。

或者，使用 `--web-secure` 标志来自动设置 `require-whisk-auth` 注释。设置为 `true` 时，将生成随机数作为 `require-whisk-auth` 注释值。设置为 `false` 时，将除去 `require-whisk-auth` 注释。设置为其他任何值时，该值将用作 `require-whisk-auth` 注释值。

使用 **--web-secure** 的示例：
```bash
ibmcloud fn action update /guest/demo/hello hello.js --web true --web-secure my-secret
```
{: pre}

使用 **require-whisk-auth** 的示例：
```bash
ibmcloud fn action update /guest/demo/hello hello.js --web true -a require-whisk-auth my-secret
```
{: pre}

使用 **X-Require-Whisk-Auth** 的示例：
```bash
curl https://${APIHOST}/api/v1/web/guest/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: my-secret"
```
{: pre}

需要注意的是，Web 操作的所有者拥有所有激活记录，并且需要在系统中运行该操作，无论该操作的调用方式如何。

## 禁用 Web 操作
{: #actions_web_disable}

要禁止通过 Web API (`https://openwhisk.bluemix.net/api/v1/web/`) 调用 Web 操作，请使用 CLI 将值 `false` 或 `no` 传递给 `--web` 标志，以更新操作。
```
ibmcloud fn action update /guest/demo/hello hello.js --web false
```
{: pre}

## 原始 HTTP 处理
{: #actions_web_raw}

Web 操作可选择直接解释和处理入局 HTTP 主体，而不将 JSON 对象提升为操作输入可用的第一类属性（例如，`args.name`，而不是解析 `args.__ow_query`）。此过程通过 `raw-http` [注释](/docs/openwhisk?topic=cloud-functions-annotations)来执行。使用上文所示的相同示例，但现在作为将 `name` 同时接收为查询参数和 HTTP 请求主体中 JSON 值的“原始”HTTP Web 操作：
```
curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}

示例输出：
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

OpenWhisk 使用 [Akka Http](https://doc.akka.io/docs/akka-http/current/?language=scala) 框架来[确定](https://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html)哪些内容类型是二进制，哪些是明文。

### 启用原始 HTTP 处理
{: #actions_web_raw_enable}

原始 HTTP Web 操作通过值为 `raw` 的 `--web` 标志来启用。
```
ibmcloud fn action create /guest/demo/hello hello.js --web raw
```
{: pre}

### 禁用原始 HTTP 处理
{: #actions_web_raw_disable}

禁用原始 HTTP 可以通过将值 `false` 或 `no` 传递给 `--web` 标志来完成。
```
ibmcloud fn update create /guest/demo/hello hello.js --web false
```
{: pre}

### 对基本 64 位编码的二进制主体内容解码
{: #actions_web_decode}

处理原始 HTTP 内容时，如果请求的 `Content-Type` 为 binary，那么会对 `__ow_body` 内容进行基本 64 位编码。
以下函数演示了如何对使用 Node、Python 和 Swift 编写的主体内容解码。只需将方法保存到文件，利用已保存的工件创建原始 HTTP Web 操作，然后调用该 Web 操作即可。

#### Node
{: #actions_web_decode_js}

```javascript
function main(args) {
     decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
    return {body: decoded}
}
```
{: codeblock}

#### Python
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

#### Swift
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

例如，将 Node 函数保存为 `decode.js`，然后执行以下命令：
```
ibmcloud fn action create decode decode.js --web raw
```
{: pre}

示例输出：
```
ok: created action decode
```
{: screen}

```
curl -k -H "content-type: application" -X POST -d "Decoded body" https:// us-south.functions.cloud.ibm.com/api/v1/web/guest/default/decodeNode.json
```
{: pre}

示例输出：
```
{
  "body": "Decoded body"
}
```
{: screen}

## OPTIONS 请求
{: #actions_web_options}

缺省情况下，对 Web 操作发起的 OPTIONS 请求将生成自动添加到响应头的 CORS 头。这些头允许使用所有源以及 options、get、delete、post、put、head 和 patch HTTP 动词。

请参阅以下头：
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

或者，OPTIONS 请求可以由 Web 操作手动处理。要启用此选项，请将值为 `true` 的 `web-custom-options` 注释添加到 Web 操作。启用了此功能时，CORS 头不会自动添加到请求响应。而是由开发者负责以编程方式附加所需的头。

请参阅以下示例以创建对 OPTIONS 请求的定制响应：
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

将该函数保存到 `custom-options.js`，然后执行以下命令：
```
ibmcloud fn action create custom-option custom-options.js --web true -a web-custom-options true
```
{: pre}

```
$ curl https://${APIHOST}/api/v1/web/guest/default/custom-options.http -kvX OPTIONS
```
{: pre}

示例输出：
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

{{site.data.keyword.openwhisk_short}} 操作以两种不同的可能失败方式失败。第一种称为_应用程序错误_，类似于捕获的异常：操作返回包含顶级 `error` 属性的 JSON 对象。第二种是_开发者错误_，当操作以灾难性的方式失败并且未生成响应时（这类似于未捕获的异常）发生。对于 Web 操作，控制器按如下所示处理应用程序错误：

- 将忽略任何指定的路径预测，控制器将改为预测 `error` 属性。
- 控制器将操作扩展名隐含的内容处理应用于 `error` 属性的值。

开发者必须了解可如何使用 Web 操作，并生成相应的错误响应。例如，与 `.http` 扩展名配合使用的 Web 操作会返回 HTTP 响应，例如 `{error: { statusCode: 400 }`。若未能返回 HTTP 响应，说明扩展名中隐含的 `Content-Type` 与错误响应中的 `Content-Type` 操作不匹配。必须格外注意作为序列的 Web 操作，以便构成序列的组成部分可以在需要时生成充分的错误。

## 示例：通过输入生成 QR 代码映像
{: #actions_web_qr}

下面是 Java Web 操作的示例，该操作采用 `text` 作为输入，并生成 QR 代码映像。

1. 在 `java_example/src/main/java/qr` 目录中创建 `Generate.java` 文件。

  ```java
  package qr;

  import java.io.*;
  import java.util.Base64;

  import com.google.gson.JsonObject;

  import com.google.zxing.*;
  import com.google.zxing.client.j2se.MatrixToImageWriter;
  import com.google.zxing.common.BitMatrix;

  public class Generate {
    public static JsonObject main(JsonObject args) throws Exception {
      String property = "text";
      String text = "Hello. Try with a 'text' value next time.";
      if (args.has(property)) {
        text = args.get(property).toString();
      }

      ByteArrayOutputStream baos = new ByteArrayOutputStream();
      OutputStream b64os = Base64.getEncoder().wrap(baos);

      BitMatrix matrix = new MultiFormatWriter().encode(text, BarcodeFormat.QR_CODE, 300, 300);
      MatrixToImageWriter.writeToStream(matrix, "png", b64os);
      b64os.close();

      String output = baos.toString("utf-8");

      JsonObject response = new JsonObject();
      JsonObject headers = new JsonObject();
      headers.addProperty("content-type", "image/png; charset=UTF-8");
      response.add("headers", headers);
      response.addProperty("body", output);
      return response;
    }
  }
  ```
  {: codeblock}

3. 通过从 `build.gradle` 文件所在的 `java_example` 目录运行以下命令来构建 Web 操作 JAR。

  ```bash
  gradle jar
  ```
  {: pre}

4. 使用 JAR `build/libs/java_example-1.0.jar` 部署 Web 操作。

  ```bash
  ibmcloud fn action update QRGenerate build/libs/java_example-1.0.jar --main qr.Generate -m 128 --web true
  ```
  {: pre}

5. 检索 Web 操作端点的公共 URL，并将其分配给环境变量。

  ```bash
  ibmcloud fn action get QRGenerate --url
  URL=$(ibmcloud fn action get QRGenerate --url | tail -1)
  ```
  {: pre}

6. 可以使用此 `URL` 来打开 Web 浏览器，并向查询参数 `text` 附加要编码为 QR 映像的消息。此外，还可以使用 HTTP 客户机（如 `curl`）来下载 QR 映像。

  ```bash
  curl -o QRImage.png $URL\?text=https://cloud.ibm.com
  ```
  {: pre}
