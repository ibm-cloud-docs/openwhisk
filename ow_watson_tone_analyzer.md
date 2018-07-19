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

# {{site.data.keyword.toneanalyzershort}} package

The {{site.data.keyword.toneanalyzerfull}} service uses linguistic analysis to detect emotional and language tones in written text.
{:shortdesc}

The service can analyze tone at both the document and sentence levels. You can use the service to understand how your written communications are perceived and then to improve the tone of your communications. Businesses can use the service to learn the tone of their customers' communications and to respond to each customer appropriately, or to understand and improve their customer conversations.

**Note:** Request logging is disabled for the Tone Analyzer service. The service neither logs nor retains data from requests and responses, regardless of whether the `X-Watson-Learning-Opt-Out` request header is set.

The {{site.data.keyword.toneanalyzershort}} package contains the following entities. You can find additional details in the {{site.data.keyword.toneanalyzershort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`tone-analyzer-v3`](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html) | package | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  | Work with the {{site.data.keyword.toneanalyzershort}} service. |
| [tone](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    tone_input,     content_type,     sentences,     tones,     content_language,     accept_language,  | Analyze general tone. |
| [tone-chat](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone-chat) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   utterances,     content_language,     accept_language,  | Analyze customer engagement tone. |

## Creating a {{site.data.keyword.toneanalyzershort}} service instance
{: #service_instance}

Before you install the package, you must create a {{site.data.keyword.toneanalyzershort}} service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.toneanalyzershort}} service instance ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/tone_analyzer).
2. When the service instance is created, auto-generated service credentials are also created for you.

## Installing the {{site.data.keyword.toneanalyzershort}} package
{: #install}

After you have an {{site.data.keyword.toneanalyzershort}} service instance, use the {{site.data.keyword.openwhisk}} CLI to install the {{site.data.keyword.toneanalyzershort}} package into your namespace.
{: shortdesc}

Before you begin:
  1. [Install the {{site.data.keyword.openwhisk_short}} plugin for the {{site.data.keyword.Bluemix_notm}} CLI](bluemix_cli.html#cloudfunctions_cli).
  2. Install the [`wskdeploy` command ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) and add the downloaded binary to your PATH.

To install the {{site.data.keyword.toneanalyzershort}} package:

1. Clone the {{site.data.keyword.toneanalyzershort}} package repo.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Navigate to the `packages/tone-analyzer-v3` directory.
    ```
    cd <filepath>/packages/tone-analyzer-v3
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
    /myOrg_mySpace/tone-analyzer-v3                        private
    ```
    {: screen}

5. Bind the credentials from the {{site.data.keyword.toneanalyzershort}} instance you created to the package.
    ```
    ibmcloud wsk service bind tone_analyzer tone-analyzer-v3
    ```
    {: pre}

    Example output:
    ```
    Credentials 'Credentials-1' from 'tone_analyzer' service instance 'Watson Tone Analyzer' bound to 'tone-analyzer-v3'.
    ```
    {: screen}

3. Verify that the package is configured with your {{site.data.keyword.toneanalyzershort}} service instance credentials.
    ```
    ibmcloud wsk package get /myBluemixOrg_myBluemixSpace/tone-analyzer-v3 parameters
    ```
    {: pre}

    Example output:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/tone-analyzer-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "tone_analyzer": {
            "credentials": "Credentials-1",
            "instance": "Watson Tone Analyzer",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/assistant/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

## Using the {{site.data.keyword.toneanalyzershort}} package
{: #usage}

To use the actions in this package, run commands in the following format:

```
ibmcloud wsk action invoke tone-analyzer-v3/<action_name> -b -p <param name> <param>
```
{: pre}
