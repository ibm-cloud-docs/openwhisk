---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: functions cli, serverless, cli, install, functions plug-in

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


# CLI とプラグインのインストール
{: #cli_install}

{{site.data.keyword.openwhisk}} は、{{site.data.keyword.openwhisk_short}} システムの完全な管理を可能にする {{site.data.keyword.cloud_notm}} CLI 用の強力なプラグインを提供します。 {{site.data.keyword.openwhisk_short}} CLI プラグインを使用すると、アクションでのコード・スニペットの管理、アクションがイベントに応答できるようにするトリガーおよびルールの作成、パッケージへのアクションのバンドルを行うことができます。
{:shortdesc}


## {{site.data.keyword.cloud_notm}} CLI のセットアップ
{: #cli_setup}

**始める前に**

[{{site.data.keyword.cloud_notm}} アカウント](https://cloud.ibm.com/){: external}を作成しなければなりません。

{{site.data.keyword.cloud_notm}} CLI をダウンロードしてインストールし、ログインします。
{: shortdesc}

1. [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli) をダウンロードしてインストールします。

2. {{site.data.keyword.cloud_notm}} CLI にログインします。 {{site.data.keyword.cloud_notm}} 地域を指定するには、[API エンドポイントを含めます](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions)。

  ```
  ibmcloud login
  ```
  {: pre}

3. プロンプトに従って {{site.data.keyword.cloud_notm}} アカウントを選択します。

4. リソース・グループのリストを取得します。 

```
ibmcloud resource groups
```
{: pre}

**出力例**

```
Retrieving all resource groups under account <account_name> as email@ibm.com...
OK
Name      ID                                 Default Group   State   
default   a8a12accd63b437bbd6d58fb8b462ca7   true            ACTIVE
test      a8a12accd63b437bbd6d58fb8b462ca7   false           ACTIVE
```
{: screen}

5. オプション: 以下のコマンドを実行して、デフォルト以外のリソース・グループをターゲットにします。
```
ibmcloud target -g <resource_group>
```
{: pre}


**出力例**

```
Targeted resource group <resource_group>
```
{: screen}

## {{site.data.keyword.openwhisk_short}} CLI プラグインのセットアップ
{: #cli_plugin_setup}

{{site.data.keyword.openwhisk_short}} で作業する場合は、CLI プラグインをダウンロードしてインストールします。
{: shortdesc}

{{site.data.keyword.openwhisk_short}} CLI プラグインを使用して、以下のタスクを実行できます。

* {{site.data.keyword.openwhisk_short}} でコード・スニペット (アクション) を実行する。 『[アクションの作成と呼び出し](/docs/openwhisk?topic=cloud-functions-actions)』を参照してください。
* トリガーおよびルールを作成して、アクションでイベントに応答できるようにする。『[トリガーとルールの作成](/docs/openwhisk?topic=cloud-functions-triggers)』を参照してください。
* アクションをバンドルして外部イベント・ソースを構成する。『[パッケージの作成と使用](/docs/openwhisk?topic=cloud-functions-pkg_ov)』を参照してください。
* パッケージ・カタログを検討し、外部サービスでアプリケーションを強化する。 [{{site.data.keyword.cloud_notm}} の追加](/docs/openwhisk?topic=cloud-functions-services)を参照してください。

{{site.data.keyword.openwhisk_short}} CLI プラグインをインストールするには、以下の手順を実行します。

1. {{site.data.keyword.openwhisk_short}} プラグインをインストールします。

  ```
  ibmcloud plugin install cloud-functions
  ```
  {: pre}

2. プラグインがインストールされたことを確認します。

  ```
  ibmcloud plugin list
  ```
  {: pre}

  **出力**
  ```
  Plugin Name          Version
  cloud-functions/wsk/functions/fn      1.0.32
  ```
  {: screen}

3. ログイン後は、すべての {{site.data.keyword.openwhisk_short}} コマンドを `ibmcloud fn` で開始します。 {{site.data.keyword.openwhisk_short}} プラグインで実行できることをすべて表示するには、引数なしで `ibmcloud fn` を実行します。
  ```
  ibmcloud fn
  ```
  {: pre}




## {{site.data.keyword.openwhisk_short}} 名前空間のターゲット設定
{: #cli_regions}
デフォルトでは、{{site.data.keyword.openwhisk_short}} は [IAM 対応の名前空間](/docs/iam?topic=iam-iamoverview){: external}を使用します。Cloud Foundry ベースの名前空間は、作成できなくなりました。
{: important}

### 名前空間を作成するか、ターゲットにします。
{{site.data.keyword.openwhisk_short}} 名前空間のリストを取得するには、`ibmcloud fn namespace list` を実行します。

#### IAM 対応の名前空間を作成します。
  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description">]
  ```
  {: pre}

**応答**
  ```
  ok: created namespace <namespace_name>
  ```
  {: screen}


#### IAM 対応の名前空間をターゲットにします。 
  ```
  ibmcloud fn property set --namespace <namespace_name>
  ``` 
  {: pre}


**応答**
  ```
  ok: whisk namespace set to <namespace_name>
  ```
  {: screen}
  
#### Cloud Foundry ベースの名前空間をターゲットにします。 
  
`-o` フラグと `-s` フラグを使用して特定の `org` と `space` をターゲットにするか、プロンプトに従うことができます。

* `target` コマンド内で `org` と `space` の名前を含めて、Cloud Foundy 名前空間をターゲットにします。

```
ibmcloud target --cf  -o <org> -s <space>
```
{: pre}

* Cloud Foundry をターゲットにして、プロンプトに従い、`org` と `space` を選択します。

```
ibmcloud target --cf
```
{: pre}


**応答**
  ```
  Targeted Cloud Foundry (https://api.ng.bluemix.net)

  Targeted org <org_name>

  Targeted space <space_name>
                        
  API endpoint:      https://cloud.ibm.com   
  Region:            us-south   
  User:              <email>   
  Account:           (<account_id>) <-> <account>   
  Resource group:    default   
  CF API endpoint:   https://api.ng.bluemix.net (API version: 2.128.0)   
  Org:               <org_name>   
  Space:             <space_name>  
  ```
  {: screen} 





#### オプション: ステージング・デプロイメントと実動デプロイメント用の名前空間を作成します。

実動前 (ステージング) と実動の {{site.data.keyword.openwhisk_short}} デプロイメントを処理するための IAM 対応の名前空間を作成できます。それぞれに名前空間を作成します。組織の下に「staging」や「production」などの名前空間をさらに作成するには、[`ibmcloud fn namespace create`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace_create) を実行します。

ステージング名前空間を作成します。
```
ibmcloud fn namespace create staging
```
{: pre}

実動名前空間を作成します。
```
ibmcloud fn namespace create production
```
{: pre}

{{site.data.keyword.openwhisk_short}} には、名前空間の名前に対する制限があります。 詳しくは、[システムの詳細および制限](/docs/openwhisk?topic=cloud-functions-limits#limits_entities)の資料を参照してください。
{: tip}


## HTTPS プロキシーを使用するための {{site.data.keyword.openwhisk_short}} CLI の構成
{: #cli_proxy}

HTTPS プロキシーを使用するように {{site.data.keyword.openwhisk_short}} CLI をセットアップできます。 HTTPS プロキシーをセットアップするには、`HTTPS_PROXY` という名前の環境変数を作成する必要があります。 フォーマット `{PROXY IP}:{PROXY PORT}` を使用して、
この変数を HTTPS プロキシーのアドレスとそのポートに設定する必要があります。

`org` または `space` の名前を変更すると、その変更後の名前に基づいて新しい名前空間が作成されます。古い名前空間に入っていたエンティティーは新しい名前空間に表示されず、削除されることがあります。
{: important}


## OpenWhisk CLI から {{site.data.keyword.openwhisk_short}} CLI プラグインへのマイグレーション
{: #cli_migrate}

{{site.data.keyword.openwhisk_short}} CLI プラグインを使用して {{site.data.keyword.openwhisk_short}} エンティティーと対話できるようになりました。 スタンドアロンの OpenWhisk CLI を引き続き使用することも可能ですが、そこには {{site.data.keyword.openwhisk_short}} でサポートされる最新のフィーチャー (IAM ベースの名前空間や `service bind` など) が含まれていません。
{: shortdesc}


### コマンド構文
{: #cli_syntax}

Cloud Functions CLI プラグインで使用できるコマンドのすべてのコマンド・オプションと引数は、[スタンドアロンの OpenWhisk CLI](https://github.com/apache/incubator-openwhisk-cli){: external} で使用できるオプションと同じです。ただし、以下の違いに注意してください。

* {{site.data.keyword.openwhisk}} プラグインによって、現在のログインとターゲットの情報が自動的に利用されます。
* `wsk` コマンドが `ibmcloud fn` として実行されるようになりました。
* `wsk ibmcloud login` コマンドは不要になりました。`ibmcloud login` を使用してサインインできます。
* `ibmcloud fn api` を使用して、API を管理できます。

詳しくは、[{{site.data.keyword.openwhisk_short}} CLI リファレンス](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli)を参照してください。

### API 認証と API ホスト
{: #cli_api_auth}

{{site.data.keyword.openwhisk_short}} CLI プラグインでは、API キーおよび API ホストを明示的に構成する必要はありません。 代わりに、`ibmcloud login` でログインできます。 `ibmcloud fn property set --namespace <namespace_name>` を実行して IAM 対応の名前空間をターゲットにするか、`ibmcloud target --cf` を実行して Cloud Foundry ベースの名前空間をターゲットにすることができます。 ログイン後は、すべてのコマンドを `ibmcloud fn` で開始します。


cURL や Postman などの外部 HTTP クライアントで {{site.data.keyword.openwhisk_short}} の認証 API キーを使用する必要がある場合は、以下のコマンドで取得できます。

以下のコマンドを実行して、現在の API キーを取得します。
```
ibmcloud fn property get --auth
```
{: pre}

以下のコマンドを実行して、現在の API ホストを取得します。
```
ibmcloud fn property get --apihost
```
{: pre}

API キーは、{{site.data.keyword.openwhisk_short}} CLI プラグインのターゲットとなる地域、組織、およびスペースごとに固有です。
{: tip}


### API ゲートウェイ認証
{: #cli_apigw_authentication}

OpenWhisk CLI では、`wsk api` コマンドを使用して API の管理のために API ゲートウェイの許可を構成するには、`wsk ibmcloud login` を実行する必要がありました。 {{site.data.keyword.openwhisk_short}} CLI プラグインでは、`wsk ibmcloud login` を実行する必要はありません。 代わりに、`ibmcloud login` コマンドを使用して {{site.data.keyword.cloud_notm}} にログインすると、{{site.data.keyword.openwhisk}} プラグインによって、現在のログインとターゲットの情報が自動的に利用されます。 これで、`ibmcloud fn api` コマンドを使用して、API を管理できます。


### デプロイメント・スクリプトのマイグレーション
{: #cli_migrating_deploy_scripts}

`wsk` コマンドで OpenWhisk CLI を使用するスクリプトがある場合、コマンド `ibmcloud fn` を使用することで、すべてのコマンドは同様に動作します。 {{site.data.keyword.cloud_notm}} CLI プラグインを使用するようにスクリプトを変更するか、別名またはラッパーを作成して、`wsk` を使用している現行コマンドが `ibmcloud fn` に変換されるようにすることができます。 {{site.data.keyword.cloud_notm}} CLI の `ibmcloud login` および `ibmcloud target` コマンドは、無人モードで動作します。 無人モードでは、`ibmcloud fn` コマンドを実行して {{site.data.keyword.openwhisk_short}} エンティティーをデプロイおよび管理する前に、環境を構成できます。

## CLI のバージョン履歴
{: #cli_versions}

以下は、バージョンの履歴レコードであり、ハイライトおよびバグ修正を示します。

v1.0.30 (2019 年 4 月 3 日)
* `service bind` で IAM サービス、組織ベースのサービス、スペース・ベースのサービスを処理する方法が改善されました。
* API エンドポイント https://cloud.ibm.com の処理に関するフィックスが追加されました。

v1.0.29 (2019 年 2 月 6 日)
* マニフェスト・ファイルによって Functions エンティティーのコレクションのデプロイとアンデプロイを実行するためのコマンド `deploy` と `undeploy` が追加されました。詳しくは、[デプロイメント](/docs/openwhisk?topic=cloud-functions-deploy#deploy)の資料を参照してください。

v1.0.28 (2019 年 1 月 21 日)
* `update|delete|get namespace name` が複数回存在する場合のエラー・メッセージが追加されました。

v1.0.27 (2018 年 12 月 11 日)
* `namespace get` のフィックスが追加されました。
* ブラック・ボックス・アクションに関する `--save-as` のフィックスが追加されました。

v1.0.26 (2018 年 11 月 30 日)
* 新しい環境で認証キーを正しく返すための `fn property get --auth` が有効になりました。

v1.0.25 (2018 年 11 月 23 日)
* エラー・メッセージの結果の表示が改善されました。
* 名前空間のプロパティーを正しく表示するための `fn namespace get` のフィックスが追加されました。

1.0.23 (2018 年 10 月 15 日)
* Ruby (`.rb`) アクション・コードを認識するためのサポートが追加されました。

1.0.22 (2018 年 8 月 20 日)
* 米国東部地域のサポートが追加されました。

1.0.21 (2018 年 8 月 1 日)
* {{site.data.keyword.openwhisk_short}} コマンドで、次の例に示すように、別名 `fn` および `functions` が使用できるようになりました: `ibmcloud fn <command>` および `ibmcloud fn <command>`。 `ibmcloud wsk <command>` もまだ使用できます。

1.0.19 (2018 年 7 月 2 日)
* 小さいバグ修正および改善。

1.0.18 (2018 年 6 月 20 日)
* ユーザー提供のサービス・インスタンスをアンバインドするためのフィックスが追加されました。
* パフォーマンスの改善。

1.0.17 (2018 年 6 月 12 日)
* `ibmcloud cf create-user-provided-service` コマンドを使用して作成されたユーザー提供のサービス・インスタンスのバインド (`ibmcloud wsk service bind`) とアンバインド (`ibmcloud wsk service unbind`) を実行するためのサポートが追加されました。

1.0.16 (2018 年 5 月 24 日)
* 小さいバグ修正および改善。

1.0.15 (2018 年 5 月 21 日)
* 小さいバグ修正および改善。

1.0.14 (2018 年 5 月 17 日)
* 組織名とスペース名に含まれる `&` 文字のサポートが有効になりました。

1.0.13 (2018 年 5 月 7 日)
* 小さいバグ修正およびエラー処理の改善。

1.0.12 (2018 年 4 月 30 日)
* `bx` CLI との互換性を維持するための {{site.data.keyword.cloud_notm}} SDK の更新。

1.0.11 (2018 年 4 月 23 日)
* 小さいバグ修正および改善。

1.0.10 (2018 年 4 月 9 日)
* Web アクション・エンドポイントを保護するための新しい `--web-secure` オプションが `ibmcloud wsk action create|update` コマンドに追加されました。
* バックツーバック・パス・パラメーターの[問題](https://github.com/apache/incubator-openwhisk-cli/issues/237){: external}が修正されました。

1.0.9 (2018 年 3 月 16 日)
* パッケージ・レベルでのサービス・バインドのサポートが有効になりました。

1.0.8 (2018 年 2 月 22 日)
* IAM サービス・バインドのサポートが有効になりました。

1.0.7 (2018 年 2 月 2 日)
* `ibmcloud wsk api` が更新され、`/api/{id}` などのパス・パラメーターを指定できるようになりました。詳しくは、[API ゲートウェイ](/docs/openwhisk?topic=cloud-functions-apigateway)の情報を参照してください。
* プロキシー・サポートが復元されました。
* `swift:3` が削除されました。

1.0.6 (2018 年 1 月 30 日)
* パッケージ内のアクションを対象にした `ibmcloud wsk service bind` コマンドのバグが修正されました。



