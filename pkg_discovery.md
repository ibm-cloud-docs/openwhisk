---

copyright:
  years: 2017, 2020
lastupdated: "2020-05-22"

keywords: functions, cognitive,

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

# {{site.data.keyword.discoveryshort}}
{: #pkg_discovery}

The installable {{site.data.keyword.discoveryfull}} service is a cognitive search and content analytics engine that you can add to applications to identify patterns, trends, and actionable insights to drive better decision-making. Securely unify structured and unstructured data with pre-enriched content, and use a simplified query language to eliminate the need for manual filtering of results. For more information about this service, see [{{site.data.keyword.discoveryshort}}](/docs/discovery?topic=discovery-getting-started).
{: shortdesc}

The {{site.data.keyword.discoveryshort}} package contains the following entities. You can find more details in the {{site.data.keyword.discoveryshort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`discovery-v1`](https://cloud.ibm.com/apidocs/discovery/discovery){: external} | Package | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`  | Work with the {{site.data.keyword.discoveryshort}} service. |
| [`create-environment`](https://cloud.ibm.com/apidocs/discovery/discovery#create-an-environment){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `description`, `size` | Create an environment. |
| [`delete-environment`](https://cloud.ibm.com/apidocs/discovery/discovery#delete-environment){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id` | Delete an environment. |
| [`get-environment`](https://cloud.ibm.com/apidocs/discovery/discovery#get-environment-info){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`  | Get information about an environment. |
| [`list-environments`](https://cloud.ibm.com/apidocs/discovery/discovery#list-environments){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`  | List environments. |
| [`list-fields`](https://cloud.ibm.com/apidocs/discovery/discovery#list-fields-across-collections){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_ids` | List fields across collections. |
| [`update-environment`](https://cloud.ibm.com/apidocs/discovery/discovery#update-an-environment){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `name`, `description` | Update an environment. |
| [`create-configuration`](https://cloud.ibm.com/apidocs/discovery/discovery#create-configuration){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `name`, `description`, `conversions`, `enrichments`, `normalizations`  | Add a configuration. |
| [`delete-configuration`](https://cloud.ibm.com/apidocs/discovery/discovery#delete-configuration){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `configuration_id` | Delete a configuration. |
| [`get-configuration`](https://cloud.ibm.com/apidocs/discovery/discovery#get-configuration){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `configuration_id` | Get configuration details. |
| [`list-configurations`](https://cloud.ibm.com/apidocs/discovery/discovery#list-configurations){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `name` | List configurations. |
| [`update-configuration`](https://cloud.ibm.com/apidocs/discovery/discovery#update-configuration){: external} | Action |  `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `configuration_id`, `name`, `description`, `conversions`, `enrichments`, `normalizations` | Update a configuration. |
| [`test-configuration-in-environment`](https://cloud.ibm.com/apidocs/discovery/discovery#test-configuration-in-environment){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `configuration`,     `step`, `configuration_id`, `file`, `metadata`, `file_content_type` | Test a configuration. |
| [`create-collection`](https://cloud.ibm.com/apidocs/discovery/discovery#create-collection){: external} | Action |  `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `name`, `description`, `configuration_id`, `language` | Create a collection. |
| [`delete-collection`](https://cloud.ibm.com/apidocs/discovery/discovery#delete-collection){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Delete a collection. |
| [`get-collection`](https://cloud.ibm.com/apidocs/discovery/discovery#get-collection){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Get collection details. |
| [`list-collection-fields`](https://cloud.ibm.com/apidocs/discovery/discovery#list-collection-fields){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | List collection fields. |
| [`list-collections`](https://cloud.ibm.com/apidocs/discovery/discovery#list-collections){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `name` | List collections. |
| [`update-collection`](https://cloud.ibm.com/apidocs/discovery/discovery#update-collection){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `name`, `description`, `configuration_id` | Update a collection. |
| [`create-expansions`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-expansions){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, ``collection_id`, `expansions` | Create or update the expansion list. |
| [`delete-expansions`](https://cloud.ibm.com/apidocs/discovery/discovery#delete-expansions){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Delete the expansion list. |
| [`list-expansions`](https://cloud.ibm.com/apidocs/discovery/discovery#list-expansions){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Get the expansion list. |
| [`add-document`](https://cloud.ibm.com/apidocs/discovery/discovery#add-document){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `file`, `metadata`, `file_content_type` | Add a document. |
| [`delete-document`](https://cloud.ibm.com/apidocs/discovery/discovery#delete-document){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `document_id` | Delete a document. |
| [`get-document-status`](https://cloud.ibm.com/apidocs/discovery/discovery#get-document-status){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `document_id` | Get document details. |
| [`update-document`](https://cloud.ibm.com/apidocs/discovery/discovery#update-document){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `document_id`, `file`,     `metadata`, `file_content_type` | Update a document. |
| [`federated-query`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_ids`, `filter`, `query`,     `natural_language_query`, `aggregation`, `count`, `return_fields`, `offset`, `sort`, `highlight`, `deduplicate`, `deduplicate_field`, `similar`, `similar_document_ids`, `similar_fields` | Query documents in multiple collections. |
| [`federated-query-notices`](https://cloud.ibm.com/apidocs/discovery/discovery#federated-query-notices){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_ids`, `filter`, `query`, `natural_language_query`, `aggregation`, `count`, `return_fields`, `offset`, `sort`, `highlight`, `deduplicate_field`, `similar`, `similar_document_ids`, `similar_fields` | Query multiple collection system notices. |
| [`query`](https://cloud.ibm.com/apidocs/discovery/discovery#query){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `filter`, `query`, `natural_language_query`, `passages`,     `aggregation`, `count`, `return_fields`, `offset`, `sort`, `highlight`, `passages_fields`, `passages_count`, `passages_characters`, `deduplicate`, `deduplicate_field`, `similar`, `similar_document_ids`, `similar_fields` | Query your collection. |
| [`query-entities`](https://cloud.ibm.com/apidocs/discovery/discovery#query-entities){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `feature`, `entity`, `context`, `count`, `evidence_count` | Query a Knowledge Graph entity. |
| [`query-notices`](https://cloud.ibm.com/apidocs/discovery/discovery#query-notices){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `filter`, `query`, `natural_language_query`, `passages`, `aggregation`, `count`, `return_fields`, `offset`, `sort`, `highlight`, `passages_fields`, `passages_count`, `passages_characters`, `deduplicate_field`, `similar`, `similar_document_ids`, `similar_fields`  | Query system notices. |
| [`query-relations`](https://cloud.ibm.com/apidocs/discovery/discovery#query-relations){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `entities`, `context`, `sort`, `filter`, `count`, `evidence_count` | Query a Knowledge Graph relationship. |
| [`add-training-data`](https://cloud.ibm.com/apidocs/discovery/discovery#add-training-data){: external} | Action |  `username`, `password`,  `iam_access_token`, `iam_apikey, iam_url`,  `headers, headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `natural_language_query`, `filter`, `examples`, | Add a query to training data. |
| [`create-training-example`](https://cloud.ibm.com/apidocs/discovery/discovery#create-training-example){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id`, `document_id`, `cross_reference`, `relevance` | Add an example to training data query. |
| [`delete-all-training-data`](https://cloud.ibm.com/apidocs/discovery/discovery#delete-all-training-data){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Delete all training data. |
| [`delete-training-data`](https://cloud.ibm.com/apidocs/discovery/discovery#delete-training-data){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id` | Delete a training data query. |
| [`delete-training-example`](https://cloud.ibm.com/apidocs/discovery/discovery#delete-training-example){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id`, `example_id` | Delete an example for training data query. |
| [`get-training-data`](https://cloud.ibm.com/apidocs/discovery/discovery#get-training-data){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id` | Get details about a query. |
| [`get-training-example`](https://cloud.ibm.com/apidocs/discovery/discovery#get-training-example){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id`, `example_id` | Get details for a training data example. |
| [`list-training-data`](https://cloud.ibm.com/apidocs/discovery/discovery#list-training-data){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | List training data. |
| [`list-training-examples`](https://cloud.ibm.com/apidocs/discovery/discovery#list-training-examples){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id` | List examples for a training data query. |
| [`update-training-example`](https://cloud.ibm.com/apidocs/discovery/discovery#update-training-example){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id`, `example_id`, `cross_reference`, `relevance` | Change a label or cross reference for an example. |
| [`delete-user-data`](https://cloud.ibm.com/apidocs/discovery/discovery#delete-user-data){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id` | Delete labeled data. |

## Creating a {{site.data.keyword.discoveryshort}} service instance
{: #service_instance_discovery}

Before you install the package, you must create a {{site.data.keyword.discoveryshort}} service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.discoveryshort}} service instance ](https://cloud.ibm.com/catalog/services/discovery){: external}.
2. When the service instance is created, auto-generated service credentials are also created for you.

## Installing the {{site.data.keyword.discoveryshort}} package
{: #install_discovery}

After you have a {{site.data.keyword.discoveryshort}} service instance, install the {{site.data.keyword.discoveryshort}} package into your namespace.
{: shortdesc}

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #discovery_cli}

Install the {{site.data.keyword.discoveryshort}} package from the CLI. Be sure to [install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=openwhisk-cli_install) first.
{: shortdesc}

To install the {{site.data.keyword.discoveryshort}} package, run the following commands:

1. Clone the {{site.data.keyword.discoveryshort}} package repo.
  
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Deploy the package.
  
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/discovery-v1/manifest.yaml
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
    /myOrg_mySpace/discovery-v1                        private
    ```
    {: screen}

4. Bind the credentials from the {{site.data.keyword.discoveryshort}} instance you created to the package.
  
    ```
    ibmcloud fn service bind discovery discovery-v1
    ```
    {: pre}

    **Example output**

    ```
    Credentials 'Credentials-1' from 'discovery' service instance 'Watson Discovery' bound to 'discovery-v1'.
    ```
    {: screen}

5. Verify that the package is configured with your {{site.data.keyword.discoveryshort}} service instance credentials.
  
    ```
    ibmcloud fn package get discovery-v1 parameters
    ```
    {: pre}

    **Example output**

    ```
    ok: got package discovery-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "discovery": {
            "credentials": "Credentials-1",
            "instance": "Watson Discovery",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/discovery/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Installing from the {{site.data.keyword.openwhisk_short}} console
{: #discovery_ui}

You can install your package from the {{site.data.keyword.openwhisk_short}} console.
{: shortdesc}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page ](https://cloud.ibm.com/functions/create){: external}.

2. Select the namespace that you want to install the package into.

3. Click **Install Packages**.

4. Click the **Watson** package group.

5. Click the **Discovery** package.

6. Click **Install**.

7. After the package is installed, you are redirected to the Actions page and can search for your new package, which is named **discovery-v1**.

8. To use the actions in the `discovery-v1` package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, complete step 4 in the CLI instructions.
  * To bind service credentials to individual actions, complete the following steps in the console.

  You must complete the following steps for each action that you want to use.
  {: note}

    1. Click on an action from the `discovery-v1` package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click on the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.discoveryshort}} package
{: #usage_discovery}

To use the actions in this package, run commands in the following format:

```
ibmcloud fn action invoke discovery-v1/<action_name> -b -p <param name> <param>
```
{: pre}

All actions require a version parameter in the format YYYY-MM-DD. When the API is changed in a backwards-incompatible way, a new version date is released. See more details in the [API reference](https://cloud.ibm.com/apidocs/discovery/discovery#versioning){: external}.

This package's functions use the current version of Discovery, `2018-03-05`. Try out the `list-environments` action.

```
ibmcloud fn action invoke discovery-v1/list-environments -b -p version 2018-03-05
```
{: pre}
