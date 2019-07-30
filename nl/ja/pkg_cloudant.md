---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: cloudant, event, action, trigger, sequence, functions

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


# Cloudant
{: #pkg_cloudant}

事前インストール済みの `/whisk.system/cloudant` パッケージを使用して、[{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) データベースを操作できます。このパッケージを使用するのにサービス・バインディングは不要です。
{: shortdesc}


## 使用可能なエンティティー
{: #cloudant_available}
次の表は、`whisk.system/cloudant` パッケージで使用可能なエンティティーを抜粋したものです。`whisk.system/cloudant` パッケージを使用して、文書の読み取り、書き込み、更新、または削除を行えます。`changes` フィードを使用して、{{site.data.keyword.cloudant_short_notm}} データベースの変更を listen することもできます。
{: shortdesc}

`/whisk.system/cloudant` パッケージで使用可能なエンティティーの完全なリストについては、`ibmcloud fn package get/whisk.system/cloudant` を実行してください。
{: note}

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | パッケージ | `dbname`、`host`、`username`、`password` | Cloudant データベースを処理。 |
| `/whisk.system/cloudant/read` | アクション | `dbname`、`id` | データベースから文書を読み取る。 |
| `/whisk.system/cloudant/write` | アクション | `dbname`、`overwrite`、`doc` | データベースに文書を書き込む。 |
| `/whisk.system/cloudant/update-document` | アクション | `dbname`、`doc` | データベース内の文書を更新する。 |
| `/whisk.system/cloudant/changes` | フィード | `dbname`、`iamApiKey`、`iamUrl`、`filter`、`query_params`、`maxTriggers` | データベース変更時のトリガー・イベントの起動。 |

`includeDoc` パラメーターは、`/whisk.system/cloudant/changes` フィードでの使用はサポートされなくなりました。このパラメーターを使用するトリガーを作成した場合は、`includeDoc` パラメーターを使用せずに再作成する必要があります。
{: deprecated}

## {{site.data.keyword.cloudant_short_notm}} データベースへの `/whisk.system/cloudant` パッケージのバインド
{{site.data.keyword.cloud_notm}} から {{site.data.keyword.openwhisk}} を使用する場合は、{{site.data.keyword.openwhisk}} CLI プラグインを使用して、サービスをアクションまたはパッケージにバインドできます。
{: #cloudant_db}

**始める前に**
{{site.data.keyword.cloudant_short_notm}} のインスタンスが必要です。インスタンスを作成するには、[{{site.data.keyword.cloudant_short_notm}} の概要](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)を参照してください。

1. {{site.data.keyword.cloudant_short_notm}} アカウント用に構成される `/whisk.system/cloudant` パッケージ・バインディングを作成します。 この例では、パッケージ名は `myCloudant` です。

  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. このパッケージ・バインディングが存在することを確認します。

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **出力例**

  ```
  packages
  /<namespace>/myCloudant private
  ```
  {: screen}

3. アクションまたはパッケージにバインドするサービス・インスタンスの名前を取得します。

    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    **出力例**
    ```
    Name          Location   State    Type
    Cloudant-gm   us-south   active   service_instance
    ```
    {: screen}

4. 前のステップで取得したサービス・インスタンスに定義された資格情報の名前を取得します。

    ```
    ibmcloud resource service-keys --instance-name Cloudant-gm
    ```
    {: pre}

    **出力例**

    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. ステップ 1 で作成したパッケージにサービスをバインドします。

    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1'
    ```
    {: pre}

6. 資格情報が正常にバインドされたことを確認します。
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    **出力例**

    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
        {
                "key": "serviceName",
                "value": "cloudantNoSQLDB"
            },
            {
                "key": "apihost",
                "value": "us-south.functions.cloud.ibm.com"
            },
            {
                "key": "__bx_creds",
            "value": {
                    "cloudantnosqldb": {
                        "apikey": "[Service apikey]",
                        "credentials": "Service credentials-1",
                        "iam_apikey_description": "[Service description]",
                        "iam_apikey_name": "[Service apikey name]",
                        "iam_role_crn": "[Service role crn]",
                        "iam_serviceid_crn": "[Service id crn]",
                        "instance": "Cloudant-gm",
                        "url": "[Service url]",
                        "username": "[Service username]"
                    }
                }
            }
        ],
    }
    ```
    {: screen}

この例では、{{site.data.keyword.cloudant_short_notm}} サービスの資格情報が `__bx_creds` という名前のパラメーターに入っています。

## {{site.data.keyword.cloudant_short_notm}} データベース内の文書の処理
{: #cloudant_read}

アクションを使用して、{{site.data.keyword.cloudant_short_notm}} データベースの文書の読み取り、書き込み、更新、削除を行えます。
{: shortdesc}

### 文書の読み取り
`/whisk.system/cloudant/read` アクションを使用して、{{site.data.keyword.cloudant_short_notm}} データベースから文書を読み取ることができます。

**始める前に**
{{site.data.keyword.cloudant_short_notm}} データベースに文書がない場合は、{{site.data.keyword.cloudant_short_notm}} ダッシュボードを使用して作成できます。ダッシュボードの URL は、`https://<mycloudantaccount>.cloudant.com/dashboard.html#database/<database_name>/_all_docs?limit=100` です。

`read` アクションを使用して文書を取り出します。 `/_/myCloudant` をパッケージ名に、`<database_name>` をデータベース名に、`<document_id>` をファイル ID にそれぞれ置き換えてください。文書の取り出しをテストするアクションを呼び出します。

**コマンド構文**

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id <document_id>
  ```
  {: pre}

**`test` データベースからの読み取りアクションの例**
ファイルの読み取りをテストするアクションを呼び出します。この例では、`id` が `9f86f4955e7a38ab0169462e6ac0f476` であるファイル (空のファイル) を読み取ります。

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id 9f86f4955e7a38ab0169462e6ac0f476
  ```
  {:pre}

**出力例**
  ```
  {
      "_id": "9f86f4955e7a38ab0169462e6ac0f476",
      "_rev": "1-967a00dff5e02add41819138abb3284d"
  }
  ```
  {: screen}

### {{site.data.keyword.cloudant_short_notm}} データベースへの文書の書き込み
{: #cloudant_write}

アクションを使用して、{{site.data.keyword.cloudant_short_notm}} データベース内の文書の作成または更新を行えます。
{: shortdesc}

**始める前に**
  {{site.data.keyword.cloudant_short_notm}} アカウント用に構成される `/whisk.system/cloudant` [パッケージ・バインディング](#cloudant_db)を作成します。

1. 作成したパッケージ・バインディング内で `write` アクションを使用して文書を保管します。 `/_/myCloudant` をパッケージ名に置き換え、`<database_name>` をデータベースの名前に、`<document_id>` を文書 ID に、`<test_name>` を名前にそれぞれ置き換えてください。

  **コマンド構文**
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param <database_name> test --param doc "{\"_id\":\"<document_id>\",\"name\":\"<test_name>\"}"
  ```
  {: pre}

  **`test` データベースへの書き込みアクションの例**

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\"}"
  ```
  {: pre}

  **出力例**

  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "color",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. {{site.data.keyword.cloudant_short_notm}} ダッシュボードで、文書が存在することを確認します。`test` データベースのダッシュボード URL の形式は、`https://<mycloudantaccount>.cloudant.com/dashboard.html#database/test/_all_docs?limit=100` です。

  **{{site.data.keyword.cloudant_short_notm}} ダッシュボードでの文書例**
  ```
  {
  "_id": "color",
  "_rev": "1-f413f4b74a724e391fa5dd2e9c8e9d3f",
  "name": "blue"
  }
  ```
  {: screen}

### 文書の更新
`/update-document` アクションを使用して、{{site.data.keyword.cloudant_short_notm}} データベース内の文書を更新できます。
{: short desc}

**始める前に**
  {{site.data.keyword.cloudant_short_notm}} アカウント用に構成される `/whisk.system/cloudant` [パッケージ・バインディング](#cloudant_db)を作成します。

以下の例では、[{{site.data.keyword.cloudant_short_notm}} データベースへの文書の書き込み](#cloudant_write)のセクションで作成された文書を更新します。
{: note}

データベース内の文書を更新するには、`<test>` をデータベース名に置き換え、`--param doc` フラグをデータベース内の更新する文書の `id` および内容に置き換えます。


1. 次のコマンドを実行することによって、`test` データベース内の文書を更新できます。この例では、`color` 文書に `shade`値を追加します。 

  ```
  ibmcloud fn action invoke /_/myCloudant/update-document --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}"
  ```
  {: pre}

  **出力**
  ```
  {
    "id": "color",
    "ok": true,
    "rev": "2-8b904347bfe52e0f388ef6f39d6ba84f"
    }
  ```
  {: screen}

2. 更新を確認するために、文書を再度取り出します。

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id color
  ```
  {: pre}

  **文書例**
  ```
  {
    "_id": "color",
    "_rev": "2-8b904347bfe52e0f388ef6f39d6ba84f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

## フィルター関数を使用したトリガーの作成
{: #cloudant_trigger}

`changes` フィードを使用して、{{site.data.keyword.cloudant_short_notm}} データベースへの変更があるたびにトリガーを起動するサービスを構成できます。

**始める前に**
  {{site.data.keyword.cloudant_short_notm}} アカウント用に構成される `/whisk.system/cloudant` [パッケージ・バインディング](#cloudant_db)を作成します。

この例で使用されるパラメーター。

| パラメーター | 説明 |
| --- | --- |
| `dbname` | (必須) {{site.data.keyword.cloudant_short_notm}} データベースの名前。 |
| `iamApiKey` | (オプション) Cloudant データベースの IAM API キー。  指定した場合は、ユーザー名とパスワードの代わりに資格情報として使用されます。 |
| `iamUrl` | (オプション) `iamApiKey` が指定されている場合に使用される IAM トークン・サービスの URL。  デフォルト値は `https://iam.cloud.ibm.com/identity/token` です。 | 
| `maxTriggers` | (オプション) この限界に達するとトリガーの発生を停止します。 デフォルトは無限です。 |
| `filter` | (オプション) 設計文書に定義されているフィルター関数。 |
| `query_params` | (オプション) フィルター関数に必要な場合に追加する照会パラメーター。 |
| `includeDoc` | (非推奨) `includeDoc` パラメーターは、`/whisk.system/cloudant/changes` フィードでの使用はサポートされなくなりました。 |

</br>

1. 前に作成したパッケージ・バインディング内の `changes` フィードを使用して、`cloudantTrigger` という名前のトリガーを作成します。 状況が `new` のときに文書が追加または (変更されたら)トリガーを起動するように、`filter` 関数および `query_params` 関数を組み込みます。

  `/_/myCloudant` をパッケージの名前に置き換えてください。 この例では、`test`という名前のデータベースを使用します。
  ```
  ibmcloud fn trigger create cloudantTrigger --feed /_/myCloudant/changes \ --param dbname test
  ```
  {: pre}

  **出力例**

  ```
  ok: created trigger feed cloudantTrigger
  ```
  {: screen}

2. 次の JavaScript コードを `cloudantChange.js` として保存します。

  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

3. 変更フィードを監視するために使用できる `cloudantChange` というアクションを作成します。 `<file_path>` をコンピューター上の `cloudantChange.js` ファイルのファイル・パスに置き換えてください。

  ```
  ibmcloud fn action create cloudantChange <file_path>/cloudantChange.js
  ```
  {: pre}

4. 前に作成した `cloudantTrigger` に `cloudantChange` アクションを接続するための `cloudantRule` という名前のルールを作成します。

  ```
  ibmcloud fn rule create cloudantRule cloudantTrigger cloudantChange
  ```
  {: pre}

5. 別の端末ウィンドウで、アクティベーションがいつ行われたかを確認できるように、ポーリングを開始します。

  ```
  ibmcloud fn activation poll
  ```
  {: pre}

6. {{site.data.keyword.cloudant_short_notm}} ダッシュボードで、既存の文書を変更するか、文書を作成します。

7. 文書変更ごとに `cloudantTrigger` トリガーのアクティベーションを監視します。

**`cloudantTrigger` のアクティベーション例**

```
Activation: 'cloudantTrigger' (ef6605cc05e04589a605cc05e04589d8)
[
    "{\"statusCode\":0,\"success\":true,\"activationId\":\"6067ed0d28774a68a7ed0d28771a684d\",\"rule\":\"<namespace>/cloudantRule\",\"action\":\"<namespace>/cloudantChange\"}"
]

Activation: 'cloudantChange' (6067ed0d28774a68a7ed0d28771a684d)
[
    "2019-06-24T16:46:10.428643Z    stdout: { changes: [ { rev: '19-f7f6d8607d6381d224321acfcfb8887e' } ],",
    "2019-06-24T16:46:10.428693Z    stdout: dbname: 'test',",
    "2019-06-24T16:46:10.428697Z    stdout: id: '6ca436c44074c4c2aa6a40c9a188b348',",
    "2019-06-24T16:46:10.428700Z    stdout: seq: '103-g1AAAAeLeJy91M9NwzAUBnCrrVQqDvTKCa4gpcT5YycnugFsAH5' }"
```
{: screen}

### トリガー・アクティベーションのデータ構造
{: #cloudant_struct}

生成されるイベントの内容には以下のパラメーターが含まれます。

| パラメーター | 説明 |
| --- | --- |
| `id` | 文書 ID。 |
| `seq` | {{site.data.keyword.cloudant_short_notm}} によって生成されるシーケンス ID。 |
| `changes` | オブジェクトの配列。各オブジェクトに `rev` フィールドがあり、文書の改訂 ID が含まれます。 |

**トリガー・アクティベーションの JSON 表記**

```json
{
    "dbname": "test",
    "id": "6ca436c44074c4c2aa6a40c9a188b348",
    "seq": "2-g1AAAAL9aJyV-GJCaEuqx4-BktQkYp_dmIfC",
    "changes": [
        {
            "rev": "2-da3f80848a480379486fb4a2ad98fa16"
          }
    ]
}
```
{: codeblock}

## データベース変更イベントのフィルタリング
{: #cloudant_filter}

フィルター関数を定義することによって、不要な変更イベントがトリガーを起動しないようにすることもできます。

**始める前に**
  {{site.data.keyword.cloudant_short_notm}} アカウント用に構成される `/whisk.system/cloudant` [パッケージ・バインディング](#cloudant_db)を作成します。

フィルター関数を作成するために、アクションを使用できます。

1. 次の JSON フィルターを `design_doc.json` というファイルに保存します。

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
  {: codeblock}

2. 次のフィルター関数を使用して、データベースに設計文書を作成します。`<database_name>` をデータベースの名前に、`<file_path>` を `design_doc.json` のファイル・パスにそれぞれ置き換えてください。`write` アクションを呼び出して、設計文書の作成をテストします。

**コマンド構文**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname <database_name> -p overwrite true -P <file_path>/design_doc.json
```
{: pre}

**`test` データベースに `design_doc.json` ファイルを書き込むコマンドの例**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname test -p overwrite true -P <file_path>/design_doc.json -r
```
{: pre}

**出力例**

```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}


{{site.data.keyword.cloudant_short_notm}} 設計文書について詳しくは、[設計文書](/docs/services/Cloudant?topic=cloudant-design-documents)を参照してください。

## アクション・シーケンスを使用した個々の文書の処理
{: #cloudant_seq}

ルール内のアクション・シーケンスを使用して、{{site.data.keyword.cloudant_short_notm}} 変更イベントに関連付けられた文書を取り出して処理することができます。

**始める前に**
  {{site.data.keyword.cloudant_short_notm}} アカウント用に構成される `/whisk.system/cloudant` [パッケージ・バインディング](#cloudant_db)を作成します。 

この例では、[{{site.data.keyword.cloudant_short_notm}} データベースへの文書の書き込み](#cloudant_write)のセクションで作成された文書を更新します。
{: note}

### 個々の文書を処理するアクションの作成

個々の文書に対する変更を処理するアクションを作成するには、以下のコマンドを実行します。
{: shortdesc}

1. 次のコードを `docChange.js` として保存します。

  ```javascript
  function main(doc){
    return { "isBlue:" : doc.name === "blue"};
  }
  ```
  {: codeblock}

2. `docChange` というアクションを作成して、前に作成した `blue` という名前の文書を処理します。`<file_path>` を `docChange.js` のファイル・パスに置き換えてください。

  ```
  ibmcloud fn action create docChange <file_path>/docChange.js
  ```
  {: pre}

  **出力**
  ```
  ok: created action docChange
  ```
  {: screen}

### `read` アクションを含むシーケンスの作成 

`read` アクションを `docChange` アクションと組み合わせて、アクション・シーケンスを作成できます。
{: shortdesc}

  ```
  ibmcloud fn action create docSequence --sequence /_/myCloudant/read,docChange
  ```
  {: pre}

  **出力**
  ```
  ok: created action docSequence
  ```
  {: screen}

### `changes` フィードを使用したトリガーの作成

  ```
  ibmcloud fn trigger create docTrigger --feed /_/myCloudant/changes \
  --param dbname test
  ```
  {: pre}

### トリガーをシーケンスに関連付けるルールの作成

`docSequence` アクションを、新規 {{site.data.keyword.cloudant_short_notm}} トリガー・イベントでこのアクションを活動化するルール内で使用できます。

  ```
  ibmcloud fn rule create docRule docTrigger docSequence
  ```
  {: pre}

  **出力**
  ```
  ok: created rule docRule
  ```

  **アクティベーションの例**
  ```
  "{\"statusCode\":0,\"success\":true,\"activationId\":\"144a4f95198a49ec8a4f95198a79ecc8\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ```
  {: screen}

### シーケンスのテスト

1. 前に作成した `blue` ファイルに変更を加えて、`docSequence` をテストします。 この例では、`shade` 値が `indigo` に変更されます。

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}" -p overwrite true
  ```
  {: pre}

  **アクティベーションの例**

  ```
  Activation: 'docChange' (aa3e8fc3030446b2be8fc3030406b2eb)
  []

  Activation: 'docSequence' (23e0a17bebd3486ca0a17bebd3186c8d)
  [
      "8d42679127f3400382679127f300039d",
      "aa3e8fc3030446b2be8fc3030406b2eb"
  ]

  Activation: 'docTrigger' (db6de778bb084366ade778bb08036685)
  [
      "{\"statusCode\":0,\"success\":true,\"activationId\":\"23e0a17bebd3486ca0a17bebd3186c8d\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ]
  ```
  {: screen}

2. `read` アクションを呼び出して、`shade` 値を含むようにファイルが更新されたことを確認します。`<database>` 名をデータベースの名前に置き換えてください。

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id color
  ```
  {: pre}

  **出力**
  ```
  {
    "_id": "color",
    "_rev": "3-6845b04618338f717676f16edf32a78f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

### 次のステップ
{{site.data.keyword.cloudant_short_notm}} データベース内の文書に対する変更を listen しているので、[`/whisk.system/slack` パッケージ](/docs/openwhisk?topic=cloud-functions-pkg_slack)を使用して、変更についての Slack 通知をトリガーできます。


