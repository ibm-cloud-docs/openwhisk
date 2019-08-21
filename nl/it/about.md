---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: platform architecture, openwhisk, couchdb, kafka, functions

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


# Come funziona {{site.data.keyword.openwhisk_short}}
{: #about}

{{site.data.keyword.openwhisk}} è una piattaforma di calcolo basato sugli eventi, indicato anche come calcolo senza server o FaaS (Function as a Service), che esegue il codice in risposta a eventi o a chiamate dirette.
{: shortdesc}

## Tecnologia {{site.data.keyword.openwhisk_short}}
{: #about_technology}

Scopri i concetti essenziali della tecnologia alla base di {{site.data.keyword.openwhisk_short}}:

**Cos'è un'azione?**

Un'azione è un piccolo frammento di codice che può essere richiamata o configurata per venire eseguita in automatico in risposta a un evento. In entrambi i casi, ogni esecuzione produce un record identificato da un ID di attivazione univoco. L'input e il risultato di un'azione possono essere visti come coppie chiave-valore. La chiave è una stringa e il valore è un valore JSON valido. Un'azione può essere scritta nel linguaggio che preferisci e fornita al servizio come codice sorgente o immagine Docker. Il codice dell'azione viene eseguito quando viene richiamato direttamente dalla CLI, dall'API Cloud Functions e dall'SDK iOS. Un'azione può rispondere automaticamente agli eventi da IBM Cloud o da servizi di terze parti.

**Perché dovrei utilizzare un'azione?**

Utilizzando le azioni, limiti il tempo di esecuzione del codice, il che riduce i tuoi costi.

Ad esempio, puoi utilizzare le azioni per rilevare dei volti in un'immagine, rispondere a delle modifiche in un database, aggregare una serie di chiamate API o anche pubblicare un tweet.

**Posso utilizzare più di un'azione alla volta?**

Sì! Puoi utilizzare le azioni per richiamare altre azioni oppure puoi unire delle azioni tra loro per [creare sequenze](/docs/openwhisk?topic=cloud-functions-actions#actions_seq). Perché ciò funzioni, l'output di un'azione dovrebbe essere l'input di un'altra azione che fornisce quindi un output che può essere utilizzato per attivare un'altra azione e così via. Puoi anche raccogliere in bundle il gruppo di azioni che hai creato per formare un pacchetto. Con un pacchetto, puoi riutilizzare azioni o sequenze comuni chiamando il pacchetto invece di configurare nuovamente l'azione o la sequenza.

## Terminologia {{site.data.keyword.openwhisk_short}}

<dl>
  <dt>Spazio dei nomi</dt>
    <dd>Gli [spazi dei nomi](/docs/openwhisk?topic=cloud-functions-namespaces) contengono entità {{site.data.keyword.openwhisk_short}}, quali azioni e trigger, e appartengono a un gruppo di risorse. Puoi consentire agli utenti di accedere alle tue entità {{site.data.keyword.openwhisk_short}} concedendo loro l'accesso allo spazio dei nomi.</dd>
  <dt>Azione</dt>
    <dd>Un'[azione](/docs/openwhisk?topic=cloud-functions-actions) è una parte di codice che esegue una specifica attività. Un'azione può essere scritta nel linguaggio che preferisci, come piccoli frammenti di codice JavaScript o Swift oppure codice binario personalizzato incorporato in un contenitore Docker. Fornisci la tua azione a Cloud Functions come codice sorgente o immagine Docker.
    <br><br>Un'azione esegue il lavoro quando viene richiamata direttamente utilizzando l'SDK iOS, CLI o API {{site.data.keyword.openwhisk_short}}. Un'azione può anche rispondere automaticamente agli eventi dei servizi {{site.data.keyword.cloud_notm}} e di terze parti utilizzando un trigger.</dd>
  <dt>Sequenza</dt>
    <dd>È possibile concatenare una serie di azioni in una [sequenza](/docs/openwhisk?topic=cloud-functions-actions#actions_seq) senza dover scrivere alcun codice. Una sequenza è una catena di azioni, richiamate in ordine, in cui l'output di un'azione viene passato come input all'azione successiva. Ciò ti consente di combinare insieme le azioni esistenti per un riutilizzo rapido e semplice. Una sequenza può quindi essere richiamata proprio come un'azione, tramite un'API REST o automaticamente in risposta agli eventi.
  </dd>
  <dt>Evento</dt>
    <dd>Esempi di eventi includono le modifiche ai record di database, letture del sensore IoT che superano una determinata temperatura, nuovi commit di codice a un repository GitHub o semplici richieste HTTP provenienti da applicazioni web o mobili. Gli eventi provenienti da origini eventi interne ed esterne vengono incanalati attraverso un trigger e le regole consentono alle azioni di reagire a tali eventi.</dd>
  <dt>Trigger</dt>
    <dd>I [trigger](/docs/openwhisk?topic=cloud-functions-triggers) sono un canale indicato per una classe di eventi. Un trigger è una dichiarazione da te scelta per reagire a un determinato tipo di evento, sia che provenga da un utente che da un'origine eventi.</dd>
  <dt>Regola</dt>
    <dd>Una [regola](/docs/openwhisk?topic=cloud-functions-rules) associa un trigger a un'azione. Ogni volta che un trigger viene attivato, la regola utilizza l'evento di trigger e richiama l'azione associata. Con l'insieme appropriato di regole, è possibile che un singolo evento trigger richiami più azioni o che un'azione venga richiamata in risposta a eventi appartenenti a più trigger.</dd>
  <dt>Feed</dt>
    <dd>Un [feed](/docs/openwhisk?topic=cloud-functions-triggers#triggers_feeds) rappresenta un modo utile per configurare un'origine eventi esterna
per l'attivazione di eventi trigger che possono essere utilizzati da {{site.data.keyword.openwhisk_short}}. Ad esempio, un feed Git può attivare un evento trigger per ogni commit a un repository Git.</dd>
  <dt>Pacchetto</dt>
    <dd>Le integrazioni con i servizi e provider di eventi possono essere aggiunte con i pacchetti. Un [pacchetto](/docs/openwhisk?topic=cloud-functions-pkg_ov) è un bundle di feed e azioni. Un feed è una parte di codice che configura un'origine eventi esterna per l'attivazione di eventi trigger. Ad esempio, un trigger creato con un feed di modifica {{site.data.keyword.cloudant}} configura un servizio per attivare il trigger ogni volta che un documento viene modificato o aggiunto a un database {{site.data.keyword.cloudant_short_notm}}. Le azioni nei pacchetti rappresentano una logica riutilizzabile che può essere resa disponibile da un provider del servizio in modo che gli sviluppatori possano utilizzare il servizio come origine eventi e richiamare le API di quel servizio.
    <br><br>Un catalogo di pacchetti esistente consente di ampliare le applicazioni con funzioni utili e accedere a servizi esterni appartenenti all'ecosistema con rapidità. Esempi di servizi esterni che dispongono di pacchetti {{site.data.keyword.openwhisk_short}} includono {{site.data.keyword.cloudant_short_notm}}, The Weather Company, Slack e GitHub.</dd>
</dl>

### Operazioni successive
{: #quiz}
Verifica le tue conoscenze e [fai un quiz ](https://ibmcloud-quizzes.mybluemix.net/functions/terms_quiz/quiz.php){: external}!


## Come funziona l'elaborazione interna di OpenWhisk
{: #about_internal}

Per spiegare tutti i componenti in modo più dettagliato, tracciamo la chiamata di un'azione attraverso il sistema {{site.data.keyword.openwhisk_short}}. Una chiamata esegue il codice che l'utente inserisce nel sistema e restituisce i risultati di tale esecuzione. La seguente figura mostra l'architettura di {{site.data.keyword.openwhisk_short}} di livello superiore.

![{{site.data.keyword.openwhisk_short}} - Architettura](./images/OpenWhisk.png)

### Cosa succede dietro le quinte di OpenWhisk?
{: #about_scenes}

OpenWhisk è un progetto open source che combina componenti come NGINX, Kafka, Docker e CouchDB per formare un servizio di programmazione basato su eventi senza server.

<img src="images/OpenWhisk_flow_of_processing.png" width="550" alt="Flusso interno di elaborazione in OpenWhisk" style="width:550px; border-style: none"/>

#### 1. Accesso al sistema: NGINX
{: #about_ngnix}

Innanzitutto, l'API rivolta agli utenti OpenWhisk è basata su HTTP e segue una progettazione RESTful. Di conseguenza, il comando inviato tramite la CLI è una richiesta HTTP nel sistema OpenWhisk. Il comando specifico si traduce approssimativamente in:
```
POST /api/v1/namespaces/$userNamespace/actions/myAction
Host: $openwhiskEndpoint
```
{: screen}

Nota qui la variabile *$userNamespace*. Un utente ha accesso ad almeno uno spazio dei nomi. Per semplicità, supponiamo che l'utente possieda lo spazio dei nomi in cui è inserito *myAction*.

Il primo punto di ingresso nel sistema avviene attraverso **NGINX**, “un server proxy HTTP e inverso”. Viene utilizzato per la terminazione SSL e l'inoltro di chiamate HTTP appropriate al componente successivo.

#### 2. Accesso al sistema: Controller
{: #about_controller}


NGINX inoltra la richiesta HTTP al **Controller**, il componente successivo nel percorso attraverso OpenWhisk. Si tratta di un'implementazione basata su Scala della reale API REST (basata su **Akka** e **Spray**) e funge quindi da interfaccia per tutte le operazioni che utente può eseguire. Queste includono le richieste di [creazione, recupero, aggiornamento ed eliminazione](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete){: external} (CRUD) per le tue entità in OpenWhisk e la chiamata delle azioni.

Per prima cosa il Controller distingue ciò che l'utente sta tentando di fare e lo fa in base al metodo HTTP che utilizzi nella tua richiesta HTTP. L'utente invia una richiesta POST a un'azione esistente, che il Controller traduce in una **chiamata di un'azione**.

Dato il ruolo centrale del Controller (da qui il nome), viene coinvolto in una certa misura in tutte le seguenti operazioni.

#### 3. Autenticazione e autorizzazione: CouchDB
{: #about_auth}

Adesso il Controller verifica chi sei (*Autenticazione*) e se disponi del privilegio di fare ciò che vuoi con quell'entità (*Autorizzazione*). Le credenziali incluse nella richiesta vengono verificate nel cosiddetto database **soggetti** in un'istanza di **CouchDB**.

In questo caso, viene verificato che l'utente esista nel database di OpenWhisk e che disponga del privilegio per richiamare l'azione *myAction*, che si suppone sia un'azione in uno spazio dei nomi di proprietà dell'utente. Quest'ultimo dà effettivamente all'utente il privilegio di richiamare l'azione.

A questo punto, tutto è pronto per la prossima fase di elaborazione.

#### 4. Richiamo dell'azione: di nuovo CouchDB…
{: #about_couchdb}

Poiché adesso il Controller è sicuro che l'utente è autorizzato e che dispone dei privilegi per richiamare l'azione, carica questa azione (in questo caso *myAction*) dal database **whisks** in CouchDB.

Il record dell'azione contiene principalmente il codice da eseguire e i parametri predefiniti che vuoi passare alla tua azione uniti ai parametri che hai incluso nell'effettiva richiesta di chiamata. Contiene inoltre le restrizioni della risorsa imposte per l'esecuzione, come ad esempio la quantità di memoria che può consumare.

In questo caso particolare, l'azione non prende alcun parametro (la definizione dei parametri della funzione è un elenco vuoto). Pertanto, si presume che i parametri predefiniti non siano impostati, compresi i parametri specifici per l'azione, rendendo il caso più semplice da questo punto di vista.


#### 5. Chi richiama l'azione: Load Balancer
{: #about_lb}

Il Load Balancer, che fa parte del Controller, ha una visione globale degli esecutori disponibili nel sistema controllando continuamente il loro stato di integrità. Questi esecutori sono chiamati **Invoker**. Il Load Balancer, che sa quali Invoker sono disponibili, ne sceglie uno per richiamare l'azione richiesta.

#### 6. Forma una linea: Kafka
{: #about_kafka}

D'ora in poi, alla richiesta di chiamata che hai inviato possono accadere principalmente due cose negative:

1. Il sistema può arrestarsi in modo anomalo, perdendo così la tua chiamata.
2. Il sistema può essere sottoposto a un carico così eccessivo che la chiamata deve attendere prima il completamento di altre chiamate.

La risposta a entrambi i problemi è **Kafka**, “un sistema di messaggistica distribuito di pubblicazione-sottoscrizione ad alta velocità”. Il Controller e l'Invoker comunicano esclusivamente attraverso messaggi che vengono memorizzati nel buffer e conservati da Kafka. Kafka alleggerisce il carico di buffer in memoria, che può provocare una *OutOfMemoryException*, sia per il Controller che per l'Invoker, garantendo anche che i messaggi non vadano persi in caso di arresto anomalo del sistema.

Per richiamare l'azione, il Controller pubblica dunque un messaggio su Kafka, che contiene l'azione da richiamare e i parametri da passare all'azione (in questo caso, nessuno). Questo messaggio viene indirizzato all'Invoker che il Controller ha scelto dall'elenco ricevuto da Consul.

Una volta che Kafka ha confermato di aver ricevuto il messaggio, la richiesta HTTP per l'utente riceve una risposta con un **ID di attivazione**. L'utente può utilizzarlo in seguito per ottenere l'accesso ai risultati di questa specifica chiamata. Questo è un modello di chiamata asincrono, in cui la richiesta HTTP termina dopo che il sistema accetta la richiesta di richiamare un'azione. È disponibile anche un modello sincrono (detto chiamata bloccante), che però non è trattato qui.

#### 7. Richiamo del codice: Invoker
{: #about_invoker}

L'**Invoker** è il nucleo di OpenWhisk. Il compito dell'Invoker è quello di richiamare un'azione, è implementato anche in Scala, ma non è tutto. Per eseguire le azioni in modo isolato e sicuro, utilizza **Docker**.

Docker viene utilizzato per configurare un nuovo ambiente autoincapsulato (chiamato *contenitore*) per ogni azione che richiamiamo in modo rapido, isolato e controllato. Per ogni chiamata di azione, viene generato un contenitore Docker e viene inserito il codice azione. Il codice viene quindi eseguito utilizzando i parametri che gli vengono passati, si ottiene il risultato e infine il contenitore viene distrutto. In questa fase è possibile eseguire ottimizzazioni delle prestazioni per ridurre i requisiti di manutenzione e consentire brevi tempi di risposta.

In questo caso, con un'azione basata su *Node.js* a portata di mano, l'Invoker avvia un contenitore Node.js. Quindi, inserisce il codice da *myAction*, lo esegue senza parametri, estrae il risultato, salva i log e distrugge nuovamente il contenitore Node.js.

#### 8. Memorizzazione dei risultati: di nuovo CouchDB
{: #about_storing}

Quando l'Invoker riceve il risultato, questo viene memorizzato nel database **whisks** come attivazione sotto l'ID attivazione. Il database **whisks** risiede in **CouchDB**.

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
ibmcloud fn activation get 31809ddca6f64cfc9de2937ebd44fbb9
```
{: pre}

### Riepilogo
{: #about_summary}

Puoi vedere come una semplice azione **ibmcloud fn action invoked myAction** passa attraverso diverse fasi del sistema {{site.data.keyword.openwhisk_short}}. Il sistema stesso consiste principalmente di due soli componenti personalizzati, il **Controller** e l'**Invoker**. Tutto il resto è già lì, sviluppato da diverse persone nella community open source.

Puoi trovare ulteriori informazioni su {{site.data.keyword.openwhisk_short}} nei seguenti argomenti:

* [Nomi delle entità](/docs/openwhisk?topic=cloud-functions-limits#limits_entities)
* [Semantica delle azioni](/docs/openwhisk?topic=cloud-functions-limits#limits_semantics)
* [Limiti](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)
* [Guida di riferimento API REST](/apidocs/functions)




