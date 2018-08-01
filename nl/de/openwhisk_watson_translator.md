---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Watson: Translator-Paket
{: #openwhisk_catalog_watson_translator}

Das Paket `/whisk.system/watson-translator` bietet eine komfortable Methode zum Aufrufen der Watson-APIs für Übersetzungen.
{: shortdesc}

Das Paket enthält die folgenden Aktionen.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/watson-translator` | Paket | username, password | Paket für Textübersetzung und Spracherkennung  |
| `/whisk.system/watson-translator/translator` | Aktion | payload, translateFrom, translateTo, translateParam, username, password | Übersetzung von Text |
| `/whisk.system/watson-translator/languageId` | Aktion | payload, username, password | Ermittlung einer Sprache |

**Hinweis:** Das Paket `/whisk.system/watson` ist einschließlich der Aktionen `/whisk.system/watson/translate` und `/whisk.system/watson/languageId` veraltet.

## Watson Translator-Paket in {{site.data.keyword.Bluemix_notm}} einrichten

Wenn Sie {{site.data.keyword.openwhisk}} über {{site.data.keyword.Bluemix_notm}} verwenden, werden die Paketbindungen automatisch für Ihre {{site.data.keyword.Bluemix_notm}} Watson-Serviceinstanzen erstellt.

1. Erstellen Sie eine Watson Translator-Serviceinstanz in Ihrem {{site.data.keyword.Bluemix_notm}}-[Dashboard](http://console.bluemix.net). Stellen Sie sicher, dass Sie sich den Namen der Serviceinstanz sowie der {{site.data.keyword.Bluemix_notm}}-Organisation und den Bereich, in dem Sie sich befinden, merken.

2. Aktualisieren Sie die Pakete in Ihrem Namensbereich. Die Aktualisierung erstellt automatisch eine Paketbindung für die Watson-Serviceinstanz, die Sie erstellt haben.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Beispielausgabe:
  ```
  created bindings:
  Bluemix_Watson_Translator_Credentials-1
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
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_Translator_Credentials-1 private
  ```
  {: screen}

## Watson Translator-Paket außerhalb von {{site.data.keyword.Bluemix_notm}} einrichten

Wenn Sie {{site.data.keyword.openwhisk_short}} nicht in {{site.data.keyword.Bluemix_notm}} verwenden oder wenn Sie Watson Translator außerhalb von {{site.data.keyword.Bluemix_notm}} einrichten möchten, müssen Sie manuell eine Paketbindung für Ihren Watson Translator-Service erstellen. Sie benötigen hierzu den Benutzernamen und das Kennwort des Watson Translator-Service.

- Erstellen Sie eine Paketbindung, die für Ihren Watson Translator-Service konfiguriert ist.
  ```
  ibmcloud fn package bind /whisk.system/watson-translator myWatsonTranslator -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}

## Text übersetzen

Die Aktion `/whisk.system/watson-translator/translator` übersetzt Text aus einer Sprache in eine andere. Die folgenden Parameter sind verfügbar:

- `username`: Der Benutzername für die Watson-API.
- `password`: Das Kennwort für die Watson-API.
- `payload`: Der zu übersetzende Text.
- `translateParam`: Der Eingabeparameter, der den zu übersetzenden Text angibt. Beispiel: Wenn `translateParam=payload` angegeben wird, wird der Wert des Parameters `payload`, der an die Aktion übergeben wird, übersetzt.
- `translateFrom`: Ein zweistelliger Code für die Ausgangssprache.
- `translateTo`: Ein zweistelliger Code für die Zielsprache.

Rufen Sie die Aktion **translator** in Ihrer Paketbindung auf, um einen Text aus dem Englischen ins Französische zu übersetzen.
```
ibmcloud fn action invoke myWatsonTranslator/translator \
--blocking --result \
--param payload "Blue skies ahead" --param translateFrom "en" \
--param translateTo "fr"
```
{: pre}

Beispielausgabe:
```
{
    "payload": "Ciel bleu a venir"
  }
```
{: screen}

## Sprache eines Texts ermitteln

Die Aktion `/whisk.system/watson-translator/languageId` ermittelt die Sprache eines Texts. Die folgenden Parameter sind verfügbar:

- `username`: Der Benutzername für die Watson-API.
- `password`: Das Kennwort für die Watson-API.
- `payload`: Der zu ermittelnde Text.

Rufen Sie die Aktion **languageId** in Ihrer Paketbindung auf, um die Sprache zu ermitteln.
```
ibmcloud fn action invoke myWatsonTranslator/languageId \
--blocking --result \
--param payload "Ciel bleu a venir"
```
{: pre}

Beispielausgabe:
```
{
  "payload": "Ciel bleu a venir",
    "language": "fr",
    "confidence": 0.710906
  }
```
{: screen}
