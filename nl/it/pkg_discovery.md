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

Il servizio {{site.data.keyword.discoveryfull}} installabile è un motore di ricerca cognitiva e di analisi dei contenuti che puoi aggiungere alle applicazioni per identificare modelli, tendenze e informazioni approfondite di utilità pratica per favorire un migliore processo decisionale. Unifica in modo sicuro dati strutturati e non strutturati con contenuto prearricchito ed utilizza un linguaggio query semplificato per eliminare la necessità di filtro manuale dei risultati.
{: shortdesc}

Il pacchetto {{site.data.keyword.discoveryshort}} contiene le seguenti entità. Puoi trovare maggiori dettagli nella guida di riferimento API {{site.data.keyword.discoveryshort}} facendo clic sul nome dell'entità.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| [`discovery-v1`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html){: external} | Pacchetto | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`  | Lavorare con il servizio {{site.data.keyword.discoveryshort}}. |
| [`create-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-environment){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `description`, `size` | Creare un ambiente. |
| [`delete-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-environment){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id` | Eliminare un ambiente. |
| [`get-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-environment){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`  | Ottenere informazioni su un ambiente. |
| [`list-environments`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-environments){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`  | Elencare gli ambienti. |
| [`list-fields`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-fields){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_ids` | Elencare i campi nell'ambito delle raccolte. |
| [`update-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-environment){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `name`, `description` | Aggiornare un ambiente. |
| [`create-configuration`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-configuration){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `name`, `description`, `conversions`, `enrichments`, `normalizations`  | Aggiungere una configurazione. |
| [`delete-configuration`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-configuration){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `configuration_id` | Eliminare una configurazione. |
| [`get-configuration`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-configuration){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `configuration_id` | Ottenere i dettagli della configurazione. |
| [`list-configurations`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-configurations){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `name` | Elencare le configurazioni. |
| [`update-configuration`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-configuration){: external} | Azione |  `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `configuration_id`, `name`, `description`, `conversions`, `enrichments`, `normalizations` | Aggiornare una configurazione. |
| [`test-configuration-in-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#test-configuration-in-environment){: external} | Azione |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `configuration`,     `step`, `configuration_id`, `file`, `metadata`, `file_content_type` | Testare una configurazione. |
| [`create-collection`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-collection){: external} | Azione |  `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `name`, `description`, `configuration_id`, `language` | Creare una raccolta. |
| [`delete-collection`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-collection){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Eliminare una raccolta. |
| [`get-collection`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-collection){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Ottenere i dettagli della raccolta. |
| [`list-collection-fields`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collection-fields){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Elencare i campi della raccolta. |
| [`list-collections`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collections){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `name` | Elencare le raccolte. |
| [`update-collection`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-collection){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `name`, `description`, `configuration_id` | Aggiornare una raccolta. |
| [`create-expansions`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-expansions){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, ``collection_id`, `expansions` | Creare o aggiornare l'elenco di espansione. |
| [`delete-expansions`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-expansions){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Eliminare l'elenco di espansione. |
| [`list-expansions`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-expansions){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Ottenere l'elenco di espansione. |
| [`add-document`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-document){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `file`, `metadata`, `file_content_type` | Aggiungere un documento. |
| [`delete-document`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-document){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `document_id` | Eliminare un documento. |
| [`get-document-status`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-document-status){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `document_id` | Ottenere i dettagli del documento. |
| [`update-document`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-document){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `document_id`, `file`,     `metadata`, `file_content_type` | Aggiornare un documento. |
| [`federated-query`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_ids`, `filter`, `query`,     `natural_language_query`, `aggregation`, `count`, `return_fields`, `offset`, `sort`, `highlight`, `deduplicate`, `deduplicate_field`, `similar`, `similar_document_ids`, `similar_fields` | Eseguire query dei documenti in più raccolte. |
| [`federated-query-notices`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query-notices){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_ids`, `filter`, `query`, `natural_language_query`, `aggregation`, `count`, `return_fields`, `offset`, `sort`, `highlight`, `deduplicate_field`, `similar`, `similar_document_ids`, `similar_fields` | Eseguire query di più avvisi di sistema di raccolta. |
| [`query`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `filter`, `query`, `natural_language_query`, `passages`,     `aggregation`, `count`, `return_fields`, `offset`, `sort`, `highlight`, `passages_fields`, `passages_count`, `passages_characters`, `deduplicate`, `deduplicate_field`, `similar`, `similar_document_ids`, `similar_fields` | Eseguire una query della tua raccolta. |
| [`query-entities`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-entities){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `feature`, `entity`, `context`, `count`, `evidence_count` | Eseguire una query di un'entità Knowledge Graph. |
| [`query-notices`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-notices){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `filter`, `query`, `natural_language_query`, `passages`, `aggregation`, `count`, `return_fields`, `offset`, `sort`, `highlight`, `passages_fields`, `passages_count`, `passages_characters`, `deduplicate_field`, `similar`, `similar_document_ids`, `similar_fields`  | Eseguire query degli avvisi di sistema. |
| [`query-relations`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-relations){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `entities`, `context`, `sort`, `filter`, `count`, `evidence_count` | Eseguire una query di una relazione Knowledge Graph. |
| [`add-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-training-data){: external} | Azione |  `username`, `password`,  `iam_access_token`, `iam_apikey, iam_url`,  `headers, headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `natural_language_query`, `filter`, `examples`, | Aggiungere una query ai dati di formazione. |
| [`create-training-example`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-training-example){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id`, `document_id`, `cross_reference`, `relevance` | Aggiungere un esempio alla query dei dati di formazione. |
| [`delete-all-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-all-training-data){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Eliminare tutti i dati di formazione. |
| [`delete-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-data){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id` | Eliminare una query dei dati di formazione. |
| [`delete-training-example`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-example){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id`, `example_id` | Eliminare un esempio per la query dei dati di formazione. |
| [`get-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-data){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id` | Ottenere i dettagli su una query. |
| [`get-training-example`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-example){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id`, `example_id` | Ottenere i dettagli per un esempio di dati di formazione. |
| [`list-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-data){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id` | Elencare i dati di formazione. |
| [`list-training-examples`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-examples){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id` | Elencare gli esempi per una query dei dati di formazione. |
| [`update-training-example`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-training-example){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `environment_id`, `collection_id`, `query_id`, `example_id`, `cross_reference`, `relevance` | Modificare un'etichetta o un riferimento incrociato per un esempio. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-user-data){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id` | Eliminare i dati etichettati. |

## Creazione di un'istanza del servizio {{site.data.keyword.discoveryshort}}
{: #service_instance_discovery}

Prima di installare il pacchetto, devi creare un'istanza del servizio {{site.data.keyword.discoveryshort}} e le credenziali del servizio.

1. [Crea un'istanza del servizio {{site.data.keyword.discoveryshort}}](https://cloud.ibm.com/catalog/services/discovery){: external}.
2. Quando l'istanza del servizio viene creata, vengono anche create per tuo conto le credenziali del servizio generate automaticamente.

## Installazione del pacchetto {{site.data.keyword.discoveryshort}}
{: #install_discovery}

Una volta che disponi di un'istanza del servizio {{site.data.keyword.discoveryshort}}, utilizza la CLI di {{site.data.keyword.openwhisk}} per installare il pacchetto {{site.data.keyword.discoveryshort}} nel tuo spazio dei nomi.
{: shortdesc}

### Installazione dalla CLI {{site.data.keyword.openwhisk_short}}
{: #discovery_cli}

**Prima di iniziare**
[Installa il plug-in {{site.data.keyword.openwhisk_short}} per la CLI {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Per installare il pacchetto {{site.data.keyword.discoveryshort}}, immetti i seguenti comandi:

1. Clona il repository del pacchetto {{site.data.keyword.discoveryshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Distribuisci il pacchetto.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/discovery-v1/manifest.yaml
    ```
    {: pre}

3. Verifica che il pacchetto venga aggiunto al tuo elenco pacchetti.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Output**
    ```
    packages
    /myOrg_mySpace/discovery-v1                        private
    ```
    {: screen}

4. Esegui il bind delle credenziali dall'istanza {{site.data.keyword.discoveryshort}} che hai creato al pacchetto.
    ```
    ibmcloud fn service bind discovery discovery-v1
    ```
    {: pre}

    **Output di esempio**
      ```
    Credentials 'Credentials-1' from 'discovery' service instance 'Watson Discovery' bound to 'discovery-v1'.
    ```
    {: screen}

5. Verifica che il pacchetto sia configurato con le tue credenziali dell'istanza del servizio {{site.data.keyword.discoveryshort}}.
    ```
    ibmcloud fn package get discovery-v1 parameters
    ```
    {: pre}

    **Output di esempio**
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

### Installazione dall'IU {{site.data.keyword.openwhisk_short}}
{: #discovery_ui}

1. Nella console {{site.data.keyword.openwhisk_short}}, vai alla [pagina di creazione](https://cloud.ibm.com/openwhisk/create){: external}.

2. Seleziona lo spazio dei nomi in cui vuoi installare il pacchetto utilizzando il menu a discesa dello spazio dei nomi.

3. Fai clic su **Installa pacchetti**.

4. Fai clic sul gruppo di pacchetti **Watson**.

5. Fai clic sul pacchetto **Discovery**.

6. Fai clic su **Installa**.

7. Dopo aver installato il pacchetto, vieni reindirizzato alla pagina Azioni e puoi cercare il tuo nuovo pacchetto, che è denominato **discovery-v1**.

8. Per utilizzare le azioni nel pacchetto `discovery-v1`, devi eseguire il bind delle credenziali del servizio alle azioni.
  * Per eseguire il bind delle credenziali del servizio a tutte le azioni nel pacchetto, completa il passo 4 nelle istruzioni della CLI.
  * Per eseguire il bind delle credenziali del servizio a singole azioni, completa la seguente procedura nell'IU.

  Devi completare la seguente procedura per ogni azione che vuoi utilizzare.
  {: note}

    1. Fai clic su un'azione dal pacchetto `discovery-v1` che vuoi utilizzare. Viene aperta la pagina dei dettagli per tale azione.
    2. Nella navigazione sulla sinistra, fai clic sulla sezione **Parametri**.
    3. Immetti un nuovo **parametro**. Per la chiave, immetti `__bx_creds`. Per il valore, incolla l'oggetto JSON delle credenziali del servizio dall'istanza del servizio che hai creato in precedenza.

## Utilizzo del pacchetto {{site.data.keyword.discoveryshort}}
{: #usage_discovery}

Per utilizzare le azioni in questo pacchetto, esegui i comandi nel seguente formato:

```
ibmcloud fn action invoke discovery-v1/<nome_azione> -b -p <param name> <param>
```
{: pre}

Tutte le azioni richiedono un parametro di versione nel formato AAAA-MM-GG. Quando l'API viene modificata in un modo non compatibile con le versioni precedenti, viene rilasciata una nuova data di versione. Ulteriori dettagli sono disponibili nella [guida di riferimento API](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html#versioning){: external}.

Le funzioni di questo pacchetto utilizzano la versione corrente di Discovery, `2018-03-05`. Prova l'azione `list-environments`.
```
ibmcloud fn action invoke discovery-v1/list-environments -b -p version 2018-03-05
```
{: pre}



