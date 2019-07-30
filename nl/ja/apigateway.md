---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: serverless, rest api, gateway, web actions, functions

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


# サーバーレス REST API の作成
{: #apigateway}

API を使用して、{{site.data.keyword.openwhisk}} の [Web アクション](/docs/openwhisk?topic=cloud-functions-actions_web)を直接管理できます。

{: shortdesc}

API ゲートウェイを使用した API の作成は、IAM ベースの名前空間ではサポートされていません。代わりに、Cloud Foundry ベースの名前空間を使用してください。
{: important}

## {{site.data.keyword.openwhisk_short}} で REST API を使用する理由

API ゲートウェイは、Web アクションに対するプロキシーとして使用できます。API ゲートウェイは、HTTP メソッド・ルーティング、クライアント ID と秘密、速度制限、CORS、API 使用量の表示、応答ログの表示、API 共有ポリシーを提供します。

API 管理について詳しくは、[API 管理の資料](/docs/api-management?topic=api-management-manage_openwhisk_apis)をお読みください。

## 最初の API の作成
{: #api_create}

REST API を作成するには、Cloud Foundry スペース内で `SpaceDeveloper` 許可が必要です。`ibmcloud account space-roles <org>` を実行すると、スペース許可を参照できます。
{: note}

始めに、[{{site.data.keyword.openwhisk_short}} CLI プラグイン](/docs/openwhisk?topic=cloud-functions-cli_install)をインストールします。

1. 以下のコードを `hello.js` という名前の JavaScript ファイルに保存します。
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello, ${name}!`};
  }
  ```
  {: codeblock}

2. 作成したファイルを使用して、`hello` という名前の Web アクションを作成します。必ずフラグ `--web true` を追加してください。`<filepath>` を `hello.js` ファイルのファイル・パスに置き換えます。

  ```
  ibmcloud fn action create hello <filepath>/hello.js --web true
  ```
  {: pre}

  **出力例**
  ```
  ok: created action hello
  ```
  {: screen}

3. 基本パス `/hello`、パス `/world`、メソッド `get`、および応答タイプ `json` を使用して API を作成します。
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  **出力例**
  `GET` HTTP メソッドを使用して `hello` アクションを公開する新規 URL が生成されます。

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  
4. 以下の cURL コマンドを使用してテスト HTTP 要求を URL に送信します。
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=Jane
  ```
  {: pre}

  **出力例**
  Web アクション `hello` が呼び出され、照会パラメーターにパラメーター `name` を含む JSON オブジェクトが返されます。パラメーターは、単純な照会パラメーターを使用してアクションに渡すことも、要求本文を使用して渡すこともできます。 Web アクションは、認証を使用せずに、アクションをパブリックに呼び出すことができます。

  ```
  {
  "payload": "Hello, Jane!"
  }
  ```
  {: screen}



## HTTP 応答の完全な制御の使用
{: #api_control}

`--response-type` フラグは、API ゲートウェイによってプロキシー処理される Web アクションのターゲット URL を制御します。 例えば、`--response-type json` フラグを使用すると、アクションの完全な結果が JSON フォーマットで返され、`Content-Type` ヘッダーが自動的に `application/json` に設定されます。

本文で異なるコンテンツ・タイプを返すには、`statusCode` や `headers` などの HTTP 応答プロパティーを完全に制御します。 `--response-type http` フラグを使用して、`http` 拡張子を使用する Web アクションのターゲット URL を構成することができます。 `http` 拡張子を使用する Web アクションの戻りに準拠するようにアクションのコードを変更するか、シーケンスにアクションを含めてその結果を新規アクションに渡すことができます。 その後、新規アクションで結果を変換し、HTTP 応答として適切なフォーマット設定になるようにすることができます。 応答タイプおよび Web アクション拡張について詳しくは、[Web アクション](/docs/openwhisk?topic=cloud-functions-actions_web)の資料を参照してください。

1. 以下のコードを `hello.js` として保存します。
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello, ${name}!`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. 新しいバージョンの `hello.js` コードを使用して、Web アクション `hello` を更新します。
  ```
  ibmcloud fn action update hello <filepath>/hello.js --web true
  ```
  {: pre}

  **出力**
  ```
  ok: updated action hello
  ```
  {: screen}

3. `--response-type http` フラグを使用して、API 応答タイプを更新します。
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

  **出力**
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

4. 以下の cURL コマンドを使用して、更新された API を呼び出します。
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  **出力例**
  ```
  {
  "payload": "Hello, Serverless API!"
  }
  ```
  {: screen}

## 構成の変更
{: #api_modify_config}

構成を作成した後、{{site.data.keyword.openwhisk_short}} ダッシュボードの[「API」タブ](https://cloud.ibm.com/openwhisk/apimanagement){: external} を使用して、以下の方法で構成を変更できます。

* 一連の {{site.data.keyword.openwhisk_short}} アクションをラップする [{{site.data.keyword.openwhisk_short}} API を作成](/docs/services/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis)します。
* API セキュリティー・ポリシーおよび速度制限ポリシーを適用することで、[API を保護](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis)します。
* API 使用量統計を表示し、応答ログを確認することで、[トラフィックを管理](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis)します。
* API を {{site.data.keyword.cloud_notm}} 内部および外部の開発者と[ソーシャル化および共有](/docs/services/api-management?topic=api-management-manage_apis#share_api_manage_apis)します。

</br>
構成の更新が完了したら、定義ファイルを JSON 形式でダウンロードし、CLI を使用してそれを再インポートできます。 構成のダウンロードとインポートは、例えば、継続的な統合およびデプロイメント (CICD) パイプラインでの無人デプロイメントなどに役立ちます。 UI を使用して API 定義ファイルのアップロードや再インポートを行うこともできます。



