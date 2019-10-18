---

copyright:
  years: 2017, 2019
lastupdated: "2019-10-18"

keywords: cognitive, functions, packages

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

# {{site.data.keyword.texttospeechshort}}
{: #pkg_text_to_speech}

The installable {{site.data.keyword.texttospeechfull}} Service provides an API with IBM's speech-synthesis capabilities to synthesize text into natural-sounding speech in a various languages, dialects, and voices.
{:shortdesc}

The service supports at least one male or female voice, sometimes both, for each language. The audio is streamed back to the client with minimal delay. For more information about the service, see [{{site.data.keyword.texttospeechshort}}](/docs/services/text-to-speech?topic=text-to-speech-about).

The {{site.data.keyword.texttospeechshort}} package contains the following entities. You can find more information in the {{site.data.keyword.texttospeechshort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`text-to-speech-v1`](https://cloud.ibm.com/apidocs/text-to-speech/text-to-speech){: external} | Package | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Work with the {{site.data.keyword.texttospeechshort}} Service. |
| [`get-voice`](https://cloud.ibm.com/apidocs/text-to-speech/text-to-speech#get-a-voice){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `voice`, `customization_id` | Get a voice. |
| [`list-voices`](https://cloud.ibm.com/apidocs/text-to-speech/text-to-speech#list-voices){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | List voices. |
| [`synthesize`](https://cloud.ibm.com/apidocs/text-to-speech/text-to-speech#synthesize-audio){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `accept`, `voice`, `customization_id` | Synthesize audio. |
| [`get-pronunciation`](https://cloud.ibm.com/apidocs/text-to-speech/text-to-speech#get-pronunciation){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `voice`, `format`, `customization_id` | Get pronunciation. |
| [`create-voice-model`](https://cloud.ibm.com/apidocs/text-to-speech/text-to-speech#create-a-custom-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `language`, `description` | Create a custom model. |
| [`delete-voice-model`](https://cloud.ibm.com/apidocs/text-to-speech/text-to-speech#delete-a-custom-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Delete a custom model. |
| [`get-voice-model`](https://cloud.ibm.com/apidocs/text-to-speech/text-to-speech#get-a-custom-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Get a custom model. |
| [`list-voice-models`](https://cloud.ibm.com/apidocs/text-to-speech/text-to-speech#list-custom-models){: external} | Action | `username`,`password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` | List custom models. |
| [`update-voice-model`](https://cloud.ibm.com/apidocs/text-to-speech/text-to-speech#update-a-custom-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `name`, `description`, `words` | Update a custom model. |
| [`add-word`](https://cloud.ibm.com/apidocs/text-to-speech/text-to-speech#add-a-custom-word){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word`, `translation`, `part_of_speech` | Add a custom word. |
| [`add-words`](https://cloud.ibm.com/apidocs/text-to-speech/text-to-speech#add-custom-words){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `words`  | Add custom words. |
| [`delete-word`](https://cloud.ibm.com/apidocs/text-to-speech/text-to-speech#delete-a-custom-word){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word` | Delete a custom word. |
| [`get-word`](https://cloud.ibm.com/apidocs/text-to-speech/text-to-speech#get-a-custom-word){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word` | Get a custom word. |
| [`list-words`](https://cloud.ibm.com/apidocs/text-to-speech/text-to-speech#list-custom-words){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | List custom words. |
| [`delete-user-data`](https://cloud.ibm.com/apidocs/text-to-speech/text-to-speech#delete-labeled-data){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id` | Delete labeled data. |

### Creating a {{site.data.keyword.texttospeechshort}} Service instance
{: #service_instance_texttospeech}

Before you install the package, you must create a {{site.data.keyword.texttospeechshort}} Service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.texttospeechshort}} Service instance ](https://cloud.ibm.com/catalog/services/text_to_speech){: external}.
2. When the service instance is created, auto-generated service credentials are also created for you.

### Installing the {{site.data.keyword.texttospeechshort}} package
{: #install_texttospeech}

After you have an {{site.data.keyword.texttospeechshort}} Service instance, install the {{site.data.keyword.texttospeechshort}} package into your namespace.
{: shortdesc}

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #texttospeech_cli}

Install the {{site.data.keyword.texttospeechshort}} package from the CLI. Be sure to [install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=cloud-functions-cli_install) first.
{: shortdesc}

To install the {{site.data.keyword.texttospeechshort}} package, run the following commands.

1. Clone the {{site.data.keyword.texttospeechshort}} package repo.

   ```
   git clone https://github.com/watson-developer-cloud/openwhisk-sdk
   ```
   {: pre}

2. Deploy the package.

   ```
   ibmcloud fn deploy -m openwhisk-sdk/packages/text-to-speech-v1/manifest.yaml
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
   /myOrg_mySpace/text-to-speech-v1                        private
   ```
   {: screen}

4. Bind the credentials from the {{site.data.keyword.texttospeechshort}} instance you created to the package.
  
   ```
   ibmcloud fn service bind text_to_speech text-to-speech-v1
   ```
   {: pre}

   Depending on the region where you created the service instance, the service instance might be named differently because it is an IAM service. If the above command fails, use the following service name for the bind command:
  
   ```
   ibmcloud fn service bind text-to-speech text-to-speech-v1
   ```
   {: pre}

   **Example output**

   ```
   Credentials 'Credentials-1' from 'text_to_speech' service instance 'Watson Text to Speech' bound to 'text-to-speech-v1'.
   ```
   {: screen}

5. Verify that the package is configured with your {{site.data.keyword.texttospeechshort}} Service instance credentials.
  
   ```
   ibmcloud fn package get text-to-speech-v1 parameters
   ```
   {: pre}

   **Example output**

   ```
   ok: got package text-to-speech-v1, displaying field parameters
   [
      {
        "key": "__bx_creds",
        "value": {
          "text_to_speech": {
            "credentials": "Credentials-1",
            "instance": "Watson Text to Speech",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/text_to_speech/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
   ]
   ```
   {: screen}

### Installing from the {{site.data.keyword.openwhisk_short}} console
{: #texttospeech_ui}

Install the {{site.data.keyword.personalityinsightsshort}} package from the console.
{: shortdesc}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page ](https://cloud.ibm.com/openwhisk/create){: external}.

2. Select the namespace that you want to install the package into.

3. Click **Install Packages**.

4. Click the **Watson** Package group.

5. Click the **Text To Speech** Package.

6. Click **Install**.

7. After the package has been installed you will be redirected to the actions page and can search for your new package, which is named `text-to-speech-v1`.

8. To use the actions in the `text-to-speech-v1` package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, follow steps [4 and 6 in the CLI instructions](#texttospeech_cli).
  * To bind service credentials to individual actions, complete the following steps in the console.

  You must complete the following steps for each action that you want to use.
  {: note}

    1. Click an action from the `text-to-speech-v1` package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click the **Parameters** section.
    3. Enter a new parameter. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.texttospeechshort}} package
{: #usage_texttospeech}

To use the actions in this package, run commands in the following format:

```
ibmcloud fn action invoke text-to-speech-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Try out the `list-voices` action.

```
ibmcloud fn action invoke text-to-speech-v1/list-voices -b
```
{: pre}
