---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-28"

keywords: packages, installable packages

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# インストール可能パッケージの使用
{: #installable-packages-overview}

インストール可能パッケージは、{{site.data.keyword.openwhisk}} 内のパッケージに対する管理と対話を行うための新しい設計を示すものです。
{: shortdesc}

## 概要
{: #overview}

**インストール可能パッケージとは何か?**

パッケージというのは、関連するアクションとフィードがバンドルされたセットのことです。 各パッケージは、サービスおよびイベント・プロバイダーと特定の対話を行うよう設計されています。 インストール可能パッケージというのは、ユーザーのニーズに基づいて選択、インストール、および編集できるパッケージのことです。

**インストール可能パッケージは事前インストール済みパッケージとどう違うか?**

[事前インストール済みパッケージ](/docs/openwhisk?topic=cloud-functions-openwhisk_packages#browse-packages)は、`/whisk.system` 名前空間内で、{{site.data.keyword.openwhisk_short}} に自動的に登録されます。 資格情報やその他のパラメーターを事前インストール済みパッケージに格納するには、[パッケージ・バインディング](/docs/openwhisk?topic=cloud-functions-openwhisk_packages#openwhisk_package_bind)を作成する必要があります。

インストール可能パッケージは、{{site.data.keyword.openwhisk_short}} システム内には存在しません。 その代わり、インストール可能パッケージは外部にある個々の Github リポジトリーに格納されています。 これらのパッケージは、`ibmcloud fn deploy` コマンドを使用して、直接独自の名前空間にインストールすることができます。パッケージにカスタム名を付けることもできます。パッケージはユーザー独自の名前空間にインストールされるので、必要に応じてパッケージ内のアクションとフィードを変更することができます。

## wskDeploy を使用したパッケージのインストール
{: #installing}

始める前に:

  1. [{{site.data.keyword.Bluemix_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli)。

パッケージをインストールするには、以下のようにします。

1. パッケージ・リポジトリーを複製します。 パッケージのリポジトリーは、この文書セット内でパッケージごとに個別のページになっています。
    ```
    git clone https://github.com/<package_repo>
    ```
    {: pre}

2. `manifest.yaml` ファイル、または `manifest.yml` ファイルが含まれているパッケージ・ディレクトリーにナビゲートします。 `manifest.yaml` ファイルには、自分の名前空間にインストールするパッケージとアクション、および `ibmcloud fn deploy` コマンドと併せて含まれている必要のあるすべてのメタデータなど、パッケージの全体構造が指定されています。 `manifest.yaml` ファイルについて詳しくは、[wskdeploy 文書 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example) を参照してください。
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. パッケージをデプロイします。 一部のパッケージでは、パッケージが適切に機能できるようにするための、特定の環境変数が必要です。
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

### {{site.data.keyword.cos_full_notm}}  パッケージの使用例
{: #example}

パッケージのインストール方法の例を確認するには、[{{site.data.keyword.cos_short}} パッケージ](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions)を調べてください。 {{site.data.keyword.cos_full}} は、イメージ、ビデオ、音楽、テキストなど、すべてのタイプのファイルを格納できるようにするサービスです。 このファイルと対話すると、キー/値のペアが含まれているクラウド・ベースのデータ・ストアがバケットに格納されます。 したがって、[{{site.data.keyword.cos_short}} パッケージ](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions)を使用するには、最初に {{site.data.keyword.cos_short}} サービス・インスタンスを作成し、次にバケットを作成する必要があります。 バケットは、このパッケージをインストールするために必要な環境変数として使用されます。

サービス・インスタンスとバケットを作成した後、パッケージをインストールするには、以下のコマンドの実行が必要です。

1. パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. `manifest.yaml` が含まれているパッケージ・ディレクトリーにナビゲートします。 以下の例では、{{site.data.keyword.cos_short}} パッケージの Node.js ランタイム・バージョンが使用されています。
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. バケットを環境変数として使用して、パッケージをデプロイします。 このパッケージの名前は `PACKAGE_NAME` 環境変数に依存しているので、パッケージにはカスタム名を与えることができます。
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> ibmcloud fn deploy
    ```
    {: pre}
