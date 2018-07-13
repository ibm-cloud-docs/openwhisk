---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Dettagli e limiti del sistema
{: #openwhisk_reference}

Le seguenti sezioni forniscono dettagli tecnici sulle impostazioni di sistema e dei limiti di {{site.data.keyword.openwhisk}}.
{: shortdesc}

## Entità di {{site.data.keyword.openwhisk_short}}
{: #openwhisk_entities}

### Spazi dei nomi e pacchetti
{: #openwhisk_entities_namespaces}

Le azioni, i trigger e le regole {{site.data.keyword.openwhisk_short}} appartengono a uno spazio dei nomi e, talvolta, a un pacchetto.

I pacchetti possono contenere azioni e feed. Un pacchetto non può contenerne un altro, pertanto la nidificazione dei pacchetti non è consentita. Inoltre, le entità non devono essere contenute in un pacchetto.

In {{site.data.keyword.Bluemix_notm}}, una coppia organizzazione+spazio corrisponde a uno spazio dei nomi {{site.data.keyword.openwhisk_short}}. Ad esempio, l'organizzazione `BobsOrg` e lo spazio `dev` corrisponderebbero allo spazio dei nomi {{site.data.keyword.openwhisk_short}} `/BobsOrg_dev`.

Se autorizzato, puoi creare il tuo spazio dei nomi personale. Lo spazio dei nomi `/whisk.system` è riservato alle entità distribuite con il sistema {{site.data.keyword.openwhisk_short}}.

### Nomi completi
{: #openwhisk_entities_fullyqual}

Il nome completo di un'entità è
`/nomeSpazioNomi[/nomePacchetto]/nomeEntità`. Nota che per delimitare gli spazi dei nomi, i pacchetti e le entità viene utilizzato `/`. Inoltre, gli spazi dei nomi devono essere preceduti da `/`.

Per praticità, lo spazio dei nomi può essere tralasciato se è lo *spazio dei nomi predefinito* dell'utente.

Ad esempio, considera un utente il cui spazio dei nomi predefinito è `/myOrg`. Di seguito sono riportati esempi di nomi completi di una serie di entità e i rispettivi alias.

| Nome completo | Alias | Spazio dei nomi | Pacchetto | Nome |
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read` |  | `/whisk.system` | `cloudant` | `read` |
| `/myOrg/video/transcode` | `video/transcode` | `/myOrg` | `video` | `transcode` |
| `/myOrg/filter` | `filter` | `/myOrg` |  | `filter` |

Puoi adoperare questo schema di denominazione quando utilizzi, ad esempio, la CLI {{site.data.keyword.openwhisk_short}}.

### Nomi delle entità
{: #openwhisk_entities_names}

I nomi di tutte le entità, tra cui azioni, trigger, regole, pacchetti e spazi dei nomi, sono una sequenza di caratteri che si attengono al seguente formato:

* Il primo carattere deve essere un carattere alfanumerico o un carattere di sottolineatura.
* I caratteri successivi possono essere alfanumerici, spazi o uno qualsiasi dei seguenti valori: `_`, `@`, `.`, `-`.
* L'ultimo carattere non può essere uno spazio.

Più precisamente, un nome deve corrispondere alla seguente espressione regolare (indicata utilizzando la sintassi di metacaratteri Java): `\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`.

## Semantica delle azioni
{: #openwhisk_semantics}

Le seguenti sezioni forniscono informazioni dettagliate sulle azioni {{site.data.keyword.openwhisk_short}}.

### Assenza di stato
{: #openwhisk_semantics_stateless}

Le implementazioni dell'azione sono senza stato o *idempotenti*. Anche se il sistema non impone questa proprietà, non è garantito che uno stato mantenuto da un'azione sia disponibile tra le chiamate.

Inoltre, potrebbero esserci più creazioni di istanze di un'azione, ciascuna delle quali con il proprio stato. La chiamata di un'azione potrebbe essere inviata a una qualsiasi di queste creazioni di istanze.

### Input e output delle chiamate
{: #openwhisk_semantics_invocationio}

L'input e l'output di un'azione costituiscono un dizionario di coppie chiave-valore. La chiave è una stringa e il valore è un valore JSON valido.

### Ordine di chiamata delle azioni
{: #openwhisk_ordering}

Le chiamate di un'azione non sono ordinate. Se l'utente richiama un'azione due volte dalla riga di comando o dall'API REST, la seconda chiamata potrebbe essere eseguita per prima. Se le azioni hanno effetti secondari, questi potrebbero essere osservati in qualsiasi ordine.

Inoltre, non è garantito che le azioni vengano eseguite automaticamente. Due azioni possono essere eseguite contemporaneamente e avere effetti secondari interfoliati. OpenWhisk non garantisce uno specifico modello di coerenza simultanea per gli effetti secondari. Eventuali effetti collaterali della simultaneità dipendono dall'implementazione.

### Garanzie di esecuzione dell'azione
{: #openwhisk_atmostonce}

Alla ricezione di una richiesta di chiamata, il sistema registra la richiesta e invia un'attivazione.

Il sistema restituisce un ID di attivazione (con una chiamata non bloccante) che conferma che è stata ricevuta.
Se si verifica un errore di rete o un altro errore che avviene prima di ricevere una risposta HTTP, è possibile che {{site.data.keyword.openwhisk_short}} abbia ricevuto ed elaborato la richiesta.

Il sistema tenta di richiamare l'azione una volta, ottenendo uno dei quattro seguenti risultati:
- *success*: la chiamata dell'azione è stata completata correttamente.
- *application error*: la chiamata dell'azione ha avuto esito positivo, ma l'azione ha restituito un valore di errore di proposito, ad esempio perché non è stata soddisfatta una precondizione sugli argomenti.
- *action developer error*: l'azione è stata richiamata, ma è terminata in modo anomalo; ad esempio l'azione non ha rilevato un'eccezione o era presente un errore di sintassi.
- *whisk internal error*: il sistema non è stato in grado di richiamare l'azione.
Il risultato viene registrato nel campo `status` del record di attivazione, come illustrato in una delle seguenti sezioni.

Per ogni chiamata ricevuta correttamente, e che potrebbe essere addebitata all'utente, ci sarà un record di attivazione.

Se il risultato è *action developer error*, l'azione potrebbe essere eseguita parzialmente e generare effetti secondari visibili esterni. È responsabilità dell'utente controllare se si sono verificati tali effetti secondari ed immettere la logica del nuovo tentativo laddove necessario. Alcuni errori di tipo *whisk internal errors* indicano che un'azione inizia l'esecuzione, ma si verifica un errore prima che l'azione registri il completamento.

## Record di attivazione
{: #openwhisk_ref_activation}

Ogni chiamata di azione e attivazione di trigger produce un record di attivazione.

Un record di attivazione contiene i seguenti campi:

- *activationId*: l'ID di attivazione.
- *start* e *end*: data/ora di inizio e fine dell'attivazione. I valori sono espressi nel [formato temporale UNIX](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15).
- *namespace* e `name`: lo spazio dei nomi e il nome dell'entità.
- *logs*: un array di stringhe con i log prodotti dall'azione durante la sua attivazione. Ogni elemento dell'array corrisponde a un riga di output emessa dall'azione in `stdout` o `stderr` e include il tempo e il flusso dell'output del log. La struttura è la seguente: `TIMESTAMP STREAM: LOG_OUTPUT`.
- *response*: un dizionario che definisce le chiavi `success`, `status` e `result`:
  - *status*: il risultato dell'attivazione, che può assumere uno dei seguenti valori: "success", "application error", "action developer error", "whisk internal error".
  - *success*: è `true` se, e solo se, lo stato è `"success"`
- *result*: un dizionario che contiene il risultato dell'attivazione. Se l'attivazione ha avuto esito positivo, il risultato contiene il valore restituito dall'azione. Se l'attivazione ha avuto esito negativo, `result` contiene la chiave `error`, generalmente accompagnata da una spiegazione dell'errore.

## Azioni JavaScript
{: #openwhisk_ref_javascript}

### Prototipo di funzione
{: #openwhisk_ref_javascript_fnproto}

Le azioni JavaScript {{site.data.keyword.openwhisk_short}} vengono eseguite in un runtime Node.js.

Le azioni scritte in JavaScript devono essere limitate a un singolo file. Il file può contenere più funzioni, ma per convenzione deve essere presente una funzione denominata `main` che è quella che viene chiamata quando viene richiamata l'azione. Il seguente esempio mostra un'azione con più funzioni.
```javascript
function main() {
    return { payload: helper() }
}

function helper() {
    return new Date();
}
```
{: codeblock}

I parametri di input dell'azione vengono passati come oggetto JSON sotto forma di parametro alla funzione `main`. Il risultato di un'attivazione eseguita correttamente è anch'esso un oggetto JSON, che viene tuttavia restituito in modo differente a seconda che l'azione sia sincrona o asincrona, come descritto nella seguente sezione.

### Funzionamento sincrono e asincrono
{: #openwhisk_ref_javascript_synchasynch}

È comune che le funzioni JavaScript proseguano la loro esecuzione in una funzione di callback anche dopo una restituzione. Per gestire questo comportamento, un'attivazione di una azione JavaScript può essere *sincrona* o *asincrona*.

L'attivazione di un'azione JavaScript è **sincrona** se la funzione principale termina in una delle seguenti condizioni:

- La funzione principale termina senza eseguire un'istruzione `return`.
- La funzione principale termina con l'esecuzione di un'istruzione `return` che restituisce qualsiasi valore *eccetto* una Promessa.

Vedi il seguente esempio di un'azione sincrona:

```javascript
// an action in which each path results in a synchronous activation
function main(params) {
  if (params.payload == 0) {
     return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
     return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}

L'attivazione di un'azione JavaScript è **asincrona** se la funzione principale termina restituendo una Promessa. In questo caso, il sistema presuppone che l'azione sia ancora in esecuzione fino a quando la Promessa non viene soddisfatta o rifiutata.
Inizia a creare l'istanza di un nuovo oggetto Promessa e a trasmettergli una funzione di callback. Il callback utilizza due argomenti, resolve e reject, che sono entrambi funzioni. Tutto il codice asincrono va all'interno di tale callback.

Nel seguente esempio, puoi vedere come soddisfare una Promossa richiamando la funzione resolve.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         resolve({ done: true });
                }, 100);
             })
}
```
{: codeblock}

Questo esempio mostra come rifiutare una Promessa richiamando la funzione reject.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 100);
    })
}
```
{: codeblock}

È possibile che un'azione sia sincrona su alcuni input e asincrona su altri, come mostrato nel seguente esempio.
```javascript
function main(params) {
     if (params.payload) {
        // asynchronous activation
         return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
                }, 100);
             })
     }  else {
        // synchronous activation
         return {done: true};
      }
}
```
{: codeblock}

Indipendentemente dal fatto che un attivazione sia sincrona o asincrona, la chiamata dell'azione può essere bloccante o non bloccante.

### Rimozione dell'oggetto whisk globale JavaScript

L'oggetto globale `whisk` è stato rimosso; migra le tue azioni nodejs per utilizzare dei metodi alternativi.
Per le funzioni `whisk.invoke()` e `whisk.trigger()`, utilizza la libreria client [openwhisk](https://www.npmjs.com/package/openwhisk) già installata.
Per `whisk.getAuthKey()`, puoi ottenere il valore della chiave API dalla variabile di ambiente `__OW_API_KEY`.
Per `whisk.error()`, puoi restituire una Promessa rifiutata (ossia, Promise.reject).

### Ambienti di runtime JavaScript
{: #openwhisk_ref_javascript_environments}

Le azioni JavaScript possono essere eseguite in Node.js versione 6 o Node.js versione 8.
Attualmente, le azioni JavaScript vengono eseguite per impostazione predefinita in un ambiente Node.js versione 6.
### Assemblaggio di pacchetti npm con le tue azioni
Tutti i pacchetti `npm` non preinstallati nell'ambiente Node.js, possono essere raggruppati come dipendenze quando crei o aggiorni la tua azione.

Per ulteriori informazioni, vedi [Assemblaggio di un'azione come modulo Node.js](./openwhisk_actions.html#openwhisk_js_packaged_action) o [Assemblaggio di un'azione come un unico bundle](./openwhisk_actions.html#openwhisk_js_webpack_action).

### Ambiente Node.js versione 8 con gli SDK IBM
{: #openwhisk_ref_javascript_environments_8}
L'ambiente Node.js versione 8.11.1 viene utilizzato se l'indicatore `--kind` viene specificato esplicitamente con il valore `nodejs:8` quando si crea o si aggiorna un'azione.

I seguenti pacchetti sono preinstallati nell'ambiente Node.js versione 8.11.1:
  - [amqplib v0.5.2](https://www.npmjs.com/package/amqplib) - Una libreria per la creazione di client AMQP 0-9-1 per Node.JS.
  - [apn v2.2.0](https://www.npmjs.com/package/apn) - Un modulo Node.js per l'interfacciamento con il servizio Apple Push Notification.
  - [async v2.6.1](https://www.npmjs.com/package/async) - Fornisce funzioni per lavorare con funzioni asincrone.
  - [bent v1.1.0](https://www.npmjs.com/package/bent) - Client HTTP funzionale per Node.js con async/await.
  - [body-parser v1.18.3](https://www.npmjs.com/package/body-parser) - Middleware di analisi del corpo Node.js. Analizza i corpi delle richieste in entrata in un middleware prima dei tuoi gestori, disponibile sotto la proprietà req.body..
  - [btoa v1.2.1](https://www.npmjs.com/package/btoa) - Una porta della funzione btoa del browser.
  - [cassandra-driver v3.5.0](https://www.npmjs.com/package/cassandra-driver) - Driver Node.js DataStax per Apache Cassandra.
  - [cloudant v1.10.0](https://www.npmjs.com/package/cloudant) - Questa è la libreria Cloudant ufficiale per Node.js.
  - [@cloudant/cloudant v2.2.0](https://www.npmjs.com/package/cloudant) - Questa è la libreria Cloudant ufficiale per Node.js.
  - [commander v2.15.1](https://www.npmjs.com/package/commander) - La soluzione completa per le interfacce della riga di comando node.js.
  - [composeaddresstranslator v1.0.4](https://www.npmjs.com/package/composeaddresstranslator) - Traduttore di indirizzi dall'IU o API Compose per i database Scylla.
  - [consul v0.32.0](https://www.npmjs.com/package/consul) - Un client per Consul, che implica il rilevamento e la configurazione del servizio.
  - [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - Analizza l'intestazione del cookie e popola req.cookies con un oggetto codificato dai nomi dei cookie.
  - [cradle v0.7.1](https://www.npmjs.com/package/cradle) - Un client CouchDB di livello superiore con memorizzazione nella cache per Node.js.
  - [elasticsearch v15.0.0](https://www.npmjs.com/package/elasticsearch) - Il client Elasticsearch di secondo livello ufficiale per Node.js.
  - [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - Middleware di gestione degli errori di solo sviluppo.
  - [etcd3 v0.2.11](https://www.npmjs.com/package/etcd3) - Un client di alta qualità, pronto per la produzione per l'API etcdv3 API basata su Protocol Buffer.
  - [express v4.16.3](https://www.npmjs.com/package/express) - Framework veloce, semplice e minimalista per Node.
  - [express-session v1.15.6](https://www.npmjs.com/package/express-session) - Consente la gestione delle sessioni da un'applicazione Express.
  - [formidable v1.2.1](https://www.npmjs.com/package/formidable) - Un modulo Node.js per analizzare i dati del modulo, in particolare i caricamenti di file.
  - [glob v7.1.2](https://www.npmjs.com/package/glob) - Abbina i file con i modelli utilizzati dalla shell, come stelle e altri elementi.
  - [gm v1.23.1](https://www.npmjs.com/package/gm) - GraphicsMagick e ImageMagick per Node.
  - [ibm-cos-sdk v1.2.1](https://www.npmjs.com/package/ibm-cos-sdk) - SDK {{site.data.keyword.cos_full}} per Node.js
  - [ibm_db v2.4.0](https://www.npmjs.com/package/ibm_db) - Un'interfaccia asincrona/sincrona per node.js su IBM DB2 e IBM Informix.
  - [ibmiotf v0.2.41](https://www.npmjs.com/package/ibmiotf) - Il client node.js viene utilizzato per semplificare l'interazione con la piattaforma IBM Watson Internet of Things.
  - [iconv-lite v0.4.23](https://www.npmjs.com/package/iconv-lite) - Conversione della codifica dei caratteri Pure JS
  - [jsdom v11.10.0](https://www.npmjs.com/package/jsdom) - jsdom è un'implementazione puramente JavaScript di molti standard web, in particolare gli standard WHATWG DOM e HTML.
  - [jsonwebtoken v8.2.2](https://www.npmjs.com/package/jsonwebtoken) - Un'implementazione dei token web JSON.
  - [lodash v4.17.10](https://www.npmjs.com/package/lodash) - La libreria Lodash esportata come moduli Node.js.
  - [log4js v2.8.0](https://www.npmjs.com/package/log4js) - Questa è una conversione del framework log4js per funzionare con Node.
  - [marked v0.4.0](https://www.npmjs.com/package/marked) - Un programma di analisi e compilatore di markdown completo di tutte le funzionalità, scritto in JavaScript. Costruito per garantire velocità.
  - [merge v1.2.0](https://www.npmjs.com/package/merge) - Unisci più oggetti in uno, per creare facoltativamente un nuovo oggetto clonato.
  - [moment v2.22.2](https://www.npmjs.com/package/moment) - Una libreria di date JavaScript leggera per l'analisi, la convalida, la manipolazione e la formattazione delle date. 
  - [mongodb v3.0.10](https://www.npmjs.com/package/mongodb) - Il driver MongoDB ufficiale per Node.js.
  - [mysql v2.15.0](https://www.npmjs.com/package/mysql) - Questo è un driver node.js per mysql.
  - [mustache v2.3.0](https://www.npmjs.com/package/mustache) - mustache.js è un'implementazione del sistema di template mustache in JavaScript.
  - [nano v6.4.4](https://www.npmjs.com/package/nano) - Driver couchdb minimalista per Node.js.
  - [nodemailer v4.6.5](https://www.npmjs.com/package/nodemailer) - Invia e-mail da Node.js in modo semplicissimo.
  - [oauth2-server v3.0.0](https://www.npmjs.com/package/oauth2-server) - Modulo completo, conforme e ben testato per l'implementazione di un server/provider OAuth2 in modo esplicito in Node.js.
  - [openwhisk v3.15.0](https://www.npmjs.com/package/openwhisk) - Libreria client JavaScript per la piattaforma OpenWhisk. Fornisce un wrapper attorno alle API OpenWhisk.
  - [path-to-regex v2.2.1](https://www.npmjs.com/package/path-to-regexp) - Trasforma una stringa di percorso come /user/:name in un'espressione regolare che può quindi essere utilizzata per la corrispondenza con i percorsi URL.
  - [pg v7.4.3](https://www.npmjs.com/package/pg) - Client PostgreSQL non bloccante per node.js. Bind di libpq nativi facoltativi e Pure JavaScript.
  - [process v0.11.10](https://www.npmjs.com/package/process) - require('process'); proprio come qualsiasi altro modulo.
  - [pug v2.0.3](https://www.npmjs.com/package/pug) - Implementa il linguaggio di templating Pug.
  - [redis v2.8.0](https://www.npmjs.com/package/redis) - Questo è un client Redis completo e ricco di funzioni per Node.js.
  - [request v2.87.0](https://www.npmjs.com/package/request) - Request è progettato per essere il modo più semplice per effettuare chiamate HTTP.
  - [request-promise v4.2.2](https://www.npmjs.com/package/request-promise) - Il client di richiesta HTTP semplificato 'request' con il supporto della Promessa. Con tecnologia Bluebird.
  - [rimraf v2.6.2](https://www.npmjs.com/package/rimraf) - Il comando UNIX rm -rf per Node.
  - [semver v5.5.0](https://www.npmjs.com/package/semver) - Controllo versioni semantico per Nodejs
  - [@sendgrid/mail@6.2.1](https://www.npmjs.com/package/@sendgrid/mail) - Fornisce il supporto e-mail tramite l'API SendGrid.
  - [serve-favicon v2.5.0](https://www.npmjs.com/package/serve-favicon) - Middleware Node.js per servire un favicon.
  - [superagent v3.8.3](https://www.npmjs.com/package/superagent) - SuperAgent è una piccola libreria di richieste HTTP lato client progressiva e un modulo Node.js con la stessa API, con molte funzioni client HTTP di livello superiore.
  - [twilio v3.17.2](https://www.npmjs.com/package/twilio) - Un wrapper per l'API Twilio, relativo a voce, video e messaggi.
  - [underscore v1.9.1](https://www.npmjs.com/package/underscore) - Underscore.js è una libreria di programmi di utilità per JavaScript che fornisce supporto per i soliti sospetti funzionali (each, map, reduce, filter...) senza estendere gli oggetti JavaScript di base.
  - [url-pattern v1.0.3](https://www.npmjs.com/package/url-pattern) - Analizza gli URL per i parametri di percorso più facilmente rispetto all'utilizzo di un matcher di stringhe di espressioni regolari.
  - [uuid v3.2.1](https://www.npmjs.com/package/uuid) - Generazione semplice e veloce di UUID RFC4122.
  - [validator v10.3.0](https://www.npmjs.com/package/validator) - Una libreria di programmi di convalida e di pulizia di stringhe.
  - [watson-developer-cloud v3.4.5](https://www.npmjs.com/package/watson-developer-cloud) - Libreria client Node.js per utilizzare i servizi Watson Developer Cloud, una raccolta di API che utilizzano il calcolo cognitivo per risolvere problemi complessi.
  - [when v3.7.8](https://www.npmjs.com/package/when) - When.js è un'implementazione solida e collaudata di Promises/A+ e when(), che include uno shim completo di ES6 Promise.
  - [winston v2.4.2](https://www.npmjs.com/package/winston) - Una libreria di registrazione asincrona multi-trasporto per node.js. "WINSTON è nei log."
  - [ws v5.2.0](https://www.npmjs.com/package/ws) - ws è un'implementazione client e server WebSocket semplice da usare, molto veloce e completamente testata.
  - [xml2js v0.4.19](https://www.npmjs.com/package/xml2js) - Semplice convertitore di oggetti XML in JavaScript. Supporta la conversione bidirezionale.
  - [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest è un wrapper per il client http incorporato per emulare l'oggetto XMLHttpRequest del browser.
  - [yauzl v2.9.1](https://www.npmjs.com/package/yauzl) - Ancora un'altra libreria di decompressione per Node. Per la compressione.

### Ambiente Node.js versione 6
{: #openwhisk_ref_javascript_environments_6}
L'ambiente Node.js 6.14.0 viene utilizzato se l'indicatore `--kind` viene specificato esplicitamente con il valore `nodejs:6` quando si crea o si aggiorna un'azione.

I seguenti pacchetti sono disponibili per l'utilizzo nell'ambiente Node.js 6.14.0:

- [apn v2.1.2](https://www.npmjs.com/package/apn) - Un modulo Node.js per l'interfacciamento con il servizio Apple Push Notification.
- [async v2.1.4](https://www.npmjs.com/package/async) - Fornisce funzioni per lavorare con funzioni asincrone.
- [btoa v1.1.2](https://www.npmjs.com/package/btoa) - Una porta della funzione btoa del browser.
- [cheerio v0.22.0](https://www.npmjs.com/package/cheerio) - Implementazione rapida, flessibile e snella del core jQuery progettato specificamente per il server.
- [cloudant v1.6.2](https://www.npmjs.com/package/cloudant) - La libreria Cloudant ufficiale per Node.js.
- [commander v2.9.0](https://www.npmjs.com/package/commander) - La soluzione completa per le interfacce della riga di comando Node.js.
- [consul v0.27.0](https://www.npmjs.com/package/consul) - Un client per Consul, che implica il rilevamento e la configurazione del servizio.
- [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - Analizza l'intestazione del cookie e popola req.cookies con un oggetto codificato dai nomi dei cookie.
- [cradle v0.7.1](https://www.npmjs.com/package/cradle) - Un client CouchDB di livello superiore con memorizzazione nella cache per Node.js.
- [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - Middleware di gestione degli errori di solo sviluppo.
- [glob v7.1.1](https://www.npmjs.com/package/glob) - Abbina i file utilizzando i modelli usati dalla shell, come stelle e altri elementi.
- [gm v1.23.0](https://www.npmjs.com/package/gm) - GraphicsMagick e ImageMagick per Node.
- [lodash v4.17.2](https://www.npmjs.com/package/lodash) - La libreria Lodash che viene esportata come moduli Node.js.
- [log4js v0.6.38](https://www.npmjs.com/package/log4js) - Una conversione del framework log4js progettato per funzionare con Node.
- [iconv-lite v0.4.15](https://www.npmjs.com/package/iconv-lite) - Conversione della codifica dei caratteri Pure JS
- [marked v0.3.6](https://www.npmjs.com/package/marked) - Un programma di analisi e compilatore markdown completo, che è scritto in JavaScript. Costruito per garantire velocità.
- [merge v1.2.0](https://www.npmjs.com/package/merge) - Unisci più oggetti in uno, per creare un nuovo oggetto clonato.
- [moment v2.17.0](https://www.npmjs.com/package/moment) - Una libreria di date JavaScript leggera per l'analisi, la convalida, la manipolazione e la formattazione delle date.
- [mongodb v2.2.11](https://www.npmjs.com/package/mongodb) - Il driver MongoDB ufficiale per Node.js.
- [mustache v2.3.0](https://www.npmjs.com/package/mustache) - Mustache.js è un'implementazione del sistema di template mustache in JavaScript.
- [nano v6.2.0](https://www.npmjs.com/package/nano) - Driver couchdb minimalista per Node.js.
- [node-uuid v1.4.7](https://www.npmjs.com/package/node-uuid) - UUID fornito obsoleto.
- [nodemailer v2.6.4](https://www.npmjs.com/package/nodemailer) - Invia e-mail da Node.js in modo semplicissimo.
- [oauth2-server v2.4.1](https://www.npmjs.com/package/oauth2-server) - Modulo completo, conforme e ben testato per l'implementazione di un server/provider OAuth2 in modo esplicito in Node.js.
- [openwhisk v3.14.0](https://www.npmjs.com/package/openwhisk) - Libreria client JavaScript per la piattaforma OpenWhisk. Fornisce un wrapper attorno alle API OpenWhisk.
- [pkgcloud v1.4.0](https://www.npmjs.com/package/pkgcloud) - pkgcloud è una libreria standard per Node.js che astrae le differenze tra più provider cloud.
- [process v0.11.9](https://www.npmjs.com/package/process) - require('process'); proprio come qualsiasi altro modulo.
- [pug v2.0.0-beta6](https://www.npmjs.com/package/pug) - Implementa il linguaggio di templating Pug.
- [redis v2.6.3](https://www.npmjs.com/package/redis) - Un client Redis completo e ricco di funzioni per Node.js.
- [request v2.79.0](https://www.npmjs.com/package/request) - La richiesta è il modo più semplice per effettuare chiamate HTTP.
- [request-promise v4.1.1](https://www.npmjs.com/package/request-promise) - Il client di richiesta HTTP semplificato 'request' con il supporto della Promessa. Con tecnologia Bluebird.
- [rimraf v2.5.4](https://www.npmjs.com/package/rimraf) - Il comando UNIX rm -rf per Node.
- [semver v5.3.0](https://www.npmjs.com/package/semver) - Supporta la creazione delle versioni semantiche.
- [sendgrid v4.7.1](https://www.npmjs.com/package/sendgrid) - Fornisce il supporto e-mail con l'API SendGrid.
- [serve-favicon v2.3.2](https://www.npmjs.com/package/serve-favicon) - Middleware Node.js per servire un favicon.
- [socket.io v1.6.0](https://www.npmjs.com/package/socket.io) - Socket.IO consente comunicazioni bidirezionali basate su eventi in tempo reale.
- [socket.io-client v1.6.0](https://www.npmjs.com/package/socket.io-client) - Supporto lato client per Socket.IO.
- [superagent v3.0.0](https://www.npmjs.com/package/superagent) - SuperAgent è una piccola libreria di richieste HTTP lato client progressiva e modulo Node.js con la stessa API, con molte funzioni client HTTP di livello superiore.
- [swagger-tools v0.10.1](https://www.npmjs.com/package/swagger-tools) - Strumenti correlati all'utilizzo di Swagger, un modo per documentare le API.
- [tmp v0.0.31](https://www.npmjs.com/package/tmp) - Un semplice programma di creazione di file e directory temporanei per node.js.
- [twilio v2.11.1](https://www.npmjs.com/package/twilio) - Un wrapper per l'API Twilio, relativo a voce, video e messaggi.
- [underscore v1.8.3](https://www.npmjs.com/package/underscore) - Underscore.js è una libreria di programmi di utilità per JavaScript che supporta i soliti aspetti funzionali (each, map, reduce, filter...) senza estendere gli oggetti JavaScript di base.
- [uuid v3.0.0](https://www.npmjs.com/package/uuid) - Generazione semplice e veloce di UUID RFC4122.
- [validator v6.1.0](https://www.npmjs.com/package/validator) - Una libreria di programmi di convalida e di pulizia di stringhe.
- [watson-developer-cloud v2.29.0](https://www.npmjs.com/package/watson-developer-cloud) - Libreria client Node.js per utilizzare i servizi Watson Developer Cloud, una raccolta di API che utilizzano il calcolo cognitivo per risolvere problemi complessi.
- [when v3.7.7](https://www.npmjs.com/package/when) - When.js è un'implementazione solida e collaudata di Promises/A+ e when(), che include uno shim completo di ES6 Promise.
- [winston v2.3.0](https://www.npmjs.com/package/winston) - Una libreria di registrazione asincrona multi-trasporto per node.js. "WINSTON è nei log."
- [ws v1.1.1](https://www.npmjs.com/package/ws) - ws è un'implementazione client e server WebSocket semplice da usare, molto veloce e completamente testata.
- [xml2js v0.4.17](https://www.npmjs.com/package/xml2js) - Semplice convertitore di oggetti XML in JavaScript. Supporta la conversione bidirezionale.
- [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest è un wrapper per il client http incorporato per emulare l'oggetto XMLHttpRequest del browser.
- [yauzl v2.7.0](https://www.npmjs.com/package/yauzl) - Ancora un'altra libreria decompressa per node. Per la compressione.


## Ambienti di runtime Python
{: #openwhisk_ref_python_environments}

OpenWhisk supporta l'esecuzione di azioni Python utilizzando due diverse versioni di runtime.

### Azioni Python 3 (basate su Jessie)
{: #openwhisk_ref_python_environments_jessie}

Le azioni Python 3 vengono eseguite con Python 3.6.5. Per utilizzare questo runtime, specifica il parametro `--kind python-jessie:3` della CLI `wsk` quando crei o aggiorni un'azione.
Quando crei azioni python usando virtualenv, utilizza l'immagine docker `ibmfunctions/action-python-v3`.
Il runtime contiene pacchetti SDK per i servizi IBM Cloud che possono essere utilizzati dalle azioni Python, in aggiunta alle librerie standard di Python 3.6.

Versione Python:
- [3.6.5](https://github.com/docker-library/python/blob/a1aa406bfd8c7b129e6e0ee0ba972b863624ac0d/3.6/jessie/Dockerfile)

Pacchetti Python:
- asn1crypto==0.24.0
- attrs==17.4.0
- Automat==0.6.0
- beautifulsoup4==4.6.0
- botocore==1.9.4
- cassandra-driver==3.14.0
- certifi==2018.1.18
- cffi==1.11.5
- chardet==3.0.4
- click==6.7
- cloudant==2.8.1
- constantly==15.1.0
- cryptography==2.1.4
- cssselect==1.0.3
- docutils==0.14
- elasticsearch==6.2.0
- Flask==1.0.2
- gevent==1.2.2
- greenlet==0.4.13
- httplib2==0.11.3
- hyperlink==18.0.0
- ibm-cos-sdk==2.1.1
- ibm-cos-sdk-core==2.1.1
- ibm-cos-sdk-s3transfer==2.1.1
- ibm-db==2.0.8a0
- ibmcloudsql==0.2.13
- idna==2.6
- incremental==17.5.0
- itsdangerous==0.24
- Jinja2==2.10
- jmespath==0.9.3
- kafka-python==1.4.3
- lxml==4.2.1
- MarkupSafe==1.0
- numpy==1.14.4
- pandas==0.23.0
- parsel==1.4.0
- pika==0.11.2
- Pillow==5.1.0
- psycopg2==2.7.4
- pyasn1==0.4.2
- pyasn1-modules==0.2.1
- pycparser==2.18
- PyDispatcher==2.0.5
- pymongo==3.6.1
- pyOpenSSL==17.5.0
- pysolr==3.7.0
- python-dateutil==2.7.3
- pytz==2018.3
- queuelib==1.4.2
- redis==2.10.6
- requests==2.18.4
- scikit-learn==0.19.1
- scipy==1.1.0
- Scrapy==1.5.0
- service-identity==17.0.0
- simplejson==3.15.0
- six==1.11.0
- tornado==5.0.2
- Twisted==18.4.0
- urllib3==1.22
- virtualenv==16.0.0
- w3lib==1.19.0
- watson-developer-cloud==1.3.5
- Werkzeug==0.14.1
- zope.interface==4.4.3

### Azioni Python 3 (basate su Alpine)
{: #openwhisk_ref_python_environments_alpine}

Le azioni Python 3 vengono eseguite con Python 3.6.1. Per utilizzare questo runtime, specifica il parametro `--kind python:3` della CLI `wsk` quando crei o aggiorni un'azione.
Quando crei azioni python usando virtualenv, utilizza l'immagine docker `openwhisk/python3action`.
I seguenti pacchetti possono essere utilizzati dalle azioni Python, in aggiunta alle librerie standard di Python 3.6.

Pacchetti Python:
- asn1crypto==0.23.0
- attrs==17.3.0
- Automat==0.6.0
- beautifulsoup4==4.5.3
- cffi==1.11.2
- click==6.7
- constantly==15.1.0
- cryptography==2.1.3
- cssselect==1.0.1
- Flask==0.12
- gevent==1.2.1
- greenlet==0.4.12
- httplib2==0.10.3
- idna==2.6
- incremental==17.5.0
- itsdangerous==0.24
- Jinja2==2.9.6
- kafka-python==1.3.4
- lxml==3.7.3
- MarkupSafe==1.0
- parsel==1.2.0
- pyasn1==0.3.7
- pyasn1-modules==0.1.5
- pycparser==2.18
- PyDispatcher==2.0.5
- pyOpenSSL==17.3.0
- python-dateutil==2.6.0
- queuelib==1.4.2
- requests==2.13.0
- Scrapy==1.3.3
- service-identity==17.0.0
- simplejson==3.10.0
- six==1.11.0
- Twisted==17.1.0
- virtualenv==15.1.0
- w3lib==1.18.0
- Werkzeug==0.12.2
- zope.interface==4.4.3

### Azioni Python 2

Le azioni Python 2 vengono eseguite con Python 2.7.12, che è il runtime predefinito per le azioni Python, a meno che non specifichi l'indicatore `--kind` quando crei o aggiorni un'azione. Per selezionare esplicitamente questo runtime, utilizza `--kind python:2`.
Quando crei azioni python usando virtualenv, utilizza l'immagine docker `openwhisk/python2action`.
I seguenti pacchetti possono essere utilizzati dalle azioni Python 2, in aggiunta alla libreria standard di Python 2.7.

Pacchetti Python:
- asn1crypto==0.23.0
- attrs==17.2.0
- beautifulsoup4==4.5.1
- cffi==1.11.1
- click==6.7
- cryptography==2.0.3
- cssselect==1.0.1
- enum34==1.1.6
- Flask==0.11.1
- gevent==1.1.2
- greenlet==0.4.12
- httplib2==0.9.2
- idna==2.6
- ipaddress==1.0.18
- itsdangerous==0.24
- Jinja2==2.9.6
- kafka-python==1.3.1
- lxml==3.6.4
- MarkupSafe==1.0
- parsel==1.2.0
- pyasn1==0.3.7
- pyasn1-modules==0.1.4
- pycparser==2.18
- PyDispatcher==2.0.5
- pyOpenSSL==17.3.0
- python-dateutil==2.5.3
- queuelib==1.4.2
- requests==2.11.1
- Scrapy==1.1.2
- service-identity==17.0.0
- simplejson==3.8.2
- six==1.11.0
- Twisted==16.4.0
- virtualenv==15.1.0
- w3lib==1.18.0
- Werkzeug==0.12.2
- zope.interface==4.4.3

## Azioni Swift
{: #swift-actions}

### Swift 3
Le azioni Swift 3 vengono eseguite con Swift 3.1.1 `--kind swift:3.1.1`. Specifica sempre il tipo `swift:3.1.1` in quanto le versioni precedenti di Swift non sono supportate.

Devi migrare tutte le azioni Swift per utilizzare il tipo `swift:3.1.1`. Come procedura consigliata, fornisci sempre il tipo specifico quando crei o aggiorni le azioni.
{: tip}

Le azioni Swift 3.1.1 possono utilizzare i seguenti pacchetti quando si utilizza un singolo file di origine Swift:
- KituraNet versione 1.7.6, https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON versione 15.0.1, https://github.com/IBM-Swift/SwiftyJSON
- SDK Watson Developer Cloud versione 0.16.0, https://github.com/watson-developer-cloud/swift-sdk

### Swift 4
Le azioni Swift 4 vengono eseguite utilizzando Swift 4.1 `--kind swift:4.1`.

Segui le istruzioni relative alle [azioni swift in pacchetto](./openwhisk_actions.html#packaging-an-action-as-a-swift-executable) per includere dipendenze utilizzando un Package.swift.

Le azioni Swift 4.1 possono utilizzare i seguenti pacchetti quando si utilizza un singolo file di origine Swift:
- SDK Watson Developer Cloud versione 0.27.0, https://github.com/watson-developer-cloud/swift-sdk

### Migrazione da Swift 3.1.1 a Swift 4.1

#### SwiftyJSON che utilizza un singolo file di azioni di origine
Se hai un'azione `swift:3.1.1` che non è compilata, come un file di origine che utilizza il pacchetto **SwiftyJSON**, devi precompilare la tua azione e specificare la versione di SwiftyJSON che vuoi utilizzare per l'azione di tipo `swift:4.1`. Tieni in considerazione che, a partire da Swift 4.1, esiste un supporto migliore per gestire i dati JSON in modo nativo.

## Azioni PHP
{: #openwhisk_ref_php}

Le azioni PHP vengono eseguite con PHP 7.1.18. Per utilizzare questo runtime, specifica il parametro `--kind php:7.1` della CLI `wsk` quando crei o aggiorni un'azione. Questo è il comportamento predefinito quando crei un'azione con un file che ha un'estensione `.php`.

Sono disponibili le seguenti estensioni PHP in aggiunta a quelle standard:

- bcmath
- curl
- gd
- intl
- mbstring
- mysqli
- pdo_mysql
- pdo_pgsql
- pdo_sqlite
- soap
- zip

### Pacchetti Composer
Sono disponibili anche i seguenti pacchetti Composer:

- guzzlehttp/guzzle       v6.7.3
- ramsey/uuid             v3.6.3

## Azioni Docker
{: #openwhisk_ref_docker}

Le azioni Docker eseguono un file binario fornito dall'utente in un contenitore Docker. Il file binario viene eseguito in un'immagine Docker basata su [python:2.7.12-alpine](https://hub.docker.com/r/library/python), pertanto il file binario deve essere compatibile con questa distribuzione.

La struttura di base Docker può essere opportunamente utilizzata per creare immagini Docker compatibili con OpenWhisk. Puoi installare la struttura di base con il comando del plug-in della CLI `ibmcloud wsk sdk install docker`.

Il programma binario principale deve trovarsi in `/action/exec` all'interno del contenitore. L'eseguibile riceve gli argomenti di input da una singola stringa di argomenti della riga di comando che può essere deserializzata come oggetto `JSON`. Deve restituire un risultato mediante `stdout` sotto forma di stringa a singola riga del `JSON` serializzato.

Puoi includere eventuali passi di compilazione o dipendenze modificando il `Dockerfile` compreso nel `dockerSkeleton`.

## API REST
{: #openwhisk_ref_restapi}
Per informazioni sull'API REST {{site.data.keyword.openwhisk_short}}, vedi la [Guida di riferimento API REST](https://console.bluemix.net/apidocs/98-cloud-functions?&language=node#introduction).

## Limiti di sistema
{: #openwhisk_syslimits}

### Azioni
{{site.data.keyword.openwhisk_short}} ha alcuni limiti di sistema, tra cui la quantità di memoria che un'azione può utilizzare e il numero di chiamate di azioni consentite al minuto.

La seguente tabella elenca i limiti predefiniti per le azioni.

| Limite | Descrizione | Impostazione predefinita | Minimo | Massimo |
| ----- | ----------- | :-------: | :---: | :---: |
| [codeSize](openwhisk_reference.html#openwhisk_syslimits_codesize) | La dimensione massima del codice azione in MB. | 48 | 1 | 48 |
| [concurrent](openwhisk_reference.html#openwhisk_syslimits_concurrent) | Non è possibile inviare più di N attivazioni per ogni spazio dei nomi in esecuzione o in coda per l'esecuzione. | 1000 | 1 | 1000* |
| [logs](openwhisk_reference.html#openwhisk_syslimits_logs) | Un contenitore non può scrivere più di N MB in stdout. | 10 | 0 | 10 |
| [memory](openwhisk_reference.html#openwhisk_syslimits_memory) | Un contenitore non può assegnare più di N MB di memoria. | 256 | 128 | 512 |
| [minuteRate](openwhisk_reference.html#openwhisk_syslimits_minuterate) | Non è possibile inviare più di N attivazioni per ogni spazio dei nomi al minuto. | 5000 | 1 | 5000* |
| [openulimit](openwhisk_reference.html#openwhisk_syslimits_openulimit) | Il numero massimo di file aperti per un'azione. | 1024 | 0 | 1024 |
| [parameters](openwhisk_reference.html#openwhisk_syslimits_parameters) | La dimensione massima dei parametri che possono essere allegati in MB. | 1 | 0 | 1 |
| [proculimit](openwhisk_reference.html#openwhisk_syslimits_proculimit) | Il numero massimo di processi disponibili per un'azione. | 1024 | 0 | 1024 |
| [result](openwhisk_reference.html#openwhisk_syslimits_result) | La dimensione massima del risultato della chiamata di azione in MB. | 1 | 0 | 1 |
| [sequenceMaxActions](openwhisk_reference.html#openwhisk_syslimits_sequencemax) | Il numero massimo di azioni comprese in una determinata sequenza. | 50 | 0 | 50* |
| [timeout](openwhisk_reference.html#openwhisk_syslimits_timeout) | Un contenitore non può essere eseguito per più di N millisecondi. | 60000 | 100 | 600000 |

### Aumento dei limiti fissi
{: #increase_fixed_limit}

I valori limite che terminano con un (*) sono fissi, ma possono essere aumentati se un caso di business può giustificare valori limite di sicurezza più elevati. Se vuoi aumentare il valore limite, contatta il supporto IBM aprendo un ticket direttamente dalla [console web IBM {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/).
  1. Seleziona **Supporto**
  2. Seleziona **Aggiungi ticket** dal menu a discesa.
  3. Seleziona **Tecnico** per il tipo di ticket.
  4. Seleziona **Functions** per l'area tecnica di supporto.

#### codeSize (MB) (valore fisso: 48 MB)
{: #openwhisk_syslimits_codesize}
* La dimensione massima del codice per l'azione è 48 MB.
* Per un'azione JavaScript, si consiglia di utilizzare uno strumento per concatenare tutto il codice sorgente, che include le dipendenze, in un singolo file di bundle.
* Questo limite è fisso e non può essere modificato.

#### concurrent (valore fisso: 1000*)
{: #openwhisk_syslimits_concurrent}
* Il numero di attivazioni che sono in esecuzione o in coda per l'esecuzione per uno spazio dei nomi non può essere maggiore di 1000.
* Questo valore limite è fisso, ma può essere aumentato se un caso di business può giustificare valori limite di sicurezza più elevati. Consulta la sezione [Aumento dei limiti fissi](openwhisk_reference.html#increase_fixed_limit) per istruzioni dettagliate su come aumentare questo limite.

#### logs (MB) (valore fisso: 10 MB)
{: #openwhisk_syslimits_logs}
* Il limite di log N è compreso nell'intervallo  [0 MB..10 MB] ed è impostato per ogni azione.
* Un utente può modificare il limite del log delle azioni quando un'azione viene creata o aggiornata.
* I log che superano il limite impostato vengono troncati, quindi tutte le nuove voci di log vengono ignorate e viene aggiunta un'avvertenza come ultimo output dell'attivazione per indicare che l'attivazione ha superato il limite di log impostato.

#### memory (MB) (valore fisso: 256 MB)
{: #openwhisk_syslimits_memory}
* Il limite di memoria M è compreso nell'intervallo [128 MB..512 MB] ed è impostato per ogni azione in MB.
* Un utente può modificare il limite di memoria quando viene creata un'azione.
* Un contenitore non può utilizzare più memoria di quella assegnata dal limite.

#### minuteRate (valore fisso: 5000*)
{: #openwhisk_syslimits_minuterate}
* Il limite di frequenza N è impostato su 5000 e limita il numero di chiamate di azioni in finestre di 1 minuto.
* Una chiamata CLI o API che superi questo limite riceverà un codice di errore corrispondente al codice di stato HTTP `429: TOO MANY REQUESTS`.
* Questo valore limite è fisso, ma può essere aumentato se un caso di business può giustificare valori limite di sicurezza più elevati. Consulta la sezione [Aumento dei limiti fissi](openwhisk_reference.html#increase_fixed_limit) per istruzioni dettagliate su come aumentare questo limite.

#### openulimit (valore fisso: 1024:1024)
{: #openwhisk_syslimits_openulimit}
* Il numero massimo di file aperti per un'azione è 1024 (per entrambi i limiti hard e soft).
* Questo limite è fisso e non può essere modificato.
* Quando viene richiamata un'azione, il comando docker run utilizza l'argomento `--ulimit nofile=1024:1024` per impostare il valore `openulimit`.
* Per ulteriori informazioni, vedi la documentazione di riferimento della riga di comando [docker run](https://docs.docker.com/engine/reference/commandline/run).

#### parameters (valore fisso: 1 MB)
{: #openwhisk_syslimits_parameters}
* Il limite di dimensione per i parametri totali durante la creazione o l'aggiornamento di un'azione, un pacchetto o un trigger è 1 MB.
* Un'entità con parametri troppo grandi viene rifiutata durante il tentativo di crearla o aggiornarla.
* Questo limite è fisso e non può essere modificato.

#### proculimit (valore fisso: 1024:1024)
{: #openwhisk_syslimits_proculimit}
* Il numero massimo di processi disponibili per un contenitore di azioni è 1024.
* Questo limite è fisso e non può essere modificato.
* Quando viene richiamata un'azione, il comando docker run utilizza l'argomento `--pids-limit 1024` per impostare il valore `proculimit`.
* Per ulteriori informazioni, vedi la documentazione di riferimento della riga di comando [docker run](https://docs.docker.com/engine/reference/commandline/run).

#### result (valore fisso: 1 MB)
{: #openwhisk_syslimits_result}
* La dimensione massima di output del risultato di una chiamata di azione in MB. 
* Questo limite è fisso e non può essere modificato.

#### sequenceMaxActions (valore fisso: 50*)
{: #openwhisk_syslimits_sequencemax}
* Il numero massimo di azioni comprese in una determinata sequenza. 
* Questo limite è fisso e non può essere modificato.

#### timeout (ms) (valore predefinito: 60s)
{: #openwhisk_syslimits_timeout}
* Il limite di timeout N è compreso nell'intervallo [100 ms..600000 ms] ed è impostato per ogni azione in millisecondi.
* Un utente può modificare il limite di timeout quando viene creata un'azione.
* Un contenitore in esecuzione per più di N millisecondi viene terminato.

### Trigger

I trigger sono soggetti a una frequenza di attivazione al minuto, come indicato nella seguente tabella.

| Limite | Descrizione | Impostazione predefinita | Minimo | Massimo |
| ----- | ----------- | :-------: | :---: | :---: |
| [minuteRate](openwhisk_reference.html#openwhisk_syslimits_tminuterate) | Non è possibile attivare più di N trigger per spazio dei nomi al minuto. | 5000* | 5000* | 5000* |

### Aumento dei limiti fissi
{: #increase_fixed_tlimit}

I valori limite che terminano con un (*) sono fissi, ma possono essere aumentati se un caso di business può giustificare valori limite di sicurezza più elevati. Se vuoi aumentare il valore limite, contatta il supporto IBM aprendo un ticket direttamente dalla [console web IBM {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/).
  1. Seleziona **Supporto**
  2. Seleziona **Aggiungi ticket** dal menu a discesa.
  3. Seleziona **Tecnico** per il tipo di ticket.
  4. Seleziona **Functions** per l'area tecnica di supporto.

#### minuteRate (valore fisso: 5000*)
{: #openwhisk_syslimits_tminuterate}

* Il limite di frequenza N è impostato su 5000 e limita il numero di trigger che un utente può attivare in finestre di 1 minuto.
* Un utente non può modificare il limite di trigger quando viene creato un trigger.
* Una chiamata CLI o API che superi questo limite riceverà un codice di errore corrispondente al codice di stato HTTP `429: TOO MANY REQUESTS`.
* Questo valore limite è fisso, ma può essere aumentato se un caso di business può giustificare valori limite di sicurezza più elevati. Consulta la sezione [Aumento dei limiti fissi](openwhisk_reference.html#increase_fixed_tlimit) per istruzioni dettagliate su come aumentare questo limite.
