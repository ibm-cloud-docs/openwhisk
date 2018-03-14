---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Cloudant パッケージの使用
{: #openwhisk_catalog_cloudant}
`/whisk.system/cloudant` パッケージを使用すると、Cloudant データベースを処理することができます。これには、以下のアクションおよびフィードが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | パッケージ | dbname、host、username、password| Cloudant データベースを処理|
| `/whisk.system/cloudant/read` | アクション | dbname、id| データベースから文書を読み取る|
| `/whisk.system/cloudant/write` | アクション | dbname、overwrite、doc| データベースに文書を書き込む|
| `/whisk.system/cloudant/changes` | フィード | dbname、filter、query_params、maxTriggers| データベース変更時のトリガー・イベントの起動|
{: shortdesc}

以降のトピックでは、Cloudant データベースのセットアップ、関連付けられたパッケージの構成、および `/whisk.system/cloudant` パッケージ内のアクションとフィードの使用方法をウォークスルーします。

## {{site.data.keyword.Bluemix_notm}} での Cloudant データベースのセットアップ
{: #openwhisk_catalog_cloudant_in}

{{site.data.keyword.Bluemix_notm}} から OpenWhisk を使用する場合、Cloudant サービス・インスタンス用のパッケージ・バインディングを OpenWhisk が自動的に作成します。OpenWhisk および Cloudant を {{site.data.keyword.Bluemix_notm}} から使用しない場合は、次のステップまでスキップしてください。

1. {{site.data.keyword.Bluemix_notm}} [ダッシュボード](http://console.ng.Bluemix.net)で Cloudant サービス・インスタンスを作成します。

  新規サービス・インスタンスごとに 1 つの資格情報キーを作成するように注意してください。

2. 名前空間内のパッケージをリフレッシュします。このリフレッシュにより、資格情報キーが定義された各 Cloudant サービス・インスタンスごとに 1 つのパッケージ・バインディングが自動的に作成されます。

  ```
  wsk package refresh
  ```
  {: pre}
  ```
  created bindings:
  Bluemix_testCloudant_Credentials-1
  ```

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 private binding
  ```

  これで、パッケージ・バインディングには、Cloudant サービス・インスタンスと関連付けられた資格情報が含まれるようになります。

3. 前に作成されたパッケージ・バインディングが Cloudant {{site.data.keyword.Bluemix_notm}} サービス・インスタンスのホストと資格情報で構成されていることを確認します。

  ```
  wsk package get /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 parameters
  ```
  {: pre}
  ```
  ok: got package /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1, displaying field parameters
  ```
  ```json
  [
      {
          "key": "username",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix"
      },
      {
          "key": "host",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix.cloudant.com"
      },
      {
          "key": "password",
          "value": "c9088667484a9ac827daf8884972737"
      }
  ]
  ```

## {{site.data.keyword.Bluemix_notm}} 外部での Cloudant データベースのセットアップ
{: #openwhisk_catalog_cloudant_outside}

OpenWhisk を {{site.data.keyword.Bluemix_notm}} で使用しない場合、または、Cloudant データベースを {{site.data.keyword.Bluemix_notm}} の外部でセットアップしたい場合は、ご使用の Cloudant アカウント用のパッケージ・バインディングを手動で作成する必要があります。Cloudant アカウントのホスト名、ユーザー名、およびパスワードが必要です。

1. Cloudant アカウント用に構成されるパッケージ・バインディングを作成します。

  ```
  wsk package bind /whisk.system/cloudant myCloudant -p username MYUSERNAME -p password MYPASSWORD -p host MYCLOUDANTACCOUNT.cloudant.com
  ```
  {: pre}
  

2. このパッケージ・バインディングが存在することを確認します。

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myNamespace/myCloudant private binding
  ```


## Cloudant データベースに対する変更の listen
{: #openwhisk_catalog_cloudant_listen}

### データベース変更イベントのフィルタリング

フィルター関数を定義して、不必要な変更イベントがトリガーを起動するのを回避できます。

新規フィルター関数を作成するために、アクションを使用できます。

以下のフィルター関数を含む json 文書ファイル `design_doc.json` を作成します。
```json
{
  "doc": {
    "_id": "_design/mailbox",
    "filters": {
      "by_status": "function(doc, req){if (doc.status != req.query.status){return false;} return true;}"
    }
  }
}
```

このフィルター関数を使用してデータベースに設計文書を作成します。
```
wsk action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
```
新規設計文書の情報が画面に表示されます。
```json
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```

### フィルター関数を使用したトリガーの作成

`changes` フィードを使用して、Cloudant データベースへの変更があるたびにトリガーを起動するサービスを構成できます。パラメーターは次のとおりです。

- `dbname`: Cloudant データベースの名前。
- `maxTriggers`: この制限に達するとトリガー起動を停止します。デフォルトは無限です。
- `filter`: 設計文書に定義されたフィルター関数。
- `query_params`: フィルター関数の追加の照会パラメーター。


1. 前に作成したパッケージ・バインディング内で `changes` フィードを使用してトリガーを作成します。状況が `new` のときに文書が追加または変更されたらトリガーを起動するように、`filter` 関数および `query_params` 関数を組み込みます。
`/_/myCloudant` は実際のパッケージ名に置き換えてください。

  ```
  wsk trigger create myCloudantTrigger --feed /_/myCloudant/changes \
  --param dbname testdb \
  --param filter "mailbox/by_status" \
  --param query_params '{"status":"new"}'
  ```
  {: pre}
  ```
  ok: created trigger feed myCloudantTrigger
  ```

2. アクティベーションをポーリングします。

  ```
  wsk activation poll
  ```
  {: pre}

3. Cloudant ダッシュボードで、既存の文書を変更するか、新しい文書を作成します。

4. フィルター関数および照会パラメーターに基づいて、文書の状況が `new` の場合のみ、文書変更のたびに `myCloudantTrigger` トリガーの新規アクティベーションがあることを確認します。
  
  **注:** 新規アクティベーションを確認できない場合は、Cloudant データベースの読み取りおよび書き込みに関する後続のセクションを参照してください。以降の読み取りおよび書き込みのステップを試すと、Cloudant 資格情報が正しいことを検証するのに役立ちます。
  
  これで、文書更新に反応できるようにするために、ルールを作成し、それらをアクションに関連付けられるようになりました。
  
  生成されるイベントの内容には以下のパラメーターが含まれます。
  
  - `id`: 文書 ID。
  - `seq`: Cloudant によって生成されるシーケンス ID。
  - `changes`: オブジェクトの配列。各オブジェクトに `rev` フィールドがあり、文書の改訂 ID が含まれます。
  
  トリガー・イベントの JSON 表記は次のとおりです。
  
  ```json
  {
      "id": "6ca436c44074c4c2aa6a40c9a188b348",
      "seq": "2-g1AAAAL9aJyV-GJCaEuqx4-BktQkYp_dmIfC",
      "changes": [
          {
              "rev": "2-da3f80848a480379486fb4a2ad98fa16"
          }
      ]
  }
  ```
  
## Cloudant データベースへの書き込み
{: #openwhisk_catalog_cloudant_write}

アクションを使用して、`testdb` という名前の Cloudant データベース内に文書を保管できます。このデータベースが、ご使用の Cloudant アカウントに必ず存在するようにしてください。

1. 前に作成したパッケージ・バインディング内で `write` アクションを使用して文書を保管します。`/_/myCloudant` は実際のパッケージ名に置き換えてください。

  ```
  wsk action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287
  ```
  ```json
  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```

2. Cloudant ダッシュボードで文書を参照して、文書が存在していることを確認します。

  `testdb` データベースのダッシュボード URL は、`https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100` のようになります。


## Cloudant データベースからの読み取り
{: #openwhisk_catalog_cloudant_read}

アクションを使用して、`testdb` という名前の Cloudant データベースから文書を取り出すことができます。このデータベースが、ご使用の Cloudant アカウントに必ず存在するようにしてください。

- 前に作成したパッケージ・バインディング内で `read` アクションを使用して文書を取り出します。`/_/myCloudant` は実際のパッケージ名に置き換えてください。

  ```
  wsk action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}
  ```json
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```

## アクション・シーケンスと変更トリガーを使用して Cloudant データベースからの文書を処理する
{: #openwhisk_catalog_cloudant_read_change notoc}

ルール内のアクション・シーケンスを使用して、Cloudant 変更イベントに関連付けられた文書を取り出して処理することができます。

以下は、文書を処理するアクションのサンプル・コードです。
```javascript
function main(doc){
  return { "isWalter:" : doc.name === "Walter White"};
}
```

Cloudant からの文書を処理するためのアクションを作成します。
```
wsk action create myAction myAction.js
```
{: pre}

データベースから文書を読み取るために、Cloudant パッケージから `read` アクションを使用できます。
`read` アクションを `myAction` と組み合わせて、アクション・シーケンスを作成できます。
```
wsk action create sequenceAction --sequence /_/myCloudant/read,myAction
```
{: pre}

`sequenceAction` アクションを、新規 Cloudant トリガー・イベントでこのアクションを活動化するルール内で使用できます。
```
wsk rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**注:** Cloudant `changes` トリガーでサポートされていた `includeDoc` パラメーターはもうサポートされていません。`includeDoc` を使用して以前に作成されたトリガーを作成し直すことができます。トリガーを作成し直すには、以下のステップを実行します。 
  ```
  wsk trigger delete myCloudantTrigger
  ```
  {: pre}
  ```
  wsk trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
  ```
  {: pre}

  この例を使用して、変更された文書を読み取って既存のアクションを呼び出すアクション・シーケンスを作成できます。
  もう有効ではないルールがあれば忘れずに使用不可にし、アクション・シーケンス・パターンを使用して新しく作成するようにしてください。

