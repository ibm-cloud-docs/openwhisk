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

# {{site.data.keyword.toneanalyzershort}}-Paket

Der {{site.data.keyword.toneanalyzerfull}}-Service verwendet die linguistische Analyse, um eine Vielzahl an Varianten im Tonfall auf emotionaler wie auch auf linguistischer Ebene zu erkennen.
{:shortdesc}

Der Service kann den Tonfall sowohl auf Dokument- als auch auf Satzebene analysieren. Sie können den Service verwenden, um zu verstehen, wie Ihre schriftliche Kommunikation wahrgenommen wird, und dann den Tonfall bei Ihrer Kommunikation zu verbessern. Unternehmen können den Service nutzen, um sich mit dem jeweiligen Kommunikationsstil ihrer Kunden vertraut zu machen und auf jeden Kunden angemessen zu reagieren, oder um ihre Kundengespräche zu verstehen und zu verbessern.

**Hinweis:** Die Protokollierung von Anforderungen ist für den Tone Analyzer-Service inaktiviert. Daten von Anforderungen und Antworten werden vom Service weder protokolliert noch gespeichert, ganz unabhängig davon, ob der Anforderungsheader `X-Watson-Learning-Opt-Out` festgelegt ist.

Das {{site.data.keyword.toneanalyzershort}}-Paket enthält die folgenden Entitäten. Weitere Details können Sie in der Referenz für die {{site.data.keyword.toneanalyzershort}}-API abrufen, indem Sie auf den Namen der betreffenden Entität klicken.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| [`tone-analyzer-v3`](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html) | Paket | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  |Ermöglicht das Arbeiten mit dem {{site.data.keyword.toneanalyzershort}}-Service. |
| [tone](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    tone_input,     content_type,     sentences,     tones,     content_language,     accept_language,  | Analysiert den allgemeinen Tonfall. |
| [tone-chat](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone-chat) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   utterances,     content_language,     accept_language,  | Analysiert den Tonfall der Kundeninteraktion. |

## {{site.data.keyword.toneanalyzershort}}-Serviceinstanz erstellen
{: #service_instance}

Vor dem Installieren des Pakets müssen Sie eine Instanz des {{site.data.keyword.toneanalyzershort}}-Service und Serviceberechtigungsnachweise erstellen.
{: shortdesc}

1. [Erstellen Sie eine {{site.data.keyword.toneanalyzershort}}-Serviceinstanz ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/catalog/services/tone_analyzer).
2. Während der Erstellung der Serviceinstanz werden ebenfalls automatisch generierte Serviceberechtigungsnachweise erstellt.

## {{site.data.keyword.toneanalyzershort}}-Paket installieren
{: #install}

Nachdem Sie über eine Instanz des {{site.data.keyword.toneanalyzershort}}-Service verfügen, verwenden Sie die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.openwhisk}}, um das {{site.data.keyword.toneanalyzershort}}-Paket in Ihrem Namensbereich zu installieren.
{: shortdesc}

### Installation über die {{site.data.keyword.openwhisk_short}}-CLI durchführen
{: #toneanalyzer_cli}

Vorbereitende Schritte:
  1. [Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.Bluemix_notm}}-CLI](bluemix_cli.html#cloudfunctions_cli).
  2. Installieren Sie den Befehl [`wskdeploy` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) und fügen Sie die heruntergeladene Binärdatei zu Ihrem Pfad (PATH) hinzu.

Gehen Sie zum Installieren des {{site.data.keyword.toneanalyzershort}}-Pakets wie folgt vor:

1. Klonen Sie das Repository für das {{site.data.keyword.toneanalyzershort}}-Paket.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Stellen Sie das Paket bereit.
    ```
    wskdeploy -m openwhisk-sdk/packages/tone-analyzer-v3/manifest.yaml
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
    /myOrg_mySpace/tone-analyzer-v3                        private
    ```
    {: screen}

4. Binden Sie die Berechtigungsnachweise aus der erstellten {{site.data.keyword.toneanalyzershort}}-Instanz an das Paket.
    ```
    ibmcloud fn service bind tone_analyzer tone-analyzer-v3
    ```
    {: pre}

    Abhängig von der Region, in der Sie die Serviceinstanz erstellt haben, wird die Serviceinstanz möglicherweise anders benannt, weil es sich um einen IAM-Service handelt. Sollte der obige Befehl fehlschlagen, verwenden Sie den folgenden Servicenamen für den Befehl 'bind':
    ```
    ibmcloud fn service bind tone-analyzer tone-analyzer-v3
    ```
    {: pre}
    Beispielausgabe:
    ```
    Credentials 'Credentials-1' from 'tone_analyzer' service instance 'Watson Tone Analyzer' bound to 'tone-analyzer-v3'.
    ```
    {: screen}

5. Stellen Sie sicher, dass das Paket mit Ihren Berechtigungsnachweisen für die {{site.data.keyword.toneanalyzershort}}-Serviceinstanz konfiguriert ist.
    ```
    ibmcloud fn package get tone-analyzer-v3 parameters
    ```
    {: pre}

    Beispielausgabe:
    ```
    ok: got package tone-analyzer-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "tone_analyzer": {
            "credentials": "Credentials-1",
            "instance": "Watson Tone Analyzer",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/assistant/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Installation über die {{site.data.keyword.openwhisk_short}}-UI durchführen
{: #toneanalyzer_ui}

1. Öffnen Sie die [Seite 'Erstellen' in der {{site.data.keyword.openwhisk_short}}-Konsole ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/openwhisk/create).

2. Wählen Sie anhand der Listen **Cloud Foundry-Organisation** und **Cloud Foundry-Bereich** den Namensbreich aus, in dem die Installation des {{site.data.keyword.cos_short}}-Pakets erfolgen soll. Namensbereiche werden aus einer Kombination des Organisations- und des Bereichsnamens gebildet.

3. Klicken Sie auf **Pakete installieren**.

4. Klicken Sie auf die Paketgruppe **Watson**.

5. Klicken Sie auf das Paket **Tone Analyzer**.

5. Klicken Sie auf **Installieren**.

6. Nachdem das Paket installiert worden ist, werden Sie zur Seite 'Aktionen' weitergeleitet, auf der Sie nach Ihrem neuen Paket suchen können. Dieses trägt die Bezeichnung **tone-analyzer-v3**.

7. Um die Aktionen im Paket **tone-analyzer-v3** verwenden zu können, müssen Sie Serviceberechtigungsnachweise an die Aktionen binden.
  * Wenn Sie Serviceberechtigungsnachweise an alle Aktionen im Paket binden wollen, führen Sie die Schritte 5 und 6 in den oben aufgeführten CLI-Anweisungen aus. 
  * Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden wollen, führen Sie die nachfolgend aufgeführten Schritte in der UI (Benutzerschnittstelle) aus. **Hinweis**: Für jede Aktion, die Sie verwenden wollen, müssen Sie die nachfolgend aufgeführten Schritte ausführen.
    1. Klicken Sie auf eine Aktion aus dem Paket **tone-analyzer-v3**, die Sie verwenden wollen. Die Detailseite für diese Aktion wird geöffnet. 
    2. Klicken Sie im Navigationsbereich links auf den Abschnitt **Parameter**. 
    3. Geben Sie einen neuen **Parameter** ein. Geben Sie als Schlüssel die Zeichenfolge `__bx_creds` ein. Fügen Sie als Wert das JSON-Serviceberechtigungsnachweisobjekt aus der Serviceinstanz ein, die Sie zuvor erstellt haben.

## {{site.data.keyword.toneanalyzershort}}-Paket verwenden
{: #usage}

Um die Aktionen in diesem Paket verwenden zu können, führen Sie Befehle in folgendem Format aus:

```
ibmcloud fn action invoke tone-analyzer-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Für alle Aktionen ist die Angabe eines Versionsparameters im Format JJJJ-MM-TT erforderlich. Wenn rückwärts kompatible Änderungen an der API vorgenommen werden, wird das Datum für eine neue Version freigegeben. Weitere Informationen enthält die [API-Referenz](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#versioning).

Die Funktionen dieses Pakets verwenden die aktuelle Version 2017-09-21 von Tone Analyzer. Testen Sie versuchshalber die Aktion `tone`.
```
ibmcloud fn action invoke tone-analyzer-v3/tone -b -p version 2017-09-21 -p text "i hope you're having a wonderful day"
```
{: pre}
