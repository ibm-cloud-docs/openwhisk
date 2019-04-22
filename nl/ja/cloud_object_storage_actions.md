---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: object storage, bucket, package

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Cloud Object Storage パッケージ
{: #cloud_object_storage_actions}

{{site.data.keyword.cos_full}} パッケージには、{{site.data.keyword.cos_full_notm}} インスタンスとの対話に必要なアクションのセットが用意されています。 これらのアクションを使用すれば、{{site.data.keyword.cos_short}} インスタンスに存在するバケットに対して、読み取り、書き込み、および削除を行うことができます。
{: shortdesc}

{{site.data.keyword.cos_short}} パッケージには、以下のアクションが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/cloud-object-storage` | パッケージ | API キー、リソース・インスタンス ID、「COS HMAC 鍵」項目の「アクセス鍵 ID」、「COS HMAC 鍵」項目の「秘密アクセス鍵」 | {{site.data.keyword.cos_short}} インスタンスと連携します。 |
| `/cloud-object-storage/object-write` | アクション | bucket、key、body、endpoint、ibmAuthEndpoint | オブジェクトをバケットに書き込みます。 |
| `/cloud-object-storage/object-read` | アクション | bucket、key、endpoint、ibmAuthEndpoint | バケットからオブジェクトを読み取ります。 |
| `/cloud-object-storage/object-delete` | アクション | bucket、key、endpoint、ibmAuthEndpoint | バケットからオブジェクトを削除します。 |
| `/cloud-object-storage/bucket-cors-put` | アクション | bucket、corsConfig、endpoint、ibmAuthEndpoint | CORS 構成をバケットに割り当てます。 |
| `/cloud-object-storage/bucket-cors-get` | アクション | bucket、endpoint、ibmAuthEndpoint | バケットの CORS 構成を読み取ります。 |
| `/cloud-object-storage/bucket-cors-delete` | アクション | bucket、endpoint、ibmAuthEndpoint | バケットの CORS 構成を削除します。 |
| `/cloud-object-storage/client-get-signed-url` | アクション | bucket、key、operation、expires、endpoint、ibmAuthEndpoint | バケットに対するオブジェクトの書き込み、読み取り、および削除を制限するため、署名済み URL を取得します。 |

## パッケージとアクションのパラメーター

#### パッケージのパラメーター

以下のパラメーターはこのパッケージにバインドされることになっています。そうすることにより、パラメーターがすべてのアクションで自動的に有効になります。いずれかのアクションを呼び出す時にこれらのパラメーターを指定することもできます。

**apikey**: `apikey` パラメーターは、{{site.data.keyword.cos_short}} インスタンスの IAM API キーです。

**resource_instance_id**: `resource_instance_id` パラメーターは、{{site.data.keyword.cos_short}} インスタンスの ID です。

**cos_hmac_keys**: `cos_hmac_keys` パラメーターは、{{site.data.keyword.cos_short}} インスタンスの HMAC 資格情報です。`access_key_id` と `secret_access_key` の値が含まれています。これらの資格情報は、`client-get-signed-url` アクションのみで使用されます。{{site.data.keyword.cos_short}} インスタンスの HMAC 資格情報を生成する方法については、[Using HMAC 資格情報の使用](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials)を参照してください。

#### アクションのパラメーター

以下のパラメーターは、個々のアクションを呼び出す時に指定します。すべてのアクションでこれらすべてのパラメーターがサポートされているわけではありません。どのアクションでどのパラメーターがサポートされているかについては、上の表を参照してください。

**bucket**: `bucket` パラメーターは、{{site.data.keyword.cloud_object_storage_short_notm}} バケットの名前です。

**endpoint**: `endpoint` パラメーターは、{{site.data.keyword.cos_short}} インスタンスに接続する時に使用する {{site.data.keyword.cos_short}} エンドポイントです。[{{site.data.keyword.cos_short}} の資料](/docs/services/cloud-object-storage?topic=cloud-object-storage-select_endpoints#select_endpoints)でエンドポイントを見つけることができます。

**expires**: `expires` パラメーターは、事前署名付きの URL 操作の有効期限が切れるまでの秒数です。`expires` のデフォルト値は 15 分です。

**ibmAuthEndpoint**: `ibmAuthEndpoint` パラメーターは、`apikey` からトークンを生成するために {site.data.keyword.cos_short}} が使用する IBM Cloud 許可エンドポイントです。デフォルトの許可エンドポイントはすべての IBM Cloud 地域に対応しています。

**key**: `key` パラメーターは、バケット・オブジェクトのキーです。

**operation**: `operation` パラメーターは、事前署名付きの URL の呼び出し操作です。

**corsConfig**: `corsConfig` パラメーターは、バケットの CORS 構成です。


## IBM Cloud Object Storage サービス・インスタンスの作成
{: #cloud_object_storage_service_instance}

パッケージをインストールするには、その前に {{site.data.keyword.cos_short}} のインスタンスを要求して、バケットを少なくとも 1 つ作成する必要があります。

1. [{{site.data.keyword.cos_short}} サービス・インスタンス ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を作成します](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-order-storage#creating-a-new-service-instance)。

2. {{site.data.keyword.cos_short}} サービス・インスタンス用に、[HMAC サービス資格情報セットを作成します  ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials)。 **「インラインの構成パラメーターの追加 (オプション)」**フィールドに `{"HMAC":true}` を追加します。

3. [バケットを少なくとも 1 つ作成します  ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-create-buckets)。

## {{site.data.keyword.cos_short}} パッケージのインストール
{: #cloud_object_storage_installation}

{{site.data.keyword.cos_short}} サービス・インスタンスを作成したら、{{site.data.keyword.openwhisk}} の CLI または UI を使用して {{site.data.keyword.cos_short}} パッケージを名前空間にインストールできます。
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI からのインストール
{: #cloud_object_storage_cli}

始める前に:

[{{site.data.keyword.Bluemix_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli)。

{{site.data.keyword.cos_short}} パッケージをインストールするには、以下のようにします。

1. {{site.data.keyword.cos_short}} パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. `runtimes/nodejs` ディレクトリー、または `runtimes/python` ディレクトリーにナビゲートします。 {{site.data.keyword.cos_short}} パッケージに含まれるアクションは、選択したランタイムにデプロイされます。
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. パッケージをデプロイします。 別のランタイムにパッケージを再デプロイする場合は、前のステップを繰り返します。
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. `cloud-object-storage` パッケージが自分のパッケージ・リストに追加されていることを確認します。
    ```
    ibmcloud fn package list
    ```
    {: pre}

    出力:
    ```
    packages
    /myOrg_mySpace/cloud-object-storage private
    ```
    {: screen}

5. 作成した {{site.data.keyword.cos_short}} インスタンスの資格情報をパッケージにバインドします。
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage
    ```
    {: pre}

    出力例:
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

6. パッケージが {{site.data.keyword.cos_short}} サービス・インスタンスの資格情報を使用して構成されていることを確認します。
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/cloud-object-storage parameters
    ```
    {: pre}

    出力例:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/cloud-object-storage, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "cloud-object-storage": {
            "apikey": "sdabac98wefuhw23erbsdufwdf7ugw",
            "credentials": "Credentials-1",
            "endpoints": "https://cos-service-s.us-south.containers.mybluemix.net/endpoints",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:cloud-object-storage:global:a/ddkgdaf89uawefoujhasdf:sd8238-sdfhwej33-234234-23423-213d::",
            "iam_apikey_name": "auto-generated-apikey-sduoiw98wefuhw23erbsdufwdf7ugw",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Reader",
            "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e::serviceid:ServiceId-sd8238-sdfhwej33-234234-23423-213d",
            "instance": "Cloud Object Storage-r1",
            "resource_instance_id": "crn:v1:staging:public:cloud-object-storage:global:a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e:sd8238-sdfhwej33-234234-23423-213d::"
          }
         }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI からのインストール
{: #cloud_object_storage_ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで、[「作成」ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/openwhisk/create) に移動します。

2. **「Cloud Foundry 組織」**リストと**「Cloud Foundry スペース」**リストを使用して、{{site.data.keyword.cos_short}} パッケージのインストール先の名前空間を選択します。名前空間は、`org` と `space` の名前を組み合わせて形成されます。

3. **「パッケージのインストール (Install Packages)」**をクリックします。

4. **「IBM Cloud オブジェクト・ストレージ」**パッケージ・グループをクリックし、次に**「IBM Cloud オブジェクト・ストレージ」** パッケージをクリックします。

5. **「使用可能なランタイム (Available Runtimes)」**セクションで、ドロップダウン・リストから `Node.JS` または `Python` を選択します。次に、**「インストール」**をクリックします。

6. パッケージがインストールされたら、アクション・ページにリダイレクトされ、この新しいパッケージを検索できます。このパッケージは **cloud-object-storage** という名前になっています。

7. **cloud-object-storage** パッケージでアクションを使用するには、サービス資格情報をアクションにバインドする必要があります。
  * サービス資格情報をパッケージ内のすべてのアクションにバインドするには、上にリストされている CLI の手順のステップ 5 と 6 に従います。
  * サービス資格情報を個々のアクションにバインドするには、UI で以下のステップを実行します。 **注**: 使用するアクションごとに以下のステップを実行する必要があります。
    1. 使用する **cloud-object-storage** パッケージのアクションをクリックします。 そのアクションの詳細ページが開きます。
    2. 左側のナビゲーションで、**「パラメーター」**セクションをクリックします。
    3. 新しい**パラメーター**を入力します。 key には `__bx_creds` を入力します。 value には、前に作成したサービス・インスタンスのサービス資格情報 JSON オブジェクトを貼り付けます。

## {{site.data.keyword.cos_short}} バケットへの書き込み
{: #cloud_object_storage_write}

`object-write` アクションを使用して、オブジェクトを {{site.data.keyword.cos_short}} バケットに書き込むことができます。
{: shortdesc}

**注**: 以下のステップでは、`testbucket` という名前が例として使用されます。 バケットは {{site.data.keyword.cos_full_notm}} 全体の中で固有でなければならないため、`testbucket` は固有のバケット名に置き換える必要があります。

### CLI によるバケットへの書き込み
{: #write_bucket_cli}

`object-write` アクションを使用して、オブジェクトをバケットに書き込みます。
```
ibmcloud fn action invoke /_/cloud-object-storage/object-write --blocking --result --param bucket testbucket --param key data.txt --param body "my_test_data"
```
{: pre}

出力例:
```
{
  "body": {
      "ETag": "\"32cef9b573122b1cf8fd9aec5fdb898c\""
  },
  "bucket": "testbucket",
  "key": "data.txt"
}
```
{: screen}

### UI によるバケットへの書き込み
{: #write_bucket_ui}

1. [{{site.data.keyword.openwhisk_short}} コンソールのアクション・ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/openwhisk/actions) に移動します。

2. `cloud-object-storage` パッケージの下で、**object-write** アクションをクリックします。

3. コード・ペインで、**「入力の変更 (Change Input)」**をクリックします。

4. bucket、key、および body をオブジェクト・キーとして含む JSON オブジェクトを入力します。
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": "my_test_data"
    }
    ```
    {: pre}

5. **「保存」**をクリックします。

6. **「起動」**をクリックします。

7. 出力が以下と同様であることを確認します。
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}

## {{site.data.keyword.cos_short}} バケットからの読み取り
{: #cloud_object_storage_read}

`object-read` アクションを使用して、{{site.data.keyword.cos_short}} バケット内のオブジェクトを読み取ることができます。
{: shortdesc}

**注**: 以下のステップでは、`testbucket` という名前が例として使用されます。 バケットは {{site.data.keyword.cos_full_notm}} 全体の中で固有でなければならないため、`testbucket` は固有のバケット名に置き換える必要があります。

### CLI によるバケットからの読み取り
{: #read_bucket_cli}

`object-read` アクションを使用して、バケット内のオブジェクトを読み取ります。
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket testbucket --param key data.txt
```
{: pre}

出力例:
```
{
  "body": "my_test_data",
  "bucket": "testbucket,
  "key": "data.txt"
}
```
{: screen}

### UI によるバケットからの読み取り
{: #read_bucket_ui}

1. [{{site.data.keyword.openwhisk_short}} コンソールのアクション・ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/openwhisk/actions) に移動します。

2. `cloud-object-storage` パッケージの下で、**object-read** アクションをクリックします。

3. コード・ペインで、**「入力の変更 (Change Input)」**をクリックします。

4. bucket と key をオブジェクト・キーとして含む JSON オブジェクトを入力します。
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
    }
    ```
    {: pre}

5. **「保存」**をクリックします。

6. **「起動」**をクリックします。

7. 出力が以下と同様であることを確認します。
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "testbucketeweit",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}
