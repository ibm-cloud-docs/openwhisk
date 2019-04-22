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

# Création d'actions Swift
{: #creating-swift-actions}

Démarrez de nouvelles actions avec l'environnement d'exécution Swift 4.2 ou migrez des actions existantes vers cet environnement à l'aide du type `swift:4.2` et de son processus de compilation.
{: tip}

Les sections suivantes expliquent comment créer et appeler une action Swift unique et conditionner cette action dans un fichier zip.

**Remarque :** les actions Swift s'exécutent dans un environnement Linux. Swift sous Linux est en cours
de développement et {{site.data.keyword.openwhisk_short}} utilise en principe la dernière édition disponible. Ces éditions ne sont peut-être pas stables. Il se peut que la version de Swift utilisée avec {{site.data.keyword.openwhisk_short}} ne corresponde pas aux versions de Swift provenant d'éditions stables de Xcode sous MacOS.

Pour plus d'informations sur l'environnement d'environnement Swift, voir [Contextes d'exécution](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions).
{: tip}

## Création et appel d'une action Swift
{: #actions_swift_invoke}
{: #openwhisk_actions_swift_invoke}

### Swift 4
{: #actions_swift4_invoke}
{: #openwhisk_actions_swift4_invoke}

En plus de la signature de la fonction main ci-dessus, Swift 4 fournit deux autres signatures prêtes à l'emploi qui bénéficient du type [Codable ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.apple.com/documentation/swift/codable). Vous pouvez obtenir plus d'informations sur les types de données [codables et décodables permettant la compatibilité avec des représentations externes comme JSON ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types). 

1. Sauvegardez le code suivant dans un fichier nommé `hello.swift`. 

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

    L'exemple suivant comporte un paramètre d'entrée en tant qu'entrée codable (`Codable Input`) avec le champ `name`, et un paramètre de sortie codable (`Codable Output`) est renvoyé avec un champ `greetings`.

2. Créez une action nommée `helloSwift`.

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.2
    ```
    {: pre}

3. Appelez l'action.

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Exemple de sortie :

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Swift 3
{: #actions_swift3_invoke}
{: #openwhisk_actions_swift3_invoke}

Une action est une fonction Swift de niveau supérieur. Pour créer une action Swift 3 :

1. Sauvegardez le code suivant dans un fichier nommé `hello.swift`. 

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

    Dans cet exemple, l'action Swift consomme et produit un dictionnaire.

2. Créez une action nommée `helloSwift`.

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

3. Appelez l'action.

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Exemple de sortie :

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Conditionnement d'une action sous forme d'exécutable Swift
{: #packaging-an-action-as-a-swift-executable}

Lorsque vous créez une action Swift {{site.data.keyword.openwhisk_short}} avec un fichier source Swift, celui-ci doit être compilé en fichier binaire avant l'exécution de l'action. Ce délai est dénommé délai de démarrage à froid. Une fois le fichier binaire créé, les appels ultérieurs de l'action sont beaucoup plus rapides jusqu'à ce que le conteneur hébergeant votre action soit purgé. Pour éviter ce délai, vous pouvez compiler votre fichier Swift en binaire, puis télécharger le fichier binaire dans {{site.data.keyword.openwhisk_short}} sous forme de fichier .zip. 

### Compilation d'actions conditionnées Swift 4.2
{: #actions_swift42_compile}
{: #openwhisk_actions_swift42_compile}

L'environnement d'exécution Docker inclut un compilateur pour aider les utilisateurs à compiler et conditionner des actions Swift 4.2. 

##### Compilation d'un fichier source unique pour Swift 4.2

Compilez un fichier source unique qui ne dépend pas de bibliothèques externes à l'aide de la commande suivante : 

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

Le conteneur Docker lit le contenu du fichier à partir de `stdin` et écrit une archive .zip avec l'exécutable swift compilé dans `stdout`.

Utilisez l'indicateur `-compile` avec le nom de la méthode main. 

L'archive .zip est prête pour le déploiement et l'appel à l'aide du type `swift:4.2`

```bash
wsk action update helloSwiftly hello.zip --kind swift:4.2
wsk action invoke helloSwiftly -r -p name World
```
{: codeblock}

#### Compilation de dépendances et de projets multifichiers pour Swift 4.2

Pour compiler plusieurs fichiers et inclure des dépendances externes, créez la structure de répertoire suivante :

```
.
├── Package.swift
└── Sources
    └── main.swift
```
{: codeblock}

Le répertoire `Sources/` doit contenir un fichier nommé `main.swift`. 

Le fichier `Package.swift` doit commencer par un commentaire indiquant la version `4.2` pour les outils Swift :

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

Créez une archive .zip avec le contenu du répertoire : 

```bash
zip ../action-src.zip -r *
```
{: codeblock}

Transmettez l'archive .zip au conteneur Docker via `stdin`. Le fichier `stdout` sera une nouvelle archive .zip avec l'exécutable compilé.
Le conteneur Docker lit le contenu de l'archive .zip à partir de `stdin` et écrit une nouvelle archive .zip avec l'exécutable Swift compilé dans `stdout`.

```
docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
```
{: codeblock}

Dans un système Linux, vous pouvez combiner les étapes `zip` et `docker run` dans une commande unique : 

```
zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
```
{: codeblock}

L'archive  `action-bin.zip` est prête pour le déploiement et l'appel à l'aide du type `swift:4.2`

```bash
wsk action update helloSwiftly action-bin.zip --kind swift:4.2
wsk action invoke helloSwiftly -r
```
{: codeblock}

### Compilation d'actions conditionnées Swift 3.1.1 et 4.1

#### Utilisation d'un script pour générer des actions conditionnées Swift 3.1.1 et 4.1

Vous pouvez utiliser un script pour automatiser le conditionnement d'une action.
{: shortdesc}

Avant de commencer, créez un répertoire `actions`, où chaque répertoire de niveau supérieur représente une action.

```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```
{: codeblock}

Créez un fichier script `compile.sh` comme suit. 

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

- Créez le fichier `Package.swift` pour ajouter des dépendances. 

La syntaxe varie en fonction de la version d'exécution de Swift.
Exemple de syntaxe Swift 3.

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

  Exemple de syntaxe Swift 4.
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

  Dans cet exemple, `example-package-deckofplayingcards` est ajouté sous forme de dépendance.
  Notez que `CCurl`, `Kitura-net` et `SwiftyJSON` sont fournis dans l'action Swift standard. Incluez-les dans votre propre fichier `Package.swift` pour les actions Swift 3.
  {: shortdesc}

1. Générez l'action en exécutant la commande suivante pour une action Swift 3. 

  ```
  bash compile.sh hello swift:3.1.1
  ```
  {: pre}

  Pour une compilation pour Swift 4, utilisez `swift:4.1` au lieu de `swift:3.1.1`

  ```
  bash compile.sh hello swift:4.1
  ```
  {: pre}

  Ce processus a créé `hello.zip` dans `build`. 

2. Téléchargez-le vers OpenWhisk sous le nom d'action helloSwifty. 

  Pour Swift 3, utilisez le type `swift:3.1.1`

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:3.1.1
  ```
  {: pre}

  Pour Swift 4.1, utilisez le type `swift:4.1`

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:4.1
  ```
  {: pre}

3. Accélérez le processus en exécutant la commande suivante : 

  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

  Le temps qui a été nécessaire à l'exécution de l'action figure dans la propriété "duration" et vous pouvez le comparer avec le temps qui est nécessaire à l'exécution via une étape de compilation dans l'action hello. 

## Erreur lors du traitement dans Swift 4
{: #error-handling-swift4}

Avec le nouveau gestionnaire completionHandler Codable, vous pouvez transmettre le paramètre error pour indiquer qu'un incident s'est produit dans votre action. Le [traitement des erreurs dans Swift ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) ressemble au traitement des exceptions dans d'autres langues, avec l'utilisation des mots clés `try`, `catch` et `throw`.
{: shortdesc}

Le fragment suivant montre un exemple de traitement d'erreur. 

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
