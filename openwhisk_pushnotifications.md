---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Mobile push on device events

Learn how to configure the Push service to fire a Trigger when there is device activity such as device (registration/unregistration) or (subscription/unsubscription) in a specified application.
{: shortdesc}

For information about the `/whisk.system/pushnotifications` package itself, see the [Mobile push](./mobile_push_actions.html) topic which covers creating a Push package binding, and sending a Push notification.

## Push parameters
{: #push_parameters}

The `/whisk.system/pushnotifications/webhook` parameters are as follows:
- **appId:** The {{site.data.keyword.Bluemix_notm}} app GUID.
- **appSecret:** The {{site.data.keyword.Bluemix_notm}} push notification service appSecret.
- **events:** _onDeviceRegister_, _onDeviceUnregister_, _onDeviceUpdate_, _onSubscribe_, _onUnsubscribe_

  You can use the wildcard character "`*`" to be notified for all events.

## Firing a Trigger event on Push Notifications service activity
{: #trigger_push_notify}

To create a Trigger that is fired each time a new device registers with the Push Notifications service application, see the following example:

1. Create a package binding that is configured for your Push Notifications service by using your **appId** and **appSecret**.
  ```
  ic wsk package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. Create a Trigger for the Push Notifications service `onDeviceRegister` event type by using your `myPush/webhook` Feed.
  ```
  ic wsk trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. You can create a Rule that sends a message every time a new device is registered. Create a Rule by using the previous Action and Trigger.
  ```
  ic wsk rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

4. Check the results by using the `ic wsk activation poll` command.
  ```
  ic wsk activation poll
  ```
  {: pre}

5. Register a device in your {{site.data.keyword.Bluemix_notm}} application. You can see the `Rule`, `Trigger`, and `Action` are executed in the {{site.data.keyword.openwhisk}} [dashboard](https://console.bluemix.net/openwhisk/dashboard).

  The Action sends a Push notification.