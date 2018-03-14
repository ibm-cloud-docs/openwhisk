---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 使用 Message Hub 套件
{: #openwhisk_catalog_message_hub}

此套件可讓您利用原生的高效能 Kafka API，與 [Message Hub](https://developer.ibm.com/messaging/message-hub) 實例進行通訊，以發佈及使用訊息。
{: shortdesc}

## 建立接聽 IBM MessageHub 實例的觸發程式
{: #openwhisk_catalog_message_hub_trigger}

若要建立在將訊息張貼至 Message Hub 實例時做出反應的「觸發程式」，您需要使用名為 `/messaging/messageHubFeed` 的「資訊來源」。此「資訊來源動作」支援下列參數：

|名稱|類型|說明|
|---|---|---|
|kafka_brokers_sasl|JSON 字串陣列|此參數是在 Message Hub 實例中包含分配管理系統的 `<host>:<port>` 字串陣列|
|使用者|字串|您的 Message Hub 使用者名稱|
|password|字串|您的 Message Hub 密碼|
|topic|字串|您想要觸發程式接聽的主題|
|kafka_admin_url|URL 字串|Message Hub 管理 REST 介面的 URL|
|isJSONData|布林（選用 - 預設=false）|設為 `true` 時，提供者會先嘗試將訊息值剖析為 JSON，再將它傳遞為「觸發程式」有效負載。|
|isBinaryKey|布林（選用 - 預設=false）|設為 `true` 時，提供者會先將金鑰值編碼為 Base64，再將它傳遞為「觸發程式」有效負載。|
|isBinaryValue|布林（選用 - 預設=false）|設為 `true` 時，提供者會先將訊息值編碼為 Base64，再將它傳遞為「觸發程式」有效負載。|

雖然此參數清單看起來令人卻步，但可以使用套件重新整理 CLI 指令自動設定：

1. 在用於 OpenWhisk 的現行組織及空間下，建立 Message Hub 服務實例。

2. 驗證您要接聽的主題在 Message Hub 中有提供，或建立新主題，例如 `mytopic`。

3. 重新整理名稱空間中的套件。重新整理會自動為您建立的 Message Hub 服務實例建立套件連結。

  ```
wsk package refresh
  ```
  {: pre}
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```

  ```
wsk package list
  ```
  {: pre}
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```

  您的套件連結現在包含與 Message Hub 實例相關聯的認證。

4. 您現在只需要建立「觸發程式」，以在將新訊息張貼至 Message Hub 主題時發動。

  ```
  wsk trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## 在 {{site.data.keyword.Bluemix_notm}} 外部設定 Message Hub 套件

如果您要在 {{site.data.keyword.Bluemix_notm}} 外部設定 Message Hub，則必須手動建立 Message Hub 服務的套件連結。您需要 Message Hub 服務認證及連線資訊。

1. 建立針對 Message Hub 服務所配置的套件連結。

  ```
  wsk package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your Message Hub user> -p password <your Message Hub password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. 您現在可以使用新的套件來建立「觸發程式」，以在將新訊息張貼至 Message Hub 主題時發動。

  ```
  wsk trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}


## 接聽訊息
{: #openwhisk_catalog_message_hub_listen}

建立「觸發程式」之後，系統會監視傳訊服務中的指定主題。張貼新的訊息時，會發動「觸發程式」。

該「觸發程式」的有效負載包含 `messages` 欄位，此欄位是自上次發動「觸發程式」後張貼的訊息陣列。陣列中的每一個訊息物件都會包含下列欄位：
- topic
- partition
- offset
- key
- value

在 Kafka 術語中，這些欄位十分清楚明瞭。不過，`key` 有一個特性，稱為 `isBinaryKey`，可讓 `key` 傳輸二進位資料。此外，`value` 需要特殊考量。欄位 `isJSONData` 和 `isBinaryValue` 可用來處理 JSON 和二進位訊息。這兩個欄位（`isJSONData` 和 `isBinaryValue`）無法一起使用。

舉例來說，如果在建立「觸發程式」時，`isBinaryKey` 設為 `true`，`key` 從所發動之「觸發程式」的有效負載傳回時，會編碼為 Base64 字串。

如果張貼 `Some key` 的 `key` 時，`isBinaryKey` 設為 `true`，則「觸發程式」有效負載會類似下列範例：

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

如果在建立「觸發程式」時，`isJSONData` 參數設為 `false`（或根本未設定），則 `value` 欄位是所張貼訊息的原始值。不過，如果 `isJSONData` 在建立「觸發程式」時設為 `true`，則系統會嘗試根據最佳效能來將此值剖析為 JSON 物件。如果剖析成功，「觸發程式」有效負載中的 `value` 則為產生的 JSON 物件。

如果張貼 `{"title": "Some string", "amount": 5, "isAwesome": true}` 的訊息時 `isJSONData` 設為 `true`，則「觸發程式」有效負載可能類似下列範例：

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

不過，如果張貼相同的訊息內容時 `isJSONData` 設為 `false`，則「觸發程式」有效負載會類似下列範例：

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

與 `isJSONData` 類似，如果在建立「觸發程式」期間，`isBinaryValue` 設為 `true`，則「觸發程式」有效負載中產生的 `value` 會編碼為 Base64 字串。

如果張貼 `Some data` 的 `value` 時，`isBinaryValue` 設為 `true`，則「觸發程式」有效負載可能會類似下列範例：

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

如果張貼相同的訊息，但未將 `isBinaryData` 設為 `true`，則「觸發程式」有效負載會類似下列範例：

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

### 會分批處理訊息
請注意，「觸發程式」有效負載包含訊息的陣列。如果訊息快速地產生給您的傳訊系統，則「資訊來源」會嘗試將張貼的訊息分批處理為「觸發程式」的單一次發動。批次程序容許將訊息更快速且更有效率地張貼至「觸發程式」。

請記住，使用程式碼編寫由「觸發程式」所發動的「動作」時，有效負載中的訊息數目在技術上是無界限的，但一律會大於 0。請參閱下列批次訊息範例（請注意 *offset* 值的變化）：
 
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

## 產生訊息至 Message Hub
如果您想要使用「OpenWhisk 動作」，以方便產生訊息至 Message Hub，您可以使用「`/messaging/messageHubProduce` 動作」。此「動作」會採用下列參數：

|名稱|類型|說明|
|---|---|---|
|kafka_brokers_sasl|JSON 字串陣列|此參數是在 Message Hub 實例中包含分配管理系統的 `<host>:<port>` 字串陣列|
|使用者|字串|您的 Message Hub 使用者名稱|
|password|字串|您的 Message Hub 密碼|
|topic|字串|您想要觸發程式接聽的主題|
|value|字串|您想要產生之訊息的值。|
|key|字串（選用）|您想要產生之訊息的金鑰。|

雖然前三個參數可以使用 `wsk package refresh` 自動連結，但還是請參閱下列範例，其使用所有必要參數來呼叫動作：

```
wsk action invoke /messaging/messageHubProduce -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p topic mytopic -p user <your Message Hub user> -p password <your Message Hub password> -p value "This is the content of my message"
```
{: pre}

## 範例

### 將 OpenWhisk 與 IBM Message Hub、Node Red、IBM Watson IoT、IBM Object Storage 和 IBM Data Science Experience 整合
您可以[在這裡找到](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0)將 OpenWhisk 與 IBM Message Hub、Node Red、IBM Watson IoT、IBM Object Storage、IBM Data Science Experience (Spark) 服務整合的範例。

## 參考資料
- [IBM Message Hub](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
