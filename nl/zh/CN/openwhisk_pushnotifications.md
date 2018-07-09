---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 设备事件上的 Mobile Push
{: #openwhisk_pushnotifications}

了解如何配置 Push 服务，以在指定应用程序中存在设备活动（例如，设备注册/注销或预订/取消预订）时触发触发器。
{: shortdesc}

有关 `/whisk.system/pushnotifications` 包本身的信息，请参阅 [Mobile Push](./mobile_push_actions.html) 主题，该主题涵盖创建 Push 包绑定和发送推送通知的内容。

## Push 参数
{: #push_parameters}

`/whisk.system/pushnotifications/webhook` 参数如下所示：
- **appId**：{{site.data.keyword.Bluemix_notm}} 应用程序 GUID。
- **appSecret**：{{site.data.keyword.Bluemix_notm}} 推送通知服务 appSecret。
- **events**：_onDeviceRegister_、_onDeviceUnregister_、_onDeviceUpdate_、_onSubscribe_ 和 _onUnsubscribe_

  可以使用通配符“`*`”来接收所有事件的通知。

## 对 Push Notifications 服务活动触发触发器事件
{: #trigger_push_notify}

要创建每次新设备注册 Push Notifications 服务应用程序时都触发的触发器，请参阅以下示例：

1. 使用 **appId** 和 **appSecret**，创建针对 Push Notifications 服务配置的包绑定。
  ```
  ibmcloud wsk package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. 使用 `myPush/webhook` 订阅源为 Push Notifications 服务 `onDeviceRegister` 事件类型创建触发器。
  ```
  ibmcloud wsk trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. 可以创建规则，用于在每次新设备注册时发送消息。使用先前的操作和触发器创建规则。
  ```
  ibmcloud wsk rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

4. 使用 `ibmcloud wsk activation poll` 命令检查结果。
  ```
  ibmcloud wsk activation poll
  ```
  {: pre}

5. 在 {{site.data.keyword.Bluemix_notm}} 应用程序中注册设备。您可以看到`规则`、`触发器`和`操作`在 {{site.data.keyword.openwhisk}} [仪表板](https://console.bluemix.net/openwhisk/dashboard)中执行。

  操作会发送推送通知。
