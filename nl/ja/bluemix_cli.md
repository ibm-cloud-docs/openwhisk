---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} CLI プラグインのセットアップ
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk}} は、{{site.data.keyword.openwhisk_short}} システムの完全な管理を可能にする {{site.data.keyword.Bluemix_notm}} CLI 用の強力なプラグインを提供します。
{: shortdesc}

## {{site.data.keyword.Bluemix_notm}} CLI のセットアップ
{: #bluemix_cli_setup}

{{site.data.keyword.Bluemix_notm}} CLI をダウンロードしてインストールし、ログインします。
{: shortdesc}

1. [{{site.data.keyword.Bluemix_notm}} CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html) をダウンロードしてインストールします。

2. {{site.data.keyword.Bluemix_notm}} CLI にログインします。 {{site.data.keyword.openwhisk_short}} は、{{site.data.keyword.Bluemix_notm}} の米国南部地域および英国地域で使用可能です。{{site.data.keyword.Bluemix_notm}} API エンドポイントが指定されていない場合、`-a` フラグを使用して指定してください。

    * 米国南部地域にログインするには、以下のようにします。
      ```
      ibmcloud login -a api.ng.bluemix.net
      ```
      {: pre}

    * 英国地域にログインするには、以下のようにします。
      ```
      ibmcloud login -a api.eu-gb.bluemix.net
      ```
      {: pre}

  `ibmcloud api` コマンドを使用して、{{site.data.keyword.Bluemix_notm}} API エンドポイントを明示的に設定できます。 現在の API エンドポイント設定を表示するには、`ibmcloud target` コマンドを使用します。
  {: tip}

3. `ibmcloud login` コマンドでは、組織、スペース、パスワードなどの情報を求めるプロンプトが出されます (指定しなかった場合)。

  ログイン時に組織およびスペースを指定することで、それらを求めるプロンプトをスキップできます。`ibmcloud login -o <ORG> -s <SPACE>` フラグを使用します。
  {: tip}

また、{{site.data.keyword.Bluemix_notm}} API キーを使用してログインすることもできます。 この方法は、フラグ `--sso` を指定してログインする必要があるフェデレーテッド・ログインがアカウントに対して構成されている場合に便利です。[API キーの使用](https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key)は、継続的な統合 (CI) をセットアップする場合や、無人パイプラインを構成する場合にも役立ちます。

1. 新しい API キーを作成します。
    ```
    ibmcloud iam api-key-create MyKey
    ```
    {: pre}

2. 生成された API キーの値を使用してログインします。
    ```
    ibmcloud login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
    ```
    {: pre}
</br>
`ibmcloud login` コマンドについて詳しくは、`ibmcloud login --help` を使用するか、[IBM Cloud (bx) コマンド](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login)のトピックを参照してください。

## {{site.data.keyword.openwhisk_short}} プラグインのセットアップ
{: #cloudfunctions_plugin_setup}

{{site.data.keyword.openwhisk_short}} プラグインをダウンロードしてインストールします。
{: shortdesc}

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

3. `echo` のブロッキング (同期) 呼び出しを実行します。引数として `hello` を渡します。
    ```
    ibmcloud wsk action invoke whisk.system/utils/echo -p message hello --result
    ```
    {: pre}

4. `hello` メッセージが出力に返されていることを確認します。
    出力例:
    ```
    {
        "message":"hello"
    }
    ```
    {: screen}

5. 以下のコマンドを実行して、{{site.data.keyword.openwhisk_short}} プラグインをアップグレードできます。
    ```
    ibmcloud plugin update cloud-functions
    ```
    {: pre}

{{site.data.keyword.openwhisk_short}} CLI を使用して、以下を行うことができます。

* {{site.data.keyword.openwhisk_short}} でコード・スニペット (アクション) を実行する。 『[アクションの作成と呼び出し](./openwhisk_actions.html)』を参照してください。
* トリガーおよびルールを使用して、アクションでイベントに応答できるようにする。 『[トリガーとルールの作成](./openwhisk_triggers_rules.html)』を参照してください。
* パッケージでアクションをバンドルする方法および外部イベント・ソースを構成する方法を学習する。 『[パッケージの作成と使用](./openwhisk_packages.html)』を参照してください。
* パッケージ・カタログを検討し、[{{site.data.keyword.cloudant}} イベント・ソース](./openwhisk_cloudant.html)などの外部サービスでアプリケーションを強化する。

{{site.data.keyword.openwhisk_short}} プラグインのコマンドをリストするには、引数を指定せずに `ibmcloud wsk` を実行します。
{: tip}

## アクションからのサービスの使用
{: #binding_services}

{{site.data.keyword.openwhisk_short}} には、`service bind` コマンドが用意されています。このコマンドは、{{site.data.keyword.Bluemix_notm}} サービス資格情報を、実行時にコードで使用できるようにします。 その後、`service bind` コマンドを使用して、{{site.data.keyword.openwhisk_short}} に定義されている任意のアクションに対して任意の {{site.data.keyword.Bluemix_notm}} サービスをバインドできます。

アクションからサービスを使用するための詳しいステップについては、[アクションへのサービスのバインド](./binding_services.html)を参照してください。

## HTTPS プロキシーを使用するための {{site.data.keyword.openwhisk_short}} CLI の構成
{: #cli_https_proxy}

HTTPS プロキシーを使用するように {{site.data.keyword.openwhisk_short}} CLI をセットアップできます。 HTTPS プロキシーをセットアップするには、`HTTPS_PROXY` という名前の環境変数を作成する必要があります。 フォーマット `{PROXY IP}:{PROXY PORT}` を使用して、
この変数を HTTPS プロキシーのアドレスとそのポートに設定する必要があります。

## 異なる地域、組織、およびスペースへの切り替え
{: #region_info}

既にログインしている場合は、{{site.data.keyword.Bluemix_notm}} CLI で `ibmcloud target` コマンドを実行して、地域、組織、およびスペースを切り替えることができます。

{{site.data.keyword.openwhisk_short}} は、{{site.data.keyword.Bluemix_notm}} の米国南部地域および英国地域で使用可能です。地域を変更するには、`ibmcloud target` コマンドを使用します。例えば、英国地域に切り替え、その地域内のスペース `staging` に切り替えるには、次のようにします。
```
ibmcloud target -r eu-gb -s staging
```
{: pre}

実動前 (ステージング) デプロイメントおよび実動デプロイメントを処理するためのスペースを作成できます。それぞれに対してスペースを作成します。 スペースを作成することで、{{site.data.keyword.openwhisk_short}} で 2 つの異なる名前空間が定義されるようにすることができます。 組織の下に「staging」や「production」などのスペースをさらに作成するには、[`ibmcloud iam space-create`](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create) を実行します。

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}} には、名前空間の名前に対する制限があります。 詳しくは、[システムの詳細および制限](https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities)の資料を参照してください。
{: tip}

## OpenWhisk CLI から {{site.data.keyword.openwhisk_short}} CLI プラグインへのマイグレーション
{: #cli_migration}

{{site.data.keyword.openwhisk_short}} CLI プラグインの導入に伴い、OpenWhisk スタンドアロン CLI は不要になりました。

### コマンド構文
{: #command_syntax}

すべての `wsk` コマンド (不要になった `wsk bluemix login` コマンドを除く) は、コマンド `ibmcloud wsk` を使用することで、同様に動作します。  すべてのコマンドのオプションおよび引数は同じです。

### API 認証と API ホスト
{: #api_authentication}

OpenWhisk CLI では、認証 API キーおよび API ホストを構成する必要がありました。
{{site.data.keyword.openwhisk_short}} CLI プラグインでは、API キーおよび API ホストを明示的に構成する必要はありません。 代わりに、`ibmcloud login` でログインし、`ibmcloud target` コマンドを使用して地域と名前空間のターゲットを指定できます。ログイン後は、すべてのコマンドを `ibmcloud wsk` で開始します。

cURL や Postman などの外部 HTTP クライアントで {{site.data.keyword.openwhisk_short}} の認証 API キーを使用する必要がある場合は、以下のコマンドで取得できます。

現在の API キーを取得するには、以下を実行します。
```
ibmcloud wsk property get --auth
```
{: pre}

現在の API ホストを取得するには、以下を実行します。
```
ibmcloud wsk property get --apihost
```
{: pre}

API キーは、{{site.data.keyword.openwhisk_short}} CLI プラグインのターゲットとなる地域、組織、およびスペースごとに固有です。
{: tip}

### API ゲートウェイ認証
{: #apigw_authentication}

OpenWhisk CLI では、`wsk api` コマンドを使用して API の管理のために API ゲートウェイの許可を構成するには、`wsk bluemix login` を実行する必要がありました。{{site.data.keyword.openwhisk_short}} CLI プラグインでは、`wsk bluemix login` を実行する必要はありません。代わりに、`ibmcloud login` コマンドを使用して {{site.data.keyword.Bluemix_notm}} にログインすると、{{site.data.keyword.openwhisk}} プラグインによって、現在のログインとターゲットの情報が自動的に利用されます。これで、`ibmcloud wsk api` コマンドを使用して、API を管理できます。

### デプロイメント・スクリプトのマイグレーション
{: #migrating_deploy_scripts}

`wsk` バイナリーで OpenWhisk CLI を使用するスクリプトがある場合、コマンド `ibmcloud wsk` を使用することで、すべてのコマンドは同様に動作します。 {{site.data.keyword.Bluemix_notm}} CLI プラグインを使用するようにスクリプトを変更するか、別名またはラッパーを作成して、`wsk` を使用している現行コマンドが `ibmcloud wsk` に変換されるようにすることができます。{{site.data.keyword.Bluemix_notm}} CLI の `ibmcloud login` および `ibmcloud target` コマンドは、無人モードで動作します。 無人モードでは、`ibmcloud wsk` コマンドを実行して {{site.data.keyword.openwhisk_short}} エンティティーをデプロイおよび管理する前に、環境を構成できます。

## バージョン履歴
{: #version_history}

以下は、バージョンの履歴レコードであり、ハイライトおよびバグ修正を示します。

1.0.18 (2018-06-20)
* ユーザー提供のサービス・インスタンスをアンバインドするためのフィックス。
* パフォーマンスの改善。

1.0.17 (2018-06-12)
* `ibmcloud cf create-user-provided-service` コマンドを使用して作成されたユーザー提供のサービス・インスタンスをバインド (`ibmcloud wsk service bind`) およびアンバインド (`ibmcloud wsk service unbind`) するサポートの追加。

1.0.16 (2018-05-24)
* 小さいバグ修正および改善.

1.0.15 (2018-05-21)
* 小さいバグ修正および改善。

1.0.14 (2018-05-17)
* 組織名およびスペース名に含まれる `&` 文字のサポート対応。

1.0.13 (2018-05-07)
* 小さいバグ修正およびエラー処理の改善。

1.0.12 (2018-04-30)
* `bx` CLI との互換性を維持するための {{site.data.keyword.Bluemix_notm}} SDK の更新。

1.0.11 (2018-04-23)
* 小さいバグ修正および改善。

1.0.10 (2018-04-09)
* Web アクション・エンドポイントを保護するために、新しい `--web-secure` オプションを `ibmcloud wsk action create|update` コマンドに追加。
* バックツーバック・パス・パラメーターの[問題](https://github.com/apache/incubator-openwhisk-cli/issues/237)修正。

1.0.9 (2018-03-16)
* パッケージ・レベルでのサービス・バインドのサポート対応。

1.0.8 (2018-02-22)
* IAM サービス・バインドのサポート対応。

1.0.7 (2018-02-02)
* `ibmcloud wsk api` が `/api/{id}` などのパス・パラメーターを受け入れるように変更。 詳しくは、『[API ゲートウェイ](./openwhisk_apigateway.html)』を参照。
* リストア・プロキシーのサポート。
* `swift:3` を削除。

1.0.6 (2018-01-30)
* パッケージ内のアクションを対象にした `ibmcloud wsk service bind` コマンドのバグ修正。
