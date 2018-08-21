---

copyright:
  years: 2018
lastupdated: "2018-08-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Deploying quickstart templates
{: #serviceauth}

{{site.data.keyword.openwhisk}} offers a catalog of templates to help you get started quickly on your next project. Templates are a combination of actions, triggers, sequences, and can also incorpoate service instances from {{site.data.keyword.Bluemix}}. By using templates, you can quickly and easily create a project, and start coding right away.

This tutorial steps you through deploying the templates that are available through {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

## Available quickstart templates
{: #available-templates}

| Name | Description | Supported Runtimes |
|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} Events](./deploy_templates.html#cloudant-template) | When an {{site.data.keyword.cloudant}} has a documented edited or added, log the change in the console. | Node.js, Swift, Python, PHP |
| [Upload Image](./deploy_templates.html#cos-upload-image) | A web action the allows you to upload an image to an {{site.data.keyword.cos_full}} instance's bucket and then retreive a thumbnail of that image. | Node.js |
| [Get HTTP Resource](./deploy_templates.html#get-http-resource-template) | A web action that is invoked in response to an HTTP event, and then fetches data from the Yahoo Weather API. | Node.js, Python |
| [Hello World](./deploy_templates.html#hello-world-template) | This action will accept a single parameter, which must be a JSON object. | Node.js, Swift, Python, PHP |
| [{{site.data.keyword.messagehub}} Events](./deploy_templates.html#messagehub-events-template) | When an {{site.data.keyword.messagehub_full}} topic has new data added, log the change in the console. | Node.js, Swift, Python, PHP |
| [Periodic Slack Reminder](./deploy_templates.html#slack-reminder-template) | An action that will post to Slack based on a periodic trigger. | Node.js, Swift, Python, PHP |

## Deploying the {{site.data.keyword.cloudant_short_notm}} Events template
{: #cloudant-template}

The {{site.data.keyword.cloudant_short_notm}} template creates a sequence of actions, and a trigger that will kick off that sequence. The trigger is fired when there is a change in the connected {{site.data.keyword.cloudant_short_notm}} database, which should be a database of cats, with a name and a color. The expected data item is a cat, with a name and a color defined. When a new cat is added to the database, or a current cat edited, the data will be logged to the console.

1. To create a template, go to [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/), and then click **Start Creating**.

2. Click **Quickstart templates**.

3. Click **Cloudant Events**.

### Creating the {{site.data.keyword.cloudant_short_notm}} action

1. Next, provide a name for your package or use the provided default name `new-cloudant-item`.

2. Under the **Actions** drop-down, select the runtime for the actions you will own (nodejs, swift, python, or php). For this example, select **nodejs**, and click **Next**.

### Creating the {{site.data.keyword.cloudant_short_notm}} trigger

Triggers invoke actions when they recieve events from event sources. To create a trigger for {{site.data.keyword.cloudant_short_notm}} template, provide the trigger with the required {{site.data.keyword.cloudant_short_notm}} service instance information.

#### Create {{site.data.keyword.cloudant_short_notm}} service instance

You can choose either:
  * **Create your own instance**
  * **Input your own credentials**

1. For this example, choose **Create your own instance**.

2. A pop-up opens to take you to a new tab with the {{site.data.keyword.cloudant_short_notm}} setup page. After you create the {{site.data.keyword.cloudant_short_notm}} instance, you must create a set of Service Credentials, and then close the tab to return to this page by clicking **Ok**.

3. Now choose **Input your own credentials** and provide the following information:
  * Username - _Your {{site.data.keyword.cloudant_short_notm}} Username_
  * Password - _Your {{site.data.keyword.cloudant_short_notm}} password_
  * Host - _This is usually your `username.{{site.data.keyword.cloudant_short_notm}}.com`_
  * Database - _The name of your {{site.data.keyword.cloudant_short_notm}} database_

### Deploy the {{site.data.keyword.cloudant_short_notm}} template

Click **Deploy**.

After template deployment, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

## Deploying the Upload Image template
{: #cos-upload-image}

The Upload Image template creates a web action that allows you to upload an image to an {{site.data.keyword.cos_short_notm}} bucket through a small interface. The template then retrieves the image as a thumbnail and displays it on the web action's interface.

To deploy the template:

1. Go to the {{site.data.keyword.openwhisk_short}} console in [{{site.data.keyword.Bluemix_notm}} ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/openwhisk/).

2. Click **Start Creating**.

2. Click **Quickstart templates**.

3. Click the **Upload Image** template.

4. Enter a name for your package or use the default name `upload-image`.

5. Click **Next**.

6. The template requires service credentials from an {{site.data.keyword.cos_full_notm}} service instance. In the **{{site.data.keyword.cos_short}}** list, select one of the following options:
  * **Create an new instance**: If you do not have an existing service instance, select this option to create one.
      1. In the {{site.data.keyword.cos_full_notm}} service instance creation page that opens, create a service instance.
      2. [Create a set of HMAC service credentials](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials).
      3. [Create at least one bucket](/docs/services/cloud-object-storage/getting-started.html#create-buckets).
  * **Input your own credentials**: Select this option to manually enter your own credentials for an {{site.data.keyword.cos_short}} service instance. The credentials must have HMAC keys and the service instance must have at least one bucket.
  * **Existing Instances**: If you have any existing {{site.data.keyword.cos_short}} instances, select one of the instances from the list. The credentials must have HMAC keys and the service instance must have at least one bucket.

7. Click **Deploy**.

8. In the left-hand navigation, click **Endpoints**

9. In the Web Action section, copy the link without the .json suffix` and paste it into your browser's address bar. The interface for the template's web action is displayed.

10. Optional: After the template deploys, you can navigate to the Actions dashboard to customize the code in the two new packages:
    * The `cloud-object-storage` package, which contains actions that work with {{site.data.keyword.cos_short}} instances
    * The template package (default name `upload-image`), which contains the `app` action

## Deploying the Get HTTP Resource template
{: #get-http-resource-template}

The Get HTTP Resource template creates an action to fetch an external resource, the Yahoo Weather API, and then returns data. The action is enabled as a web action, allowing it to be invoked with a URL which is CORS enabled and does not need an authentication key, which is useful for building backends for web applications. **Note**: By default, the `get-http-resource` endpoint is publically available to anyone who may want to call it.

1. To create a template, go to [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/), and then click **Start Creating**.

2. Click **Quickstart templates**.

3. Review the **Package Name** field, you can update it as needed. The default is set for you as `get-http-resource`.

4. Choose the runtime for the actions you will own: Node.js 8, Node.js 6, or Python 3.

5. Click **Deploy**.

After template deployment, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

## Deploying the Hello World template
{: #hello-world-template}

This action will accept a single parameter, which must be a JSON object.

1. To create a template, go to [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/), and then click **Start Creating**.

2. Click **Quickstart templates**.

3. Review the **Package Name** field, you can update it as needed. The default is set for you as `hello-world`.

4. Choose the runtime for the actions you will own: Node.js 8, Node.js 6, Python 3, Swift 4, or PHP 7.1.

5. Click **Deploy**.

After template deployment, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

## Deploying the {{site.data.keyword.messagehub}} Events template
{: #messagehub-events-template}

The {{site.data.keyword.messagehub}} Events template creates an action and a trigger that kicks off that action. The trigger is fired whenever there is a new item added to the {{site.data.keyword.messagehub}} topic that is chosen during template creation.

1. To create a template, go to [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/), and then click **Start Creating**.

2. Click **Quickstart templates**.

3. Review the **Package Name** field, you can update it as needed. The default is set for you as `message-hub-events`.

4. Choose the runtime for the actions you will own: Node.js 8, Node.js 6, Python 3, Swift 4, or PHP 7.1.

5. Click **Next**.

### Creating the {{site.data.keyword.messagehub}} trigger

Triggers invoke actions when they recieve events from event sources. To create a trigger for the {{site.data.keyword.messagehub}} template, provide the trigger with the required {{site.data.keyword.messagehub}} service instance information.

Review the **Trigger Name** field, you can update it as needed. The default is set for you as `message-hub-events-trgr`.

### Creating the {{site.data.keyword.messagehub}} service instance

You can choose either:
  * **Create your own instance**
  * **Input your own credentials**

1. For this example, choose **Create your own instance**.

2. A pop-up opens to take you to a new tab with the {{site.data.keyword.messagehub}} setup page. After you create the {{site.data.keyword.messagehub}} instance, you must create a set of Service Credentials, and then close the tab to return to this page by clicking **Ok**.

3. Now choose **Input your own credentials** and provide the following information:
  * Username - _Your {{site.data.keyword.messagehub}} Username_
  * Password - _Your {{site.data.keyword.messagehub}} password_
  * kafka_admin_url - _Your {{site.data.keyword.messagehub}} admin REST URL_
  * Database - _The name of your {{site.data.keyword.messagehub}} database_
  * Topic - _Topic to subscribe to_

### Deploy the {{site.data.keyword.messagehub}} template

Click **Deploy**.

After template deployment, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

## Deploying the Periodic Slack Reminder template
{: #slack-reminder-template}

The Periodic Slack Reminder template posts to Slack on an interval provided by the user during trigger creation. Before creating this template, go to https://api.slack.com/incoming-webhooks to set up the incoming webhooks URL required.

1. Review the **Package Name** field, you can update it as needed. The default is set for you as `periodic-slack-reminder`.

2. Choose the runtime for the actions you will own: Node.js 8, Node.js 6, Python 3, Swift 4, or PHP 7.1.

3. Under the **Parameters** section, enter the webhook URL into the **Parameter Value** field, and then click **Next**. (Example: https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX).

### Creating the Slack Reminder trigger

Triggers invoke actions when they recieve events from event sources. To create a trigger for the Slack Reminder template, provide the trigger with the required {{site.data.keyword.messagehub}} service instance information.

1. Review the **Trigger Name** field, you can update it as needed. The default is set for you as `periodic-slack-reminder-trgr`.

2. Next, you can specify the interval in which the trigger is to fire by using a Pattern or Cron expression. You can select UTC times for Weekdays, Hours, and Minutes. Select your desired inverval options and you will be ready for template deployment.

3. Click **Deploy**.

After template deployment, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.
