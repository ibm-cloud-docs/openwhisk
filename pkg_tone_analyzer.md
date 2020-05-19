---

copyright:
  years: 2017, 2020
lastupdated: "2020-04-24"

keywords: functions, serverless, watson

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

# {{site.data.keyword.toneanalyzershort}}
{: #pkg_tone_analyzer}

The installable {{site.data.keyword.toneanalyzerfull}} service uses linguistic analysis to detect emotional and language tones in written text.
{:shortdesc}

The service can analyze tone at both the document and sentence levels. You can use the service to understand how your written communications are perceived and then to improve the tone of your communications. Businesses can use the service to learn the tone of their customers' communications and to respond to each customer, or to understand and improve their customer conversations. For more information about this service, see [Tone analyzer](/docs/tone-analyzer?topic=tone-analyzer-gettingStarted).

Request logging is disabled for the Tone Analyzer service. The service doesn't log or retain data from requests and responses, regardless of whether the `X-Watson-Learning-Opt-Out` request header is set.
{: note}

The {{site.data.keyword.toneanalyzershort}} package contains the following entities. You can find more details in the {{site.data.keyword.toneanalyzershort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`tone-analyzer-v3`](https://cloud.ibm.com/apidocs/tone-analyzer){: external} | Package | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Work with the {{site.data.keyword.toneanalyzershort}} service. |
| [`tone`](https://cloud.ibm.com/apidocs/tone-analyzer#analyze-general-tone){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`,`iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,    `tone_input`, `content_type`, `sentences`, `tones`, `content_language`, `accept_language` | Analyze general tone. |
| [`tone-chat`](https://cloud.ibm.com/apidocs/tone-analyzer#analyze-customer-engagement-tone){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `utterances`, `content_language`, `accept_language` | Analyze customer engagement tone. |

## Creating a {{site.data.keyword.toneanalyzershort}} service instance
{: #service_instance_tone}

Before you install the package, you must create a {{site.data.keyword.toneanalyzershort}} service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.toneanalyzershort}} service instance ](https://cloud.ibm.com/catalog/tone_analyzer){: external}.
2. When the service instance is created, auto-generated service credentials are also created for you.

## Installing the {{site.data.keyword.toneanalyzershort}} package
{: #install_tone}

After you have a {{site.data.keyword.toneanalyzershort}} service instance, install the {{site.data.keyword.toneanalyzershort}} package into your namespace.
{: shortdesc}

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #toneanalyzer_cli}

Install the {{{site.data.keyword.toneanalyzershort}} package from the CLI. Be sure to [install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=openwhisk-cli_install) first.
{: shortdesc}

To install the {{site.data.keyword.toneanalyzershort}} package:

1. Clone the {{site.data.keyword.toneanalyzershort}} package repo.

   ```
   git clone https://github.com/watson-developer-cloud/openwhisk-sdk
   ```
   {: pre}

2. Deploy the package.

   ```
   ibmcloud fn deploy -m openwhisk-sdk/packages/tone-analyzer-v3/manifest.yaml
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
   /myOrg_mySpace/tone-analyzer-v3                        private
   ```
   {: screen}

4. Bind the credentials from the {{site.data.keyword.toneanalyzershort}} instance you created to the package.

   ```
   ibmcloud fn service bind tone_analyzer tone-analyzer-v3
   ```
   {: pre}

   Depending on the region where you created the service instance, the service instance might be named differently because it is an IAM service. If the command fails, use the following service name for the bind command:

   ```
   ibmcloud fn service bind tone-analyzer tone-analyzer-v3
   ```
   {: pre}

   **Example output**

   ```
   Credentials 'Credentials-1' from 'tone_analyzer' service instance 'Watson Tone Analyzer' bound to 'tone-analyzer-v3'.
   ```
   {: screen}

5. Verify that the package is configured with your {{site.data.keyword.toneanalyzershort}} service instance credentials.

   ```
   ibmcloud fn package get tone-analyzer-v3 parameters
   ```
   {: pre}

   **Example output**

   ```
   ok: got package tone-analyzer-v3, displaying field parameters
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

### Installing from the {{site.data.keyword.openwhisk_short}} console
{: #toneanalyzer_ui}

Install the {{site.data.keyword.toneanalyzershort}} package from the console.
{: shortdesc}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page ](https://cloud.ibm.com/functions/create){: external}.

2. Select the namespace that you want to install the package into.

3. Click **Install Packages**.

4. Click the **Watson** Package group.

5. Click the **Tone Analyzer** Package.

6. Click **Install**.

7. After the package is installed you are redirected to the actions page and can search for your new package, which is named **`tone-analyzer-v3`**.

8. To use the actions in the **tone-analyzer-v3** package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, follow steps 4 and 5 in the [CLI instructions](#toneanalyzer_cli).
  * To bind service credentials to individual actions, complete the following steps in the console:
  
  You must complete the following steps for each action that you want to use.
  {: note}
  
    1. Click an action from the **tone-analyzer-v3** Package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.toneanalyzershort}} package
{: #usage_tone}

To use the actions in this package, run commands in the following format:

```
ibmcloud fn action invoke tone-analyzer-v3/<action_name> -b -p <param name> <param>
```
{: pre}

All actions require a version parameter in the format YYYY-MM-DD. When the API is changed in a backwards-incompatible way, a new version date is released. See more details in the [API reference](https://cloud.ibm.com/apidocs/tone-analyzer#versioning){: external}.

This package's functions use the current version of Tone Analyzer, 2017-09-21. Try out the `tone` action.

```
ibmcloud fn action invoke tone-analyzer-v3/tone -b -p version 2017-09-21 -p text "i hope you're having a wonderful day"
```
{: pre}
