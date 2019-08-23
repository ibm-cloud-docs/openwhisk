---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: object storage, bucket, package, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Object Storage
{: #pkg_obstorage}

您可以透過與 {{site.data.keyword.cos_full}} 實例整合來延伸 {{site.data.keyword.openwhisk}} 應用程式的功能。

**開始之前**
若要瞭解有關 {{site.data.keyword.cos_full_notm}} 的資訊，請參閱[關於 Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api)。如需設定 {{site.data.keyword.cos_full_notm}} 實例的相關資訊，請參閱[佈建實例 {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#gs-dev-provision)。

## 套件
{: #obstorage_packages}
|套件|可用性|說明|
| --- | --- | --- |
| [{{site.data.keyword.cos_full_notm}} 套件](#pkg_obstorage_install)|可安裝|在 {{site.data.keyword.cos_full_notm}} 實例中執行讀取、寫入和刪除動作。|
|[（實驗性）{{site.data.keyword.cos_full_notm}} 事件來源](#pkg_obstorage_ev)|預先安裝（僅限美國南部）|接聽對 {{site.data.keyword.cos_full_notm}} 實例的變更。|

## 建立 IBM Cloud Object Storage 服務實例
{: #pkg_obstorage_service}

使用任一套件之前，必須要求 {{site.data.keyword.cos_full_notm}} 的實例，並至少建立一個儲存區。

1. [建立 {{site.data.keyword.cos_full_notm}} 服務實例](/docs/services/cloud-object-storage?topic=cloud-object-storage-gs-dev#gs-dev-provision)。

2. 為 {{site.data.keyword.cos_full_notm}} 服務實例[建立一組 HMAC 服務認證](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials)。在**新增線型配置參數（選用）**欄位中，新增 `{"HMAC":true}`。

3. [建立至少一個儲存區](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets)。

## 透過 {{site.data.keyword.openwhisk_short}} CLI 安裝 {{site.data.keyword.cos_full_notm}} 套件
{: #pkg_obstorage_install}

在具有 {{site.data.keyword.cos_full_notm}} 服務實例之後，您可以使用 {{site.data.keyword.openwhisk}} CLI 或使用者介面，將 {{site.data.keyword.cos_full_notm}} 套件安裝到您的名稱空間中。
{: shortdesc}
{: #pkg_obstorage_cli}

可安裝的 {{site.data.keyword.cos_full_notm}} 套件會部署一組動作，可用於處理 {{site.data.keyword.cos_full_notm}} 實例。這些操作在 Node.js 或 Python 中執行。安裝套件後，可以選取運行環境。如需 `cloud-object-storage` 套件中動作的清單，請參閱[可用實體](#pkg_obstorage_actions)。
{: note}

**開始之前**

[安裝 {{site.data.keyword.cloud_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 外掛程式](/docs/openwhisk?topic=cloud-functions-cli_install)。

若要安裝 {{site.data.keyword.cos_full_notm}} 套件，請執行下列動作：

1. 複製 {{site.data.keyword.cos_full_notm}} 套件儲存庫。
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. 導覽至 `runtimes/nodjs` 或 `runtimes/python` 目錄以選取運行環境用於套件中的動作。
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

4. 部署 `cloud-object-storage` 套件。
    ```
    ibmcloud fn deploy
    ```
    {: pre}

    **回應**
    ```
    Success: Deployment completed successfully.
    ```
    {: scree}

5. 驗證 `cloud-object-storage` 套件已新增至套件清單。
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **輸出**
    ```
    packages
    /<org_space>/cloud-object-storage         private
    ```
    {: screen}

6. 將認證從您建立的 {{site.data.keyword.cos_full_notm}} 實例連結到該套件。
    可以包含 `--keyname` 旗標來連結特定服務認證。如需連結服務的相關資訊，請參閱[服務指令](/docs/cloud-functions-cli-plugin?topic=cloud-functions-cli-plugin-functions-cli#cli_service)。

    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage --keyname `<service_key>`
    ```
    {: pre}

    **輸出範例**
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

7. 驗證已使用 {{site.data.keyword.cos_full_notm}} 服務實例認證配置該套件。
    ```
    ibmcloud fn package get /<org_space>/cloud-object-storage parameters
    ```
    {: pre}

    **輸出範例**
    ```
    ok: got package /<org_space>/cloud-object-storage, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "cloud-object-storage": {
            "apikey": "sdabac98wefuhw23erbsdufwdf7ugw",
            "credentials": "Credentials-1",
            "endpoints": "https://cos-service.bluemix.net/endpoints",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:cloud-object-storage:global:a/ddkgdaf89uawefoujhasdf:sd8238-sdfhwej33-234234-23423-213d::",
            "iam_apikey_name": "auto-generated-apikey-sduoiw98wefuhw23erbsdufwdf7ugw",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Reader",
            "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e::serviceid:ServiceId-sd8238-sdfhwej33-234234-23423-213d",
            "instance": "Cloud Object Storage-r1",
            "resource_instance_id": "crn:v1:staging:public:cloud-object-storage:global:a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e:sd8238-sdfhwej33-234234-23423-213d::"
          }
         }
      }
    ]
    ```
    {: screen}

## 連結參數

您可以使用 [`package update`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_pkg_bind) 指令將儲存區的端點連結到特定動作或 `cloud-object-storage` 套件。將 `<bucket_endpoint>` 取代為儲存區的端點。

當您更新套件、動作或觸發程式的參數時，您必須指定所有先前建立的參數。否則先前建立的參數會被移除。還會移除已連結至套件的任何服務，因此在更新其他參數後，必須再次[將服務連結](/docs/openwhisk?topic=cloud-functions-services)至套件。
{: important}

**將參數連結到套件中的所有動作**
```
ibmcloud fn package update cloud-object-storage --param endpoint <bucket_endpoint>
```
{: pre}

**將參數連結到特定動作**
```
ibmcloud fn action update cloud-object-storage/object-write --param endpoint <bucket_endpoint>
```
{: pre}

您還可以使用使用者介面中的**參數**標籤將參數連結到動作。若要在使用者介面中新增參數，請導覽至[**動作**頁面](https://cloud.ibm.com/openwhisk/actions){: external}，然後按一下其中一個動作。接著，按一下**參數** > **新增參數**。必須以 `<key>` 和 `<value>` 配對的格式新增參數。
{: tip}


## 透過 {{site.data.keyword.openwhisk_short}} 使用者介面安裝 {{site.data.keyword.cos_full_notm}} 套件
{: #pkg_obstorage_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 主控台中，移至[建立頁面 ](https://cloud.ibm.com/openwhisk/create){: external}。

2. 使用「名稱空間」下拉功能表，選取將 {{site.data.keyword.cos_full_notm}} 套件安裝到其中的名稱空間。

3. 按一下**安裝套件**。

4. 按一下 **IBM Cloud Object Storage** 套件群組，然後按一下 **IBM Cloud Object Storage** 套件。

5. 在**可用的運行環境**區段中，從下拉清單中選取 `Node.JS` 或 `Python`。然後，按一下**安裝**。

6. 安裝套件後，會將您重新導向到**動作**頁面，您可以在其中搜尋名稱為 `cloud-object-storage` 的新套件。

7. 若要使用 `cloud-object-storage` 套件中的動作，您必須將服務認證連結至動作。
  * 若要將服務認證連結到套件中的所有動作，請遵循 [CLI 指示](#pkg_obstorage_cli)中的步驟 5。
  * 若要將服務認證連結至個別動作，請在使用者介面中完成下列步驟。 
  
如果將服務認證連結到各個動作，則必須針對要使用的每個動作完成下列步驟。
{: note}

1. 按一下您要使用的 `cloud-object-storage` 套件中的動作。即會開啟該動作的詳細資料頁面。
2. 在左側導覽中，按一下**參數**。
3. 輸入新的參數。對於索引鍵，輸入 `__bx_creds`。針對該值，請貼上先前建立之服務實例中的服務認證 JSON 物件。


如需連結參數的相關資訊，請參閱[將參數連結到套件](/docs/openwhisk?topic=cloud-functions-actions#actions_pkgs_params)。


## 可用的實體
{: #pkg_obstorage_actions}

{{site.data.keyword.cos_full_notm}} 套件包括下列動作：

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| `/cloud-object-storage` |套件| `apikey`、`resource_instance_id`、`cos_hmac_keys.access_key_id`、`cos_hmac_keys.secret_access_key` | 使用 {{site.data.keyword.cos_full_notm}} 實例。|
| `/cloud-object-storage/object-write` |動作| `bucket`、`key`、`body`、`endpoint`、`ibmAuthEndpoint` | 將物件寫入儲存區中。|
| `/cloud-object-storage/object-read` |動作| `bucket`、`key`、`endpoint`、`ibmAuthEndpoint` | 從儲存區中讀取物件。|
| `/cloud-object-storage/object-delete` |動作| `bucket`、`key`、`endpoint`、`ibmAuthEndpoint` | 從儲存區中刪除物件。|
| `/cloud-object-storage/bucket-cors-put` |動作| `bucket`、`corsConfig`、`endpoint`、`ibmAuthEndpoint` | 將 CORS 配置指派給儲存區。|
| `/cloud-object-storage/bucket-cors-get` |動作| `bucket`、`endpoint`、`ibmAuthEndpoint` | 讀取儲存區的 CORS 配置。|
| `/cloud-object-storage/bucket-cors-delete` |動作| `bucket`、`endpoint`、`ibmAuthEndpoint` | 刪除儲存區的 CORS 配置。|
| `/cloud-object-storage/client-get-signed-url` |動作| `bucket`、`key`、`operation`、`expires`、`endpoint`、`ibmAuthEndpoint` | 取得已簽署的 URL，以限制從儲存區中「寫入」、「讀取」及「刪除」物件。|

若要取得可用實體的完整清單，請執行 `ibmcloud fn package get cloud-object-storage`。
{: note}

### 套件參數
{: #pkg_obstorage_pkgparams}

預期下列套件參數會連結至套件，且這些參數可自動用於所有動作。還可以在呼叫其中一個動作時指定這些參數。

|套件參數|說明|
| --- | --- |
| `apikey` |`apikey` 參數是 {{site.data.keyword.cos_full_notm}} 實例的 IAM API 金鑰。|
| `resource_instance_id` |`resource_instance_id` 參數是 {{site.data.keyword.cos_full_notm}} 實例 ID。|
| `cos_hmac_keys` |`cos_hmac_keys` 參數是 {{site.data.keyword.cos_full_notm}} 實例 HMAC 認證，其中包括 `access_key_id` 及 `secret_access_key` 值。這些認證專供 `client-get-signed-url` 動作使用。如需如何產生 {{site.data.keyword.cos_full_notm}} 實例的 HMAC 認證的指示，請參閱[使用 HMAC 認證](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials)。|
 
### 動作參數
{: #pkg_obstorage_actparams}

呼叫個別動作時可指定下列動作參數。每個動作不一定支援所有這些參數。請參閱[可用實體](#pkg_obstorage_actions)表格以瞭解哪個動作支援哪些參數。

|動作參數|說明|
| --- | --- |
| `bucket` |`bucket` 參數是 {{site.data.keyword.cos_full_notm}} 儲存區的名稱。|
| `endpoint` |`endpoint` 參數是用來連接至 {{site.data.keyword.cos_full_notm}} 實例的 {{site.data.keyword.cos_full_notm}} 端點。您可以在 [{{site.data.keyword.cos_full_notm}} 文件](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints)中尋找端點。|
| `expires` |`expires` 參數是預先簽署 URL 作業到期的秒數。預設 `expires` 值是 15 分鐘。|
| `ibmAuthEndpoint` |`ibmAuthEndpoint` 參數是 {site.data.keyword.cos_short}} 用來從 `apikey` 產生記號的 IBM Cloud 授權端點。預設授權端點對所有 IBM Cloud 地區都有效。|
| `key` |`key` 參數是儲存區物件索引鍵。|
| `operation` |`operation` 參數是要呼叫的預先簽署 URL 作業。|
| `corsConfig` |`corsConfig` 參數是儲存區的 CORS 配置。|


## 將物件寫入儲存區
{: #pkg_obstorage_write}

您可以使用 `object-write` 動作，將物件寫入 {{site.data.keyword.cos_full_notm}} 儲存區。
{: shortdesc}

在下列步驟中，使用 `test-bucket` 名稱作為範例。{{site.data.keyword.cos_full_notm}} 中的儲存區必須是廣域唯一的，因此您必須將 `test-bucket` 取代為唯一儲存區名稱。
{: note}

### 使用 CLI 將物件寫入儲存區
{: #pkg_obstorage_write_cli}
使用 `object-write` 動作，將物件寫入儲存區。
{: shortdesc}


呼叫 `object-write` 動作可將物件寫入儲存區。如果已將儲存區端點連結至套件或 `object-write` 動作，則不需要包括端點作為參數。將 `<org_space>` 取代為 Cloud Foundry 組織和空間的名稱，將 `<test-bucket>` 取代為儲存區的名稱，並將 `<test.txt>` 取代為要寫入的物件的名稱。

```
ibmcloud fn action invoke /<org_space>/cloud-object-storage/object-write --blocking --result --param bucket <test-bucket> --param key <test.txt> --param body <test> --param endpoint <bucket_endpoint>
```
{: pre}

**輸出範例**

```
{
  "body": {
      "ETag": "\"32cef9b573122b1cf8fd9aec5fdb898c\""
  },
  "bucket": "test-bucket",
  "key": "test.txt"
}
```
{: screen}

### 使用使用者介面將物件寫入儲存區
{: #pkg_obstorage_write_ui}


1. 移至 {{site.data.keyword.openwhisk_short}} 主控台中的[「動作」頁面](https://cloud.ibm.com/openwhisk/actions){: external}。

2. 在 `cloud-object-storage` 套件下，按一下 **object-write** 動作。

3. 在「程式碼」窗格中，按一下**變更輸入**。

4. 輸入包含儲存區、索引鍵和內文作為物件索引鍵的 JSON 物件。
    ```
    {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": "test"
    }
    ```
    {: pre}

5. 按一下**儲存**。

6. 按一下**呼叫**。

7. 驗證此輸出與下列內容類似：
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}

## 從儲存區讀取物件
{: #pkg_obstorage_read}

您可以使用 `object-read` 動作，從 {{site.data.keyword.cos_full_notm}} 儲存區中的物件讀取。
{: shortdesc}

在下列步驟中，使用 `test-bucket` 名稱作為範例。{{site.data.keyword.cos_full_notm}} 中的儲存區必須是廣域唯一的，因此您必須將 `test-bucket` 取代為唯一儲存區名稱。
{: note}

### 使用 CLI 從儲存區讀取物件
{: #pkg_obstorage_read_cli}

使用 `object-read` 動作，從儲存區中的物件讀取。
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket test-bucket --param key data.txt
```
{: pre}

**輸出範例**
```
{
  "body": "test",
  "bucket": "test-bucket,
  "key": "data.txt"
}
```
{: screen}

### 使用使用者介面從儲存區讀取物件
{: #pkg_obstorage_read_ui}

1. 移至[「動作」頁面](https://cloud.ibm.com/openwhisk/actions){: external}。

2. 在 `cloud-object-storage` 套件下，按一下 `object-read` 動作。

3. 在「程式碼」窗格中，按一下**變更輸入**。

4. 輸入包含儲存區和索引鍵作為物件索引鍵的 JSON 物件。
    ```
    {
      "bucket": "test-bucket",
      "key": "test.txt",
    }
    ```
    {: pre}

5. 按一下**儲存**。

6. 按一下**呼叫**。

7. 驗證此輸出與下列內容類似：
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}


## 使用（實驗性）Object Storage 事件來源接聽對儲存區的變更
{: #pkg_obstorage_ev}

`/whisk.system/cos-experimental` 套件可能不穩定、因與舊版不相容而經常變更，並且可能會在短時間內停止提供。不建議在正式作業環境中使用此套件。此實驗性套件僅在美國南部地區可用。
{: important}

您可以使用 {{site.data.keyword.openwhisk}} 接聽 [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations) 儲存區變更，以及使用可處理儲存區中一個以上物件的動作。

<br>

**傳回使用案例：**透過 `/whisk.system/cos-experimental` 套件，可以接聽對儲存在 {{site.data.keyword.cos_full_notm}} 儲存區中的 GPS 街道資料的變更。然後，在發生變更時，可以觸發自動重新產生 GPS 地圖，以便使用者可以使用其 GPS 應用程式存取最新的街道資料。

### （實驗性）Object Storage 事件來源參數
{: #pkg_obstorage_ev_ch}

透過 `/whisk.system/cos-experimental` 套件，您可以配置 {{site.data.keyword.cos_full_notm}} 實例中的事件，並包含下列資訊來源：

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` |套件| `apikey`、`auth_endpoint`、`bucket`、`endpoint`、`interval` | 包含 `changes` 資訊來源動作的套件。|
| `/whisk.system/cos-experimental/changes` |資訊來源| `apikey`、`auth_endpoint`、`bucket`、`endpoint`、`interval` |對 {{site.data.keyword.cos_full_notm}} 儲存區進行變更時發動觸發程式事件。|
{: shortdesc}

您可以使用 `changes` 資訊來源，配置 {{site.data.keyword.cos_full_notm}} 事件來源服務，以在每次變更 {{site.data.keyword.cos_full_notm}} 實例中的儲存區時發動觸發程式。

此範例中使用的參數：

| 參數 |說明|
| --- | --- |
| `apikey` |（必要，除非連結至套件）。`apikey` 參數是 {{site.data.keyword.cos_full_notm}} 實例的 IAM API 金鑰。通常，此值會連結至套件。不過，如果在使用 `changes` 資訊來源動作時指定 `apikey` 值，則會將指定的值用於認證，而非所連結認證的 apikey。|
|` auth_endpoint`|（選用）。`auth_endpoint` 參數是 {{site.data.keyword.cos_full_notm}} 用於透過 `apikey` 產生記號的授權端點。預設端點是 {{site.data.keyword.cloud}} 端點。|
| `bucket` |（必要）。`bucket` 參數是 {{site.data.keyword.cos_full_notm}} 儲存區的名稱。|
| `endpoint` |（必要）。`endpoint` 參數是用來連接至 {{site.data.keyword.cos_full_notm}} 實例的 {{site.data.keyword.cos_full_notm}} 端點。您可以在 [{{site.data.keyword.cos_full_notm}} 文件](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints)中尋找端點。|
| `interval` |（選用）。`interval` 參數是儲存區輪詢間隔（以完整分鐘為單位）。`interval` 值必須至少為 1 分鐘，且依預設設為 1 分鐘。|

## 建立觸發程式以回應 changes 資訊來源
{: #pkg_obstorage_ev_trig}

建立觸發程式時，您可以直接將認證連結至 `cos-experimental` 套件，以避免將 {{site.data.keyword.cos_full_notm}} 認證傳遞給 `changes` 資訊來源動作。
{: shortdesc}

 1. 首先，建立可修改為包含您認證的套件連結。下列指令會在您的名稱空間中建立套件連結 `myCosPkg`。

  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}

 2. 將 {{site.data.keyword.cos_full_notm}} 認證連結至套件。將 {{site.data.keyword.cos_full_notm}} 認證連結到套件會將 `apikey` 值連結到套件，以便在呼叫 `changes` 資訊來源動作時不需要指定 `apikey` 值。

  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}

 3. 使用建立的套件連結中的 `changes` 資訊來源來建立名稱為 `CosTrigger` 的觸發程式。使用您的儲存區名稱及 {{site.data.keyword.cos_full_notm}} 端點參數值。

  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
--param bucket myBucket \
--param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}

  **輸出範例**

    ```
    ok: created trigger feed cosTrigger
    ```
  {: pre}
 

建立動作，用於驗證觸發程式、變更資訊來源和規則是否都已正確配置並正常運作。
 
  1. 將下列 JavaScript 程式碼儲存為 `cosChange.js`。 

  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

  2. 使用 `cosChange.js` 程式碼建立名為 `cosChange` 的動作。

  ```
  ibmcloud fn action create cosChange <filepath>/cosChange.js
  ```
  {: pre}

  3. 建立規則，用於將 `cosChange` 動作連接至 `cosTrigger` 觸發程式。

  ```
  ibmcloud fn rule create cosRule cosTrigger cosChange
  ```
  {: pre}

  4. 在個別的視窗中，開始對啟動進行輪詢，以清楚地瞭解正在發生的情況。當發動觸發程式且執行作業時，此指令將列出其中每個作業執行時的啟動記錄。

  ```
  ibmcloud fn activation poll
  ```
  {: pre}
  
  5. 在 {{site.data.keyword.cos_full_notm}} 儀表板中，修改現有儲存區物件，或建立儲存區物件。若要瞭解如何將物件新增至儲存區，請參閱[在儲存區中新增一些物件](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-add-objects)。
  
  6. 對於每個儲存區物件變更，請觀察 `cosTrigger` 觸發程式和 `cosChange` 動作的新啟動。您可以透過在配置的儲存區輪詢間隔內執行 `ibmcloud fn activation poll` 指令來檢視這些啟動。
  
  7. 如果觀察不到新的啟動，請驗證 `apikey`、`endpoint` 和 `bucket` 參數值是否正確。
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}


### Object Storage 觸發程式啟動的資料結構
{: #pkg_obstorage_ev_data}

所產生事件的內容具有下列參數：

| 參數 |說明|
| --- | --- |
|`file`|檔案或物件 meta 資料。此結構在[列出特定儲存區中的物件](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations#compatibility-api-list-buckets)中進行了說明。|
|`status`|偵測到的變更。此值是 `added`、`modified` 或 `deleted`。|
|`bucket`|{{site.data.keyword.cos_full_notm}} 儲存區的名稱。|
| `endpoint` |用於連接至 {{site.data.keyword.cos_full_notm}} 實例的 {{site.data.keyword.cos_full_notm}} 端點。|
| `key` |變更的儲存區物件的 ID。此值與 `file.Key` 相同，但可在觸發程式事件 JSON 上使用。|

**儲存區變更觸發程式啟動的範例 JSON 表示**

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

## 建立用於處理已變更物件的動作
{: #pkg_obstorage_ev_act}

您可以建立單一動作，以擷取及處理物件。或者，您可以建立一個序列，以使用一個動作來擷取物件，並建立另一個動作來處理物件。

### 建立動作以擷取及處理物件
{: #pkg_obstorage_ev_act_ret}

此範例動作碼會擷取及處理儲存區變更通知文件。在手動動作呼叫期間，可以直接將 `apikey` 和 `serviceInstanceId` 參數傳遞到動作，但當觸發程式呼叫此動作時，這些值必須從 {{site.data.keyword.cos_full_notm}} 中取得，Object Storage 必須使用 `ibmcloud fn service bind` 指令連結至此動作。

1. 將下列程式碼儲存在名為 `myCosAction.zip` 的 .zip 檔案中。

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

  由於此動作使用 `ibm-cos-sdk` NPM 套件，因此該動作必須包裝為 [Node.js 模組](/docs/openwhisk?topic=cloud-functions-prep#prep_js_npm)或[單一軟體組](/docs/openwhisk?topic=cloud-functions-prep#prep_js_pkg)。

2. 建立用於擷取和處理 {{site.data.keyword.cos_full_notm}} 中物件的動作：

  ```
  ibmcloud fn action create myCosAction <filepath>/myCosAction.zip --kind nodejs:10
  ```
  {: pre}



### 建立動作序列以擷取及處理物件
{: #pkg_obstorage_ev_act_seq}

無需在動作中包含物件擷取程式碼，而是可以使用 `cloud-object-storage` 套件（必須[手動安裝](#pkg_obstorage_install)）中的 `object-read` 動作。動作碼僅需要處理從 `object-read` 傳回的結果。
{: shortdesc}

若要建立用於僅處理儲存區物件的動作，請執行以下動作：

1. 將下列程式碼儲存為 `myCosAction.js`。

  ```javascript
  function main(data) {
    if (data) {
    // Process the object
  }
}
  ```
  {: codeblock}

2. 建立用於僅處理 {{site.data.keyword.cos_full_notm}} 中物件的動作。

  ```
  ibmcloud fn action create myCosProcessObjectAction <filepath>/myCosAction.js
  ```
  {: pre}

3. 將 {{site.data.keyword.cos_full_notm}} 認證連結到手動安裝的 `cloud-object-storage` 套件。

  ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage
    ```
  {: pre}

4. 可以使用 `myCosProcessObjectAction` 來組合 `object-read` 動作，以建立動作序列。
  ```
  ibmcloud fn action create myCosAction --sequence cloud-object-storage/object-read,myCosProcessObjectAction
  ```
  {: pre}

除了 `object-read` 動作外，還可以使用可安裝的 `cloud-object-storage` 套件中包含的其他動作。
{: tip}

若要取得可用實體的清單，請執行下列指令。
```
ibmcloud fn package get cloud-object-storage
```
{: pre}

### 將認證連結至動作
{: #pkg_obstorage_ev_bind}

透過下列指令將 {{site.data.keyword.cos_full_notm}} 認證連結到動作，可以避免在呼叫期間傳遞機密認證：
  ```
 ibmcloud fn service bind cloud-object-storage myCosAction
 ```
  {: pre}

### 建立規則以建立動作與變更觸發程式的關聯
{: #pkg_obstorage_ev_rule}

可以使用[規則](/docs/openwhisk?topic=cloud-functions-rules)中的動作或動作序列來提取和處理與 {{site.data.keyword.cos_full_notm}} 變更事件關聯的物件。

建立規則，以在新的 {{site.data.keyword.cos_full_notm}} 觸發程式事件上啟動 `MyCosAction` 動作。
  ```
ibmcloud fn rule create myRule myCosTrigger myCosAction
```
  {: pre}



