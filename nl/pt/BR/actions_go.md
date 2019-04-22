---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: docker, actions, serverless

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

# Criando ações do Go
{: #creating-go-actions}

As seções a seguir orientam você na criação e na chamada de uma ação do Go única e na inclusão de parâmetros nessa ação. É possível executar ações do Go usando o Go 1.11. Para usar esse tempo de execução, especifique o parâmetro da CLI `ibmcloud fn`
`-- kind go: 1.11`ao criar ou atualizar uma ação.
{: shortdesc}

## Criando e chamando uma ação Go
{: #invoking-go-actions}

Uma ação do Go é simplesmente uma função pública do pacote `main`. Use um arquivo único para fins de teste rápido ou desenvolvimento. Para apps de produção, [pré-compile suas ações do Go em um executável](#packaging-go-actions) para obter melhor desempenho ou suporte a múltiplos arquivos de origem, incluindo bibliotecas do fornecedor.
{: shortdesc}

Crie uma ação do Go.

1. Salve o código a seguir em um arquivo denominado `hello.go`. No exemplo, a função é denominada `Main`, mas é possível mudar o nome usando o sinalizador `--main`. Se você renomear a função, o nome deverá começar com uma letra maiúscula e não poderá ser o termo `main`, que é o nome do pacote Go.

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

    </br>
    A assinatura esperada para uma função `Main` é:

    ```go
    func Main(event map[string]interface{}) map[string]interface{}
    ```
    {: codeblock}

2. Crie ou atualize uma ação chamada `helloGo`.

    ```bash
    ibmcloud fn action update helloGo hello.go --kind go:1.11
    ```
    {: pre}

3. Invoque a ação.

    ```bash
    ibmcloud fn action invoke --result helloGo --param name gopher
    ```
    {: pre}

    Exemplo de Saída:
    ```json
      {
          "greeting": "Hello gopher!"
      }
    ```
    {: screen}

## Empacotando uma ação como um executável Go
{: #packaging-go-actions}

Embora seja possível criar um binário em qualquer plataforma Go, compilando com `GOOS=Linux` e `GOARCH=amd64`, use o recurso de pré-compilação que está integrado à imagem do contêiner de tempo de execução. É possível empacotar [vários arquivos de origem](#multiple-packages-go-actions) ou [bibliotecas do fornecedor](#vendor-libs-go-actions).
{: shortdesc}

### Trabalhando com vários arquivos de origem do pacote
{: #multiple-packages-go-actions}

Para usar vários arquivos de origem de pacote, use um diretório `src` de nível superior, coloque os arquivos de origem que pertencem ao pacote principal na raiz de `src` ou dentro de um diretório `main` e crie diretórios para outros pacotes. Por exemplo, o pacote `hello` torna-se o diretório `src/hello`.
{: shortdesc}

```
go-action-hello/
└── src
    ├── hello
    | de ajuda -- hello.go
    └── main
        └── main.go
```
{: screen}

Com esse layout, é possível importar subpacotes (`import "hello"`) conforme mostrado no exemplo a seguir.

É possível compilar localmente, configurando seu `GOPATH` para o pai do diretório `src`. Se você usar VSCode, será necessário ativar a configuração `go.inferGopath` como `true`.
{: note}

Exemplo de `main/main.go`:

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
		= greetings name
	} retornar hello.Hello (saudações)
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

É possível compilar usando o ambiente de tempo de execução. Crie um archive .zip do conteúdo do diretório `src`. **Não** inclua o diretório de projeto de nível superior `go-action-project/`. Para criar o archive .zip `hello-src.zip`:

```bash
cd src
zip -r ../hello-src.zip *
cd ..
```
{: pre}

Compile e compacte o executável Go como `exec` na raiz do archive .zip. Construa o archive `hello-bin.zip` ao executar o comando a seguir. Isso supõe que você tenha a CLI do Docker instalada em sua estação de trabalho e `docker` em seu `PATH`.

```bash
docker run -i openwhisk/actionloop-golang-v1.11 -compile main < hello-src.zip >hello-bin.zip
```
{: pre}

Neste exemplo, a função principal é `-compile main`. Para usar uma função diferente como principal, mude o valor para `-compile`.
A função principal é selecionada no tempo de compilação. Ao pré-compilar, `ibmcloud fn action [update | create]` ignora o `--main`.

O contêiner obtém o conteúdo do .zip de origem em `stdin`, compila o conteúdo e cria um novo archive .zip com o executável `exec` na raiz. O conteúdo do archive .zip flui para `stdout` que é redirecionado para o archive `hello-bin.zip` a ser implementado como a ação do Go.

Agora, é possível atualizar sua ação para produção usando a CLI e o novo archive .zip `hello-bin.zip`.

```bash
ibmcloud fn action update helloGo hello-bin.zip --kind go:1.11
```
{: codeblock}

### Trabalhando com bibliotecas do fornecedor
{: #vendor-libs-go-actions}

É possível incluir dependências, preenchendo um diretório `vendor` dentro do archive `zip` de origem ao compilar a ação do Go. O diretório `vendor` não funciona no nível superior. É necessário colocar o diretório `vendor` dentro de `src/` e dentro de um diretório de pacote.
{: shortdesc}

Continuando com o exemplo anterior, use o pacote de log `logrus` em `hello.go`.

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

É possível automatizar esse processo usando uma ferramenta de implementação ou um script, como um [Makefile](#makefile).
{: tip}

### Usando um Makefile para ações do Go
{: #makefile}

Continuando com o exemplo anterior, seu diretório de projeto é o seguinte.

```bash
go-action-hello/
├── Makefile
└── src
    ├── hello
    │   ├── Gopkg.toml
    │   ├── hello.go
    │   └── vendor/
    └── main
        └── main.go
```
{: screen}

1. Crie o arquivo `Makefile` para automatizar o processo de implementação.

```Makefile
GO_COMPILER?=openwhisk/actionloop-golang-v1.11
CLI?=ibmcloud fn
MAIN=main
APP=hello
SRCS=$(MAIN)/$(MAIN).go $(APP)/$(APP).go
VENDORS=$(APP)/vendor
NAME=go-action-$(APP)
BINZIP=$(APP)-bin.zip
SRCZIP=$(APP)-src.zip

deploy: $(BINZIP)
	$(CLI) action update $(NAME) $(BINZIP) --main $(MAIN) --kind go:1.11

$(BINZIP): $(SRCZIP)
	docker run -i $(GO_COMPILER) -compile $(MAIN) <$(SRCZIP) >$(BINZIP)

$(SRCZIP): src/$(VENDORS)
	cd src ; zip ../$(SRCZIP) -r $(SRCS) $(VENDORS)

src/%/fornecedor:
	cd $(@D); DEPPROJECTROOT = $(realpath $(@D) / ../..) dep ensure

clean:
	-rm -rf $(BINZIP) $(SRCZIP) $(VENDORS)

invoke:
	$(CLI) action invoke $(NAME) -r -p name Gopher
```
{: codeblock}

2. Exclua os archives .zip e o diretório do fornecedor.

```bash
make clean
```
{: pre}

3. Preencha o diretório do fornecedor, crie o .zip de origem, compile o código-fonte, arquive o exec em um .zip e implemente a ação do Go executando.

```bash
make deploy
```
{: pre}

4. Agora que o `go-action-hello` foi criado, chame a ação.

```bash
ibmcloud fn action invoke go-action-hello -r -p name Go
```
{: pre}

Exemplo de Saída:

```json
    {
        "Saudação": "Hello Go"
    }
```
{: screen}
