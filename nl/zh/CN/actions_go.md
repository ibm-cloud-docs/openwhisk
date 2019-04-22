---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: docker, actions, serverless

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

# 创建 Go 操作
{: #creating-go-actions}

以下各部分将指导您创建并调用单个 Go 操作，然后向该操作添加参数。可以使用 Go 1.11 执行 Go 操作。要使用此运行时，请在创建或更新操作时，指定 `ibmcloud fn` CLI 参数 `--kind go:1.11`。
{: shortdesc}

## 创建并调用 Go 操作
{: #invoking-go-actions}

Go 操作仅仅是 `main` 包中的公共函数。对于快速测试或开发，请使用单个文件。对于生产应用程序，请[将 Go 操作预编译成可执行文件](#packaging-go-actions)，以获得更好的性能或支持多个源文件，包括供应商库。
{: shortdesc}

创建 Go 操作。

1. 将以下代码保存在名为 `hello.go` 的文件中。在该示例中，函数名为 `Main`，但您可以使用 `--main` 标志来更改该名称。如果对函数重命名，名称必须以大写字母开头，且不能是术语 `main`，这是 Go 包的名称。

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

    </br>
    `Main` 函数的预期特征符为：

    ```go
    func Main(event map[string]interface{}) map[string]interface{}
    ```
    {: codeblock}

2. 创建或更新名为 `helloGo` 的操作。

    ```bash
    ibmcloud fn action update helloGo hello.go --kind go:1.11
    ```
    {: pre}

3. 调用操作。
    

    ```bash
    ibmcloud fn action invoke --result helloGo --param name gopher
    ```
    {: pre}

    示例输出：
    ```json
      {
          "greeting": "Hello gopher!"
      }
    ```
    {: screen}

## 将操作打包成 Go 可执行文件
{: #packaging-go-actions}

虽然可以通过使用 `GOOS=Linux` 和 `GOARCH=amd64` 进行交叉编译以在任何 Go 平台上创建二进制文件，但还是请使用运行时容器映像中嵌入的预编译功能。您可以对[多个源文件](#multiple-packages-go-actions)或[供应商库](#vendor-libs-go-actions)打包。
{: shortdesc}

### 使用多个包源文件
{: #multiple-packages-go-actions}

要使用多个包源文件，请使用顶级 `src` 目录，将属于主数据包的源文件放在 `src` 的根目录或 `main` 目录内，并为其他包创建目录。例如，`hello` 包将变为 `src/hello` 目录。
{: shortdesc}

```
go-action-hello/
└── src
    ├── hello
│   └── hello.go
    └── main
        └── main.go
```
{: screen}

通过此布局，可以导入子包 (`import "hello"`)，如以下示例中所示。

可以通过将 `GOPATH` 设置为 `src` 目录的父代来进行本地编译。如果使用的是 VSCode，那么需要将 `go.inferGopath` 设置启用为 `true`。
{: note}

`main/main.go` 的示例：

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

可以使用运行时环境进行编译。为 `src` 目录的内容创建 .zip 归档。**不要**包含顶级项目目录 `go-action-project/`。要创建 .zip 归档 `hello-src.zip`，请运行以下命令：

```bash
cd src
zip -r ../hello-src.zip *
cd ..
```
{: pre}

在该 .zip 归档的根目录中，编译 Go 可执行文件并将其打包成 `exec`。通过运行以下命令来构建 `hello-bin.zip` 归档。这假定您已在工作站上安装 Docker CLI，并且 `docker` 位于 `PATH` 中。

```bash
docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
```
{: pre}

在此示例中，主函数为 `-compile main`。要使用其他函数作为主函数，请更改 `-compile` 的值。主函数是在编译时选择的。预编译时，`ibmcloud fn action [update | create]` 会忽略 `--main`。

容器在 `stdin` 中获取源 .zip 的内容，编译这些内容，然后在根目录中创建包含可执行文件 `exec` 的新 .zip 归档。该 .zip 归档的内容会流出到 `stdout`，在其中会重定向到要部署为 Go 操作的 `hello-bin.zip` 归档。

现在，可以使用 CLI 和新的 .zip 归档 `hello-bin.zip` 来更新操作以用于生产。

```bash
ibmcloud fn action update helloGo hello-bin.zip --kind go:1.11
```
{: codeblock}

### 使用供应商库
{: #vendor-libs-go-actions}

编译 Go 操作时，可以通过在源 `zip` 归档中填充 `vendor` 目录来包含依赖项。`vendor` 目录不能在顶层工作。您需要将 `vendor` 目录放在 `src/` 内和包目录内。
{: shortdesc}

继续先前的示例，请使用 `hello.go` 中的日志包 `logrus`。

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

可以使用部署工具或脚本（如 [Makefile](#makefile)）来自动执行此过程。
{: tip}

### 将 Makefile 用于 Go 操作
{: #makefile}

继续先前的示例，您的项目目录如下所示。

```bash
go-action-hello/
├── Makefile
└── src
    ├── hello
│   ├── Gopkg.toml
    │   ├── hello.go
    │   └── vendor/
    └── main
        └── main.go
```
{: screen}

1. 创建 `Makefile` 文件以自动执行部署过程。

```Makefile
GO_COMPILER?=openwhisk/actionloop-golang-v1.11
CLI?=ibmcloud fn
MAIN=main
APP=hello
SRCS=$(MAIN)/$(MAIN).go $(APP)/$(APP).go
VENDORS=$(APP)/vendor
NAME=go-action-$(APP)
BINZIP=$(APP)-bin.zip
SRCZIP=$(APP)-src.zip

deploy: $(BINZIP)
	$(CLI) action update $(NAME) $(BINZIP) --main $(MAIN) --kind go:1.11

$(BINZIP): $(SRCZIP)
	docker run -i $(GO_COMPILER) -compile $(MAIN) <$(SRCZIP) >$(BINZIP)

$(SRCZIP): src/$(VENDORS)
	cd src ; zip ../$(SRCZIP) -r $(SRCS) $(VENDORS)

src/%/vendor:
	cd $(@D) ; DEPPROJECTROOT=$(realpath $(@D)/../..) dep ensure

clean:
	-rm -rf $(BINZIP) $(SRCZIP) $(VENDORS)

invoke:
	$(CLI) action invoke $(NAME) -r -p name Gopher
```
{: codeblock}

2. 删除 .zip 归档和 vendor 目录。

```bash
make clean
```
{: pre}

3. 填充 vendor 目录，创建源 .zip，编译源代码，将 exec 归档成 .zip，然后通过运行该归档来部署 Go 操作。

```bash
make deploy
```
{: pre}

4. 既然 `go-action-hello` 已创建，现在请调用操作。

```bash
ibmcloud fn action invoke go-action-hello -r -p name Go
```
{: pre}

示例输出：

```json
    {
        "greeting": "Hello Go"
    }
```
{: screen}
