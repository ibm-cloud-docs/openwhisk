---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: natural language, understanding, watson knowledge studio, functions

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


# {{site.data.keyword.nlushort}}
{: #pkg_natlang_understanding}

The installable {{site.data.keyword.nlufull}} service helps you analyze various features of text content at scale.
{: shortdesc}

Provide text, raw HTML, or a public URL, and {{site.data.keyword.nlushort}} gives you results for the features you request. The service cleans HTML content before analysis by default, so the results can ignore most advertisements and other unwanted content. You can create <a target="_blank" href="https://www.ibm.com/watson/developercloud/doc/natural-language-understanding/customizing.html">custom models</a> with Watson Knowledge Studio that can be used to detect custom entities and relations in Natural Language Understanding.

The {{site.data.keyword.nlushort}} package contains the following entities. For more information, see the {{site.data.keyword.nlushort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`natural-language-understanding-v1`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl.html){: external} | Package | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,  |  Work with the {{site.data.keyword.nlushort}} service. |
| [`analyze`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#analyze){: external} | Action |  `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `features`, `text`, `html`, `url`, `clean`, `xpath`, `fallback_to_raw`, `return_analyzed_text`, `language`, `limit_text_characters`,  | Analyze text, HTML, or a public web page. |
| [`delete-model`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#delete-model){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id`,  | Delete model. |
| [`list-models`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#list-models){: external} | Action |  `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, | List models. |

## Creating a {{site.data.keyword.nlushort}} service instance
{: #service_instance_understanding}

Before you install the package, you must create a {{site.data.keyword.nlushort}} service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.nlushort}} service instance ](https://cloud.ibm.com/catalog/services/natural-language-understanding){: external}.
2. When the service instance is created, auto-generated service credentials are also created for you.

## Installing the {{site.data.keyword.nlushort}} package
{: #install_understanding}

After you have a {{site.data.keyword.nlushort}} service instance, use the {{site.data.keyword.openwhisk}} CLI to install the {{site.data.keyword.nlushort}} package into your namespace.
{: shortdesc}

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #nlus_cli}

**Before you begin**
[Install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=cloud-functions-cli_install).

To install the {{site.data.keyword.nlushort}} package:

1. Clone the {{site.data.keyword.nlushort}} package repo.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Deploy the package.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/natural-language-understanding-v1/manifest.yaml
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
    /myOrg_mySpace/natural-language-understanding-v1                        private
    ```
    {: screen}

4. Bind the credentials from the {{site.data.keyword.nlushort}} instance you created to the package.
    ```
    ibmcloud fn service bind natural-language-understanding natural-language-understanding-v1
    ```
    {: pre}

    **Example output**
    ```
    Credentials 'Credentials-1' from 'natural-language-understanding' service instance 'Watson Natural Language Understanding' bound to 'natural-language-understanding-v1'.
    ```
    {: screen}

5. Verify that the package is configured with your {{site.data.keyword.nlushort}} service instance credentials.
    ```
    ibmcloud fn package get natural-language-understanding-v1 parameters
    ```
    {: pre}

    **Example output**
    ```
    ok: got package natural-language-understanding-v1, displaying field parameters
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

### Installing from the {{site.data.keyword.openwhisk_short}} UI
{: #nlus_ui}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page ](https://cloud.ibm.com/openwhisk/create){: external}.

2. Select the namespace that you want to install the package into by using the namespace drop-down menu. Namespaces are formed from the combined org and space names.

3. Click **Install Packages**.

4. Click the **Watson** Package group.

5. Click the **Natural Language Understanding** Package.

5. Click **Install**.

6. Once the package is installed you are redirected to the actions page and can search for your new package, which is named **natural-language-understanding-v1**.

7. To use the actions in the **natural-language-understanding-v1** Package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, follow steps 4 and 5 in the [CLI instructions](#nlus_cli).
  * To bind service credentials to individual actions, complete the following steps in the UI. 

  You must complete the following steps for each action that you want to use.
  {: note}

    1. Click an Action from the **natural-language-understanding-v1** package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.nlushort}} package
{: #usage_understanding}

To use the actions in this package, run commands in the following format:

```
ibmcloud fn action invoke natural-language-understanding-v1/<action_name> -b -p <param name> <param>
```
{: pre}

All actions require a version parameter in the format YYYY-MM-DD. When the API is changed in a backwards-incompatible way, a new version date is released. See more details in the [API reference](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/#versioning){: external}.

This package's functions use the current version of Natural Language Understanding, 2018-03-16. Try out the `list-models` action.
```
ibmcloud fn action invoke natural-language-understanding-v1/list-models -b -p version 2018-03-16
```
{: pre}



