---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: functions cli, serverless, bluemix cli, install, functions plug-in

subcollection: cloud-functions

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.openwhisk_short}} CLI プラグインのセットアップ
{: #cloudfunctions_cli}


{{site.data.keyword.openwhisk}} は、{{site.data.keyword.openwhisk_short}} システムの完全な管理を可能にする {{site.data.keyword.Bluemix_notm}} CLI 用の強力なプラグインを提供します。 {{site.data.keyword.openwhisk_short}} CLI プラグインを使用すると、アクションでのコード・スニペットの管理、アクションがイベントに応答できるようにするトリガーおよびルールの作成、パッケージへのアクションのバンドルを行うことができます。
{:shortdesc}

{{site.data.keyword.openwhisk_short}} プラグイン・コマンドで、次の例に示すように、別名 `fn` が使用できるようになりました: `ibmcloud fn <command>`
{: tip}

## {{site.data.keyword.Bluemix_notm}} CLI のセットアップ
{: #bluemix_cli_setup}

{{site.data.keyword.Bluemix_notm}} CLI をダウンロードしてインストールし、ログインします。
{: shortdesc}

1. [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli) をダウンロードしてインストールします。

2. {{site.data.keyword.Bluemix_notm}} CLI にログインします。 IBM Cloud 地域を指定するには、[API エンドポイントを含めます](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions)。

  ```
  ibmcloud login
  ```
  {: pre}

  ログイン時に `ibmcloud login -o <ORG> -s <SPACE>` フラグを使用して組織とスペースを指定すれば、それらの入力を求めるプロンプトをスキップできます。
  {: tip}

3. 組織とスペースを指定しなかった場合は、login コマンドの後に表示されるプロンプトに値を入力します。


## {{site.data.keyword.openwhisk_short}} プラグインのセットアップ
{: #cloudfunctions_plugin_setup}

{{site.data.keyword.openwhisk_short}} で作業する場合は、CLI プラグインをダウンロードしてインストールします。
{: shortdesc}

プラグインを使用して、以下を行うことができます。

* {{site.data.keyword.openwhisk_short}} でコード・スニペット (アクション) を実行する。 『[アクションの作成と呼び出し](/docs/openwhisk?topic=cloud-functions-openwhisk_actions)』を参照してください。
* トリガーおよびルールを使用して、アクションでイベントに応答できるようにする。 『[トリガーとルールの作成](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers)』を参照してください。
* パッケージでアクションをバンドルする方法および外部イベント・ソースを構成する方法を学習する。 『[パッケージの作成と使用](/docs/openwhisk?topic=cloud-functions-openwhisk_packages)』を参照してください。
* パッケージ・カタログを検討し、[{{site.data.keyword.cloudant}} イベント・ソース](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant)などの外部サービスでアプリケーションを強化する。

{{site.data.keyword.openwhisk_short}} プラグインで実行できることをすべて表示するには、引数なしで `ibmcloud fn` を実行します。
{: tip}

1. {{site.data.keyword.openwhisk_short}} プラグインをインストールします。

  ```
  ibmcloud plugin install cloud-functions
  ```
  {: pre}

2. プラグインがインストールされたことを確認します。

  ```
  ibmcloud plugin list cloud-functions
  ```
  {: pre}

  出力:
  ```
  Plugin Name          Version
    Cloud-Functions      1.0.16
  ```
  {: screen}

既存のプラグインを更新する必要がある場合は、`ibmcloud plugin update cloud-functions` を実行します。
{:tip}



## アクションからのサービスの使用
{: #binding_services_cli}

{{site.data.keyword.openwhisk_short}} には、`service bind` コマンドが用意されています。このコマンドは、{{site.data.keyword.Bluemix_notm}} サービス資格情報を、実行時にコードで使用できるようにします。 その後、`service bind` コマンドを使用して、{{site.data.keyword.openwhisk_short}} に定義されている任意のアクションに対して任意の {{site.data.keyword.Bluemix_notm}} サービスをバインドできます。

アクションからサービスを使用するための詳しいステップについては、[アクションへのサービスのバインド](/docs/openwhisk?topic=cloud-functions-binding_services)を参照してください。


## HTTPS プロキシーを使用するための {{site.data.keyword.openwhisk_short}} CLI の構成
{: #cli_https_proxy}

HTTPS プロキシーを使用するように {{site.data.keyword.openwhisk_short}} CLI をセットアップできます。 HTTPS プロキシーをセットアップするには、`HTTPS_PROXY` という名前の環境変数を作成する必要があります。 フォーマット `{PROXY IP}:{PROXY PORT}` を使用して、
この変数を HTTPS プロキシーのアドレスとそのポートに設定する必要があります。



## 異なる地域、組織、およびスペースへの切り替え
{: #region_info}

既にログインしている場合は、{{site.data.keyword.Bluemix_notm}} CLI で `ibmcloud target` コマンドを実行して、地域、組織、およびスペースを切り替えることができます。


エンティティーを作成して管理するには、名前空間をターゲットにする必要があります。デフォルトの名前空間 (状況によっては下線 (`_`) で判別できる) は、現時点でターゲットになっている Cloud Foundry ベースの名前空間に対応しています。

実動前 (ステージング) デプロイメントおよび実動デプロイメントを処理するためのスペースを作成できます。それぞれに対してスペースを作成します。 スペースを作成することで、{{site.data.keyword.openwhisk_short}} で 2 つの異なる名前空間が定義されるようにすることができます。 組織の下に「staging」や「production」などのスペースをさらに作成するには、[`ibmcloud iam space-create`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_account#ibmcloud_account_space_create) を実行します。

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}} には、名前空間の名前に対する制限があります。 詳しくは、[システムの詳細および制限](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_entities)の資料を参照してください。
{: tip}

**警告**: 組織またはスペースの名前を変更すると、その変更後の名前に基づいて新しい名前空間が作成されます。古い名前空間に入っていたエンティティーは新しい名前空間に表示されず、削除されることがあります。


## OpenWhisk CLI から {{site.data.keyword.openwhisk_short}} CLI プラグインへのマイグレーション
{: #cli_migration}

{{site.data.keyword.openwhisk_short}} CLI プラグインを使用して {{site.data.keyword.openwhisk_short}} エンティティーと対話できるようになりました。スタンドアロンの OpenWhisk CLI を引き続き使用することも可能ですが、そこには {{site.data.keyword.openwhisk_short}} でサポートされる最新のフィーチャー (IAM ベースの名前空間や `service bind` など) が含まれていません。
{: shortdesc}

### コマンド構文
{: #command_syntax}

すべての `wsk` コマンド (不要になった `wsk bluemix login` コマンドを除く) は、コマンド `ibmcloud fn` を使用することで、同様に動作します。 Cloud Functions CLI プラグインで使用できるコマンドのすべてのコマンド・オプションと引数は、スタンドアロンの OpenWhisk CLI で使用できるコマンドの場合と同じです。詳しくは、[Apache OpenWhisk CLI プロジェクト ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/apache/incubator-openwhisk-cli) を参照してください。

### API 認証と API ホスト
{: #api_authentication}

OpenWhisk CLI では、認証 API キーおよび API ホストを構成する必要がありました。 {{site.data.keyword.openwhisk_short}} CLI プラグインでは、API キーおよび API ホストを明示的に構成する必要はありません。 代わりに、`ibmcloud login` でログインし、`ibmcloud target` コマンドを使用して地域と名前空間のターゲットを指定できます。 ログイン後は、すべてのコマンドを `ibmcloud fn` で開始します。


cURL や Postman などの外部 HTTP クライアントで {{site.data.keyword.openwhisk_short}} の認証 API キーを使用する必要がある場合は、以下のコマンドで取得できます。

現在の API キーを取得するには、以下を実行します。
```
ibmcloud fn property get --auth
```
{: pre}

現在の API ホストを取得するには、以下を実行します。
```
ibmcloud fn property get --apihost
```
{: pre}

API キーは、{{site.data.keyword.openwhisk_short}} CLI プラグインのターゲットとなる地域、組織、およびスペースごとに固有です。
{: tip}

### API ゲートウェイ認証
{: #apigw_authentication}

OpenWhisk CLI では、`wsk api` コマンドを使用して API の管理のために API ゲートウェイの許可を構成するには、`wsk bluemix login` を実行する必要がありました。 {{site.data.keyword.openwhisk_short}} CLI プラグインでは、`wsk bluemix login` を実行する必要はありません。 代わりに、`ibmcloud login` コマンドを使用して {{site.data.keyword.Bluemix_notm}} にログインすると、{{site.data.keyword.openwhisk}} プラグインによって、現在のログインとターゲットの情報が自動的に利用されます。 これで、`ibmcloud fn api` コマンドを使用して、API を管理できます。

### デプロイメント・スクリプトのマイグレーション
{: #migrating_deploy_scripts}

`wsk` バイナリーで OpenWhisk CLI を使用するスクリプトがある場合、コマンド `ibmcloud fn` を使用することで、すべてのコマンドは同様に動作します。 {{site.data.keyword.Bluemix_notm}} CLI プラグインを使用するようにスクリプトを変更するか、別名またはラッパーを作成して、`wsk` を使用している現行コマンドが `ibmcloud fn` に変換されるようにすることができます。 {{site.data.keyword.Bluemix_notm}} CLI の `ibmcloud login` および `ibmcloud target` コマンドは、無人モードで動作します。 無人モードでは、`ibmcloud fn` コマンドを実行して {{site.data.keyword.openwhisk_short}} エンティティーをデプロイおよび管理する前に、環境を構成できます。





## CLI のバージョン履歴
{: #version_history}

以下は、バージョンの履歴レコードであり、ハイライトおよびバグ修正を示します。

v1.0.30 (2019-04-03)
* `service bind` で IAM サービス、組織ベースのサービス、スペース・ベースのサービスを処理する方法が改善されました。
* API エンドポイント https://cloud.ibm.com の処理に関するフィックスが追加されました。

v1.0.29 (2019-02-06)
* マニフェスト・ファイルによって Functions エンティティーのコレクションのデプロイとアンデプロイを実行するためのコマンド `deploy` と `undeploy` が追加されました。詳しくは、[デプロイメント](/docs/openwhisk?topic=cloud-functions-deploy#deploy)の資料を参照してください。

v1.0.28 (2019-01-21)
* `update|delete|get namespace name` が複数回存在する場合のエラー・メッセージが追加されました。

v1.0.27 (2018-12-11)
* `namespace get` のフィックスが追加されました。
* ブラック・ボックス・アクションに関する `--save-as` のフィックスが追加されました。
* action create コマンドと action update コマンドの `--concurrency` フラグが追加されました。

v1.0.26 (2018-11-30)
* 新しい環境で認証キーを正しく返すための `fn property get --auth` が有効になりました。

v1.0.25 (2018-11-23)
* エラー・メッセージの結果の表示が改善されました。
* 名前空間のプロパティーを正しく表示するための `fn namespace get` のフィックスが追加されました。

1.0.23 (2018-10-15)
* Ruby (.rb) アクション・コードを認識するためのサポートが追加されました。

1.0.22 (2018-08-20)
* 米国東部地域のサポートが追加されました。

1.0.21 (2018-08-01)
* {{site.data.keyword.openwhisk_short}} コマンドで、次の例に示すように、別名 `fn` および `functions` が使用できるようになりました: `ibmcloud fn <command>` および `ibmcloud fn <command>`。 `ibmcloud wsk <command>` もまだ使用できます。

1.0.19 (2018-07-02)
* 小さいバグ修正および改善。

1.0.18 (2018-06-20)
* ユーザー提供のサービス・インスタンスをアンバインドするためのフィックスが追加されました。
* パフォーマンスの改善。

1.0.17 (2018-06-12)
* `ibmcloud cf create-user-provided-service` コマンドを使用して作成されたユーザー提供のサービス・インスタンスのバインド (`ibmcloud wsk service bind`) とアンバインド (`ibmcloud wsk service unbind`) を実行するためのサポートが追加されました。

1.0.16 (2018-05-24)
* 小さいバグ修正および改善。

1.0.15 (2018-05-21)
* 小さいバグ修正および改善。

1.0.14 (2018-05-17)
* 組織名とスペース名に含まれる `&` 文字のサポートが有効になりました。

1.0.13 (2018-05-07)
* 小さいバグ修正およびエラー処理の改善。

1.0.12 (2018-04-30)
* `bx` CLI との互換性を維持するための {{site.data.keyword.Bluemix_notm}} SDK の更新。

1.0.11 (2018-04-23)
* 小さいバグ修正および改善。

1.0.10 (2018-04-09)
* Web アクション・エンドポイントを保護するための新しい `--web-secure` オプションが `ibmcloud wsk action create|update` コマンドに追加されました。
* バックツーバック・パス・パラメーターの[問題](https://github.com/apache/incubator-openwhisk-cli/issues/237)が修正されました。

1.0.9 (2018-03-16)
* パッケージ・レベルでのサービス・バインドのサポートが有効になりました。

1.0.8 (2018-02-22)
* IAM サービス・バインドのサポートが有効になりました。

1.0.7 (2018-02-02)
* `ibmcloud wsk api` が更新され、`/api/{id}` などのパス・パラメーターを指定できるようになりました。詳しくは、『[API ゲートウェイ](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway)』を参照。
* プロキシー・サポートが復元されました。
* `swift:3` が削除されました。

1.0.6 (2018-01-30)
* パッケージ内のアクションを対象にした `ibmcloud wsk service bind` コマンドのバグが修正されました。
