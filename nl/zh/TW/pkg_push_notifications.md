---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: push notifications, functions, webhooks

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

# Push Notifications
{: #pkg_push_notifications}


## 套件
{: #pkg_push_packages}

|套件|可用性|說明|
| --- | --- | --- |
| [`/whisk.system/pushnotifications`](#pkg_push_send) |預先安裝（在東京無法使用）|將推送通知傳送至一個以上的指定裝置。|
| [`/whisk.system/pushnotifications/webhook`](#pkg_push_mobile) |預先安裝（在東京無法使用）|傳送有關裝置事件的行動推送通知。|
| [`/push-notifications`](#pkg_push_mobile_send) |可安裝|與 {{site.data.keyword.mobilepushfull}} 服務實例互動。傳送訊息，或者建立、更新或刪除 Webhook。|

## 傳送推送通知
{: #pkg_push_send}

此預先安裝套件在東京地區中無法使用。請參閱使用 IAM 鑑別可安裝的 [Push Notification](#pkg_push_mobile_send) 套件，以進行動作 `sendMessage`。
{: tip}

瞭解如何建立 Push Notification 套件連結，並使用 `/whisk.system/pushnotifications` 套件傳送簡單的推送通知。
{: shortdesc}

該套件包括下列動作及資訊來源：

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` |套件| appId、appSecret、admin_url |使用 Push 服務。|
|`/whisk.system/pushnotifications/sendMessage` |動作|text、url、deviceIds、platforms、userIds、tagNames、gcmCollapseKey、gcmCategory、gcmIcon、gcmDelayWhileIdle、gcmSync、gcmVisibility、gcmPayload、gcmPriority、gcmSound、gcmTimeToLive、gcmStyleType、gcmStyleTitle、gcmStyleUrl、gcmStyleText、gcmStyleLines、gcmLightsLedArgb、gcmLightsLedOnMs、gcmLightsLedOffMs、apnsBadge、apnsCategory、apnsIosActionKey、apnsPayload、apnsType、apnsSound、apnsTitleLocKey、apnsLocKey、apnsLaunchImage、apnsTitleLocArgs、apnsLocArgs、apnstitle、apnsSubtitle、apnsAttachmentUrl、fireFoxTitle、fireFoxIconUrl、fireFoxTimeToLive、fireFoxPayload、safariTitle、safariUrlArgs、safariAction、chromeTitle、chromeIconUrl、chromeTimeToLive、chromePayload、chromeAppExtTitle、chromeAppExtCollapseKey、chromeAppExtDelayWhileIdle、chromeAppExtIconUrl、chromeAppExtTimeToLive、chromeAppExtPayload|將推送通知傳送至一個以上的指定裝置。|


如需在存在裝置活動時發動觸發程式事件的資訊，請參閱[有關裝置事件的行動推送](#pkg_push_mobile)。

### 建立 Push 套件連結
{: #pkg_push_create}

若要建立 Push Notification 套件連結，您必須指定下列參數：

-  **appId**：{{site.data.keyword.Bluemix}} **應用程式 GUID**。
-  **appSecret**：{{site.data.keyword.Bluemix_notm}} Push Notification 服務**應用程式密碼**。

若要建立套件連結，請參閱下列步驟：

1. 在 [{{site.data.keyword.Bluemix_notm}} 儀表板](http://cloud.ibm.com)中建立 {{site.data.keyword.Bluemix_notm}} 應用程式。

2. 起始設定「Push Notification 服務」，並將服務連結至 {{site.data.keyword.Bluemix_notm}} 應用程式。

3. 配置 [Push Notification 應用程式](/docs/services/mobilepush?topic=mobile-pushnotification-gettingstartedtemplate)。

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

### Push Notification 參數
{: #pkg_push_params}

`/whisk.system/pushnotifications/sendMessage` 動作會將推送通知傳送到已登錄的裝置。參數如下所示：
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
- `apnsLaunchImage`：應用程式組合中影像檔的檔名，不論有沒有副檔名。使用者點選動作按鈕或者移動動作調節器時，使用此影像作為啟動影像。
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

### 範例推送通知
{: #pkg_push_ex}

請參閱下列範例以透過 Push Notification 套件傳送推送通知。

使用先前建立的套件連結中的 **sendMessage** 動作來傳送推送通知。請務必將 `/myNamespace/myPush` 取代成您的套件名稱。
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

## 傳送有關行動式裝置事件的推送通知
{: #pkg_push_mobile}

在東京地區無法使用此預先安裝的套件。
{: tip}

瞭解如何配置 Push Notification 服務，以在指定應用程式中存在裝置活動（例如，裝置登錄、取消登錄、訂閱或取消訂閱）時發動觸發程式。
{: shortdesc}

### 參數
{: #pkg_push_mobile_params}

`/whisk.system/pushnotifications/webhook` 參數如下所示：
- `appId`：{{site.data.keyword.Bluemix_notm}} 應用程式 GUID。
- `appSecret`：{{site.data.keyword.Bluemix_notm}} Push Notification 服務 `appSecret`。
- `events`：`onDeviceRegister`、`onDeviceUnregister`、`onDeviceUpdate`、`onSubscribe`、`onUnsubscribe`

  您可以使用萬用字元 "`*`" 來接收所有事件的通知。

### 發動 Push Notification 服務活動的觸發程式事件
{: #pkg_push_mobile_trigger}

若要建立觸發程式，以在每當新的裝置向 Push Notification 服務應用程式登錄時發動，請參閱下列範例：

1. 使用 `appId` 及 `appSecret` 來建立為 Push Notification 服務所配置的套件連結。
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. 使用 `myPush/webhook` 資訊來源，為 Push Notification 服務的 `onDeviceRegister` 事件類型建立觸發程式。
  ```
  ibmcloud fn trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. 您可以建立一個每次登錄新裝置時即傳送訊息的規則。使用前一個動作及觸發程式來建立規則。
  ```
  ibmcloud fn rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

4. 使用 `ibmcloud fn activation poll` 指令來檢查結果。
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

5. 在 {{site.data.keyword.Bluemix_notm}} 應用程式中登錄裝置。您可以看到 `rule`、`trigger` 和 `action` 在 {{site.data.keyword.openwhisk}} [儀表板](https://cloud.ibm.com/openwhisk/dashboard)中執行。

  動作會傳送推送通知。


## 傳送推送通知或者建立、更新和刪除 Webhook
{: #pkg_push_mobile_send}

可安裝的 {{site.data.keyword.mobilepushshort}} 套件提供了一組用於與 {{site.data.keyword.mobilepushfull}} 服務實例互動的動作。這些動作容許您傳送訊息，或是建立、更新或刪除 Webhook。
{: shortdesc}

{{site.data.keyword.mobilepushshort}} 套件包括下列動作：

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| `/push-notifications` |套件| apikey、appGuid |使用 {{site.data.keyword.mobilepushshort}} 實例。|
| `/push-notifications/send-message` |動作|text、url、deviceIds、platforms、userIds、tagNames、gcmCollapseKey、gcmCategory、gcmIcon、gcmDelayWhileIdle、gcmSync、gcmVisibility、gcmPayload、gcmPriority、gcmSound、gcmTimeToLive、gcmStyleType、gcmStyleTitle、gcmStyleUrl、gcmStyleText、gcmStyleLines、gcmLightsLedArgb、gcmLightsLedOnMs、gcmLightsLedOffMs、apnsBadge、apnsCategory、apnsIosActionKey、apnsPayload、apnsType、apnsSound、apnsTitleLocKey、apnsLocKey、apnsLaunchImage、apnsTitleLocArgs、apnsLocArgs、apnstitle、apnsSubtitle、apnsAttachmentUrl、fireFoxTitle、fireFoxIconUrl、fireFoxTimeToLive、fireFoxPayload、safariTitle、safariUrlArgs、safariAction、chromeTitle、chromeIconUrl、chromeTimeToLive、chromePayload、chromeAppExtTitle、chromeAppExtCollapseKey、chromeAppExtDelayWhileIdle、chromeAppExtIconUrl、chromeAppExtTimeToLive、chromeAppExtPayload|將推送通知傳送至一個以上的指定裝置。|
| `/push-notifications/webhook` |動作|事件|在 Push 服務上產生裝置活動（裝置登錄、取消登錄、訂閱或取消訂閱）時發動觸發程式事件。|

### 建立 {{site.data.keyword.mobilepushshort}} 服務實例
{: #service_instance_push}

在安裝套件之前，您必須先建立 {{site.data.keyword.mobilepushshort}} 實例。

1. [建立 {{site.data.keyword.mobilepushshort}} 服務實例 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a)。

2. 為 Push Notifications 服務實例[建立一組服務認證 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1)。

3. [配置 {{site.data.keyword.mobilepushshort}} 服務實例 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2)。

### 安裝 {{site.data.keyword.mobilepushshort}} 套件
{: #pkg_push_mobile_install}

在您具有 {{site.data.keyword.mobilepushshort}} 服務實例之後，請使用 {{site.data.keyword.openwhisk}} CLI 或使用者介面，將 {{site.data.keyword.mobilepushshort}} 套件安裝到您的名稱空間中。

### 透過 {{site.data.keyword.openwhisk_short}} CLI 安裝
{: #pkg_push_mobile_cli}

開始之前：
  1. [安裝 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 外掛程式](/docs/openwhisk?topic=cloud-functions-cli_install)。

若要安裝 {{site.data.keyword.mobilepushshort}} 套件，請執行下列動作：

1. 複製 {{site.data.keyword.mobilepushshort}} 套件儲存庫。
    ```
    git clone https://github.com/ibm-functions/package-push-notifications.git
    ```
    {: pre}

2. 導覽至 `runtimes/nodejs` 目錄。
    ```
    cd package-push-notifications/runtimes/nodejs
    ```
    {: pre}

3. 部署套件。
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. 驗證 `push-notifications` 套件已新增至套件清單。
    ```
    ibmcloud fn package list
    ```
    {: pre}

    輸出：
        ```
    packages
    /myOrg_mySpace/push-notifications private
    ```
    {: screen}

5. 將認證從您建立的 {{site.data.keyword.mobilepushshort}} 服務實例連結至套件。
    ```
    ibmcloud fn service bind imfpush push-notifications
    ```
    {: pre}

    輸出範例：
    ```
    Credentials 'Credentials-1' from 'imfpush' service instance 'Push-Notifications-r1' bound to 'push-notifications'.
    ```
    {: screen}

6. 驗證已使用 {{site.data.keyword.mobilepushshort}} 服務實例認證配置該套件。
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/push-notifications parameters
    ```
    {: pre}

    輸出範例：
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/push-notifications, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "imfpush": {
            "admin_url": "https://mobile.ng.bluemix.net/imfpushdashboard/?appGuid=12345a-a123-1234-ab12-1ba1234567",
            "apikey": "abcd1234abcd1234abcd1234",
            "appGuid": "12341-12345-1234-a1234-1abcd12345",
            "clientSecret": "1b1234ab-1234-1234-123a-ab12345abcd",
            "credentials": "Service credentials-1",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:imfpush:us-south:a/abcd1234abcd1234:abcd1234-abcd-1234-abcd1234::",
            "iam_apikey_name": "auto-generated-apikey-abcd1234abcd1234abcd1234",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/1234abcd1234abcd::serviceid:ServiceId-12345678-1234-12ab-abc1-1234abcd1234abcd",
            "instance": "Push Notifications-ab",
            "plan": "LITE",
            "url": "https://imfpush.ng.bluemix.net/imfpush/v1/apps/1234abcd-1234-abcd-1234"
          }
        }
      }
    ]
    ```
    {: screen}

### 透過 {{site.data.keyword.openwhisk_short}} 使用者介面安裝
{: #pkg_push_mobile_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 主控台中，移至[建立頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/openwhisk/create)。

2. 使用右上角的名稱空間切換器，選取您要在其中安裝 {{site.data.keyword.cos_full_notm}} 套件的名稱空間。

3. 按一下**安裝套件**。

4. 按一下**IBM {{site.data.keyword.mobilepushshort}}** 套件群組，然後按一下 **IBM {{site.data.keyword.mobilepushshort}}** 套件。

5. 在「可用的運行環境」區段中，從下拉清單中選取 NodeJS，然後按一下**安裝**。

6. 安裝好套件之後，您會被重新導向至「動作」頁面，而且您可以搜尋新的套件，其名稱為 **push-notifications**。

7. 若要使用 **push-notifications** 套件中的動作，您必須將服務認證連結至動作。
  * 若要將服務認證連結至套件中的所有動作，請遵循上述 CLI 指令中的步驟 5 及 6。
  * 若要將服務認證連結至個別動作，請在使用者介面中完成下列步驟。**附註**：您必須針對要使用的每一個動作，完成下列步驟。
    1. 按一下您要使用的 **push-notifications** 套件中的動作。即會開啟該動作的詳細資料頁面。
    2. 在左側導覽中，按一下**參數**區段。
    3. 輸入新的**參數**。對於索引鍵，輸入 `__bx_creds`。針對該值，請貼上先前建立之服務實例中的服務認證 JSON 物件。

### 傳送推送通知
{: #pkg_push_mobile_sendmsg}

若要使用 `send-message` 動作，透過推送通知服務來傳送訊息，請執行下列動作：
```
ibmcloud fn action invoke push-notifications/send-message --blocking --result --param messageText "Let's code something" --param messageUrl "http://developer.ibm.com"
```
{: pre}

輸出範例：
```
{
  "response": {
    "result": {
          "message": {
              "alert": "let's code something",
              "url": "http://developer.ibm.com"
          },
          "messageId": "fLyql2tx"
      },
      "status": "success",
      "success": true
  },
}
```
{: screen}

### 建立 Webhook
{: #pkg_push_mobile_hook}

若要為 onDeviceRegister 事件建立 {{site.data.keyword.mobilepushshort}} 服務的 Webhook，請執行下列指令：

```
 ibmcloud fn action invoke push-notifications/webhook --blocking --param triggerName "/myPackage/myTrigger" --param events onDeviceRegister
```
{: pre}

輸出範例：
```
{
  "response": {
    "result": {
          "error": {}
    },
  "status": "application error",
  "success": false
  },
}
```
{: screen}

