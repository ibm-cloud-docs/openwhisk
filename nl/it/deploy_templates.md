---

copyright:

  years: 2018

lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Distribuzione di template in {{site.data.keyword.openwhisk_short}}
{: #serviceauth}

{{site.data.keyword.openwhisk}} offre un catalogo di template per aiutarti a iniziare rapidamente il tuo prossimo progetto. I template sono una combinazione di azioni, trigger, sequenze e possono anche incorporare istanze di servizi da {{site.data.keyword.Bluemix}}. Utilizzando i template, puoi creare rapidamente e facilmente un progetto e iniziare subito la codifica. 

Questa esercitazione ti guida attraverso la distribuzione del template Cloudant.
{: shortdesc}

## Template disponibili
{: #available-templates}

| Nome | Descrizione | Runtime supportati | 
|:-----------------|:-----------------|:-----------------|
| Cloudant Events | Quando in un database DB viene modificato o aggiunto un documento, registra la modifica nella console. | Node.js, Swift, Python, PHP |
| Get HTTP Resource | Un'azione web che viene richiamata in risposta a un evento HTTP e quindi recupera i dati dall'API Yahoo Meteo. | Node.js, Python |
| Hello World | Questa azione accetterà un singolo parametro, che deve essere un oggetto JSON. | Node.js, Swift, Python, PHP |
| Message Hub Events | Quando in un argomento di Hub dei messaggi vengono aggiunti nuovi dati, registra la modifica nella console. | Node.js, Swift, Python, PHP | 
| Periodic Slack Reminder | Un'azione che pubblicherà su Slack in base a un trigger periodico. | Node.js, Swift, Python, PHP |

## Distribuzione del template Cloudant Events
{: #cloudant-template}

Il template Cloudant crea una sequenza di azioni e un trigger che avvierà tale sequenza. Il trigger viene attivato quando si verifica una modifica nel database Cloudant connesso, che dovrebbe essere un database di CAT, con un nome e un colore. L'elemento di dati previsto è un CAT, con un nome e un colore definiti. Quando nel database viene aggiunto un nuovo CAT o modificato uno corrente, i dati verranno registrati nella console.

1. Per creare un template, vai a [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://dev-console.stage1.bluemix.net/openwhisk/) e fai quindi clic su **Start Creating**. 

2. Fai clic su **Deploy Template**.

3. Fai clic su **New Cloudant Item**.

### Creazione dell'azione Cloudant

1. Successivamente, fornisci un nome per il tuo pacchetto o utilizza il nome predefinito fornito `new-cloudant-item`. 

2. Sotto l'elenco a discesa **Actions**, seleziona il runtime per le azioni che possiederai (nodejs, swift, python o php). In questo esempio, seleziona **nodejs** e fai clic su **Next**.

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

1. Fai clic su **Deploy**.

Dopo la distribuzione del template, puoi apportare ulteriori modifiche al codice per personalizzarlo secondo necessità oppure tornare indietro e consultare il catalogo dei template disponibili.

