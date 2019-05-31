---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: serverless, rest api, gateway, web actions

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

# サーバーレス REST API の作成
{: #apigateway}

API を使用して、{{site.data.keyword.openwhisk}} アクションを直接管理します。 API ゲートウェイは、[Web アクション](/docs/openwhisk?topic=cloud-functions-actions_web)に対するプロキシーとして機能し、HTTP メソッド・ルーティング、クライアント ID と秘密、速度制限、CORS、API 使用量の表示、応答ログの表示、および API 共有ポリシーを提供します。
{: shortdesc}

API 管理について詳しくは、[API 管理の資料](/docs/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis)をお読みください。



## 最初の API の作成
{: #api_create}

始めに、[{{site.data.keyword.openwhisk_short}} CLI プラグイン](/docs/openwhisk?topic=cloud-functions-cli_install)をインストールします。

1. 以下のコードを `hello.js` という名前の JavaScript ファイルに保存します。
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. 作成したファイルを使用して、`hello` という名前の Web アクションを作成します。 **注:** 必ず、フラグ `--web true` を追加してください。
  ```
  ibmcloud fn action create hello hello.js --web true
  ```
  {: pre}

  出力例:
  ```
  ok: created action hello
  ```
  {: screen}

3. 基本パス `/hello`、パス `/world`、メソッド `get`、および応答タイプ `json` を使用して API を作成します。
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

  GET HTTP メソッドを使用して `hello` アクションを公開する新規 URL が生成されます。

4. cURL コマンドを使用してテスト HTTP 要求を URL に送信します。
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  出力例:
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

Web アクション `hello` が呼び出され、照会パラメーターにパラメーター **name** を含む JSON オブジェクトが返されます。 パラメーターは、単純な照会パラメーターを使用してアクションに渡すことも、要求本文を使用して渡すこともできます。 Web アクションは、認証を使用せずに、アクションをパブリックに呼び出すことができます。

## HTTP 応答の完全な制御の使用
{: #api_control}

`--response-type` フラグは、API ゲートウェイによってプロキシー処理される Web アクションのターゲット URL を制御します。 例えば、`--response-type json` フラグを使用すると、アクションの完全な結果が JSON フォーマットで返され、**Content-Type** ヘッダーが自動的に `application/json` に設定されます。

本文で異なるコンテンツ・タイプを返すには、**statusCode** や **headers** などの HTTP 応答プロパティーを完全に制御します。 `--response-type http` フラグを使用して、`http` 拡張子を使用する Web アクションのターゲット URL を構成することができます。 `http` 拡張子を使用する Web アクションの戻りに準拠するようにアクションのコードを変更するか、シーケンスにアクションを含めてその結果を新規アクションに渡すことができます。 その後、新規アクションで結果を変換し、HTTP 応答として適切なフォーマット設定になるようにすることができます。 応答タイプおよび Web アクション拡張について詳しくは、[Web アクション](/docs/openwhisk?topic=cloud-functions-actions_web)の資料を参照してください。

1. `hello.js` アクションのコードを変更して、JSON プロパティー `body`、`statusCode`、および `headers` を返すようにします。
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

2. 変更された結果でアクションを更新します。
  ```
  ibmcloud fn action update hello hello.js --web true
  ```
  {: pre}

3. `--response-type http` フラグを使用して、API 応答タイプを更新します。
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

4. 以下の cURL コマンドを使用して、更新された API を呼び出します。
  ```
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


<staging books>

## 複数の Web アクションの公開
{: #api_multiple_web}

複数の Web アクションを公開して、アプリケーション・バックエンドを実装することができます。 例えば、ブック・クラブ用の一連のアクションを公開するには、以下のような一連のアクションを使用して、ブック・クラブ用のバックエンドを実装します。

| アクション | HTTP メソッド | 説明 |
| ----------- | ----------- | ------------ |
| getBooks    | GET | 本の詳細を取得  |
| postBooks   | POST | 本を追加 |
| putBooks    | PUT | 本の詳細を更新 |
| deleteBooks | DELETE | 本を削除 |

この例では、API はパス・パラメーターを使用して定義されます。 パス・パラメーターを使用する場合、API には応答タイプ `http` が定義される必要があります。 パスの値 (基本パスで始まり、実際のパス・パラメーター値を含む) は、アクションの JSON パラメーターの `__ow_path` フィールドで使用できます。 HTTP コンテキスト・フィールドについて詳しくは、[Web アクションの HTTP コンテキスト](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_context)の資料を参照してください。

このブック・クラブ Web アクションの例を試すには、以下のようにします。

1. HTTP URL 基本パスに `/club`、リソースに `books`、パス・パラメーターに、本の国際標準図書番号 (ISBN) を使用して特定の本を識別するために使用される `{isbn}` を指定して、ブック・クラブ用の API を `Book Club` という名前で作成します。
  ```
  ibmcloud fn api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud fn api create /club /books get getBooks                       --response-type http
  ibmcloud fn api create /club /books post postBooks                     --response-type http
  ibmcloud fn api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud fn api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: pre}

  基本パス `/club` を使用して公開されている最初のアクションには、`Book Club` という名前のラベルが付けられています。 これで、`/club` の下に公開されるその他すべてのアクションが、`Book Club` に関連付けられます。

2. 公開されているすべての `Book Club` アクションをリストします。
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

3. HTTP POST を使用して、`JavaScript: The Good Parts` というタイトルのブックを追加します。
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

4. `getBooks` アクションに対する HTTP GET 呼び出しを使用して、ブックのリストを取得します。
  ```
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

5. `deleteBooks` アクションに対する HTTP DELETE 呼び出しを使用して、特定のブックを削除します。 この例の場合、`deleteBooks` アクションの `__ow_path` フィールド値は、`/club/books/978-0596517748` です。ここで、`978-0596517748` はパスの `{isbn}` の実際の値です。
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

## 構成のエクスポートおよびインポート
{: #api_export_import}

構成をエクスポートまたはインポートするには、ブック・クラブの例を引き続き使用します。

1. `Book Club` API を `club-swagger.json` という名前のファイルにエクスポートします。 ファイルを入力として使用して API を再作成するための基盤として、このファイルを使用できます。
  ```
  ibmcloud fn api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. まず、共通した基本パスの下に公開されたすべての URL を削除することで、swagger ファイルをテストします。
  ```
  ibmcloud fn api delete /club
  ```
  {: pre}

  出力例:
  ```
  ok: deleted API /club
  ```
  {: screen}

  公開されたすべての URL の削除は、基本パス `/club` を使用して行うか、または API 名前ラベル `"Book Club"` を使用して行うことができます。
  {: tip}

3. `club-swagger.json` ファイルを使用して、`Book Club` API をリストアします。
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

4. `Book Club` API が再作成されたことを確認します。
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


</staging book>

## 構成の変更
{: #api_modify_config}

構成を作成した後、{{site.data.keyword.openwhisk_short}} ダッシュボードの[**「API」タブ**](https://cloud.ibm.com/openwhisk/apimanagement) を使用して、以下の方法で構成を変更できます。

* 一連の {{site.data.keyword.openwhisk_short}} アクションをラップする [{{site.data.keyword.openwhisk_short}} API を作成](/docs/services/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis)します。
* API セキュリティー・ポリシーおよび速度制限ポリシーを適用することで、[API を保護](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis)します。
* API 使用量統計を表示し、応答ログを確認することで、[トラフィックを管理](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis)します。
* API を {{site.data.keyword.Bluemix_notm}} 内部および外部の開発者と[ソーシャル化および共有](/docs/services/api-management?topic=api-management-manage_apis#share_api_manage_apis)します。

構成の更新が完了したら、定義ファイルを JSON 形式でダウンロードし、CLI を使用してそれを再インポートできます。 構成のダウンロードとインポートは、例えば、継続的な統合およびデプロイメント (CICD) パイプラインでの無人デプロイメントなどに役立ちます。 また、UI を使用して API 定義ファイルをアップロードおよび再インポートするオプションもあります。

