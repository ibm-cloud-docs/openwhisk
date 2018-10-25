---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-23"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.mobilepushshort}} package

The {{site.data.keyword.mobilepushshort}} package provides a set of actions for interacting with {{site.data.keyword.mobilepushfull}} service instances. These actions allow you to send a message or create, update, or delete a webhook.
{: shortdesc}

The {{site.data.keyword.mobilepushshort}} package includes the following actions:

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/push-notifications` | package | apikey, appGuid | Work with a {{site.data.keyword.mobilepushshort}} instance. |
| `/push-notifications/send-message` | action | text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | Send push notification to one or more specified devices. |
| `/push-notifications/webhook` | action | events | Fire trigger events on device activities (device registration, unregistration, subscription, or unsubscription) on the Push service. |

## Creating a {{site.data.keyword.mobilepushshort}} service instance
{: #service_instance}

Before you install the package, you must create a {{site.data.keyword.mobilepushshort}} instance.

1. [Create a {{site.data.keyword.mobilepushshort}} service instance ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/services/mobilepush/push_step_prereq.html#push_step_1).

2. [Create a set of service credentials ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/services/mobilepush/push_step_1.html#push_step_1) for the Push Notificiations service instance.

3. [Configure the {{site.data.keyword.mobilepushshort}} service instance ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/services/mobilepush/push_step_2.html#push_step_2).

## Installing the {{site.data.keyword.mobilepushshort}} package
{: #install}

After you have a {{site.data.keyword.mobilepushshort}} service instance, use either the {{site.data.keyword.openwhisk}} CLI or UI to install the {{site.data.keyword.mobilepushshort}} package into your namespace.

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #install_cli}

Before you begin:
  1. [Install the {{site.data.keyword.openwhisk_short}} plugin for the {{site.data.keyword.Bluemix_notm}} CLI](bluemix_cli.html#cloudfunctions_cli).
  2. Install the `wskdeploy` command. See the [Apache OpenWhisk documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/apache/incubator-openwhisk-wskdeploy#building-the-project).

To install the {{site.data.keyword.mobilepushshort}} package:

1. Clone the {{site.data.keyword.mobilepushshort}} package repo.
    ```
    git clone https://github.com/ibm-functions/package-push-notifications.git
    ```
    {: pre}

2. Navigate to the `runtimes/nodejs` directory.
    ```
    cd package-push-notifications/runtimes/nodejs
    ```
    {: pre}

3. Deploy the package.
    ```
    wskdeploy -m manifest.yaml
    ```
    {: pre}

4. Verify that the `push-notifications` package is added to your package list.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Output:
    ```
    packages
    /myOrg_mySpace/push-notifications private
    ```
    {: screen}

5. Bind the credentials from the {{site.data.keyword.mobilepushshort}} service instance you created to the package.
    ```
    ibmcloud fn service bind imfpush push-notifications
    ```
    {: pre}

    Example output:
    ```
    Credentials 'Credentials-1' from 'imfpush' service instance 'Push-Notifications-r1' bound to 'push-notifications'.
    ```
    {: screen}

6. Verify that the package is configured with your {{site.data.keyword.mobilepushshort}} service instance credentials.
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/push-notifications parameters
    ```
    {: pre}

    Example output:
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

### Installing from the {{site.data.keyword.openwhisk_short}} UI
{: #install_ui}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/openwhisk/create).

2. Using the namespace switcher in the top right corner, select the namespace that you want to install the {{site.data.keyword.cos_short}} package into.

3. Click **Install Packages**.

4. Click the **IBM {{site.data.keyword.mobilepushshort}}** package group, and then click the **IBM {{site.data.keyword.mobilepushshort}}** package.

5. In the Available Runtimes section, select NodeJS from the drop-down list and then click **Install**.

6. Once the package has been installed you will be redirected to the Actions page and can search for your new package, which is named **push-notifications**.

7. To use the actions in the **push-notifications** package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, follow steps 5 and 6 in the CLI instructions listed above.
  * To bind service credentials to individual actions, complete the following steps in the UI. **Note**: You must complete the following steps for each action that you want to use.
    1. Click an action from the **push-notifications** package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.mobilepushshort}} package
{: #usage}

### Send a push notification
{: #push_notif}

To send a message through the push notification service using the `send-message` action:
```
ibmcloud fn action invoke push-notifications/send-message --blocking --result --param messageText "Let's code something" --param messageUrl "http://developer.ibm.com"
```
{: pre}

Example output:
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

## Create a webhook
{: #webhook}

To create a webhook for the {{site.data.keyword.mobilepushshort}} service for onDeviceRegister events:

```
 ibmcloud fn action invoke push-notifications/webhook --blocking --param triggerName "/myPackage/myTrigger" --param events onDeviceRegister
```
{: pre}

Example output:
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
