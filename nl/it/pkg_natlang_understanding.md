---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: natural language, understanding, watson knowledge studio, functions

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


# {{site.data.keyword.nlushort}}
{: #pkg_natlang_understanding}

Il servizio {{site.data.keyword.nlufull}} installabile ti aiuta ad analizzare varie funzioni di contenuto testuale in una certa scala.
{: shortdesc}

Fornisci testo, HTML non elaborato oppure un URL pubblico e {{site.data.keyword.nlushort}} ti dà i risultati per le caratteristiche da te richieste. Il servizio ripulisce il contenuto HTML prima dell'analisi per impostazione predefinita, per cui i risultati possono ignorare la maggior parte degli annunci pubblicitari e altri contenuti indesiderati. Puoi creare dei <a target="_blank" href="https://www.ibm.com/watson/developercloud/doc/natural-language-understanding/customizing.html">modelli personalizzati</a> con Watson Knowledge Studio che possono essere utilizzati per rilevare relazioni ed entità personalizzate in Natural Language Understanding.

Il pacchetto {{site.data.keyword.nlushort}} contiene le seguenti entità. Per ulteriori informazioni, consulta la guida di riferimento API {{site.data.keyword.nlushort}} facendo clic sul nome dell'entità.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| [`natural-language-understanding-v1`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl.html){: external} | Pacchetto | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,  |  Lavorare con il servizio {{site.data.keyword.nlushort}}. |
| [`analyze`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#analyze){: external} | Azione |  `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `features`, `text`, `html`, `url`, `clean`, `xpath`, `fallback_to_raw`, `return_analyzed_text`, `language`, `limit_text_characters`,  | Analizzare testo, HTML o una pagina web pubblica. |
| [`delete-model`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#delete-model){: external} | Azione |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id`,  | Eliminare il modello. |
| [`list-models`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#list-models){: external} | Azione |  `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, | Elencare i modelli. |

## Creazione di un'istanza del servizio {{site.data.keyword.nlushort}}
{: #service_instance_understanding}

Prima di installare il pacchetto, devi creare un'istanza del servizio {{site.data.keyword.nlushort}} e le credenziali del servizio.
{: shortdesc}

1. [Crea un'istanza del servizio {{site.data.keyword.nlushort}}](https://cloud.ibm.com/catalog/services/natural-language-understanding){: external}.
2. Quando l'istanza del servizio viene creata, vengono anche create per tuo conto le credenziali del servizio generate automaticamente.

## Installazione del pacchetto {{site.data.keyword.nlushort}}
{: #install_understanding}

Una volta che disponi di un'istanza del servizio {{site.data.keyword.nlushort}}, utilizza la CLI di {{site.data.keyword.openwhisk}} per installare il pacchetto {{site.data.keyword.nlushort}} nel tuo spazio dei nomi.
{: shortdesc}

### Installazione dalla CLI {{site.data.keyword.openwhisk_short}}
{: #nlus_cli}

**Prima di iniziare**
[Installa il plug-in {{site.data.keyword.openwhisk_short}} per la CLI {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Per installare il pacchetto {{site.data.keyword.nlushort}}:

1. Clona il repository del pacchetto {{site.data.keyword.nlushort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Distribuisci il pacchetto.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/natural-language-understanding-v1/manifest.yaml
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
    /myOrg_mySpace/natural-language-understanding-v1                        private
    ```
    {: screen}

4. Esegui il bind delle credenziali dall'istanza {{site.data.keyword.nlushort}} che hai creato al pacchetto.
    ```
    ibmcloud fn service bind natural-language-understanding natural-language-understanding-v1
    ```
    {: pre}

    **Output di esempio**
      ```
    Credentials 'Credentials-1' from 'natural-language-understanding' service instance 'Watson Natural Language Understanding' bound to 'natural-language-understanding-v1'.
    ```
    {: screen}

5. Verifica che il pacchetto sia configurato con le tue credenziali dell'istanza del servizio {{site.data.keyword.nlushort}}.
    ```
    ibmcloud fn package get natural-language-understanding-v1 parameters
    ```
    {: pre}

    **Output di esempio**
      ```
    ok: got package natural-language-understanding-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "natural-language-understanding": {
            "credentials": "Credentials-1",
            "instance": "Watson Natural Language Understanding",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/natural-language-understanding/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Installazione dall'IU {{site.data.keyword.openwhisk_short}}
{: #nlus_ui}

1. Nella console {{site.data.keyword.openwhisk_short}}, vai alla [pagina di creazione](https://cloud.ibm.com/openwhisk/create){: external}.

2. Seleziona lo spazio dei nomi in cui vuoi installare il pacchetto utilizzando il menu a discesa dello spazio dei nomi. Gli spazi dei nomi sono formati dai nomi di organizzazione e spazio combinati.

3. Fai clic su **Installa pacchetti**.

4. Fai clic sul gruppo di pacchetti **Watson**.

5. Fai clic sul pacchetto **Natural Language Understanding**.

5. Fai clic su **Installa**.

6. Dopo aver installato il pacchetto, vieni reindirizzato alla pagina Azioni e puoi cercare il tuo nuovo pacchetto, che è denominato **natural-language-understanding-v1**.

7. Per utilizzare le azioni nel pacchetto **natural-language-understanding-v1**, devi eseguire il bind delle credenziali del servizio alle azioni.
  * Per eseguire il bind delle credenziali del servizio a tutte le azioni nel pacchetto, segui i passi 4 e 5 nelle [istruzioni della CLI](#nlus_cli).
  * Per eseguire il bind delle credenziali del servizio a singole azioni, completa la seguente procedura nell'IU. 

  Devi completare la seguente procedura per ogni azione che vuoi utilizzare.
  {: note}

    1. Fai clic su un'azione dal pacchetto **natural-language-understanding-v1** che vuoi utilizzare. Viene aperta la pagina dei dettagli per tale azione.
    2. Nella navigazione sulla sinistra, fai clic sulla sezione **Parametri**.
    3. Immetti un nuovo **parametro**. Per la chiave, immetti `__bx_creds`. Per il valore, incolla l'oggetto JSON delle credenziali del servizio dall'istanza del servizio che hai creato in precedenza.

## Utilizzo del pacchetto {{site.data.keyword.nlushort}}
{: #usage_understanding}

Per utilizzare le azioni in questo pacchetto, esegui i comandi nel seguente formato:

```
ibmcloud fn action invoke natural-language-understanding-v1/<nome_azione> -b -p <param name> <param>
```
{: pre}

Tutte le azioni richiedono un parametro di versione nel formato AAAA-MM-GG. Quando l'API viene modificata in un modo non compatibile con le versioni precedenti, viene rilasciata una nuova data di versione. Ulteriori dettagli sono disponibili nella [guida di riferimento API](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/#versioning){: external}.

Le funzioni di questo pacchetto utilizzano la versione corrente di Natural Language Understanding, 16-03-2018. Prova l'azione `list-models`.
```
ibmcloud fn action invoke natural-language-understanding-v1/list-models -b -p version 2018-03-16
```
{: pre}



