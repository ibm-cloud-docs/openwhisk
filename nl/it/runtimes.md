---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-20"

keywords: runtimes, support

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

# Runtime
{: #runtimes}
Le tue applicazioni possono essere codificate e eseguite in linguaggi di programmazione quali Javascript o Python. Molti runtime vengono forniti per impostazione predefinita con {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

Visualizza i runtime disponibili per IBM Cloud Functions in ciascuna regione. I seguenti link restituiscono una risposta JSON.

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [eu-de](https://eu-de.functions.cloud.ibm.com/)

La sezione `runtimes` della risposta contiene una serie di runtime disponibili.

La sezione `image` contiene il nome dell'immagine di runtime su [Docker Hub](https://hub.docker.com/) e la tag utilizzata.


I seguenti esempi puntano alle immagini `ibmfunctions/action-nodejs-v10` e `openwhisk/nodejs8action`.
Le tag possono essere numeri di versione come `1.9.0` o il formato breve di un hash di commit git, ad esempio `b99d71e`.

Campi immagine di esempio.
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs8action:b99d71e"
  ```

I runtime vengono aggiornati regolarmente. Questi aggiornamenti includono correzioni di sicurezza e aggiornamenti della versione minori ai pacchetti all'interno dei runtime. Gli aggiornamenti della versione minori potrebbero introdurre delle interruzioni con la retrocompatibilità. Gli aggiornamenti del runtime potrebbero influire sulle tue azioni. Devi eseguire la migrazione delle azioni che stanno eseguendo un runtime a una versione più recente aggiornandolo.

Le applicazioni eseguite su runtime obsoleti non possono essere completate correttamente fino a quando il runtime non viene aggiornato a uno supportato. Quando risolvi i problemi di un'azione non riuscita, per identificare se un runtime è obsoleto, esegui un controllo con `deprecated=true` nella risposta della query. Per aggiornare il runtime, vedi [modifica del runtime dell'azione](/docs/openwhisk?topic=cloud-functions-actions#actions_update)

I seguenti runtime sono obsoleti:
<ul>
  <li><code>nodejs:6</code> (obsoleto)</li>
  <li><code>php:7.1</code> (obsoleto)</li>
  <li><code>php:7.2</code> (obsoleto)</li>
  <li><code>swift:3</code> (obsoleto)</li>
  <li><code>swift:3.1.1</code> (obsoleto)</li>
  <li><code>swift:4.1</code> (obsoleto)</li>
  <li><code>ballerina:0.990</code> (obsoleto)</li>
</ul>






## Runtime JavaScript
{: #openwhisk_ref_javascript_environments}

Le azioni JavaScript possono essere eseguite in Node.js versione 8 o 10. Per impostazione predefinita, tutte le azioni Node.js vengono eseguite in un ambiente versione 10.



### Ambiente Node.js versione 10 con gli SDK IBM
{: #openwhisk_ref_javascript_environments_10}
L'ambiente Node.js versione 10 viene utilizzato se l'indicatore `--kind` viene specificato esplicitamente con un valore `nodejs:10` quando si crea o si aggiorna un'azione.

#### Migrazione da `nodejs:8` a `nodejs:10`
- Il pacchetto npm `ibm_db` non è disponibile in `nodejs:10`. Il pacchetto `ibm_db` non supporta Node.js 10. Puoi tenere traccia dei progressi in questo problema [issue ibmdb/node-ibm_db/issues/482](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541).
- Il pacchetto npm `cloudant` non è disponibile in `nodejs:10`, il pacchetto è obsoleto, devi utilizzare il pacchetto npm ufficiale [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) v3.0.0 quando importi il modulo nodejs (ossia `require('@cloudant/cloudant')`) inoltre [v3.x restituisce solo Promises](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x).
- Il pacchetto npm `cradle` non è disponibile in `nodejs:10`.
- Il pacchetto npm `log4js` non è disponibile in `nodejs:10`. Puoi tenere traccia del problema all'indirizzo [log4js-node/issues/805](https://github.com/log4js-node/log4js-node/issues/805)
- Il pacchetto npm `watson-developer-cloud` non è disponibile in `nodejs:10`. Puoi tenere traccia dei progressi sulla nuova versione in questo problema [watson-developer-cloud/node-sdk/issues/780](https://github.com/watson-developer-cloud/node-sdk/issues/780)

Possono essere trovate delle informazioni dettagliate sull'ambiente di runtime Nodejs versione 10 in [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md).

#### Pacchetti Node.js 10.15

   - [10.15.0](https://nodejs.org/en/blog/release/v10.15.0/)

 Pacchetti NPM:
   - [amqplib](https://www.npmjs.com/package/amqplib): una libreria per la creazione di client AMQP 0-9-1 per Node.JS.
   - [apn](https://www.npmjs.com/package/apn): un modulo Node.js per l'interfacciamento con il servizio Apple Push Notification.
   - [async](https://www.npmjs.com/package/async): fornisce funzioni per utilizzare funzioni asincrone.
   - [bent](https://www.npmjs.com/package/bent): client HTTP funzionale per Node.js con async e await.
   - [bodyparser](https://www.npmjs.com/package/body-parser): analizza i corpi delle richieste in entrata in un middleware prima dei tuoi gestori, disponibile sotto la proprietà req.body.
   - [btoa](https://www.npmjs.com/package/btoa): una porta della funzione btoa del browser.
   - [cassandra-driver](https://www.npmjs.com/package/cassandra-driver): driver Node.js DataStax per Apache Cassandra.
   - [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant): libreria Cloudant ufficiale per Node.js.
   - [commander](https://www.npmjs.com/package/commander): soluzione completa per le interfacce della riga di comando node.js.
   - [composeaddresstranslator](https://www.npmjs.com/package/composeaddresstranslator): traduttore di indirizzi dall'IU o API Compose per i database Scylla.
   - [consul](https://www.npmjs.com/package/consul): un client per Consul, che implica il rilevamento e la configurazione del servizio.
   - [cookie-parser](https://www.npmjs.com/package/cookie-parser): analizza l'intestazione del cookie e popola req.cookies con un oggetto codificato dai nomi dei cookie.
   - [elasticsearch](https://www.npmjs.com/package/elasticsearch): il client Elasticsearch di secondo livello ufficiale per Node.js.
   - [errorhandler](https://www.npmjs.com/package/errorhandler): middleware di gestione degli errori di solo sviluppo.
   - [etcd3](https://www.npmjs.com/package/etcd3): un client di alta qualità, pronto per la produzione per l'API etcdv3 API basata su Protocol Buffer.
   - [formidable](https://www.npmjs.com/package/formidable): un modulo Node.js per analizzare i dati del modulo, in particolare i caricamenti di file.
   - [glob](https://www.npmjs.com/package/glob): Abbina i file con i modelli utilizzati dalla shell, come stelle e altri elementi.
   - [gm](https://www.npmjs.com/package/gm): GraphicsMagick e ImageMagick per Node.
   - [ibm-cos-sdk](https://www.npmjs.com/package/ibm-cos-sdk): SDK {{site.data.keyword.cos_full}} per Node.js
   - [ibm_db](https://www.npmjs.com/package/ibm_db): un'interfaccia asincrona/sincrona per node.js su IBM DB2 e IBM Informix.
   - [ibmiotf](https://www.npmjs.com/package/ibmiotf): il client node.js viene utilizzato per semplificare l'interazione con la piattaforma IBM Watson Internet of Things.
   - [iconv-lite](https://www.npmjs.com/package/iconv-lite): conversione della codifica dei caratteri Pure JS
   - [jsdom](https://www.npmjs.com/package/jsdom): jsdom è un'implementazione puramente JavaScript di molti standard web, in particolare gli standard WHATWG DOM e HTML.
   - [jsforce](https://www.npmjs.com/package/jsforce): libreria di API Salesforce per applicazioni JavaScript.
   - [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken): un'implementazione dei token web JSON.
   - [lodash](https://www.npmjs.com/package/lodash): la libreria Lodash esportata come moduli Node.js.
   - [marked](https://www.npmjs.com/package/marked): un programma di analisi e compilatore di markdown completo di tutte le funzionalità, scritto in JavaScript. Costruito per garantire velocità.
   - [merge](https://www.npmjs.com/package/merge): unisci più oggetti in uno, per creare facoltativamente un nuovo oggetto clonato.
   - [moment](https://www.npmjs.com/package/moment): una libreria di date JavaScript leggera per l'analisi, la convalida, la manipolazione e la formattazione delle date.
   - [mongodb](https://www.npmjs.com/package/mongodb): il driver MongoDB ufficiale per Node.js.
   - [mysql](https://www.npmjs.com/package/mysql): driver node.js per mysql.
   - [mustache](https://www.npmjs.com/package/mustache): mustache.js è un'implementazione del sistema di template mustache in JavaScript.
   - [nano](https://www.npmjs.com/package/nano): driver couchdb minimalista per Node.js.
   - [nodemailer](https://www.npmjs.com/package/nodemailer): invia e-mail da Node.js in modo semplicissimo.
   - [oauth2-server](https://www.npmjs.com/package/oauth2-server): modulo completo, conforme e ben testato per l'implementazione di un server/provider OAuth2 in modo esplicito in Node.js.
   - [openwhisk](https://www.npmjs.com/package/openwhisk): libreria client JavaScript per la piattaforma OpenWhisk. Fornisce un wrapper attorno alle API OpenWhisk.
   - [path-to-regex](https://www.npmjs.com/package/path-to-regexp): trasforma una stringa di percorso come /user/:name in un'espressione regolare che può quindi essere utilizzata per la corrispondenza con i percorsi URL.
   - [pg](https://www.npmjs.com/package/pg): client PostgreSQL non bloccante per node.js. Bind di libpq nativi facoltativi e Pure JavaScript.
   - [process](https://www.npmjs.com/package/process) - require('process'); proprio come qualsiasi altro modulo.
   - [pug](https://www.npmjs.com/package/pug): implementa il linguaggio di templating Pug.
   - [redis](https://www.npmjs.com/package/redis): client Redis completo e ricco di funzioni per Node.js.
   - [request](https://www.npmjs.com/package/request): request è progettato per essere il modo più semplice per effettuare chiamate HTTP.
   - [request-promise](https://www.npmjs.com/package/request-promise): il client di richiesta HTTP semplificato 'request' con il supporto della Promessa. Con tecnologia Bluebird.
   - [rimraf](https://www.npmjs.com/package/rimraf): comando UNIX rm -rf per Node.
   - [semver](https://www.npmjs.com/package/semver): controllo versioni semantico per Nodejs
   - [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail): fornisce il supporto e-mail tramite l'API SendGrid.
   - [serialize-error](https://www.npmjs.com/package/serialize-error): serializza un errore in un oggetto normale.
   - [serve-favicon](https://www.npmjs.com/package/serve-favicon): middleware Node.js per servire un favicon.
   - [socket.io](https://www.npmjs.com/package/socket.io): Socket.IO consente comunicazioni bidirezionali basate su eventi in tempo reale.
   - [socket.io-client](https://www.npmjs.com/package/socket.io-client): framework applicazione in tempo reale per socket.io.
   - [superagent](https://www.npmjs.com/package/superagent): superAgent è una piccola libreria di richieste HTTP lato client progressiva e un modulo Node.js con la stessa API, con molte funzioni client HTTP di livello superiore.
   - [swagger-tools](https://www.npmjs.com/package/swagger-tools): pacchetto che fornisce vari strumenti per l'integrazione e l'interazionecon Swagger.
   - [twilio](https://www.npmjs.com/package/twilio): un wrapper per l'API Twilio, relativo a voce, video e messaggi.
   - [underscore](https://www.npmjs.com/package/underscore): underscore.js è una libreria di programmi di utilità per JavaScript che fornisce supporto per i soliti sospetti funzionali (each, map, reduce, filter...) senza estendere gli oggetti JavaScript di base.
   - [url-pattern](https://www.npmjs.com/package/url-pattern): analizza gli URL per i parametri di percorso più facilmente rispetto all'utilizzo di un matcher di stringhe di espressioni regolari.
   - [uuid](https://www.npmjs.com/package/uuid): generazione semplice e veloce di UUID RFC4122.
   - [validator](https://www.npmjs.com/package/validator): una libreria di programmi di convalida e di pulizia di stringhe.
   - [vcap_services](https://www.npmjs.com/package/vcap_services): analizza e restituisce credenziali del servizio dalla variabile di ambiente VCAP_SERVICES fornita da IBM Cloud.
   - [when](https://www.npmjs.com/package/when): when.js è un'implementazione solida e collaudata di Promises/A+ e when(), che include uno shim completo di ES6 Promise.
   - [winston](https://www.npmjs.com/package/winston): una libreria di registrazione asincrona multi-trasporto per node.js. "WINSTON è nei log."
   - [ws](https://www.npmjs.com/package/ws): ws è un'implementazione client e server WebSocket semplice da usare, molto veloce e completamente testata.
   - [xlsx](https://www.npmjs.com/package/xlsx): programma di analisi e scrittura per vari formati di foglio di calcolo.
   - [xml2js](https://www.npmjs.com/package/xml2js): semplice convertitore di oggetti XML in JavaScript. Supporta la conversione bidirezionale.
   - [xmlhttprequest](https://www.npmjs.com/package/xmlhttprequest): node-XMLHttpRequest è un wrapper per il client http incorporato per emulare l'oggetto XMLHttpRequest del browser.
   - [yauzl](https://www.npmjs.com/package/yauzl): un'altra libreria di estrazione per il nodo.

### Ambiente Node.js versione 8 con gli SDK IBM
{: #openwhisk_ref_javascript_environments_8}
L'ambiente Node.js versione 8 viene utilizzato se l'indicatore `--kind` viene specificato esplicitamente con un valore `nodejs:8` quando si crea o si aggiorna un'azione.

Node.js versione 8 è nella modalità di manutenzione ed è disponibile fino a dicembre 2019. Vedi la [pianificazione delle release di Node.js](https://github.com/nodejs/Release).
{: deprecated}

#### Pacchetti Node.js 8.15

 - [8.15.0](https://nodejs.org/en/blog/release/v8.15.0)

  - [amqplib](https://www.npmjs.com/package/amqplib): una libreria per la creazione di client AMQP 0-9-1 per Node.JS.
   - [apn](https://www.npmjs.com/package/apn): un modulo Node.js per l'interfacciamento con il servizio Apple Push Notification.
   - [async](https://www.npmjs.com/package/async): fornisce funzioni per utilizzare funzioni asincrone.
   - [bent](https://www.npmjs.com/package/bent): client HTTP funzionale per Node.js con async/await.
   - [bodyparser](https://www.npmjs.com/package/body-parser): analizza i corpi delle richieste in entrata in un middleware prima dei tuoi gestori, disponibile sotto la proprietà req.body.
   - [btoa](https://www.npmjs.com/package/btoa): una porta della funzione btoa del browser.
   - [cassandra-driver](https://www.npmjs.com/package/cassandra-driver): driver Node.js DataStax per Apache Cassandra.
   - [cloudant](https://www.npmjs.com/package/cloudant): libreria Cloudant ufficiale per Node.js.
   - [@cloudant/cloudant](https://www.npmjs.com/package/cloudant): libreria Cloudant ufficiale per Node.js.
   - [commander](https://www.npmjs.com/package/commander): soluzione completa per le interfacce della riga di comando node.js.
   - [composeaddresstranslator](https://www.npmjs.com/package/composeaddresstranslator): traduttore di indirizzi dall'IU o API Compose per i database Scylla.
   - [consul](https://www.npmjs.com/package/consul): un client per Consul, che implica il rilevamento e la configurazione del servizio.
   - [cookie-parser](https://www.npmjs.com/package/cookie-parser): analizza l'intestazione del cookie e popola req.cookies con un oggetto codificato dai nomi dei cookie.
   - [elasticsearch](https://www.npmjs.com/package/elasticsearch): il client Elasticsearch di secondo livello ufficiale per Node.js.
   - [errorhandler](https://www.npmjs.com/package/errorhandler): middleware di gestione degli errori di solo sviluppo.
   - [etcd3](https://www.npmjs.com/package/etcd3): un client di alta qualità, pronto per la produzione per l'API etcdv3 API basata su Protocol Buffer.
   - [formidable](https://www.npmjs.com/package/formidable): un modulo Node.js per analizzare i dati del modulo, in particolare i caricamenti di file.
   - [glob](https://www.npmjs.com/package/glob): Abbina i file con i modelli utilizzati dalla shell, come stelle e altri elementi.
   - [gm](https://www.npmjs.com/package/gm): GraphicsMagick e ImageMagick per Node.
   - [ibm-cos-sdk](https://www.npmjs.com/package/ibm-cos-sdk): SDK {{site.data.keyword.cos_full}} per Node.js
   - [ibm_db](https://www.npmjs.com/package/ibm_db): un'interfaccia asincrona/sincrona per node.js su IBM DB2 e IBM Informix.
   - [ibmiotf](https://www.npmjs.com/package/ibmiotf): il client node.js viene utilizzato per semplificare l'interazione con la piattaforma IBM Watson Internet of Things.
   - [iconv-lite](https://www.npmjs.com/package/iconv-lite): conversione della codifica dei caratteri Pure JS
   - [jsdom](https://www.npmjs.com/package/jsdom): jsdom è un'implementazione puramente JavaScript di molti standard web, in particolare gli standard WHATWG DOM e HTML.
   - [jsforce](https://www.npmjs.com/package/jsforce): libreria di API Salesforce per applicazioni JavaScript.
   - [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken): un'implementazione dei token web JSON.
   - [lodash](https://www.npmjs.com/package/lodash): la libreria Lodash esportata come moduli Node.js.
   - [log4js](https://www.npmjs.com/package/log4js): conversione del framework log4js per renderlo utilizzabile con Node.
   - [marked](https://www.npmjs.com/package/marked): un programma di analisi e compilatore di markdown completo di tutte le funzionalità, scritto in JavaScript. Costruito per garantire velocità.
   - [merge](https://www.npmjs.com/package/merge): unisci più oggetti in uno, per creare facoltativamente un nuovo oggetto clonato.
   - [moment](https://www.npmjs.com/package/moment): una libreria di date JavaScript leggera per l'analisi, la convalida, la manipolazione e la formattazione delle date.
   - [mongodb](https://www.npmjs.com/package/mongodb): il driver MongoDB ufficiale per Node.js.
   - [mysql](https://www.npmjs.com/package/mysql): driver node.js per mysql.
   - [mustache](https://www.npmjs.com/package/mustache): mustache.js è un'implementazione del sistema di template mustache in JavaScript.
   - [nano](https://www.npmjs.com/package/nano): driver couchdb minimalista per Node.js.
   - [nodemailer](https://www.npmjs.com/package/nodemailer): invia e-mail da Node.js in modo semplicissimo.
   - [oauth2-server](https://www.npmjs.com/package/oauth2-server): modulo completo, conforme e ben testato per l'implementazione di un server/provider OAuth2 in modo esplicito in Node.js.
   - [openwhisk](https://www.npmjs.com/package/openwhisk): libreria client JavaScript per la piattaforma OpenWhisk. Fornisce un wrapper attorno alle API OpenWhisk.
   - [path-to-regex](https://www.npmjs.com/package/path-to-regexp): trasforma una stringa di percorso come /user/:name in un'espressione regolare che può quindi essere utilizzata per la corrispondenza con i percorsi URL.
   - [pg](https://www.npmjs.com/package/pg): client PostgreSQL non bloccante per node.js. Bind di libpq nativi facoltativi e Pure JavaScript.
   - [process](https://www.npmjs.com/package/process) - require('process'); proprio come qualsiasi altro modulo.
   - [pug](https://www.npmjs.com/package/pug): implementa il linguaggio di templating Pug.
   - [redis](https://www.npmjs.com/package/redis): client Redis completo e ricco di funzioni per Node.js.
   - [request](https://www.npmjs.com/package/request): request è progettato per essere il modo più semplice per effettuare chiamate HTTP.
   - [request-promise](https://www.npmjs.com/package/request-promise): il client di richiesta HTTP semplificato 'request' con il supporto della Promessa. Con tecnologia Bluebird.
   - [rimraf](https://www.npmjs.com/package/rimraf): comando UNIX rm -rf per Node.
   - [semver](https://www.npmjs.com/package/semver): controllo versioni semantico per Nodejs
   - [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail): fornisce il supporto e-mail tramite l'API SendGrid.
   - [serialize-error](https://www.npmjs.com/package/serialize-error): serializza un errore in un oggetto normale.
   - [serve-favicon](https://www.npmjs.com/package/serve-favicon): middleware Node.js per servire un favicon.
   - [socket.io](https://www.npmjs.com/package/socket.io): Socket.IO consente comunicazioni bidirezionali basate su eventi in tempo reale.
   - [socket.io-client](https://www.npmjs.com/package/socket.io-client): framework applicazione in tempo reale per socket.io.
   - [superagent](https://www.npmjs.com/package/superagent): superAgent è una piccola libreria di richieste HTTP lato client progressiva e un modulo Node.js con la stessa API, con molte funzioni client HTTP di livello superiore.
   - [swagger-tools](https://www.npmjs.com/package/swagger-tools): pacchetto che fornisce vari strumenti per l'integrazione e l'interazionecon Swagger.
   - [twilio](https://www.npmjs.com/package/twilio): un wrapper per l'API Twilio, relativo a voce, video e messaggi.
   - [underscore](https://www.npmjs.com/package/underscore): underscore.js è una libreria di programmi di utilità per JavaScript che fornisce supporto per i soliti sospetti funzionali (each, map, reduce, filter...) senza estendere gli oggetti JavaScript di base.
   - [url-pattern](https://www.npmjs.com/package/url-pattern): analizza gli URL per i parametri di percorso più facilmente rispetto all'utilizzo di un matcher di stringhe di espressioni regolari.
   - [uuid](https://www.npmjs.com/package/uuid): generazione semplice e veloce di UUID RFC4122.
   - [validator](https://www.npmjs.com/package/validator): una libreria di programmi di convalida e di pulizia di stringhe.
   - [vcap_services](https://www.npmjs.com/package/vcap_services): analizza e restituisce credenziali del servizio dalla variabile di ambiente VCAP_SERVICES fornita da IBM Cloud.
   - [watson-developer-cloud](https://www.npmjs.com/package/watson-developer-cloud): libreria client Node.js per utilizzare i servizi Watson Developer Cloud, una raccolta di API che utilizzano il calcolo cognitivo per risolvere problemi complessi.
   - [when](https://www.npmjs.com/package/when): when.js è un'implementazione solida e collaudata di Promises/A+ e when(), che include uno shim completo di ES6 Promise.
   - [winston](https://www.npmjs.com/package/winston): una libreria di registrazione asincrona multi-trasporto per node.js. "WINSTON è nei log."
   - [ws](https://www.npmjs.com/package/ws): ws è un'implementazione client e server WebSocket semplice da usare, molto veloce e completamente testata.
   - [xml2js](https://www.npmjs.com/package/xml2js): semplice convertitore di oggetti XML in JavaScript. Supporta la conversione bidirezionale.
   - [xmlhttprequest](https://www.npmjs.com/package/xmlhttprequest): node-XMLHttpRequest è un wrapper per il client http incorporato per emulare l'oggetto XMLHttpRequest del browser.
   - [yauzl](https://www.npmjs.com/package/yauzl): un'altra libreria di estrazione per il nodo.

Possono essere trovate delle informazioni dettagliate sull'ambiente di runtime Node.js versione 8 in [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md).


## Runtime Python
{: #openwhisk_ref_python_environments}

Puoi scegliere tra due differenti versioni di runtime per le azioni Python. Per impostazione predefinita, tutte le azioni Python vengono eseguite in un ambiente versione 2.


### Azioni Python 3.7 (basate su Debian Stretch)
{: #openwhisk_ref_python_environments_3.7}

Le azioni Python 3.7 vengono eseguite con Python 3.7.x. Per utilizzare questo runtime, specifica il parametro `--kind python:3.7` della CLI quando crei o aggiorni un'azione.

Il runtime contiene pacchetti SDK per i servizi IBM Cloud che possono essere utilizzati dalle azioni Python, in aggiunta alle librerie standard di Python 3.7.

#### Pacchetti 3.7.2

 - [3.7.2](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile)

 Pacchetti Python:
 - asn1crypto
 - attrs
 - Automat
 - beautifulsoup4
 - botocore
 - cassandra-driver
 - certifi
 - cffi
 - chardet
 - Click
 - cloudant
 - constantly
 - cryptography
 - cssselect
 - docutils
 - elasticsearch
 - etcd3
 - Flask
 - gevent
 - greenlet
 - grpcio
 - httplib2
 - hyperlink
 - ibm-cos-sdk
 - ibm-cos-sdk-core
 - ibm-cos-sdk-s3transfer
 - ibm-db
 - ibmcloudsql
 - idna
 - incremental
 - itsdangerous
 - Jinja2
 - jmespath
 - kafka-python
 - lxml
 - MarkupSafe
 - numpy
 - pandas
 - parsel
 - pika
 - Pillow
 - protobuf
 - psycopg2
 - pyarrow
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pymongo
 - pyOpenSSL
 - python-dateutil
 - pytz
 - queuelib
 - redis
 - requests
 - scikit-learn
 - scipy
 - Scrapy
 - service-identity
 - simplejson
 - six
 - soupsieve
 - tenacity
 - tornado
 - Twisted
 - urllib3
 - virtualenv
 - w3lib
 - watson-developer-cloud
 - websocket-client
 - Werkzeug
 - zope.interface

Possono essere trovate delle informazioni dettagliate sull'ambiente di runtime Python 3.7 in [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md).

### Azioni Python 3.6.8 (basate su Debian Jessie)
{: #openwhisk_ref_python_environments_3.6}

Le azioni Python 3 vengono eseguite con Python 3.6.x. Per utilizzare questo runtime, specifica il parametro `--kind python:3.6` della CLI quando crei o aggiorni un'azione.

Il runtime contiene pacchetti SDK per i servizi IBM Cloud che possono essere utilizzati dalle azioni Python, in aggiunta alle librerie standard di Python 3.6.

#### Pacchetti Python 3.6.8

Versione Python:
 - [3.6.8](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile)

 - Pacchetti Python:
 - asn1crypto
 - attrs
 - autobahn
 - Automat
 - beautifulsoup4
 - botocore
 - cassandra-driver
 - certifi
 - cffi
 - chardet
 - Click
 - cloudant
 - constantly
 - cryptography
 - cssselect
 - docutils
 - elasticsearch
 - Flask
 - gevent
 - greenlet
 - httplib2
 - hyperlink
 - ibm-cos-sdk
 - ibm-cos-sdk-core
 - ibm-cos-sdk-s3transfer
 - ibm-db
 - ibmcloudsql
 - idna
 - incremental
 - itsdangerous
 - Jinja2
 - jmespath
 - kafka-python
 - lxml
 - MarkupSafe
 - numpy
 - pandas
 - parsel
 - pika
 - Pillow
 - psycopg2
 - pyarrow
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pymongo
 - pyOpenSSL
 - python-dateutil
 - pytz
 - queuelib
 - redis
 - requests
 - scikit-learn
 - scipy
 - Scrapy
 - service-identity
 - simplejson
 - six=
 - soupsieve
 - tornado
 - Twisted
 - txaio
 - urllib3
 - virtualenv
 - w3lib
 - watson-developer-cloud
 - Werkzeug
 - zope.interface

Possono essere trovate delle informazioni dettagliate sull'ambiente di runtime Python 3.6 in [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md).

### Azioni Python 2

Le azioni Python 2 vengono eseguite con Python 2.7.15 a meno che non specifichi l'indicatore `--kind` quando crei o aggiorni un'azione.

Quando crei azioni python usando virtualenv, utilizza l'immagine docker `openwhisk/python2action`.
I seguenti pacchetti possono essere utilizzati dalle azioni Python 2, in aggiunta alla libreria standard di Python 2.7.

#### Pacchetti Python 2

 - asn1crypto
 - attrs
 - Automat
 - beautifulsoup4
 - certifi
 - cffi
 - chardet
 - Click
 - constantly
 - cryptography
 - cssselect
 - enum34
 - Flask
 - functools32
 - gevent
 - greenlet
 - httplib2
 - hyperlink
 - idna
 - incremental
 - ipaddress
 - itsdangerous
 - Jinja2
 - kafka-python
 - lxml
 - MarkupSafe
 - parsel
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pyOpenSSL
 - python-dateutil
 - queuelib
 - requests
 - Scrapy
 - service-identity
 - simplejson
 - six
 - Twisted
 - urllib3
 - virtualenv=
 - w3lib
 - Werkzeug
 - zope.interface

Possono essere trovate delle informazioni dettagliate sull'ambiente di runtime Python 2 in [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md).


## Runtime Swift
{: #swift-actions}

Per impostazione predefinita, tutte le azioni Swift vengono eseguite in un ambiente versione 4.2.

I runtime di azione Swift 4.x non incorporano alcun pacchetto, segui le istruzioni per le [azioni swift impacchettate](/docs/openwhisk?topic=cloud-functions-prep#prep_swift42_single) per includere delle dipendenze utilizzando un Package.swift.

Le azioni Swift 4.2 possono utilizzare i seguenti pacchetti quando si utilizza un singolo file di origine Swift:
- SDK Watson Developer Cloud versione 1.2.0, https://github.com/watson-developer-cloud/swift-sdk


### SwiftyJSON che utilizza un singolo file di azioni di origine
Se hai un'azione non compilata e utilizzi il pacchetto **SwiftyJSON**, devi precompilare la tua azione e specificare la versione di SwiftyJSON che vuoi utilizzare per l'azione di tipo `swift:4.2`.


## Runtime PHP
{: #openwhisk_ref_php}

Per impostazione predefinita, tutte le azioni PHP vengono eseguite in un ambiente di versione 7.3.

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

## Runtime Docker
{: #openwhisk_ref_docker}

Le azioni Docker eseguono un file binario fornito dall'utente in un contenitore Docker. Il file binario viene eseguito in un'immagine Docker basata su [python:3.6-alpine](https://hub.docker.com/r/library/python), pertanto il file binario deve essere compatibile con questa distribuzione.

La struttura di base Docker può essere opportunamente utilizzata per creare immagini Docker compatibili con OpenWhisk. Puoi installare la struttura di base con il comando del plug-in della CLI `ibmcloud fn sdk install docker`.

Il programma binario principale deve trovarsi in `/action/exec` all'interno del contenitore. L'eseguibile riceve gli argomenti di input da una singola stringa di argomenti della riga di comando che può essere deserializzata come oggetto `JSON`. Deve restituire un risultato mediante `stdout` sotto forma di stringa a singola riga del `JSON` serializzato.

Puoi includere eventuali passi di compilazione o dipendenze modificando il `Dockerfile` compreso nel `dockerSkeleton`.



## Runtime Go
{: #runtimes_go}

Per impostazione predefinita, tutte le azioni Go vengono eseguite in un ambiente versione 1.11.



## Runtime Java
{: #runtimes_java}

Per impostazione predefinita, tutte le azioni Java vengono eseguite in un ambiente versione 8.



## Runtime Ruby
{: #runtimes_ruby}

Per impostazione predefinita, tutte le azioni Ruby vengono eseguite in un ambiente versione 2.5.



## Runtime Core .NET
{: #runtimes_dotnet}

Per impostazione predefinita, tutte le azioni .NET Core vengono eseguite in un ambiente versione 2.2.
