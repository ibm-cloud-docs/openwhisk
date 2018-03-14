---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Watson-Paket 'Text to Speech' verwenden
{: #openwhisk_catalog_watson_texttospeech}

Das Paket `/whisk.system/watson-textToSpeech` bietet eine komfortable Methode zum Aufrufen der Watson-APIs für die Konvertierung von Text in Sprache.
{: shortdesc}

Das Paket enthält die folgenden Aktionen.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` | Paket | username, password | Paket zum Umwandeln von Text in Sprache |
| `/whisk.system/watson-textToSpeech/textToSpeech` | Aktion | payload, voice, accept, encoding, username, password | Umsetzung von Text in Sprache |

**Hinweis:** Das Paket `/whisk.system/watson` ist einschließlich der Aktion `/whisk.system/watson/textToSpeech` veraltet.

## Watson-Paket 'Text to Speech' in {{site.data.keyword.Bluemix_notm}} einrichten

Wenn Sie OpenWhisk über {{site.data.keyword.Bluemix_notm}} verwenden, erstellt OpenWhisk automatisch Paketbindungen für Ihre {{site.data.keyword.Bluemix_notm}}-Watson-Serviceinstanzen.

1. Erstellen Sie eine Watson-Serviceinstanz für 'Text to Speech' in Ihrem {{site.data.keyword.Bluemix_notm}}-[Dashboard](http://console.ng.Bluemix.net).
  
  Stellen Sie sicher, dass Sie sich den Namen der Serviceinstanz sowie der {{site.data.keyword.Bluemix_notm}}-Organisation und den Bereich, in dem Sie sich befinden, merken.
  
2. Aktualisieren Sie die Pakete in Ihrem Namensbereich. Die Aktualisierung erstellt automatisch eine Paketbindung für die Watson-Serviceinstanz, die Sie erstellt haben.
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
  
  
## Watson-Paket 'Text to Speech' außerhalb von {{site.data.keywrod.Bluemix_notm}} einrichten

Wenn Sie OpenWhisk nicht in {{site.data.keyword.Bluemix_notm}} verwenden oder wenn Sie den Watson-Service 'Text to Speech' außerhalb von {{site.data.keyword.Bluemix_notm}} einrichten möchten, müssen Sie manuell eine Paketbindung für Ihren Watson-Service 'Text to Speech' erstellen. Sie benötigen hierzu den Benutzernamen und das Kennwort des Watson-Service 'Text to Speech'.

- Erstellen Sie eine Paketbindung, die für Ihren Watson-Service 'Text to Speech' konfiguriert ist.
  
  ```
  wsk package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username MYUSERNAME -p password MYPASSWORD
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


- Rufen Sie die Aktion `textToSpeech` in Ihrer Paketbindung auf, um den Text umzusetzen.
  
  ```
  wsk action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
  ```
  {: pre}
  
  ```json
  {
    "payload": "<base64 encoding of a .wav file>"
  }
  ```
  
