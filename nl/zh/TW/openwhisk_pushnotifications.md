---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: push notifications, events, parameters, triggers, service activity

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 裝置事件上的行動推送
{: #openwhisk_pushnotifications}

在東京地區無法使用此預先安裝的套件。
{: tip}

瞭解如何配置 Push 服務，以在指定的應用程式中有裝置（登錄/取消登錄）或（訂閱/取消訂閱）這類裝置活動時發動觸發程式。
{: shortdesc}

如需 `/whisk.system/pushnotifications` 套件本身的相關資訊，請參閱[行動推送](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a#push_step_1a)主題，其中涵蓋建立 Push 套件連結，以及傳送推送通知。

## Push 參數
{: #push_notif_parameters}

`/whisk.system/pushnotifications/webhook` 參數如下所示：
- `appId`：{{site.data.keyword.Bluemix_notm}} 應用程式 GUID。
- `appSecret`：{{site.data.keyword.Bluemix_notm}} Push Notification 服務 `appSecret`。
- `events`：`onDeviceRegister`、`onDeviceUnregister`、`onDeviceUpdate`、`onSubscribe`、`onUnsubscribe`

  您可以使用萬用字元 "`*`" 來接收所有事件的通知。

## 發動 Push Notification 服務活動的觸發程式事件
{: #trigger_push_notify}

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
