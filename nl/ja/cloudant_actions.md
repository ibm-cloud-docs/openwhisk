---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: cloudant, database, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Cloudant パッケージ
{: #cloudant_actions}

`/whisk.system/cloudant` パッケージを使用すると、[{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) データベースを操作できます。このパッケージには、以下のアクションとフィードが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | パッケージ | dbname、host、username、password | Cloudant データベースを処理。 |
| `/whisk.system/cloudant/read` | アクション | dbname、id | データベースから文書を読み取る。 |
| `/whisk.system/cloudant/write` | アクション | dbname、overwrite、doc | データベースに文書を書き込む。 |
| `/whisk.system/cloudant/changes` | フィード | dbname、iamApiKey、iamUrl、filter、query_params、maxTriggers | データベース変更時のトリガー・イベントの起動。 |
{: shortdesc}

以下のセクションでは、{{site.data.keyword.cloudant_short_notm}} データベースをセットアップする方法と、データベースの読み取りおよび書き込みの方法を説明します。
`/whisk.system/cloudant` パッケージでフィードを使用する方法について詳しくは、[{{site.data.keyword.cloudant_short_notm}} イベント・ソース](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant)を参照してください。

## {{site.data.keyword.Bluemix_notm}} での {{site.data.keyword.cloudant_short_notm}} データベースのセットアップ
{: #cloudantdb_cloud}

{{site.data.keyword.Bluemix_notm}} から {{site.data.keyword.openwhisk}} を使用する場合は、[{{site.data.keyword.openwhisk}} CLI プラグイン](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli)を使用して、サービスをアクションまたはパッケージにバインドできます。

まず、{{site.data.keyword.cloudant_short_notm}} アカウント用のパッケージ・バインディングを手動で作成する必要があります。

1. {{site.data.keyword.cloudant_short_notm}} アカウント用に構成されるパッケージ・バインディングを作成します。
  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. このパッケージ・バインディングが存在することを確認します。
  ```
  ibmcloud fn package list
  ```
  {: pre}

  出力例:
  ```
  packages
  /myNamespace/myCloudant private
  ```
  {: screen}
  
3. アクションまたはパッケージにバインドするサービス・インスタンスの名前を取得します。
    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    出力例:
    ```
    Name          Location   State    Type
    Cloudant-gm   us-south   active   service_instance
    ```
    {: screen}

4. 前のステップで取得したサービス・インスタンスに定義された資格情報の名前を取得します。
    ```
    ibmcloud resource service-keys --instance-name Cloudant-gm
    ```
    {: pre}

    出力例:
    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. ステップ 1 で作成したパッケージにサービスをバインドします。
    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1' 
    ```
    {: pre}

6. 資格情報が正常にバインドされたことを確認します。
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    出力例:
    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
        {
                "key": "bluemixServiceName",
                "value": "cloudantNoSQLDB"
            },
            {
                "key": "apihost",
                "value": "us-south.functions.cloud.ibm.com"
            },
            {
                "key": "__bx_creds",
            "value": {
                    "cloudantnosqldb": {
                        "apikey": "[Service apikey]",
                        "credentials": "Service credentials-1",
                        "iam_apikey_description": "[Service description]",
                        "iam_apikey_name": "[Service apikey name]",
                        "iam_role_crn": "[Service role crn]",
                        "iam_serviceid_crn": "[Service id crn]",
                        "instance": "Cloudant-gm",
                        "url": "[Service url]",
                        "username": "[Service username]"
                    }
                }
            }
        ],
    }
    ```
    {: screen}

    この例では、Cloudant サービスの資格情報が `__bx_creds` という名前のパラメーターに入っています。
  

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
