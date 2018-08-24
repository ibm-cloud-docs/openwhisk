---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-17"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.visualrecognitionshort}}-Paket

Der {{site.data.keyword.visualrecognitionfull}}-Service verwendet Deep-Learning-Algorithmen, um Szenen, Objekte und Gesichter in Bildern zu erkennen, die Sie in den Service hochladen. Sie können ein angepasstes Klassifikationsmerkmal erstellen und trainieren, um diejenigen Motive zu erkennen, die Ihren Anforderungen entsprechen.
{:shortdesc}

Das {{site.data.keyword.visualrecognitionshort}}-Paket enthält die folgenden Entitäten. Weitere Details können Sie in der Referenz für die {{site.data.keyword.visualrecognitionshort}}-API abrufen, indem Sie auf den Namen der betreffenden Entität klicken.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| [`visual-recognition-v3`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html) | Paket | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  |Ermöglicht das Arbeiten mit dem {{site.data.keyword.visualrecognitionshort}}-Service. |
| [classify](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#classify) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    images_file,     accept_language,     url,     threshold,     owners,     classifier_ids,     images_file_content_type,  | Klassifiziert Bilder. |
| [detect-faces](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#detect-faces) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    images_file,     url,     images_file_content_type,  | Erkennt Gesichter in Bildern. |
| [create-classifier](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#create-classifier) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    name,     classname_positive_examples,     negative_examples,  | Erstellt ein Klassifikationsmerkmal. |
| [delete-classifier](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-classifier) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  | Löscht ein Klassifikationsmerkmal. |
| [get-classifier](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-classifier) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  | Ruft Details zu einem Klassifikationsmerkmal ab. |
| [list-classifiers](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#list-classifiers) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    verbose,  | Ruft eine Liste von Klassifikationsmerkmalen ab. |
| [update-classifier](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#update-classifier) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,     classname_positive_examples,     negative_examples,  | Aktualisiert ein Klassifikationsmerkmal. |
| [get-core-ml-model](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-core-ml-model) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  | Ruft ein Core ML-Modell eines Klassifikationsmerkmals ab. |
| [delete-user-data](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-user-data) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customer_id,  | Löscht gekennzeichnete Daten. |

## {{site.data.keyword.}}-Serviceinstanz erstellen
{: #service_instance}

Vor dem Installieren des Pakets müssen Sie eine Instanz des {{site.data.keyword.}}-Service und Serviceberechtigungsnachweise erstellen.
{: shortdesc}

1. [Erstellen Sie eine {{site.data.keyword.}}-Serviceinstanz ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/catalog/services/watson_vision_combined).
2. Während der Erstellung der Serviceinstanz werden ebenfalls automatisch generierte Serviceberechtigungsnachweise erstellt.

## {{site.data.keyword.}}-Paket installieren
{: #install}

Nachdem Sie über eine Instanz des {{site.data.keyword.}}-Service verfügen, verwenden Sie die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.openwhisk}}, um das {{site.data.keyword.}}-Paket in Ihrem Namensbereich zu installieren.
{: shortdesc}

### Installation über die {{site.data.keyword.openwhisk_short}}-CLI durchführen
{: #visualrecognition_cli}

Vorbereitende Schritte:
  1. [Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.Bluemix_notm}}-CLI](bluemix_cli.html#cloudfunctions_cli).
  2. Installieren Sie den Befehl [`wskdeploy` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) und fügen Sie die heruntergeladene Binärdatei zu Ihrem Pfad (PATH) hinzu.

Gehen Sie zum Installieren des {{site.data.keyword.}}-Pakets wie folgt vor:

1. Klonen Sie das Repository für das {{site.data.keyword.}}-Paket.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Stellen Sie das Paket bereit.
    ```
    wskdeploy -m openwhisk-sdk/packages/visual-recognition-v3/manifest.yaml
    ```
    {: pre}

3. Stellen Sie sicher, dass das Paket zu Ihrer Paketliste hinzugefügt wird.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Ausgabe:
    ```
    packages
    /myOrg_mySpace/visual-recognition-v3                        private
    ```
    {: screen}

4. Binden Sie die Berechtigungsnachweise aus der erstellten {{site.data.keyword.}}-Instanz an das Paket.
    ```
    ibmcloud fn service bind watson-vision-combined visual-recognition-v3
    ```
    {: pre}

    Beispielausgabe:
    ```
    Credentials 'Credentials-1' from 'watson-vision-combined' service instance 'Watson Visual Recognition' bound to 'visual-recognition-v3'.
    ```
    {: screen}

5. Stellen Sie sicher, dass das Paket mit Ihren Berechtigungsnachweisen für die {{site.data.keyword.}}-Serviceinstanz konfiguriert ist.
    ```
    ibmcloud fn package get visual-recognition-v3 parameters
    ```
    {: pre}

    Beispielausgabe:
    ```
    ok: got package visual-recognition-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "watson-vision-combined": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
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

1. Öffnen Sie die [Seite 'Erstellen' in der {{site.data.keyword.openwhisk_short}}-Konsole ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/openwhisk/create).

2. Wählen Sie anhand der Listen **Cloud Foundry-Organisation** und **Cloud Foundry-Bereich** den Namensbreich aus, in dem die Installation des {{site.data.keyword.cos_short}}-Pakets erfolgen soll. Namensbereiche werden aus einer Kombination des Organisations- und des Bereichsnamens gebildet.

3. Klicken Sie auf **Pakete installieren**.

4. Klicken Sie auf die Paketgruppe **Watson**.

5. Klicken Sie auf das Paket **Visual Recognition**.

5. Klicken Sie auf **Installieren**.

6. Nachdem das Paket installiert worden ist, werden Sie zur Seite 'Aktionen' weitergeleitet, auf der Sie nach Ihrem neuen Paket suchen können. Dieses trägt die Bezeichnung **visual-recognition-v3**.

7. Um die Aktionen im Paket **visual-recognition-v3** verwenden zu können, müssen Sie Serviceberechtigungsnachweise an die Aktionen binden.
  * Wenn Sie Serviceberechtigungsnachweise an alle Aktionen im Paket binden wollen, führen Sie die Schritte 5 und 6 in den oben aufgeführten CLI-Anweisungen aus. 
  * Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden wollen, führen Sie die nachfolgend aufgeführten Schritte in der UI (Benutzerschnittstelle) aus. **Hinweis**: Für jede Aktion, die Sie verwenden wollen, müssen Sie die nachfolgend aufgeführten Schritte ausführen.
    1. Klicken Sie auf eine Aktion aus dem Paket **visual-recognition-v3**, die Sie verwenden wollen. Die Detailseite für diese Aktion wird geöffnet. 
    2. Klicken Sie im Navigationsbereich links auf den Abschnitt **Parameter**. 
    3. Geben Sie einen neuen **Parameter** ein. Geben Sie als Schlüssel die Zeichenfolge `__bx_creds` ein. Fügen Sie als Wert das JSON-Serviceberechtigungsnachweisobjekt aus der Serviceinstanz ein, die Sie zuvor erstellt haben.

## {{site.data.keyword.visualrecognitionshort}}-Paket verwenden
{: #usage}

Um die Aktionen in diesem Paket verwenden zu können, führen Sie Befehle in folgendem Format aus:

```
ibmcloud fn action invoke visual-recognition-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Für alle Aktionen ist die Angabe eines Versionsparameters im Format JJJJ-MM-TT erforderlich. Wenn rückwärts kompatible Änderungen an der API vorgenommen werden, wird das Datum für eine neue Version freigegeben. Weitere Informationen enthält die [API-Referenz](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#versioning).

Die Funktionen dieses Pakets verwenden die aktuelle Version 2018-03-19 von Visual Recognition. Testen Sie versuchshalber die Aktion `list-classifiers`.
```
ibmcloud fn action invoke visual-recognition-v3/list-classifiers -b -p version 2018-03-19
```
{: pre}
