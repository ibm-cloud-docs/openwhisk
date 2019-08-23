---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: functions, cognitive,

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


# {{site.data.keyword.visualrecognitionshort}}
{: #pkg_visual_recognition}

Der installierbare {{site.data.keyword.visualrecognitionfull}}-Service verwendet Deep-Learning-Algorithmen, um Szenen, Objekte und Gesichter in Bildern zu erkennen, die Sie in den Service hochladen. Sie können ein angepasstes Klassifikationsmerkmal erstellen und trainieren, um diejenigen Motive zu erkennen, die Ihren Anforderungen entsprechen.
{:shortdesc}

Das {{site.data.keyword.visualrecognitionshort}}-Paket enthält die folgenden Entitäten. Weitere Informationen können Sie in der Referenz für die {{site.data.keyword.visualrecognitionshort}}-API abrufen, indem Sie auf den Namen der betreffenden Entität klicken. 

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| [`visual-recognition-v3`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html){: external} | Paket | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`  | Ermöglicht das Arbeiten mit dem {{site.data.keyword.visualrecognitionshort}}-Service. |
| [`classify`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#classify){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `images_file`, `accept_language`, `url`, `threshold`, `owners`, `classifier_ids`, `images_file_content_type`  | Klassifiziert Bilder. |
| [`detect-faces`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#detect-faces){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `images_file`, `url`, `images_file_content_type`  | Erkennt Gesichter in Bildern. |
| [`create-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#create-classifier){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `classname_positive_examples`, `negative_examples` | Erstellt ein Klassifikationsmerkmal. |
| [`delete-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-classifier){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id`  | Löscht ein Klassifikationsmerkmal. |
| [`get-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-classifier){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id`  | Ruft Details zu einem Klassifikationsmerkmal ab. |
| [`list-classifiers`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#list-classifiers){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `verbose`  | Ruft eine Liste von Klassifikationsmerkmalen ab. |
| [`update-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#update-classifier){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id`, `classname_positive_examples`, `negative_examples`  | Aktualisiert ein Klassifikationsmerkmal. |
| [`get-core-ml-model`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-core-ml-model){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id`  | Ruft ein Core ML-Modell eines Klassifikationsmerkmals ab. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-user-data){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id`  | Löscht gekennzeichnete Daten. |

## {{site.data.keyword.visualrecognitionshort}}-Serviceinstanz erstellen
{: #service_instance_recognition}

Vor dem Installieren des Pakets müssen Sie eine Instanz des {{site.data.keyword.visualrecognitionshort}}-Service und Serviceberechtigungsnachweise erstellen.
{: shortdesc}

1. [Erstellen Sie eine {{site.data.keyword.visualrecognitionshort}}-Serviceinstanz](https://cloud.ibm.com/catalog/services/watson_vision_combined){: external}. 
2. Während der Erstellung der Serviceinstanz werden ebenfalls automatisch generierte Serviceberechtigungsnachweise erstellt.

## {{site.data.keyword.visualrecognitionshort}}-Paket installieren
{: #install_recognition}

Nachdem Sie über eine Instanz des {{site.data.keyword.visualrecognitionshort}}-Service verfügen, verwenden Sie die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.openwhisk}}, um das {{site.data.keyword.visualrecognitionshort}}-Paket in Ihrem Namensbereich zu installieren.
{: shortdesc}

### Installation über die {{site.data.keyword.openwhisk_short}}-CLI durchführen
{: #visualrecognition_cli}

**Vorbereitende Schritte**
[Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.cloud_notm}}-CLI](/docs/openwhisk?topic=cloud-functions-cli_install). 

Führen Sie die folgenden Befehle aus, um das {{site.data.keyword.visualrecognitionshort}}-Paket zu installieren. 

1. Klonen Sie das Repository für das {{site.data.keyword.visualrecognitionshort}}-Paket.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Stellen Sie das Paket bereit.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/visual-recognition-v3/manifest.yaml
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
    /myOrg_mySpace/visual-recognition-v3                        private
    ```
    {: screen}

4. Binden Sie die Berechtigungsnachweise aus der erstellten {{site.data.keyword.visualrecognitionshort}}-Instanz an das Paket.
    ```
    ibmcloud fn service bind watson-vision-combined visual-recognition-v3
    ```
    {: pre}

    **Beispielausgabe**
    ```
    Credentials 'Credentials-1' from 'watson-vision-combined' service instance 'Watson Visual Recognition' bound to 'visual-recognition-v3'.
    ```
    {: screen}

5. Stellen Sie sicher, dass das Paket mit Ihren Berechtigungsnachweisen für die {{site.data.keyword.visualrecognitionshort}}-Serviceinstanz konfiguriert ist.
    ```
    ibmcloud fn package get visual-recognition-v3 parameters
    ```
    {: pre}

    **Beispielausgabe**
    ```
    ok: got package visual-recognition-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "watson-vision-combined": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:ibmcloud:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:ibmcloud:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
            "instance": "Watson Visual Recognition-g2",
            "url": "https://gateway.watsonplatform.net/visual-recognition/api"
          }
        }
      }
    ]
    ```
    {: screen}

### Installation über die {{site.data.keyword.openwhisk_short}}-UI durchführen
{: #visualrecognition_ui}

1. In der {{site.data.keyword.openwhisk_short}}-Konsole rufen Sie die [Seite 'Erstellen'](https://cloud.ibm.com/openwhisk/create){: external} auf. 

2. Wählen Sie anhand der Listen **Cloud Foundry-Organisation** und **Cloud Foundry-Bereich** den Namensbereich aus, in dem die Installation des Pakets erfolgen soll.

3. Klicken Sie auf **Pakete installieren**.

4. Klicken Sie auf die Paketgruppe **Watson**.

5. Klicken Sie auf das Paket **Visual Recognition**.

5. Klicken Sie auf **Installieren**.

6. Nachdem das Paket installiert wurde, werden Sie zur Seite 'Aktionen' weitergeleitet und Sie können nach Ihrem neuen Paket suchen. Sein Name lautet **visual-recognition-v3**. 

7. Um die Aktionen im Paket **visual-recognition-v3** verwenden zu können, müssen Sie Serviceberechtigungsnachweise an die Aktionen binden.
  * Wenn Sie Serviceberechtigungsnachweise an alle Aktionen im Paket binden wollen, führen Sie die Schritte 4 und 5 in den [CLI-Anweisungen](#visualrecognition_cli) aus. 
  * Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden wollen, führen Sie die nachfolgend aufgeführten Schritte in der UI (Benutzerschnittstelle) aus.
   
  Sie müssen die folgenden Schritte für jede Aktion ausführen, die Sie verwenden möchten.
  {: note}

    1. Klicken Sie auf eine Aktion aus dem Paket **visual-recognition-v3**, die Sie verwenden wollen. Die Detailseite für diese Aktion wird geöffnet.
    2. Klicken Sie im Navigationsbereich links auf den Abschnitt **Parameter**.
    3. Geben Sie einen neuen **Parameter** ein. Geben Sie als Schlüssel die Zeichenfolge `__bx_creds` ein. Fügen Sie als Wert das JSON-Serviceberechtigungsnachweisobjekt aus der Serviceinstanz ein, die Sie zuvor erstellt haben.

## {{site.data.keyword.visualrecognitionshort}}-Paket verwenden
{: #usage_recognition}

Um die Aktionen in diesem Paket verwenden zu können, führen Sie Befehle in folgendem Format aus:

```
ibmcloud fn action invoke visual-recognition-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Für alle Aktionen ist die Angabe eines Versionsparameters im Format JJJJ-MM-TT erforderlich. Wenn rückwärts kompatible Änderungen an der API vorgenommen werden, wird das Datum für eine neue Version freigegeben. Weitere Informationen enthält die [API-Referenz](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#versioning){: external}.

Die Funktionen dieses Pakets verwenden die aktuelle Version von Visual Recognition, `2018-03-19`. Testen Sie versuchshalber die Aktion `list-classifiers`.
```
ibmcloud fn action invoke visual-recognition-v3/list-classifiers -b -p version 2018-03-19
```
{: pre}


