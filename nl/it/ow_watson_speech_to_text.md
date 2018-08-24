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

# Pacchetto {{site.data.keyword.speechtotextshort}}

Il servizio {{site.data.keyword.speechtotextfull}} fornisce un'API che utilizza le funzionalità di riconoscimento vocale di IBM per produrre trascrizioni di audio parlato.
{:shortdesc}

Il servizio può trascrivere il parlato da diverse lingue e formati audio. Oltre alla trascrizione di base, il servizio può produrre informazioni dettagliate su molti aspetti dell'audio. Per la maggior parte delle lingue, il servizio supporta due frequenze di campionamento: banda larga e banda stretta. Restituisce tutto il contenuto della risposta JSON nella serie di caratteri UTF-8. Per ulteriori informazioni sul servizio, vedi la [documentazione di IBM&reg; Cloud](https://console.bluemix.net/docs/services/speech-to-text/index.html).

Il pacchetto {{site.data.keyword.speechtotextshort}} contiene le seguenti entità. Puoi trovare ulteriori dettagli nella guida di riferimento API {{site.data.keyword.speechtotextshort}} facendo clic sul nome dell'entità.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html) | pacchetto | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  | Lavorare con il servizio {{site.data.keyword.speechtotextshort}} V1. |
| [get-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    model_id,  | Ottenere un modello. |
| [list-models](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-models) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, | Elencare i modelli. |
| [recognize-sessionless](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#recognize-sessionless) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    audio,     content_type,     model,     customization_id,     acoustic_customization_id,     base_model_version,     customization_weight,     inactivity_timeout,     keywords,     keywords_threshold,     max_alternatives,     word_alternatives_threshold,     word_confidence,     timestamps,     profanity_filter,     smart_formatting,     speaker_labels,  | Riconoscere l'audio (senza sessione). |
| [check-job](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-job) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    id,  | Controllare un lavoro. |
| [check-jobs](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-jobs) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, | Controllare i lavori. |
| [create-job](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-job) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    audio,     content_type,     model,     callback_url,     events,     user_token,     results_ttl,     customization_id,     acoustic_customization_id,     base_model_version,     customization_weight,     inactivity_timeout,     keywords,     keywords_threshold,     max_alternatives,     word_alternatives_threshold,     word_confidence,     timestamps,     profanity_filter,     smart_formatting,     speaker_labels,  | Creare un lavoro. |
| [delete-job](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-job) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    id,  | Eliminare un lavoro.|
| [register-callback](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#register-callback) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    callback_url,     user_secret,  | Registrare un callback. |
| [unregister-callback](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#unregister-callback) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    callback_url,  | Annullare la registrazione di un callback. |
| [create-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-language-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   name, base_model_name, dialect, description,  | Creare un modello di lingua personalizzato. |
| [delete-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-language-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Eliminare un modello di lingua personalizzato. |
| [get-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-language-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Ottenere un modello di lingua personalizzato. |
| [list-language-models](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-language-models) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    language,  | Elencare i modelli di lingua personalizzati. |
| [reset-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-language-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Reimpostare un modello di lingua personalizzato. |
| [train-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-language-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word_type_to_add,     customization_weight,  | Formare un modello di lingua personalizzato. |
| [upgrade-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-language-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Eseguire un upgrade di un modello di lingua personalizzato. |
| [add-corpus](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-corpus) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     corpus_name,     corpus_file,     allow_overwrite,  | Aggiungere un corpus. |
| [delete-corpus](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-corpus) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     corpus_name,  | Eliminare un corpus. |
| [get-corpus](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-corpus) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     corpus_name,  | Ottenere un corpus. |
| [list-corpora](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-corpora) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Elencare i corpus. |
| [add-word](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-word) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word_name,    word, sounds_like, display_as,  | Aggiungere una parola personalizzata. |
| [add-words](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-words) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,    words,  | Aggiungere parole personalizzate. |
| [delete-word](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-word) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word_name,  | Eliminare una parola personalizzata. |
| [get-word](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-word) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word_name,  | Ottenere una parola personalizzata. |
| [list-words](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-words) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word_type,     sort,  | Elencare le parole personalizzate. |
| [create-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-acoustic-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   name, base_model_name, description,  | Creare un modello acustico personalizzato. |
| [delete-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-acoustic-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Eliminare un modello acustico personalizzato. |
| [get-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-acoustic-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Ottenere un modello acustico personalizzato. |
| [list-acoustic-models](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-acoustic-models) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    language,  | Elencare i modelli acustici personalizzati. |
| [reset-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-acoustic-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Reimpostare un modello acustico personalizzato. |
| [train-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-acoustic-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     custom_language_model_id,  | Formare un modello acustico personalizzato. |
| [upgrade-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-acoustic-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     custom_language_model_id,  | Eseguire un upgrade di un modello acustico personalizzato. |
| [add-audio](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-audio) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     audio_name,     audio_resource,     content_type,     contained_content_type,     allow_overwrite,  | Aggiungere una risorsa audio. |
| [delete-audio](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-audio) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     audio_name,  | Eliminare una risorsa audio. |
| [get-audio](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-audio) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     audio_name,  | Ottenere una risorsa audio. |
| [list-audio](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-audio) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Elencare le risorse audio. |
| [delete-user-data](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-user-data) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customer_id,  | Eliminare i dati etichettati. |

## Creazione di un'istanza del servizio {{site.data.keyword.speechtotextshort}}
{: #service_instance}

Prima di installare il pacchetto, devi creare un'istanza del servizio {{site.data.keyword.speechtotextshort}} e le credenziali del servizio.
{: shortdesc}

1. [Crea un'istanza del servizio {{site.data.keyword.speechtotextshort}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/catalog/services/speech_to_text).
2. Quando l'istanza del servizio viene creata, vengono anche create per tuo conto le credenziali del servizio generate automaticamente.

## Installazione del pacchetto {{site.data.keyword.speechtotextshort}}
{: #install}

Una volta che disponi di un'istanza del servizio {{site.data.keyword.speechtotextshort}}, utilizza la CLI {{site.data.keyword.openwhisk}} per installare il pacchetto {{site.data.keyword.speechtotextshort}} nel tuo spazio dei nomi.
{: shortdesc}

### Installazione dalla CLI {{site.data.keyword.openwhisk_short}}
{: #speechtotext_cli}

Prima di iniziare:
  1. [Installa il plug-in {{site.data.keyword.openwhisk_short}} per la CLI {{site.data.keyword.Bluemix_notm}}](bluemix_cli.html#cloudfunctions_cli).
  2. Installa il comando [`wskdeploy` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) e aggiungi il file binario scaricato al tuo PERCORSO.

Per installare il pacchetto {{site.data.keyword.speechtotextshort}}:

1. Clona il repository del pacchetto {{site.data.keyword.speechtotextshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Distribuisci il pacchetto.
    ```
    wskdeploy -m openwhisk-sdk/packages/speech-to-text-v1/manifest.yaml
    ```
    {: pre}

3. Verifica che il pacchetto venga aggiunto al tuo elenco pacchetti.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Output:
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

    A seconda della regione dove l'hai creata, l'istanza del servizio potrebbe essere denominata in modo diverso perché è un servizio IAM. Se il comando sopra indicato non riesce, utilizza il seguente nome del servizio per il comando bind:
    ```
    ibmcloud fn service bind speech-to-text speech-to-text-v1
    ```
    {: pre}

    Output di esempio:
    ```
    Credentials 'Credentials-1' from 'speech_to_text' service instance 'Watson Speech to Text' bound to 'speech-to-text-v1'.
    ```
    {: screen}

5. Verifica che il pacchetto sia configurato con le tue credenziali dell'istanza del servizio {{site.data.keyword.speechtotextshort}}.
    ```
    ibmcloud fn package get speech-to-text-v1 parameters
    ```
    {: pre}

    Output di esempio:
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

1. Nella console {{site.data.keyword.openwhisk_short}}, vai alla [pagina Crea ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/openwhisk/create).

2. Utilizzando gli elenchi **Organizzazione Cloud Foundry** e **Spazio Cloud Foundry**, seleziona lo spazio dei nomi in cui vuoi installare il pacchetto {{site.data.keyword.cos_short}}. Gli spazi dei nomi sono formati dai nomi di organizzazione e spazio combinati.

3. Fai clic su **Installa pacchetti**.

4. Fai clic sul gruppo di pacchetti **Watson**.

5. Fai clic sul pacchetto **Speech To Text**.

5. Fai clic su **Installa**.

6. Dopo che il pacchetto è stato installato, verrai reindirizzato alla pagina Azioni e puoi cercare il tuo nuovo pacchetto, che è denominato **speech-to-text-v1**.

7. Per utilizzare le azioni nel pacchetto **speech-to-text-v1**, devi eseguire il bind delle credenziali del servizio alle azioni.
  * Per eseguire il bind delle credenziali del servizio a tutte le azioni nel pacchetto, attieniti ai passi 5 e 6 nelle istruzioni della CLI sopra elencate. 
  * Per eseguire il bind delle credenziali del servizio a singole azioni, completa la seguente procedura nell'IU. **Nota**: devi completare la seguente procedura per ogni azione che desideri utilizzare.
    1. Fai clic su un'azione dal pacchetto **speech-to-text-v1** che vuoi utilizzare. Viene aperta la pagina dei dettagli per tale azione. 
    2. Nella navigazione sulla sinistra, fai clic sulla sezione **Parametri**. 
    3. Immetti un nuovo **parametro**. Per la chiave, immetti `__bx_creds`. Per il valore, incolla l'oggetto JSON delle credenziali del servizio dall'istanza del servizio che hai creato in precedenza.

## Utilizzo del pacchetto {{site.data.keyword.speechtotextshort}}
{: #usage}

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
