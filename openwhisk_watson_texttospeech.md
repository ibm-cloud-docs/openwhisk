---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Using the Watson Text to Speech package
{: #openwhisk_catalog_watson_texttospeech}

The `/whisk.system/watson-textToSpeech` package offers a convenient way to call Watson APIs to convert the text into speech.
{: shortdesc}

The package includes the following Actions.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` | Package | username, password | Package to convert text into speech |
| `/whisk.system/watson-textToSpeech/textToSpeech` | Action | payload, voice, accept, encoding, username, password | Convert text into audio |

**Note**: The package `/whisk.system/watson` is deprecated including the Action `/whisk.system/watson/textToSpeech`.

## Setting up the Watson Text to Speech package in {{site.data.keyword.Bluemix_notm}}

If you're using OpenWhisk from {{site.data.keyword.Bluemix_notm}}, OpenWhisk automatically creates package bindings for your {{site.data.keyword.Bluemix_notm}} Watson service instances.

1. Create a Watson Text to Speech service instance in your {{site.data.keyword.Bluemix_notm}} [dashboard](http://console.ng.Bluemix.net).
  
  Be sure to remember the name of the service instance and the {{site.data.keyword.Bluemix_notm}} organization and space you're in.
  
2. Refresh the packages in your namespace. The refresh automatically creates a package binding for the Watson service instance that you created.
  ```
  wsk package refresh
  ```
  
  ```
  created bindings:
  Bluemix_Watson_TextToSpeech_Credentials-1
  ```
  
  ```
  wsk package list
  ```
  {: pre}
  
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_TextToSpeec_Credentials-1 private
  ```
  
  
## Setting up a Watson Text to Speech package outside {{site.data.keywrod.Bluemix_notm}}

If you're not using OpenWhisk in {{site.data.keyword.Bluemix_notm}} or if you want to set up your Watson Text to Speech outside of {{site.data.keyword.Bluemix_notm}}, you must manually create a package binding for your Watson Text to Speech service. You need the Watson Text to Speech service user name, and password.

- Create a package binding that is configured for your Watson Speech to Text service.
  
  ```
  wsk package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}
  

## Converting some text to speech

The `/whisk.system/watson-textToSpeech/textToSpeech` Action converts some text into an audio speech. The parameters are as follows:

- `username`: The Watson API user name.
- `password`: The Watson API password.
- `payload`: The text to convert into speech.
- `voice`: The voice of the speaker.
- `accept`: The format of the speech file.
- `encoding`: The encoding of the speech binary data.


- Invoke the `textToSpeech` Action in your package binding to convert the text.
  
  ```
  wsk action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
  ```
  {: pre}
  
  ```json
  {
    "payload": "<base64 encoding of a .wav file>"
  }
  ```
  
