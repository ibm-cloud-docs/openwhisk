---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: package, cognitive, functions

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


# {{site.data.keyword.speechtotextshort}}
{: #pkg_speech_to_text}

## Pacchetti

| Nome | Disponibilità | Descrizione |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](#speech_to_text) | Installabile | Funziona con il servizio {{site.data.keyword.speechtotextshort}} V1. |
| [`/whisk.system/watson-speechToText`](#preinstall_speechtotext) | Preinstallato (non disponibile a Tokyo) | Richiamare le API Watson per la conversione da voce a testo |

## Servizio {{site.data.keyword.speechtotextshort}}
{: #speech_to_text}

Il servizio {{site.data.keyword.speechtotextfull}} installabile fornisce un'[API](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html){: external} che utilizza le funzionalità di riconoscimento vocale di IBM per produrre trascrizioni di audio parlato.
{:shortdesc}

Il servizio può trascrivere il parlato da diverse lingue e formati audio. Oltre alla trascrizione di base, il servizio può produrre informazioni dettagliate su molti aspetti dell'audio. Per la maggior parte delle lingue, il servizio supporta due frequenze di campionamento: banda larga e banda stretta. Restituisce tutto il contenuto della risposta JSON nella serie di caratteri UTF-8. Per ulteriori informazioni sul servizio, vedi la [documentazione di IBM&reg; Cloud](/docs/services/speech-to-text?topic=speech-to-text-about).

Il pacchetto {{site.data.keyword.speechtotextshort}} contiene le seguenti entità. Puoi trovare ulteriori informazioni nella guida di riferimento API {{site.data.keyword.speechtotextshort}} facendo clic sul nome dell'entità.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html){: external} | Pacchetto | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Lavorare con il servizio {{site.data.keyword.speechtotextshort}} V1. |
| [`get-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id` | Ottenere un modello. |
| [`list-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-models){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Elencare i modelli. |
| [`recognize-sessionless`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#recognize-sessionless){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `audio`, `content_type`, `model`, `customization_id`, `acoustic_customization_id`, `base_model_version`, `customization_weight`, `inactivity_timeout`, `keywords`, `keywords_threshold`, `max_alternatives`, `word_alternatives_threshold`, `word_confidence`, `timestamps`, `profanity_filter`, `smart_formatting`, `speaker_labels` | Riconoscere l'audio (senza sessione). |
| [`check-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-job){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `id`  | Controllare un lavoro. |
| [`check-jobs`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-jobs){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Controllare i lavori. |
| [`create-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-job){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `audio`, `content_type`, `model`, `callback_url`, `events`, `user_token`, `results_ttl`, `customization_id`, `acoustic_customization_id`, `base_model_version`, `customization_weight`, `inactivity_timeout`, `keywords`, `keywords_threshold`, `max_alternatives`, `word_alternatives_threshold`, `word_confidence`, `timestamps`, `profanity_filter`, `smart_formatting`, `speaker_labels` | Creare un lavoro. |
| [`delete-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-job){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `id` | Eliminare un lavoro. |
| [`register-callback`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#register-callback){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `callback_url`, `user_secret` | Registrare un callback. |
| [`unregister-callback`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#unregister-callback){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `callback_url` | Annullare la registrazione di un callback. |
| [`create-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-language-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `base_model_name`, `dialect`, `description` | Creare un modello di lingua personalizzato. |
| [`delete-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-language-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Eliminare un modello di lingua personalizzato. |
| [`get-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-language-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Ottenere un modello di lingua personalizzato. |
| [`list-language-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-language-models){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` | Elencare i modelli di lingua personalizzati. |
| [`reset-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-language-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Reimpostare un modello di lingua personalizzato. |
| [`train-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-language-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_type_to_add`, `customization_weight` | Formare un modello di lingua personalizzato. |
| [`upgrade-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-language-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Eseguire un upgrade di un modello di lingua personalizzato. |
| [`add-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-corpus){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name`, `corpus_file`, `allow_overwrite` | Aggiungere un corpus. |
| [`delete-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-corpus){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name` | Eliminare un corpus. |
| [`get-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-corpus){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name` | Ottenere un corpus. |
| [`list-corpora`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-corpora){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Elencare i corpus. |
| [`add-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-word){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name`, `word`, `sounds_like`, `display_as` | Aggiungere una parola personalizzata. |
| [`add-words`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-words){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `words` | Aggiungere parole personalizzate. |
| [`delete-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-word){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name` | Eliminare una parola personalizzata. |
| [`get-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-word){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name` | Ottenere una parola personalizzata. |
| [`list-words`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-words){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_type`, `sort` | Elencare le parole personalizzate. |
| [`create-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-acoustic-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `base_model_name`,` description` | Creare un modello acustico personalizzato. |
| [`delete-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-acoustic-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Eliminare un modello acustico personalizzato. |
| [`get-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-acoustic-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Ottenere un modello acustico personalizzato. |
| [`list-acoustic-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-acoustic-models){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` | Elencare i modelli acustici personalizzati. |
| [`reset-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-acoustic-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Reimpostare un modello acustico personalizzato. |
| [`train-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-acoustic-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `custom_language_model_id` | Formare un modello acustico personalizzato. |
| [`upgrade-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-acoustic-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `custom_language_model_id` | Eseguire un upgrade di un modello acustico personalizzato. |
| [`add-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-audio){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name`, `audio_resource`, `content_type`, `contained_content_type`, `allow_overwrite` | Aggiungere una risorsa audio. |
| [`delete-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-audio){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name` | Eliminare una risorsa audio. |
| [`get-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-audio){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name` | Ottenere una risorsa audio. |
| [`list-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-audio){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Elencare le risorse audio. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-user-data){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id` | Eliminare i dati etichettati. |

### Creazione di un'istanza del servizio {{site.data.keyword.speechtotextshort}}
{: #service_instance_speechtotext}

Prima di installare il pacchetto, devi creare un'istanza del servizio {{site.data.keyword.speechtotextshort}} e le credenziali del servizio.
{: shortdesc}

1. [Crea un'istanza del servizio {{site.data.keyword.speechtotextshort}}](https://cloud.ibm.com/catalog/services/speech_to_text){: external}.
2. Quando l'istanza del servizio viene creata, vengono anche create per tuo conto le credenziali del servizio generate automaticamente.

### Installazione del pacchetto {{site.data.keyword.speechtotextshort}}
{: #install_speechtotext}

Una volta che disponi di un'istanza del servizio {{site.data.keyword.speechtotextshort}}, utilizza la CLI {{site.data.keyword.openwhisk}} per installare il pacchetto {{site.data.keyword.speechtotextshort}} nel tuo spazio dei nomi.
{: shortdesc}

### Installazione dalla CLI {{site.data.keyword.openwhisk_short}}
{: #speechtotext_cli}

**Prima di iniziare**

[Installa il plug-in {{site.data.keyword.openwhisk_short}} per la CLI {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Per installare il pacchetto {{site.data.keyword.speechtotextshort}}:

1. Clona il repository del pacchetto {{site.data.keyword.speechtotextshort}}.

    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Distribuisci il pacchetto.

    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/speech-to-text-v1/manifest.yaml
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
    /myOrg_mySpace/speech-to-text-v1                        private
    ```
    {: screen}

4. Esegui il bind delle credenziali dall'istanza {{site.data.keyword.speechtotextshort}} che hai creato al pacchetto.

    ```
    ibmcloud fn service bind speech_to_text speech-to-text-v1
    ```
    {: pre}

    A seconda della regione dove l'hai creata, l'istanza del servizio potrebbe essere denominata in modo diverso perché è un servizio IAM. Se il comando non riesce, utilizza il seguente nome di servizio per il comando bind:
    ```
    ibmcloud fn service bind speech-to-text speech-to-text-v1
    ```
    {: pre}

    **Output di esempio**

    ```
    Credentials 'Credentials-1' from 'speech_to_text' service instance 'Watson Speech to Text' bound to 'speech-to-text-v1'.
    ```
    {: screen}

5. Verifica che il pacchetto sia configurato con le tue credenziali dell'istanza del servizio {{site.data.keyword.speechtotextshort}}.

    ```
    ibmcloud fn package get speech-to-text-v1 parameters
    ```
    {: pre}

    **Output di esempio**

    ```
    ok: got package speech-to-text-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "speech_to_text": {
            "credentials": "Credentials-1",
            "instance": "Watson Speech to Text",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/speech_to_text/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Installazione dall'IU {{site.data.keyword.openwhisk_short}}
{: #speechtotext_ui}

1. Nella console {{site.data.keyword.openwhisk_short}}, vai alla [pagina di creazione](https://cloud.ibm.com/openwhisk/create){: external}.

2. Seleziona lo spazio dei nomi in cui vuoi installare il pacchetto utilizzando il menu a discesa dello spazio dei nomi.

3. Fai clic su **Installa pacchetti**.

4. Fai clic sul gruppo di pacchetti **Watson**.

5. Fai clic sul pacchetto **Speech To Text**.

5. Fai clic su **Installa**.

6. Una volta installato il pacchetto, vieni reindirizzato alla pagina Azioni e puoi cercare il tuo nuovo pacchetto, che è denominato `speech-to-text-v1`.

7. Per utilizzare le azioni nel pacchetto **speech-to-text-v1**, devi eseguire il bind delle credenziali del servizio alle azioni.
  * Per eseguire il bind delle credenziali del servizio a tutte le azioni nel pacchetto, segui i passi 4 e 5 nelle [istruzioni della CLI](#speechtotext_cli).
  * Per eseguire il bind delle credenziali del servizio a singole azioni, completa la seguente procedura nell'IU. 
  
  Devi completare la seguente procedura per ogni azione che vuoi utilizzare.
  {: note}

    1. Fai clic su un'azione dal pacchetto **speech-to-text-v1** che vuoi utilizzare. Viene aperta la pagina dei dettagli per tale azione.
    2. Nella navigazione sulla sinistra, fai clic sulla sezione **Parametri**.
    3. Immetti un nuovo **parametro**. Per la chiave, immetti `__bx_creds`. Per il valore, incolla l'oggetto JSON delle credenziali del servizio dall'istanza del servizio che hai creato in precedenza.

### Utilizzo del pacchetto {{site.data.keyword.speechtotextshort}}
{: #usage_speechtotext}

Per utilizzare le azioni in questo pacchetto, esegui i comandi nel seguente formato:

```
ibmcloud fn action invoke speech-to-text-v1/<nome_azione> -b -p <param name> <param>
```
{: pre}

Prova l'azione `list-models`.
```
ibmcloud fn action invoke speech-to-text-v1/list-models -b
```
{: pre}

## Watson: Speech to Text
{: #preinstall_speechtotext}

Questo pacchetto pre-installato non è disponibile nella regione Tokyo. Vedi il pacchetto [Speech to Text](#install_speechtotext) installabile.
{: tip}

Il pacchetto `/whisk.system/watson-speechToText` offre una soluzione pratica per richiamare le diverse API Watson per la conversione da voce a testo.
{: shortdesc}

Il pacchetto include le seguenti azioni.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/watson-speechToText` | Pacchetto | `username`, `password` | Pacchetto per convertire la voce in testo. |
| `/whisk.system/watson-speechToText/speechToText` | Azione | `payload`, `content_type`, `encoding`, `username`, `password`, `continuous`, `inactivity_timeout`, `interim_results`, `keywords`, `keywords_threshold`, `max_alternatives`, `model`, `timestamps`, `watson-token`, `word_alternatives_threshold`, `word_confidence`, `X-Watson-Learning-Opt-Out` | Convertire l'audio in testo. |

Il pacchetto `/whisk.system/watson` è obsoleto, inclusa l'azione `/whisk.system/watson/speechToText`. Visualizza invece il [pacchetto {{site.data.keyword.speechtotextshort}}](#setting-up-the-watson-speech-to-text-package-in-ibm-cloud).
{: deprecated}

### Configurazione del pacchetto Watson Speech to Text in {{site.data.keyword.cloud_notm}}

Se utilizzi {{site.data.keyword.openwhisk}} da {{site.data.keyword.cloud_notm}}, vengono creati automaticamente i bind di pacchetto per le tue istanze del servizio {{site.data.keyword.cloud_notm}} Watson.

1. Crea un'istanza del servizio Watson Speech to Text nel tuo [dashboard](https://cloud.ibm.com){: external} {{site.data.keyword.cloud_notm}}. Assicurati di ricordare il nome dell'istanza del servizio e dell'organizzazione e dello spazio {{site.data.keyword.cloud_notm}} in cui ti trovi.

2. Aggiorna i pacchetti nel tuo spazio dei nomi. L'aggiornamento crea automaticamente un bind di pacchetto per l'istanza del servizio Watson da te creata.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **Output di esempio**

  ```
  created bindings:
  Watson_SpeechToText_Credentials-1
  ```
  {: screen}

3. Elenca i pacchetti per verificare che il bind sia stato creato.

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Output di esempio**

  ```
  packages
  /myOrg_mySpace/Watson_SpeechToText_Credentials-1 private
  ```
  {: screen}

### Configurazione di un pacchetto Watson Speech to Text all'esterno di {{site.data.keyword.cloud_notm}}

Se non utilizzi {{site.data.keyword.openwhisk_short}} in {{site.data.keyword.cloud_notm}} o se vuoi configurare Watson Speech to Text all'esterno di {{site.data.keyword.cloud_notm}}, devi creare manualmente un bind di pacchetto per il tuo servizio Watson Speech to Text. Ti servono il nome utente e la password del servizio Watson Speech to Text.

Crea un bind di pacchetto configurato per il tuo servizio Watson Speech to Text.
```
ibmcloud fn package bind /whisk.system/watson-speechToText myWatsonSpeechToText -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

### Conversione da voce a testo

L'azione `/whisk.system/watson-speechToText/speechToText` converte l'audio in testo. Sono supportati i seguenti parametri.

| Parametri | Descrizione |
| --- | --- |
| `username` | Il nome utente dell'API Watson.|
| `password` | La password dell'API Watson. |
| `payload` | I dati binari del discorso codificato da trasformare in testo. |
| `content_type` | Il tipo MIME dell'audio. |
| `encoding` | La codifica dei dati binari del discorso. |
| `continuous` | Indica se vengono restituiti più risultati finali che rappresentano frasi consecutive separate da pause lunghe. |
| `inactivity_timeout` | Il tempo, in secondi, dopo il quale viene chiusa la connessione, se nell'audio inviato viene rilevato solo silenzio. |
| `interim_results` | Indica se il servizio deve restituire risultati provvisori. |
| `keywords` | Un elenco di parole chiave da individuare nell'audio. |
| `keywords_threshold` | Un valore di attendibilità che è il limite inferiore per individuare una parola chiave. |
| `max_alternatives` | Il numero massimo di trascrizioni alternative da restituire. |
| `model` | L'identificativo del modello da utilizzare per la richiesta di riconoscimento. |
| `timestamps` | Indica se viene restituito l'allineamento temporale per ogni parola. |
| `watson-token` | Fornisce un token di autenticazione per il servizio come alternativa all'immissione delle credenziali del servizio. |
| `word_alternatives_threshold` | Un valore di attendibilità che è il limite inferiore per identificare un'ipotesi come una possibile parola alternativa. |
| `word_confidence` | Indica se per ogni parola deve essere restituita una misura di attendibilità compresa nell'intervallo da 0 a 1. |
| `X-Watson-Learning-Opt-Out` | Indica se annullare la selezione della raccolta dati per la chiamata. |

Richiama l'azione `speechToText` nel tuo bind di pacchetto per convertire l'audio codificato.

```
ibmcloud fn action invoke myWatsonSpeechToText/speechToText --blocking --result --param payload <base64 encoding of a .wav file> --param content_type 'audio/wav' --param encoding 'base64'
```
{: pre}

**Output di esempio**
```
{
  "data": "Hello Watson"
  }
```
{: screen}


