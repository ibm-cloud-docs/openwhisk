---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, serverless, javascript, node, node.js, functions

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



# ルールによるトリガーとアクションの関連付け
{: #rules}

トリガーが起動するたびに、ルールはトリガー・イベントを入力として使用し、関連付けられているアクションを呼び出します。 適切なルール・セットを使用することで、単一のトリガーで複数のアクションを呼び出したり、複数のトリガーからのイベントへの応答として 1 つのアクションを呼び出したりできます。
{: shortdesc}


## UI からのルールの作成
{: #rules_ui}

ユーザー・インターフェースを使用すると、アクションとトリガーを関連付けるルールが自動的に作成されます。
{: shortdesc}

アクションまたはトリガーを作成したり細部にアクセスしたりするときには、既存のものを関連付けるか新規のものを関連付けるかを選択できます。 関連付けを行うと、ルールが自動的に作成され、`ACTION_NAME-TRIGGER_NAME` という形式の名前が付けられます。

CLI から、`ibmcloud fn rule list` を実行して、ルールが作成されたことを確認できます。


## CLI からのルールの作成
{: #rules_create}

ルールは、トリガーとアクションを関連付けるために使用されます。 トリガー・イベントが起動されるたびに、トリガー・イベントのパラメーターを使用してアクションが呼び出されます。

始めに、[アクション](/docs/openwhisk?topic=cloud-functions-actions)と[トリガー](/docs/openwhisk?topic=cloud-functions-triggers)を 1 つずつ作成します。


トリガーをアクションと関連付けるルールを作成します。 ルールは、名前空間内に直接作成する必要があり、パッケージ内には作成できません。
```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}


ルールを使用不可にするには、以下のコマンドを実行します。
```
ibmcloud fn rule disable RULE_NAME
```
{: pre}


## アクション・シーケンスのルールの作成
{: #rules_seq}

ルールを使用して、トリガーをアクション・シーケンスに関連付けることもできます。

始めに、[アクション・シーケンス](/docs/openwhisk?topic=cloud-functions-actions#actions_seq)と[トリガー](/docs/openwhisk?topic=cloud-functions-triggers)を 1 つずつ作成します。

```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_SEQUENCE_NAME
```
{: pre}


## 複数のトリガーとアクションの関連付け
{: #rules_assoc}

トリガーとアクションはさまざまに組み合わせて使用できます。そのためには組み合わせごとにルールを 1 つ作成します。 アクションとトリガーは 1 対 1 の関係にする必要はありません。

例えば、次のアクションについて考えてください。

| アクション | 説明 |
| --- | --- |
| `classifyImage` | イメージ内のオブジェクトを検出し、それらを分類するアクション。 |
| `thumbnailImage` | イメージのサムネール・バージョンを作成するアクション。 |

また、2 つのイベント・ソースが以下のトリガーを起動するとします。

| トリガー | 説明 |
| --- | --- |
| `newTweet` | 新規ツイートが投稿されたときに起動するトリガー。 |
| `imageUpload` | Web サイトにイメージがアップロードされたときに起動するトリガー。 |

単一のトリガー・イベントが複数のアクションを起動し、複数のトリガーが同じアクションを呼び出すように、ルールをセットアップすることができます。
- `newTweet -> classifyImage` ルール
- `imageUpload -> classifyImage` ルール
- `imageUpload -> thumbnailImage` ルール

この 3 つのルールによって、以下の動作が設定されます。
- 各ツイートのイメージが分類されます。
- アップロードされたイメージが分類されます。
- サムネールのバージョンが生成されます。

