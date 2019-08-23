---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: watson, translator, cognitive, translating text, language, functions

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


# Traduttore

## Pacchetto {{site.data.keyword.languagetranslatorshort}}

{: #pkg_translator}

Il pacchetto {{site.data.keyword.languagetranslatorfull}} installabile traduce il testo da una lingua a un'altra. Il servizio offre più modelli di traduzione forniti da IBM che puoi personalizzare in base alla tua terminologia univoca e alla tua lingua.
{: shortdesc}

Il pacchetto {{site.data.keyword.languagetranslatorshort}} contiene le seguenti entità. Puoi trovare maggiori dettagli nella guida di riferimento API {{site.data.keyword.languagetranslatorshort}} facendo clic sul nome dell'entità.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| [`language-translator-v3`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html){: external} | Pacchetto | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Lavorare con il servizio {{site.data.keyword.languagetranslatorshort}}. |
| [`translate`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#translate){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `model_id`, `source`, `target` | Tradurre il testo. |
| [`identify`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#identify){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text` | Identificare la lingua del testo. |
| [`list-identifiable-languages`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-identifiable-languages){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Elencare le lingue identificabili. |
| [`create-model`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#create-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `base_model_id`, `name`, `forced_glossary`, `parallel_corpus` | Creare un modello. |
| [`delete-model`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#delete-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id` | Eliminare un modello. |
| [`get-model`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#get-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id` | Ottenere i dettagli del modello. |
| [`list-models`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-models){: external} | Azione | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `source`, `target`, `default_models` | Elencare i modelli. |

## Creazione di un'istanza del servizio {{site.data.keyword.languagetranslatorshort}}
{: #service_instance_translator}

Prima di installare il pacchetto, devi creare un'istanza del servizio {{site.data.keyword.languagetranslatorshort}} e le credenziali del servizio.
{: shortdesc}

1. [Crea un'istanza del servizio {{site.data.keyword.languagetranslatorshort}}](https://cloud.ibm.com/catalog/services/language_translator){: external}.
2. Quando l'istanza del servizio viene creata, vengono anche create per tuo conto le credenziali del servizio generate automaticamente.

## Installazione del pacchetto {{site.data.keyword.languagetranslatorshort}}
{: #install_translator}

Una volta che disponi di un'istanza del servizio {{site.data.keyword.languagetranslatorshort}}, utilizza la CLI di {{site.data.keyword.openwhisk}} per installare il pacchetto {{site.data.keyword.languagetranslatorshort}} nel tuo spazio dei nomi.
{: shortdesc}

### Installazione dalla CLI {{site.data.keyword.openwhisk_short}}
{: #languagetranslator_cli}

**Prima di iniziare**

[Installa il plug-in {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-cli_install) per la CLI {{site.data.keyword.cloud_notm}}.

Installa il pacchetto {{site.data.keyword.languagetranslatorshort}}.

1. Clona il repository del pacchetto {{site.data.keyword.languagetranslatorshort}}.

    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Distribuisci il pacchetto.

    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/language-translator-v3/manifest.yaml
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
    /myOrg_mySpace/language-translator-v3                        private
    ```
    {: screen}

4. Esegui il bind delle credenziali dall'istanza {{site.data.keyword.languagetranslatorshort}} che hai creato al pacchetto.

    ```
    ibmcloud fn service bind language-translator language-translator-v3
    ```
    {: pre}

    **Output di esempio**

    ```
    Credentials 'Credentials-1' from 'language-translator' service instance 'Watson Language Translator' bound to 'language-translator-v3'.
    ```
    {: screen}

5. Verifica che il pacchetto sia configurato con le tue credenziali dell'istanza del servizio {{site.data.keyword.languagetranslatorshort}}.

    ```
    ibmcloud fn package get language-translator-v3 parameters
    ```
    {: pre}

    **Output di esempio**

    ```
    ok: got package language-translator-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "language-translator": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:ibmcloud:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:ibmcloud:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
            "instance": "Language Translator-g2",
            "url": "https://gateway.watsonplatform.net/language-translator/api"
          }
        }
      }
    ]
    ```
    {: screen}

### Installazione dall'IU {{site.data.keyword.openwhisk_short}}
{: #languagetranslator_ui}

1. Nella console {{site.data.keyword.openwhisk_short}}, vai alla [pagina di creazione](https://cloud.ibm.com/openwhisk/create){: external}.

2. Seleziona lo spazio dei nomi in cui vuoi installare il pacchetto utilizzando il menu a discesa dello spazio dei nomi. Gli spazi dei nomi sono formati dai nomi di organizzazione e spazio combinati.

3. Fai clic su **Installa pacchetti**.

4. Fai clic sul gruppo di pacchetti **Watson**.

5. Fai clic sul pacchetto **Language Translator**.

6. Fai clic su **Installa**.

7. Una volta installato il pacchetto, vieni reindirizzato alla pagina Azioni e puoi cercare il tuo nuovo pacchetto, che è denominato `language-translator-v3`.

8. Per utilizzare le azioni nel pacchetto `language-translator-v3`, devi eseguire il bind delle credenziali del servizio alle azioni.
  * Per eseguire il bind delle credenziali del servizio a tutte le azioni nel pacchetto, segui i passi [5 e 6 nelle istruzioni della CLI](#languagetranslator_cli).
  * Per eseguire il bind delle credenziali del servizio a singole azioni, completa la seguente procedura nell'IU.
  
  Devi completare la seguente procedura per ogni azione che vuoi utilizzare.
  {: note}

    1. Fai clic su un'azione dal pacchetto `language-translator-v3` che vuoi utilizzare. Viene aperta la pagina dei dettagli per tale azione.
    2. Nella navigazione sulla sinistra, fai clic sulla sezione **Parametri**.
    3. Immetti un nuovo `parametro`. Per la chiave, immetti `__bx_creds`. Per il valore, incolla l'oggetto JSON delle credenziali del servizio dall'istanza del servizio che hai creato in precedenza.

## Utilizzo del pacchetto {{site.data.keyword.languagetranslatorshort}}
{: #usage_translator}

Per utilizzare le azioni in questo pacchetto, esegui i comandi nel seguente formato:

```
ibmcloud fn action invoke language-translator-v3/<nome_azione> -b -p <param name> <param>
```
{: pre}

Tutte le azioni richiedono un parametro di versione nel formato AAAA-MM-GG. Quando l'API viene modificata in un modo non compatibile con le versioni precedenti, viene rilasciata una nuova data di versione. Ulteriori dettagli sono disponibili nella [guida di riferimento API](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#versioning){: external}.

Le funzioni di questo pacchetto utilizzano la versione corrente di Language Translator, `2018-05-01`. Prova l'azione `identify`.
```
ibmcloud fn action invoke language-translator-v3/identify -b -p version 2018-05-01 -p text hola
```
{: pre}




