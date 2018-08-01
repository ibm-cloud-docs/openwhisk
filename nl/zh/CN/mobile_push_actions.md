---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Mobile Push
{: #openwhisk_catalog_pushnotifications}

了解如何创建 Push 包绑定，并使用 `/whisk.system/pushnotifications` 包发送简单的推送通知，这将使您能够使用 Push 服务。
{: shortdesc}

此包中包含以下操作和订阅源：

|实体|类型|参数|描述
|
| --- | --- | --- | --- |
|`/whisk.system/pushnotifications`|包|appId、appSecret、admin_url|使用 Push 服务。|
|`/whisk.system/pushnotifications/sendMessage`|操作|text、url、deviceIds、platforms、userIds、tagNames、gcmCollapseKey、gcmCategory、gcmIcon、gcmDelayWhileIdle、gcmSync、gcmVisibility、gcmPayload、gcmPriority、gcmSound、gcmTimeToLive、gcmStyleType、gcmStyleTitle、gcmStyleUrl、gcmStyleText、gcmStyleLines、gcmLightsLedArgb、gcmLightsLedOnMs、gcmLightsLedOffMs、apnsBadge、apnsCategory、apnsIosActionKey、apnsPayload、apnsType、apnsSound、apnsTitleLocKey、apnsLocKey、apnsLaunchImage、apnsTitleLocArgs、apnsLocArgs、apnstitle、apnsSubtitle、apnsAttachmentUrl、fireFoxTitle、fireFoxIconUrl、fireFoxTimeToLive、fireFoxPayload、safariTitle、safariUrlArgs、safariAction、chromeTitle、chromeIconUrl、chromeTimeToLive、chromePayload、chromeAppExtTitle、chromeAppExtCollapseKey、chromeAppExtDelayWhileIdle、chromeAppExtIconUrl、chromeAppExtTimeToLive、chromeAppExtPayload|将推送通知发送到一个或多个指定设备。|
|`/whisk.system/pushnotifications/webhook`|订阅源|事件|对 Push 服务上的设备活动（设备注册、取消注册、预订或取消预订）触发触发器事件。|

有关在存在设备活动时触发触发器事件的信息，请参阅[设备事件上的 Mobile Push](./openwhisk_pushnotifications.html) 主题。

## 创建 Push 包绑定
{: #create_push_binding}

要创建 Push Notifications 包绑定，必须指定以下参数：

-  **appId**：{{site.data.keyword.Bluemix}} **应用程序 GUID**。
-  **appSecret**：{{site.data.keyword.Bluemix_notm}} Push Notification 服务**应用程序私钥**。

要创建包绑定，请参阅以下步骤：

1. 在 [{{site.data.keyword.Bluemix_notm}} 仪表板](http://console.bluemix.net)中创建 {{site.data.keyword.Bluemix_notm}} 应用程序。

2. 初始化 Push Notification 服务，并将该服务绑定到 {{site.data.keyword.Bluemix_notm}} 应用程序。

3. 配置 [Push Notification 应用程序](https://console.bluemix.net/docs/services/mobilepush/index.html)。

  确保记住您所创建的 {{site.data.keyword.Bluemix_notm}} 应用程序的 **App GUID** 和 **App Secret**。

4. 创建与 `/whisk.system/pushnotifications` 的包绑定。
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myPush -p appId myAppID -p appSecret myAppSecret
  ```
  {: pre}

5. 验证包绑定是否存在：
  ```
  ibmcloud fn package list
  ```
  {: pre}

  示例输出：
  ```
  packages
  /myNamespace/myPush private binding
  ```
  {: screen}

## 推送通知参数
{: #push_parameters}

`/whisk.system/pushnotifications/sendMessage` 操作会将推送通知发送到已注册的设备。参数如下所示：
- `text`：要向用户显示的通知消息。例如，`-p text "Hi, OpenWhisk send a notification"`。
- `url`：可与警报一起发送的 URL。例如，`-p url "https:\\www.w3.ibm.com"`。
- `apiHost`：用于指定 API 主机的可选字符串。缺省值为 `mobile.ng.bluemix.net`。例如：`-p apiHost "mobile.eu-gb.bluemix.net"`
- `deviceIds`：指定设备的列表。例如，`-p deviceIds ["deviceID1"]`。
- `platforms`：向指定平台的设备发送通知。“A”表示 Apple (iOS) 设备，“G”表示 Google (Android) 设备。例如，`-p platforms ["A"]`。
- `userIds`：向指定用户的设备发送通知。例如，`-p userIds "[\"testUser\"]"`。
- `tagNames`：向已预订其中任何标记的设备发送通知。例如，`-p tagNames "[\"tag1\"]"`。
- `gcmCollapseKey`：此参数可识别一组消息
- `gcmCategory`：要用于交互式推送通知的类别标识。
- `gcmIcon`：指定要为通知显示的图标的名称。确保图标已经与客户机应用程序打包在一起。
- `gcmDelayWhileIdle`：当此参数设置为 true 时，表示在设备变为活动之前，不会发送消息。
- `gcmSync`：设备组消息传递使得组中每一个应用程序实例都可反映出最新的消息传递状态。
- `gcmVisibility`：private/public - 此通知的可视性，这将影响在安全锁定屏幕上如何以及何时显示通知。
- `gcmPayload`：将作为通知消息的一部分发送的定制 JSON 有效内容。例如，`-p gcmPayload "{\"hi\":\"hello\"}"`。
- `gcmPriority`：设置消息的优先级。
- `gcmSound`：通知到达设备时播放的声音文件（在设备上）。
- `gcmTimeToLive`：此参数指定当设备脱机时，会在 GCM 存储器中保留消息的时间长度（秒）。
- `gcmStyleType`：指定可展开通知的类型。可能的值为 `bigtext_notification`、`picture_notification` 和 `inbox_notification`。
- `gcmStyleTitle`：指定通知的标题。标题在展开通知时显示。必须为所有三个可展开通知指定标题。
- `gcmStyleUrl`：必须从中获取通知图片的 URL。必须为 `picture_notification` 指定。
- `gcmStyleText`：需要在展开 `bigtext_notification` 时显示的大文本。必须为 `bigtext_notification` 指定。
- `gcmStyleLines`：要在 `inbox_notification` 的收件箱样式中显示的字符串数组。必须为 `inbox_notification` 指定。
- `gcmLightsLedArgb`：LED 指示灯的颜色。硬件将尽力接近设定值。
- `gcmLightsLedOnMs`：LED 指示灯闪烁时点亮的毫秒数。硬件将尽力接近设定值。
- `gcmLightsLedOffMs`：LED 指示灯闪烁时不亮的毫秒数。硬件将尽力接近设定值。
- `apnsBadge`：显示为应用程序图标的角标的数字。
- `apnsCategory`：要用于交互式推送通知的类别标识。
- `apnsIosActionKey`：操作键的标题。
- `apnsPayload`：将作为通知消息的一部分发送的定制 JSON 有效内容。
- `apnsType`：[“DEFAULT”、“MIXED”、“SILENT”]。
- `apnsSound`：应用程序捆绑软件中声音文件的名称。此文件的声音播放为警报。
- `apnsTitleLocKey`：当前本地化的 `Localizable.strings` 文件中标题字符串的键。该键字符串可以使用 %@ 和 %n$@ 说明符设置格式，以采用 `titleLocArgs` 数组中指定的变量。
- `apnsLocKey`：当前本地化的 `Localizable.strings` 文件（通过用户的语言首选项进行设置）中警报消息字符串的键。该键字符串可以使用 %@ 和 %n$@ 说明符设置格式，以采用 locArgs 数组中指定的变量。
- `apnsLaunchImage`：应用程序捆绑软件中图像文件的文件名，可带或不带文件扩展名。图像用作用户点击操作按钮或移动操作滑块时的启动图像。
- `pnsTitleLocArgs`：要显示以取代 `title-loc-key` 中格式说明符的变量字符串值。
- `apnsLocArgs`：要显示以取代 `locKey` 中格式说明符的变量字符串值。
- `apnstitle`：富文本推送通知的标题（仅 iOS 10 及更高版本上支持）。
- `apnsSubtitle`：富文本通知的子标题。（仅 iOS 10 及更高版本上支持）。
- `apnsAttachmentUrl`：iOS 通知媒体的链接（视频、音频、GIF、图像 - 仅 iOS 10 及更高版本上支持）。
- `fireFoxTitle`：指定要为 WebPush Notification 设置的标题。
- `fireFoxIconUrl`：要为 WebPush Notification 设置的图标的 URL。
- `fireFoxTimeToLive`：此参数指定当设备脱机时，会在 GCM 存储器中保留消息的时间长度（秒）。
- `fireFoxPayload`：将作为通知消息的一部分发送的定制 JSON 有效内容。
- `chromeTitle`：指定要为 WebPush Notification 设置的标题。
- `chromeIconUrl`：要为 WebPush Notification 设置的图标的 URL。
- `chromeTimeToLive`：此参数指定当设备脱机时，会在 GCM 存储器中保留消息的时间长度（秒）。
- `chromePayload`：将作为通知消息的一部分发送的定制 JSON 有效内容。
- `safariTitle`：指定要为 Safari 推送通知设置的标题。
- `safariUrlArgs`：需要用于此通知的 URL 自变量。这些自变量应以 JSON 数组格式提供。
- `safariAction`：操作按钮的标签。
- `chromeAppExtTitle`：指定要为 WebPush Notification 设置的标题。
- `chromeAppExtCollapseKey`：此参数用于识别一组消息。
- `chromeAppExtDelayWhileIdle`：当此参数设置为 true 时，表示在设备变为活动之前，不会发送消息。
- `chromeAppExtIconUrl`：要为 WebPush Notification 设置的图标的 URL。
- `chromeAppExtTimeToLive`：此参数指定当设备脱机时，会在 GCM 存储器中保留消息的时间长度（秒）。
- `chromeAppExtPayload`：将作为通知消息的一部分发送的定制 JSON 有效内容。

## 发送推送通知
{: #send_push_notifications}

请参阅以下示例以通过 Push Notification 包发送推送通知。

使用先前创建的包绑定中的 **sendMessage** 操作来发送推送通知。确保将 `/myNamespace/myPush` 替换为您的包名。
```
ibmcloud fn action invoke /myNamespace/myPush/sendMessage --blocking --result -p url https://example.com -p text "this is my message" -p sound soundFileName -p deviceIds "[\"T1\",\"T2\"]"
```
{: pre}

示例输出：
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
