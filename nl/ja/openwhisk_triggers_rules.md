---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-28"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# トリガーおよびルールによるイベントへの応答
{: #openwhisk_triggers}

{{site.data.keyword.openwhisk}} のトリガーおよびルールにより、プラットフォームにイベント・ドリブンの機能が実現されます。 外部および内部のイベント・ソースからのイベントがトリガーを通して送信され、アクションがそうしたイベントに反応することがルールによって可能になります。
{: shortdesc}

## 一般的な概念
{: #definitions}

### トリガー
{: #openwhisk_triggers_create}

トリガーは、ある種のイベントに対して指定されたチャネルです。
{: shortdesc}

トリガーとは、ユーザーから起動されるか、イベント・ソースによって起動されるかに関わらず、特定のタイプのイベントに対応するための宣言です。 トリガーの例を以下に示します。
- ロケーション更新イベントのトリガー
- Web サイトへの文書アップロードのトリガー
- 着信 E メールのトリガー

トリガーは、キーと値のペアのディクショナリーを使用して起動またはアクティブ化することができます。 このディクショナリーは、イベントと呼ばれることもあります。 トリガーは、ユーザーが明示的に起動することも、ユーザーの代わりに外部イベント・ソースによって起動することもできます。 アクションと同様に、トリガーの起動のたびに、アクティベーション ID が生成されます。 ルールに関連付けられていないトリガーは、起動されても視覚的な効果はありません。

フィードは、{{site.data.keyword.openwhisk_short}} で消費可能なトリガー・イベントを起動するように、外部イベント・ソースを構成するための便利な方法です。 フィードの例を以下に示します。
- データベース内の文書が追加または変更されるたびにトリガー・イベントを起動する、{{site.data.keyword.cloudant}} データ変更フィード
- Git リポジトリーに対するコミットのたびにトリガー・イベントを起動する Git フィード

### ルール
{: #openwhisk_rules_use}

ルールは、トリガーをアクションと関連付けます。
{: shortdesc}

トリガーが起動するたびに、ルールはトリガー・イベントを入力として使用して、関連付けられたアクションを呼び出します。適切なルール・セットを使用して、単一のトリガー・イベントが
複数のアクションを呼び出すことも、複数のトリガーからのイベントに対する応答として 1 つのアクションを呼び出すこともできます。

例えば、以下のアクションを持つシステムがあるとします。
- `classifyImage` - イメージ内のオブジェクトを検出し、それらを分類するアクション。
- `thumbnailImage` - イメージのサムネール・バージョンを作成するアクション。

また、2 つのイベント・ソースが以下のトリガーを起動するとします。
- `newTweet` - 新規ツイートが投稿されたときに起動するトリガー。
- `imageUpload` - Web サイトにイメージがアップロードされたときに起動するトリガー。

単一のトリガー・イベントが複数のアクションを起動し、複数のトリガーが同じアクションを呼び出すように、ルールをセットアップすることができます。
- `newTweet -> classifyImage` ルール
- `imageUpload -> classifyImage` ルール
- `imageUpload -> thumbnailImage` ルール

この 3 つのルールによって、以下の動作が設定されます。
- 各ツイートのイメージが分類されます。
- アップロードされたイメージが分類されます。
- サムネールのバージョンが生成されます。

## チャネル・イベントに対するトリガーの作成
{: #openwhisk_triggers_fire}

以下のステップは、ユーザー・ロケーションの更新を送信するトリガーの例を作成する方法と、手動でトリガーを起動する方法を示しています。

1. トリガーを作成します。 トリガーは、名前空間内に直接作成する必要があり、パッケージ内には作成できません。
    ```
    ibmcloud fn trigger create locationUpdate
    ```
    {: pre}

    出力例:
    ```
    ok: created trigger locationUpdate
    ```
    {: screen}

2. トリガーが作成されたことを確認します。
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
    トリガーは、イベントを起動できる、指定されたチャネルとして機能します。

3. トリガー・イベントを起動します。
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    このトリガーにはルールが関連付けられていないため、渡されたパラメーターは、どのアクションによっても入力として使用されません。 出力例:
    ```
    ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

次のセクションでは、ルールを作成することによって、トリガーをアクションに関連付けることができます。

## ルールを使用したトリガーとアクションの関連付け
{: #openwhisk_rules_assoc}

ルールは、トリガーとアクションを関連付けるために使用されます。 トリガー・イベントが起動されるたびに、トリガー・イベントのパラメーターを使用してアクションが呼び出されます。

以下のステップは、[`locationUpdate` トリガー](#openwhisk_triggers_fire)の作成後に、ロケーション更新がポストされるたびに `hello` アクションを呼び出すルール例を作成する方法を示しています。

1. 以下のアクション・コードを含んでいる「hello.js」という名前のファイルを作成します。
    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: pre}

2. `hello` アクションを作成します。
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. `locationUpdate` トリガーを `hello` アクションに関連付ける `myRule` ルールを作成します。ルールは、名前空間内に直接作成する必要があり、パッケージ内には作成できません。
    ```
    ibmcloud fn rule create myRule locationUpdate hello
    ```
    {: pre}

4. `locationUpdate` トリガーを起動します。 トリガー・イベントが発生するたびに、イベント・パラメーターを使用して `hello` アクションが呼び出されます。
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    出力例:
    ```
    ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
    ```
    {: screen}

5. 最新のアクティベーション・レコードを調べて、`hello` アクションが呼び出されたことを確認します。
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

6. 前のコマンド出力からアクティベーション ID に関する詳細情報を取得します。
    ```
    ibmcloud fn activation result 9c98a083b924426d8b26b5f41c5ebc0d
    ```
    {: pre}

    出力例:
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}
    `Hello` アクションがイベント・ペイロードを受信して、予期されるストリングを返したことが分かります。

7. ルールを使用不可にするには、以下のコマンドを実行します。
    ```
    ibmcloud fn rule disable myRule
    ```
    {: pre}

また、ルールを使用して、トリガーをシーケンスに関連付けることもできます。例えば、ルール `anotherRule` によってアクティブ化される、`recordLocationAndHello` というアクション・シーケンスを作成することができます。
```
ibmcloud fn action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
```
{: pre}

```
ibmcloud fn rule create anotherRule locationUpdate recordLocationAndHello
```
{: pre}
