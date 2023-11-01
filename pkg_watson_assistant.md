---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-01"

keywords: openwhisk, functions, Watson assistant, chatbot, cognitive

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.conversationshort}}
{: #pkg_watson_assistant}

{{site.data.keyword.openwhisk}} is deprecated. Existing Functions entities such as actions, triggers, or sequences will continue to run, but as of 28 December 2023, you can’t create new Functions entities. Existing Functions entities are supported until October 2024. Any Functions entities that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

The installable {{site.data.keyword.conversationfull}} service combines machine learning, natural language understanding, and integrated dialog tools to create conversation flows between your apps and your users. For more information about this service, see [{{site.data.keyword.conversationfull}}](/docs/assistant?topic=assistant-getting-started).
{: shortdesc}

The Watson packages are deprecated. Please use the [Watson SDKs](/docs/watson?topic=watson-using-sdks){: external} to perform Watson related functionality. For more information about which Watson SDK is included in the available runtimes, see the [Functions runtime](/docs/openwhisk?topic=openwhisk-runtimes) documentation.
{: deprecated}

The {{site.data.keyword.conversationshort}} package contains the following entities. For more information, see the [{{site.data.keyword.conversationshort}} API reference](https://cloud.ibm.com/apidocs/assistant-v1#introduction){: external}.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `assistant-v1` | Package | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`  | Work with the {{site.data.keyword.conversationshort}} service. |
| `message` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `assistant_id`,   | Get the response to user input. |
| `create-workspace` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `description`, `language`, `intents`, `entities`, `dialog_nodes`, `counterexamples`, `metadata`, `learning_opt_out`  | Create a workspace. |
| `delete-workspace` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`  | Delete a workspace. |
| `get-workspace` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `export`, `include_audit`  | Get information about a workspace. |
| `list-workspaces` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | List workspaces. |
| `update-workspace` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `name`, `description`, `language`, `intents`, `entities`, `dialog_nodes`, `counterexamples`, `metadata`, `learning_opt_out`, `append`  | Update a workspace. |
| `create-intent` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `description`, `examples`  | Create an intent. |
| `delete-intent` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`  | Delete an intent. |
| `get-intent` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `export`, `include_audit`  | Get information about an intent. |
| `list-intents` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `export`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | List intents. |
| `update-intent` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `new_intent`, `new_description`, `new_examples`  | Update an intent. |
| `create-example` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `text`  | Create a user input example. |
| `delete-example` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `text`  | Delete a user input example. |
| `get-example` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `text`, `include_audit`  | Get information about a user input example. |
| `list-examples` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | List user input examples. |
| `update-example` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `text`, `new_text`  | Update a user input example. |
| `create-counterexample` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `text`  | Create a counterexample. |
| `delete-counterexample` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `text`  | Delete a counterexample. |
| `get-counterexample` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `text`, `include_audit`  | Get information about a counterexample. |
| `list-counterexamples` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | List counterexamples. |
| `update-counterexample` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `text`, `new_text`  | Update a counterexample. |
| `create-entity` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `description`, `metadata`, `values`, `fuzzy_match`  | Create an entity. |
| `delete-entity` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`  | Delete an entity. |
| `get-entity` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `export`, `include_audit`  | Get information about an entity. |
| `list-entities` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `export`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | List entities. |
| `update-entity` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `new_entity`, `new_description`, `new_metadata`, `new_fuzzy_match`, `new_values`  | Update an entity. |
| `create-value` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `metadata`, `synonyms`, `patterns`, `value_type`  | Add an entity value. |
| `delete-value` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`  | Delete an entity value. |
| `get-value` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `export`, `include_audit`  | Get an entity value. |
| `list-values` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `export`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | List entity values. |
| `update-value` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `new_value`, `new_metadata`, `new_type`, `new_synonyms`, `new_patterns`  | Update an entity value. |
| `create-synonym` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `synonym`  | Add an entity value synonym. |
| `delete-synonym` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `synonym`  | Delete an entity value synonym. |
| `get-synonym` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `synonym`, `include_audit`  | Get an entity value synonym. |
| `list-synonyms` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | List entity value synonyms. |
| `update-synonym` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `synonym`, `new_synonym`  | Update an entity value synonym. |
| `create-dialog-node` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `dialog_node`, `description`, `conditions`, `parent`, `previous_sibling`, `output`, `context`, `metadata`, `next_step`, `actions`, `title`, `node_type`, `event_name`, `variable`, `digress_in`, `digress_out`, `digress_out_slots`  | Create a dialog node. |
| `delete-dialog-node` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `dialog_node`  | Delete a dialog node. |
| `get-dialog-node` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `dialog_node`, `include_audit`  | Get a dialog node. |
| `list-dialog-nodes` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | List dialog nodes. |
| `update-dialog-node` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `dialog_node`, `new_dialog_node`, `new_description`, `new_conditions`, `new_parent`, `new_previous_sibling`, `new_output`, `new_context`, `new_metadata`, `new_next_step`, `new_title`, `new_type`, `new_event_name`, `new_variable`, `new_actions`, `new_digress_in`, `new_digress_out`, `new_digress_out_slots`  | Update a dialog node. |
| `list-all-logs` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `filter`, `sort`, `page_limit`, `cursor`  | List log events in all workspaces. |
| `list-logs` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `sort`, `filter`, `page_limit`, `cursor`  | List log events in a workspace. |
| `delete-user-data` | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id`  | Delete labeled data. |
{: caption="Table 1. Watson assistant package entities" caption-side="bottom"}



## Creating a {{site.data.keyword.conversationshort}} service instance
{: #service_instance_conversation}

Before you install the package, you must create a {{site.data.keyword.conversationshort}} service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.conversationshort}} service instance](https://cloud.ibm.com/catalog/services/conversation){: external}.
2. When the service instance is created, service credentials are automatically created for you.

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

    ```sh
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Deploy the package.

    ```sh
    ibmcloud fn deploy -m openwhisk-sdk/packages/assistant-v1/manifest.yaml
    ```
    {: pre}

3. Verify that the package is added to your package list.

    ```sh
    ibmcloud fn package list
    ```
    {: pre}

    **Example output**

    ```sh
    packages
    /myOrg_mySpace/assistant-v1                        private
    ```
    {: screen}

4. Bind the credentials from the {{site.data.keyword.conversationshort}} instance you created to the package.

    ```sh
    ibmcloud fn service bind conversation assistant-v1
    ```
    {: pre}

    **Example output**

    ```sh
    Credentials 'Credentials-1' from 'conversation' service instance 'Watson Assistant (formerly Conversation)-8h' bound to 'assistant-v1'.
    ```
    {: screen}

5. Verify that the package is configured with your {{site.data.keyword.conversationshort}} service instance credentials.

    ```sh
    ibmcloud fn package get assistant-v1 parameters
    ```
    {: pre}

    **Example output**

    ```sh
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

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page](https://cloud.ibm.com/functions/create){: external}.

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
    2. In the left navigation, click on the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.conversationshort}} package
{: #usage_conversation}

To use the actions in this package, run commands in the following format.

```sh
ibmcloud fn action invoke assistant-v1/<action_name> -b -p <param name> <param>
```
{: pre}

All actions require a version parameter in the format `YYYY-MM-DD`. When the API is changed in a backwards-incompatible way, a new version date is released. For more information, see the [API reference](https://cloud.ibm.com/apidocs/assistant/assistant-v1#versioning){: external}.

This package's functions use the current version of Watson Assistant, `2018-07-10`. Try out the `list-workspaces` action.

```sh
ibmcloud fn action invoke assistant-v1/list-workspaces -b -p version 2018-07-10
```
{: pre}


