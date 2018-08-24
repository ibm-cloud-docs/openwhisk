---

copyright:
  years: 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Distribuzione dei template Quickstart
{: #serviceauth}

{{site.data.keyword.openwhisk}} offre un catalogo di template per aiutarti a iniziare rapidamente il tuo prossimo progetto. I template sono una combinazione di azioni, trigger, sequenze e possono anche incorporare istanze di servizi da {{site.data.keyword.Bluemix}}. Utilizzando i template, puoi creare rapidamente e facilmente un progetto e iniziare subito la codifica.

Questa esercitazione ti guida attraverso la distribuzione dei template disponibili tramite {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

## Template Quickstart disponibili
{: #available-templates}

| Nome | Descrizione | Runtime supportati |
|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} Events](./deploy_templates.html#cloudant-template) | Quando in un documento {{site.data.keyword.cloudant}} viene modificato o aggiunto, registra la modifica nella console. | Node.js, Swift, Python, PHP |
| [Upload Image](./deploy_templates.html#cos-upload-image) | Un'azione web che ti consente di caricare un'immagine in un bucket dell'istanza {{site.data.keyword.cos_full}} e poi di richiamare una miniatura di tale immagine. | Node.js |
| [Get HTTP Resource](./deploy_templates.html#get-http-resource-template) | Un'azione web che viene richiamata in risposta a un evento HTTP e quindi recupera i dati dall'API Yahoo Weather. | Node.js, Python |
| [Hello World](./deploy_templates.html#hello-world-template) | Questa azione accetterà un singolo parametro, che deve essere un oggetto JSON. | Node.js, Swift, Python, PHP |
| [{{site.data.keyword.messagehub}} Events](./deploy_templates.html#messagehub-events-template) | Quando in un argomento di {{site.data.keyword.messagehub_full}} vengono aggiunti nuovi dati, registra la modifica nella console. | Node.js, Swift, Python, PHP |
| [Periodic Slack Reminder](./deploy_templates.html#slack-reminder-template) | Un'azione che pubblicherà su Slack in base a un trigger periodico. | Node.js, Swift, Python, PHP |

## Distribuzione del template {{site.data.keyword.cloudant_short_notm}} Events
{: #cloudant-template}

Il template {{site.data.keyword.cloudant_short_notm}} crea una sequenza di azioni e un trigger che avvierà tale sequenza. Il trigger viene attivato quando si verifica una modifica nel database {{site.data.keyword.cloudant_short_notm}} connesso, che dovrebbe essere un database di gatti, con un nome e un colore. L'elemento di dati previsto è un gatto, con un nome e un colore definiti. Quando nel database viene aggiunto un nuovo gatto o modificato uno attuale, i dati verranno registrati nella console.

1. Per creare un template, vai a [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) e fai quindi clic su **Avvia creazione**.

2. Fai clic su **Template Quickstart**.

3. Fai clic su **Cloudant Events**.

### Creazione dell'azione {{site.data.keyword.cloudant_short_notm}}

1. Successivamente, fornisci un nome per il tuo pacchetto o utilizza il nome predefinito fornito `new-cloudant-item`.

2. Nell'elenco a discesa **Azioni**, seleziona il runtime per le azioni che possiederai (nodejs, swift, python o php). In questo esempio, seleziona **nodejs** e fai clic su **Avanti**.

### Creazione del trigger {{site.data.keyword.cloudant_short_notm}}

I trigger richiamano le azioni quando ricevono eventi dalle origini evento. Per creare un trigger per il template {{site.data.keyword.cloudant_short_notm}}, fornisci il trigger con le informazioni sull'istanza del servizio {{site.data.keyword.cloudant_short_notm}} richieste.

#### Crea l'istanza del servizio {{site.data.keyword.cloudant_short_notm}}

Puoi scegliere di:
  * **Creare la tua propria istanza**
  * **Immettere le tue credenziali**

1. In questo esempio, scegli di **Creare la tua propria istanza**.

2. Si apre un menu a comparsa che ti porta in una nuova scheda con la pagina di configurazione {{site.data.keyword.cloudant_short_notm}}. Dopo aver creato l'istanza {{site.data.keyword.cloudant_short_notm}}, devi creare una serie di credenziali di servizio e quindi chiudere la scheda per tornare alla pagina facendo clic su **Ok**.

3. Ora scegli di **Immettere le tue credenziali** e fornisci le seguenti informazioni:
  * Nome utente - _il tuo nome utente {{site.data.keyword.cloudant_short_notm}}_
  * Password - _la tua password {{site.data.keyword.cloudant_short_notm}}_
  * Host - _Questo è di solito il tuo `nome utente {{site.data.keyword.cloudant_short_notm}}.com`_
  * Database - _Il nome del tuo database {{site.data.keyword.cloudant_short_notm}}_

### Distribuisci il template {{site.data.keyword.cloudant_short_notm}}

Fai clic su **Distribuisci**.

Dopo la distribuzione del template, puoi apportare ulteriori modifiche al codice per personalizzarlo secondo necessità oppure tornare indietro e consultare il catalogo dei template disponibili.

## Distribuzione del template Upload Image
{: #cos-upload-image}

Il template Upload Image crea un'azione web che ti consente di caricare un'immagine in un bucket {{site.data.keyword.cos_short_notm}} tramite una piccola interfaccia. Il template richiama quindi l'immagine come una miniatura e la visualizza nell'interfaccia dell'azione web.

Per distribuire il template:

1. Vai alla console {{site.data.keyword.openwhisk_short}} in [{{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/openwhisk/).

2. Fai clic su **Avvia creazione**.

2. Fai clic su **Template Quickstart**.

3. Fai clic sul template **Upload Image**.

4. Immetti un nome per il tuo pacchetto o utilizza il nome predefinito `upload-image`.

5. Fai clic su **Avanti**.

6. Il template richiede le credenziali del servizio da un'istanza del servizio {{site.data.keyword.cos_full_notm}}. Nell'elenco **{{site.data.keyword.cos_short}}**, seleziona una delle seguenti opzioni:
  * **Crea una nuova istanza**: se non hai un'istanza del servizio esistente, seleziona questa opzione per crearne una.
      1. Nella pagina di creazione dell'istanza del servizio {{site.data.keyword.cos_full_notm}} che viene aperta, crea un'istanza del servizio.
      2. [Crea una serie di credenziali del servizio HMAC](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials).
      3. [Crea almeno un bucket](/docs/services/cloud-object-storage/getting-started.html#create-buckets).
  * **Immetti le tue credenziali**: seleziona questa opzione per immettere manualmente le tue credenziali per un'istanza del servizio {{site.data.keyword.cos_short}}. Le credenziali devono avere le chiavi HMAC e le istanze del servizio devono avere almeno un bucket.
  * **Istanze esistenti**: se hai qualche istanza {{site.data.keyword.cos_short}} esistente, seleziona una delle istanze dall'elenco. Le credenziali devono avere le chiavi HMAC e le istanze del servizio devono avere almeno un bucket.

7. Fai clic su **Distribuisci**.

8. Nella navigazione sulla sinistra, fai clic su **Endpoint**.

9. Nella sezione Azione web, copia il link senza il suffisso .json e incollalo nella barra degli indirizzi del tuo browser. Viene visualizzata l'interfaccia per l'azione web del template.

10. Facoltativo: dopo la distribuzione del template, puoi passare al dashboard Azioni per personalizzare il codice nei due nuovi pacchetti:
    * Il pacchetto `cloud-object-storage`, che contiene le azioni che funzionano con le istanze {{site.data.keyword.cos_short}}
    * Il pacchetto di template (nome predefinito `upload-image`), che contiene l'azione `app`

## Distribuzione del template Get HTTP Resource
{: #get-http-resource-template}

Il template Get HTTP Resource crea un'azione per recuperare una risorsa esterna, l'API Yahoo Weather, e quindi restituisce i dati. L'azione è abilitata come azione web, permettendone la chiamata con un URL abilitato a CORS e che non richiede una chiave di autenticazione, utile per creare backend per le applicazioni web. **Nota**: per impostazione predefinita, l'endpoint `get-http-resource` è pubblicamente disponibile per chiunque voglia richiamarlo.

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

## Distribuzione del template {{site.data.keyword.messagehub}} Events
{: #messagehub-events-template}

Il template {{site.data.keyword.messagehub}} Events crea un'azione e un trigger che avvia tale azione. Il trigger viene attivato ogni volta che viene aggiunto un nuovo elemento all'argomento {{site.data.keyword.messagehub}} scelto durante la creazione del template.

1. Per creare un template, vai a [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) e fai quindi clic su **Avvia creazione**.

2. Fai clic su **Template Quickstart**.

3. Esamina il campo **Nome pacchetto** e aggiornalo secondo necessità. Il valore predefinito è impostato su `message-hub-events`.

4. Scegli il runtime per le azioni che possiederai: Node.js 8, Node.js 6, Python 3, Swift 4 o PHP 7.1.

5. Fai clic su **Avanti**.

### Creazione del trigger {{site.data.keyword.messagehub}}

I trigger richiamano le azioni quando ricevono eventi dalle origini evento. Per creare un trigger per il template {{site.data.keyword.messagehub}}, fornisci il trigger con le informazioni sull'istanza del servizio {{site.data.keyword.messagehub}} richieste.

Esamina il campo **Nome trigger** e aggiornalo secondo necessità. Il valore predefinito è impostato su `message-hub-events-trgr`.

### Creazione dell'istanza del servizio {{site.data.keyword.messagehub}}

Puoi scegliere di:
  * **Creare la tua propria istanza**
  * **Immettere le tue credenziali**

1. In questo esempio, scegli di **Creare la tua propria istanza**.

2. Si apre un menu a comparsa che ti porta in una nuova scheda con la pagina di configurazione {{site.data.keyword.messagehub}}. Dopo aver creato l'istanza {{site.data.keyword.messagehub}}, devi creare una serie di credenziali di servizio e quindi chiudere la scheda per tornare alla pagina facendo clic su **Ok**.

3. Ora scegli di **Immettere le tue credenziali** e fornisci le seguenti informazioni:
  * Nome utente - _il tuo nome utente {{site.data.keyword.messagehub}}_
  * Password - _la tua password {{site.data.keyword.messagehub}}_
  * kafka_admin_url - _il tuo URL REST di gestione {{site.data.keyword.messagehub}}_
  * Database - _Il nome del tuo database {{site.data.keyword.messagehub}}_
  * Argomento - _Argomento a cui sottoscriversi_

### Distribuisci il template {{site.data.keyword.messagehub}}

Fai clic su **Distribuisci**.

Dopo la distribuzione del template, puoi apportare ulteriori modifiche al codice per personalizzarlo secondo necessità oppure tornare indietro e consultare il catalogo dei template disponibili.

## Distribuzione del template Periodic Slack Reminder
{: #slack-reminder-template}

Il template Periodic Slack Reminder pubblica in Slack sulla base di un intervallo fornito dall'utente durante la creazione del trigger. Prima di creare questo template, vai all'indirizzo https://api.slack.com/incoming-webhooks per configurare l'URL dei webhook in entrata richiesto.

1. Esamina il campo **Nome pacchetto** e aggiornalo secondo necessità. Il valore predefinito è impostato su `periodic-slack-reminder`.

2. Scegli il runtime per le azioni che possiederai: Node.js 8, Node.js 6, Python 3, Swift 4 o PHP 7.1.

3. Nella sezione **Parametri**, immetti l'URL webhook nel campo **Valore parametro** e fai clic su **Avanti**. (Esempio: https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX).

### Creazione del trigger Slack Reminder

I trigger richiamano le azioni quando ricevono eventi dalle origini evento. Per creare un trigger per il template Slack Reminder, fornisci il trigger con le informazioni sull'istanza del servizio {{site.data.keyword.messagehub}} richieste.

1. Esamina il campo **Nome trigger** e aggiornalo secondo necessità. Il valore predefinito è impostato su `periodic-slack-reminder-trgr`.

2. Successivamente, puoi specificare l'intervallo in cui attivare il trigger utilizzando un'espressione Pattern o Cron. Puoi selezionare le ore UTC per giorni feriali, ore e minuti. Seleziona le opzioni di intervallo desiderate e sarai pronto per la distribuzione del template.

3. Fai clic su **Distribuisci**.

Dopo la distribuzione del template, puoi apportare ulteriori modifiche al codice per personalizzarlo secondo necessità oppure tornare indietro e consultare il catalogo dei template disponibili.
