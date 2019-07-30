---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: github, actions, trigger, event, functions

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


# GitHub
{: #pkg_github}

この事前インストール済みパッケージは、東京地域では利用できません。
{: tip}

`/whisk.system/github` パッケージは、[GitHub API](https://developer.github.com/){: external} を使用するための便利な方法を提供します。
{: shortdesc}

GitHub パッケージには、以下のエンティティーが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/github` | パッケージ | `username`、`repository`、`accessToken` | GitHub API と対話 |
| `/whisk.system/github/webhook` | フィード | `events`、`username`、`repository`、`accessToken` | GitHub アクティビティーに対するトリガー・イベントの起動 |

`username`、`repository`、および `accessToken` の各値を使用してパッケージ・バインディングを作成することをお勧めします。  バインディングがあると、パッケージ内のフィードを使用するたびにこれらの値を指定する必要がありません。

## GitHub アクティビティーでのトリガー・イベントの起動

`/whisk.system/github/webhook` フィードは、指定された GitHub リポジトリーにアクティビティーがあるとトリガーを起動するようにサービスを構成します。 パラメーターは次のとおりです。

| パラメーター | 説明 |
| --- | --- |
| `username` | GitHub リポジトリーのユーザー名。 |
| `repository` | GitHub リポジトリー。 |
| `accessToken` | GitHub パーソナル・アクセス・トークン。 トークンの作成時に、必ず `repo:status` スコープと `public_repo` スコープを選択してください。 また、リポジトリー用に既に定義された Webhook がないことを確認してください。 |
| `events` | 対象の [GitHub イベント・タイプ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.github.com/v3/activity/events/types/) |

以下の例では、GitHub リポジトリーへの新しいコミットのたびに起動されるトリガーが作成されます。

1. GitHub パーソナル・アクセス・トークンを生成します。 **GitHub.com** > **「設定」** > **「個人用アクセス・トークン (Personal access tokens)」**にナビゲートして、トークンを生成します。アクセス・トークンは次のステップで使用されます。

2. GitHub リポジトリー用に構成されたパッケージ・バインディングをアクセス・トークンを使用して作成します。
  ```
  ibmcloud fn package bind /whisk.system/github myGit \
    --param username myGitUser \
    --param repository myGitRepo \
    --param accessToken aaaaa1111a1a1a1a1a111111aaaaaa1111aa1a1a
  ```
  {: pre}

3. `myGit/webhook` フィードを使用して、GitHub `push` イベント・タイプ用のトリガーを作成します。
  ```
  ibmcloud fn trigger create myGitTrigger --feed myGit/webhook --param events push
  ```
  {: pre}

  `git push` を使用した GitHub リポジトリーへのコミットがあると、このトリガーが Webhook によって起動されます。 このトリガーに一致するルールがある場合、関連付けられたアクションが呼び出されます。 そのアクションは、入力パラメーターとして GitHub Webhook ペイロードを受け取ります。 各 GitHub Webhook イベントは、類似した JSON スキーマを持っていますが、それぞれのイベント・タイプによって判別される固有のペイロード・オブジェクトです。 ペイロード・コンテンツについては、[GitHub events and payload](https://developer.github.com/v3/activity/events/types/){: external} API の資料を参照してください。



