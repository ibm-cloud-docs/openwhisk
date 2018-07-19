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

# {{site.data.keyword.nlushort}} package

The {{site.data.keyword.nlufull}} service helps you analyze various features of text content at scale.
{: shortdesc}

Provide text, raw HTML, or a public URL, and {{site.data.keyword.nlushort}} gives you results for the features you request. The service cleans HTML content before analysis by default, so the results can ignore most advertisements and other unwanted content. You can create <a target="_blank" href="https://www.ibm.com/watson/developercloud/doc/natural-language-understanding/customizing.html">custom models</a> with Watson Knowledge Studio that can be used to detect custom entities and relations in Natural Language Understanding.

The {{site.data.keyword.nlushort}} package contains the following entities. You can find additional details in the {{site.data.keyword.nlushort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`natural-language-understanding-v1`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl.html) | package | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  |  Work with the {{site.data.keyword.nlushort}} service. |
| [analyze](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#analyze) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   features, text, html, url, clean, xpath, fallback_to_raw, return_analyzed_text, language, limit_text_characters,  | Analyze text, HTML, or a public webpage |
| [delete-model](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#delete-model) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    model_id,  | Delete model |
| [list-models](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#list-models) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, | List models |

## Creating a {{site.data.keyword.nlushort}} service instance
{: #service_instance}

Before you install the package, you must create a {{site.data.keyword.nlushort}} service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.nlushort}} service instance ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/natural-language-understanding).
2. When the service instance is created, auto-generated service credentials are also created for you.

## Installing the {{site.data.keyword.nlushort}} package
{: #install}

After you have an {{site.data.keyword.nlushort}} service instance, use the {{site.data.keyword.openwhisk}} CLI to install the {{site.data.keyword.nlushort}} package into your namespace.
{: shortdesc}

Before you begin:
  1. [Install the {{site.data.keyword.openwhisk_short}} plugin for the {{site.data.keyword.Bluemix_notm}} CLI](bluemix_cli.html#cloudfunctions_cli).
  2. Install the [`wskdeploy` command ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) and add the downloaded binary to your PATH.

To install the {{site.data.keyword.nlushort}} package:

1. Clone the {{site.data.keyword.nlushort}} package repo.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Navigate to the `packages/natural-language-understanding-v1` directory.
    ```
    cd <filepath>/packages/natural-language-understanding-v1
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
    /myOrg_mySpace/natural-language-understanding-v1                        private
    ```
    {: screen}

5. Bind the credentials from the {{site.data.keyword.nlushort}} instance you created to the package.
    ```
    ibmcloud wsk service bind natural-language-understanding natural-language-understanding-v1
    ```
    {: pre}

    Example output:
    ```
    Credentials 'Credentials-1' from 'natural-language-understanding' service instance 'Watson Natural Language Understanding' bound to 'natural-language-understanding-v1'.
    ```
    {: screen}

3. Verify that the package is configured with your {{site.data.keyword.nlushort}} service instance credentials.
    ```
    ibmcloud wsk package get /myBluemixOrg_myBluemixSpace/natural-language-understanding-v1 parameters
    ```
    {: pre}

    Example output:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/natural-language-understanding-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "natural-language-understanding": {
            "credentials": "Credentials-1",
            "instance": "Watson Natural Language Understanding",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/natural-language-understanding/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

## Using the {{site.data.keyword.nlushort}} package
{: #usage}

To use the actions in this package, run commands in the following format:

```
ibmcloud wsk action invoke natural-language-understanding-v1/<action_name> -b -p <param name> <param>
```
{: pre}
