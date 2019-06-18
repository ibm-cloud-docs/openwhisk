---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: actions, serverless, javascript, node, node.js

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


# Preparando apps para ações
{: #prep}

Se você levar um app com você para torná-lo sem servidor ou gravar um script especificamente para responder a um evento, seu código deverá atender a determinados requisitos antes de criar uma ação por meio dele.
{: shortdesc}

Cada linguagem de programação possui requisitos específicos para execução, mas a maioria possui os requisitos gerais a seguir:
- o nome esperado para o ponto de entrada para o código é `main` por padrão. Se o ponto de entrada não for `main`, um nome customizado poderá ser especificado quando a ação for criada, portanto, anote esse nome.
- Os parâmetros de entrada em app e os resultados de saída de seu app devem ser formatados em uma estrutura específica que possa ser transmitida entre entidades. A estrutura depende de sua linguagem de código. Por exemplo, com apps Python, os parâmetros devem ser inseridos em seu app como um dicionário e o resultado de seu app deve ser estruturado como um dicionário. Como também é possível passar parâmetros em um objeto estruturado para sua ação, como JSON, por exemplo, é possível estruturar seu código para esperar um parâmetro de entrada com valores JSON de determinados campos, como `name` e `place`.

    JSON input example:
    ```json
    {"name": "Dorothy", "place": "Kansas"}
    ```
    {: codeblock}

    JavaScript example:
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
    ```
    {: codeblock}
- Se o seu app contiver múltiplos arquivos, eles deverão ser combinados em um único arquivo para ser usado em uma ação. É possível regravar seu código em um arquivo ou é possível empacotar os arquivos e as dependências em um único archive. Se o tempo de execução não for suportado, será possível empacotar seu app em uma imagem do Docker.
- As dependências também devem ser empacotadas com seu app. Os tempos de execução disponíveis vêm com alguns pacotes e extensões pré-instalados. [Revise as informações de referência para seu tempo de execução](/docs/openwhisk?topic=cloud-functions-runtimes) a fim de ver se uma dependência de seu app já está incluída com o tempo de execução. Se a sua dependência estiver incluída, não será necessário empacotá-la com seu app.

    A compilação de código não é necessária, mas, se possível para seu tempo de execução, a compilação de seu código com antecedência pode melhorar o desempenho.
    {: tip}

## Preparando apps em imagens do Docker
{: #prep_docker}

Com o {{site.data.keyword.openwhisk_short}}, é possível gravar seu app em qualquer idioma e empacotá-lo como uma imagem do Docker.
{: shortdesc}

É possível usar apenas imagens de registros públicos, como uma imagem que está publicamente disponível no Docker Hub. Registros privados não são suportados.
{: important}

### Empacotando código em imagens do Docker
{: #prep_docker_pkg}

O seu código é compilado em um binário executável e integrado em uma imagem do Docker. O programa binário interage com o sistema aceitando entrada de `stdin` e respondendo por meio de `stdout`.
{: shortdesc}

Antes de começar:
- Deve-se ter uma conta do Docker Hub. É possível configurar um ID do Docker grátis e uma conta no [Docker Hub ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://hub.docker.com).
- [Instale o Docker](https://hub.docker.com/search?offering=community&type=edition).
- [Revise os requisitos para o tempo de execução do Docker](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker).

Para empacotar seu app:

para empacotar seu código como uma imagem do Docker:
1. Faça download e instale o esqueleto do Docker. A estrutura básica é um modelo do contêiner do Docker no qual é possível injetar seu código na forma de binários customizados.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Configure seu binário customizado na estrutura básica da caixa preta. A estrutura básica inclui um programa C que pode ser usado. Parte do arquivo `example.c` é compilada como parte do processo de construção da imagem do Docker, de modo que o C não precisa ser compilado em sua máquina.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  Exemplo de Saída:
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. Opcional: inclua código e dependências adicionais na imagem do Docker modificando o `Dockerfile` para construir seu executável. Observe os requisitos a seguir:
  * O binário deve estar localizado dentro do contêiner em `/action/exec`.
  * O executável recebe um único argumento a partir da linha de comandos. Esse argumento é uma serialização de sequência do objeto JSON que representa os argumentos para a ação.
  * O programa pode efetuar log em `stdout` ou `stderr`.
  * Por convenção, a última linha de saída deve ser um objeto da JSON em sequência que represente o resultado da ação.
  Para obter mais informações sobre a construção de Dockerfiles, consulte a [Referência do Dockerfile](https://docs.docker.com/engine/reference/builder/).

4. Construa a imagem do Docker e faça upload da mesma usando um script fornecido.
    1. Efetue login no Docker.
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. Navegue para o diretório `dockerSkeleton`.
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. Execute o script.
        ```
        ./buildAndPush.sh < username> /blackboxdemo
        ```
        {: pre}




## Preparando apps JavaScript
{: #prep_js}

Antes de criar uma ação, tenha seu código JavaScript pronto. Confirme se seu código está estruturado corretamente e, em seguida, decida se ele precisa ser empacotado.
{: shortdesc}

### Estruturando código JavaScript
{: #prep_js_struct}

- O nome esperado para a função de ponto de entrada é `main`. Se a função em seu código não for `main`, anote o nome para especificá-la quando a ação for criada.
- Os parâmetros de entrada são passados como um objeto JSON.
- O resultado de uma ativação bem-sucedida também é um objeto JSON, mas é retornado de forma diferente, dependendo se a ação é [síncrona](#prep_js_sync) ou [assíncrona](#prep_js_async).



Exemplo:
```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```

Exemplo com várias funções:

  ```javascript
  function main() {
      return { payload: helper() }
  }

  function helper() {
      return new Date();
}
  ```
  {: codeblock}


### Estruturando código JavaScript com comportamento síncrono
{: #prep_js_sync}

A ativação do JavaScript é síncrona quando a função principal sai sem executar uma instrução `return` ou sai executando uma instrução `return` que retorna qualquer valor, exceto uma promessa.
{: shortdesc}

Exemplo de código síncrono:

```javascript
// each path results in a synchronous activation
function main(params) {
  if (params.payload == 0) {
     return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
     return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}




### Estruturando código JavaScript com comportamento assíncrono
{: #prep_js_async}

As funções do JavaScript podem continuar a execução em uma função de retorno de chamada mesmo após um retorno. A ativação do JavaScript é assíncrona se a função principal sai retornando uma promessa. Nesse caso, o sistema presume que a ação ainda esteja em execução até que a promessa seja cumprida ou rejeitada. As funções do JavaScript que são executadas de forma assíncrona podem retornar o resultado de ativação após a função `main` retornar, retornando uma promessa em sua ação.
{: shortdesc}

Inicie instanciando um novo objeto de promessa e passando uma função de retorno de chamada. O
retorno de chamada aceita dois argumentos, resolver e rejeitar, que são ambos funções. Todos
os códigos assíncronos vão dentro desse retorno de chamada. O manipulador de ações pode ter qualquer nome, desde que ele esteja em conformidade com a
assinatura convencional de aceitação e de retorno de um objeto (ou com um `Promise` de um
objeto).

No exemplo a seguir, é possível ver como cumprir uma promessa chamando a função de resolução.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         resolve({ done: true });
                }, 100);
             })
}
```
{: codeblock}

Este exemplo mostra como rejeitar uma promessa ao chamar a função de rejeição.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 100);
    })
}
```
{: codeblock}

Nos exemplos acima, os detalhes a seguir são executados.
* A função `main` retorna uma promessa. A promessa indica que a ativação ainda não foi concluída, mas espera-se que seja no futuro.
* A função JavaScript `setTimeout()` espera por 2 segundos antes de chamar a função de retorno de chamada da promessa, que representa o código assíncrono.
* O retorno de chamada da promessa aceita os argumentos `resolve` e `reject`, que são funções.
  * A chamada para `resolve()` preenche a promessa e indica que a ativação é concluída normalmente.
  * Uma chamada para `reject()` pode ser usada para rejeitar a promessa e sinalizar que a ativação é concluída anormalmente.


### Estruturando código JavaScript com comportamento síncrono e assíncrono
{: #prep_js_both}

Uma ação pode ser síncrona em algumas entradas e assíncronas em outras, conforme mostrado no exemplo a seguir.
{: shortdesc}

```javascript
function main(params) {
     if (params.payload) {
        // asynchronous activation
         return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
                }, 100);
             })
     } else {
        // synchronous activation
         return {done: true};
      }
}
```
{: codeblock}





### Exemplo: chamando uma API externa com JavaScript
{: #prep_js_api}

O exemplo a seguir chama a API externa para o serviço NASA Astronomy Picture of the Day (APOD), que fornece uma imagem exclusiva de nosso universo todos os dias.
{: shortdesc}


```javascript
let rp = require('request-promise')

function main(params) {
    const options = {
        uri: "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo",
            json: true
        }
        return rp(options)
    .then (res = >{
        return { response: res }
    })
}
```
{: codeblock}

Uma chamada é feita para a API APOD da NASA e os campos são extraídos do resultado JSON.

Em seguida, [crie](/docs/openwhisk?topic=cloud-functions-actions) e [chame a ação](/docs/openwhisk?topic=cloud-functions-test) para testá-la. O objeto de exemplo a seguir é retornado:

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






### Empacotando o código JavaScript com o módulo webpack
{: #prep_js_pkg}

É possível empacotar um app usando um bundler de módulo JavaScript, como [webpack ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://webpack.js.org/concepts/). Quando o `webpack` processa seu código, ele constrói recursivamente um gráfico de
dependência que inclui cada módulo de que a sua ação precisa.
{: shortdesc}

Antes de iniciar, [revise os pacotes incluídos com o tempo de execução do JavaScript](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments) para ver se uma dependência de seu app já está incluída com o tempo de execução. Caso sua dependência não seja incluída, deve-se empacotá-la com seu app.

1. Crie um arquivo `package.json`. Inclua `webpack` como uma dependência de desenvolvimento.

    ```json
    {
      "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
        "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10"
      },
  "dependencies": {
        "left-pad" : "1.1.3"
      },
  "devDependencies": {
        "webpack": "^3.8.1"
      }
    }
    ```
    {: codeblock}

2. Salve o código de configuração do webpack a seguir em um arquivo denominado `webpack.config.js`.

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

3. Prepare seu código de app. Neste exemplo, denominado `index.js`, a variável `global.main` é configurada para a função principal do app.

    Exemplo:
    ```javascript function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    global.main = myAction;
    ```
    {: codeblock}

4. Instale todas as dependências localmente.

    ```
    npm install
    ```
    {: pre}

5. Construa o pacote configurável do webpack.

    ```
    npm run build
    ```
    {: pre}

    O arquivo `dist/bundle.js` é criado e implementado como o código de origem da
ação.

6. Crie a ação usando o script `npm` ou a CLI.

    * Usando o script  ` npm ` :

        ```
        npm run deploy
        ```
        {: pre}

    * Usando a CLI:

        ```
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    O arquivo de pacote configurável que é construído pelo `webpack`suporta apenas dependências JavaScript. Chamadas de ação podem falhar se o pacote configurável depender de dependências de arquivo binário porque isso não está incluído com o arquivo `bundle.js`.
    {: tip}



### Empacotando código JavaScript como arquivos NPM
{: #prep_js_npm}

Como uma alternativa para gravar todo o seu código de ação em um único arquivo de origem JavaScript, é possível empacotar seu código como um pacote `npm` em um arquivo .zip.
{: shortdesc}

Antes de iniciar, [revise os pacotes incluídos com o tempo de execução do JavaScript](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments) para ver se uma dependência de seu app já está incluída com o tempo de execução. Caso sua dependência não seja incluída, deve-se empacotá-la com seu app.

1. No diretório raiz, crie um arquivo `package.json`. Exemplo:

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

2. Instale todas as dependências localmente.

    ```
    npm install DEPENDENCY
    ```
    {: pre}

    **Nota:** embora a maioria dos pacotes `npm` instala origens
do JavaScript no `npm install`, alguns também instalam e compilam artefatos binários. O upload de arquivo de archive suporta apenas dependências do JavaScript. Se o archive incluir dependências binárias, as chamadas de ação poderão não ser bem-sucedidas.

3. Crie um archive `.zip` contendo todos os arquivos, incluindo todas as dependências.

    ```
    zip -r action.zip *
    ```
    {: pre}

    Usuários do Windows: usar a ação do Windows Explorer para criar o arquivo .zip resulta em uma estrutura de arquivo incorreta. As ações .zip do {{site.data.keyword.openwhisk_short}} devem ter `package.json` na raiz do archive, mas o Windows Explorer coloca-o dentro de uma pasta aninhada. Em vez disso, use o comando `zip`.
    {: tip}





## Preparando apps Go
{: #prep_go}

Use um arquivo único para fins de teste rápido ou desenvolvimento. Para apps de produção, pré-compile suas ações do Go em um executável para melhor desempenho ou para suporte de diversos arquivos de origem, incluindo bibliotecas do fornecedor.
{: shortdesc}

Embora seja possível criar um binário em qualquer plataforma Go, compilando com `GOOS=Linux` e `GOARCH=amd64`, use o recurso de pré-compilação que está integrado à imagem do contêiner de tempo de execução. É possível empacotar [vários arquivos de origem](#prep_go_multi) ou [bibliotecas do fornecedor](#prep_go_vendor).
{: tip}


### Estruturando código do Go
{: #prep_go_struct}

- O nome esperado para o pacote de ponto de entrada é `main`. Se o pacote em seu código não for `main`, anote o nome para especificá-lo quando a ação for criada.
- O pacote deve ser público.

Exemplo:
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

### Empacotando vários arquivos de origem do Go
{: #prep_go_multi}

1. Crie um diretório `src` de nível superior. Coloque os arquivos de origem que pertencem ao pacote principal na raiz de `src` ou dentro de um diretório `main` e crie subdiretórios para outros pacotes. Por exemplo, o pacote `hello` torna-se o diretório `src/hello`.
  ```
  go-action-hello/
  └── src
      ├── hello
      | de ajuda -- hello.go
      └── main
          └── main.go
  ```
  {: screen}

2. Importe subpacotes. Exemplo de `main/main.go` importando o subpacote hello:

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

  Exemplo de `hello/hello.go`:

  ```go
  package hello

  import "fmt"

  // Hello return a greeting message
func Hello(name string) map[string]interface{} {
  	fmt.Println ("Este é hello.Hello")
	res: = fazer (mapa [ string ] interface { })
	res [ "body" ] = "Hello" + name
	retorne res
}
  ```
  {: codeblock}

3. Compile o código. Crie um archive .zip do diretório `src`. **Não** inclua o diretório de projeto de nível superior `go-action-project/`.

  ```bash
  cd src
zip -r ../hello-src.zip *
cd ..
  ```
  {: pre}

  É possível compilar localmente, configurando seu `GOPATH` para o pai do diretório `src`. Se você usar VSCode, deverá mudar a configuração `go.inferGopath` para `true`.
  {: note}

4. Compile e compacte o executável Go como `exec` na raiz do archive .zip. Construa o archive `hello-bin.zip` ao executar o comando a seguir. Deve-se ter a CLI do Docker instalada em sua estação de trabalho e `docker` em seu `PATH`.

  ```bash
  docker run -i openwhisk/actionloop-golang-v1.11 -compile main < hello-src.zip >hello-bin.zip
  ```
  {: pre}

  Neste exemplo, a função principal é `-compile main`. Para usar uma função diferente como principal, mude o valor para `-compile`. A função principal é selecionada no tempo de compilação. Ao pré-compilar, `ibmcloud fn action [update | create]` ignora o `--main`.

  O contêiner obtém o conteúdo do .zip de origem em `stdin`, compila o conteúdo e cria um novo archive .zip com o executável `exec` na raiz. O conteúdo do archive .zip flui para `stdout` que é redirecionado para o archive `hello-bin.zip` a ser implementado como a ação do Go.




### Empacotando código de Go com bibliotecas do fornecedor
{: #prep_go_vendor}

É possível incluir dependências, preenchendo um diretório `vendor` dentro do archive `zip` de origem quando você compila os arquivos Go. O diretório `vendor` não funciona no nível superior. Deve-se colocar o diretório `vendor` dentro de `src/` e dentro de um diretório de pacote.
{: shortdesc}

Pacote de log de exemplo `logrus` em um app `hello.go`:

```go
package hello

import (
	"os"

	"github.com/Sirupsen/logrus"
)

var log = logrus.New ()

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
Neste exemplo, o diretório `vendor` está localizado em `src/hello/vendor`. É possível incluir bibliotecas de terceiros que são usadas pelo pacote `hello`. É possível usar várias ferramentas, como [dep ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://golang.github.io/dep/docs/installation.html) para preencher e gerenciar dependências.

É possível usar `dep` criando um arquivo `src/main/Gopkg.toml` descrevendo a versão e o local das bibliotecas.

```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

Preencha o diretório `vendor`, execute `dep ensure`.







## Preparando apps Swift
{: #prep_swift}

Os arquivos Swift devem ser compilados em um binário antes que uma ação seja executada. Esse atraso é conhecido como o atraso de cold start. Para evitar o atraso de cold start, é possível compilar seu arquivo Swift em um binário e, em seguida, fazer upload do binário para o {{site.data.keyword.openwhisk_short}} em um arquivo .zip. O tempo de execução do Docker inclui um compilador para ajudar os usuários a compilar e empacotar ações do Swift 4.2. As chamadas subsequentes para a ação serão muito mais rápidas até que o contêiner que retém sua ação seja limpo.

As ações do Swift são executadas em um ambiente Linux. O Swift no Linux
ainda está em desenvolvimento e o {{site.data.keyword.openwhisk_short}} usa a liberação mais recente
disponível. Talvez essas liberações não estejam estáveis. A versão do Swift que é usada com o
{{site.data.keyword.openwhisk_short}} pode estar inconsistente com as versões do Swift de liberações
estáveis do Xcode no MacOS.
{: important}



### Estruturando o código Swift
{: #prep_swift_struc}

O nome esperado para a função de ponto de entrada é `main`. Se a função em seu código não for `main`, anote o nome para especificá-la quando a ação for criada.

Além da assinatura da função principal, o Swift 4 fornece mais duas assinaturas que aproveitam o tipo [Codable ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.apple.com/documentation/swift/codable). É possível aprender mais sobre os tipos de dados que são [codificáveis e decodificáveis para compatibilidade com representações externas, como JSON ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types).

Exemplo:
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


#### Manipulando erros no Swift
{: #prep_swift_error}

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


### Empacotando um arquivo Swift 4.2 em um binário
{: #prep_swift42_single}

Compile um arquivo de origem única que não dependa de bibliotecas externas. Use o sinalizador `-compile` com o nome do método principal.

Antes de começar:
- [Instale o Docker](https://hub.docker.com/search?offering=community&type=edition).
- [Revise os pacotes incluídos com o tempo de execução do Swift](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions) para ver se uma dependência de seu app já está incluída com o tempo de execução. Caso sua dependência não seja incluída, deve-se empacotá-la com seu app.

Para empacotar seu app:

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main < hello.swift >hello.zip
```
{: pre}

O contêiner Docker lê o conteúdo do arquivo de `stdin` e grava um archive .zip com o executável swift compilado para `stdout`.



### Empacotando projetos de vários arquivos e dependências do Swift 4.2
{: #prep_swift42_multi}

Antes de começar:
- [Instale o Docker](https://hub.docker.com/search?offering=community&type=edition).
- [Revise os pacotes incluídos com o tempo de execução do Swift](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions) para ver se uma dependência de seu app já está incluída com o tempo de execução. Caso sua dependência não seja incluída, deve-se empacotá-la com seu app.

Para empacotar seu app:

1. para compilar vários arquivos e incluir dependências externas, crie a estrutura de diretório a seguir.

  ```
  .
  ├── Package.swift
  └── Sources
      └── main.swift
  ```
  {: codeblock}

  O diretório `Sources/` contém um arquivo denominado `main.swift`.

  O `Package.swift` deve iniciar com um comentário especificando a versão `4.2` para o conjunto de ferramentas do Swift:

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

2. Crie um archive .zip com o conteúdo do diretório:

  ```bash
  zip ../action-src.zip -r *
  ```
  {: codeblock}

3. Passe o archive .zip para o contêiner do Docker sobre `stdin`. O `stdout` é um novo archive .zip com o executável compilado. O contêiner Docker lê o conteúdo do archive .zip de `stdin` e grava um novo archive .zip com o executável do Swift compilado para `stdout`.

  ```
  docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
  ```
  {: codeblock}

  Em um sistema baseado em Linux, é possível combinar as etapas `zip` e `docker run` em um único comando:

  ```
  zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
  ```
  {: codeblock}





## Empacotando apps Python
{: #prep_python}


### Estruturando o código Python
{: #prep_python_struct}

- Os apps Python devem consumir um dicionário e produzir um dicionário.
- O nome esperado para o método de ponto de entrada é `main`. Se a função em seu código não for `main`, anote o nome para especificá-la quando a ação for criada.
{: shortdesc}

Exemplo:
```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
	print(greeting)
    return {"greeting": greeting}
```

### Empacotando código Python
{: #prep_python_pkg}

Empacote o código Python e os módulos dependentes em um arquivo .zip. Neste exemplo, o arquivo de origem que contém o ponto de entrada é `__main__.py` e os módulos auxiliares estão em um arquivo chamado `helper.py`.

Antes de iniciar, [revise os pacotes incluídos com o tempo de execução do Python](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) para ver se uma dependência de seu app já está incluída com o tempo de execução. Caso sua dependência não seja incluída, deve-se empacotá-la com seu app.

Para empacotar seu app:

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}


### Empacotando o código Python com um ambiente virtual em arquivos .zip
{: #prep_python_virtenv}

É possível empacotar dependências do Python usando um ambiente virtual, `virtualenv`. O ambiente virtual permite vincular pacotes adicionais que podem ser instalados usando o [`pip` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://packaging.python.org/installing/).

Antes de iniciar, [revise os pacotes incluídos com o tempo de execução do Python](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) para ver se uma dependência de seu app já está incluída com o tempo de execução. Caso sua dependência não seja incluída, deve-se empacotá-la com seu app.

Para empacotar seu app:

1. Crie um arquivo [requirements.txt
![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://pip.pypa.io/en/latest/user_guide/#requirements-files)
que contenha os módulos e as versões do `pip` a serem instalados.

  Para manter o `virtualenv` para um tamanho mínimo, inclua apenas os módulos que não fazem parte do ambiente de tempo de execução selecionado para o `requirements.txt`. Para obter mais informações sobre os pacotes que estão incluídos nos tempos de execução do Python, consulte a [referência de tempo de execução](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) do Python.
  {: tip}

2. Puxe uma das imagens a seguir para seu tempo de execução. Para assegurar a compatibilidade com o contêiner de tempo de execução, as instalações empacotadas dentro de um ambiente virtual devem usar a imagem que corresponde ao tempo de execução especificado.
    * Para `python:3.7`, use a imagem do Docker `ibmfunctions/action-python-v3.7`.
    * Para `python:3.6`, use a imagem do Docker `ibmfunctions/action-python-v3.6`.
    * Para `python:2`, use a imagem do Docker `openwhisk/python2action`.

   Exemplo:
   ```
   docker pull ibmfunctions/action-python-v3.7
   ```
   {: pre}

2. Instale as dependências e crie um ambiente virtual. O diretório de ambiente virtual deve ser
denominado `virtualenv`.

   ```
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. Empacote o diretório `virtualenv` e quaisquer arquivos Python adicionais. O arquivo
de origem que contém o ponto de entrada deve ser denominado `__main__.py`.

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}





## Preparando apps Ruby
{: #prep_ruby}

Antes de criar uma ação, tenha seu código Ruby pronto.

### Estruturando código Ruby
{: #prep_ruby_struct}

* As ações do Ruby sempre consomem um Hash (coleta como de dicionário) e retornam um Hash.
* O nome esperado para a função de ponto de entrada é `main`. Se a função em seu código não for `main`, anote o nome para especificá-la quando a ação for criada.


Exemplo:
```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
    ```
    {: codeblock}

### Empacotando código Ruby
{: #prep_ruby_pkg}

É possível empacotar um app Ruby e pacotes dependentes em um arquivo .zip. Por exemplo, é possível empacotar uma ação com um segundo arquivo chamado `helper.rb`.

Crie um archive contendo os seus arquivos de origem. O arquivo de origem que contém o ponto de entrada deve ser denominado `main.rb`.

```bash
zip -r helloRuby.zip main.rb helper.rb
```
{: pre}

Os gems `mechanize` e `jwt` estão disponíveis além dos gems padrão e empacotados. É possível usar gemas arbitrárias, desde que você use ações compactadas para empacotar todas as dependências.



## Preparando apps PHP
{: #prep_php}

Antes de criar uma ação, tenha seu código PHP pronto.

### Estruturando código PHP
{: #prep_php_struct}

- As ações PHP consomem uma matriz associativa e retornam uma matriz associativa.
- O nome esperado para a função de ponto de entrada é `main`. Se a função em seu código não for `main`, anote o nome para especificá-la quando a ação for criada.

Exemplo:
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

### Empacotando código PHP
{: #prep_php_pkg}

É possível empacotar arquivos PHP ou pacotes dependentes em um arquivo .zip.

Antes de iniciar, [revise os pacotes que estão incluídos com o tempo de execução do PHP](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php) para ver se uma dependência de seu app já está incluída com o tempo de execução. Caso sua dependência não seja incluída, deve-se empacotá-la com seu app.

Para empacotar seu app:

```bash
zip -r ARCHIVE_NAME.zip FILE_1.php FILE_2.php
```
{: pre}

Exemplo:
```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}







## Preparando apps Java
{: #prep_java}

Antes de criar uma ação, tenha seu código Java pronto.

### Estruturando código Java
{: #prep_java_struct}

Uma ação Java é um programa Java com um método chamado `main`. `main` deve ter a assinatura a seguir.


```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}


* Deve-se especificar o nome da classe principal usando `--main`. Uma classe principal elegível é aquela que implementa um método `main` estático. Se a classe não estiver no pacote padrão, use o nome completo de classe Java, por exemplo, `--main com.example.MyMain`.
* É possível customizar o nome do método de sua ação Java. Isso é feito especificando o nome completo do método de sua ação, por exemplo, `--main com.example.MyMain#methodName`.
* O tipo de ação é determinado pelo uso da extensão do arquivo de origem.

Exemplo:
```java
import com.google.gson.JsonObject;
public class Hello {
    public static JsonObject main(JsonObject args) {
        String name = "stranger";
        if (args.has("name"))
            name = args.getAsJsonPrimitive("name").getAsString();
        JsonObject response = new JsonObject();
        response.addProperty("greeting", "Hello " + name + "!");
        return response;
    }
}
```
{: codeblock}


### Empacotando código Java
{: #prep_java_pkg}

Para compilar, testar e arquivar arquivos Java, deve-se ter o [JDK 8 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://openjdk.java.net/install) instalado localmente.

1. Salve o código a seguir em um arquivo denominado `Hello.java`.

    ```java
    import com.google.gson.JsonObject;
public class Hello {
        public static JsonObject main(JsonObject args) {
            String name = "stranger";
        if (args.has("name"))
            name = args.getAsJsonPrimitive("name").getAsString();
        JsonObject response = new JsonObject();
        response.addProperty("greeting", "Hello " + name + "!");
        return response;
    }
    }
    ```
    {: codeblock}

2. Compile o arquivo `Hello.java` em um arquivo de classe.

    ```
    javac Hello.java
    ```
    {: pre}

2. Compacte o arquivo de classe em um arquivo JAR denominado `hello.jar`. **Nota:** o [google-gson ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/google/gson) deve existir em seu CLASSPATH Java.
3.
    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}


### Empacotando código Java com Gradle
{: #prep_java_gradle}

É possível usar uma construção de uma ferramenta como [Gradle](https://gradle.org) para buscar as bibliotecas de um repositório como Maven Central e construir um archive JAR final que inclua seu código e todas as dependências.

Aqui está um exemplo usando Gradle para construir uma ação Java que alavanque a biblioteca `com.google.zxing` que fornece a funcionalidade para gerar uma imagem de código QR.

1. Crie um arquivo denominado `build.gradle` e especifique as dependências.

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

2. Execute o comando `gradle jar`, que gera um archive JAR no diretório `build/libs/`.

  Para obter mais informações, leia a documentação do Gradle [Declarando dependências](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies).





## Preparando apps .NET Core
{: #prep_dotnet}

Antes de criar uma ação, tenha seu código .NET Core pronto.

### Estruturando o código .NET Core
{: #prep_dotnet_struct}

Uma ação .NET Core é uma biblioteca de classes .NET Core com um método espera-se que seja denominado `Main`. Se o método em seu código não for `Main`, anote o nome para especificá-lo quando a ação for criada no formato: `--main {Assembly}::{Class Full Name}::{Method}`

Exemplo:
```
Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main
```

### Empacotando o código .NET Core
{: #prep_dotnet_pkg}

Antes de iniciar: para compilar, testar e arquivar projetos .NET Core, deve-se:
- instalar o [.NET Core SDK](https://dotnet.microsoft.com/download) localmente.
- Configurar a variável de ambiente `DOTNET_HOME` para o local no qual o executável `dotnet` pode ser localizado.



Para empacotar seu código:

  1. Crie um projeto C# chamado `Apache.OpenWhisk.Example.Dotnet`.

      ```bash
      dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
      ```
      {: pre}

  2. Navegue para o diretório `Apache.OpenWhisk.Example.Dotnet`.

      ```bash
      cd Apache.OpenWhisk.Example.Dotnet
      ```
      {: pre}

  3. Instale o pacote NuGet [Newtonsoft.Json](https://www.newtonsoft.com/json) conforme a seguir.

      ```bash
      dotnet add package Newtonsoft.Json -v 12.0.1
      ```
      {: pre}

  4. Salve o código a seguir em um arquivo denominado `Hello.cs`.

      ```csharp
      usando System;
    usando Newtonsoft.Json.Linq;

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

  5. Compile `Hello.cs`e quaisquer outros arquivos e saída para o diretório `out`.

      ```bash
      dotnet publish -c Release -o out
      ```
      {: pre}

  6. Navegue para o diretório de saída.

      ```bash
      cd out
      ```
      {: pre}

  7. Compacte os arquivos publicados.

      ```bash
      zip -r -0 ../helloDotNet.zip *
      ```
      {: pre}


