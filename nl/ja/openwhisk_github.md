---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# GitHub イベント・ソース
{: #openwhisk_catalog_github}

`/whisk.system/github` パッケージは、[GitHub API](https://developer.github.com/) を使用するための便利な方法を提供します。
{: shortdesc}

このパッケージには、以下のフィードが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/github` | パッケージ | username、repository、accessToken | GitHub API と対話 |
| `/whisk.system/github/webhook` | フィード | events、username、repository、accessToken | GitHub アクティビティーに対するトリガー・イベントの起動 |

`username`、`repository`、および `accessToken` の各値を使用してパッケージ・バインディングを作成することをお勧めします。  バインディングがあると、パッケージ内のフィードを使用するたびにこれらの値を指定する必要がありません。

## GitHub アクティビティーでのトリガー・イベントの起動

`/whisk.system/github/webhook` フィードは、指定された GitHub リポジトリーにアクティビティーがあるとトリガーを起動するようにサービスを構成します。 パラメーターは次のとおりです。

- `username`: GitHub リポジトリーのユーザー名。
- `repository`: GitHub リポジトリー。
- `accessToken`: GitHub パーソナル・アクセス・トークン。 [トークンの作成](https://github.com/settings/tokens)時に、必ず **repo:status** スコープと **public_repo** スコープを選択してください。 また、リポジトリー用に既に定義された Webhook がないことを確認してください。
- `events`: 対象の [GitHub イベント・タイプ](https://developer.github.com/v3/activity/events/types/)。

以下の例では、GitHub リポジトリーへの新しいコミットのたびに起動されるトリガーが作成されます。

1. GitHub [パーソナル・アクセス・トークン](https://github.com/settings/tokens)を生成します。 アクセス・トークンは次のステップで使用されます。

2. GitHub リポジトリー用に構成されたパッケージ・バインディングをアクセス・トークンを使用して作成します。
  ```
  ibmcloud wsk package bind /whisk.system/github myGit \
    --param username myGitUser \
    --param repository myGitRepo \
    --param accessToken aaaaa1111a1a1a1a1a111111aaaaaa1111aa1a1a
  ```
  {: pre}

3. `myGit/webhook` フィードを使用して、GitHub `push` イベント・タイプ用のトリガーを作成します。
  ```
  ibmcloud wsk trigger create myGitTrigger --feed myGit/webhook --param events push
  ```
  {: pre}

  `git push` を使用した GitHub リポジトリーへのコミットがあると、このトリガーが Webhook によって起動されます。 このトリガーに一致するルールがある場合、関連付けられたアクションが呼び出されます。 そのアクションは、入力パラメーターとして GitHub Webhook ペイロードを受け取ります。 各 GitHub Webhook イベントは、類似した JSON スキーマを持っていますが、それぞれのイベント・タイプによって判別される固有のペイロード・オブジェクトです。 ペイロード・コンテンツについては、[GitHub events and payload](https://developer.github.com/v3/activity/events/types/) API の資料を参照してください。
