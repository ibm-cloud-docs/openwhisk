---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Cloudant 事件來源
{: #openwhisk_cloudant}

瞭解如何接聽對 {{site.data.keyword.cloudant}} 資料庫的變更、過濾資料庫變更事件，以及使用動作序列來處理 {{site.data.keyword.cloudant_short_notm}} 資料庫中的文件。`/whisk.system/cloudant` 套件可讓您使用 {{site.data.keyword.cloudant_short_notm}} 資料庫，並包括下列動作及「資訊來源」：

|實體|類型|參數|說明
|
| --- | --- | --- | --- |
| `/whisk.system/cloudant` |套件|dbname、host、username、password|使用 Cloudant 資料庫。|
|`/whisk.system/cloudant/read` |動作|dbname、id|讀取資料庫中的文件。|
|`/whisk.system/cloudant/write` |動作|dbname、overwrite、doc|將文件寫入資料庫。|
|`/whisk.system/cloudant/changes` |資訊來源|dbname、filter、query_params、maxTriggers|在資料庫變更時發動觸發程式事件。|
{: shortdesc}

下列各節將引導您逐步配置相關聯的套件，並且說明如何使用 `/whisk.system/cloudant` 套件中的動作及「資訊來源」。如需設定 {{site.data.keyword.cloudant_short_notm}} 資料庫以及讀取或寫入其中的相關資訊，請參閱 [{{site.data.keyword.cloudant_short_notm}} 動作](./cloudant_actions.html)。

## 使用過濾器函數建立觸發程式

您可以使用「`changes` 資訊來源」，配置服務在每次變更 {{site.data.keyword.cloudant_short_notm}} 資料庫時發動觸發程式。

此範例中使用的參數如下所示：

**dbname**：{{site.data.keyword.cloudant_short_notm}} 資料庫的名稱_（必要）_。

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

如果您無法觀察到新啟動，請參閱 [{{site.data.keyword.cloudant_short_notm}}](./cloudant_actions.html) 主題，以示範如何在 {{site.data.keyword.cloudant_short_notm}} 資料庫中讀取及寫入。測試讀取及寫入步驟，有助於驗證 {{site.data.keyword.cloudant_short_notm}} 認證正確無誤。
{: tip}

## 觸發程式事件的資料結構

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

## 過濾資料庫變更事件

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

## 使用動作序列及變更觸發程式來處理 {{site.data.keyword.cloudant_short_notm}} 資料庫中的文件
{: #openwhisk_catalog_cloudant_read_change notoc}

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

您可以重建先前使用 `includeDoc` 所建立的觸發程式。請遵循下列步驟來重建觸發程式：
```
ibmcloud fn trigger delete myCloudantTrigger
```
{: pre}

```
ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
```
{: pre}

此範例可以用來建立動作序列，以讀取已變更的文件以及呼叫您的現有動作。請記得停用所有可能不再有效的規則，並且使用動作序列型樣來建立新規則。
