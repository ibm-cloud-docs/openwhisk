---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-06"

keywords: deploy, deployment templates, templates, example, quickstart

subcollection: cloud-functions

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Deploying quickstart templates
{: #templates}

{{site.data.keyword.openwhisk}} offers a catalog of templates to help you get started quickly on your next project. Templates are a combination of actions, triggers, sequences. Some templates also incorporate other service instances from {{site.data.keyword.Bluemix_notm}}. By using templates, you can quickly and easily create a project, and start coding right away.
{: shortdesc}
{: #serviceauth}

## Available quickstart templates
{: #available-templates}

| Name | Description | Supported Runtimes |
|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} Events](#cloudant-template) | When a document is edited or added in an {{site.data.keyword.cloudantfull}} database, log the change in the console. | Node.js, Swift, Python, PHP |
| [Upload Image](#cos-upload-image) | Use a web action to upload an image to an {{site.data.keyword.cos_full}} instance's bucket and then retrieve a thumbnail of that image. | Node.js |
| [Get HTTP Resource](#get-http-resource-template) | Invoke a web action by using an HTTP event, and get data from the Yahoo Weather API. | Node.js, Python |
| [Hello World](#hello-world-template) | Create a basic action that accepts a JSON object as a single parameter. | Node.js, Swift, Python, PHP |
| [{{site.data.keyword.messagehub}} Events](#messagehub-events-template) | When new data is added in an {{site.data.keyword.messagehub_full}} topic, log the change in the console. | Node.js, Swift, Python, PHP |
| [Periodic Slack Reminder](#slack-reminder-template) | Use a webhook to Slack based on a periodic trigger. | Node.js, Swift, Python, PHP |

## Deploying the {{site.data.keyword.cloudant_short_notm}} Events template
{: #cloudant-template}

The {{site.data.keyword.cloudant_short_notm}} template creates a sequence of actions and a trigger that kicks off that sequence. The trigger is fired when a change is made in the connected {{site.data.keyword.cloudant_short_notm}} example database of cats. The expected data item is a cat, with a name and a color defined. When a new cat is added to the database or a current cat is edited, the data is logged to the console.

### Deploying the {{site.data.keyword.cloudant_short_notm}} Events template from the UI
{: #cloudant-template-ui}

1. Go to the [Create page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk/create) in the {{site.data.keyword.openwhisk_short}} console.

2. Click **Quickstart templates**.

3. Click **Cloudant Events**.

4. Create the {{site.data.keyword.cloudant_short_notm}} action.
    1. Provide a name for your package or use the provided default name `new-cloudant-item`.
    2. In the **Actions** list, select `Node.js 6` for the cats database example, and click **Next**.

5. Create the {{site.data.keyword.cloudant_short_notm}} trigger. Triggers invoke actions when they receive events from event sources. To create the trigger, you must provide a {{site.data.keyword.messagehub}} service instance and service credentials.
    1. In the **Cloudant Instance** list, select **Create your own instance**. The {{site.data.keyword.cloudant_short_notm}} setup page opens.
    2. Create the {{site.data.keyword.cloudant_short_notm}} service instance.
    3. Create a set of Service Credentials.
    4. Click **OK** to close the {{site.data.keyword.cloudant_short_notm}} setup page and return to the {{site.data.keyword.openwhisk_short}} console.
    5. In the **Cloudant Instance** list, you can now select **Input your own credentials** and provide the following information:
      * Username: Your {{site.data.keyword.cloudant_short_notm}} username
      * Password: Your {{site.data.keyword.cloudant_short_notm}} password
      * Host: `<username>.cloudant.com`
      * Database: The name of your {{site.data.keyword.cloudant_short_notm}} database

5. Click **Deploy**. After template deployment, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Deploying the {{site.data.keyword.cloudant_short_notm}} Events template from the CLI
{: #cloudant-template-cli}

1. Clone the template repo.
    ```
    git clone https://github.com/ibm-functions/template-cloudant-trigger.git
    ```
    {: pre}

2. Navigate to the directory for the action runtime that you want to use: `nodejs-6`, `nodejs`, `php`, `python`, or `swift`.
    ```
    cd template-cloudant-trigger/runtimes/nodejs
    ```
    {: pre}

3. Deploy the template, using the following environment variables.
    ```
    CLOUDANT_HOSTNAME=<host> CLOUDANT_USERNAME=<username> CLOUDANT_PASSWORD=<password> CLOUDANT_DATABASE=<database> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>Understanding the environment variables</caption>
    <thead>
    <th colspan=2>Understanding the environment variables</th>
    </thead>
    <tbody>
    <tr><td><code>CLOUDANT_HOSTNAME</code></td><td><code>&lt;username&gt;.cloudant.com</code></td></tr>
    <tr><td><code>CLOUDANT_USERNAME</code></td><td>Your {{site.data.keyword.cloudant_short_notm}} username</td></tr>
    <tr><td><code>CLOUDANT_PASSWORD</code></td><td>Your {{site.data.keyword.cloudant_short_notm}} password</td></tr>
    <tr><td><code>CLOUDANT_DATABASE</code></td><td>The name of your {{site.data.keyword.cloudant_short_notm}} database</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>A custom name for the package</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>A custom name for the rule</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>A custom name for the trigger</td></tr>
    </tbody></table>

## Deploying the Upload Image template
{: #image-template}

The Upload Image template creates a web action that allows you to upload an image to an {{site.data.keyword.cos_short}} bucket through a small interface. The template then retrieves the image as a thumbnail and displays it on the web action's interface.
{: #cos-upload-image}

### Deploying the Upload Image template from the UI
{: #image-template-ui}

1. Go to the [Create page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk/create) in the {{site.data.keyword.openwhisk_short}} console.

2. Click **Quickstart templates**.

3. Click **Upload Image**.

4. Enter a name for your package or use the default name `upload-image`, and click **Next**.

6. The template requires service credentials from an {{site.data.keyword.cos_full_notm}} service instance. In the **{{site.data.keyword.cos_short}}** list, select one of the following options:
  * **Create an new instance**: If you do not have an existing service instance, select this option to create one.
      1. In the {{site.data.keyword.cos_full_notm}} service instance creation page that opens, create a service instance.
      2. [Create a set of HMAC service credentials](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials).
      3. [Create at least one bucket](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets).
  * **Input your own credentials**: Select this option to manually enter your own credentials for an {{site.data.keyword.cos_short}} service instance. The credentials must have HMAC keys and the service instance must have at least one bucket.
  * **Existing Instances**: If you have any existing {{site.data.keyword.cos_short}} instances, select one of the instances from the list. The credentials must have HMAC keys and the service instance must have at least one bucket.

7. Click **Deploy**.

8. In the left-hand navigation, click **Endpoints**

9. In the **Web Action** section, copy the link without the .json suffix and paste it into your browser's address bar. The interface for the template's web action is displayed.

10. Optional: After the template deploys, you can navigate to the **Actions** dashboard to customize the code in the two new packages:
    * The `cloud-object-storage` package, which contains actions that work with {{site.data.keyword.cos_short}} instances
    * The template package (default name `upload-image`), which contains the `app` action

### Deploying the Upload Image template from the CLI
{: #image-template-cli}

1. [Install the {{site.data.keyword.cos_full_notm}} package](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions#cloud_object_storage_cli).

2. Clone the template repo.
    ```
    git clone https://github.com/ibm-functions/template-cloud-object-storage.git
    ```
    {: pre}

3. Navigate to the `nodejs` runtime directory.
    ```
    cd template-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Deploy the template, using a custom package name and the name of your {{site.data.keyword.cos_short}} bucket as environment variables.
    ```
    PACKAGE_NAME=<name> BUCKET_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. In the [Actions page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk/actions) of the {{site.data.keyword.openwhisk_short}} console, click the `process-change` action.

5. In the left-hand navigation, click **Endpoints**.

6. In the **Web Action** section, copy the link without the .json suffix and paste it into your browser's address bar. The interface for the template's web action is displayed.

## Deploying the Get HTTP Resource template
{: #get-http-resource-template}

The Get HTTP Resource template creates an action to fetch an external resource, the Yahoo Weather API, and then returns data. The action is enabled as a web action, allowing it to be invoked with a URL which is CORS enabled and does not need an authentication key, which is useful for building backends for web applications. **Note**: By default, the `get-http-resource` endpoint is publicly available to anyone who calls it.

### Deploying the Get HTTP Resource template from the UI
{: #get-http-resource-template-ui}

1. Go to the [Create page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk/create) in the {{site.data.keyword.openwhisk_short}} console.

2. Click **Quickstart templates**.

3. Click **Get HTTP Resource**.

3. Enter a name for your package or use the default name `get-http-resource`.

4. Choose the runtime for your actions: Node.js 8, Node.js 6, or Python 3.

5. Click **Deploy**.

6. Invoke the action by curling the following URL: `https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`. For example:
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

After template deployment, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Deploying the Get HTTP Resource template from the CLI
{: #get-http-resource-template-cli}

1. Clone the template repo.
    ```
    git clone https://github.com/ibm-functions/template-get-external-resource.git
    ```
    {: pre}

2. Navigate to the directory for the action runtime that you want to use: `nodejs-6`, `nodejs`, or `python`.
    ```
    cd template-get-external-resource/runtimes/nodejs
    ```
    {: pre}

3. Deploy the template, using a custom package name as an environment variable.
    ```
    PACKAGE_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Invoke the action by curling the following URL: `https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`. For example:
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

## Deploying the Hello World template
{: #hello-world-template}

You can deploy this basic Hello World action to get started with {{site.data.keyword.openwhisk_short}} or to test other entities you create, such as triggers and rules.

### Deploying the Hello World template from the UI
{: #hello-world-template-ui}

1. Go to the [Create page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk/create) in the {{site.data.keyword.openwhisk_short}} console.

2. Click **Quickstart templates**.

3. Click **Hello World**.

4. Enter a name for your package or use the default name `hello-world`.

5. Choose the runtime for your actions: Node.js 8, Node.js 6, Python 3, Swift 4, or PHP 7.1.

6. Click **Deploy**.

After template deployment, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Deploying the Hello World template from the CLI
{: #hello-world-template-cli}

1. Clone the Hello World template repo.
    ```
    git clone https://github.com/ibm-functions/template-hello-world.git
    ```
    {: pre}

2. Navigate to the directory for the action runtime that you want to use: `nodejs-6`, `nodejs`, `php`, `python`, or `swift`.
    ```
    cd template-hello-world/runtimes/nodejs
    ```
    {: pre}

3. Deploy the template.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

## Deploying the {{site.data.keyword.messagehub}} Events template
{: #messagehub-events-template}

The {{site.data.keyword.messagehub}} Events template creates an action and a trigger that kicks off that action. The trigger is fired whenever there is a new item added to the {{site.data.keyword.messagehub}} topic that is chosen during template creation.

### Deploying the {{site.data.keyword.messagehub}} Events template from the UI
{: #messagehub-events-template-ui}

1. Go to the [Create page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk/create) in the {{site.data.keyword.openwhisk_short}} console.

2. Click **Quickstart templates**.

3. Click **{{site.data.keyword.messagehub}} Events**.

4. Create the {{site.data.keyword.messagehub}} action.
    1. Provide a name for your package or use the provided default name `message-hub-events`.
    2. Choose the runtime for your actions: Node.js 8, Node.js 6, Python 3, Swift 4, or PHP 7.1.
    3. Click **Next**.

5. Create the {{site.data.keyword.messagehub}} trigger. Triggers invoke actions when they receive events from event sources. To create the trigger, you must provide a {{site.data.keyword.messagehub}} service instance and service credentials.
    1. In the **MessageHub Instance** list, select **Create your own instance**. The {{site.data.keyword.messagehub}} creation page opens.
    2. Create the {{site.data.keyword.messagehub}} service instance.
    3. Create a set of Service Credentials.
    4. Click **OK** to close the {{site.data.keyword.messagehub}} creation page and return to the {{site.data.keyword.messagehub}} console.
    5. In the **MessageHub Instance** list, you can now select **Input your own credentials** and provide the following information:
      * Username: Your {{site.data.keyword.messagehub}} username
      * Password: Your {{site.data.keyword.messagehub}} password
      * kafka_admin_url: Your {{site.data.keyword.messagehub}} admin REST URL
      * Database: The name of your {{site.data.keyword.messagehub}} database
      * Topic: The topic to subscribe to

5. Click **Deploy**.

After template deployment, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Deploying the {{site.data.keyword.messagehub}} Events template from the CLI
{: #messagehub-events-template-cli}

1. Clone the template repo.
    ```
    git clone https://github.com/ibm-functions/template-messagehub-trigger.git
    ```
    {: pre}

2. Navigate to the directory for the action runtime that you want to use: `nodejs-6`, `nodejs`, `php`, `python`, or `swift`.
    ```
    cd template-messagehub-trigger/runtimes/nodejs
    ```
    {: pre}

3. Deploy the template, using the following environment variables.
    ```
    KAFKA_BROKERS=<host> KAFKA_TOPIC=<topic> MESSAGEHUB_USER=<username> MESSAGEHUB_PASS=<password> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>Understanding the environment variables</caption>
    <thead>
    <th colspan=2>Understanding the environment variables</th>
    </thead>
    <tbody>
    <tr><td><code>KAFKA_BROKERS</code></td><td>Your {{site.data.keyword.messagehub}} admin REST URL</td></tr>
    <tr><td><code>KAFKA_TOPIC</code></td><td>The topic to subscribe to</td></tr>
    <tr><td><code>MESSAGEHUB_USER</code></td><td>Your {{site.data.keyword.messagehub}} username</td></tr>
    <tr><td><code>MESSAGEHUB_PASS</code></td><td>Your {{site.data.keyword.messagehub}} password</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>A custom name for the package</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>A custom name for the rule</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>A custom name for the trigger</td></tr>
    </tbody></table>

## Deploying the Periodic Slack Reminder template
{: #slack-reminder-template}

The Periodic Slack Reminder template posts to Slack on an interval provided by the user during trigger creation.

### Deploying the Periodic Slack Reminder template from the UI
{: #slack-reminder-template-ui}

1. Go to https://api.slack.com/incoming-webhooks to set up the required incoming webhooks URL.

1. Go to the [Create page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk/create) in the {{site.data.keyword.openwhisk_short}} console.

2. Click **Quickstart templates**.

3. Click **Periodic Slack Reminder Events**.

4. Create the Periodic Slack Reminder action.
    1. Provide a name for your package or use the provided default name `periodic-slack-reminder`.
    2. Choose the runtime for your actions: Node.js 8, Node.js 6, Python 3, Swift 4, or PHP 7.1.
    3. In the Parameters section, enter the webhook URL into the **Parameter Value** field, such as `https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX`.
    4. Click **Next**.

5. Create the Periodic Slack Reminder trigger. Triggers invoke actions when they receive events from event sources.
    1. Specify the trigger's time interval by using a Pattern or Cron expression.
        * Pattern: Select UTC times for weekdays, hours, and minutes.
        * Cron: Specify a Cron sequence based on the <a href="http://crontab.org">UNIX crontab syntax</a>. Use 5 or fewer separated by spaces in the format `X X X X X`.
    2. Add the trigger JSON payload.

6. Click **Deploy**.

After template deployment, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Deploying the Periodic Slack Reminder template from the CLI
{: #slack-reminder-template-cli}

1. Go to https://api.slack.com/incoming-webhooks to set up the required incoming webhooks URL.

1. Clone the template repo.
    ```
    git clone https://github.com/ibm-functions/template-reminder-slack.git
    ```
    {: pre}

2. Navigate to the directory for the action runtime that you want to use: `nodejs-6`, `nodejs`, `php`, `python`, or `swift`.
    ```
    cd template-reminder-slack/runtimes/nodejs
    ```
    {: pre}

3. Deploy the template, using the following environment variables.
    ```
    SLACK_WEBHOOK_URL=<url> ALARM_CRON=<cron> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>Understanding the environment variables</caption>
    <thead>
    <th colspan=2>Understanding the environment variables</th>
    </thead>
    <tbody>
    <tr><td><code>SLACK_WEBHOOK_URL</code></td><td>The webhook URL, such as <code>https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX</code></td></tr>
    <tr><td><code>ALARM_CRON</code></td><td>A Cron sequence based on the <a href="http://crontab.org">UNIX crontab syntax</a>. Use 5 or fewer fields separated by spaces in the format <code>X X X X X</code>.</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>A custom name for the package</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>A custom name for the rule</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>A custom name for the trigger</td></tr>
    </tbody></table>
