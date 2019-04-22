---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: push notifications, functions, webhooks

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.mobilepushshort}} 套件

{{site.data.keyword.mobilepushshort}} 套件提供一組動作來與 {{site.data.keyword.mobilepushfull}} 服務實例進行互動。這些動作容許您傳送訊息，或是建立、更新或刪除 Webhook。
{: shortdesc}

{{site.data.keyword.mobilepushshort}} 套件包括下列動作：

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| `/push-notifications` |套件| apikey、appGuid |使用 {{site.data.keyword.mobilepushshort}} 實例。|
| `/push-notifications/send-message` |動作|text、url、deviceIds、platforms、userIds、tagNames、gcmCollapseKey、gcmCategory、gcmIcon、gcmDelayWhileIdle、gcmSync、gcmVisibility、gcmPayload、gcmPriority、gcmSound、gcmTimeToLive、gcmStyleType、gcmStyleTitle、gcmStyleUrl、gcmStyleText、gcmStyleLines、gcmLightsLedArgb、gcmLightsLedOnMs、gcmLightsLedOffMs、apnsBadge、apnsCategory、apnsIosActionKey、apnsPayload、apnsType、apnsSound、apnsTitleLocKey、apnsLocKey、apnsLaunchImage、apnsTitleLocArgs、apnsLocArgs、apnstitle、apnsSubtitle、apnsAttachmentUrl、fireFoxTitle、fireFoxIconUrl、fireFoxTimeToLive、fireFoxPayload、safariTitle、safariUrlArgs、safariAction、chromeTitle、chromeIconUrl、chromeTimeToLive、chromePayload、chromeAppExtTitle、chromeAppExtCollapseKey、chromeAppExtDelayWhileIdle、chromeAppExtIconUrl、chromeAppExtTimeToLive、chromeAppExtPayload|將推送通知傳送至一個以上的指定裝置。|
| `/push-notifications/webhook` |動作|事件|在 Push 服務上產生裝置活動（裝置登錄、取消登錄、訂閱或取消訂閱）時發動觸發程式事件。|

## 建立 {{site.data.keyword.mobilepushshort}} 服務實例
{: #service_instance_push}

在安裝套件之前，您必須先建立 {{site.data.keyword.mobilepushshort}} 實例。

1. [建立 {{site.data.keyword.mobilepushshort}} 服務實例 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a)。

2. 為 Push Notificiations 服務實例[建立一組服務認證 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1)。

3. [配置 {{site.data.keyword.mobilepushshort}} 服務實例 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2)。

## 安裝 {{site.data.keyword.mobilepushshort}} 套件
{: #install_push}

在您具有 {{site.data.keyword.mobilepushshort}} 服務實例之後，請使用 {{site.data.keyword.openwhisk}} CLI 或使用者介面，將 {{site.data.keyword.mobilepushshort}} 套件安裝到您的名稱空間中。

### 透過 {{site.data.keyword.openwhisk_short}} CLI 安裝
{: #install_cli}

開始之前：
  1. [安裝 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 外掛程式](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli)。

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
{: #install_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 主控台中，移至[建立頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/openwhisk/create)。

2. 使用右上角的名稱空間切換器，選取您要在其中安裝 {{site.data.keyword.cos_short}} 套件的名稱空間。

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

## 使用 {{site.data.keyword.mobilepushshort}} 套件
{: #usage_push}

### 傳送推送通知
{: #push_notif}

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

## 建立 Webhook
{: #webhook}

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
