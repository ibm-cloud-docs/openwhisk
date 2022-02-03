---

copyright:
  years: 2017, 2022
lastupdated: "2022-01-12"

keywords: natural language, understanding, watson knowledge studio, functions

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.nlushort}}
{: #pkg_natlang_understanding}

The installable {{site.data.keyword.nlufull}} service helps you analyze various features of text content at scale.
{: shortdesc}

The Watson packages are deprecated. Please use the [Watson SDKs](/docs/watson?topic=watson-using-sdks){: external} to perform Watson related functionality. For more information about which Watson SDK is included in the available runtimes, see the [Functions runtime](/docs/openwhisk?topic=openwhisk-runtimes) documentation.
{: deprecated}

Provide text, raw HTML, or a public URL, and {{site.data.keyword.nlushort}} gives you results for the features you request. The service cleans HTML content before analysis by default, so the results can ignore most advertisements and other unwanted content. You can create [custom models](/docs/natural-language-understanding?topic=natural-language-understanding-customizing) with Watson Knowledge Studio that can be used to detect custom entities and relations in Natural Language Understanding. For more information about this service, see [{{site.data.keyword.nlushort}}](/docs/natural-language-understanding?topic=natural-language-understanding-getting-started).

The {{site.data.keyword.nlushort}} package contains the following entities. For more information, see the {{site.data.keyword.nlushort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`natural-language-understanding-v1`](https://cloud.ibm.com/apidocs/natural-language-understanding){: external} | Package | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,  |  Work with the {{site.data.keyword.nlushort}} service. |
| [`analyze`](https://cloud.ibm.com/apidocs/natural-language-understanding#analyze-text){: external} | Action |  `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `features`, `text`, `html`, `url`, `clean`, `xpath`, `fallback_to_raw`, `return_analyzed_text`, `language`, `limit_text_characters`, | Analyze text, HTML, or a public web page. |
| [`delete-model`](https://cloud.ibm.com/apidocs/natural-language-understanding#delete-model){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id`,  | Delete model. |
| [`list-models`](https://cloud.ibm.com/apidocs/natural-language-understanding#list-models){: external} | Action |  `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, | List models. |
{: caption="Table 1. National language Understanding package entities" caption-side="bottom"}

## Creating a {{site.data.keyword.nlushort}} service instance
{: #service_instance_understanding}

Before you install the package, you must create a {{site.data.keyword.nlushort}} service instance and service credentials.
{: shortdesc}


1. [Create a {{site.data.keyword.nlushort}} service instance](https://cloud.ibm.com/catalog/services/natural-language-understanding){: external}.
2. When the service instance is created, auto-generated service credentials are also created for you.

## Installing the {{site.data.keyword.nlushort}} package
{: #install_understanding}

After you have a {{site.data.keyword.nlushort}} service instance, install the {{site.data.keyword.nlushort}} package into your namespace.
{: shortdesc}

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #nlus_cli}

Install the {{site.data.keyword.nlushort}} package from the CLI. Be sure to [install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=openwhisk-cli_install) first.
{: shortdesc}

To install the {{site.data.keyword.nlushort}} package:

1. Clone the {{site.data.keyword.nlushort}} package repo.

    ```sh
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Deploy the package.

    ```sh
    ibmcloud fn deploy -m openwhisk-sdk/packages/natural-language-understanding-v1/manifest.yaml
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
    /myOrg_mySpace/natural-language-understanding-v1                        private
    ```
    {: screen}

4. Bind the credentials from the {{site.data.keyword.nlushort}} instance you created to the package.

    ```sh
    ibmcloud fn service bind natural-language-understanding natural-language-understanding-v1
    ```
    {: pre}

    **Example output**

    ```sh
    Credentials 'Credentials-1' from 'natural-language-understanding' service instance 'Watson Natural Language Understanding' bound to 'natural-language-understanding-v1'.
    ```
    {: screen}

5. Verify that the package is configured with your {{site.data.keyword.nlushort}} service instance credentials.

    ```sh
    ibmcloud fn package get natural-language-understanding-v1 parameters
    ```
    {: pre}

    **Example output**

    ```sh
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

### Installing from the {{site.data.keyword.openwhisk_short}} console
{: #nlus_ui}

You can install your package from the {{site.data.keyword.openwhisk_short}} console.
{: shortdesc}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page](https://cloud.ibm.com/functions/create){: external}.

2. Select the namespace that you want to install the package into.

3. Click **Install Packages**.

4. Click the **Watson** Package group.

5. Click the **Natural Language Understanding** Package.

6. Click **Install**.

7. After the package is installed, you are redirected to the actions page and can search for your new package, which is named **natural-language-understanding-v1**.

8. To use the actions in the **natural-language-understanding-v1** Package, you must bind service credentials to the actions.
    * To bind service credentials to all actions in the package, follow steps 4 and 5 in the [CLI instructions](#nlus_cli).
    * To bind service credentials to individual actions, complete the following steps in the console.

    You must complete the following steps for each action that you want to use.
    {: note}

    1. Click an Action from the **natural-language-understanding-v1** package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.nlushort}} package
{: #usage_understanding}

To use the actions in this package, run commands in the following format:

```sh
ibmcloud fn action invoke natural-language-understanding-v1/<action_name> -b -p <param name> <param>
```
{: pre}

All actions require a version parameter in the format `YYYY-MM-DD`. When the API is changed in a backwards-incompatible way, a new version date is released. See more details in the [API reference](https://cloud.ibm.com/apidocs/natural-language-understanding#versioning){: external}.

This package's functions use the current version of Natural Language Understanding, 2018-03-16. Try out the `list-models` action.

```sh
ibmcloud fn action invoke natural-language-understanding-v1/list-models -b -p version 2018-03-16
```
{: pre}


