---

copyright:

  years: 2018

lastupdated: "2018-05-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Distribuzione dei template Quickstart
{: #serviceauth}

{{site.data.keyword.openwhisk}} offre un catalogo di template per aiutarti a iniziare rapidamente il tuo prossimo progetto. I template sono una combinazione di azioni, trigger, sequenze e possono anche incorporare istanze di servizi da {{site.data.keyword.Bluemix}}. Utilizzando i template, puoi creare rapidamente e facilmente un progetto e iniziare subito la codifica.

Questa esercitazione ti guida attraverso la distribuzione del template Cloudant.
{: shortdesc}

## Template Quickstart disponibili
{: #available-templates}

| Nome | Descrizione | Runtime supportati |
|:-----------------|:-----------------|:-----------------|
| [Cloudant Events](./deploy_templates.html#cloudant-template) | Quando in un database DB viene modificato o aggiunto un documento, registra la modifica nella console. | Node.js, Swift, Python, PHP |
| [Get HTTP Resource](./deploy_templates.html#get-http-resource-template) | Un'azione web che viene richiamata in risposta a un evento HTTP e quindi recupera i dati dall'API Yahoo Meteo. | Node.js, Python |
| [Hello World](./deploy_templates.html#hello-world-template) | Questa azione accetterà un singolo parametro, che deve essere un oggetto JSON. | Node.js, Swift, Python, PHP |
| [Message Hub Events](./deploy_templates.html#messagehub-events-template) | Quando in un argomento di Message Hub vengono aggiunti nuovi dati, registra la modifica nella console. | Node.js, Swift, Python, PHP |
| [Periodic Slack Reminder](./deploy_templates.html#slack-reminder-template) | Un'azione che pubblicherà su Slack in base a un trigger periodico. | Node.js, Swift, Python, PHP |

## Distribuzione del template Cloudant Events
{: #cloudant-template}

Il template Cloudant crea una sequenza di azioni e un trigger che avvierà tale sequenza. Il trigger viene attivato quando si verifica una modifica nel database Cloudant connesso, che dovrebbe essere un database di CAT, con un nome e un colore. L'elemento di dati previsto è un CAT, con un nome e un colore definiti. Quando nel database viene aggiunto un nuovo CAT o modificato uno corrente, i dati verranno registrati nella console.

1. Per creare un template, vai a [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) e fai quindi clic su **Avvia creazione**.

2. Fai clic su **Template Quickstart**.

3. Fai clic su **Nuovo elemento Cloudant**.

### Creazione dell'azione Cloudant

1. Successivamente, fornisci un nome per il tuo pacchetto o utilizza il nome predefinito fornito `new-cloudant-item`.

2. Nell'elenco a discesa **Azioni**, seleziona il runtime per le azioni che possiederai (nodejs, swift, python o php). In questo esempio, seleziona **nodejs** e fai clic su **Avanti**.

### Creazione del trigger Cloudant

I trigger richiamano le azioni quando ricevono eventi dalle origini evento. Per creare un trigger per il template Cloudant, fornisci al trigger le informazioni sull'istanza del servizio Cloudant richieste.

#### Crea l'istanza del servizio Cloudant

Puoi scegliere di:
  * **Creare la tua propria istanza**
  * **Immettere le tue credenziali**

1. In questo esempio, scegli di **Creare la tua propria istanza**.

2. Si apre un menu a comparsa che ti porta in una nuova scheda con la pagina di configurazione Cloudant. Dopo aver creato l'istanza Cloudant, devi creare una serie di credenziali di servizio e quindi chiudere la scheda per tornare alla pagina facendo clic su **Ok**.

3. Ora scegli di **Immettere le tue credenziali** e fornisci le seguenti informazioni:
  * Username - _Il tuo nome utente Cloudant_
  * Password - _La tua password Cloudant_
  * Host - _ Questo è di solito `username.cloudant.com`_
  * Database - _Il nome del tuo database Cloudant_

### Distribuisci il template Cloudant

Fai clic su **Distribuisci**.

Dopo la distribuzione del template, puoi apportare ulteriori modifiche al codice per personalizzarlo secondo necessità oppure tornare indietro e consultare il catalogo dei template disponibili.

## Distribuzione del template Get HTTP Resource
{: #get-http-resource-template}

Il template Get HTTP Resource crea un'azione per recuperare una risorsa esterna, l'API Yahoo Meteo, e quindi restituisce i dati. L'azione è abilitata come azione web, permettendone il richiamo con un URL abilitato a CORS e che non richiede una chiave di autenticazione, utile per creare backend per le applicazioni web. **Nota**: per impostazione predefinita, l'endpoint `get-http-resource` è pubblicamente disponibile per chiunque voglia richiamarlo.

1. Per creare un template, vai a [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) e fai quindi clic su **Avvia creazione**.

2. Fai clic su **Template Quickstart**.

3. Esamina il campo **Nome pacchetto** e aggiornalo secondo necessità. Il valore predefinito è impostato su `get-http-resource`.

4. Scegli il runtime per le azioni che possiederai: Node.js 8, Node.js 6 o Python 3.

5. Fai clic su **Distribuisci**.

Dopo la distribuzione del template, puoi apportare ulteriori modifiche al codice per personalizzarlo secondo necessità oppure tornare indietro e consultare il catalogo dei template disponibili.

## Distribuzione del template Hello World
{: #hello-world-template}

Questa azione accetterà un singolo parametro, che deve essere un oggetto JSON.

1. Per creare un template, vai a [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) e fai quindi clic su **Avvia creazione**.

2. Fai clic su **Template Quickstart**.

3. Esamina il campo **Nome pacchetto** e aggiornalo secondo necessità. Il valore predefinito è impostato su `hello-world`.

4. Scegli il runtime per le azioni che possiederai: Node.js 8, Node.js 6, Python 3, Swift 4 o PHP 7.1.

5. Fai clic su **Distribuisci**.

Dopo la distribuzione del template, puoi apportare ulteriori modifiche al codice per personalizzarlo secondo necessità oppure tornare indietro e consultare il catalogo dei template disponibili.

## Distribuzione del template Message Hub Events
{: #messagehub-events-template}

Il template Message Hub Events crea un'azione e un trigger che avvia tale azione. Il trigger viene attivato ogni volta che viene aggiunto un nuovo elemento all'argomento Message Hub scelto durante la creazione del template.

1. Per creare un template, vai a [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) e fai quindi clic su **Avvia creazione**.

2. Fai clic su **Template Quickstart**.

3. Esamina il campo **Nome pacchetto** e aggiornalo secondo necessità. Il valore predefinito è impostato su `message-hub-events`.

4. Scegli il runtime per le azioni che possiederai: Node.js 8, Node.js 6, Python 3, Swift 4 o PHP 7.1.

5. Fai clic su **Avanti**.

### Creazione del trigger Message Hub

I trigger richiamano le azioni quando ricevono eventi dalle origini evento. Per creare un trigger per il template Message Hub, fornisci al trigger le informazioni sull'istanza del servizio Message Hub richieste.

Esamina il campo **Nome trigger** e aggiornalo secondo necessità. Il valore predefinito è impostato su `message-hub-events-trgr`.

### Creazione dell'istanza del servizio Message Hub

Puoi scegliere di:
  * **Creare la tua propria istanza**
  * **Immettere le tue credenziali**

1. In questo esempio, scegli di **Creare la tua propria istanza**.

2. Si apre un menu a comparsa che ti porta in una nuova scheda con la pagina di configurazione di Message Hub. Dopo aver creato l'istanza Message Hub, devi creare una serie di credenziali di servizio e quindi chiudere la scheda per tornare alla pagina facendo clic su **Ok**.

3. Ora scegli di **Immettere le tue credenziali** e fornisci le seguenti informazioni:
  * Username - _Il tuo nome utente Message Hub_
  * Password - _La tua password Message Hub_
  * kafka_admin_url - _Il tuo URL REST di gestione Message Hub_
  * Database - _Il nome del tuo database Message Hub_
  * Topic - _Argomento a cui sottoscriversi_

### Distribuisci il template Message Hub

Fai clic su **Distribuisci**.

Dopo la distribuzione del template, puoi apportare ulteriori modifiche al codice per personalizzarlo secondo necessità oppure tornare indietro e consultare il catalogo dei template disponibili.

## Distribuzione del template Periodic Slack Reminder
{: #slack-reminder-template}

Il template Periodic Slack Reminder pubblica in Slack sulla base di un intervallo fornito dall'utente durante la creazione del trigger. Prima di creare questo template, vai all'indirizzo https://api.slack.com/incoming-webhooks per configurare l'URL dei webhook in entrata richiesto.

1. Esamina il campo **Nome pacchetto** e aggiornalo secondo necessità. Il valore predefinito è impostato su `periodic-slack-reminder`.

2. Scegli il runtime per le azioni che possiederai: Node.js 8, Node.js 6, Python 3, Swift 4 o PHP 7.1.

3. Nella sezione **Parametri**, immetti l'URL webhook nel campo **Valore parametro** e fai clic su **Avanti**. (Esempio: https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX).

### Creazione del trigger Slack Reminder

I trigger richiamano le azioni quando ricevono eventi dalle origini evento. Per creare un trigger per il template Slack Reminder, fornisci al trigger le informazioni sull'istanza del servizio Message Hub richieste.

1. Esamina il campo **Nome trigger** e aggiornalo secondo necessità. Il valore predefinito è impostato su `periodic-slack-reminder-trgr`.

2. Successivamente, puoi specificare l'intervallo in cui attivare il trigger utilizzando un'espressione Pattern o Cron. Puoi selezionare le ore UTC per giorni feriali, ore e minuti. Seleziona le opzioni di intervallo desiderate e sarai pronto per la distribuzione del template.

3. Fai clic su **Distribuisci**.

Dopo la distribuzione del template, puoi apportare ulteriori modifiche al codice per personalizzarlo secondo necessità oppure tornare indietro e consultare il catalogo dei template disponibili.
