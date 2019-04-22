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

# Swift アクションの作成
{: #creating-swift-actions}

種類として `swift:4.2` を指定し、そのコンパイル・プロセスを使用すれば、Swift 4.2 ランタイムで新しいアクションを開始したり、既存のアクションを Swift 4.2 ランタイムにマイグレーションしたりできます。
{: tip}

1 つの Swift アクションを作成して呼び出したり、zip ファイルにアクションをパッケージしたりする時には、以下の各セクションを参考にしてください。

**注:** Swift アクションは Linux 環境で実行されます。 Linux 上の Swift はまだ開発中なので、{{site.data.keyword.openwhisk_short}} では最新の使用可能なリリースが使用されています。 これらのリリースは安定していないかもしれません。 {{site.data.keyword.openwhisk_short}} で使用されている Swift のバージョンは、MacOS 上の Xcode の安定リリースに含まれている Swift のバージョンとは整合性がない場合があります。

Swift ランタイムについて詳しくは、[ランタイム](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions)を参照してください。
{: tip}

## Swift アクションの作成と呼び出し
{: #actions_swift_invoke}
{: #openwhisk_actions_swift_invoke}

### Swift 4
{: #actions_swift4_invoke}
{: #openwhisk_actions_swift4_invoke}

main 関数のシグニチャーに加えて、Swift 4 では、さらに [Codable ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.apple.com/documentation/swift/codable) タイプを利用したシグニチャーを 2 つ用意しています。 [JSON などの外部表現との互換性を保つために使用できるデータ型 encodable と decodable ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types) の詳細についても確認できます。 

1. 以下のコードを `hello.swift` という名前のファイルに保存します。

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

    この例では、フィールド `name` を使用し `Codable Input` として入力パラメーターを受け取り、フィールド `greeting` を使用し `Codable output` を返しています。

2. `helloSwift` というアクションを作成します。

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.2
    ```
    {: pre}

3. アクションを呼び出します。

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    出力例:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Swift 3
{: #actions_swift3_invoke}
{: #openwhisk_actions_swift3_invoke}

アクションは、トップレベルの Swift 関数です。 Swift 3 のアクションを作成するには、以下のようにします。

1. 以下のコードを `hello.swift` という名前のファイルに保存します。

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

    この例では、Swift アクションはディクショナリーを取り込み、ディクショナリーを生成します。

2. `helloSwift` というアクションを作成します。

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

3. アクションを呼び出します。

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    出力例:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Swift 実行可能ファイルとしてのアクションのパッケージ化
{: #packaging-an-action-as-a-swift-executable}

{{site.data.keyword.openwhisk_short}} Swift アクションを Swift のソース・ファイルを使用して作成する際は、アクションの実行前にそのファイルをコンパイルしてバイナリーにしておく必要があります。 この遅延は、コールド・スタートの遅延と呼ばれます。 バイナリーが作成されると、アクションを保持しているコンテナーがパージされるまで、以降のそのアクションの呼び出しは大幅に高速になります。 コールド・スタートの遅延を回避するため、Swift ファイルをコンパイルしてバイナリーにし、そのバイナリーを .zip ファイルにして {{site.data.keyword.openwhisk_short}} にアップロードできます。

### Swift 4.2 のパッケージ化アクションのコンパイル
{: #actions_swift42_compile}
{: #openwhisk_actions_swift42_compile}

Docker ランタイムには、ユーザーが Swift 4.2 アクションをコンパイルしてパッケージするのに役立つコンパイラーが組み込まれています。

##### Swift 4.2 の 1 つのソース・ファイルのコンパイル

以下のコマンドを使用して、外部ライブラリーに依存していない 1 つのソース・ファイルをコンパイルします。

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

Docker コンテナーが `stdin` からファイルの内容を読み取り、コンパイルした swift 実行可能ファイルを組み込んだ .zip アーカイブを `stdout` に書き込みます。

`-compile` フラグを使用して main メソッドの名前を指定します。

.zip アーカイブの準備ができたので、`swift:4.2` という種類を使用してデプロイメントと呼び出しを実行できます

```bash
wsk action update helloSwiftly hello.zip --kind swift:4.2
wsk action invoke helloSwiftly -r -p name World
```
{: codeblock}

#### Swift 4.2 の依存項目と複数ファイル・プロジェクトのコンパイル

複数のファイルをコンパイルして外部の依存項目を組み込むには、以下のディレクトリー構造を作成します。

```
.
├── Package.swift
└── Sources
    └── main.swift
```
{: codeblock}

ディレクトリー `Sources/` に `main.swift` という名前のファイルを組み込む必要があります。

`Package.swift` の先頭に、Swift ツールのバージョン `4.2` を指定するコメントを付ける必要があります。

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

ディレクトリーの内容を組み込んだ .zip アーカイブを作成します。

```bash
zip ../action-src.zip -r *
```
{: codeblock}

`stdin` を使用して .zip アーカイブを Docker コンテナーに渡します。`stdout` が、コンパイルした実行可能ファイルを組み込んだ新しい .zip アーカイブになります。
Docker コンテナーが `stdin` から .zip アーカイブの内容を読み取り、コンパイルした Swift 実行可能ファイルを組み込んだ新しい .zip アーカイブを `stdout` に書き込みます。

```
docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
```
{: codeblock}

Linux ベースのシステムでは、`zip` と `docker run` のステップを 1 つのコマンドで結合できます。

```
zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
```
{: codeblock}

`action-bin.zip` アーカイブの準備ができたので、`swift:4.2` という種類を使用してデプロイメントと呼び出しを実行できます

```bash
wsk action update helloSwiftly action-bin.zip --kind swift:4.2
wsk action invoke helloSwiftly -r
```
{: codeblock}

### Swift 3.1.1 と 4.1 のパッケージ化アクションのコンパイル

#### スクリプトを使用した Swift 3.1.1 と 4.1 のパッケージ化アクションのビルド

スクリプトを使用して、アクションのパッケージ化を自動化できます。
{: shortdesc}

始めに、ディレクトリー `actions` を作成し、アクションを表す最上位ディレクトリーをそれぞれ組み込みます。

```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```
{: codeblock}

以下のようにして、`compile.sh` というスクリプト・ファイルを作成します。

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

- `Package.swift` ファイルを作成して依存項目を追加します。

Swift ランタイムのバージョンに基づいて構文が変わります。
以下は Swift 3 の構文例です。

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

  以下は Swift 4 の構文例です。
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

  この例では、`example-package-deckofplayingcards` を依存項目として追加しています。
  `CCurl`、`Kitura-net`、`SwiftyJSON` は標準の Swift アクションに用意されています。Swift 3 アクションの場合は、それらの項目を独自の `Package.swift` に組み込んでください。
  {: shortdesc}

1. Swift 3 アクションの場合は、以下のコマンドを実行してアクションをビルドします。

  ```
  bash compile.sh hello swift:3.1.1
  ```
  {: pre}

  Swift 4 でコンパイルする場合は、`swift:3.1.1` の代わりに `swift:4.1` を使用します

  ```
  bash compile.sh hello swift:4.1
  ```
  {: pre}

  このプロセスで、`hello.zip` が `build` に作成されます。

2. アクション名 helloSwifty を使用して OpenWhisk にアップロードします。

  Swift 3 では、種類として `swift:3.1.1` を使用します

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:3.1.1
  ```
  {: pre}

  Swift 4.1 では、種類として `swift:4.1` を使用します

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:4.1
  ```
  {: pre}

3. 以下のコマンドを実行して、処理速度の違いを確認します。

  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

  アクションの実行にかかった時間は「duration」プロパティーにあります。hello アクションでコンパイル・ステップを含む実行にかかる時間と比較してください。

## Swift 4 でのエラー処理
{: #error-handling-swift4}

Codable の完了ハンドラーを使用して、アクション内での失敗を示すエラーを渡すことができます。 [Swift でのエラー処理 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) は他言語の例外処理に似ており、`try`、`catch`、および `throw` の各キーワードを使用します。
{: shortdesc}

以下のスニペットは、エラー処理の例を示しています。

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
