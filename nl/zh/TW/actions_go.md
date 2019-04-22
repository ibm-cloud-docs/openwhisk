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

# 建立 Go 動作
{: #creating-go-actions}

下列各節會引導您建立及呼叫單一 Go 動作，並且將參數新增至該動作。您可以使用 Go 1.11 來執行 Go 動作。若要使用此運行環境，請在建立或更新動作時指定 `ibmcloud fn` CLI 參數 `--kind go:1.11`。
{: shortdesc}

## 建立及呼叫 Go 動作
{: #invoking-go-actions}

Go 動作只是 `main` 套件中的公用函數。使用單一檔案來進行快速測試或開發。若為正式作業應用程式，請[將 Go 動作前置編譯為執行檔](#packaging-go-actions)，以取得較佳的效能或多個原始檔支援（包括供應商程式庫）。
{: shortdesc}

建立 Go 動作。

1. 將下列程式碼儲存至名為 `hello.go` 的檔案中。在此範例中，函數的名稱為 `Main`，但您可以使用 `--main` 旗標來變更名稱。如果您重新命名函數，則名稱必須以大寫字母開頭，且不能是 Go 套件名稱 `main` 這個術語。

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
    `Main` 函數的預期簽章為：

    ```go
    func Main(event map[string]interface{}) map[string]interface{}
    ```
    {: codeblock}

2. 建立或更新稱為 `helloGo` 的動作。

    ```bash
    ibmcloud fn action update helloGo hello.go --kind go:1.11
    ```
    {: pre}

3. 呼叫動作。
    

    ```bash
    ibmcloud fn action invoke --result helloGo --param name gopher
    ```
    {: pre}

    輸出範例：
    ```json
      {
          "greeting": "Hello gopher!"
      }
    ```
    {: screen}

## 將動作包裝為 Go 執行檔
{: #packaging-go-actions}

雖然您可以使用 `GOOS=Linux` 及 `GOARCH=amd64` 交互編譯以在任何 Go 平台上建立二進位檔，但請使用運行環境容器映像檔中所內嵌的前置編譯特性。您可以包裝[多個原始檔](#multiple-packages-go-actions)或[供應商程式庫](#vendor-libs-go-actions)。
{: shortdesc}

### 使用多個套件原始檔
{: #multiple-packages-go-actions}

若要使用多個套件原始檔，請使用最上層 `src` 目錄，並將屬於 main 套件的原始檔放在 `src` 根目錄或 `main` 目錄內，然後建立其他套件的目錄。例如，`hello` 套件會變成 `src/hello` 目錄。
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

使用此佈置，您可以匯入子套件 (`import "hello"`)，如下列範例所示。

您可以將 `GOPATH` 設為 `src` 目錄的上層目錄，以在本端編譯。如果您使用 VSCode，則需要啟用設為 `true` 的 `go.inferGopath` 設定。
{: note}

`main/main.go` 範例：

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

`hello/hello.go` 範例：

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

您可以使用運行環境來編譯。建立包含 `src` 目錄內容的 .zip 保存檔。請**不要**包括最上層專案目錄 `go-action-project/`。若要建立 .zip 保存檔 `hello-src.zip`，請執行下列動作：

```bash
cd src
zip -r ../hello-src.zip *
cd ..
```
{: pre}

編譯 Go 執行檔，並將它包裝為 .zip 保存檔根目錄中的 `exec`。執行下列指令，以建置 `hello-bin.zip` 保存檔。這假設您已在工作站以及 `PATH` 的 `docker` 中安裝 Docker CLI。

```bash
docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
```
{: pre}

在此範例中，main 函數是 `-compile main`。若要使用與 main 不同的函數，請變更 `-compile` 的值。
在編譯時會選取 main 函數。前置編譯時，`ibmcloud fn action [update | create]` 會忽略 `--main`。

容器會以 `stdin` 取得來源 .zip 的內容，編譯內容，然後使用根目錄中的執行檔 `exec` 來建立新的 .zip 保存檔。.zip 保存檔內容會向外串流至 `stdout`，以重新導向至要部署為「Go 動作」的 `hello-bin.zip` 保存檔。

現在，您可以使用 CLI 及新的 .zip 保存檔 `hello-bin.zip` 來更新適用於正式作業的動作。

```bash
ibmcloud fn action update helloGo hello-bin.zip --kind go:1.11
```
{: codeblock}

### 使用供應商程式庫
{: #vendor-libs-go-actions}

當您編譯「Go 動作」時，可以在來源 `zip` 保存檔內移入 `vendor` 目錄，以併入相依關係。`vendor` 目錄在最上層無法運作。您需要將 `vendor` 目錄置於 `src/` 及套件目錄內。
{: shortdesc}

繼續進行前一個範例時，請使用 `hello.go` 中的日誌套件 `logrus`。

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
在此範例中，`vendor` 目錄位於 `src/hello/vendor` 中。您可以新增 `hello` 套件所使用的協力廠商程式庫。您可以使用多個工具（例如 [dep ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://golang.github.io/dep/docs/installation.html)）來移入及管理相依關係。

您可以建立說明程式庫版本及位置的檔案 `src/main/Gopkg.toml`，來使用 `dep`。

```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

移入 `vendor` 目錄，並執行 `dep ensure`。

您可以使用部署工具或 Script（例如 [Makefile](#makefile)）來自動化此處理程序。
{: tip}

### 使用 Makefile 以進行 Go 動作
{: #makefile}

繼續進行前一個範例時，您的專案目錄如下。

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

1. 建立 `Makefile` 檔案，以自動化部署處理程序。

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

2. 刪除 .zip 保存檔及 vendor 目錄。

```bash
make clean
```
{: pre}

3. 移入 vendor 目錄、建立來源 .zip、編譯原始碼、將 exec 保存到 .zip，然後執行下列指令來部署 Go 動作。

```bash
make deploy
```
{: pre}

4. 現在，已建立 `go-action-hello`，請呼叫動作。

```bash
ibmcloud fn action invoke go-action-hello -r -p name Go
```
{: pre}

輸出範例：

```json
    {
        "greeting": "Hello Go"
    }
```
{: screen}
