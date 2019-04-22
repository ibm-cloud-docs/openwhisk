---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: feed, event, polling, hooks, trigger, 

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 建立自訂事件提供者資訊來源
{: #openwhisk_feeds}

{{site.data.keyword.openwhisk_short}} 支援開放式 API，任何使用者都可以在其中將事件生產者服務公開為套件中的資訊來源。下節說明架構，以及用於提供專屬自訂資訊來源的實作選項。
{: shortdesc}

本資料適用於要發佈其專屬資訊來源的進階 {{site.data.keyword.openwhisk_short}} 使用者。大部分的 {{site.data.keyword.openwhisk_short}} 使用者都可以放心地跳過下列架構小節。

## 資訊來源架構

至少有三個架構型樣可用於建立資訊來源：**連結鉤**、**輪詢**及**連線**。

### 連結鉤
在*連結鉤* 型樣中，使用另一個服務所公開的 [Webhook](https://en.wikipedia.org/wiki/Webhook) 機能來設定資訊來源。在此策略中，於外部服務上配置 Webhook，以直接 POST 至 URL 來發動觸發程式。到目前為止，對於實作低頻率資訊來源，這個方法是最簡單且最具吸引力的選項。



### 輪詢
在「輪詢」型樣中，安排 {{site.data.keyword.openwhisk_short}} 動作定期輪詢端點，以提取新的資料。此型樣相當容易建置，但事件頻率受限於輪詢間隔。

### 連線
在「連線」型樣中，個別服務會維護連至資訊饋送來源的持續性連線。連線型實作可能會使用長時間輪詢間隔來與服務端點互動，或設定推送通知。





## 資訊來源與觸發程式的差異

資訊來源及觸發程式密切相關，但技術上而言是不同的概念。   

- {{site.data.keyword.openwhisk_short}} 會處理流入系統的**事件**。

- **觸發程式**技術上而言是某個事件類別的名稱。每一個事件只屬於一個觸發程式；觸發程式則類似主題型發佈/訂閱系統中的*主題*。**規則** *T -> A* 表示只要觸發程式 *T* 的事件到達，就會使用觸發程式有效負載來呼叫動作 *A*。

- **資訊來源**是全部屬於某個觸發程式 *T* 的一連串事件。資訊來源是透過**資訊來源動作**所控制的，而資訊來源動作會處理建立、刪除、暫停及回復可構成資訊來源的一連串事件。資訊來源動作一般會使用管理通知的 REST API，與產生事件的外部服務互動。

##  實作資訊來源動作

*資訊來源動作* 是一般 {{site.data.keyword.openwhisk_short}} *動作*，接受下列參數：
* **lifecycleEvent**：'CREATE'、'READ'、'UPDATE'、'DELETE'、'PAUSE' 或 'UNPAUSE' 其中之一。
* **triggerName**：觸發程式的完整名稱，其中包含從此資訊來源產生的事件。
* **authKey**：擁有觸發程式之 {{site.data.keyword.openwhisk_short}} 使用者的「基本」鑑別認證。

資訊來源動作也可以接受任何管理資訊來源所需的其他參數。例如，{{site.data.keyword.cloudant}} changes 資訊來源動作預期接收的參數包括 *'dbname'*、*'username'* 等等。

使用者從 CLI 使用 **--feed** 參數建立觸發程式時，系統會使用適當的參數自動呼叫資訊來源動作。

例如，假設使用者利用使用者名稱及密碼作為已連結參數，來建立 **cloudant** 套件的 `mycloudant` 連結。如果使用者從 CLI 發出下列指令：
```
ibmcloud fn trigger create T --feed mycloudant/changes -p dbName myTable
```
{: pre}

然後，在涵蓋系統的情況下，與下列指令相等的事項如下：
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

名為 *changes* 的資訊來源動作會採用這些參數，且預期會執行任何必要動作，以從 {{site.data.keyword.cloudant_short_notm}} 設定一連串事件。資訊來源動作是使用適當的配置進行，其會導向觸發程式 *T*。

針對 {{site.data.keyword.cloudant_short_notm}} *changes* 資訊來源，動作會直接與使用連線型架構所實作的 *{{site.data.keyword.cloudant_short_notm}} 觸發程式* 服務交談。

`ibmcloud fn trigger delete`、`ibmcloud fn trigger update` 及 `ibmcloud fn trigger get` 會執行類似的資訊來源動作通訊協定。

## 使用連結鉤實作資訊來源

如果事件生產者支援 Webhook/回呼機能，則使用連結鉤可以輕鬆地設定資訊來源。

運用此方法，_不需要_ 在 {{site.data.keyword.openwhisk_short}} 外部使用任何持續性服務。所有資訊來源管理都會透過無狀態 {{site.data.keyword.openwhisk_short}} *資訊來源動作* 自然進行，而這些資訊來源動作會直接與協力廠商的 Webhook API 進行協議。

使用 `CREATE` 呼叫時，資訊來源動作只會安裝某個其他服務的 Webhook，並要求遠端服務將通知 POST 至 {{site.data.keyword.openwhisk_short}} 中的適當 `fireTrigger` URL。

指示 Webhook 將通知傳送至 URL，例如：

`POST /namespaces/{namespace}/triggers/{triggerName}`

具有 POST 要求的表單會解譯為 JSON 文件，該文件會在觸發程式事件上定義參數。{{site.data.keyword.openwhisk_short}} 規則會將這些觸發程式參數傳遞至因該事件而發動的任何動作。

## 使用輪詢實作資訊來源

可以在 {{site.data.keyword.openwhisk_short}} 內設定 {{site.data.keyword.openwhisk_short}} *動作* 來完整輪詢資訊來源，而不需要使用任何持續性連線或外部服務。

針對無法使用 Webhook 但不需要高容量或低延遲回應時間的資訊來源，輪詢是具吸引力的選項。

為了設定輪詢型資訊來源，資訊來源動作會在呼叫 `CREATE` 時採取下列步驟：

1. 資訊來源動作會使用 `whisk.system/alarms` 資訊來源，以所要的頻率設定定期觸發程式 (*T*)。
2. 資訊來源開發人員會建立 `pollMyService` 動作，此動作會輪詢遠端服務並傳回任何新事件。
3. 資訊來源動作會設定*規則* *T -> pollMyService*。

此程序會使用 {{site.data.keyword.openwhisk_short}} 動作完整實作輪詢型觸發程式，而不需要使用個別服務。

## 使用連線實作資訊來源

前兩個架構選項十分簡單且容易實作。不過，如果您想要有高效能資訊來源，則最好的選擇是持續性連線及長期輪詢或類似技術。

因為 {{site.data.keyword.openwhisk_short}} 動作必須是短時間執行的，所以動作無法維護與協力廠商的持續性連線。您可以改為在一直執行的 {{site.data.keyword.openwhisk_short}} 外部，使用一個稱為*提供者服務* 的個別服務。提供者服務可以維護支援長時間輪詢或其他連線型通知的協力廠商事件來源連線。

提供者服務有一個 REST API，其容許 {{site.data.keyword.openwhisk_short}} *資訊來源動作* 控制資訊來源。提供者服務會作為事件提供者與 {{site.data.keyword.openwhisk_short}} 之間的 Proxy。當它收到來自協力廠商的事件時，會透過發動觸發程式將它們傳送至 {{site.data.keyword.openwhisk_short}}。

{{site.data.keyword.cloudant_short_notm}} *changes* 資訊來源是標準範例，因為它是一個 `cloudanttrigger` 服務，在持續性連線的 {{site.data.keyword.cloudant_short_notm}} 通知與 {{site.data.keyword.openwhisk_short}} 觸發程式之間調解。


*警示* 資訊來源是使用類似的型樣進行實作。

連線型架構是最高效能選項，但與輪詢及連結鉤架構相較之下，會增加更多的作業額外負擔。
