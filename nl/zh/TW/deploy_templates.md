---

copyright:

  years: 2018

lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# 在 {{site.data.keyword.openwhisk_short}} 中部署範本
{: #serviceauth}

{{site.data.keyword.openwhisk}} 提供一個「範本」型錄，協助您在下一個專案中快速開始。範本是「動作」、「觸發程式」、「序列」的組合，也可以從 {{site.data.keyword.Bluemix}} 中併入服務實例。使用「範本」，您可以快速且輕易地建立專案，並立即開始撰寫程式。 

此指導教學會逐步引導您部署「Cloudant 範本」。
{: shortdesc}

## 可用的範本
{: #available-templates}

| 名稱| 說明
| 支援的執行時期 | 
|:-----------------|:-----------------|:-----------------|
| Cloudant 事件 | 當 Cloudant DB 已編輯或新增文件時，請在主控台中記載該變更。 | Node.js、Swift、Python、PHP |
| 取得 HTTP 資源 | 為了回應 HTTP 事件會呼叫 Web 動作，然後從 Yahoo Weather API 提取資料。 | Node.js、Python |
| Hello World | 此動作接受單一參數，但必須是 JSON 物件。 | Node.js、Swift、Python、PHP |
| 訊息中心事件 | 當「訊息中心」主題有新增資料時，請在主控台中記載該變更。 | Node.js、Swift、Python、PHP | 
| 定期 Slack 提示 | 定期觸發並公佈在 Slack 上的動作。 | Node.js、Swift、Python、PHP |

## 部署 Cloudant 事件範本
{: #cloudant-template}

「Cloudant 範本」會建立「動作」序列，以及啟動該「序列」的「觸發程式」。當所連接的 Cloudant DB 發生變更時將會發動「觸發程式」，它應該是一個具有名稱和顏色的 cat 資料庫。預期的資料項目是 cat，其具有已定義的名稱和顏色。將新的 cat 新增至資料庫或已編輯現行的 cat 時，資料會記載至主控台。

1. 若要建立「範本」，請移至 [{{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.openwhisk_short}}](https://dev-console.stage1.bluemix.net/openwhisk/)，然後按一下**開始建立**。 

2. 按一下**部署範本**。

3. 按一下**新建 Cloudant 項目**。

### 建立 Cloudant 動作

1. 接下來，提供套件的名稱，或使用提供的預設名稱 `new-cloudant-item`。 

2. 在**動作**下拉清單之下，選取您要擁有之動作的執行時期（nodejs、swift、python 或 php）。在這個範例中，請選取 **nodejs**，然後按**下一步**。

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

1. 按一下**部署**。

在「範本」部署之後，您可以對程式碼進行進一步的編輯，依照需要來加以自訂，或返回並查看可用「範本」的型錄。

