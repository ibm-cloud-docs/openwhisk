---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: web actions, serverless, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Web アクションの作成
{: #actions_web}

Web アクションの作成結果は、任意の Web アプリからのアクションのトリガーとして使用できる URL になります。
{: shortdesc}

## 標準のアクションの代わりに Web アクションを使用する理由

### 1. Web アクションは匿名で実行される

Web アクションのアクティベーションは、アクションの呼び出し元ではなく、アクションを作成したユーザーと関連付けられます。通常は、Github などのアプリに対する API 呼び出しの場合、特定のユーザーまたは機能 ID に対する API 呼び出しにユーザー名とトークンを組み込みます。Web アクションを使用する場合、この種の資格情報は必要ありません。資格情報を必要とせずに、REST インターフェース経由で Web アクションにアクセスできます。

Web アクションで資格情報を使用する必要はありませんが、独自の認証と許可や OAuth フローを実装できます。資格情報を伴う Web アクションを構成するには、『[Web アクションの保護](#actions_web_secure)』を参照してください。

### 2. 不特定型の HTTP 要求を使用する

デフォルトでは、アクションは `POST` 要求のみ受け入れますが、Web アクションは HTTP メソッド (`GET`、`POST`、`PUT`、`PATCH`、`DELETE`、ならびに `HEAD` および `OPTIONS`) のどれを通しても起動できます。

### 3. Web アクションはどこからでもトリガーできる

{{site.data.keyword.openwhisk}} Web アクションを作成する際には、Web ベースのアプリからこのアクションを呼び出す URL を生成します。Web アクションでないアクションは、認証を必要とし、JSON オブジェクトで応答することも必要です。 

Web アクションの API パスは cURL または `wget` で使用でき、ブラウザーに直接入力することもできます。Web アクションは、`https://<apihost>/api/v1/web/<namespace>/<packageName>/<actionName>.<ext>` と構造化された URL を使用して呼び出すことができます。

### 4. 作成する {{site.data.keyword.openwhisk_short}} エンティティーの数が減る

Web アクションはどこからでも呼び出すことができるため、トリガーやルールなどのその他の {{site.data.keyword.openwhisk_short}} エンティティーを作成する必要はありません。

## Web アクションはどのように機能しますか?

Web アクションは認証なしで起動でき、さまざまなタイプの `headers`、`statusCode`、および `body` コンテンツで応答する HTTP ハンドラーを実装するために使用できます。

Web アクションは JSON オブジェクトを返す必要があります。 ただし、コントローラーは、結果に以下の 1 つ以上が最上位 [JSON プロパティー](#web_action_properties)として含まれる場合、Web アクションを別の方法で処理します。
{: shortdesc}

## Web アクションの使用可能なフィーチャー
{: #actions_web_extra}

Web アクションは、以下のフィーチャーをサポートしています。

| フィーチャー | 説明 |
| --- | --- |
| [コンテンツ拡張子](#extra_features) | `.json`、`.html`、`.http`、`.svg`、`.text` などの、HTTP 要求のコンテンツ・タイプを指定できます。コンテンツ・タイプを指定しない場合は `.http` 拡張子が想定されます。コンテンツ・タイプの指定は、URI 内のアクション名に拡張子を追加することで行います。これによって、アクション `demo/hello` は `/demo/hello.svg` として参照されます。射影パスは、`.json` と `.http` の拡張子には必要ありませんが、`.html`、`.svg`、`.text` の拡張子には必要です。 デフォルトのパスは、拡張子名と一致すると想定されます。 Web アクションを起動して、`.html` 応答を受け取るには、アクションは、`html` という名前の最上位プロパティーを含む JSON オブジェクトを用いて応答する必要があります (または、応答は明示的パスに存在する必要があります)。 言い換えると、`/<namespace>/demo/hello.html` は、`/<namespace>/demo/hello.html/html` のように、`html` プロパティーを明示的に射影することと等価です。 アクションの完全修飾名には、そのパッケージ名が含まれている必要があります。このパッケージ名は、アクションが名前付きパッケージ内にない場合は `default` になります。 |
| [結果からのフィールドの射影](#projecting_fields) | アクション名に続くパスを使用して、応答の 1 つ以上のレベルを射影します。 例えば、`/demo/hello.html/body` のようにします。このフィーチャーにより、`{body: "..." }` などのディクショナリーを返す Web アクションは `body` プロパティーを射影して、そのディクショナリー値の代わりにストリング値を直接戻すことができます。射影パスは、絶対パス・モデル (例: XPath) に従います。 |
| [入力としての照会および本体のパラメーター](#query_test) | アクションは、照会パラメーターと要求本体のパラメーターを受け取ります。 パラメーターをマージするときの優先順位は、パッケージ・パラメーター、アクション・パラメーター、照会パラメーター、および本体パラメーターです。 これらの各パラメーターは、オーバーラップが発生すると、前の値をオーバーライドできます。 例えば、`/demo/hello.http?name=Jane` は、引数 `{name: "Jane"}` をアクションに渡します。 |
| [フォーム・データ](#form_data) | 標準の `application/json` に加えて、Web アクションは、URL エンコードのフォーム・データ `application/x-www-form-urlencoded data` を入力として受け取ることができます。
| [複数の HTTP 動詞を使用するアクティベーション](#actions_web_options) | Web アクションは、HTTP メソッド (`GET`、`POST`、`PUT`、`PATCH`、および `DELETE`、ならびに `HEAD` および `OPTIONS`) のどれを通しても起動できます。 |
| [非 JSON 本体および未加工 HTTP エンティティーの処理](#actions_web_raw_enable) | Web アクションは、JSON オブジェクト以外の HTTP 要求本体を受け入れることができ、不透明値のような値 (バイナリー・ファイルではない場合はプレーン・テキスト、バイナリーの場合は base64 エンコード・ストリング)を常に受け取ることを選択できます。 |

## Web アクションの作成
{: #actions_web_example}

Web アクションを作成するには以下のようにします。 

1. 以下の JavaScript コードを `hello.js` として保存します。

  ```javascript
  function main({name}) {
    var msg = 'You did not tell me who you are.';
  if (name) {
      msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
  }
  ```
  {: codeblock}

2. `demo` パッケージを作成します。パッケージ名は、明示的に指定しなければ `default` です。
  ```
  ibmcloud fn package create demo
  ```
  {: pre}

3. `hello` アクションを作成します。 この例では、`packageName/actionName` は `demo/hello` です。`<filepath>` 変数を `hello.js` ファイルのファイル・パスに置き換え、`--web` フラグを `true`に設定します。 

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

4. パラメーターを指定せずに Web アクション `hello` を呼び出すかテストします。`<apihost>` 変数と `<namespace>` 変数を置き換えます。`<apihost>` を取得するには、`ibmcloud fn property get--apihost` を実行します。`<apihost>` の例: `us-south.functions.cloud.ibm.com`。

  IAM 対応の名前空間の場合、`<namespace>` 変数を名前空間 ID に置き換えます。この ID を取得するには、`ibmcloud fn namespace get <namespace_name>` を実行します。
  {: note}

  a. 以下のいずれかの方法で Web アクションをテストできます。 
    * ブラウザーで、`https://<apihost>/api/v1/web/<namespace>/demo/hello` という構造を使用して URL を開きます。
    * cURL コマンドを使用してアクションをテストします。
      ```
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

    * `wget` コマンドを使用してアクションをテストします。  
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

  b. アクション・コードは、以下のディクショナリーを返します。
    ```
    {body: `<html><body><h3>${msg}</h3></body></html>`}
    ``` 
    {: screen}
    
  以下のコマンドを使用して `body` プロパティーのみを返すという方法で、アクションをテストすることもできます。
  {: #projecting_fields}

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello.html/body
    ```
    {: pre}

    **出力例**

    `<name>` パラメーターが指定されていないので、以下のメッセージが返されます。
    ```
    <html><body><h3>You did not tell me who you are.</h3></body></html>
    ```
    {: screen}

5. 次に、`<name>` パラメーターの定義を試みます。以下のいずれかの方法で、`<name>` パラメーターを指定してアクションをテストします。
  * ブラウザーで `https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane` を開きます。 
  * cURL コマンドを使用してアクションをテストします。

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}
  * `wget` コマンドを使用してアクションをテストします。  
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}

  **出力例**
  ```
  <html><body><h3>Hello, Jane!</h3></body></html>
  ```
  {: screen}


**次のステップ**

Web アクション `hello` の URL を Web アプリに追加してテストします。

### Web アクションの JSON プロパティー
{: #web_action_properties}

HTTP 応答のデフォルトの `Content-Type` は `application/json` で、本体は任意の使用可能な JSON 値にすることができます。`Content-Type` が `application/json` でない場合は、アクション・コードの `headers` 内に `Content-Type` を指定する必要があります。

アクションの[結果のサイズの限度](/docs/openwhisk?topic=cloud-functions-limits)に達すると、応答は失敗します。アクションの結果が 5 MB より大きいことが判明している場合は、[オブジェクト・ストア](/docs/openwhisk?topic=cloud-functions-pkg_obstorage)をセットアップします。

| JSON プロパティー | 説明 |
| --- | --- |
| `headers`| キーがヘッダー名であり、値がストリング値、数値、またはブール値である JSON オブジェクト。 1 つのヘッダーで複数の値を送信する場合、ヘッダーの値は、複数の値の JSON 配列になります。 デフォルトで設定されるヘッダーはありません。 |
| `statusCode` | 有効な HTTP 状況コード。 本体コンテンツが存在する場合、デフォルトは `200 OK` です。 本体コンテンツが存在しない場合、デフォルトは `204 No Content` です。 |
| `body` | プレーン・テキスト、JSON オブジェクトや配列、または Base64 エンコードのストリング (バイナリー・データの場合) のいずれかであるストリング。 body が `null`、空ストリング `""`、または未定義の場合、本体は空であると見なされます。 デフォルトは空の本体です。 |

アクションで指定されたヘッダー、状況コード、または本体があれば、[コントローラー](/docs/openwhisk?topic=cloud-functions-about#about_controller)はそれを、要求や応答を終了する HTTP クライアントに渡します。 アクション結果の `headers` で `Content-Type` ヘッダーが宣言されていない場合、本体は、非ストリング値であれば `application/json`、その他の場合は `text/html` として解釈されます。 `Content-Type` ヘッダーが定義されている場合、コントローラーは応答がバイナリー・データなのかプレーン・テキストなのかを判別し、必要に応じて base64 デコーダーを使用してストリングをデコードします。 本体が正しくデコードされていない場合は、クライアントにエラーが返されます。

Web アクションの所有者が、すべてのアクティベーション・レコードを所有し、アクションがどのように呼び出されたのかに関係なくシステムでのアクションの実行のコストを負担します。
{: note}

#### 保護されたパラメーター
アクション・パラメーターは保護されており、変更する方法はアクションを更新すること以外にはありません。Web アクションを有効にするために、パラメーターは自動的にファイナライズされます。

```
ibmcloud fn action create /<namespace>/demo/hello hello.js --parameter name Jane --web true
```
{: pre}


これらの変更の結果、`name` が `Jane` にバインドされ、final アノテーションがあるため照会パラメーターまたは本体パラメーターでオーバーライドすることはできません。 この設計は、意図的または偶発的にこの値を変更しようとする照会パラメーターまたは本体パラメーターからアクションを保護します。

### Web アクションを使用した HTTP リダイレクトの実行
{: #http_redirect}
Web アプリケーションでこのフィーチャーを使用して、ユーザーを新しいバージョンのサイトにリダイレクトすることもできます。

**始める前に** [Web アクションの作成](#actions_web_example)のステップをすべて実行して、`demo` パッケージと Web アクション `hello` を作成します。

HTTP リダイレクトを実行する Web アクションを作成するには、以下のようにします。

1. コードを `hello.js` として保存します。

  ```javascript
  function main() {
    return {
      headers: { location: 'https://cloud.ibm.com/openwhisk/' },
      statusCode: 302
    }
  }
  ```
  {: codeblock}

2. 新しいバージョンの `hello.js` コードで Web アクション `hello` を更新します。`<filepath>` を `hello.js` ファイルのファイル・パスに置き換えます。

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Web アクション `hello` をテストします。`<apihost>` 変数と `<namespace>` 変数を置き換えます。以下のいずれかの方法で Web アクションをテストできます。

  * ブラウザーで URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` を開きます。 
  * 以下の cURL コマンドを実行します。
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * 以下の `wget` コマンドを実行します。
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

  **結果の例** 
  
  この Web アクションの例では、ブラウザーを [{{site.data.keyword.openwhisk_short}} のダッシュボード](https://cloud.ibm.com/openwhisk/){: external}にリダイレクトします。

### Web アクションを使用した Cookie の設定
{: #multiple_cookie}
Web アプリケーションでこのフィーチャーを使用して、ログイン成功後に JSON Web トークンをセッション Cookie として保管することもできます。

複数の Cookie を設定する Web アクションを作成するには、以下のようにします。

**始める前に** [Web アクションの作成](#actions_web_example)のステップをすべて実行して、`demo` パッケージと Web アクション `hello` を作成します。

1. コードを `hello.js` として保存します。
  ```javascript
  function main() {
    return {
      headers: {
        'Set-Cookie': [
          'UserID=Jane; Max-Age=3600; Version=',
        'SessionID=asdfgh123456; Path = /'
        ],
      'Content-Type': 'text/html'
      },
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
  }
  ```
  {: codeblock}

2. 新しいバージョンの `hello.js` コードで Web アクション `hello` を更新します。`<filepath>` を `hello.js` ファイルのファイル・パスに置き換えます。

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}
    
3. アクションをテストする前に、ブラウザーの Cookie を消去します。

4. ブラウザーで URL を開いて、Web アクション `hello` をテストします。`<apihost>` 変数と `<namespace>` 変数を置き換え、`https://<apihost>/api/v1/web/<namespace>/demo/hello`を開きます。`<apihost>` の例: `us-south.functions.cloud.ibm.com`。

**結果
**

ブラウザーの開発者用ツールで Cookie `UserID=Jane` と `SessionID=asdfgh123456` が設定されます。


### Web アクションを使用してイメージを返す
{: #return_image}
Web アプリケーションでこのフィーチャーを使用して、ユーザー・ロケールに基づいて国旗のイメージを返すこともできます。

**始める前に** 

[Web アクションの作成](#actions_web_example)のステップをすべて実行して、`demo` パッケージと Web アクション `hello` を作成します。

`image/png` を返す Web アクションを作成するには、以下のようにします。 

1. コードを `hello.js` として保存します。

  ```javascript
  function main() {
      let png = '<base 64 encoded string';
      return { headers: { 'Content-Type': 'image/png' },
              statusCode: 200,
              body: png };
  }
  ```
  {: codeblock}

2. 新しいバージョンの `hello.js` コードで Web アクション `hello` を更新します。`<filepath>` を `hello.js` ファイルのファイル・パスに置き換えます。

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. ブラウザーでアクションをテストするか、cURL コマンドを使用してアクションをテストします。`<apihost>` 変数と `<namespace>` 変数を置き換えます。以下のいずれかの方法で Web アクションをテストできます。

  * ブラウザーで URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` を開きます。 
  * 以下の cURL コマンドを実行します。
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * 以下の `wget` コマンドを実行します。
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}



### Web アクションを使用して JSON を返す
{: #return_json}
Web アプリケーションでこのフィーチャーを使用して、ユーザー IP 情報の JSON オブジェクトを返すこともできます。

**始める前に** 

[Web アクションの作成](#actions_web_example)のステップをすべて実行して、`demo` パッケージと Web アクション `hello` を作成します。

`application/json` を返す Web アクションを作成するには、以下のようにします。

1. コードを `hello.js` として保存します。
  ```javascript
  function main(params) {
      return {
          statusCode: 200,
          headers: { 'Content-Type': 'application/json' },
          body: params
      };
  }
  ```
  {: codeblock}

2. 新しいバージョンの `hello.js` コードで Web アクション `hello` を更新します。`<filepath>` を `hello.js` ファイルのファイル・パスに置き換えます。
  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. ブラウザーでアクションをテストするか、cURL コマンドを使用してアクションをテストします。`<apihost>` 変数と `<namespace>` 変数を置き換えます。以下のいずれかの方法で Web アクションをテストできます。
  * ブラウザーで URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` を開きます。 
  * 以下の cURL コマンドを実行します。
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * 以下の `wget` コマンドを実行します。
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

    **出力例**

    ```
    {
      "__ow_headers": {
        "accept": "*/*",
        "accept-encoding": "gzip",
        "cdn-loop": "cloudflare",
        "cf-connecting-ip": "XX.XXX.XXX.XXX",
        "cf-ipcountry": "US",
        "cf-ray": "4d9f3e442a86cf28-IAD",
        "cf-visitor": "{\"scheme\":\"https\"}",
        "host": "<apihost>",
        "user-agent": "curl/7.54.0",
        "x-forwarded-for": "XX.XXX.XX.XXX, XX.XXX.XX.XXX",
        "x-forwarded-host": "<apihost>",
        "x-forwarded-port": "443",
        "x-forwarded-proto": "https",
        "x-global-k8fdic-transaction-id": "11fd03071bd0841d3a00f52354ab880f",
        "x-real-ip": "XXX.XX.XX.XX",
        "x-request-id": "11fd03071bd0841d3a00f52354ab880f"
      },
      "__ow_method": "get",
      "__ow_path": ""
    }
    ```
    {: screen}


### HTTP コンテキスト
{: #actions_web_context}

すべての Web アクションは、起動されると、アクション引数への入力パラメーターとして、HTTP 要求の詳細を受け取ります。

| HTTP パラメーター | タイプ | 説明 |
| --- | --- | --- |
| `__ow_method` | ストリング | 要求の HTTP メソッド。 |
| `__ow_headers` | 文字列間のマップ | 要求ヘッダー。|
| `__ow_path` | ストリング | マッチングされていない要求パス (マッチングは、アクションの拡張子が取り込まれると停止します)。 |
| `__ow_user` | ストリング | {{site.data.keyword.openwhisk_short}} 認証済みサブジェクトを識別する名前空間。 |
| `__ow_body` | ストリング | コンテンツがバイナリー・ファイルの場合は base64 エンコード・ストリング、それ以外の場合はプレーン・ストリングの要求本体エンティティー。 |
| `__ow_query` | ストリング | 構文解析されていないストリングである、要求からの照会パラメーター。 |

要求は、指定された `__ow_` パラメーターのいずれもオーバーライドできません。 これを行うと、要求は失敗し、状況は 400 の Bad Request になります。

`__ow_user` は、Web アクションが[認証を必要とするものとしてアノテーションが付けられている](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions)場合にのみ存在し、これにより、Web アクションは独自の許可ポリシーを実装することができます。 `__ow_query` は、Web アクションが[「未加工」 HTTP 要求 ](#actions_web_raw_enable)を処理することを選択した場合にのみ使用可能です。 `__ow_query` は、URI から解析される照会パラメーターを含むストリングです (`&` で分離)。`__ow_body` プロパティーは、未加工 HTTP 要求の中、または HTTP 要求エンティティーが JSON オブジェクトでも形式データでもない場合に存在します。 それ以外の場合、Web アクションは、アクション引数内の第 1 クラス・プロパティーとして、照会および本体のパラメーターを受け取ります。 本体パラメーターは照会パラメーターより優先され、照会パラメーターはアクション・パラメーターおよびパッケージ・パラメーターより優先されます。

### HTTPS エンドポイント・サポート
{: #actions_web_endpoint}

サポートされる SSL プロトコル: TLS 1.2、TLS 1.3 ([ドラフト・バージョン 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18){: external})

### Web アクションの応答コンテンツの変更
{: #extra_features}
[コンテンツ拡張子](#actions_web_extra)を使用して、さまざまなコンテンツ・タイプを返すように Web アクションの応答コンテンツを変更できます。
{: shortdesc}

**始める前に**

[Web アクションの作成](#actions_web_example)のステップをすべて実行して、`demo` パッケージと Web アクション `hello` を作成します。

Web アクションの応答を変更するには以下のようにします。

1. 以下のコードを `hello.js` として保存します。

  ```javascript
  function main(params) {
      return { response: params };
}
  ```
  {: codeblock}

2. 新しいバージョンの `hello.js` コードを使用して、Web アクション `hello` を更新します。`<filepath>` を `hello.js` ファイルのファイル・パスに置き換えます。

  ```bash
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. ブラウザーでアクションをテストするか、cURL コマンドを使用してアクションをテストします。`<apihost>` 変数と `<namespace>` 変数を置き換えます。

  a. 以下のいずれかの方法で JSON を返します。
    * Web ブラウザーで `https://<apihost>/api/v1/web/<namespace>/demo/hello.json` を開きます。 
    * 以下の cURL コマンドを実行します。
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}
    * 以下の `wget` コマンドを実行します。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}

      **出力例**

      ```
      {
        "response": {
          "__ow_method": "get",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  b. 照会パラメーターを使用して、アクションをテストします。以下のいずれかの方法でアクションをテストできます。
  {: #query_test}

    * 以下の cURL コマンドを実行します。

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

    * 以下の `wget` コマンドを実行します。

        ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

      **出力例**
      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "get",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  c. フォーム・データを使用して Web アクションをテストすることもできます。以下のいずれかの方法で Web アクションをテストできます。
  {: #form_data}
  
    * 以下の cURL コマンドを実行します。

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}
      
    * 以下の `wget` コマンドを実行します。
        ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}

      **出力例**

      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "content-length": "10",
            "content-type": "application/x-www-form-urlencoded",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  d. 以下のコマンドを実行して、JSON オブジェクトを指定できます。以下のいずれかの方法で Web アクションをテストできます。
    * 以下の cURL コマンドを実行します。
        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}
      
    * 以下の `wget` コマンドを実行します。
        ```
        wget https://<apihost>/api/v1/web/{namespace/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}

      **出力例**

      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "content-length": "15",
            "content-type": "application/json",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  e. 以下のいずれかの方法で、`name` 値をテキストとして返すこともできます。
  * 以下の cURL コマンドを実行します。

      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}
  * 以下の `wget` コマンドを実行します。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}

    **出力例**

    ```
    Jane
    ```
    {: screen}

    標準のアクションでは、照会パラメーター、フォーム・データ、JSON オブジェクト本体エンティティーはすべてディクショナリーとして扱われ、それぞれの値は、アクション入力プロパティーとして直接アクセス可能です。HTTP 要求エンティティーを処理する Web アクションの場合、あるいは Web アクションが JSON オブジェクトではないエンティティーを受け取る場合には、この動作は当てはまりません。
    {: note}

  f. 以下のいずれかの方法で `Content-Type` を設定できます。
  * 以下の cURL コマンドを実行します。  
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}
    
  * 以下の `wget` コマンドを実行します。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}

    **出力例**

    ```
    {
      "response": {
        "__ow_method": "post",
    "__ow_headers": {
          "accept": "*/*",
          "connection": "close",
          "content-length": "4",
          "content-type": "text/plain",
          "host": "172.17.0.1",
          "user-agent": "curl/7.43.0"
        },
        "__ow_path": "",
        "__ow_body": "Jane"
      }
    }
    ```
    {: screen}

## Web アクションの保護
{: #actions_web_secure}

**始める前に** [Web アクションの作成](#actions_web_example)のステップをすべて実行して、`demo` パッケージと Web アクション `hello` を作成します。

デフォルトでは、誰でも呼び出し URL を使用して Web アクションを呼び出すことができます。以下のいずれかの方法で、[Web アクションのアノテーション](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions) `require-whisk-auth` を使用して Web アクションを保護できます。
  1. `require-whisk-auth` アノテーションを `true` に設定します。`require-whisk-auth` アノテーションが `true` に設定されている場合、Web アクションは、呼び出し要求の基本許可資格情報を Web アクション所有者の whisk 認証キーに照らして認証します。 数字または大/小文字の区別があるストリングに設定されている場合、Web アクションの呼び出し要求には、この同じ数字または大/小文字の区別があるストリングに設定された `X-Require-Whisk-Auth` ヘッダーが含まれていなければなりません。保護された Web アクションは、資格情報の検証が失敗した場合はメッセージ `Not Authorized` を返します。

  2. `--web-secure` フラグを使用して、`require-whisk-auth` アノテーションを自動的に設定できるようにします。`--web-secure` フラグを `true` に設定すると、`require-whisk-auth` アノテーション値として乱数が生成されます。`false` に設定すると、`require-whisk-auth` アノテーションは削除されます。  その他の値に設定すると、その値が `require-whisk-auth` アノテーション値として使用されます。

セキュア Web アクションをテストするには、以下のようにします。

1. 以下の JavaScript コードを `hello.js` として保存します。
  ```javascript
  function main({name}) {
    var msg = 'You did not tell me who you are.';
  if (name) {
      msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
  }
  ```
  {: codeblock}

2. 新しいバージョンの `hello.js` コードを使用して Web アクション `hello` を更新し、`--web secure` フラグを `true` に設定します。
  ```bash
  ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true
  ```
  {: pre}

3. ランダムに生成された `require-whisk-auth` 値を表示する Web アクション `hello` を取得します。

  ```bash
  ibmcloud fn action get demo/hello
  ```
  {: pre}

    **出力例**

    `require-whisk-auth` 値は `7819991076995522` に設定されました。
    ```
    {
      "namespace": "<namespace>/demo",
      "name": "hello",
      "version": "0.0.34",
      "exec": {
          "kind": "nodejs:10",
          "binary": false
      },
      "annotations": [
          {
              "key": "web-export",
              "value": true
          },
          {
              "key": "raw-http",
              "value": false
          },
          {
              "key": "final",
              "value": true
          },
          {
              "key": "require-whisk-auth",
              "value": 7819991076995522
          },
          {
              "key": "exec",
              "value": "nodejs:10"
          }
      ],
    "limits": {
          "timeout": 60000,
          "memory": 256,
          "logs": 10,
          "concurrency": 1
      },
    "publish": false
}
    ```
    {: screen}

認証が作動していることをテストするには、次のようにします。

1. `X-Require-Whisk-Auth` パラメーターを設定せずに Web アクション `hello` をテストして、認証が必須であることを検証します。このテストの結果は、エラーになります。以下のいずれかの方法で Web アクションをテストできます。

  * cURL コマンドを使用して Web アクションをテストします。
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}
    
  * `wget` コマンドを使用して Web アクションをテストします。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}

   **出力例**

    ```
      {
      "code": "4c4423556547f6ac764ee192d4ed27a6",
      "error": "Authentication is possible but has failed or not yet been provided."
    }
    ```
    {: screen}

    `X-Require-Whisk-Auth` 値を設定しなかったので、呼び出しは失敗します。
    {: note}

2. 今度は、ランダムに生成された `X-Require-Whisk-Auth` 値を設定して Web アクション `hello` をテストします。`<apihost>` 値と `<namespace>` 値を置き換えます。`<my-secret>` 値を、ステップ 3 で作成した、ランダムに生成された数値に置き換えます。以下のいずれかの方法で Web アクションをテストできます。
  * cURL コマンドを使用して Web アクションをテストします。
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  * `wget` コマンドを使用して Web アクションをテストします。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  **出力例**
    
    ```
    {
    "body": "<html><body><h3>Hello, Jane!</h3></body></html>"
    }
    ```
    {: screen}

カスタムの `require-whisk-auth` 値を使用して Web アクションをテストするには、以下のようにします。

1. 独自の `require-whisk-auth` 値を使用して Web アクション `hello` を更新します。続いて、呼び出し時に `X-Require-Whisk-Auth` 値を指定して Web アクションのテストを試みます。

  a. `require-whisk-auth` 値を設定します。`<my-secret>` は、大/小文字を区別する認証トークンです。
    ```bash
    ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true --require-whisk-auth <mysecret>
    ```
    {: pre}
  
  b. `<my-secret>` 値を含めて、Web アクションをテストします。以下のいずれかの方法で Web アクションをテストできます。
  * cURL コマンドを使用して Web アクションをテストします。
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}
  * `wget` コマンドを使用してアクションをテストします。
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}


## 未加工 HTTP 処理
{: #actions_web_raw}

Web アクションは、Web アクション入力に使用できる第 1 クラス・プロパティーに JSON オブジェクトをプロモーションせずに、着信 HTTP 本体を直接解釈して処理することを選択できます (例: `args.name` 対 `args.__ow_query` の構文解析)。 この処理は、`raw-http` [アノテーション](/docs/openwhisk?topic=cloud-functions-annotations)を介して行います。 前述の同じ例を使用しますが、今度は「未加工」HTTP Web アクションとして使用し、以下のように、照会パラメーターと、HTTP 要求本体内の JSON 値の両方として `name` を受け取ります。
```bash
curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}


**出力例**
```
{
  "response": {
    "__ow_method": "post",
    "__ow_query": "name=Jane",
    "__ow_body": "eyJuYW1lIjoiSmFuZSJ9",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "15",
      "content-type": "application/json",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```
{: screen}

{{site.data.keyword.openwhisk_short}} は、[Akka HTTP](https://doc.akka.io/docs/akka-http/current/?language=scala){: external} フレームワークを使用して、[どのコンテンツ・タイプがバイナリー・ファイルであり、どれがプレーン・テキストであるかを判別](https://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html){: external}します。

### 未加工 HTTP 処理の有効化
{: #actions_web_raw_enable}

`--web` を `raw` に設定して、未加工 HTTP の Web アクションを作成できます。
```bash
ibmcloud fn action create demo/hello /<filepath>/hello.js --web raw
```
{: pre}

### バイナリーの本体コンテンツを Base64 からデコード
{: #actions_web_decode}

未加工 HTTP コンテンツが処理されるとき、要求の `Content-Type` がバイナリー形式の場合、`__ow_body`コンテンツは、Base64 でエンコードされます。以下の関数は、Node、Python、および Swift で本体コンテンツをデコードする方法を示しています。

1. 優先言語のサンプル・コードを、`decode.<ext>` と呼ばれるファイルに保存します。`<ext>` を、優先言語のサンプル・コードのファイル拡張子に置き換えます。

  **Node**
  {: #actions_web_decode_js}

  ```javascript
  function main(args) {
      decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
      return {body: decoded}
  }
  ```
  {: codeblock}

  **Python**
  {: #actions_web_decode_python}

  ```python
  def main(args):
    try:
        decoded = args['__ow_body'].decode('base64').strip()
        return {"body": decoded}
    except:
        return {"body": "Could not decode body from Base64."}
  ```
  {: codeblock}

  **Swift**
  {: #actions_web_decode_swift}

  ```swift
  extension String {
      func base64Decode() -> String? {
          guard let data = Data(base64Encoded: self) else {
              return nil
          }

          return String(data: data, encoding: .utf8)
      }
  }

func main(args: [String:Any]) -> [String:Any] {
      if let body = args["__ow_body"] as? String {
          if let decoded = body.base64Decode() {
              return [ "body" : decoded ]
        }
      }

      return ["body": "Could not decode body from Base64."]
  }
  ```
  {: codeblock}

2. 以下のコマンドを実行して、サンプル・コードを使用して未加工 HTTP の Web アクションを作成します。この例では、Node 関数は `decode.js` として保管されます。ファイル・パスを `decode` ファイルのファイル・パスに置き換え、使用したサンプル・コードの拡張子と一致するようにファイル拡張子を更新します。

  ```bash
  ibmcloud fn action create decode <filepath>/decode.js --web raw
  ```
  {: pre}

  **出力例**
  ```
  ok: created action decode
  ```
  {: screen}

3. 以下の cURL コマンドを実行して、`decode` アクションをテストします。
    ```bash
    curl -k -H "content-type: application" -X POST -d "Decoded body" https://<apihost>/api/v1/web/<namespace>/default/decode.json
    ```
    {: pre}

  **出力例**
    ```
    {
      "body": "Decoded body"
    }
    ```
    {: screen}

## OPTIONS 要求
{: #actions_web_options}

デフォルトで、Web アクションに対して `OPTIONS` 要求を行うと、自動的に CORS ヘッダーが応答ヘッダーに追加されます。 これらのヘッダーにより、すべてのオリジン、および `OPTIONS`、`GET`、`DELETE`、`POST`、`PUT`、`HEAD`、および `PATCH` の各 HTTP 動詞が可能になります。
{: shortdesc}

以下のヘッダーを参照してください。
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

あるいは、`OPTIONS` 要求は、Web アクションによって手動で処理することもできます。 このオプションを有効にするには、`web-custom-options` アノテーションを `true` の値に設定して Web アクションに追加します。 このフィーチャーが有効になると、CORS ヘッダーは自動的に要求応答に追加されません。 代わりに、ヘッダーをプログラマチックに付加する必要があります。

`OPTIONS` 要求に対するカスタム応答を作成するには、以下のようにします。

1. 以下のコードを `custom-options.js` ファイルに保存します。

  ```js
  function main(params) {
    if (params.__ow_method == "options") {
      return {
        headers: {
          'Access-Control-Allow-Methods': 'OPTIONS, GET',
        'Access-Control-Allow-Origin': 'example.com'
        },
      statusCode: 200
    }
    }
  }
  ```
  {: codeblock}

2. Web アクションを作成します。`--web-custom-options` を `true` に設定します。

  ```bash
  ibmcloud fn action create custom-option <filepath>/custom-options.js --web true -a web-custom-options true
  ```
  {: pre}

3. 以下の cURL コマンドを使用して、アクションをテストします。

  ```bash
  $ curl https://<apihost>/api/v1/web/<namespace>/default/custom-option.http -kvX OPTIONS
  ```
  {: pre}

  **出力例**
  ```
  < HTTP/1.1 200 OK
< Server: nginx/1.11.13
< Content-Length: 0
< Connection: keep-alive
< Access-Control-Allow-Methods: OPTIONS, GET
< Access-Control-Allow-Origin: example.com
  ```
  {: screen}

## エラー処理
{: #actions_web_errors}

{{site.data.keyword.openwhisk_short}} アクションは、2 つの異なる障害モードで失敗します。 1 つは_アプリケーション・エラー_ と呼ばれるものであり、キャッチされた例外に似ています。アクションが返す JSON オブジェクトには、最上位に `error` プロパティーが含まれます。 もう 1 つは _開発者エラー_ であり、これはアクションで障害が起こって、応答が生成されない場合に発生します (キャッチされていない例外に似ています)。 Web アクションの場合、コントローラーは次のようにアプリケーション・エラーを処理します。

- 指定されたパス射影はすべて無視され、コントローラーは代わりに `error` プロパティーを射影します。
- コントローラーはアクションの拡張子で暗黙に示されるコンテンツ処理を `error` プロパティーの値に適用します。

開発者は、Web アクションの使用方法を認識し、適切なエラー応答を生成する必要があります。 例えば、`.http` 拡張子と共に使用される Web アクションは、`{error: { statusCode: 400 }` のような HTTP 応答を返します。 適切にエラーが生成されないと、拡張子で暗黙に示される `Content-Type` と、エラー応答内のアクション `Content-Type` とが一致しません。 Web アクションがシーケンスになっている場合は、シーケンスを形成しているコンポーネントが必要に応じて適切なエラーを生成できるように、特別な考慮が必要です。



## Web アクションの無効化
{: #actions_web_disable}

CLI で `--web` フラグを `false` または `no` に設定して、Web アクションを無効にすることができます。`<packageName>/<actionName>` と `<filepath>/<filename>` を、コード・ファイルのパッケージ名、Web アクション名、ファイル・パス、ファイル名に置き換えます。

```bash
ibmcloud fn action update <packageName>/<actionName> <filepath>/<filename> --web false
```
{: pre}



