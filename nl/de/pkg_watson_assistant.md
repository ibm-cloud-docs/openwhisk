---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: openwhisk, functions

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


# {{site.data.keyword.conversationshort}}
{: #pkg_watson_assistant}

Der installierbare {{site.data.keyword.conversationfull}}-Service kombiniert maschinelles Lernen, das Verständnis natürlicher Sprache und integrierte Dialog-Tools, um Konversationsflüsse zwischen Ihren Apps und Ihren Nutzern zu erstellen.
{: shortdesc}

Das {{site.data.keyword.conversationshort}}-Paket enthält die folgenden Entitäten. Weitere Informationen können Sie in der Referenz für die {{site.data.keyword.conversationshort}}-API abrufen, indem Sie auf den Namen der betreffenden Entität klicken. 

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| [`assistant-v1`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html){: external} | Paket | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`  | Ermöglicht das Arbeiten mit dem {{site.data.keyword.conversationshort}}-Service. |
| [`message`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `input`, `alternate_intents`, `context`, `entities`, `intents`, `output`, `nodes_visited_details`  | Ruft die Antwort auf eine Benutzereingabe ab. |
| [`create-workspace`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-workspace){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `description`, `language`, `intents`, `entities`, `dialog_nodes`, `counterexamples`, `metadata`, `learning_opt_out`  | Erstellt einen Arbeitsbereich. |
| [`delete-workspace`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-workspace){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`  | Löscht einen Arbeitsbereich. |
| [`get-workspace`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-workspace){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `export`, `include_audit`  | Ruft Informationen zu einem Arbeitsbereich ab. |
| [`list-workspaces`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-workspaces){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | Listet Arbeitsbereiche auf. |
| [`update-workspace`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-workspace){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `name`, `description`, `language`, `intents`, `entities`, dialog_nodes, counterexamples, `metadata`, `learning_opt_out`, `append`  | Aktualisiert einen Arbeitsbereich. |
| [`create-intent`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-intent){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `description`, `examples`  | Erstellt eine Absicht. |
| [`delete-intent`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-intent){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`  | Löscht eine Absicht. |
| [`get-intent`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-intent){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `export`, `include_audit`  | Ruft Informationen zu einer Absicht ab. |
| [`list-intents`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-intents){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `export`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | Listet Absichten auf. |
| [`update-intent`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-intent){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `new_intent`, `new_description`, `new_examples`  | Aktualisiert eine Absicht. |
| [`create-example`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-example){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `text`  | Erstellt ein Benutzereingabebeispiel. |
| [`delete-example`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-example){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `text`  | Löscht ein Benutzereingabebeispiel. |
| [`get-example`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-example){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `text`, `include_audit`  | Ruft Informationen zu einem Benutzereingabebeispiel ab. |
| [`list-examples`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-examples){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | Listet Benutzereingabebeispiele auf. |
| [`update-example`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-example){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `intent`, `text`, `new_text`  | Aktualisiert ein Benutzereingabebeispiel. |
| [`create-counterexample`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-counterexample){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `text`  | Erstellt ein Gegenbeispiel. |
| [`delete-counterexample`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-counterexample){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `text`  | Löscht ein Gegenbeispiel. |
| [`get-counterexample`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-counterexample){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `text`, `include_audit`  | Ruft Informationen zu einem Gegenbeispiel ab. |
| [`list-counterexamples`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-counterexamples){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | Listet Gegenbeispiele auf. |
| [`update-counterexample`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-counterexample){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `text`, `new_text`  | Aktualisiert ein Gegenbeispiel. |
| [`create-entity`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-entity){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `description`, `metadata`, `values`, `fuzzy_match`  | Erstellt eine Entität. |
| [`delete-entity`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-entity){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`  | Löscht eine Entität. |
| [`get-entity`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-entity){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `export`, `include_audit`  | Ruft Informationen zu einer Entität ab. |
| [`list-entities`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-entities){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `export`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | Listet Entitäten auf. |
| [`update-entity`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-entity){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `new_entity`, `new_description`, `new_metadata`, `new_fuzzy_match`, `new_values`  | Aktualisiert eine Entität. |
| [`create-value`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-value){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `metadata`, `synonyms`, `patterns`, `value_type`  | Fügt einen Entitätswert hinzu. |
| [`delete-value`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-value){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`  | Löscht einen Entitätswert. |
| [`get-value`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-value){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `export`, `include_audit`  | Ruft einen Entitätswert ab. |
| [`list-values`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-values){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `export`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | Listet Entitätswerte auf. |
| [`update-value`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-value){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `new_value`, `new_metadata`, `new_type`, `new_synonyms`, `new_patterns`  | Aktualisiert einen Entitätswert. |
| [`create-synonym`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-synonym){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `synonym`  | Fügt ein Synonym eines Entitätswerts hinzu. |
| [`delete-synonym`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-synonym){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `synonym`  | Löscht ein Synonym eines Entitätswerts. |
| [`get-synonym`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-synonym){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `synonym`, `include_audit`  | Ruft ein Synonym eines Entitätswerts ab. |
| [`list-synonyms`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-synonyms){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | Listet Synonym von Entitätswerten auf. |
| [`update-synonym`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-synonym){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `entity`, `value`, `synonym`, `new_synonym`  | Aktualisiert ein Synonym eines Entitätswerts. |
| [`create-dialog-node`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-dialog-node){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `dialog_node`, `description`, `conditions`, `parent`, `previous_sibling`, `output`, `context`, `metadata`, `next_step`, `actions`, `title`, `node_type`, `event_name`, `variable`, `digress_in`, `digress_out`, `digress_out_slots`  | Erstellt einen Dialogknoten. |
| [`delete-dialog-node`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-dialog-node){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, dialog_node  | Löscht einen Dialogknoten. |
| [`get-dialog-node`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-dialog-node){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `dialog_node`, `include_audit`  | Ruft einen Dialogknoten ab. |
| [`list-dialog-nodes`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-dialog-nodes){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `page_limit`, `include_count`, `sort`, `cursor`, `include_audit`  | Listet Dialogknoten auf. |
| [`update-dialog-node`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-dialog-node){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `dialog_node`, `new_dialog_node`, `new_description`, `new_conditions`, `new_parent`, `new_previous_sibling`, `new_output`, `new_context`, `new_metadata`, `new_next_step`, `new_title`, `new_type`, `new_event_name`, `new_variable`, `new_actions`, `new_digress_in`, `new_digress_out`, `new_digress_out_slots`  | Aktualisiert einen Dialogknoten. |
| [`list-all-logs`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-all-logs){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `filter`, `sort`, `page_limit`, `cursor`  | Listet Protokollereignisse in allen Arbeitsbereichen auf. |
| [`list-logs`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-logs){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `workspace_id`, `sort`, `filter`, `page_limit`, `cursor`  | Listet Protokollereignisse in einem Arbeitsbereich auf. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-user-data){: external} | Aktion |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id`  | Löscht gekennzeichnete Daten. |

## {{site.data.keyword.conversationshort}}-Serviceinstanz erstellen
{: #service_instance_conversation}

Vor dem Installieren des Pakets müssen Sie eine Instanz des {{site.data.keyword.conversationshort}}-Service und Serviceberechtigungsnachweise erstellen.

1. [Erstellen Sie eine {{site.data.keyword.conversationshort}}-Serviceinstanz](https://cloud.ibm.com/catalog/services/conversation){: external}. 
2. Während der Erstellung der Serviceinstanz werden ebenfalls automatisch generierte Serviceberechtigungsnachweise erstellt.

## {{site.data.keyword.conversationshort}}-Paket installieren
{: #install_conversation}

Nachdem Sie über eine Instanz des {{site.data.keyword.conversationshort}}-Service verfügen, verwenden Sie die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.openwhisk}}, um das {{site.data.keyword.conversationshort}}-Paket in Ihrem Namensbereich zu installieren.
{: shortdesc}

### Installation über die {{site.data.keyword.openwhisk_short}}-CLI durchführen
{: #conversation_cli}

**Vorbereitende Schritte**
[Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.cloud_notm}}-CLI](/docs/openwhisk?topic=cloud-functions-cli_install). 

Führen Sie die folgenden Befehle aus, um das {{site.data.keyword.conversationshort}}-Paket zu installieren. 

1. Klonen Sie das Repository für das {{site.data.keyword.conversationshort}}-Paket.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Stellen Sie das Paket bereit.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/assistant-v1/manifest.yaml
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
    /myOrg_mySpace/assistant-v1                        private
    ```
    {: screen}

4. Binden Sie die Berechtigungsnachweise aus der erstellten {{site.data.keyword.conversationshort}}-Instanz an das Paket.
    ```
    ibmcloud fn service bind conversation assistant-v1
    ```
    {: pre}

    **Beispielausgabe**
    ```
    Credentials 'Credentials-1' from 'conversation' service instance 'Watson Assistant (formerly Conversation)-8h' bound to 'assistant-v1'.
    ```
    {: screen}

5. Stellen Sie sicher, dass das Paket mit Ihren Berechtigungsnachweisen für die {{site.data.keyword.conversationshort}}-Serviceinstanz konfiguriert ist.
    ```
    ibmcloud fn package get assistant-v1 parameters
    ```
    {: pre}

    **Beispielausgabe**
    ```
    ok: got package assistant-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "conversation": {
            "credentials": "Credentials-1",
            "instance": "Watson Assistant (formerly Conversation)-8h",
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
{: #conversation_ui}

1. In der {{site.data.keyword.openwhisk_short}}-Konsole rufen Sie die [Seite 'Erstellen'](https://cloud.ibm.com/openwhisk/create){: external} auf. 

2. Wählen Sie mithilfe des Dropdown-Menüs für den Namensbereich den Namensbereich aus, in dem das Paket installiert werden soll. 

3. Klicken Sie auf **Pakete installieren**.

4. Klicken Sie auf die Paketgruppe **Watson**. 

5. Klicken Sie auf das Paket **Watson Assistant**.

5. Klicken Sie auf **Installieren**.

6. Nachdem das Paket installiert wurde, werden Sie zur Seite 'Aktionen' weitergeleitet und Sie können nach Ihrem neuen Paket suchen. Sein Name lautet **assistant-v1**. 

7. Um die Aktionen im Paket **assistant-v1** verwenden zu können, müssen Sie Serviceberechtigungsnachweise an die Aktionen binden.
  * Wenn Sie Serviceberechtigungsnachweise an alle Aktionen im Paket binden wollen, führen Sie die Schritte 4 und 5 in den [CLI-Anweisungen](#conversation_cli) aus. 
  * Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden wollen, führen Sie die nachfolgend aufgeführten Schritte in der UI (Benutzerschnittstelle) aus. 
  
  Sie müssen die folgenden Schritte für jede Aktion ausführen, die Sie verwenden möchten.
  {: note}

    1. Klicken Sie auf eine Aktion aus dem Paket **assistant-v1**, die Sie verwenden wollen. Die Detailseite für diese Aktion wird geöffnet.
    2. Klicken Sie im Navigationsbereich links auf den Abschnitt **Parameter**.
    3. Geben Sie einen neuen **Parameter** ein. Geben Sie als Schlüssel die Zeichenfolge `__bx_creds` ein. Fügen Sie als Wert das JSON-Serviceberechtigungsnachweisobjekt aus der Serviceinstanz ein, die Sie zuvor erstellt haben.

## {{site.data.keyword.conversationshort}}-Paket verwenden
{: #usage_conversation}

Um die Aktionen in diesem Paket verwenden zu können, führen Sie Befehle in folgendem Format aus. 

```
ibmcloud fn action invoke assistant-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Für alle Aktionen ist die Angabe eines Versionsparameters im Format JJJJ-MM-TT erforderlich. Wenn rückwärts kompatible Änderungen an der API vorgenommen werden, wird das Datum für eine neue Version freigegeben. Weitere Informationen finden Sie in der [API-Referenz](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html#versioning){: external}. 

Die Funktionen dieses Pakets verwenden die aktuelle Version von Watson Assistant, `2018-07-10`. Testen Sie versuchshalber die Aktion `list-workspaces`.
```
ibmcloud fn action invoke assistant-v1/list-workspaces -b -p version 2018-07-10
```
{: pre}


