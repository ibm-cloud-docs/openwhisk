---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# API ゲートウェイ
{: #openwhisk_apigateway}

OpenWhisk アクションは、API 管理によって管理することからメリットを享受できます。

API ゲートウェイは、[Web アクション](./openwhisk_webactions.html)のプロキシーの役割を果たし、追加の機能を提供します。追加の機能には、HTTP メソッド・ルーティング、クライアント ID/秘密、速度制限、CORS、API 使用量の表示、応答ログの表示、API 共有ポリシーがあります。
API 管理について詳しくは、[API 管理の資料](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis)をお読みください。
{: shortdesc}

## ブラウザーを使用した OpenWhisk Web アクションからの API の作成

API ゲートウェイを使用して、OpenWhisk アクションを API として公開できます。API を定義した後に、セキュリティー・ポリシーおよび速度制限ポリシーを適用したり、API 使用量および応答ログを表示したり、API 共有ポリシーを定義したりすることができます。
OpenWhisk ダッシュボードで、[「API」タブ](https://console.ng.bluemix.net/openwhisk/apimanagement)をクリックします。


## CLI を使用した OpenWhisk Web アクションからの API の作成

### OpenWhisk CLI 構成

API ホストを使用して OpenWhisk CLI を構成します。

以下の 2 つの {{site.data.keyword.Bluemix_notm}} 地域が使用可能であり、独自の固有の API ホストおよび許可キーが必要です。

* 米国南部
  * API ホスト: `openwhisk.ng.bluemix.net`

* 英国
  * API ホスト: `openwhisk.eu-gb.bluemix.net`

以下のコマンドを実行して、希望する Bluemix 地域用に API ホストを設定します。

米国南部:
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

英国:
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

許可キーは地域ごとに固有のため、地域の切り替えが必要になった場合は、API ホストと許可キーの両方を使用して CLI を再構成する必要があります。
{: tip}

アクション、ルール、パッケージなどの成果物は、地域固有です。そのため、複数の地域で同じ成果物を使用する場合は、目的の各地域にその成果物をデプロイする必要があります。

`wsk api` コマンドを使用できるようにするには、CLI 構成ファイル `~/.wskprops` に Bluemix アクセス・トークンを含める必要があります。

アクセス・トークンを取得するには、以下の CLI コマンドを使用します。
```
wsk bluemix login
```
{: pre}

このコマンドの詳細情報を表示するには、以下を実行します。
```
wsk bluemix login -h
```
{: pre}

`wsk bluemix login` コマンドがエラー「`BMXLS0202E: You are using a federated user ID, please use one time code to login with option --sso`」で失敗する場合、`bluemix login` を使用して {{site.data.keyword.Bluemix_notm}} CLI でログインし、`wsk bluemix login --sso` を発行してください。{: tip}

### CLI を使用した最初の API の作成

1. 以下の内容を持つ JavaScript ファイルを作成します。この例では、ファイル名は「hello.js」です。
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}
  
2. 以下の JavaScript 関数から Web アクションを作成します。この例では、アクションは「hello」という名前です。必ず、フラグ `--web true` を追加してください。
  ```
  wsk action create hello hello.js --web true
  ```
  {: pre}

  ```
  ok: created action hello
  ```
  
3. 以下のように、基本パス `/hello`、パス `/world`、メソッド `get`、および応答タイプ `json` を使用して、API を作成します。
  ```
  wsk api create /hello /world get hello --response-type json
  ```

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  __GET__ HTTP メソッドを介して `hello` アクションを公開する新規 URL が生成されます。
  
4. 最後に、HTTP 要求を URL に送信します。
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world?name=OpenWhisk
  ```

  ```json
  {
  "payload": "Hello world OpenWhisk"
  }
  ```

  Web アクション `hello` が呼び出され、照会パラメーターを介して送信されたパラメーター `name` を含む JSON オブジェクトが返されます。単純な照会パラメーターまたは要求本文を介して、パラメーターをアクションに渡すことができます。Web アクションにより、OpenWhisk 許可 API キーを使用せずに、パブリックな方法でアクションを呼び出すことができます。
  
### HTTP 応答の完全な制御
  
  `--response-type` フラグは、API ゲートウェイによってプロキシー処理される Web アクションのターゲット URL を制御します。`--response-type json` を使用すると、アクションの完全な結果が JSON フォーマットで返され、Content-Type ヘッダーが `application/json` に自動的に設定されます。 
  
  開始後に `statusCode`、`headers` などの HTTP 応答プロパティーを完全に制御し、`body` 内に異なるコンテンツ・タイプが返るようにしたいと考える場合があります。フラグ `--response-type http` は、`http` 拡張を使用して Web アクションのターゲット URL を構成することで、これを可能にします。

  `http` 拡張を使用して Web アクションの戻りに準拠するようにアクションのコードを変更するか、アクションをシーケンスに組み込んで、結果を新規アクションに渡すことを選択できます。その後、新規アクションで結果を変換し、HTTP 応答として適切なフォーマット設定になるようにすることができます。応答タイプおよび Web アクション拡張について詳しくは、[Web アクション](./openwhisk_webactions.html)の資料を参照してください。

  `hello.js` のコードを変更して、JSON プロパティー `body`、`statusCode`、および `headers` を返すようにします。
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: new Buffer(JSON.stringify({payload:`Hello world ${name}`})).toString('base64'), 
        statusCode:200, 
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}
  本文はストリングではなく `base64` でエンコードされて戻る必要がある点に注意してください。
  
  以下のように、変更された結果でアクションを更新します。 
  ```
  wsk action update hello hello.js --web true
  ```
  {: pre}

  以下のように、`--response-type http` を指定して、API を更新します。 
  ```
  wsk api create /hello /world get hello --response-type http
  ```
  {: pre}
  
  以下のように、更新された API を呼び出します。 
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  {: pre}

  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  これで、API を完全に制御し、内容を制御できるようになりました。HTML を返すのと同様に、「見つかりません (404)」、「認証が必要 (401)」、さらには「内部エラー (500)」などの状況コードを設定できます。

### 複数の Web アクションの公開

例えば、ブック・クラブ用の一連のアクションを公開する場合、以下のような一連のアクションを使用して、ブック・クラブ用のバックエンドを実装できます。

| アクション | HTTP メソッド | 説明 |
| ----------- | ----------- | ------------ |
| getBooks    | GET | 本の詳細を取得  |
| postBooks   | POST | 本を追加 |
| putBooks    | PUT | 本の詳細を更新 |
| deleteBooks | DELETE | 本を削除 |

HTTP URL 基本パスとして `/club` を使用し、リソースとして `books` を指定して、`Book Club` という名前のブック・クラブ用の API を作成します。
```
wsk api create -n "Book Club" /club /books get getBooks --response-type http
wsk api create /club /books post postBooks              --response-type http
wsk api create /club /books put putBooks                --response-type http
wsk api create /club /books delete deleteBooks          --response-type http
```

基本パス `/club` を指定して公開されている最初のアクションが、`Book Club` という名前の API ラベルを取得している点に注意してください。`/club` の下で公開されている他のすべてのアクションは、`Book Club` に関連付けられます。

以下のコマンドを使用して、公開されているすべてのアクションをリストします。
```
wsk api list -f
```
{: pre}

```
ok: APIs
Action: getBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: get
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: postBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: post
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: putBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: put
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: deleteBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: delete
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

試しに、以下のように、HTTP __POST__ を使用して、本「`JavaScript: The Good Parts`」を追加できます。
```
curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": "success"
}
```

HTTP __GET__ を介してアクション `getBooks` を使用してリストを取得します。
```
curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
}
```

### 構成のエクスポート
`Book Club` という名前の API をファイルにエクスポートします。ファイルを入力として使用して API を再作成するための基盤として、このファイルを使用できます。 
```
wsk api get "Book Club" > club-swagger.json
```
{: pre}

まず、共通の基本パスの下に公開されたすべての URL を削除して、swagger ファイルをテストします。
公開されたもののすべての削除は、基本パス `/club` を使用して行うか、または API 名前ラベル `"Book Club"` を使用して行うことができます。
```
wsk api delete /club
```
```
ok: deleted API /club
```
### 構成の変更

OpenWhisk ダッシュボードで構成を編集し、[「API」タブ](https://console.ng.bluemix.net/openwhisk/apimanagement)をクリックして、セキュリティー、速度制限、およびその他の機能をセットアップできます。

### 構成のインポート

次に、`club-swagger.json` ファイルを使用して、`Book Club` という名前の API をリストアします。
```
wsk api create --config-file club-swagger.json
```
{: pre}

```
ok: created api /books delete for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books get for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books post for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books put for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

以下のように、API が再作成されたことを確認します。
```
wsk api list /club
```
{: pre}

```
ok: apis
Action                    Verb         API Name        URL
getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
