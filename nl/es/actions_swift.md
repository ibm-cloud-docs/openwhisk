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

# Creación de acciones Swift
{: #creating-swift-actions}

Inicie nuevas acciones o migre acciones existentes al entorno de ejecución Swift 4.2 utilizando el tipo
`swift:4.2` y su proceso de compilación.
{: tip}

Las siguientes secciones le guían a través de la creación e invocación de una única acción de Swift y por el empaquetado de una acción en un archivo zip.

**Nota:** Acciones Swift se ejecutan en un entorno Linux. Swift en Linux aún está en desarrollo, y {{site.data.keyword.openwhisk_short}} utiliza el release disponible más reciente. Es posible que estos releases no sean estables. La versión de Swift que se utiliza con {{site.data.keyword.openwhisk_short}} podría no ser coherente con versiones de Swift de releases estables de Xcode en MacOS.

Para obtener más información sobre el entorno de ejecución Swift, consulte [Entornos de ejecución](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions).
{: tip}

## Creación e invocación de una acción Swift
{: #actions_swift_invoke}
{: #openwhisk_actions_swift_invoke}

### Swift 4
{: #actions_swift4_invoke}
{: #openwhisk_actions_swift4_invoke}

Además de la firma para la función main, Swift 4 proporciona dos firmas más para sacar partido del tipo [Codable ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.apple.com/documentation/swift/codable). Puede obtener más información sobre los tipos de datos que se pueden
[codificar y descodificar para la compatibilidad con representaciones externas como JSON ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types). 

1. Guarde el código siguiente en un archivo denominado `hello.swift`.

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

    Este ejemplo toma un parámetro de entrada como `entrada codificable (Codable Input)` con un campo `name` y devuelve una `salida codificable (Codable output)` con un campo `greetings`.

2. Cree una acción denominada `helloSwift`.

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.2
    ```
    {: pre}

3. Invoque la acción.

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Salida de ejemplo:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Swift 3
{: #actions_swift3_invoke}
{: #openwhisk_actions_swift3_invoke}

Una acción es una función Swift de nivel superior. Para crear una acción de Swift 3:

1. Guarde el código siguiente en un archivo denominado `hello.swift`.

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

    En este ejemplo, la acción Swift consume un diccionario y genera un diccionario.

2. Cree una acción denominada `helloSwift`.

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

3. Invoque la acción.

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Salida de ejemplo:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Empaquetado de una acción como un ejecutable Swift
{: #packaging-an-action-as-a-swift-executable}

Cuando cree una acción Swift de {{site.data.keyword.openwhisk_short}} con un archivo fuente Swift, el archivo se debe compilar en un código binario antes de poder ejecutar la acción. Este retraso se conoce como retraso de inicio en frío. Una vez creado el binario, las siguientes llamadas a la acción serán mucho más rápidas hasta que se purgue el contenedor que alberga la acción. Para evitar el retraso de inicio en frío, puede compilar el archivo Swift en un binario y luego cargar dicho binario en {{site.data.keyword.openwhisk_short}} en un archivo .zip.

### Compilación de acciones empaquetadas Swift 4.2
{: #actions_swift42_compile}
{: #openwhisk_actions_swift42_compile}

El entorno de ejecución de Docker incluye un compilador para ayudar a los usuarios a compilar y empaquetar acciones Swift 4.2.

##### Compilación de un archivo de origen individual para Swift 4.2

Compile un único archivo de origen que no dependa de bibliotecas externas utilizando el mandato siguiente.

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

El contenedor de Docker lee el contenido del archivo de `stdin` y escribe un archivo .zip con el ejecutable swift compilado en
`stdout`.

Utilice el distintivo `-compile` con el nombre del método main.

El archivo .zip está listo para su despliegue e invocación utilizando el tipo `swift:4.2`

```bash
wsk action update helloSwiftly hello.zip --kind swift:4.2
wsk action invoke helloSwiftly -r -p name World
```
{: codeblock}

#### Compilación de dependencias y proyectos de varios archivos para Swift 4.2

Para compilar varios archivos e incluir dependencias externas, cree la estructura de directorios siguiente.

```
.
├── Package.swift
└── Sources
    └── main.swift
```
{: codeblock}

El directorio `Sources/` debe contener un archivo denominado `main.swift`.

El `Package.swift` debe empezar con un comentario que especifica la versión `4.2` del conjunto de herramientas de Swift:

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

Cree un archivo .zip con el contenido del directorio:

```bash
zip ../action-src.zip -r *
```
{: codeblock}

Pase el archivo .zip al contenedor de Docker a través de `stdin`. `stdout` seré un nuevo archivo .zip con el ejecutable compilado.
El contenedor de Docker lee el contenido del archivo .zip de `stdin` y escribe un nuevo archivo .zip con el ejecutable Swift compilado en `stdout`.

```
docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
```
{: codeblock}

En un sistema basado en Linux, puede combinar los pasos de `zip` y `docker run` en un único mandato:

```
zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
```
{: codeblock}

El archivo `action-bin.zip` está listo para su despliegue e invocación utilizando el tipo `swift:4.2`

```bash
wsk action update helloSwiftly action-bin.zip --kind swift:4.2
wsk action invoke helloSwiftly -r
```
{: codeblock}

### Compilación de acciones empaquetadas Swift 3.1.1 y 4.1

#### Utilización de un script para compilar acciones empaquetadas Swift 3.1.1 y 4.1

Puede utilizar un script para automatizar el empaquetado de la acción.
{: shortdesc}

Antes de empezar, cree un directorio `actions`, en el que cada directorio de nivel superior representa una acción.

```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```
{: codeblock}

Cree el archivo de script `compile.sh` siguiente.

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

# El archivo de acción puede ser {nombre_acción}.swift o main.swift
if [ -f \"$DEST_SOURCE/$1.swift\" ] ; then
    echo 'renaming $DEST_SOURCE/$1.swift $DEST_SOURCE/main.swift'
    mv \"$DEST_SOURCE/$1.swift\" $DEST_SOURCE/main.swift
fi
# Añadir las partes específicas de OW
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
# Tenemos nuestro propio paquete Package.swift, realizar una compilación completa
swift build ${BUILD_FLAGS} -c release

echo 'Creating archive $1.zip...'
#.build/release/Action
mkdir -p /owexec/$OUTPUT_DIR
zip \"/owexec/$OUTPUT_DIR/$1.zip\" .build/release/Action
"
```
{: codeblock}

- Cree el archivo `Package.swift` para añadir dependencias.

La sintaxis cambia según la versión del entorno de ejecución Swift.
Sintaxis de ejemplo de Swift 3.

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

  Sintaxis de ejemplo de Swift 4.
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

  En este ejemplo, se añade `example-package-deckofplayingcards` como dependencia.
  Tenga en cuenta que se proporcionan `CCurl`, `Kitura-net` y `SwiftyJSON` en la acción Swift estándar. Inclúyalas en su `Package.swift` para las acciones Swift 3.
  {: shortdesc}

1. Compile la acción ejecutando el mandato siguiente para una acción Swift 3.

  ```
  bash compile.sh hello swift:3.1.1
  ```
  {: pre}

  Para compilar para Swift 4, utilice `swift:4.1` en lugar de `swift:3.1.1`

  ```
  bash compile.sh hello swift:4.1
  ```
  {: pre}

  Este proceso ha creado `hello.zip` en `build`.

2. Cárguelo en OpenWhisk con el nombre de acción helloSwifty.

  Para Swift 3, utilice el tipo `swift:3.1.1`

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:3.1.1
  ```
  {: pre}

  Para Swift 4.1, utilice el tipo `swift:4.1`

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:4.1
  ```
  {: pre}

3. Compruebe la diferencia de velocidad ejecutando el mandato siguiente.

  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

  El tiempo que se ha tardado en ejecutar la acción está en la propiedad "duration" y se compara con el tiempo que se tarda en ejecutar con un paso de compilación en la acción hello.

## Manejo de errores en Swift 4
{: #error-handling-swift4}

Con la utilización del manejador de terminación Codable, se pueden pasar errores para indicar una anomalía en su acción. El [Manejo de errores en Swift ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) es parecido al manejo de excepciones en otros lenguajes, con la utilización de las palabras clave `try`, `catch` y `throw`.
{: shortdesc}

En el siguiente fragmento de código se muestra un ejemplo de manejo de un error.

```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    // Devolver error real
    do{
        throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
    } catch {
        completion(nil, error)
    }
}
```
{: codeblock}
