---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# オブジェクト・ストレージ・パッケージ
{: #cloud_object_storage_actions}

{{site.data.keyword.cos_full_notm}} パッケージには、{{site.data.keyword.cos_full}} インスタンスとの対話に必要なアクションのセットが用意されています。これらのアクションを使用すれば、{{site.data.keyword.cos_short}} インスタンスに存在するバケットに対して、読み取り、書き込み、および削除を行うことができます。
{: shortdesc}

{{site.data.keyword.cos_short}} パッケージには、以下のアクションが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/cloud-object-storage` | パッケージ | API キー、リソース・インスタンス ID、「COS HMAC 鍵」項目の「アクセス鍵 ID」、「COS HMAC 鍵」項目の「秘密アクセス鍵」 | {{site.data.keyword.cos_full_notm}} インスタンスと連携します。|
| `/cloud-object-storage/object-write` | アクション | バケット、鍵、本体 | オブジェクトをバケットに書き込みます。|
| `/cloud-object-storage/object-read` | アクション | バケット、鍵 | バケットからオブジェクトを読み取ります。|
| `/cloud-object-storage/object-delete` | アクション | バケット、鍵 | バケットからオブジェクトを削除します。|
| `/cloud-object-storage/bucket-cors-put` | アクション | バケット、CORS 構成 | CORS 構成をバケットに割り当てます。|
| `/cloud-object-storage/bucket-cors-get` | アクション | バケット | バケットの CORS 構成を読み取ります。|
| `/cloud-object-storage/bucket-cors-delete` | アクション | バケット | バケットの CORS 構成を削除します。|
| `/cloud-object-storage/client-get-signed-url` | アクション | バケット、鍵、操作 | バケットに対するオブジェクトの書き込み、読み取り、および削除を制限するため、署名済み URL を取得します。|

## {{site.data.keyword.cos_full_notm}} サービス・インスタンスの作成
{: #cloud_object_storage_service_instance}

パッケージをインストールするには、その前に {{site.data.keyword.cos_short}} のインスタンスを要求して、バケットを少なくとも 1 つ作成する必要があります。

1. [{{site.data.keyword.cos_full_notm}} サービス・インスタンスを作成します ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/cloud-object-storage/basics/order-storage.html#creating-a-new-service-instance)。

2. {{site.data.keyword.cos_short}} サービス・インスタンス用に、[HMAC サービス資格情報セットを作成します  ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials)。**「インラインの構成パラメーターの追加 (オプション)」**フィールドに `{"HMAC":true}` を追加します。

3. [バケットを少なくとも 1 つ作成します  ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/cloud-object-storage/getting-started.html#create-buckets)。

## {{site.data.keyword.cos_short}} パッケージのインストール
{: #cloud_object_storage_installation}

{{site.data.keyword.cos_short}} サービス・インスタンスを作成したら、{{site.data.keyword.openwhisk}} CLI または UI を使用して、{{site.data.keyword.cos_short}} パッケージを自分の名前空間にインストールします。
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI からのインストール
{: #cloud_object_storage_cli}

始める前に:
  1. [{{site.data.keyword.Bluemix_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールしてください](bluemix_cli.html#cloudfunctions_cli)。
  2. `wskdeploy` コマンドをインストールしてください。[Apache OpenWhisk の文書 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")を参照してください](https://github.com/apache/incubator-openwhisk-wskdeploy#building-the-project)。

{{site.data.keyword.cos_short}} パッケージをインストールするには、以下のようにします。

1. {{site.data.keyword.cos_short}} パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. `runtimes/nodejs` ディレクトリー、または `runtimes/python` ディレクトリーにナビゲートします。{{site.data.keyword.cos_short}} パッケージに含まれるアクションは、選択したランタイムにデプロイされます。
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. パッケージをデプロイします。後で、このパッケージ内のアクションを他のランタイムで実行することにした場合は、直前のステップとこのステップを繰り返すと、パッケージを再デプロイできます。
    ```
    wskdeploy -m manifest.yaml
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
    /myOrg_mySpace/cloud-object-storage-pkg private
    ```
    {: screen}

5. 作成した {{site.data.keyword.cos_short}} インスタンスの資格情報をパッケージにバインドします。
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage-pkg
    ```
    {: pre}

    出力例:
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage-pkg'.
    ```
    {: screen}

6. パッケージが {{site.data.keyword.cos_short}} サービス・インスタンスの資格情報を使用して構成されていることを確認します。
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/cloud-object-storage-pkg parameters
    ```
    {: pre}

    出力例:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/cloud-object-storage-pkg, displaying field parameters
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

1. {{site.data.keyword.openwhisk_short}} コンソールで、[「作成」ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") に移動します](https://console.bluemix.net/openwhisk/create)。

2. **「Cloud Foundry 組織」**リストと**「Cloud Foundry スペース」**リストを使用して、{{site.data.keyword.cos_short}} パッケージのインストール先の名前空間を選択します。名前空間は、組織名とスペース名の組み合わせから形成されます。

3. **「パッケージのインストール (Install Packages)」**をクリックします。

4. **「IBM Cloud オブジェクト・ストレージ」**パッケージ・グループをクリックし、次に**「IBM Cloud オブジェクト・ストレージ」** パッケージをクリックします。

5. 「使用可能なランタイム (Available Runtimes)」セクションで、ドロップダウン・リストから NodeJS または Python を選択し、次に**「インストール」**をクリックします。

6. パッケージがインストールされたら、「アクション」ページにリダイレクトされ、この新しいパッケージを検索できます。このパッケージは **cloud-object-storage** という名前になっています。

7. **cloud-object-storage** パッケージに含まれるアクションを使用するには、サービス資格情報をアクションにバインドする必要があります。
  * サービス資格情報をパッケージ内のすべてのアクションにバインドするには、上にリストされている CLI の手順のステップ 5 と 6 に従います。
  * サービス資格情報を個々のアクションにバインドするには、UI で以下のステップを実行します。**注**: 使用するアクションごとに以下のステップを実行する必要があります。
    1. 使用したい **cloud-object-storage** パッケージのアクションをクリックします。そのアクションの詳細ページが開きます。
    2. 左側のナビゲーションで、**「パラメーター」**セクションをクリックします。
    3. 新しい**パラメーター**を入力します。key には `__bx_creds` を入力します。value には、前に作成したサービス・インスタンスのサービス資格情報 JSON オブジェクトを貼り付けます。

## {{site.data.keyword.cos_short}} バケットへの書き込み
{: #cloud_object_storage_write}

`object-write` アクションを使用して、オブジェクトを {{site.data.keyword.cos_short}} バケットに書き込むことができます。{: shortdesc}

**注**: 以下のステップでは、`testbucket` という名前が例として使用されます。バケットは {{site.data.keyword.cos_full_notm}} 全体の中で固有でなければならないため、`testbucket` は固有のバケット名に置き換える必要があります。

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

1. [{{site.data.keyword.openwhisk_short}} コンソールの「アクション」ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") に移動します](https://console.bluemix.net/openwhisk/actions)。

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

**注**: 以下のステップでは、`testbucket` という名前が例として使用されます。バケットは {{site.data.keyword.cos_full_notm}} 全体の中で固有でなければならないため、`testbucket` は固有のバケット名に置き換える必要があります。

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

1. [{{site.data.keyword.openwhisk_short}} コンソールの「アクション」ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") に移動します](https://console.bluemix.net/openwhisk/actions)。

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
