---

copyright:

  years: 2018

lastupdated: "2018-04-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Deploying Quickstart Templates
{: #serviceauth}

{{site.data.keyword.openwhisk}} offers a catalog of Templates to help you get started quickly on your next project. Templates are a combination of Actions, Triggers, Sequences, and can also incorpoate service instances from {{site.data.keyword.Bluemix}}. By using Templates, you can quickly and easily create a project, and start coding right away. 

This tutorial steps you through deploying the Cloudant Template.
{: shortdesc}

## Available Quickstart Templates
{: #available-templates}

| Name | Description | Supported Runtimes | 
|:-----------------|:-----------------|:-----------------|
| [Cloudant Events](./deploy_templates.html#cloudant-template) | When a Cloudant DB has a documented edited or added, log the change in the console. | Node.js, Swift, Python, PHP |
| [Get HTTP Resource](./deploy_templates.html#get-http-resource-template) | A web action that is invoked in response to an HTTP event, and then fetches data from the Yahoo Weather API. | Node.js, Python |
| [Hello World](./deploy_templates.html#hello-world-template) | This action will accept a single parameter, which must be a JSON object. | Node.js, Swift, Python, PHP |
| [Message Hub Events](./deploy_templates.html#messagehub-events-template) | When a Message Hub topic has new data added, log the change in the console. | Node.js, Swift, Python, PHP | 
| [Periodic Slack Reminder](./deploy_templates.html#slack-reminder-template) | An action that will post to Slack based on a periodic trigger. | Node.js, Swift, Python, PHP |

## Deploying the Cloudant Events template
{: #cloudant-template}

The Cloudant Template creates a sequence of Actions, and a Trigger that will kick off that Sequence. The Trigger is fired when there is a change in the connected Cloudant DB, which should be a database of cats, with a name and a color. The expected data item is a cat, with a name and a color defined. When a new cat is added to the database, or a current cat edited, the data will be logged to the console.

1. To create a Template, go to [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/), and then click **Start Creating**.

2. Click **Quickstart Templates**.

3. Click **New Cloudant Item**.

### Creating the Cloudant Action

1. Next, provide a name for your package or use the provided default name `new-cloudant-item`.

2. Under the **Actions** drop-down, select the runtime for the actions you will own (nodejs, swift, python, or php). For this example, select **nodejs**, and click **Next**.

### Creating the Cloudant trigger

Triggers invoke Actions when they recieve events from event sources. To create a Trigger for Cloudant Template, provide the trigger with the required Cloudant service instance information.

#### Create Cloudant service instance

You can choose either:
  * **Create your own instance**
  * **Input your own credentials**

1. For this example, choose **Create your own instance**.

2. A pop-up opens to take you to a new tab with the Cloudant setup page. After you create the Cloudant instance, you must create a set of Service Credentials, and then close the tab to return to this page by clicking **Ok**.

3. Now choose **Input your own credentials** and provide the following information:
  * Username - _Your Cloudant Username_
  * Password - _Your Cloudant password_
  * Host - _This is usually your `username.cloudant.com`_
  * Database - _The name of your Cloudant database_

### Deploy the Cloudant template

Click **Deploy**.

After Template deployment, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available Templates.

## Deploying the Get HTTP Resource template
{: #get-http-resource-template}

The Get HTTP Resource template creates an Action to fetch an external resource, the Yahoo Weather API, and then returns data. The Action is enabled as a Web action, allowing it to be invoked with a URL which is CORS enabled and does not need an authentication key, which is useful for building backends for web applications. **Note**: By default, the `get-http-resource` endpoint is publically available to anyone who may want to call it.

1. To create a Template, go to [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/), and then click **Start Creating**.

2. Click **Quickstart Templates**.

3. Review the **Package Name** field, you can update it as needed. The default is set for you as `get-http-resource`.

4. Choose the runtime for the Actions you will own: Node.js 8, Node.js 6, or Python 3.

5. Click **Deploy**.

After Template deployment, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available Templates.

## Deploying the Hello World template
{: #hello-world-template}

This action will accept a single parameter, which must be a JSON object.

1. To create a Template, go to [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/), and then click **Start Creating**.

2. Click **Quickstart Templates**.

3. Review the **Package Name** field, you can update it as needed. The default is set for you as `hello-world`.

4. Choose the runtime for the Actions you will own: Node.js 8, Node.js 6, Python 3, Swift 4, or PHP 7.1.

5. Click **Deploy**.

After Template deployment, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available Templates.

## Deploying the Message Hub Events template
{: #messagehub-events-templates}

The Message Hub Events template creates an Action and a Trigger that kicks off that Action. The Trigger is fired whenever there is a new item added to the Message Hub topic that is chosen during template creation.

1. To create a Template, go to [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/), and then click **Start Creating**.

2. Click **Quickstart Templates**.

3. Review the **Package Name** field, you can update it as needed. The default is set for you as `message-hub-events`.

4. Choose the runtime for the Actions you will own: Node.js 8, Node.js 6, Python 3, Swift 4, or PHP 7.1.

5. Click **Next**.

### Creating the Message Hub trigger

Triggers invoke Actions when they recieve events from event sources. To create a Trigger for the Message Hub template, provide the Trigger with the required Message Hub service instance information.

Review the **Trigger Name** field, you can update it as needed. The default is set for you as `message-hub-events-trgr`.

### Creating the Message Hub service instance

You can choose either:
  * **Create your own instance**
  * **Input your own credentials**

1. For this example, choose **Create your own instance**.

2. A pop-up opens to take you to a new tab with the Message Hub setup page. After you create the Message Hub instance, you must create a set of Service Credentials, and then close the tab to return to this page by clicking **Ok**.

3. Now choose **Input your own credentials** and provide the following information:
  * Username - _Your Message Hub Username_
  * Password - _Your Message Hub password_
  * kafka_admin_url - _Your Message Hub admin REST URL_
  * Database - _The name of your Message Hub database_
  * Topic - _Topic to subscribe to_

### Deploy the Message Hub template

Click **Deploy**.

After Template deployment, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available Templates.

## Deploying the Periodic Slack Reminder template
{: #slack-reminder-template}

The Periodic Slack Reminder template posts to Slack on an interval provided by the user during Trigger creation. Before creating this Template, go to https://api.slack.com/incoming-webhooks to set up the incoming webhooks URL required.

1. Review the **Package Name** field, you can update it as needed. The default is set for you as `periodic-slack-reminder`.

2. Choose the runtime for the Actions you will own: Node.js 8, Node.js 6, Python 3, Swift 4, or PHP 7.1.

3. Under the **Parameters** section, enter the webhook URL into the **Parameter Value** field, and then click **Next**. (Example: https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX).

### Creating the Slack Reminder trigger

Triggers invoke Actions when they recieve events from event sources. To create a Trigger for the Slack Reminder template, provide the Trigger with the required Message Hub service instance information.

1. Review the **Trigger Name** field, you can update it as needed. The default is set for you as `periodic-slack-reminder-trgr`.

2. Next, you can specify the interval in which the Trigger is to fire by using a Pattern or Cron expression. You can select UTC times for Weekdays, Hours, and Minutes. Select your desired inverval options and you will be ready for template deployment.

3. Click **Deploy**.

After Template deployment, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available Templates.