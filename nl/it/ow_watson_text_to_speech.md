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

# Pacchetto {{site.data.keyword.texttospeechshort}}

Il servizio {{site.data.keyword.texttospeechfull}} fornisce un'API che utilizza le funzionalità di sintesi vocale di IBM per sintetizzare il testo in un parlato naturale in una gamma di lingue, dialetti e voci.
{:shortdesc}

Il servizio supporta almeno una voce maschile o femminile, a volte entrambe, per ciascuna lingua. L'audio viene inviato nuovamente al client con un ritardo minimo. Per ulteriori informazioni sul servizio, vedi la [documentazione di IBM Cloud](https://console.bluemix.net/docs/services/text-to-speech/index.html).

Il pacchetto {{site.data.keyword.texttospeechshort}} contiene le seguenti entità. Puoi trovare ulteriori dettagli nella guida di riferimento API {{site.data.keyword.texttospeechshort}} facendo clic sul nome dell'entità.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| [`text-to-speech-v1`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html) | pacchetto | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  | Lavorare con il servizio {{site.data.keyword.texttospeechshort}}. |
| [get-voice](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    voice,     customization_id,  | Ottenere una voce. |
| [list-voices](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voices) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, | Elencare le voci. |
| [synthesize](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#synthesize) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   text,     accept,     voice,     customization_id,  | Sintetizzare l'audio. |
| [get-pronunciation](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-pronunciation) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    text,     voice,     format,     customization_id,  | Ottenere la pronuncia. |
| [create-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#create-voice-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   name, language, description,  | Creare un modello personalizzato. |
| [delete-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-voice-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Eliminare un modello personalizzato. |
| [get-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Ottenere un modello personalizzato. |
| [list-voice-models](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voice-models) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    language,  | Elencare i modelli personalizzati. |
| [update-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#update-voice-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,    name, description, words,  | Aggiornare un modello personalizzato. |
| [add-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-word) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word,    translation, part_of_speech,  | Aggiungere una parola personalizzata. |
| [add-words](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-words) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,    words,  | Aggiungere parole personalizzate. |
| [delete-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-word) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word,  | Eliminare una parola personalizzata. |
| [get-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-word) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word,  | Ottenere una parola personalizzata. |
| [list-words](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-words) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Elencare le parole personalizzate. |
| [delete-user-data](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-user-data) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customer_id,  | Eliminare i dati etichettati. |

## Creazione di un'istanza del servizio {{site.data.keyword.texttospeechshort}}
{: #service_instance}

Prima di installare il pacchetto, devi creare un'istanza del servizio {{site.data.keyword.texttospeechshort}} e le credenziali del servizio.
{: shortdesc}

1. [Crea un'istanza del servizio {{site.data.keyword.texttospeechshort}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/catalog/services/text_to_speech).
2. Quando l'istanza del servizio viene creata, vengono anche create per tuo conto le credenziali del servizio generate automaticamente.

## Installazione del pacchetto {{site.data.keyword.texttospeechshort}}
{: #install}

Una volta che disponi di un'istanza del servizio {{site.data.keyword.texttospeechshort}}, utilizza la CLI {{site.data.keyword.openwhisk}} per installare il pacchetto {{site.data.keyword.texttospeechshort}} nel tuo spazio dei nomi.
{: shortdesc}

### Installazione dalla CLI {{site.data.keyword.openwhisk_short}}
{: #texttospeech_cli}

Prima di iniziare:
  1. [Installa il plug-in {{site.data.keyword.openwhisk_short}} per la CLI {{site.data.keyword.Bluemix_notm}}](bluemix_cli.html#cloudfunctions_cli).
  2. Installa il comando [`wskdeploy` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) e aggiungi il file binario scaricato al tuo PERCORSO.

Per installare il pacchetto {{site.data.keyword.texttospeechshort}}:

1. Clona il repository del pacchetto {{site.data.keyword.texttospeechshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Distribuisci il pacchetto.
    ```
    wskdeploy -m openwhisk-sdk/packages/text-to-speech-v1/manifest.yaml
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
    /myOrg_mySpace/text-to-speech-v1                        private
    ```
    {: screen}

4. Esegui il bind delle credenziali dall'istanza {{site.data.keyword.texttospeechshort}} che hai creato al pacchetto.
    ```
    ibmcloud fn service bind text_to_speech text-to-speech-v1
    ```
    {: pre}

    A seconda della regione dove l'hai creata, l'istanza del servizio potrebbe essere denominata in modo diverso perché è un servizio IAM. Se il comando sopra indicato non riesce, utilizza il seguente nome del servizio per il comando bind:
    ```
    ibmcloud fn service bind text-to-speech text-to-speech-v1
    ```
    {: pre}
    Output di esempio:
    ```
    Credentials 'Credentials-1' from 'text_to_speech' service instance 'Watson Text to Speech' bound to 'text-to-speech-v1'.
    ```
    {: screen}

5. Verifica che il pacchetto sia configurato con le tue credenziali dell'istanza del servizio {{site.data.keyword.texttospeechshort}}.
    ```
    ibmcloud fn package get text-to-speech-v1 parameters
    ```
    {: pre}

    Output di esempio:
    ```
    ok: got package text-to-speech-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "text_to_speech": {
            "credentials": "Credentials-1",
            "instance": "Watson Text to Speech",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/text_to_speech/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Installazione dall'IU {{site.data.keyword.openwhisk_short}}
{: #texttospeech_ui}

1. Nella console {{site.data.keyword.openwhisk_short}}, vai alla [pagina Crea ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/openwhisk/create).

2. Utilizzando gli elenchi **Organizzazione Cloud Foundry** e **Spazio Cloud Foundry**, seleziona lo spazio dei nomi in cui vuoi installare il pacchetto {{site.data.keyword.cos_short}}. Gli spazi dei nomi sono formati dai nomi di organizzazione e spazio combinati.

3. Fai clic su **Installa pacchetti**.

4. Fai clic sul gruppo di pacchetti **Watson**.

5. Fai clic sul pacchetto **Text To Speech**.

5. Fai clic su **Installa**.

6. Dopo che il pacchetto è stato installato, verrai reindirizzato alla pagina Azioni e puoi cercare il tuo nuovo pacchetto, che è denominato **text-to-speech-v1**.

7. Per utilizzare le azioni nel pacchetto **text-to-speech-v1**, devi eseguire il bind delle credenziali del servizio alle azioni.
  * Per eseguire il bind delle credenziali del servizio a tutte le azioni nel pacchetto, attieniti ai passi 5 e 6 nelle istruzioni della CLI sopra elencate. 
  * Per eseguire il bind delle credenziali del servizio a singole azioni, completa la seguente procedura nell'IU. **Nota**: devi completare la seguente procedura per ogni azione che desideri utilizzare.
    1. Fai clic su un'azione dal pacchetto **text-to-speech-v1** che vuoi utilizzare. Viene aperta la pagina dei dettagli per tale azione. 
    2. Nella navigazione sulla sinistra, fai clic sulla sezione **Parametri**. 
    3. Immetti un nuovo **parametro**. Per la chiave, immetti `__bx_creds`. Per il valore, incolla l'oggetto JSON delle credenziali del servizio dall'istanza del servizio che hai creato in precedenza.

## Utilizzo del pacchetto {{site.data.keyword.texttospeechshort}}
{: #usage}

Per utilizzare le azioni in questo pacchetto, esegui i comandi nel seguente formato:

```
ibmcloud fn action invoke text-to-speech-v1/<nome_azione> -b -p <param name> <param>
```
{: pre}

Prova l'azione `list-voices`.
```
ibmcloud fn action invoke text-to-speech-v1/list-voices -b
```
{: pre}
