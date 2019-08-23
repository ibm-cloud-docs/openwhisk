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


# {{site.data.keyword.discoveryshort}}
{: #pkg_discovery}

Der installierbare {{site.data.keyword.discoveryfull}}-Service ist eine Engine für kognitive Suche und Inhaltsanalyse, die Sie zu Anwendungen hinzufügen können, um Muster, Trends und umsetzbare Erkenntnisse zu identifizieren und so die Entscheidungsfindung zu verbessern. Sie können strukturierte und unstrukturierte Daten mit vorab angereichertem Inhalten sicher vereinen und eine vereinfachte Abfragesprache verwenden, um die Notwendigkeit für manuelles Filtern der Ergebnisse zu beseitigen.
{: shortdesc}

Das {{site.data.keyword.discoveryshort}}-Paket enthält die folgenden Entitäten. Weitere Details können Sie in der Referenz für die {{site.data.keyword.discoveryshort}}-API abrufen, indem Sie auf den Namen der betreffenden Entität klicken. 

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| [`discovery-v1`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html){: external} | Paket | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`  | Ermöglicht das Arbeiten mit dem {{site.data.keyword.discoveryshort}}-Service. |
| [`create-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-environment){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `description`, `size` | Erstellt eine Umgebung. |
| [`delete-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-environment){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id` | Löscht eine Umgebung. |
| [`get-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-environment){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`  | Ruft Informationen zu einer Umgebung ab. |
| [`list-environments`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-environments){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`  | Listet Umgebungen auf. |
| [`list-fields`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-fields){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_ids` | Listet Felder sammlungsübergreifend auf. |
| [`update-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-environment){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `name`, `description` | Aktualisiert eine Umgebung. |
| [`create-configuration`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-configuration){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `name`, `description`, `conversions`, `enrichments`, `normalizations`  | Fügt eine Konfiguration hinzu. |
| [`delete-configuration`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-configuration){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `configuration_id` | Löscht eine Konfiguration. |
| [`get-configuration`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-configuration){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `configuration_id` | Ruft Konfigurationsdetails ab. |
| [`list-configurations`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-configurations){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `name` | Listet Konfigurationen auf. |
| [`update-configuration`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-configuration){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `configuration_id`, `name`, `description`, `conversions`, `enrichments`, `normalizations` | Aktualisiert eine Konfiguration. |
| [`test-configuration-in-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#test-configuration-in-environment){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `configuration`,  `step`, `configuration_id`, `file`, `metadata`, `file_content_type` | Testet eine Konfiguration. |
| [`create-collection`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-collection){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `name`, `description`, `configuration_id`, `language` | Erstellt eine Sammlung. |
| [`delete-collection`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-collection){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Löscht eine Sammlung. |
| [`get-collection`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-collection){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Ruft Sammlungsdetails ab. |
| [`list-collection-fields`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collection-fields){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Listet Sammlungsfelder auf. |
| [`list-collections`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collections){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `name` | Listet Sammlungen auf. |
| [`update-collection`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-collection){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `name`, `description`, `configuration_id` | Aktualisiert eine Sammlung. |
| [`create-expansions`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-expansions){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, ``collection_id`, `expansions` | Erstellt oder aktualisiert die Erweiterungsliste. |
| [`delete-expansions`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-expansions){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Löscht die Erweiterungsliste. |
| [`list-expansions`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-expansions){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Ruft die Erweiterungsliste ab. |
| [`add-document`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-document){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `file`, `metadata`, `file_content_type` | Fügt ein Dokument hinzu. |
| [`delete-document`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-document){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `document_id` | Löscht ein Dokument. |
| [`get-document-status`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-document-status){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `document_id` | Ruft Dokumentdetails ab. |
| [`update-document`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-document){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `document_id`, `file`,   `metadata`, `file_content_type` | Aktualisiert ein Dokument. |
| [`federated-query`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_ids`, `filter`, `query`,     `natural_language_query`, `aggregation`, `count`, `return_fields`, `offset`, `sort`, `highlight`, `deduplicate`, `deduplicate_field`, `similar`, `similar_document_ids`, `similar_fields` | Führt eine Abfrage von Dokumenten in mehreren Sammlungen durch. |
| [`federated-query-notices`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query-notices){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_ids`, `filter`, `query`, `natural_language_query`, `aggregation`, `count`, `return_fields`, `offset`, `sort`, `highlight`, `deduplicate_field`, `similar`, `similar_document_ids`, `similar_fields` | Fragt Bemerkungen mehrerer Sammelsysteme ab. |
| [`query`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `filter`, `query`, `natural_language_query`, `passages`, `aggregation`, `count`, `return_fields`, `offset`, `sort`, `highlight`, `passages_fields`, `passages_count`, `passages_characters`, `deduplicate`, `deduplicate_field`, `similar`, `similar_document_ids`, `similar_fields` | Fragt Ihre Sammlung ab. |
| [`query-entities`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-entities){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `feature`, `entity`, `context`, `count`, `evidence_count` | Fragt eine Knowledge Graph-Entität ab. |
| [`query-notices`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-notices){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `filter`, `query`, `natural_language_query`, `passages`, `aggregation`, `count`, `return_fields`, `offset`, `sort`, `highlight`, `passages_fields`, `passages_count`, `passages_characters`, `deduplicate_field`, `similar`, `similar_document_ids`, `similar_fields`  | Fragt Bemerkungen des Systems ab. |
| [`query-relations`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-relations){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `entities`, `context`, `sort`, `filter`, `count`, `evidence_count` | Fragt eine Knowledge Graph-Beziehung ab. |
| [`add-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-training-data){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey, iam_url`, `headers, headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `natural_language_query`, `filter`, `examples`, | Fügt eine Abfrage zu Trainingsdaten hinzu. |
| [`create-training-example`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-training-example){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id`, `document_id`, `cross_reference`, `relevance` | Fügt ein Beispiel zu einer Trainingsdatenabfrage hinzu. |
| [`delete-all-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-all-training-data){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Löscht alle Trainingsdaten. |
| [`delete-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-data){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id` | Löscht eine Trainingsdatenabfrage. |
| [`delete-training-example`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-example){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id`, `example_id` | Löscht ein Beispiel für eine Trainingsdatenabfrage. |
| [`get-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-data){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id` | Ruft Details zu einer Abfrage ab. |
| [`get-training-example`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-example){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id`, `example_id` | Ruft Details für ein Trainingsdatenbeispiel ab. |
| [`list-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-data){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Listet Trainingsdaten auf. |
| [`list-training-examples`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-examples){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id` | Listet Beispiele für eine Trainingsdatenabfrage auf. |
| [`update-training-example`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-training-example){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id`, `example_id`, `cross_reference`, `relevance` | Ändert eine Bezeichnung oder einen Querverweis für ein Beispiel. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-user-data){: external} | Aktion | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id` | Löscht gekennzeichnete Daten. |

## {{site.data.keyword.discoveryshort}}-Serviceinstanz erstellen
{: #service_instance_discovery}

Vor dem Installieren des Pakets müssen Sie eine Instanz des {{site.data.keyword.discoveryshort}}-Service und Serviceberechtigungsnachweise erstellen.

1. [Erstellen Sie eine {{site.data.keyword.discoveryshort}}-Serviceinstanz](https://cloud.ibm.com/catalog/services/discovery){: external}. 
2. Während der Erstellung der Serviceinstanz werden ebenfalls automatisch generierte Serviceberechtigungsnachweise erstellt.

## {{site.data.keyword.discoveryshort}}-Paket installieren
{: #install_discovery}

Nachdem Sie über eine Instanz des {{site.data.keyword.discoveryshort}}-Service verfügen, verwenden Sie die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.openwhisk}}, um das {{site.data.keyword.discoveryshort}}-Paket in Ihrem Namensbereich zu installieren.
{: shortdesc}

### Installation über die {{site.data.keyword.openwhisk_short}}-CLI durchführen
{: #discovery_cli}

**Vorbereitende Schritte**
[Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.cloud_notm}}-CLI](/docs/openwhisk?topic=cloud-functions-cli_install). 

Führen Sie die folgenden Befehle aus, um das {{site.data.keyword.discoveryshort}}-Paket zu installieren: 

1. Klonen Sie das Repository für das {{site.data.keyword.discoveryshort}}-Paket.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Stellen Sie das Paket bereit.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/discovery-v1/manifest.yaml
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
    /myOrg_mySpace/discovery-v1                        private
    ```
    {: screen}

4. Binden Sie die Berechtigungsnachweise aus der erstellten {{site.data.keyword.discoveryshort}}-Instanz an das Paket.
    ```
    ibmcloud fn service bind discovery discovery-v1
    ```
    {: pre}

    **Beispielausgabe**
    ```
    Credentials 'Credentials-1' from 'discovery' service instance 'Watson Discovery' bound to 'discovery-v1'.
    ```
    {: screen}

5. Stellen Sie sicher, dass das Paket mit Ihren Berechtigungsnachweisen für die {{site.data.keyword.discoveryshort}}-Serviceinstanz konfiguriert ist.
    ```
    ibmcloud fn package get discovery-v1 parameters
    ```
    {: pre}

    **Beispielausgabe**
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

1. In der {{site.data.keyword.openwhisk_short}}-Konsole rufen Sie die [Seite 'Erstellen'](https://cloud.ibm.com/openwhisk/create){: external} auf. 

2. Wählen Sie mithilfe des Dropdown-Menüs für den Namensbereich den Namensbereich aus, in dem das Paket installiert werden soll. 

3. Klicken Sie auf **Pakete installieren**.

4. Klicken Sie auf die Paketgruppe **Watson**. 

5. Klicken Sie auf das Paket **Discovery**. 

6. Klicken Sie auf **Installieren**.

7. Nachdem das Paket installiert wurde, werden Sie zur Seite 'Aktionen' weitergeleitet und Sie können nach Ihrem neuen Paket suchen. Sein Name lautet **discovery-v1**. 

8. Um die Aktionen im Paket `discovery-v1` verwenden zu können, müssen Sie Serviceberechtigungsnachweise an die Aktionen binden. 
  * Wenn Sie Serviceberechtigungsnachweise an alle Aktionen im Paket binden wollen, führen Sie Schritt 4 in den CLI-Anweisungen aus. 
  * Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden wollen, führen Sie die nachfolgend aufgeführten Schritte in der UI (Benutzerschnittstelle) aus.

  Sie müssen die folgenden Schritte für jede Aktion ausführen, die Sie verwenden möchten.
  {: note}

    1. Klicken Sie auf eine Aktion aus dem Paket `discovery-v1`, die Sie verwenden möchten. Die Detailseite für diese Aktion wird geöffnet.
    2. Klicken Sie im Navigationsbereich links auf den Abschnitt **Parameter**.
    3. Geben Sie einen neuen **Parameter** ein. Geben Sie als Schlüssel die Zeichenfolge `__bx_creds` ein. Fügen Sie als Wert das JSON-Serviceberechtigungsnachweisobjekt aus der Serviceinstanz ein, die Sie zuvor erstellt haben.

## {{site.data.keyword.discoveryshort}}-Paket verwenden
{: #usage_discovery}

Um die Aktionen in diesem Paket verwenden zu können, führen Sie Befehle in folgendem Format aus:

```
ibmcloud fn action invoke discovery-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Für alle Aktionen ist die Angabe eines Versionsparameters im Format JJJJ-MM-TT erforderlich. Wenn rückwärts kompatible Änderungen an der API vorgenommen werden, wird das Datum für eine neue Version freigegeben. Weitere Informationen enthält die [API-Referenz](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html#versioning){: external}.

Die Funktionen dieses Pakets verwenden die aktuelle Version von Discovery, `2018-03-05`. Testen Sie versuchshalber die Aktion `list-environments`.
```
ibmcloud fn action invoke discovery-v1/list-environments -b -p version 2018-03-05
```
{: pre}



