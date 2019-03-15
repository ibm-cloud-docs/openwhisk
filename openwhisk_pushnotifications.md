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

# Mobile push on device events
{: #openwhisk_pushnotifications}

This pre-installed package is not available in the Tokyo region.
{: tip}

Learn how to configure the Push service to fire a trigger when there is device activity such as device (registration/unregistration) or (subscription/unsubscription) in a specified application.
{: shortdesc}

For information about the `/whisk.system/pushnotifications` package itself, see the [Mobile push](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a#push_step_1a) topic which covers creating a Push package binding, and sending a Push notification.

## Push parameters
{: #push_notif_parameters}

The `/whisk.system/pushnotifications/webhook` parameters are as follows:
- `appId`: The {{site.data.keyword.Bluemix_notm}} app GUID.
- `appSecret`: The {{site.data.keyword.Bluemix_notm}} push notification service `appSecret`.
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

  The action sends a Push notification.
