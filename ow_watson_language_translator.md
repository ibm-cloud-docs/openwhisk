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

# {{site.data.keyword.languagetranslatorshort}} package

The {{site.data.keyword.languagetranslatorfull}} translates text from one language to another. The service offers multiple IBM provided translation models that you can customize based on your unique terminology and language.
{: shortdesc}

The {{site.data.keyword.languagetranslatorshort}} package contains the following entities. You can find additional details in the {{site.data.keyword.languagetranslatorshort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`language-translator-v3`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html) | package | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  | Work with the {{site.data.keyword.languagetranslatorshort}} service. |
| [translate](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#translate) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   text, model_id, source, target,  | Translate text. |
| [identify](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#identify) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    text,  | Identify the language of text. |
| [list-identifiable-languages](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-identifiable-languages) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, | List identifiable languages. |
| [create-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#create-model) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    base_model_id,     name,     forced_glossary,     parallel_corpus,  | Create a model. |
| [delete-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#delete-model) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    model_id,  | Delete a model. |
| [get-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#get-model) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    model_id,  | Get model details. |
| [list-models](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-models) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    source,     target,     default_models,  | List models. |

## Creating a {{site.data.keyword.languagetranslatorshort}} service instance
{: #service_instance}

Before you install the package, you must create a {{site.data.keyword.languagetranslatorshort}} service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.languagetranslatorshort}} service instance ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/language_translator).
2. When the service instance is created, auto-generated service credentials are also created for you.

## Installing the {{site.data.keyword.languagetranslatorshort}} package
{: #install}

After you have an {{site.data.keyword.languagetranslatorshort}} service instance, use the {{site.data.keyword.openwhisk}} CLI to install the {{site.data.keyword.languagetranslatorshort}} package into your namespace.
{: shortdesc}

Before you begin:
  1. [Install the {{site.data.keyword.openwhisk_short}} plugin for the {{site.data.keyword.Bluemix_notm}} CLI](bluemix_cli.html#cloudfunctions_cli).
  2. Install the [`wskdeploy` command ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) and add the downloaded binary to your PATH.

To install the {{site.data.keyword.languagetranslatorshort}} package:

1. Clone the {{site.data.keyword.languagetranslatorshort}} package repo.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Navigate to the `packages/language-translator-v3` directory.
    ```
    cd <filepath>/packages/language-translator-v3
    ```
    {: pre}

3. Deploy the package.
    ```
    wskdeploy -m manifest.yaml
    ```
    {: pre}

4. Verify that the package is added to your package list.
    ```
    ibmcloud wsk package list
    ```
    {: pre}

    Output:
    ```
    packages
    /myOrg_mySpace/language-translator-v3                        private
    ```
    {: screen}

5. Bind the credentials from the {{site.data.keyword.languagetranslatorshort}} instance you created to the package.
    ```
    ibmcloud wsk service bind language_translator language-translator-v3
    ```
    {: pre}

    Example output:
    ```
    Credentials 'Credentials-1' from 'language_translator' service instance 'Watson Language Translator' bound to 'language-translator-v3'.
    ```
    {: screen}

3. Verify that the package is configured with your {{site.data.keyword.languagetranslatorshort}} service instance credentials.
    ```
    ibmcloud wsk package get /myBluemixOrg_myBluemixSpace/language-translator-v3 parameters
    ```
    {: pre}

    Example output:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/language-translator-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "language_translator": {
            "credentials": "Credentials-1",
            "instance": "Watson Language Translator",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/language_translator/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

## Using the {{site.data.keyword.languagetranslatorshort}} package
{: #usage}

To use the actions in this package, run commands in the following format:

```
ibmcloud wsk action invoke language-translator-v3/<action_name> -b -p <param name> <param>
```
{: pre}
