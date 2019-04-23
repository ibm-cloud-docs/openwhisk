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

# Swift-Aktionen erstellen
{: #creating-swift-actions}

Starten Sie neue Aktionen mit der Swift 4.2-Laufzeit oder migrieren Sie vorhandene Aktionen damit, indem Sie `swift:4.2` und den zugehörigen Kompilierungsprozess verwenden.
{: tip}

In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen Swift-Aktion sowie zum Packen einer Aktion in eine ZIP-Datei beschrieben. 

**Hinweis:** Swift-Aktionen werden in einer Linux-Umgebung ausgeführt. Swift unter Linux befindet sich noch in der Entwicklung und {{site.data.keyword.openwhisk_short}} verwendet das neueste verfügbare Release. Diese Releases sind möglicherweise nicht stabil. Darüber hinaus ist es möglich, dass die mit {{site.data.keyword.openwhisk_short}} verwendete Version von Swift nicht mit den Versionen von Swift aus stabilen Releases von Xcode unter MacOS konsistent ist.

Weitere Informationen zur Swift-Laufzeit finden Sie unter [Laufzeiten](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions).
{: tip}

## Swift-Aktion erstellen und aufrufen
{: #actions_swift_invoke}
{: #openwhisk_actions_swift_invoke}

### Swift 4
{: #actions_swift4_invoke}
{: #openwhisk_actions_swift4_invoke}

Neben der Hauptfunktionssignatur stellt Swift 4 zwei weitere Signaturen bereit, die den [codierbaren Typ ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.apple.com/documentation/swift/codable) nutzen. Erfahren Sie mehr über Datentypen, die [zur Kompatibilität mit externen Darstellungen wie JSON codierbar und decodierbar sind ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types).  

1. Speichern Sie den folgenden Code in einer Datei namens `hello.swift`. 

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

    Dieses Beispiel verwendet einen Eingabeparameter als `Codable-Eingabe` mit dem Feld `name` und gibt eine `Codable-Ausgabe` mit einem Feld `greetings` zurück.

2. Erstellen Sie eine Aktion namens `helloSwift`.

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.2
    ```
    {: pre}

3. Rufen Sie die Aktion auf.

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Beispielausgabe:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Swift 3
{: #actions_swift3_invoke}
{: #openwhisk_actions_swift3_invoke}

Eine Aktion ist eine Swift-Funktion der höchsten Ebene. So erstellen Sie eine Swift 3-Aktion:

1. Speichern Sie den folgenden Code in einer Datei namens `hello.swift`. 

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

    In diesem Beispiel liest die Swift-Aktion ein Wörterverzeichnis (Dictionary) und sie generiert ein Wörterverzeichnis.

2. Erstellen Sie eine Aktion namens `helloSwift`.

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

3. Rufen Sie die Aktion auf.

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Beispielausgabe:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Aktion als ausführbare Swift-Datei packen
{: #packaging-an-action-as-a-swift-executable}

Wenn Sie eine {{site.data.keyword.openwhisk_short}}-Swift-Aktion mit einer Swift-Quellendatei erstellen, muss die Datei in eine Binärdatei kompiliert werden, bevor die Aktion ausgeführt wird. Diese Verzögerung wird als Kaltstartverzögerung bezeichnet. Nachdem die Binärdatei erst einmal erstellt worden ist, werden Aufrufe der Aktion viel schneller durchgeführt, bis der Container, der die Aktion enthält, bereinigt wird. Zur Vermeidung der Kaltstartverzögerung können Sie Ihre Swift-Datei in eine Binärdatei kompilieren und diese anschließend in einer ZIP-Datei in {{site.data.keyword.openwhisk_short}} hochladen.

### Mit Swift 4.2 gepackte Aktionen kompilieren
{: #actions_swift42_compile}
{: #openwhisk_actions_swift42_compile}

Die Docker-Laufzeit enthält einen Compiler, der Sie beim Kompilieren und Packen von Swift 4.2-Aktionen unterstützt. 

##### Einzelne Quellendatei für Swift 4.2 kompilieren

Kompilieren Sie eine einzelne Quellendatei ohne Abhängigkeiten von externen Bibliotheken mithilfe des folgenden Befehls. 

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

Der Docker-Container liest den Inhalt der Datei aus `stdin` und schreibt ein ZIP-Archiv mit der kompilierten ausführbaren Swift-Datei in `stdout`. 

Verwenden Sie das Flag `-compile` mit dem Namen der Hauptmethode. 

Das ZIP-Archiv kann mit `swift:4.2` bereitgestellt und aufgerufen werden. 

```bash
wsk action update helloSwiftly hello.zip --kind swift:4.2
wsk action invoke helloSwiftly -r -p name World
```
{: codeblock}

#### Abhängigkeiten und Projekte mit mehreren Dateien für Swift 4.2 kompilieren

Erstellen Sie die folgende Verzeichnisstruktur, um mehrere Dateien zu kompilieren und externe Abhängigkeiten einzuschließen. 

```
.
├── Package.swift
└── Sources
    └── main.swift
```
{: codeblock}

Das Verzeichnis `Sources/` sollte eine Datei namens `main.swift` enthalten. 

Die Datei `Package.swift` sollte mit einem Kommentar beginnen, der die Version `4.2` für die Swift-Tools angibt: 

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

Erstellen Sie ein ZIP-Archiv mit dem Inhalt des Verzeichnisses: 

```bash
zip ../action-src.zip -r *
```
{: codeblock}

Übergeben Sie das ZIP-Archiv an den Docker-Container über `stdin`. `stdout` wird zum neuen ZIP-Archiv mit der kompilierten ausführbaren Datei.
Der Docker-Container liest den Inhalt des ZIP-Archivs aus `stdin` und schreibt ein neues ZIP-Archiv mit der kompilierten ausführbaren Swift-Datei in `stdout`. 

```
docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
```
{: codeblock}

Auf einem Linux-basierten System können Sie die Schritte `zip` und `docker run` in einem einzigen Befehl kombinieren: 

```
zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
```
{: codeblock}

Das Archiv `action-bin.zip` kann mit `swift:4.2` bereitgestellt und aufgerufen werden. 

```bash
wsk action update helloSwiftly action-bin.zip --kind swift:4.2
wsk action invoke helloSwiftly -r
```
{: codeblock}

### Mit Swift 3.1.1 und 4.1 gepackte Aktionen kompilieren

#### Script verwenden, um mit Swift 3.1.1 und 4.1 gepackte Aktionen zu erstellen

Sie können ein Script verwenden, um das Packen der Aktion zu automatisieren.
{: shortdesc}

Erstellen Sie zunächst ein Verzeichnis `actions`, wobei jedes Verzeichnis der höchsten Ebene eine Aktion darstellt. 

```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```
{: codeblock}

Erstellen Sie die Scriptdatei `compile.sh` wie folgt. 

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

- Erstellen Sie die Datei `Package.swift`, um Abhängigkeiten hinzuzufügen. 

Die Syntax ändert sich auf der Basis der Swift-Laufzeitversion.
Swift 3 - Beispielsyntax. 

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

  Swift 4 - Beispielsyntax.
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

  In diesem Beispiel wird `example-package-deckofplayingcards` als Abhängigkeit hinzugefügt.
Beachten Sie, dass `CCurl`, `Kitura-net` und `SwiftyJSON` in der Swift-Standardaktion bereitgestellt werden. Schließen Sie sie in Ihre eigene Datei `Package.swift` für Swift 3-Aktionen ein.
  {: shortdesc}

1. Erstellen Sie die Aktion, indem Sie den folgenden Befehl für eine Swift 3-Aktion ausführen. 

  ```
  bash compile.sh hello swift:3.1.1
  ```
  {: pre}

  Verwenden Sie für die Kompilierung für Swift 4 `swift:4.1` statt `swift:3.1.1`. 

  ```
  bash compile.sh hello swift:4.1
  ```
  {: pre}

  Dieser Prozess erstellt `hello.zip` in `build`. 

2. Laden Sie sie mit dem Aktionsnamen 'helloSwifty' auf OpenWhisk hoch. 

  Verwenden Sie für Swift 3 `swift:3.1.1`. 

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:3.1.1
  ```
  {: pre}

  Verwenden Sie für Swift 4.1 `swift:4.1`. 

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:4.1
  ```
  {: pre}

3. Prüfen Sie, wie viel schneller es passiert, indem Sie den folgenden Befehl ausführen. 

  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

  Die Dauer der Aktion finden Sie in der Eigenschaft 'duration'. Vergleichen Sie sie mit der Zeit, die ein Kompilierungsschritt in der Aktion 'hello' benötigen würde. 

## Fehlerbehandlung in Swift 4
{: #error-handling-swift4}

Anhand des neuen Codable-Completion-Handlers können Sie einen Fehler übergeben, um auf einen Fehler in Ihrer Aktion hinzuweisen. Die [Fehlerbehandlung in Swift ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) ähnelt der Behandlung von Ausnahmebedingungen in anderen Sprachen, in denen die Schlüsselwörter `try`, `catch` und `throw` verwendet werden.
{: shortdesc}

Das folgende Snippet zeigt ein Beispiel für die Fehlerbehandlung. 

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
