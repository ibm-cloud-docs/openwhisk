, select the namespace that you want to install the package into. Namespaces are formed from the combined org and space names.

3. Click **Install Packages**.

4. Click the **Watson** Package group.

5. Click the **Speech To Text** Package.

5. Click **Install**.

6. Once the package has been installed you will be redirected to the actions page and can search for your new package, which is named **speech-to-text-v1**.

7. To use the actions in the **speech-to-text-v1** Package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, follow steps 5 and 6 in the CLI instructions listed above.
  * To bind service credentials to individual actions, complete the following steps in the UI. **Note**: You must complete the following steps for each action that you want to use.
    1. Click an action from the **speech-to-text-v1** package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.speechtotextshort}} package
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

This pre-installed package is not available in the Tokyo region. Please see the installable [Speech to Text](#install_speechtotext) package using IAM authentication.
{: tip}

The `/whisk.system/watson-speechToText` package offers a convenient way to call Watson APIs to convert the speech into text.
{: shortdesc}

The package includes the following actions.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/watson-speechToText` | package | username, password | Package to convert speech into text |
| `/whisk.system/watson-speechToText/speechToText` | action | payload, content_type, encoding, username, password, continuous, inactivity_timeout, interim_results, keywords, keywords_threshold, max_alternatives, model, timestamps, watson-token, word_alternatives_threshold, word_confidence, X-Watson-Learning-Opt-Out | Convert audio into text |

**Note**: The package `/whisk.system/watson` is deprecated including the action `/whisk.system/watson/speechToText`. See the [{{site.data.keyword.speechtotextshort}} package](#setting-up-the-watson-speech-to-text-package-in-ibm-cloud) instead.

## Setting up the Watson Speech to Text package in {{site.data.keyword.Bluemix_notm}}

If you're using {{site.data.keyword.openwhisk}} from {{site.data.keyword.Bluemix_notm}}, the package bindings are automatically created for your {{site.data.keyword.Bluemix_notm}} Watson service instances.

1. Create a Watson Speech to Text service instance in your {{site.data.keyword.Bluemix_notm}} [dashboard](http://cloud.ibm.com).

  Be sure to remember the name of the service instance and the {{site.data.keyword.Bluemix_notm}} organization and space you're in.

2. Refresh the packages in your namespace. The refresh automatically creates a package binding for the Watson service instance that you created.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Example output:
  ```
  created bindings:
  Bluemix_Watson_SpeechToText_Credentials-1
  ```
  {: screen}

  List packages to see that the binding was created:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Example output:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_SpeechToText_Credentials-1 private
  ```
  {: screen}

## Setting up a Watson Speech to Text package outside {{site.data.keyword.Bluemix_notm}}

If you're not using {{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}, or if you want to set up your Watson Speech to Text outside of {{site.data.keyword.Bluemix_notm}}, you must manually create a package binding for your Watson Speech to Text service. You need the Watson Speech to Text service user name, and password.

Create a package binding that is configured for your Watson Speech to Text service.
```
ibmcloud fn package bind /whisk.system/watson-speechToText myWatsonSpeechToText -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Converting speech to text

The `/whisk.system/watson-speechToText/speechToText` action converts audio speech into text. The parameters are as follows:

- `username`: The Watson API user name.
- `password`: The Watson API password.
- `payload`: The encoded speech binary data to turn into text.
- `content_type`: The MIME type of the audio.
- `encoding`: The encoding of the speech binary data.
- `continuous`: Indicates whether multiple final results that represent consecutive phrases that are separated by long pauses are returned.
- `inactivity_timeout`: The time in seconds after which, if only silence is detected in submitted audio, the connection is closed.
- `interim_results`: Indicates whether the service is to return interim results.
- `keywords`: A list of keywords to spot in the audio.
- `keywords_threshold`: A confidence value that is the lower bound for spotting a keyword.
- `max_alternatives`: The maximum number of alternative transcripts to be returned.
- `model`: The identifier of the model to be used for the recognition request.
- `timestamps`: Indicates whether time alignment is returned for each word.
- `watson-token`: Provides an authentication token for the service as an alternative to providing service credentials.
- `word_alternatives_threshold`: A confidence value that is the lower bound for identifying a hypothesis as a possible word alternative.
- `word_confidence`: Indicates whether a confidence measure in the range of 0 to 1 is to be returned for each word.
- `X-Watson-Learning-Opt-Out`: Indicates whether to opt out of data collection for the call.

Invoke the **speechToText** action in your package binding to convert the encoded audio.
```
ibmcloud fn action invoke myWatsonSpeechToText/speechToText --blocking --result --param payload <base64 encoding of a .wav file> --param content_type 'audio/wav' --param encoding 'base64'
```
{: pre}

Example output:
```
{
  "data": "Hello Watson"
}
```
{: screen}
</staging>
