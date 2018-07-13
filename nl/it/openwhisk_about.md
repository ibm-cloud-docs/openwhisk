---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Architettura della piattaforma
{: #openwhisk_about}

{{site.data.keyword.openwhisk}} è una piattaforma di calcolo basato sugli eventi, indicato anche come calcolo senza server o FaaS (Function as a Service), che esegue il codice in risposta a eventi o a chiamate dirette.
{: shortdesc}

## Tecnologia {{site.data.keyword.openwhisk_short}} 
{: #technology}

Scopri alcuni concetti basilari della tecnologia alla base di {{site.data.keyword.openwhisk_short}}:

<dl>
<dt>Azione</dt>
<dd>Un'[azione](openwhisk_actions.html) è una parte di codice che esegue una specifica attività. Un'azione può essere scritta nel linguaggio che preferisci, come piccoli frammenti di codice JavaScript o Swift oppure codice binario personalizzato incorporato in un contenitore Docker. Fornisci la tua azione a Cloud Functions come codice sorgente o immagine Docker.
<br><br>Un'azione esegue il lavoro quando viene richiamata direttamente utilizzando l'SDK iOS, CLI o API {{site.data.keyword.openwhisk_short}}. Un'azione può anche rispondere automaticamente agli eventi da servizi {{site.data.keyword.Bluemix_notm}} e di terze parti utilizzando un trigger.</dd>
<dt>Sequenza</dt>
<dd>È possibile concatenare una serie di azioni in una [sequenza](openwhisk_actions.html#openwhisk_create_action_sequence) senza dover scrivere alcun codice. Una sequenza è una catena di azioni, richiamate in ordine, in cui l'output di un'azione viene passato come input all'azione successiva. Ciò ti consente di combinare insieme le azioni esistenti per un rapido e semplice riutilizzo. Una sequenza può quindi essere richiamata proprio come un'azione, tramite un'API REST o automaticamente in risposta agli eventi.
</dd>
<dt>Evento</dt>
<dd>Esempi di eventi includono le modifiche ai record di database, letture del sensore IoT che superano una determinata temperatura, nuovi commit di codice a un repository GitHub o semplici richieste HTTP provenienti da applicazioni web o mobili. Gli eventi provenienti da origini eventi interne ed esterne vengono incanalati attraverso un trigger e le regole consentono alle azioni di reagire a tali eventi.</dd>
<dt>Trigger</dt>
<dd>I [trigger](openwhisk_triggers_rules.html#openwhisk_triggers_create) sono un canale indicato per una classe di eventi. Un trigger è una dichiarazione da te scelta per reagire a un determinato tipo di evento, sia che provenga da un utente che da un'origine eventi.</dd>
<dt>Regola</dt>
<dd>Una [regola](openwhisk_triggers_rules.html#openwhisk_rules_use) associa un trigger a un'azione. Ogni volta che un trigger viene attivato, la regola richiama l'azione associata. Con l'insieme appropriato di regole, è possibile che un singolo evento trigger richiami più azioni o che un'azione venga richiamata in risposta a eventi appartenenti a più trigger.</dd>
<dt>Pacchetto</dt>
<dd>Le integrazioni con i servizi e provider di eventi possono essere aggiunte con i pacchetti. Un [pacchetto](openwhisk_packages.html) è un bundle di feed e azioni. Un feed è una parte di codice che configura un'origine eventi esterna per l'attivazione di eventi trigger. Ad esempio, un trigger creato con un feed di modifica {{site.data.keyword.cloudant}} configura un servizio per attivare il trigger ogni volta che un documento viene modificato o aggiunto a un database {{site.data.keyword.cloudant_short_notm}}. Le azioni nei pacchetti rappresentano una logica riutilizzabile che può essere resa disponibile da un provider del servizio in modo che gli sviluppatori possano utilizzare il servizio come origine eventi e richiamare le API di quel servizio.
<br><br>Un catalogo di pacchetti esistente consente di ampliare le applicazioni con funzioni utili e accedere a servizi esterni appartenenti all'ecosistema con rapidità. Esempi di servizi esterni che dispongono di pacchetti {{site.data.keyword.openwhisk_short}} includono {{site.data.keyword.cloudant_short_notm}}, The Weather Company, Slack e GitHub.</dd>
</dl>

## Come funziona {{site.data.keyword.openwhisk_short}}
{: #openwhisk_how}

Per spiegare tutti i componenti in modo più dettagliato, tracciamo la chiamata di un'azione attraverso il sistema {{site.data.keyword.openwhisk_short}}. Una chiamata esegue il codice che l'utente inserisce nel sistema e restituisce i risultati di tale esecuzione. La seguente figura mostra l'architettura di {{site.data.keyword.openwhisk_short}} di livello superiore.

![{{site.data.keyword.openwhisk_short}} - Architettura](./images/OpenWhisk.png)


## Come funziona l'elaborazione interna di OpenWhisk
{: #openwhisk_internal}

Cosa succede dietro le quinte di OpenWhisk?

OpenWhisk è un progetto open-source che combina componenti come Nginx, Kafka, Docker e CouchDB per formare un servizio di programmazione basato su eventi senza server.

<img src="images/OpenWhisk_flow_of_processing.png" width="550" alt="Flusso interno di elaborazione in OpenWhisk" style="width:550px; border-style: none"/>

### Accesso al sistema: nginx

Innanzitutto,  l'API rivolta agli utenti OpenWhisk è completamente basata su HTTP e segue una progettazione RESTful. Di conseguenza, il comando inviato tramite la CLI è una richiesta HTTP nel sistema OpenWhisk. Il comando specifico si traduce approssimativamente in:
```
POST /api/v1/namespaces/$userNamespace/actions/myAction
Host: $openwhiskEndpoint
```
{: screen}

Nota qui la variabile *$userNamespace*. Un utente ha accesso ad almeno uno spazio dei nomi. Per semplicità, supponiamo che l'utente possieda lo spazio dei nomi in cui è inserito *myAction*.

Il primo punto di ingresso nel sistema avviene attraverso **nginx**, “un server proxy inverso e HTTP”. Viene utilizzato per la terminazione SSL e l'inoltro di chiamate HTTP appropriate al componente successivo.

### Accesso al sistema: Controller

Nginx inoltra la richiesta HTTP al **Controller**, il componente successivo nel percorso attraverso OpenWhisk. Si tratta di un'implementazione basata su Scala della reale API REST (basata su **Akka** e **Spray**) e funge quindi da interfaccia per tutte le operazioni che utente può eseguire. Queste includono le richieste [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) per le tue entità in OpenWhisk e la chiamata delle azioni.

Per prima cosa il Controller distingue ciò che l'utente sta tentando di fare e lo fa in base al metodo HTTP che utilizzi nella tua richiesta HTTP. Come per la traduzione precedente, l'utente invia una richiesta POST a un'azione esistente, che il Controller traduce in una **chiamata di un'azione**.

Dato il ruolo centrale del Controller (da qui il nome), sarà coinvolto in una certa misura in tutte le seguenti operazioni.

### Autenticazione e autorizzazione: CouchDB

Adesso il Controller verifica chi sei (*Autenticazione*) e se disponi del privilegio di fare ciò che vuoi con quell'entità (*Autorizzazione*). Le credenziali incluse nella richiesta vengono verificate nel cosiddetto database **soggetti** in un'istanza di **CouchDB**.

In questo caso, viene verificato che l'utente esista nel database di OpenWhisk e che disponga del privilegio per richiamare l'azione *myAction*, che si suppone sia un'azione in uno spazio dei nomi di proprietà dell'utente. Quest'ultimo dà effettivamente all'utente il privilegio di richiamare l'azione.

A questo punto, tutto è pronto per la prossima fase di elaborazione.

### Richiamo dell'azione: di nuovo CouchDB…

Poiché adesso il Controller è sicuro che l'utente è autorizzato e che dispone dei privilegi per richiamare l'azione, carica questa azione (in questo caso *myAction*) dal database **whisks** in CouchDB.

Il record dell'azione contiene principalmente il codice da eseguire e i parametri predefiniti che vuoi passare alla tua azione uniti ai parametri che hai incluso nell'effettiva richiesta di chiamata. Contiene inoltre le restrizioni della risorsa imposte per l'esecuzione, come ad esempio la quantità di memoria che può consumare.

In questo caso particolare, l'azione non prende alcun parametro (la definizione dei parametri della funzione è un elenco vuoto). Pertanto, si presume che i parametri predefiniti non siano impostati, compresi i parametri specifici per l'azione, rendendo il caso più semplice da questo punto di vista.


### Chi richiama l'azione: Load Balancer

Il Load Balancer, che fa parte del Controller, ha una visione globale degli esecutori disponibili nel sistema controllando continuamente il loro stato di integrità. Questi esecutori sono chiamati **Invoker**. Il Load Balancer, che sa quali Invoker sono disponibili, ne sceglie uno per richiamare l'azione richiesta.

### Forma una linea: Kafka

D'ora in poi, alla richiesta di chiamata che hai inviato possono accadere principalmente due cose negative:

1. Il sistema può arrestarsi in modo anomalo, perdendo così la tua chiamata.
2. Il sistema può essere sottoposto a un carico così eccessivo che la chiamata deve attendere prima il completamento di altre chiamate.

La risposta a entrambi i problemi è **Kafka**, “un sistema di messaggistica distribuito di pubblicazione-sottoscrizione ad alta velocità”. Il Controller e l'Invoker comunicano esclusivamente attraverso messaggi che vengono memorizzati nel buffer e conservati da Kafka. Kafka alleggerisce il carico di buffer in memoria, che può provocare una *OutOfMemoryException*, sia per il Controller che per l'Invoker, garantendo anche che i messaggi non vadano persi in caso di arresto anomalo del sistema.

Per richiamare l'azione, il Controller pubblica dunque un messaggio su Kafka, che contiene l'azione da richiamare e i parametri da passare all'azione (in questo caso, nessuno). Questo messaggio viene indirizzato all'Invoker che il Controller ha scelto dall'elenco ricevuto da Consul.

Una volta che Kafka conferma di aver ricevuto il messaggio, la richiesta HTTP per l'utente riceve come risposta un **ActivationId**. L'utente può utilizzarlo in seguito per ottenere l'accesso ai risultati di questa specifica chiamata. Questo è un modello di chiamata asincrono, in cui la richiesta HTTP termina dopo che il sistema accetta la richiesta di richiamare un'azione. È disponibile anche un modello sincrono (detto chiamata bloccante), che però non è trattato qui.

### Richiamo del codice: Invoker

L'**Invoker** è il nucleo di OpenWhisk. Il compito dell'Invoker è quello di richiamare un'azione, è implementato anche in Scala, ma non è tutto. Per eseguire le azioni in modo isolato e sicuro, utilizza **Docker**.

Docker è usato per configurare un nuovo ambiente autoincapsulato (chiamato *contenitore*) per ogni azione che richiamiamo in modo rapido, isolato e controllato. Per ogni chiamata di azione, viene generato un contenitore Docker e viene inserito il codice azione. Il codice viene quindi eseguito utilizzando i parametri che gli vengono passati, si ottiene il risultato e infine il contenitore viene distrutto. In questa fase è possibile effettuare ottimizzazioni delle prestazioni per ridurre il sovraccarico e limitare il più possibile i tempi di risposta.

In questo caso, avendo a portata di mano un'azione basata su *Node.js*, l'Invoker avvia un contenitore Node.js. Quindi, immette il codice da *myAction*, lo esegue senza parametri, estrae il risultato, salva i log e distrugge di nuovo il contenitore Node.js.

### Memorizzazione dei risultati: di nuovo CouchDB

Quando l'Invoker riceve il risultato, questo viene memorizzato nel database **whisks** sotto forma di attivazione nell'ActivationId. Il database **whisks** risiede in **CouchDB**.

In questo caso specifico, l'Invoker recupera l'oggetto JSON risultante dall'azione, prende il log scritto da Docker, li inserisce tutti nel record di attivazione e memorizza il record nel database. Fai riferimento al seguente esempio:
```json
{
   "activationId": "31809ddca6f64cfc9de2937ebd44fbb9",
   "response": {
       "statusCode": 0,
       "result": {
           "hello": "world"
       }
   },
   "end": 1474459415621,
   "logs": [
       "2016-09-21T12:03:35.619234386Z stdout: Hello World"
   ],
   "start": 1474459415595,
}
```
{: codeblock}

Nota che il record contiene sia il risultato restituito che i log scritti. Contiene anche l'ora di inizio e di fine della chiamata dell'azione. I record di attivazione contengono più campi, ma per semplicità in questo esempio sono stati ridotti.

Adesso puoi utilizzare di nuovo l'API REST (iniziando dal passo 1) per ottenere la tua attivazione e quindi il risultato della tua azione. Per farlo, immetti questo comando:

```bash
ibmcloud wsk activation get 31809ddca6f64cfc9de2937ebd44fbb9
```
{: pre}

### Riepilogo

Puoi vedere come una semplice azione **ibmcloud wsk action invoked myAction** passa attraverso diverse fasi del sistema {{site.data.keyword.openwhisk_short}}. Il sistema stesso consiste principalmente di due soli componenti personalizzati, il **Controller** e l'**Invoker**. Tutto il resto è già lì, sviluppato da diverse persone nella community open source.

Puoi trovare ulteriori informazioni su {{site.data.keyword.openwhisk_short}} nei seguenti argomenti:

* [Nomi delle entità](./openwhisk_reference.html#openwhisk_entities)
* [Semantica delle azioni](./openwhisk_reference.html#openwhisk_semantics)
* [Limiti](./openwhisk_reference.html#openwhisk_syslimits)
* [Guida di riferimento API REST](https://console.bluemix.net/apidocs/98-cloud-functions?&language=node#introduction)
