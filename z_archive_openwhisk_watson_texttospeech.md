
---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: text to speech, watson, cognitive, converting, package

subcollection: cloud-functions

---





{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
# Watson: Text to Speech package
{: #openwhisk_catalog_watson_texttospeech}

This pre-installed package is not available in the Tokyo region. Please see the installable [Text to Speech](/docs/openwhisk?topic=cloud-functions-text-to-speech-package) package using IAM authentication.
{: tip}

The `/whisk.system/watson-textToSpeech` package offers a convenient way to call Watson APIs to convert the text into speech.
{: shortdesc}

The package includes the following actions.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` | package | username, password | Package to convert text into speech |
| `/whisk.system/watson-textToSpeech/textToSpeech` | action | payload, voice, accept, encoding, username, password | Convert text into audio |

**Note**: The package `/whisk.system/watson` is deprecated including the action `/whisk.system/watson/textToSpeech`. See the [installable {{site.data.keyword.texttospeechshort}} package](/docs/openwhisk?topic=cloud-functions-text-to-speech-package) instead.

## Setting up the Watson Text to Speech package in {{site.data.keyword.Bluemix_notm}}

If you're using {{site.data.keyword.openwhisk}} from {{site.data.keyword.Bluemix_notm}}, the package bindings are automatically created for your {{site.data.keyword.Bluemix_notm}} Watson service instances.

1. Create a Watson Text to Speech service instance in your {{site.data.keyword.Bluemix_notm}} [dashboard](http://cloud.ibm.com).

  Be sure to remember the name of the service instance and the {{site.data.keyword.Bluemix_notm}} organization and space you're in.

2. Refresh the packages in your namespace. The refresh automatically creates a package binding for the Watson service instance that you created.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Example output:
  ```
  created bindings:
  Bluemix_Watson_TextToSpeech_Credentials-1
  ```
  {: screen}

  List packages to see that the package binding was created:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Example output:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_TextToSpeec_Credentials-1 private
  ```
  {: screen}

## Setting up a Watson Text to Speech package outside {{site.data.keyword.Bluemix_notm}}

If you're not using {{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}} or if you want to set up your Watson Text to Speech outside of {{site.data.keyword.Bluemix_notm}}, you must manually create a package binding for your Watson Text to Speech service. You need the Watson Text to Speech service user name, and password.

Create a package binding that is configured for your Watson Speech to Text service.
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Converting some text to speech

The `/whisk.system/watson-textToSpeech/textToSpeech` action converts some text into an audio speech. The parameters are as follows:

- `username`: The Watson API user name.
- `password`: The Watson API password.
- `payload`: The text to convert into speech.
- `voice`: The voice of the speaker.
- `accept`: The format of the speech file.
- `encoding`: The encoding of the speech binary data.

Invoke the **textToSpeech** action in your package binding to convert the text.
```
ibmcloud fn action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
```
{: pre}

Example output:
```
{
  "payload": "<base64 encoding of a .wav file>"
}
```
{: screen}

