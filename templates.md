---

copyright:
  years: 2017, 2019
lastupdated: "2019-12-05"

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

{{site.data.keyword.openwhisk}} offers a catalog of templates to help you get started on your next project. Templates are a combination of actions, triggers, sequences. Some templates also incorporate other services from {{site.data.keyword.cloud_notm}}. By using these templates, you can understand how {{site.data.keyword.openwhisk}} entities work together and even use these entities as a basis for your own project.
{: shortdesc}

## Available quickstart templates
{: #available-templates}

The following quickstart templates are available for deployment. You can deploy the templates as is, or you can access the code for each template in its code repository, modify it as you like, and then create your own function.
{: shortdesc}

| Name | Description | Supported Runtimes | Source repository |
|:-----------------|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} Events](#cloudant-template) | When a document is edited or added in an {{site.data.keyword.cloudantfull}} database, log the change in the console. | Node.js, Swift, Python, PHP | [Code](https://github.com/ibm-functions/template-cloudant-trigger){: external} |
| [Get HTTP Resource](#get-http-resource-template) | Invoke a web action by using an HTTP event. | Node.js, Python | [Code](https://github.com/ibm-functions/template-get-external-resource){: external}
| [Hello World](#hello-world-template) | Create a basic action that accepts a JSON object as a single parameter. | Node.js, Swift, Python, PHP | [Code](https://github.com/ibm-functions/template-hello-world){: external}
| [{{site.data.keyword.messagehub_full}}](#messagehub-events-template) | When new data is added in an {{site.data.keyword.messagehub_full}} topic, log the change in the console. | Node.js, Swift, Python, PHP | [Code](https://github.com/ibm-functions/template-messagehub-trigger){: external}
| [Periodic Slack Reminder](#slack-reminder-template) | Use a webhook to Slack based on a periodic trigger. | Node.js, Swift, Python, PHP | [Code](https://github.com/ibm-functions/template-reminder-slack){: external}

## Deploying the {{site.data.keyword.cloudant_short_notm}} Events template
{: #cloudant-template}

The {{site.data.keyword.cloudant_short_notm}} template creates a sequence of actions and a trigger that kicks off that sequence. The trigger is fired when a change is made in the connected {{site.data.keyword.cloudant_short_notm}} example database.
{: shortdesc}

When you deploy this template, you create the following entities:

* An action called `process-change`.
* A sequence that uses the `process change` action as well as a pre-imstalled action for {{site.data.keyword.cloudant_short_notm}} called `read` that reads entries from the {{site.data.keyword.cloudant_short_notm}} database.
* A user-named trigger that is fired whenever an entry is added to the connected database. This trigger calls the sequence.

**Before you begin**

You must have an instance of [{{site.data.keyword.cloudant_short_notm}}](https://cloud.ibm.com/catalog/services/cloudant) set up before you deploy this quick start template. When you create your {{site.data.keyword.cloudant_short_notm}} instance, you must select `Use both legacy credentials and IAM` as your available authentication methods. This template does not work with an {{site.data.keyword.cloudant_short_notm}} instance that uses `IAM only`. 

1. After your {{site.data.keyword.cloudant_short_notm}} instance is provisioned, select the instance from the Resource list and then select **Launch Cloudant Dashboard**.
2. From the {{site.data.keyword.cloudant_short_notm}} Dashboard, click **Create Database**.
3. Create a non-partitioned database called `cats`.
4. From the {{site.data.keyword.cloudant_short_notm}} instance, select **Service credentials**. If you do not have service credentials set up, click **New Credentials** to create them. These values are required for deploying this template.

Now that your instance of {{site.data.keyword.cloudant_short_notm}} is provisioned and a sample database of `cats` set up, you are ready to deploy the {{site.data.keyword.cloudant_short_notm}} Events template.

### Deploying the {{site.data.keyword.cloudant_short_notm}} Events template from the console
{: #cloudant-template-ui}

Deploy the {{site.data.keyword.cloudant_short_notm}} Events template from the console.
{: shortdesc}

**Before you begin**

Select a namespace to contain your {{site.data.keyword.openwhisk}} entities from the [console](https://cloud.ibm.com/openwhisk){: external}. For more information about namespaces, see [Managing namespaces](/docs/openwhisk?topic=cloud-functions-namespaces).


1. Go to the [Create page ](https://cloud.ibm.com/openwhisk/create){: external} in the {{site.data.keyword.openwhisk_short}} console.

2. Click **Quickstart templates**.

3. Click **{{site.data.keyword.cloudant_short_notm}} Events**.

4. Create the {{site.data.keyword.cloudant_short_notm}} action.
    1. Provide a name for your package or use the provided default name `cloudant-events`.
    2. In the **Actions** list, select a runtime (for example, `Node.js` or `Python`) and click **Next**.

5. Create the {{site.data.keyword.cloudant_short_notm}} trigger. Triggers invoke actions when they receive events from event sources. To create the trigger, you must provide a {{site.data.keyword.messagehub}} service instance and service credentials. Provide the following information from your {{site.data.keyword.cloudant_short_notm}} instance:

    * `Username` - Your {{site.data.keyword.cloudant_short_notm}} username.
    * `Password` - Your {{site.data.keyword.cloudant_short_notm}} password.
    * `Host` - `<username>.cloudantnosqldb.appdomain.cloud`. 
    * `Database` - The name of your {{site.data.keyword.cloudant_short_notm}} database: `cats`.
    * `iamApiKey`- Your {{site.data.keyword.cloudant_short_notm}} `iam_apikey_name`.
    * `iamURL` - Your {{site.data.keyword.cloudant_short_notm}} `url`.

6. Click **Deploy**.

After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Deploying the {{site.data.keyword.cloudant_short_notm}} Events template from the CLI
{: #cloudant-template-cli}

Use the CLI to deploy the {{site.data.keyword.cloudant_short_notm}} Events template.
{: shortdesc}

**Before you begin**

* Install the [{{site.data.keyword.openwhisk}}](/docs/openwhisk?topic=cloud-functions-cli_install) CLI.

* Specify a namespace to contain your {{site.data.keyword.openwhisk}} entities. For more information about namespaces, see [Managing namespaces](/docs/openwhisk?topic=cloud-functions-namespaces).


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
    <tr><td><code>CLOUDANT_HOSTNAME</code></td><td><code><username>.cloudantnosqldb.appdomain.cloud</code></td></tr>
    <tr><td><code>CLOUDANT_USERNAME</code></td><td>Your {{site.data.keyword.cloudant_short_notm}} username</td></tr>
    <tr><td><code>CLOUDANT_PASSWORD</code></td><td>Your {{site.data.keyword.cloudant_short_notm}} password</td></tr>
    <tr><td><code>CLOUDANT_DATABASE</code></td><td>The name of your {{site.data.keyword.cloudant_short_notm}} database: `cats`.</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>A custom name for the package</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>A custom name for the rule</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>A custom name for the trigger</td></tr>
    </tbody></table>
    
    Find this information in the console by navigating to your {{site.data.keyword.cloudant_short_notm}} instance and then selecting Service Credentials. You can also find service credentials by running the `ibmcloud resource` command.
    
    1. Find the credentials for your {{site.data.keyword.cloudant_short_notm}} instance.
       
       ```
       ibmcloud resource service-keys --instance-name <Cloudant-name>
       ```
       {: pre}
      
       Example output:
      
       ```
       Name            State    Created At   
       Cloudant-cred   active   Fri Nov  1 17:40:13 UTC 2019 
       ```
       {: screen}
    
    2. Retrieve the details of the service key. Replace `Cloudant-cred` with the credentials that you retrieved in the previous step.
    
       ```
       ibmcloud resource service-key Cloudant-cred
       ```
       {: pre}
      
       Example output:
      
       ```
       Name:          Cloudant-cred  
       ID:            crn:v1:bluemix:public:cloudantnosqldb:us-south:a/6ef045fd2b43266cfe8e6388dd2ec098:397b3dbc-abd3-46cf-a3de-c1d556c8959f:resource-key:a3db5b84-7fe9-4ddb-bf67-e556a278833f   
       Created At:    Fri Nov  1 17:40:13 UTC 2019   
       State:         active   
       Credentials:                                   
               apikey:                   uYowcbhwgOMzO61AJTHzCsPMmG0lB69pzr6helVlE6JF      
               host:                     37c53a06-5193-23f3-b681-7c8b76ce7qaa-bluemix.cloudantnosqldb.appdomain.cloud      
               iam_apikey_description:   Auto-generated for key a3db65f4-7fe9-4ddb-bf67-e588a278753f      
               iam_apikey_name:          Cloudant-cred    
               iam_role_crn:             crn:v1:bluemix:public:iam::::serviceRole:Manager      
               iam_serviceid_crn:        crn:v1:bluemix:public:iam-identity::a/6ef045fd2b43266cfe8e6388dd2ec098::serviceid:ServiceId-132fe428-7a4f-43bf-8c50-b8a169c97988      
               password:                 e2dbc589632a0651ae44ee3920f8cdb02bdcb68e1909b6314c42f5b0d39c043a      
               port:                     423      
               url:                      https://37c53a06-5193-23f3-b681-7c8b76ce7qaa-bluemix:e2ddc589631a0651af44ee3920f9bab02bdcb68e1908b6314c43f5b0d39c043a@337c53a06-5193-23f3-b681-7c8b76ce7qaa-bluemix.cloudantnosqldb.appdomain.cloud      
               username:                 37c53a06-5193-23f3-b681-7c8b76ce7qaa-bluemix      

       ```
       {: screen}
       
       Using the service credential information found in the example output, run the following command:
       
       ```
       CLOUDANT_HOSTNAME=37c53a06-5193-23f3-b681-7c8b76ce7qaa-bluemix.cloudantnosqldb.appdomain.cloud CLOUDANT_USERNAME=37c53a06-5193-23f3-b681-7c8b76ce7qaa-bluemix CLOUDANT_PASSWORD=e2dbc589632a0651ae44ee3920f8cdb02bdcb68e1909b6314c42f5b0d39c043a CLOUDANT_DATABASE=cats PACKAGE_NAME=my-package RULE_NAME=my-rule TRIGGER_NAME=my-trigger ibmcloud fn deploy -m manifest.yaml
       ```
       {: pre}
       

After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Testing your {{site.data.keyword.cloudant_short_notm}} Events package
{: #cloudant-template_test}

Now that you have your {{site.data.keyword.cloudant_short_notm}}l database and your {{site.data.keyword.cloudant_short_notm}} events (action and trigger) set up, let's try it out!  The expected data item is a cat, with a name and a color defined. When a new cat is added to the database or a current cat is edited, the data is logged to the console module for Node.js.
{: shortdesc}

#### 1. Testing the action
{: #cloudant-template_test_action}

Before you make entries in the {{site.data.keyword.cloudant_short_notm}} database, make sure that the `process change` action is working properly. Note that this entry does not update the {{site.data.keyword.cloudant_short_notm}} database, but instead is a method of testing the action itself.
{: shortdesc}

1. Open the [Actions page ](https://cloud.ibm.com/openwhisk/actions){: external} from the {{site.data.keyword.openwhisk_short}} console.

2. From the {{site.data.keyword.cloudant_short_notm}} events package (called `cloudant-events`, if you accepted the default name), select **Manage Action** from the `process change` action.

3. Click **Change input** and enter a valid json entry for `name` and `color`, similar to the following example:

   ```
   {
       "name": "Tarball",
       "color": "Black"
   }
   ```
   {: pre}

4. Click **Apply**.

5. Click **Invoke**.  In the Activation pane, find output similar to the following example:

   ```
   **Activation ID:**
   528414375ae544bc8414375ae5d4bcc9

   **Results:**
   {
      "change": "A Black cat named Tarball was added"
   }

   **Logs:**
   [
      "2019-11-01T18:14:52.056570Z    stdout: A Black cat named Tarball was added"
   ]
   ```
   {: screen}

Congratulations! The action is functional. 

#### 1. Testing the trigger
{: #cloudant-template_test_trigger}

While you can test the action, you cannot test the trigger without making an entry to the {{site.data.keyword.cloudant_short_notm}} database. Try that next.
{: shortdesc}

1. From the command line, run the [`activation poll`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_activation_poll) command to view a streaming, live list of activations for your namespace.  

   ```
   ibmcloud fn activation poll
   ```
   {: pre}

2. From the [IBM Cloud resource](https://cloud.ibm.com/resources){: external} list, select your {{site.data.keyword.cloudant_short_notm}} instance and click **Launch Cloudant Dashboard**.  Note that you might be prompted for your {{site.data.keyword.cloudant_short_notm}} user name and password. You can find them on the service credentials for your {{site.data.keyword.cloudant_short_notm}} instance.

3. Select your `cats` database.

4. Click **Create Document**.

5. Enter valid Json and click **Create Document**.

   ```
   {
       "name": "Tarball",
       "color": "Black"
   }
   ```
   {: pre}

6. Check the activation poll to see if the trigger fired.

   ```
   Activation: 'process-change' (874b327c6d2841ca8b327c6d28d1ca17)
   [
       "2019-11-01T18:47:16.966276Z    stdout: A Black cat named Tarball was added"
   ]

   Activation: 'read' (375f8e39b5c445f39f8e39b5c415f36e)
   [
       "2019-11-01T18:47:16.915421Z    stdout: success { _id: '48ef87b2e1905a488606960b8368f835',",
       "2019-11-01T18:47:16.915453Z    stdout: _rev: '1-39df67b063efa068a7b869e81e017596',",
       "2019-11-01T18:47:16.915458Z    stdout: name: 'Tarball',",
       "2019-11-01T18:47:16.915480Z    stdout: color: 'Black' }"
   ]

   Activation: 'process-change-cloudant-sequence' (68a7083b088f4be9a7083b088f0be9b4)
   [
       "375f8e39b5c445f39f8e39b5c415f36e",
       "874b327c6d2841ca8b327c6d28d1ca17"
   ]

   Activation: 'cloudant-events-trgr' (fbf92f75057b42a9b92f75057b32a9b7)
   [
       "{\"statusCode\":0,\"success\":true,\"activationId\":\"68a7083b088f4be9a7083b088f0be9b4\",\"rule\":\"86ee32b0-8094-4d1e-a612-83dC74cddeb9/cloudant-events-rule-5\",\"action\":\"86ee32b0-8094-4d1e-a612-83dC74cddeb9/cloudant-events/process-change-cloudant-sequence\"}"
    ```
    {: screen}



## Deploying the Get HTTP Resource template
{: #get-http-resource-template}

The **Get HTTP Resource** template creates an action, which takes as input a location parameter. The action is enabled as a web action, allowing it to be invoked with a URL, which is CORS enabled and does not need an authentication key, which is useful for building backends for web applications.
{: shortdesc}

When you deploy this template, you create a single, web-enabled action called `location`. This action accepts a single parameter in JSON format, `{ "location": "xxxx" }`.

By default, the `get-http-resource` endpoint is publicly available to anyone who calls it.
{: note}

### Deploying the Get HTTP Resource template from the console
{: #get-http-resource-template-ui}

Use the console to deploy the **Get HTTP Resource** template.
{: shortdesc}

**Before you begin**

Select a namespace to contain your {{site.data.keyword.openwhisk}} entities from the [console](https://cloud.ibm.com/openwhisk){: external}. For more information about namespaces, see [Managing namespaces](/docs/openwhisk?topic=cloud-functions-namespaces).


1. Go to the [Create page ](https://cloud.ibm.com/openwhisk/create){: external} in the {{site.data.keyword.openwhisk_short}} console.

2. Click **Quickstart templates**.

3. Click **Get HTTP Resource**.

4. Enter a name for your package or use the default name `get-http-resource`.

5. Choose the runtime for your actions.

6. Click **Deploy**.

After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Deploying the Get HTTP Resource template from the CLI
{: #get-http-resource-template-cli}

Use the CLI to deploy the **Get HTTP Resource** template.
{: shortdesc}

**Before you begin**

* Install the [{{site.data.keyword.openwhisk}}](/docs/openwhisk?topic=cloud-functions-cli_install) CLI.

* Specify a namespace to contain your {{site.data.keyword.openwhisk}} entities. For more information about namespaces, see [Managing namespaces](/docs/openwhisk?topic=cloud-functions-namespaces).


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

After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Invoking the **Get HTTP Resource** action
{: #get-http-resource-template-invoke}

Test out the **Get HTTP Resource** action using one of the following methods:
{: shortdesc}

   * From the {{site.data.keyword.openwhisk_short}} [Actions](https://cloud.ibm.com/functions/actions){: external} page, select the action and then click **Invoke**.
   
     You can change the input parameter by selecting **Change Input** and then entering valid JSON in the following format:

     ```
     {
        "location": "<new location>"
     }
     ```
     {: pre}

     For example:

     ```
     {
        "location": "Paris"
     }
     ```
     {: pre}

   * Opening a URL by using the following structure `https://<apihost>/api/v1/web/<namespace>/<package name>/location.html` in your browser.
     You can get the URL for any action by running this command:

     ```
     ibmcloud fn action get <action_name> --url
     ```
     {: pre}
     
     Add a parameter for location by appending `?location=<city>` to the end of the URL. For example:
     
     ```
     https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/get-http-resource/location?location=Paris
     ```
     {: pre}

   * Curling the following URL: `https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/<package_name>/location?location=<city>`. For example:

     ```
     curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/get-http-resource/location?location=Paris
     ```
     {: pre}
     
   All of these invokations return the default location `Austin` or else the location parameter that you specified in JSON format.  
   
   **Example output**
   
   ```
   {
     "body": {
     "location": "Paris"
     },
     "headers": {
       "Content-Type": "application/json"
     },
     "statusCode": 200
   }
   ```
   {: screen}

## Deploying the Hello World template
{: #hello-world-template}

You can deploy this basic **Hello World** action to get started with {{site.data.keyword.openwhisk_short}} or to test other entities you create, such as triggers and rules.
{: shortdesc}

When you deploy this template, you create a single action called `helloworld`. This action accepts a single parameter in JSON format, `{ "name": "xxxx" }`.

### Deploying the Hello World template from the console
{: #hello-world-template-ui}

You can deploy the **Hello World** template from the console.
{: shortdesc}

**Before you begin**

Select a namespace to contain your {{site.data.keyword.openwhisk}} entities from the [console](https://cloud.ibm.com/openwhisk){: external}. For more information about namespaces, see [Managing namespaces](/docs/openwhisk?topic=cloud-functions-namespaces).


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

**Before you begin**

* Install the [{{site.data.keyword.openwhisk}}](/docs/openwhisk?topic=cloud-functions-cli_install) CLI.

* Specify a namespace to contain your {{site.data.keyword.openwhisk}} entities. For more information about namespaces, see [Managing namespaces](/docs/openwhisk?topic=cloud-functions-namespaces).


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

3. Deploy the template.  You must include a package name to contain your action. Replace `<name>` with a custom name for your package.

    ```
    PACKAGE_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Testing the Hello World action
{: #hello-world-template-test

You can test the **Hello World** template in the following ways.
{: shortdesc}

From the {{site.data.keyword.openwhisk_short}} [Actions](https://cloud.ibm.com/functions/actions){: external} page, select the action and then click **Invoke**.

Invoking the action without any parameters returns the following output:

```
**Results**:
{
  "greeting": "Hello stranger!"
}
```
{: screen}

You can change the input parameter by selecting **Change Input** and then entering valid JSON in the following format:

```
{ "name": "xxxx" }
```
{: pre}

For example:

```
{ "name": "Carl" }
```
{: pre}
     
**Example output**
     
```
**Results**:
{
  "greeting": "Hello Carl"
}
```
{: screen}


## Deploying the {{site.data.keyword.messagehub_full}} template
{: #messagehub-events-template}

The {{site.data.keyword.messagehub}} Events template creates an action and a trigger that kicks off that action. The trigger is fired whenever a new item is added to the {{site.data.keyword.messagehub}} topic that is chosen when you create the template.
{: shortdesc}

When you deploy this template, you create the following entities:

* An action called `process-message`.
* A user-named trigger that is fired whenever an entry is streamed. This trigger calls the action.

### Deploying the {{site.data.keyword.messagehub}} template from the console
{: #messagehub-events-template-ui}

Deploy the **{{site.data.keyword.messagehub}}** template from the console.
{: shortdesc}

**Before you begin**

Select a namespace to contain your {{site.data.keyword.openwhisk}} entities from the [console](https://cloud.ibm.com/openwhisk){: external}. For more information about namespaces, see [Managing namespaces](/docs/openwhisk?topic=cloud-functions-namespaces).


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

**Before you begin**

* Install the [{{site.data.keyword.openwhisk}}](/docs/openwhisk?topic=cloud-functions-cli_install) CLI.

* Specify a namespace to contain your {{site.data.keyword.openwhisk}} entities. For more information about namespaces, see [Managing namespaces](/docs/openwhisk?topic=cloud-functions-namespaces).


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

The **Periodic Slack Reminder** template posts to Slack on an interval provided by you when you create the trigger.
{: shortdesc}

When you deploy this template, you create the following entities:

* An action called `send-message`.
* A sequence that uses the `send-message` action as well as a pre-installed action for Slack called `post` that posts the message to a Slack channel.
* A user-named trigger that is fired whenever the time interval has been achieved. This trigger calls the sequence.

**Before you begin**

Set up the required incoming webhooks URL in the Slack API. For more information, see [Incoming Webhooks](https://api.slack.com/incoming-webhooks){: external}.  You need this URL before you deploy the template.

### Deploying the Periodic Slack Reminder template from the console
{: #slack-reminder-template-ui}

Deploy the **Periodic Slack Reminder** template from the console.
{: shortdesc}

**Before you begin**

Select a namespace to contain your {{site.data.keyword.openwhisk}} entities from the [console](https://cloud.ibm.com/openwhisk){: external}. For more information about namespaces, see [Managing namespaces](/docs/openwhisk?topic=cloud-functions-namespaces).


1. Go to the [Create page ](https://cloud.ibm.com/openwhisk/create){: external} in the {{site.data.keyword.openwhisk_short}} console.

2. Click **Quickstart templates**.

3. Click **Periodic Slack Reminder**.

4. Create the Periodic Slack Reminder action.
    1. Provide a name for your package or use the provided default name `periodic-slack-reminder`.
    2. Choose the runtime for your actions.
    3. In the Parameters section, enter the webhook URL into the **Parameter Value** field, such as `https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX`.
    4. Click **Next**.

5. Create the Periodic Slack Reminder trigger. Triggers invoke actions when they receive events from event sources. Specify the trigger's time interval by using a Pattern or `Cron` expression.
     * `Pattern` - Select UTC times for weekdays, hours, and minutes.
     * `Cron` - Specify a cron sequence based on the <a href="http://crontab.org">UNIX crontab syntax</a>. Use five or fewer separated by spaces in the format `X X X X X`.

6. Click **Deploy**.

After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Deploying the Periodic Slack Reminder template from the CLI
{: #slack-reminder-template-cli}

Use the CLI to deploy the **Periodic Slack Reminder** template.
{: shortdesc}

**Before you begin**

* Install the [{{site.data.keyword.openwhisk}}](/docs/openwhisk?topic=cloud-functions-cli_install) CLI.

* Specify a namespace to contain your {{site.data.keyword.openwhisk}} entities. For more information about namespaces, see [Managing namespaces](/docs/openwhisk?topic=cloud-functions-namespaces).


1. Clone the template repo.

    ```
    git clone https://github.com/ibm-functions/template-reminder-slack.git
    ```
    {: pre}

2. Navigate to the directory for the action runtime that you want to use. For example, `nodejs`.

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
    <tr><td><code>ALARM_CRON</code></td><td>A cron sequence based on the <a href="http://crontab.org">UNIX crontab syntax</a>. Use 5 or fewer fields in the format <code>X X X X X</code>.</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>A custom name for the package</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>A custom name for the rule</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>A custom name for the trigger</td></tr>
    </tbody></table>
    
    Depending on your operating system, you might need to add quotation marks around your options. For example, on a mac, the following example deploys correctly:
    
    ```
    SLACK_WEBHOOK_URL="https://hooks.slack.com/services/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" ALARM_CRON="* * * * *" PACKAGE_NAME="slackpackage" RULE_NAME="slackrule" TRIGGER_NAME="slacktrigger" ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

After the template deploys, you can make further edits to the code to customize it as needed, or go back and check out the catalog of available templates.

### Testing the Periodic Slack Reminder template
{: #periodic-slack-template-test

You can test the **Periodic Slack Reminder** template in the following ways.
{: shortdesc}

   * Your trigger fires as soon as the rule is satisfied, so depending on what value you set for `cron` or `pattern`, your trigger automatically sends a message. For example, if you set `cron` to `* * * * *`, your trigger fires every minute and sends a message to the designated slack channel. 
   
   **Example output**
   
   ```
   Your scrum is starting now.  Time to find your team!
   Your scrum is starting now.  Time to find your team!
   Your scrum is starting now.  Time to find your team!
   Your scrum is starting now.  Time to find your team!
   Your scrum is starting now.  Time to find your team!
   ```
   {: screen}
   
   * From the {{site.data.keyword.openwhisk_short}} [Actions](https://cloud.ibm.com/functions/actions){: external} page, select the action and then click **Invoke**.
     You can change the input parameter by selecting **Change Input** and then entering valid JSON in the following format:

     ```
     { "message": "xxxx" }
     ```
     {: pre}

     For example:

     ```
     { "message": "Time for a walk" }
     ```
     {: pre}
     
 ### Pausing or stopping your Periodic Slack Reminder trigger
{: #periodic-slack-stop-trigger

You can stop the **Periodic Slack Reminder** trigger in the following ways.
{: shortdesc}

   * From the console, go to the [Triggers ](https://cloud.ibm.com/functions/triggers){: external} page and select the  **Periodic Slack Reminder** trigger. Uncheck **Enable**.
   
   * From the CLI:
   
      *  Disable the rule by runnning the [`ibmcloud fn rule disable RULE_NAME`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_rule_disable) command where `RULE_NAME` is the name of the rule that you created for the **Periodic Slack Reminder** trigger.  You can find rule names in your name space by running the [`ibmcloud fn rule list RULE_NAME`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_rule_list) command.
   
     * Edit the trigger by running the [`ibmcloud fn trigger update`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_trigger_update) command.
