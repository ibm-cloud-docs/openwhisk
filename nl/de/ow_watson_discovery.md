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

# {{site.data.keyword.discoveryshort}}-Paket

Der {{site.data.keyword.discoveryfull}}-Service ist eine Engine für kognitive Suche und Inhaltsanalyse, die Sie zu Anwendungen hinzufügen können, um Muster, Trends und umsetzbare Erkenntnisse zu erkennen und so eine bessere Entscheidungsfindung voranzutreiben. Sie können strukturierte und unstrukturierte Daten mit vorab angereichertem Inhalten sicher vereinen und eine vereinfachte Abfragesprache verwenden, um die Notwendigkeit für manuelles Filtern der Ergebnisse zu beseitigen.
{: shortdesc}

Das {{site.data.keyword.discoveryshort}}-Paket enthält die folgenden Entitäten. Weitere Details können Sie in der Referenz für die {{site.data.keyword.discoveryshort}}-API abrufen, indem Sie auf den Namen der betreffenden Entität klicken.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| [`discovery-v1`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html) | Paket | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  |Ermöglicht das Arbeiten mit dem {{site.data.keyword.discoveryshort}}-Service. |
| [create-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-environment) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   name, description, size,  | Erstellt eine Umgebung. |
| [delete-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-environment) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,  | Löscht eine Umgebung. |
| [get-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-environment) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,  | Ruft Informationen zu einer Umgebung ab. |
| [list-environments](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-environments) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    name,  | Listet Umgebungen auf. |
| [list-fields](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-fields) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_ids,  | Listet Felder sammlungsübergreifend auf. |
| [update-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-environment) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,    name, description,  | Aktualisiert eine Umgebung. |
| [create-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-configuration) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,    name, description, conversions, enrichments, normalizations,  | Fügt eine Konfiguration hinzu. |
| [delete-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-configuration) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     configuration_id,  | Löscht eine Konfiguration. |
| [get-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-configuration) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     configuration_id,  | Ruft Konfigurationsdetails ab. |
| [list-configurations](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-configurations) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     name,  | Listet Konfigurationen auf. |
| [update-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-configuration) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     configuration_id,    name, description, conversions, enrichments, normalizations,  | Aktualisiert eine Konfiguration. |
| [test-configuration-in-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#test-configuration-in-environment) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     configuration,     step,     configuration_id,     file,     metadata,     file_content_type,  | Testet eine Konfiguration. |
| [create-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-collection) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,    name, description, configuration_id, language,  | Erstellt eine Sammlung. |
| [delete-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-collection) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  | Löscht eine Sammlung. |
| [get-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-collection) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  | Ruft Sammlungsdetails ab. |
| [list-collection-fields](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collection-fields) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  | Listet Sammlungsfelder auf. |
| [list-collections](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collections) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     name,  | Listet Sammlungen auf. |
| [update-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-collection) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    name, description, configuration_id,  | Aktualisiert eine Sammlung. |
| [create-expansions](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-expansions) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    expansions,  | Erstellt oder aktualisiert die Erweiterungsliste. |
| [delete-expansions](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-expansions) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  | Löscht die Erweiterungsliste. |
| [list-expansions](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-expansions) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  | Ruft die Erweiterungsliste ab. |
| [add-document](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-document) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     file,     metadata,     file_content_type,  | Fügt ein Dokument hinzu. |
| [delete-document](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-document) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     document_id,  | Löscht ein Dokument. |
| [get-document-status](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-document-status) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     document_id,  | Ruft Dokumentdetails ab. |
| [update-document](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-document) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     document_id,     file,     metadata,     file_content_type,  | Aktualisiert ein Dokument. |
| [federated-query](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_ids,     filter,     query,     natural_language_query,     aggregation,     count,     return_fields,     offset,     sort,     highlight,     deduplicate,     deduplicate_field,     similar,     similar_document_ids,     similar_fields,  | Führt eine Abfrage von Dokumenten in mehreren Sammlungen durch. |
| [federated-query-notices](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query-notices) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_ids,     filter,     query,     natural_language_query,     aggregation,     count,     return_fields,     offset,     sort,     highlight,     deduplicate_field,     similar,     similar_document_ids,     similar_fields,  | Fragt Bemerkungen mehrerer Sammelsysteme ab. |
| [query](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     filter,     query,     natural_language_query,     passages,     aggregation,     count,     return_fields,     offset,     sort,     highlight,     passages_fields,     passages_count,     passages_characters,     deduplicate,     deduplicate_field,     similar,     similar_document_ids,     similar_fields,  | Fragt Ihre Sammlung ab. |
| [query-entities](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-entities) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    feature, entity, context, count, evidence_count,  | Fragt eine Knowledge Graph-Entität ab. |
| [query-notices](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-notices) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     filter,     query,     natural_language_query,     passages,     aggregation,     count,     return_fields,     offset,     sort,     highlight,     passages_fields,     passages_count,     passages_characters,     deduplicate_field,     similar,     similar_document_ids,     similar_fields,  | Fragt Bemerkungen des Systems ab. |
| [query-relations](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-relations) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    entities, context, sort, filter, count, evidence_count,  | Fragt eine Knowledge Graph-Beziehung ab. |
| [add-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-training-data) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    natural_language_query, filter, examples,  | Fügt eine Abfrage zu Trainingsdaten hinzu. |
| [create-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-training-example) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,    document_id, cross_reference, relevance,  | Fügt ein Beispiel zu einer Trainingsdatenabfrage hinzu. |
| [delete-all-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-all-training-data) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  | Löscht alle Trainingsdaten. |
| [delete-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-data) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,  | Löscht eine Trainingsdatenabfrage. |
| [delete-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-example) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,     example_id,  | Löscht ein Beispiel für eine Trainingsdatenabfrage. |
| [get-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-data) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,  | Ruft Details zu einer Abfrage ab. |
| [get-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-example) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,     example_id,  | Ruft Details für ein Trainingsdatenbeispiel ab. |
| [list-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-data) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  | Listet Trainingsdaten auf. |
| [list-training-examples](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-examples) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,  | Listet Beispiele für eine Trainingsdatenabfrage auf. |
| [update-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-training-example) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,     example_id,    cross_reference, relevance,  | Ändert eine Bezeichnung oder einen Querverweis für ein Beispiel. |
| [delete-user-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-user-data) | Aktion |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customer_id,  | Löscht gekennzeichnete Daten. |

## {{site.data.keyword.discoveryshort}}-Serviceinstanz erstellen
{: #service_instance}

Vor dem Installieren des Pakets müssen Sie eine Instanz des {{site.data.keyword.discoveryshort}}-Service und Serviceberechtigungsnachweise erstellen.

1. [Erstellen Sie eine {{site.data.keyword.discoveryshort}}-Serviceinstanz ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/catalog/services/discovery).
2. Während der Erstellung der Serviceinstanz werden ebenfalls automatisch generierte Serviceberechtigungsnachweise erstellt.

## {{site.data.keyword.discoveryshort}}-Paket installieren
{: #install}

Nachdem Sie über eine Instanz des {{site.data.keyword.discoveryshort}}-Service verfügen, verwenden Sie die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.openwhisk}}, um das {{site.data.keyword.discoveryshort}}-Paket in Ihrem Namensbereich zu installieren.
{: shortdesc}

### Installation über die {{site.data.keyword.openwhisk_short}}-CLI durchführen
{: #discovery_cli}

Vorbereitende Schritte:
  1. [Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.Bluemix_notm}}-CLI](bluemix_cli.html#cloudfunctions_cli).
  2. Installieren Sie den Befehl [`wskdeploy` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) und fügen Sie die heruntergeladene Binärdatei zu Ihrem Pfad (PATH) hinzu.

Gehen Sie zum Installieren des {{site.data.keyword.discoveryshort}}-Pakets wie folgt vor:

1. Klonen Sie das Repository für das {{site.data.keyword.discoveryshort}}-Paket.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Stellen Sie das Paket bereit.
    ```
    wskdeploy -m openwhisk-sdk/packages/discovery-v1/manifest.yaml
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
    /myOrg_mySpace/discovery-v1                        private
    ```
    {: screen}

4. Binden Sie die Berechtigungsnachweise aus der erstellten {{site.data.keyword.discoveryshort}}-Instanz an das Paket.
    ```
    ibmcloud fn service bind discovery discovery-v1
    ```
    {: pre}

    Beispielausgabe:
    ```
    Credentials 'Credentials-1' from 'discovery' service instance 'Watson Discovery' bound to 'discovery-v1'.
    ```
    {: screen}

5. Stellen Sie sicher, dass das Paket mit Ihren Berechtigungsnachweisen für die {{site.data.keyword.discoveryshort}}-Serviceinstanz konfiguriert ist.
    ```
    ibmcloud fn package get discovery-v1 parameters
    ```
    {: pre}

    Beispielausgabe:
    ```
    ok: got package discovery-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "discovery": {
            "credentials": "Credentials-1",
            "instance": "Watson Discovery",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/discovery/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Installation über die {{site.data.keyword.openwhisk_short}}-UI durchführen
{: #discovery_ui}

1. Öffnen Sie die [Seite 'Erstellen' in der {{site.data.keyword.openwhisk_short}}-Konsole ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/openwhisk/create).

2. Wählen Sie anhand der Listen **Cloud Foundry-Organisation** und **Cloud Foundry-Bereich** den Namensbreich aus, in dem die Installation des {{site.data.keyword.cos_short}}-Pakets erfolgen soll. Namensbereiche werden aus einer Kombination des Organisations- und des Bereichsnamens gebildet.

3. Klicken Sie auf **Pakete installieren**.

4. Klicken Sie auf die Paketgruppe **Watson**.

5. Klicken Sie auf das Paket **Discovery**.

5. Klicken Sie auf **Installieren**.

6. Nachdem das Paket installiert worden ist, werden Sie zur Seite 'Aktionen' weitergeleitet, auf der Sie nach Ihrem neuen Paket suchen können. Dieses trägt die Bezeichnung **discovery-v1**.

7. Um die Aktionen im Paket **discovery-v1** verwenden zu können, müssen Sie Serviceberechtigungsnachweise an die Aktionen binden.
  * Wenn Sie Serviceberechtigungsnachweise an alle Aktionen im Paket binden wollen, führen Sie die Schritte 5 und 6 in den oben aufgeführten CLI-Anweisungen aus. 
  * Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden wollen, führen Sie die nachfolgend aufgeführten Schritte in der UI (Benutzerschnittstelle) aus. **Hinweis**: Für jede Aktion, die Sie verwenden wollen, müssen Sie die nachfolgend aufgeführten Schritte ausführen.
    1. Klicken Sie auf eine Aktion aus dem Paket **discovery-v1**, die Sie verwenden wollen. Die Detailseite für diese Aktion wird geöffnet. 
    2. Klicken Sie im Navigationsbereich links auf den Abschnitt **Parameter**. 
    3. Geben Sie einen neuen **Parameter** ein. Geben Sie als Schlüssel die Zeichenfolge `__bx_creds` ein. Fügen Sie als Wert das JSON-Serviceberechtigungsnachweisobjekt aus der Serviceinstanz ein, die Sie zuvor erstellt haben.

## {{site.data.keyword.discoveryshort}}-Paket verwenden
{: #usage}

Um die Aktionen in diesem Paket verwenden zu können, führen Sie Befehle in folgendem Format aus:

```
ibmcloud fn action invoke discovery-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Für alle Aktionen ist die Angabe eines Versionsparameters im Format JJJJ-MM-TT erforderlich. Wenn rückwärts kompatible Änderungen an der API vorgenommen werden, wird das Datum für eine neue Version freigegeben. Weitere Informationen enthält die [API-Referenz](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html#versioning).

Die Funktionen dieses Pakets verwenden die aktuelle Version 2018-03-05 von Discovery. Testen Sie versuchshalber die Aktion `list-environments`.
```
ibmcloud fn action invoke discovery-v1/list-environments -b -p version 2018-03-05
```
{: pre}
