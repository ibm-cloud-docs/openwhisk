---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-17"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.nlclassifiershort}} package

The {{site.data.keyword.nlclassifierfull}} uses machine learning algorithms to return the top matching predefined classes for short text input. You create and train a classifier to connect predefined classes to example texts so that the service can apply those classes to new inputs.
{: shortdesc}

The {{site.data.keyword.nlclassifiershort}} package contains the following entities. You can find additional details in the {{site.data.keyword.nlclassifiershort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`natural-language-classifier-v1`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html) | package | username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url,  | Work with the {{site.data.keyword.nlclassifiershort}} service. |
| [classify](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#classify) | action |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,    text,  | Classify a phrase. |
| [classify-collection](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#classify-collection) | action |  username, password, iam_access_token, iam_apikey, iam_url,   headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,    collection,  | Classify multiple phrases. |
| [create-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#create-classifier) | action |  username, password, iam_access_token, iam_apikey, iam_url,   headers, headers[X-Watson-Learning-Opt-Out], url,    metadata,     training_data,  | Create a classifier. |
| [delete-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#delete-classifier) | action |  username, password, iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  | Delete a classifier. |
| [get-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#get-classifier) | action |  username, password, iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  | Get information about a classifier. |
| [list-classifiers](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#list-classifiers) | action |  username, password, iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, | List classifiers. |

## Creating a {{site.data.keyword.nlclassifiershort}} service instance
{: #service_instance}

Before you install the package, you must create a {{site.data.keyword.nlclassifiershort}} service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.nlclassifiershort}} service instance ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/natural_language_classifier).
2. When the service instance is created, auto-generated service credentials are also created for you.

## Installing the {{site.data.keyword.nlclassifiershort}} package
{: #install}

After you have an {{site.data.keyword.nlclassifiershort}} service instance, use the {{site.data.keyword.openwhisk}} CLI to install the {{site.data.keyword.nlclassifiershort}} package into your namespace.
{: shortdesc}

Before you begin:
  1. [Install the {{site.data.keyword.openwhisk_short}} plugin for the {{site.data.keyword.Bluemix_notm}} CLI](bluemix_cli.html#cloudfunctions_cli).
  2. Install the [`wskdeploy` command ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) and add the downloaded binary to your PATH.

To install the {{site.data.keyword.nlclassifiershort}} package:

1. Clone the {{site.data.keyword.nlclassifiershort}} package repo.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Deploy the package.
    ```
    wskdeploy -m openwhisk-sdk/packages/natural-language-classifier-v1/manifest.yaml
    ```
    {: pre}

3. Verify that the package is added to your package list.
    ```
    ibmcloud wsk package list
    ```
    {: pre}

    Output:
    ```
    packages
    /myOrg_mySpace/natural-language-classifier-v1                        private
    ```
    {: screen}

4. Bind the credentials from the {{site.data.keyword.nlclassifiershort}} instance you created to the package.  
    ```
    ibmcloud wsk service bind natural_language_classifier natural-language-classifier-v1
    ```
    {: pre}

    Depending on the region where you created the service instance, the service instance might be named differently because it is an IAM service. If the above command fails, use the following service name for the bind command:
    ```
    ibmcloud wsk service bind natural-language-classifier natural-language-classifier-v1
    ```
    {: pre}

    Example output:
    ```
    Credentials 'Credentials-1' from 'natural_language_classifier' service instance 'Watson Natural Language Classifier' bound to 'natural-language-classifier-v1'.
    ```
    {: screen}

5. Verify that the package is configured with your {{site.data.keyword.nlclassifiershort}} service instance credentials.
    ```
    ibmcloud wsk package get natural-language-classifier-v1 parameters
    ```
    {: pre}

    Example output:
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

## Using the {{site.data.keyword.nlclassifiershort}} package
{: #usage}

To use the actions in this package, run commands in the following format:

```
ibmcloud wsk action invoke natural-language-classifier-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Try out the `list-classifiers` action.
```
ibmcloud wsk action invoke natural-language-classifier-v1/list-classifiers -b
```
{: pre}
