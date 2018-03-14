---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# トリガーおよびルールの作成
{: #openwhisk_triggers}

{{site.data.keyword.openwhisk_short}} のトリガーおよびルールにより、プラットフォームにイベント・ドリブンの機能が実現されます。外部および内部のイベント・ソースからのイベントは、トリガーによって送信されます。また、ルールによって、アクションがこれらのイベントに対応できるようになります。
{: shortdesc}

## トリガーの作成
{: #openwhisk_triggers_create}

トリガーは、ある種のイベントに対して指定されたチャネルです。トリガーの例を以下に示します。
- ロケーション更新イベントのトリガー。
- Web サイトへの文書アップロードのトリガー。
- 着信 E メールのトリガー。

トリガーは、キーと値のペアのディクショナリーを使用して*起動する* (アクティブ化する) ことができます。このディクショナリーは、*イベント* と呼ばれることもあります。アクションと同様に、トリガーの起動のたびに、アクティベーション ID が生成されます。

トリガーは、ユーザーが明示的に起動することも、ユーザーの代わりに外部イベント・ソースによって起動することもできます。
*フィード* は、{{site.data.keyword.openwhisk_short}} で消費可能なトリガー・イベントを起動するように、外部イベント・ソースを構成するための便利な方法です。以下のフィードの例を参照してください。
- データベース内の文書が追加または変更されるたびにトリガー・イベントを起動する、Cloudant データ変更フィード。
- Git リポジトリーに対するコミットのたびにトリガー・イベントを起動する Git フィード。

## ルールの使用
{: #openwhisk_rules_use}

ルールは 1 つのトリガーを 1 つのアクションに関連付けます。トリガーが起動するたびに、該当のアクションが、トリガー・イベントを入力として呼び出されます。

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
  wsk trigger create locationUpdate
  ```
  {: pre}

  ```
  ok: created trigger locationUpdate
  ```

2. トリガーのセットをリストして、トリガーを作成したことを確認します。
  ```
  wsk trigger list
  ```
  {: pre}

  ```
  triggers
  /someNamespace/locationUpdate                            private
  ```

  これで、イベントを起動できる、指定された「チャネル」が作成されました。

3. 次に、トリガー名とパラメーターを指定することにより、トリガー・イベントを起動します。
  ```
  wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  ```
  ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
  ```

マッチングするルールを伴わずに起動されたトリガーでは、実質的な効果は何もありません。
トリガーをパッケージ内に作成することはできません。トリガーは、名前空間の下に直接作成する必要があります。

## ルールを使用したトリガーとアクションの関連付け
{: #openwhisk_rules_assoc}

ルールは、トリガーとアクションを関連付けるために使用されます。トリガー・イベントが起動するたびに、イベント・パラメーターを使用してアクションが呼び出されます。

例えば、ロケーション更新がポストされるたびに、`hello` アクションを呼び出すルールを作成します。
1. 次のようなアクション・コードを含む 'hello.js' ファイルを作成します。
  ```javascript
  function main(params) {
     return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. トリガーとアクションが存在することを確認します。
  ```
  wsk trigger update locationUpdate
  ```
  {: pre}

  ```
  wsk action update hello hello.js
  ```
  {: pre}

3. 次のステップでは、ルールを作成します。ルールは作成時に有効になります。つまり、ルールは即時にトリガーのアクティベーションに応答できるようになります。3 つのパラメーターは、ルールの名前、トリガー、およびアクションです。

  ```
  wsk rule create myRule locationUpdate hello
  ```
  {: pre}

  いつでも、ルールを無効にすることを選択できます。
  ```
  wsk rule disable myRule
  ```
  {: pre}

4. `locationUpdate` トリガーを起動します。イベントを起動するたびに、イベント・パラメーターを使用して `hello` アクションが呼び出されます。
  ```
  wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  ```
  ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
  ```

5. 最新のアクティベーションを調べて、アクションが呼び出されたことを確認します。
  ```
  wsk activation list --limit 1 hello
  ```
  {: pre}

  ```
  activations
  9c98a083b924426d8b26b5f41c5ebc0d             hello
  ```
  ```
  wsk activation result 9c98a083b924426d8b26b5f41c5ebc0d
  ```
  {: pre}

  ```json
  {
     "payload": "Hello, Donald from Washington, D.C."
  }
  ```

  `Hello` アクションがイベント・ペイロードを受信して、予期されるストリングを返したことが分かります。

同じトリガーを異なるアクションに関連付ける複数のルールを作成できます。
トリガーおよびルールはパッケージに所属できません。ただし、パッケージに属しているアクションに、ルールを関連付けることはできます。
以下に例を示します。
  ```
  wsk rule create recordLocation locationUpdate /whisk.system/utils/echo
  ```
  {: pre}

また、シーケンスとともにルールを使用することも可能です。例えば、ルール `anotherRule`
でアクティブ化されるアクション・シーケンス `recordLocationAndHello` を作成することができます。
  ```
  wsk action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
  ```
  {: pre}

  ```
  wsk rule create anotherRule locationUpdate recordLocationAndHello
  ```
  {: pre}
