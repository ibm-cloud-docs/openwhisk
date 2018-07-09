---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# {{site.data.keyword.messagehub}}
{: #openwhisk_catalog_message_hub}

此套件可讓您利用原生高效能 Kafka API 與 [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub) 實例進行通訊，以發佈及使用訊息。
{: shortdesc}

## 使用 {{site.data.keyword.Bluemix_notm}} 設定 {{site.data.keyword.messagehub}} 套件
{: #create_message_hub_ibm}

1. 在用於 {{site.data.keyword.openwhisk}} 的現行組織及空間下，建立 {{site.data.keyword.messagehub}} 服務的實例。

2. 驗證您要接聽的主題現已在 {{site.data.keyword.messagehub}} 中提供，或建立新主題，例如，標題為 **mytopic**。

3. 重新整理「名稱空間」中的套件。重新整理會自動建立您所建立之 {{site.data.keyword.messagehub}} 服務實例的套件連結。
  ```
  ibmcloud wsk package refresh
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
  ibmcloud wsk package list
  ```
  {: pre}

  輸出範例：
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```
  {: screen}

  您的套件連結現在包含與 {{site.data.keyword.messagehub}} 服務實例相關聯的認證。

## 在 {{site.data.keyword.Bluemix_notm}} 外部設定 {{site.data.keyword.messagehub}} 套件

如果您要在 {{site.data.keyword.Bluemix_notm}} 外部設定 {{site.data.keyword.messagehub}}，則必須手動建立 {{site.data.keyword.messagehub}} 服務的套件連結。您需要 {{site.data.keyword.messagehub}} 服務認證及連線資訊。

建立針對 {{site.data.keyword.messagehub}} 服務所配置的套件連結。
```
ibmcloud wsk package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

## 使用事件來接聽訊息

如需如何在 {{site.data.keyword.messagehub}} 中使用觸發程式接聽訊息的詳細資訊，請參閱涵蓋下列作業的下列 [{{site.data.keyword.messagehub}} 事件來源](./openwhisk_messagehub.html)主題：
* [建立接聽 {{site.data.keyword.messagehub}} 實例的觸發程式](./openwhisk_messagehub.html#create_message_hub_trigger)
* [在 {{site.data.keyword.Bluemix_notm}} 外部建立 {{site.data.keyword.messagehub}} 套件的觸發程式](./openwhisk_messagehub.html#create_message_hub_trigger_outside)
* [接聽訊息](./openwhisk_messagehub.html#message_hub_listen)
* [範例](./openwhisk_messagehub.html#examples)

## 將訊息產生至 {{site.data.keyword.messagehub}}
{: #producing_messages}

`/messaging/messageHubProduce` 動作已遭淘汰，將在未來予以移除。若要維護最佳效能，請移轉 `/messaging/messageHubProduce` 動作的使用，以在將資料產生至 {{site.data.keyword.messagehub}}/Kafka 時使用持續性連線。
{: tip}

如果您要使用 {{site.data.keyword.openwhisk_short}} 動作，以方便將訊息產生至 {{site.data.keyword.messagehub}}，您可以使用 `/messaging/messageHubProduce` 動作。此動作會採用下列參數：

|名稱|類型|說明|
|---|---|---|
|kafka_brokers_sasl|JSON 字串陣列|此參數是在 {{site.data.keyword.messagehub}} 實例中包含分配管理系統的 `<host>:<port>` 字串陣列。|
|使用者|字串|您的 {{site.data.keyword.messagehub}} 使用者名稱。|
|password|字串|您的 {{site.data.keyword.messagehub}} 密碼。|
|topic|字串|您想要觸發程式接聽的主題。|
|value|字串|您想要產生之訊息的值。|
|key|字串（選用）|您想要產生之訊息的金鑰。|

雖然前三個參數可以使用 `ibmcloud wsk package refresh` 自動連結，但還是請參閱下列範例，其使用所有必要參數來呼叫動作：
```
ibmcloud wsk action invoke /messaging/messageHubProduce -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p topic mytopic -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p value "This is the content of my message"
```
{: pre}

## 參考資料
- [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
