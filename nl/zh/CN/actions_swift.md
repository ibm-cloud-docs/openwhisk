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

# 创建 Swift 操作
{: #creating-swift-actions}

通过使用 `swift:4.2` kind 及其编译过程，利用 Swift 4.2 运行时启动新的操作，或将现有操作迁移到该运行时。
{: tip}

以下各部分将指导您创建并调用单个 Swift 操作，以及将该操作打包成 zip 文件。

**注：**Swift 操作在 Linux 环境中运行。Linux 上的 Swift 仍在开发中；{{site.data.keyword.openwhisk_short}} 会使用最新可用的发行版。但这些发行版可能不稳定。此外，用于 {{site.data.keyword.openwhisk_short}} 的 Swift 版本可能与 macOS 上 Xcode 的稳定发行版中的 Swift 版本不一致。

有关 Swift 运行时的更多信息，请参阅[运行时](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions)。
{: tip}

## 创建并调用 Swift 操作
{: #actions_swift_invoke}
{: #openwhisk_actions_swift_invoke}

### Swift 4
{: #actions_swift4_invoke}
{: #openwhisk_actions_swift4_invoke}

除了 main 函数特征符外，Swift 4 中还提供了两个利用 [Codable ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.apple.com/documentation/swift/codable) 类型的特征符。您可以了解有关[可编码和可解码以与 JSON 等外部表示法兼容 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types) 的数据类型的更多信息。 

1. 将以下代码保存在名为 `hello.swift` 的文件中。

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

2. 创建名为 `helloSwift` 的操作。
    

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.2
    ```
    {: pre}

3. 调用操作。
    

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    示例输出：

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Swift 3
{: #actions_swift3_invoke}
{: #openwhisk_actions_swift3_invoke}

操作是顶级 Swift 函数。要创建 Swift 3 操作，请执行以下操作：

1. 将以下代码保存在名为 `hello.swift` 的文件中。

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

    在此示例中，Swift 操作会使用一个字典并生成一个字典。

2. 创建名为 `helloSwift` 的操作。
    

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

3. 调用操作。
    

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    示例输出：

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## 将操作打包成 Swift 可执行文件
{: #packaging-an-action-as-a-swift-executable}

使用 Swift 源文件创建 {{site.data.keyword.openwhisk_short}} Swift 操作后，必须先将该文件编译成二进制文件，才能运行该操作。此延迟称为冷启动延迟。创建二进制文件后，对该操作的后续调用会快得多，直到清除保存该操作的容器为止。要避免冷启动延迟，可以将 Swift 文件编译为二进制文件，然后将该二进制文件以 .zip 文件格式上传到 {{site.data.keyword.openwhisk_short}}。

### 编译 Swift 4.2 打包操作
{: #actions_swift42_compile}
{: #openwhisk_actions_swift42_compile}

Docker 运行时包含一个编译器，用于帮助用户编译和打包 Swift 4.2 操作。

##### 针对 Swift 4.2 编译单个源文件

使用以下命令来编译不依赖于外部库的单个源文件。

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

Docker 容器会从 `stdin` 中读取文件的内容，并将包含已编译 Swift 可执行文件的 .zip 归档写入 `stdout`。

将 `-compile` 标志与 main 方法的名称一起使用。

.zip 归档已准备就绪，可使用 kind `swift:4.2` 进行部署和调用。

```bash
wsk action update helloSwiftly hello.zip --kind swift:4.2
wsk action invoke helloSwiftly -r -p name World
```
{: codeblock}

#### 针对 Swift 4.2 编译依赖项和多文件项目

要编译多个文件并包含外部依赖项，请创建以下目录结构。

```
.
├── Package.swift
└── Sources
    └── main.swift
```
{: codeblock}

`Sources/` 目录应该包含名为 `main.swift` 的文件。

`Package.swift` 的开头应该是用于为 Swift 工具指定版本 `4.2` 的注释：

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

创建包含该目录内容的 .zip 归档：

```bash
zip ../action-src.zip -r *
```
{: codeblock}

通过 `stdin` 将 .zip 归档传递到 Docker 容器。`stdout` 将成为包含已编译可执行文件的新 .zip 归档。Docker 容器会从 `stdin` 中读取 .zip 归档的内容，并将包含已编译 Swift 可执行文件的新 .zip 归档写入 `stdout`。

```
docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
```
{: codeblock}

在基于 Linux 的系统中，可以将 `zip` 和 `docker run` 步骤合并到单个命令中：

```
zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
```
{: codeblock}

`action-bin.zip` 归档已准备就绪，可使用 kind `swift:4.2` 进行部署和调用。

```bash
wsk action update helloSwiftly action-bin.zip --kind swift:4.2
wsk action invoke helloSwiftly -r
```
{: codeblock}

### 编译 Swift 3.1.1 和 4.1 打包操作

#### 使用脚本来构建 Swift 3.1.1 和 4.1 打包操作

可以使用脚本来自动对操作打包。
{: shortdesc}

开始之前，请为每个顶级目录创建一个表示操作的目录 `actions`。

```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```
{: codeblock}

创建如下 `compile.sh` 脚本文件。

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
docker run --rm --name=compile-ow-swift -it -v "$(pwd):/owexec" $RUNTIME bash -ex -c "

if [ -f \"/owexec/$OUTPUT_DIR/$1.zip\" ] ; then
    rm \"/owexec/$OUTPUT_DIR/$1.zip\"
fi

echo 'Setting up build...'
cp /owexec/actions/$1/Sources/*.swift $DEST_SOURCE/

# action file can be either {action name}.swift or main.swift
if [ -f \"$DEST_SOURCE/$1.swift\" ] ; then
    echo 'renaming $DEST_SOURCE/$1.swift $DEST_SOURCE/main.swift'
    mv \"$DEST_SOURCE/$1.swift\" $DEST_SOURCE/main.swift
fi
# Add in the OW specific bits
cat $BASE_PATH/epilogue.swift >> $DEST_SOURCE/main.swift
echo '_run_main(mainFunction:main)' >> $DEST_SOURCE/main.swift

# Only for Swift4
if [ ${2} != "swift:3.1.1" ]; then
  echo 'Adding wait to deal with escaping'
  echo '_ = _whisk_semaphore.wait(timeout: .distantFuture)' >> $DEST_SOURCE/main.swift
fi

echo \"Compiling $1...\"
cd /$BASE_PATH/spm-build
cp /owexec/actions/$1/Package.swift $DEST_PACKAGE_SWIFT
# we have our own Package.swift, do a full compile
swift build ${BUILD_FLAGS} -c release

echo 'Creating archive $1.zip...'
#.build/release/Action
mkdir -p /owexec/$OUTPUT_DIR
zip \"/owexec/$OUTPUT_DIR/$1.zip\" .build/release/Action
"
```
{: codeblock}

- 创建 `Package.swift` 文件以添加依赖项。

语法会根据 Swift 运行时版本而变化。Swift 3 示例语法。

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

  Swift 4 示例语法。
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

  在此示例中，`example-package-deckofplayingcards` 将添加为依赖项。
  请注意，标准 Swift 操作中提供了 `CCurl`、`Kitura-net` 和 `SwiftyJSON`。对于 Swift 3 操作，请在您自己的 `Package.swift` 中包含这些项。
  {: shortdesc}

1. 对于 Swift 3 操作，请通过运行以下命令来构建操作。

  ```
  bash compile.sh hello swift:3.1.1
  ```
  {: pre}

  要对 Swift 4 进行编译，请使用 `swift:4.1`，而不是 `swift:3.1.1`。

  ```
  bash compile.sh hello swift:4.1
  ```
  {: pre}

  此过程在 `build` 中创建了 `hello.zip`。

2. 使用名为 helloSwifty 的操作将其上传到 OpenWhisk。

  对于 Swift 3，请使用 kind `swift:3.1.1`。

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:3.1.1
  ```
  {: pre}

  对于 Swift 4.1，请使用 kind `swift:4.1`。

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:4.1
  ```
  {: pre}

3. 通过运行以下命令来检查速度快了多少。

  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

  运行操作所用时间会显示在“duration”属性中，并与在 hello 操作中使用编译步骤运行所用的时间进行比较。

## Swift 4 中的错误处理
{: #error-handling-swift4}

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
