---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 平台架構
{: #openwhisk_about}

{{site.data.keyword.openwhisk}} 是事件驅動運算平台（也稱為「無伺服器運算」或「函數即服務 (FaaS)」），會執行程式碼來回應事件或直接呼叫。
{: shortdesc}

## {{site.data.keyword.openwhisk_short}} 技術
{: #technology}

瞭解 {{site.data.keyword.openwhisk_short}} 基礎技術的一些基本概念：

<dl>
<dt>動作 (Action)</dt>
<dd>[動作](openwhisk_actions.html)是執行某個特定作業的一段程式碼。您可以使用所選擇的語言來撰寫動作，例如 JavaScript 或 Swift 程式碼的小型 Snippet，或者內嵌在 Docker 容器中的自訂二進位程式碼。您可以將動作以原始碼或 Docker 映像檔形式提供給 Cloud Functions。<br><br>使用 {{site.data.keyword.openwhisk_short}} API、CLI 或 iOS SDK 直接呼叫動作時，動作會執行工作。動作也可以使用觸發程式，自動回應來自 {{site.data.keyword.Bluemix_notm}} 服務及協力廠商服務的事件。</dd>
<dt>序列 (Sequence)</dt>
<dd>可以一起鏈結至[序列](openwhisk_actions.html#openwhisk_create_action_sequence)的一組動作，而不需要撰寫任何程式碼。序列是一串按順序呼叫的動作，其中某個動作的輸出會傳遞為下一個動作的輸入。這可讓您合併現有動作，以快速且輕鬆地重複使用。然後，可以透過 REST API 或自動回應事件，像動作一樣地呼叫序列。
</dd>
<dt>事件 (Event)</dt>
<dd>事件範例包括資料庫記錄變更、超出特定溫度的 IoT 感應器讀數、GitHub 儲存庫的新程式碼確定，或來自 Web 或行動應用程式的簡單 HTTP 要求。來自外部及內部事件來源的事件是透過觸發程式進行傳送，而規則容許動作反應這些事件。</dd>
<dt>觸發程式 (Trigger)</dt>
<dd>[觸發程式](openwhisk_triggers_rules.html#openwhisk_triggers_create)是某類別事件的具名頻道。觸發程式是您要反應特定類型事件的宣告（不論是由使用者還是透過事件來源）。</dd>
<dt>規則 (Rule)</dt>
<dd>[規則](openwhisk_triggers_rules.html#openwhisk_rules_use)會建立觸發程式與動作的關聯。每次發動觸發程式時，規則都會呼叫相關聯的動作。運用一組適當的規則，單一觸發程式事件可能會呼叫多個動作，也可能會呼叫動作以作為多個觸發程式的事件的回應。</dd>
<dt>套件</dt>
<dd>可以利用套件來新增與服務及事件提供者的整合。[套件](openwhisk_packages.html)是資訊來源與動作的組合。資訊來源是一段程式碼，可配置外部事件來源來發動觸發程式事件。例如，使用 {{site.data.keyword.cloudant}} 變更資訊來源所建立的觸發程式，會將服務配置成在每次修改文件或將其新增至 {{site.data.keyword.cloudant_short_notm}} 資料庫時發動觸發程式。套件中的動作代表服務提供者可設為可用的可重複使用邏輯，如此，開發人員可以使用服務作為事件來源，以及呼叫該服務的 API。<br><br>現有套件型錄提供快速的方式以使用有用的功能來加強應用程式，以及在生態系統中存取外部服務。具有 {{site.data.keyword.openwhisk_short}} 套件的外部服務範例包括 {{site.data.keyword.cloudant_short_notm}}、The Weather Company、Slack 及 GitHub。</dd>
</dl>

## {{site.data.keyword.openwhisk_short}} 的運作方式
{: #openwhisk_how}

若要更詳細地說明所有元件，請透過 {{site.data.keyword.openwhisk_short}} 系統追蹤動作的呼叫。呼叫會執行使用者已提供給系統的程式碼，並傳回該執行的結果。下圖顯示高階 {{site.data.keyword.openwhisk_short}} 架構。


![{{site.data.keyword.openwhisk_short}} 架構](./images/OpenWhisk.png)


## OpenWhisk 內部處理的運作方式
{: #openwhisk_internal}

OpenWhisk 中幕後發生什麼情況？

OpenWhisk 是一個開放程式碼專案，結合 Nginx、Kafka、Docker 及 CouchDB 這類元件，以形成無伺服器事件型程式設計服務。

<img src="images/OpenWhisk_flow_of_processing.png" width="550" alt="OpenWhisk 幕後的內部處理流程" style="width:550px; border-style: none"/>

### 進入系統：nginx

首先，OpenWhisk 的使用者應對 API 完全是以 HTTP 為基礎，並且遵循 RESTful 設計。因此，透過 CLI 傳送的指令，就是對 OpenWhisk 系統提出的 HTTP 要求。特定指令大致上會轉換為：
```
POST /api/v1/namespaces/$userNamespace/actions/myAction
Host: $openwhiskEndpoint
```
{: screen}

請記下這裡的 *$userNamespace* 變數。使用者可以存取至少一個名稱空間。為求簡化，假設使用者擁有放入 *myAction* 的名稱空間。

系統的第一個進入點是透過 **nginx**（HTTP 及反向 Proxy 伺服器）。它用於 SSL 終止，並且將適當的 HTTP 呼叫轉遞給下一個元件。

### 進入系統：控制器

Nginx 將 HTTP 要求轉遞至**控制器**，透過 OpenWhisk，此為路徑上的下一個元件。它是實際 REST API 的 Scala 型實作（根據 **Akka** 及 **Spray**），因此可作為使用者執行一切作業的介面。包括 OpenWhisk 中實體的 [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) 要求，以及動作的呼叫。

「控制器」會先釐清使用者嘗試執行的動作。作法是根據您在 HTTP 要求中使用的 HTTP 方法。根據上述轉換，使用者會對現有的動作發出 POST 要求，而「控制器」會將其轉換為**動作的呼叫**。

由於「控制器」的中央角色（因此得名），下列步驟全都會有一定程度的相關性。

### 鑑別及授權：CouchDB

現在，「控制器」會驗證您的身分（*鑑別*），並驗證您是否具有專用權可以執行您想要對該實體執行的動作（*授權*）。針對 **CouchDB** 實例中所謂的 **subjects** 資料庫驗證要求中所含的認證。

在此情況下，會確認使用者存在於 OpenWhisk 的資料庫中，並且具有專用權可以呼叫 *myAction* 動作（假設此動作是使用者所擁有的名稱空間中的動作）。後者有效率地向使用者提供呼叫動作的專用權。

一切就緒後，就會開啟下一個處理階段的閘道。

### 正在重新取得動作：CouchDB…

因為「控制器」現在確定使用者已被容許加入並且具有呼叫動作的專用權，所以會從 CouchDB 的 **whisks** 資料庫中載入此動作（在此情況下為 *myAction*）。

動作的記錄主要包含要執行的程式碼，以及您要傳遞給動作並與實際呼叫要求中所含參數合併的預設參數。同時包含執行時強制實施的資源限制（例如允許它使用的記憶體）。

在此特定情況下，動作不會採用任何參數（函數的參數定義是空清單）。因此，假設尚未設定預設參數，包括動作的特定參數，從此觀點來看，這是最簡單的情況。


### 呼叫動作的對象：負載平衡器

「負載平衡器」是「控制器」的一部分，透過持續檢查其性能狀態，可以具有系統中可用執行程式的廣域視圖。這些執行程式稱為**呼叫程式**。「負載平衡器」知道哪些「呼叫程式」可供使用，因此會選擇其中一個來呼叫所要求的動作。

### 請排隊：Kafka

從現在開始，您傳入的呼叫要求主要可能會發生兩件不好的事情：

1. 系統可能當機，遺失呼叫。
2. 系統可能有這類大量載入，因此呼叫需要先等待其他呼叫完成。

這兩者的解答都是 **Kafka**，它是一種高產量的分散式發佈/訂閱傳訊系統。「控制器」及「呼叫程式」只能透過 Kafka 所緩衝及持續保存的訊息進行通訊。Kafka 會增加「控制器」及「呼叫程式」記憶體中的緩衝負擔，導致發生 *OutOfMemoryException* 的風險，同時確保訊息不會在系統當機時遺失。

若要呼叫動作，「控制器」會將訊息發佈至 Kafka，其包含要呼叫的動作以及要傳遞給該動作的參數（在此情況下沒有參數）。此訊息會定址到「呼叫程式」，其為從 Consul 取得的清單中所選擇的「控制器」。

Kafka 確認取得訊息之後，會使用 **ActivationId** 回應對使用者的 HTTP 要求。使用者可以稍後使用該項目，來存取此特定呼叫的結果。這是非同步呼叫模型，其中，HTTP 要求會在系統接受呼叫動作的要求之後終止。可以使用同步模型（稱為區塊處理呼叫），但這裡並未涵蓋。

### 正在呼叫程式碼：呼叫程式

**呼叫程式**是 OpenWhisk 的核心。「呼叫程式」的責任是呼叫動作。它也會在 Scala 中進行實作。但還需要進行許多作業。若要使用隔離及安全的方式執行動作，請使用 **Docker**。

針對我們以快速、隔離及控制方式所呼叫的每一個動作，使用 Docker 可以設定新的自行封裝環境（稱為*容器*）。對於每一個動作呼叫，會大量產生 Docker 容器，並注入動作碼。然後，透過使用傳遞給它的參數來執行程式碼，取得結果，並銷毀容器。可以在此階段完成效能最佳化，以減少額外負擔，並盡可能減少回應時間。

在此情況下，如果已有 *Node.js* 型動作，「呼叫程式」即會啟動一個 Node.js 容器。然後，從 *myAction* 注入程式碼、在不使用參數的情況下予以執行、擷取結果、儲存日誌，並且再次銷毀 Node.js 容器。

### 重新儲存結果：CouchDB

「呼叫程式」取得結果時，會將它儲存至 **whisks** 資料庫，在 ActivationId 下作為啟動。**whisks** 資料庫存在於 **CouchDB** 中。

在此特定情況下，「呼叫程式」會從動作取回產生的 JSON 物件、抓取 Docker 所撰寫的日誌、將它們全都放入啟動記錄，並將其儲存至資料庫。請參閱下列範例：
```json
{
   "activationId": "31809ddca6f64cfc9de2937ebd44fbb9",
   "response": {
       "statusCode": 0,
       "result": {
           "hello": "world"
       }
   },
   "end": 1474459415621,
   "logs": [
       "2016-09-21T12:03:35.619234386Z stdout: Hello World"
   ],
   "start": 1474459415595,
}
```
{: codeblock}

請注意記錄如何包含傳回的結果及撰寫的日誌。它也包含呼叫動作的開始及結束時間。啟動記錄包含更多欄位，但在此範例中為了簡化而除去並未列出。

現在，您可以重新使用 REST API（從步驟 1 重新開始）以取得您的啟動，進而取得動作的結果。若要這樣做，請執行下列指令：

```bash
ibmcloud wsk activation get 31809ddca6f64cfc9de2937ebd44fbb9
```
{: pre}

### 摘要

您可以瞭解簡單的 **ibmcloud wsk action invoked myAction** 如何通過 {{site.data.keyword.openwhisk_short}} 系統的不同階段。系統本身主要僅包含兩個自訂元件：**控制器**及**呼叫程式**。由開放程式碼社群中的許多人員所開發的其他所有項目也都已就緒。

您可以在下列主題中尋找 {{site.data.keyword.openwhisk_short}} 的其他資訊：

* [實體名稱](./openwhisk_reference.html#openwhisk_entities)
* [動作語意](./openwhisk_reference.html#openwhisk_semantics)
* [限制](./openwhisk_reference.html#openwhisk_syslimits)
* [REST API 參考資料](https://console.bluemix.net/apidocs/98-cloud-functions?&language=node#introduction)
