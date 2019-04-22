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

# {{site.data.keyword.mobilepushshort}} 包

{{site.data.keyword.mobilepushshort}} 包提供了一组用于与 {{site.data.keyword.mobilepushfull}} 服务实例进行交互的操作。通过这些操作，可以发送消息，或者创建、更新或删除 Webhook。
{: shortdesc}

{{site.data.keyword.mobilepushshort}} 包中包含以下操作：

|实体|类型|参数|描述
|
| --- | --- | --- | --- |
|`/push-notifications`|包|apikey、appGuid|使用 {{site.data.keyword.mobilepushshort}} 实例。|
|`/push-notifications/send-message`|操作|text、url、deviceIds、platforms、userIds、tagNames、gcmCollapseKey、gcmCategory、gcmIcon、gcmDelayWhileIdle、gcmSync、gcmVisibility、gcmPayload、gcmPriority、gcmSound、gcmTimeToLive、gcmStyleType、gcmStyleTitle、gcmStyleUrl、gcmStyleText、gcmStyleLines、gcmLightsLedArgb、gcmLightsLedOnMs、gcmLightsLedOffMs、apnsBadge、apnsCategory、apnsIosActionKey、apnsPayload、apnsType、apnsSound、apnsTitleLocKey、apnsLocKey、apnsLaunchImage、apnsTitleLocArgs、apnsLocArgs、apnstitle、apnsSubtitle、apnsAttachmentUrl、fireFoxTitle、fireFoxIconUrl、fireFoxTimeToLive、fireFoxPayload、safariTitle、safariUrlArgs、safariAction、chromeTitle、chromeIconUrl、chromeTimeToLive、chromePayload、chromeAppExtTitle、chromeAppExtCollapseKey、chromeAppExtDelayWhileIdle、chromeAppExtIconUrl、chromeAppExtTimeToLive、chromeAppExtPayload|将推送通知发送到一个或多个指定设备。|
|`/push-notifications/webhook`|操作|事件|对 Push 服务上的设备活动（设备注册、取消注册、预订或取消预订）触发触发器事件。|

## 创建 {{site.data.keyword.mobilepushshort}} 服务实例
{: #service_instance_push}

安装包之前，必须创建 {{site.data.keyword.mobilepushshort}} 实例。

1. [创建 {{site.data.keyword.mobilepushshort}} 服务实例 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a)。

2. 为 Push Notifications 服务实例[创建一组服务凭证 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1)。

3. [配置 {{site.data.keyword.mobilepushshort}} 服务实例 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2)。

## 安装 {{site.data.keyword.mobilepushshort}} 包
{: #install_push}

具有 {{site.data.keyword.mobilepushshort}} 服务实例后，请使用 {{site.data.keyword.openwhisk}} CLI 或 UI 将 {{site.data.keyword.mobilepushshort}} 包安装到名称空间中。

### 通过 {{site.data.keyword.openwhisk_short}} CLI 进行安装
{: #install_cli}

开始之前：
  1. [安装 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 插件](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli)。

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
{: #install_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 控制台中，转至[“创建”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/create)。

2. 使用右上角的名称空间切换工具，选择要将 {{site.data.keyword.cos_short}} 包安装到其中的名称空间。

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

## 使用 {{site.data.keyword.mobilepushshort}} 包
{: #usage_push}

### 发送推送通知
{: #push_notif}

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

## 创建 Webhook
{: #webhook}

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
