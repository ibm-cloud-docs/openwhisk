---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: cloudant, event, action, trigger, sequence

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


# Cloudant
{: #pkg_cloudant}

事前インストール済みの `/whisk.system/cloudant` パッケージを使用すると、[{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) データベースを操作できます。このパッケージを使用するのにサービス・バインディングは不要です。


## 使用可能なアクションとフィード
{: #cloudant_available}

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | パッケージ | dbname、host、username、password | Cloudant データベースを処理。 |
| `/whisk.system/cloudant/read` | アクション | dbname、id | データベースから文書を読み取る。 |
| `/whisk.system/cloudant/write` | アクション | dbname、overwrite、doc | データベースに文書を書き込む。 |
| `/whisk.system/cloudant/changes` | フィード | dbname、iamApiKey、iamUrl、filter、query_params、maxTriggers | データベース変更時のトリガー・イベントの起動。 |
{: shortdesc}

### {{site.data.keyword.Bluemix_notm}} での {{site.data.keyword.cloudant_short_notm}} データベースのセットアップ
{: #cloudant_db}

{{site.data.keyword.Bluemix_notm}} から {{site.data.keyword.openwhisk}} を使用する場合は、{{site.data.keyword.openwhisk}} CLI プラグインを使用して、サービスをアクションまたはパッケージにバインドできます。

まず、{{site.data.keyword.cloudant_short_notm}} アカウント用のパッケージ・バインディングを手動で作成する必要があります。

1. {{site.data.keyword.cloudant_short_notm}} アカウント用に構成されるパッケージ・バインディングを作成します。
  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. このパッケージ・バインディングが存在することを確認します。
  ```
  ibmcloud fn package list
  ```
  {: pre}

  出力例:
  ```
  packages
  /myNamespace/myCloudant private
  ```
  {: screen}

3. アクションまたはパッケージにバインドするサービス・インスタンスの名前を取得します。
    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    出力例:
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

    出力例:
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

    出力例:
    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
        {
                "key": "bluemixServiceName",
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

    この例では、Cloudant サービスの資格情報が `__bx_creds` という名前のパラメーターに入っています。


### {{site.data.keyword.cloudant_short_notm}} データベースからの読み取り
{: #cloudant_read}

アクションを使用して、**testdb** という名前の {{site.data.keyword.cloudant_short_notm}} データベースから文書を取り出すことができます。 このデータベースが、ご使用の {{site.data.keyword.cloudant_short_notm}} アカウントに必ず存在するようにしてください。

- 前に作成したパッケージ・バインディング内で **read** アクションを使用して文書を取り出します。 `/_/myCloudant` は実際のパッケージ名に置き換えてください。
  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}

  出力例:
  ```
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```
  {: screen}

### {{site.data.keyword.cloudant_short_notm}} データベースへの書き込み
{: #cloudant_write}

アクションを使用して、**testdb** という名前の {{site.data.keyword.cloudant_short_notm}} データベース内に文書を保管できます。 このデータベースが、ご使用の {{site.data.keyword.cloudant_short_notm}} アカウントに必ず存在するようにしてください。

1. 前に作成したパッケージ・バインディング内で **write** アクションを使用して文書を保管します。 `/_/myCloudant` は実際のパッケージ名に置き換えてください。
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  {: pre}

  出力例:
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. {{site.data.keyword.cloudant_short_notm}} ダッシュボードで文書を参照して、文書が存在していることを確認します。

  **testdb** データベースのダッシュボード URL は、`https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100` のようになります。


### フィルター関数を使用したトリガーの作成
{: #cloudant_trigger}

`changes` フィードを使用して、{{site.data.keyword.cloudant_short_notm}} データベースへの変更があるたびにトリガーを起動するサービスを構成できます。

この例で使用されるパラメーターは、以下のとおりです。

**dbname**: {{site.data.keyword.cloudant_short_notm}} データベースの名前 _(必須)_。

**iamApiKey**: Cloudant データベースの IAM API キー。  指定した場合は、ユーザー名とパスワードの代わりに資格情報として使用されます _(オプション)_。

**iamUrl**: `iamApiKey` が指定されている場合に使用される IAM トークン・サービスの URL。  デフォルトは `https://iam.bluemix.net/identity/token` _(オプション)_ です。

**maxTriggers**: この制限に達するとトリガー起動を停止します _(オプション)_。 デフォルトは無限です。

**filter**: 設計文書に定義されたフィルター関数 _(オプション)_。

**query_params**: フィルター関数の追加の照会パラメーター _(オプション)_。

1. 前に作成したパッケージ・バインディング内の `changes` フィードを使用して、**myCloudantTrigger** という名前のトリガーを作成します。 状況が `new` のときに文書が追加または (変更されたら)トリガーを起動するように、`filter` 関数および `query_params` 関数を組み込みます。

  `/_/myCloudant` は実際のパッケージ名に置き換えてください。
  ```
  ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes \
  --param dbname testdb \
  --param filter "mailbox/by_status" \
  --param query_params '{"status":"new"}'
  ```
  {: pre}

  出力例:
  ```
  ok: created trigger feed myCloudantTrigger
  ```
  {: screen}

2. アクティベーションのポーリングを開始して、何が起こっているのかを明確に可視化します。
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

3. 変更フィードの影響を監視するために使用できるアクションを作成します。 例えば、以下の JavaScript コードを含んだ **showCloudantChange** というアクションを作成します。
  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

4. **showCloudantChange** アクションを前に作成したトリガーに接続するルールを作成します。
  ```
  ibmcloud fn rule update aCloudantRule myCloudantTrigger showCloudantChange
  ```
  {: pre}

5. アクションおよびそれらのアクションを **myCloudantTrigger** トリガーに関連付けるルールを作成します。

6. {{site.data.keyword.cloudant_short_notm}} ダッシュボードで、既存の文書を変更するか、新しい文書を作成します。 文書には、**new** が設定される _status_ フィールドが必要です。

7. フィルター関数および照会パラメーターに基づいて、文書の状況が **new** の場合のみ、文書変更のたびに **myCloudantTrigger** トリガーの新規アクティベーションがあることを確認します。

読み取りおよび書き込みのステップを試すと、ご使用の {{site.data.keyword.cloudant_short_notm}} 資格情報が正しいかどうかを検証するのに役立ちます。

### トリガー・イベントのデータ構造
{: #cloudant_struct}

生成されるイベントの内容には以下のパラメーターが含まれます。

  - `id`: 文書 ID。
  - `seq`: {{site.data.keyword.cloudant_short_notm}} によって生成されるシーケンス ID。
  - `changes`: オブジェクトの配列。各オブジェクトに `rev` フィールドがあり、文書の改訂 ID が含まれます。

トリガー・イベントの JSON 表記は次のとおりです。
```json
{
    "dbname": "testdb",
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

### データベース変更イベントのフィルタリング
{: #cloudant_filter}

オプションでフィルター関数を定義して、トリガーを起動する必要のない変更イベントを回避できます。

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
{: codeblock}

以下のフィルター関数を使用してデータベースに設計文書を作成します。
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
```
{: pre}

新しい設計文書の情報が画面に表示されます。
```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}

### アクション・シーケンスと変更トリガーを使用して {{site.data.keyword.cloudant_short_notm}} データベースからの文書を処理する
{: #cloudant_seq}

ルール内のアクション・シーケンスを使用して、{{site.data.keyword.cloudant_short_notm}} 変更イベントに関連付けられた文書を取り出して処理することができます。

以下は、文書を処理するアクションのサンプル・コードです。
```javascript
function main(doc){
  return { "isWalter:" : doc.name === "Walter White"};
}
```
{: codeblock}

{{site.data.keyword.cloudant_short_notm}} からの文書を処理するためのアクションを作成します。
```
ibmcloud fn action create myAction myAction.js
```
{: pre}

データベースから文書を読み取るために、{{site.data.keyword.cloudant_short_notm}} パッケージから `read` アクションを使用できます。
`read` アクションを `myAction` と組み合わせて、アクション・シーケンスを作成できます。
```
ibmcloud fn action create sequenceAction --sequence /_/myCloudant/read,myAction
```
{: pre}

`sequenceAction` アクションを、新規 {{site.data.keyword.cloudant_short_notm}} トリガー・イベントでこのアクションを活動化するルール内で使用できます。
```
ibmcloud fn rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**注:** {{site.data.keyword.cloudant_short_notm}} `changes` トリガーは、以前はパラメーター `includeDoc` をサポートしていましたが、これはもうサポートされなくなりました。

`includeDoc` を使用して過去に作成したトリガーを作成し直すことができます。トリガーを作成し直すには、以下の手順に従います。
```
ibmcloud fn trigger delete myCloudantTrigger
```
{: pre}

```
ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
```
{: pre}

この例を使用して、変更された文書を読み取って既存のアクションを呼び出すアクション・シーケンスを作成できます。 もう有効ではないルールがあれば忘れずに使用不可にし、アクション・シーケンス・パターンを使用して新しく作成するようにしてください。

