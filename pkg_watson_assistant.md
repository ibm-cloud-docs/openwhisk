---

copyright:
  years: 2017, 2020
lastupdated: "2020-10-09"

keywords: openwhisk, functions

subcollection: openwhisk

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

# {{site.data.keyword.conversationshort}}
{: #pkg_watson_assistant}

The installable {{site.data.keyword.conversationfull}} service combines machine learning, natural language understanding, and integrated dialog tools to create conversation flows between your apps and your users. For more information about this service, see [{{site.data.keyword.conversationfull}}](/docs/assistant?topic=assistant-getting-started).
{: shortdesc}

The {{site.data.keyword.conversationshort}} package contains the following entities. For more information, see the {{site.data.keyword.conversationshort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`assistant-v1`](https://cloud.ibm.com/apidocs/assistant/assistant-v1){: external} | Package | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`  | Work with the {{site.data.keyword.conversationshort}} service. |
| [`message`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#get-response-to-user-input){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `assistant_id`,   | Get the response to user input. |
| [`create-workspace`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#create-workspace){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `description`, `language`, `intents`, `entities`, `dialog_nodes`, `counterexamples`, `metadata`, `learning_opt_out`  | Create a workspace. |
| [`delete-workspace`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#delete-workspace){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`  | Delete a workspace. |
| [`get-workspace`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#get-information-about-a-workspace){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `export`, `include_audit`  | Get information about a workspace. |
| [`list-workspaces`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#list-workspaces){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | List workspaces. |
| [`update-workspace`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#update-workspace){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `name`, `description`, `language`, `intents`, `entities`, dialog_nodes, counterexamples, `metadata`, `learning_opt_out`, `append`  | Update a workspace. |
| [`create-intent`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#create-intent){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `description`, `examples`  | Create an intent. |
| [`delete-intent`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#delete-intent){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`  | Delete an intent. |
| [`get-intent`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#get-intent){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `export`, `include_audit`  | Get information about an intent. |
| [`list-intents`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#list-intents){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `export`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | List intents. |
| [`update-intent`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#update-intent){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `new_intent`, `new_description`, `new_examples`  | Update an intent. |
| [`create-example`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#create-user-input-example){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `text`  | Create a user input example. |
| [`delete-example`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#delete-user-input-example){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `text`  | Delete a user input example. |
| [`get-example`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#get-user-input-example){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `text`, `include_audit`  | Get information about a user input example. |
| [`list-examples`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#list-user-input-examples){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | List user input examples. |
| [`update-example`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#update-user-input-example){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `text`, `new_text`  | Update a user input example. |
| [`create-counterexample`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#create-counterexample){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `text`  | Create a counterexample. |
| [`delete-counterexample`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#delete-counterexample){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `text`  | Delete a counterexample. |
| [`get-counterexample`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#get-counterexample){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `text`, `include_audit`  | Get information about a counterexample. |
| [`list-counterexamples`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#list-counterexamples){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | List counterexamples. |
| [`update-counterexample`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#update-counterexample){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `text`, `new_text`  | Update a counterexample. |
| [`create-entity`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#create-entity){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `description`, `metadata`, `values`, `fuzzy_match`  | Create an entity. |
| [`delete-entity`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#delete-entity){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`  | Delete an entity. |
| [`get-entity`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#get-entity){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `export`, `include_audit`  | Get information about an entity. |
| [`list-entities`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#list-entities){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `export`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | List entities. |
| [`update-entity`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#update-entity){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `new_entity`, `new_description`, `new_metadata`, `new_fuzzy_match`, `new_values`  | Update an entity. |
| [`create-value`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#create-entity-value){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `metadata`, `synonyms`, `patterns`, `value_type`  | Add an entity value. |
| [`delete-value`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#delete-entity-value){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`  | Delete an entity value. |
| [`get-value`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#get-entity-value){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `export`, `include_audit`  | Get an entity value. |
| [`list-values`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#list-entity-values){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `export`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | List entity values. |
| [`update-value`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#update-entity-value){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `new_value`, `new_metadata`, `new_type`, `new_synonyms`, `new_patterns`  | Update an entity value. |
| [`create-synonym`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#create-entity-value-synonym){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `synonym`  | Add an entity value synonym. |
| [`delete-synonym`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#delete-entity-value-synonym){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `synonym`  | Delete an entity value synonym. |
| [`get-synonym`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#get-entity-value-synonym){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `synonym`, `include_audit`  | Get an entity value synonym. |
| [`list-synonyms`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#list-entity-value-synonyms){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | List entity value synonyms. |
| [`update-synonym`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#update-entity-value-synonym){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `synonym`, `new_synonym`  | Update an entity value synonym. |
| [`create-dialog-node`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#create-dialog-node){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `dialog_node`, `description`, `conditions`, `parent`, `previous_sibling`, `output`, `context`, `metadata`, `next_step`, `actions`, `title`, `node_type`, `event_name`, `variable`, `digress_in`, `digress_out`, `digress_out_slots`  | Create a dialog node. |
| [`delete-dialog-node`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#delete-dialog-node){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, dialog_node  | Delete a dialog node. |
| [`get-dialog-node`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#get-dialog-node){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `dialog_node`, `include_audit`  | Get a dialog node. |
| [`list-dialog-nodes`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#list-dialog-nodes){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | List dialog nodes. |
| [`update-dialog-node`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#update-dialog-node){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `dialog_node`, `new_dialog_node`, `new_description`, `new_conditions`, `new_parent`, `new_previous_sibling`, `new_output`, `new_context`, `new_metadata`, `new_next_step`, `new_title`, `new_type`, `new_event_name`, `new_variable`, `new_actions`, `new_digress_in`, `new_digress_out`, `new_digress_out_slots`  | Update a dialog node. |
| [`list-all-logs`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#list-log-events-in-all-workspaces){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `filter`, `sort`, `page_limit`, `cursor`  | List log events in all workspaces. |
| [`list-logs`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#list-log-events-in-a-workspace){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `sort`, `filter`, `page_limit`, `cursor`  | List log events in a workspace. |
| [`delete-user-data`](https://cloud.ibm.com/apidocs/assistant/assistant-v1#delete-labeled-data){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id`  | Delete labeled data. |



## Creating a {{site.data.keyword.conversationshort}} service instance
{: #service_instance_conversation}

Before you install the package, you must create a {{site.data.keyword.conversationshort}} service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.conversationshort}} service instance ](https://cloud.ibm.com/catalog/services/conversation){: external}.
2. When the service instance is created, auto-generated service credentials are also created for you.

## Installing the {{site.data.keyword.conversationshort}} package
{: #install_conversation}

After you have a {{site.data.keyword.conversationshort}} service instance, install the {{site.data.keyword.conversationshort}} package into your namespace.
{: shortdesc}

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #conversation_cli}

Install the {{site.data.keyword.conversationshort}} package from the CLI. Be sure to [install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=openwhisk-cli_install) first.
{: shortdesc}

To install the {{site.data.keyword.conversationshort}} package, run the following commands.

1. Clone the {{site.data.keyword.conversationshort}} package repo.

   ```
   git clone https://github.com/watson-developer-cloud/openwhisk-sdk
   ```
   {: pre}

2. Deploy the package.

   ```
   ibmcloud fn deploy -m openwhisk-sdk/packages/assistant-v1/manifest.yaml
   ```
   {: pre}

3. Verify that the package is added to your package list.

   ```
   ibmcloud fn package list
   ```
   {: pre}

   **Output**

   ```
   packages
   /myOrg_mySpace/assistant-v1                        private
   ```
   {: screen}

4. Bind the credentials from the {{site.data.keyword.conversationshort}} instance you created to the package.

   ```
   ibmcloud fn service bind conversation assistant-v1
   ```
   {: pre}

   **Example output**

   ```
   Credentials 'Credentials-1' from 'conversation' service instance 'Watson Assistant (formerly Conversation)-8h' bound to 'assistant-v1'.
   ```
   {: screen}

5. Verify that the package is configured with your {{site.data.keyword.conversationshort}} service instance credentials.

   ```
   ibmcloud fn package get assistant-v1 parameters
   ```
   {: pre}

   **Example output**

   ```
   ok: got package assistant-v1, displaying field parameters
   [
      {
        "key": "__bx_creds",
        "value": {
          "conversation": {
            "credentials": "Credentials-1",
            "instance": "Watson Assistant (formerly Conversation)-8h",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/assistant/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
   ]
   ```
   {: screen}

### Installing from the {{site.data.keyword.openwhisk_short}} console
{: #conversation_ui}

Install the {{site.data.keyword.conversationshort}} package from the console.
{: shortdesc}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page ](https://cloud.ibm.com/functions/create){: external}.

2. Select the namespace that you want to install the package into.

3. Click **Install Packages**.

4. Click on the **Watson** package group.

5. Click on the **Watson Assistant** Package.

6. Click **Install**.

7. After the package is installed you are redirected to the Actions page and can search for your new package, which is named **assistant-v1**.

8. To use the Actions in the **assistant-v1** Package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, follow steps 4 and 5 in the [CLI instructions](#conversation_cli).
  * To bind service credentials to individual actions, complete the following steps in the console.
  
  You must complete the following steps for each action that you want to use.
  {: note}

    1. Click on an action from the **assistant-v1** Package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click on the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.conversationshort}} package
{: #usage_conversation}

To use the actions in this package, run commands in the following format.

```
ibmcloud fn action invoke assistant-v1/<action_name> -b -p <param name> <param>
```
{: pre}

All actions require a version parameter in the format `YYYY-MM-DD`. When the API is changed in a backwards-incompatible way, a new version date is released. For more information, see the [API reference](https://cloud.ibm.com/apidocs/assistant/assistant-v1#versioning){: external}.

This package's functions use the current version of Watson Assistant, `2018-07-10`. Try out the `list-workspaces` action.

```
ibmcloud fn action invoke assistant-v1/list-workspaces -b -p version 2018-07-10
```
{: pre}
