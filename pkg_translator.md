---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-01"

keywords: watson, translator, cognitive, translating text, language, functions

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.languagetranslatorshort}} 
{: #pkg_translator}

{{site.data.keyword.openwhisk}} is deprecated. Existing Functions entities such as actions, triggers, or sequences will continue to run, but as of 28 December 2023, you can’t create new Functions entities. Existing Functions entities are supported until October 2024. Any Functions entities that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

The installable {{site.data.keyword.languagetranslatorfull}} package translates text from one language to another. The service offers multiple IBM provided translation models that you can customize based on your unique terminology and language.
{: shortdesc}

The Watson packages are deprecated. Please use the [Watson SDKs](/docs/watson?topic=watson-using-sdks){: external} to perform Watson related functionality. For more information about which Watson SDK is included in the available runtimes, see the [Functions runtime](/docs/openwhisk?topic=openwhisk-runtimes) documentation.
{: deprecated}

The {{site.data.keyword.languagetranslatorshort}} package contains the following entities. You can find more details in the {{site.data.keyword.languagetranslatorshort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `language-translator-v3` | Package | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Work with the {{site.data.keyword.languagetranslatorshort}} service. |
| `translate` | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `model_id`, `source`, `target` | Translate text. |
| `identify` | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text` | Identify the language of text. |
| `list-identifiable-languages` | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | List identifiable languages. |
| `create-model` | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `base_model_id`, `name`, `forced_glossary`, `parallel_corpus` | Create a model. |
| `delete-model` | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id` | Delete a model. |
| `get-model` | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id` | Get model details. |
| `list-models` | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `source`, `target`, `default_models` | List models. |
{: caption="Table 1. Language translator" caption-side="bottom"}

## Creating a {{site.data.keyword.languagetranslatorshort}} service instance
{: #service_instance_translator}

Before you install the package, you must create a {{site.data.keyword.languagetranslatorshort}} service instance and service credentials.
{: shortdesc}

1. Create a {{site.data.keyword.languagetranslatorshort}} service instance.
2. When the service instance is created, service credentials are automatically created for you.

## Installing the {{site.data.keyword.languagetranslatorshort}} package
{: #install_translator}

After you have a {{site.data.keyword.languagetranslatorshort}} service instance, install the {{site.data.keyword.languagetranslatorshort}} package into your namespace.
{: shortdesc}

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #languagetranslator_cli}

Install the {{site.data.keyword.languagetranslatorshort}} package from the CLI. Be sure to [install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=openwhisk-cli_install) first.
{: shortdesc}

1. Clone the {{site.data.keyword.languagetranslatorshort}} package repo.

    ```sh
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Deploy the package.

    ```sh
    ibmcloud fn deploy -m openwhisk-sdk/packages/language-translator-v3/manifest.yaml
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
    /myOrg_mySpace/language-translator-v3                        private
    ```
    {: screen}

4. Bind the credentials from the {{site.data.keyword.languagetranslatorshort}} instance you created to the package.

    ```sh
    ibmcloud fn service bind language-translator language-translator-v3
    ```
    {: pre}

    **Example output**

    ```sh
    Credentials 'Credentials-1' from 'language-translator' service instance 'Watson Language Translator' bound to 'language-translator-v3'.
    ```
    {: screen}

5. Verify that the package is configured with your {{site.data.keyword.languagetranslatorshort}} service instance credentials.

    ```sh
    ibmcloud fn package get language-translator-v3 parameters
    ```
    {: pre}

    **Example output**

    ```sh
    ok: got package language-translator-v3, displaying field parameters
    [
        {
        "key": "__bx_creds",
        "value": {
          "language-translator": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:ibmcloud:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:ibmcloud:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
            "instance": "Language Translator-g2",
            "url": "https://gateway.watsonplatform.net/language-translator/api"
          }
        }
        }
    ]
    ```
    {: screen}

### Installing from the {{site.data.keyword.openwhisk_short}} console
{: #languagetranslator_ui}

Install the {{site.data.keyword.languagetranslatorshort}} package from the console.
{: shortdesc}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page](https://cloud.ibm.com/functions/create){: external}.

2. Select the namespace that you want to install the package into.

3. Click **Install Packages**.

4. Click the **Watson** Package group.

5. Click the **Language Translator** Package.

6. Click **Install**.

7. After the package has been installed you are redirected to the actions page and can search for your new package, which is named `language-translator-v3`.

8. To use the actions in the `language-translator-v3` package, you must bind service credentials to the actions.
    * To bind service credentials to all actions in the package, follow steps [5 and 6 in the CLI instructions](#languagetranslator_cli).
    * To bind service credentials to individual actions, complete the following steps in the console.

    You must complete the following steps for each action that you want to use.
    {: note}

    1. Click an action from the `language-translator-v3` package that you want to use. The details page for that action opens.
    2. In the left navigation, click the **Parameters** section.
    3. Enter a new `parameter`. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.languagetranslatorshort}} package
{: #usage_translator}

To use the actions in this package, run commands in the following format:
{: shortdesc}

```sh
ibmcloud fn action invoke language-translator-v3/<action_name> -b -p <param name> <param>
```
{: pre}

All actions require a version parameter in the format `YYYY-MM-DD`. When the API is changed in a backwards-incompatible way, a new version date is released. 

This package's functions use the current version of Language Translator, `2018-05-01`. Try out the `identify` action.

```sh
ibmcloud fn action invoke language-translator-v3/identify -b -p version 2018-05-01 -p text hola
```
{: pre}


