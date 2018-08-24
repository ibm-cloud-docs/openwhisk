---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-23"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# アクションの作成と呼び出し
{: #openwhisk_actions}

アクションとは、{{site.data.keyword.openwhisk}} プラットフォームで実行されるステートレスなコード・スニペットです。 例えば、アクションを使用して、画像内の顔の検出、データベース変更に対する応答、一連の API 呼び出しの集約、ツイートの投稿を行うことができます。
{:shortdesc}

アクションは明示的に呼び出すか、イベントに応じて実行することができます。 いずれの場合も、アクションを実行するたびに、固有のアクティベーション ID で識別されるアクティベーション・レコードが生成されます。 アクションへの入力、およびアクションの結果は、キーと値のペアからなるディクショナリーになっています。ここで、キーはストリング、値は有効な JSON 値です。アクションは、他のアクションや定義されたアクション・シーケンスの呼び出しで構成することもできます。

アクションは、JavaScript、Swift、Python、PHP の関数として、また Java メソッドとして、あるいは Go プログラムや Docker コンテナーとしてパッケージされたカスタム実行可能ファイルなど任意のバイナリー互換実行可能ファイルとして作成できます。
お好みの開発環境におけるアクションの作成、呼び出し、デバッグ方法について説明します。

## JavaScript アクションの作成
{: #creating-and-invoking-javascript-actions}

以下のセクションでは、JavaScript でのアクションの操作について説明します。 はじめに、簡単なアクションを作成して呼び出します。それから、アクションにパラメーターを追加し、そのアクションをパラメーターを指定して呼び出します。次に、デフォルトのパラメーターを設定して、アクションを呼び出します。最後に、非同期アクションを作成します。

### 単純な JavaScript アクションの作成と呼び出し
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

2. JavaScript 関数を使用して、`hello` というアクションを作成します。
  ```
  ibmcloud fn action create hello hello.js
  ```
  {: pre}

  出力例:
  ```
  ok: created action hello
  ```
  {: screen}

  CLI は、ソース・ファイルの拡張子を使用して、アクションのタイプを自動的に推定します。 `.js` ソース・ファイルの場合、アクションは、Node.js 6 ランタイムを使用して実行されます。 また、パラメーター `--kind nodejs:8` を明示的に指定することで、Node.js 8 で実行されるアクションを作成することも可能です。 詳しくは、Node.js 6 と 8 を比較した[リファレンス](./openwhisk_reference.html#openwhisk_ref_javascript_environments)を参照してください。

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

4. ブロッキング呼び出しを実行して、クラウド内でアクションを実行します。ブロッキング呼び出しは、要求/応答形式を使用するため、アクティベーション結果が使用可能になるまで待機します。待機時間は、60 秒と、アクションの[制限時間値](./openwhisk_reference.html#openwhisk_syslimits)のいずれか小さいほうです。
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
    アクティベーションのモニタリングに関するヒントについては、[「アクションの出力のモニター」](openwhisk_managing.html#monitor-action-output)を参照してください。
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

### 非同期アクションの作成
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

    * `main` 関数は Promise を返しています。この Promise は、アクティベーションはまだ完了していないが、今後完了すると見込まれることを示しています。
    * `setTimeout()` という JavaScript 関数は、Promise のコールバック関数を呼び出す前に 2 秒間待機します。このコールバック関数は非同期コードを表しています。
    * Promise のコールバックは、`resolve` 引数と `reject` 引数を受け入れます。この 2 つはいずれも関数です。
      * `resolve()` の呼び出しは、Promise を完了し、アクティベーションが正常に実行されたことを示します。
      * `reject()` の呼び出しでは、Promise を拒否し、アクティベーションの実行で異常が発生したことを示すことができます。

2. `asyncAction` というアクションを作成します。
    ```
    ibmcloud fn action create asyncAction asyncAction.js
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

3. アクティベーション・ログを取得して、アクティベーションの実行に要した時間を確認します。

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

### 外部 API を呼び出すためのアクションの使用
{: #openwhisk_apicall_action}

これまで示した例では、JavaScript 関数はすべて自己完結型でした。 外部 API を呼び出すアクションを作成することもできます。
{: shortdesc}

以下の例は、毎日、宇宙のユニークなイメージを提供している NASA Astronomy Picture of the Day (APOD) サービスを呼び出します。

1. 以下のコードを `apod.js` という名前のファイルに保存します。
    ```javascript
    var url = "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo";

    $.ajax({
    url: url,
    success: function(result){
    if("copyright" in result) {
        $("#copyright").text("Image Credits: " + result.copyright);
    }
    else {
        $("#copyright").text("Image Credits: " + "Public Domain");
      }

      if(result.media_type == "video") {
        $("#apod_img_id").css("display", "none");
      $("#apod_vid_id").attr("src", result.url);
    }
    else {
        $("#apod_vid_id").css("display", "none");
      $("#apod_img_id").attr("src", result.url);
    }
      $("#reqObject").text(url);
    $("#returnObject").text(JSON.stringify(result, null, 4));
    $("#apod_explaination").text(result.explanation);
    $("#apod_title").text(result.title);
  }
    });
    ```
    {: codeblock}

    NASA APOD API への呼び出しが行われ、JSON 結果からフィールドが抽出されます。自分のアクションで使用できる Node.js パッケージについて詳しくは、[「システムの詳細および制限」](./openwhisk_reference.html#openwhisk_ref_javascript_environments)を参照してください。

2. `apod` という名前のアクションを作成します。
    ```
    ibmcloud fn action create apod apod.js
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

### Node.js モジュールとしてのアクションのパッケージ化
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
    * このファイルには **index.js** という名前を付けるか、または **package.json** に、`main`プロパティーとして好ましいファイル名を指定する必要があります。

3. すべての依存関係をローカルにインストールします。
    ```
    npm install
    ```
    {: pre}
    **注**: ほとんどの `npm` パッケージでは、`npm install` を実行すると JavaScript のソースがインストールされますが、中には、バイナリー成果物のインストールおよびコンパイルを行うパッケージもあります。アーカイブ・ファイルのアップロードでは、現在 JavaScript の依存関係のみがサポートされています。アーカイブにバイナリーの依存関係が含まれていると、アクションの呼び出しが失敗する場合があります。

4. すべての依存関係をはじめ、すべてのファイルが含まれている `.zip` アーカイブを作成します。
    ```
    zip -r action.zip *
    ```
    {: pre}

    Windows Explorer の操作を使用して zip ファイルを作成すると、誤った構造になります。 {{site.data.keyword.openwhisk_short}} zip アクションでは、`package.json` が zip のルートに存在する必要がありますが、Windows Explorer はこれをネストされたフォルダー内に配置します。 最も安全な方法としては、コマンド・ラインで `zip` コマンドを使用します。
    {: tip}

5. アクションを作成します。 `.zip` アーカイブからアクションを作成する際に、`nodejs:6` または `nodejs:8` を使用して、`--kind` フラグの値を明示的に指定する必要があります。
    ```
    ibmcloud fn action create packageAction --kind nodejs:6 action.zip
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

### 単一のバンドルとしてのアクションのパッケージ化
{: #openwhisk_js_webpack_action}

アクションを zip としてパッケージ化すると不要なファイルも多数含まれてしまう場合や、より高速なデプロイメントが必要な場合には、依存関係が含まれている単一の `.js` ファイルに必要最小限のコードを作成するとよいでしょう。
{: shortdesc}

[webpack ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://webpack.js.org/concepts/) のような JavaScript のモジュール・バンドラーを使用して、アクションをパッケージ化することができます。`webpack` は、コードを処理する際、アクションで必要なすべてのモジュールが含まれた依存関係グラフを再帰的に作成します。

1. 以下のコードを `package.json` という名前のファイルに保存します。 `webpack` は開発時に必要となる依存関係として追加されます。
    ```json
    {
      "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
        "build": "webpack --config webpack.config.js",
    "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:8"
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
        ibmcloud fn action update my-action dist/bundle.js
        ```
        {: pre}

    **注**: `webpack` でビルドされたバンドル・ファイルでは、JavaScript の依存関係のみがサポートされています。バイナリーの依存関係はファイル `bundle.js` には付属していないため、バンドルがバイナリーの依存関係に依存していると、アクションの呼び出しが失敗する場合があります。

## Python アクションの作成
{: #creating-python-actions}

以下のセクションでは、単一 Python アクションの作成と呼び出し、および、そのアクションへのパラメーターの追加について説明します。

### Python アクションの作成と呼び出し
{: #openwhisk_actions_python_invoke}

アクションは、単にトップレベルの Python 関数です。 Python のアクションを作成するには以下のようにします。

1. 以下のコードを `hello.py` という名前のファイルに保存します。
    ```python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
        print(greeting)
    return {"greeting": greeting}
    ```
    {: codeblock}

  * Python アクションは常に辞書を取り込み、辞書を生成します。
  * このアクションの入り口メソッドはデフォルトで `main` になっていますが、`wsk` CLI を使用して `--main` フラグを指定すれば、明示的に入り口メソッドを指定してアクションを作成することができます。

2. `helloPython` アクションを作成します。
    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    出力例:
    ```
    ok: created action helloPython
    ```
    {: screen}

    CLI は、ソース・ファイルの拡張子を使用して、アクションのタイプを自動的に推定します。 `.py` ソース・ファイルの場合、アクションは、Python 2 ランタイムを使用して実行されます。 また、パラメーター `--kind python:3` を明示的に指定することで、Python 3 で実行されるアクションを作成することも可能です。 kind `python-jessie:3` を指定して Python 3 ランタイムを使用することもできます。これには、IBM Cloud Services 用の追加パッケージ ({{site.data.keyword.cloudant_short_notm}}、{{site.data.keyword.Db2_on_Cloud_long_notm}}、{{site.data.keyword.cos_full_notm}}、{{site.data.keyword.ibmwatson_notm}} など) が含まれています。この Python 3 ランタイムに含まれているパッケージについて詳しくは、Python ランタイムの[リファレンス](./openwhisk_reference.html#openwhisk_ref_python_environments)を参照してください。

3. アクションを呼び出します。
    ```
    ibmcloud fn action invoke --result helloPython --param name World
    ```
    {: pre}

    出力例:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### zip ファイルへの Python アクションのパッケージ化
{: #openwhisk_actions_python_zip}

Python アクションおよび依存モジュールを zip ファイルでパッケージ化することができます。 例えば、`helper.py` という名前のヘルパー・モジュールを使用したアクションを作成するには、以下のようにします。

1. ソース・ファイルを含めたアーカイブを作成します。**注**: エントリー・ポイントを含むソース・ファイルは `__main__.py` という名前でなければなりません。
    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. アクションを作成します。
    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

### zip ファイルへの、仮想環境を使用した Python アクションのパッケージ化
{: #openwhisk_actions_python_virtualenv}

Python の依存関係は、仮想環境 `virtualenv` を使用してパッケージ化できます。この仮想環境を使用すれば、例えば、[`pip` ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン ")](https://packaging.python.org/installing/) を使用してインストールできる追加のパッケージをリンクすることができます。

依存関係をインストールするには、以下のように、その依存関係を仮想環境でパッケージ化し、互換性のある OpenWhisk アクションを作成します。

1. インストールする `pip` モジュールおよびバージョンを含む [requirements.txt ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) ファイルを作成します。

2. 依存関係をインストールし、仮想環境を作成します。仮想環境ディレクトリーの名前は `virtualenv` でなければなりません。OpenWhisk ランタイム・コンテナーとの互換性を確保するには、仮想環境内でパッケージをインストールする際、その kind に対応するイメージを使用する必要があります。
    - kind `python:2` の場合、docker イメージ `openwhisk/python2action` を使用します。
    - kind `python:3` の場合、docker イメージ `openwhisk/python3action` を使用します。
    - kind `python-jessie:3` の場合、docker イメージ `ibmfunctions/action-python-v3` を使用します。
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp &&virtualenv virtualenv &&source virtualenv/bin/activate &&pip install -r requirements.txt"
    ```
    {: pre}

3. `virtualenv` ディレクトリーと追加の Python ファイルをパッケージ化します。エントリー・ポイントを含むソース・ファイルは `__main__.py` という名前でなければなりません。
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. アクション `helloPython` を作成します。
    ```
    ibmcloud fn action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}

`requirements.txt` には、選択したランタイム環境にないモジュールのみを追加します。そうすると、`virtualenv` のサイズを最低限に抑えられます。
{: tip}

## PHP アクションの作成
{: #creating-php-actions}

以下のセクションでは、単一 PHP アクションの作成と呼び出し、およびそのアクションへのパラメーターの追加について説明します。

### PHP アクションの作成と呼び出し
{: #openwhisk_actions_php_invoke}

アクションは、単にトップレベルの PHP 関数です。 PHP のアクションを作成するには以下のようにします。

1. 以下のコードを `hello.php` という名前のファイルに保存します。
    ```
    <?php
function main(array $args) : array
{
        $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
    ```
    {: codeblock}

    * PHP アクションは常に連想配列を取り込み、連想配列を返します。
    * このアクションの入り口メソッドはデフォルトで `main` になっていますが、`ibmcloud fn` CLI を使用してアクションを作成する際、`--main` フラグを指定して明示的に入り口メソッドを指定することができます。

2. `helloPHP` というアクションを作成します。
    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    CLI は、ソース・ファイルの拡張子からアクションのタイプを自動的に推定します。 `.php` ソース・ファイルの場合、アクションは PHP 7.1 ランタイムを使用して実行されます。 詳しくは、PHP の[リファレンス](./openwhisk_reference.html#openwhisk_ref_php)を参照してください。

3. アクションを呼び出します。
    ```
    ibmcloud fn action invoke --result helloPHP --param name World
    ```
    {: pre}

    出力例:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### zip ファイルへの PHP アクションのパッケージ化
{: #openwhisk_actions_php_zip}

PHP アクション、およびその他のファイルや依存パッケージを zip ファイルにパッケージ化することができます。例えばアクションを、さらに `helper.php` というファイルも加えてパッケージ化するには、以下のようにします。

1. ソース・ファイルを含めたアーカイブを作成します。**注**: エントリー・ポイントを含むソース・ファイルは `index.php` という名前でなければなりません。
    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. アクションを作成します。
    ```bash
    ibmcloud fn action create helloPHP --kind php:7.1 helloPHP.zip
    ```
    {: pre}

## Swift アクションの作成
{: #creating-swift-actions}

The following sections guide you through creating and invoking a single Swift action and packaging an action in a zip file.

**注:** Swift アクションは Linux 環境で実行されます。 Linux 上の Swift はまだ開発中なので、{{site.data.keyword.openwhisk_short}} では最新の使用可能なリリースが使用されています。これらのリリースは安定していないかもしれません。{{site.data.keyword.openwhisk_short}} で使用されている Swift のバージョンは、MacOS 上の Xcode の安定リリースに含まれている Swift のバージョンとは整合性がない場合があります。

Swift のランタイムについて詳しくは、Swift [「リファレンス (reference)」](./openwhisk_reference.html#swift-actions)を参照してください。
{: tip}

### アクションの作成と呼び出し
{: #openwhisk_actions_swift_invoke}

#### Swift 3
{: #openwhisk_actions_swift3_invoke}

アクションは、単にトップレベルの Swift 関数です。 Swift 3 のアクションを作成するには、以下のようにします。

1. 以下のコードを `hello.swift` という名前のファイルに保存します。
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

    この例では、Swift アクションはディクショナリーを取り込み、ディクショナリーを生成します。

2. `helloSwift` というアクションを作成します。
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

    CLI は、ソース・ファイルの拡張子からアクションのタイプを自動的に推定します。 `.php` ソース・ファイルの場合、アクションは PHP 7.1 ランタイムを使用して実行されます。 詳しくは、PHP の[リファレンス](./openwhisk_reference.html#openwhisk_ref_php)を参照してください。

3. アクションを呼び出します。
    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    出力例:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

#### Swift 4
{: #openwhisk_actions_swift4_invoke}

main 関数のシグニチャーに加えて、Swift 4 では、さらに [Codable ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.apple.com/documentation/swift/codable) タイプを利用したシグニチャーを 2 つ用意しています。JSON などの外部表現との互換性を保つために使われるデータ型 encodable および decodable について詳しくは、[ここ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types)を参照してください。

1. 以下のコードを `hello.swift` という名前のファイルに保存します。
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

2. `helloSwift` というアクションを作成します。
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.1
    ```
    {: pre}

3. アクションを呼び出します。
    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    出力例:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Swift 実行可能ファイルとしてのアクションのパッケージ化
{: #packaging-an-action-as-a-swift-executable}

{{site.data.keyword.openwhisk_short}} Swift アクションを Swift のソース・ファイルを使用して作成する際は、アクションの実行前にそのファイルをコンパイルしてバイナリーにしておく必要があります。この遅延は、コールド・スタートの遅延と呼ばれます。 バイナリーが作成されると、アクションを保持しているコンテナーがパージされるまで、以降のそのアクションの呼び出しは大幅に高速になります。コールド・スタートの遅延を回避するため、Swift ファイルをコンパイルしてバイナリーにし、そのバイナリーを zip ファイルにして {{site.data.keyword.openwhisk_short}} にアップロードできます。

スクリプトを使用して、アクションのパッケージ化を自動化できます。

**前提条件**: 以下のステップで使用されるスクリプトは、`actions` という名前のディレクトリーがあり、各最上位ディレクトリーはアクションを表していることを前提としています。
```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```

1. 以下のコードを `compile.sh` という名前のスクリプト・ファイルに保存します。
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
      RUNTIME="ibmfunctions/action-swift-v4.1"
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

2. 依存関係を追加するため、`Package.swift` ファイルを作成します。以下の例では、依存関係として `example-package-deckofplayingcards` を追加しています。`CCurl`、`Kitura-net`、および `SwiftyJSON` は標準の Swift アクションに用意されているので、Swift 3 アクションの場合のみ、それらを `Package.swift` に組み込んでください。
    * Swift 3 の構文例:
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

    * Swift 4 の構文例:
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

3. `build` に `hello.zip` を作成するため、アクションをビルドします。
    * Swift 3:
      ```
      bash compile.sh hello swift:3.1.1
      ```
      {: pre}

    * Swift 4:
      ```
      bash compile.sh hello swift:4.1
      ```
      {: pre}

4. zip を `helloSwiftly` というアクション名で {{site.data.keyword.openwhisk_short}} にアップロードします。
    * Swift 3:
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:3.1.1
      ```
      {: pre}

    * Swift 4:
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:4.1
      ```
      {: pre}

5. アクションを呼び出します。
    ```
    ibmcloud fn action invoke helloSwiftly --blocking
    ```
    {: pre}

    アクションの実行にかかった時間は「duration1」プロパティーにあります。

6. プリコンパイル済みアクションの呼び出しの所要時間を、コンパイル・ステップを使用したコマンド呼び出しの所要時間と比較することができます。以下のようにして、直前のセクションからアクションを呼び出します。
    ```
    ibmcloud fn action invoke --result helloSwift --param name World --blocking
    ```
    {: pre}

### Swift 4 でのエラー処理
{: #error-handling-swift4}

Codable の完了ハンドラーを使用して、アクション内での失敗を示すエラーを渡すことができます。[Swift でのエラー処理 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) は他言語の例外処理に似ており、`try`、`catch`、および `throw` の各キーワードを使用します。

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

## Java アクションの作成
{: #creating-java-actions}

以下のセクションでは、単一 Java アクションの作成と呼び出し、および、そのアクションへのパラメーターの追加について説明します。

Java ファイルをコンパイル、テスト、およびアーカイブするには、[JDK 8 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://openjdk.java.net/install) がローカルにインストールされている必要があります。

### Java アクションの作成と呼び出し
{: #openwhisk_actions_java_invoke}

Java のアクションというのは、`main` という名前のメソッドを持つ Java プログラムのことです。`main` は、以下とまったく同じシグニチャーを持っていなければなりません。
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Java のアクションを作成するには、以下のようにします。

1. 以下のコードを `Hello.java` という名前のファイルに保存します。
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

2. `Hello.java` をコンパイルして `hello.jar` という名前の JAR ファイルを作ります。**注**: Java の CLASSPATH に [google-gson ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/google/gson) が存在している必要があります。
    ```
    javac Hello.java
    ```
    {: pre}
    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}

3. アクションを作成します。
    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}
  * `--main` を使用して、メイン・クラスの名前を指定する必要があります。 適格なメイン・クラスは、静的 `main` メソッドを実装するクラスです。 クラスがデフォルト・パッケージ内にない場合は、完全修飾 Java クラス名を使用してください (例: `--main com.example.MyMain`)。
  * Java のアクションのメソッド名はカスタマイズできます。これを行うには、アクションの完全修飾メソッド名 (例えば、`--main com.example.MyMain#methodName`) を指定します。
  * CLI は、ソース・ファイルの拡張子からアクションのタイプを自動的に推定します。

4. アクションを呼び出します。
    ```
    ibmcloud fn action invoke --result helloJava --param name World
    ```
    {: pre}

    出力例:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Docker アクションの作成
{: #creating-docker-actions}

{{site.data.keyword.openwhisk_short}} Docker アクションでは、任意の言語でアクションを作成できます。
{: shortdesc}

コードはコンパイルされて実行可能バイナリーになり、Docker イメージに組み込まれます。 バイナリー・プログラムとシステムの対話は、`stdin` から入力を受け取り、`stdout` を通して応答することによって行われます。   『[リファレンス](./openwhisk_reference.html#openwhisk_ref_docker)』セクションに、Docker アクションの作成に関する詳細情報があります。

前提条件: ユーザーは Docker Hub アカウントを持っている必要があります。[Docker Hub ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://hub.docker.com) で、無料の Docker ID とアカウントをセットアップしてください。

カスタム・バイナリーをセットアップし、アップロードした Docker イメージをアクションとして使用するには、以下のようにします。

1. Docker スケルトンをダウンロードしてインストールします。このスケルトンは、Docker コンテナー・テンプレートであり、そこにカスタム・バイナリーの形でコードを注入できます。
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. ブラック・ボックス・スケルトン内にカスタム・バイナリーをセットアップします。 スケルトンには C プログラムが含まれているので、それを使用できます。 `example.c` ファイルの部分は Docker イメージのビルド・プロセスの一環でコンパイルされるので、C を自分のマシンでコンパイルする必要はありません。
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  出力例:
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. オプション: 実行可能ファイルをビルドする `Dockerfile` を変更して、さらにコードと依存関係を Docker イメージに追加します。以下の要件と制限に注意してください。
  * そのバイナリーはコンテナー内部の `/action/exec` に置く必要があります。
  * 実行可能ファイルはコマンド・ラインから単一の引数を受け取ります。 この引数は、アクションへの引数を表す JSON オブジェクトのストリング・シリアライゼーションです。
  * プログラムは `stdout` または `stderr` にログを記録することがあります。
  * 規則により、出力の最終行は、アクションの結果を表す、stringify によって文字列化された JSON オブジェクトでなければなりません。

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

5. Docker コンテナーを使用してアクションを作成します。
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. アクションを呼び出します。
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    出力例:
    ```
    {
        "args": {
            "payload": "Rey"
        },
      "msg": "Hello from arbitrary C program!"
  }
    ```
    {: screen}

7. Docker アクションを更新するため、最新のイメージを Docker Hub にアップロードします。これにより、システムは、アクション用のコードの次回実行時に新規 Docker イメージをプルできるようになります。
    ```
    ./buildAndPush.sh <username>/blackboxdemo
    ```
    {: pre}

8. Docker イメージの前のバージョンを使用しているウォーム・コンテナーがあると、新しい呼び出しではすべて、引き続きそのイメージが使用されます。新規の呼び出しが新規イメージを使用し始めるように、アクションを更新します。
    ```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. オプション: `--docker openwhisk/dockerskeleton` の省略表現として、`--native` 引数が使用できます。この引数を使用すると、標準的な Docker アクション SDK 内で実行される実行可能ファイルの作成とデプロイがさらに容易になります。
    1. 上記のステップで、`/action/exec` にあるコンテナー内にバイナリーの実行可能ファイルが作成されます。`/action/exec` ファイルをローカルのファイル・システムにコピーし、それを zip して `exec.zip` を作成します。
    2. 実行可能ファイルを初期化データとして受け取る Docker アクションを作成します。`--docker openwhisk/dockerskeleton` 引数に代わって、`--native` 引数が使用されています。
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}

## Go アクションの作成
{: #creating-go-actions}

`--native` 引数を使用すると、どのような Go 実行可能ファイルでもアクションとしてパッケージ化できます。

以下の要件と制限に注意してください。
  * Go 実行可能ファイルは、コマンド・ラインから単一の引数を受け取ります。この引数は、アクションへの引数を表している JSON オブジェクトのストリング・シリアライゼーションです。
  * プログラムは `stdout` または `stderr` にログを記録することがあります。
  * 規則により、出力の最終行は、アクションの結果を表す、stringify によって文字列化された JSON オブジェクトでなければなりません。

Go のアクションを作成するには、以下のようにします。

1. 以下のコードを `sample.go` という名前のファイルに保存します。
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
    {: codeblock}

2. `sample.go` を {{site.data.keyword.openwhisk_short}} 用にクロス・コンパイルします。実行可能ファイルの名前は `exec` でなければなりません。
    ```bash
    GOOS=linux GOARCH=amd64 go build -o exec
    zip exec.zip exec
    ibmcloud fn action create helloGo --native exec.zip
    ```
    {: codeblock}

3. アクションを呼び出します。
    ```bash
    ibmcloud fn action invoke helloGo -r -p name gopher
    {
        "msg": "Hello, gopher!"
    }
    ```
    {: pre}

## アクション・シーケンスの作成
{: #openwhisk_create_action_sequence}

アクションのシーケンスを一緒にチェーニングしたアクションを作成できます。 1 つのアクションの結果が次のアクションへの引数として渡されます。
{: shortdesc}

`/whisk.system/utils` パッケージには、最初のシーケンスを作成するために使用できるユーティリティー・アクションがいくつか用意されています。

1. `/whisk.system/utils` パッケージに含まれているアクションをリストします。
    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    出力例:
    ```
    package /whisk.system/utils: Building blocks that format and assemble data
   action /whisk.system/utils/head: Extract prefix of an array
   action /whisk.system/utils/split: Split a string into an array
   action /whisk.system/utils/sort: Sorts an array
   action /whisk.system/utils/echo: Returns the input
   action /whisk.system/utils/date: Current date and time
   action /whisk.system/utils/cat: Concatenates input into a string
    ```
    {: screen}

2. `split` アクションと `sort` アクションを使用して、`split` の結果が `sort`への引数として渡されるようにアクション・シーケンスを作成します。このアクション・シーケンスは、数行のテキストを 1 つの配列に変換し、行をソートします。
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. アクションを呼び出します。
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    出力では、分割行がアルファベット順にソートされています。
    ```
    {
        "length": 3,
      "lines": [
            "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
    }
    ```
    {: screen}

**注**:
* シーケンス中のアクション間で渡されるパラメーターは、デフォルト・パラメーターを除いて、明示的です。 したがって、アクション・シーケンスに渡されるパラメーターを使用できるのはシーケンス中の先頭アクションのみです。 シーケンス中の最初のアクションの結果が、シーケンス中の 2 番目のアクションへの入力 JSON オブジェクトになり、以降同様に渡されます。このオブジェクトには、最初にシーケンスに渡されたパラメーターはどれも含まれません (ただし、先頭アクションがその結果にそれらのパラメーターを明示的に組み込んだ場合は除きます)。 あるアクションへの入力パラメーターは、そのアクションのデフォルト・パラメーターとマージされます。その際、入力パラメーターが優先され、一致するデフォルト・パラメーターはオーバーライドされます。 複数の名前付きパラメーターを指定したアクション・シーケンス呼び出しについて詳しくは、[アクションでのデフォルト・パラメーターの設定](./parameters.html#default-params-action)を参照してください。
* シーケンスには、シーケンス内の各アクションのタイムアウトとは別の、全体的なタイムアウトはありません。シーケンスは操作のパイプラインなので、1 つのアクションで失敗すると、パイプラインが中断します。1 つのアクションでタイムアウトが発生すると、その失敗でシーケンス全体が終了します。

## 大規模アクションの管理
{: #large-app-support}

アクションの最大コード・サイズは 48 MB です。 多くのサード・パーティーのモジュール、ネイティブ・ライブラリー、外部ツールが含まれたアプリケーションでは、この制限に達する可能性があります。48 MB より大きい .zip パッケージまたは .jar パッケージのアクションを作成する場合は、依存関係を含めてランタイム・イメージを拡張し、それから、単一のソース・ファイルか 48MB より小さなアーカイブを使用します。

例えば、必要な共有ライブラリーが含まれているカスタム Docker ランタイムをビルドすれば、依存関係はアーカイブ・ファイルに存在する必要がありません。プライベート・ソース・ファイルは、引き続き、アーカイブにバンドルし、実行時に注入できます。

### アクションのサイズ削減
{: #large-app-reduce}

Python アプリケーションのコード・サイズを削減するには、以下のようにします。

1. ライブラリー `opencv-python` を `opencv` に入れます。
2. opencv バイナリーを OS イメージにインストールします。
3. `pip install requirements.txt` を実行して、より多くの Python ライブラリーを使用してこのイメージを拡張します。
4. 新しいイメージで `action.py` を使用します。

Node.js アプリケーションのコード・サイズを削減するには、以下のようにします。

1. `opencv` をインストールします。
   ```
   npm install opencv
   ```
   {: pre}

2. `package.json` をインストールします。
   ```
   npm install package.json
   ```
   {: pre}

3. 新しいイメージで `action.js` を使用します。
