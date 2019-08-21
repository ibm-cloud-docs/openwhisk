---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: deploy, deployment templates, templates, example, quickstart, functions, serverless

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


# Distribuzione dei template Quickstart
{: #templates}

{{site.data.keyword.openwhisk}} offre un catalogo di template per aiutarti a iniziare rapidamente il tuo prossimo progetto. I template sono una combinazione di azioni, trigger e sequenze. Alcuni template inoltre incorporano altre istanze del servizio da {{site.data.keyword.cloud_notm}}. Utilizzando i template, puoi creare rapidamente e facilmente un progetto e iniziare subito la codifica.
{: shortdesc}


## Template Quickstart disponibili
{: #available-templates}

| Nome | Descrizione | Runtime supportati | Repository origine |
|:-----------------|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} Events](#cloudant-template) | Quando un documento viene modificato o aggiunto in un database {{site.data.keyword.cloudantfull}}, registra la modifica nella console. | Node.js, Swift, Python, PHP | [Codice](https://github.com/ibm-functions/template-cloudant-trigger){: external} |
| [Upload Image](#image-template) | Utilizza un'azione web per caricare un'immagine nel bucket di un'istanza {{site.data.keyword.cos_full}} e poi richiamare una miniatura di tale immagine. | Node.js | [Codice](https://github.com/ibm-functions/template-cloud-object-storage){: external}
| [Get HTTP Resource](#get-http-resource-template) | Richiama un'azione web utilizzando un evento HTTP e ottieni i dati dall'API Yahoo Weather. | Node.js, Python | [Codice](https://github.com/ibm-functions/template-get-external-resource){: external}
| [Hello World](#hello-world-template) | Crea un'azione di base che accetta un oggetto JSON come un singolo parametro. | Node.js, Swift, Python, PHP | [Codice](https://github.com/ibm-functions/template-hello-world){: external}
| [{{site.data.keyword.messagehub}} Events](#messagehub-events-template) | Quando vengono aggiunti dei nuovi dati in un argomento {{site.data.keyword.messagehub_full}}, registra la modifica nella console. | Node.js, Swift, Python, PHP | [Codice](https://github.com/ibm-functions/template-messagehub-trigger){: external}
| [Periodic Slack Reminder](#slack-reminder-template) | Utilizza un webhook su Slack in base a un trigger periodico. | Node.js, Swift, Python, PHP | [Codice](https://github.com/ibm-functions/template-reminder-slack){: external}

Puoi accedere al codice di ciascun template nel rispettivo repository di codici, modificarlo a piacere e creare la tua funzione in base ad esso.
{: tip}


## Distribuzione del template {{site.data.keyword.cloudant_short_notm}} Events
{: #cloudant-template}

Il template {{site.data.keyword.cloudant_short_notm}} crea una sequenza di azioni e un trigger che avvia tale sequenza. Il trigger viene attivato quando viene apportata una modifica nel database di esempio {{site.data.keyword.cloudant_short_notm}} connesso di gatti. L'elemento di dati previsto è un gatto, con un nome e un colore definiti. Quando nel database viene aggiunto un nuovo gatto o modificato uno attuale, i dati vengono registrati nella console.

### Distribuzione del template {{site.data.keyword.cloudant_short_notm}} Events dall'IU
{: #cloudant-template-ui}

1. Vai alla [pagina di creazione ](https://cloud.ibm.com/openwhisk/create){: external} nella console {{site.data.keyword.openwhisk_short}}.

2. Fai clic su **Template Quickstart**.

3. Fai clic su **Cloudant Events**.

4. Crea l'azione {{site.data.keyword.cloudant_short_notm}}.
    1. Fornisci un nome per il tuo pacchetto o utilizza il nome predefinito fornito `new-cloudant-item`.
    2. Nell'elenco **Azioni**, seleziona `Node.js 6` per l'esempio database di gatti e fai clic su **Avanti**.

5. Crea il trigger {{site.data.keyword.cloudant_short_notm}}. I trigger richiamano le azioni quando ricevono eventi dalle origini evento. Per creare il trigger, devi fornire le credenziali del servizio e un'istanza del servizio {{site.data.keyword.messagehub}}.
    1. Nell'elenco **Istanza Cloudant**, seleziona **Crea la tua istanza**. Viene aperta la pagina di configurazione {{site.data.keyword.cloudant_short_notm}}.
    2. Crea l'istanza del servizio {{site.data.keyword.cloudant_short_notm}}.
    3. Crea una serie di credenziali del servizio.
    4. Fai clic su **OK** per chiudere la pagina di configurazione {{site.data.keyword.cloudant_short_notm}} e ritornare alla console {{site.data.keyword.openwhisk_short}}.
    5. Nell'elenco **Istanza Cloudant**, puoi ora selezionare **Immetti le tue credenziali** e fornisci le seguenti informazioni.
        
        * `Username` - Il tuo nome utente {{site.data.keyword.cloudant_short_notm}}.
        * `Password` - La tua password {{site.data.keyword.cloudant_short_notm}}.
        * `Host` - `<username>.cloudant.com`. 
        * `Database` - Il nome del tuo database {{site.data.keyword.cloudant_short_notm}}.

6. Fai clic su **Distribuisci**. Dopo la distribuzione del template, puoi apportare ulteriori modifiche al codice per personalizzarlo secondo necessità oppure tornare indietro e consultare il catalogo dei template disponibili.

### Distribuzione del template {{site.data.keyword.cloudant_short_notm}} Events dalla CLI
{: #cloudant-template-cli}

1. Clona il repository del template.
    ```
    git clone https://github.com/ibm-functions/template-cloudant-trigger.git
    ```
    {: pre}

2. Passa alla directory per il runtime dell'azione che vuoi utilizzare: `nodejs-6`, `nodejs`, `php`, `python` o `swift`.
    ```
    cd template-cloudant-trigger/runtimes/nodejs
    ```
    {: pre}

3. Distribuisci il template, utilizzando le seguenti variabili di ambiente.
    ```
    CLOUDANT_HOSTNAME=<host> CLOUDANT_USERNAME=<username> CLOUDANT_PASSWORD=<password> CLOUDANT_DATABASE=<database> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>Descrizione delle variabili di ambiente</caption>
    <thead>
    <th colspan=2>Descrizione delle variabili di ambiente</th>
    </thead>
    <tbody>
    <tr><td><code>CLOUDANT_HOSTNAME</code></td><td><code>&lt;username&gt;.cloudant.com</code></td></tr>
    <tr><td><code>CLOUDANT_USERNAME</code></td><td>Il tuo nome utente {{site.data.keyword.cloudant_short_notm}}</td></tr>
    <tr><td><code>CLOUDANT_PASSWORD</code></td><td>La tua password {{site.data.keyword.cloudant_short_notm}}</td></tr>
    <tr><td><code>CLOUDANT_DATABASE</code></td><td>Il nome del tuo database {{site.data.keyword.cloudant_short_notm}}</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>Un nome personalizzato per il pacchetto</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>Un nome personalizzato per la regola</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>Un nome personalizzato per il trigger</td></tr>
    </tbody></table>

## Distribuzione del template Upload Image
{: #image-template}

Il template Upload Image crea un'azione web che puoi utilizzare per caricare un'immagine in un bucket {{site.data.keyword.cos_full_notm}} tramite una piccola interfaccia. Il template richiama quindi l'immagine come una miniatura e la visualizza nell'interfaccia dell'azione web.


### Distribuzione del template Upload Image dall'IU
{: #image-template-ui}

1. Vai alla [pagina di creazione ](https://cloud.ibm.com/openwhisk/create){: external} nella console {{site.data.keyword.openwhisk_short}}.

2. Fai clic su **Template Quickstart**.

3. Fai clic su **Upload Image**.

4. Immetti un nome per il tuo pacchetto o utilizza il nome predefinito `upload-image` e fai clic su **Avanti**.

6. Il template richiede le credenziali del servizio da un'istanza del servizio {{site.data.keyword.cos_full_notm}}. Nell'elenco **{{site.data.keyword.cos_full_notm}}**, seleziona una delle seguenti opzioni:
  * **Crea un'istanza**. Se non hai un'istanza del servizio esistente, seleziona questa opzione per crearne una.
      1. Nella pagina di creazione dell'istanza del servizio {{site.data.keyword.cos_full_notm}} che viene aperta, crea un'istanza del servizio.
      2. [Crea una serie di credenziali del servizio HMAC](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials).
      3. [Crea almeno un bucket](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets).
  * **Immetti le tue credenziali**. Seleziona questa opzione per immettere manualmente le tue credenziali per un'istanza del servizio {{site.data.keyword.cos_full_notm}}. Le credenziali devono avere le chiavi HMAC e le istanze del servizio devono avere almeno un bucket.
  * **Istanze esistenti**. Se hai delle istanze esistenti di {{site.data.keyword.cos_full_notm}}, seleziona una delle istanze dall'elenco. Le credenziali devono avere le chiavi HMAC e le istanze del servizio devono avere almeno un bucket.

7. Fai clic su **Distribuisci**.

8. Nella navigazione sulla sinistra, fai clic su **Endpoint**

9. Nella sezione **Azione web**, copia il link senza il suffisso .json e incollalo nella barra degli indirizzi del tuo browser. Viene visualizzata l'interfaccia per l'azione web del template.

10. Facoltativo: dopo la distribuzione del template, puoi passare al dashboard **Azioni** per personalizzare il codice nei due nuovi pacchetti:
    * Il pacchetto `cloud-object-storage`, che contiene le azioni che funzionano con le istanze {{site.data.keyword.cos_full_notm}}
    * Il pacchetto di template (nome predefinito `upload-image`), che contiene l'azione `app`

### Distribuzione del template Upload Image dalla CLI
{: #image-template-cli}

1. [Installa il pacchetto {{site.data.keyword.cos_full_notm}}](/docs/openwhisk?topic=cloud-functions-pkg_obstorage).

2. Clona il repository del template.
    ```
    git clone https://github.com/ibm-functions/template-cloud-object-storage.git
    ```
    {: pre}

3. Passa alla directory di runtime `nodejs`.
    ```
    cd template-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Distribuisci il template utilizzando un nome di pacchetto personalizzato e il nome del tuo bucket {{site.data.keyword.cos_full_notm}} come variabili di ambiente.
    ```
    PACKAGE_NAME=<name> BUCKET_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Nella [pagina Azioni](https://cloud.ibm.com/openwhisk/actions){: external} della console {{site.data.keyword.openwhisk_short}}, fai clic sull'azione `process-change`.

5. Nella navigazione sulla sinistra, fai clic su **Endpoint**.

6. Nella sezione **Azione web**, copia il link senza il suffisso .json e incollalo nella barra degli indirizzi del tuo browser. Viene visualizzata l'interfaccia per l'azione web del template.

## Distribuzione del template Get HTTP Resource
{: #get-http-resource-template}

Il template Get HTTP Resource crea un'azione per recuperare una risorsa esterna, l'API Yahoo Weather, e quindi restituisce i dati. L'azione è abilitata come azione web, il che ne permette la chiamata con un URL, che è abilitata a CORS e non richiede una chiave di autenticazione, utile per creare backend per le applicazioni web. 

Per impostazione predefinita, l'endpoint `get-http-resource` è pubblicamente disponibile per chiunque lo richiami.
{: note}

### Distribuzione del template Get HTTP Resource dall'IU
{: #get-http-resource-template-ui}

1. Vai alla [pagina di creazione ](https://cloud.ibm.com/openwhisk/create){: external} nella console {{site.data.keyword.openwhisk_short}}.

2. Fai clic su **Template Quickstart**.

3. Fai clic su **Get HTTP Resource**.

3. Immetti un nome per il tuo pacchetto o utilizza il nome predefinito `get-http-resource`.

4. Scegli il runtime per le tue azioni: Node.js 8, Node.js 6 o Python 3.

5. Fai clic su **Distribuisci**.

6. Richiama l'azione eseguendo il curl del seguente URL: `https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`. Ad esempio:
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

Dopo la distribuzione del template, puoi apportare ulteriori modifiche al codice per personalizzarlo secondo necessità oppure tornare indietro e consultare il catalogo dei template disponibili.

### Distribuzione del template Get HTTP Resource dalla CLI
{: #get-http-resource-template-cli}

1. Clona il repository del template.
    ```
    git clone https://github.com/ibm-functions/template-get-external-resource.git
    ```
    {: pre}

2. Passa alla directory per il runtime dell'azione che vuoi utilizzare: `nodejs-6`, `nodejs` o `python`.
    ```
    cd template-get-external-resource/runtimes/nodejs
    ```
    {: pre}

3. Distribuisci il template, utilizzando un nome di pacchetto personalizzato come una variabile di ambiente.
    ```
    PACKAGE_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Richiama l'azione eseguendo il curl del seguente URL: `https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`. Ad esempio:
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

## Distribuzione del template Hello World
{: #hello-world-template}

Puoi distribuire questa azione Hello World di base per iniziare ad utilizzare {{site.data.keyword.openwhisk_short}} o per verificare altre entità che hai creato, ad esempio trigger e regole.

### Distribuzione del template Hello World dall'IU
{: #hello-world-template-ui}

1. Vai alla [pagina di creazione ](https://cloud.ibm.com/openwhisk/create){: external} nella console {{site.data.keyword.openwhisk_short}}.

2. Fai clic su **Template Quickstart**.

3. Fai clic su **Hello World**.

4. Immetti un nome per il tuo pacchetto o utilizza il nome predefinito `hello-world`.

5. Scegli il runtime per le tue azioni: Node.js 8, Node.js 6, Python 3, Swift 4 o PHP 7.1.

6. Fai clic su **Distribuisci**.

Dopo la distribuzione del template, puoi apportare ulteriori modifiche al codice per personalizzarlo secondo necessità oppure tornare indietro e consultare il catalogo dei template disponibili.

### Distribuzione del template Hello World dalla CLI
{: #hello-world-template-cli}

1. Clona il repository del template Hello World.
    ```
    git clone https://github.com/ibm-functions/template-hello-world.git
    ```
    {: pre}

2. Passa alla directory per il runtime dell'azione che vuoi utilizzare: `nodejs-6`, `nodejs`, `php`, `python` o `swift`.
    ```
    cd template-hello-world/runtimes/nodejs
    ```
    {: pre}

3. Distribuisci il template.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

## Distribuzione del template {{site.data.keyword.messagehub}} Events
{: #messagehub-events-template}

Il template {{site.data.keyword.messagehub}} Events crea un'azione e un trigger che avvia tale azione. Il trigger viene attivato ogni volta che viene aggiunto un nuovo elemento all'argomento {{site.data.keyword.messagehub}} scelto durante la creazione del template.

### Distribuzione del template {{site.data.keyword.messagehub}} Events dall'IU
{: #messagehub-events-template-ui}

1. Vai alla [pagina di creazione ](https://cloud.ibm.com/openwhisk/create){: external} nella console {{site.data.keyword.openwhisk_short}}.

2. Fai clic su **Template Quickstart**.

3. Fai clic su **{{site.data.keyword.messagehub}} Events**.

4. Crea l'azione {{site.data.keyword.messagehub}}.
    1. Fornisci un nome per il tuo pacchetto o utilizza il nome predefinito fornito `message-hub-events`.
    2. Scegli il runtime per le tue azioni: Node.js 8, Node.js 6, Python 3, Swift 4 o PHP 7.1.
    3. Fai clic su **Avanti**.

5. Crea il trigger {{site.data.keyword.messagehub}}. I trigger richiamano le azioni quando ricevono eventi dalle origini evento. Per creare il trigger, devi fornire un'istanza del servizio {{site.data.keyword.messagehub}} e le credenziali del servizio.
    1. Nell'elenco **Istanza MessageHub**, seleziona **Crea la tua istanza**. Viene aperta la pagina di creazione {{site.data.keyword.messagehub}}.
    2. Crea l'istanza del servizio {{site.data.keyword.messagehub}}.
    3. Crea una serie di credenziali del servizio.
    4. Fai clic su **OK** per chiudere la pagina di creazione {{site.data.keyword.messagehub}} e ritornare alla console {{site.data.keyword.messagehub}}.
    5. Nell'elenco **Istanza MessageHub**, puoi ora selezionare **Immetti le tue credenziali** e fornisci le seguenti informazioni:
        * `Username` - Il tuo nome utente {{site.data.keyword.messagehub}}.
        * `Password` - La tua password {{site.data.keyword.messagehub}}.
        * `kafka_admin_url` - Il tuo URL REST di gestione {{site.data.keyword.messagehub}}.
        * `Database` - Il nome del tuo database {{site.data.keyword.messagehub}}.
        * `Topic` - L'argomento a cui sottoscrivere.

6. Fai clic su **Distribuisci**.

Dopo la distribuzione del template, puoi apportare ulteriori modifiche al codice per personalizzarlo secondo necessità oppure tornare indietro e consultare il catalogo dei template disponibili.

### Distribuzione del template {{site.data.keyword.messagehub}} Events dalla CLI
{: #messagehub-events-template-cli}

1. Clona il repository del template.
    ```
    git clone https://github.com/ibm-functions/template-messagehub-trigger.git
    ```
    {: pre}

2. Passa alla directory per il runtime dell'azione che vuoi utilizzare: `nodejs-6`, `nodejs`, `php`, `python` o `swift`.
    ```
    cd template-messagehub-trigger/runtimes/nodejs
    ```
    {: pre}

3. Distribuisci il template utilizzando le seguenti variabili di ambiente.
    ```
    KAFKA_BROKERS=<host> KAFKA_TOPIC=<topic> MESSAGEHUB_USER=<username> MESSAGEHUB_PASS=<password> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>Descrizione delle variabili di ambiente</caption>
    <thead>
    <th colspan=2>Descrizione delle variabili di ambiente</th>
    </thead>
    <tbody>
    <tr><td><code>KAFKA_BROKERS</code></td><td>Il tuo URL REST di gestione {{site.data.keyword.messagehub}}</td></tr>
    <tr><td><code>KAFKA_TOPIC</code></td><td>L'argomento a cui sottoscriversi</td></tr>
    <tr><td><code>MESSAGEHUB_USER</code></td><td>Il tuo nome utente {{site.data.keyword.messagehub}}</td></tr>
    <tr><td><code>MESSAGEHUB_PASS</code></td><td>La tua password {{site.data.keyword.messagehub}}</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>Un nome personalizzato per il pacchetto</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>Un nome personalizzato per la regola</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>Un nome personalizzato per il trigger</td></tr>
    </tbody></table>

## Distribuzione del template Periodic Slack Reminder
{: #slack-reminder-template}

Il template Periodic Slack Reminder pubblica in Slack sulla base di un intervallo fornito dall'utente durante la creazione del trigger.

### Distribuzione del template Periodic Slack Reminder dall'IU
{: #slack-reminder-template-ui}

1. Vai a https://api.slack.com/incoming-webhooks per configurare l'URL dei webhook in entrata richiesto.

1. Vai alla [pagina di creazione ](https://cloud.ibm.com/openwhisk/create){: external} nella console {{site.data.keyword.openwhisk_short}}.

2. Fai clic su **Template Quickstart**.

3. Fai clic su **Periodic Slack Reminder Events**.

4. Crea l'azione Periodic Slack Reminder.
    1. Fornisci un nome per il tuo pacchetto o utilizza il nome predefinito fornito `periodic-slack-reminder`.
    2. Scegli il runtime per le tue azioni: Node.js 8, Node.js 6, Python 3, Swift 4 o PHP 7.1.
    3. Nella sezione Parametri, immetti l'URL webhook nel campo **Valore parametro**, ad esempio `https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX`.
    4. Fai clic su **Avanti**.

5. Crea il trigger Periodic Slack Reminder. I trigger richiamano le azioni quando ricevono eventi dalle origini evento.
    1. Specifica l'intervallo di tempo del trigger utilizzando un'espressione Pattern o `Cron`.
        * `Pattern` - Seleziona le ore UTC per i giorni feriali, le ore e i minuti.
        * `Cron` - Specifica una sequenza cron basata sulla <a href="http://crontab.org">sintassi crontab UNIX</a>. Utilizza 5 o meno campi separati da spazi nel formato `X X X X X`.
    2. Aggiungi il payload JSON del trigger.

6. Fai clic su **Distribuisci**.

Dopo la distribuzione del template, puoi apportare ulteriori modifiche al codice per personalizzarlo secondo necessità oppure tornare indietro e consultare il catalogo dei template disponibili.

### Distribuzione del template Periodic Slack Reminder dalla CLI
{: #slack-reminder-template-cli}

1. Vai a https://api.slack.com/incoming-webhooks per configurare l'URL dei webhook in entrata richiesto.

1. Clona il repository del template.
    ```
    git clone https://github.com/ibm-functions/template-reminder-slack.git
    ```
    {: pre}

2. Passa alla directory per il runtime dell'azione che vuoi utilizzare: `nodejs-6`, `nodejs`, `php`, `python` o `swift`.
    ```
    cd template-reminder-slack/runtimes/nodejs
    ```
    {: pre}

3. Distribuisci il template, utilizzando le seguenti variabili di ambiente.
    ```
    SLACK_WEBHOOK_URL=<url> ALARM_CRON=<cron> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>Descrizione delle variabili di ambiente</caption>
    <thead>
    <th colspan=2>Descrizione delle variabili di ambiente</th>
    </thead>
    <tbody>
    <tr><td><code>SLACK_WEBHOOK_URL</code></td><td>L'URL del webhook, ad esempio <code>https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX</code></td></tr>
    <tr><td><code>ALARM_CRON</code></td><td>Una sequenza cron basata sulla <a href="http://crontab.org">sintassi crontab UNIX</a>. Utilizza 5 o meno campi nel formato <code>X X X X X</code>.</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>Un nome personalizzato per il pacchetto</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>Un nome personalizzato per la regola</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>Un nome personalizzato per il trigger</td></tr>
    </tbody></table>


