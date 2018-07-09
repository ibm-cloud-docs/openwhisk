---

copyright:

  years: 2018

lastupdated: "2018-05-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# 部署快速入門範本
{: #serviceauth}

{{site.data.keyword.openwhisk}} 提供一個「範本」型錄，協助您在下一個專案中快速開始。範本是「動作」、「觸發程式」、「序列」的組合，也可以從 {{site.data.keyword.Bluemix}} 中併入服務實例。使用「範本」，您可以快速且輕易地建立專案，並立即開始撰寫程式。

此指導教學會逐步引導您部署「Cloudant 範本」。
{: shortdesc}

## 可用的快速入門範本
{: #available-templates}

|名稱|說明
|支援的執行時期 |
|:-----------------|:-----------------|:-----------------|
| [Cloudant 事件](./deploy_templates.html#cloudant-template) |當 Cloudant DB 已編輯或新增文件時，請在主控台中記載該變更。 |Node.js、Swift、Python、PHP |
| [取得 HTTP 資源](./deploy_templates.html#get-http-resource-template) |為了回應 HTTP 事件而呼叫的「Web 動作」，然後從 Yahoo Weather API 提取資料。 |Node.js、Python |
| [Hello World](./deploy_templates.html#hello-world-template) |此「動作」接受單一參數，但必須是 JSON 物件。 |Node.js、Swift、Python、PHP |
| [Message Hub 事件](./deploy_templates.html#messagehub-events-template) |當「訊息中心」主題有新增資料時，請在主控台中記載該變更。 |Node.js、Swift、Python、PHP |
| [定期 Slack 提示](./deploy_templates.html#slack-reminder-template) |根據定期觸發程式而張貼至 Slack 的「動作」。|Node.js、Swift、Python、PHP |

## 部署 Cloudant 事件範本
{: #cloudant-template}

「Cloudant 範本」會建立「動作」序列，以及啟動該「序列」的「觸發程式」。當所連接的 Cloudant DB 發生變更時將會發動「觸發程式」，它應該是一個具有名稱和顏色的 cat 資料庫。預期的資料項目是 cat，其具有已定義的名稱和顏色。將新的 cat 新增至資料庫或已編輯現行的 cat 時，資料會記載至主控台。

1. 若要建立「範本」，請移至 [{{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)，然後按一下**開始建立**。

2. 按一下**快速入門範本**。

3. 按一下**新建 Cloudant 項目**。

### 建立 Cloudant 動作

1. 接下來，提供套件的名稱，或使用提供的預設名稱 `new-cloudant-item`。

2. 在**動作**下拉清單下，選取您要擁有之「動作」的運行環境（nodejs、swift、python 或 php）。在這個範例中，請選取 **nodejs**，然後按**下一步**。

### 建立 Cloudant 觸發程式

觸發程式會在他們接收來自事件來源的事件時呼叫「動作」。若要建立「Cloudant 範本」的「觸發程式」，請提供所需的 Cloudant 服務實例資訊給觸發程式。

#### 建立 Cloudant 服務實例

您可以選取以下任一項：
  * **建立您自己的實例**
  * **輸入您自己的認證**

1. 在這個範例中，請選擇**建立您自己的實例**。

2. 即會開啟一個蹦現畫面，將您帶至 Cloudant 設定頁面的新標籤。建立 Cloudant 實例之後，您必須建立一組「服務認證」，然後按一下**確定**來關閉此標籤並回到這個頁面。

3. 現在，請選擇**輸入您自己的認證**，並提供下列資訊：
  * 使用者名稱 - _您的 Cloudant 使用者名稱_
  * 密碼 - _您的 Cloudant 密碼_
  * 主機 - _這通常是您的 `username.cloudant.com`_
  * 資料庫 - _Cloudant 資料庫的名稱_

### 部署 Cloudant 範本

按一下**部署**。

在「範本」部署之後，您可以對程式碼進行進一步的編輯，依照需要來加以自訂，或返回並查看可用「範本」的型錄。

## 部署取得 HTTP 資源範本
{: #get-http-resource-template}

「取得 HTTP 資源」範本會建立「動作」來提取外部資源 (Yahoo Weather API)，然後傳回資料。「動作」是當成「Web 動作」啟用，以使用已啟用 CORS 的 URL 來呼叫它，而且不需要鑑別金鑰，這對於建置 Web 應用程式的後端十分有用。**附註**：依預設，想要呼叫 `get-http-resource` 端點的任何人都可以公開使用它。

1. 若要建立「範本」，請移至 [{{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)，然後按一下**開始建立**。

2. 按一下**快速入門範本**。

3. 檢閱**套件名稱**欄位，並視需要進行更新。預設會設定為 `get-http-resource`。

4. 選擇您要擁有之「動作」的運行環境：Node.js 8、Node.js 6 或 Python 3。

5. 按一下**部署**。

在「範本」部署之後，您可以對程式碼進行進一步的編輯，依照需要來加以自訂，或返回並查看可用「範本」的型錄。

## 部署 Hello World 範本
{: #hello-world-template}

此「動作」接受單一參數，但必須是 JSON 物件。 

1. 若要建立「範本」，請移至 [{{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)，然後按一下**開始建立**。

2. 按一下**快速入門範本**。

3. 檢閱**套件名稱**欄位，並視需要進行更新。預設會設定為 `hello-world`。

4. 選擇您要擁有之「動作」的運行環境：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。

5. 按一下**部署**。

在「範本」部署之後，您可以對程式碼進行進一步的編輯，依照需要來加以自訂，或返回並查看可用「範本」的型錄。

## 部署 Message Hub 事件範本
{: #messagehub-events-template}

「Message Hub 事件」範本會建立「動作」，以及開始該「動作」的「觸發程式」。每當在建立範本期間所選擇的 Message Hub 主題中新增項目時，就會發動「觸發程式」。

1. 若要建立「範本」，請移至 [{{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/)，然後按一下**開始建立**。

2. 按一下**快速入門範本**。

3. 檢閱**套件名稱**欄位，並視需要進行更新。預設會設定為 `message-hub-events`。

4. 選擇您要擁有之「動作」的運行環境：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。

5. 按**下一步**。

### 建立 Message Hub 觸發程式

觸發程式會在他們接收來自事件來源的事件時呼叫「動作」。若要建立 Message Hub 範本的「觸發程式」，請將所需的 Message Hub 服務實例資訊提供給「觸發程式」。

檢閱**觸發程式名稱**欄位，並視需要進行更新。預設會設定為 `message-hub-events-trgr`。

### 建立 Message Hub 服務實例

您可以選取以下任一項：
  * **建立您自己的實例**
  * **輸入您自己的認證**

1. 在這個範例中，請選擇**建立您自己的實例**。

2. 即會開啟一個蹦現畫面，將您帶至 Message Hub 設定頁面的新標籤。建立 Message Hub 實例之後，您必須建立一組「服務認證」，然後按一下**確定**來關閉此標籤並回到這個頁面。

3. 現在，請選擇**輸入您自己的認證**，並提供下列資訊：
  * 使用者名稱 - _您的 Message Hub 使用者名稱_
  * 密碼 - _您的 Message Hub 密碼_
  * kafka_admin_url - _您的 Message Hub 管理 REST URL_
  * 資料庫 - _Message Hub 資料庫的名稱_
  * 主題 - _要訂閱的主題_

### 部署 Message Hub 範本

按一下**部署**。

在「範本」部署之後，您可以對程式碼進行進一步的編輯，依照需要來加以自訂，或返回並查看可用「範本」的型錄。

## 部署定期 Slack 提示範本
{: #slack-reminder-template}

「定期 Slack 提示」範本會根據使用者在建立「觸發程式」期間所提供的間隔而張貼至 Slack。在建立此「範本」之前，請前往 https://api.slack.com/incoming-webhooks 以設定所需的送入 Webhook URL。

1. 檢閱**套件名稱**欄位，並視需要進行更新。預設會設定為 `periodic-slack-reminder`。

2. 選擇您要擁有之「動作」的運行環境：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。

3. 在**參數**區段下的**參數值**欄位中輸入 Webhook URL，然後按**下一步**（範例：https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX）。

### 建立 Slack 提示觸發程式

觸發程式會在他們接收來自事件來源的事件時呼叫「動作」。若要建立「Slack 提示」範本的「觸發程式」，請將所需的 Message Hub 服務實例資訊提供給「觸發程式」。

1. 檢閱**觸發程式名稱**欄位，並視需要進行更新。預設會設定為 `periodic-slack-reminder-trgr`。

2. 接下來，您可以使用「型樣」或 Cron 表示式來指定「觸發程式」的發動間隔。您可以選取「平日」、「小時」及「分鐘」的 UTC 時間。請選取您所需的間隔選項，然後您將備妥進行範本部署。

3. 按一下**部署**。

在「範本」部署之後，您可以對程式碼進行進一步的編輯，依照需要來加以自訂，或返回並查看可用「範本」的型錄。
