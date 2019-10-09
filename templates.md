---

copyright:
  years: 2017, 2019
lastupdated: "2019-10-09"

keywords: deploy, deployment templates, templates, example, quickstart, functions, serverless

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}

# Deploying quickstart templates
{: #templates}

{{site.data.keyword.openwhisk}} offers a catalog of templates to help you get started quickly on your next project. Templates are a combination of actions, triggers, sequences. Some templates also incorporate other service instances from {{site.data.keyword.cloud_notm}}. By using templates, you can quickly and easily create a project, and start coding right away.
{: shortdesc}

## Available quickstart templates
{: #available-templates}

The following quickstart templates are available for deployment. You can deploy the templates as is, or you can access the code for each template in its code repository, modify it as you like, and then create your own function.
{: shortdesc}

| Name | Description | Supported Runtimes | Source repository |
|:-----------------|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} Events](#cloudant-template) | When a document is edited or added in an {{site.data.keyword.cloudantfull}} database, log the change in the console. | Node.js, Swift, Python, PHP | [Code](https://github.com/ibm-functions/template-cloudant-trigger){: external} |
| [Get HTTP Resource](#get-http-resource-template) | Invoke a web action by using an HTTP event, and get data from the Yahoo Weather API. | Node.js, Python | [Code](https://github.com/ibm-functions/template-get-external-resource){: external}
| [Hello World](#hello-world-template) | Create a basic action that accepts a JSON object as a single parameter. | Node.js, Swift, Python, PHP | [Code](https://github.com/ibm-functions/template-hello-world){: external}
| [{{site.data.keyword.messagehub}} Events](#messagehub-events-template) | When new data is added in an {{site.data.keyword.messagehub_full}} topic, log the change in the console. | Node.js, Swift, Python, PHP | [Code](https://github.com/ibm-functions/template-messagehub-trigger){: external}
| [Periodic Slack Reminder](#slack-reminder-template) | Use a webhook to Slack based on a periodic trigger. | Node.js, Swift, Python, PHP | [Code](https://github.com/ibm-functions/template-reminder-slack){: external}

## Deploying the {{site.data.keyword.cloudant_short_notm}} Events template
{: #cloudant-template}

The {{site.data.keyword.cloudant_short_notm}} template creates a sequence of actions and a trigger that kicks off that sequence. The trigger is fired when a change is made in the connected {{site.data.keyword.cloudant_short_notm}} example database of cats. The expected data item is a cat, with a name and a color defined. When a new cat is added to the database or a current cat is edited, the data is logged to the console.
{: shortdesc}

### Deploying the {{site.data.keyword.cloudant_short_notm}} Events template from the console
{: #cloudant-template-ui}

Deploy the {{site.data.keyword.cloudant_short_notm}} Events template from the console.
{: shortdesc}

1. Go to the [Create page ](https://cloud.ibm.com/openwhisk/create){: external} in the {{site.data.keyword.openwhisk_short}} console.

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
    5. In the **Cloudant Instance** list, you can now select **Input your own credentials** and provide the following information.

        * `Username` - Your {{site.data.keyword.cloudant_short_notm}} username.
        * `Password` - Your {{site.data.keyword.cloudant_short_notm}} password.
        * `Host` - `<username>.cloudant.com`. 
        * `Database` - The name of your {{site.data.keyword.cloudant_short_notm}} database.

6. Click **Deploy**.

After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Deploying the {{site.data.keyword.cloudant_short_notm}} Events template from the CLI
{: #cloudant-template-cli}

Use the CLI to deploy the {{site.data.keyword.cloudant_short_notm}} Events template.
{: shortdesc}

1. Clone the template repo.

    ```
    git clone https://github.com/ibm-functions/template-cloudant-trigger.git
    ```
    {: pre}

2. Navigate to the directory for the action runtime that you want to use.  For example, `nodejs`.

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

After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.


## Deploying the Get HTTP Resource template
{: #get-http-resource-template}

The **Get HTTP Resource** template creates an action to fetch an external resource, the Yahoo Weather API, and then returns data. The action is enabled as a web action, allowing it to be invoked with a URL, which is CORS enabled and does not need an authentication key, which is useful for building backends for web applications.
{: shortdesc}

By default, the `get-http-resource` endpoint is publicly available to anyone who calls it.
{: note}

### Deploying the **Get HTTP Resource** template from the console
{: #get-http-resource-template-ui}

Use the console to deploy the **Get HTTP Resource** template.
{: shortdesc}

1. Go to the [Create page ](https://cloud.ibm.com/openwhisk/create){: external} in the {{site.data.keyword.openwhisk_short}} console.

2. Click **Quickstart templates**.

3. Click **Get HTTP Resource**.

4. Enter a name for your package or use the default name `get-http-resource`.

5. Choose the runtime for your actions.

6. Click **Deploy**.

7. Invoke the action by curling the following URL: `https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/<package_name>/weather?location=<city>`. For example:

    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Deploying the Get HTTP Resource template from the CLI
{: #get-http-resource-template-cli}

Use the CLI to deploy the **Get HTTP Resource** template.
{: shortdesc}

1. Clone the template repo.

    ```
    git clone https://github.com/ibm-functions/template-get-external-resource.git
    ```
    {: pre}

2. Navigate to the directory for the action runtime that you want to use. For example, `nodejs`.

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

After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

## Deploying the Hello World template
{: #hello-world-template}

You can deploy this basic **Hello World** action to get started with {{site.data.keyword.openwhisk_short}} or to test other entities you create, such as triggers and rules.
{: shortdesc}

### Deploying the Hello World template from the console
{: #hello-world-template-ui}

You can deploy the **Hello World** template from the console.
{: shortdesc}

1. Go to the [Create page ](https://cloud.ibm.com/openwhisk/create){: external} in the {{site.data.keyword.openwhisk_short}} console.

2. Click **Quickstart templates**.

3. Click **Hello World**.

4. Enter a name for your package or use the default name `hello-world`.

5. Choose the runtime for your actions.

6. Click **Deploy**.

After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Deploying the Hello World template from the CLI
{: #hello-world-template-cli}

Use the CLI to deploy the **Hello World** template.
{: shortdesc}

1. Clone the Hello World template repo.

    ```
    git clone https://github.com/ibm-functions/template-hello-world.git
    ```
    {: pre}

2. Navigate to the directory for the action runtime that you want to use. For example, `nodejs`.

    ```
    cd template-hello-world/runtimes/nodejs
    ```
    {: pre}

3. Deploy the template.

    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

## Deploying the {{site.data.keyword.messagehub}} Events template
{: #messagehub-events-template}

The {{site.data.keyword.messagehub}} Events template creates an action and a trigger that kicks off that action. The trigger is fired whenever a new item is added to the {{site.data.keyword.messagehub}} topic that is chosen during template creation.
{: shortdesc}

### Deploying the {{site.data.keyword.messagehub}} Events template from the console
{: #messagehub-events-template-ui}

Deploy the **{{site.data.keyword.messagehub}} Events** template from the console.
{: shortdesc}

1. Go to the [Create page ](https://cloud.ibm.com/openwhisk/create){: external} in the {{site.data.keyword.openwhisk_short}} console.

2. Click **Quickstart templates**.

3. Click **{{site.data.keyword.messagehub}} Events**.

4. Create the {{site.data.keyword.messagehub}} action.
    1. Provide a name for your package or use the provided default name `message-hub-events`.
    2. Choose the runtime for your actions.
    3. Click **Next**.

5. Create the {{site.data.keyword.messagehub}} trigger. Triggers invoke actions when they receive events from event sources. To create the trigger, you must provide an {{site.data.keyword.messagehub}} service instance and service credentials.
    1. In the **MessageHub Instance** list, select **Create your own instance**. The {{site.data.keyword.messagehub}} creation page opens.
    2. Create the {{site.data.keyword.messagehub}} service instance.
    3. Create a set of Service Credentials.
    4. Click **OK** to close the {{site.data.keyword.messagehub}} creation page and return to the {{site.data.keyword.messagehub}} console.
    5. In the **MessageHub Instance** list, you can now select **Input your own credentials** and provide the following information:
        * `Username` - Your {{site.data.keyword.messagehub}} username.
        * `Password` - Your {{site.data.keyword.messagehub}} password.
        * `kafka_admin_url` - Your {{site.data.keyword.messagehub}} admin REST URL.
        * `Database` - The name of your {{site.data.keyword.messagehub}} database.
        * `Topic` - The topic to subscribe to.

6. Click **Deploy**.

After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Deploying the {{site.data.keyword.messagehub}} Events template from the CLI
{: #messagehub-events-template-cli}

Use the CLI to deploy the **{{site.data.keyword.messagehub}} Events** template.
{: shortdesc}

1. Clone the template repo.

    ```
    git clone https://github.com/ibm-functions/template-messagehub-trigger.git
    ```
    {: pre}

2. Navigate to the directory for the action runtime that you want to use. For example, `nodejs`.

    ```
    cd template-messagehub-trigger/runtimes/nodejs
    ```
    {: pre}

3. Deploy the template by using the following environment variables.

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
    
After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

## Deploying the Periodic Slack Reminder template
{: #slack-reminder-template}

The **Periodic Slack Reminder** template posts to Slack on an interval provided by the user during trigger creation.
{: shortdesc}

### Deploying the Periodic Slack Reminder template from the console
{: #slack-reminder-template-ui}

Deploy the **Periodic Slack Reminder** template from the console.
{: shortdesc}

1. Go to [Incoming Webhooks](https://api.slack.com/incoming-webhooks){: external} in the Slack API to set up the required incoming webhooks URL.

2. Go to the [Create page ](https://cloud.ibm.com/openwhisk/create){: external} in the {{site.data.keyword.openwhisk_short}} console.

3. Click **Quickstart templates**.

4. Click **Periodic Slack Reminder**.

5. Create the Periodic Slack Reminder action.
    1. Provide a name for your package or use the provided default name `periodic-slack-reminder`.
    2. Choose the runtime for your actions.
    3. In the Parameters section, enter the webhook URL into the **Parameter Value** field, such as `https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX`.
    4. Click **Next**.

6. Create the Periodic Slack Reminder trigger. Triggers invoke actions when they receive events from event sources.
    1. Specify the trigger's time interval by using a Pattern or `Cron` expression.
        * `Pattern` - Select UTC times for weekdays, hours, and minutes.
        * `Cron` - Specify a cron sequence based on the <a href="http://crontab.org">UNIX crontab syntax</a>. Use five or fewer separated by spaces in the format `X X X X X`.
    2. Add the trigger JSON payload.

7. Click **Deploy**.

After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Deploying the Periodic Slack Reminder template from the CLI
{: #slack-reminder-template-cli}

Use the CLI to deploy the **Periodic Slack Reminder** template.
{: shortdesc}

1. Go to [Incoming Webhooks](https://api.slack.com/incoming-webhooks){: external} in the Slack API to set up the required incoming webhooks URL.

2. Clone the template repo.

    ```
    git clone https://github.com/ibm-functions/template-reminder-slack.git
    ```
    {: pre}

3. Navigate to the directory for the action runtime that you want to use. For example, `nodejs`.

    ```
    cd template-reminder-slack/runtimes/nodejs
    ```
    {: pre}

4. Deploy the template, using the following environment variables.

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
    <tr><td><code>ALARM_CRON</code></td><td>A cron sequence based on the <a href="http://crontab.org">UNIX crontab syntax</a>. Use 5 or fewer fields in the format <code>X X X X X</code>.</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>A custom name for the package</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>A custom name for the rule</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>A custom name for the trigger</td></tr>
    </tbody></table>

After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.
