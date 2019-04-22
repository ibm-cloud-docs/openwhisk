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

# Go アクションの作成
{: #creating-go-actions}

以下のセクションでは、単一 Go アクションの作成と呼び出し、および、そのアクションへのパラメーターの追加について説明します。 Go 1.11 を使用して Go アクションを実行できます。このランタイムを使用するには、アクションの作成時または更新時に `ibmcloud fn` CLI パラメーター `--kind go:1.11` を指定します。
{: shortdesc}

## Go アクションの作成と呼び出し
{: #invoking-go-actions}

Go アクションは、`main` パッケージのみで構成されるパブリック関数です。クイック・テストや開発のときには、1 つのファイルを使用します。実動アプリの場合は、[Go アクションを実行可能ファイルとしてプリコンパイル](#packaging-go-actions)すると、パフォーマンスが良くなり、複数のソース・ファイルやベンダー・ライブラリーを使用できるようになります。
{: shortdesc}

Go アクションを作成します。

1. 以下のコードを `hello.go` という名前のファイルに保存します。 この例では、関数名が `Main` になっていますが、`--main` フラグを使用して名前を変更することも可能です。関数名を変更する場合は、名前の先頭を大文字にしてください。`main` という語は使用できません。これは Go パッケージの名前です。

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
    `Main` 関数で予期されるシグニチャーは以下のとおりです。

    ```go
    func Main(event map[string]interface{}) map[string]interface{}
    ```
    {: codeblock}

2. `helloGo` というアクションを作成するか更新します。

    ```bash
    ibmcloud fn action update helloGo hello.go --kind go:1.11
    ```
    {: pre}

3. アクションを呼び出します。

    ```bash
    ibmcloud fn action invoke --result helloGo --param name gopher
    ```
    {: pre}

    出力例:
    ```json
      {
          "greeting": "Hello gopher!"
      }
    ```
    {: screen}

## Go 実行可能ファイルとしてのアクションのパッケージ化
{: #packaging-go-actions}

`GOOS=Linux` と `GOARCH=amd64` を使用してクロスコンパイルを実行することによって、どの Go プラットフォームでもバイナリーを作成することは可能ですが、ランタイム・コンテナー・イメージに組み込まれているプリコンパイル・フィーチャーを使用してください。[複数のソース・ファイル](#multiple-packages-go-actions)や[ベンダー・ライブラリー](#vendor-libs-go-actions)をパッケージすることができます。
{: shortdesc}

### 複数のパッケージ・ソース・ファイルの処理
{: #multiple-packages-go-actions}

複数のパッケージ・ソース・ファイルを使用するには、最上位の `src` ディレクトリーを使用し、メインパッケージに属するソース・ファイルを `src` のルートか `main` ディレクトリーの中に配置して、他のパッケージのためのディレクトリーを作成します。例えば、`hello` パッケージは `src/hello` ディレクトリーになります。
{: shortdesc}

```
go-action-hello/
└── src
    ├── hello
    │   └── hello.go
    └── main
        └── main.go
```
{: screen}

このレイアウトを使用すれば、以下の例のようにサブパッケージをインポートできます (`import "hello"`)。

`GOPATH` を `src` ディレクトリーの親に設定することによって、ローカル環境でコンパイルできるようになります。VSCode を使用する場合は、`go.inferGopath` 設定を `true` にして有効にすることが必要です。
{: note}

`main/main.go` の例を以下に示します。

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

`hello/hello.go` の例を以下に示します。

```go
package hello

import "fmt"

// Hello return a greeting message
func Hello(name string) map[string]interface{} {
	fmt.Println("This is hello.Hello")
	res := make(map[string]interface{})
	res["body"] = "Hello " + name
	return res
}
```
{: codeblock}

ランタイム環境を使用してコンパイルできます。`src` ディレクトリーの内容を組み込んだ .zip アーカイブを作成してください。最上位のプロジェクト・ディレクトリー `go-action-project/` は**組み込まない**でください。.zip アーカイブ `hello-src.zip` を作成するには、以下のようにします。

```bash
cd src
zip -r ../hello-src.zip *
cd ..
```
{: pre}

.zip アーカイブのルートで Go 実行可能ファイルを `exec` としてコンパイルしてパッケージします。以下のコマンドを実行して、`hello-bin.zip` アーカイブを作成してください。そのためには、ワークステーションに Docker CLI をインストールして、`PATH` に `docker` を入れておくことが必要です。

```bash
docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
```
{: pre}

この例では、`-compile main` が main 関数です。別の関数をメインとして使用する場合は、`-compile` の値を変更してください。
コンパイル時に main 関数が選択されます。プリコンパイル時には、`ibmcloud fn action [update | create]` で `--main` は無視されます。

コンテナーが `stdin` でソース .zip の内容を取り込み、その内容をコンパイルし、新しい .zip アーカイブを作成し、そのルートに実行可能ファイル `exec` を組み込みます。.zip アーカイブの内容が `stdout` にストリーム送信されると `hello-bin.zip` アーカイブにリダイレクトされ、Go アクションとしてデプロイされます。

CLI と新しい .zip アーカイブ `hello-bin.zip` を使用して、実動用としてアクションを更新できます。

```bash
ibmcloud fn action update helloGo hello-bin.zip --kind go:1.11
```
{: codeblock}

### ベンダー・ライブラリーの処理
{: #vendor-libs-go-actions}

Go アクションをコンパイルする時に、ソース `zip` アーカイブの中にある `vendor` ディレクトリーに依存項目を組み込むことができます。`vendor` ディレクトリーは、最上位では正しく動作しません。`vendor` ディレクトリーは、`src/` の中とパッケージ・ディレクトリーの中に配置する必要があります。
{: shortdesc}

前の例をさらに続けて、`hello.go` にあるログ・パッケージ `logrus` を使用します。

```go
package hello

import (
	"os"

	"github.com/Sirupsen/logrus"
)

var log = logrus.New()

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
この例では、`vendor` ディレクトリーが `src/hello/vendor` に配置されています。`hello` パッケージで使用するサード・パーティー・ライブラリーを追加できます。依存項目の追加や管理のために、複数のツール ([dep ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://golang.github.io/dep/docs/installation.html) など) を使用できます。

`dep` を使用するには、ライブラリーのバージョンと場所を指定した `src/main/Gopkg.toml` ファイルを作成します。

```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

`vendor` ディレクトリーに項目を追加し、`dep ensure` を実行します。

デプロイメント・ツールやスクリプト ([Make ファイル](#makefile)など) を使用して、このプロセスを自動化することもできます。
{: tip}

### Go アクションでの Make ファイルの使用
{: #makefile}

前の例をさらに続けます。プロジェクト・ディレクトリーは以下のとおりです。

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

1. デプロイメント・プロセスを自動化するためのファイルである `Make ファイル`を作成します。

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

src/%/vendor:
	cd $(@D) ; DEPPROJECTROOT=$(realpath $(@D)/../..) dep ensure

clean:
	-rm -rf $(BINZIP) $(SRCZIP) $(VENDORS)

invoke:
	$(CLI) action invoke $(NAME) -r -p name Gopher
```
{: codeblock}

2. .zip アーカイブと vendor ディレクトリーを削除します。

```bash
make clean
```
{: pre}

3. vendor ディレクトリーに項目を追加し、ソース .zip を作成し、ソース・コードをコンパイルし、exec を .zip アーカイブに組み込み、Go アクションをデプロイするために、以下のコマンドを実行します。

```bash
make deploy
```
{: pre}

4. `go-action-hello` が作成されたので、アクションを呼び出します。

```bash
ibmcloud fn action invoke go-action-hello -r -p name Go
```
{: pre}

出力例:

```json
    {
        "greeting": "Hello Go"
    }
```
{: screen}
