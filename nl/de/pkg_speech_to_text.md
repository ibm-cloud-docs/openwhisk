---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: package, cognitive, functions

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


# {{site.data.keyword.speechtotextshort}}
{: #pkg_speech_to_text}

## Pakete

| Name | Verfügbarkeit | Beschreibung |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](#speech_to_text) | Installierbar | Arbeitet mit dem {{site.data.keyword.speechtotextshort}} V1-Service. |
| [`/whisk.system/watson-speechToText`](#preinstall_speechtotext) | Vorinstalliert (nicht in Tokio verfügbar) | Watson-APIs aufrufen, um Sprache in Text umzuwandeln. |

## {{site.data.keyword.speechtotextshort}}-Service
{: #speech_to_text}

Der installierbare {{site.data.keyword.speechtotextfull}}-Service stellt eine [API](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html){: external} zur Verfügung, die die Spracherkennung von IBM nutzt, um Transkripte von gesprochenem Audiomaterial zu erzeugen.
{:shortdesc}

Der Service ist in der Lage, Gesprochenes aus verschiedenen Sprachen und Audioformaten zu transkribieren. Neben der grundlegenden Transkription kann der Service detaillierte Informationen zu zahlreichen Aspekten des Audiomaterials liefern. Für die meisten Sprachen unterstützt der Service zwei Abtastraten, Breitband und Schmalband. Er gibt den gesamten JSON-Antwortinhalt im UTF-8-Zeichensatz zurück. Weitere Informationen zu diesem Service finden Sie in der [IBM&reg; Cloud-Dokumentation](/docs/services/speech-to-text?topic=speech-to-text-about).

Das {{site.data.keyword.speechtotextshort}}-Paket enthält die folgenden Entitäten. Weitere Informationen können Sie in der Referenz für die {{site.data.keyword.speechtotextshort}}-API abrufen, indem Sie auf den Namen der betreffenden Entität klicken. 

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html){: external} | Paket | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Ermöglicht das Arbeiten mit dem {{site.data.keyword.speechtotextshort}} V1-Service. |
| [`get-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id` | Ruft ein Modell ab. |
| [`list-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-models){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Listet Modelle auf. |
| [`recognize-sessionless`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#recognize-sessionless){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `audio`, `content_type`, `model`, `customization_id`, `acoustic_customization_id`, `base_model_version`, `customization_weight`, `inactivity_timeout`, `keywords`, `keywords_threshold`, `max_alternatives`, `word_alternatives_threshold`, `word_confidence`, `timestamps`, `profanity_filter`, `smart_formatting`, `speaker_labels` | Erkennt Audio (ohne Sitzung). |
| [`check-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-job){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `id`  | Prüft einen Job. |
| [`check-jobs`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-jobs){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Prüft Jobs. |
| [`create-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-job){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `audio`, `content_type`, `model`, `callback_url`, `events`, `user_token`, `results_ttl`, `customization_id`, `acoustic_customization_id`, `base_model_version`, `customization_weight`, `inactivity_timeout`, `keywords`, `keywords_threshold`, `max_alternatives`, `word_alternatives_threshold`, `word_confidence`, `timestamps`, `profanity_filter`, `smart_formatting`, `speaker_labels` | Erstellt einen Job. |
| [`delete-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-job){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `id` | Löscht einen Job. |
| [`register-callback`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#register-callback){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `callback_url`, `user_secret` | Registriert einen Callback. |
| [`unregister-callback`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#unregister-callback){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `callback_url` | Hebt die Registrierung eines Callbacks auf. |
| [`create-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-language-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `base_model_name`, `dialect`, `description` | Erstellt ein angepasstes Sprachmodell. |
| [`delete-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-language-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Löscht ein angepasstes Sprachmodell. |
| [`get-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-language-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Ruft ein angepasstes Sprachmodell ab. |
| [`list-language-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-language-models){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` | Listet angepasste Sprachmodelle auf. |
| [`reset-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-language-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Setzt ein angepasstes Sprachmodell zurück. |
| [`train-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-language-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_type_to_add`, `customization_weight` | Trainiert ein angepasstes Sprachmodell. |
| [`upgrade-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-language-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Führt ein Upgrade für ein angepasstes Sprachmodell durch. |
| [`add-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-corpus){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name`, `corpus_file`, `allow_overwrite` | Fügt ein Korpus hinzu. |
| [`delete-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-corpus){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name` | Löscht ein Korpus. |
| [`get-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-corpus){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name` | Ruft ein Korpus ab. |
| [`list-corpora`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-corpora){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Listet Korpora auf. |
| [`add-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-word){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name`, `word`, `sounds_like`, `display_as` | Fügt ein angepasstes Wort hinzu. |
| [`add-words`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-words){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `words` | Fügt angepasste Wörter hinzu. |
| [`delete-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-word){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name` | Löscht ein angepasstes Wort. |
| [`get-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-word){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name` | Ruft ein angepasstes Wort ab. |
| [`list-words`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-words){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_type`, `sort` | Listet angepasste Wörter auf. |
| [`create-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-acoustic-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `base_model_name`,` description` | Erstellt ein angepasstes akustisches Modell. |
| [`delete-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-acoustic-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Löscht ein angepasstes akustisches Modell. |
| [`get-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-acoustic-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Ruft ein angepasstes akustisches Modell ab. |
| [`list-acoustic-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-acoustic-models){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` | Listet angepasste akustische Modelle auf. |
| [`reset-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-acoustic-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Setzt ein angepasstes akustisches Modell zurück. |
| [`train-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-acoustic-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `custom_language_model_id` | Trainiert ein angepasstes akustisches Modell. |
| [`upgrade-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-acoustic-model){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `custom_language_model_id` | Führt ein Upgrade für ein angepasstes akustisches Modell durch. |
| [`add-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-audio){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name`, `audio_resource`, `content_type`, `contained_content_type`, `allow_overwrite` | Fügt eine Audioressource hinzu. |
| [`delete-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-audio){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name` | Löscht eine Audioressource. |
| [`get-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-audio){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name` | Ruft eine Audioressource ab. |
| [`list-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-audio){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Listet Audioressourcen auf. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-user-data){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id` | Löscht gekennzeichnete Daten. |

### {{site.data.keyword.speechtotextshort}}-Serviceinstanz erstellen
{: #service_instance_speechtotext}

Vor dem Installieren des Pakets müssen Sie eine Instanz des {{site.data.keyword.speechtotextshort}}-Service und Serviceberechtigungsnachweise erstellen.
{: shortdesc}

1. [Erstellen Sie eine {{site.data.keyword.speechtotextshort}}-Serviceinstanz](https://cloud.ibm.com/catalog/services/speech_to_text){: external}. 
2. Während der Erstellung der Serviceinstanz werden ebenfalls automatisch generierte Serviceberechtigungsnachweise erstellt.

### {{site.data.keyword.speechtotextshort}}-Paket installieren
{: #install_speechtotext}

Nachdem Sie über eine Instanz des {{site.data.keyword.speechtotextshort}}-Service verfügen, verwenden Sie die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.openwhisk}}, um das {{site.data.keyword.speechtotextshort}}-Paket in Ihrem Namensbereich zu installieren.
{: shortdesc}

### Installation über die {{site.data.keyword.openwhisk_short}}-CLI durchführen
{: #speechtotext_cli}

**Vorbereitende Schritte**

[Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.cloud_notm}}-CLI](/docs/openwhisk?topic=cloud-functions-cli_install). 

Gehen Sie zum Installieren des {{site.data.keyword.speechtotextshort}}-Pakets wie folgt vor:

1. Klonen Sie das Repository für das {{site.data.keyword.speechtotextshort}}-Paket.

    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Stellen Sie das Paket bereit.

    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/speech-to-text-v1/manifest.yaml
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
    /myOrg_mySpace/speech-to-text-v1                        private
    ```
    {: screen}

4. Binden Sie die Berechtigungsnachweise aus der erstellten {{site.data.keyword.speechtotextshort}}-Instanz an das Paket.

    ```
    ibmcloud fn service bind speech_to_text speech-to-text-v1
    ```
    {: pre}

    Abhängig von der Region, in der Sie die Serviceinstanz erstellt haben, wird die Serviceinstanz möglicherweise anders benannt, weil es sich um einen IAM-Service handelt. Wenn der Befehl fehlschlägt, verwenden Sie den folgenden Servicenamen für den Befehl 'bind':
    ```
    ibmcloud fn service bind speech-to-text speech-to-text-v1
    ```
    {: pre}

    **Beispielausgabe**

    ```
    Credentials 'Credentials-1' from 'speech_to_text' service instance 'Watson Speech to Text' bound to 'speech-to-text-v1'.
    ```
    {: screen}

5. Stellen Sie sicher, dass das Paket mit Ihren Berechtigungsnachweisen für die {{site.data.keyword.speechtotextshort}}-Serviceinstanz konfiguriert ist.

    ```
    ibmcloud fn package get speech-to-text-v1 parameters
    ```
    {: pre}

    **Beispielausgabe**

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

### Installation über die {{site.data.keyword.openwhisk_short}}-UI durchführen
{: #speechtotext_ui}

1. In der {{site.data.keyword.openwhisk_short}}-Konsole rufen Sie die [Seite 'Erstellen'](https://cloud.ibm.com/openwhisk/create){: external} auf. 

2. Wählen Sie mithilfe des Dropdown-Menüs für den Namensbereich den Namensbereich aus, in dem das Paket installiert werden soll. 

3. Klicken Sie auf **Pakete installieren**.

4. Klicken Sie auf die Paketgruppe **Watson**.

5. Klicken Sie auf das Paket **Speech To Text**.

5. Klicken Sie auf **Installieren**.

6. Nachdem das Paket installiert wurde, werden Sie zur Seite 'Aktionen' weitergeleitet und Sie können nach Ihrem neuen Paket suchen. Sein Name lautet `speech-to-text-v1`. 

7. Um die Aktionen im Paket **speech-to-text-v1** verwenden zu können, müssen Sie Serviceberechtigungsnachweise an die Aktionen binden. 
  * Wenn Sie Serviceberechtigungsnachweise an alle Aktionen im Paket binden wollen, führen Sie die Schritte 4 und 5 in den [CLI-Anweisungen](#speechtotext_cli) aus. 
  * Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden wollen, führen Sie die nachfolgend aufgeführten Schritte in der UI (Benutzerschnittstelle) aus. 
  
  Sie müssen die folgenden Schritte für jede Aktion ausführen, die Sie verwenden möchten.
  {: note}

    1. Klicken Sie auf eine Aktion aus dem Paket **speech-to-text-v1**, die Sie verwenden wollen. Die Detailseite für diese Aktion wird geöffnet.
    2. Klicken Sie im Navigationsbereich links auf den Abschnitt **Parameter**.
    3. Geben Sie einen neuen **Parameter** ein. Geben Sie als Schlüssel die Zeichenfolge `__bx_creds` ein. Fügen Sie als Wert das JSON-Serviceberechtigungsnachweisobjekt aus der Serviceinstanz ein, die Sie zuvor erstellt haben.

### {{site.data.keyword.speechtotextshort}}-Paket verwenden
{: #usage_speechtotext}

Um die Aktionen in diesem Paket verwenden zu können, führen Sie Befehle in folgendem Format aus:

```
ibmcloud fn action invoke speech-to-text-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Testen Sie versuchshalber die Aktion `list-models`.
```
ibmcloud fn action invoke speech-to-text-v1/list-models -b
```
{: pre}

## Watson: 'Speech to Text'
{: #preinstall_speechtotext}

Dieses vorinstallierte Paket ist nicht in der Region 'Tokio' verfügbar. Siehe installierbares Paket [Speech to Text](#install_speechtotext).
{: tip}

Das Paket `/whisk.system/watson-speechToText` bietet eine komfortable Methode zum Aufrufen der Watson-APIs für die Konvertierung von Sprache in Text.
{: shortdesc}

Das Paket enthält die folgenden Aktionen.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/watson-speechToText` | Paket | `username`, `password` | Paket zum Umwandeln von Sprache in Text. |
| `/whisk.system/watson-speechToText/speechToText` | Aktion | `payload`, `content_type`, `encoding`, `username`, `password`, `continuous`, `inactivity_timeout`, `interim_results`, `keywords`, `keywords_threshold`, `max_alternatives`, `model`, `timestamps`, `watson-token`, `word_alternatives_threshold`, `word_confidence`, `X-Watson-Learning-Opt-Out` | Audiodaten in Text umwandeln. |

Das Paket `/whisk.system/watson` einschließlich der Aktion `/whisk.system/watson/speechToText` ist veraltet. Verwenden Sie stattdessen das [{{site.data.keyword.speechtotextshort}}-Paket](#setting-up-the-watson-speech-to-text-package-in-ibm-cloud).
{: deprecated}

### Watson-Paket 'Speech to Text' in {{site.data.keyword.cloud_notm}} einrichten

Wenn Sie {{site.data.keyword.openwhisk}} über {{site.data.keyword.cloud_notm}} verwenden, werden die Paketbindungen automatisch für Ihre {{site.data.keyword.cloud_notm}} Watson-Serviceinstanzen erstellt.

1. Erstellen Sie eine Watson Speech to Text-Serviceinstanz in Ihrem {{site.data.keyword.cloud_notm}}-[Dashboard](https://cloud.ibm.com){: external}. Stellen Sie sicher, dass Sie sich den Namen der Serviceinstanz sowie der {{site.data.keyword.cloud_notm}}-Organisation und den Bereich, in dem Sie sich befinden, merken.

2. Aktualisieren Sie die Pakete in Ihrem Namensbereich. Die Aktualisierung erstellt automatisch eine Paketbindung für die Watson-Serviceinstanz, die Sie erstellt haben.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **Beispielausgabe**

  ```
  created bindings:
  Watson_SpeechToText_Credentials-1
  ```
  {: screen}

3. Listen Sie die Pakete auf, um zu ermitteln, ob die Bindung erstellt wurde. 

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Beispielausgabe**

  ```
  packages
  /myOrg_mySpace/Watson_SpeechToText_Credentials-1 private
  ```
  {: screen}

### Watson-Paket 'Speech to Text' außerhalb von {{site.data.keyword.cloud_notm}} einrichten

Wenn Sie {{site.data.keyword.openwhisk_short}} nicht in {{site.data.keyword.cloud_notm}} verwenden oder wenn Sie den Watson-Service 'Speech to Text' außerhalb von {{site.data.keyword.cloud_notm}} einrichten möchten, müssen Sie manuell eine Paketbindung für Ihren Watson-Service 'Speech to Text' erstellen. Sie benötigen hierzu den Benutzernamen und das Kennwort des Watson-Service 'Speech to Text'.

Erstellen Sie eine Paketbindung, die für Ihren Watson-Service 'Text to Speech' konfiguriert ist.
```
ibmcloud fn package bind /whisk.system/watson-speechToText myWatsonSpeechToText -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

### Umsetzung von Sprache in Text

Mit der Aktion `/whisk.system/watson-speechToText/speechToText` kann eine Audiorede in Text konvertiert werden. Die folgenden Parameter werden unterstützt. 

| Parameter | Beschreibung |
| --- | --- |
| `username` | Der Benutzername für die Watson-API. |
| `password` | Das Kennwort für die Watson-API. |
| `payload` | Die codierten binären Sprachdaten, die in Text umgesetzt werden sollen. |
| `content_type` | Der MIME-Typ der Audiodaten. |
| `encoding` | Die Codierung der binären Sprachdaten. |
| `continuous` | Gibt an, ob mehrere Endergebnisse, die durch lange Pausen voneinander getrennte aufeinanderfolgende Phrasen darstellen, zurückgegeben werden. |
| `inactivity_timeout` | Die Zeit in Sekunden, nach deren Ablauf die Verbindung geschlossen wird, wenn in den übertragenen Audiodaten nur Stille festgestellt wird. |
| `interim_results` | Gibt an, ob der Service Zwischenergebnisse zurückgeben soll. |
| `keywords` | Eine Liste der Schlüsselwörter, die in den Audiodaten erkannt werden sollen. |
| `keywords_threshold` | Ein Übereinstimmungswert, der die Untergrenze für die Erkennung eines Schlüsselworts darstellt. |
| `max_alternatives` | Die maximale Anzahl alternativer Aufzeichnungen, die zurückgegeben werden sollen. |
| `model` | Die Kennung des Modells für die Erkennungsanforderung. |
| `timestamps` | Gibt an, ob für jedes Wort Laufzeitanpassungen (Time Alignment) zurückgegeben werden. |
| `watson-token` | Gibt alternativ zu Serviceberechtigungsnachweisen ein Authentifizierungstoken für den Service an. |
| `word_alternatives_threshold` | Ein Übereinstimmungswert, der die Untergrenze für die Identifikation einer Hypothese als mögliche Wortalternative darstellt. |
| `word_confidence` | Gibt an, ob für jedes Wort ein Übereinstimmungswert im Bereich von 0 bis 1 zurückgegeben wird. |
| `X-Watson-Learning-Opt-Out` | Gibt an, ob die Datenerfassung für den Aufruf abgelehnt wird. |

Rufen Sie die Aktion `speechToText` in Ihrer Paketbindung auf, um die codierten Sprachdaten umzusetzen.

```
ibmcloud fn action invoke myWatsonSpeechToText/speechToText --blocking --result --param payload <base64 encoding of a .wav file> --param content_type 'audio/wav' --param encoding 'base64'
```
{: pre}

**Beispielausgabe**
```
{
  "data": "Hello Watson"
}
```
{: screen}


