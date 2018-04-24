---

copyright:

  years: 2018

lastupdated: "2018-04-24"

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
| Cloudant Events | When a Cloudant DB has a documented edited or added, log the change in the console. | Node.js, Swift, Python, PHP |
| Get HTTP Resource | A web action that is invoked in response to an HTTP event, and then fetches data from the Yahoo Weather API. | Node.js, Python |
| Hello World | This action will accept a single parameter, which must be a JSON object. | Node.js, Swift, Python, PHP |
| Message Hub Events | When a Message Hub topic has new data added, log the change in the console. | Node.js, Swift, Python, PHP | 
| Periodic Slack Reminder | An action that will post to Slack based on a periodic trigger. | Node.js, Swift, Python, PHP |

## Deploying the Cloudant Events template
{: #cloudant-template}

The Cloudant Template creates a sequence of Actions, and a Trigger that will kick off that Sequence. The Trigger is fired when there is a change in the connected Cloudant DB, which should be a database of cats, with a name and a color. The expected data item is a cat, with a name and a color defined. When a new cat is added to the database, or a current cat edited, the data will be logged to the console.

1. To create a Template, go to [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/), and then click **Start Creating**. 

2. Click **Quickstart Templates**.

3. Click **New Cloudant Item**.

### Creating the Cloudant Action

1. Next, provide a name for your package or use the provided default name `new-cloudant-item`. 

2. Under the **Actions** drop-down, select the runtime for the actions you will own (nodejs, swift, python, or php). For this example, select **nodejs**, and click **Next**.

### Creating the Cloudant Trigger

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

### Deploy the Cloudant Template

1. Click **Deploy**.

After Template deployment, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available Templates.

