---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: actions, serverless, swift

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

# 建立 Swift 動作
{: #creating-swift-actions}

使用 `swift:4.2` 類型及其編譯處理程序，以使用 Swift 4.2 運行環境來啟動新的動作，或將現有動作移轉至 Swift 4.2 運行環境。
{: tip}

下列各節會引導您建立及呼叫單一 Swift 動作，並將動作包裝在 zip 檔案中。

**附註：**Swift 動作是在 Linux 環境中執行。Linux 上的 Swift 仍在開發中，而 {{site.data.keyword.openwhisk_short}} 會使用最新的可用版本。這些版本不一定穩定。與 {{site.data.keyword.openwhisk_short}} 搭配使用的 Swift 版本，可能與 MacOS 上穩定 Xcode 版本的 Swift 版本不一致。

如需 Swift 運行環境的相關資訊，請參閱[運行環境](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions)。
{: tip}

## 建立及呼叫 Swift 動作
{: #actions_swift_invoke}
{: #openwhisk_actions_swift_invoke}

### Swift 4
{: #actions_swift4_invoke}
{: #openwhisk_actions_swift4_invoke}

除了 main 函數簽章之外，Swift 4 還提供另外兩種充分運用 [Codable ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.apple.com/documentation/swift/codable) 類型的簽章。您可以進一步瞭解資料類型，而資料類型為[可編碼及可解碼以與外部表示法相容（例如 JSON ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types)）。 

1. 將下列程式碼儲存至稱為 `hello.swift` 的檔案中。

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

2. 建立稱為 `helloSwift` 的動作。
    

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.2
    ```
    {: pre}

3. 呼叫動作。
    

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    輸出範例：

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Swift 3
{: #actions_swift3_invoke}
{: #openwhisk_actions_swift3_invoke}

動作就是最上層 Swift 函數。若要建立 Swift 3 動作，請執行下列動作：

1. 將下列程式碼儲存至稱為 `hello.swift` 的檔案中。

    ```swift
func main(args: [String:Any]) -> [String:Any] {
    if let name = args["name"] as? String {
            return [ "greeting" : "Hello \(name)!" ]
        } else {
            return [ "greeting" : "Hello stranger!" ]
        }
    }
    ```
    {: codeblock}

    在此範例中，Swift 動作會取用字典，並產生字典。

2. 建立稱為 `helloSwift` 的動作。
    

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

3. 呼叫動作。
    

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    輸出範例：

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## 將動作包裝為 Swift 執行檔
{: #packaging-an-action-as-a-swift-executable}

當您使用 Swift 來源檔建立 {{site.data.keyword.openwhisk_short}} Swift 動作時，必須先將檔案編譯成二進位檔，才能執行動作。這項延遲稱為冷啟動延遲。建立二進位檔之後，後續呼叫動作時就會快很多，直到用來保存動作的容器被清除為止。若要避免冷啟動延遲，您可以將 Swift 檔案編譯成二進位檔，然後將二進位檔以 .zip 檔案形式上傳至 {{site.data.keyword.openwhisk_short}}。

### 編譯 Swift 4.2 包裝的動作
{: #actions_swift42_compile}
{: #openwhisk_actions_swift42_compile}

Docker 運行環境包括一個編譯器，可協助使用者編譯及包裝 Swift 4.2 動作。

##### 編譯 Swift 4.2 的單一原始檔

使用下列指令，以編譯未與外部程式庫相依的單一原始檔。

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

Docker 容器會從 `stdin` 讀取檔案的內容，並將具有已編譯 Swift 執行檔的 .zip 保存檔寫入至 `stdout`。

請搭配使用旗標 `-compile` 與 main 方法的名稱。

.zip 保存檔已備妥可使用類型 `swift:4.2` 進行部署及呼叫

```bash
wsk action update helloSwiftly hello.zip --kind swift:4.2
wsk action invoke helloSwiftly -r -p name World
```
{: codeblock}

#### 編譯 Swift 4.2 的相依關係及多檔案專案

若要編譯多個檔案並包括外部相依關係，請建立下列目錄結構。

```
.
├── Package.swift
└── Sources
    └── main.swift
```
{: codeblock}

目錄 `Sources/` 應該包含名為 `main.swift` 的檔案。

`Package.swift` 的開頭應該為指定 Swift 工具 `4.2` 版的註解：

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

建立含有目錄內容的 .zip 保存檔：

```bash
zip ../action-src.zip -r *
```
{: codeblock}

透過 `stdin` 將 .zip 保存檔傳遞至 Docker 容器。`stdout` 將會成為具有已編譯執行檔的新 .zip 保存檔。
Docker 容器會從 `stdin` 讀取 .zip 保存檔的內容，並將具有已編譯 Swift 執行檔的新 .zip 保存檔寫入至 `stdout`。

```
docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
```
{: codeblock}

在 Linux 型系統中，您可以在單一指令中結合 `zip` 及 `docker run` 步驟：

```
zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
```
{: codeblock}

`action-bin.zip` 保存檔已備妥可使用類型 `swift:4.2` 進行部署及呼叫

```bash
wsk action update helloSwiftly action-bin.zip --kind swift:4.2
wsk action invoke helloSwiftly -r
```
{: codeblock}

### 編譯 Swift 3.1.1 及 4.1 包裝的動作

#### 使用 Script 建置 Swift 3.1.1 及 4.1 包裝的動作

您可以使用 Script 來自動化動作的包裝。
{: shortdesc}

開始之前，請建立 `actions` 目錄，其中每個最上層目錄都代表一個動作。

```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```
{: codeblock}

使用下列指令，建立 Script `compile.sh` 檔案。

```bash
#!/bin/bash
set -ex

if [ -z "$1" ] ; then
        echo 'Error: Missing action name'
        exit 1
    fi
    if [ -z "$2" ] ; then
        echo 'Error: Missing kind, for example swift:4.1'
        exit 2
fi
OUTPUT_DIR="build"
if [ ${2} == "swift:3.1.1" ]; then
  BASE_PATH="/swift3Action"
  DEST_SOURCE="$BASE_PATH/spm-build"
  RUNTIME="openwhisk/action-swift-v3.1.1"
elif [ ${2} == "swift:4.1" ]; then
  RUNTIME="openwhisk/action-swift-v4.1"
  BASE_PATH="/swift4Action"
  DEST_SOURCE="/$BASE_PATH/spm-build/Sources/Action"
else
  echo "Error: Kind $2 not recognize"
  exit 3
fi
DEST_PACKAGE_SWIFT="$BASE_PATH/spm-build/Package.swift"

BUILD_FLAGS=""
if [ -n "$3" ] ; then
  BUILD_FLAGS=${3}
fi

echo "Using runtime $RUNTIME to compile swift"
    docker run --rm --name=compile-ow-swift -it -v "$(pwd):/owexec" $RUNTIME bash -ex -c "if [ -f \"/owexec/$OUTPUT_DIR/$1.zip\" ] ; then
        rm \"/owexec/$OUTPUT_DIR/$1.zip\"
    fiecho 'Setting up build...'
    cp /owexec/actions/$1/Sources/*.swift $DEST_SOURCE/# action file can be either {action name}.swift or main.swift
    if [ -f \"$DEST_SOURCE/$1.swift\" ] ; then
        echo 'renaming $DEST_SOURCE/$1.swift $DEST_SOURCE/main.swift'
        mv \"$DEST_SOURCE/$1.swift\" $DEST_SOURCE/main.swift
    fi
    # Add in the OW specific bits
    cat $BASE_PATH/epilogue.swift >> $DEST_SOURCE/main.swift
    echo '_run_main(mainFunction:main)' >> $DEST_SOURCE/main.swift# Only for Swift4
if [ ${2} != "swift:3.1.1" ]; then
  echo 'Adding wait to deal with escaping'
  echo '_ = _whisk_semaphore.wait(timeout: .distantFuture)' >> $DEST_SOURCE/main.swift
fi

echo \"Compiling $1...\"
    cd /$BASE_PATH/spm-build
    cp /owexec/actions/$1/Package.swift $DEST_PACKAGE_SWIFT
    # we have our own Package.swift, do a full compile
    swift build ${BUILD_FLAGS} -c releaseecho 'Creating archive $1.zip...'
    #.build/release/Action
    mkdir -p /owexec/$OUTPUT_DIR
    zip \"/owexec/$OUTPUT_DIR/$1.zip\" .build/release/Action
    "
    ```
{: codeblock}

- 建立 `Package.swift` 檔案，以新增相依關係。

語法會根據 Swift 運行環境版本進行變更。
Swift 3 範例語法。

  ```swift
  import PackageDescription

  let package = Package(
     name: "Action",
         dependencies: [
             .Package(url: "https://github.com/apple/example-package-deckofplayingcards.git", majorVersion: 3),
             .Package(url: "https://github.com/IBM-Swift/CCurl.git", "0.2.3"),
             .Package(url: "https://github.com/IBM-Swift/Kitura-net.git", "1.7.10"),
             .Package(url: "https://github.com/IBM-Swift/SwiftyJSON.git", "15.0.1"),
             .Package(url: "https://github.com/watson-developer-cloud/swift-sdk.git", "0.16.0")
         ]
   )
  ```
  {: codeblock}

  Swift 4 範例語法。
  {: codeblock}

  ```swift
  // swift-tools-version:4.0
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
              .package(url: "https://github.com/apple/example-package-deckofplayingcards.git", .upToNextMajor(from: "3.0.0"))
            ],
            targets: [
              .target(
                name: "Action",
                dependencies: ["DeckOfPlayingCards"],
                path: "."
              )
            ]
  )
  ```
  {: codeblock}

  在此範例中，`example-package-deckofplayingcards` 被新增為相依關係。請注意，標準 Swift 動作中會提供 `CCurl`、`Kitura-net` 及 `SwiftyJSON`。針對 Swift 3 動作，請將它們併入您自己的 `Package.swift` 中。
  {: shortdesc}

1. 針對 Swift 3 動作，執行下列指令，以建置動作。

  ```
      bash compile.sh hello swift:3.1.1
      ```
  {: pre}

  若要針對 Swift 4 進行編譯，請使用 `swift:4.1`，而非 `swift:3.1.1`

  ```
      bash compile.sh hello swift:4.1
      ```
  {: pre}

  此處理程序已在 `build` 中建立 `hello.zip`。

2. 使用動作名稱 helloSwifty，將它上傳至 OpenWhisk。

  針對 Swift 3，請使用 `swift:3.1.1` 類型

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:3.1.1
  ```
  {: pre}

  針對 Swift 4.1，請使用 `swift:4.1` 類型

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:4.1
  ```
  {: pre}

3. 執行下列指令，以檢查其速度快多少。

  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

  動作所需的執行時間位於 "duration" 內容中，並且與使用 hello 動作中的編譯步驟執行所需的時間進行比較。

## Swift 4 中的錯誤處理
{: #error-handling-swift4}

藉由使用 Codable 完成處理程式，您可以傳遞錯誤來指出動作中的失敗。[Swift 中的錯誤處理 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) 類似於其他語言的異常狀況處理，使用 `try`、`catch` 及 `throw` 等關鍵字。
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
