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

# Criando ações do Swift
{: #creating-swift-actions}

Inicie novas ações ou migre as ações existentes para o tempo de execução do Swift 4.2 usando o tipo `swift:4.2` e seu processo de compilação.
{: tip}

As seções a seguir orientam você na criação e na chamada de uma ação do Swift única e no empacotamento de
uma ação em um arquivo zip.

**Nota:** as ações do Swift são executadas em um ambiente Linux. O Swift no Linux
ainda está em desenvolvimento e o {{site.data.keyword.openwhisk_short}} usa a liberação mais recente
disponível. Talvez essas liberações não estejam estáveis. A versão do Swift que é usada com o
{{site.data.keyword.openwhisk_short}} pode estar inconsistente com as versões do Swift de liberações
estáveis do Xcode no MacOS.

Para obter mais informações sobre o tempo de execução do Swift, [Runtimes](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions).
{: tip}

## Criando e chamando uma ação do Swift
{: #actions_swift_invoke}
{: #openwhisk_actions_swift_invoke}

### Swift 4
{: #actions_swift4_invoke}
{: #openwhisk_actions_swift4_invoke}

Além da assinatura da função principal, o Swift 4 fornece mais duas assinaturas que aproveitam o tipo [Codable ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.apple.com/documentation/swift/codable). É possível aprender mais sobre os tipos de dados que são [codificáveis e decodificáveis para compatibilidade com representações externas, como JSON ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types). 

1. Salve o código a seguir em um arquivo chamado `hello.swift`.

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

    Este exemplo usa um parâmetro de entrada como `Codable Input` com o campo
`name` e retorna uma `Codable output` com um campo
`greetings`.

2. Crie uma ação chamada  ` helloSwift `.

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.2
    ```
    {: pre}

3. Invoque a ação.

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Exemplo de Saída:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Swift 3
{: #actions_swift3_invoke}
{: #openwhisk_actions_swift3_invoke}

Uma ação é uma função Swift de nível superior. Para criar uma ação do Swift 3:

1. Salve o código a seguir em um arquivo chamado `hello.swift`.

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

    Neste exemplo, a ação do Swift consome um dicionário e produz um dicionário.

2. Crie uma ação chamada  ` helloSwift `.

    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

3. Invoque a ação.

    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Exemplo de Saída:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Empacotando uma ação como um executável do Swift
{: #packaging-an-action-as-a-swift-executable}

Ao criar uma ação do Swift do {{site.data.keyword.openwhisk_short}} com um arquivo de
origem do Swift, o arquivo deverá ser compilado em um binário antes de a ação ser executada. Esse atraso é conhecido como o atraso de cold start. Depois que o binário é criado, as chamadas subsequentes para a ação são muito mais rápidas até que o contêiner
que retém sua ação seja limpo. Para evitar o atraso de cold start, é possível compilar seu arquivo Swift em um binário e, em seguida, fazer upload do binário para o {{site.data.keyword.openwhisk_short}} em um arquivo .zip.

### Compilando ações empacotadas do Swift 4.2
{: #actions_swift42_compile}
{: #openwhisk_actions_swift42_compile}

O tempo de execução do Docker inclui um compilador para ajudar os usuários a compilar e empacotar ações do Swift 4.2.

##### Compilando um arquivo de origem único para Swift 4.2

Compile um único arquivo de origem que não dependa de bibliotecas externas, usando o comando a seguir.

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main < hello.swift >hello.zip
```
{: pre}

O contêiner Docker lê o conteúdo do arquivo de `stdin` e grava um archive .zip com o executável swift compilado para `stdout`.

Use o sinalizador `-compile` com o nome do método principal.

O archive .zip está pronto para implementação e chamada usando o tipo `swift:4.2`

```bash
wsk action update helloSwiftly hello.zip --kind swift:4.2
wsk action invoke helloSwiftly -r -p name World
```
{: codeblock}

#### Compilando dependências e projetos de vários arquivos para Swift 4.2

Para compilar vários arquivos e incluir dependências externas, crie a estrutura de diretório a seguir.

```
.
├── Package.swift
└── Sources
    └── main.swift
```
{: codeblock}

O diretório `Sources/` deve conter um arquivo denominado `main.swift`.

O `Package.swift` deve iniciar com um comentário especificando a versão `4.2` para o conjunto de ferramentas Swift:

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
    .package (url: "https: //github.com/IBM-Swift/SwiftyRequest.git", .upToNextMajor (de: "1.0.0"))
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

Crie um archive .zip com o conteúdo do diretório:

```bash
zip ../action-src.zip -r *
```
{: codeblock}

Passe o archive .zip para o contêiner do Docker sobre `stdin`. O `stdout`será um novo archive .zip com o executável compilado.
O contêiner Docker lê o conteúdo do archive .zip de `stdin` e grava um novo archive .zip com o executável do Swift compilado para `stdout`.

```
docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
```
{: codeblock}

Em um sistema baseado em Linux, é possível combinar as etapas `zip` e `docker run` em um único comando:

```
zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
```
{: codeblock}

O archive `action-bin.zip` está pronto para implementação e chamada usando o tipo `swift:4.2`

```bash
wsk action update helloSwiftly action-bin.zip --kind swift:4.2
wsk action invoke helloSwiftly -r
```
{: codeblock}

### Compilando as ações empacotadas do Swift 3.1.1 e 4.1

#### Usando um script para construir ações empacotadas do Swift 3.1.1 e 4.1

É possível usar um script para automatizar o empacotamento da ação.
{: shortdesc}

Antes de iniciar, crie um diretório `actions` com cada diretório de nível superior que represente uma ação.

```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```
{: codeblock}

Crie o script `compile.sh` no arquivo a seguir.

```bash
#!/bin/bash
set -ex

if [ -z "$1" ] ; then
    echo 'Error: Missing action name'
    exit 1
fi
if [ -z "$2" ] ; then
    echo 'Error: Missing kind, for example swift:4.1'
  saída 2
fi
OUTPUT_DIR = "construção"
se [ ${ 2 } == "swift: 3.1.1 " ]; em seguida,
  BASE_PATH= "/swift3Action"
  DEST_SOURCE=" $BASE_PATH/spm-build "
  RUNTIME="openwhisk/action-swift-v3.1.1 "
elif [ ${ 2 } == "swift: 4.1 " ]; em seguida,
  RUNTIME="openwhisk/action-swift-v4.1 "
  BASE_PATH= "/swift4Action"
  DEST_SOURCE= "/ $BASE_PATH/spm-build/Sources/Action"
ou
  echo "Erro: Kind $2 não reconhece"
  saída 3
fi
DEST_PACKAGE_SWIFT=" $BASE_PATH/spm-build/Package.swift "

BUILD_FLAGS = ""
if [ -n "$3" ]; then
  BUILD_FLAGS=$ { 3 }
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

- Crie o arquivo `Package.swift` para incluir dependências.

A sintaxe é mudada com base na versão de runtime do Swift.
Sintaxe de exemplo do Swift 3.

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

  Sintaxe de exemplo do Swift 4.
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

  Neste exemplo, `exemplo-package-deckofplayingcards`é incluído como uma dependência.
  Observe que `CCurl`, `Kitura-net` e `SwiftyJSON` são fornecidos na ação Swift padrão. Inclua-as em seu próprio `Package.swift` para ações do Swift 3.
  {: shortdesc}

1. Construa a ação executando o comando a seguir para uma ação do Swift 3.

  ```
  bash compile.sh hello swift:3.1.1
  ```
  {: pre}

  Para compilar para o Swift 4, use `swift:4.1` em vez de `swift:3.1.1`

  ```
  bash compile.sh hello swift:4.1
  ```
  {: pre}

  Esse processo criou `hello.zip` na `build`.

2. Faça upload dele para o OpenWhisk com o nome da ação helloSwifty.

  Para o Swift 3, use o tipo `swift:3.1.1`

  ```
  wsk action update helloSwiftly build/hello.zip --kind swift:3.1.1
  ```
  {: pre}

  Para o Swift 4.1, use o tipo `swift:4.1`

  ```
  a ação wsk update helloSwiftly build/hello.zip -- kind swift: 4.1
  ```
  {: pre}

3. Verifique o quanto mais rápido ele está executando o comando a seguir.

  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

  O tempo que a ação levou para ser executada está na propriedade "duration" e é comparado com o tempo que
leva para executar com uma etapa de compilação na ação hello.

## Manipulação de erros no Swift 4
{: #error-handling-swift4}

Usando o manipulador de conclusão Codable, é possível passar um erro para indicar uma falha em sua ação. [A manipulação de erros no Swift ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) lembra a manipulação de exceções em outros idiomas, com o uso das palavras-chave `try`, `catch` e `throw`.
{: shortdesc}

O snippet a seguir mostra um exemplo de manipulação de um erro.

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
