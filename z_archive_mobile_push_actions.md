---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-09"

keywords: mobile, push notifications, binding, notifications

subcollection: cloud-functions

---





{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Push Notification package
{: #openwhisk_catalog_pushnotifications}

This pre-installed package is not available in the Tokyo region. Please see the installable [Push Notification](/docs/openwhisk?topic=cloud-functions-ow_push_notifications) package for the action `sendMessage` using IAM authentication
{: tip}

Learn how to create a Push Notification package binding, and send a simple push notification using the `/whisk.system/pushnotifications` package.
{: shortdesc}

The package includes the following actions and feeds:

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` | package | appId, appSecret, admin_url | Work with the Push Service. |
| `/whisk.system/pushnotifications/sendMessage` | action | text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | Send push notification to one or more specified devices. |


For information about firing trigger events when there is device activity, see [Mobile push on device events](#openwhisk_pushnotifications).

## Creating a Push package binding
{: #create_push_binding}

To create a Push Notifications package binding, you must specify the following parameters:

-  **appId**: The {{site.data.keyword.Bluemix}} **App GUID**.
-  **appSecret**: The {{site.data.keyword.Bluemix_notm}} Push Notification service **App Secret**.

To create a package binding, see the following steps:

1. Create an {{site.data.keyword.Bluemix_notm}} application in the [{{site.data.keyword.Bluemix_notm}} Dashboard](http://cloud.ibm.com).

2. Initialize the Push Notification Service and bind the service to the {{site.data.keyword.Bluemix_notm}} application.

3. Configure the [Push Notification application](/docs/services/mobilepush?topic=mobile-pushnotification-gettingstartedtemplate).

  Be sure to remember the **App GUID** and the **App Secret** of the {{site.data.keyword.Bluemix_notm}} app you created.

4. Create a package binding with the `/whisk.system/pushnotifications`.
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myPush -p appId myAppID -p appSecret myAppSecret
  ```
  {: pre}

5. Verify that the package binding exists:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Example output:
  ```
  packages
  /myNamespace/myPush private binding
  ```
  {: screen}

## Push notification parameters
{: #push_parameters}

The `/whisk.system/pushnotifications/sendMessage` action sends push notifications to registered devices. The parameters are as follows:
- `text`: The notification message to be shown to the user. For example, `-p text "Hi, OpenWhisk send a notification"`.
- `url`: A URL that can be sent along with the alert. For example, `-p url "https:\\www.w3.ibm.com"`.
- `apiHost`: An optional string that specifies the API host. The default is `mobile.ng.bluemix.net`.  For example: `-p apiHost "mobile.eu-gb.bluemix.net"`
- `deviceIds`: The list of specified devices. For example, `-p deviceIds ["deviceID1"]`.
- `platforms`: Send notification to the devices of the specified platforms. 'A' for Apple (iOS) devices and 'G' for Google (Android) devices. For example, `-p platforms ["A"]`.
- `userIds`: Send notification to the devices of the specified users. For example, `-p userIds "[\"testUser\"]"`
- `tagNames`: Send notification to the devices that are subscribed to any of these tags. For example, `-p tagNames "[\"tag1\"]"`.
- `gcmCollapseKey`: This parameter identifies a group of messages
- `gcmCategory`: The category identifier to be used for the interactive push notifications.
- `gcmIcon`: Specify the name of the icon to be displayed for the notification. Make sure that the icon is already packaged with the client application.
- `gcmDelayWhileIdle`: When this parameter is set to true, the message is sent until the device becomes active.
- `gcmSync`: Device group messaging makes it possible for every app instance in a group to reflect the latest messaging state.
- `gcmVisibility`: private/public - Visibility of this notification, which affects how and when the notifications are revealed on a secure locked screen.
- `gcmPayload`: Custom JSON payload that is sent as part of the notification message. For example, `-p gcmPayload "{\"hi\":\"hello\"}"`
- `gcmPriority`: Sets the priority of the message.
- `gcmSound`: The sound file (on device) that plays when the notification arrives on the device.
- `gcmTimeToLive`: This parameter specifies how long (in seconds) the message is kept in GCM storage if the device is offline.
- `gcmStyleType`: Specifies the type of expandable notifications. The possible values are `bigtext_notification`, `picture_notification`, `inbox_notification`.
- `gcmStyleTitle`: Specifies the title of the notification. The title is displayed when the notification is expanded. Title must be specified for all three expandable notifications.
- `gcmStyleUrl`: A URL from which the picture has to be obtained for the notification. Must be specified for `picture_notification`.
- `gcmStyleText`: The large text that needs to be displayed on expanding a `bigtext_notification`. Must be specified for `bigtext_notification`.
- `gcmStyleLines`: An array of strings that is to be displayed in inbox style for `inbox_notification`. Must be specified for `inbox_notification`.
- `gcmLightsLedArgb`: The color of the led. The hardware does its best approximation.
- `gcmLightsLedOnMs`: The number of milliseconds for the LED to be on while it's flashing. The hardware does its best approximation.
- `gcmLightsLedOffMs`: The number of milliseconds for the LED to be off while it's flashing. The hardware does its best approximation.
- `apnsBadge`: The number to display as the badge of the application icon.
- `apnsCategory`: The category identifier to be used for the interactive push notifications.
- `apnsIosActionKey`: The title for the action key.
- `apnsPayload`: Custom JSON payload that is sent as part of the notification message.
- `apnsType`: ['DEFAULT', 'MIXED', 'SILENT'].
- `apnsSound`: The name of the sound file in the application bundle. The sound of this file is played as an alert.
- `apnsTitleLocKey`: The key to a title string in the `Localizable.strings` file for the current localization. The key string can be formatted with %@ and %n$@ specifiers to take the variables that are specified in the `titleLocArgs` array.
- `apnsLocKey`: A key to an alert-message string in a `Localizable.strings` file for the current localization (which is set by the user’s language preference). The key string can be formatted with %@ and %n$@ specifiers to take the variables that are specified in the locArgs array.
- `apnsLaunchImage`: The filename of an image file in the app bundle, with or without the filename extension. The image is used as the launch image when users tap the action button or move the action slider.
- `pnsTitleLocArgs`: Variable string values to appear in place of the format specifiers in `title-loc-key`.
- `apnsLocArgs`: Variable string values to appear in place of the format specifiers in `locKey`.
- `apnstitle`: The title of Rich Push notifications (Supported only on iOS 10 and above).
- `apnsSubtitle`: The subtitle of the Rich Notifications. (Supported only on iOS 10 and above).
- `apnsAttachmentUrl`: The link to the iOS notifications media (video, audio, GIF, images - Supported only on iOS 10 and above).
- `fireFoxTitle`: Specifies the title to be set for the WebPush Notification.
- `fireFoxIconUrl`: The URL of the icon to be set for the WebPush Notification.
- `fireFoxTimeToLive`: This parameter specifies how long (in seconds) the message is kept in GCM storage if the device is offline.
- `fireFoxPayload`: Custom JSON payload that is sent as part of the notification message.
- `chromeTitle`: Specifies the title to be set for the WebPush Notification.
- `chromeIconUrl`: The URL of the icon to be set for the WebPush Notification.
- `chromeTimeToLive`: This parameter specifies how long (in seconds) the message is kept in GCM storage if the device is offline.
- `chromePayload`: Custom JSON payload that is sent as part of the notification message.
- `safariTitle`: Specifies the title to be set for the Safari Push Notifications.
- `safariUrlArgs`: The URL arguments that need to be used with this notification. These arguments are to be provided in the form of a JSON Array.
- `safariAction`: The label of the action button.
- `chromeAppExtTitle`: Specifies the title to be set for the WebPush Notification.
- `chromeAppExtCollapseKey`: This parameter identifies a group of messages.
- `chromeAppExtDelayWhileIdle`: When this parameter is set to true, it indicates that the message is not sent until the device becomes active.
- `chromeAppExtIconUrl`: The URL of the icon to be set for the WebPush Notification.
- `chromeAppExtTimeToLive`: This parameter specifies how long (in seconds) the message is kept in GCM storage if the device is offline.
- `chromeAppExtPayload`: Custom JSON payload that is sent as part of the notification message.

## Sending push notifications
{: #send_push_notifications}

See the following example to send a push notification from the Push notification package.

Send a push notification by using the **sendMessage** action in the package binding that you created previously. Be sure to replace `/myNamespace/myPush` with your package name.
```
ibmcloud fn action invoke /myNamespace/myPush/sendMessage --blocking --result -p url https://example.com -p text "this is my message" -p sound soundFileName -p deviceIds "[\"T1\",\"T2\"]"
```
{: pre}

Example output:
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

# Mobile push on device events package
{: #openwhisk_pushnotifications}

This pre-installed package is not available in the Tokyo region.
{: tip}

Learn how to configure the Push Notification service to fire a trigger when there is device activity such as device (registration/unregistration) or (subscription/unsubscription) in a specified application.
{: shortdesc}

## Push parameters
{: #push_notif_parameters}

The `/whisk.system/pushnotifications/webhook` parameters are as follows:
- `appId`: The {{site.data.keyword.Bluemix_notm}} app GUID.
- `appSecret`: The {{site.data.keyword.Bluemix_notm}} Push Notification service `appSecret`.
- `events`: `onDeviceRegister`, `onDeviceUnregister`, `onDeviceUpdate`, `onSubscribe`, `onUnsubscribe`

  You can use the wildcard character "`*`" to be notified for all events.

## Firing a trigger event on Push Notifications service activity
{: #trigger_push_notify}

To create a trigger that is fired each time a new device registers with the Push Notifications service application, see the following example:

1. Create a package binding that is configured for your Push Notifications service by using your `appId` and `appSecret`.
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. Create a trigger for the Push Notifications service `onDeviceRegister` event type by using your `myPush/webhook` feed.
  ```
  ibmcloud fn trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. You can create a rule that sends a message every time a new device is registered. Create a rule by using the previous action and trigger.
  ```
  ibmcloud fn rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

4. Check the results by using the `ibmcloud fn activation poll` command.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

5. Register a device in your {{site.data.keyword.Bluemix_notm}} application. You can see the `rule`, `trigger`, and `action` are executed in the {{site.data.keyword.openwhisk}} [dashboard](https://cloud.ibm.com/openwhisk/dashboard).

  The action sends a push notification.

