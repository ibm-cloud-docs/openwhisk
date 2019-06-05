---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

keywords: speech to text, watson, package, cognitive,

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# {{site.data.keyword.speechtotextshort}}
{: #pkg_speech_to_text}

## Packages

| Name | Availability | Description |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](#speech_to_text) | Installable | Works with the {{site.data.keyword.speechtotextshort}} V1 service. |
| [`/whisk.system/watson-speechToText`](#preinstall_speechtotext) | Pre-installed (Not available in Tokyo) | Call Watson APIs to convert the speech into text |

## {{site.data.keyword.speechtotextshort}} service
{: #speech_to_text}

The installable {{site.data.keyword.speechtotextfull}} service provides an [API](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html) that uses IBM's speech-recognition capabilities to produce transcripts of spoken audio.
{:shortdesc}

The service can transcribe speech from various languages and audio formats. It addition to basic transcription, the service can produce detailed information about many aspects of the audio. For most languages, the service supports two sampling rates, broadband and narrowband. It returns all JSON response content in the UTF-8 character set. For more information about the service, see the [IBM&reg; Cloud documentation](/docs/services/speech-to-text?topic=speech-to-text-about).

The {{site.data.keyword.speechtotextshort}} package contains the following entities. You can find more information in the {{site.data.keyword.speechtotextshort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html) | Package | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Work with the {{site.data.keyword.speechtotextshort}} V1 service. |
| [`get-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-model) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id` | Get a model. |
| [`list-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-models) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | List models. |
| [`recognize-sessionless`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#recognize-sessionless) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `audio`, `content_type`, `model`, `customization_id`, `acoustic_customization_id`, `base_model_version`, `customization_weight`, `inactivity_timeout`, `keywords`, `keywords_threshold`, `max_alternatives`, `word_alternatives_threshold`, `word_confidence`, `timestamps`, `profanity_filter`, `smart_formatting`, `speaker_labels` | Recognize audio (sessionless). |
| [`check-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-job) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `id`  | Check a job. |
| [`check-jobs`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-jobs) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Check jobs. |
| [`create-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-job) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `audio`, `content_type`, `model`, `callback_url`, `events`, `user_token`, `results_ttl`, `customization_id`, `acoustic_customization_id`, `base_model_version`, `customization_weight`, `inactivity_timeout`, `keywords`, `keywords_threshold`, `max_alternatives`, `word_alternatives_threshold`, `word_confidence`, `timestamps`, `profanity_filter`, `smart_formatting`, `speaker_labels` | Create a job. |
| [`delete-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-job) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `id` | Delete a job. |
| [`register-callback`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#register-callback) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `callback_url`, `user_secret` | Register a callback. |
| [`unregister-callback`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#unregister-callback) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `callback_url` | Unregister a callback. |
| [`create-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-language-model) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `base_model_name`, `dialect`, `description` | Create a custom language model. |
| [`delete-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-language-model) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Delete a custom language model. |
| [`get-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-language-model) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Get a custom language model. |
| [`list-language-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-language-models) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` | List custom language models. |
| [`reset-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-language-model) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Reset a custom language model. |
| [`train-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-language-model) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_type_to_add`, `customization_weight` | Train a custom language model. |
| [`upgrade-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-language-model) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Upgrade a custom language model. |
| [`add-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-corpus) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name`, `corpus_file`, `allow_overwrite` | Add a corpus. |
| [`delete-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-corpus) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name` | Delete a corpus. |
| [`get-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-corpus) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name` | Get a corpus. |
| [`list-corpora`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-corpora) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | List corpora. |
| [`add-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-word) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name`, `word`, `sounds_like`, `display_as` | Add a custom word. |
| [`add-words`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-words) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `words` | Add custom words. |
| [`delete-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-word) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name` | Delete a custom word. |
| [`get-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-word) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name` | Get a custom word. |
| [`list-words`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-words) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_type`, `sort` | List custom words. |
| [`create-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-acoustic-model) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `base_model_name`,` description` | Create a custom acoustic model. |
| [`delete-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-acoustic-model) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Delete a custom acoustic model. |
| [`get-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-acoustic-model) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Get a custom acoustic model. |
| [`list-acoustic-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-acoustic-models) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` | List custom acoustic models. |
| [`reset-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-acoustic-model) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Reset a custom acoustic model. |
| [`train-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-acoustic-model) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `custom_language_model_id` | Train a custom acoustic model. |
| [`upgrade-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-acoustic-model) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `custom_language_model_id` | Upgrade a custom acoustic model. |
| [`add-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-audio) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name`, `audio_resource`, `content_type`, `contained_content_type`, `allow_overwrite` | Add an audio resource. |
| [`delete-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-audio) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name` | Delete an audio resource. |
| [`get-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-audio) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name` | Get an audio resource. |
| [`list-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-audio) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | List audio resources. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-user-data) | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id` | Delete labeled data. |

### Creating a {{site.data.keyword.speechtotextshort}} service instance
{: #service_instance_speechtotext}

Before you install the package, you must create a {{site.data.keyword.speechtotextshort}} service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.speechtotextshort}} service instance ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/catalog/services/speech_to_text).
2. When the service instance is created, auto-generated service credentials are also created for you.

### Installing the {{site.data.keyword.speechtotextshort}} package
{: #install_speechtotext}

After you have an {{site.data.keyword.speechtotextshort}} service instance, use the {{site.data.keyword.openwhisk}} CLI to install the {{site.data.keyword.speechtotextshort}} package into your namespace.
{: shortdesc}

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #speechtotext_cli}

**Before you begin**

[Install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=cloud-functions-cli_install).

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

    **Output**

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

    Depending on the region where you created the service instance, the service instance might be named differently because it is an IAM service. If the command fails, use the following service name for the bind command:
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

### Installing from the {{site.data.keyword.openwhisk_short}} UI
{: #speechtotext_ui}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk/create).

2. Select the namespace that you want to install the package into by using the namespace switcher.

3. Click **Install Packages**.

4. Click the **Watson** Package group.

5. Click the **Speech To Text** Package.

5. Click **Install**.

6. Once the package is installed you are redirected to the actions page and can search for your new package, which is named `speech-to-text-v1`.

7. To use the actions in the **speech-to-text-v1** package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, follow steps 4 and 5 in the [CLI instructions](#speechtotext_cli).
  * To bind service credentials to individual actions, complete the following steps in the UI. 
  
  You must complete the following steps for each action that you want to use.
  {: note}

    1. Click an action from the **speech-to-text-v1** package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

### Using the {{site.data.keyword.speechtotextshort}} package
{: #usage_speechtotext}

To use the actions in this package, run commands in the following format:

```
ibmcloud fn action invoke speech-to-text-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Try out the `list-models` action.
```
ibmcloud fn action invoke speech-to-text-v1/list-models -b
```
{: pre}

## Watson: Speech to Text
{: #preinstall_speechtotext}

This pre-installed package is not available in the Tokyo region. See the installable [Speech to Text](#install_speechtotext).
{: tip}

The `/whisk.system/watson-speechToText` package offers a convenient way to call Watson APIs to convert the speech into text.
{: shortdesc}

The package includes the following actions.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/watson-speechToText` | Package | `username`, `password` | Package to convert speech into text. |
| `/whisk.system/watson-speechToText/speechToText` | Action | `payload`, `content_type`, `encoding`, `username`, `password`, `continuous`, `inactivity_timeout`, `interim_results`, `keywords`, `keywords_threshold`, `max_alternatives`, `model`, `timestamps`, `watson-token`, `word_alternatives_threshold`, `word_confidence`, `X-Watson-Learning-Opt-Out` | Convert audio into text. |

The package `/whisk.system/watson` is deprecated including the action `/whisk.system/watson/speechToText`. See the [{{site.data.keyword.speechtotextshort}} package](#setting-up-the-watson-speech-to-text-package-in-ibm-cloud) instead.
{: deprecated}

### Setting up the Watson Speech to Text package in {{site.data.keyword.cloud_notm}}

If you're using {{site.data.keyword.openwhisk}} from {{site.data.keyword.cloud_notm}}, the package bindings are automatically created for your {{site.data.keyword.cloud_notm}} Watson service instances.

1. Create a Watson Speech to Text service instance in your {{site.data.keyword.cloud_notm}} [dashboard](http://cloud.ibm.com). Be sure to remember the name of the service instance and the {{site.data.keyword.cloud_notm}} organization and space you're in.

2. Refresh the packages in your namespace. The refresh automatically creates a package binding for the Watson service instance that you created.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **Example output**

  ```
  created bindings:
  Bluemix_Watson_SpeechToText_Credentials-1
  ```
  {: screen}

3. List packages to see that the binding was created.

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Example output**

  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_SpeechToText_Credentials-1 private
  ```
  {: screen}

### Setting up a Watson Speech to Text package outside {{site.data.keyword.cloud_notm}}

If you're not using {{site.data.keyword.openwhisk_short}} in {{site.data.keyword.cloud_notm}}, or if you want to set up your Watson Speech to Text outside of {{site.data.keyword.cloud_notm}}, you must manually create a package binding for your Watson Speech to Text service. You need the Watson Speech to Text service user name, and password.

Create a package binding that is configured for your Watson Speech to Text service.
```
ibmcloud fn package bind /whisk.system/watson-speechToText myWatsonSpeechToText -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

### Converting speech to text

The `/whisk.system/watson-speechToText/speechToText` action converts audio speech into text. The following parameters are supported.

| Parameters | Description |
| --- | --- |
| `username` | The Watson API user name. |
| `password` | The Watson API password. |
| `payload` | The encoded speech binary data to turn into text. |
| `content_type` | The MIME type of the audio. |
| `encoding` | The encoding of the speech binary data. |
| `continuous` | Indicates whether multiple final results that represent consecutive phrases that are separated by long pauses are returned. |
| `inactivity_timeout` | The time in seconds after which, if only silence is detected in submitted audio, the connection is closed. |
| `interim_results` | Indicates whether the service is to return interim results. |
| `keywords` | A list of keywords to spot in the audio. |
| `keywords_threshold` | A confidence value that is the lower bound for spotting a keyword. |
| `max_alternatives` | The maximum number of alternative transcripts to be returned. |
| `model` | The identifier of the model to be used for the recognition request. |
| `timestamps` | Indicates whether time alignment is returned for each word. |
| `watson-token` | Provides an authentication token for the service as an alternative to providing service credentials. |
| `word_alternatives_threshold` | A confidence value that is the lower bound for identifying a hypothesis as a possible word alternative. |
| `word_confidence` | Indicates whether a confidence measure in the range of 0 to 1 is to be returned for each word. |
| `X-Watson-Learning-Opt-Out` | Indicates whether to opt out of data collection for the call. |

Invoke the `speechToText` action in your package binding to convert the encoded audio.

```
ibmcloud fn action invoke myWatsonSpeechToText/speechToText --blocking --result --param payload <base64 encoding of a .wav file> --param content_type 'audio/wav' --param encoding 'base64'
```
{: pre}

**Example output**
```
{
  "data": "Hello Watson"
}
```
{: screen}

