---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-19"

keywords: message hub, event, trigger, messages, batch, listen

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Event Streams 事件來源
{: #openwhisk_catalog_message_hub}

您可以使用資訊來源，來建立在訊息張貼至 {{site.data.keyword.messagehub_full}} 實例時做出反應的觸發程式。瞭解如何在使用或未使用 {{site.data.keyword.Bluemix}} 的情況下建立 {{site.data.keyword.messagehub}} 觸發程式、接聽訊息，以及處理批次的訊息。
{: shortdesc}

## {{site.data.keyword.messagehub}} 套件

`/messaging/messageHubProduce` 動作已遭淘汰，將在未來予以移除。它在東京地區已被移除。若要維護最佳效能，請移轉 `/messaging/messageHubProduce` 動作的使用，以在將資料產生至 Message Hub/Kafka 時使用持續性連線。
{: deprecated}

此套件可讓您利用原生高效能 Kafka API，與 [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub) 實例進行通訊，以發佈及使用訊息。如需 {{site.data.keyword.messagehub}} 套件、其設定方式及訊息產生方式的相關資訊，請參閱 [{{site.data.keyword.messagehub}} 套件](/docs/openwhisk?topic=cloud-functions-catalog_message_hub)主題。

## 建立接聽 {{site.data.keyword.messagehub}} 實例的觸發程式
{: #create_message_hub_trigger}

為了能夠建立在將訊息張貼至 {{site.data.keyword.messagehub}} 實例時做出反應的觸發程式，您需要使用名為 `/messaging/messageHubFeed` 的資訊來源。資訊來源動作支援下列參數：

|名稱|類型|說明|
|---|---|---|
|kafka_brokers_sasl|JSON 字串陣列|此參數是在 {{site.data.keyword.messagehub}} 實例中包含分配管理系統的 `<host>:<port>` 字串陣列|
|使用者|字串|您的 {{site.data.keyword.messagehub}} 使用者名稱。|
|password|字串|您的 {{site.data.keyword.messagehub}} 密碼。|
|topic|字串|您想要觸發程式接聽的主題。|
|kafka_admin_url|URL 字串|{{site.data.keyword.messagehub}} 管理 REST 介面的 URL。|
|isJSONData|布林（選用 - 預設=false）|設為 `true` 時，提供者會先嘗試將訊息值剖析為 JSON，再將它傳遞為觸發程式有效負載。|
|isBinaryKey|布林（選用 - 預設=false）|設為 `true` 時，提供者會先將金鑰值編碼為 Base64，再將它傳遞為觸發程式有效負載。|
|isBinaryValue|布林（選用 - 預設=false）|設為 `true` 時，提供者會先將訊息值編碼為 Base64，再將它傳遞為觸發程式有效負載。|

雖然此參數清單看起來令人卻步，但可以使用 `ibmcloud fn package refresh` CLI 外掛程式指令自動設定。

1. 在用於 {{site.data.keyword.openwhisk}} 的現行組織及空間下，建立 {{site.data.keyword.messagehub}} 服務的實例。

2. 驗證您要接聽的主題現已在 {{site.data.keyword.messagehub}} 中提供，或建立新主題，例如 **mytopic**。

3. 重新整理「名稱空間」中的套件。重新整理會自動建立您所建立之 {{site.data.keyword.messagehub}} 服務實例的套件連結。
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  輸出範例：
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```
  {: screen}

4. 列出您「名稱空間」中的套件，以顯示您的套件連結現在可供使用。
  ```
  ibmcloud fn package list
  ```
  {: pre}

  輸出範例：
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```
  {: screen}

  您的套件連結現在包含與 {{site.data.keyword.messagehub}} 服務實例相關聯的認證。

5. 您現在只需要建立觸發程式，以在將新訊息張貼至 {{site.data.keyword.messagehub}} 主題時發動。
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## 在 {{site.data.keyword.Bluemix_notm}} 外部建立 {{site.data.keyword.messagehub}} 套件的觸發程式
{: #create_message_hub_trigger_outside}

如果您要在 {{site.data.keyword.Bluemix_notm}} 外部設定 {{site.data.keyword.messagehub}}，則必須手動建立 {{site.data.keyword.messagehub}} 服務的套件連結。您需要 {{site.data.keyword.messagehub}} 服務認證及連線資訊。

1. 建立針對 {{site.data.keyword.messagehub}} 服務所配置的套件連結。
  ```
  ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. 您現在可以使用新的套件來建立觸發程式，以在將新訊息張貼至 {{site.data.keyword.messagehub}} 主題時發動。
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}

## 接聽訊息
{: #message_hub_listen}

建立觸發程式之後，系統即會監視傳訊服務中的指定主題。張貼新的訊息時，會發動觸發程式。

該觸發程式的有效負載包含 `messages` 欄位，此欄位是自上次發動觸發程式後張貼的訊息陣列。陣列中的每一個訊息物件都會包含下列欄位：
- topic
- partition
- offset
- key
- value

在 Kafka 術語中，這些欄位十分清楚明瞭。不過，`key` 有一個特性，稱為 `isBinaryKey`，可讓 `key` 傳輸二進位資料。此外，`value` 需要特殊考量。欄位 `isJSONData` 和 `isBinaryValue` 可用來處理 JSON 和二進位訊息。這兩個欄位（`isJSONData` 和 `isBinaryValue`）無法一起使用。

舉例來說，如果在建立觸發程式時已將 `isBinaryKey` 設為 `true`，則 `key` 從所發動之觸發程式的有效負載傳回時會編碼為 Base64 字串。

如果張貼 `Some key` 的 `key` 時將 `isBinaryKey` 設為 `true`，則觸發程式有效負載會類似下列範例：
```json
{
    "messages": [
    {
      "partition": 0,
            "key": "U29tZSBrZXk=",
            "offset": 421760,
            "topic": "mytopic",
            "value": "Some value"
        }
    ]
}
```
{: codeblock}

如果在建立觸發程式時將 `isJSONData` 參數設為 `false`（或根本未設定），則 `value` 欄位是所張貼訊息的原始值。不過，如果在建立觸發程式時將 `isJSONData` 設為 `true`，則系統會嘗試根據最佳效能來將此值剖析為 JSON 物件。如果剖析成功，觸發程式有效負載中的 `value` 則為產生的 JSON 物件。

如果張貼 `{"title": "Some string", "amount": 5, "isAwesome": true}` 的訊息時將 `isJSONData` 設為 `true`，則觸發程式有效負載可能類似下列範例：
```json
{
  "messages": [
    {
      "partition": 0,
        "key": null,
        "offset": 421760,
        "topic": "mytopic",
        "value": {
            "amount": 5,
            "isAwesome": true,
            "title": "Some string"
        }
      }
  ]
}
```
{: codeblock}

不過，如果張貼相同的訊息內容時將 `isJSONData` 設為 `false`，則觸發程式有效負載會類似下列範例：
```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421761,
      "topic": "mytopic",
      "value": "{\"title\": \"Some string\", \"amount\": 5, \"isAwesome\": true}"
    }
  ]
}
```
{: codeblock}

與 `isJSONData` 類似，如果在建立觸發程式期間將 `isBinaryValue` 設為 `true`，則觸發程式有效負載中產生的 `value` 會編碼為 Base64 字串。

如果張貼 `Some data` 的 `value` 時將 `isBinaryValue` 設為 `true`，則觸發程式有效負載可能會類似下列範例：
```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": "U29tZSBkYXRh"
    }
  ]
}
```
{: codeblock}

如果張貼相同的訊息，但未將 `isBinaryData` 設為 `true`，則觸發程式有效負載會類似下列範例：
```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": "Some data"
    }
  ]
}
```
{: codeblock}

### 會分批處理訊息
請注意，觸發程式有效負載包含訊息的陣列。如果這些訊息快速地產生至您的傳訊系統，則資訊來源會嘗試將張貼的訊息分批處理為觸發程式的單一次發動。批次處理容許將訊息更快速且更有效率地張貼至觸發程式。

請記住，使用程式碼編寫由觸發程式所發動的動作時，有效負載中的訊息數目在技術上是無界限的，但一律會大於 0。請參閱下列批次訊息範例（請注意 *offset* 值的變化）：
```json
{
  "messages": [
    {
      "partition": 0,
         "key": null,
         "offset": 100,
         "topic": "mytopic",
         "value": {
             "amount": 5
         }
       },
      {
        "partition": 0,
         "key": null,
         "offset": 101,
         "topic": "mytopic",
         "value": {
             "amount": 1
         }
       },
      {
        "partition": 0,
         "key": null,
         "offset": 102,
         "topic": "mytopic",
         "value": {
             "amount": 999
         }
       }
  ]
}
```

## 範例
{: #examples}

### 將 OpenWhisk 與 {{site.data.keyword.messagehub}}、Node Red、IBM Watson IoT、{{site.data.keyword.cos_full_notm}} 和 IBM Data Science Experience 整合
您可以[在這裡找到](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0)將 OpenWhisk 與 {{site.data.keyword.messagehub}}、Node Red、IBM Watson IoT、{{site.data.keyword.cos_full}}、IBM Data Science Experience (Spark) 服務整合的範例。

## 參考資料
{: #message_references}
- [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org)
