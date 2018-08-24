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

# {{site.data.keyword.languagetranslatorshort}}-Paket

{{site.data.keyword.languagetranslatorfull}} übersetzt Text aus einer Sprache in eine andere. Der Service bietet mehrere von IBM bereitgestellte Übersetzungsmodelle an, die Sie basierend auf Ihrer speziellen Terminologie und Sprache anpassen können.
{: shortdesc}

Das {{site.data.keyword.languagetranslatorshort}}-Paket enthält die folgenden Entitäten. Weitere Details können Sie in der Referenz für die {{site.data.keyword.languagetranslatorshort}}-API abrufen, indem Sie auf den Namen der betreffenden Entität klicken.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| [`language-translator-v3`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html) | Paket | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  |Ermöglicht das Arbeiten mit dem {{site.data.keyword.languagetranslatorshort}}-Service. |
| [translate](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#translate) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   text, model_id, source, target,  | Übersetzt Text. |
| [identify](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#identify) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    text,  | Erkennt die Sprache eines Texts. |
| [list-identifiable-languages](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-identifiable-languages) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, | Listet erkennbare Sprachen auf. |
| [create-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#create-model) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    base_model_id,     name,     forced_glossary,     parallel_corpus,  | Erstellt ein Modell. |
| [delete-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#delete-model) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    model_id,  | Löscht ein Modell. |
| [get-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#get-model) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    model_id,  | Ruft Modelldetails ab. |
| [list-models](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-models) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    source,     target,     default_models,  | Listet Modelle auf. |

## {{site.data.keyword.languagetranslatorshort}}-Serviceinstanz erstellen
{: #service_instance}

Vor dem Installieren des Pakets müssen Sie eine Instanz des {{site.data.keyword.languagetranslatorshort}}-Service und Serviceberechtigungsnachweise erstellen.
{: shortdesc}

1. [Erstellen Sie eine {{site.data.keyword.languagetranslatorshort}}-Serviceinstanz ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/catalog/services/language_translator).
2. Während der Erstellung der Serviceinstanz werden ebenfalls automatisch generierte Serviceberechtigungsnachweise erstellt.

## {{site.data.keyword.languagetranslatorshort}}-Paket installieren
{: #install}

Nachdem Sie über eine Instanz des {{site.data.keyword.languagetranslatorshort}}-Service verfügen, verwenden Sie die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.openwhisk}}, um das {{site.data.keyword.languagetranslatorshort}}-Paket in Ihrem Namensbereich zu installieren.
{: shortdesc}

### Installation über die {{site.data.keyword.openwhisk_short}}-CLI durchführen
{: #languagetranslator_cli}

Vorbereitende Schritte:
  1. [Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.Bluemix_notm}}-CLI](bluemix_cli.html#cloudfunctions_cli).
  2. Installieren Sie den Befehl [`wskdeploy` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) und fügen Sie die heruntergeladene Binärdatei zu Ihrem Pfad (PATH) hinzu.

Gehen Sie zum Installieren des {{site.data.keyword.languagetranslatorshort}}-Pakets wie folgt vor:

1. Klonen Sie das Repository für das {{site.data.keyword.languagetranslatorshort}}-Paket.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Stellen Sie das Paket bereit.
    ```
    wskdeploy -m openwhisk-sdk/packages/language-translator-v3/manifest.yaml
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
    /myOrg_mySpace/language-translator-v3                        private
    ```
    {: screen}

4. Binden Sie die Berechtigungsnachweise aus der erstellten {{site.data.keyword.languagetranslatorshort}}-Instanz an das Paket.
    ```
    ibmcloud fn service bind language-translator language-translator-v3
    ```
    {: pre}

    Beispielausgabe:
    ```
    Credentials 'Credentials-1' from 'language-translator' service instance 'Watson Language Translator' bound to 'language-translator-v3'.
    ```
    {: screen}

5. Stellen Sie sicher, dass das Paket mit Ihren Berechtigungsnachweisen für die {{site.data.keyword.languagetranslatorshort}}-Serviceinstanz konfiguriert ist.
    ```
    ibmcloud fn package get language-translator-v3 parameters
    ```
    {: pre}

    Beispielausgabe:
    ```
    ok: got package language-translator-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "language-translator": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
            "instance": "Language Translator-g2",
            "url": "https://gateway.watsonplatform.net/language-translator/api"
          }
        }
      }
    ]
    ```
    {: screen}

### Installation über die {{site.data.keyword.openwhisk_short}}-UI durchführen
{: #languagetranslator_ui}

1. Öffnen Sie die [Seite 'Erstellen' in der {{site.data.keyword.openwhisk_short}}-Konsole ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/openwhisk/create).

2. Wählen Sie anhand der Listen **Cloud Foundry-Organisation** und **Cloud Foundry-Bereich** den Namensbreich aus, in dem die Installation des {{site.data.keyword.cos_short}}-Pakets erfolgen soll. Namensbereiche werden aus einer Kombination des Organisations- und des Bereichsnamens gebildet.

3. Klicken Sie auf **Pakete installieren**.

4. Klicken Sie auf die Paketgruppe **Watson**.

5. Klicken Sie auf das Paket **Language Translator**.

5. Klicken Sie auf **Installieren**.

6. Nachdem das Paket installiert worden ist, werden Sie zur Seite 'Aktionen' weitergeleitet, auf der Sie nach Ihrem neuen Paket suchen können. Dieses trägt die Bezeichnung **language-translator-v3**.

7. Um die Aktionen im Paket **language-translator-v3** verwenden zu können, müssen Sie Serviceberechtigungsnachweise an die Aktionen binden.
  * Wenn Sie Serviceberechtigungsnachweise an alle Aktionen im Paket binden wollen, führen Sie die Schritte 5 und 6 in den oben aufgeführten CLI-Anweisungen aus. 
  * Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden wollen, führen Sie die nachfolgend aufgeführten Schritte in der UI (Benutzerschnittstelle) aus. **Hinweis**: Für jede Aktion, die Sie verwenden wollen, müssen Sie die nachfolgend aufgeführten Schritte ausführen.
    1. Klicken Sie auf eine Aktion aus dem Paket **language-translator-v3**, die Sie verwenden wollen. Die Detailseite für diese Aktion wird geöffnet. 
    2. Klicken Sie im Navigationsbereich links auf den Abschnitt **Parameter**. 
    3. Geben Sie einen neuen **Parameter** ein. Geben Sie als Schlüssel die Zeichenfolge `__bx_creds` ein. Fügen Sie als Wert das JSON-Serviceberechtigungsnachweisobjekt aus der Serviceinstanz ein, die Sie zuvor erstellt haben.

## {{site.data.keyword.languagetranslatorshort}}-Paket verwenden
{: #usage}

Um die Aktionen in diesem Paket verwenden zu können, führen Sie Befehle in folgendem Format aus:

```
ibmcloud fn action invoke language-translator-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Für alle Aktionen ist die Angabe eines Versionsparameters im Format JJJJ-MM-TT erforderlich. Wenn rückwärts kompatible Änderungen an der API vorgenommen werden, wird das Datum für eine neue Version freigegeben. Weitere Informationen enthält die [API-Referenz](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#versioning).

Die Funktionen dieses Pakets verwenden die aktuelle Version 2018-05-01 von Language Translator. Testen Sie versuchshalber die Aktion `identify`.
```
ibmcloud fn action invoke language-translator-v3/identify -b -p version 2018-05-01 -p text hola
```
{: pre}
