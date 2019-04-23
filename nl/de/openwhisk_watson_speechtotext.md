---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: watson, speech to text, package, converting, cognitive

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Watson: 'Speech to Text'-Paket
{: #openwhisk_catalog_watson_speechtotext}

Dieses vorinstallierte Paket ist nicht in der Region 'Tokio' verfügbar. Informationen finden Sie im installierbaren [Speech to Text](/docs/openwhisk?topic=cloud-functions-speech-to-text-package)-Paket mit IAM-Authentifizierung.
{: tip}

Das Paket `/whisk.system/watson-speechToText` bietet eine komfortable Methode zum Aufrufen der Watson-APIs für die Konvertierung von Sprache in Text.
{: shortdesc}

Das Paket enthält die folgenden Aktionen.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/watson-speechToText` | Paket | username, password | Paket zum Umwandeln von Sprache in Text |
| `/whisk.system/watson-speechToText/speechToText` | Aktion | payload, content_type, encoding, username, password, continuous, inactivity_timeout, interim_results, keywords, keywords_threshold, max_alternatives, model, timestamps, watson-token, word_alternatives_threshold, word_confidence, X-Watson-Learning-Opt-Out | Umsetzung von Sprache in Text |

**Hinweis:** Das Paket `/whisk.system/watson` ist einschließlich der Aktion `/whisk.system/watson/speechToText` veraltet. Weitere Informationen finden sie stattdessen im [{{site.data.keyword.speechtotextshort}}-Paket](/docs/openwhisk?topic=cloud-functions-speech-to-text-package). 

## Watson-Paket 'Speech to Text' in {{site.data.keyword.Bluemix_notm}} einrichten

Wenn Sie {{site.data.keyword.openwhisk}} über {{site.data.keyword.Bluemix_notm}} verwenden, werden die Paketbindungen automatisch für Ihre {{site.data.keyword.Bluemix_notm}} Watson-Serviceinstanzen erstellt.

1. Erstellen Sie eine Watson-Serviceinstanz für 'Speech to Text' in Ihrem {{site.data.keyword.Bluemix_notm}}-[Dashboard](http://cloud.ibm.com). 

  Stellen Sie sicher, dass Sie sich den Namen der Serviceinstanz sowie der {{site.data.keyword.Bluemix_notm}}-Organisation und den Bereich, in dem Sie sich befinden, merken.

2. Aktualisieren Sie die Pakete in Ihrem Namensbereich. Die Aktualisierung erstellt automatisch eine Paketbindung für die Watson-Serviceinstanz, die Sie erstellt haben.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Beispielausgabe:
  ```
  created bindings:
  Bluemix_Watson_SpeechToText_Credentials-1
  ```
  {: screen}

  Listen Sie die Pakete auf, um zu ermitteln, ob die Bindung erstellt wurde:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Beispielausgabe:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_SpeechToText_Credentials-1 private
  ```
  {: screen}

## Watson-Paket 'Speech to Text' außerhalb von {{site.data.keyword.Bluemix_notm}} einrichten

Wenn Sie {{site.data.keyword.openwhisk_short}} nicht in {{site.data.keyword.Bluemix_notm}} verwenden oder wenn Sie den Watson-Service 'Speech to Text' außerhalb von {{site.data.keyword.Bluemix_notm}} einrichten möchten, müssen Sie manuell eine Paketbindung für Ihren Watson-Service 'Speech to Text' erstellen. Sie benötigen hierzu den Benutzernamen und das Kennwort des Watson-Service 'Speech to Text'.

Erstellen Sie eine Paketbindung, die für Ihren Watson-Service 'Text to Speech' konfiguriert ist.
```
ibmcloud fn package bind /whisk.system/watson-speechToText myWatsonSpeechToText -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Umsetzung von Sprache in Text

Mit der Aktion `/whisk.system/watson-speechToText/speechToText` kann eine Audiorede in Text konvertiert werden. Die folgenden Parameter sind verfügbar:

- `username`: Der Benutzername für die Watson-API.
- `password`: Das Kennwort für die Watson-API.
- `payload`: Die codierten binären Sprachdaten, die in Text umgesetzt werden sollen.
- `content_type`: Der MIME-Typ der Sprachdaten.
- `encoding`: Die Codierung der binären Sprachdaten.
- `continuous`: Gibt an, ob mehrere Endergebnisse, die durch lange Pausen voneinander getrennte aufeinanderfolgende Phrasen darstellen, zurückgegeben werden.
- `inactivity_timeout`: Die Zeit in Sekunden, nach deren Ablauf die Verbindung geschlossen wird, wenn in den übertragenen Sprachdaten nur Stille festgestellt wird.
- `interim_results`: Gibt an, ob der Service Zwischenergebnisse zurückgeben soll.
- `keywords`: Eine Liste der Schlüsselwörter, die in den Sprachdaten erkannt werden sollen.
- `keywords_threshold`: Ein Übereinstimmungswert, der die Untergrenze für die Erkennung eines Schlüsselworts darstellt.
- `max_alternatives`: Die maximale Anzahl alternativer Aufzeichnungen, die zurückgegeben werden sollen.
- `model`: Die Kennung des Modells für die Erkennungsanforderung.
- `timestamps`: Gibt an, ob für jedes Wort Laufzeitkorrekturen (Time Alignment) zurückgegeben werden.
- `watson-token`: Gibt alternativ zu Serviceberechtigungsnachweisen ein Authentifizierungstoken für den Service an.
- `word_alternatives_threshold`: Ein Übereinstimmungswert, der die Untergrenze für die Angabe einer Hypothese als mögliche Wortalternative darstellt.
- `word_confidence`: Gibt an, ob für jedes Wort ein Übereinstimmungswert im Bereich von 0 bis 1 zurückgegeben wird.
- `X-Watson-Learning-Opt-Out`: Gibt an, ob die Datenerfassung für den Aufruf abgelehnt wird.

Rufen Sie die Aktion **speechToText** in Ihrer Paketbindung auf, um die codierten Sprachdaten umzusetzen.
```
ibmcloud fn action invoke myWatsonSpeechToText/speechToText --blocking --result --param payload <base64 encoding of a .wav file> --param content_type 'audio/wav' --param encoding 'base64'
```
{: pre}

Beispielausgabe:
```
{
  "data": "Hello Watson"
}
```
{: screen}
