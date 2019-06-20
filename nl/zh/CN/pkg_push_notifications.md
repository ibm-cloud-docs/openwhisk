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


## 包
{: #pkg_push_packages}

|包|可用性|描述|
| --- | --- | --- |
|[`/whisk.system/pushnotifications`](#pkg_push_send)|预安装（在东京不可用）|将推送通知发送到一个或多个指定设备。|
|[`/whisk.system/pushnotifications/webhook`](#pkg_push_mobile)|预安装（在东京不可用）|发送有关设备事件的移动推送通知。|
|[`/push-notifications`](#pkg_push_mobile_send)|可安装|与 {{site.data.keyword.mobilepushfull}} 服务实例进行交互。发送消息，或者创建、更新或删除 Webhook。|

## 发送推送通知
{: #pkg_push_send}

此预安装包在东京区域中不可用。请使用 IAM 认证来查看可安装的 [Push Notification](#pkg_push_mobile_send) 包中的操作 `sendMessage`。
{: tip}

了解如何创建 Push Notification 包绑定，并使用 `/whisk.system/pushnotifications` 包发送简单的推送通知。
{: shortdesc}

此包中包含以下操作和订阅源：

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|`/whisk.system/pushnotifications`|包|appId、appSecret、admin_url|使用 Push 服务。|
|`/whisk.system/pushnotifications/sendMessage`|操作|text、url、deviceIds、platforms、userIds、tagNames、gcmCollapseKey、gcmCategory、gcmIcon、gcmDelayWhileIdle、gcmSync、gcmVisibility、gcmPayload、gcmPriority、gcmSound、gcmTimeToLive、gcmStyleType、gcmStyleTitle、gcmStyleUrl、gcmStyleText、gcmStyleLines、gcmLightsLedArgb、gcmLightsLedOnMs、gcmLightsLedOffMs、apnsBadge、apnsCategory、apnsIosActionKey、apnsPayload、apnsType、apnsSound、apnsTitleLocKey、apnsLocKey、apnsLaunchImage、apnsTitleLocArgs、apnsLocArgs、apnstitle、apnsSubtitle、apnsAttachmentUrl、fireFoxTitle、fireFoxIconUrl、fireFoxTimeToLive、fireFoxPayload、safariTitle、safariUrlArgs、safariAction、chromeTitle、chromeIconUrl、chromeTimeToLive、chromePayload、chromeAppExtTitle、chromeAppExtCollapseKey、chromeAppExtDelayWhileIdle、chromeAppExtIconUrl、chromeAppExtTimeToLive、chromeAppExtPayload|将推送通知发送到一个或多个指定设备。|


有关在存在设备活动时触发触发器事件的信息，请参阅[有关设备事件的移动推送](#pkg_push_mobile)。

### 创建 Push 包绑定
{: #pkg_push_create}

要创建 Push Notifications 包绑定，必须指定以下参数：

-  **appId**：{{site.data.keyword.Bluemix}} **应用程序 GUID**。
-  **appSecret**：{{site.data.keyword.Bluemix_notm}} Push Notification 服务**应用程序私钥**。

要创建包绑定，请参阅以下步骤：

1. 在 [{{site.data.keyword.Bluemix_notm}} 仪表板](http://cloud.ibm.com)中创建 {{site.data.keyword.Bluemix_notm}} 应用程序。

2. 初始化 Push Notification 服务，并将该服务绑定到 {{site.data.keyword.Bluemix_notm}} 应用程序。

3. 配置 [Push Notification 应用程序](/docs/services/mobilepush?topic=mobile-pushnotification-gettingstartedtemplate)。

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

### 推送通知参数
{: #pkg_push_params}

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

### 示例推送通知
{: #pkg_push_ex}

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

## 发送有关移动设备事件的推送通知
{: #pkg_push_mobile}

此预安装包在东京区域中不可用。
{: tip}

了解如何配置 Push Notification 服务，以在指定应用程序中存在设备活动（例如，设备注册/注销或预订/取消预订）时触发触发器。
{: shortdesc}

### 参数
{: #pkg_push_mobile_params}

`/whisk.system/pushnotifications/webhook` 参数如下所示：
- `appId`：{{site.data.keyword.Bluemix_notm}} 应用程序 GUID。
- `appSecret`：{{site.data.keyword.Bluemix_notm}} Push Notification 服务 `appSecret`。
- `events`：`onDeviceRegister`、`onDeviceUnregister`、`onDeviceUpdate`、`onSubscribe` 和 `onUnsubscribe`

  可以使用通配符“`*`”来接收所有事件的通知。

### 对 Push Notifications 服务活动触发触发器事件
{: #pkg_push_mobile_trigger}

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


## 发送推送通知或者创建、更新和删除 Webhook
{: #pkg_push_mobile_send}

可安装的 {{site.data.keyword.mobilepushshort}} 包提供了一组用于与 {{site.data.keyword.mobilepushfull}} 服务实例进行交互的操作。通过这些操作，可以发送消息，或者创建、更新或删除 Webhook。
{: shortdesc}

{{site.data.keyword.mobilepushshort}} 包中包含以下操作：

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|`/push-notifications`|包|apikey、appGuid|使用 {{site.data.keyword.mobilepushshort}} 实例。|
|`/push-notifications/send-message`|操作|text、url、deviceIds、platforms、userIds、tagNames、gcmCollapseKey、gcmCategory、gcmIcon、gcmDelayWhileIdle、gcmSync、gcmVisibility、gcmPayload、gcmPriority、gcmSound、gcmTimeToLive、gcmStyleType、gcmStyleTitle、gcmStyleUrl、gcmStyleText、gcmStyleLines、gcmLightsLedArgb、gcmLightsLedOnMs、gcmLightsLedOffMs、apnsBadge、apnsCategory、apnsIosActionKey、apnsPayload、apnsType、apnsSound、apnsTitleLocKey、apnsLocKey、apnsLaunchImage、apnsTitleLocArgs、apnsLocArgs、apnstitle、apnsSubtitle、apnsAttachmentUrl、fireFoxTitle、fireFoxIconUrl、fireFoxTimeToLive、fireFoxPayload、safariTitle、safariUrlArgs、safariAction、chromeTitle、chromeIconUrl、chromeTimeToLive、chromePayload、chromeAppExtTitle、chromeAppExtCollapseKey、chromeAppExtDelayWhileIdle、chromeAppExtIconUrl、chromeAppExtTimeToLive、chromeAppExtPayload|将推送通知发送到一个或多个指定设备。|
|`/push-notifications/webhook`|操作|事件|对 Push 服务上的设备活动（设备注册、取消注册、预订或取消预订）触发触发器事件。|

### 创建 {{site.data.keyword.mobilepushshort}} 服务实例
{: #service_instance_push}

安装包之前，必须创建 {{site.data.keyword.mobilepushshort}} 实例。

1. [创建 {{site.data.keyword.mobilepushshort}} 服务实例 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a)。

2. 为 Push Notifications 服务实例[创建一组服务凭证 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1)。

3. [配置 {{site.data.keyword.mobilepushshort}} 服务实例 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2)。

### 安装 {{site.data.keyword.mobilepushshort}} 包
{: #pkg_push_mobile_install}

具有 {{site.data.keyword.mobilepushshort}} 服务实例后，请使用 {{site.data.keyword.openwhisk}} CLI 或 UI 将 {{site.data.keyword.mobilepushshort}} 包安装到名称空间中。

### 通过 {{site.data.keyword.openwhisk_short}} CLI 进行安装
{: #pkg_push_mobile_cli}

开始之前：
  1. [安装 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 插件](/docs/openwhisk?topic=cloud-functions-cli_install)。

要安装 {{site.data.keyword.mobilepushshort}} 包，请执行以下操作：

1. 克隆 {{site.data.keyword.mobilepushshort}} 包存储库。
    ```
    git clone https://github.com/ibm-functions/package-push-notifications.git
    ```
    {: pre}

2. 导航至 `runtimes/nodejs` 目录。
    ```
    cd package-push-notifications/runtimes/nodejs
    ```
    {: pre}

3. 部署包。
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. 验证 `push-notifications` 包是否已添加到包列表中。
    ```
    ibmcloud fn package list
    ```
    {: pre}

    输出：
    ```
    packages
    /myOrg_mySpace/push-notifications private
    ```
    {: screen}

5. 将所创建的 {{site.data.keyword.mobilepushshort}} 服务实例中的凭证绑定到包。
    ```
    ibmcloud fn service bind imfpush push-notifications
    ```
    {: pre}

    示例输出：
      ```
    Credentials 'Credentials-1' from 'imfpush' service instance 'Push-Notifications-r1' bound to 'push-notifications'.
    ```
    {: screen}

6. 验证该包是否已配置为使用 {{site.data.keyword.mobilepushshort}} 服务实例凭证。
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/push-notifications parameters
    ```
    {: pre}

    示例输出：
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

### 通过 {{site.data.keyword.openwhisk_short}} UI 进行安装
{: #pkg_push_mobile_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 控制台中，转至[“创建”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/create)。

2. 使用右上角的名称空间切换工具，选择要将 {{site.data.keyword.cos_full_notm}} 包安装到其中的名称空间。

3. 单击**安装包**。

4. 单击 **IBM {{site.data.keyword.mobilepushshort}}** 包组，然后单击 **IBM {{site.data.keyword.mobilepushshort}}** 包。

5. 在“可用运行时”部分中，从下拉列表中选择 NodeJS，然后单击**安装**。

6. 安装包后，会将您重定向到“操作”页面，您可以在其中搜索名为 **push-notifications** 的新包。

7. 要使用 **push-notifications** 包中的操作，必须将服务凭证绑定到这些操作。
  * 要将服务凭证绑定到包中的所有操作，请遵循上面列出的 CLI 指示信息中的步骤 5 和 6。
  * 要将服务凭证绑定到单个操作，请在 UI 中完成以下步骤。**注**：对于要使用的每个操作，必须完成以下步骤。
    1. 单击 **push-notifications** 包中要使用的操作。这将打开该操作的详细信息页面。
    2. 在左侧导航中，单击**参数**部分。
    3. 输入新的**参数**。对于键，输入 `__bx_creds`。对于值，请从先前创建的服务实例中粘贴服务凭证 JSON 对象。

### 发送推送通知
{: #pkg_push_mobile_sendmsg}

要使用 `send-message` 操作通过 Push Notification 服务发送消息，请运行以下命令：
```
ibmcloud fn action invoke push-notifications/send-message --blocking --result --param messageText "Let's code something" --param messageUrl "http://developer.ibm.com"
```
{: pre}

示例输出：
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

### 创建 Webhook
{: #pkg_push_mobile_hook}

要为 {{site.data.keyword.mobilepushshort}} 服务创建用于 onDeviceRegister 事件的 Webhook，请运行以下命令：

```
 ibmcloud fn action invoke push-notifications/webhook --blocking --param triggerName "/myPackage/myTrigger" --param events onDeviceRegister
```
{: pre}

示例输出：
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

