---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# アクションの作成および呼び出し
{: #openwhisk_actions}

アクションとは、{{site.data.keyword.openwhisk}} プラットフォームで実行されるステートレスなコード・スニペットです。例えば、アクションを使用して、画像内の顔の検出、データベース変更に対する応答、一連の API 呼び出しの集約、ツイートの投稿を行うことができます。アクションは、JavaScript、Swift、Python、PHP 関数、Java メソッド、任意のバイナリー互換実行可能ファイル (Go プログラムや Docker コンテナーとしてパッケージされたカスタム実行可能ファイルを含む) として作成できます。
{:shortdesc}

アクションは明示的に呼び出すか、イベントに応じて実行することができます。いずれの場合も、アクションを実行するたびに、固有のアクティベーション ID で識別されるアクティベーション・レコードが生成されます。アクションに対する入力とアクションの結果は、キーと値のペアで構成される辞書です。ここで、キーはストリングであり、値は有効な JSON 値です。アクションは、他のアクションや定義されたアクション・シーケンスの呼び出しで構成することもできます。

以下のお好みの開発環境におけるアクションの作成、呼び出し、デバッグ方法について説明します。
* [JavaScript](#creating-and-invoking-javascript-actions)
* [Swift](#creating-swift-actions)
* [Python](#creating-python-actions)
* [Java](#creating-java-actions)
* [PHP](#creating-php-actions)
* [Docker](#creating-docker-actions)
* [Go](#creating-go-actions)
* [任意の実行可能ファイル](#creating-actions-arbitrary)

さらに、以下について説明します。
* [アクションの出力の監視](#watching-action-output)
* [大規模アプリケーション・サポート](#large-app-support)
* [アクションのリスト表示](#listing-actions)
* [アクションの削除](#deleting-actions)
* [アクション本体の中のアクション・メタデータへのアクセス](#accessing-action-metadata-within-the-action-body)


## JavaScript アクションの作成と呼び出し
{: #creating-and-invoking-javascript-actions}

以下のセクションでは、JavaScript でのアクションの操作について説明します。まず、単純なアクションを作成して呼び出します。その後、アクションにパラメーターを追加し、パラメーターを指定してそのアクションを呼び出します。次に、デフォルト・パラメーターを設定して呼び出します。その後、非同期アクションを作成し、最後にアクション・シーケンスを処理します。


### 単純な JavaScript アクションの作成と呼び出し
{: #openwhisk_single_action_js}

以下のステップと例を見て、最初の JavaScript アクションを作成してください。

1. 以下の内容を持つ JavaScript ファイルを作成します。この例では、ファイル名は「hello.js」です。

  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  この JavaScript ファイルにさらに関数を含めることもできます。ただし、規則により、アクションのエントリー・ポイントを提供するために `main` という名前の関数が存在している必要があります。

2. 以下の JavaScript 関数からアクションを作成します。この例では、アクションは「hello」という名前です。

  ```
  wsk action create hello hello.js
  ```
  {: pre}

  ```
  ok: created action hello
  ```
  CLI は、ソース・ファイルの拡張子を使用して、アクションのタイプを自動的に推定します。`.js` ソース・ファイルの場合、アクションは、Node.js 6 ランタイムを使用して実行されます。また、パラメーター `--kind nodejs:8` を明示的に指定することで、Node.js 8 で実行されるアクションを作成することも可能です。詳しくは、Node.js 6 と 8 を比較した[リファレンス](./openwhisk_reference.html#openwhisk_ref_javascript_environments)を参照してください。
  
3. 以下のように、作成したアクションをリストします。

  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  hello       private
  ```

  作成した `hello` アクションが表示されているのを確認できます。

4. アクションを作成した後、invoke コマンドを使用して、そのアクションを OpenWhisk においてクラウド内で実行できます。コマンドにフラグを指定することによって、*ブロッキング* 呼び出し (つまり、要求/応答スタイル) または*非ブロッキング* 呼び出しのいずれかでアクションを呼び出すことができます。ブロッキング呼び出し要求は、アクティベーション結果が使用可能になるのを_待機_ します。待機時間は、60 秒と、アクションの[制限時間値](./openwhisk_reference.html#openwhisk_syslimits)のいずれか小さいほうです。アクティベーションの結果が待機時間内に使用可能になった場合、その結果が返されます。使用可能にならない場合、非ブロッキング要求の場合と同様に、アクティベーション処理はシステムで続行され、結果を後でチェックできるようにアクティベーション ID が返されます (アクティベーションのモニターに関するヒントについては、[ここ](#watching-action-output)を参照してください)。

  次の例では、ブロッキングを示す `--blocking` パラメーターが使用されています。

  ```
  wsk action invoke --blocking hello
  ```
  {: pre}

  ```
  ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b
  ```

  ```json
  {
      "result": {
          "payload": "Hello world"
      },
      "status": "success",
      "success": true
  }
  ```

  コマンドの出力は、次の 2 つの重要な情報です。
  * アクティベーション ID (`44794bd6aab74415b4e42a308d880e5b`)
  * 予期される待機時間内に使用可能になった場合は呼び出し結果

  この例の結果は、JavaScript 関数によって返されたストリング `Hello world` です。アクティベーション ID は、後でログまたは呼び出し結果を取り出すときに使用できます。  

5. アクションの結果をすぐに必要としない場合は、`--blocking` フラグを省略して非ブロッキング呼び出しを行うことができます。結果は後でアクティベーション ID を使用して取得できます。以下の例を参照してください。

  ```
  wsk action invoke hello
  ```
  {: pre}

  ```
  ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
  ```

  ```
  wsk activation result 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: pre}

  ```json
  {
      "payload": "Hello world"
  }
  ```

6. アクティベーション ID を記録しておくのを忘れた場合、最新のものから古いものへと順に並べられたアクティベーションのリストを取得できます。アクティベーションのリストを取得するには、次のコマンドを実行します。

  ```
  wsk activation list
  ```
  {: pre}

  ```
  activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
  ```

### アクションへのパラメーターの受け渡し
{: #openwhisk_pass_params}

アクションが呼び出されるときにパラメーターを渡すことができます。

1. アクションでパラメーターを使用します。例えば、「hello.js」ファイルを更新して、次のような内容にします。

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  入力パラメーターが 1 つの JSON オブジェクト・パラメーターとして `main` 関数に渡されます。この例で、`name` パラメーターと `place` パラメーターが `params` オブジェクトからどのように取り出されるのかに注意してください。

2. `hello` アクションを更新して呼び出します。その際、`name` および `place` パラメーター値を渡します。以下の例を参照してください。

  ```
  wsk action update hello hello.js
  ```
  {: pre}

  非サービス資格情報パラメーターを変更する必要がある場合は、新しいパラメーターを指定して `action update` コマンドを実行すると、現在存在しているが `action update` コマンドで指定されていないパラメーターがすべて削除されるので注意してください。例えば、`__bx_creds` 以外に 2 つのパラメーターが存在し、そのキーの名前が key1 および key2 であるものとします。`__bx_creds` パラメーターを省き、`-p key1 new-value -p key2 new-value` を指定して `action update` コマンドを実行した場合、`action update` が正常に完了した後に、`__bx_creds` パラメーターは存在しなくなっています。その場合は、サービス資格情報を再バインドする必要があります。これは、回避策がない既知の制限です。{: tip}  

3.  パラメーターは、明示的にコマンド・ラインで指定するか、必要なパラメーターを含むファイルを提供することによって指定できます。

  コマンド・ラインを使用して直接パラメーターを渡すには、以下に示すように、`--param` フラグにキー/値のペアを指定します。
  ```
  wsk action invoke --result hello --param name Bernie --param place Vermont
  ```
  {: pre}

  パラメーターの内容を含むファイルを使用するには、JSON フォーマットでそれらのパラメーターを含むファイルを作成します。次に、以下に示すように、ファイル名を `param-file` フラグに渡します。

  `parameters.json` という名前の以下のサンプル・パラメーター・ファイルを参照してください。
  ```json
  {
      "name": "Bernie",
      "place": "Vermont"
  }
  ```

  ```
  wsk action invoke --result hello --param-file parameters.json
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

  `--result` オプションの使用に注意してください。これは、CLI がアクティベーションの完了を待機してから、結果のみを表示しているブロッキング呼び出しを示しています。便宜上、自動的に推定される `--blocking` を指定せずにこのオプションを使用することができます。

  また、コマンド・ラインで指定されたパラメーター値が有効な JSON である場合、それらのパラメーター値は構文解析され、1 つの構造化オブジェクトとしてアクションに送信されます。例えば、hello アクションを次のように更新します。

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  これで、アクションは単一の `person` パラメーターに `name` フィールドと `place` フィールドがあることを予期するようになります。次に、以下の例のように、有効な JSON である単一の `person` パラメーターを指定してアクションを呼び出します。

  ```
  wsk action invoke --result hello -p person '{"name": "Bernie", "place": "Vermont"}'
  ```
  {: pre}

  CLI が自動的に `person` パラメーター値を構文解析して、アクションが予期するようになった構造化オブジェクトにするため、結果は同じです。
  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

### デフォルト・パラメーターの設定
{: #openwhisk_binding_actions}

複数の名前付きパラメーターを指定してアクションを呼び出すことができます。前の例では `hello` アクションは、個人を表す *name* パラメーターと出身地を表す *place* の 2 つのパラメーターを予期していました。

アクションに毎回すべてのパラメーターを渡すのではなく、特定のパラメーターをバインドすることができます。次の例は、*place* パラメーターをバインドして、アクションがデフォルトで場所「Vermont」を設定するようにします。

1. `--param` オプションを使用してパラメーター値をバインドするか、パラメーターを含むファイルを `--param-file` に渡してアクションを更新します。

  デフォルト・パラメーターを明示的にコマンド・ラインに指定するには、以下に示すように、`param` フラグにキー/値のペアを指定してください。

  ```
  wsk action update hello --param place Vermont
  ```
  {: pre}

  ファイルからパラメーターを渡すには、必要な内容を JSON フォーマットで含むファイルを作成する必要があります。次に、以下に示すように、ファイル名を `-param-file` フラグに渡します。

  `parameters.json` という名前の以下のサンプル・パラメーター・ファイルを参照してください。
  ```json
  {
      "place": "Vermont"
  }
  ```
  {: codeblock}

  ```
  wsk action update hello --param-file parameters.json
  ```
  {: pre}

2. 今回は `name` パラメーターのみを渡してアクションを呼び出します。

  ```
  wsk action invoke --result hello --param name Bernie
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

  アクションを呼び出すときに place パラメーターを指定する必要がなかったことに注意してください。パラメーターをバインドしても、呼び出し時にそのパラメーターの値を指定すれば上書きできます。

3. `name` 値と `place` 値の両方を渡してアクションを呼び出します。後者は、アクションにバインドされた値を上書きします。

  `--param` フラグを使用した場合:

  ```
  wsk action invoke --result hello --param name Bernie --param place "Washington, DC"
  ```
  {: pre}

  `--param-file` フラグを使用した場合:

  parameters.json ファイルを使用した場合:
  ```json
  {
    "name": "Bernie",
    "place": "Vermont"
  }
  ```
  {: codeblock}
  ```
  wsk action invoke --result hello --param-file parameters.json
  ```
  {: pre}

  ```json
  {  
      "payload": "Hello, Bernie from Washington, DC"
  }
  ```

### アクション URL の取得

アクションは、HTTPS 要求を介して REST インターフェースを使用して呼び出すことができます。アクション URL を取得するには、以下のコマンドを実行します。

```
wsk action get actionName --url
```
{: pre}

```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```

HTTPS 要求を介してアクションを呼び出すときに、認証を行う必要があります。REST インターフェースを使用したアクションの呼び出しについて詳しくは、
『[OpenWhisk REST API の使用](./openwhisk_rest_api.html#openwhisk_rest_api)』を参照してください。
{: tip}

### アクション・コードの保存

既存のアクションに関連付けられているコードがフェッチされ、ローカルに保存されます。シーケンスおよび docker アクションを除き、すべてのアクションについて保存が実行されます。アクション・コードをファイルに保存する際に、コードは、現行作業ディレクトリーに保存され、保存されたファイルのパスが表示されます。

1. 既存のアクション名に対応するファイル名にアクション・コードを保存します。アクションの種類に対応するファイル拡張子が使用されるか、zip ファイルであるアクション・コードにはタイプ `.zip` の拡張子が使用されます。
  ```
  wsk action get actionName --save
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```

2. 保存されるコードのファイル名と拡張子が CLI で自動判別されるようにする代わりに、`--save-as` フラグを使用して、カスタムのファイル名と拡張子を指定できます。
  ```
  wsk action get actionName --save-as codeFile.js
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```

### 非同期アクションの作成
{: #openwhisk_asynchrony_js}

非同期に実行される JavaScript 関数により、アクションで Promise を返すことで、`main` 関数が戻った後に、アクティベーション結果を返すことができます。

1. 以下の内容を `asyncAction.js` という名前のファイルに保存します。

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

  `main` 関数は Promise を返し、アクティベーションがまだ完了していないが、今後完了すると見込まれていることを示しています。

  この例における `setTimeout()` JavaScript 関数は、コールバック関数を呼び出す前に 2 秒間待機します。これは、非同期コードを表し、Promise のコールバック関数の内部に進みます。

  Promise のコールバックは、resolve と reject という 2 つの引数を使用します。これらはどちらも関数です。`resolve()` の呼び出しは、Promise を完了し、アクティベーションが正常に実行されたことを示します。

  `reject()` の呼び出しでは、Promise を拒否し、アクティベーションの実行で異常が発生したことを示すことができます。

2. 以下のコマンドを実行して、アクションを作成して呼び出します。

  ```
  wsk action create asyncAction asyncAction.js
  ```
  {: pre}

  ```
  wsk action invoke --result asyncAction
  ```
  {: pre}

  ```json
  {
      "done": true
  }
  ```

  実行したのは非同期アクションのブロッキング呼び出しであることに注意してください。

3. アクティベーション・ログを取り出して、アクティベーションの完了にかかった時間を確認します。

  ```
  wsk activation list --limit 1 asyncAction
  ```
  {: pre}
  
  ```
  activations
  b066ca51e68c4d3382df2d8033265db0             asyncAction
  ```

  ```
  wsk activation get b066ca51e68c4d3382df2d8033265db0
  ```
  {: pre}
 
  ```json
  {
      "start": 1455881628103,
      "end":   1455881648126,
      ...
  }
  ```

  アクティベーション・レコード中の `start` と `end` のタイム・スタンプを比較することで、このアクティベーションの完了に 2 秒と少しかかったことが分かります。

### 外部 API を呼び出すためのアクションの使用
{: #openwhisk_apicall_action}

ここまでの例では、JavaScript 関数はすべて自己完結型でした。外部 API を呼び出すアクションを作成することもできます。

次の例は、Yahoo Weather サービスを呼び出して、特定の場所での現在の状態を取得します。

1. 以下の内容を `weather.js` という名前のファイルに保存します。

  ```javascript
  var request = require('request');

  function main(params) {
      var location = params.location || 'Vermont';
      var url = 'https://query.yahooapis.com/v1/public/yql?q=select item.condition from weather.forecast where woeid in (select woeid from geo.places(1) where text="' + location + '")&format=json';

      return new Promise(function(resolve, reject) {
          request.get(url, function(error, response, body) {
              if (error) {
                  reject(error);
              }
              else {
                  var condition = JSON.parse(body).query.results.channel.item.condition;
                  var text = condition.text;
                  var temperature = condition.temp;
                  var output = 'It is ' + temperature + ' degrees in ' + location + ' and ' + text;
                  resolve({msg: output});
              }
          });
      });
  }
  ```
  {: codeblock}

 この例では、アクションは JavaScript `request` ライブラリーを使用して Yahoo Weather API への HTTP 要求を行い、JSON 結果からフィールドを抽出します。[このリファレンス](./openwhisk_reference.html#openwhisk_ref_javascript_environments)で、アクションで使用できる Node.js パッケージの詳細が示されています。

  この例では、非同期アクションの必要性も示されます。アクションは Promise を返すことで、関数が戻ったときにこのアクションの結果はまだ使用可能になっていないことを示します。代わりに、結果は、HTTP 呼び出しが完了した後に `request` コールバックで使用可能になり、引数として `resolve()` 関数に渡されます。

2. 以下のコマンドを実行して、アクションを作成して呼び出します。

  ```
  wsk action create weather weather.js
  ```
  {: pre}

  ```
  wsk action invoke --result weather --param location "Brooklyn, NY"
  ```
  {: pre}

  ```json
  {
      "msg": "It is 28 degrees in Brooklyn, NY and Cloudy"
  }
  ```

### Node.js モジュールとしてのアクションのパッケージ化
{: #openwhisk_js_packaged_action}

単一の JavaScript ソース・ファイル内にすべてのアクション・コードを作成する代わりに、`npm` パッケージとしてアクションを作成できます。例として、以下のファイルが含まれたディレクトリーについて考えます。

まず、`package.json`:

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

次に、`index.js`:

```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}

exports.main = myAction;
```
{: codeblock}

アクションは、`exports.main` を介して公開されます。アクション・ハンドラー自体の名前は、オブジェクトを受け入れ、オブジェクトを返す通常のシグニチャー (オブジェクトの `Promise`) に準拠している限り、任意のものにすることができます。Node.js 規則により、このファイルの名前は `index.js` にするか、または、package.json 内の `main` プロパティーとして任意のファイル名を指定する必要があります。

このパッケージから OpenWhisk アクションを作成するには、以下のようにします。

1. まず、すべての依存関係をローカルでインストールします。

  ```
  npm install
  ```
  {: pre}

2. 以下のように、すべてのファイル (すべての依存関係を含む) が入った `.zip` アーカイブを作成します。

  ```
  zip -r action.zip *
  ```
  {: pre}

  Windows Explorer の操作を使用して zip ファイルを作成すると、誤った構造になります。OpenWhisk zip アクションでは、`package.json` が zip のルートに存在する必要がありますが、Windows Explorer はこれをネストされたフォルダー内に配置します。最も安全な方法としては、コマンド・ラインで `zip` コマンドを使用します。
  {: tip}

3. 以下のように、アクションを作成します。

  ```
  wsk action create packageAction --kind nodejs:6 action.zip
  ```
  {: pre}

  CLI ツールを使用して `.zip` アーカイブからアクションを作成する際に、`nodejs:6` または `nodejs:8` を使用して、`--kind` フラグの値を明示的に指定する必要があります。

4. 以下のように、アクションの呼び出しは、他と同様に行うことができます。

  ```
  wsk action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
  ```
  {: pre}
  
  ```json
  {
      "padded": [
          ".......................and now",
          "......for something completely",
          ".....................different"
      ]
  }
  ```

最後に、ほとんどの `npm` パッケージは `npm install` で JavaScript ソースをインストールしますが、一部のパッケージはバイナリー成果物をインストールおよびコンパイルすることにも注意してください。現在、アーカイブ・ファイルのアップロードでは、バイナリー依存関係はサポートされず、JavaScript 依存関係のみがサポートされます。アーカイブにバイナリーの依存関係が含まれている場合、アクションの呼び出しが失敗することがあります。

### 単一のバンドルとしてのアクションのパッケージ化
{: #openwhisk_js_webpack_action}

依存関係が含まれた単一の `.js` ファイルに最小限のコードのみを含めると簡便です。このアプローチにより、アクションを zip としてパッケージ化すると、不要なファイルが含まれているために、大きくなりすぎる場合などに、デプロイメントが高速化します。

[webpack](https://webpack.js.org/concepts/) など、JavaScript モジュール・バンドラーを使用できます。webpack は、コードを処理する際、アクションで必要なすべてのモジュールが含まれた依存関係グラフを再帰的に作成します。

以下に、webpack を使用した簡単な例を示します。

前述の例の `package.json` を使用し、`webpack` を開発依存関係として追加し、いくつかの npm スクリプト・コマンドを追加します。
```json
{
  "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
    "build": "webpack --config webpack.config.js",
    "deploy": "ibmcloud wsk action update my-action dist/bundle.js --kind nodejs:8"
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

webpack 構成ファイル `webpack.config.js` を作成します。
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

変数 `global.main` をアクションの main 関数に設定します。
前述の例より:
```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}
global.main = myAction;
```
{: codeblock}

関数名が `main` の場合、代わりに以下の構文を使用します。
```javascript
global.main = main;
```
{: codeblock}


`npm` および `webpack` を使用して OpenWhisk アクションを作成してデプロイするには、以下のようにします。

1. まず、以下のように、依存関係をローカルでインストールします。

  ```
  npm install
  ```
  {: pre}

2. 以下のように、webpack バンドルをビルドします。

  ```
  npm run build
  ```
  {: pre}

  ファイル `dist/bundle.js` が作成され、アクションのソース・コードとしてデプロイで使用されます。

3. `npm` スクリプトまたは CLI を使用して、アクションを作成します。
  `npm` スクリプトを使用する場合:
  ```
  npm run deploy
  ```
  {: pre}
  CLI を使用する場合:
  ```
  ibmcloud wsk action update my-action dist/bundle.js
  ```
  {: pre}


最後に、`webpack` でビルドされたバンドル・ファイルでは、バイナリー依存関係はサポートされず、JavaScript 依存関係がサポートされます。そのため、バンドルがバイナリー依存関係に依存している場合、ファイル `bundle.js` に含まれていないことが原因で、アクションの呼び出しが失敗します。

## アクション・シーケンスの作成
{: #openwhisk_create_action_sequence}

アクションのシーケンスを一緒にチェーニングしたアクションを作成できます。

いくつかのユーティリティー・アクションが `/whisk.system/utils` という名前のパッケージに入って提供されており、初めてのシーケンスを作成するためにこれを使用できます。パッケージについて詳しくは、『[パッケージ](./openwhisk_packages.html)』セクションを参照してください。

1. `/whisk.system/utils` パッケージのアクションを表示します。

  ```
  wsk package get --summary /whisk.system/utils
  ```
  {: pre}

  ```
  package /whisk.system/utils: Building blocks that format and assemble data
   action /whisk.system/utils/head: Extract prefix of an array
   action /whisk.system/utils/split: Split a string into an array
   action /whisk.system/utils/sort: Sorts an array
   action /whisk.system/utils/echo: Returns the input
   action /whisk.system/utils/date: Current date and time
   action /whisk.system/utils/cat: Concatenates input into a string
  ```

  この例では、`split` アクションと `sort` アクションを使用します。

2. アクション・シーケンスを作成して、1 つのアクションの結果が次のアクションに引数として渡されるようにします。

  ```
  wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

  このアクション・シーケンスは、数行のテキストを 1 つの配列に変換し、行をソートします。

3. 以下のように、アクションを呼び出します。

  ```
  wsk action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
  ```
  {: pre}

  ```json
  {
      "length": 3,
      "lines": [
          "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
  }
  ```

  結果では行がソートされていることが分かります。

**注**: シーケンス中のアクション間で渡されるパラメーターは、デフォルト・パラメーターを除いて、明示的です。
したがって、アクション・シーケンスに渡されるパラメーターを使用できるのはシーケンス中の先頭アクションのみです。
シーケンス中の先頭アクションの結果が、シーケンス中の 2 番目のアクションへの入力 JSON オブジェクトになります (以下同様)。
このオブジェクトには、最初にシーケンスに渡されたパラメーターはどれも含まれません (ただし、先頭アクションがその結果にそれらのパラメーターを明示的に組み込んだ場合は除きます)。
あるアクションへの入力パラメーターは、そのアクションのデフォルト・パラメーターとマージされます。その際、入力パラメーターが優先され、一致するデフォルト・パラメーターはオーバーライドされます。
複数の名前付きパラメーターを指定したアクション・シーケンス呼び出しについて詳しくは、『[デフォルト・パラメーターの設定](./openwhisk_actions.html#openwhisk_binding_actions)』を参照してください。

## Python アクションの作成
{: #creating-python-actions}

Python アクションを作成するプロセスは、JavaScript アクションと似ています。以下のセクションでは、単一 Python アクションの作成と呼び出し、および、そのアクションへのパラメーターの追加について説明します。

### Python アクションの作成と呼び出し
{: #openwhisk_actions_python_invoke}

アクションは、単にトップレベルの Python 関数です。例えば、以下のソース・コードで `hello.py` という名前のファイルを作成します。

```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
    print(greeting)
    return {"greeting": greeting}
```
{: codeblock}

Python アクションは常に辞書を取り込み、辞書を生成します。アクションの入り口メソッドはデフォルトで `main` ですが、他のアクション・タイプと同様に、`wsk` CLI でアクションを作成するために `--main` を使用して明示的に指定することもできます。

次のように、この関数から `helloPython` という名前の OpenWhisk アクションを作成できます。
```
wsk action create helloPython hello.py
```
{: pre}

CLI は、ソース・ファイルの拡張子からアクションのタイプを自動的に推定します。`.py` ソース・ファイルの場合、アクションは、Python 2 ランタイムを使用して実行されます。また、パラメーター `--kind python:3` を明示的に指定することで、Python 3 で実行されるアクションを作成することも可能です。さらに、IBM Cloudant、IBM DB2、IBM COS、IBM Watson などの IBM Cloud Services 用の追加パッケージが含まれた、種類が `python-jessie:3` の Python 3 ランタイムもあります。
この Python 3 ランタイムに含まれているパッケージについて詳しくは、Python ランタイムの[リファレンス](./openwhisk_reference.html#openwhisk_ref_python_environments)を参照してください。

以下のように、Python アクションにおけるアクション呼び出しは、JavaScript アクションの場合と同じです。
```
wsk action invoke --result helloPython --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

### zip ファイルへの Python アクションのパッケージ化
{: #openwhisk_actions_python_zip}

Python アクションおよび依存モジュールを zip ファイルでパッケージ化することができます。
エントリー・ポイント (例: `main`) を含むソース・ファイルのファイル名は `__main__.py` でなければなりません。
例えば、`helper.py` という名前のヘルパー・モジュールを含むアクションを作成するには、以下のように、まずソース・ファイルを含むアーカイブを作成します。

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}

次に、以下のようにアクションを作成します。

```bash
wsk action create helloPython --kind python:3 helloPython.zip
```
{: pre}

ここでのステップは、Python 3 (種類は `python:3`) の場合について示していますが、代替の Python の種類 `python:2` や `python-jessie:3` でも同様に実行できます。


### zip ファイルへの、仮想環境を使用した Python アクションのパッケージ化
{: #openwhisk_actions_python_virtualenv}

Python 依存関係をパッケージ化する別の方法として、仮想環境 (`virtualenv`) を使用できます。これにより、例えば、[`pip`](https://packaging.python.org/installing/) を介してインストールできる追加パッケージをリンクできます。


基本 zip ファイル・サポートと同様に、メインエントリー・ポイントを含むソース・ファイルの名前は `__main__.py` でなければなりません。明確に言えば、`__main__.py` の内容は main 関数であるため、この例では、前のセクションの `hello.py` を `__main__.py` に名前変更できます。また、virtualenv ディレクトリーは、`virtualenv` という名前でなければなりません。依存関係をインストールし、それらを virtualenv でパッケージ化し、互換性のある OpenWhisk アクションを作成する以下のシナリオ例を参照してください。

OpenWhisk ランタイム・コンテナーとの互換性を確保するために、virtualenv 内のパッケージのインストールは、種類に合った対応しているイメージを使用するターゲット環境で行う必要があります。
- 種類 `python:2` の場合、docker イメージ `openwhisk/python2action` を使用します。
- 種類 `python:3` の場合、docker イメージ `openwhisk/python3action` を使用します。
- 種類 `python-jessie:3` の場合、docker イメージ `ibmfunctions/action-python-v3` を使用します。

1. インストールする `pip` モジュールおよびバージョンが含まれた [requirements.txt ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) ファイルがある場合、以下を実行することで、依存関係をインストールし、互換性のある Docker イメージを使用して virtualenv を作成します。
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
    ```
    {: pre}

2. 以下のように、virtualenv ディレクトリーと追加の Python ファイルをアーカイブします。
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

3. 以下のように、アクションを作成します。
    ```
    wsk action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}


## PHP アクションの作成
{: #creating-php-actions}

PHP アクションを作成するプロセスは、JavaScript アクションと似ています。以下のセクションでは、単一 PHP アクションの作成と呼び出し、およびそのアクションへのパラメーターの追加について説明します。

### PHP アクションの作成と呼び出し
{: #openwhisk_actions_php_invoke}

アクションは、単にトップレベルの PHP 関数です。例えば、以下のソース・コードを使用して `hello.php` という名前のファイルを作成します。

```php
<?php
function main(array $args) : array
{
    $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
```

PHP アクションは常に連想配列を取り込み、連想配列を返します。アクションの入り口メソッドはデフォルトで `main` ですが、他のアクション・タイプと同様に、`wsk` CLI でアクションを作成する際に `--main` を使用して明示的に指定することもできます。

以下のように、この関数から `helloPHP` という名前の OpenWhisk アクションを作成できます。

```
wsk action create helloPHP hello.php
```
{: pre}

CLI は、ソース・ファイルの拡張子からアクションのタイプを自動的に推定します。`.php` ソース・ファイルの場合、アクションは PHP 7.1 ランタイムを使用して実行されます。詳しくは、PHP の[リファレンス](./openwhisk_reference.html#openwhisk_ref_php)を参照してください。

以下のように、PHP アクションにおけるアクション呼び出しは、JavaScript アクションの場合と同じです。

```
wsk action invoke --result helloPHP --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

### zip ファイルへの PHP アクションのパッケージ化
{: #openwhisk_actions_php_zip}

PHP アクションを、他のファイルおよび依存パッケージと共に zip ファイルにパッケージ化することができます。
エントリー・ポイント (例えば、`main`) を含むソース・ファイルのファイル名は、`index.php` にする必要があります。
例えば、`helper.php` という名前の 2 番目のファイルを含むアクションを作成するには、以下のように、まずソース・ファイルを含むアーカイブを作成します。

```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

次に、以下のようにアクションを作成します。

```bash
wsk action create helloPHP --kind php:7.1 helloPHP.zip
```
{: pre}

## Swift アクションの作成
{: #creating-swift-actions}

Swift アクションを作成するプロセスは、JavaScript アクションと似ています。以下のセクションでは、単一 Swift アクションの作成と呼び出し、および、そのアクションへのパラメーターの追加について説明します。

オンラインの [Swift Sandbox](https://swiftlang.ng.bluemix.net) を使用して、Xcode をマシンにインストールする必要なく Swift コードをテストすることもできます。

### アクションの作成と呼び出し

アクションは、単にトップレベルの Swift 関数です。例えば、
以下の内容で `hello.swift` という名前のファイルを作成します。

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

Swift アクションは常に辞書を取り込み、辞書を生成します。

次のように、
この関数から `helloSwift` という名前の {{site.data.keyword.openwhisk_short}} アクションを作成できます。

```
wsk action create helloSwift hello.swift --kind swift:3.1.1
```
{: pre}
 

常に `swift:3.1.1` を指定します。これは、以前の Swift バージョンがサポートされないためです。
{: tip}

以下のように、Swift アクションにおけるアクション呼び出しは、JavaScript アクションの場合と同じです。

```
wsk action invoke --result helloSwift --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

**注意:** Linux 環境で実行される Swift アクションは、まだ開発中です。{{site.data.keyword.openwhisk_short}} は通常、使用可能な最新のリリースを使用しますが、これは必ずしも安定していません。それに加えて、{{site.data.keyword.openwhisk_short}} で使用される Swift のバージョンは、安定したリリースの MacOS 用 XCode からの Swift のバージョンと不整合である可能性があります。

### Swift 実行可能ファイルとしてのアクションのパッケージ化
{: #openwhisk_actions_swift_zip}

Swift ソース・ファイルを使用して OpenWhisk Swift アクションを作成した場合、アクションを実行する前に、バイナリーにコンパイルしておく必要があります。これを行った後は、アクションを保持しているコンテナーがパージされるまで、そのアクションに対する後続の呼び出しが大幅に高速になります。この遅延は、コールド・スタートの遅延と呼ばれます。

コールド・スタートの遅延を避けるために、Swift ファイルをバイナリーにコンパイルしてから、zip ファイルとして OpenWhisk にアップロードすることができます。OpenWhisk スキャフォールドが必要になるため、バイナリーを作成する最も簡単な方法は、実行する環境と同じ環境内でビルドすることです。以下のステップを参照してください。

- 以下のコマンドを使用して、対話式 Swift アクション・コンテナーを実行します。
  ```
  docker run --rm -it -v "$(pwd):/owexec" openwhisk/action-swift-v3.1.1 bash
  ```
  {: pre}
  
- ソース・コードをコピーして、それをビルドする準備をします。
  ```
  cp /owexec/hello.swift /swift3Action/spm-build/main.swift 
  ```
  {: pre}

  ```
  cat /swift3Action/epilogue.swift >> /swift3Action/spm-build/main.swift
  ```
  {: pre}

  ```
  echo '_run_main(mainFunction:main)' >> /swift3Action/spm-build/main.swift
  ```
  {: pre}

- (オプション) 依存関係を追加する `Package.swift` ファイルを作成します。
   ```
   swift import PackageDescription
   
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
   {: pre}

  この例では、依存関係 `swift-watson-sdk` および `example-package-deckofplayingcards` を追加します。
  `CCurl`、`Kitura-net`、および `SwiftyJSON` は標準の Swift アクションで提供されているため、それらを独自の `Package.swift` に含めることができる点に留意してください。

- Package.swift を spm-build ディレクトリーにコピーします。
  ```
  cp /owexec/Package.swift /swift3Action/spm-build/Package.swift
  ```
  {: pre}

- spm-build ディレクトリーに移動します。
  ```
  cd /swift3Action/spm-build
  ```
  {: pre}

- Swift アクションをコンパイルします。
  ```
  swift build -c release
  ```
  {: pre}

- zip アーカイブを作成します。
  ```
  zip /owexec/hello.zip .build/release/Action
  ```
  {: pre}

- Docker コンテナーを終了します。
  ```
  exit
  ```
  {: pre}

hello.zip が hello.swift と同じディレクトリーに作成されたことが分かります。 

- 以下のように、これをアクション名 helloSwifty として OpenWhisk にアップロードします。
  ```
  wsk action update helloSwiftly hello.zip --kind swift:3.1.1
  ```
  {: pre}

- どのくらい高速になったかを確認するために、以下を実行します。 
  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

アクションの実行にかかった時間は「duration」プロパティーにあります。hello アクションでコンパイル・ステップを含む実行にかかる時間と比較してください。

## Java アクションの作成
{: #creating-java-actions}

Java アクションを作成するプロセスは、JavaScript アクションや Swift アクションと似ています。以下のセクションでは、単一 Java アクションの作成と呼び出し、および、そのアクションへのパラメーターの追加について説明します。

Java ファイルをコンパイル、テスト、およびアーカイブするには、[JDK 8](http://openjdk.java.net/install) がローカルにインストールされている必要があります。

### アクションの作成と呼び出し
{: #openwhisk_actions_java_invoke}

Java アクションは、以下とまったく同じシグニチャーを持つ `main` と呼ばれるメソッドを持つ Java プログラムです。
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

例えば、以下の内容で `Hello.java` という Java ファイルを作成します。

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

次に、以下のように、`Hello.java` をコンパイルして JAR ファイル `hello.jar` を作成します。
```
javac Hello.java
```
{: pre}

```
jar cvf hello.jar Hello.class
```
{: pre}

Java ファイルをコンパイルするには、[google-gson](https://github.com/google/gson) が Java CLASSPATH に含まれている必要があります。
{: tip}

以下に示すように、
この JAR ファイルから `helloJava` という OpenWhisk アクションを作成できます。

```
wsk action create helloJava hello.jar --main Hello
```
{: pre}

コマンド・ラインと `.jar` ソース・ファイルを使用する場合、Java アクションを作成していることを指定する必要はありません。ツールは、ファイル拡張子からそのことを判別します。

`--main` を使用して、メイン・クラスの名前を指定する必要があります。適格なメイン・クラスは、静的 `main` メソッドを実装するクラスです。クラスがデフォルト・パッケージ内にない場合は、完全修飾 Java クラス名を使用してください (例: `--main com.example.MyMain`)。

必要に応じて、Java アクションのメソッド名をカスタマイズすることもできます。これを行うには、アクションの Java 完全修飾メソッド名 (例えば、`--main com.example.MyMain#methodName`) を指定します。

以下のように、Java アクションにおけるアクション呼び出しは、Swift および JavaScript アクションの場合と同じです。

```
wsk action invoke --result helloJava --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

## Docker アクションの作成
{: #creating-docker-actions}

{{site.data.keyword.openwhisk_short}} Docker アクションでは、任意の言語でアクションを作成できます。

コードはコンパイルされて実行可能バイナリーになり、Docker イメージに組み込まれます。バイナリー・プログラムとシステムの対話は、`stdin` から入力を受け取り、`stdout` を通して応答することによって行われます。

前提条件として、Docker Hub アカウントを持っている必要があります。無料の Docker ID およびアカウントをセットアップするには、[Docker Hub](https://hub.docker.com) にアクセスしてください。

以下の説明では、Docker ユーザー ID が `janesmith` であり、パスワードが `janes_password`　であると想定しています。CLI がセットアップ済みである場合、{{site.data.keyword.openwhisk_short}} で使用するためのカスタム・バイナリーをセットアップするには、残りのステップは 3 つです。その後、アップロードされた Docker イメージをアクションとして使用できます。

1. Docker スケルトンをダウンロードします。以下のように、CLI を使用してダウンロードしてインストールできます。

  ```
  wsk sdk install docker
  ```
  {: pre}

  Docker スケルトンが現行ディレクトリーにインストールされました。
  
  ```
  ls dockerSkeleton/
  ```
  {: pre}

  ```
  Dockerfile      README.md       buildAndPush.sh example.c
  ```

  このスケルトンは、Docker コンテナー・テンプレートであり、そこにカスタム・バイナリーの形でコードを注入できます。

2. ブラック・ボックス・スケルトン内にカスタム・バイナリーをセットアップします。スケルトンには既に C プログラムが含まれているので、それを使用できます。

  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

  必要に応じてこのファイルを変更するか、または、追加コードおよび依存関係を Docker イメージに追加できます。
  後者の場合、必要に応じて `Dockerfile` を調整して実行可能ファイルをビルドできます。
  そのバイナリーはコンテナー内部の `/action/exec` に置く必要があります。

  実行可能ファイルはコマンド・ラインから単一の引数を受け取ります。それは、アクションへの引数を表す JSON オブジェクトのストリング・シリアライゼーションです。プログラムは `stdout` または `stderr` にログを記録することがあります。
  規則により、出力の最終行は、アクションの結果を表す、stringify によって文字列化された JSON オブジェクト_でなければなりません_。

3. Docker イメージをビルドし、提供されているスクリプトを使用してアップロードします。最初に `docker login` を実行して認証し、次に、選択したイメージ名でスクリプトを実行する必要があります。

  ```
  docker login -u janesmith -p janes_password
  ```
  {: pre}

  ```
  cd dockerSkeleton
  ```
  {: pre}

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  `example.c` ファイルの部分は Docker イメージのビルド・プロセスの一環としてコンパイルされるので、ご使用のマシン上で C をコンパイルする必要はないことに注意してください。
  実際、このバイナリーは、互換ホスト・マシン上でコンパイルしないと、フォーマットが一致しないためにコンテナー内では稼働できません。

  これで、Docker コンテナーを OpenWhisk アクションとして使用できます。


  ```
  wsk action create example --docker janesmith/blackboxdemo
  ```
  {: pre}

  アクションを作成するための `--docker` の使用に注意してください。すべての Docker イメージが Docker Hub でホストされると想定されています。
  このアクションは他の {{site.data.keyword.openwhisk_short}} アクションと同様に呼び出すことができます。 

  ```
  wsk action invoke --result example --param payload Rey
  ```
  {: pre}

  ```json
  {
      "args": {
          "payload": "Rey"
      },
      "msg": "Hello from arbitrary C program!"
  }
  ```

  Docker アクションを更新するには、`buildAndPush.sh` を実行して、最新イメージを Docker Hub にアップロードします。これにより、システムは、アクション用のコードの次回実行時に新規 Docker イメージをプルできるようになります。ウォーム・コンテナーがない場合、新しい呼び出しは新規 Docker イメージを使用します。ただし、前のバージョンの Docker イメージを使用しているウォーム・コンテナーがある場合は、`wsk action update` を実行しない限り、新しい呼び出しはそのイメージを使用し続けます。これは、新しい呼び出しには Docker プルを実行して新規 Docker イメージを取得するようにシステムに指示します。

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  ```
  wsk action update example --docker janesmith/blackboxdemo
  ```
  {: pre}

  『[リファレンス](./openwhisk_reference.html#openwhisk_ref_docker)』セクションに、Docker アクションの作成に関する詳細情報があります。

  CLI の以前のバージョンは、パラメーターなしで `--docker` をサポートしており、イメージ名は位置引数でした。Docker アクションが (zip) ファイルを介して初期化データを受け入れられるようにするために、Docker アクションのユーザー・エクスペリエンスを正規化します。そのため、位置引数 (ある場合) は、代わりにファイル (例えば、zip ファイル) でなければなりません。イメージ名は、`--docker` オプションの後に指定する必要があります。お客様からのフィードバックのおかげで、`--native` 引数が、`--docker openwhisk/dockerskeleton` の省略表現として組み込まれています。そのため、標準 Docker アクション SDK 内で実行される実行可能ファイルの作成およびデプロイが簡便になっています。
  
  例えば、このチュートリアルは、コンテナー内の `/action/exec` にバイナリー実行可能ファイルを作成しました。このファイルをローカル・ファイル・システムにコピーし、`exec.zip` に zip すると、以下のコマンドを使用して、その実行可能ファイルを初期化データとして受け取る Docker アクションを作成することができます。 

  ```
  wsk action create example exec.zip --native
  ```
  {: pre}

  これは、以下のコマンドと同等です。 
  ```
  wsk action create example exec.zip --docker openwhisk/dockerskeleton
  ```
  {: pre}

## 任意の実行可能ファイルを使用したアクションの作成
{: #creating-actions-arbitrary}

`--native` を使用して、任意の実行可能ファイルを OpenWhisk アクションとして実行できることが分かります。これには、`bash` スクリプトやクロス・コンパイルされたバイナリーが含まれます。後者の場合、バイナリーに `openwhisk/dockerskeleton` イメージとの互換性がなければならないという制約があります。

## Go アクションの作成
{: #creating-go-actions}

`--native` オプションにより、任意の実行可能ファイルをアクションとしてパッケージ化できます。例えば、これは Go の場合に利用可能です。
Docker アクションと同様に、Go 実行可能ファイルはコマンド・ラインから単一の引数を受け取ります。
それは、アクションへの引数を表す JSON オブジェクトのストリング・シリアライゼーションです。
プログラムは `stdout` または `stderr` にログを記録することがあります。
規則により、出力の最終行は、アクションの結果を表す、stringify によって文字列化された JSON オブジェクト_でなければなりません_。

以下に、Go アクションの例を示します。
```go
package main

import "encoding/json"
import "fmt"
import "os"

func main() {
    // プログラムで 1 つの引数 (ストリングとしての JSON オブジェクト) を受け取る
    arg := os.Args[1]

    // ストリングを JSON オブジェクトにアンマーシャルする
    var obj map[string]interface{}
    json.Unmarshal([]byte(arg), &obj)

    // オプションとして、STDOUT (または STDERR) に出力可能
    fmt.Println("hello Go action")

    name, ok := obj["name"].(string)
    if !ok { name = "Stranger" }

    // STDOUT の最終行は、ストリングとしての結果の JSON オブジェクト
    msg := map[string]string{"msg": ("Hello, " + name + "!")}
    res, _ := json.Marshal(msg)
    fmt.Println(string(res))
}
```

上記のコードをファイル `sample.go` に保存し、それを OpenWhisk 用にクロス・コンパイルします。実行可能ファイルの名前は `exec` でなければなりません。
```bash
GOOS=linux GOARCH=amd64 go build -o exec
zip exec.zip exec
wsk action create helloGo --native exec.zip
```

このアクションは、他のアクションと同様に実行可能です。
```bash
wsk action invoke helloGo -r -p name gopher
{
    "msg": "Hello, gopher!"
}
```

同様にログも取得されます。
```bash
wsk activation logs --last --strip
my first Go action.
```

## アクション出力のモニター
{: #watching-action-output}

{{site.data.keyword.openwhisk_short}} アクションは、他のユーザーによって、各種イベントに対する応答として、あるいはアクション・シーケンスの一部として呼び出すことができます。そのような場合には、呼び出しをモニターすると役立ちます。

{{site.data.keyword.openwhisk_short}} CLI を使用して、呼び出されたアクションの出力を監視できます。

1. シェルから以下のコマンドを発行します。
  ```
  wsk activation poll
  ```
  {: pre}

  このコマンドは、アクティベーションからのログを継続的にチェックするポーリング・ループを開始します。

2. 別のウィンドウに切り替えて、以下のようにアクションを呼び出します。

  ```
  wsk action invoke /whisk.system/samples/helloWorld --param payload Bob
  ```
  {: pre}

  ```
  ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
  ```

3. 以下のように、ポーリング・ウィンドウでアクティベーション・ログを監視します。

  ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
  ```

  同様に、ポーリング・ユーティリティーを実行すると、OpenWhisk で実行されたアクションのログをリアルタイムで確認できます。

## 大規模アプリケーション・サポート
{: #large-app-support}

アクションの最大コード・サイズは 48 MB です。多くのサード・パーティーのモジュール、ネイティブ・ライブラリー、外部ツールが含まれたアプリケーションでは、この制限に達する可能性があります。

48 MB を超えるパッケージ・アクション (zip または jar) が作成された場合、解決策としては、依存関係を含めてランタイム・イメージを拡張してから、単一のソース・ファイルまたは 48 MB より小さいアーカイブを使用します。

例えば、必要な共有ライブラリーが含まれたカスタム Docker ランタイムをビルドすることで、該当する依存関係をアーカイブ・ファイル内に含める必要がなくなります。プライベート・ソース・ファイルは、引き続き、アーカイブにバンドルし、実行時に注入できます。

アーカイブ・ファイルのサイズを削減する別のメリットとして、デプロイメント時間も改善されることが挙げられます。

### Python の例

以下の Python の例では、opencv にライブラリー `opencv-python` を含めてから、opencv バイナリーを OS イメージにインストールできます。次に、`requirements.txt` を使用し、`pip install requirements.txt` を実行して、追加の Python ライブラリーでイメージを拡張できます。その後、新しいイメージで `action.py` を使用できます。

### Node.js の例

以下の Node.js の例では、追加のパッケージを OS イメージにインストールできます。

以下のように、`npm` を使用して opencv をインストールします。
```
npm install opencv
```
{: pre}

同様に、`package.json` がある場合、以下のように、`npm` を使用してそれをインストールします。
```
npm install package.json
```
{: pre}

その後、新しいイメージで `action.js` を使用します。

## アクションのリスト表示
{: #listing-actions}

以下のコマンドを使用して、作成されたすべてのアクションをリストできます。

```
wsk action list
```
{: pre}

作成したアクションが多くなるほど、このリストは長くなるため、関連するアクションを[パッケージ](./openwhisk_packages.html)にグループ化すると役立ちます。アクションのリストをフィルターに掛けて、特定のパッケージ内のもののみにするために、以下のコマンド構文を使用できます。 

```
wsk action list [PACKAGE NAME]
```
{: pre}

## アクションの削除
{: #deleting-actions}

使用しないアクションを削除することによって、クリーンアップできます。

1. アクションを削除するには、以下のコマンドを実行します。
  ```
  wsk action delete hello
  ```
  {: pre}

  ```
  ok: deleted hello
  ```

2. 当該アクションがアクションのリストに表示されなくなっていることを確認します。
  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  ```

## アクション本体の中のアクション・メタデータへのアクセス
{: #accessing-action-metadata-within-the-action-body}

アクション環境は、実行中のアクションに固有のいくつかのプロパティーを含んでいます。
これらによって、アクションは REST API を介して OpenWhisk アセットをプログラマチックに処理したり、
アクションに割り振られた時間割り当て量を使い切ってしまいそうなときに内部アラームを設定したりできます。
サポートされるすべてのランタイム (Node.js、Python、Swift、Java、OpenWhisk Docker スケルトンを使用した場合の Docker アクション) で、
システム環境を介して各種プロパティーにアクセスできます。

* `__OW_API_HOST`: このアクションを実行している OpenWhisk デプロイメントの API ホスト
* `__OW_API_KEY`: アクションを起動するサブジェクトの API キー (制限付き API キーである場合もあります)
* `__OW_NAMESPACE`: _アクティベーション_ の名前空間 (アクションの名前空間と同じでないこともあります)
* `__OW_ACTION_NAME`: 実行中のアクションの完全修飾名
* `__OW_ACTIVATION_ID`: 実行中のアクション・インスタンスのアクティベーション ID
* `__OW_DEADLINE`: このアクションが期間割り当て量全体を消費し切ると推定されるおよその時刻 (エポック・ミリ秒単位)
