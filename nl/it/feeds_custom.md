---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: feeds, serverless

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Creazione di feed di provider di eventi personalizzati
{: #feeds_custom}

{{site.data.keyword.openwhisk_short}} supporta un'API aperta, in cui qualsiasi utente può esporre un servizio di produzione di eventi sotto forma di feed in un pacchetto.
{: shortdesc}


## Architettura dei feed
{: #feeds_arch}

Esistono tre modelli architetturali per la creazione di un feed: **Hook**, **Polling** e **Connessioni**.

### Hook

Con il modello hook, un feed viene configurato utilizzando un [webhook](https://en.wikipedia.org/wiki/Webhook) esposto da un altro servizio. In questa strategia, un webhook viene configurato su un servizio esterno per consentire di pubblicare direttamente in un URL per attivare un trigger. Questo metodo è di gran lunga l'opzione più semplice e interessante per implementare feed a bassa frequenza.

Ad esempio, il [pacchetto Github](/docs/openwhisk?topic=cloud-functions-pkg_github)  e il [pacchetto Push notification](/docs/openwhisk?topic=cloud-functions-pkg_push_notifications) utilizzano un webhook.


### Polling

Nel modello polling, un'azione {{site.data.keyword.openwhisk_short}} è organizzata per eseguire il polling periodico di un endpoint per il recupero di nuovi dati. Questo modello è relativamente semplice da costruire, ma le frequenze degli eventi sono limitate dall'intervallo di polling.

### Connessioni

Con il modello connessioni, un servizio distinto mantiene una connessione persistente con un'origine di feed. L'implementazione basata sulla connessione potrebbe interagire con un endpoint del servizio utilizzando intervalli di polling lunghi o per configurare una notifica di push.

Ad esempio, il [pacchetto {{site.data.keyword.cloudant}}](/docs/openwhisk?topic=cloud-functions-pkg_cloudant) utilizza il modello connessioni.



##  Implementazione delle azioni di feed
{: #feeds_actions}

L'azione di feed è un'azione e accetta i seguenti parametri:
* **lifecycleEvent**: 'CREATE', 'READ', 'UPDATE', 'DELETE', 'PAUSE' o 'UNPAUSE'.
* **triggerName**: il nome completo del trigger, che contiene gli eventi prodotti da questo feed.
* **authKey**: le credenziali per l'autenticazione di base dell'utente {{site.data.keyword.openwhisk_short}} proprietario del trigger.

L'azione di feed può anche accettare qualsiasi altro parametro di cui ha bisogno per gestire il feed. Ad esempio, l'azione di feed relativa alle modifiche {{site.data.keyword.cloudant}} prevede di ricevere parametri che includono `dbname` e `username`.

Quando l'utente crea un trigger dalla CLI con il parametro `--feed`, l'azione di feed viene richiamata automaticamente con i parametri appropriati.

Ad esempio, un utente crea un bind **mycloudant** per il pacchetto `cloudant` utilizzando un nome utente e una password come parametri associati. Quando l'utente immette il seguente comando dalla CLI:
```
ibmcloud fn trigger create my_cloudant_trigger --feed mycloudant/changes -p dbName myTable
```
{: pre}

Quindi, viene eseguito un comando equivalente al seguente:
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

L'azione di feed denominata *changes* accetta questi parametri e si prevede che esegua qualsiasi azione necessaria per configurare un flusso di eventi da {{site.data.keyword.cloudant_short_notm}}. L'azione di feed avviene utilizzando la configurazione appropriata, che è diretta al trigger.

Per il feed *changes* di {{site.data.keyword.cloudant_short_notm}}, l'azione comunica direttamente con un servizio *trigger {{site.data.keyword.cloudant_short_notm}}* implementato con un'architettura basata sulla connessione.

Un protocollo analogo di azione di feed si verifica per `ibmcloud fn trigger delete`, `ibmcloud fn trigger update` e `ibmcloud fn trigger get`.

## Implementazione di feed con hook
{: #feeds_hooks}

Configura un feed mediante un hook se un produttore eventi supporta una funzione di webhook/callback.

Con questo metodo, non devi impostare alcun servizio persistente al di fuori di {{site.data.keyword.openwhisk_short}}. Tutta la gestione dei feed avviene naturalmente tramite le *azioni di feed* {{site.data.keyword.openwhisk_short}} senza stato, che negoziano direttamente con un'API webhook di terze parti.

Quando richiamata con `CREATE`, l'azione di feed installa semplicemente un webhook per qualche altro servizio, chiedendo al servizio remoto di pubblicare (POST) le notifiche all'URL `fireTrigger` appropriato in {{site.data.keyword.openwhisk_short}}.

Il webhook è diretto a inviare notifiche a un URL come:

`POST /namespaces/{namespace}/triggers/{triggerName}`

Il modulo con la richiesta POST viene interpretato come documento JSON che definisce i parametri sull'evento trigger. Le regole {{site.data.keyword.openwhisk_short}} passano questi parametri trigger a qualsiasi azione da attivare come risultato dell'evento.

## Implementazione di feed con polling
{: #feeds_polling}

Puoi configurare un'azione per eseguire il polling di un'origine di feed interamente all'interno di {{site.data.keyword.openwhisk_short}}, senza dover impostare connessioni persistenti o servizi esterni.

Per i feed in cui non è disponibile un webhook, ma non sono necessari tempi di risposta a bassa latenza o di grandi volumi, puoi utilizzare il polling.

Per impostare un feed basato sul polling, l'azione di feed utilizza i seguenti passi quando viene chiamata per eseguire `CREATE`:

1. L'azione di feed configura un trigger periodico con la frequenza desiderata, utilizzando il feed `whisk.system/alarms`.
2. Lo sviluppatore del feed crea un'azione `pollMyService` che esegue il polling del servizio remoto e restituisce eventuali nuovi eventi.
3. L'azione di feed imposta una *regola* *T -> pollMyService*.

Questa procedura implementa un trigger basato sul polling utilizzando esclusivamente azioni {{site.data.keyword.openwhisk_short}}, senza alcun bisogno di un servizio separato.

## Implementazione di feed tramite connessioni
{: #feeds_connections}

Le due precedenti scelte architetturali sono semplici e facili da implementare. Tuttavia, se vuoi ottenere un feed ad alte prestazioni, non esiste alternativa alle connessioni persistenti e polling lungo o tecniche simili.

Poiché le azioni {{site.data.keyword.openwhisk_short}} devono essere di breve esecuzione, un'azione non può mantenere una connessione persistente a una terza parte. Puoi, invece, impostare un servizio separato, chiamato *servizio provider*, al di fuori di {{site.data.keyword.openwhisk_short}} che resti sempre in esecuzione. Un servizio provider può mantenere connessioni a origini eventi di terze parti che supportano il polling lungo o altre notifiche basate sulla connessione.

Il servizio provider ha un'API REST che consente all'*azione di feed* {{site.data.keyword.openwhisk_short}} di controllare il feed. Il servizio provider funge da proxy tra il provider di eventi e {{site.data.keyword.openwhisk_short}}. Quando riceve gli eventi dalla terza parte, li invia a {{site.data.keyword.openwhisk_short}} attivando un trigger.

Il feed *changes* di {{site.data.keyword.cloudant_short_notm}} è l'esempio canonico in quanto imposta un servizio `cloudanttrigger`, che media tra le notifiche {{site.data.keyword.cloudant_short_notm}} su una connessione persistente e i trigger {{site.data.keyword.openwhisk_short}}.


Il feed *alarm* viene implementato con un modello simile.

L'architettura basata sulla connessione è l'opzione più performante, ma impone un sovraccarico maggiore sulle operazioni rispetto alle architetture di polling e hook.


