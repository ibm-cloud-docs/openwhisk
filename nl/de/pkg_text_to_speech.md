---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

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


# Text to Speech
{: #pkg_text_to_speech}

## Paketoptionen

| Paket | Verfügbarkeit | Beschreibung |
| --- | --- | --- | --- |
| [`/whisk.system/watson-textToSpeech`](#text_to_speech) | Vorinstalliert (nicht in Tokio verfügbar) | Paket zum Umwandeln von Text in Sprache |
| [`text-to-speech-v1`](#text_to_speech_ins) | Installierbar | Ermöglicht das Arbeiten mit dem {{site.data.keyword.texttospeechshort}}-Service. |

## Watson Text to Speech
{: #text_to_speech}

Dieses vorinstallierte Paket ist nicht in der Region 'Tokio' verfügbar. Informationen finden Sie im installierbaren [Text to Speech](#text_to_speech_ins)-Paket mit IAM-Authentifizierung.
{: tip}

Das Paket `/whisk.system/watson-textToSpeech` bietet eine komfortable Methode zum Aufrufen der Watson-APIs für die Konvertierung von Text in Sprache.
{: shortdesc}

Das Paket enthält die folgenden Aktionen.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` | Paket | `username`, `password` | Paket zum Umwandeln von Text in Sprache. |
| `/whisk.system/watson-textToSpeech/textToSpeech` | Aktion | `payload`, `voice`, `accept`, `encoding`, `username`, `password` | Text in Audiodaten umwandeln. |

Das Paket `/whisk.system/watson` einschließlich der Aktion `/whisk.system/watson/textToSpeech` ist veraltet. Verwenden Sie stattdessen das [installierbare {{site.data.keyword.texttospeechshort}}-Paket](#text_to_speech_ins).
{: deprecated}

### {{site.data.keyword.texttospeechshort}}-Paket in {{site.data.keyword.cloud_notm}} einrichten

Wenn Sie {{site.data.keyword.openwhisk}} über {{site.data.keyword.cloud_notm}} verwenden, werden die Paketbindungen automatisch für Ihre {{site.data.keyword.cloud_notm}} Watson-Serviceinstanzen erstellt.

1. Erstellen Sie eine {{site.data.keyword.texttospeechshort}}-Serviceinstanz in Ihrem {{site.data.keyword.cloud_notm}}-[Dashboard](https://cloud.ibm.com){: external}. Stellen Sie sicher, dass Sie sich den Namen der Serviceinstanz sowie der {{site.data.keyword.cloud_notm}}-Organisation und den Bereich, in dem Sie sich befinden, merken.

2. Aktualisieren Sie die Pakete in Ihrem Namensbereich. Die Aktualisierung erstellt automatisch eine Paketbindung für die Watson-Serviceinstanz, die Sie erstellt haben.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **Beispielausgabe**
  ```
  created bindings:
  Watson_TextToSpeech_Credentials-1
  ```
  {: screen}

  Listen Sie die Pakete auf, um zu ermitteln, ob die Paketbindung erstellt wurde. 
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Beispielausgabe**
  ```
  packages
  /myOrg_mySpace/Watson_TextToSpeec_Credentials-1 private
  ```
  {: screen}

### {{site.data.keyword.texttospeechshort}}-Paket außerhalb von {{site.data.keyword.cloud_notm}} einrichten

Wenn Sie {{site.data.keyword.openwhisk_short}} in {{site.data.keyword.cloud_notm}} nicht verwenden oder wenn Sie {{site.data.keyword.texttospeechshort}} außerhalb von {{site.data.keyword.cloud_notm}} einrichten möchten, müssen Sie manuell eine Paketbindung für Ihren {{site.data.keyword.texttospeechshort}}-Service erstellen. Sie benötigen hierzu den Benutzernamen und das Kennwort des {{site.data.keyword.texttospeechshort}}-Service. 

Erstellen Sie eine Paketbindung, die für Ihren {{site.data.keyword.texttospeechshort}}-Service konfiguriert ist.
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username <username> -p password <password>
```
{: pre}

### Umsetzung von Text in Sprache

Die Aktion `/whisk.system/watson-textToSpeech/textToSpeech` wandelt Text in einen gesprochenen Audiotext um. Die folgenden Parameter werden unterstützt. 

| `username` | Der Benutzername für die Watson-API. |
| `password` | Das Kennwort für die Watson-API. |
| `payload` | Der Text, der in Sprache umgesetzt werden soll. |
| `voice` | Die Stimme des Sprechers. |
| `accept` | Das Format der Sprachdatei. |
| `encoding` | Die Codierung der binären Sprachdaten. |

Testen Sie die Aktion `textToSpeech` in Ihrer Paketbindung, um den Text umzusetzen. 
```
ibmcloud fn action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
```
{: pre}

**Beispielausgabe**
```
{
  "payload": "<base64 encoding of a .wav file>"
}
```
{: screen}


## {{site.data.keyword.texttospeechshort}}
{: #text_to_speech_ins}

Der installierbare {{site.data.keyword.texttospeechfull}}-Service stellt eine API mit der IBM Sprachsynthesefunktionalität zur Verfügung, um Text in natürlich klingende Sprache in verschiedenen Sprachen, Dialekten und Stimmen zu synthetisieren.
{:shortdesc}

Für jede Sprache unterstützt der Service mindestens eine männliche oder eine weibliche Stimme, manchmal auch beides. Das Audiomaterial wird mit minimaler Verzögerung per Streaming an den Client zurückgegeben. Weitere Informationen zu diesem Service finden Sie in der [IBM Cloud-Dokumentation](/docs/services/text-to-speech?topic=text-to-speech-about).

Das {{site.data.keyword.texttospeechshort}}-Paket enthält die folgenden Entitäten. Weitere Informationen können Sie in der Referenz für die {{site.data.keyword.texttospeechshort}}-API abrufen, indem Sie auf den Namen der betreffenden Entität klicken. 

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| [`text-to-speech-v1`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html){: external} | Paket | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Ermöglicht das Arbeiten mit dem {{site.data.keyword.texttospeechshort}}-Service. |
| [`get-voice`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `voice`, `customization_id` | Ruft eine Stimme ab. |
| [`list-voices`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voices){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Listet Stimmen auf. |
| [`synthesize`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#synthesize){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `accept`, `voice`, `customization_id` | Erstellt Audiomaterial synthetisch. |
| [`get-pronunciation`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-pronunciation){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `voice`, `format`, `customization_id` | Ruft die Aussprache ab. |
| [`create-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#create-voice-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `language`, `description` | Erstellt ein angepasstes Modell. |
| [`delete-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-voice-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Löscht ein angepasstes Modell. |
| [`get-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Ruft ein angepasstes Modell ab. |
| [`list-voice-models`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voice-models){: external} | Aktion | `username`,`password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` | Listet angepasste Modelle auf. |
| [`update-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#update-voice-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `name`, `description`, `words` | Aktualisiert ein angepasstes Modell. |
| [`add-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-word){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word`, `translation`, `part_of_speech` | Fügt ein angepasstes Wort hinzu. |
| [`add-words`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-words){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `words`  | Fügt angepasste Wörter hinzu. |
| [`delete-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-word){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word` | Löscht ein angepasstes Wort. |
| [`get-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-word){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word` | Ruft ein angepasstes Wort ab. |
| [`list-words`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-words){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Listet angepasste Wörter auf. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-user-data){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id` | Löscht gekennzeichnete Daten. |

### {{site.data.keyword.texttospeechshort}}-Serviceinstanz erstellen
{: #service_instance_texttospeech}

Vor dem Installieren des Pakets müssen Sie eine Instanz des {{site.data.keyword.texttospeechshort}}-Service und Serviceberechtigungsnachweise erstellen.
{: shortdesc}

1. [Erstellen Sie eine {{site.data.keyword.texttospeechshort}}-Serviceinstanz](https://cloud.ibm.com/catalog/services/text_to_speech){: external}. 
2. Während der Erstellung der Serviceinstanz werden ebenfalls automatisch generierte Serviceberechtigungsnachweise erstellt.

### {{site.data.keyword.texttospeechshort}}-Paket installieren
{: #install_texttospeech}

Nachdem Sie über eine Instanz des {{site.data.keyword.texttospeechshort}}-Service verfügen, verwenden Sie die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.openwhisk}}, um das {{site.data.keyword.texttospeechshort}}-Paket in Ihrem Namensbereich zu installieren.
{: shortdesc}

### Installation über die {{site.data.keyword.openwhisk_short}}-CLI durchführen
{: #texttospeech_cli}

**Vorbereitende Schritte**
[Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.cloud_notm}}-CLI](/docs/openwhisk?topic=cloud-functions-cli_install). 

Führen Sie die folgenden Befehle aus, um das {{site.data.keyword.texttospeechshort}}-Paket zu installieren. 

1. Klonen Sie das Repository für das {{site.data.keyword.texttospeechshort}}-Paket.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Stellen Sie das Paket bereit.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/text-to-speech-v1/manifest.yaml
    ```
    {: pre}

3. Stellen Sie sicher, dass das Paket zu Ihrer Paketliste hinzugefügt wird.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Ausgabe**
    ```
    packages
    /myOrg_mySpace/text-to-speech-v1                        private
    ```
    {: screen}

4. Binden Sie die Berechtigungsnachweise aus der erstellten {{site.data.keyword.texttospeechshort}}-Instanz an das Paket.
    ```
    ibmcloud fn service bind text_to_speech text-to-speech-v1
    ```
    {: pre}

    Abhängig von der Region, in der Sie die Serviceinstanz erstellt haben, wird die Serviceinstanz möglicherweise anders benannt, weil es sich um einen IAM-Service handelt. Sollte der obige Befehl fehlschlagen, verwenden Sie den folgenden Servicenamen für den Befehl 'bind':
    ```
    ibmcloud fn service bind text-to-speech text-to-speech-v1
    ```
    {: pre}

    **Beispielausgabe**
    ```
    Credentials 'Credentials-1' from 'text_to_speech' service instance 'Watson Text to Speech' bound to 'text-to-speech-v1'.
    ```
    {: screen}

5. Stellen Sie sicher, dass das Paket mit Ihren Berechtigungsnachweisen für die {{site.data.keyword.texttospeechshort}}-Serviceinstanz konfiguriert ist.
    ```
    ibmcloud fn package get text-to-speech-v1 parameters
    ```
    {: pre}

    **Beispielausgabe**
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

### Installation über die {{site.data.keyword.openwhisk_short}}-UI durchführen
{: #texttospeech_ui}

1. In der {{site.data.keyword.openwhisk_short}}-Konsole rufen Sie die [Seite 'Erstellen'](https://cloud.ibm.com/openwhisk/create){: external} auf. 

2. Wählen Sie mithilfe des Dropdown-Menüs für den Namensbereich den Namensbereich aus, in dem das Paket installiert werden soll. 

3. Klicken Sie auf **Pakete installieren**.

4. Klicken Sie auf die Paketgruppe **Watson**.

5. Klicken Sie auf das Paket **Text To Speech**.

5. Klicken Sie auf **Installieren**.

6. Nachdem das Paket installiert worden ist, werden Sie zur Seite 'Aktionen' weitergeleitet, auf der Sie nach Ihrem neuen Paket suchen können. Dieses trägt die Bezeichnung `text-to-speech-v1`. 

7. Um die Aktionen im Paket `text-to-speech-v1` verwenden zu können, müssen Sie Serviceberechtigungsnachweise an die Aktionen binden. 
  * Wenn Sie Serviceberechtigungsnachweise an alle Aktionen im Paket binden wollen, führen Sie die Schritte [4 und 6 in den CLI-Anweisungen](#texttospeech_cli) aus. 
  * Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden wollen, führen Sie die nachfolgend aufgeführten Schritte in der UI (Benutzerschnittstelle) aus.

  Sie müssen die folgenden Schritte für jede Aktion ausführen, die Sie verwenden möchten.
  {: note}

    1. Klicken Sie auf eine Aktion aus dem Paket `text-to-speech-v1`, die Sie verwenden möchten. Die Detailseite für diese Aktion wird geöffnet.
    2. Klicken Sie im Navigationsbereich links auf den Abschnitt **Parameter**.
    3. Geben Sie einen neuen Parameter ein. Geben Sie als Schlüssel die Zeichenfolge `__bx_creds` ein. Fügen Sie als Wert das JSON-Serviceberechtigungsnachweisobjekt aus der Serviceinstanz ein, die Sie zuvor erstellt haben.

## {{site.data.keyword.texttospeechshort}}-Paket verwenden
{: #usage_texttospeech}

Um die Aktionen in diesem Paket verwenden zu können, führen Sie Befehle in folgendem Format aus:

```
ibmcloud fn action invoke text-to-speech-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Testen Sie versuchshalber die Aktion `list-voices`.
```
ibmcloud fn action invoke text-to-speech-v1/list-voices -b
```
{: pre}


