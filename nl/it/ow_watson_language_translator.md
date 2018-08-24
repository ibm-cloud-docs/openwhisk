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

# Pacchetto {{site.data.keyword.languagetranslatorshort}}

{{site.data.keyword.languagetranslatorfull}} traduce il testo da una lingua a un'altra. Il servizio offre più modelli di traduzione forniti da IBM che puoi personalizzare in base alla tua terminologia univoca e alla tua lingua.
{: shortdesc}

Il pacchetto {{site.data.keyword.languagetranslatorshort}} contiene le seguenti entità. Puoi trovare ulteriori dettagli nella guida di riferimento API {{site.data.keyword.languagetranslatorshort}} facendo clic sul nome dell'entità.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| [`language-translator-v3`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html) | pacchetto | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  | Lavorare con il servizio {{site.data.keyword.languagetranslatorshort}}. |
| [translate](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#translate) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   text, model_id, source, target,  | Tradurre il testo. |
| [identify](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#identify) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    text,  | Identificare la lingua del testo. |
| [list-identifiable-languages](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-identifiable-languages) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, | Elencare le lingue identificabili. |
| [create-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#create-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    base_model_id,     name,     forced_glossary,     parallel_corpus,  |Creare un modello.|
| [delete-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#delete-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    model_id,  | Eliminare un modello. |
| [get-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#get-model) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    model_id,  | Ottenere i dettagli del modello. |
| [list-models](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-models) | azione |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    source,     target,     default_models,  | Elencare i modelli. |

## Creazione di un'istanza del servizio {{site.data.keyword.languagetranslatorshort}}
{: #service_instance}

Prima di installare il pacchetto, devi creare un'istanza del servizio {{site.data.keyword.languagetranslatorshort}} e le credenziali del servizio.
{: shortdesc}

1. [Crea un'istanza del servizio {{site.data.keyword.languagetranslatorshort}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/catalog/services/language_translator).
2. Quando l'istanza del servizio viene creata, vengono anche create per tuo conto le credenziali del servizio generate automaticamente.

## Installazione del pacchetto {{site.data.keyword.languagetranslatorshort}}
{: #install}

Una volta che disponi di un'istanza del servizio {{site.data.keyword.languagetranslatorshort}}, utilizza la CLI {{site.data.keyword.openwhisk}} per installare il pacchetto {{site.data.keyword.languagetranslatorshort}} nel tuo spazio dei nomi.
{: shortdesc}

### Installazione dalla CLI {{site.data.keyword.openwhisk_short}}
{: #languagetranslator_cli}

Prima di iniziare:
  1. [Installa il plug-in {{site.data.keyword.openwhisk_short}} per la CLI {{site.data.keyword.Bluemix_notm}}](bluemix_cli.html#cloudfunctions_cli).
  2. Installa il comando [`wskdeploy` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) e aggiungi il file binario scaricato al tuo PERCORSO.

Per installare il pacchetto {{site.data.keyword.languagetranslatorshort}}:

1. Clona il repository del pacchetto {{site.data.keyword.languagetranslatorshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Distribuisci il pacchetto.
    ```
    wskdeploy -m openwhisk-sdk/packages/language-translator-v3/manifest.yaml
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
    /myOrg_mySpace/language-translator-v3                        private
    ```
    {: screen}

4. Esegui il bind delle credenziali dall'istanza {{site.data.keyword.languagetranslatorshort}} che hai creato al pacchetto.
    ```
    ibmcloud fn service bind language-translator language-translator-v3
    ```
    {: pre}

    Output di esempio:
    ```
    Credentials 'Credentials-1' from 'language-translator' service instance 'Watson Language Translator' bound to 'language-translator-v3'.
    ```
    {: screen}

5. Verifica che il pacchetto sia configurato con le tue credenziali dell'istanza del servizio {{site.data.keyword.languagetranslatorshort}}.
    ```
    ibmcloud fn package get language-translator-v3 parameters
    ```
    {: pre}

    Output di esempio:
    ```
    ok: got package language-translator-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "language-translator": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
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

1. Nella console {{site.data.keyword.openwhisk_short}}, vai alla [pagina Crea ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/openwhisk/create).

2. Utilizzando gli elenchi **Organizzazione Cloud Foundry** e **Spazio Cloud Foundry**, seleziona lo spazio dei nomi in cui vuoi installare il pacchetto {{site.data.keyword.cos_short}}. Gli spazi dei nomi sono formati dai nomi di organizzazione e spazio combinati.

3. Fai clic su **Installa pacchetti**.

4. Fai clic sul gruppo di pacchetti **Watson**.

5. Fai clic sul pacchetto **Language Translator**.

5. Fai clic su **Installa**.

6. Dopo che il pacchetto è stato installato, verrai reindirizzato alla pagina Azioni e puoi cercare il tuo nuovo pacchetto, che è denominato **language-translator-v3**.

7. Per utilizzare le azioni nel pacchetto **language-translator-v3**, devi eseguire il bind delle credenziali del servizio alle azioni.
  * Per eseguire il bind delle credenziali del servizio a tutte le azioni nel pacchetto, attieniti ai passi 5 e 6 nelle istruzioni della CLI sopra elencate. 
  * Per eseguire il bind delle credenziali del servizio a singole azioni, completa la seguente procedura nell'IU. **Nota**: devi completare la seguente procedura per ogni azione che desideri utilizzare.
    1. Fai clic su un'azione dal pacchetto **language-translator-v3** che vuoi utilizzare. Viene aperta la pagina dei dettagli per tale azione. 
    2. Nella navigazione sulla sinistra, fai clic sulla sezione **Parametri**. 
    3. Immetti un nuovo **parametro**. Per la chiave, immetti `__bx_creds`. Per il valore, incolla l'oggetto JSON delle credenziali del servizio dall'istanza del servizio che hai creato in precedenza.

## Utilizzo del pacchetto {{site.data.keyword.languagetranslatorshort}}
{: #usage}

Per utilizzare le azioni in questo pacchetto, esegui i comandi nel seguente formato:

```
ibmcloud fn action invoke language-translator-v3/<nome_azione> -b -p <param name> <param>
```
{: pre}

Tutte le azioni richiederanno un parametro della versione nel formato AAAA-MM-GG. Quando l'API viene modificata in un modo non compatibile con le versioni precedenti, viene rilasciata una nuova data di versione. Ulteriori dettagli sono disponibili nella [guida di riferimento API](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#versioning).

Le funzioni di questo pacchetto utilizzano la versione corrente di Language Translator, 2018-05-01. Prova l'azione `identify`.
```
ibmcloud fn action invoke language-translator-v3/identify -b -p version 2018-05-01 -p text hola
```
{: pre}
