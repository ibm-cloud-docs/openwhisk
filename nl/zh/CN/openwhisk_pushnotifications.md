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

# 设备事件上的 Mobile Push
{: #openwhisk_pushnotifications}

此预安装包在东京区域中不可用。
{: tip}

了解如何配置 Push 服务，以在指定应用程序中存在设备活动（例如，设备注册/注销或预订/取消预订）时触发触发器。
{: shortdesc}

有关 `/whisk.system/pushnotifications` 包本身的信息，请参阅 [Mobile Push](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a#push_step_1a) 主题，该主题涵盖创建 Push 包绑定和发送推送通知的内容。

## Push 参数
{: #push_notif_parameters}

`/whisk.system/pushnotifications/webhook` 参数如下所示：
- `appId`：{{site.data.keyword.Bluemix_notm}} 应用程序 GUID。
- `appSecret`：{{site.data.keyword.Bluemix_notm}} Push Notification 服务 `appSecret`。
- `events`：`onDeviceRegister`、`onDeviceUnregister`、`onDeviceUpdate`、`onSubscribe` 和 `onUnsubscribe`

  可以使用通配符“`*`”来接收所有事件的通知。

## 对 Push Notifications 服务活动触发触发器事件
{: #trigger_push_notify}

要创建每次新设备注册 Push Notifications 服务应用程序时都触发的触发器，请参阅以下示例：

1. 使用 `appId` 和 `appSecret`，创建针对 Push Notifications 服务配置的包绑定。
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. 使用 `myPush/webhook` 订阅源为 Push Notifications 服务 `onDeviceRegister` 事件类型创建触发器。
  ```
  ibmcloud fn trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. 可以创建规则，用于在每次新设备注册时发送消息。使用先前的操作和触发器创建规则。
  ```
  ibmcloud fn rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

4. 使用 `ibmcloud fn activation poll` 命令检查结果。
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

5. 在 {{site.data.keyword.Bluemix_notm}} 应用程序中注册设备。您可以看到`规则`、`触发器`和`操作`在 {{site.data.keyword.openwhisk}} [仪表板](https://cloud.ibm.com/openwhisk/dashboard)中执行。

  操作会发送推送通知。
