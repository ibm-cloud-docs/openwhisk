---

copyright:
  years: 2019, 2019
lastupdated: "2019-04-04"

keywords: object storage, bucket, event, action, trigger

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# (試験用) Object Storage イベント・ソース
{: #cloud_object_storage}

`/whisk.system/cos-experimental` パッケージは、不安定だったり、前のバージョンと非互換であるような変更が頻繁に行われたり、十分な通知期間を設けずに中断されたりすることがあります。このパッケージを実稼働環境で使用することは推奨されません。現在、この試験的パッケージは米国南部地域でのみ使用可能です。
{: important}

この例では、以下の方法について学習します。 
* {{site.data.keyword.cos_full}} インスタンスの[変更を listen する](#listening_to_cos_bucket_changes)。
* それらの変更に応答する[トリガーを作成する](#creating_a_trigger_cos)。
* 変更を取得して処理する[アクションを作成する](#creating_action_to_process_object)。
* アクションを変更トリガーに関連付ける[ルールを作成する](#associating_action_with_change_trigger)。
<br>

**サンプルのユース・ケース:** `/whisk.system/cos-experimental` パッケージを使用すると、{{site.data.keyword.cos_full_notm}} バケットに保管された GPS ストリート・データに対する変更を listen することができます。その後、変更が生じたときに GPS マップの自動再生成をトリガーして、ユーザーが GPS アプリケーション用の最新のストリート・データにアクセスできるようにすることができます。

## IBM Cloud Object Storage について
{: #cloud_object_storage_info}

**始める前に:** {{site.data.keyword.cos_full_notm}} について詳しくは、[Object Storage について](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-object-storage#about-object-storage)を参照してください。{{site.data.keyword.cos_full_notm}} インスタンスのセットアップについて詳しくは、[インスタンス {{site.data.keyword.cos_short}} のプロビジョン](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-for-developers#provision-an-instance-of-ibm-cloud-object-storage)を参照してください。

## IBM Cloud Object Storage バケットへの変更の listen
{: #listening_to_cos_bucket_changes}

{{site.data.keyword.openwhisk}} を使用して [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/api-reference?topic=cloud-object-storage-about-ibm-cloud-object-storage#about-ibm-cloud-object-storage) バケットへの変更を listen し、アクションを使用してバケットの 1 つ以上のオブジェクトを処理できます。 

`/whisk.system/cos-experimental` パッケージを使用すると、{{site.data.keyword.cos_full_notm}} インスタンスからのイベントを構成し、以下のフィードを含めることができます。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` | パッケージ | apikey、auth_endpoint、bucket、endpoint、interval | `changes` フィード・アクションを組み込んだパッケージ。|
| `/whisk.system/cos-experimental/changes` | フィード | apikey、auth_endpoint、bucket、endpoint、interval | {{site.data.keyword.cos_full_notm}} バケットへの変更に対してトリガー・イベントを起動します。|
{: shortdesc}

`changes` フィードを使用して、{{site.data.keyword.cos_full_notm}} インスタンス内でバケットへの変更があるたびにトリガーを起動するように、{{site.data.keyword.cos_full_notm}} イベント・ソース・サービスを構成できます。

この例で使用されるパラメーターは、以下のとおりです。

**apikey**: _(パッケージにバインドされていない場合に必須)_。`apikey` パラメーターは、{{site.data.keyword.cos_full_notm}} インスタンスの IAM API キーです。通常、この値はパッケージにバインドされています。ただし、`changes` フィード・アクションを使用する際に `apikey` 値が指定されている場合は、バインドされた資格情報の apikey ではなく、その指定された値が資格情報として使用されます。

**auth_endpoint**: _(オプション)_。`auth_endpoint` パラメーターは、`apikey` からトークンを生成するために {{site.data.keyword.cos_full_notm}} が使用する許可エンドポイントです。デフォルトのエンドポイントは {{site.data.keyword.Bluemix}} エンドポイントです。

**bucket**: _(必須)_。`bucket` パラメーターは、{{site.data.keyword.cos_full_notm}} バケットの名前です。

**endpoint**: _(必須)_。`endpoint` パラメーターは、{{site.data.keyword.cos_full_notm}} インスタンスに接続する時に使用する {{site.data.keyword.cos_full_notm}} エンドポイントです。[{{site.data.keyword.cos_full_notm}} の資料](/docs/services/cloud-object-storage?topic=cloud-object-storage-select_endpoints#select_endpoints)で、エンドポイントを見つけることができます。

**interval**: _(オプション)_。`interval` パラメーターは、分単位のバケット・ポーリング間隔です。`interval` 値は 1 分以上であることが必要で、デフォルトでは 1 分に設定されます。

## 変更フィードに応答するためのトリガーの作成
{: #creating_a_trigger_cos}

トリガーを作成するとき、資格情報を `cos-experimental` パッケージに直接バインドすることにより、{{site.data.keyword.cos_full_notm}} 資格情報を `changes` フィード・アクションに渡すことを回避できます。
 {: shortdesc}
 
 1. 最初に、資格情報を格納するように変更できるパッケージ・バインディングを作成します。以下の方法によって、パッケージ・バインディング `myCosPkg` が名前空間に作成されます。
  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}
 2. {{site.data.keyword.cos_short}} 資格情報をパッケージにバインドします。
 {{site.data.keyword.cos_short}} 資格情報をパッケージにバインドすることにより、`apikey` 値がパッケージにバインドされるので、`changes` フィード・アクションを呼び出す時に `apikey` 値を指定する必要がなくなります。 
  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}
 3. 作成したパッケージ・バインディング内の `changes` フィードを使用して、`myCosTrigger` という名前のトリガーを作成します。 バケット名と {{site.data.keyword.cos_short}} エンドポイント・パラメーターの値を使用します。
  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
  --param bucket myBucket
  --param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}
    出力例:
    ```
    ok: created trigger feed myCosTrigger
    ```
    {: screen}
  4. アクティベーションのポーリングを開始して、何が起こっているのかを明確に可視化します。
  ```
  ibmcloud fn activation poll
  ```
  {: pre}
 5. 変更フィードを監視するアクションを作成します。例えば、以下の JavaScript コードを含んだ `showCosChange` というアクションを作成します。
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
 6. `showCosChange` アクションを `myCosTrigger` トリガーに接続するルールを作成します。
  ```
  ibmcloud fn rule create myCosRule myCosTrigger showCosChange
  ```
  {: pre}
 7. {{site.data.keyword.cos_short}} ダッシュボードで、既存のバケット・オブジェクトを変更するか、新規に作成します。オブジェクトをバケットに追加する方法について詳しくは、[バケットへのオブジェクトの追加](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-add-objects)を参照してください。
 
 8. バケット・オブジェクトが変更されるごとに、`myCosTrigger` トリガーと `showCosChange` アクションの新しいアクティベーションがないかを監視します。これらのアクティベーションは、構成されたバケット・ポーリング間隔内に出現します。

新規アクティベーションを確認できない場合は、パラメーター `apikey`、`endpoint`、および `bucket` の値が正しいことを確認してください。
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}
{: tip}

### トリガー・イベントのデータ構造
{: #data_structure_trigger_event}

生成されるイベントの内容には以下のパラメーターが含まれます。

  - `file`: ファイルまたはオブジェクトのメタデータ。
  - `status`: 検出された変更。この値は、`added`、`modified`、または `deleted` のいずれかです。
  - `bucket`: {{site.data.keyword.cos_short}} バケットの名前。
  - `endpoint`: {{site.data.keyword.cos_short}} インスタンスに接続するために使用される {{site.data.keyword.cos_short}} エンドポイント。
  - `key`: 変更されたバケット・オブジェクトの ID。この値は `file.Key` と同じですが、トリガー・イベント JSON の上部にあります。

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

## 変更オブジェクトを処理するアクションの作成
{: #creating_action_to_process_object}

オブジェクトを取得して処理する 1 つのアクションを作成できます。または、1 つのシーケンスを作成し、その中の 1 つのアクションを使用してオブジェクトを取得し、別のアクションでオブジェクトを処理することもできます。

### オブジェクトを取得して処理するアクションの作成
{: #creating_action_to_retrieve_object}

このサンプルのアクション・コードは、バケット変更の通知文書を取得して処理します。アクションを手動で呼び出す際には `apikey` パラメーターと `serviceInstanceId` パラメーターをアクションに直接渡すことができますが、このアクションをトリガーで呼び出すときには、`ibmcloud fn service bind` コマンドでアクションにバインドされる必要がある {{site.data.keyword.cos_short}} からこれらの値を取得する必要があります。

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
```
{: codeblock}

このアクションでは `ibm-cos-sdk` npm パッケージが使用されるので、アクションは [Node.js モジュール](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_packaged_action)または[単一のバンドル](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_webpack_action)としてパッケージする必要があります。

このアクションを .zip ファイルにパッケージした後に、{{site.data.keyword.cos_short}} からオブジェクトを取得して処理するアクションを作成します。

```
ibmcloud fn action create myCosAction myCosAction.zip --kind nodejs:10
```
{: pre}

{{site.data.keyword.cos_short}} 資格情報をこのアクションに[バインド](#cos_binding_credentials_to_action)します。その後、トリガーの起動時にこのアクションを呼び出す[ルールを作成します](#associating_action_with_change_trigger)。

### オブジェクトを取得して処理するアクション・シーケンスの作成

アクションにオブジェクト取得コードを組み込む代わりに、[手動でインストールする](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions#cloud_object_storage_installation)必要がある {{site.data.keyword.cos_short}} パッケージの `object-read` アクションを使用することもできます。アクション・コードで処理する必要があるのは、`object-read` から返される結果だけです。

バケット・オブジェクトの処理のみを行うアクションのコード例:
```javascript
function main(data) {
  if (data) {
    // Process the object
  }
}
```
{: codeblock}

1. {{site.data.keyword.cos_short}} のオブジェクトのみを処理するアクションを作成します。
```
ibmcloud fn action create myCosProcessObjectAction myCosAction.js
```
{: pre}
2. {{site.data.keyword.cos_short}} 資格情報を `cos-experimental` パッケージ・バインディングにバインドします。
```
ibmcloud fn service bind cloud-object-storage myCloudObjectStoragePackage
```
{: pre}
3. `object-read` アクションを `myCosProcessObjectAction` と組み合わせて、アクション・シーケンスを作成できます。
```
ibmcloud fn action create myCosAction --sequence myCloudObjectStoragePackage/object-read,myCosProcessObjectAction
```
{: pre}

`object-read` アクションに加えて、インストール可能な {{site.data.keyword.cos_short}} パッケージに含まれている他のアクションも使用できます。

{{site.data.keyword.cos_short}} 資格情報をこのアクションに[バインド](#cos_binding_credentials_to_action)します。その後、トリガーの起動時にこのアクションを呼び出す[ルールを作成します](#associating_action_with_change_trigger)。

 ## アクションへの資格情報のバインド
 {: #cos_binding_credentials_to_action}
 
 以下のコマンドを使用して、{{site.data.keyword.cos_short}} 資格情報をアクションにバインドすることにより、呼び出しの際に機密性の高い資格情報が渡されることを回避できます。
 ```
 ibmcloud fn service bind cloud-object-storage myCosAction
 ```
 {: pre}

## アクションを変更トリガーに関連付けるルールの作成
{: #associating_action_with_change_trigger}

{: #openwhisk_catalog_cloud_object_storage_read_change notoc}

[ルール](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_rules_use)の中でアクションまたはアクション・シーケンスを使用して、{{site.data.keyword.cos_short}} 変更イベントに関連付けられたオブジェクトを取り出して処理することができます。

新規 {{site.data.keyword.cos_short}} トリガー・イベントで `MyCosAction` アクションをアクティブにするルールを作成します。
```
ibmcloud fn rule create myRule myCosTrigger myCosAction
```
{: pre}



