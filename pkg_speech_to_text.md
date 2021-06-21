---

copyright:
  years: 2017, 2021
lastupdated: "2021-06-21"

keywords: package, cognitive, functions, watson, speech to text

subcollection: openwhisk

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

# {{site.data.keyword.speechtotextshort}}
{: #pkg_speech_to_text}

The installable {{site.data.keyword.speechtotextfull}} service provides an [API](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html){: external} that uses IBM's speech-recognition capabilities to produce transcripts of spoken audio.
{:shortdesc}

The Watson packages are deprecated. Please use the [Watson SDKs](/docs/watson?topic=watson-using-sdks){: external} to perform Watson related functionality. For more information about which Watson SDK is included in the available runtimes, see the [Functions runtime](/docs/openwhisk?topic=openwhisk-runtimes) documentation.
{: deprecated}

This service can transcribe speech from various languages and audio formats. It addition to basic transcription, the service can produce detailed information about many aspects of the audio. For most languages, the service supports two sampling rates, broadband and narrowband. It returns all JSON response content in the `UTF-8` character set. For more information about the service, see [{{site.data.keyword.speechtotextshort}}](/docs/speech-to-text?topic=speech-to-text-about).

The {{site.data.keyword.speechtotextshort}} package contains the following entities. You can find more information in the {{site.data.keyword.speechtotextshort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](https://cloud.ibm.com/apidocs/speech-to-text){: external} | Package | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Work with the {{site.data.keyword.speechtotextshort}} V1 service. |
| [`get-model`](https://cloud.ibm.com/apidocs/speech-to-text#get-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id` | Get a model. |
| [`list-models`](https://cloud.ibm.com/apidocs/speech-to-text#list-models){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | List models. |
| [`recognize-audio`](https://cloud.ibm.com/apidocs/speech-to-text#recognize-audio){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `audio`, `content_type`, `model`, `customization_id`, `acoustic_customization_id`, `base_model_version`, `customization_weight`, `inactivity_timeout`, `keywords`, `keywords_threshold`, `max_alternatives`, `word_alternatives_threshold`, `word_confidence`, `timestamps`, `profanity_filter`, `smart_formatting`, `speaker_labels` | Recognize audio. |
| [`check-job`](https://cloud.ibm.com/apidocs/speech-to-text#check-a-job){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `id`  | Check a job. |
| [`check-jobs`](https://cloud.ibm.com/apidocs/speech-to-text#check-jobs){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Check jobs. |
| [`create-job`](https://cloud.ibm.com/apidocs/speech-to-text#create-job){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `audio`, `content_type`, `model`, `callback_url`, `events`, `user_token`, `results_ttl`, `customization_id`, `acoustic_customization_id`, `base_model_version`, `customization_weight`, `inactivity_timeout`, `keywords`, `keywords_threshold`, `max_alternatives`, `word_alternatives_threshold`, `word_confidence`, `timestamps`, `profanity_filter`, `smart_formatting`, `speaker_labels` | Create a job. |
| [`delete-job`](https://cloud.ibm.com/apidocs/speech-to-text#delete-a-job){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `id` | Delete a job. |
| [`register-callback`](https://cloud.ibm.com/apidocs/speech-to-text#register-a-callback){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `callback_url`, `user_secret` | Register a callback. |
| [`unregister-callback`](https://cloud.ibm.com/apidocs/speech-to-text#unregister-a-callback){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `callback_url` | Unregister a callback. |
| [`create-language-model`](https://cloud.ibm.com/apidocs/speech-to-text#create-a-custom-language-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `base_model_name`, `dialect`, `description` | Create a custom language model. |
| [`delete-language-model`](https://cloud.ibm.com/apidocs/speech-to-text#delete-a-custom-language-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Delete a custom language model. |
| [`get-language-model`](https://cloud.ibm.com/apidocs/speech-to-text#get-a-custom-language-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Get a custom language model. |
| [`list-language-models`](https://cloud.ibm.com/apidocs/speech-to-text#list-custom-language-models){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` | List custom language models. |
| [`reset-language-model`](https://cloud.ibm.com/apidocs/speech-to-text#reset-a-custom-language-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Reset a custom language model. |
| [`train-language-model`](https://cloud.ibm.com/apidocs/speech-to-text#train-a-custom-language-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_type_to_add`, `customization_weight` | Train a custom language model. |
| [`upgrade-language-model`](https://cloud.ibm.com/apidocs/speech-to-text#upgrade-a-custom-language-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Upgrade a custom language model. |
| [`add-corpus`](https://cloud.ibm.com/apidocs/speech-to-text#add-a-corpus){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name`, `corpus_file`, `allow_overwrite` | Add a corpus. |
| [`delete-corpus`](https://cloud.ibm.com/apidocs/speech-to-text#delete-a-corpus){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name` | Delete a corpus. |
| [`get-corpus`](https://cloud.ibm.com/apidocs/speech-to-text#get-a-corpus){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name` | Get a corpus. |
| [`list-corpora`](https://cloud.ibm.com/apidocs/speech-to-text#list-corpora){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | List corpora. |
| [`add-word`](https://cloud.ibm.com/apidocs/speech-to-text#add-a-custom-word){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name`, `word`, `sounds_like`, `display_as` | Add a custom word. |
| [`add-words`](https://cloud.ibm.com/apidocs/speech-to-text#add-custom-words){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `words` | Add custom words. |
| [`delete-word`](https://cloud.ibm.com/apidocs/speech-to-text#delete-a-custom-word){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name` | Delete a custom word. |
| [`get-word`](https://cloud.ibm.com/apidocs/speech-to-text#get-a-custom-word){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name` | Get a custom word. |
| [`list-words`](https://cloud.ibm.com/apidocs/speech-to-text#list-custom-words){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_type`, `sort` | List custom words. |
| [`create-acoustic-model`](https://cloud.ibm.com/apidocs/speech-to-text#create-a-custom-acoustic-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `base_model_name`, `description` | Create a custom acoustic model. |
| [`delete-acoustic-model`](https://cloud.ibm.com/apidocs/speech-to-text#delete-a-custom-acoustic-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Delete a custom acoustic model. |
| [`get-acoustic-model`](https://cloud.ibm.com/apidocs/speech-to-text#get-a-custom-acoustic-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Get a custom acoustic model. |
| [`list-acoustic-models`](https://cloud.ibm.com/apidocs/speech-to-text#list-custom-acoustic-models){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` | List custom acoustic models. |
| [`reset-acoustic-model`](https://cloud.ibm.com/apidocs/speech-to-text#reset-a-custom-acoustic-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Reset a custom acoustic model. |
| [`train-acoustic-model`](https://cloud.ibm.com/apidocs/speech-to-text#train-a-custom-acoustic-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `custom_language_model_id` | Train a custom acoustic model. |
| [`upgrade-acoustic-model`](https://cloud.ibm.com/apidocs/speech-to-text#upgrade-a-custom-acoustic-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `custom_language_model_id` | Upgrade a custom acoustic model. |
| [`add-audio`](https://cloud.ibm.com/apidocs/speech-to-text#add-an-audio-resource){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name`, `audio_resource`, `content_type`, `contained_content_type`, `allow_overwrite` | Add an audio resource. |
| [`delete-audio`](https://cloud.ibm.com/apidocs/speech-to-text#delete-an-audio-resource){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name` | Delete an audio resource. |
| [`get-audio`](https://cloud.ibm.com/apidocs/speech-to-text#get-an-audio-resource){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name` | Get an audio resource. |
| [`list-audio`](https://cloud.ibm.com/apidocs/speech-to-text#list-audio-resources){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | List audio resources. |
| [`delete-user-data`](https://cloud.ibm.com/apidocs/speech-to-text#delete-labeled-data){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id` | Delete labeled data. |

## Creating a {{site.data.keyword.speechtotextshort}} service instance
{: #service_instance_speechtotext}

Before you install the package, you must create a {{site.data.keyword.speechtotextshort}} service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.speechtotextshort}} service instance ](https://cloud.ibm.com/catalog/services/speech_to_text){: external}.
2. When the service instance is created, auto-generated service credentials are also created for you.

## Installing the {{site.data.keyword.speechtotextshort}} package
{: #install_speechtotext}

After you have an {{site.data.keyword.speechtotextshort}} service instance, install the {{site.data.keyword.speechtotextshort}} package into your namespace.
{: shortdesc}

## Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #speechtotext_cli}

Install the {{site.data.keyword.speechtotextshort}} package from the CLI. Be sure to [install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=openwhisk-cli_install) first.
{: shortdesc}

To install the {{site.data.keyword.speechtotextshort}} package:

1. Clone the {{site.data.keyword.speechtotextshort}} package repo.

   ```
   git clone https://github.com/watson-developer-cloud/openwhisk-sdk
   ```
   {: pre}

2. Deploy the package.

   ```
   ibmcloud fn deploy -m openwhisk-sdk/packages/speech-to-text-v1/manifest.yaml
   ```
   {: pre}

3. Verify that the package is added to your package list.

   ```
   ibmcloud fn package list
   ```
   {: pre}

    **Example output**

   ```
   packages
   /myOrg_mySpace/speech-to-text-v1                        private
   ```
   {: screen}

4. Bind the credentials from the {{site.data.keyword.speechtotextshort}} instance you created to the package.

   ```
   ibmcloud fn service bind speech_to_text speech-to-text-v1
   ```
   {: pre}

   Depending on the region where you created the service instance, the service instance might be named differently because it is an IAM service. If the command fails, use the following service name for the bind command.
   
   ```
   ibmcloud fn service bind speech-to-text speech-to-text-v1
   ```
   {: pre}

   **Example output**

   ```
   Credentials 'Credentials-1' from 'speech_to_text' service instance 'Watson Speech to Text' bound to 'speech-to-text-v1'.
   ```
   {: screen}

5. Verify that the package is configured with your {{site.data.keyword.speechtotextshort}} service instance credentials.

   ```
   ibmcloud fn package get speech-to-text-v1 parameters
   ```
   {: pre}

   **Example output**

   ```
   ok: got package speech-to-text-v1, displaying field parameters
   [
      {
        "key": "__bx_creds",
        "value": {
          "speech_to_text": {
            "credentials": "Credentials-1",
            "instance": "Watson Speech to Text",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/speech_to_text/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
   ]
   ```
   {: screen}

## Installing from the {{site.data.keyword.openwhisk_short}} console
{: #speechtotext_ui}

Install the {{site.data.keyword.speechtotextshort}} package from the console.
{: shortdesc}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page ](https://cloud.ibm.com/functions/create){: external}.

2. Select the namespace that you want to install the package into.

3. Click **Install Packages**.

4. Click the **Watson** Package group.

5. Click the **Speech To Text** Package.

6. Click **Install**.

7. After the package is installed you are redirected to the actions page and can search for your new package, which is named `speech-to-text-v1`.

8. To use the actions in the **speech-to-text-v1** package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, follow steps 4 and 5 in the [CLI instructions](#speechtotext_cli).
  * To bind service credentials to individual actions, complete the following steps in the console.
  
  You must complete the following steps for each action that you want to use.
  {: note}

    1. Click an action from the **speech-to-text-v1** package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.speechtotextshort}} package
{: #usage_speechtotext}

To use the actions in this package, run commands in the following format:
{: shortdesc}

```
ibmcloud fn action invoke speech-to-text-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Try out the `list-models` action.

```
ibmcloud fn action invoke speech-to-text-v1/list-models -b
```
{: pre}
