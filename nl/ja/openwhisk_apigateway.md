---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# サーバーレス REST API の作成
{: #openwhisk_apigateway}

{{site.data.keyword.openwhisk}} アクションには、API ゲートウェイを導入して直接 API によって管理されることによるメリットがあります。API ゲートウェイは、[Web アクション](./openwhisk_webactions.html)に対しプロキシーの役割を果たし、追加の機能を提供します。追加の機能には、HTTP メソッド・ルーティング、クライアント ID/秘密、速度制限、CORS、API 使用量の表示、応答ログの表示、API 共有ポリシーがあります。 API 管理について詳しくは、[API 管理の資料](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis)をお読みください。
{: shortdesc}

## ブラウザーを使用した OpenWhisk Web アクションからの API の作成
{: #create_api_browser}

[{{site.data.keyword.openwhisk_short}} ダッシュボード](https://console.bluemix.net/openwhisk/)の[**「API」タブ**](https://console.bluemix.net/openwhisk/apimanagement)を使用して、以下のタスクを実行できます。

* [Cloud Functions API の作成](https://console.bluemix.net/openwhisk/apimanagement) - 一連の OpenWhisk アクションをラップする API を作成できます。
* [API の保護](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - API を保護するための API セキュリティー・ポリシーおよび速度制限ポリシーを適用できます。
* [トラフィックの管理](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - API 使用量統計を表示し、応答ログを確認できます。
* [ソーシャル化と共有](https://console.bluemix.net/docs/apis/management/manage_apis.html#share_api) - {{site.data.keyword.Bluemix_notm}} 内外の開発者と API を共有できます。

## CLI プラグインを使用した OpenWhisk Web アクションからの API の作成
{: #create_api_cli}

以下のセクションで、{{site.data.keyword.openwhisk_short}} CLI プラグインを使用した API 管理タスクを順に説明します。CLI から API を作成および管理するには、まず、{{site.data.keyword.Bluemix_notm}} 用の [{{site.data.keyword.openwhisk_short}} CLI プラグイン](https://console.bluemix.net/docs/openwhisk/bluemix_cli.html)をインストールする必要があります。

便宜上、ステップは、より小さなサブトピックに分かれています。以下の API タスクのリストを使用して、それらの各サブトピックに迅速にジャンプできます。

* [最初の API の作成](openwhisk_apigateway.html#create_cli_api)
* [HTTP 応答の完全な制御](openwhisk_apigateway.html#full_control)
* [複数の Web アクションの公開](openwhisk_apigateway.html#multiple_web_actions)
* [構成のエクスポート](openwhisk_apigateway.html#export_config)
* [構成のインポート](openwhisk_apigateway.html#import_config)
* [構成の変更](openwhisk_apigateway.html#modify_config)

### CLI を使用した最初の API の作成
{: #create_cli_api}

1. 以下の内容を含む **hello.js** という名前の JavaScript ファイルを作成します。
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. ステップ 1 で作成した `hello.js` ファイルを使用して、**hello** という名前の Web アクションを作成します。**注:** 必ず、フラグ `--web true` を追加してください。
  ```
  ibmcloud fn action create hello hello.js --web true
  ```
  {: pre}

  出力例:
  ```
  ok: created action hello
  ```
  {: screen}

3. 以下のように、基本パス `/hello`、パス `/world`、メソッド `get`、および応答タイプ `json` を使用して、API を作成します。
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  出力例:
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  __GET__ HTTP メソッドを使用して `hello` アクションを公開する新規 URL が生成されます。

4. 最後に、**curl** コマンドを使用して、URL に HTTP 要求を送信します。
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  出力例:
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

Web アクション **hello** が呼び出され、照会パラメーターを介して送信されたパラメーター **name** を含む JSON オブジェクトが返されます。パラメーターは、単純な照会パラメーターを使用してアクションに渡すことも、要求本文を使用して渡すこともできます。Web アクションは、OpenWhisk 許可 API キーを使用せずに、パブリックな方法でアクションを呼び出すことができます。

### HTTP 応答の完全な制御
{: #full_control}

`--response-type` フラグは、API ゲートウェイによってプロキシー処理される Web アクションのターゲット URL を制御します。 `--response-type json` を使用すると、アクションの完全な結果が JSON フォーマットで返され、Content-Type ヘッダーが `application/json` に自動的に設定されます。

`statusCode` や `headers` などの HTTP 応答プロパティーを完全に制御して、`body` 内に異なるコンテンツ・タイプを返すことが必要になる場合が考えられます。フラグ `--response-type http` は、`http` 拡張を使用して Web アクションのターゲット URL を構成することで、これを可能にします。

`http` 拡張を使用して Web アクションの戻りに準拠するようにアクションのコードを変更するか、アクションをシーケンスに組み込んで、結果を新規アクションに渡すことを選択できます。 その後、新規アクションで結果を変換し、HTTP 応答として適切なフォーマット設定になるようにすることができます。 応答タイプおよび Web アクション拡張について詳しくは、[Web アクション](./openwhisk_webactions.html)の資料を参照してください。

1. `hello.js` のコードを変更して、JSON プロパティー `body`、`statusCode`、および `headers` を返すようにします。
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello world ${name}`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. 以下のように、変更された結果でアクションを更新します。
  ```
  ibmcloud fn action update hello hello.js --web true
  ```
  {: pre}

3. `--response-type http` フラグを使用して、API 応答タイプを更新します。
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

4. 以下の **curl** コマンドを使用して、更新された API を呼び出します。
  ```bash
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  出力例:
  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  {: screen}

これで、API を完全に制御し、内容を制御できるようになりました。 HTML を返すのと同様に、「見つかりません (404)」、「認証が必要 (401)」、さらには「内部エラー (500)」などの状況コードを設定できます。

### 複数の Web アクションの公開
{: #multiple_web_actions}

例えば、ブック・クラブ用の一連のアクションを公開する場合、以下のような一連のアクションを使用して、ブック・クラブ用のバックエンドを実装できます。

| アクション | HTTP メソッド | 説明 |
| ----------- | ----------- | ------------ |
| getBooks    | GET | 本の詳細を取得  |
| postBooks   | POST | 本を追加 |
| putBooks    | PUT | 本の詳細を更新 |
| deleteBooks | DELETE | 本を削除 |

この例では、API は**パス・パラメーター**を使用して定義されます。パス・パラメーターを使用する場合、API には応答タイプ `http` が定義される必要があります。パスの値 (基本パスで始まり、実際のパス・パラメーター値を含む) は、アクションの JSON パラメーターの `__ow_path` フィールドで使用できます。`http` 応答タイプを指定して呼び出される Web アクションで使用可能な追加の HTTP コンテキスト・フィールドに関する情報を含め、詳細については、[Web アクション HTTP コンテキスト](./openwhisk_webactions.html#http-context)の資料を参照してください。

1. HTTP URL 基本パスに `/club`、リソースに `books`、パス・パラメーターに、本の国際標準図書番号 (ISBN) を使用して特定の本を識別するために使用される `{isbn}` を指定して、ブック・クラブ用の API を **Book Club** という名前で作成します。
  ```bash
  ibmcloud fn api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud fn api create /club /books get getBooks                       --response-type http
  ibmcloud fn api create /club /books post postBooks                     --response-type http
  ibmcloud fn api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud fn api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: codeblock}

  基本パス `/club` を指定して公開されている最初のアクションが、**Book Club** という名前の API ラベルを取得している点に注意してください。 これで、`/club` の下に公開されるその他すべてのアクションが、**Book Club** に関連付けられます。

2. 以下のコマンドを使用して、公開されているすべての **Book Club** アクションをリストします。
  ```
  ibmcloud fn api list /club -f
  ```
  {: pre}

  出力例:
  ```
  ok: APIs
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: postBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: post
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: putBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: put
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: deleteBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: delete
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

3. 試しに、以下のように、HTTP __POST__ を使用して、「**JavaScript: The Good Parts**」というタイトルの本を追加できます。
  ```
  curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' -H "Content-Type: application/json" https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  出力例:
  ```
  {
    "result": "success"
  }
  ```
  {: screen}

4. アクション **getBooks** と HTTP __GET__ を使用して、本のリストを取得します。
  ```bash
  curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  出力例:
  ```
  {
    "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
  }
  ```
  {: screen}

5. アクション **deleteBooks** と HTTP __DELETE__ を使用して、特定の本を削除できます。この例の場合、**deleteBooks** アクションの `__ow_path` フィールド値は、`/club/books/978-0596517748` です。ここで、`978-0596517748` はパスの `{isbn}` の実際の値です。
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

### 構成のエクスポート
{: #export_config}

1. **Book Club** という名前の API をファイルにエクスポートします。ファイルを入力として使用して API を再作成するための基盤として、このファイルを使用できます。
  ```
  ibmcloud fn api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. まず、以下のコマンドを使用して、共通した基本パスの下に公開されたすべての URL を削除することで、swagger ファイルをテストします。
  ```
  ibmcloud fn api delete /club
  ```
  {: pre}

  出力例:
  ```
  ok: deleted API /club
  ```
  {: screen}

  公開されたすべての URL の削除は、基本パス `/club` を使用して行うか、または API 名前ラベル **"Book Club"** を使用して行うことができます。
  {: tip}

### 構成のインポート
{: #import_config}

1. 今度は、ファイル名 **club-swagger.json** を使用して、`Book Club` という名前の API を復元します。
  ```
  ibmcloud fn api create --config-file club-swagger.json
  ```
  {: pre}

  出力例:
  ```
  ok: created api /club/books/{isbn} get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} put for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} delete for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books post for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: screen}

2. **Book Club** API が再作成されたことを確認します。
  ```
  ibmcloud fn api list /club
  ```
  {: pre}

  出力例:
  ```
  ok: apis
  Action                    Verb         API Name        URL
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

### UI を使用した構成の変更
{: #modify_config}

{{site.data.keyword.openwhisk_short}} ダッシュボードで構成を編集し、[「API」タブ](https://console.ng.bluemix.net/openwhisk/apimanagement)をクリックして、セキュリティー、速度制限、およびその他の機能をセットアップできます。 構成の更新が完了したら、定義ファイルを JSON 形式でダウンロードし、CLI を使用してそれを再インポートできます。これは、例えば、継続的な統合およびデプロイメント (CICD) パイプラインでの無人デプロイメントなどに役立ちます。また、UI を使用して API 定義ファイルをアップロードおよび再インポートするオプションもあります。
