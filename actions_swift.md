---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-08"

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

# Creating Swift actions
{: #creating-swift-actions}

Start new actions with or migrate existing actions to the Swift 4.2 runtime by using the `swift:4.2` kind and its compile process.
{: tip}

The following sections guide you through creating and invoking a single Swift action and packaging an action in a zip file.

**Note:** Swift actions run in a Linux environment. Swift on Linux is still in development, and {{site.data.keyword.openwhisk_short}} uses the latest available release. These releases might not be stable. The version of Swift that is used with {{site.data.keyword.openwhisk_short}} might be inconsistent with versions of Swift from stable releases of Xcode on MacOS.

For more information about the Swift runtime, see the Swift [reference](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#swift-actions).
{: tip}

## Creating and invoking a Swift action
{: #actions_swift_invoke}
{: #openwhisk_actions_swift_invoke}

### Swift 4
{: #actions_swift4_invoke}
{: #openwhisk_actions_swift4_invoke}

In addition to the main function signature, Swift 4 provides two more signatures that take advantage of the [Codable ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.apple.com/documentation/swift/codable) type. You can learn more about data types that are [encodable and decodable for compatibility with external representations such as JSON ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types). 

1. Save the following code in a file called `hello.swift`.

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

    This example takes an input parameter as `Codable Input` with field `name`, and returns a `Codable output` with a field `greetings`.

2. Create an action called `helloSwift`.

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.2
    ```
    {: pre}

3. Invoke the action.

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Example output:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Swift 3
{: #actions_swift3_invoke}
{: #openwhisk_actions_swift3_invoke}

An action is a top-level Swift function. To create a Swift 3 action:

1. Save the following code in a file called `hello.swift`.

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

    In this example, the Swift action consumes a dictionary and produces a dictionary.

2. Create an action called `helloSwift`.

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

3. Invoke the action.

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Example output:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Packaging an action as a Swift executable
{: #packaging-an-action-as-a-swift-executable}

When you create an {{site.data.keyword.openwhisk_short}} Swift action with a Swift source file, the file must be compiled into a binary before the action is run. This delay is known as the cold-start delay. Once the binary is created, subsequent calls to the action are much faster until the container holding your action is purged. To avoid the cold-start delay, you can compile your Swift file into a binary and then upload the binary to {{site.data.keyword.openwhisk_short}} in a .zip file.

### Compiling Swift 4.2 packaged actions
{: #actions_swift42_compile}
{: #openwhisk_actions_swift42_compile}

The Docker runtime includes a compiler to help users compile and package Swift 4.2 actions.

##### Compiling a single source file for Swift 4.2

Compile a single source file that doesn't depend on external libaries by using the following command.

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

The Docker container reads the content of the file from `stdin`, and writes a .zip archive with the compiled swift executable to `stdout`.

Use the flag `-compile` with the name of the main method.

The .zip archive is ready for deployment and invocation using the kind `swift:4.2`

```bash
wsk action update helloSwiftly hello.zip --kind swift:4.2
wsk action invoke helloSwiftly -r -p name World
```
{: codeblock}

#### Compiling dependencies and multi-file projects for Swift 4.2

To compile multiple files and include external dependencies create the following directory structure.

```
.
├── Package.swift
└── Sources
    └── main.swift
```
{: codeblock}

The directory `Sources/` should contain a file named `main.swift`.

The `Package.swift` should start with a comment specifying version `4.2` for the Swift tooling:

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

Create a .zip archive with the content of the directory:

```bash
zip ../action-src.zip -r *
```
{: codeblock}

Pass the .zip archive to the Docker container over `stdin`. The `stdout` will be a new .zip archive with the compiled executable.
The Docker container reads the content of the .zip archive from `stdin` and writes a new .zip archive with the compiled Swift executable to `stdout`.

```
docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
```
{: codeblock}

In a Linux based system you can combine the `zip` and `docker run` steps in a single command:

```
zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
```
{: codeblock}

The `action-bin.zip` archive is ready for deployment and invocation using the kind `swift:4.2`

```bash
wsk action update helloSwiftly action-bin.zip --kind swift:4.2
wsk action invoke helloSwiftly -r
```
{: codeblock}

### Compiling Swift 3.1.1 and 4.1 packaged actions

#### Using a script to build Swift 3.1.1 and 4.1 packaged actions

You can use a script to automate the packaging of the action.
{: shortdesc}

Before you begin, create a directory `actions` with each top level directory representing an action.

```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```
{: codeblock}

Create script `compile.sh` file the following.

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

- Create the `Package.swift` file to add dependencies.

The syntax changes based on the Swift runtime version.
Swift 3 example syntax.

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

  Swift 4 example syntax.
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

  In this example, `example-package-deckofplayingcards` is added as a dependency.
  Notice that `CCurl`, `Kitura-net` and `SwiftyJSON` are provided in the standard Swift action. Include them in your own `Package.swift` for Swift 3 actions.
  {: shortdesc}

1. Build the action by running the following command for a Swift 3 action.

  ```
  bash compile.sh hello swift:3.1.1
  ```
  {: pre}

  To compile for Swift 4 use `swift:4.1` instead of `swift:3.1.1`

  ```
  bash compile.sh hello swift:4.1
  ```
  {: pre}

  This process created `hello.zip` in the `build`.

2. Upload it to OpenWhisk with the action name helloSwifty.

  For Swift 3 use the kind `swift:3.1.1`

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:3.1.1
  ```
  {: pre}

  For Swift 4.1 use the kind `swift:4.1`

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:4.1
  ```
  {: pre}

3. Check how much faster it is by running the following command.

  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

  The time it took for the action to run is in the "duration" property and compare to the time it takes to run with a compilation step in the hello action.

## Error handling in Swift 4
{: #error-handling-swift4}

By using the Codable completion handler, you can pass an error to indicate a failure in your action. [Error handling in Swift ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) resembles exception handling in other languages, with the use of the `try`, `catch`, and `throw` keywords.
{: shortdesc}

The following snippet shows an example of handling an error.

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
