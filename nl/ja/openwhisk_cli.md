---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} スタンドアロン CLI

**2018 年 3 月 9 日から、{{site.data.keyword.openwhisk_short}} スタンドアロン CLI は、ダウンロードできなくなります。この日付以降に {{site.data.keyword.openwhisk_short}} エンティティーの管理を続行する場合は、{{site.data.keyword.Bluemix}} CLI 用の [{{site.data.keyword.openwhisk_short}} CLI プラグイン](./bluemix_cli.html)を使用してください。**

{{site.data.keyword.openwhisk}} 分散 **wsk** コマンド・ライン・インターフェースにより、すべての {{site.data.keyword.openwhisk_short}} エンティティーを管理できます。
{: shortdesc}

<!--
This service is deprecated: All instances of this service are deprecated. Existing instances can be used until 09 December 2016. For more information, see the [deprecation announcement blog](http://www.com){: new_window}.
{:deprecated}

IBM recommends that you use the new [{{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.Bluemix_notm}} CLI](./bluemix_cli.html) to manage {{site.data.keyword.openwhisk_short}} entities. The following management tasks are easier if you use the plugin.
{: tip}
-->

* {{site.data.keyword.openwhisk_short}} 認証鍵の構成
  * 認証鍵を取得するために {{site.data.keyword.openwhisk_short}} コンソールにアクセスする必要がありません
  * 地域、組織、およびスペースを切り替えたときに、認証鍵の自動生成が行われます
* 有効期限が切れた {{site.data.keyword.openwhisk_short}} 認証鍵のリフレッシュ
* 新しいバージョンへの CLI の更新


以下の管理タスクでは、プラグインを使用する必要があります。

* API 管理
  * API GW アクセス・トークンの構成
  * 有効期限が切れた API GW アクセス・トークンのリフレッシュ

## {{site.data.keyword.openwhisk_short}} CLI のセットアップ 
{: #openwhisk_cli_setup}

{{site.data.keyword.openwhisk_short}} コマンド・ライン・インターフェース (CLI) を使用して、名前空間および許可キーをセットアップできます。
[「CLI の構成」](https://console.bluemix.net/openwhisk/cli?loadWsk=true)に移動し、手順に従ってインストールしてください。

まず、以下の 2 つのプロパティーを構成します。

1. 使用したい {{site.data.keyword.openwhisk_short}} デプロイメントの **API ホスト** (名前または IP アドレス)。
2. {{site.data.keyword.openwhisk_short}} API へのアクセスを認可する**許可キー** (ユーザー名とパスワード)。

以下の 2 つの {{site.data.keyword.Bluemix_notm}} 地域が使用可能であり、独自の固有の API ホストおよび許可キーが必要です。

* 米国南部
  * API ホスト: `openwhisk.ng.bluemix.net`

* 英国
  * API ホスト: `openwhisk.eu-gb.bluemix.net`

以下のコマンドを実行して、希望する {{site.data.keyword.Bluemix_notm}} 地域用に API ホストを設定します。

米国南部:
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

英国:
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

許可キーは地域ごとに固有のため、地域の切り替えが必要になった場合は、API ホストと許可キーの両方を使用して CLI を再構成する必要があります。
{: tip}

アクション、ルール、パッケージなどの成果物は、地域固有です。そのため、複数の地域で同じ成果物を使用する場合は、目的の各地域にその成果物をデプロイする必要があります。

許可キーが分かっている場合、それを使用するように CLI を構成できます。 

許可キーを設定するには、以下のコマンドを実行します。

```
wsk property set --auth <authorization_key>
```
{: pre}

**ヒント:** デフォルトでは、{{site.data.keyword.openwhisk_short}} CLI は、`~/.wskprops` に設定されたプロパティーを保管します。このファイルの場所は、`WSK_CONFIG_FILE` 環境変数を設定することにより変更できます。 

CLI セットアップを検証するには、[アクションの作成および実行](./index.html#openwhisk_start_hello_world)を試行します。

## {{site.data.keyword.openwhisk_short}} CLI の使用
{: #using_openwhisk_cli}

環境が構成された後には、{{site.data.keyword.openwhisk_short}} CLI を使用して以下のタスクを実行できます。

* {{site.data.keyword.openwhisk_short}} でコード・スニペット (アクション) を実行する。『[アクションの作成と呼び出し](./openwhisk_actions.html)』を参照してください。
* トリガーおよびルールを使用して、アクションでイベントに応答できるようにする。『[トリガーとルールの作成](./openwhisk_triggers_rules.html)』を参照してください。
* パッケージでアクションをバンドルする方法および外部イベント・ソースを構成する方法を学習する。『[パッケージの作成と使用](./openwhisk_packages.html)』を参照してください。
* パッケージ・カタログを検討し、[Cloudant イベント・ソース](./openwhisk_cloudant.html)などの外部サービスでアプリケーションを強化する。『[事前インストール済み {{site.data.keyword.openwhisk_short}} パッケージ](./openwhisk_catalog.html)』を参照してください。

## HTTPS プロキシーを使用するための CLI の構成
{: #cli_https_proxy}

HTTPS プロキシーを使用するように CLI をセットアップできます。HTTPS プロキシーをセットアップするには、`HTTPS_PROXY` という名前の環境変数を作成する必要があります。フォーマット `{PROXY IP}:{PROXY PORT}` を使用して、
この変数を HTTPS プロキシーのアドレスとそのポートに設定する必要があります。
