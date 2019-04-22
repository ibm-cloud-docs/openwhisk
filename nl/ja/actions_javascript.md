---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-27"

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


# JavaScript アクションの作成
{: #actions_javascript
{: #creating-and-invoking-javascript-actions}

以下のセクションでは、JavaScript でのアクションの操作について説明します。 はじめに、簡単なアクションを作成して呼び出します。 それから、アクションにパラメーターを追加し、そのアクションをパラメーターを指定して呼び出します。 次に、デフォルトのパラメーターを設定して、アクションを呼び出します。 最後に、非同期アクションを作成します。
{: shortdesc}

## 単純な JavaScript アクションの作成と呼び出し
{: #single_action_js}
{: #openwhisk_single_action_js}

以下のステップと例を見て、最初の JavaScript アクションを作成してください。

1. 以下のコードを `hello.js` という名前のファイルに保存します。

  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  この JavaScript ファイルにさらに関数を含めることもできます。 ただし、規則により、アクションのエントリー・ポイントを提供するために `main` という名前の関数が存在している必要があります。
  {: shortdesc}

2. JavaScript 関数を使用して、`hello` というアクションを作成します。

  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  出力例:
  
  ```
  ok: created action hello
  ```
  {: screen}

  アクションのタイプは、ソース・ファイルの拡張子を使用して判別されます。`.js` ソース・ファイルの場合、アクションは Node.js ランタイムを使用して実行されます。JavaScript アクションの Node.js ランタイムのバージョンを指定するには、`--kind` パラメーターに `nodejs:10` または `nodejs:8` を設定します。Node.js ランタイムについて詳しくは、[ランタイム](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments)を参照してください。
  {: shortdesc}

3. 作成した `hello` アクションがアクション・リストに載っていることを確認します。

  ```
  ibmcloud fn action list
  ```
  {: pre}

  出力例:
  
  ```
  actions
  hello       private
  ```
  {: screen}

4. ブロッキング呼び出しを実行して、クラウド内でアクションを実行します。 ブロッキング呼び出しは、要求/応答形式を使用するため、アクティベーション結果が使用可能になるまで待機します。 待機時間は、60 秒と、アクションの[制限時間値](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits)のいずれか小さいほうです。

    ```
    ibmcloud fn action invoke --blocking hello
    ```
    {: pre}

    このコマンドにより、以下の情報が出力されます。
        * ログや呼び出し結果の取得に使用できるアクティベーション ID (`44794bd6aab74415b4e42a308d880e5b`)
        * 予期される待機時間内に使用可能になった場合は呼び出し結果

    ```
    ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

    {
        "result": {
            "payload": "Hello world"
        },
      "status": "success",
      "success": true
  }
    ```
    {: screen}
    
    アクティベーションのモニタリングに関するヒントについては、[「アクションの出力のモニター」](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#monitor-action-output)を参照してください。
    {: tip}

5. アクションの結果が今すぐ必要でない場合は、`--blocking` フラグを省略して非ブロッキング呼び出しを実行してもかまいません。

    1. 非ブロッキング呼び出しを実行します。

        ```
        ibmcloud fn action invoke hello
        ```
        {: pre}

        出力例:

        ```
        ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: screen}

    2. アクティベーション ID を使用してアクションの結果を取得します。

        ```
        ibmcloud fn activation result 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: pre}

        以下に示すアクションの結果が返ります。

        ```
        {
            "payload": "Hello world"
  }
        ```
        {: screen}

6. アクティベーション ID を記録し忘れた場合でも、最も新しいものから順に並んだアクティベーションのリストを取得できます。

    ```
    ibmcloud fn activation list
    ```
    {: pre}

    出力:
    ```
    activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
    ```
    {: screen}
    
### JavaScript 関数プロトタイプ
{: #openwhisk_ref_javascript_fnproto}

{{site.data.keyword.openwhisk_short}} JavaScript アクションは Node.js ランタイムで実行されます。

JavaScript で記述されたアクションは、単一ファイルに収容しなければなりません。 そのファイルには複数の関数を含めることができますが、規則により `main` という名前の関数が存在しなければならず、これが、アクションを起動したときに呼び出されます。 例えば、以下の例は、複数の関数があるアクションを示しています。
```javascript
function main() {
    return { payload: helper() }
}

function helper() {
    return new Date();
}
```
{: codeblock}

アクションの入力パラメーターは、1 つの JSON オブジェクトのパラメーターとして、`main` 関数に渡されます。 成功したアクティベーションの結果も JSON オブジェクトですが、どのように返されるのかは、以下のセクションで説明されているように、アクションが同期か非同期化によって異なります。

### 同期と非同期の動作
{: #openwhisk_ref_javascript_synchasynch}

JavaScript 関数が、戻った後でもコールバック関数で実行を続行することはよくあります。 この動作に対応するため、JavaScript アクションのアクティベーションは、*同期* にすることも*非同期* にすることもできます。

JavaScript アクションのアクティベーションは、以下のいずれかの状態で main 関数が終了する場合は**同期** です。

- main 関数が `return` ステートメントを実行せずに終了する。
- main 関数が Promise *以外* の値を返す `return` ステートメントを実行して終了する。

同期アクションの例を以下に示します。

```javascript
// an action in which each path results in a synchronous activation
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

main 関数が Promise を返して終了する場合、JavaScript アクションのアクティベーションは**非同期** です。 この場合、Promise が完了するか、拒否されるまで、システムはアクションがまだ実行中であると見なします。
まず、新規の Promise オブジェクトをインスタンス化して、それをコールバック関数に渡します。 コールバックは、resolve と reject という 2 つの引数を使用します。これらはどちらも関数です。 すべての非同期コードが、そのコールバックに入っていきます。

以下の例は、resolve 関数を呼び出すことによって Promise を完了する方法を示します。
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

次の例は、reject 関数を呼び出すことによって Promise を拒否する方法を示します。
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

以下の例に示すように、入力によって同期であったり非同期であったりするアクションがあり得ます。
```javascript
function main(params) {
     if (params.payload) {
        // asynchronous activation
         return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
                }, 100);
             })
     }  else {
        // synchronous activation
         return {done: true};
      }
}
```
{: codeblock}

アクティベーションが同期か非同期かに関係なく、アクションの呼び出しはブロッキングであることも非ブロッキングであることも可能です。

## 非同期アクションの作成
{: #asynchronous_javascript}
{: #openwhisk_asynchrony_js}

非同期に実行される JavaScript 関数により、アクションで Promise を返すことで、`main` 関数が戻った後に、アクティベーション結果を返すことができます。

1. 以下のコードを `asyncAction.js` という名前のファイルに保存します。

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

    * `main` 関数は Promise を返しています。 この Promise は、アクティベーションはまだ完了していないが、今後完了すると見込まれることを示しています。

    * `setTimeout()` という JavaScript 関数は、Promise のコールバック関数を呼び出す前に 2 秒間待機します。このコールバック関数は非同期コードを表しています。

    * Promise のコールバックは、`resolve` 引数と `reject` 引数を受け入れます。この 2 つはいずれも関数です。

      * `resolve()` の呼び出しは、Promise を完了し、アクティベーションが正常に実行されたことを示します。
      * `reject()` の呼び出しでは、Promise を拒否し、アクティベーションの実行で異常が発生したことを示すことができます。

2. `asyncAction` というアクションを作成します。
    ```
    ibmcloud fn action create asyncAction asyncAction.js --kind nodejs:10
    ```
    {: pre}

3. アクションを呼び出します。

    ```
    ibmcloud fn action invoke --result asyncAction
    ```
    {: pre}

    出力例:

    ```
    {
        "done": true
  }
    ```
    {: screen}

4. アクティベーション・ログを取得して、アクティベーションの実行に要した時間を確認します。

  1. アクティベーション ID を取得します。

      ```
      ibmcloud fn activation list --limit 1 asyncAction
      ```
      {: pre}

      出力例:
      ```
      activations
  b066ca51e68c4d3382df2d8033265db0             asyncAction
      ```
      {: screen}

  2. そのアクティベーション ID のログを取得します。

      ```
      ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
      ```
      {: pre}

      `duration` には、このアクティベーションが完了までに 2 秒強かかったことが示されています。

      ```
      ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
          ...
          "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
          ...
      }
      ```
      {: screen}

## 外部 API を呼び出すためのアクションの使用
{: #apicall_action}
{: #openwhisk_apicall_action}

これまで示した例では、JavaScript 関数はすべて自己完結型でした。 外部 API を呼び出すアクションを作成することもできます。
{: shortdesc}

以下の例は、毎日、宇宙のユニークなイメージを提供している NASA Astronomy Picture of the Day (APOD) サービスを呼び出します。

1. 以下のコードを `apod.js` という名前のファイルに保存します。

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

2. NASA APOD API への呼び出しが行われ、JSON 結果からフィールドが抽出されます。 アクションで使用できる Node.js パッケージについて詳しくは、[ランタイム](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments)を参照してください。

3. `apod` という名前のアクションを作成します。
    ```
    ibmcloud fn action create apod apod.js --kind nodejs:10
    ```
    {: pre}

3. `apod` アクションを呼び出します。
    ```
    ibmcloud fn action invoke --result apod
    ```
    {: pre}

    以下のサンプル・オブジェクトが返されます。

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

## Node.js モジュールとしてのアクションのパッケージ化
{: #packaging_javascript_actions}
{: #openwhisk_js_packaged_action}

単一の JavaScript ソース・ファイル内にすべてのアクション・コードを作成する代わりに、`npm` パッケージとしてアクションを作成できます。

例えば、以下のファイルが含まれるディレクトリーについて考えます。

1. 以下のコードを `package.json` という名前のファイルに保存します。

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

2. 以下のコードを `index.js` という名前のファイルに保存します。

    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    exports.main = myAction;
    ```
    {: codeblock}

    * アクションは、`exports.main` を介して公開されます。
    * アクション・ハンドラーの名前は、オブジェクトを受け入れたり、オブジェクト (またはオブジェクトの `Promise`) を返したりする際の標準的なシグニチャーに準拠している限り、どのような名前でもかまいません。
    * このファイルには `index.js` という名前を付けるか、または `package.json` 内の `main` プロパティーとして希望のファイル名指定する必要があります。

3. すべての依存関係をローカルにインストールします。

    ```
    npm install
    ```
    {: pre}

    **注**: ほとんどの `npm` パッケージでは、`npm install` を実行すると JavaScript のソースがインストールされますが、中には、バイナリー成果物のインストールおよびコンパイルを行うパッケージもあります。 アーカイブ・ファイルのアップロードでは、JavaScript の依存関係のみがサポートされています。 アーカイブにバイナリーの依存関係が含まれていると、アクションの呼び出しが失敗する場合があります。

4. すべての依存関係をはじめ、すべてのファイルが含まれている `.zip` アーカイブを作成します。

    ```
    zip -r action.zip *
    ```
    {: pre}

    Windows Explorer の操作を使用して zip ファイルを作成すると、誤った構造になります。 {{site.data.keyword.openwhisk_short}} の .zip アクションでは、`package.json` が zip のルートに存在する必要がありますが、Windows Explorer はこれをネストされたフォルダー内に配置します。 最も安全な方法としては、コマンド・ラインで `zip` コマンドを使用します。
    {: tip}

5. アクションを作成します。 `.zip` アーカイブからアクションを作成する時には、`--kind` パラメーターの値を設定して Node.js ランタイムのバージョンを指定する必要があります。`nodejs:8` または `nodejs:10` を選択してください。

    ```
    ibmcloud fn action create packageAction action.zip --kind nodejs:10
    ```
    {: pre}

6. アクションを呼び出します。

    ```
    ibmcloud fn action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
    ```
    {: pre}

    出力例:

    ```
    {
        "padded": [
            ".......................and now",
          "......for something completely",
          ".....................different"
      ]
    }
    ```
    {: screen}

## 単一のバンドルとしてのアクションのパッケージ化
{: #webpack_javascript}
{: #openwhisk_js_webpack_action}

アクションを .zip としてパッケージ化すると不要なファイルも多数含まれてしまう場合や、より高速なデプロイメントが必要な場合には、依存関係が含まれている単一の `.js` ファイルに必要最小限のコードを作成するとよいでしょう。
{: shortdesc}

[webpack ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://webpack.js.org/concepts/) のような JavaScript のモジュール・バンドラーを使用して、アクションをパッケージ化することができます。 `webpack` は、コードを処理する際、アクションで必要なすべてのモジュールが含まれた依存関係グラフを再帰的に作成します。

1. 以下のコードを `package.json` という名前のファイルに保存します。 `webpack` は開発時に必要となる依存関係として追加されます。

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

2. 以下の webpack 構成コードを `webpack.config.js` という名前のファイルに保存します。

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

3. 以下のコードを `index.js` という名前のファイルに保存します。 変数 `global.main` がアクションの main 関数に設定されます。

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

5. webpack バンドルをビルドします。

    ```
    npm run build
    ```
    {: pre}

    ファイル `dist/bundle.js` が作成され、アクションのソース・コードとしてデプロイされます。

6. `npm` スクリプトまたは CLI を使用して、アクションを作成します。

    * `npm` スクリプトを使用する場合:

        ```
        npm run deploy
        ```
        {: pre}

    * CLI を使用する場合:

        ```
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    `webpack` でビルドされたバンドル・ファイルでは、JavaScript の依存関係のみがサポートされています。 バイナリー・ファイルの依存項目は `bundle.js` ファイルに組み込まれていないので、バンドルがその依存項目に依存していると、アクションの呼び出しが失敗する場合があります。
    {: tip}
    



