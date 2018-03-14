---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Web アクション
{: #openwhisk_webactions}

Web アクションは、迅速に開発者が Web ベースのアプリケーションを構築することを可能にするために、アノテーションが付けられた OpenWhisk アクションです。アノテーションが付けられたこれらのアクションにより、OpenWhisk 認証キーを必要とせずに Web アプリケーションが匿名でアクセスできるバックエンド・ロジックを、開発者がプログラムできます。アクション開発者は、自由に独自の認証および許可 (つまり OAuth フロー) を実装できます。
{: shortdesc}

Web アクションのアクティベーションは、アクションを作成したユーザーと関連付けられます。このアクションでは、アクションのアクティベーションのコストが、呼び出し元からアクションの所有者に移り、委ねられます。

以下に、JavaScript アクション `hello.js` の例を示します。
```javascript
function main({name}) {
  var msg = 'you did not tell me who you are.';
  if (name) {
    msg = `hello ${name}!`
  }
  return {body: `<html><body><h3>${msg}</h3></body></html>`}
}
```
{: codeblock}  

以下のように、CLI の `--web` フラグを値 `true` または `yes` で指定し、_Web アクション_ `hello` を、名前空間 `guest` のパッケージ `demo` 内に作成できます。
```
wsk package create demo
```
{: pre}

```
wsk action create /guest/demo/hello hello.js --web true
```
{: pre}

値が `true` または `yes` の `--web` フラグを使用すると、資格情報を必要とせずに、REST インターフェース経由でアクションにアクセスできます。Web アクションは、以下のように構造化された URL を使用して呼び出すことができます。
`https://{APIHOST}/api/v1/web/{namespace}/{packageName}/{actionName}.{EXT}`

アクションが名前付きパッケージ内にない場合、パッケージ名は `default` になります。

例えば、`guest/demo/hello` などです。API キーなしで Web アクション API パスを `curl` または `wget` で使用できます。ブラウザーに直接入力することも可能です。

[https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane](https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane) を、ご使用の Web ブラウザーで開いてみてください。または、次のようにして、`curl` でアクションを呼び出してみてください。
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane
```
{: pre}

次の例では、Web アクションが HTTP リダイレクトを実行します。
```javascript
function main() {
  return {
    headers: { location: 'http://openwhisk.org' },
    statusCode: 302
  }
}
```
{: codeblock}    

次の例では、Web アクションが単一の Cookie を設定します。
```javascript
function main() {
  return {
    headers: {
      'Set-Cookie': 'UserID=Jane; Max-Age=3600; Version=',
      'Content-Type': 'text/html'
    },
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
}
```
{: codeblock}  

次の例では、Web アクションが複数の Cookie を設定します。
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

次の例は、`image/png` を返します。
```javascript
function main() {
    let png = <base 64 encoded string>
    return { headers: { 'Content-Type': 'image/png' },
             statusCode: 200,
             body: png };
}
```
{: codeblock}  

次の例は、`application/json` を返します。
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

HTTP 応答のデフォルトの `Content-Type` は `application/json` で、body には許容される任意の JSON 値が可能です。デフォルトの `Content-Type` は headers から省略可能です。

事前定義されたシステム制限を超える応答は失敗するため、アクションの[応答サイズの制限](./openwhisk_reference.html)を把握しておくことが重要です。例えば、ラージ・オブジェクトは OpenWhisk を通じてインラインで送信するのではなく、オブジェクト・ストアに置きます。

## アクションによる HTTP 要求の処理
{: #openwhisk_webactions_http}

Web アクションではない OpenWhisk アクションは、認証が必要で、また、JSON オブジェクトで応答する必要があります。対照的に、Web アクションは認証なしで起動でき、さまざまなタイプの _headers_、_statusCode_、および _body_ コンテンツで応答する HTTP ハンドラーを実装するために使用できます。Web アクションは JSON オブジェクトを返す必要があります。ただし、OpenWhisk システム (つまり、`コントローラー`) は、結果に以下の最上位 JSON プロパティーの 1 つ以上が含まれる場合、Web アクションを別の方法で処理します。

- `headers`: キーがヘッダー名であり、値がそれらのヘッダーのストリング値、数値、またはブール値である、JSON オブジェクト (デフォルトはヘッダーなし)。1 つのヘッダーで複数の値を送信する場合、ヘッダーの値は、値の JSON 配列になります。
- `statusCode`: 有効な HTTP 状況コード (デフォルトは 200 の OK です )。
- `body`: プレーン・テキストまたは Base64 エンコードのストリング (バイナリー・データの場合) のいずれかであるストリング。

コントローラーは、アクションが指定したヘッダーがあれば、要求/応答を終了する HTTP クライアントにそれを渡します。同様に、コントローラーは状況コード (存在する場合) で応答します。最後に、本体が応答本体として渡されます。アクション結果の `headers` で `Content-Type` ヘッダーが宣言されていない限り、本体は、ストリングであればそのまま渡されます (それ以外の場合はエラーになります)。`Content-Type` が定義されている場合、コントローラーは応答がバイナリー・データなのかプレーン・テキストなのかを判別し、必要に応じて base64 デコーダーを使用してストリングをデコードします。本体を正しくデコードできない場合、呼び出し元にエラーが返されます。

_注_ : JSON オブジェクトまたは配列はバイナリー・データとして扱われます。これは、base64 エンコードでなければなりません。

## HTTP コンテキスト

すべての Web アクションは、起動されると、アクション入力引数へのパラメーターとして、HTTP 要求の詳細を受け取ります。 

以下の HTTP パラメーターを参照してください。

- `__ow_method` (タイプ: ストリング): 要求の HTTP メソッド。
- `__ow_headers` (タイプ: ストリング間マップ): 要求ヘッダー。
- `__ow_path` (タイプ: ストリング): マッチングされていない要求パス (マッチングは、アクションの拡張子が取り込まれると停止します)。
- `__ow_user` (タイプ: ストリング): OpenWhisk 認証済みサブジェクトを識別する名前空間
- `__ow_body` (タイプ: ストリング): コンテンツがバイナリーの場合は base64 エンコード・ストリング、それ以外の場合はプレーン・ストリングの要求本体エンティティー。
- `__ow_query` (タイプ: ストリング): 構文解析されていないストリングである、要求からの照会パラメーター。

要求は、指定された `__ow_` パラメーターのいずれもオーバーライドできません。これを行うと、要求は失敗し、状況は 400 の Bad Request になります。

`__ow_user` は、Web アクションが[認証を必要とするものとしてアノテーションが付けられている](./openwhisk_annotations.html#openwhisk_annotations_webactions)場合にのみ存在し、これにより、Web アクションは独自の許可ポリシーを実装することができます。`__ow_query` は、Web アクションが[「未加工」 HTTP 要求 ](#raw-http-handling)を処理することを選択した場合にのみ使用可能です。これは、URI からの解析される照会パラメーターを含むストリングです (`&` で分離)。`__ow_body` プロパティーは、「未加工」HTTP 要求の中、または HTTP 要求エンティティーが JSON オブジェクトでも形式データでもない場合に存在します。それ以外の場合、Web アクションは、アクション引数内の第 1 クラス・プロパティーとして、照会および本体のパラメーターを受け取ります。本体パラメーターは照会パラメーターより優先され、照会パラメーターはアクション・パラメーターおよびパッケージ・パラメーターより優先されます。

## HTTPS エンドポイント・サポート

サポートされる SSL プロトコル: TLS 1.0、TLS 1.1、TLS 1.2、TLS 1.3 ([ドラフト・バージョン 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18))

サポートされない SSL プロトコル: SSLv2、SSLv3

## 追加の機能
{: #extra-features}

Web アクションは、以下を含む追加機能を提供します。

- `コンテンツ拡張子`: 要求は、その要求にとって望ましいコンテンツ・タイプを、`.json`、`.html`、`.http`、`.svg`、または `.text` として指定する必要があります。タイプは、URI 内のアクション名に拡張子を追加することで指定されます。これによって、例えば、アクション `/guest/demo/hello` は `/guest/demo/hello.http` として示され、HTTP 応答を受け取ります。便宜上、拡張子が検出されない場合は `.http` 拡張子が想定されます。
- `結果からのフィールドの射影`: アクション名に続くパスを使用して、応答の 1 つ以上のレベルを射影します。
`/guest/demo/hello.html/body`。この機能により、ディクショナリー `{body: "..." }` を返すアクションは `body` プロパティーを射影して、代わりにそのストリング値を直接戻すことができます。射影パスは、絶対パス・モデル (例: XPath) に従います。
- `入力としての照会および本体のパラメーター`: アクションは、照会パラメーターと要求本体のパラメーターを受け取ります。パラメーターをマージするときの優先順位は、パッケージ・パラメーター、アクション・パラメーター、照会パラメーター、および本体パラメーターです。これらの各パラメーターは、オーバーラップが発生すると、前の値をオーバーライドできます。例えば、`/guest/demo/hello.http?name=Jane` は、引数 `{name: "Jane"}` をアクションに渡します。
- `形式データ`: 標準 `application/json` に加えて、Web アクションは、入力としてデータ `application/x-www-form-urlencoded data` から、エンコードされた URL を受け取ることができます。
- `複数の HTTP 動詞を介したアクティベーション`: Web アクションは、HTTP メソッド (`GET`、`POST`、`PUT`、`PATCH`、および `DELETE`、ならびに `HEAD` および `OPTIONS`) のいずれかを介して起動できます。
- `非 JSON 本体および未加工 HTTP エンティティーの処理`: Web アクションは、JSON オブジェクト以外の HTTP 要求本体を受け入れることができ、不透明値のような値 (バイナリーではない場合はプレーン・テキスト、バイナリーの場合は base64 エンコード・ストリング) を常に受け取ることを選択できます。

以下の例では、Web アクションでこれらの機能を使用する方法を簡単に説明します。以下の本体を持つアクション `/guest/demo/hello` を考えてみます。
```javascript
function main(params) {
    return { response: params };
}
```

このアクションが Web アクションとして呼び出されるときに、結果と異なるパスを射影することによって、Web アクションの応答を変更できます。
例えば、オブジェクト全体を返し、アクションが受け取る引数を確認するには、以下のようにします。

```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json
 ```
{: pre}
```json
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

照会パラメーターで実行するには、以下のコマンド例を参照してください。
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json?name=Jane
 ```
{: pre}
```json
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

次のように形式データを使用して実行することもできます。
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -d "name":"Jane"
 ```
{: pre}
```json
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

JSON オブジェクトの場合、以下のコマンドを実行します。
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
```
{: pre}
```json
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

名前を (テキストとして) 射影するには、以下のコマンドを実行します。
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.text/response/name?name=Jane
```
{: pre}
```
Jane
```

便宜上、照会パラメーター、形式データ、および JSON オブジェクト本体エンティティーはすべてディクショナリーとして扱われ、それぞれの値は、アクション入力プロパティーとして直接アクセス可能です。より直接的に HTTP 要求エンティティーを処理しようとする Web アクションの場合、あるいは Web アクションが JSON オブジェクトではないエンティティーを受け取る場合には、この動作は当てはまりません。

前に示したように、「text」の content-type を使用した以下の例を参照してください。
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
```
{: pre}
```json
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


## コンテンツ拡張子
{: #openwhisk_webactions_extensions}

Web アクションを呼び出すには通常、コンテンツ拡張子が必要です。拡張子がない場合は、デフォルトとして `.http` が想定されます。射影パスは、`.json` と `.http` の拡張子には必要ありませんが、`.html`、`.svg`、`.text` の拡張子には必要です。便宜上、デフォルトのパスは、拡張子名と一致すると想定されます。Web アクションを起動して、`.html` 応答を受け取るには、アクションは、`html` という名前の最上位プロパティーを含む JSON オブジェクトを用いて応答する必要があります (または、応答は明示的パスに存在する必要があります)。言い換えると、`/guest/demo/hello.html` は、`/guest/demo/hello.html/html` のように、`html` プロパティーを明示的に射影することと等価です。アクションの完全修飾名には、そのパッケージ名が含まれている必要があります。このパッケージ名は、アクションが名前付きパッケージ内にない場合は `default` になります。

## 保護されたパラメーター
{: #openwhisk_webactions_protected}

アクション・パラメーターは保護され、変更不可能として処理されます。Web アクションを有効にするために、パラメーターは自動的にファイナライズされます。

```
 wsk action create /guest/demo/hello hello.js \
      --parameter name Jane \
      --web true
```

これらの変更の結果、`name` が `Jane` にバインドされ、final アノテーションがあるため照会パラメーターまたは本体パラメーターでオーバーライドすることはできません。この設計は、意図的または偶発的にこの値を変更しようとする照会パラメーターまたは本体パラメーターからアクションを保護します。 

## Web アクションの無効化

Web API (`https://openwhisk.ng.bluemix.net/api/v1/web/`) を介した Web アクションの起動を無効にするには、`--web` フラグに値 `false` または `no` を渡して、CLI で アクションを更新します。

```
 wsk action update /guest/demo/hello hello.js --web false
```
{: pre}

## 未加工 HTTP 処理

Web アクションは、アクション入力に使用できる第 1 クラス・プロパティーに JSON オブジェクトをプロモーションせずに、着信 HTTP 本体を直接解釈して処理することを選択できます (例: `args.name` 対 `args.__ow_query` の構文解析)。この処理は、`raw-http` [アノテーション](./openwhisk_annotations.html)を介して行います。前述の同じ例を使用しますが、今度は「未加工」HTTP Web アクションとして使用し、以下のように、照会パラメーターと、HTTP 要求本体内の JSON 値の両方として `name` を受け取ります。
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}
```json 
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

OpenWhisk は、[Akka HTTP](http://doc.akka.io/docs/akka-http/current/scala/http/) フレームワークを使用して、どのコンテンツ・タイプがバイナリーであり、どれがプレーン・テキストであるかを[判別](http://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html)します。

### 未加工 HTTP 処理の有効化

未加工 HTTP Web アクションは、`--web` フラグで値 `raw` を使用して有効化します。

```
 wsk action create /guest/demo/hello hello.js --web raw
```

### 未加工 HTTP 処理の無効化

未加工 HTTP を無効にするには、`--web` フラグに値 `false` または `no` を渡します。

```
 wsk update create /guest/demo/hello hello.js --web false
```

### バイナリーの本体コンテンツを Base64 からデコード

未加工 HTTP コンテンツが処理されるとき、要求の `Content-Type` がバイナリーの場合、`__ow_body` コンテンツは、Base64 でエンコードされます。
以下の関数は、Node、Python、および Swift で本体コンテンツをデコードする方法を示しています。単純に、メソッドをファイルに保存し、保存された成果物を使用する未加工 HTTP Web アクションを作成し、その後、Web アクションを起動します。

#### Node

```javascript
function main(args) {
    decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
    return {body: decoded}
}
```
{: codeblock}

#### Python

```python
def main(args):
    try:
        decoded = args['__ow_body'].decode('base64').strip()
        return {"body": decoded}
    except:
        return {"body": "Could not decode body from Base64."}
```
{: codeblock}

#### Swift

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

例として、この Node 関数を `decode.js` として保存し、以下のコマンドを実行します。
```
 wsk action create decode decode.js --web raw
```
{: pre}

```
ok: created action decode
```

```
curl -k -H "content-type: application" -X POST -d "Decoded body" https:// openwhisk.ng.bluemix.net/api/v1/web/guest/default/decodeNode.json
```
{: pre}

```json
{
  "body": "Decoded body"
}
```

## OPTIONS 要求
{: #options-requests}

デフォルトで、Web アクションに対して OPTIONS 要求を行うと、自動的に CORS ヘッダーが応答ヘッダーに追加されます。これらのヘッダーにより、すべてのオリジン、および options、get、delete、post、put、head、および patch の各 HTTP 動詞が可能になります。

以下のヘッダーを参照してください。

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

あるいは、OPTIONS 要求は、Web アクションによって手動で処理することもできます。このオプションを有効にするには、`web-custom-options` アノテーションを `true` の値に設定して Web アクションに追加します。
このフィーチャーが有効になると、CORS ヘッダーは自動的に要求応答に追加されません。代わりに、開発者の責任で、希望するヘッダーをプログラマチックに追加することになります。OPTIONS 要求に対するカスタム応答を作成するには、次の例を参照してください。

```
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

この関数を `custom-options.js` に保存し、以下のコマンドを実行します。

```
$ wsk action create custom-option custom-options.js --web true -a web-custom-options true
$ curl https://${APIHOST}/api/v1/web/guest/default/custom-options.http -kvX OPTIONS
< HTTP/1.1 200 OK
< Server: nginx/1.11.13
< Content-Length: 0
< Connection: keep-alive
< Access-Control-Allow-Methods: OPTIONS, GET
< Access-Control-Allow-Origin: example.com
```

## エラー処理
{: #openwhisk_webactions_errors}

OpenWhisk アクションは、2 つの異なる障害モードで失敗します。1 つは_アプリケーション・エラー_ と呼ばれるものであり、キャッチされた例外に似ています。アクションが返す JSON オブジェクトには、最上位に `error` プロパティーが含まれます。もう 1 つは _開発者エラー_ であり、これはアクションで壊滅的な障害が起こって、応答が生成されない場合に発生します (キャッチされていない例外に似ています)。Web アクションの場合、コントローラーは次のようにアプリケーション・エラーを処理します。

- 指定されたパス射影はすべて無視され、コントローラーは代わりに `error` プロパティーを射影します。
- コントローラーはアクションの拡張子で暗黙に示されるコンテンツ処理を `error` プロパティーの値に適用します。

開発者は、Web アクションの使用方法を認識し、適切なエラー応答を生成する必要があります。例えば、`.http` 拡張子と共に使用される Web アクションは、`{error: { statusCode: 400 }` のような HTTP 応答を返します。適切にエラーが生成されないと、拡張子で暗黙に示される `Content-Type` と、エラー応答内のアクション `Content-Type` とが一致しません。Web アクションがシーケンスになっている場合は、シーケンスを形成しているコンポーネントが必要に応じて適切なエラーを生成できるように、特別な考慮が必要です。

