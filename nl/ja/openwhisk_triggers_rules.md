---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 一般的な概念
{: #openwhisk_triggers}

{{site.data.keyword.openwhisk_short}} のトリガーおよびルールにより、プラットフォームにイベント・ドリブンの機能が実現されます。 外部および内部のイベント・ソースからのイベントがトリガーを通して送信され、アクションがそうしたイベントに反応することがルールによって可能になります。
{: shortdesc}

## トリガーとは
{: #openwhisk_triggers_create}

トリガーは、ある種のイベントに対して指定されたチャネルです。 トリガーの例を以下に示します。
- ロケーション更新イベントのトリガー。
- Web サイトへの文書アップロードのトリガー。
- 着信 E メールのトリガー。

トリガーは、キーと値のペアのディクショナリーを使用して*起動する* (アクティブ化する) ことができます。 このディクショナリーは、*イベント* と呼ばれることもあります。 アクションと同様に、トリガーの起動のたびに、**アクティベーション ID** が生成されます。

トリガーは、ユーザーが明示的に起動することも、ユーザーの代わりに外部イベント・ソースによって起動することもできます。
*フィード* は、{{site.data.keyword.openwhisk_short}} で消費可能なトリガー・イベントを起動するように、外部イベント・ソースを構成するための便利な方法です。 以下のフィードの例を参照してください。
- データベース内の文書が追加または変更されるたびにトリガー・イベントを起動する、{{site.data.keyword.cloudant}} データ変更フィード。
- Git リポジトリーに対するコミットのたびにトリガー・イベントを起動する Git フィード。

## ルールがトリガーに影響する仕組み
{: #openwhisk_rules_use}

1 つのルールは 1 つのトリガーを 1 つのアクションに関連付けます。トリガーが発生するたびに、対応するアクションが、トリガー・イベントを入力として呼び出されます。

適切なルール・セットを使用して、単一のトリガー・イベントが
複数のアクションを呼び出すことも、複数のトリガーからのイベントに対する応答として 1 つのアクションを呼び出すこともできます。

例えば、以下のアクションを持つシステムがあるとします。
- `classifyImage` - イメージ内のオブジェクトを検出し、それらを分類するアクション。
- `thumbnailImage` - イメージのサムネール・バージョンを作成するアクション。

また、2 つのイベント・ソースが以下のトリガーを起動するとします。
- `newTweet` - 新規ツイートが投稿されたときに起動するトリガー。
- `imageUpload` - Web サイトにイメージがアップロードされたときに起動するトリガー。

単一のトリガー・イベントが複数のアクションを起動し、複数のトリガーが同じアクションを呼び出すように、ルールをセットアップすることができます。
- `newTweet -> classifyImage` ルール。
- `imageUpload -> classifyImage` ルール。
- `imageUpload -> thumbnailImage` ルール。

この 3 つのルールによって、以下の動作が設定されます。
- 各ツイートのイメージが分類されます。
- アップロードされたイメージが分類されます。
- サムネールのバージョンが生成されます。

## トリガーの作成と起動
{: #openwhisk_triggers_fire}

トリガーは、特定のイベントの発生時に起動することも、手動で起動することもできます。

例として、ユーザー・ロケーションの更新を送信するトリガーを作成し、手動でトリガーを起動します。
1. 以下のコマンドを入力してトリガーを作成します。
  ```
  ibmcloud fn trigger create locationUpdate
  ```
  {: pre}

  出力例:
  ```
  ok: created trigger locationUpdate
  ```
  {: screen}

2. トリガーのセットをリストして、トリガーを作成したことを確認します。
  ```
  ibmcloud fn trigger list
  ```
  {: pre}

  出力例:
  ```
  triggers
  /someNamespace/locationUpdate                            private
  ```
  {: screen}

  これで、イベントを起動できる、指定された「チャネル」が作成されました。

3. 次に、トリガー名とパラメーターを指定することにより、トリガー・イベントを起動します。
  ```
  ibmcloud fn trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  出力例:
  ```
  ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
  ```
  {: screen}

発生したトリガーは、それに突き合わせる付随のルールがない場合は、目に見える効果はありません。
トリガーをパッケージ内に作成することはできません。トリガーは、**名前空間**の下に直接作成する必要があります。

## ルールを使用したトリガーとアクションの関連付け
{: #openwhisk_rules_assoc}

ルールは、トリガーとアクションを関連付けるために使用されます。 トリガー・イベントが起動するたびに、イベント・パラメーターを使用してアクションが呼び出されます。

例えば、ロケーション更新がポストされるたびに、`hello` アクションを呼び出すルールを作成します。
1. 以下のアクション・コードを含んでいる「hello.js」という名前のファイルを作成します。
  ```javascript
  function main(params) {
     return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. トリガーとアクションが存在することを確認します。
  ```
  ibmcloud fn trigger update locationUpdate
  ```
  {: pre}

  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

3. 次のステップでは、ルールを作成します。 ルールは作成時に有効になります。つまり、ルールは即時にトリガーのアクティベーションに応答できるようになります。 3 つのパラメーターは、_ルール名_、_トリガー名_、および_アクション名_です。
  ```
  ibmcloud fn rule create myRule locationUpdate hello
  ```
  {: pre}

  ルールを無効にすることをいつでも選択できます。
  ```
  ibmcloud fn rule disable myRule
  ```
  {: pre}

4. **locationUpdate** トリガーを起動します。 イベントを起動するたびに、イベント・パラメーターを使用して **hello** アクションが呼び出されます。
  ```
  ibmcloud fn trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  出力例:
  ```
  ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
  ```
  {: screen}

5. 最新のアクティベーションを調べて、**hello** アクションが呼び出されたことを確認します。
  ```
  ibmcloud fn activation list --limit 1 hello
  ```
  {: pre}

  出力例:
  ```
  activations
  9c98a083b924426d8b26b5f41c5ebc0d             hello
  ```
  {: screen}

  次に、前のコマンド出力にリストされたアクティベーション ID を照会します。
  ```
  ibmcloud fn activation result 9c98a083b924426d8b26b5f41c5ebc0d
  ```
  {: pre}

  出力例:
  ```
  {
     "payload": "Hello, Donald from Washington, D.C."
  }
  ```
  {: screen}

  **Hello** アクションがイベント・ペイロードを受信して、予期されるストリングを返したことが分かります。

同じトリガーを異なるアクションに関連付ける複数のルールを作成できます。
トリガーおよびルールはパッケージに所属できません。 ただし、パッケージに属しているアクションに、ルールを関連付けることはできます。
以下に例を示します。
  ```
  ibmcloud fn rule create recordLocation locationUpdate /whisk.system/utils/echo
  ```
  {: pre}

また、シーケンスとともにルールを使用することも可能です。 例えば、ルール `anotherRule`
でアクティブ化されるアクション・シーケンス `recordLocationAndHello` を作成することができます。
  ```
  ibmcloud fn action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
  ```
  {: pre}

  ```
  ibmcloud fn rule create anotherRule locationUpdate recordLocationAndHello
  ```
  {: pre}
