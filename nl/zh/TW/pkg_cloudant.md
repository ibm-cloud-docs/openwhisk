---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: cloudant, event, action, trigger, sequence, functions

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
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Cloudant
{: #pkg_cloudant}

透過預先安裝的 `/whisk.system/cloudant` 套件，您可以使用 [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) 資料庫。使用此套件不需要任何服務連結。
{: shortdesc}


## 可用的實體
{: #cloudant_available}
下表顯示了 `whisk.system/cloudant` 套件中的一些可用實體。您可以使用 `whisk.system/cloudant` 套件來讀取、寫入、更新或刪除文件。還可以使用 `changes` 資訊來源來接聽對 {{site.data.keyword.cloudant_short_notm}} 資料庫的變更。
{: shortdesc}

如需 `/whisk.system/cloudant` 套件中可用實體的完整清單，請執行 `ibmcloud fn package get /whisk.system/cloudant`。
{: note}

|實體|類型|參數|說明|
| --- | --- | --- | --- |
|`/whisk.system/cloudant` |套件|`dbname`、`host`、`username`、`password`|使用 Cloudant 資料庫。|
|`/whisk.system/cloudant/read` |動作|`dbname`、`id`|讀取資料庫中的文件。|
|`/whisk.system/cloudant/write` |動作|`dbname`、`overwrite`、`doc`|將文件寫入資料庫。|
|`/whisk.system/cloudant/update-document`|動作|`dbname`、`doc`|更新資料庫中的文件。|
|`/whisk.system/cloudant/changes` |資訊來源|`dbname`、`iamApiKey`、`iamUrl`、`filter`、`query_params`、`maxTriggers`|在資料庫變更時發動觸發程式事件。|

不再支援 `includeDoc` 參數用於 `/whisk.system/cloudant/changes` 資訊來源。如果已建立使用此參數的觸發程式，則必須在不使用 `includeDoc` 參數的情況下重新建立這些觸發程式。
{: deprecated}

## 將 `/whisk.system/cloudant` 套件連結到 {{site.data.keyword.cloudant_short_notm}} 資料庫。
如果使用的是 {{site.data.keyword.cloud_notm}} 的 {{site.data.keyword.openwhisk}}，則可以使用 {{site.data.keyword.openwhisk}} CLI 外掛程式將服務連結到動作或套件。
{: #cloudant_db}

**開始之前**
您必須具有 {{site.data.keyword.cloudant_short_notm}} 實例。若要建立實例，請參閱[開始使用 {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)。

1. 建立為 {{site.data.keyword.cloudant_short_notm}} 帳戶配置的 `/whisk.system/cloudant` 套件連結。在此範例中，套件名稱為 `myCloudant`。

  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. 驗證套件連結已存在。

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **輸出範例**

  ```
  packages
  /<namespace>/myCloudant private
  ```
  {: screen}

3. 取得您要連結至動作或套件之服務實例的名稱。

    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    **輸出範例**
    ```
    Name          Location   State    Type
    Cloudant-gm   us-south   active   service_instance
    ```
    {: screen}

4. 取得針對您在前一個步驟中所取得之服務實例而定義的認證名稱。

    ```
    ibmcloud resource service-keys --instance-name Cloudant-gm
    ```
    {: pre}

    **輸出範例**

    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. 將服務連結到步驟 1 中建立的套件。

    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1' 
    ```
    {: pre}

6. 驗證認證已順利連結。
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    **輸出範例**

    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
            {
                "key": "serviceName",
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

在此範例中，{{site.data.keyword.cloudant_short_notm}} 服務的認證屬於名稱為 `__bx_creds` 的參數。

## 使用 {{site.data.keyword.cloudant_short_notm}} 資料庫中的文件
{: #cloudant_read}

可以使用動作來讀取、寫入、更新和刪除 {{site.data.keyword.cloudant_short_notm}} 資料庫中的文件。
{: shortdesc}

### 讀取文件
可以使用 `/whisk.system/cloudant/read` 動作讀取 {{site.data.keyword.cloudant_short_notm}} 資料庫中的文件。

**開始之前**
如果 {{site.data.keyword.cloudant_short_notm}} 資料庫中沒有文件，則可以使用 {{site.data.keyword.cloudant_short_notm}} 儀表板來建立文件。儀表板的 URL 為 `https://<mycloudantaccount>.cloudant.com/dashboard.html#database/<database_name>/_all_docs?limit=100`。

使用 `read` 動作提取文件。將 `/_/myCloudant` 取代為套件名稱，將 `<database_name>` 取代為資料庫名稱，並將 `<document_id>` 取代為檔案 ID。呼叫動作以測試提取文件。

**指令語法**

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id <document_id>
  ```
  {: pre}

**對 `test` 資料庫執行的範例讀取動作**
呼叫動作以測試讀取檔案。此範例讀取 `id` 為 `9f86f4955e7a38ab0169462e6ac0f476` 的檔案，這是一個空檔案。

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id 9f86f4955e7a38ab0169462e6ac0f476
  ```
  {:pre}

**輸出範例**
  ```
  {
      "_id": "9f86f4955e7a38ab0169462e6ac0f476",
      "_rev": "1-967a00dff5e02add41819138abb3284d"
  }
  ```
  {: screen}

### 將文件寫入 {{site.data.keyword.cloudant_short_notm}} 資料庫
{: #cloudant_write}

可以使用動作在 {{site.data.keyword.cloudant_short_notm}} 資料庫中建立或更新文件。
{: shortdesc}

**開始之前**

  建立針對 {{site.data.keyword.cloudant_short_notm}} 帳戶所配置的 `/whisk.system/cloudant` [套件連結](#cloudant_db)。

1. 使用您建立的套件連結中的 `write` 動作，來儲存文件。將 `/_/myCloudant` 取代為套件名稱，將 `<database_name>` 取代為資料庫名稱，將 `<document_id>` 取代為文件 ID，並將 `<test_name>` 取代為名稱。

  **指令語法**
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param <database_name> test --param doc "{\"_id\":\"<document_id>\",\"name\":\"<test_name>\"}"
  ```
  {: pre}

  **對 `test` 資料庫執行的範例撰寫動作**

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\"}"
  ```
  {: pre}

  **輸出範例**

  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "color",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. 驗證文件在 {{site.data.keyword.cloudant_short_notm}} 儀表板中是否存在。`test` 資料庫的儀表板 URL 的格式如下：`https://<mycloudantaccount>.cloudant.com/dashboard.html#database/test/_all_docs?limit=100`。

  **{{site.data.keyword.cloudant_short_notm}} 儀表板中的範例文件**
  ```
  {
  "_id": "color",
  "_rev": "1-f413f4b74a724e391fa5dd2e9c8e9d3f",
  "name": "blue"
  }
  ```
  {: screen}

### 更新文件
可以使用 `/update-document` 動作更新 {{site.data.keyword.cloudant_short_notm}} 資料庫中的文件。
{: short desc}

**開始之前**

  建立針對 {{site.data.keyword.cloudant_short_notm}} 帳戶所配置的 `/whisk.system/cloudant` [套件連結](#cloudant_db)。

下列範例更新在[將文件寫入 {{site.data.keyword.cloudant_short_notm}} 資料庫](#cloudant_write)部分中建立的文件。
{: note}

可以透過將 `<test>` 取代為資料庫名稱，並將 `--param doc` 旗標取代為要更新的資料庫中文件的 `id` 和內容，更新資料庫中的文件。


1. 可以透過執行下列指令來更新 `test` 資料庫中的文件。此範例將 `shade` 值新增到 `color` 文件。 

  ```
  ibmcloud fn action invoke /_/myCloudant/update-document --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}"
  ```
  {: pre}

  **輸出**
  ```
  {
    "id": "color",
    "ok": true,
    "rev": "2-8b904347bfe52e0f388ef6f39d6ba84f"
    }
  ```
  {: screen}

2. 若要查看更新，請重新提取該文件。

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id color
  ```
  {: pre}

  **範例文件**
  ```
  {
    "_id": "color",
    "_rev": "2-8b904347bfe52e0f388ef6f39d6ba84f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

## 使用過濾器函數建立觸發程式
{: #cloudant_trigger}

您可以使用 `changes` 資訊來源，配置服務在每次變更 {{site.data.keyword.cloudant_short_notm}} 資料庫時發動觸發程式。

**開始之前**

  建立針對 {{site.data.keyword.cloudant_short_notm}} 帳戶所配置的 `/whisk.system/cloudant` [套件連結](#cloudant_db)。

此範例中使用的參數。

| 參數 |說明|
| --- | --- |
|`dbname`|（必要）{{site.data.keyword.cloudant_short_notm}} 資料庫的名稱。|
|`iamApiKey`|（選用）Cloudant 資料庫的 IAM API 金鑰。如果指定，此值將用作認證以取代使用者名稱及密碼。|
|`iamUrl`|（選用）指定 `iamApiKey` 時使用的 IAM 記號服務 URL。預設值為 `https://iam.cloud.ibm.com/identity/token`。| 
|`maxTriggers`|（選用）達到此限制時，停止發動觸發程式。預設值為無限。|
|`filter` |（選用）在設計文件中定義的過濾函數。|
|`query_params`|（選用）過濾函數可能需要的任何其他查詢參數。|
|`includeDoc`|（淘汰）不再支援 `includeDoc` 參數用於 `/whisk.system/cloudant/changes` 資訊來源。|

</br>

1. 使用先前建立的套件連結中的 `changes` 資訊來源來建立名稱為 `cloudantTrigger` 的觸發程式。包括 `filter` 和 `query_params` 函數，以在新增（或修改）文件（當狀態為 `new` 時）時發動觸發程式。

  將 `/_/myCloudant` 取代為套件的名稱。此範例使用的是名稱為 `test` 的資料庫。
  ```
  ibmcloud fn trigger create cloudantTrigger --feed /_/myCloudant/changes \ --param dbname test
  ```
  {: pre}

  **輸出範例**

  ```
  ok: created trigger feed cloudantTrigger
  ```
  {: screen}

2. 將下列 JavaScript 程式碼儲存為 `cloudantChange.js`。

  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

3. 建立名為 `cloudantChange` 的動作，可用於觀察 changes 資訊來源。將 `<file_path>` 取代為電腦上 `cloudantChange.js` 檔案的檔案路徑。

  ```
  ibmcloud fn action create cloudantChange <file_path>/cloudantChange.js
  ```
  {: pre}

4. 建立名稱為 `cloudantRule` 的規則，用於將 `cloudantChange` 動作連接至先前建立的 `cloudantTrigger`。

  ```
  ibmcloud fn rule create cloudantRule cloudantTrigger cloudantChange
  ```
  {: pre}

5. 在另一個終端機視窗中，啟動輪詢，以便您可以看到啟動何時發生。

  ```
  ibmcloud fn activation poll
  ```
  {: pre}

6. 在 {{site.data.keyword.cloudant_short_notm}} 儀表板中，修改現有文件或建立文件。

7. 觀察針對每個文件變更的 `cloudantTrigger` 觸發程式的啟動。

**`cloudantTrigger` 的範例啟動**

```
Activation: 'cloudantTrigger' (ef6605cc05e04589a605cc05e04589d8)
[
    "{\"statusCode\":0,\"success\":true,\"activationId\":\"6067ed0d28774a68a7ed0d28771a684d\",\"rule\":\"<namespace>/cloudantRule\",\"action\":\"<namespace>/cloudantChange\"}"
]

Activation: 'cloudantChange' (6067ed0d28774a68a7ed0d28771a684d)
[
    "2019-06-24T16:46:10.428643Z    stdout: { changes: [ { rev: '19-f7f6d8607d6381d224321acfcfb8887e' } ],",
    "2019-06-24T16:46:10.428693Z    stdout: dbname: 'test',",
    "2019-06-24T16:46:10.428697Z    stdout: id: '6ca436c44074c4c2aa6a40c9a188b348',",
    "2019-06-24T16:46:10.428700Z    stdout: seq: '103-g1AAAAeLeJy91M9NwzAUBnCrrVQqDvTKCa4gpcT5YycnugFsAH5' }"
```
{: screen}

### 觸發程式啟動的資料結構
{: #cloudant_struct}

產生的事件的內容具有下列參數。

| 參數 |說明|
| --- | --- |
|`id`|文件 ID。|
|`seq`|由 {{site.data.keyword.cloudant_short_notm}} 產生的序列 ID。|
|`changes`|物件陣列，其中每個物件都有 `rev` 欄位，用於包含文件的修訂 ID。|

**觸發程式啟動的 JSON 表示**

```json
{
    "dbname": "test",
    "id": "6ca436c44074c4c2aa6a40c9a188b348",
    "seq": "2-g1AAAAL9aJyV-GJCaEuqx4-BktQkYp_dmIfC",
    "changes": [
        {
            "rev": "2-da3f80848a480379486fb4a2ad98fa16"
          }
      ]
}
```
{: codeblock}

## 過濾資料庫變更事件
{: #cloudant_filter}

可以定義過濾函數，以避免不必要的變更事件發動觸發程式。

**開始之前**

  建立針對 {{site.data.keyword.cloudant_short_notm}} 帳戶所配置的 `/whisk.system/cloudant` [套件連結](#cloudant_db)。

若要建立新的過濾函數，可以使用動作。

1. 將下列 JSON 過濾器儲存在名為 `design_doc.json` 的檔案中。

  ```json
  {
    "doc": {
      "_id": "_design/mailbox",
      "filters": {
        "by_status": "function(doc, req){if (doc.status != req.query.status){return false;} return true;}"
      }
    }
  }
  ```
  {: codeblock}

2. 在資料庫中建立設計文件並包含下列過濾函數。將 `<database_name>` 取代為資料庫的名稱，並將 `<file_path>` 取代為 `design_doc.json` 的檔案路徑。呼叫 `write` 動作以測試建立設計文件。

**指令語法**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname <database_name> -p overwrite true -P <file_path>/design_doc.json
```
{: pre}

**用於將 `design_doc.json` 檔案寫入 `test` 資料庫的範例指令**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname test -p overwrite true -P <file_path>/design_doc.json -r
```
{: pre}

**輸出範例**

```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}


如需 {{site.data.keyword.cloudant_short_notm}} 設計文件的相關資訊，請參閱[設計文件](/docs/services/Cloudant?topic=cloudant-design-documents)

## 使用動作序列處理個別文件
{: #cloudant_seq}

您可以在規則中使用動作序列，來提取及處理與 {{site.data.keyword.cloudant_short_notm}} 變更事件相關聯的文件。

**開始之前**

  建立針對 {{site.data.keyword.cloudant_short_notm}} 帳戶所配置的 `/whisk.system/cloudant` [套件連結](#cloudant_db)。 

此範例更新在[將文件寫入 {{site.data.keyword.cloudant_short_notm}} 資料庫](#cloudant_write)區段中建立的文件。
{: note}

### 建立用於處理個別文件的動作

若要建立用於處理對個別文件的變更的動作，請執行下列指令。
{: shortdesc}

1. 將下列程式碼儲存為 `docChange.js`。

  ```javascript
  function main(doc){
    return { "isBlue:" : doc.name === "blue"};
  }
  ```
  {: codeblock}

2. 建立名為 `docChange` 的動作，用於處理先前建立的名稱為 `blue` 的文件。將 `<file_path>` 取代為 `docChange.js` 的檔案路徑。

  ```
  ibmcloud fn action create docChange <file_path>/docChange.js
  ```
  {: pre}

  **輸出**
  ```
  ok: created action docChange
  ```
  {: screen}

### 建立具有 `read` 動作的序列 

可以使用 `docChange` 動作來組合`read` 動作，以建立動作序列。
{: shortdesc}

  ```
  ibmcloud fn action create docSequence --sequence /_/myCloudant/read,docChange
  ```
  {: pre}

  **輸出**
  ```
  ok: created action docSequence
  ```
  {: screen}

### 建立用於 `changes` 資訊來源的觸發程式

  ```
  ibmcloud fn trigger create docTrigger --feed /_/myCloudant/changes \
  --param dbname test
  ```
  {: pre}

### 建立規則以將觸發程式與序列相關聯

`docSequence` 動作可以在對新 {{site.data.keyword.cloudant_short_notm}} 觸發程式事件啟動該動作的規則中使用。

  ```
  ibmcloud fn rule create docRule docTrigger docSequence
  ```
  {: pre}

  **輸出**
  ```
  ok: created rule docRule
  ```

  **啟動範例**
  ```
  "{\"statusCode\":0,\"success\":true,\"activationId\":\"144a4f95198a49ec8a4f95198a79ecc8\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ```
  {: screen}

### 測試序列

1. 透過對先前建立的 `blue` 檔案進行變更來測試 `docSequence`。在此範例中，`shade` 值變更為 `indigo`。

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}" -p overwrite true
  ```
  {: pre}

  **啟動範例**

  ```
  Activation: 'docChange' (aa3e8fc3030446b2be8fc3030406b2eb)
  []

  Activation: 'docSequence' (23e0a17bebd3486ca0a17bebd3186c8d)
  [
      "8d42679127f3400382679127f300039d",
      "aa3e8fc3030446b2be8fc3030406b2eb"
  ]

  Activation: 'docTrigger' (db6de778bb084366ade778bb08036685)
  [
      "{\"statusCode\":0,\"success\":true,\"activationId\":\"23e0a17bebd3486ca0a17bebd3186c8d\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ]
  ```
  {: screen}

2. 透過呼叫 `read` 動作來驗證檔案是否已更新為包含 `shade` 值。將 `<database>` 名稱取代為資料庫的名稱。

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id color
  ```
  {: pre}

  **輸出**
  ```
  {
    "_id": "color",
    "_rev": "3-6845b04618338f717676f16edf32a78f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

### 後續步驟
既然您在接聽對 {{site.data.keyword.cloudant_short_notm}} 資料庫中文件的變更，現在可以使用 [`/whisk.system/slack` 套件](/docs/openwhisk?topic=cloud-functions-pkg_slack)來觸發與變更相關的 Slack 通知。


