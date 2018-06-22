---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} CLI プラグイン
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk_short}} は、{{site.data.keyword.openwhisk_short}} システムの完全な管理を可能にする {{site.data.keyword.Bluemix_notm}} CLI 用の強力なプラグインを提供します。
{: shortdesc}

## {{site.data.keyword.Bluemix_notm}} CLI のセットアップ
{: #bluemix_cli_setup}

[{{site.data.keyword.Bluemix_notm}} CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html) をダウンロードしてインストールします。

## {{site.data.keyword.openwhisk_short}} プラグインのセットアップ
{: #cloudfunctions_plugin_setup}

{{site.data.keyword.openwhisk_short}} プラグインをインストールするには、以下のコマンドを実行します。
```
ibmcloud plugin install cloud-functions
```
{: pre}


{{site.data.keyword.openwhisk_short}} プラグインが正常にインストールされたことを確認するために、以下のコマンドを実行します。
```
ibmcloud plugin list cloud-functions
```
{: pre}


以下のように、出力にはインストールされている {{site.data.keyword.openwhisk_short}} のバージョンが表示されます。
```
Plugin Name          Version
Cloud-Functions      1.0.0
```

以下のコマンドを実行して、{{site.data.keyword.openwhisk_short}} プラグインをアップグレードできます。
```
ibmcloud plugin update Cloud-Functions
```
{: pre}


プラグイン・コマンドについて詳しくは、`ibmcloud plugin --help` を使用するか、以下の資料を確認してください。
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_plugin_list

## {{site.data.keyword.Bluemix_notm}} CLI 認証
{: #bluemix_cli_auth}

{{site.data.keyword.openwhisk_short}} は、2 つの {{site.data.keyword.Bluemix_notm}} 地域で使用可能です。

{{site.data.keyword.Bluemix_notm}} CLI にログインする際に、組織およびスペースとともに、ターゲット地域の {{site.data.keyword.Bluemix_notm}} API エンドポイントを指定できます。

米国南部地域にログインするには、以下のようにします。
```
ibmcloud login -a api.ng.bluemix.net
```
{: pre}


英国地域にログインするには、以下のようにします。
```
ibmcloud login -a api.eu-gb.bluemix.net
```
{: pre}


`-a` フラグは、使用する {{site.data.keyword.Bluemix_notm}} API エンドポイントを指定します。API エンドポイントを指定した場合、`-a` オプションは不要です。`ibmcloud api` コマンドを使用して、{{site.data.keyword.Bluemix_notm}} API エンドポイントを明示的に設定できます。現在の API エンドポイント設定を表示するには、`ibmcloud target` コマンドを使用します。

`login` コマンドでは、組織、スペース、パスワードなどの情報を求めるプロンプトが出されます (指定しなかった場合)。コマンド・ラインで組織およびスペースを指定することで、それらを求めるプロンプトをスキップできます。
```
ibmcloud login -o <MY_ORG> -s <MY_SPACE>
```
{: pre}


また、{{site.data.keyword.Bluemix_notm}} API キーを使用してログインすることもできます。この方法は、フラグ `--sso` を指定してログインする必要があるフェデレーテッド・ログインがアカウントに対して構成されている場合に役立ちます。API キーの使用は、継続的統合 (CI) を設定する場合および無人パイプラインを構成する場合にも有益です。
https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key

{{site.data.keyword.Bluemix_notm}} CLI を使用して新規 API キーを作成するには、以下のコマンドを実行します。
```
ibmcloud iam api-key-create MyKey
```
{: pre}


次に、以下の例のように、API キーの生成値を使用してログインします。
```
ibmcloud login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
```
{: pre}


login コマンドについて詳しくは、`ibmcloud login --help` を使用するか、以下の資料を確認してください。
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login


## {{site.data.keyword.openwhisk_short}} CLI プラグインの使用
{: #cloudfunctions_plugin_usage}

セットアップを検証します。以下の例のように、引数として `hello` を渡して、echo のブロッキング (同期) 呼び出しを実行します。
```
ibmcloud wsk action invoke whisk.system/utils/echo -p message hello --result
```
{: pre}


以下の出力例を参照してください。
```
{
    "message":"hello"
}
```

環境を構成した後には、{{site.data.keyword.openwhisk_short}} CLI を使用して以下のタスクを実行できます。

* {{site.data.keyword.openwhisk_short}} でコード・スニペット (アクション) を実行する。『[アクションの作成と呼び出し](./openwhisk_actions.html)』を参照してください。
* トリガーおよびルールを使用して、アクションでイベントに応答できるようにする。『[トリガーとルールの作成](./openwhisk_triggers_rules.html)』を参照してください。
* パッケージでアクションをバンドルする方法および外部イベント・ソースを構成する方法を学習する。『[パッケージの作成と使用](./openwhisk_packages.html)』を参照してください。
* パッケージ・カタログを検討し、[Cloudant イベント・ソース](./openwhisk_cloudant.html)などの外部サービスでアプリケーションを強化する。『[{{site.data.keyword.openwhisk_short}} 対応サービスの使用](./openwhisk_catalog.html)』を参照してください。

{{site.data.keyword.openwhisk_short}} プラグインのコマンドのリストを表示するには、引数を指定せずに `ibmcloud wsk` を実行します。

## アクションからのサービスの使用
{: #binding_services}

{{site.data.keyword.openwhisk_short}} には、`service bind` コマンドが用意されています。このコマンドは、{{site.data.keyword.Bluemix_notm}} サービス資格情報を、実行時にコードで使用できるようにします。そのため、`service bind` コマンドを利用して、{{site.data.keyword.openwhisk_short}} で定義された任意のアクションに任意の {{site.data.keyword.Bluemix_notm}} サービスをバインドできます。

アクションからサービスを使用する方法に関する詳細なステップについては、『[アクションからのサービスの使用](./binding_services.html)』トピックを参照してください。

## HTTPS プロキシーを使用するための {{site.data.keyword.openwhisk_short}} CLI の構成
{: #cli_https_proxy}

HTTPS プロキシーを使用するように {{site.data.keyword.openwhisk_short}} CLI をセットアップできます。HTTPS プロキシーをセットアップするには、`HTTPS_PROXY` という名前の環境変数を作成する必要があります。フォーマット `{PROXY IP}:{PROXY PORT}` を使用して、
この変数を HTTPS プロキシーのアドレスとそのポートに設定する必要があります。

## 地域、組織、およびスペースの操作
{: #region_info}

実動前 (ステージング) デプロイメントおよび実動デプロイメントを処理するためのスペースを作成できます。それぞれに対してスペースを作成します。スペースを作成することで、{{site.data.keyword.openwhisk_short}} で 2 つの異なる名前空間が定義されるようにすることができます。

`ibmcloud iam space-create` を使用して、組織の下に「staging」や「production」などの追加スペースを作成できます。以下の例を参照してください。
```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```

`ibmcloud iam` コマンドについて詳しくは、資料 https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create を参照してください。

{{site.data.keyword.openwhisk_short}} には、名前空間の名前に対する制限があります。その制限について詳しくは、資料 https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities を参照してください。

既にログインしている場合は、{{site.data.keyword.Bluemix_notm}} CLI で `ibmcloud target` コマンドを実行して、地域、組織、およびスペースを切り替えることができます。

すべての {{site.data.keyword.Bluemix_notm}} 地域を表示するには、以下のコマンドを使用します。

{{site.data.keyword.openwhisk_short}} は、`us-south` 地域および `eu-gb` 地域でのみサポートされます。
{: tip}

```
ibmcloud regions

Name       Geolocation          Customer   Deployment   Domain                   CF API Endpoint                  Type
us-south   US South             IBM        Production   ng.bluemix.net     https://api.ng.bluemix.net             public
eu-gb      United Kingdom       IBM        Production   eu-gb.bluemix.net  https://api.eu-gb.bluemix.net          public
```

地域を変更するには、`ibmcloud target` コマンドを使用します。例えば、英国地域およびスペース `staging` に切り替える場合、以下のようにします。
```
ibmcloud target -r eu-gb -s staging
```
{: pre}


同じ地域内でスペースを変更する必要がある場合 (staging から production への変更など)、以下のコマンドを実行します。
```
ibmcloud target -s production
```
{: pre}


`target` コマンドについて詳しくは、`ibmcloud target --help` を使用するか、以下の資料を確認してください。
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_target

## OpenWhisk CLI から {{site.data.keyword.openwhisk_short}} CLI プラグインへのマイグレーション
{: #cli_migration}

{{site.data.keyword.openwhisk_short}} CLI プラグインの導入に伴い、OpenWhisk CLI は不要になりました。

### コマンド構文
{: #command_syntax}

すべての `wsk` コマンド (不要になった `wsk bluemix login` コマンドを除く) は、コマンド `ibmcloud wsk` を使用することで、同様に動作します。すべてのコマンドのオプションおよび引数は同じです。

### API 認証と API ホスト
{: #api_authentication}

OpenWhisk CLI を使用するには、認証 API キーおよび API ホストを構成する必要があります。
{{site.data.keyword.openwhisk_short}} CLI プラグインでは、API キーおよび API ホストを明示的に構成する必要はありません。`ibmcloud login` でログインし、`ibmcloud target` コマンドを使用して地域と名前空間のターゲットを指定する必要があります。それ以降、すべての後続のコマンドは、`ibmcloud wsk` で開始します。このように認証することで、{{site.data.keyword.Bluemix_notm}} Web ページにアクセスして CLI 環境を構成するための特定の情報を取得しなくても済むようにもなります。

cURL や Postman などの外部 HTTP クライアントから {{site.data.keyword.openwhisk_short}} を使用するために認証 API キーを取得する必要がある場合は、以下のコマンドで取得できます。

現在の API キーを取得するには、以下のコマンドを実行します。
```
ibmcloud wsk property get --auth
```
{: pre}


現在の API ホストを取得するには、以下のコマンドを実行します。
```
ibmcloud wsk property get --apihost
```
{: pre}


API キーは、{{site.data.keyword.openwhisk_short}} CLI プラグインのターゲットとなる地域、組織、およびスペースごとに固有です。
{: tip}

### API ゲートウェイ認証
{: #apigw_authentication}

現在、OpenWhisk CLI では、`wsk api` コマンドを使用して API の管理のために API ゲートウェイの許可を構成するには、`wsk bluemix login` を実行する必要があります。

{{site.data.keyword.openwhisk}} CLI プラグインでは、`wsk bluemix login` を実行する必要はなくなりました。代わりに、`ibmcloud login` コマンドを使用して {{site.data.keyword.Bluemix_notm}} にログインします。そうすると、{{site.data.keyword.openwhisk}} プラグインは、現在のログインとターゲットの情報を自動的に利用します。これで、`ibmcloud wsk api` コマンドを使用して、API を管理できます。

### デプロイメント・スクリプトのマイグレーション
{: #migrating_deploy_scripts}

`wsk` バイナリーで OpenWhisk CLI を使用するスクリプトがある場合、コマンド `ibmcloud wsk` を使用することで、すべてのコマンドは同様に動作します。{{site.data.keyword.Bluemix_notm}} CLI プラグインを使用するようにスクリプトを変更するか、別名またはラッパーを作成して、`wsk` の現在の実行が `ibmcloud wsk` に変換されるようにすることができます。{{site.data.keyword.Bluemix_notm}} CLI の `ibmcloud login` および `ibmcloud target` コマンドは、無人モードで動作します。無人モードでは、`ibmcloud wsk` コマンドを実行して {{site.data.keyword.openwhisk_short}} エンティティーをデプロイおよび管理する前に、環境を構成できます。


## バージョン履歴
{: #version_history}

以下は、バージョンの履歴レコードであり、ハイライトおよびバグ修正を示します。

1.0.7 (2018-02-02)
* `ibmcloud wsk api` が `/api/{id}` などのパス・パラメーターを受け入れるように変更。詳しくは、『[API ゲートウェイ](./openwhisk_apigateway.html)』を参照。
* リストア・プロキシーのサポート。
* `swift:3` を削除。

1.0.6 (2018-01-30)
* パッケージ内のアクションでのコマンド `ibmcloud wsk service bind` のバグを修正。
