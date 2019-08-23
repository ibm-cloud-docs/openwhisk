---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: cognitive, serverless, functions

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


# {{site.data.keyword.personalityinsightsshort}}
{: #pkg_person_insights}

Il servizio {{site.data.keyword.personalityinsightsfull}} installabile abilita le applicazioni a ricavare informazioni approfondite da social media, dati aziendali o altre comunicazioni digitali. Il servizio utilizza l'analisi linguistica per desumere le caratteristiche della personalità intrinseche, compreso il modello Big Five, Needs e Values, da comunicazioni digitali quali email, messaggi di testo, tweet e post nei forum.
{: shortdesc}

Il servizio può desumere automaticamente, da social media potenzialmente rumorosi, profili di singole persone che riflettono le loro caratteristiche della personalità. Il servizio può desumere le preferenze di consumo sulla base dei risultati della sua analisi e, per il contenuto JSON che ha una data/ora, puoi notificare il comportamento temporale.
* Per ulteriori informazioni sul significato dei modelli utilizzati dal servizio per descrivere le caratteristiche della personalità, vedi [Modelli di personalità](/docs/services/personality-insights?topic=personality-insights-models).
* Per ulteriori informazioni sul significato delle preferenze di consumo, vedi [Preferenze di consumo](/docs/services/personality-insights?topic=personality-insights-preferences).

La registrazione delle richieste è disabilitata per il servizio {{site.data.keyword.personalityinsightsshort}}. Il servizio non registra o conserva i dati delle richieste e delle risposte, indipendentemente dal fatto che sia impostata l'intestazione della richiesta `X-Watson-Learning-Opt-Out`.
{: note}

Il pacchetto {{site.data.keyword.personalityinsightsshort}} contiene le seguenti entità. Puoi trovare ulteriori dettagli nella guida di riferimento API {{site.data.keyword.personalityinsightsshort}} facendo clic sul nome dell'entità.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| [`personality-insights-v3`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html){: external} | Pacchetto | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Lavorare con il servizio {{site.data.keyword.personalityinsightsshort}} V3. |
| [`profile`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `content`, `content_type`, `content_language`, `accept_language`, `raw_scores`, `csv_headers`, `consumption_preferences` | Ottenere un profilo. |
| [`profile-as-csv`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile-as-csv){: external} | Azione | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `content`, `content_type`, `content_language`, `accept_language`, `raw_scores`, `csv_headers`, `consumption_preferences` | Ottenere un profilo come file CSV. |

## Creazione di un'istanza del servizio {{site.data.keyword.personalityinsightsshort}}
{: #service_instance_insights}

Prima di installare il pacchetto, devi creare un'istanza del servizio {{site.data.keyword.personalityinsightsshort}} e le credenziali del servizio.
{: shortdesc}

1. [Crea un'istanza del servizio {{site.data.keyword.personalityinsightsshort}}](https://cloud.ibm.com/catalog/services/personality_insights){: external}.
2. Quando l'istanza del servizio viene creata, vengono anche create per tuo conto le credenziali del servizio generate automaticamente.

## Installazione del pacchetto {{site.data.keyword.personalityinsightsshort}}
{: #install_insights}

Una volta che disponi di un'istanza del servizio {{site.data.keyword.personalityinsightsshort}}, utilizza la CLI di {{site.data.keyword.openwhisk}} per installare il pacchetto {{site.data.keyword.personalityinsightsshort}} nel tuo spazio dei nomi.
{: shortdesc}

### Installazione dalla CLI {{site.data.keyword.openwhisk_short}}
{: #personalityinsights_cli}

**Prima di iniziare**

[Installa il plug-in {{site.data.keyword.openwhisk_short}} per la CLI {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Per installare il pacchetto {{site.data.keyword.personalityinsightsshort}}:

1. Clona il repository del pacchetto {{site.data.keyword.personalityinsightsshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Distribuisci il pacchetto.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/personality-insights-v3/manifest.yaml
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
    /myOrg_mySpace/personality-insights-v3                        private
    ```
    {: screen}

4. Esegui il bind delle credenziali dall'istanza {{site.data.keyword.personalityinsightsshort}} che hai creato al pacchetto.
    ```
    ibmcloud fn service bind personality_insights personality-insights-v3
    ```
    {: pre}

    A seconda della regione dove l'hai creata, l'istanza del servizio potrebbe essere denominata in modo diverso perché è un servizio IAM. Se il comando non riesce, utilizza il seguente nome di servizio per il comando bind:
    ```
    ibmcloud fn service bind personality-insights personality-insights-v3
    ```
    {: pre}

    **Output di esempio**
      ```
    Credentials 'Credentials-1' from 'personality_insights' service instance 'Watson Personality Insights' bound to 'personality-insights-v3'.
    ```
    {: screen}

5. Verifica che il pacchetto sia configurato con le tue credenziali dell'istanza del servizio {{site.data.keyword.personalityinsightsshort}}.
    ```
    ibmcloud fn package get personality-insights-v3 parameters
    ```
    {: pre}

    **Output di esempio**
      ```
    ok: got package personality-insights-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "personality_insights": {
            "credentials": "Credentials-1",
            "instance": "Watson Personality Insights",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/personality_insights/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Installazione dall'IU {{site.data.keyword.openwhisk_short}}
{: #personalityinsights_ui}

1. Nella console {{site.data.keyword.openwhisk_short}}, vai alla [pagina di creazione](https://cloud.ibm.com/openwhisk/create){: external}.

2. Utilizzando il menu a discesa dello spazio dei nomi, seleziona lo spazio dei nomi in cui vuoi installare il pacchetto. 

3. Fai clic su **Installa pacchetti**.

4. Fai clic sul gruppo di pacchetti **Watson**.

5. Fai clic sul pacchetto **Personality Insights**.

6. Fai clic su **Installa**.

7. Una volta installato il pacchetto, vieni reindirizzato alla pagina Azioni e puoi cercare il tuo nuovo pacchetto, che è denominato `personality-insights-v3`.

8. Per utilizzare le azioni nel pacchetto `personality-insights-v3`, devi eseguire il bind delle credenziali del servizio alle azioni.
  * Per eseguire il bind delle credenziali del servizio a tutte le azioni nel pacchetto, segui i passi 5 e 6 nelle istruzioni della CLI.
  * Per eseguire il bind delle credenziali del servizio a singole azioni, completa la seguente procedura nell'IU.

  Devi completare la seguente procedura per ogni azione che vuoi utilizzare.
  {: note}

    1. Fai clic su un'azione dal pacchetto `personality-insights-v3` che vuoi utilizzare. Viene aperta la pagina dei dettagli per tale azione.
    2. Nella navigazione sulla sinistra, fai clic sulla sezione **Parametri**.
    3. Immetti un nuovo parametro. Per la chiave, immetti `__bx_creds`. Per il valore, incolla l'oggetto JSON delle credenziali del servizio dall'istanza del servizio che hai creato in precedenza.

## Utilizzo del pacchetto {{site.data.keyword.personalityinsightsshort}}
{: #usage_insights}

Per utilizzare le azioni in questo pacchetto, esegui i comandi nel seguente formato:

```
ibmcloud fn action invoke personality-insights-v3/<nome_azione> -b -p <param name> <param>
```
{: pre}

Tutte le azioni richiedono un parametro di versione nel formato AAAA-MM-GG. Quando l'API viene modificata in un modo non compatibile con le versioni precedenti, viene rilasciata una nuova data di versione. Ulteriori dettagli sono disponibili nella [guida di riferimento API](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#versioning){: external}.

Le funzioni di questo pacchetto utilizzano la versione corrente di {{site.data.keyword.personalityinsightsshort}}, 2017-10-13. Prova l'azione `profile`.
```
ibmcloud fn action invoke personality-insights-v3/profile -b -p version 2017-10-13 -p text "Qui puoi scrivere un riepilogo di te stesso, ma dovrà essere lungo almeno 100 parole. Questo riepilogo è solo un testo di riempimento e probabilmente non restituirà nulla di particolarmente interessante dal servizio personality insights. Il servizio utilizza l'analisi linguistica per desumere le caratteristiche della personalità intrinseche, compreso il modello Big Five, Needs e Values, da comunicazioni digitali quali email, messaggi di testo, tweet e post nei forum. Il servizio può desumere automaticamente, da social media potenzialmente rumorosi, profili di singole persone che riflettono le loro caratteristiche della personalità. Il servizio può desumere le preferenze di consumo sulla base dei risultati della sua analisi e, per il contenuto JSON che ha una data/ora, puoi notificare il comportamento temporale."
```
{: pre}



