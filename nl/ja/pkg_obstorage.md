---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: object storage, bucket, package

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}

# Object
Storage
{: #pkg_obstorage}

{{site.data.keyword.cos_full}} インスタンスと統合して、{{site.data.keyword.openwhisk}} アプリの機能を拡張できます。

**始める前に:** {{site.data.keyword.cos_full_notm}} について詳しくは、[About Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api) を参照してください。{{site.data.keyword.cos_full_notm}} インスタンスのセットアップについて詳しくは、[Provision an instance {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#gs-dev-provision) を参照してください。

## パッケージ
{: #obstorage_packages}
| パッケージ | 可用性 | 説明 |
| --- | --- | --- |
| [{{site.data.keyword.cos_full_notm}} パッケージ](#pkg_obstorage)| インストール可能 | {{site.data.keyword.cos_full_notm}} インスタンスの読み取り、書き込み、削除を行います。 |
| [(試験用) {{site.data.keyword.cos_full_notm}} イベント・ソース](#pkg_obstorage_ev) | 事前インストール済み (米国南部のみ) | {{site.data.keyword.cos_full_notm}} インスタンスの変更を listen します。 |

## IBM Cloud Object Storage サービス・インスタンスの作成
{: #pkg_obstorage_service}

いずれかのパッケージを使用するには、その前に {{site.data.keyword.cos_full_notm}} のインスタンスを要求して、バケットを少なくとも 1 つ作成する必要があります。

1. [{{site.data.keyword.cos_full_notm}} サービス・インスタンスを作成します ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/cloud-object-storage?topic=cloud-object-storage-gs-dev#gs-dev-provision)。

2. {{site.data.keyword.cos_full_notm}} サービス・インスタンス用に、[HMAC サービス資格情報セットを作成します ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials)。**「インラインの構成パラメーターの追加 (オプション)」**フィールドに `{"HMAC":true}` を追加します。

3. [バケットを少なくとも 1 つ作成します  ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets)。

## {{site.data.keyword.cos_full_notm}} パッケージによるバケットの読み取りと書き込み
{: #pkg_obstorage_install}

{{site.data.keyword.cos_full_notm}} サービス・インスタンスを作成したら、{{site.data.keyword.openwhisk}} の CLI または UI を使用して {{site.data.keyword.cos_full_notm}} パッケージを名前空間にインストールできます。
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI からのインストール
{: #pkg_obstorage_cli}

始める前に:

[{{site.data.keyword.Bluemix_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](/docs/openwhisk?topic=cloud-functions-cli_install)。

{{site.data.keyword.cos_full_notm}} パッケージをインストールするには、以下のようにします。

1. {{site.data.keyword.cos_full_notm}} パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. `runtimes/nodejs` ディレクトリー、または `runtimes/python` ディレクトリーにナビゲートします。 {{site.data.keyword.cos_full_notm}} パッケージに含まれるアクションは、選択したランタイムにデプロイされます。
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

5. 作成した {{site.data.keyword.cos_full_notm}} インスタンスの資格情報をパッケージにバインドします。
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage
    ```
    {: pre}

    出力例:
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

6. パッケージが {{site.data.keyword.cos_full_notm}} サービス・インスタンスの資格情報を使用して構成されていることを確認します。
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
{: #pkg_obstorage_ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで、[「作成」ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/openwhisk/create) に移動します。

2. **「Cloud Foundry 組織」**リストと**「Cloud Foundry スペース」**リストを使用して、{{site.data.keyword.cos_full_notm}} パッケージのインストール先の名前空間を選択します。 

3. **「パッケージのインストール (Install Packages)」**をクリックします。

4. **「IBM Cloud オブジェクト・ストレージ」**パッケージ・グループをクリックし、次に**「IBM Cloud オブジェクト・ストレージ」** パッケージをクリックします。

5. **「使用可能なランタイム (Available Runtimes)」**セクションで、ドロップダウン・リストから `Node.JS` または `Python` を選択します。 次に、**「インストール」**をクリックします。

6. パッケージがインストールされたら、アクション・ページにリダイレクトされ、この新しいパッケージを検索できます。このパッケージは **cloud-object-storage** という名前になっています。

7. **cloud-object-storage** パッケージでアクションを使用するには、サービス資格情報をアクションにバインドする必要があります。
  * サービス資格情報をパッケージ内のすべてのアクションにバインドするには、上にリストされている CLI の手順のステップ 5 と 6 に従います。
  * サービス資格情報を個々のアクションにバインドするには、UI で以下のステップを実行します。 **注**: 使用するアクションごとに以下のステップを実行する必要があります。
    1. 使用する **cloud-object-storage** パッケージのアクションをクリックします。 そのアクションの詳細ページが開きます。
    2. 左側のナビゲーションで、**「パラメーター」**セクションをクリックします。
    3. 新しい**パラメーター**を入力します。 key には `__bx_creds` を入力します。 value には、前に作成したサービス・インスタンスのサービス資格情報 JSON オブジェクトを貼り付けます。


### 使用可能なアクション
{: #pkg_obstorage_actions}

{{site.data.keyword.cos_full_notm}} パッケージには、以下のアクションが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/cloud-object-storage` | パッケージ | API キー、リソース・インスタンス ID、「COS HMAC 鍵」項目の「アクセス鍵 ID」、「COS HMAC 鍵」項目の「秘密アクセス鍵」 | {{site.data.keyword.cos_full_notm}} インスタンスと連携します。 |
| `/cloud-object-storage/object-write` | アクション | bucket、key、body、endpoint、ibmAuthEndpoint | オブジェクトをバケットに書き込みます。 |
| `/cloud-object-storage/object-read` | アクション | bucket、key、endpoint、ibmAuthEndpoint | バケットからオブジェクトを読み取ります。 |
| `/cloud-object-storage/object-delete` | アクション | bucket、key、endpoint、ibmAuthEndpoint | バケットからオブジェクトを削除します。 |
| `/cloud-object-storage/bucket-cors-put` | アクション | bucket、corsConfig、endpoint、ibmAuthEndpoint | CORS 構成をバケットに割り当てます。 |
| `/cloud-object-storage/bucket-cors-get` | アクション | bucket、endpoint、ibmAuthEndpoint | バケットの CORS 構成を読み取ります。 |
| `/cloud-object-storage/bucket-cors-delete` | アクション | bucket、endpoint、ibmAuthEndpoint | バケットの CORS 構成を削除します。 |
| `/cloud-object-storage/client-get-signed-url` | アクション | bucket、key、operation、expires、endpoint、ibmAuthEndpoint | バケットに対するオブジェクトの書き込み、読み取り、および削除を制限するため、署名済み URL を取得します。 |


### パッケージのパラメーター
{: #pkg_obstorage_pkgparams}

以下のパラメーターはこのパッケージにバインドされることになっています。そうすることにより、パラメーターがすべてのアクションで自動的に有効になります。 いずれかのアクションを呼び出す時にこれらのパラメーターを指定することもできます。

**apikey**: `apikey` パラメーターは、{{site.data.keyword.cos_full_notm}} インスタンスの IAM API キーです。

**resource_instance_id**: `resource_instance_id` パラメーターは、{{site.data.keyword.cos_full_notm}} インスタンス ID です。

**cos_hmac_keys**: `cos_hmac_keys` パラメーターは、{{site.data.keyword.cos_full_notm}} インスタンスの HMAC 資格情報です。`access_key_id` と `secret_access_key` の値が含まれています。  これらの資格情報は、`client-get-signed-url` アクションのみで使用されます。  {{site.data.keyword.cos_full_notm}} インスタンスの HMAC 資格情報を生成する方法については、[Using HMAC 資格情報の使用](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials)を参照してください。

#### アクションのパラメーター
{: #pkg_obstorage_actparams}

以下のパラメーターは、個々のアクションを呼び出す時に指定します。  すべてのアクションでこれらすべてのパラメーターがサポートされているわけではありません。どのアクションでどのパラメーターがサポートされているかについては、上の表を参照してください。

**bucket**: `bucket` パラメーターは、{{site.data.keyword.cos_full_notm}} バケットの名前です。

**endpoint**: `endpoint` パラメーターは、{{site.data.keyword.cos_full_notm}} インスタンスに接続する時に使用する {{site.data.keyword.cos_full_notm}} エンドポイントです。 [{{site.data.keyword.cos_full_notm}} の資料](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints)で、エンドポイントを見つけることができます。

**expires**: `expires` パラメーターは、事前署名付きの URL 操作の有効期限が切れるまでの秒数です。  `expires` のデフォルト値は 15 分です。

**ibmAuthEndpoint**: `ibmAuthEndpoint` パラメーターは、`apikey` からトークンを生成するために {site.data.keyword.cos_short}} が使用する IBM Cloud 許可エンドポイントです。 デフォルトの許可エンドポイントはすべての IBM Cloud 地域に対応しています。

**key**: `key` パラメーターは、バケット・オブジェクトのキーです。

**operation**: `operation` パラメーターは、事前署名付きの URL の呼び出し操作です。

**corsConfig**: `corsConfig` パラメーターは、バケットの CORS 構成です。


### {{site.data.keyword.cos_full_notm}} バケットへの書き込み
{: #pkg_obstorage_write}

`object-write` アクションを使用して、オブジェクトを {{site.data.keyword.cos_full_notm}} バケットに書き込むことができます。
{: shortdesc}

**注**: 以下のステップでは、`testbucket` という名前が例として使用されます。 バケットは {{site.data.keyword.cos_full_notm}} 全体の中で固有でなければならないため、`testbucket` は固有のバケット名に置き換える必要があります。

#### CLI によるバケットへの書き込み
{: #pkg_obstorage_write_cli}

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
{: #pkg_obstorage_write_ui}

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

### {{site.data.keyword.cos_full_notm}} バケットからの読み取り
{: #pkg_obstorage_read}

`object-read` アクションを使用して、{{site.data.keyword.cos_full_notm}} バケット内のオブジェクトを読み取ることができます。
{: shortdesc}

**注**: 以下のステップでは、`testbucket` という名前が例として使用されます。 バケットは {{site.data.keyword.cos_full_notm}} 全体の中で固有でなければならないため、`testbucket` は固有のバケット名に置き換える必要があります。

#### CLI によるバケットからの読み取り
{: #pkg_obstorage_read_cli}

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

#### UI によるバケットからの読み取り
{: #pkg_obstorage_read_ui}

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


## (試験用) Object Storage イベント・ソースによるバケットの変更の listen
{: #pkg_obstorage_ev}

`/whisk.system/cos-experimental` パッケージは、不安定だったり、前のバージョンと非互換であるような変更が頻繁に行われたり、十分な通知期間を設けずに中断されたりすることがあります。 このパッケージを実稼働環境で使用することは推奨されません。 現在、この試験的パッケージは米国南部地域でのみ使用可能です。
{: important}

{{site.data.keyword.openwhisk}} を使用して [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations) バケットへの変更を listen し、アクションを使用してバケットの 1 つ以上のオブジェクトを処理できます。

<br>

**サンプルのユース・ケース:** `/whisk.system/cos-experimental` パッケージを使用すると、{{site.data.keyword.cos_full_notm}} バケットに保管された GPS ストリート・データに対する変更を listen することができます。 その後、変更が生じたときに GPS マップの自動再生成をトリガーして、ユーザーが GPS アプリケーション用の最新のストリート・データにアクセスできるようにすることができます。

### (試験用) Object Storage イベント・ソースのパラメーター
{: #pkg_obstorage_ev_ch}

`/whisk.system/cos-experimental` パッケージを使用すると、{{site.data.keyword.cos_full_notm}} インスタンスからのイベントを構成し、以下のフィードを含めることができます。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` | パッケージ | apikey、auth_endpoint、bucket、endpoint、interval | `changes` フィード・アクションを組み込んだパッケージ。 |
| `/whisk.system/cos-experimental/changes` | フィード | apikey、auth_endpoint、bucket、endpoint、interval | {{site.data.keyword.cos_full_notm}} バケットへの変更に対してトリガー・イベントを起動します。 |
{: shortdesc}

`changes` フィードを使用して、{{site.data.keyword.cos_full_notm}} インスタンス内でバケットへの変更があるたびにトリガーを起動するように、{{site.data.keyword.cos_full_notm}} イベント・ソース・サービスを構成できます。

この例で使用されるパラメーターは、以下のとおりです。

**apikey**: _(パッケージにバインドされていない場合に必須)_。 `apikey` パラメーターは、{{site.data.keyword.cos_full_notm}} インスタンスの IAM API キーです。  通常、この値はパッケージにバインドされています。 ただし、`changes` フィード・アクションを使用する際に `apikey` 値が指定されている場合は、バインドされた資格情報の apikey ではなく、その指定された値が資格情報として使用されます。

**auth_endpoint**: _(オプション)_。 `auth_endpoint` パラメーターは、`apikey` からトークンを生成するために {{site.data.keyword.cos_full_notm}} が使用する許可エンドポイントです。  デフォルトのエンドポイントは {{site.data.keyword.Bluemix}} エンドポイントです。

**bucket**: _(必須)_。 `bucket` パラメーターは、{{site.data.keyword.cos_full_notm}} バケットの名前です。

**endpoint**: _(必須)_。 `endpoint` パラメーターは、{{site.data.keyword.cos_full_notm}} インスタンスに接続する時に使用する {{site.data.keyword.cos_full_notm}} エンドポイントです。 [{{site.data.keyword.cos_full_notm}} の資料](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints)で、エンドポイントを見つけることができます。

**interval**: _(オプション)_。 `interval` パラメーターは、分単位のバケット・ポーリング間隔です。 `interval` 値は 1 分以上であることが必要で、デフォルトでは 1 分に設定されます。

### 変更フィードに応答するためのトリガーの作成
{: #pkg_obstorage_ev_trig}

トリガーを作成するとき、資格情報を `cos-experimental` パッケージに直接バインドすることにより、{{site.data.keyword.cos_full_notm}} 資格情報を `changes` フィード・アクションに渡すことを回避できます。
 {: shortdesc}

 1. 最初に、資格情報を格納するように変更できるパッケージ・バインディングを作成します。 以下の方法によって、パッケージ・バインディング `myCosPkg` が名前空間に作成されます。
  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}
 2. {{site.data.keyword.cos_full_notm}} 資格情報をパッケージにバインドします。
 {{site.data.keyword.cos_full_notm}} 資格情報をパッケージにバインドすることにより、`apikey` 値がパッケージにバインドされるので、`changes` フィード・アクションを呼び出す時に `apikey` 値を指定する必要がなくなります。
  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}
 3. 作成したパッケージ・バインディング内の `changes` フィードを使用して、`myCosTrigger` という名前のトリガーを作成します。 バケット名と {{site.data.keyword.cos_full_notm}} エンドポイント・パラメーターの値を使用します。
  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
--param bucket myBucket \
--param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}

    出力例:
    ```
    ok: created trigger feed myCosTrigger
    ```
  {: pre}
 4. トリガー、変更フィード、ルールがすべて正しく構成され、正常に機能することを検証するだけの単純なアクションを作成します。例えば、以下の `showCosChange.js` JavaScript コードを含む `showCosChange` というアクションを作成します。
  ```javascript
  function main(data) {
      console.log(data);
  }
  ```
  {: codeblock}
  ```
  ibmcloud fn action create showCosChange showCosChange.js
  ```
  {: pre}
サンプル・コードの表示
 5. `showCosChange` アクションを `myCosTrigger` トリガーに接続するルールを作成します。
  ```
  ibmcloud fn rule create myCosRule myCosTrigger showCosChange
  ```
  {: pre}
 6. 別ウィンドウで、状況を明確に可視化するためにアクティベーションのポーリングを開始します。以下のコマンドにより、トリガーが起動してアクションが実行されたときに、実行された各操作のアクティベーション・レコードがリスト表示されます。
  ```
  ibmcloud fn activation poll
  ```
  {: pre}
 7. {{site.data.keyword.cos_full_notm}} ダッシュボードで、既存のバケット・オブジェクトを変更するか、新規に作成します。 オブジェクトをバケットに追加する方法について詳しくは、[バケットへのオブジェクトの追加](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-add-objects)を参照してください。

 8. バケット・オブジェクトが変更されるごとに、`myCosTrigger` トリガーと `showCosChange` アクションの新しいアクティベーションがないかを監視します。 これらのアクティベーションは、`ibmcloud fn activation poll` コマンドを実行しているウィンドウに、構成したバケットのポーリング間隔で表示されます。

新規アクティベーションを確認できない場合は、パラメーター `apikey`、`endpoint`、および `bucket` の値が正しいことを確認してください。
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}
{: tip}

### Object Storage トリガー・イベントのデータ構造
{: #pkg_obstorage_ev_data}

生成されるイベントの内容には以下のパラメーターが含まれます。

  - `file`: ファイルまたはオブジェクトのメタデータ。 この構造については、[特定のバケット内のオブジェクトのリスト](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations#compatibility-api-list-buckets)で説明しています。
  - `status`: 検出された変更。  この値は、`added`、`modified`、または `deleted` のいずれかです。
  - `bucket`: {{site.data.keyword.cos_full_notm}} バケットの名前。
  - `endpoint`: {{site.data.keyword.cos_full_notm}} インスタンスに接続するために使用される {{site.data.keyword.cos_full_notm}} エンドポイント。
  - `key`: 変更されたバケット・オブジェクトの ID。 この値は `file.Key` と同じですが、トリガー・イベント JSON の上部にあります。

バケット変更トリガー・イベントの JSON 表記の例:
```json
{
  "file": {
    "ETag": "\"fb47672a6f7c34339ca9f3ed55c6e3a9\"",
    "Key": "file-86.txt",
    "LastModified": "2018-12-19T08:33:27.388Z",
    "Owner": {
      "DisplayName": "80a2054e-8d16-4a47-a46d-4edf5b516ef6",
      "ID": "80a2054e-8d16-4a47-a46d-4edf5b516ef6"
    },
    "Size": 25,
    "StorageClass": "STANDARD"
  },
  "status": "added",
  "bucket": "myBucket",
  "endpoint": "s3.us-south.cloud-object-storage.appdomain.cloud",
  "key": "file-86.txt"
}
```
{: codeblock}

### 変更オブジェクトを処理するアクションの作成
{: #pkg_obstorage_ev_act}

オブジェクトを取得して処理する 1 つのアクションを作成できます。 または、1 つのシーケンスを作成し、その中の 1 つのアクションを使用してオブジェクトを取得し、別のアクションでオブジェクトを処理することもできます。

### オブジェクトを取得して処理するアクションの作成
{: #pkg_obstorage_ev_act_ret}

このサンプルのアクション・コードは、バケット変更の通知文書を取得して処理します。 アクションを手動で呼び出す際には `apikey` パラメーターと `serviceInstanceId` パラメーターをアクションに直接渡すことができますが、このアクションをトリガーで呼び出すときには、`ibmcloud fn service bind` コマンドでアクションにバインドされる必要がある {{site.data.keyword.cos_full_notm}} からこれらの値を取得する必要があります。

サンプル・コード:

```javascript
const COS = require('ibm-cos-sdk')

function main(params){
  const apikey = params.apikey || params.__bx_creds['cloud-object-storage'].apikey
  const serviceInstanceId = params.serviceInstanceId || params.__bx_creds['cloud-object-storage'].resource_instance_id
  const ibmAuthEndpoint = params.ibmAuthEndpoint
  const endpoint = params.endpoint
  const bucket = params.bucket
  const file = params.key

  const cos_config = { endpoint: endpoint, apiKeyId: apikey, ibmAuthEndpoint: ibmAuthEndpoint, serviceInstanceId: serviceInstanceId }
  const client = new COS.S3(cos_config);

  return new Promise(function(resolve, reject) {
    client.getObject({ Bucket: bucket, Key: file }, (err, results) => {
      if (err != null) {
        console.log(err)
        reject({ err: err })
      } else {
        console.log(results)
        resolve({ contents: Buffer.from(results.Body).toString() })
      }
    })
  });
}
exports.main = main;
```
{: codeblock}

このアクションでは `ibm-cos-sdk` npm パッケージが使用されるので、アクションは [Node.js モジュール](/docs/openwhisk?topic=cloud-functions-prep#prep_js_npm)または[単一のバンドル](/docs/openwhisk?topic=cloud-functions-prep#prep_js_pkg)としてパッケージする必要があります。

このアクションを .zip ファイル `myCosAction.zip` としてパッケージ化した後に、{{site.data.keyword.cos_full_notm}} からオブジェクトを取得して処理するアクションを作成します。

```
ibmcloud fn action create myCosAction myCosAction.zip --kind nodejs:10
```
{: pre}

### オブジェクトを取得して処理するアクション・シーケンスの作成
{: #pkg_obstorage_ev_act_seq}

アクションにオブジェクト取得コードを組み込む代わりに、[手動でインストールする](#pkg_obstorage_install)必要がある `cloud-object-storage` パッケージの `object-read` アクションを使用することもできます。  アクション・コードで処理する必要があるのは、`object-read` から返される結果だけです。

バケット・オブジェクトの処理のみを行うアクションの `myCosAction.js` コード例:
```javascript
function main(data) {
  if (data) {
    // Process the object
  }
}
```
{: codeblock}

1. {{site.data.keyword.cos_full_notm}} のオブジェクトのみを処理するアクションを作成します。
  ```
  ibmcloud fn action create myCosProcessObjectAction myCosAction.js
  ```
  {: pre}
2. {{site.data.keyword.cos_full_notm}} 資格情報を、手動でインストールした `cloud-object-storage` パッケージにバインドします。
  ```
  ibmcloud fn service bind cloud-object-storage cloud-object-storage
  ```
  {: pre}
3. `object-read` アクションを `myCosProcessObjectAction` と組み合わせて、アクション・シーケンスを作成できます。
  ```
  ibmcloud fn action create myCosAction --sequence cloud-object-storage/object-read,myCosProcessObjectAction
  ```
  {: pre}

`object-read` アクションに加えて、インストール可能な `cloud-object-storage` パッケージに含まれている他のアクションも使用できます。

### アクションへの資格情報のバインド
{: #pkg_obstorage_ev_bind}

以下のコマンドを使用して、{{site.data.keyword.cos_full_notm}} 資格情報をアクションにバインドすることにより、呼び出しの際に機密性の高い資格情報が渡されることを回避できます。
```
ibmcloud fn service bind cloud-object-storage myCosAction
```
{: pre}

### アクションを変更トリガーに関連付けるルールの作成
{: #pkg_obstorage_ev_rule}

{: #openwhisk_catalog_cloud_object_storage_read_change notoc}

[ルール](/docs/openwhisk?topic=cloud-functions-rules)の中でアクションまたはアクション・シーケンスを使用して、{{site.data.keyword.cos_full_notm}} 変更イベントに関連付けられたオブジェクトを取り出して処理することができます。

新規 {{site.data.keyword.cos_full_notm}} トリガー・イベントで `MyCosAction` アクションをアクティブにするルールを作成します。
```
ibmcloud fn rule create myRule myCosTrigger myCosAction
```
{: pre}

