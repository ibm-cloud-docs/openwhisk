---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: feeds, serverless

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


# 建立自訂事件提供者資訊來源
{: #feeds_custom}

{{site.data.keyword.openwhisk_short}} 支援開放式 API，任何使用者都可以在其中將事件生產者服務公開為套件中的資訊來源。
{: shortdesc}


## 資訊來源架構
{: #feeds_arch}

有 3 種架構型樣可用於建立資訊來源：**連結鉤**、**輪詢**和**連線**。

### 連結鉤

在連結鉤型樣下，使用由其他服務公開的 [Webhook](https://en.wikipedia.org/wiki/Webhook) 來設定資訊來源。在此策略中，於外部服務上配置 Webhook，以直接 POST 至 URL 來發動觸發程式。到目前為止，對於實作低頻率資訊來源，這個方法是最簡單且最具吸引力的選項。

例如，[Github 套件](/docs/openwhisk?topic=cloud-functions-pkg_github)和 [Push Notification 套件](/docs/openwhisk?topic=cloud-functions-pkg_push_notifications)使用 Webhook。


### 輪詢

使用「輪詢」型樣時，{{site.data.keyword.openwhisk_short}} 動作安排為定期輪詢端點以提取新資料。此型樣相當容易建置，但事件頻率受限於輪詢間隔。

### 連線

使用「連線」型樣時，個別的服務用於保持與來源資訊來源的持續連線。以連線為基礎的實作可能會使用長輪詢間隔來與服務端點互動，或設定推送通知。

例如，[{{site.data.keyword.cloudant}} 套件](/docs/openwhisk?topic=cloud-functions-pkg_cloudant)會使用「連線」型樣。



##  實作資訊來源動作
{: #feeds_actions}

資訊來源動作是接受下列參數的一種動作：
* **lifecycleEvent**： "CREATE" 、 "READ" 、 "UPDATE" 、 "DELETE" 、 "PAUSE" 或 "UNPAUSE" 。
* **triggerName**：觸發程式的完整名稱，其中包含從此資訊來源產生的事件。
* **authKey**：擁有觸發程式的 {{site.data.keyword.openwhisk_short}} 使用者的基本鑑別認證。

資訊來源動作也可以接受任何管理資訊來源所需的其他參數。例如，{{site.data.keyword.cloudant}} changes 資訊來源動作預期接收多個參數，包括 `dbname` 和 `username`。

使用者使用 `--feed` 參數透過 CLI 建立觸發程式時，系統會自動使用相應參數呼叫資訊來源動作。

例如，使用者透過將其使用者名稱及密碼作為連結參數，為 **cloudant** 套件建立了 `mycloudant` 連結。如果使用者從 CLI 發出下列指令：
```
ibmcloud fn trigger create my_cloudant_trigger --feed mycloudant/changes -p dbName myTable
```
{: pre}

然後，將執行同等於下列指令的內容：
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

名為 *changes* 的資訊來源動作會採用這些參數，且預期會執行任何必要動作，以從 {{site.data.keyword.cloudant_short_notm}} 設定一連串事件。資訊來源動作是使用適當的配置來執行的，此配置將導向至觸發程式。

對於 {{site.data.keyword.cloudant_short_notm}} *changes* 資訊來源，該動作與透過以連線為基礎之架構實作的 *{{site.data.keyword.cloudant_short_notm}} 觸發程式*服務直接對話。

`ibmcloud fn trigger delete`、`ibmcloud fn trigger update` 及 `ibmcloud fn trigger get` 會執行類似的資訊來源動作通訊協定。

## 透過連結鉤實作資訊來源
{: #feeds_hooks}

事件生產者支援 Webhook/回呼機能時，可透過連結鉤設定資訊來源。

透過此方法，無需在 {{site.data.keyword.openwhisk_short}} 外部維持任何持續性服務。所有資訊來源管理都會透過無狀態 {{site.data.keyword.openwhisk_short}} *資訊來源動作* 自然進行，而這些資訊來源動作會直接與協力廠商的 Webhook API 進行協議。

使用 `CREATE` 呼叫時，資訊來源動作只會安裝某個其他服務的 Webhook，並要求遠端服務將通知 POST 至 {{site.data.keyword.openwhisk_short}} 中的適當 `fireTrigger` URL。

指示 Webhook 將通知傳送至 URL，例如：

`POST /namespaces/{namespace}/triggers/{triggerName}`

具有 POST 要求的表單會解譯為 JSON 文件，該文件會在觸發程式事件上定義參數。{{site.data.keyword.openwhisk_short}} 規則會將這些觸發程式參數傳遞至因該事件而發動的任何動作。

## 透過輪詢實作資訊來源
{: #feeds_polling}

可以設定動作以完全在 {{site.data.keyword.openwhisk_short}} 中輪詢來源資訊來源，而無需維持任何持續連線或外部服務。

對於資訊來源，如果其中的 Webhook 無法使用，但您並不需要很大的容量或延遲的回應時間短，還是可以使用輪詢。

為了設定輪詢型資訊來源，資訊來源動作會在呼叫 `CREATE` 時採取下列步驟：

1. 資訊來源動作使用 `whisk.system/alarms` 資訊來源將定期觸發程式設定為所需頻率。
2. 資訊來源開發人員會建立 `pollMyService` 動作，此動作會輪詢遠端服務並傳回任何新事件。
3. 資訊來源動作會設定*規則* *T -> pollMyService*。

此程序會使用 {{site.data.keyword.openwhisk_short}} 動作完整實作輪詢型觸發程式，而不需要使用個別服務。

## 使用連線實作資訊來源
{: #feeds_connections}

前兩個架構選項十分簡單且容易實作。不過，如果您想要有高效能資訊來源，則最好的選擇是持續性連線及長期輪詢或類似技術。

因為 {{site.data.keyword.openwhisk_short}} 動作必須是短時間執行的，所以動作無法維護與協力廠商的持續性連線。您可以改為在一直執行的 {{site.data.keyword.openwhisk_short}} 外部，使用一個稱為*提供者服務* 的個別服務。提供者服務可以維護支援長時間輪詢或其他連線型通知的協力廠商事件來源連線。

提供者服務有一個 REST API，其容許 {{site.data.keyword.openwhisk_short}} *資訊來源動作* 控制資訊來源。提供者服務會作為事件提供者與 {{site.data.keyword.openwhisk_short}} 之間的 Proxy。當它收到來自協力廠商的事件時，會透過發動觸發程式將它們傳送至 {{site.data.keyword.openwhisk_short}}。

{{site.data.keyword.cloudant_short_notm}} *changes* 資訊來源是標準範例，因為它是一個 `cloudanttrigger` 服務，在持續性連線的 {{site.data.keyword.cloudant_short_notm}} 通知與 {{site.data.keyword.openwhisk_short}} 觸發程式之間調解。


*警示* 資訊來源是使用類似的型樣進行實作。

連線型架構是最高效能選項，但與輪詢及連結鉤架構相較之下，會增加更多的作業額外負擔。


