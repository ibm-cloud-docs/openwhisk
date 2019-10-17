---

copyright:
  years: 2017, 2019
lastupdated: "2019-10-17"

keywords: watson, translator, cognitive, translating text, language, functions

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

# {{site.data.keyword.languagetranslatorshort}}
{: #pkg_translator}

The installable {{site.data.keyword.languagetranslatorfull}} package translates text from one language to another. The service offers multiple IBM provided translation models that you can customize based on your unique terminology and language. For more information about this service, see [{{site.data.keyword.languagetranslatorfull}}](/docs/services/language-translator?topic=language-translator-gettingstarted).
{: shortdesc}

The {{site.data.keyword.languagetranslatorshort}} package contains the following entities. You can find more details in the {{site.data.keyword.languagetranslatorshort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`language-translator-v3`](https://cloud.ibm.com/apidocs/language-translator){: external} | Package | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Work with the {{site.data.keyword.languagetranslatorshort}} service. |
| [`translate`](https://cloud.ibm.com/apidocs/language-translator#translate){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `model_id`, `source`, `target` | Translate text. |
| [`identify`](https://cloud.ibm.com/apidocs/language-translator#identify-language){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text` | Identify the language of text. |
| [`list-identifiable-languages`](https://cloud.ibm.com/apidocs/language-translator#list-identifiable-languages){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | List identifiable languages. |
| [`create-model`](https://cloud.ibm.com/apidocs/language-translator#create-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `base_model_id`, `name`, `forced_glossary`, `parallel_corpus` | Create a model. |
| [`delete-model`](https://cloud.ibm.com/apidocs/language-translator#delete-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id` | Delete a model. |
| [`get-model`](https://cloud.ibm.com/apidocs/language-translator#get-model-details){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id` | Get model details. |
| [`list-models`](https://cloud.ibm.com/apidocs/language-translator#list-models){: external} | Action | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `source`, `target`, `default_models` | List models. |

## Creating a {{site.data.keyword.languagetranslatorshort}} service instance
{: #service_instance_translator}

Before you install the package, you must create a {{site.data.keyword.languagetranslatorshort}} service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.languagetranslatorshort}} service instance ](https://cloud.ibm.com/catalog/services/language_translator){: external}.
2. When the service instance is created, auto-generated service credentials are also created for you.

## Installing the {{site.data.keyword.languagetranslatorshort}} package
{: #install_translator}

After you have a {{site.data.keyword.languagetranslatorshort}} service instance, install the {{site.data.keyword.languagetranslatorshort}} package into your namespace.
{: shortdesc}

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #languagetranslator_cli}

Install the {{site.data.keyword.languagetranslatorshort}} package from the CLI. Be sure to [install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=cloud-functions-cli_install) first.
{: shortdesc}

1. Clone the {{site.data.keyword.languagetranslatorshort}} package repo.

   ```
   git clone https://github.com/watson-developer-cloud/openwhisk-sdk
   ```
   {: pre}

2. Deploy the package.

   ```
   ibmcloud fn deploy -m openwhisk-sdk/packages/language-translator-v3/manifest.yaml
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
   /myOrg_mySpace/language-translator-v3                        private
   ```
   {: screen}

4. Bind the credentials from the {{site.data.keyword.languagetranslatorshort}} instance you created to the package.

   ```
   ibmcloud fn service bind language-translator language-translator-v3
   ```
   {: pre}

   **Example output**

   ```
   Credentials 'Credentials-1' from 'language-translator' service instance 'Watson Language Translator' bound to 'language-translator-v3'.
   ```
   {: screen}

5. Verify that the package is configured with your {{site.data.keyword.languagetranslatorshort}} service instance credentials.

   ```
   ibmcloud fn package get language-translator-v3 parameters
   ```
   {: pre}

   **Example output**

   ```
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

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page ](https://cloud.ibm.com/openwhisk/create){: external}.

2. Select the namespace that you want to install the package into. Namespaces are formed from the combined org and space names.

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
    2. In the left-hand navigation, click the **Parameters** section.
    3. Enter a new `parameter`. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.languagetranslatorshort}} package
{: #usage_translator}

To use the actions in this package, run commands in the following format:
{: shortdesc}

```
ibmcloud fn action invoke language-translator-v3/<action_name> -b -p <param name> <param>
```
{: pre}

All actions require a version parameter in the format YYYY-MM-DD. When the API is changed in a backwards-incompatible way, a new version date is released. See more details in the [API reference](https://cloud.ibm.com/apidocs/language-translator#versioning){: external}.

This package's functions use the current version of Language Translator, `2018-05-01`. Try out the `identify` action.

```
ibmcloud fn action invoke language-translator-v3/identify -b -p version 2018-05-01 -p text hola
```
{: pre}
