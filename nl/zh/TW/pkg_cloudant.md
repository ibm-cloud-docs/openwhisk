---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: cloudant, event, action, trigger, sequence

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


# Cloudant
{: #pkg_cloudant}

預先安裝的 `/whisk.system/cloudant` 套件可讓您使用 [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) 資料庫。使用此套件不需要任何服務連結。


## 可用動作和資訊來源
{: #cloudant_available}

|實體|類型|參數|說明|
| --- | --- | --- | --- |
|`/whisk.system/cloudant` |套件|dbname、host、username、password|使用 Cloudant 資料庫。|
|`/whisk.system/cloudant/read` |動作|dbname、id|讀取資料庫中的文件。|
|`/whisk.system/cloudant/write` |動作|dbname、overwrite、doc|將文件寫入資料庫。|
|`/whisk.system/cloudant/changes` |資訊來源| dbname、iamApiKey、iamUrl、filter、query_params、maxTriggers |在資料庫變更時發動觸發程式事件。|
{: shortdesc}

### 在 {{site.data.keyword.Bluemix_notm}} 中設定 {{site.data.keyword.cloudant_short_notm}} 資料庫
{: #cloudant_db}

如果您從 {{site.data.keyword.Bluemix_notm}} 使用 {{site.data.keyword.openwhisk}}，則可以使用 {{site.data.keyword.openwhisk}} CLI 外掛程式，將服務連結至動作或套件。

您必須先手動建立 {{site.data.keyword.cloudant_short_notm}} 帳戶的套件連結。

1. 建立針對 {{site.data.keyword.cloudant_short_notm}} 帳戶所配置的套件連結。
  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. 驗證套件連結已存在。
  ```
  ibmcloud fn package list
  ```
  {: pre}

  輸出範例：
  ```
  packages
  /myNamespace/myCloudant private
  ```
  {: screen}

3. 取得您要連結至動作或套件之服務實例的名稱。
    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    輸出範例：
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

    輸出範例：
    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. 將服務連結至您在步驟 1 中建立的套件。
    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1' 
    ```
    {: pre}

6. 驗證認證已順利連結。
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    輸出範例：
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

    在此範例中，Cloudant 服務的認證屬於名為 `__bx_creds` 的參數。


### 從 {{site.data.keyword.cloudant_short_notm}} 資料庫中讀取
{: #cloudant_read}

您可以使用動作，從稱為 **testdb** 的 {{site.data.keyword.cloudant_short_notm}} 資料庫中提取文件。請確定此資料庫存在於 {{site.data.keyword.cloudant_short_notm}} 帳戶中。

- 使用您先前建立的套件連結中的 **read** 動作，來提取文件。請務必將 `/_/myCloudant` 取代為您的套件名稱。
  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}

  輸出範例：
  ```
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```
  {: screen}

### 寫入 {{site.data.keyword.cloudant_short_notm}} 資料庫
{: #cloudant_write}

您可以使用動作，將文件儲存至稱為 **testdb** 的 {{site.data.keyword.cloudant_short_notm}} 資料庫。請確定此資料庫存在於 {{site.data.keyword.cloudant_short_notm}} 帳戶中。

1. 使用您先前建立的套件連結中的 **write** 動作，來儲存文件。請務必將 `/_/myCloudant` 取代為您的套件名稱。
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  {: pre}

  輸出範例：
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. 在 {{site.data.keyword.cloudant_short_notm}} 儀表板中瀏覽到文件，以驗證文件已存在。

  **testdb** 資料庫的儀表板 URL 可能與下面類似：`https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100`。


### 使用過濾器函數建立觸發程式
{: #cloudant_trigger}

您可以使用 `changes` 資訊來源，配置服務在每次變更 {{site.data.keyword.cloudant_short_notm}} 資料庫時發動觸發程式。

此範例中使用的參數如下所示：

**dbname**：{{site.data.keyword.cloudant_short_notm}} 資料庫的名稱_（必要）_。

**iamApiKey**：Cloudant 資料庫的 IAM API 金鑰。如果已指定，則會用來作為認證，而非使用者名稱和密碼_（選用）_。

**iamUrl**：指定 `iamApiKey` 時使用的 IAM 記號服務 URL。預設值為 `https://iam.bluemix.net/identity/token`_（選用）_。

**maxTriggers**：在達到此限制時停止發動觸發程式_（選用）_。預設值為無限。

**filter**：設計文件上定義的過濾器函數_（選用）_。

**query_params**：過濾器函數的額外查詢參數_（選用）_。

1. 使用您先前建立的套件連結中的 `changes` 資訊來源，來建立名為 **myCloudantTrigger** 的觸發程式。包括 `filter` 和 `query_params` 函數，以在新增（或修改）文件（當狀態為 `new` 時）時發動觸發程式。


  請務必將 `/_/myCloudant` 取代為您的套件名稱。
  ```
  ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes \
  --param dbname testdb \
  --param filter "mailbox/by_status" \
  --param query_params '{"status":"new"}'
  ```
  {: pre}

  輸出範例：
  ```
  ok: created trigger feed myCloudantTrigger
  ```
  {: screen}

2. 開始輪詢啟動，以清楚瞭解發生的情況。
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

3. 建立可用來觀察變更資訊來源效果的動作。例如，稱為 **showCloudantChange** 且包含下列 JavaScript 程式碼的動作：
  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

4. 建立規則，以將 **showCloudantChange** 動作連接至先前建立的觸發程式：
  ```
  ibmcloud fn rule update aCloudantRule myCloudantTrigger showCloudantChange
  ```
  {: pre}

5. 建立動作以及規則來將它們與 **myCloudantTrigger** 觸發程式相關聯。

6. 在 {{site.data.keyword.cloudant_short_notm}} 儀表板中，修改現有文件，或建立新文件。文件應該有設為 **new** 的_狀態_ 欄位。

7. 根據過濾器函數及查詢參數，只有在文件狀態為 **new** 時，才會觀察到每一個文件變更的 **myCloudantTrigger** 觸發程式的新啟動。

測試讀取及寫入步驟，有助於驗證 {{site.data.keyword.cloudant_short_notm}} 認證正確無誤。


### 觸發程式事件的資料結構
{: #cloudant_struct}

所產生事件的內容具有下列參數：

  - `id`：文件 ID。
  - `seq`：{{site.data.keyword.cloudant_short_notm}} 所產生的序列 ID。
  - `changes`：物件陣列，各物件有包含文件修訂 ID 的 `rev` 欄位。

觸發程式事件的 JSON 表示法如下所示：
```json
{
    "dbname": "testdb",
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

### 過濾資料庫變更事件
{: #cloudant_filter}

您可以選擇性地定義過濾器函數，以避免不必要的變更事件發動觸發程式。

若要建立新的過濾器函數，您可以使用動作。

使用下列過濾器函數來建立 json 文件檔案 `design_doc.json`：
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

使用下列過濾器函數，在資料庫上建立設計文件：
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
```
{: pre}

畫面上會列印新設計文件的資訊：
```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}

### 使用動作序列及變更觸發程式來處理 {{site.data.keyword.cloudant_short_notm}} 資料庫中的文件
{: #cloudant_seq}

您可以在規則中使用動作序列，來提取及處理與 {{site.data.keyword.cloudant_short_notm}} 變更事件相關聯的文件。

處理文件之動作的範例程式碼：
```javascript
function main(doc){
  return { "isWalter:" : doc.name === "Walter White"};
}
```
{: codeblock}

建立動作來處理 {{site.data.keyword.cloudant_short_notm}} 中的文件：
```
ibmcloud fn action create myAction myAction.js
```
{: pre}

若要從資料庫中讀取文件，您可以使用 {{site.data.keyword.cloudant_short_notm}} 套件中的 `read` 動作。
可以使用 `myAction` 來組合 `read` 動作，以建立動作序列。
```
ibmcloud fn action create sequenceAction --sequence /_/myCloudant/read,myAction
```
{: pre}

動作 `sequenceAction` 可以用於規則中，在新的 {{site.data.keyword.cloudant_short_notm}} 觸發程式事件上啟動動作。
```
ibmcloud fn rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**附註：**{{site.data.keyword.cloudant_short_notm}} `changes` 觸發程式用來支援不再受支援的參數 `includeDoc`。

您可以重新建立之前使用 `includeDoc` 建立的觸發程式。請遵循下列步驟來重建觸發程式：
```
ibmcloud fn trigger delete myCloudantTrigger
```
{: pre}

```
ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
```
{: pre}

此範例可以用來建立動作序列，以讀取已變更的文件以及呼叫您的現有動作。請記得停用所有可能不再有效的規則，並且使用動作序列型樣來建立新規則。

