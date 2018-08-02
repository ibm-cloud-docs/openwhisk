---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Provider di eventi personalizzati
{: #openwhisk_feeds}

{{site.data.keyword.openwhisk_short}} supporta un'API aperta, in cui qualsiasi utente può esporre un servizio di produzione di eventi sotto forma di feed in un pacchetto. La seguente sezione descrive le opzioni di architettura e implementazione per fornire il tuo feed personalizzato.
{: shortdesc}

Questo materiale è destinato agli utenti esperti di {{site.data.keyword.openwhisk_short}} che intendono pubblicare i propri feed. La maggior parte degli utenti di {{site.data.keyword.openwhisk_short}} può saltare tranquillamente la seguente sezione relativa all'architettura.

## Architettura dei feed

Esistono almeno tre modelli architetturali per la creazione di un feed: **Hook**, **Polling** e **Connessioni**.

### Hook
Nel modello *Hook*, un feed viene configurato utilizzando una funzione [webhook](https://en.wikipedia.org/wiki/Webhook) esposta da un altro servizio.   In questa strategia, un webhook viene configurato su un servizio esterno per consentire di pubblicare direttamente in un URL per attivare un trigger. Questo metodo è di gran lunga l'opzione più semplice e interessante per implementare feed a bassa frequenza.

<!-- The github feed is implemented using webhooks.  Put a link here when we have the open repo ready -->

### Polling
Nel modello "Polling", un'azione {{site.data.keyword.openwhisk_short}} è organizzata per eseguire il polling periodico di un endpoint per il recupero di nuovi dati. Questo modello è relativamente semplice da costruire, ma le frequenze degli eventi sono limitate dall'intervallo di polling.

### Connessioni
Nel modello "Connessioni", un servizio separato mantiene una connessione persistente a un'origine di feed. L'implementazione basata sulla connessione potrebbe interagire con un endpoint del servizio utilizzando intervalli di polling lunghi o per impostare una notifica di push.

<!-- Our cloudant changes feed is connection based.  Put a link here to
an open repo -->

<!-- What is the foundation for the Message Hub feed? If it is "connections" then lets put a link here as well -->

## Differenza tra feed e trigger

I feed e i trigger sono strettamente correlati, ma restano due concetti tecnicamente distinti.   

- {{site.data.keyword.openwhisk_short}} elabora gli **eventi** che fluiscono nel sistema.

- Un **trigger** è tecnicamente un nome per una classe di eventi. Ogni evento appartiene esattamente a un solo trigger; per analogia, un trigger assomiglia a un *argomento* nei sistemi di pubblicazione/sottoscrizione basati sugli argomenti. Una **regola** *T -> A* significa che "ogni volta che un evento arriva da un trigger *T*, si richiama l'azione *A* con il payload del trigger.

- Un **feed** è un flusso di eventi che appartengono tutti a un certo trigger *T*. Un feed è controllato da un'**azione di feed**, che gestisce la creazione, l'eliminazione, la pausa e il ripristino del flusso di eventi che comprendono un feed. L'azione di feed in genere interagisce con i servizi esterni che generano gli eventi, utilizzando un'API REST che gestisce le notifiche.

##  Implementazione delle azioni di feed

L'*azione di feed* è una normale *azione* {{site.data.keyword.openwhisk_short}} e accetta i seguenti parametri:
* **lifecycleEvent**: un valore tra 'CREATE', 'READ', 'UPDATE', 'DELETE', 'PAUSE' o 'UNPAUSE'.
* **triggerName**: il nome completo del trigger, che contiene gli eventi prodotti da questo feed.
* **authKey**: le credenziali per l'autenticazione di base dell'utente {{site.data.keyword.openwhisk_short}} proprietario del trigger.

L'azione di feed può anche accettare qualsiasi altro parametro di cui ha bisogno per gestire il feed. Ad esempio, l'azione di feed relativa alle modifiche {{site.data.keyword.cloudant}} prevede di ricevere parametri che includono *'dbname'*, *'username'* e così via.

Quando l'utente crea un trigger dalla CLI con il parametro **--feed**, il sistema richiama automaticamente l'azione di feed con i parametri appropriati.

Ad esempio, supponiamo che l'utente crei un bind **mycloudant** per il pacchetto `cloudant` con un nome utente e una password come parametri associati. Quando l'utente immette il seguente comando dalla CLI:
```
ibmcloud fn trigger create T --feed mycloudant/changes -p dbName myTable
```
{: pre}

Il sistema effettua implicitamente un comando equivalente al seguente:
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

L'azione di feed denominata *changes* accetta questi parametri e si prevede che esegua qualsiasi azione necessaria per configurare un flusso di eventi da {{site.data.keyword.cloudant_short_notm}}. L'azione di feed si verifica utilizzando la configurazione appropriata, che è diretta al trigger *T*.

Per il feed *changes* di {{site.data.keyword.cloudant_short_notm}}, l'azione avviene per comunicare direttamente con un servizio *trigger {{site.data.keyword.cloudant_short_notm}}* che è implementato con un'architettura basata sulla connessione.

Un protocollo analogo di azione di feed si verifica per `ibmcloud fn trigger delete`, `ibmcloud fn trigger update` e `ibmcloud fn trigger get`.

## Implementazione di feed con gli hook

È facile configurare un feed mediante un hook se il produttore eventi supporta una funzione webhook/callback.

Con questo metodo, _non è necessario_ impostare alcun servizio persistente al di fuori di {{site.data.keyword.openwhisk_short}}. Tutta la gestione dei feed avviene naturalmente tramite le *azioni di feed* {{site.data.keyword.openwhisk_short}} senza stato, che negoziano direttamente con un'API webhook di terze parti.

Quando richiamata con `CREATE`, l'azione di feed installa semplicemente un webhook per qualche altro servizio, chiedendo al servizio remoto di pubblicare le notifiche all'ULR `fireTrigger` appropriato in {{site.data.keyword.openwhisk_short}}.

Il webhook è diretto a inviare notifiche a un URL come:

`POST /namespaces/{namespace}/triggers/{triggerName}`

Il modulo con la richiesta POST viene interpretato come documento JSON che definisce i parametri sull'evento trigger. Le regole {{site.data.keyword.openwhisk_short}} passano questi parametri trigger a qualsiasi azione da attivare come risultato dell'evento.

## Implementazione di feed con il polling

È possibile configurare un'*azione* {{site.data.keyword.openwhisk_short}} per eseguire il polling di un'origine di feed interamente all'interno di {{site.data.keyword.openwhisk_short}}, senza la necessità di impostare connessioni persistenti o servizi esterni.

Per i feed in cui non è disponibile un webhook, ma che non richiedono tempi di risposta a bassa latenza o di volume elevato, il polling risulta essere un'opzione interessante.

Per configurare un feed basato sul polling, l'azione di feed effettua i seguenti passi quando viene richiamata con `CREATE`:

1. L'azione di feed configura un trigger periodico (*T*) con la frequenza desiderata, utilizzando il feed `whisk.system/alarms`.
2. Lo sviluppatore del feed crea un'azione `pollMyService` che esegue il polling del servizio remoto e restituisce eventuali nuovi eventi.
3. L'azione di feed configura una *regola* *T -> pollMyService*.

Questa procedura implementa un trigger basato sul polling utilizzando esclusivamente azioni {{site.data.keyword.openwhisk_short}}, senza alcun bisogno di un servizio separato.

## Implementazione di feed mediante le connessioni

Le due precedenti scelte architetturali sono semplici e facili da implementare. Tuttavia, se vuoi ottenere un feed ad alte prestazioni, non esiste alternativa alle connessioni persistenti e polling lungo o tecniche simili.

Poiché le azioni {{site.data.keyword.openwhisk_short}} devono essere di breve esecuzione, un'azione non può mantenere una connessione persistente a una terza parte. Puoi, invece, impostare un servizio separato, chiamato *servizio provider*, al di fuori di {{site.data.keyword.openwhisk_short}} che resti sempre in esecuzione. Un servizio provider può mantenere connessioni a origini eventi di terze parti che supportano il polling lungo o altre notifiche basate sulla connessione.

Il servizio provider ha un'API REST che consente all'*azione di feed* {{site.data.keyword.openwhisk_short}} di controllare il feed. Il servizio provider funge da proxy tra il provider di eventi e {{site.data.keyword.openwhisk_short}}. Quando riceve gli eventi dalla terza parte, li invia a {{site.data.keyword.openwhisk_short}} attivando un trigger.

Il feed *changes* di {{site.data.keyword.cloudant_short_notm}} è l'esempio canonico in quanto imposta un servizio `cloudanttrigger`, che si interpone tra le notifiche {{site.data.keyword.cloudant_short_notm}} su una connessione persistente e i trigger {{site.data.keyword.openwhisk_short}}.
<!-- TODO: add a reference to the open source implementation -->

Il feed *alarm* viene implementato con un modello simile.

L'architettura basata sulla connessione è l'opzione più performante, ma impone un sovraccarico maggiore sulle operazioni rispetto alle architetture di polling e hook.
