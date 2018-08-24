---

copyright:
  years: 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# 部署快速入門範本
{: #serviceauth}

{{site.data.keyword.openwhisk}} 提供範本型錄，以協助您在下一個專案中快速開始。範本是動作、觸發程式、序列的組合，也可以併入 {{site.data.keyword.Bluemix}} 中的服務實例。使用範本，您可以快速且輕鬆地建立專案，並立即開始編碼。

此指導教學逐步引導您部署可透過 {{site.data.keyword.openwhisk_short}} 提供的範本。
{: shortdesc}

## 可用的快速入門範本
{: #available-templates}

|名稱|說明|支援的執行時期 |
|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} 事件](./deploy_templates.html#cloudant-template) | 當 {{site.data.keyword.cloudant}} 有記載已編輯或已新增時，請在主控台中記載該變更。|Node.js、Swift、Python、PHP |
| [上傳影像](./deploy_templates.html#cos-upload-image) | Web 動作可讓您將影像上傳至 {{site.data.keyword.cos_full}} 實例的儲存區，然後擷取該影像的縮圖。| Node.js |
| [取得 HTTP 資源](./deploy_templates.html#get-http-resource-template) | 為了回應 HTTP 事件而呼叫的 Web 動作，會從 Yahoo Weather API 提取資料。|Node.js、Python |
| [Hello World](./deploy_templates.html#hello-world-template) | 此動作接受單一參數，但必須是 JSON 物件。|Node.js、Swift、Python、PHP |
| [{{site.data.keyword.messagehub}} 事件](./deploy_templates.html#messagehub-events-template) | 當 {{site.data.keyword.messagehub_full}} 主題有新增資料時，請在主控台中記載該變更。|Node.js、Swift、Python、PHP |
| [定期 Slack 提示](./deploy_templates.html#slack-reminder-template) | 根據定期觸發程式而張貼至 Slack 的動作。|Node.js、Swift、Python、PHP |

## 部署 {{site.data.keyword.cloudant_short_notm}} 事件範本
{: #cloudant-template}

{{site.data.keyword.cloudant_short_notm}} 範本會建立一連串動作，以及開始該序列的觸發程式。當所連接的 {{site.data.keyword.cloudant_short_notm}} 資料庫（應該是 cat 資料庫，有名稱及顏色）發生變更時，即會發動觸發程式。預期的資料項目是 cat，其具有已定義的名稱和顏色。將新的 cat 新增至資料庫或已編輯現行的 cat 時，資料會記載至主控台。

1. 若要建立範本，請移至 [{{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)，然後按一下**開始建立**。

2. 按一下**快速入門範本**。

3. 按一下 **Cloudant 事件**。

### 建立 {{site.data.keyword.cloudant_short_notm}} 動作

1. 接下來，提供套件的名稱，或使用提供的預設名稱 `new-cloudant-item`。

2. 在**動作**下拉清單下，選取您要擁有之動作的運行環境（nodejs、swift、python 或 php）。在這個範例中，請選取 **nodejs**，然後按**下一步**。

### 建立 {{site.data.keyword.cloudant_short_notm}} 觸發程式

觸發程式會在接收事件來源中的事件時呼叫動作。若要建立 {{site.data.keyword.cloudant_short_notm}} 範本的觸發程式，請提供所需的 {{site.data.keyword.cloudant_short_notm}} 服務實例資訊給觸發程式。

#### 建立 {{site.data.keyword.cloudant_short_notm}} 服務實例

您可以選取以下任一項：
  * **建立您自己的實例**
  * **輸入您自己的認證**

1. 在這個範例中，請選擇**建立您自己的實例**。

2. 即會開啟一個蹦現畫面，將您帶到含有 {{site.data.keyword.cloudant_short_notm}} 設定頁面的新標籤。建立 {{site.data.keyword.cloudant_short_notm}} 實例之後，您必須建立一組「服務認證」，然後按一下**確定**來關閉此標籤並回到這個頁面。

3. 現在，請選擇**輸入您自己的認證**，並提供下列資訊：
  * 使用者名稱 - _您的 {{site.data.keyword.cloudant_short_notm}} 使用者名稱_
  * 密碼 - _您的 {{site.data.keyword.cloudant_short_notm}} 密碼_
  * 主機 - _這通常是您的 `username.{{site.data.keyword.cloudant_short_notm}}.com`_
  * 資料庫 - _您的 {{site.data.keyword.cloudant_short_notm}} 資料庫的名稱_

### 部署 {{site.data.keyword.cloudant_short_notm}} 範本

按一下**部署**。

部署範本之後，您可以進一步編輯程式碼，以根據需要加以自訂，或返回並查看可用範本的型錄。

## 部署上傳影像範本
{: #cos-upload-image}

「上傳影像」範本會建立一個 Web 動作，容許您透過一個小介面，將影像上傳至 {{site.data.keyword.cos_short_notm}} 儲存區。然後，範本會將影像擷取為縮圖，並將它顯示在 Web 動作的介面中。

若要部署範本，請執行下列動作：

1. 移至 [{{site.data.keyword.Bluemix_notm}} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/openwhisk/) 中的 {{site.data.keyword.openwhisk_short}} 主控台。

2. 按一下**開始建立**。

2. 按一下**快速入門範本**。

3. 按一下**上傳影像**範本。

4. 輸入套件的名稱，或使用預設名稱 `upload-image`。

5. 按**下一步**。

6. 範本需要 {{site.data.keyword.cos_full_notm}} 服務實例中的服務認證。在 **{{site.data.keyword.cos_short}}** 清單中，選取下列其中一個選項：
  * **建立新實例**：如果您沒有現有服務實例，請選取此選項來建立一個。
      1. 在開啟的 {{site.data.keyword.cos_full_notm}} 服務實例建立頁面中，建立服務實例。
      2. [建立一組 HMAC 服務認證](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials)。
      3. [建立至少一個儲存區](/docs/services/cloud-object-storage/getting-started.html#create-buckets)。
  * **輸入您自己的認證**：選取此選項，以手動輸入您自己的 {{site.data.keyword.cos_short}} 服務實例認證。認證必須具有 HMAC 金鑰，且服務實例必須至少有一個儲存區。
  * **現有實例**：如果您有任何現有 {{site.data.keyword.cos_short}} 實例，請從清單中選取其中一個實例。認證必須具有 HMAC 金鑰，且服務實例必須至少有一個儲存區。

7. 按一下**部署**。

8. 在左側導覽中，按一下**端點**

9. 在「Web 動作」區段中，複製不含 .json 字尾的鏈結，並將它貼到瀏覽器的位址列中。即會顯示範本 Web 動作的介面。

10. 選用項目：在範本部署之後，您可以導覽至「動作」儀表板，以自訂兩個新套件中的程式碼：
    * `cloud-object-storage` 套件，其包含使用 {{site.data.keyword.cos_short}} 實例的動作
    * 範本套件（預設名稱 `upload-image`），其包含 `app` 動作

## 部署取得 HTTP 資源範本
{: #get-http-resource-template}

「取得 HTTP 資源」範本會建立動作來提取外部資源、Yahoo Weather API，然後傳回資料。此動作已作為 Web 動作啟用，容許使用已啟用 CORS 的 URL 來呼叫它，且不需要鑑別金鑰，這對於建置 Web 應用程式的後端非常實用。**附註**：依預設，想要呼叫 `get-http-resource` 端點的任何人都可以公開使用它。

1. 若要建立範本，請移至 [{{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)，然後按一下**開始建立**。

2. 按一下**快速入門範本**。

3. 檢閱**套件名稱**欄位，並視需要進行更新。預設會設定為 `get-http-resource`。

4. 選擇您要擁有之動作的運行環境：Node.js 8、Node.js 6 或 Python 3。

5. 按一下**部署**。

部署範本之後，您可以進一步編輯程式碼，以根據需要加以自訂，或返回並查看可用範本的型錄。

## 部署 Hello World 範本
{: #hello-world-template}

 此動作接受單一參數，但必須是 JSON 物件。

1. 若要建立範本，請移至 [{{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)，然後按一下**開始建立**。

2. 按一下**快速入門範本**。

3. 檢閱**套件名稱**欄位，並視需要進行更新。預設會設定為 `hello-world`。

4. 選擇您要擁有之動作的運行環境：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。

5. 按一下**部署**。

部署範本之後，您可以進一步編輯程式碼，以根據需要加以自訂，或返回並查看可用範本的型錄。

## 部署 {{site.data.keyword.messagehub}} 事件範本
{: #messagehub-events-template}

「{{site.data.keyword.messagehub}} 事件」範本會建立一個動作，以及一個將開始該動作的觸發程式。每當有新項目新增至建立範本期間所選擇的 {{site.data.keyword.messagehub}} 主題時，就會發動觸發程式。

1. 若要建立範本，請移至 [{{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)，然後按一下**開始建立**。

2. 按一下**快速入門範本**。

3. 檢閱**套件名稱**欄位，並視需要進行更新。預設會設定為 `message-hub-events`。

4. 選擇您要擁有之動作的運行環境：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。

5. 按**下一步**。

### 建立 {{site.data.keyword.messagehub}} 觸發程式

觸發程式會在接收事件來源中的事件時呼叫動作。若要建立 {{site.data.keyword.messagehub}} 範本的觸發程式，請提供所需的 {{site.data.keyword.messagehub}} 服務實例資訊給觸發程式。

檢閱**觸發程式名稱**欄位，並視需要進行更新。預設會設定為 `message-hub-events-trgr`。

### 建立 {{site.data.keyword.messagehub}} 服務實例

您可以選取以下任一項：
  * **建立您自己的實例**
  * **輸入您自己的認證**

1. 在這個範例中，請選擇**建立您自己的實例**。

2. 即會開啟一個蹦現畫面，將您帶到含有 {{site.data.keyword.messagehub}} 設定頁面的新標籤。建立 {{site.data.keyword.messagehub}} 實例之後，您必須建立一組「服務認證」，然後按一下**確定**來關閉此標籤並回到這個頁面。

3. 現在，請選擇**輸入您自己的認證**，並提供下列資訊：
  * 使用者名稱 - _您的 {{site.data.keyword.messagehub}} 使用者名稱_
  * 密碼 - _您的 {{site.data.keyword.messagehub}} 密碼_
  * kafka_admin_url - _您的 {{site.data.keyword.messagehub}} 管理 REST URL_
  * 資料庫 - _您的 {{site.data.keyword.messagehub}} 資料庫的名稱_
  * 主題 - _要訂閱的主題_

### 部署 {{site.data.keyword.messagehub}} 範本

按一下**部署**。

部署範本之後，您可以進一步編輯程式碼，以根據需要加以自訂，或返回並查看可用範本的型錄。

## 部署定期 Slack 提示範本
{: #slack-reminder-template}

「定期 Slack 提示」範本會根據使用者在建立觸發程式期間所提供的間隔而張貼至 Slack。在建立此範本之前，請前往 https://api.slack.com/incoming-webhooks 來設定所需的送入 Webhook URL。

1. 檢閱**套件名稱**欄位，並視需要進行更新。預設會設定為 `periodic-slack-reminder`。

2. 選擇您要擁有之動作的運行環境：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。

3. 在**參數**區段下的**參數值**欄位中輸入 Webhook URL，然後按**下一步**（範例：https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX）。

### 建立 Slack 提示觸發程式

觸發程式會在接收事件來源中的事件時呼叫動作。若要建立「Slack 提示」範本的觸發程式，請提供所需的 {{site.data.keyword.messagehub}} 服務實例資訊的觸發程式。

1. 檢閱**觸發程式名稱**欄位，並視需要進行更新。預設會設定為 `periodic-slack-reminder-trgr`。

2. 接下來，您可以使用「型樣」或 Cron 表示式來指定觸發程式的發動間隔。您可以選取「平日」、「小時」及「分鐘」的 UTC 時間。請選取您所需的間隔選項，然後您將備妥進行範本部署。

3. 按一下**部署**。

部署範本之後，您可以進一步編輯程式碼，以根據需要加以自訂，或返回並查看可用範本的型錄。
