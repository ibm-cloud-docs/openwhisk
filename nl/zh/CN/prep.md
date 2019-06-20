---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: actions, serverless, javascript, node, node.js

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


# 准备应用程序以用于操作
{: #prep}

无论您是使自己的应用程序成为无服务器的，还是编写专门用于响应事件的脚本，您的代码都必须满足特定需求，然后才能通过代码创建操作。
{: shortdesc}

每种编程语言都有特定的运行需求，但大多数编程语言具有以下一般需求：
- 缺省情况下，代码入口点的预期名称为 `main`。如果入口点不是 `main`，可以在创建操作时指定定制名称，因此请记下该名称。
- 进入应用程序的输入参数和来自应用程序的输出结果的格式必须设置为可在实体之间传递的特定结构。结构取决于代码语言。例如，对于 Python 应用程序，参数必须作为字典输入到应用程序中，并且应用程序的结果必须构造为字典。由于还可以将结构化对象中的参数传递到操作（如 JSON），因此可以将代码构造为预期使用特定字段（如 `name` 和 `place`）中 JSON 值的输入参数。

    JSON 输入示例：
    ```json
    {"name": "Dorothy", "place": "Kansas"}
    ```
    {: codeblock}

    JavaScript 示例：
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
    {: codeblock}
- 如果应用程序包含多个文件，那么必须将这些文件合并成一个文件，以便在操作中使用。可以将代码重写成一个文件，也可以将文件和依赖项打包成单个归档文件。如果不支持运行时，那么可以将应用程序打包成 Docker 映像。
- 还必须将依赖项与应用程序打包在一起。可用的运行时随附一些预安装的包和扩展。请[查看运行时的参考信息](/docs/openwhisk?topic=cloud-functions-runtimes)，以了解运行时是否已包含应用程序的依赖项。如果已包含依赖项，那么无需将其与应用程序打包在一起。

    代码编译不是必需的，但在针对运行时可行的情况下，提前编译代码可以提高性能。
    {: tip}

## 准备 Docker 映像形式的应用程序
{: #prep_docker}

通过 {{site.data.keyword.openwhisk_short}}，可以使用任何语言编写应用程序，并将其打包成 Docker 映像。
{: shortdesc}

可以仅使用公共注册表中的映像，例如 Docker Hub 上公开可用的映像。不支持专用注册表。
{: important}

### 将代码打包成 Docker 映像
{: #prep_docker_pkg}

您的代码会编译为可执行二进制文件，并嵌入到 Docker 映像中。二进制程序通过采用来自 `stdin` 的输入并通过 `stdout` 进行回复，从而与系统进行交互。
{: shortdesc}

开始之前：
- 您必须拥有 Docker Hub 帐户。可以在 [Docker Hub ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://hub.docker.com) 上设置免费 Docker 标识和帐户。
- [安装 Docker](https://hub.docker.com/search?offering=community&type=edition)。
- [查看 Docker 运行时的需求](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker)。

打包应用程序：

要将代码打包成 Docker 映像，请执行以下操作：
1. 下载并安装 Docker 框架。框架是一种 Docker 容器模板，可以在其中以定制二进制文件的形式插入代码。 
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. 在黑匣框架中设置定制二进制文件。该框架包含可以使用的 C 程序。在 Docker 映像构建过程中，会对 `example.c` 文件中的部分内容进行编译，所以无需在您的计算机上进行 C 程序编译。
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  示例输出：
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
            printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. 可选：通过修改 `Dockerfile` 来构建可执行文件，从而向 Docker 映像添加其他代码和依赖项。请注意以下需求：
  * 二进制文件必须位于容器内的 `/action/exec` 中。
  * 可执行文件会从命令行接收单个自变量。此自变量是字符串序列化的 JSON 对象，表示操作的自变量。
  * 程序可能会记录到 `stdout` 或 `stderr`。
  * 根据约定，输出的最后一行必须是字符串化的 JSON 对象，用于表示操作结果。有关构造 Dockerfile 的更多信息，请参阅 [Dockerfile 参考](https://docs.docker.com/engine/reference/builder/)。

4. 使用提供的脚本来构建 Docker 映像并进行上传。
    1. 登录到 Docker。
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. 导航至 `dockerSkeleton` 目录。
        ```
  cd dockerSkeleton
  ```
        {: pre}

    3. 运行脚本。
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}




## 准备 JavaScript 应用程序
{: #prep_js}

创建操作之前，请准备好 JavaScript 代码。确认代码结构是否正确，然后决定代码是否需要打包。
{: shortdesc}

### 构造 JavaScript 代码
{: #prep_js_struct}

- 入口点函数的预期名称为 `main`。如果代码中的函数不是 `main`，请记下函数名称，以在创建操作时指定该名称。
- 输入参数将作为 JSON 对象传递。
- 成功激活的结果也是 JSON 对象，但返回方式根据操作是[同步](#prep_js_sync)还是[异步](#prep_js_async)而有所不同。



示例：
```javascript
  function main() {
    return {payload: 'Hello world'};
  }
  ```

使用多个函数的示例：

  ```javascript
  function main() {
    return { payload: helper() }
  }

  function helper() {
      return new Date();
}
```
  {: codeblock}


### 构造具有同步行为的 JavaScript 代码
{: #prep_js_sync}

如果 main 函数不执行 `return` 语句就退出，或者退出时执行的 `return` 语句返回了除 Promise 以外的任何值，那么 JavaScript 激活是同步的。
{: shortdesc}

同步代码示例：

```javascript
// each path results in a synchronous activation
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




### 构造具有异步行为的 JavaScript 代码
{: #prep_js_async}

JavaScript 函数即使在返回之后，仍可以在回调函数中继续执行。如果 main 函数通过返回 Promise 退出，那么 JavaScript 激活是异步的。在此情况下，系统假定操作仍在运行，直到履行或拒绝 Promise 为止。通过在操作中返回 Promise，异步运行的 JavaScript 函数可在 `main` 函数返回后返回激活结果。
{: shortdesc}

首先，实例化新 Promise 对象并向其传递回调函数。回调函数采用两个自变量 resolve 和 reject，这两个自变量都是函数。所有异步代码都会进入该回调函数。操作处理程序可以具有任何名称，只要名称符合接受对象和返回对象（或对象的 `Promise`）的常规特征符即可。

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

在以上示例中，执行的详细信息如下。
* `main` 函数返回 Promise。Promise 指示激活尚未完成，而是预期在未来完成。
* `setTimeout()` JavaScript 函数等待 2 秒再调用 Promise 的回调函数，这代表异步代码。
* Promise 的回调接受自变量 `resolve` 和 `reject`，这两个自变量都是函数。
  * 对 `resolve()` 的调用履行 Promise 并指示激活已正常完成。
  * 对 `reject()` 的调用可用于拒绝 Promise 并指示激活异常完成。


### 构造具有同步和异步行为的 JavaScript 代码
{: #prep_js_both}

一个操作可以针对某些输入是同步的，而针对其他输入是异步的，如以下示例中所示。
{: shortdesc}

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





### 示例：使用 JavaScript 调用外部 API
{: #prep_js_api}

以下示例调用 NASA Astronomy Picture of the Day (APOD) 服务的外部 API，用于每天提供我们宇宙的独特图像。
{: shortdesc}


```javascript
let rp = require('request-promise')

  function main(params) {
      const options = {
        uri: "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo",
        json: true
    }
    return rp(options)
    .then(res => {
        return { response: res }
    })
}
```
{: codeblock}

将对 NASA APOD API 进行调用，并从 JSON 结果中抽取字段。

接下来，[创建](/docs/openwhisk?topic=cloud-functions-actions)并[调用操作](/docs/openwhisk?topic=cloud-functions-test)以对其进行测试。将返回以下示例对象：

```
{
  "copyright": "Eric Houck",
      "date": "2018-03-28",
      "explanation": "照片中这样的景象真的只在有蓝月亮的时候才会出现吗？...",
      "hdurl": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_1799.jpg",
      "media_type": "image",
      "service_version": "v1",
      "title": "Blue Moon Tree",
      "url": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_960.jpg"
    }
    ```
{: screen}






### 使用 Web 包模块打包 JavaScript 代码
{: #prep_js_pkg}

可以使用 JavaScript 模块打包器（如 [webpack ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://webpack.js.org/concepts/)）来打包应用程序。`webpack` 处理代码时，会以递归方式构建依赖项图，其中包含操作需要的每个模块。
{: shortdesc}

开始之前，请[查看 JavaScript 运行时随附的包](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments)，以了解运行时是否已包含应用程序的依赖项。如果未包含依赖项，那么必须将其与应用程序打包在一起。

1. 创建 `package.json` 文件。将 `webpack` 添加为开发依赖项。

    ```json
    {
      "name": "my-action",
      "main": "dist/bundle.js",
      "scripts": {
        "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10"
      },
      "dependencies": {
        "left-pad": "1.1.3"
      },
      "devDependencies": {
        "webpack": "^3.8.1"
      }
    }
    ```
    {: codeblock}

2. 将以下 webpack 配置代码保存在名为 `webpack.config.js` 的文件中。

    ```javascript
    var path = require('path');
    module.exports = {
      entry: './index.js',
      output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
      },
      target: 'node'
};
    ```
    {: codeblock}

3. 准备应用程序代码。在名为 `index.js` 的示例中，`global.main` 变量设置为应用程序的主函数。

    示例：
    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
        const lines = args.lines || [];
        return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    global.main = myAction;
    ```
    {: codeblock}

4. 在本地安装所有依赖项。

    ```
    npm install
    ```
    {: pre}

5. 构建 webpack 捆绑软件。

    ```
    npm run build
    ```
    {: pre}

    这将创建 `dist/bundle.js` 文件，并将其部署为操作源代码。

6. 使用 `npm` 脚本或 CLI 创建操作。

    * 使用 `npm` 脚本：

        ```
        npm run deploy
        ```
        {: pre}

    *   使用 CLI：

        ```
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    由 `webpack` 构建的捆绑软件文件只支持 JavaScript 依赖项。因此，如果捆绑软件依赖于二进制文件依赖项，那么操作调用可能会失败，因为 `bundle.js` 文件中不包含此依赖项。
    {: tip}



### 将 JavaScript 代码打包成 NPM 文件
{: #prep_js_npm}

作为在单个 JavaScript 源文件中编写所有操作代码的替代方法，可以将代码打包成 .zip 文件中的 `npm` 包。
{: shortdesc}

开始之前，请[查看 JavaScript 运行时随附的包](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments)，以了解运行时是否已包含应用程序的依赖项。如果未包含依赖项，那么必须将其与应用程序打包在一起。

1. 在根目录中，创建 `package.json` 文件。示例：

    ```json
    {
      "name": "my-action",
      "main": "index.js",
      "dependencies" : {
        "left-pad" : "1.1.3"
      }
    }
    ```
    {: codeblock}

2. 在本地安装所有依赖项。

    ```
    npm install DEPENDENCY
    ```
    {: pre}

    **注**：虽然大部分 `npm` 包会在执行 `npm install` 时安装 JavaScript 源代码，但还有些 npm 包会安装并编译二进制工件。归档文件上传只支持 JavaScript 依赖项。如果归档包含二进制依赖项，那么操作调用可能不会成功。

3. 创建包含所有文件（包括所有依赖项）的 `.zip` 归档。

    ```
  zip -r action.zip *
  ```
    {: pre}

    Windows 用户：使用 Windows 资源管理器操作来创建 .zip 文件将导致文件结构不正确。{{site.data.keyword.openwhisk_short}} .zip 操作必须在该归档的根目录中具有 `package.json`，但 Windows 资源管理器会将其放入嵌套文件夹内。请改为使用 `zip` 命令。
    {: tip}





## 准备 Go 应用程序
{: #prep_go}

对于快速测试或开发，请使用单个文件。对于生产应用程序，请将 Go 操作预编译成可执行文件，以获得更好的性能或支持多个源文件，包括供应商库。
{: shortdesc}

虽然可以通过使用 `GOOS=Linux` 和 `GOARCH=amd64` 进行交叉编译以在任何 Go 平台上创建二进制文件，但还是请使用运行时容器映像中嵌入的预编译功能。您可以对[多个源文件](#prep_go_multi)或[供应商库](#prep_go_vendor)打包。
{: tip}


### 构造 Go 代码
{: #prep_go_struct}

- 入口点包的预期名称为 `main`。如果代码中的包不是 `main`，请记下包名称，以在创建操作时指定该名称。
- 该包必须是公共包。

示例：
```go
package main

import "fmt"

// Main is the function implementing the action
    func Main(params map[string]interface{}) map[string]interface{} {
        // parse the input JSON
        name, ok := params["name"].(string)
        if !ok {
            name = "World"
        }
        msg := make(map[string]interface{})
        msg["body"] = "Hello " + name + "!"
        // can optionally log to stdout (or stderr)
        fmt.Println("hello Go action")
        // return the output JSON
        return msg
    }
    ```
    {: codeblock}

### 打包多个 Go 源文件
{: #prep_go_multi}

1. 创建顶级 `src` 目录。将属于主包的源文件放在 `src` 的根目录或 `main` 目录内，并为其他包创建子目录。例如，`hello` 包将变为 `src/hello` 目录。
  ```
go-action-hello/
└── src
    ├── hello
│   └── hello.go
    └── main
        └── main.go
```
  {: screen}

2. 导入子包。用于导入 hello 子包的 `main/main.go` 的示例：

  ```go
  package main

  import (
	"fmt"
  	"hello"
  )

  // Main forwading to Hello
  func Main(args map[string]interface{}) map[string]interface{} {
  	fmt.Println("This is main.Main")
  	greetings := "World"
  	name, ok := args["name"].(string)
  	if ok {
  		greetings = name
  	}
  	return hello.Hello(greetings)
  }
  ```
  {: codeblock}

  `hello/hello.go` 的示例：

  ```go
  package hello

  import "fmt"

  // Hello return a greeting message
  func Hello(name string) map[string]interface{} {
  	fmt.Println("This is hello.Hello")
  	res := make(map[string]interface{})
  	res["body"] = "Hello " + name
  	return res
  }
  ```
  {: codeblock}

3. 编译代码。为 `src` 目录创建 .zip 归档。**不要**包含顶级项目目录 `go-action-project/`。

  ```bash
  cd src
  zip -r ../hello-src.zip *
  cd ..
  ```
  {: pre}

  可以通过将 `GOPATH` 设置为 `src` 目录的父目录来进行本地编译。如果使用的是 VSCode，那么必须将 `go.inferGopath` 设置更改为 `true`。
  {: note}

4. 在该 .zip 归档的根目录中，编译 Go 可执行文件并将其打包成 `exec`。通过运行以下命令来构建 `hello-bin.zip` 归档。您必须已在工作站中安装 Docker CLI，并将 `docker` 置于 `PATH` 中。

  ```bash
  docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
  ```
  {: pre}

  在此示例中，主函数为 `-compile main`。要使用其他函数作为主函数，请更改 `-compile` 的值。主函数是在编译时选择的。预编译时，`ibmcloud fn action [update | create]` 会忽略 `--main`。

  容器在 `stdin` 中获取源 .zip 的内容，编译这些内容，然后在根目录中创建包含可执行文件 `exec` 的新 .zip 归档。该 .zip 归档的内容会流出到 `stdout`，在其中会重定向到要部署为 Go 操作的 `hello-bin.zip` 归档。




### 打包使用供应商库的 Go 代码
{: #prep_go_vendor}

编译 Go 文件时，可以通过在源 `zip` 归档中填充 `vendor` 目录来包含依赖项。`vendor` 目录不能在顶层工作。您必须将 `vendor` 目录放在 `src/` 内和包目录内。
{: shortdesc}

`hello.go` 应用程序中的示例日志包 `logrus`：

```go
package hello

import (
	"os"

	"github.com/Sirupsen/logrus"
)

var log = logrus.New()

func init() {
	log.Out = os.Stdout
}

// Hello return a greeting message
func Hello(name string) map[string]interface{} {
	log.WithFields(logrus.Fields{"greetings": name}).Info("Hello")
	res := make(map[string]interface{})
	res["body"] = "Hello, " + name
	return res
}
```
{: codeblock}

</br>
在此示例中，`vendor` 目录位于 `src/hello/vendor` 中。可以添加 `hello` 包使用的第三方库。您可以使用多个工具（如 [dep ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://golang.github.io/dep/docs/installation.html)）来填充和管理依赖项。

通过创建用于描述库版本和位置的文件 `src/main/Gopkg.toml`，可以使用 `dep`。

```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

填充 `vendor` 目录，然后运行 `dep ensure`。







## 准备 Swift 应用程序
{: #prep_swift}

Swift 文件必须先编译成二进制文件，然后才能运行操作。此延迟称为冷启动延迟。要避免冷启动延迟，可以将 Swift 文件编译为二进制文件，然后将该二进制文件以 .zip 文件格式上传到 {{site.data.keyword.openwhisk_short}}。Docker 运行时包含一个编译器，用于帮助用户编译和打包 Swift 4.2 操作。对该操作的后续调用会快得多，直到清除保存该操作的容器为止。

Swift 操作在 Linux 环境中运行。Linux 上的 Swift 仍在开发中；{{site.data.keyword.openwhisk_short}} 会使用最新可用的发行版。但这些发行版可能不稳定。此外，用于 {{site.data.keyword.openwhisk_short}} 的 Swift 版本可能与 macOS 上 Xcode 的稳定发行版中的 Swift 版本不一致。
{: important}



### 构造 Swift 代码
{: #prep_swift_struc}

入口点函数的预期名称为 `main`。如果代码中的函数不是 `main`，请记下函数名称，以在创建操作时指定该名称。

除了 main 函数特征符外，Swift 4 中还提供了两个利用 [Codable ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.apple.com/documentation/swift/codable) 类型的特征符。您可以了解有关[可编码和可解码以与 JSON 等外部表示法兼容 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types) 的数据类型的更多信息。

示例：
```swift
struct Input: Codable {
    let name: String?
}
struct Output: Codable {
    let greeting: String
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    let result = Output(greeting: "Hello \(param.name ?? "stranger")!")
        print("Log greeting:\(result.greeting)")
    completion(result, nil)
}
```
{: codeblock}

此示例采用带有 `name` 字段的 `Codable Input` 作为输入参数，并返回带有 `greetings` 字段的 `Codable Output`。


#### 处理 Swift 中的错误
{: #prep_swift_error}

通过使用 Codable 完成处理程序，可以传递错误以指示操作中的故障。[Swift 中的错误处理 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) 与其他语言中使用 `try`、`catch` 和 `throw` 关键字的异常处理类似。
{: shortdesc}

以下片段显示错误处理的示例。

```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    // 返回实际错误
    do{
        throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
    } catch {
        completion(nil, error)
    }
}
```
{: codeblock}


### 将 Swift 4.2 文件打包成二进制文件
{: #prep_swift42_single}

编译不依赖于外部库的单个源文件。将 `-compile` 标志与 main 方法的名称一起使用。

开始之前：
- [安装 Docker](https://hub.docker.com/search?offering=community&type=edition)。
- [查看 Swift 运行时随附的包](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions)，以了解运行时是否已包含应用程序的依赖项。如果未包含依赖项，那么必须将其与应用程序打包在一起。

打包应用程序：

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

Docker 容器会从 `stdin` 中读取文件的内容，并将包含已编译 Swift 可执行文件的 .zip 归档写入 `stdout`。



### 打包 Swift 4.2 多文件项目和依赖项
{: #prep_swift42_multi}

开始之前：
- [安装 Docker](https://hub.docker.com/search?offering=community&type=edition)。
- [查看 Swift 运行时随附的包](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions)，以了解运行时是否已包含应用程序的依赖项。如果未包含依赖项，那么必须将其与应用程序打包在一起。

打包应用程序：

1. 要编译多个文件并包含外部依赖项，请创建以下目录结构。

  ```
  .
  ├── Package.swift
└── Sources
    └── main.swift
```
  {: codeblock}

  `Sources/` 目录包含名为 `main.swift` 的文件。

  `Package.swift` 的开头必须是用于为 Swift 工具指定版本 `4.2` 的注释：

  ```swift
  // swift-tools-version:4.2
  import PackageDescription

  let package = Package(
      name: "Action",
      products: [
      .executable(
          name: "Action",
          targets:  ["Action"]
      )
      ],
      dependencies: [
      .package(url: "https://github.com/IBM-Swift/SwiftyRequest.git", .upToNextMajor(from: "1.0.0"))
      ],
      targets: [
      .target(
          name: "Action",
          dependencies: ["SwiftyRequest"],
          path: "."
      )
      ]
  )
  ```
  {: codeblock}

2. 创建包含该目录内容的 .zip 归档：

  ```bash
  zip ../action-src.zip -r *
  ```
  {: codeblock}

3. 通过 `stdin` 将 .zip 归档传递到 Docker 容器。`stdout` 是包含已编译可执行文件的新 .zip 归档。Docker 容器会从 `stdin` 中读取 .zip 归档的内容，并将包含已编译 Swift 可执行文件的新 .zip 归档写入 `stdout`。

  ```
docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
```
  {: codeblock}

  在基于 Linux 的系统中，可以将 `zip` 和 `docker run` 步骤合并到单个命令中：

  ```
zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
```
  {: codeblock}





## 打包 Python 应用程序
{: #prep_python}


### 构造 Python 代码
{: #prep_python_struct}

- Python 应用程序必须使用一个字典并生成一个字典。
- 入口点方法的预期名称为 `main`。如果代码中的函数不是 `main`，请记下函数名称，以在创建操作时指定该名称。
{: shortdesc}

示例：
```python
def main(args):
	name = args.get("name", "stranger")
	greeting = "Hello " + name + "!"
	print(greeting)
        return {"greeting": greeting}
```

### 打包 Python 代码
{: #prep_python_pkg}

将 Python 代码和从属模块打包成 .zip 文件。在此示例中，包含入口点的源文件为 `__main__.py`，帮助程序模块位于名为 `helper.py` 的文件中。

开始之前，请[查看 Python 运行时随附的包](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments)，以了解运行时是否已包含应用程序的依赖项。如果未包含依赖项，那么必须将其与应用程序打包在一起。

打包应用程序：

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}


### 将虚拟环境的 Python 代码打包成 zip 文件
{: #prep_python_virtenv}

可以使用虚拟环境 `virtualenv` 来打包 Python 依赖项。虚拟环境允许您链接可以使用 [`pip` ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://packaging.python.org/installing/) 安装的其他包。

开始之前，请[查看 Python 运行时随附的包](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments)，以了解运行时是否已包含应用程序的依赖项。如果未包含依赖项，那么必须将其与应用程序打包在一起。

打包应用程序：

1. 创建包含要安装的 `pip` 模块和版本的 [requirements.txt ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) 文件。

  要使 `virtualenv` 保持最小大小，请仅将不属于所选运行时环境的模块添加到 `requirements.txt` 中。有关 Python 运行时中包含的包的更多信息，请参阅 Python [运行时参考](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments)。
  {: tip}

2. 为运行时拉出下列其中一个映像。为了确保与运行时容器的兼容性，虚拟环境内部的打包安装必须使用与所指定运行时对应的映像。
    * 对于 `python:3.7`，请使用 Docker 映像 `ibmfunctions/action-python-v3.7`。
    * 对于 `python:3.6`，请使用 Docker 映像 `ibmfunctions/action-python-v3.6`。
    * 对于 `python:2`，请使用 Docker 映像 `openwhisk/python2action`。

   示例：
   ```
   docker pull ibmfunctions/action-python-v3.7
   ```
   {: pre}

2. 安装依赖项并创建虚拟环境。虚拟环境目录必须命名为 `virtualenv`。

   ```
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. 打包 `virtualenv` 目录和任何其他 Python 文件。包含入口点的源文件必须命名为 `__main__.py`。

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}





## 准备 Ruby 应用程序
{: #prep_ruby}

创建操作之前，请准备好 Ruby 代码。

### 构造 Ruby 代码
{: #prep_ruby_struct}

* Ruby 操作始终使用散列（类似于字典的集合）并返回散列。
* 入口点函数的预期名称为 `main`。如果代码中的函数不是 `main`，请记下函数名称，以在创建操作时指定该名称。


示例：
```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
    ```
    {: codeblock}

### 打包 Ruby 代码
{: #prep_ruby_pkg}

可以将 Ruby 应用程序和从属包打包成 .zip 文件。例如，可以打包具有名为 `helper.rb` 的第二个文件的操作。

创建包含源文件的归档。包含入口点的源文件必须命名为 `main.rb`。

```bash
    zip -r helloRuby.zip main.rb helper.rb
    ```
{: pre}

除了缺省和捆绑的 gem 外，还提供了 gem `mechanize` 和 `jwt`。只要使用压缩的操作来打包所有依赖项，就可以使用任意 gem。



## 准备 PHP 应用程序
{: #prep_php}

创建操作之前，请准备好 PHP 代码。

### 构造 PHP 代码
{: #prep_php_struct}

- PHP 操作始终使用一个关联数组并返回一个关联数组。
- 入口点函数的预期名称为 `main`。如果代码中的函数不是 `main`，请记下函数名称，以在创建操作时指定该名称。

示例：
```php
<?php
function main(array $args) : array
{
    $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
?>
```
{: codeblock}

### 打包 PHP 代码
{: #prep_php_pkg}

可以将 PHP 文件或从属包打包成 .zip 文件。

开始之前，请[查看 PHP 运行时随附的包](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php)，以了解运行时是否已包含应用程序的依赖项。如果未包含依赖项，那么必须将其与应用程序打包在一起。

打包应用程序：

```bash
zip -r ARCHIVE_NAME.zip FILE_1.php FILE_2.php
```
{: pre}

示例：
```bash
    zip -r helloPHP.zip index.php helper.php
    ```
{: pre}







## 准备 Java 应用程序
{: #prep_java}

创建操作之前，请准备好 Java 代码。

### 构造 Java 代码
{: #prep_java_struct}

Java 操作是包含名为 `main` 的方法的 Java 程序。`main` 必须具有以下特征符。


```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}


* 必须使用 `--main` 来指定主类的名称。符合要求的主类是实现了静态 `main` 方法的主类。如果该类不在缺省包中，请使用 Java 标准类名，例如 `--main com.example.MyMain`。
* 可以定制 Java 操作的方法名称。这将通过指定操作的标准方法名称来实现，例如 `--main com.example.MyMain#methodName`。
* 操作类型使用源文件扩展名来确定。

示例：
```java
    import com.google.gson.JsonObject;
    public class Hello {
        public static JsonObject main(JsonObject args) {
        String name = "stranger";
        if (args.has("name"))
            name = args.getAsJsonPrimitive("name").getAsString();
        JsonObject response = new JsonObject();
        response.addProperty("greeting", "Hello " + name + "!");
        return response;
    }
}
```
{: codeblock}


### 打包 Java 代码
{: #prep_java_pkg}

要编译、测试和归档 Java 文件，必须在本地安装 [JDK 8 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://openjdk.java.net/install)。

1. 将以下代码保存在名为 `Hello.java` 的文件中。

    ```java
    import com.google.gson.JsonObject;
    public class Hello {
        public static JsonObject main(JsonObject args) {
        String name = "stranger";
        if (args.has("name"))
            name = args.getAsJsonPrimitive("name").getAsString();
        JsonObject response = new JsonObject();
        response.addProperty("greeting", "Hello " + name + "!");
        return response;
    }
}
    ```
    {: codeblock}

2. 将 `Hello.java` 文件编译成类文件。

    ```
javac Hello.java
```
    {: pre}

2. 将类文件压缩成名为 `hello.jar` 的 JAR 文件。**注**：[google-gson ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/google/gson) 必须存在于 Java CLASSPATH 中。
3.
    ```
    jar cvf hello.jar Hello.class
```
    {: pre}


### 使用 Gradle 打包 Java 代码
{: #prep_java_gradle}

可以使用构建工具（如 [Gradle](https://gradle.org)）来访存存储库（如 Maven Central）中的库，并构建包含代码和所有依赖项的最终 JAR 归档。

下面是使用 Gradle 来构建利用 `com.google.zxing` 库的 Java 操作的示例，该库提供了生成 QR 代码映像的功能。

1. 创建名为 `build.gradle` 的文件并指定依赖项。

  ```gradle
  apply plugin: 'java'

  version = '1.0'

  repositories {
     mavenCentral()
  }

  configurations {
      provided
      compile.extendsFrom provided
  }

  dependencies {
       provided 'com.google.code.gson:gson:2.6.2'
      compile 'com.google.zxing:core:3.3.0'
       compile 'com.google.zxing:javase:3.3.0'
  }

  jar {
      dependsOn configurations.runtime

     from {
          (configurations.runtime - configurations.provided).collect {
              it.isDirectory() ? it : zipTree(it)
          }
      }
  }
  ```
  {: codeblock}

2. 运行 `gradle jar` 命令，这将在 `build/libs/` 目录中生成 JAR 归档。

  有关更多信息，请阅读 Gradle 文档 [Declaring Dependencies](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies)。





## 准备 .NET Core 应用程序
{: #prep_dotnet}

创建操作之前，请准备好 .NET Core 代码。

### 构造 .NET Core 代码
{: #prep_dotnet_struct}

.NET Core 操作是一个 .NET Core 类库，使用预期名为 `Main` 的方法。如果代码中的方法不是 `Main`，请记下方法名称，以在创建操作时指定该名称，其格式为：`--main {Assembly}::{Class Full Name}::{Method}`

示例：
```
Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main
```

### 打包 .NET Core 代码
{: #prep_dotnet_pkg}

开始之前：
要编译、测试和归档 .NET Core 项目，必须执行以下操作：
- 在本地安装 [.NET Core SDK](https://dotnet.microsoft.com/download)。
- 将 `DOTNET_HOME` 环境变量设置为可以找到 `dotnet` 可执行文件的位置。



要打包代码，请执行以下操作：

  1. 创建名为 `Apache.OpenWhisk.Example.Dotnet` 的 C# 项目。

      ```bash
      dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
      ```
      {: pre}

  2. 导航至 `Apache.OpenWhisk.Example.Dotnet` 目录。

      ```bash
      cd Apache.OpenWhisk.Example.Dotnet
      ```
      {: pre}

  3. 安装 [Newtonsoft.Json](https://www.newtonsoft.com/json) NuGet 包，如下所示。

      ```bash
    dotnet add package Newtonsoft.Json -v 12.0.1
    ```
      {: pre}

  4. 将以下代码保存在名为 `Hello.cs` 的文件中。

      ```csharp
    using System;
    using Newtonsoft.Json.Linq;

      namespace Apache.OpenWhisk.Example.Dotnet
    {
        public class Hello
        {
            public JObject Main(JObject args)
            {
                string name = "stranger";
                if (args.ContainsKey("name")) {
                    name = args["name"].ToString();
                }
                JObject message = new JObject();
                message.Add("greeting", new JValue($"Hello, {name}!"));
                return (message);
            }
        }
      }
      ```
      {: codeblock}

  5. 编译 `Hello.cs` 和其他任何文件，然后输出到 `out` 目录。

      ```bash
    dotnet publish -c Release -o out
    ```
      {: pre}

  6. 导航至 out 目录。

      ```bash
      cd out
      ```
      {: pre}

  7. 压缩发布的文件。

      ```bash
      zip -r -0 ../helloDotNet.zip *
      ```
      {: pre}


