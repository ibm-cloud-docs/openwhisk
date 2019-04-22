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

# （實驗性）Object Storage 事件來源
{: #cloud_object_storage}

`/whisk.system/cos-experimental` 套件可能不穩定、因與舊版不相容而經常變更，並且可能會在短時間內停止提供。不建議在正式作業環境中使用此套件。此實驗性套件目前僅適用於美國南部地區。
{: important}

在此範例中，您將瞭解如何： 
* 針對 {{site.data.keyword.cos_full}} 實例，[接聽變更](#listening_to_cos_bucket_changes)。
* [建立觸發程式](#creating_a_trigger_cos)，以回應那些變更。
* [建立動作](#creating_action_to_process_object)，以擷取及處理變更。
* [建立規則](#associating_action_with_change_trigger)，以建立動作與變更觸發程式的關聯。
<br>

**範例使用案例：**使用 `/whisk.system/cos-experimental` 套件，您可以接聽 {{site.data.keyword.cos_full_notm}} 儲存區中儲存的 GPS 街道資料變更。然後，發生變更時，您可以觸發自動重新產生 GPS 地圖，讓使用者可以存取其 GPS 應用程式的最新街道資料。

## 關於 IBM Cloud Object Storage
{: #cloud_object_storage_info}

**開始之前：**若要瞭解 {{site.data.keyword.cos_full_notm}}，請參閱[關於 Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-object-storage#about-object-storage)。如需設定 {{site.data.keyword.cos_full_notm}} 實例的相關資訊，請參閱[佈建實例 {{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-for-developers#provision-an-instance-of-ibm-cloud-object-storage)。

## 接聽 IBM Cloud Object Storage 儲存區變更
{: #listening_to_cos_bucket_changes}

您可以使用 {{site.data.keyword.openwhisk}} 接聽 [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/api-reference?topic=cloud-object-storage-about-ibm-cloud-object-storage#about-ibm-cloud-object-storage) 儲存區變更，以及使用可處理儲存區中一個以上物件的動作。 

`/whisk.system/cos-experimental` 套件可讓您配置來自 {{site.data.keyword.cos_full_notm}} 實例的事件，並包括下列資訊來源：

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` |套件| apikey、auth_endpoint、bucket、endpoint、interval | 包含 `changes` 資訊來源動作的套件。|
| `/whisk.system/cos-experimental/changes` |資訊來源| apikey、auth_endpoint、bucket、endpoint、interval | 在變更 {{site.data.keyword.cos_full_notm}} 儲存區時發動觸發程式事件。|
{: shortdesc}

您可以使用 `changes` 資訊來源，配置 {{site.data.keyword.cos_full_notm}} 事件來源服務，以在每次變更 {{site.data.keyword.cos_full_notm}} 實例中的儲存區時發動觸發程式。

此範例中使用的參數：

**apikey**：_（必要，除非連結至套件）_。`apikey` 參數是 {{site.data.keyword.cos_full_notm}} 實例的 IAM API 金鑰。通常，此值會連結至套件。不過，如果在使用 `changes` 資訊來源動作時指定 `apikey` 值，則會將指定的值用於認證，而非所連結認證的 apikey。

**auth_endpoint**：_（選用）_。`auth_endpoint` 參數是 {{site.data.keyword.cos_full_notm}} 用來從 `apikey` 產生記號的授權端點。預設端點是 {{site.data.keyword.Bluemix}} 端點。

**bucket**：_（必要）_。`bucket` 參數是 {{site.data.keyword.cos_full_notm}} 儲存區的名稱。

**endpoint**：_（必要）_。`endpoint` 參數是用來連接至 {{site.data.keyword.cos_full_notm}} 實例的 {{site.data.keyword.cos_full_notm}} 端點。您可以在 [{{site.data.keyword.cos_full_notm}} 文件](/docs/services/cloud-object-storage?topic=cloud-object-storage-select_endpoints#select_endpoints)中尋找端點。

**interval**：_（選用）_。`interval` 參數是儲存區輪詢間隔（以完整分鐘為單位）。`interval` 值必須至少為 1 分鐘，且依預設設為 1 分鐘。

## 建立觸發程式以回應 changes 資訊來源
{: #creating_a_trigger_cos}

建立觸發程式時，您可以直接將認證連結至 `cos-experimental` 套件，以避免將 {{site.data.keyword.cos_full_notm}} 認證傳遞給 `changes` 資訊來源動作。
 {: shortdesc}
 
 1. 首先，建立可修改為包含您認證的套件連結。下列指令會在您的名稱空間中建立套件連結 `myCosPkg`。
  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}
 2. 將 {{site.data.keyword.cos_short}} 認證連結至套件。將 {{site.data.keyword.cos_short}} 認證連結至套件會將 `apikey` 值連結至套件，因此您不需要在呼叫 `changes` 資訊來源動作時指定 `apikey` 值。 
  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}
 3. 使用您所建立的套件連結中的 `changes` 資訊來源，來建立名為 `myCosTrigger` 的觸發程式。使用您的儲存區名稱及 {{site.data.keyword.cos_short}} 端點參數值。
  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
  --param bucket myBucket
  --param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}
    輸出範例：
    ```
    ok: created trigger feed myCosTrigger
    ```
    {: screen}
  4. 開始輪詢啟動，以清楚瞭解發生的情況。
  ```
  ibmcloud fn activation poll
  ```
  {: pre}
 5. 建立動作，以觀察 change 資訊來源。例如，稱為 `showCosChange` 且包含下列 JavaScript 程式碼的動作：
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
 6. 建立規則，以將 `showCosChange` 動作連接至 `myCosTrigger` 觸發程式：
  ```
  ibmcloud fn rule create myCosRule myCosTrigger showCosChange
  ```
  {: pre}
 7. 在 {{site.data.keyword.cos_short}} 儀表板中，修改現有儲存區物件，或建立儲存區物件。若要瞭解如何將物件新增至儲存區，請參閱[在儲存區中新增一些物件](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-add-objects)。
 
 8. 對於每個儲存區物件變更，觀察 `myCosTrigger` 觸發程式及 `showCosChange` 動作的新啟動。這些啟動會出現在配置的儲存區輪詢間隔內。

如果您觀察不到新啟動，請驗證 `apikey`、`endpoint` 及 `bucket` 參數值都是正確的。
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}
{: tip}

### 觸發程式事件的資料結構
{: #data_structure_trigger_event}

所產生事件的內容具有下列參數：

  - `file`：檔案或物件 meta 資料。
  - `status`：偵測到的變更。此值是 `added`、`modified` 或 `deleted`。
  - `bucket`：{{site.data.keyword.cos_short}} 儲存區的名稱。
  - `endpoint`：用來連接至 {{site.data.keyword.cos_short}} 實例的 {{site.data.keyword.cos_short}} 端點。
  - `key`：已變更儲存區物件的 ID。此值與 `file.Key` 相同，但可在觸發程式事件 JSON 上使用。

儲存區變更觸發程式事件的 JSON 表示法範例：
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

## 建立動作以處理變更過的物件
{: #creating_action_to_process_object}

您可以建立單一動作，以擷取及處理物件。或者，您可以建立一個序列，以使用一個動作來擷取物件，並建立另一個動作來處理物件。

### 建立動作以擷取及處理物件
{: #creating_action_to_retrieve_object}

此範例動作碼會擷取及處理儲存區變更通知文件。在手動動作呼叫期間，您可以將 `apikey` 及 `serviceInstanceId` 參數直接傳遞給動作，但是觸發程式呼叫此動作時，必須從 {{site.data.keyword.cos_short}} 取得這些值，其必須使用 `ibmcloud fn service bind` 指令連結至該動作。

程式碼範例：

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

因為此動作使用 `ibm-cos-sdk` npm 套件，所以必須將動作包裝為 [Node.js 模組](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_packaged_action)或[單一組合](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_webpack_action)。

將此動作包裝為 .zip 檔案之後，請建立動作以從 {{site.data.keyword.cos_short}} 擷取及處理物件：

```
ibmcloud fn action create myCosAction myCosAction.zip --kind nodejs:10
```
{: pre}

將 {{site.data.keyword.cos_short}} 認證[連結](#cos_binding_credentials_to_action)至此動作。然後，[建立規則](#associating_action_with_change_trigger)，以在觸發程式發動時呼叫此動作。

### 建立動作序列以擷取及處理物件

您可以使用必須[手動安裝](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions#cloud_object_storage_installation)之 {{site.data.keyword.cos_short}} 套件中的 `object-read` 動作，而非在動作中包括物件擷取程式碼。您的動作碼只需要處理從 `object-read` 傳回的結果。

僅處理儲存區物件的動作碼範例：
```javascript
  function main(data) {
    if (data) {
    // Process the object
  }
}
```
{: codeblock}

1. 建立動作，僅處理 {{site.data.keyword.cos_short}} 中的物件：
```
ibmcloud fn action create myCosProcessObjectAction myCosAction.js
```
{: pre}
2. 將 {{site.data.keyword.cos_short}} 認證連結至 `cos-experimental` 套件連結。
```
ibmcloud fn service bind cloud-object-storage myCloudObjectStoragePackage
```
{: pre}
3. 可以使用 `myCosProcessObjectAction` 來組合 `object-read` 動作，以建立動作序列。
```
ibmcloud fn action create myCosAction --sequence myCloudObjectStoragePackage/object-read,myCosProcessObjectAction
```
{: pre}

除了 `object-read` 動作之外，您還可以使用可安裝的 {{site.data.keyword.cos_short}} 套件中內含的其他動作。

將 {{site.data.keyword.cos_short}} 認證[連結](#cos_binding_credentials_to_action)至此動作。然後，[建立規則](#associating_action_with_change_trigger)，以在觸發程式發動時呼叫此動作。

 ## 將認證連結至動作
 {: #cos_binding_credentials_to_action}
 
 您可以使用下列指令將 {{site.data.keyword.cos_short}} 認證連結至動作，以避免在呼叫期間傳遞機密認證：
 ```
 ibmcloud fn service bind cloud-object-storage myCosAction
 ```
 {: pre}

## 建立規則以建立動作與變更觸發程式的關聯
{: #associating_action_with_change_trigger}

{: #openwhisk_catalog_cloud_object_storage_read_change notoc}

您可以在[規則](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_rules_use)中使用動作或動作序列，以提取及處理與 {{site.data.keyword.cos_short}} 變更事件相關聯的物件。

建立規則，以在新的 {{site.data.keyword.cos_short}} 觸發程式事件上啟動 `MyCosAction` 動作。
```
ibmcloud fn rule create myRule myCosTrigger myCosAction
```
{: pre}



