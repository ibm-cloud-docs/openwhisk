---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 行動推送
{: #openwhisk_catalog_pushnotifications}

瞭解如何建立 Push 套件連結，並使用 `/whisk.system/pushnotifications` 套件傳送簡單推送通知，而此套件可讓您使用推送服務。
{: shortdesc}

該套件包括下列動作及「資訊來源」：

|實體|類型|參數|說明
|
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` |套件| appId、appSecret、admin_url |使用 Push 服務。|
|`/whisk.system/pushnotifications/sendMessage` |動作|text、url、deviceIds、platforms、userIds、tagNames、gcmCollapseKey、gcmCategory、gcmIcon、gcmDelayWhileIdle、gcmSync、gcmVisibility、gcmPayload、gcmPriority、gcmSound、gcmTimeToLive、gcmStyleType、gcmStyleTitle、gcmStyleUrl、gcmStyleText、gcmStyleLines、gcmLightsLedArgb、gcmLightsLedOnMs、gcmLightsLedOffMs、apnsBadge、apnsCategory、apnsIosActionKey、apnsPayload、apnsType、apnsSound、apnsTitleLocKey、apnsLocKey、apnsLaunchImage、apnsTitleLocArgs、apnsLocArgs、apnstitle、apnsSubtitle、apnsAttachmentUrl、fireFoxTitle、fireFoxIconUrl、fireFoxTimeToLive、fireFoxPayload、safariTitle、safariUrlArgs、safariAction、chromeTitle、chromeIconUrl、chromeTimeToLive、chromePayload、chromeAppExtTitle、chromeAppExtCollapseKey、chromeAppExtDelayWhileIdle、chromeAppExtIconUrl、chromeAppExtTimeToLive、chromeAppExtPayload|將推送通知傳送至一個以上的指定裝置。|
|`/whisk.system/pushnotifications/webhook` |資訊來源|事件|在 Push 服務上產生裝置活動（裝置登錄、取消登錄、訂閱或取消訂閱）時發動觸發程式事件。|

如需在有裝置活動時發動「觸發程式」事件的相關資訊，請參閱[裝置事件上的行動推送](./openwhisk_pushnotifications.html)主題。

## 建立 Push 套件連結
{: #create_push_binding}

若要建立 Push Notification 套件連結，您必須指定下列參數：

-  **appId**：{{site.data.keyword.Bluemix}} **應用程式 GUID**。
-  **appSecret**：{{site.data.keyword.Bluemix_notm}} Push Notification 服務**應用程式密碼**。

若要建立套件連結，請參閱下列步驟：

1. 在 [{{site.data.keyword.Bluemix_notm}} 儀表板](http://console.bluemix.net)中建立 {{site.data.keyword.Bluemix_notm}} 應用程式。

2. 起始設定「Push Notification 服務」，並將服務連結至 {{site.data.keyword.Bluemix_notm}} 應用程式。

3. 配置 [Push Notification 應用程式](https://console.bluemix.net/docs/services/mobilepush/index.html)。

  請務必記住您建立之 {{site.data.keyword.Bluemix_notm}} 應用程式的 **App GUID** 和 **App Secret**。

4. 建立與 `/whisk.system/pushnotifications` 連結的套件。
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myPush -p appId myAppID -p appSecret myAppSecret
  ```
  {: pre}

5. 驗證套件連結已存在：
  ```
  ibmcloud fn package list
  ```
  {: pre}

  輸出範例：
  ```
  packages
  /myNamespace/myPush private binding
  ```
  {: screen}

## Push Notification 參數
{: #push_parameters}

`/whisk.system/pushnotifications/sendMessage` 動作會將推送通知傳送至已登錄的裝置。參數如下所示：
- `text`：要對使用者顯示的通知訊息。例如，`-p text "Hi, OpenWhisk send a notification"`。
- `url`：可以隨警示一起傳送的 URL。例如，`-p url "https:\\www.w3.ibm.com"`。
- `apiHost`：指定 API 主機的選用字串。預設值為 `mobile.ng.bluemix.net`。例如：`-p apiHost "mobile.eu-gb.bluemix.net"`
- `deviceIds`：指定裝置的清單。例如，`-p deviceIds ["deviceID1"]`。
- `platforms`：將通知傳送至指定平台的裝置。'A' 表示 Apple (iOS) 裝置及 'G' 表示 Google (Android) 裝置。例如，`-p platforms ["A"]`。
- `userIds`：將通知傳送至指定使用者的裝置。例如，`-p userIds "[\"testUser\"]"`
- `tagNames`：將通知傳送至已訂閱其中任何標籤的裝置。例如，`-p tagNames "[\"tag1\"]"`。
- `gcmCollapseKey`：此參數可識別訊息的群組。
- `gcmCategory`：要用於互動式推送通知的種類 ID。
- `gcmIcon`：指定要針對通知顯示的圖示名稱。請確定圖示已與用戶端應用程式包裝在一起。
- `gcmDelayWhileIdle`：當此參數設為 true 時，會傳送訊息，直到裝置變成作用中為止。
- `gcmSync`：裝置群組傳訊可讓群組中的每個應用程式實例反映最新傳訊狀態。
- `gcmVisibility`：private/public - 此通知的可見性，其影響在安全鎖定的畫面上顯示通知的時間和方式。
- `gcmPayload`：放在通知訊息中一起傳送的自訂 JSON 有效負載。例如，`-p gcmPayload "{\"hi\":\"hello\"}"`
- `gcmPriority`：設定訊息的優先順序。
- `gcmSound`：當通知到達裝置時播放的音效檔（裝置上）。
- `gcmTimeToLive`：此參數指定當裝置離線時，訊息保留在 GCM 儲存空間中的時間長度（以秒為單位）。
- `gcmStyleType`：指定可擴充通知的類型。可能值為 `bigtext_notification`、`picture_notification` 或 `inbox_notification`。
- `gcmStyleTitle`：指定通知的標題。擴充通知時，會顯示標題。所有這三個可擴充的通知都必須指定標題。
- `gcmStyleUrl`：必須從中取得通知圖片的 URL。必須針對 `picture_notification` 指定。
- `gcmStyleText`：需要在擴充 `bigtext_notification` 時顯示的大型文字。必須針對 `bigtext_notification` 指定。
- `gcmStyleLines`：要顯示在 `inbox_notification` 的收件匣樣式中的字串陣列。必須針對 `inbox_notification` 指定。
- `gcmLightsLedArgb`：LED 的顏色。硬體會達成其最佳近似值。
- `gcmLightsLedOnMs`：LED 在閃爍時要開啟的毫秒數。硬體會達成其最佳近似值。
- `gcmLightsLedOffMs`：LED 在閃爍時要關閉的毫秒數。硬體會達成其最佳近似值。
- `apnsBadge`：要顯示成應用程式圖示徽章的數字。
- `apnsCategory`：要用於互動式推送通知的種類 ID。
- `apnsIosActionKey`：動作鍵的標題。
- `apnsPayload`：放在通知訊息中一起傳送的自訂 JSON 有效負載。
- `apnsType`：['DEFAULT'、'MIXED'、'SILENT']。
- `apnsSound`：應用程式組合中的音效檔名稱。將會播放此檔案的音效作為警示。
- `apnsTitleLocKey`：現行本地化之 `Localizable.strings` 檔案中某個標題字串的索引鍵。索引鍵字串可以使用 %@ 和 %n$@ 指定元來格式化，以採用 `titleLocArgs` 陣列中指定的變數。
- `apnsLocKey`：在 `Localizable.strings` 檔案中，針對現行本地化（由使用者的語言喜好設定來設定）的某個索引鍵指向 alert-message 字串。索引鍵字串可以使用 %@ 和 %n$@ 指定元來格式化，以採用 locArgs 陣列中指定的變數。
- `apnsLaunchImage`：應用程式軟體組中影像檔的檔名，不論有沒有副檔名。使用者點選動作按鈕或者移動動作調節器時，使用此影像作為啟動影像。
- `pnsTitleLocArgs`：用來取代 `title-loc-key` 中格式指定元的變數字串值。
- `apnsLocArgs`：用來取代 `locKey` 中格式指定元的變數字串值。
- `apnstitle`：「Rich Push 通知」的標題（只有 iOS 10 及更高版本才支援）。
- `apnsSubtitle`：「Rich 通知」的子標題（只有 iOS 10 及更高版本才支援）。
- `apnsAttachmentUrl`：iOS 通知媒體的鏈結（視訊、音訊、GIF、影像 - 只有 iOS 10 及更高版本才支援）。
- `fireFoxTitle`：指定要針對「WebPush 通知」設定的標題。
- `fireFoxIconUrl`：要針對「WebPush 通知」設定之圖示的 URL。
- `fireFoxTimeToLive`：此參數指定當裝置離線時，訊息保留在 GCM 儲存空間中的時間長度（以秒為單位）。
- `fireFoxPayload`：放在通知訊息中一起傳送的自訂 JSON 有效負載。
- `chromeTitle`：指定要針對「WebPush 通知」設定的標題。
- `chromeIconUrl`：要針對「WebPush 通知」設定之圖示的 URL。
- `chromeTimeToLive`：此參數指定當裝置離線時，訊息保留在 GCM 儲存空間中的時間長度（以秒為單位）。
- `chromePayload`：放在通知訊息中一起傳送的自訂 JSON 有效負載。
- `safariTitle`：指定要針對「Safari Push 通知」設定的標題。
- `safariUrlArgs`：與此通知搭配使用所需的 URL 引數。這些引數是以「JSON 陣列」的形式來提供。
- `safariAction`：動作按鈕的標籤。
- `chromeAppExtTitle`：指定要針對「WebPush 通知」設定的標題。
- `chromeAppExtCollapseKey`：此參數可識別一組訊息。
- `chromeAppExtDelayWhileIdle`：當此參數設為 true 時，表示要等到裝置變成作用中時才會傳送訊息。
- `chromeAppExtIconUrl`：要針對「WebPush 通知」設定之圖示的 URL。
- `chromeAppExtTimeToLive`：此參數指定當裝置離線時，訊息保留在 GCM 儲存空間中的時間長度（以秒為單位）。
- `chromeAppExtPayload`：放在通知訊息中一起傳送的自訂 JSON 有效負載。

## 傳送推送通知
{: #send_push_notifications}

請參閱下列範例，以從 Push Notification 套件傳送推送通知。

使用您先前建立之套件連結中的 **sendMessage** 動作，來傳送推送通知。請務必將 `/myNamespace/myPush` 取代成您的套件名稱。
```
ibmcloud fn action invoke /myNamespace/myPush/sendMessage --blocking --result -p url https://example.com -p text "this is my message" -p sound soundFileName -p deviceIds "[\"T1\",\"T2\"]"
```
{: pre}

輸出範例：
```
{
  "result": {
          "pushResponse": 
      {
        "messageId":"11111H",
        "message":{
          "alert":"this is my message",
          "url":""
        },
        "settings":{
          "apns":{
            "sound":"default"
          },
          "gcm":{
            "sound":"default"
          },
          "target":{
            "deviceIds":["T1","T2"]
          }
        }
    }
  },
      "status": "success",
      "success": true
  }
  ```
{: screen}
