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

# Object Storage 套件
{: #cloud_object_storage_actions}

{{site.data.keyword.cos_full_notm}} 套件提供一組動作來與 {{site.data.keyword.cos_full}} 實例進行互動。這些動作可讓您讀取、寫入及刪除存在於 {{site.data.keyword.cos_short}} 實例上的儲存區。
{: shortdesc}

{{site.data.keyword.cos_short}} 套件包括下列動作：

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| `/cloud-object-storage` |套件| apikey、resource_instance_id、cos_hmac_keys.access_key_id、cos_hmac_keys.secret_access_key | 使用 {{site.data.keyword.cos_full_notm}} 實例。|
| `/cloud-object-storage/object-write` |動作| bucket、key、body | 將物件寫入儲存區中。|
| `/cloud-object-storage/object-read` |動作| bucket、key | 從儲存區中讀取物件。|
| `/cloud-object-storage/object-delete` |動作| bucket、key | 從儲存區中刪除物件。|
| `/cloud-object-storage/bucket-cors-put` |動作| bucket、corsConfig | 將 CORS 配置指派給儲存區。|
| `/cloud-object-storage/bucket-cors-get` |動作| bucket | 讀取儲存區的 CORS 配置。|
| `/cloud-object-storage/bucket-cors-delete` |動作| bucket | 刪除儲存區的 CORS 配置。|
| `/cloud-object-storage/client-get-signed-url` |動作| bucket、key、operation | 取得已簽署的 URL，以限制從儲存區中「寫入」、「讀取」及「刪除」物件。|

## 建立 {{site.data.keyword.cos_full_notm}} 服務實例
{: #cloud_object_storage_service_instance}

在安裝套件之前，您必須先要求 {{site.data.keyword.cos_short}} 實例，並至少建立一個儲存區。

1. [建立 {{site.data.keyword.cos_full_notm}} 服務實例 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/cloud-object-storage/basics/order-storage.html#creating-a-new-service-instance)。

2. 為 {{site.data.keyword.cos_short}} 服務實例[建立一組 HMAC 服務認證 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials)。在**新增線型配置參數（選用）**欄位中，新增 `{"HMAC":true}`。

3. [至少建立一個儲存區 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/cloud-object-storage/getting-started.html#create-buckets)。

## 安裝 {{site.data.keyword.cos_short}} 套件
{: #cloud_object_storage_installation}

在您具有 {{site.data.keyword.cos_short}} 服務實例之後，請使用 {{site.data.keyword.openwhisk}} CLI 或使用者介面，將 {{site.data.keyword.cos_short}} 套件安裝到您的名稱空間中。
{: shortdesc}

### 透過 {{site.data.keyword.openwhisk_short}} CLI 安裝
{: #cloud_object_storage_cli}

開始之前：
  1. [安裝 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 外掛程式](bluemix_cli.html#cloudfunctions_cli)。
  2. 安裝 `wskdeploy` 指令。請參閱 [Apache OpenWhisk 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/apache/incubator-openwhisk-wskdeploy#building-the-project)。

若要安裝 {{site.data.keyword.cos_short}} 套件，請執行下列動作：

1. 複製 {{site.data.keyword.cos_short}} 套件儲存庫。
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. 導覽至 `runtimes/nodejs` 或 `runtimes/python` 目錄。{{site.data.keyword.cos_short}} 套件中的動作會部署在您選擇的運行環境中。
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. 部署套件。如果您稍後決定在其他運行環境中執行這個套件中的動作，您可以重複前一個步驟和這個步驟，來重新部署套件。
    ```
    wskdeploy -m manifest.yaml
    ```
    {: pre}

4. 驗證 `cloud-object-storage` 套件已新增至套件清單。
    ```
    ibmcloud fn package list
    ```
    {: pre}

    輸出：
    ```
    packages
    /myOrg_mySpace/cloud-object-storage-pkg private
    ```
    {: screen}

5. 將認證從您建立的 {{site.data.keyword.cos_short}} 實例連結到該套件。
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage-pkg
    ```
    {: pre}

    輸出範例：
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage-pkg'.
    ```
    {: screen}

6. 驗證已使用 {{site.data.keyword.cos_short}} 服務實例認證配置該套件。
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/cloud-object-storage-pkg parameters
    ```
    {: pre}

    輸出範例：
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

### 透過 {{site.data.keyword.openwhisk_short}} 使用者介面安裝
{: #cloud_object_storage_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 主控台中，移至[建立頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/openwhisk/create)。

2. 使用 **Cloud Foundry 組織**和 **Cloud Foundry 空間**清單，選取您要在其中安裝 {{site.data.keyword.cos_short}} 套件的名稱空間。名稱空間是由合併的組織及空間名稱組成。

3. 按一下**安裝套件**。

4. 按一下 **IBM Cloud Object Storage** 套件群組，然後按一下 **IBM Cloud Object Storage** 套件。

5. 在「可用的運行環境」區段中，從下拉清單中選取 NodeJS 或 Python，然後按一下**安裝**。

6. 安裝好「套件」之後，您將被重新導向至「動作」頁面，您可以搜尋新的套件，其名稱為 **cloud-object-storage**。

7. 若要使用 **cloud-object-storage** 套件中的「動作」，您必須將服務認證連結至動作。
  * 若要將服務認證連結至套件中的所有動作，請遵循上述 CLI 指令中的步驟 5 及 6。
  * 若要將服務認證連結至個別動作，請在使用者介面中完成下列步驟。**附註**：您必須針對要使用的每一個動作，完成下列步驟。
    1. 按一下您要使用的 **cloud-object-storage** 套件中的「動作」。即會開啟該「動作」的詳細資料頁面。
    2. 在左側導覽中，按一下**參數**區段。
    3. 輸入新的**參數**。對於索引鍵，輸入 `__bx_creds`。針對該值，請貼上先前建立之服務實例中的服務認證 JSON 物件。

## 寫入 {{site.data.keyword.cos_short}} 儲存區
{: #cloud_object_storage_write}

您可以使用 `object-write` 動作，將物件寫入 {{site.data.keyword.cos_short}} 儲存區。
{: shortdesc}

**附註**：在下列步驟中，使用 `testbucket` 名稱作為範例。{{site.data.keyword.cos_full_notm}} 中的儲存區必須是全域唯一的，因此您必須將 `testbucket` 取代為唯一儲存區名稱。

### 透過 CLI 寫入儲存區
{: #write_bucket_cli}

使用 `object-write` 動作，將物件寫入儲存區。
```
ibmcloud fn action invoke /_/cloud-object-storage/object-write --blocking --result --param bucket testbucket --param key data.txt --param body "my_test_data"
```
{: pre}

輸出範例：
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

### 透過使用者介面寫入儲存區
{: #write_bucket_ui}

1. 移至 [{{site.data.keyword.openwhisk_short}} 主控台中的「動作」頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/openwhisk/actions)。

2. 在 `cloud-object-storage` 套件下，按一下 **object-write** 動作。

3. 在「程式碼」窗格中，按一下**變更輸入**。

4. 輸入包含儲存區、索引鍵和內文作為物件索引鍵的 JSON 物件。
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": "my_test_data"
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

## 從 {{site.data.keyword.cos_short}} 儲存區中讀取
{: #cloud_object_storage_read}

您可以使用 `object-read` 動作，從 {{site.data.keyword.cos_short}} 儲存區中的物件讀取。
{: shortdesc}

**附註**：在下列步驟中，使用 `testbucket` 名稱作為範例。{{site.data.keyword.cos_full_notm}} 中的儲存區必須是全域唯一的，因此您必須將 `testbucket` 取代為唯一儲存區名稱。

### 透過 CLI 從儲存區中讀取
{: #read_bucket_cli}

使用 `object-read` 動作，從儲存區中的物件讀取。
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket testbucket --param key data.txt
```
{: pre}

輸出範例：
```
{
  "body": "my_test_data",
  "bucket": "testbucket,
  "key": "data.txt"
}
```
{: screen}

### 透過使用者介面從儲存區中讀取
{: #read_bucket_ui}

1. 移至 [{{site.data.keyword.openwhisk_short}} 主控台中的「動作」頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/openwhisk/actions)。

2. 在 `cloud-object-storage` 套件下，按一下 **object-read** 動作。

3. 在「程式碼」窗格中，按一下**變更輸入**。

4. 輸入包含儲存區和索引鍵作為物件索引鍵的 JSON 物件。
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
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
