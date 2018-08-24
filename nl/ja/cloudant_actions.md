---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Cloudant パッケージ
{: #cloudant_actions}

`/whisk.system/cloudant` パッケージを使用すると、[{{site.data.keyword.cloudant}}](/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant) データベースを操作できます。このパッケージには、以下のアクションとフィードが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | パッケージ | dbname、host、username、password | Cloudant データベースを処理。 |
| `/whisk.system/cloudant/read` | アクション | dbname、id | データベースから文書を読み取る。 |
| `/whisk.system/cloudant/write` | アクション | dbname、overwrite、doc | データベースに文書を書き込む。 |
| `/whisk.system/cloudant/changes` | フィード | dbname、filter、query_params、maxTriggers | データベース変更時のトリガー・イベントの起動。 |
{: shortdesc}

以下のセクションでは、{{site.data.keyword.cloudant_short_notm}} データベースをセットアップする方法と、データベースの読み取りおよび書き込みの方法を説明します。
`/whisk.system/cloudant` パッケージでフィードを使用する方法について詳しくは、[{{site.data.keyword.cloudant_short_notm}} イベント・ソース](./openwhisk_cloudant.html)を参照してください。

## {{site.data.keyword.Bluemix_notm}} での {{site.data.keyword.cloudant_short_notm}} データベースのセットアップ
{: #cloudantdb_cloud}

{{site.data.keyword.Bluemix_notm}} から {{site.data.keyword.openwhisk}} を使用する場合、{{site.data.keyword.cloudant_short_notm}} サービス・インスタンス用のパッケージ・バインディングを {{site.data.keyword.openwhisk_short}} が自動的に作成します。 {{site.data.keyword.Bluemix_notm}} から {{site.data.keyword.openwhisk_short}} および {{site.data.keyword.cloudant_short_notm}} を使用していない場合は、次のセクションにスキップしてください。

1. [{{site.data.keyword.Bluemix_notm}}ダッシュボード](http://console.bluemix.net)で {{site.data.keyword.cloudant_short_notm}} サービス・インスタンスを作成します。

  新規サービス・インスタンスごとに 1 つの資格情報キーを作成するように注意してください。

2. 名前空間でパッケージを最新表示します。 このリフレッシュにより、資格情報キーが定義された各 {{site.data.keyword.cloudant_short_notm}} サービス・インスタンスごとに 1 つのパッケージ・バインディングが自動的に作成されます。
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  出力例:
  ```
  created bindings:
  Bluemix_testCloudant_Credentials-1
  ```
  {: screen}

  ```
  ibmcloud fn package list
  ```
  {: pre}

  出力例:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 private binding
  ```
  {: screen}

  これで、パッケージ・バインディングには、{{site.data.keyword.cloudant_short_notm}} サービス・インスタンスと関連付けられた資格情報が含まれるようになります。

3. 前に作成されたパッケージ・バインディングが {{site.data.keyword.cloudant_short_notm}} {{site.data.keyword.Bluemix_notm}} サービス・インスタンスのホストと資格情報で構成されていることを確認します。

  ```
  ibmcloud fn package get /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 parameters
  ```
  {: pre}

  出力例:
  ```
  ok: got package /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1, displaying field parameters

  [
      {
          "key": "username",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix"
      },
      {
          "key": "host",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix.cloudant.com"
      },
      {
          "key": "password",
          "value": "c9088667484a9ac827daf8884972737"
      }
  ]
  ```
  {: screen}

## {{site.data.keyword.Bluemix_notm}} 外部での {{site.data.keyword.cloudant_short_notm}} データベースのセットアップ
{: #cloudantdb_nocloud}

{{site.data.keyword.openwhisk_short}} を {{site.data.keyword.Bluemix_notm}} で使用しない場合、または、{{site.data.keyword.cloudant_short_notm}} データベースを {{site.data.keyword.Bluemix_notm}} の外部でセットアップしたい場合は、ご使用の {{site.data.keyword.cloudant_short_notm}} アカウント用のパッケージ・バインディングを手動で作成する必要があります。 {{site.data.keyword.cloudant_short_notm}} アカウントのホスト名、ユーザー名、およびパスワードが必要です。

1. {{site.data.keyword.cloudant_short_notm}} アカウント用に構成されるパッケージ・バインディングを作成します。
  ```
  wsk package bind /whisk.system/cloudant myCloudant -p username MYUSERNAME -p password MYPASSWORD -p host MYCLOUDANTACCOUNT.cloudant.com
  ```
  {: pre}


2. このパッケージ・バインディングが存在することを確認します。
  ```
  wsk package list
  ```
  {: pre}

  出力例:
  ```
  packages
  /myNamespace/myCloudant private binding
  ```
  {: screen}

## {{site.data.keyword.cloudant_short_notm}} データベースからの読み取り
{: #cloudant_read}

アクションを使用して、**testdb** という名前の {{site.data.keyword.cloudant_short_notm}} データベースから文書を取り出すことができます。 このデータベースが、ご使用の {{site.data.keyword.cloudant_short_notm}} アカウントに必ず存在するようにしてください。

- 前に作成したパッケージ・バインディング内で **read** アクションを使用して文書を取り出します。 `/_/myCloudant` は実際のパッケージ名に置き換えてください。
  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}

  出力例:
  ```
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```
  {: screen}

## {{site.data.keyword.cloudant_short_notm}} データベースへの書き込み
{: #cloudant_write}

アクションを使用して、**testdb** という名前の {{site.data.keyword.cloudant_short_notm}} データベース内に文書を保管できます。 このデータベースが、ご使用の {{site.data.keyword.cloudant_short_notm}} アカウントに必ず存在するようにしてください。

1. 前に作成したパッケージ・バインディング内で **write** アクションを使用して文書を保管します。 `/_/myCloudant` は実際のパッケージ名に置き換えてください。
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  {: pre}

  出力例:
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. {{site.data.keyword.cloudant_short_notm}} ダッシュボードで文書を参照して、文書が存在していることを確認します。

  **testdb** データベースのダッシュボード URL は、`https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100` のようになります。
