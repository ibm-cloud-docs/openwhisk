---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: personality insights, watson, cognitive, serverless, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.personalityinsightsshort}}-Paket

Der {{site.data.keyword.personalityinsightsfull}}-Service ermöglicht Anwendungen, Erkenntnisse aus Social Media, Unternehmensdaten and sonstigen Methoden der digitalen Kommunikation abzuleiten. Der Service nutzt linguistische Analysen, um aus der digitalen Kommunikation von Einzelpersonen, wie E-Mails, Textnachrichten, Tweets und Forenbeiträgen, Schlüsse auf die individuellen Persönlichkeitsmerkmale dieser Personen unter Berücksichtigung der 'Big Five' der Persönlichkeitsmerkmale (Fünf-Faktoren-Modell) wie auch der Bedürfnisse und der Werte zu ziehen.
{: shortdesc}

Der Dienst kann aus den Social Media, die möglicherweise einen sehr hohen Anteil an redundanten Informationen aufweisen, automatisch Porträts von Einzelpersonen ableiten, die deren Persönlichkeitsmerkmale widerspiegeln. Basierend auf den Ergebnissen seiner Analyse kann der Service auf Verbraucherpräferenzen schließen und bei JSON-Inhalten, die mit Zeitstempeln versehen sind, das zeitliche Verhalten melden.
* Informationen zur Bedeutung der Modelle, auf die sich der Service zur Beschreibung von Persönlichkeitsmerkmalen stützt, finden Sie im Abschnitt [Persönlichkeitsmodelle](https://cloud.ibm.com/docs/services/personality-insights/models.html).
* Informationen zur Bedeutung der Verbraucherpräferenzen enthält der Abschnitt [Verbraucherpräferenzen](https://cloud.ibm.com/docs/services/personality-insights/preferences.html).

**Hinweis:** Die Protokollierung von Anforderungen ist für den {{site.data.keyword.personalityinsightsshort}}-Service inaktiviert. Daten von Anforderungen und Antworten werden vom Service weder protokolliert noch gespeichert, ganz unabhängig davon, ob der Anforderungsheader `X-Watson-Learning-Opt-Out` festgelegt ist.

Das {{site.data.keyword.personalityinsightsshort}}-Paket enthält die folgenden Entitäten. Weitere Details können Sie in der Referenz für die {{site.data.keyword.personalityinsightsshort}}-API abrufen, indem Sie auf den Namen der betreffenden Entität klicken.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| [`personality-insights-v3`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html) | Paket | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  | Ermöglicht das Arbeiten mit dem {{site.data.keyword.personalityinsightsshort}} V3-Service. |
| [profile](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    content,     content_type,     content_language,     accept_language,     raw_scores,     csv_headers,     consumption_preferences,  | Ruft ein Profil ab. |
| [profile-as-csv](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile-as-csv) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    content,     content_type,     content_language,     accept_language,     raw_scores,     csv_headers,     consumption_preferences,  | Ruft ein Profil als CSV-Datei ab. |

## {{site.data.keyword.personalityinsightsshort}}-Serviceinstanz erstellen
{: #service_instance_insights}

Vor dem Installieren des Pakets müssen Sie eine Instanz des {{site.data.keyword.personalityinsightsshort}}-Service und Serviceberechtigungsnachweise erstellen.
{: shortdesc}

1. [Erstellen Sie eine {{site.data.keyword.personalityinsightsshort}}-Serviceinstanz ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/catalog/services/personality_insights).
2. Während der Erstellung der Serviceinstanz werden ebenfalls automatisch generierte Serviceberechtigungsnachweise erstellt.

## {{site.data.keyword.personalityinsightsshort}}-Paket installieren
{: #install_insights}

Nachdem Sie über eine Instanz des {{site.data.keyword.personalityinsightsshort}}-Service verfügen, verwenden Sie die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.openwhisk}}, um das {{site.data.keyword.personalityinsightsshort}}-Paket in Ihrem Namensbereich zu installieren.
{: shortdesc}

### Installation über die {{site.data.keyword.openwhisk_short}}-CLI durchführen
{: #personalityinsights_cli}

Vorbereitende Schritte:
  1. [Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.Bluemix_notm}}-CLI](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

Gehen Sie zum Installieren des {{site.data.keyword.personalityinsightsshort}}-Pakets wie folgt vor:

1. Klonen Sie das Repository für das {{site.data.keyword.personalityinsightsshort}}-Paket.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Stellen Sie das Paket bereit.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/personality-insights-v3/manifest.yaml
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
    /myOrg_mySpace/personality-insights-v3                        private
    ```
    {: screen}

4. Binden Sie die Berechtigungsnachweise aus der erstellten {{site.data.keyword.personalityinsightsshort}}-Instanz an das Paket.
    ```
    ibmcloud fn service bind personality_insights personality-insights-v3
    ```
    {: pre}

    Abhängig von der Region, in der Sie die Serviceinstanz erstellt haben, wird die Serviceinstanz möglicherweise anders benannt, weil es sich um einen IAM-Service handelt. Sollte der obige Befehl fehlschlagen, verwenden Sie den folgenden Servicenamen für den Befehl 'bind':
    ```
    ibmcloud fn service bind personality-insights personality-insights-v3
    ```
    {: pre}

    Beispielausgabe:
    ```
    Credentials 'Credentials-1' from 'personality_insights' service instance 'Watson Personality Insights' bound to 'personality-insights-v3'.
    ```
    {: screen}

5. Stellen Sie sicher, dass das Paket mit Ihren Berechtigungsnachweisen für die {{site.data.keyword.personalityinsightsshort}}-Serviceinstanz konfiguriert ist.
    ```
    ibmcloud fn package get personality-insights-v3 parameters
    ```
    {: pre}

    Beispielausgabe:
    ```
    ok: got package personality-insights-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "personality_insights": {
            "credentials": "Credentials-1",
            "instance": "Watson Personality Insights",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/personality_insights/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Installation über die Benutzerschnittstelle von {{site.data.keyword.openwhisk_short}} durchführen
{: #personalityinsights_ui}

1. Öffnen Sie die [Seite 'Erstellen' ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/openwhisk/create) in der {{site.data.keyword.openwhisk_short}}-Konsole. 

2. Wählen Sie anhand der Listen **Cloud Foundry-Organisation** und **Cloud Foundry-Bereich** den Namensbereich aus, in dem die Installation des Pakets erfolgen soll. Namensbereiche werden aus einer Kombination des Organisations- und des Bereichsnamens gebildet.

3. Klicken Sie auf **Pakete installieren**.

4. Klicken Sie auf die Paketgruppe **Watson**. 

5. Klicken Sie auf das Paket **Personality Insights** .

5. Klicken Sie auf **Installieren**.

6. Nachdem das Paket installiert worden ist, werden Sie zur Seite 'Aktionen' weitergeleitet, auf der Sie nach Ihrem neuen Paket suchen können. Dieses trägt die Bezeichnung **personality-insights-v3**. 

7. Um die Aktionen im Paket **personality-insights-v3** verwenden zu können, müssen Sie Serviceberechtigungsnachweise an die Aktionen binden. 
  * Wenn Sie Serviceberechtigungsnachweise an alle Aktionen im Paket binden wollen, führen Sie die Schritte 5 und 6 in den oben aufgeführten CLI-Anweisungen aus.
  * Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden wollen, führen Sie die nachfolgend aufgeführten Schritte in der UI (Benutzerschnittstelle) aus. **Hinweis**: Für jede Aktion, die Sie verwenden wollen, müssen Sie die nachfolgend aufgeführten Schritte ausführen.
    1. Klicken Sie auf eine Aktion aus dem Paket **personality-insights-v3**, die Sie verwenden wollen. Die Detailseite für diese Aktion wird geöffnet. 
    2. Klicken Sie im Navigationsbereich links auf den Abschnitt **Parameter**. 
    3. Geben Sie einen neuen **Parameter** ein. Geben Sie als Schlüssel die Zeichenfolge `__bx_creds` ein. Fügen Sie als Wert das JSON-Serviceberechtigungsnachweisobjekt aus der Serviceinstanz ein, die Sie zuvor erstellt haben.

## {{site.data.keyword.personalityinsightsshort}}-Paket verwenden
{: #usage_insights}

Um die Aktionen in diesem Paket verwenden zu können, führen Sie Befehle in folgendem Format aus:

```
ibmcloud fn action invoke personality-insights-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Für alle Aktionen ist die Angabe eines Versionsparameters im Format JJJJ-MM-TT erforderlich. Wenn rückwärts kompatible Änderungen an der API vorgenommen werden, wird das Datum für eine neue Version freigegeben. Weitere Informationen enthält die [API-Referenz](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#versioning).

Die Funktionen dieses Pakets verwenden die aktuelle Version 2017-10-13 von Personality Insights. Testen Sie versuchshalber die Aktion `profile`.
```
ibmcloud fn action invoke personality-insights-v3/profile -b -p version 2017-10-13 -p text "Sie können hier einen Auszug über sich selbst verfassen, der mindestens 100 Wörter lang sein muss. Dieser Auszug ist lediglich Fülltext und liefert wahrscheinlich nichts von besonderem Interesse vom Personality Insights-Service zurück. Der Service nutzt linguistische Analysen, um aus der digitalen Kommunikation von Einzelpersonen, wie E-Mails, Textnachrichten, Tweets und Forenbeiträgen, Schlüsse auf die individuellen Persönlichkeitsmerkmale dieser Personen unter Berücksichtigung der 'Big Five' der Persönlichkeitsmerkmale (Fünf-Faktoren-Modell) wie auch der Bedürfnisse und der Werte zu ziehen. Der Dienst kann aus den Social Media, die möglicherweise einen sehr hohen Anteil an redundanten Informationen aufweisen, automatisch Porträts von Einzelpersonen ableiten, die deren Persönlichkeitsmerkmale widerspiegeln. Basierend auf den Ergebnissen seiner Analyse kann der Service auf Verbraucherpräferenzen schließen und bei JSON-Inhalten, die mit Zeitstempeln versehen sind, das zeitliche Verhalten melden."
```
{: pre}
