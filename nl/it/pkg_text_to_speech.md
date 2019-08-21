---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: cognitive, functions, packages

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


# Text to Speech
{: #pkg_text_to_speech}

## Opzioni pacchetto

| Pacchetto | Disponibilità | Descrizione |
| --- | --- | --- | --- |
| [`/whisk.system/watson-textToSpeech`](#text_to_speech) | Preinstallato (non disponibile a Tokyo) | Pacchetto per convertire il testo in voce |
| [`text-to-speech-v1`](#text_to_speech_ins) | Installabile | Lavorare con il servizio {{site.data.keyword.texttospeechshort}}. |

## Watson Text to Speech
{: #text_to_speech}

Questo pacchetto pre-installato non è disponibile nella regione Tokyo. Visualizza il pacchetto [Text to Speech](#text_to_speech_ins) installabile utilizzando l'autenticazione IAM.
{: tip}

Il pacchetto `/whisk.system/watson-textToSpeech` offre una soluzione pratica per richiamare le diverse API Watson per la conversione da testo a voce.
{: shortdesc}

Il pacchetto include le seguenti azioni.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` | Pacchetto | `username`, `password` | Pacchetto per convertire il testo in voce. |
| `/whisk.system/watson-textToSpeech/textToSpeech` | Azione | `payload`, `voice`, `accept`, `encoding`, `username`, `password` | Convertire il testo in audio. |

Il pacchetto `/whisk.system/watson` è obsoleto, inclusa l'azione `/whisk.system/watson/textToSpeech`. Visualizza invece il [pacchetto {{site.data.keyword.texttospeechshort}} installabile](#text_to_speech_ins).
{: deprecated}

### Configurazione del pacchetto {{site.data.keyword.texttospeechshort}} in {{site.data.keyword.cloud_notm}}

Se utilizzi {{site.data.keyword.openwhisk}} da {{site.data.keyword.cloud_notm}}, vengono creati automaticamente i bind di pacchetto per le tue istanze del servizio {{site.data.keyword.cloud_notm}} Watson.

1. Crea un'istanza del servizio {{site.data.keyword.texttospeechshort}} nel tuo [dashboard](https://cloud.ibm.com){: external} {{site.data.keyword.cloud_notm}}. Assicurati di ricordare il nome dell'istanza del servizio e dell'organizzazione e dello spazio {{site.data.keyword.cloud_notm}} in cui ti trovi.

2. Aggiorna i pacchetti nel tuo spazio dei nomi. L'aggiornamento crea automaticamente un bind di pacchetto per l'istanza del servizio Watson da te creata.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **Output di esempio**
  ```
  created bindings:
  Watson_TextToSpeech_Credentials-1
  ```
  {: screen}

  Elenca i pacchetti per verificare che il bind di pacchetto sia stato creato.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Output di esempio**
  ```
  packages
  /myOrg_mySpace/Watson_TextToSpeec_Credentials-1 private
  ```
  {: screen}

### Configurazione di un pacchetto {{site.data.keyword.texttospeechshort}} all'esterno di {{site.data.keyword.cloud_notm}}

Se non utilizzi {{site.data.keyword.openwhisk_short}} in {{site.data.keyword.cloud_notm}} o se vuoi configurare {{site.data.keyword.texttospeechshort}} all'esterno di {{site.data.keyword.cloud_notm}}, devi creare manualmente un bind di pacchetto per il tuo servizio {{site.data.keyword.texttospeechshort}}. Ti servono il nome utente e la password del servizio {{site.data.keyword.texttospeechshort}}.

Crea un bind di pacchetto configurato per il tuo servizio {{site.data.keyword.texttospeechshort}}.
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username <username> -p password <password>
```
{: pre}

### Conversione da testo a voce

L'azione `/whisk.system/watson-textToSpeech/textToSpeech` converte un testo in audio. Sono supportati i seguenti parametri.

| `username` | Il nome utente dell'API Watson. |
| `password` | La password dell'API Watson. |
| `payload` | Il testo da convertire in voce. |
| `voice` | La voce del parlante. |
| `accept` | Il formato del file del discorso. |
| `encoding` | La codifica dei dati binari del discorso. |

Verifica l'azione `textToSpeech` nel tuo bind di pacchetto per convertire il testo.
```
ibmcloud fn action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
```
{: pre}

**Output di esempio**
```
{
  "payload": "<base64 encoding of a .wav file>"
  }
```
{: screen}


## {{site.data.keyword.texttospeechshort}}
{: #text_to_speech_ins}

Il servizio {{site.data.keyword.texttospeechfull}} installabile fornisce un'API con le funzionalità di sintesi vocale di IBM per sintetizzare il testo in un parlato naturale in una vasta gamma di lingue, dialetti e voci.
{:shortdesc}

Il servizio supporta almeno una voce maschile o femminile, a volte entrambe, per ciascuna lingua. L'audio viene inviato nuovamente al client con un ritardo minimo. Per ulteriori informazioni sul servizio, vedi la [documentazione di IBM Cloud](/docs/services/text-to-speech?topic=text-to-speech-about).

Il pacchetto {{site.data.keyword.texttospeechshort}} contiene le seguenti entità. Puoi trovare ulteriori informazioni nella guida di riferimento API {{site.data.keyword.texttospeechshort}} facendo clic sul nome dell'entità.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| [`text-to-speech-v1`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html){: external} | Pacchetto | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Lavorare con il servizio {{site.data.keyword.texttospeechshort}}. |
| [`get-voice`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `voice`, `customization_id` | Ottenere una voce. |
| [`list-voices`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voices){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Elencare le voci. |
| [`synthesize`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#synthesize){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `accept`, `voice`, `customization_id` | Sintetizzare l'audio. |
| [`get-pronunciation`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-pronunciation){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `voice`, `format`, `customization_id` | Ottenere la pronuncia. |
| [`create-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#create-voice-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `language`, `description` | Creare un modello personalizzato. |
| [`delete-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-voice-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Eliminare un modello personalizzato. |
| [`get-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Ottenere un modello personalizzato. |
| [`list-voice-models`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voice-models){: external} | Azione | `username`,`password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` | Elencare i modelli personalizzati. |
| [`update-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#update-voice-model){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `name`, `description`, `words` | Aggiornare un modello personalizzato. |
| [`add-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-word){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word`, `translation`, `part_of_speech` | Aggiungere una parola personalizzata. |
| [`add-words`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-words){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `words`  | Aggiungere parole personalizzate. |
| [`delete-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-word){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word` | Eliminare una parola personalizzata. |
| [`get-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-word){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word` | Ottenere una parola personalizzata. |
| [`list-words`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-words){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Elencare le parole personalizzate. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-user-data){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id` | Eliminare i dati etichettati. |

### Creazione di un'istanza del servizio {{site.data.keyword.texttospeechshort}}
{: #service_instance_texttospeech}

Prima di installare il pacchetto, devi creare un'istanza del servizio {{site.data.keyword.texttospeechshort}} e le credenziali del servizio.
{: shortdesc}

1. [Crea un'istanza del servizio {{site.data.keyword.texttospeechshort}}](https://cloud.ibm.com/catalog/services/text_to_speech){: external}.
2. Quando l'istanza del servizio viene creata, vengono anche create per tuo conto le credenziali del servizio generate automaticamente.

### Installazione del pacchetto {{site.data.keyword.texttospeechshort}}
{: #install_texttospeech}

Una volta che disponi di un'istanza del servizio {{site.data.keyword.texttospeechshort}}, utilizza la CLI {{site.data.keyword.openwhisk}} per installare il pacchetto {{site.data.keyword.texttospeechshort}} nel tuo spazio dei nomi.
{: shortdesc}

### Installazione dalla CLI {{site.data.keyword.openwhisk_short}}
{: #texttospeech_cli}

**Prima di iniziare**
[Installa il plug-in {{site.data.keyword.openwhisk_short}} per la CLI {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Per installare il pacchetto {{site.data.keyword.texttospeechshort}}, immetti i seguenti comandi.

1. Clona il repository del pacchetto {{site.data.keyword.texttospeechshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Distribuisci il pacchetto.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/text-to-speech-v1/manifest.yaml
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

    **Output di esempio**
      ```
    Credentials 'Credentials-1' from 'text_to_speech' service instance 'Watson Text to Speech' bound to 'text-to-speech-v1'.
    ```
    {: screen}

5. Verifica che il pacchetto sia configurato con le tue credenziali dell'istanza del servizio {{site.data.keyword.texttospeechshort}}.
    ```
    ibmcloud fn package get text-to-speech-v1 parameters
    ```
    {: pre}

    **Output di esempio**
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

1. Nella console {{site.data.keyword.openwhisk_short}}, vai alla [pagina di creazione](https://cloud.ibm.com/openwhisk/create){: external}.

2. Seleziona lo spazio dei nomi in cui vuoi installare il pacchetto utilizzando il menu a discesa dello spazio dei nomi.

3. Fai clic su **Installa pacchetti**.

4. Fai clic sul gruppo di pacchetti **Watson**.

5. Fai clic sul pacchetto **Text To Speech**.

5. Fai clic su **Installa**.

6. Dopo che il pacchetto è stato installato, verrai reindirizzato alla pagina Azioni e puoi cercare il tuo nuovo pacchetto, che è denominato `text-to-speech-v1`.

7. Per utilizzare le azioni nel pacchetto `text-to-speech-v1`, devi eseguire il bind delle credenziali del servizio alle azioni.
  * Per eseguire il bind delle credenziali del servizio a tutte le azioni nel pacchetto, segui i passi [4 e 6 nelle istruzioni della CLI](#texttospeech_cli).
  * Per eseguire il bind delle credenziali del servizio a singole azioni, completa la seguente procedura nell'IU.

  Devi completare la seguente procedura per ogni azione che vuoi utilizzare.
  {: note}

    1. Fai clic su un'azione dal pacchetto `text-to-speech-v1` che vuoi utilizzare. Viene aperta la pagina dei dettagli per tale azione.
    2. Nella navigazione sulla sinistra, fai clic sulla sezione **Parametri**.
    3. Immetti un nuovo parametro. Per la chiave, immetti `__bx_creds`. Per il valore, incolla l'oggetto JSON delle credenziali del servizio dall'istanza del servizio che hai creato in precedenza.

## Utilizzo del pacchetto {{site.data.keyword.texttospeechshort}}
{: #usage_texttospeech}

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


