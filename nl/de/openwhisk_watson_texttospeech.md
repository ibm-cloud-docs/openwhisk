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
# Watson: 'Text to Speech'-Paket
{: #openwhisk_catalog_watson_texttospeech}

Dieses vorinstallierte Paket ist nicht in der Region 'Tokio' verfügbar. Informationen finden Sie im installierbaren [Text to Speech](/docs/openwhisk?topic=cloud-functions-text-to-speech-package)-Paket mit IAM-Authentifizierung.
{: tip}

Das Paket `/whisk.system/watson-textToSpeech` bietet eine komfortable Methode zum Aufrufen der Watson-APIs für die Konvertierung von Text in Sprache.
{: shortdesc}

Das Paket enthält die folgenden Aktionen.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` | Paket | username, password | Paket zum Umwandeln von Text in Sprache |
| `/whisk.system/watson-textToSpeech/textToSpeech` | Aktion | payload, voice, accept, encoding, username, password | Umsetzung von Text in Sprache |

**Hinweis:** Das Paket `/whisk.system/watson` ist einschließlich der Aktion `/whisk.system/watson/textToSpeech` veraltet. Weitere Informationen finden Sie stattdessen im [installierbaren {{site.data.keyword.texttospeechshort}}-Paket](/docs/openwhisk?topic=cloud-functions-text-to-speech-package). 

## Watson-Paket 'Text to Speech' in {{site.data.keyword.Bluemix_notm}} einrichten

Wenn Sie {{site.data.keyword.openwhisk}} über {{site.data.keyword.Bluemix_notm}} verwenden, werden die Paketbindungen automatisch für Ihre {{site.data.keyword.Bluemix_notm}} Watson-Serviceinstanzen erstellt.

1. Erstellen Sie eine Watson-Serviceinstanz für 'Text to Speech' in Ihrem {{site.data.keyword.Bluemix_notm}}-[Dashboard](http://cloud.ibm.com). 

  Stellen Sie sicher, dass Sie sich den Namen der Serviceinstanz sowie der {{site.data.keyword.Bluemix_notm}}-Organisation und den Bereich, in dem Sie sich befinden, merken.

2. Aktualisieren Sie die Pakete in Ihrem Namensbereich. Die Aktualisierung erstellt automatisch eine Paketbindung für die Watson-Serviceinstanz, die Sie erstellt haben.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Beispielausgabe:
  ```
  created bindings:
  Bluemix_Watson_TextToSpeech_Credentials-1
  ```
  {: screen}

  Listen Sie die Pakete auf, um zu ermitteln, ob die Paketbindung erstellt wurde:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Beispielausgabe:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_TextToSpeec_Credentials-1 private
  ```
  {: screen}

## Watson-Paket 'Text to Speech' außerhalb von {{site.data.keyword.Bluemix_notm}} einrichten

Wenn Sie {{site.data.keyword.openwhisk_short}} nicht in {{site.data.keyword.Bluemix_notm}} verwenden oder wenn Sie den Watson-Service 'Text to Speech' außerhalb von {{site.data.keyword.Bluemix_notm}} einrichten möchten, müssen Sie manuell eine Paketbindung für Ihren Watson-Service 'Text to Speech' erstellen. Sie benötigen hierzu den Benutzernamen und das Kennwort des Watson-Service 'Text to Speech'.

Erstellen Sie eine Paketbindung, die für Ihren Watson-Service 'Text to Speech' konfiguriert ist.
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Umsetzung von Text in Sprache

Die Aktion `/whisk.system/watson-textToSpeech/textToSpeech` wandelt Text in einen gesprochenen Audiotext um. Die folgenden Parameter sind verfügbar:

- `username`: Der Benutzername für die Watson-API.
- `password`: Das Kennwort für die Watson-API.
- `payload`: Der Text, der in Sprache umgesetzt werden soll.
- `voice`: Die Stimme des Sprechers.
- `accept`: Das Format der Sprachdatei.
- `encoding`: Die Codierung der binären Sprachdaten.

Rufen Sie die Aktion **textToSpeech** in Ihrer Paketbindung auf, um den Text umzusetzen.
```
ibmcloud fn action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
```
{: pre}

Beispielausgabe:
```
{
  "payload": "<base64 encoding of a .wav file>"
}
```
{: screen}
