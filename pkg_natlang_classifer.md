---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: machine learning, functions

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


# {{site.data.keyword.nlclassifiershort}}
{: #pkg_natlang_classifier}

The {{site.data.keyword.nlclassifierfull}} uses machine learning algorithms to return the matching predefined classes for short text input. You create and train a classifier to connect predefined classes to example texts so that the service can apply those classes to new inputs.
{: shortdesc}

The {{site.data.keyword.nlclassifiershort}} package contains the following entities. You can find additional details in the {{site.data.keyword.nlclassifiershort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`natural-language-classifier-v1`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html){: external} | Package | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`  | Work with the {{site.data.keyword.nlclassifiershort}} service. |
| [`classify`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#classify){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id`, `text`,  | Classify a phrase. |
| [`classify-collection`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#classify-collection){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id`, `collection` | Classify multiple phrases. |
| [`create-classifier`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#create-classifier){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `metadata`, `training_data` | Create a classifier. |
| [`delete-classifier`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#delete-classifier){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id` | Delete a classifier. |
| [`get-classifier`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#get-classifier){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id` | Get information about a classifier. |
| [`list-classifiers`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#list-classifiers){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | List classifiers. |


## Creating a {{site.data.keyword.nlclassifiershort}} service instance
{: #service_instance_classifier}

Before you install the package, you must create a {{site.data.keyword.nlclassifiershort}} service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.nlclassifiershort}} service instance ](https://cloud.ibm.com/catalog/services/natural_language_classifier){: external}.
2. When the service instance is created, auto-generated service credentials are also created for you.

## Installing the {{site.data.keyword.nlclassifiershort}} package
{: #install_classifier}

After you have a {{site.data.keyword.nlclassifiershort}} service instance, use the {{site.data.keyword.openwhisk}} CLI to install the {{site.data.keyword.nlclassifiershort}} package into your namespace.
{: shortdesc}

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #nlclassifier_cli}

**Before you begin**
[Install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=cloud-functions-cli_install).

To install the {{site.data.keyword.nlclassifiershort}} package, run the following command.

1. Clone the {{site.data.keyword.nlclassifiershort}} package repo.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Deploy the package.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/natural-language-classifier-v1/manifest.yaml
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
    /myOrg_mySpace/natural-language-classifier-v1                        private
    ```
    {: screen}

4. Bind the credentials from the {{site.data.keyword.nlclassifiershort}} instance you created to the package.
    ```
    ibmcloud fn service bind natural_language_classifier natural-language-classifier-v1
    ```
    {: pre}

    Depending on the region where you created the service instance, the service instance might be named differently because it is an IAM service. If the command fails, use the following service name for the bind command:
    ```
    ibmcloud fn service bind natural-language-classifier natural-language-classifier-v1
    ```
    {: pre}

    **Example output**
    ```
    Credentials 'Credentials-1' from 'natural_language_classifier' service instance 'Watson Natural Language Classifier' bound to 'natural-language-classifier-v1'.
    ```
    {: screen}

5. Verify that the package is configured with your {{site.data.keyword.nlclassifiershort}} service instance credentials.
    ```
    ibmcloud fn package get natural-language-classifier-v1 parameters
    ```
    {: pre}

    **Example output**
    ```
    ok: got package natural-language-classifier-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "natural_language_classifier": {
            "credentials": "Credentials-1",
            "instance": "Watson Natural Language Classifier",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/natural_language_classifier/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Installing from the {{site.data.keyword.openwhisk_short}} UI
{: #nlclassifier_ui}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page ](https://cloud.ibm.com/openwhisk/create){: external}.

2. By using the **Cloud Foundry Org** and **Cloud Foundry Space** lists, select the namespace that you want to install the package into. 

3. Click **Install Packages**.

4. Click the **Watson** Package group.

5. Click the **Natural Language Classifier** Package.

5. Click **Install**.

6. Once the package is installed you are redirected to the actions page and can search for your new package, which is named **natural-language-classifier-v1**.

7. To use the actions in the **natural-language-classifier-v1** Package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, follow steps 4 and 5 in the [CLI instructions](#nlclassifier_cli).
  * To bind service credentials to individual actions, complete the following steps in the UI. 
  
  You must complete the following steps for each action that you want to use.
  {: note}

    1. Click an action from the **natural-language-classifier-v1** Package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.nlclassifiershort}} package
{: #usage_classifier}

To use the actions in this package, run commands in the following format.

```
ibmcloud fn action invoke natural-language-classifier-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Try out the `list-classifiers` action.
```
ibmcloud fn action invoke natural-language-classifier-v1/list-classifiers -b
```
{: pre}



