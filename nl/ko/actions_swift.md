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

# Swift 액션 작성
{: #creating-swift-actions}

새 액션을 시작하거나 `swift:4.2` 유형 및 해당 컴파일 프로세스를 사용하여 기존 액션을 Swift 4.2 런타임에 마이그레이션하십시오.
{: tip}

다음 절에서는 단일 Swift 액션 작성 및 호출과 zip 파일에 액션 패키징에 대해 안내합니다.

**참고:** Swift 액션은 Linux 환경에서 실행됩니다. Swift on Linux는 아직 개발 중이며 {{site.data.keyword.openwhisk_short}}에서는 최신 사용 가능 릴리스를 사용합니다. 이 릴리스는 안정적이지 않을 수 있습니다. {{site.data.keyword.openwhisk_short}}에서 사용되는 Swift의 버전이 MacOS에서 안정적인 XCode 릴리스의 Swift 버전과 일치하지 않을 수 있습니다.

Swift 런타임에 대한 자세한 정보는 [런타임](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions)을 참조하십시오.
{: tip}

## Swift 액션 작성 및 호출 
{: #actions_swift_invoke}
{: #openwhisk_actions_swift_invoke}

### Swift 4
{: #actions_swift4_invoke}
{: #openwhisk_actions_swift4_invoke}

기본 기능 시그니처에 추가하여, Swift 4에서는 [Codable ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.apple.com/documentation/swift/codable) 유형을 활용하는 2개의 추가 시그니처를 제공합니다. [JSON ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 등의 외부 표현과의 호환성을 위해 인코딩 및 디코딩 가능한](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types) 데이터 유형에 대해 자세히 알아볼 수 있습니다. 

1. `hello.swift` 파일에 다음 코드를 저장하십시오.

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

    이 예제에서는 `name` 필드에서 `Codable Input`으로 입력 매개변수를 취하고 `greetings` 필드에서 `Codable output`을 리턴합니다.

2. `helloSwift`라고 하는 액션을 작성하십시오.

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.2
    ```
    {: pre}

3. 액션을 호출하십시오.

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    출력 예:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Swift 3
{: #actions_swift3_invoke}
{: #openwhisk_actions_swift3_invoke}

액션은 최상위 레벨의 Swift 함수입니다. Swift 3 액션을 작성하려면 다음을 수행하십시오.

1. `hello.swift` 파일에 다음 코드를 저장하십시오.

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

    이 예제에서 Swift 액션은 사전을 이용하고 사전을 생성합니다.

2. `helloSwift`라고 하는 액션을 작성하십시오.

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

3. 액션을 호출하십시오.

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    출력 예:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## 액션을 Swift 실행 파일로 패키징
{: #packaging-an-action-as-a-swift-executable}

Swift 소스 파일로 {{site.data.keyword.openwhisk_short}} Swift 액션을 작성하는 경우, 해당 파일은 액션이 실행되기 전에 2진으로 컴파일되어야 합니다. 이 지연을 콜드 스타트 지연이라고 합니다. 일단 2진이 작성되면, 액션을 보유한 컨테이너가 제거될 때까지 액션에 대한 후속 호출이 훨씬 더 빠릅니다. 콜드 스타트 지연을 피하기 위해 Swift 파일을 2진으로 컴파일한 후에 .zip 파일로 {{site.data.keyword.openwhisk_short}}에 2진을 업로드할 수 있습니다. 

### Swift 4.2 패키지된 액션 컴파일
{: #actions_swift42_compile}
{: #openwhisk_actions_swift42_compile}

Docker 런타임에는 사용자가 Swift 4.2 액션을 컴파일 및 패키징하는 데 도움이 되는 컴파일러가 포함되어 있습니다.

##### Swift 4.2에 대한 단일 소스 파일 컴파일 

다음 명령을 사용하여 외부 라이브러리에 의존하지 않는 단일 소스 파일을 컴파일하십시오. 

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

Docker 컨테이너는 `stdin`에서 파일의 컨텐츠를 읽고, 컴파일된 swift 실행 파일로 .zip 아카이브를 `stdout`에 기록합니다.

기본 메소드의 이름으로 `-compile` 플래그를 사용하십시오. 

.zip 아카이브는 `swift:4.2` 유형을 사용하여 배치 및 호출을 준비합니다. 

```bash
wsk action update helloSwiftly hello.zip --kind swift:4.2
wsk action invoke helloSwiftly -r -p name World
```
{: codeblock}

#### Swift 4.2에 대한 종속 항목 및 다중 파일 프로젝트 컴파일 

다중 파일을 컴파일하고 외부 종속 항목을 포함시키려면 다음 디렉토리 구조를 작성하십시오. 

```
.
├── Package.swift
└── Sources
    └── main.swift
```
{: codeblock}

`Sources/` 디렉토리에 이름이 `main.swift`인 파일이 포함되어야 합니다.

`Package.swift`는 Swift 도구용으로 버전 `4.2`를 지정하는 주석으로 시작해야 합니다.

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

디렉토리의 컨텐츠를 사용하여 .zip 아카이브를 작성하십시오. 

```bash
zip ../action-src.zip -r *
```
{: codeblock}

`stdin`을 통해 .zip 아카이브를 Docker 컨테이너에 전달하십시오. `stdout`는 컴파일된 실행 파일이 있는 새 .zip 아카이브가 됩니다.
Docker 컨테이너는 .zip 아카이브의 컨텐츠를 `stdin`에서 읽고 새 .zip 아카이브를 컴파일된 Swift 실행 파일로 `stdout`에 기록합니다.

```
docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
```
{: codeblock}

Linux 기반 시스템에서는 `zip` 및 `docker run` 단계를 단일 명령으로 결합할 수 있습니다. 

```
zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
```
{: codeblock}

`action-bin.zip` 아카이브는 `swift:4.2` 유형을 사용하여 배치 및 호출을 준비합니다. 

```bash
wsk action update helloSwiftly action-bin.zip --kind swift:4.2
wsk action invoke helloSwiftly -r
```
{: codeblock}

### Swift 3.1.1 및 4.1 패키지 액션 컴파일

#### Swift 3.1.1 및 4.1 패키지 액션 빌드를 위해 스크립트 사용

스크립트를 사용하여 액션 패키징을 자동화할 수 있습니다.
{: shortdesc}

시작하기 전에 액션을 나타내는 각 최상위 디렉토리와 함께 `actions` 디렉토리를 작성하십시오. 

```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```
{: codeblock}

다음과 같이 스크립트 `compile.sh` 파일을 작성하십시오. 

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

- `Package.swift` 파일을 작성하여 종속 항목을 추가하십시오. 

이 구문은 Swift 런타임 버전을 기반으로 변경됩니다.
Swift 3 예제 구문.

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

  Swift 4 예제 구문.
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

  이 예에서는 `example-package-deckofplayingcards`가 종속 항목으로 추가됩니다.
  `CCurl`, `Kitura-net` 및 `SwiftyJSON`이 표준 Swift 액션에 제공됨에 유의하십시오. Swift 3 액션에 대해 자체 `Package.swift`에 이를 포함시키십시오.
  {: shortdesc}

1. Swift 3 액션에 대해 다음 명령을 실행하여 액션을 빌드하십시오. 

  ```
  bash compile.sh hello swift:3.1.1
  ```
  {: pre}

  Swift 4를 컴파일하려면 `swift:3.1.1` 대신 `swift:4.1`을 사용하십시오. 

  ```
  bash compile.sh hello swift:4.1
  ```
  {: pre}

  이 프로세스는 `build`에서 `hello.zip`을 작성했습니다.

2. 액션 이름 helloSwifty로 이를 OpenWhisk에 업로드하십시오. 

  Swift 3의 경우 `swift:3.1.1` 유형을 사용하십시오. 

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:3.1.1
  ```
  {: pre}

  Swift 4.1의 경우 `swift:4.1` 유형을 사용하십시오. 

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:4.1
  ```
  {: pre}

3. 다음 명령을 실행하여 실행 속도가 얼마나 빨라지는지 확인하십시오. 

  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

  액션 실행에 걸린 시간은 "지속 기간" 특성에 있으며, 시작 액션에서 컴파일 단계로 실행하는 데 걸리는 시간과 비교됩니다. 

## Swift 4의 오류 처리
{: #error-handling-swift4}

Codable 완료 핸들러를 사용하면 액션의 실패를 표시하는 오류를 전달할 수 있습니다. [Swift의 오류 처리 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)는 `try`, `catch` 및 `throw` 키워드의 사용으로 기타 언어의 예외 처리와 유사합니다.
{: shortdesc}

다음 스니펫은 오류 처리의 예를 표시합니다. 

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
