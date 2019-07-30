---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: packages, installable packages, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# パッケージの取り込み
{: #pkg_ov}

パッケージというのは、関連するアクションとフィードがバンドルされたセットのことです。 各パッケージは、サービスおよびイベント・プロバイダーと特定の対話を行うよう設計されています。 ユーザーが使用できるように {{site.data.keyword.openwhisk}} と一緒に既にインストールされているパッケージもありますが、他のパッケージをユーザーがインストールすることもできます。
{: shortdesc}

## 概要
{: #pkg_overview}

[事前インストール済みパッケージ](/docs/openwhisk?topic=cloud-functions-pkg_ov#pkg_browse)は、`/whisk.system` 名前空間の {{site.data.keyword.openwhisk_short}} に自動で登録されます。 インストール手順を実行しなくても使用できます。

インストール可能パッケージは、ユーザーが要件に応じてインストール、編集、使用できるパッケージです。 インストール可能パッケージは、{{site.data.keyword.openwhisk_short}} システム内には存在しません。 その代わり、インストール可能パッケージは外部にある個々の GitHub リポジトリーに格納されています。

ユーザーはこのようなパッケージや独自のパッケージを名前空間にインストールできます。また、パッケージにカスタム名を付けることもできます。 パッケージはユーザー独自の名前空間にインストールされるので、必要に応じてパッケージ内のアクションとフィードを変更することができます。



## 事前インストール済みパッケージの参照
{: #pkg_browse}

複数のパッケージが既に {{site.data.keyword.openwhisk_short}} に自動で登録されています。 名前空間内のパッケージのリストを取得したり、パッケージ内のエンティティーをリストしたり、パッケージ内の個々のエンティティーの説明を取得したりできます。
{: shortdesc}

1. `/whisk.system` 名前空間内のパッケージのリストを取得します。
  ```
  ibmcloud fn package list /whisk.system
  ```
  {: pre}

  package list の出力:
  ```
  packages
  /whisk.system/cloudant                                                 shared
  /whisk.system/alarms                                                   shared
  /whisk.system/watson                                                   shared
  /whisk.system/websocket                                                shared
  /whisk.system/weather                                                  shared
  /whisk.system/system                                                   shared
  /whisk.system/utils                                                    shared
  /whisk.system/slack                                                    shared
  /whisk.system/samples                                                  shared
  /whisk.system/github                                                   shared
  /whisk.system/pushnotifications                                        shared
  ```
  {: screen}

2. 特定のパッケージ内のエンティティーのリストを取得します。

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **例**
  ```
  ibmcloud fn package get --summary /whisk.system/cloudant
  ```
  {: pre}

  **出力例**
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.cloud_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  この出力は、{{site.data.keyword.cloudant_short_notm}} パッケージにアクションとフィードが含まれていることを示しています。 例えば、`read` と `write` の 2 つのアクションと、`changes` という 1 つのトリガー・フィードがあります。 `changes` フィードによって、指定された {{site.data.keyword.cloudant_short_notm}} データベースに文書が追加されるとトリガーが起動されます。

  {{site.data.keyword.cloudant_short_notm}} パッケージでは、パラメーター `username`、`password`、`host`、および `port` も定義されています。 アクションおよびフィードを有意味なものにするために、これらのパラメーターを指定する必要があります。 例えば、これらのパラメーターによって、アクションは特定の {{site.data.keyword.cloudant_short_notm}} アカウントで作動することができます。

3. アクションまたはフィードの説明を取得して、必要なパラメーターを確認します。

  **例**
  ```
  ibmcloud fn action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  **出力例**
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```
  {: screen}

  この出力は、{{site.data.keyword.cloudant_short_notm}} `read` アクションには、データベースおよび取得する文書 ID などの 3 つのパラメーターが必要であることを示しています。



## 事前インストール済みパッケージへのパラメーターのバインド
{: #pkg_bind}

パッケージ内のエンティティーを直接使用することができますが、いつも同じパラメーターをアクションに渡している場合があります。 こういった処理を単純化するため、パッケージにバインドしてデフォルト・パラメーターを指定し、パッケージ内のアクションがそれを継承するようにできます。
{: shortdesc}

例えば、`/whisk.system/cloudant` パッケージでは、`username`、`password`、および `dbname` のデフォルト値をパッケージ・バインディング内に設定できます。そうすると、それらの値がこのパッケージ内のどのアクションにも自動的に渡されるようになります。

次の例では、`/whisk.system/samples` パッケージにバインドします。

1. `/whisk.system/samples` パッケージにバインドし、`place` パラメーターのデフォルト値を設定します。
  ```
  ibmcloud fn package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  **出力例**
  ```
  ok: created binding valhallaSamples
  ```
  {: screen}

2. パッケージ・バインディングの説明を取得します。
  ```
  ibmcloud fn package get --summary valhallaSamples
  ```
  {: pre}

  **出力例**
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Returns a friendly greeting
   action /myNamespace/valhallaSamples/wordCount: Count words in a string
   action /myNamespace/valhallaSamples/helloWorld: Demonstrates logging facilities
   action /myNamespace/valhallaSamples/curl: Curl a host url
  ```
  {: screen}

  `/whisk.system/samples` パッケージ内のすべてのアクションが `valhallaSamples` パッケージ・バインディング内で使用可能であることに注目してください。

3. パッケージ・バインディング内のアクションを呼び出します。
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  **出力例**
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  この結果では `valhallaSamples` パッケージ・バインディングを作成したときに設定した `place` パラメーターをアクションが継承していることに注目してください。

4. アクションを呼び出し、パラメーターのデフォルト値を上書きします。
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  **出力例**
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  このアクション呼び出しで指定された `place` パラメーター値が、`valhallaSamples` パッケージ・バインディングに設定したデフォルト値を上書きしていることに注目してください。



## 独自パッケージの追加
{: #pkg_add}

ローカル・コードのパッケージを作成したり、任意の GitHub リポジトリーの複製を作成したりできます。
{: shortdesc}

**始める前に**
- [{{site.data.keyword.cloud_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](/docs/openwhisk?topic=cloud-functions-cli_install)。
- アプリ用に `manifest.yaml` ファイルまたは `manifest.yml` ファイルを作成し、ルート・ディレクトリーに保管します。 `manifest.yaml` ファイルには、パッケージの全体的な構造を指定します。`ibmcloud fn deploy` コマンドに指定する必要があるメタデータなども含めます。 `manifest.yaml` ファイルについて詳しくは、[<ph class="ignoreSpelling">wskdeploy</ph> の資料](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example){: external}を参照してください。

パッケージを追加するには、以下のようにします。

1. パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/ORG_NAME/REPOSITORY_NAME
    ```
    {: pre}

2. `manifest.yaml` ファイルが入っているディレクトリーにナビゲートします。
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. パッケージをデプロイします。
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    パッケージによっては、特定の環境変数が設定されていないと正常に機能しないものがあります。 その場合は、`deploy` コマンドに環境変数を指定します。 例えば、パッケージの名前を選択し、PACKAGE_NAME 変数にその名前を指定したりします。

    ```
    PACKAGE_NAME=CUSTOM_PACKAGE_NAME VARIABLE_NAME=VARIABLE_VALUE ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

### {{site.data.keyword.cos_full_notm}} パッケージの例
{: #pkg_ex}

パッケージのインストール方法の例を確認するには、[{{site.data.keyword.cos_full_notm}} パッケージ](/docs/openwhisk?topic=cloud-functions-pkg_obstorage)を参照してください。 {{site.data.keyword.cos_full}} は、イメージ、ビデオ、音楽、テキストなど、すべてのタイプのファイルを格納できるようにするサービスです。 ファイルと対話するために、キーと値のペアの Cloud クラウド・ベースのデータ・ストアがバケットに保管されます。したがって、[{{site.data.keyword.cos_full_notm}} パッケージ](/docs/openwhisk?topic=cloud-functions-pkg_obstorage)を使用するには、最初に {{site.data.keyword.cos_full_notm}} サービス・インスタンスを作成し、次にバケットを作成する必要があります。 バケットは、このパッケージをインストールするために必要な環境変数として使用されます。

サービス・インスタンスとバケットを作成した後、以下のコマンドを使用してパッケージをインストールできます。

1. パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. `manifest.yaml` が含まれているパッケージ・ディレクトリーにナビゲートします。 以下の例では、{{site.data.keyword.cos_full_notm}} パッケージの Node.js ランタイム・バージョンが使用されています。
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. バケットを環境変数として使用して、パッケージをデプロイします。  `PACKAGE_NAME` 環境変数を使用して、パッケージにカスタム名を付けることができます。
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> ibmcloud fn deploy
    ```
    {: pre}



