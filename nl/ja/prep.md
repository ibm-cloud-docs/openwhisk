---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-10"

keywords: actions, serverless, javascript, node, node.js, functions

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
{:external: target="_blank" .external}


# アクションとして使用するアプリを準備する
{: #prep}

アプリを持ち込むのか、イベントに応答するスクリプトを特別に作成するのかにかかわらず、コードからアクションを作成するためには、コードがいくつかの要件を満たしている必要があります。
{: shortdesc}

プログラミング言語ごとに固有の実行要件がありますが、ほとんどのプログラミング言語には次の一般的な要件が適用されます。
- コードのエントリー・ポイントの名前は、デフォルトでは `main` と想定されます。 エントリー・ポイントが `main` ではない場合は、アクションの作成時にカスタム名を指定できるので、その名前をメモしておいてください。
- アプリの入力パラメーターとアプリの出力結果は、エンティティー間で受け渡し可能な特定の構造になるように形式設定する必要があります。 この構造は、コードの言語によって異なります。 例えば、Python アプリの場合、入力パラメーターはディクショナリーでなければならず、アプリの結果をディクショナリーとして構造化する必要があります。パラメーターを構造化オブジェクトに入れてアクションに渡すこともできるからです。例えば JSON では、`name` や `place` のような特定のフィールドの JSON 値が設定された入力パラメーターを予期するようにコードを構造化することもできます。

    **JSON 入力の例**
    ```json
    {"name": "Dorothy", "place": "Kansas"}
    ```
    {: codeblock}

    **JavaScript の例**
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
    ```
    {: codeblock}
- アプリに複数のファイルが含まれている場合は、アクションで使用するためにそれらのファイルをまとめて 1 つのファイルにする必要があります。 コードを書き直して 1 つファイルにするか、ファイルや依存関係をパッケージ化して単一のアーカイブ・ファイルにします。 使用するランタイムがサポートされていない場合は、アプリを Docker イメージとしてパッケージ化します。
- 依存関係もアプリと一緒にパッケージに入れる必要があります。 提供されているランタイムには、いくつかのパッケージと拡張機能が事前にインストールされています。 [使用するランタイムの参照情報を確認](/docs/openwhisk?topic=cloud-functions-runtimes)して、アプリの依存関係がランタイムに既に含まれているかどうかを調べてください。 依存関係が含まれている場合は、依存関係をアプリと一緒にパッケージ化する必要はありません。

    コードのコンパイルは必須ではありませんが、ランタイムで可能であれば、コードを事前にコンパイルしておくとパフォーマンスが向上する可能性があります。
    {: tip}

## アプリを Docker イメージとして準備する
{: #prep_docker}

{{site.data.keyword.openwhisk_short}} では、任意の言語でアプリを作成して Docker イメージとしてパッケージ化できます。
{: shortdesc}

使用できるイメージは、パブリック・レジストリーにあるイメージだけです (Docker Hub で公開されているイメージなど)。 プライベート・レジストリーはサポートされません。
{: important}

### コードを Docker イメージとしてパッケージ化する
{: #prep_docker_pkg}

コードは実行可能ファイルにコンパイルされ、Docker イメージに組み込まれます。実行可能ファイルは、`stdin` から入力を受け取り、`stdout` を通して応答することでシステムと対話します。
{: shortdesc}

**始める前に**
- ユーザーは Docker Hub アカウントを持っている必要があります。 [Docker Hub](https://hub.docker.com){: external} で、無料の Docker ID とアカウントをセットアップできます。
- [Docker をインストールします](https://hub.docker.com/search/?offering=community&type=edition){:external}。
- [Docker ランタイムの要件を確認します](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker)。

アプリをパッケージするには、以下のステップを実行します。

コードを Docker イメージとしてパッケージ化するには、以下のコマンドを実行します。
1. Docker スケルトンをダウンロードしてインストールします。 このスケルトンは、Docker コンテナー・テンプレートであり、そこにカスタム・バイナリーの形でコードを注入できます。
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. ブラック・ボックス・スケルトン内にコードをセットアップします。 スケルトンには C プログラムが含まれているので、それを使用できます。 `example.c` ファイルの部分は Docker イメージのビルド・プロセスの一環でコンパイルされるので、C を自分のマシンでコンパイルする必要はありません。
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  **出力例**
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. (オプション) 実行可能ファイルをビルドするように `Dockerfile` を変更することで、さらなるコードと依存関係を Docker イメージに追加します。 次の要件に注意してください。
  * コードはコンテナー内部の `/action/exec` に置く必要があります。
  * 実行可能ファイルはコマンド・ラインから単一の引数を受け取ります。 この引数は、アクションへの引数を表す JSON オブジェクトのストリング・シリアライゼーションです。
  * このプログラムは `stdout` または `stderr` にログを出力できます。
  * 規則により、出力の最終行は、<ph class="ignoreSpelling">stringify</ph> によって文字列化された、アクションの結果を表す JSON オブジェクトでなければなりません。
  Dockerfile の構成方法について詳しくは、[Dockerfile reference](https://docs.docker.com/engine/reference/builder/){: external} を参照してください。

4. Docker イメージをビルドし、提供されているスクリプトを使用してアップロードします。
    1. Docker にログインします。
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. `dockerSkeleton` ディレクトリーに移動します。
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. スクリプトを実行します。
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}




## JavaScript アプリを準備する
{: #prep_js}

アクションを作成する前に、JavaScript コードの準備を整えます。 コードの構造が適切であることを確認してから、コードをパッケージ化する必要があるかどうかを判断します。
{: shortdesc}

### JavaScript コードを構造化する
{: #prep_js_struct}

- エントリー・ポイントの関数の名前は `main` と想定されます。 コードの関数が `main` でない場合は、その名前をメモし、アクションの作成時に指定してください。
- 複数の入力パラメーターを 1 つの JSON オブジェクトとして渡します。
- 成功したアクティベーションの結果も JSON オブジェクトですが、アクションが[同期](#prep_js_sync)か[非同期](#prep_js_async)かによって異なる方法で返されます。



**例**
```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```

**複数の関数を含む例**

  ```javascript
  function main() {
      return { payload: helper() }
  }

  function helper() {
      return new Date();
}
  ```
  {: codeblock}


### 同期動作の JavaScript コードを構造化する
{: #prep_js_sync}

main 関数が `return` ステートメントを実行せずに終了するか、promise 以外の値を返す `return` ステートメントを実行して終了する場合、その JavaScript のアクティベーションは同期的です。
{: shortdesc}

**同期コードの例。**

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




### 非同期動作の JavaScript コードを構造化する
{: #prep_js_async}

コールバック関数では return の後も JavaScript 関数の実行を継続できます。 main 関数が promise を返して終了する場合、JavaScript のアクティベーションは非同期的です。 この場合は、promise が履行または拒否されるまで、システムはそのアクションをまだ実行中であると見なします。 非同期実行の JavaScript 関数では、アクションの中で promise を返すことで、`main` 関数が戻った後にアクティベーションの結果を返すことができます。
{: shortdesc}

まずは、新規の promise オブジェクトをインスタンス化して、コールバック関数を渡します。 コールバックは、resolve と reject という 2 つの引数を使用します。これらはどちらも関数です。 すべての非同期コードが、そのコールバックに入っていきます。 アクション・ハンドラーの名前は、オブジェクトを受け入れたり、オブジェクト (またはオブジェクトの `Promise`) を返したりする際の標準的なシグニチャーに準拠している限り、どのような名前でもかまいません。

次の例では、resolve 関数を呼び出して promise を完了させていることがわかります。
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         resolve({ done: true });
         }, 2000);
      })
}
```
{: codeblock}

次の例は、reject 関数を呼び出して promise を拒否していることがわかります。
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 2000);
     })
}
```
{: codeblock}

例の実行内容を詳しく説明します。
* `main` 関数が promise を返します。 promise は、アクティベーションがまだ実行されていないけれども後で実行されるということを示すものです。
* JavaScript 関数 `setTimeout()` が、2 秒間待機してから promise のコールバック関数を呼び出します。このコールバック関数が非同期コードを表しています。
* promise のコールバックは、`resolve` 引数と `reject` 引数を受け入れます。この 2 つはどちらも関数です。
  * `resolve()` の呼び出しは、promise を履行し、アクティベーションが正常に実行されたことを示します。
  * `reject()` の呼び出しは、promise を拒否し、アクティベーションが正常に実行されなかったことを通知するために使用できます。


### 同期動作と非同期動作を含む JavaScript コードを構造化する
{: #prep_js_both}

次の例に示すように、入力によって同期的にも非同期的にもなるアクションにすることができます。
{: shortdesc}

```javascript
function main(params) {
     if (params.payload) {
        // asynchronous activation
         return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
         }, 2000);
      })
     }  else {
        // synchronous activation
         return {done: true};
      }
}
```
{: codeblock}





### JavaScript による外部 API 呼び出しの例
{: #prep_js_api}

次の例では、毎日、宇宙のユニークな画像を掲載している NASA Astronomy Picture of the Day (APOD) サービスの外部 API を呼び出しています。
{: shortdesc}


```javascript
let rp = require('request-promise')

function main(params) {
    const options = {
        uri: "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo",
            json: true
        }
        return rp(options)
    .then(res => {
            return { response: res }
    })
}
```
{: codeblock}

NASA APOD API への呼び出しが行われ、JSON 結果からフィールドが抽出されます。

次は、アクションを[作成](/docs/openwhisk?topic=cloud-functions-actions)し、[呼び出し](/docs/openwhisk?topic=cloud-functions-test)てテストします。 以下のサンプル・オブジェクトが返されます。

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






### JavaScript コードを `webpack` モジュールと一緒にパッケージ化する
{: #prep_js_pkg}

JavaScript のモジュール・バンドラー (例えば、`[webpack ](https://webpack.js.org/concepts/){: external}`) を使用してアプリをパッケージ化することができます。 `webpack` は、コードを処理する際、アクションで必要なすべてのモジュールが含まれた依存関係グラフを再帰的に作成します。
{: shortdesc}

始めに、[JavaScript ランタイムに組み込まれているパッケージを参照](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments)し、アプリの依存関係がランタイムに既に組み込まれているかどうかを調べます。 依存関係が組み込まれていない場合は、アプリと一緒にパッケージ化する必要があります。

1. `package.json` ファイルを作成します。 `webpack` を開発のための依存関係として追加します。

    ```json
    {
      "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
        "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10"
      },
  "dependencies": {
        "left-pad": "1.1.3"
      },
  "devDependencies": {
        "webpack": "^3.8.1"
      }
    }
    ```
    {: codeblock}

2. 以下の `webpack` 構成コードを `webpack.config.js` という名前のファイルに保存します。

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

3. アプリのコードを準備します。 この例 (`index.js` という名前のファイルとして保存可能) では、変数 `global.main` がアプリの main 関数として設定されています。

    **例**
    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    global.main = myAction;
    ```
    {: codeblock}

4. すべての依存関係をローカルにインストールします。

    ```
    npm install
    ```
    {: pre}

5. `webpack` バンドルをビルドします。

    ```
    npm run build
    ```
    {: pre}

    ファイル `dist/bundle.js` が作成され、アクションのソース・コードとしてデプロイされます。

6. `npm` スクリプトまたは CLI を使用して、アクションを作成します。

    * 以下の `npm` スクリプトを実行します。

        ```
        npm run deploy
        ```
        {: pre}

    * 以下の CLI コマンドを実行します。

        ```
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    `webpack` でビルドされたバンドル・ファイルでは、JavaScript の依存関係のみがサポートされています。 バンドルに他の依存関係がある場合、こうした依存関係はファイル `bundle.js` に含まれていないため、アクションの呼び出しが失敗することがあります。
    {: tip}



### JavaScript コードを NPM ファイルとしてパッケージ化する
{: #prep_js_npm}

単一の JavaScript ソース・ファイルにすべてのアクション・コードを記述する代わりに、.zip ファイル形式の `npm` パッケージとしてコードをパッケージ化することもできます。
{: shortdesc}

始めに、[JavaScript ランタイムに組み込まれているパッケージを参照](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments)し、アプリの依存関係がランタイムに既に組み込まれているかどうかを調べます。 依存関係が組み込まれていない場合は、アプリと一緒にパッケージ化する必要があります。

1. ルート・ディレクトリーに、`package.json` ファイルを作成します。 

**例**

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

2. すべての依存関係をローカルにインストールします。

    ```
    npm install DEPENDENCY
    ```
    {: pre}

    ほとんどの `npm` パッケージでは、`npm install` を実行すると JavaScript のソースがインストールされますが、中には、バイナリー・ファイル成果物のインストールおよびコンパイルを行うパッケージもあります。 アーカイブ・ファイルのアップロードでは、JavaScript の依存関係のみがサポートされています。 アーカイブにバイナリー・ファイルの依存関係が含まれていると、アクションの呼び出しが成功しないことがあります。
    {: note}

3. すべての依存関係をはじめ、すべてのファイルが含まれている `.zip` アーカイブを作成します。

    ```
    zip -r action.zip *
    ```
    {: pre}

    **Windows ユーザー** Windows エクスプローラーの操作で .zip ファイルを作成すると、誤ったファイル構造になります。 {{site.data.keyword.openwhisk_short}} の .zip 操作では `package.json` がアーカイブのルートに置かれますが、Windows エクスプローラーではネストされたフォルダーの中に配置されます。 代わりに、`zip` コマンドを使用してください。
    {: tip}





## Go アプリを準備する
{: #prep_go}

クイック・テストや開発のためには、1 つのファイルを使用します。 実動アプリの場合は、パフォーマンスの向上、およびベンダー・ライブラリーを含む複数のソース・ファイルのサポートのために、Go アクションを実行可能ファイルとしてプリコンパイルします。
{: shortdesc}

`GOOS=Linux` と `GOARCH=amd64` を使用してクロスコンパイルを実行することによって、どの Go プラットフォームでも圧縮ファイルを作成することは可能ですが、ランタイム・コンテナー・イメージに組み込まれているプリコンパイル・フィーチャーを使用してください。 [複数のソース・ファイル](#prep_go_multi)や[ベンダー・ライブラリー](#prep_go_vendor)をパッケージすることができます。
{: tip}


### Go コードを構造化する
{: #prep_go_struct}

- エントリー・ポイントのパッケージの名前は `main` と想定されます。 コードのパッケージが `main` でない場合は、その名前をメモし、アクションの作成時に指定してください。
- パッケージは public でなければなりません。

**例**
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

### 複数の Go ソース・ファイルをパッケージ化する
{: #prep_go_multi}

1. 最上位の `src` ディレクトリーを作成します。 main パッケージに属するソース・ファイルを `src` のルートか `main` ディレクトリーの中のいずれかに配置し、他のパッケージ用のサブディレクトリーを作成します。 例えば、`hello` パッケージは `src/hello` ディレクトリーになります。
  ```
  go-action-hello/
  └── src
      ├── hello
      │   └── hello.go
      └── main
          └── main.go
  ```
  {: screen}

2. サブパッケージをインポートします。 hello サブパッケージをインポートする `main/main.go` の例。

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

  `hello/hello.go` の例。

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

3. コードをコンパイルします。 `src` ディレクトリーの .zip アーカイブを作成します。 最上位のプロジェクト・ディレクトリー `go-action-project/` は組み込まないでください。

  ```bash
  cd src
zip -r ../hello-src.zip *
cd ..
  ```
  {: pre}

  `GOPATH` を `src` ディレクトリーの親に設定することによって、ローカル環境でコンパイルできるようになります。 VS Code を使用する場合は、`go.inferGopath` 設定を `true` に変更する必要があります。{: note}

4. Go 実行可能ファイルをコンパイルして、.zip アーカイブのルートに `exec` としてパッケージします。以下のコマンドを実行して、`hello-bin.zip` アーカイブを作成してください。ワークステーションに Docker CLI をインストールし、`PATH` に `docker` を設定しておく必要があります。

  ```bash
  docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
  ```
  {: pre}

  この例では、`-compile main` が main 関数です。 別の関数をメインとして使用する場合は、`-compile` の値を変更してください。 コンパイル時に main 関数が選択されます。 プリコンパイル時には、`ibmcloud fn action [update | create]` で `--main` は無視されます。

  コンテナーが `stdin` でソース .zip の内容を取り込み、その内容をコンパイルし、ルートに実行可能ファイル `exec` を組み込んだ新しい .zip アーカイブを作成します。 .zip アーカイブの内容が `stdout` にストリーム送信されると `hello-bin.zip` アーカイブにリダイレクトされ、Go アクションとしてデプロイされます。




### Go コードをベンダー・ライブラリーと一緒にパッケージ化する
{: #prep_go_vendor}

依存関係を組み込むには、Go ファイルをコンパイルするときにソースの `zip` アーカイブの中の `vendor` ディレクトリーに依存関係を取り込みます。 `vendor` ディレクトリーは、最上位では正しく動作しません。 `vendor` ディレクトリーは、`src/` の中のパッケージ・ディレクトリーに配置する必要があります。
{: shortdesc}

`hello.go` アプリのログ・パッケージ `logrus` の例。

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
この例では、`vendor` ディレクトリーが `src/hello/vendor` に配置されています。 `hello` パッケージで使用するサード・パーティー・ライブラリーを追加できます。 

依存関係の取り込みや管理のために、[<code>dep</code>](https://golang.github.io/dep/docs/installation.html){: external} など、複数のツールを使用できます。

`dep` を使用するには、ライブラリーのバージョンと場所を指定した `src/main/Gopkg.toml` ファイルを作成します。

```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

`vendor` ディレクトリーに項目を追加し、`dep ensure` を実行します。




## Swift アプリを準備する
{: #prep_swift}

アクションを実行する前に、Swift ファイルをコンパイルする必要があります。 この遅延は、コールド・スタートの遅延と呼ばれます。 コールド・スタートの遅延を回避するため、Swift ファイルをコンパイルして .zip ファイルで {{site.data.keyword.openwhisk_short}} にアップロードできます。Docker ランタイムには、ユーザーが Swift 4.2 アクションをコンパイルしてパッケージするのに役立つコンパイラーが組み込まれています。 その後のアクションの呼び出しは、アクションが含まれるコンテナーがパージされるまで、大幅に高速になります。

Swift アクションは Linux 環境で実行されます。Linux 上の Swift はまだ開発中なので、{{site.data.keyword.openwhisk_short}} では最新の使用可能なリリースが使用されています。 これらのリリースは安定していないかもしれません。 {{site.data.keyword.openwhisk_short}} で使用されている Swift のバージョンは、MacOS 上の Xcode の安定リリースに含まれている Swift のバージョンとは整合性がない場合があります。
{: important}


### Swift コードを構造化する
{: #prep_swift_struc}

エントリー・ポイントの関数の名前は `main` と想定されます。 コードの関数が `main` でない場合は、その名前をメモし、アクションの作成時に指定してください。

main 関数シグニチャーに加えて、Swift 4 にはさらに 2 つのシグニチャーが用意されており、これらは [<code>Codable</code>](https://developer.apple.com/documentation/swift/codable){: external} タイプを利用します。[JSON などの外部表現との互換性を保つために使用できるデータ型 encodable と decodable](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types){: external} の詳細についても確認できます。

**例**
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


#### Swift のエラー処理
{: #prep_swift_error}

`Codable` の完了ハンドラーを使用して、アクション内での失敗を示すエラーを渡すことができます。 [Swift でのエラー処理](https://docs.swift.org/swift-book/LanguageGuide/ErrorHandling.html){: external}は他言語の例外処理に似ており、`try`、`catch`、および `throw` の各キーワードを使用します。
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


### Swift 4.2 ファイルのパッケージ化
{: #prep_swift42_single}

外部ライブラリーに依存していない 1 つのソース・ファイルをコンパイルします。`-compile` フラグを使用して main メソッドの名前を指定します。

**始める前に**
- [Docker をインストールします](https://hub.docker.com/search/?offering=community&type=edition){: external}。
- [Swift ランタイムに組み込まれているパッケージを参照](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions)し、アプリの依存関係がランタイムに既に組み込まれているかどうかを調べます。 依存関係が組み込まれていない場合は、アプリと一緒にパッケージ化する必要があります。

アプリをパッケージします。

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

Docker コンテナーが `stdin` からファイルの内容を読み取り、コンパイルした swift 実行可能ファイルを組み込んだ .zip アーカイブを `stdout` に書き込みます。



### Swift 4.2 のマルチファイルのプロジェクトと依存関係をパッケージ化する
{: #prep_swift42_multi}

**始める前に**
- [Docker をインストールします](https://hub.docker.com/search/?offering=community&type=edition){: external}。
- [Swift ランタイムに組み込まれているパッケージを参照](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions)し、アプリの依存関係がランタイムに既に組み込まれているかどうかを調べます。 依存関係が組み込まれていない場合は、アプリと一緒にパッケージ化する必要があります。

アプリをパッケージします。

1. 複数のファイルをコンパイルして外部の依存項目を組み込むには、以下のディレクトリー構造を作成します。

  ```
  .
  ├── Package.swift
  └── Sources
      └── main.swift
  ```
  {: codeblock}

  ディレクトリーの `Sources/` に `main.swift` という名前のファイルが含まれています。

  `Package.swift` の先頭に、Swift ツールのバージョン `4.2` を示すコメントを入力する必要があります。

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

2. ディレクトリーの内容を含む .zip アーカイブを作成します。

  ```bash
  zip ../action-src.zip -r *
  ```
  {: codeblock}

3. `stdin` を使用して .zip アーカイブを Docker コンテナーに渡します。 `stdout` が、コンパイルした実行可能ファイルの新しい .zip アーカイブです。 Docker コンテナーが `stdin` から .zip アーカイブの内容を読み取り、コンパイルした Swift 実行可能ファイルの新しい .zip アーカイブを `stdout` に書き込みます。

  ```
  docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
  ```
  {: codeblock}

  Linux ベースのシステムでは、`zip` と `docker run` のステップを 1 つのコマンドで結合できます。

  ```
  zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
  ```
  {: codeblock}





## Python アプリのパッケージ化
{: #prep_python}


### Python コードを構造化する
{: #prep_python_struct}

- Python アプリは、ディクショナリーを取り込み、ディクショナリーを生成する必要があります。
- エントリー・ポイントのメソッドの名前は `main` と想定されます。 コードの関数が `main` でない場合は、その名前をメモし、アクションの作成時に指定してください。
{: shortdesc}

**例**
```python
def main(args):
	name = args.get("name", "stranger")
	greeting = "Hello " + name + "!"
	print(greeting)
    return {"greeting": greeting}
```

### Python コードをパッケージ化する
{: #prep_python_pkg}

Python コードと依存関係モジュールを .zip ファイル形式でパッケージ化します。 次の例では、エントリー・ポイントを含むソース・ファイルが `__main__.py` で、ヘルパー・モジュールが `helper.py` というファイル内にあります。

始めに、[Python ランタイムに組み込まれているパッケージを参照](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments)し、アプリの依存関係がランタイムに既に組み込まれているかどうかを調べます。 依存関係が組み込まれていない場合は、アプリと一緒にパッケージ化する必要があります。

アプリをパッケージするには、次のコマンドを実行します。

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}


### Python コードを仮想環境と一緒に .zip ファイル形式でパッケージ化する
{: #prep_python_virtenv}

Python の依存関係は、仮想環境 `virtualenv` を使用してパッケージ化できます。 仮想環境を使用することで、[`pip`](https://packaging.python.org/tutorials/installing-packages/){: external} を使用してインストールできるパッケージをさらにリンクできます。

始めに、[Python ランタイムに組み込まれているパッケージを参照](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments)し、アプリの依存関係がランタイムに既に組み込まれているかどうかを調べます。 依存関係が組み込まれていない場合は、アプリと一緒にパッケージ化する必要があります。

以下のステップを実行して、アプリをパッケージします。

1. インストールする `pip` モジュールおよびバージョンを含む [requirements.txt](https://pip.pypa.io/en/latest/user_guide/#requirements-files){: external} ファイルを作成します。

  `virtualenv` を最小サイズに抑えるために、`requirements.txt` には、選択したランタイム環境に含まれていないモジュールのみを追加してください。 Python ランタイムに含まれているパッケージについて詳しくは、Python [ランタイムのリファレンス](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments)を参照してください。
  {: tip}

2. ランタイム用に次のいずれかのイメージをプルします。 ランタイム・コンテナーとの適合性を確保するために、仮想環境内のパッケージ化されたインストール環境では、指定したランタイムに対応するイメージを使用する必要があります。
    * `python:3.7` の場合は、Docker イメージの `ibmfunctions/action-python-v3.7` を使用します。
    * `python:3.6` の場合は、Docker イメージの `ibmfunctions/action-python-v3.6` を使用します。
    * `python:2` の場合は、Docker イメージの `openwhisk/python2action` を使用します。

   **例**
   ```
   docker pull ibmfunctions/action-python-v3.7
   ```
   {: pre}

2. 依存関係をインストールし、仮想環境を作成します。 仮想環境ディレクトリーの名前は `virtualenv` でなければなりません。

   ```
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. `virtualenv` ディレクトリーと追加の Python ファイルをパッケージ化します。 エントリー・ポイントを含むソース・ファイルは `__main__.py` という名前でなければなりません。

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}


## Ruby アプリを準備する
{: #prep_ruby}

アクションを作成する前に、Ruby コードの準備を整えます。

### Ruby コードを構造化する
{: #prep_ruby_struct}

* Ruby アクションは、常に Hash (辞書のようなコレクション) を取り込んで Hash を返します。
* エントリー・ポイントの関数の名前は `main` と想定されます。 コードの関数が `main` でない場合は、その名前をメモし、アクションの作成時に指定してください。


**例**

```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
```
{: codeblock}

### Ruby コードをパッケージ化する
{: #prep_ruby_pkg}

Ruby アプリと依存関係パッケージを .zip ファイル形式でパッケージ化できます。 例えば、`helper.rb` という 2 番目のファイルを組み込んでアクションをパッケージすることもできます。

ソース・ファイルを含めたアーカイブを作成します。 エントリー・ポイントを含むソース・ファイルには、`main.rb` という名前を付ける必要があります。

```bash
zip -r helloRuby.zip main.rb helper.rb
```
{: pre}

バンドルに含まれているデフォルトの gem に加えて、`mechanize` と `jwt` という gem もあります。 zip を使用したアクションですべての依存項目をパッケージするだけであれば、任意の gem を使用できます。



## PHP アプリを準備する
{: #prep_php}

アクションを作成する前に、PHP コードの準備を整えます。

### PHP コードを構造化する
{: #prep_php_struct}

- PHP アクションは常に連想配列を取り込み、連想配列を返します。
- エントリー・ポイントの関数の名前は `main` と想定されます。 コードの関数が `main` でない場合は、その名前をメモし、アクションの作成時に指定してください。

**例**
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

### PHP コードをパッケージ化する
{: #prep_php_pkg}

PHP ファイルや依存関係パッケージを .zip ファイル形式でパッケージ化できます。

始めに、[PHP ランタイムに組み込まれているパッケージを参照](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php)し、アプリの依存関係がランタイムに既に組み込まれているかどうかを調べます。 依存関係が組み込まれていない場合は、アプリと一緒にパッケージ化する必要があります。

アプリをパッケージするには、次のコマンドを実行します。

```bash
zip -r ARCHIVE_NAME.zip FILE_1.php FILE_2.php
```
{: pre}

**例**
```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

## Java アプリを準備する
{: #prep_java}

アクションを作成する前に、Java コードの準備を整えます。

### Java コードを構造化する
{: #prep_java_struct}

Java のアクションというのは、`main` という名前のメソッドを持つ Java プログラムのことです。 `main` は、以下のシグニチャーを持っていなければなりません。

**例**
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

* `--main` を使用して、メイン・クラスの名前を指定する必要があります。 適格なメイン・クラスは、静的 `main` メソッドを実装するクラスです。 クラスがデフォルト・パッケージ内にない場合は、完全修飾 Java クラス名を使用してください (例: `--main com.example.MyMain`)。
* アクションの完全修飾メソッド名 (例えば、`--main com.example.MyMain#methodName`) を指定することによって、Java アクションのメソッド名をカスタマイズできます。

### Java コードをパッケージ化する
{: #prep_java_pkg}


**始める前に**
[JDK 8](http://openjdk.java.net/install/){: external} がローカルにインストールされている必要があります。この例では、[`google-gson-2.8.5.jar`](http://central.maven.org/maven2/com/google/code/gson/gson/2.8.5/){: external} を使用します。

JDK 8 以外の JDK バージョンで作業している場合は、`javac` コマンドでコードをコンパイルするときに `--release 8` を指定する必要があります。
{: note}

Java アクションを作成するには、以下のステップを実行します。

1. 以下のコードを `Hello.java` という名前のファイルに保存します。

  ```java
  import com.google.gson.JsonObject;
    public class Hello {
      public static JsonObject main(JsonObject args) {
          String name = "stranger";
          if (args.has("name"))
              name = args.getAsJsonPrimitive("name").getAsString();
          JsonObject response = new JsonObject();
          response.addProperty("greeting", "Hello, " + name + "!");
        return response;
    }
  }
  ```
  {: codeblock}

2. [`gson-2.8.5.jar`](http://central.maven.org/maven2/com/google/code/gson/gson/2.8.5/) をダウンロードします。

3. `gson-2.8.5.jar` を `ClASSPATH` に追加します。この例では、`gson-2.8.5.jar` を使用します。これは、`Desktop` ディレクトリーの `test` フォルダーに保存されています。
  
  ```
  export CLASSPATH=$CLASSPATH:/Users/Desktop/test/gson-2.8.5.jar
  ```
  {: pre}

4. JDK の `bin` フォルダーを `CLASSPATH`に追加します。この例では、`openjdk-8` を使用します。
  
  ```
  export CLASSPATH=$CLASSPATH:/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/bin
  ```
  {: pre}

5. JDK の `bin` フォルダーと `gson-2.8.5.jar` が `CLASSPATH` にあることを確認します。
  ```
  echo $CLASSPATH
  ```
  {: pre}

  **出力例**
  ```
  /Desktop/test/gson-2.8.5.jar:/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/bin
  ```
  {: screen}

6. `Hello.java` ファイルが保管されているフォルダーにナビゲートします。この例では、`Desktop/test` フォルダーに `Hello.java` ファイルが保存されます。
  ```
  cd Desktop/test
  ```
  {: pre}

7. `Hello.java` ファイルをコンパイルしてクラス・ファイルにします。
  ```
  javac Hello.java
  ```
  {: pre}

8. このクラス・ファイルを圧縮して `hello.jar` という名前の .jar ファイルにします。

  ```
  jar cvf hello.jar Hello.class
  ```
  {: pre}

**次のステップ**
`hello.jar` を使用してアクションを作成できます。作成したクラス・ファイルはデフォルト名 `main` を使用しないため、アクションの作成時に `--main` フラグを `Hello` に設定する必要があります。`--main` フラグは、ご使用の Java `クラス`と一致する必要があります。詳しくは、[アクションの作成](/docs/openwhisk?topic=cloud-functions-actions)を参照してください。
 
Java コードを更新する場合は、これらのステップを繰り返して、コードを新しい `.jar` ファイルに再コンパイルする必要があります。
{: note}

### Gradle を使用した Java コードをパッケージ化する
{: #prep_java_gradle}

コマンド・ラインからコンパイルする代わりに、[Gradle](https://gradle.org){: external} などのビルド・ツールを使用して、Maven Central などのリポジトリーからライブラリーを取り出すことができます。Gradle を使用して取り出し、コードとすべての依存関係を含む最終的な .jar アーカイブをビルドできます。

Gradle によって `com.google.zxing` ライブラリーを利用した Java アクションをビルドする例を以下に示します。このライブラリーは、QR コード・イメージを生成する機能を提供します。

1. `build.gradle` という名前のファイルを作成して、依存関係を指定します。

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

2. `gradle jar` コマンドを実行します。ディレクトリー `build/libs/` に .jar アーカイブが生成されます。

  詳しくは、Gradle の資料 [Declaring Dependencies](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies){: external} を参照してください。


## .NET Core アプリを準備する
{: #prep_dotnet}

アクションを作成する前に、.NET Core コードの準備を整えます。

### .NET Core コードを構造化する
{: #prep_dotnet_struct}

.NET Core アクションは、`Main` という名前のメソッドが含まれている .NET Core クラス・ライブラリーです。 コードのメソッドが `Main` でない場合は、その名前をメモし、アクションの作成時に `--main {Assembly}::{Class Full Name}::{Method}` という形式で指定してください

**例**
```
Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main
```

### .NET Core コードをパッケージ化する
{: #prep_dotnet_pkg}

**始める前に**
.NET Core プロジェクトをコンパイル、テスト、およびアーカイブするには、以下の準備が必要です。
- [.NET Core SDK](https://dotnet.microsoft.com/download){: external} をローカルにインストールします。
- `DOTNET_HOME` 環境変数に `dotnet` 実行可能ファイルの配置場所を設定します。



コードをパッケージするには、以下のコマンドを実行します。

  1. `Apache.OpenWhisk.Example.Dotnet` という C# プロジェクトを作成します。

      ```bash
      dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
      ```
      {: pre}

  2. `Apache.OpenWhisk.Example.Dotnet` ディレクトリーに移動します。

      ```bash
      cd Apache.OpenWhisk.Example.Dotnet
      ```
      {: pre}

  3. [<ph class="ignoreSpelling">Newtonsoft.Json NuGet</ph> パッケージ](https://www.nuget.org/packages/Newtonsoft.Json/){: external}をインストールします。

      ```bash
      dotnet add package Newtonsoft.Json -v 12.0.1
      ```
      {: pre}

  4. 以下のコードを `Hello.cs` という名前のファイルに保存します。

      ```csharp
      using System;
    using Newtonsoft.Json.Linq;

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

  5. `Hello.cs` と他のファイルをコンパイルして、`out` ディレクトリーに出力します。

      ```bash
      dotnet publish -c Release -o out
      ```
      {: pre}

  6. `out` ディレクトリーにナビゲートします。

      ```bash
      cd out
      ```
      {: pre}

  7. パブリッシュしたファイルを圧縮します。

      ```bash
      zip -r -0 ../helloDotNet.zip *
      ```
      {: pre}



