---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-10"

keywords: actions, serverless, javascript, node, node.js, functions

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
{:external: target="_blank" .external}


# 準備 APP 以用於動作
{: #prep}

無論您是使用自帶應用程式，還是撰寫專門用於回應事件的 Script，您的程式碼都必須符合特定需求，然後才能透過程式碼建立動作。
{: shortdesc}

每種程式設計語言都有特定的執行需求，但大多數程式設計語言具有下列一般需求：
- 依預設，程式碼進入點的預期名稱為 `main`。如果進入點不是 `main`，可以在建立動作時指定自訂名稱，因此請記下該名稱。
- 進入應用程式的輸入參數和來自應用程式的輸出結果的格式，必須設定為可在實體之間傳遞的特定結構。結構取決於程式碼語言。例如，對於 Python APP，輸入參數必須是字典，並且 APP 的結果必須建構為字典。因為還可以將結構化物件中的參數傳遞給動作。例如，在 JSON 中，可以將程式碼建構為預期使用特定欄位（如 `name` 和 `place`）中 JSON 值的輸入參數。

    **JSON 輸入範例**
    ```json
    {"name": "Dorothy", "place": "Kansas"}
    ```
    {: codeblock}

    **JavaScript 範例**
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
    }
    ```
    {: codeblock}
- 如果應用程式包含多個檔案，必須將這些檔案合併成一個檔案，以便在動作中使用。可以將程式碼重寫成一個檔案，也可以將檔案和相依關係包裝成單一保存檔。如果不支援運行環境，可以將應用程式包裝成 Docker 映像檔。
- 還必須將相依關係與應用程式包裝在一起。可用的運行環境隨附一些預先安裝的套件和延伸。請[檢查運行環境的參考資訊](/docs/openwhisk?topic=cloud-functions-runtimes)，以瞭解運行環境是否已隨附應用程式的相依關係。如果已包含相依關係，則不必將其與應用程式包裝在一起。

    程式碼編譯不是必要的，但在針對運行環境可行的情況下，事先編譯程式碼可以提高性能。
    {: tip}

## 準備 Docker 映像檔形式的 APP
{: #prep_docker}

透過 {{site.data.keyword.openwhisk_short}}，可以使用任何語言撰寫應用程式，並將其包裝成 Docker 映像檔。
{: shortdesc}

可以僅使用公用登錄中的映像檔，例如 Docker Hub 上公開可用的映像檔。不支援專用登錄。
{: important}

### 將程式碼包裝成 Docker 映像檔
{: #prep_docker_pkg}

您的程式碼會編譯為執行檔，並內嵌到 Docker 映像檔中。執行檔透過採用來自 `stdin` 的輸入並透過 `stdout` 進行回覆，來與系統互動。
{: shortdesc}

**開始之前**
- 您必須具備 Docker Hub 帳戶。可以在 [Docker Hub](https://hub.docker.com){: external} 上設定免費 Docker ID 和帳戶。
- [安裝 Docker](https://hub.docker.com/search/?offering=community&type=edition){:external}。
- [檢閱 Docker 運行環境的需求](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker)。

若要包裝您的應用程式，請完成下列步驟。

若要將程式碼包裝成 Docker 映像檔，請執行下列指令。
1. 下載並安裝 Docker 架構。架構是一個 Docker 容器範本，您可以在其中以自訂二進位檔形式來注入程式碼。
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. 在 Blackbox 架構中設定程式碼。此架構包含您可以使用的 C 程式。`example.c` 檔案的一部分會在 Docker 映像檔建置程序之中編譯，因此不需要在機器上編譯 C。
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  **輸出範例**
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. （選用）透過修改 `Dockerfile` 將更多程式碼及相依關係新增至 Docker 映像檔，以建置執行檔。請注意下列需求：
  * 程式碼必須位於容器內的 `/action/exec` 中。
  * 執行檔會從指令行收到單一引數。這個引數是代表動作引數之 JSON 物件的字串序列化。
  * 程式可能會記錄到 `stdout` 或 `stderr`。
  * 根據慣例，輸出的最後一行必須是<ph class="ignoreSpelling">字串化</ph>的 JSON 物件，用於代表操作結果。如需建構 Dockerfile 的相關資訊，請參閱 [Dockerfile 參照](https://docs.docker.com/engine/reference/builder/){: external}。

4. 建置 Docker 映像檔，並使用提供的 Script 予以上傳。
    1. 登入 Docker。
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. 導覽至 `dockerSkeleton` 目錄。
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. 執行 Script。
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}




## 準備 JavaScript APP
{: #prep_js}

建立動作之前，請準備好 JavaScript 程式碼。確認程式碼結構是否正確，然後決定程式碼是否需要進行包裝。
{: shortdesc}

### 建構 JavaScript 程式碼
{: #prep_js_struct}

- 進入點函數的預期名稱為 `main`。如果程式碼中的函數不是 `main`，請記下名稱以便在建立動作時指定它。
- 輸入參數會以 JSON 物件來傳遞。
- 順利啟動的結果也是 JSON 物件，但會根據動作是[同步](#prep_js_sync)還是[非同步](#prep_js_async)，而以不同的方式傳回。



**範例**
```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```

**使用多個函數的範例**

  ```javascript
  function main() {
      return { payload: helper() }
  }

  function helper() {
      return new Date();
}
```
  {: codeblock}


### 建構具有同步行為的 JavaScript 程式碼
{: #prep_js_sync}

如果 main 函數不執行 `return` 陳述式就結束，或者結束時執行的 `return` 陳述式傳回了除 Promise 以外的任何值，則 JavaScript 啟動是同步的。
{: shortdesc}

**同步程式碼範例。**

```javascript
// each path results in a synchronous activation
function main(params) {
  if (params.payload == 0) {return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
    return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}




### 建構具有非同步行為的 JavaScript 程式碼
{: #prep_js_async}

JavaScript 函數即使在傳回之後，仍可以在回呼函數中繼續執行。如果 main 函數透過傳回 Promise 結束，則 JavaScript 啟動是非同步的。在此情況下，系統假設動作仍在執行，直到履行或已拒絕 Promise 為止。透過在動作中傳回 Promise，非同步執行的 JavaScript 函數可在 `main` 函數傳回後傳回啟動結果。
{: shortdesc}

首先，實例化新 Promise 物件並向其傳遞回呼函數。此回呼接受兩個引數（resolve 及 reject），而這兩者同時也是函數。所有非同步程式碼都在該回呼內。動作處理程式可以有任何名稱，只要符合接受物件以及傳回物件的慣用簽章（或物件的 `Promise`）即可。

在下列範例中，可以查看如何透過呼叫 resolve 函數來履行 Promise。
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
            resolve({ done: true });
         }, 2000);
      })
   }
```
{: codeblock}

以下範例顯示如何透過呼叫 reject 函數來拒絕 Promise。
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
            reject({ done: true });
       }, 2000);
     })
}
```
{: codeblock}

在範例中，執行的詳細資料如下。
* `main` 函數傳回 Promise。Promise 指示啟動尚未完成，而是預期在未來完成。
* `setTimeout()` JavaScript 函數等待 2 秒再呼叫 Promise 的回呼函數，這代表非同步程式碼。
* Promise 的回呼接受引數 `resolve` 和 `reject`，這兩個引數都是函數。
  * 對 `resolve()` 的呼叫履行 Promise 並指示啟動已正常完成。
  * 對 `reject()` 的呼叫可用於拒絕 Promise 並指示啟動異常完成。


### 建構具有同步和非同步行為的 JavaScript 程式碼
{: #prep_js_both}

一個動作可以針對某些輸入是同步的，而針對其他輸入是非同步的，如下列範例中所示。
{: shortdesc}

```javascript
function main(params) {
    if (params.payload) {
        // asynchronous activation
        return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
         }, 2000);
      })
   }  else {
        // synchronous activation
        return {done: true};
     }
}
```
{: codeblock}





### 使用 JavaScript 呼叫外部 API 的範例
{: #prep_js_api}

下列範例呼叫 NASA Astronomy Picture of the Day (APOD) 服務的外部 API，用於每天提供我們宇宙的獨特影像。
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

呼叫 NASA APOD API，即會從 JSON 結果擷取欄位。

接下來，[建立](/docs/openwhisk?topic=cloud-functions-actions)並[呼叫動作](/docs/openwhisk?topic=cloud-functions-test)以對其測試。會傳回下列範例物件：

```
{
  "copyright": "Eric Houck",
  "date": "2018-03-28",
  "explanation": "Does an alignment like this occur only once in a blue moon? ...",
  "hdurl": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_1799.jpg",
  "media_type": "image",
  "service_version": "v1",
  "title": "Blue Moon Tree",
  "url": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_960.jpg"
}
```
{: screen}






### 使用 `webpack` 模組對 JavaScript 程式碼包裝
{: #prep_js_pkg}

可以使用 JavaScript 模組連結器（如 `[webpack ](https://webpack.js.org/concepts/){: external}`）來包裝應用程式。當 `webpack` 處理您的程式碼時，它會遞迴地建置相依關係圖形，其中包含您的動作所需的每個模組。
{: shortdesc}

開始之前，[請檢閱 JavaScript 運行環境一起包含的套件](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments)，以查看您的應用程式相依關係是否已包含在運行環境。如果未包含您的相依關係，您必須將它與應用程式一起包裝。

1. 建立 `package.json` 檔案。將 `webpack` 新增為開發相依關係。

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

2. 將下列 `webpack` 配置程式碼儲存至名稱為 `webpack.config.js` 的檔案中。

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

3. 準備應用程式程式碼。在此範例（可以將其儲存為名稱為 `index.js` 的檔案）中，`global.main` 變數設定為應用程式的 main 函數。

    **範例**
    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
        const lines = args.lines || [];
        return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    global.main = myAction;
    ```
    {: codeblock}

4. 在本端安裝所有相依關係。

    ```
    npm install
    ```
    {: pre}

5. 建置 `webpack` 組合。

    ```
    npm run build
    ```
    {: pre}

    會建立 `dist/bundle.js` 檔案，並部署為動作原始碼。

6. 使用 `npm` Script 或 CLI 來建立動作。

    * 執行下列 `npm` Script。

        ```
        npm run deploy
        ```
        {: pre}

    * 執行下列 CLI 指令。

        ```
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    `webpack` 所建置的組合檔案僅支援 JavaScript 相依關係。因此，如果軟體組具有其他相依關係，則動作呼叫可能會失敗，因為 `bundle.js` 檔案中不包含這些相依關係。
    {: tip}



### 將 JavaScript 程式碼包裝成 NPM 檔案
{: #prep_js_npm}

使用單一 JavaScript 原始檔撰寫所有動作碼的替代方案，是您可以將程式碼包裝成 .zip 檔案中的 `npm` 套件。
{: shortdesc}

開始之前，[請檢閱 JavaScript 運行環境一起包含的套件](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments)，以查看您的應用程式相依關係是否已包含在運行環境。如果未包含您的相依關係，您必須將它與應用程式一起包裝。

1. 在根目錄中，建立 `package.json` 檔案。 

**範例**

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

2. 在本端安裝所有相依關係。

    ```
    npm install DEPENDENCY
    ```
    {: pre}

    雖然大部分 `npm` 套件會在執行 `npm install` 時安裝 JavaScript 原始碼，但還有些 npm 套件會安裝並編譯二進位檔構件。保存檔上傳只支援 JavaScript 相依關係。如果保存包含二進位檔相依關係，則動作呼叫可能不會成功。
    {: note}

3. 建立包含所有檔案（包括所有相依關係）的 `.zip` 保存。

    ```
    zip -r action.zip *
    ```
    {: pre}

    **Windows 使用者**：使用 Windows 瀏覽器動作來建立 .zip 檔案將導致檔案結構不正確。{{site.data.keyword.openwhisk_short}} .zip 動作必須在該保存的根目錄中具有 `package.json`，但 Windows 瀏覽器會將其放入巢狀資料夾內。請改為使用 `zip` 指令。
    {: tip}





## 準備 Go 應用程式
{: #prep_go}

使用單一檔案來進行快速測試或開發。若為正式作業應用程式，請將 Go 動作前置編譯為執行檔，以取得較佳的效能或多個原始檔支援（包括供應商程式庫）。
{: shortdesc}

雖然可以透過使用 `GOOS=Linux` 和 `GOARCH=amd64` 進行交叉編譯以在任何 Go 平台上建立壓縮檔，但還是請使用運行環境容器映像檔中內嵌的預先編譯特性。您可以包裝[多個原始檔](#prep_go_multi)或[供應商程式庫](#prep_go_vendor)。
{: tip}


### 建構 Go 程式碼
{: #prep_go_struct}

- 進入點套件的預期名稱為 `main`。如果程式碼中的套件不是 `main`，請記下套件名稱，以在建立動作時指定該名稱。
- 該套件必須是公用套件。

**範例**
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

### 包裝多個 Go 原始檔
{: #prep_go_multi}

1. 建立最上層 `src` 目錄。將屬於 main 套件的原始檔放在 `src` 的根目錄或 `main` 目錄內，並為其他套件建立子目錄。例如，`hello` 套件會變成 `src/hello` 目錄。
  ```
  go-action-hello/
  └── src
      ├── hello
      │   └── hello.go
      └── main
          └── main.go
  ```
  {: screen}

2. 匯入子套件。用於匯入 hello 子套件的 `main/main.go` 的範例。

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

  `hello/hello.go` 的範例。

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

3. 編譯程式碼。為 `src` 目錄建立 .zip 保存檔。不要包含最上層專案目錄 `go-action-project/`。

  ```bash
  cd src
  zip -r ../hello-src.zip *
  cd ..
  ```
  {: pre}

  可以透過將 `GOPATH` 設定為 `src` 目錄的上層目錄來進行本端編譯。如果使用的是 VS Code，則必須將 `go.inferGopath` 設定變更為 `true`。
  {: note}

4. 編譯 Go 執行檔，並將它包裝為 .zip 保存檔根目錄中的 `exec`。執行下列指令，以建置 `hello-bin.zip` 保存檔。您必須已在工作站中安裝 Docker CLI，並將 `docker` 置於 `PATH` 中。

  ```bash
  docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
  ```
  {: pre}

  在此範例中，main 函數是 `-compile main`。若要使用與 main 不同的函數，請變更 `-compile` 的值。在編譯時會選取 main 函數。前置編譯時，`ibmcloud fn action [update | create]` 會忽略 `--main`。

  容器會以 `stdin` 取得來源 .zip 的內容，編譯內容，然後使用根目錄中的執行檔 `exec` 來建立新的 .zip 保存檔。.zip 保存檔內容會向外串流至 `stdout`，以重新導向至要部署為「Go 動作」的 `hello-bin.zip` 保存檔。




### 包裝 Go 程式碼與供應商程式庫
{: #prep_go_vendor}

編譯 Go 檔案時，可以透過在來源 `zip` 保存檔中移入 `vendor` 目錄來包含相依關係。`vendor` 目錄在最上層無法運作。您必須將 `vendor` 目錄放在 `src/` 內和套件目錄內。
{: shortdesc}

`hello.go` 應用程式中的範例日誌套件 `logrus`。

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
在此範例中，`vendor` 目錄位於 `src/hello/vendor` 中。您可以新增 `hello` 套件所使用的協力廠商程式庫。 

您可以使用多個工具（如 [<code>dep</code> ](https://golang.github.io/dep/docs/installation.html){: external}）來移入和管理相依關係。

您可以建立說明程式庫版本及位置的檔案 `src/main/Gopkg.toml`，來使用 `dep`。

```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

移入 `vendor` 目錄，並執行 `dep ensure`。




## 準備 Swift APP
{: #prep_swift}

Swift 檔案必須先進行編譯，然後才能執行動作。這項延遲稱為冷啟動延遲。若要避免冷啟動延遲，可以先編譯 Swift 檔案，然後將該檔案以 .zip 檔案格式上傳到 {{site.data.keyword.openwhisk_short}}。Docker 運行環境包括一個編譯器，可協助使用者編譯及包裝 Swift 4.2 動作。對該動作的後續呼叫會快得多，直到清除保存該動作的容器為止。

Swift 動作在 Linux 環境中執行。Linux 上的 Swift 仍在開發中，而 {{site.data.keyword.openwhisk_short}} 會使用最新的可用版本。這些版本不一定穩定。此外，用於 {{site.data.keyword.openwhisk_short}} 的 Swift 版本可能與 macOS 上 Xcode 的穩定版本中的 Swift 版本不一致。
{: important}


### 建構 Swift 程式碼
{: #prep_swift_struc}

進入點函數的預期名稱為 `main`。如果程式碼中的函數不是 `main`，請記下名稱以便在建立動作時指定它。

除了 main 函數簽章外，Swift 4 中還提供了兩個利用 [<code>Codable</code> ](https://developer.apple.com/documentation/swift/codable){: external} 類型的簽章。您可以瞭解有關[可編碼和可解碼以與 JSON 等外部表示法相容](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types){: external}的資料類型的更多資訊。

**範例**
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

此範例接受輸入參數作為具有 `name` 欄位的 `Codable 輸入`，並傳回具有 `greetings` 欄位的 `Codable 輸出`。


#### 處理 Swift 中的錯誤
{: #prep_swift_error}

透過使用 `Codable` 完成處理程式，可以傳遞錯誤以指出該動作失敗。[Swift 中的錯誤處理](https://docs.swift.org/swift-book/LanguageGuide/ErrorHandling.html){: external}與其他語言中使用 `try`、`catch` 和 `throw` 關鍵字的異常狀況處理類似。
{: shortdesc}

下列 Snippet 顯示處理錯誤的範例。

```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    // Return real error
    do{
        throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
    } catch {
        completion(nil, error)
    }
}
```
{: codeblock}


### 包裝 Swift 4.2 檔案
{: #prep_swift42_single}

編譯不依賴於外部程式庫的單一原始檔。請搭配使用旗標 `-compile` 與 main 方法的名稱。

**開始之前**
- [安裝 Docker](https://hub.docker.com/search/?offering=community&type=edition){: external}。
- [檢閱 Swift 運行環境一起包含的套件](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions)，以查看您的應用程式相依關係是否已包含在運行環境。如果未包含您的相依關係，您必須將它與應用程式一起包裝。

包裝您的應用程式。

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

Docker 容器會從 `stdin` 讀取檔案的內容，並將具有已編譯 Swift 執行檔的 .zip 保存檔寫入至 `stdout`。



### 包裝 Swift 4.2 多檔案專案和相依關係
{: #prep_swift42_multi}

**開始之前**
- [安裝 Docker](https://hub.docker.com/search/?offering=community&type=edition){: external}。
- [檢閱 Swift 運行環境一起包含的套件](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions)，以查看您的應用程式相依關係是否已包含在運行環境。如果未包含您的相依關係，您必須將它與應用程式一起包裝。

包裝您的應用程式。

1. 若要編譯多個檔案並包含外部相依關係，請建立下列目錄結構。

  ```
  .
  ├── Package.swift
  └── Sources
      └── main.swift
  ```
  {: codeblock}

  `Sources/` 目錄包含名稱為 `main.swift` 的檔案。

  `Package.swift` 的開頭必須是用於為 Swift 工具指定版本 `4.2` 的註解：

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

2. 建立包含該目錄內容的 .zip 保存檔。

  ```bash
  zip ../action-src.zip -r *
  ```
  {: codeblock}

3. 透過 `stdin` 將 .zip 保存檔傳遞至 Docker 容器。`stdout` 是包含已編譯執行檔的新 .zip 保存檔。Docker 容器會從 `stdin` 讀取 .zip 保存檔的內容，並將具有已編譯 Swift 執行檔的新 .zip 保存檔寫入至 `stdout`。

  ```
  docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
  ```
  {: codeblock}

  在 Linux 型系統中，可以將 `zip` 和 `docker run` 步驟合併到單一指令中：

  ```
  zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
  ```
  {: codeblock}





## 包裝 Python APP
{: #prep_python}


### 建構 Python 程式碼
{: #prep_python_struct}

- Python APP 必須使用一個字典並產生一個字典。
- 進入點方法的預期名稱為 `main`。如果程式碼中的函數不是 `main`，請記下名稱以便在建立動作時指定它。
{: shortdesc}

**範例**
```python
def main(args):
	name = args.get("name", "stranger")
	greeting = "Hello " + name + "!"
	print(greeting)
    return {"greeting": greeting}
```

### 包裝 Python 程式碼
{: #prep_python_pkg}

將 Python 程式碼和相依模組包裝成 .zip 檔案。在此範例中，包含進入點的原始檔為 `__main__.py`，Helper 模組位於名為 `helper.py` 的檔案中。

開始之前，[請檢閱 Python 運行環境一起包含的套件](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments)，以查看您的應用程式相依關係是否已包含在運行環境。如果未包含您的相依關係，您必須將它與應用程式一起包裝。

若要包裝您的應用程式，請執行下列指令。

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}


### 將虛擬環境的 Python 程式碼包裝成 .zip 檔案
{: #prep_python_virtenv}

您可以使用虛擬環境 `virtualenv` 來包裝 Python 相依關係。透過使用虛擬環境，您可以鏈結可使用 [`pip`](https://packaging.python.org/tutorials/installing-packages/){: external} 安裝的更多套件。

開始之前，[請檢閱 Python 運行環境一起包含的套件](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments)，以查看您的應用程式相依關係是否已包含在運行環境。如果未包含您的相依關係，您必須將它與應用程式一起包裝。

透過完成下列步驟來包裝應用程式。

1. 建立包含要安裝的 `pip` 模組和版本的 [requirements.txt](https://pip.pypa.io/en/latest/user_guide/#requirements-files){: external} 檔案。

  若要使 `virtualenv` 保持最小大小，請僅將不屬於所選運行環境的模組新增到 `requirements.txt` 中。如需 Python 運行環境中所含套件的相關資訊，請參閱 Python [運行環境參考資料](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments)。
  {: tip}

2. 為運行環境取回下列其中一個映像檔。為了確保與運行環境容器的相容性，虛擬環境內部的包裝安裝必須使用與所指定運行環境對應的映像檔。
    * 對於 `python:3.7`，請使用 Docker 映像檔 `ibmfunctions/action-python-v3.7`。
    * 對於 `python:3.6`，請使用 Docker 映像檔 `ibmfunctions/action-python-v3.6`。
    * 對於 `python:2`，請使用 Docker 映像檔 `openwhisk/python2action`。

   **範例**
   ```
   docker pull ibmfunctions/action-python-v3.7
   ```
   {: pre}

2. 安裝相依關係並建立虛擬環境。虛擬環境目錄必須命名為 `virtualenv`。

   ```
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. 包裝 `virtualenv` 目錄及任何其他 Python 檔案。包含進入點的原始檔必須名稱為 `__main__.py`。

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}


## 準備 Ruby APP
{: #prep_ruby}

建立動作之前，請準備好 Ruby 程式碼。

### 建構 Ruby 程式碼
{: #prep_ruby_struct}

* Ruby 動作一律使用「雜湊」（字典式集合），並傳回「雜湊」。
* 進入點函數的預期名稱為 `main`。如果程式碼中的函數不是 `main`，請記下名稱以便在建立動作時指定它。


**範例**

```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
```
{: codeblock}

### 包裝 Ruby 程式碼
{: #prep_ruby_pkg}

可以將 Ruby 應用程式和相依套件包裝成 .zip 檔案。例如，您可以使用第二個稱為 `helper.rb` 的檔案來包裝動作。

建立包含原始檔的保存。包含進入點的原始檔必須命名為 `main.rb`。

```bash
zip -r helloRuby.zip main.rb helper.rb
```
{: pre}

除了預設及組合 Gem 之外，還提供 Gem `mechanize` 及 `jwt`。只要使用壓縮動作來包裝所有相依關係，您就可以使用任意 Gem。



## 準備 PHP APP
{: #prep_php}

建立動作之前，請準備好 PHP 程式碼。

### 建構 PHP 程式碼
{: #prep_php_struct}

- PHP 動作一律使用聯合陣列，並傳回聯合陣列。
- 進入點函數的預期名稱為 `main`。如果程式碼中的函數不是 `main`，請記下名稱以便在建立動作時指定它。

**範例**
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

### 包裝 PHP 程式碼
{: #prep_php_pkg}

可以將 PHP 檔案或相依套件包裝成 .zip 檔案。

開始之前，請[檢閱 PHP 運行環境隨附的套件](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php)，以瞭解運行環境是否已隨附應用程式的相依關係。如果未包含您的相依關係，您必須將它與應用程式一起包裝。

若要包裝您的應用程式，請執行下列指令。

```bash
zip -r ARCHIVE_NAME.zip FILE_1.php FILE_2.php
```
{: pre}

**範例**
```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

## 準備 Java APP
{: #prep_java}

建立動作之前，請準備好 Java 程式碼。

### 建構 Java 程式碼
{: #prep_java_struct}

Java 動作是一種 Java 程式，搭配稱為 `main` 的方法。`main` 必須具有下列簽章。

**範例**
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

* 您必須使用 `--main` 來指定 main 類別的名稱。有資格的 main 類別是實作 static `main` 方法的類別。如果類別不在預設套件中，請使用 Java 完整類別名稱，例如，`--main com.example.MyMain`。
* 可以透過指定動作的標準方法名稱來自訂 Java 動作的方法名稱，例如 `--main com.example.MyMain#methodName`。

### 包裝 Java 程式碼
{: #prep_java_pkg}


**開始之前**
您必須已在本端安裝 [JDK 8](http://openjdk.java.net/install/){: external}。此範例使用的是 [`google-gson-2.8.5.jar`](http://central.maven.org/maven2/com/google/code/gson/gson/2.8.5/){: external}。

如果使用除 JDK 8 以外的 JDK 版本，則在使用 `javac` 指令編譯程式碼時，必須指定 `--release 8`。
{: note}

若要建立 Java 動作，請完成下列步驟。

1. 將下列程式碼儲存至名稱為 `Hello.java` 的檔案中。

  ```java
  import com.google.gson.JsonObject;
  public class Hello {
      public static JsonObject main(JsonObject args) {
          String name = "stranger";
          if (args.has("name"))
              name = args.getAsJsonPrimitive("name").getAsString();
          JsonObject response = new JsonObject();
          response.addProperty("greeting", "Hello, " + name + "!");
          return response;
      }
  }
  ```
  {: codeblock}

2. 下載 [`gson-2.8.5.jar`](http://central.maven.org/maven2/com/google/code/gson/gson/2.8.5/)。

3. 將 `gson-2.8.5.jar` 新增到 `ClASSPATH`。此範例使用的 `gson-2.8.5.jar` 儲存在 `Desktop` 目錄的 `test` 資料夾中。
  
  ```
  export CLASSPATH=$CLASSPATH:/Users/Desktop/test/gson-2.8.5.jar
  ```
  {: pre}

4. 將 JDK 的 `bin` 資料夾新增到 `CLASSPATH`。此範例使用的是 `openjdk-8`。
  
  ```
  export CLASSPATH=$CLASSPATH:/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/bin
  ```
  {: pre}

5. 驗證 JDK `bin` 資料夾和 `gson-2.8.5.jar` 是否位於 `CLASSPATH` 中。
  ```
  echo $CLASSPATH
  ```
  {: pre}

  **輸出範例**
  ```
  /Desktop/test/gson-2.8.5.jar:/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/bin
  ```
  {: screen}

6. 導覽至儲存 `Hello.java` 檔案的資料夾。在此範例中，`Hello.java` 檔案儲存到 `Desktop/test` 資料夾中。
  ```
  cd Desktop/test
  ```
  {: pre}

7. 將 `Hello.java` 檔案編譯成類別檔。
  ```
  javac Hello.java
  ```
  {: pre}

8. 將該類別檔壓縮成名稱為 `hello.jar` 的 .Jar 檔。

  ```
  jar cvf hello.jar Hello.class
  ```
  {: pre}

**後續步驟**
您可以使用 `hello.jar` 來建立動作。由於您建立的類別檔未使用預設名稱 `main`，因此建立動作時，必須將 `--main` 旗標設定為 `Hello`。`--main` 旗標必須與 Java `class` 相符合。如需相關資訊，請參閱[建立動作](/docs/openwhisk?topic=cloud-functions-actions)。
 
更新 Java 程式碼時，必須重複這些步驟以將程式碼重新編譯成新的 `.jar` 檔案。
{: note}

### 使用 Gradle 包裝 Java 程式碼
{: #prep_java_gradle}

您可以使用建置工具（例如：[Gradle](https://gradle.org){: external}）來提取儲存庫（如 Maven Central）中的儲存庫，而不是從指令行進行編譯。您可以使用 Gradle 來提取並建置包含程式碼和所有相依關係的最終 .jar 保存檔。

以下範例使用 Gradle 建置可運用 `com.google.zxing` 程式庫的 Java 動作，而此程式庫提供功能來產生 QR 碼影像。

1. 建立名稱為 `build.gradle` 的檔案並指定相依關係。

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

2. 執行 `gradle jar` 指令，這將在 `build/libs/` 目錄中產生 .jar 保存檔。

  如需相關資訊，請閱讀 Gradle 文件：[宣告相依關係](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies){: external}。


## 準備 .NET Core APP
{: #prep_dotnet}

建立動作之前，請準備好 .NET Core 程式碼。

### 建構 .NET Core 程式碼
{: #prep_dotnet_struct}

.NET Core 動作是一個 .NET Core 類別庫，使用預期名稱為 `Main` 的方法。如果程式碼中的方法不是 `Main`，請記下方法名稱，以在建立動作時指定該名稱，其格式為：`--main {Assembly}::{Class Full Name}::{Method}`

**範例**
```
Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main
```

### 包裝 .NET Core 程式碼
{: #prep_dotnet_pkg}

**開始之前**
若要編譯、測試和保存 .NET Core 專案，必須執行下列動作：
- 在本端安裝 [.NET Core SDK](https://dotnet.microsoft.com/download){: external}。
- 將 `DOTNET_HOME` 環境變數設定為可以找到 `dotnet` 執行檔的位置。



若要包裝程式碼，請執行下列指令。

  1. 建立稱為 `Apache.OpenWhisk.Example.Dotnet` 的 C# 專案。

      ```bash
      dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
      ```
      {: pre}

  2. 導覽至 `Apache.OpenWhisk.Example.Dotnet` 目錄。

      ```bash
      cd Apache.OpenWhisk.Example.Dotnet
      ```
      {: pre}

  3. 安裝 [<ph class="ignoreSpelling">Newtonsoft.Json NuGet</ph> 套件](https://www.nuget.org/packages/Newtonsoft.Json/){: external}。

      ```bash
      dotnet add package Newtonsoft.Json -v 12.0.1
      ```
      {: pre}

  4. 將下列程式碼儲存至名稱為 `Hello.cs` 的檔案中。

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

  5. 編譯 `Hello.cs` 及任何其他檔案，並輸出至 `out` 目錄。

      ```bash
      dotnet publish -c Release -o out
      ```
      {: pre}

  6. 導覽至 `out` 目錄。

      ```bash
      cd out
      ```
      {: pre}

  7. 壓縮已發佈的檔案。

      ```bash
      zip -r -0 ../helloDotNet.zip *
      ```
      {: pre}



