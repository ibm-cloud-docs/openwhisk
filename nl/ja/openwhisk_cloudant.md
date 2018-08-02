---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Cloudant イベント・ソース
{: #openwhisk_cloudant}

{{site.data.keyword.cloudant}} データベースへの変更を listen し、データベース変更イベントをフィルタリングし、アクション・シーケンスを使用して {{site.data.keyword.cloudant_short_notm}} データベースからの文書を処理する方法を説明します。`/whisk.system/cloudant` パッケージを使用すると、{{site.data.keyword.cloudant_short_notm}} データベースを操作できます。このパッケージには、以下のアクションとフィードが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | パッケージ | dbname、host、username、password | Cloudant データベースを処理。 |
| `/whisk.system/cloudant/read` | アクション | dbname、id | データベースから文書を読み取る。 |
| `/whisk.system/cloudant/write` | アクション | dbname、overwrite、doc | データベースに文書を書き込む。 |
| `/whisk.system/cloudant/changes` | フィード | dbname、filter、query_params、maxTriggers | データベース変更時のトリガー・イベントの起動。 |
{: shortdesc}

以下のセクションでは、関連パッケージを構成する方法、および `/whisk.system/cloudant` パッケージ内のアクションとフィードを使用する方法を順に説明します。{{site.data.keyword.cloudant_short_notm}} データベースのセットアップ、およびそのデータベースの読み取りまたは書き込みについて詳しくは、[{{site.data.keyword.cloudant_short_notm}} アクション](./cloudant_actions.html)を参照してください。

## フィルター関数を使用したトリガーの作成

`changes` フィードを使用して、{{site.data.keyword.cloudant_short_notm}} データベースへの変更があるたびにトリガーを起動するサービスを構成できます。

この例で使用されるパラメーターは、以下のとおりです。

**dbname**: {{site.data.keyword.cloudant_short_notm}} データベースの名前 _(必須)_。

**maxTriggers**: この制限に達するとトリガー起動を停止します _(オプション)_。 デフォルトは無限です。

**filter**: 設計文書に定義されたフィルター関数 _(オプション)_。

**query_params**: フィルター関数の追加の照会パラメーター _(オプション)_。

1. 前に作成したパッケージ・バインディング内の `changes` フィードを使用して、**myCloudantTrigger** という名前のトリガーを作成します。状況が `new` のときに文書が追加または (変更されたら)トリガーを起動するように、`filter` 関数および `query_params` 関数を組み込みます。

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

3. 変更フィードの影響を監視するために使用できるアクションを作成します。例えば、以下の JavaScript コードを含んだ **showCloudantChange** というアクションを作成します。
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

新規アクティベーションを確認できない場合は、{{site.data.keyword.cloudant_short_notm}} データベースの読み取りおよび書き込みの方法を示している、[{{site.data.keyword.cloudant_short_notm}}](./cloudant_actions.html)のトピックを参照してください。読み取りおよび書き込みのステップを試すと、ご使用の {{site.data.keyword.cloudant_short_notm}} 資格情報が正しいかどうかを検証するのに役立ちます。
{: tip}

## トリガー・イベントのデータ構造

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

## データベース変更イベントのフィルタリング

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

## アクション・シーケンスと変更トリガーを使用して {{site.data.keyword.cloudant_short_notm}} データベースからの文書を処理する
{: #openwhisk_catalog_cloudant_read_change notoc}

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

`includeDoc` を使用して以前に作成されたトリガーを作成し直すことができます。 トリガーを作成し直すには、以下のステップを実行します。
```
ibmcloud fn trigger delete myCloudantTrigger
```
{: pre}

```
ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
```
{: pre}

この例を使用して、変更された文書を読み取って既存のアクションを呼び出すアクション・シーケンスを作成できます。 もう有効ではないルールがあれば忘れずに使用不可にし、アクション・シーケンス・パターンを使用して新しく作成するようにしてください。
