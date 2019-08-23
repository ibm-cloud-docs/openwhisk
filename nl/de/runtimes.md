---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: runtimes, support, functions

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


# Laufzeiten
{: #runtimes}
Ihre Apps können in Programmiersprachen wie JavaScript oder Python programmiert und ausgeführt werden. Zahlreiche Laufzeiten sind standardmäßig mit {{site.data.keyword.openwhisk_short}} verfügbar.
{: shortdesc}

Sie können die verfügbaren Laufzeiten für IBM Cloud-Funktionen in den einzelnen Regionen anzeigen. Die folgenden Links geben eine JSON-Antwort zurück. Der Abschnitt `runtimes` der Antwort enthält den Satz von verfügbaren Laufzeiten. Der Abschnitt `image` enthält den Namen des Laufzeit-Image unter [Docker Hub](https://hub.docker.com/){: external} und das verwendete Tag.

  - [`us-south`](https://us-south.functions.cloud.ibm.com/){: external}
  - [`us-east`](https://us-east.functions.cloud.ibm.com/){: external}
  - [`eu-gb`](https://eu-gb.functions.cloud.ibm.com/){: external}
  - [`eu-de`](https://eu-de.functions.cloud.ibm.com/){: external}


Die folgenden Beispiele verweisen auf die Images `ibmfunctions/action-nodejs-v10` und `openwhisk/nodejs8action`.
Die Tags können Versionsnummern wie `1.9.0` oder die Kurzform eines Git-Commit-Hashwerts wie `b99d71e` sein.

Beispiele für Imagefelder.
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs8action:b99d71e"
  ```

Laufzeiten werden regelmäßig aktualisiert. Diese Aktualisierungen umfassen Sicherheitskorrekturen und Nebenversionsaktualisierungen für die Pakete in den Laufzeiten. Nebenversionsaktualisierungen können zu Unterbrechungen der Abwärtskompatibilität führen. Laufzeitaktualisierungen können sich negativ auf Ihre Aktionen auswirken. Sie müssen Aktionen, die eine Laufzeit ausführen, auf eine neuere Version migrieren, indem Sie sie ein entsprechendes Update vornehmen.

Apps, die in veralteten Laufzeiten ausgeführt werden, können erst erfolgreich abgeschlossen werden, wenn die Laufzeit auf eine unterstützte Version aktualisiert wird. Suchen Sie im Rahmen der Fehlerbehebung für eine fehlgeschlagene Aktion in der Abfrageantwort nach `deprecated=true`, um zu ermitteln, ob eine Laufzeitumgebung veraltet ist. Informationen zum Aktualisieren der Laufzeit finden Sie unter [Aktionslaufzeit ändern](/docs/openwhisk?topic=cloud-functions-actions#actions_update).

Diese Laufzeiten sind veraltet:
<ul>
  <li><code>nodejs:6</code> (veraltet)</li>
  <li><code>php:7.1</code> (veraltet)</li>
  <li><code>php:7.2</code> (veraltet)</li>
  <li><code>swift:3</code> (veraltet)</li>
  <li><code>swift:3.1.1</code> (veraltet)</li>
  <li><code>swift:4.1</code> (veraltet)</li>
  <li><code>ballerina:0.990</code> (veraltet)</li>
</ul>



## JavaScript-Laufzeiten
{: #openwhisk_ref_javascript_environments}

Standardmäßig werden alle Node.js-Aktionen in einer Umgebung der Version 10 ausgeführt.
{: note}

JavaScript-Aktionen können in Node.js Version 8 oder 10 ausgeführt werden. Node.js Version 8 befindet sich im Wartungsmodus und ist bis Dezember 2019 verfügbar. Weitere Informationen finden Sie im [Node.js-Releaseplan](https://github.com/nodejs/Release){: external}. 
{: deprecated}

| Laufzeit | Beschreibung | Änderungsprotokoll |
| --- | --- | --- |
| [10.15.0](https://nodejs.org/en/blog/release/v10.15.0/){: external} | Standardmäßig werden alle Node.js-Aktionen in einer Umgebung der Version 10 ausgeführt. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md){: external}. |
| [8.15.0](https://nodejs.org/en/blog/release/v8.15.0/){: external} | Die Node.js-Version 8.15.0 wird verwendet, wenn beim Erstellen oder Aktualisieren einer Aktion für das Flag `--kind` explizit der Wert `nodejs:8` angegeben wird. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md){: external}. |


### Von nodeJS 8 auf nodeJS 10 migrieren

| Paket | Details |
| --- | --- |
| `ibm_db` | Das NPM-Paket `ibm_db` ist in `nodejs:10` nicht verfügbar. Das Paket `ibm_db` bietet keine Unterstützung für Node.js 10. Sie können den Fortschritt in [diesem Problem](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541){: external} verfolgen. |
| `cloudant` | Das NPM-Paket `cloudant` ist in `nodejs:10` nicht verfügbar. Das Paket ist veraltet. Sie müssen das offizielle NPM-Paket [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant){: external} V3.0.0 verwenden, wenn Sie das Node.js-Modul importieren (d. h. `require('@cloudant/cloudant')`). Auch [V3.x gibt nur Promises zurück](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x){: external}. |
| `cradle` | Das NPM-Paket `cradle` ist in `nodejs:10` nicht verfügbar. |
| `log4js` | Das NPM-Paket `log4js` ist in `nodejs:10` nicht verfügbar. Sie können [dieses Problem](https://github.com/log4js-node/log4js-node/issues/805){: external} verfolgen. |
| `watson-developer-cloud` | Das NPM-Paket `watson-developer-cloud` ist in `nodejs:10` nicht verfügbar. Sie können den Fortschritt der neuen Version in[diesem Problem](https://github.com/watson-developer-cloud/node-sdk/issues/780){: external} verfolgen. |


### Node.js-Pakete

| Node.js 10.15-Pakete | Beschreibung |
|:-----------------|:-----------------|
| [`amqplib`](https://www.npmjs.com/package/amqplib){: external} | Eine Bibliothek zum Erstellen von Advanced Message Queuing Protocol 0-9-1-Clients für Node.JS. |
| [`apn`](https://www.npmjs.com/package/apn){: external} | Ein Node.js-Modul, das als Schnittstelle zum Apple Push Notification-Service fungiert. |
| [`async`](https://www.npmjs.com/package/async){: external} | Stellt Funktionen für die Arbeit mit asynchronen Funktionen bereit. |
| [`bent`](https://www.npmjs.com/package/bent){: external} | Funktionaler HTTP-Client für Node.js mit 'async' und 'await'. |
| [`bodyparser`](https://www.npmjs.com/package/body-parser){: external} | Parst eingehende Anforderungshauptteile in einer Middleware vor Ihren Handlern, verfügbar unter der Eigenschaft 'req.body'. |
| [`btoa`](https://www.npmjs.com/package/btoa){: external} | Ein Port der Funktion `btoa` des Browsers. |
| [`cassandra-driver`](https://www.npmjs.com/package/cassandra-driver){: external} | DataStax Node.js-Treiber für Apache Cassandra. |
| [`@cloudant/cloudant`](https://www.npmjs.com/package/@cloudant/cloudant){: external} | Die offizielle Cloudant-Bibliothek für Node.js. |
| [`commander`](https://www.npmjs.com/package/commander){: external} | Die vollständige Lösung für Node.js-Befehlszeilenschnittstellen. |
| [`composeaddresstranslator`](https://www.npmjs.com/package/composeaddresstranslator){: external} | Umsetzer für Adressen aus der Compose-Benutzerschnittstelle oder -API für Scylla-Datenbanken. |
| [`consul`](https://www.npmjs.com/package/consul){: external} | Ein Client für Consul, der Serviceerkennung und Konfiguration beinhaltet. |
| [`cookie-parser`](https://www.npmjs.com/package/cookie-parser){: external} | Parst Cookie-Header und füllt 'req.cookies' mit einem Objekt, dessen Schlüssel durch die Cookienamen bestimmt werden. |
| [`elasticsearch`](https://www.npmjs.com/package/elasticsearch){: external} | Der offizielle Elasticsearch-Low-Level-Client für Node.js. |
| [`errorhandler`](https://www.npmjs.com/package/errorhandler){: external} | Middleware für Fehlerbehandlung ausschließlich während der Entwicklung. |
| [`etcd3`](https://www.npmjs.com/package/etcd3){: external} | Qualitativ hochwertiger, einsatzbereiter Client für die Protocol Buffer-basierte etcdv3-API. |
| [`formidable`](https://www.npmjs.com/package/formidable){: external} | Node.js-Modul für das Parsen von Formulardaten, besonders Dateiuploads. |
| [`glob`](https://www.npmjs.com/package/glob){: external} | Abgleich von Dateien mithilfe von durch die Shell verwendeten Mustern wie Sternen u. a. |
| [`gm`](https://www.npmjs.com/package/gm){: external} | GraphicsMagick und ImageMagick für Node. |
| [`ibm-cos-sdk`](https://www.npmjs.com/package/ibm-cos-sdk){: external} | {{site.data.keyword.cos_full}}-SDK für Node.js |
| [`ibm_db`](https://www.npmjs.com/package/ibm_db){: external} | Eine asynchrone/synchrone Schnittstelle für Node.js zu IBM Db2 und IBM Informix. |
| [`ibmiotf`](https://www.npmjs.com/package/ibmiotf){: external} | Der Node.js-Client, der für die Vereinfachung der Interaktion mit der IBM Watson Internet of Things-Plattform verwendet wird. |
| [`iconv-lite`](https://www.npmjs.com/package/iconv-lite){: external} | Umsetzung der Pure JS-Zeichencodierung. |
| [`jsdom`](https://www.npmjs.com/package/jsdom){: external} | `jsdom` ist eine Pure JavaScript-Implementierung zahlreicher Webstandards, insbesondere der DOM- und HTML-Standards der Web Hypertext Application Technology Working Group. |
| [`jsforce`](https://www.npmjs.com/package/jsforce){: external} | Salesforce-API-Bibliothek für JavaScript-Anwendungen. |
| [`jsonwebtoken`](https://www.npmjs.com/package/jsonwebtoken){: external} | Eine Implementierung von JSON-Web-Tokens. |
| [`lodash`](https://www.npmjs.com/package/lodash){: external} | Die Bibliothek `lodash` wird in Form von Node.js-Modulen exportiert. |
| [`marked`](https://www.npmjs.com/package/marked){: external} | Ein voll ausgestatteter Markdown-Parser und -Compiler, der in JavaScript geschrieben ist. Auf Geschwindigkeit ausgerichtet. |
| [`merge`](https://www.npmjs.com/package/merge){: external} | Mehrere Objekte zusammenführen und optional ein neues geklontes Objekt erstellen. | [`moment`](https://www.npmjs.com/package/moment){: external} | Eine einfache JavaScript-Datumsbibliothek zum Parsen, Validieren, Bearbeiten und Formatieren von Datumswerten. |
| [`mongodb`](https://www.npmjs.com/package/mongodb){: external} | Der offizielle MongoDB-Treiber für Node.js. |
| [`mysql`](https://www.npmjs.com/package/mysql){: external} | Ein Node.js-Treiber für MySQL. |
| [`mustache`](https://www.npmjs.com/package/mustache){: external} | Mustache.js ist eine Implementierung des Mustache-Vorlagensystems in JavaScript. |
| [`nano`](https://www.npmjs.com/package/nano){: external} | Minimalistischer CouchDB-Treiber für Node.js. |
| [`nodemailer`](https://www.npmjs.com/package/nodemailer){: external} | Einfaches Versenden von E-Mails von Node.js. |
| [`oauth2-server`](https://www.npmjs.com/package/oauth2-server){: external} | Vollständiges, kompatibles und getestetes Modul zur Implementierung eines OAuth2-Servers/-Providers mit Express in Node.js. |
| [`openwhisk`](https://www.npmjs.com/package/openwhisk){: external} | JavaScript-Clientbibliothek für die OpenWhisk-Plattform. Stellt einen Wrapper um die OpenWhisk-APIs herum bereit. |
| [`path-to-regex`](https://www.npmjs.com/package/path-to-regexp){: external} | Umwandeln einer Pfadzeichenfolge wie `/user/:name` in einen regulären Ausdruck, der dann für den Abgleich mit URL-Pfaden verwendet werden kann. |
| [`pg`](https://www.npmjs.com/package/pg){: external} | Nicht blockierender PostgreSQL-Client für Node.js. Pure JavaScript und optionale native `libpq`-Bindungen. |
| [`process`](https://www.npmjs.com/package/process){: external} | `require('process')`; wie jedes andere Modul. |
| [`pug`](https://www.npmjs.com/package/pug){: external} | Implementiert die Vorlagensprache Pug. |
| [`redis`](https://www.npmjs.com/package/redis){: external} | Ein vollständiger und umfassend ausgestatteter Redis-Client für Node.js. |
| [`request`](https://www.npmjs.com/package/request){: external} | Ausführen von HTTP-Aufrufen. |
| [`request-promise`](https://www.npmjs.com/package/request-promise){: external} | Der vereinfachte HTTP-Anforderungsclient 'request' mit Promise-Unterstützung. Powered by Bluebird. |
| [`rimraf`](https://www.npmjs.com/package/rimraf){: external} | Der UNIX-Befehl 'rm -rf' für Node. |
| [`semver`](https://www.npmjs.com/package/semver){: external} | Semantische Versionierung für nodeJS. |
| [`@sendgrid/mail`](https://www.npmjs.com/package/@sendgrid/mail){: external} | Stellt E-Mail-Unterstützung über die SendGrid-API bereit. |
| [`serialize-error`](https://www.npmjs.com/package/serialize-error){: external} | Serialisieren eines Fehlers in ein einfaches Objekt. |
| [`serve-favicon`](https://www.npmjs.com/package/serve-favicon){: external} | Node.js-Middleware für die Bereitstellung eines Favicons. | [`socket.io`](https://www.npmjs.com/package/socket.io){: external} | `socket.io` ermöglicht eine bidirektionale, ereignisgesteuerte Echtzeitkommunikation. |
| [`socket.io-client`](https://www.npmjs.com/package/socket.io-client){: external} | Echtzeitanwendungsframework für `socket.io`. |
| [`superagent`](https://www.npmjs.com/package/superagent){: external} | `superagent` ist eine kleine, progressive, clientseitige HTTP-Anforderungsbibliothek und ein Node.js-Modul mit derselben API. Bietet zahlreiche allgemeine HTTP-Clientfunktionen. |
| [`swagger-tools`](https://www.npmjs.com/package/swagger-tools){: external} | Paket, das verschiedene Tools für die Integration und Interaktion mit Swagger bereitstellt. |
| [`twilio`](https://www.npmjs.com/package/twilio){: external} | Ein Wrapper für die Twilio-API für Sprache, Video und Messaging. |
| [`underscore`](https://www.npmjs.com/package/underscore){: external} | Underscore.js ist eine Dienstprogrammbibliothek für JavaScript, die die üblichen Funktionen (each, map, reduce, filter...) ohne Erweiterung von JavaScript-Kernobjekten unterstützt. |
| [`url-pattern`](https://www.npmjs.com/package/url-pattern){: external} | Parst URLs für Pfadparameter auf einfachere Weise als mit einem Regex-String-Matcher. |
| [`uuid`](https://www.npmjs.com/package/uuid){: external} | Einfache, schnelle Generierung von RFC4122-UUIDs. |
| [`validator`](https://www.npmjs.com/package/validator){: external} | Eine Bibliothek von Validatoren und Korrekturfunktionen für Zeichenfolgen. |
| [`vcap_services`](https://www.npmjs.com/package/vcap_services){: external} | Parsing und Rückgabe von Serviceberechtigungsnachweisen von der Umgebungsvariablen VCAP_SERVICES, die IBM Cloud bereitstellt. |
| [`when`](https://www.npmjs.com/package/when){: external} | When.js ist eine sehr solide, bewährte Implementierung von `Promises/A+` und `when()`, einschließlich eines vollständigen ES6-Promise-Shims. |
| [`winston`](https://www.npmjs.com/package/winston){: external} | Eine Multitransport-Bibliothek für asynchrone Protokollierung für Node.js. "Nur die Ruhe, Winston! ... Ich hab die Protokolle eingefügt." |
| [`ws`](https://www.npmjs.com/package/ws){: external} | `ws` ist eine bedienerfreundliche, äußerst schnelle und gründlich getestete WebSocket-Client/Server-Implementierung. |
| [`xlsx`](https://www.npmjs.com/package/xlsx){: external} | Parser und Writer für verschiedene Tabellenformate. |
| [`xml2js`](https://www.npmjs.com/package/xml2js){: external} | Einfacher Konverter von XML in JavaScript-Objekte. Unterstützt die bidirektionale Konvertierung. |
| [`xmlhttprequest`](https://www.npmjs.com/package/xmlhttprequest){: external} | node-XMLHttpRequest ist ein Wrapper für den integrierten HTTP-Client zum Emulieren des XMLHttpRequest-Browserobjekts. |
| [`yauzl`](https://www.npmjs.com/package/yauzl){: external} | Eine weitere Extraktionsbibliothek für Node. |
{: caption="Tabelle 1. Node.js 10.15-Pakete." caption-side="top"}
{: #javascript-1}
{: tab-title="Node.js 10.15 packages"}
{: tab-group="node"}
{: class="simple-tab-table"}

| Node.js 8.15-Pakete | Beschreibung |
|:-----------------|:-----------------|
| [`amqplib`](https://www.npmjs.com/package/amqplib){: external} | Eine Bibliothek zum Erstellen von Advanced Message Queuing Protocol 0-9-1-Clients für Node.JS. |
| [`apn`](https://www.npmjs.com/package/apn){: external} | Ein Node.js-Modul, das als Schnittstelle zum Apple Push Notification-Service fungiert. | [`async`](https://www.npmjs.com/package/async){: external} | Stellt Funktionen für die Arbeit mit asynchronen Funktionen bereit. |
| [`bent`](https://www.npmjs.com/package/bent){: external} | Funktionaler HTTP-Client für Node.js mit 'async-await'. |
| [`bodyparser`](https://www.npmjs.com/package/body-parser){: external} | Parst eingehende Anforderungshauptteile in einer Middleware vor Ihren Handlern, verfügbar unter der Eigenschaft 'req.body'. |
| [`btoa`](https://www.npmjs.com/package/btoa){: external} | Ein Port der Funktion `btoa` des Browsers. |
| [`cassandra-driver`](https://www.npmjs.com/package/cassandra-driver){: external} | DataStax Node.js-Treiber für Apache Cassandra. |
| [`cloudant`](https://www.npmjs.com/package/cloudant){: external} | Die offizielle Cloudant-Bibliothek für Node.js. |
| [`@cloudant/cloudant`](https://www.npmjs.com/package/cloudant){: external} | Die offizielle Cloudant-Bibliothek für Node.js. |
| [`commander`](https://www.npmjs.com/package/commander){: external} | Die vollständige Lösung für Node.js-Befehlszeilenschnittstellen. |
| [`composeaddresstranslator`](https://www.npmjs.com/package/composeaddresstranslator){: external} | Umsetzer für Adressen aus der Compose-Benutzerschnittstelle oder -API für Scylla-Datenbanken. |
| [`consul`](https://www.npmjs.com/package/consul){: external} | Ein Client für Consul, der Serviceerkennung und Konfiguration beinhaltet. |
| [`cookie-parser`](https://www.npmjs.com/package/cookie-parser){: external} | Parst Cookie-Header und füllt 'req.cookies' mit einem Objekt, dessen Schlüssel durch die Cookienamen bestimmt werden. |
| [`elasticsearch`](https://www.npmjs.com/package/elasticsearch){: external} | Der offizielle Elasticsearch-Low-Level-Client für Node.js. |
| [`errorhandler`](https://www.npmjs.com/package/errorhandler){: external} | Middleware für Fehlerbehandlung ausschließlich während der Entwicklung. |
| [`etcd3`](https://www.npmjs.com/package/etcd3){: external} | Qualitativ hochwertiger, einsatzbereiter Client für die Protocol Buffer-basierte etcdv3-API. |
| [`formidable`](https://www.npmjs.com/package/formidable){: external} | Node.js-Modul für das Parsen von Formulardaten, besonders Dateiuploads. |
| [`glob`](https://www.npmjs.com/package/glob){: external} | Abgleich von Dateien mithilfe von durch die Shell verwendeten Mustern wie Sternen u. a. |
| [`gm`](https://www.npmjs.com/package/gm){: external} | GraphicsMagick und ImageMagick für Node. |
| [`ibm-cos-sdk`](https://www.npmjs.com/package/ibm-cos-sdk){: external} | {{site.data.keyword.cos_full}}-SDK für Node.js. |
| [`ibm_db`](https://www.npmjs.com/package/ibm_db){: external} | Eine asynchrone/synchrone Schnittstelle für Node.js zu IBM Db2 und IBM Informix. |
| [`ibmiotf`](https://www.npmjs.com/package/ibmiotf){: external} | Der Node.js-Client, der für die Vereinfachung der Interaktion mit der IBM Watson Internet of Things-Plattform verwendet wird. |
| [`iconv-lite`](https://www.npmjs.com/package/iconv-lite){: external} | Umsetzung der Pure JS-Zeichencodierung. |
| [`jsdom`](https://www.npmjs.com/package/jsdom){: external} | `jsdom` ist eine Pure JavaScript-Implementierung zahlreicher Webstandards, insbesondere der DOM- und HTML-Standards der Web Hypertext Application Technology Working Group. |
| [`jsforce`](https://www.npmjs.com/package/jsforce){: external} | Salesforce-API-Bibliothek für JavaScript-Anwendungen. |
| [`jsonwebtoken`](https://www.npmjs.com/package/jsonwebtoken){: external} | Eine Implementierung von JSON-Web-Tokens. |
| [`lodash`](https://www.npmjs.com/package/lodash){: external} | Die Bibliothek `lodash`, die in Form von Node.js-Modulen exportiert wird. |
| [`log4js`](https://www.npmjs.com/package/log4js){: external} | Eine Konvertierung des log4js-Frameworks für den Einsatz mit Node. |
| [`marked`](https://www.npmjs.com/package/marked){: external} | Ein voll ausgestatteter Markdown-Parser und -Compiler, der in JavaScript geschrieben ist. Auf Geschwindigkeit ausgerichtet. |
| [`merge`](https://www.npmjs.com/package/merge){: external} | Mehrere Objekte zusammenführen und optional ein neues geklontes Objekt erstellen. |
| [`moment`](https://www.npmjs.com/package/moment){: external} | Eine einfache JavaScript-Datumsbibliothek zum Parsen, Validieren, Bearbeiten und Formatieren von Datumswerten. |
| [`mongodb`](https://www.npmjs.com/package/mongodb){: external} | Der offizielle MongoDB-Treiber für Node.js. |
| [`mysql`](https://www.npmjs.com/package/mysql){: external} | Ein Node.js-Treiber für MySQL. |
| [`mustache`](https://www.npmjs.com/package/mustache){: external} | mustache.js ist eine Implementierung des mustache-Vorlagensystems in JavaScript. |
| [`nano`](https://www.npmjs.com/package/nano){: external} | Minimalistischer CouchDB-Treiber für Node.js. |
| [`nodemailer`](https://www.npmjs.com/package/nodemailer){: external} | Einfaches Versenden von E-Mails von Node.js. |
| [`oauth2-server`](https://www.npmjs.com/package/oauth2-server){: external} | Vollständiges, kompatibles und getestetes Modul zur Implementierung eines OAuth2-Servers/-Providers mit Express in Node.js. |
| [`openwhisk`](https://www.npmjs.com/package/openwhisk){: external} | JavaScript-Clientbibliothek für die OpenWhisk-Plattform. Stellt einen Wrapper um die OpenWhisk-APIs herum bereit. |
| [`path-to-regex`](https://www.npmjs.com/package/path-to-regexp){: external} | Umwandeln einer Pfadzeichenfolge wie `/user/:name` in einen regulären Ausdruck, der dann für den Abgleich mit URL-Pfaden verwendet werden kann. | [`pg`](https://www.npmjs.com/package/pg){: external} | Nicht blockierender PostgreSQL-Client für Node.js. Pure JavaScript und optionale native `libpq`-Bindungen. |
| [`process`](https://www.npmjs.com/package/process){: external} | `require('process')`; wie jedes andere Modul. |
| [`pug`](https://www.npmjs.com/package/pug){: external} | Implementiert die Vorlagensprache Pug. |
| [`redis`](https://www.npmjs.com/package/redis){: external} | Ein vollständiger und umfassend ausgestatteter Redis-Client für Node.js. |
| [`request`](https://www.npmjs.com/package/request){: external} | Ausführen von HTTP-Aufrufen. |
| [`request-promise`](https://www.npmjs.com/package/request-promise){: external} | Der vereinfachte HTTP-Anforderungsclient 'request' mit Promise-Unterstützung. Powered by Bluebird. |
| [`rimraf`](https://www.npmjs.com/package/rimraf){: external} | Der UNIX-Befehl 'rm -rf' für Node. |
| [`semver`](https://www.npmjs.com/package/semver){: external} | Semantische Versionierung für nodeJS. |
| [`@sendgrid/mail`](https://www.npmjs.com/package/@sendgrid/mail){: external} | Stellt E-Mail-Unterstützung über die SendGrid-API bereit. |
| [`serialize-error`](https://www.npmjs.com/package/serialize-error){: external} | Serialisieren eines Fehlers in ein einfaches Objekt. |
| [`serve-favicon`](https://www.npmjs.com/package/serve-favicon){: external} | Node.js-Middleware für die Bereitstellung eines Favicons. |
| [`socket.io`](https://www.npmjs.com/package/socket.io){: external} | `socket.io` ermöglicht eine bidirektionale, ereignisgesteuerte Echtzeitkommunikation. |
| [`socket.io-client`](https://www.npmjs.com/package/socket.io-client){: external} | Echtzeitanwendungsframework für `socket.io`. |
| [`superagent`](https://www.npmjs.com/package/superagent){: external} | `superagent` ist eine kleine, progressive, clientseitige HTTP-Anforderungsbibliothek und ein Node.js-Modul mit derselben API. Bietet zahlreiche allgemeine HTTP-Clientfunktionen. |
| [`swagger-tools`](https://www.npmjs.com/package/swagger-tools){: external} | Paket, das verschiedene Tools für die Integration und Interaktion mit Swagger bereitstellt. |
| [`twilio`](https://www.npmjs.com/package/twilio){: external} | Ein Wrapper für die Twilio-API für Sprache, Video und Messaging. |
| [`underscore`](https://www.npmjs.com/package/underscore){: external} | Underscore.js ist eine Dienstprogrammbibliothek für JavaScript, die die üblichen Funktionen (each, map, reduce, filter...) ohne Erweiterung von JavaScript-Kernobjekten unterstützt. |
| [`url-pattern`](https://www.npmjs.com/package/url-pattern){: external} | Parst URLs für Pfadparameter auf einfachere Weise als mit einem Regex-String-Matcher. |
| [`uuid`](https://www.npmjs.com/package/uuid){: external} | Einfache, schnelle Generierung von RFC4122-UUIDs. |
| [`validator`](https://www.npmjs.com/package/validator){: external} | Eine Bibliothek von Validatoren und Korrekturfunktionen für Zeichenfolgen. |
| [`vcap_services`](https://www.npmjs.com/package/vcap_services){: external} | Parsing und Rückgabe von Serviceberechtigungsnachweisen von der Umgebungsvariablen VCAP_SERVICES, die IBM Cloud bereitstellt. |
| [`watson-developer-cloud`](https://www.npmjs.com/package/watson-developer-cloud){: external} | Node.js-Clientbibliothek zur Verwendung der Watson Developer Cloud-Services, einer Sammlung von APIs, die Cognitive Computing zum Lösen komplexer Probleme verwenden. |
| [`when`](https://www.npmjs.com/package/when){: external} | When.js ist eine sehr solide, bewährte Implementierung von `Promises/A+` und `when()`, einschließlich eines vollständigen ES6-Promise-Shims. |
| [`winston`](https://www.npmjs.com/package/winston){: external} | Eine Multitransport-Bibliothek für asynchrone Protokollierung für Node.js. "Nur die Ruhe, Winston! ... Ich hab die Protokolle eingefügt." |
| [`ws`](https://www.npmjs.com/package/ws){: external} | `ws` ist eine bedienerfreundliche, äußerst schnelle und gründlich getestete WebSocket-Client/Server-Implementierung. |
| [`xml2js`](https://www.npmjs.com/package/xml2js){: external} | Einfacher Konverter von XML in JavaScript-Objekte. Unterstützt die bidirektionale Konvertierung. |
| [`xmlhttprequest`](https://www.npmjs.com/package/xmlhttprequest){: external} | `node-XMLHttpRequest` ist ein Wrapper für den integrierten HTTP-Client zum Emulieren des XMLHttpRequest-Browserobjekts. |
| [`yauzl`](https://www.npmjs.com/package/yauzl){: external} | Eine weitere Extraktionsbibliothek für Node. |
{: caption="Tabelle 2. Node.js 8.15-Pakete." caption-side="top"}
{: #javascript-2}
{: tab-title="Node.js 8.15 packages"}
{: tab-group="node"}
{: class="simple-tab-table"}


## Python-Laufzeiten
{: #openwhisk_ref_python_environments}

Standardmäßig werden alle Python-Aktionen in einer Umgebung der Version 2.7.15 ausgeführt.
{: note}

| Python-Version | Beschreibung | Änderungsprotokoll |
| --- | --- | --- |
| 2.7.15 | Standardmäßig werden alle Python-Aktionen in einer Umgebung der Version 2.7.15 ausgeführt, sofern Sie nicht das Flag `--kind` beim Erstellen oder Aktualisieren einer Aktion angeben. Wenn Sie Python-Aktionen mit `virtualenv` erstellen, verwenden Sie das Docker-Image `openwhisk/python2action`. Die folgenden Pakete sind neben der Standardbibliothek von Python 2.7 zur Verwendung durch Python 2-Aktionen verfügbar: | [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md){: external}. |
| [3.6.8](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile){: external} | Python 3-Aktionen werden mit Python 3.6.x ausgeführt. Zur Verwendung dieser Laufzeit geben Sie den CLI-Parameter `--kind python:3.6` an, wenn Sie eine Aktion erstellen oder aktualisieren. Die Laufzeit enthält neben den Standardbibliotheken von Python 3.6 auch SDK-Pakete für IBM Cloud-Services, die zur Verwendung durch Python-Aktionen verfügbar sind. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md){: external}. |
| [3.7.2](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile){: external} | Python 3.7-Aktionen (auf Basis von Debian Stretch) werden mit Python 3.7.x ausgeführt. Zur Verwendung dieser Laufzeit geben Sie den CLI-Parameter `--kind python:3.7` an, wenn Sie eine Aktion erstellen oder aktualisieren. Die Laufzeit enthält neben den Standardbibliotheken von Python 3.7 auch SDK-Pakete für IBM Cloud-Services, die zur Verwendung durch Python-Aktionen verfügbar sind. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md){: external}. |


### Python-Pakete

| Python 2.7.15-Pakete | 
|:-----------------|
| `asn1crypto` |
| `attrs` |
| `Automat` |
| `beautifulsoup4` |
| `certifi` |
| `cffi` |
| `chardet` |
| `Click` |
| `constantly` |
| `cryptography` |
| `cssselect` |
| `enum34` |
| `Flask` |
| `functools32` |
| `gevent` |
| `greenlet` |
| `httplib2` |
| `hyperlink` |
| `idna` |
| `incremental` |
| `ipaddress` |
| `itsdangerous` |
| `Jinja2` |
| `kafka-python` |
| `lxml` |
| `MarkupSafe` |
| `parsel` |
| `pyasn1` |
| `pyasn1-modules` |
| `pycparser` |
| `PyDispatcher` |
| `PyHamcrest` |
| `pyOpenSSL` |
| `python-dateutil` |
| `queuelib` |
| `requests` |
| `Scrapy` |
| `service-identity` |
| `simplejson` |
| `six` |
| `Twisted` |
| `urllib3` |
| `virtualenv=` |
| `w3lib` |
| `Werkzeug` |
| `zope.interface` |
{: caption="Tabelle 1. Python 2.7.15-Pakete." caption-side="top"}
{: #python-1}
{: tab-title="Python 2.7.15 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}

| Python 3.6.8-Pakete | 
|:-----------------|
| `asn1crypto` |
| `attrs` |
| `autobahn` |
| `Automat` |
| `beautifulsoup4` |
| `botocore` |
| `cassandra-driver` |
| `certifi` |
| `cffi` |
| `chardet` |
| `Click` |
| `cloudant` |
| `constantly` |
| `cryptography` |
| `cssselect` |
| `docutils` |
| `elasticsearch` |
| `Flask` |
| `gevent` |
| `greenlet` |
| `httplib2` |
| `hyperlink` |
| `ibm-cos-sdk` |
| `ibm-cos-sdk-core` |
| `ibm-cos-sdk-s3transfer` |
| `ibm-db` |
| `ibmcloudsql` |
| `idna` |
| `incremental` |
| `itsdangerous` |
| `Jinja2` |
| `jmespath` |
| `kafka-python` |
| `lxml` |
| `MarkupSafe` |
| `numpy` |
| `pandas` |
| `parsel` |
| `pika` |
| `Pillow` |
| `psycopg2` |
| `pyarrow` |
| `pyasn1` |
| `pyasn1-modules` |
| `pycparser` |
| `PyDispatcher` |
| `PyHamcrest` |
| `pymongo` |
| `pyOpenSSL` |
| `python-dateutil` |
| `pytz` |
| `queuelib` |
| `redis` |
| `requests` |
| `scikit-learn` |
| `scipy` |
| `Scrapy` |
| `service-identity` |
| `simplejson` |
| `six=` |
| `soupsieve` |
| `tornado` |
| `Twisted` |
| `txaio` |
| `urllib3` |
| `virtualenv` |
| `w3lib` |
| `watson-developer-cloud` |
| `Werkzeug` |
| `zope.interface` |
{: caption="Tabelle 2. Python 3.6.8-Pakete." caption-side="top"}
{: #python-2}
{: tab-title="Python 3.6.8 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}

| Python 3.7.2-Pakete | 
|:-----------------|
| `asn1crypto` |
| `attrs` |
| `Automat` |
| `beautifulsoup4` |
| `botocore` |
| `cassandra-driver` |
| `certifi` |
| `cffi` |
| `chardet` |
| `Click` |
| `cloudant` |
| `constantly` |
| `cryptography` |
| `cssselect` |
| `docutils` |
| `elasticsearch` |
| `etcd3` |
| `Flask` |
| `gevent` |
| `greenlet` |
| `grpcio` |
| `httplib2` |
| `hyperlink` |
| `ibm-cos-sdk` |
| `ibm-cos-sdk-core` |
| `ibm-cos-sdk-s3transfer` |
| `ibm-db` |
| `ibmcloudsql` |
| `idna` |
| `incremental` |
| `itsdangerous` |
| `Jinja2` |
| `jmespath` |
| `kafka-python` |
| `lxml` |
| `MarkupSafe` |
| `numpy` |
| `pandas` |
| `parsel` |
| `pika` |
| `Pillow` |
| `protobuf` |
| `psycopg2` |
| `pyarrow` |
| `pyasn1` |
| `pyasn1-modules` |
| `pycparser` |
| `PyDispatcher` |
| `PyHamcrest` |
| `pymongo` |
| `pyOpenSSL` |
| `python-dateutil` |
| `pytz` |
| `queuelib` |
| `redis` |
| `requests` |
| `scikit-learn` |
| `scipy` |
| `Scrapy` |
| `service-identity` |
| `simplejson` |
| `six` |
| `soupsieve` |
| `tenacity` |
| `tornado` |
| `Twisted` |
| `urllib3` |
| `virtualenv` |
| `w3lib` |
| `watson-developer-cloud` |
| `websocket-client` |
| `Werkzeug` |
| `zope.interface` |
{: caption="Tabelle 3. Python 3.7.2-Pakete." caption-side="top"}
{: #python-3}
{: tab-title="Python 3.7.2 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}


## Swift-Laufzeit
{: #swift-actions}

Standardmäßig werden alle Swift-Aktionen in einer Umgebung der Version 4.2 ausgeführt.
{: note}

Swift 4.x-Aktionslaufzeiten betten keine Pakete ein. Befolgen Sie die Anweisungen für [gepackte Swift-Aktionen](/docs/openwhisk?topic=cloud-functions-prep#prep_swift42_single), um Abhängigkeiten mit einer Datei 'Package.swift' hinzuzufügen. 

Swift 4.2-Aktionen können die folgenden Pakete verwenden, wenn Sie eine einzelne Swift-Quellendatei verwenden: 
- Watson Developer Cloud SDK Version 1.2.0, https://github.com/watson-developer-cloud/swift-sdk


### <ph class="ignoreSpelling">SwiftyJSON</ph> mit einer einzelnen Quellenaktionsdatei
Bei einer Aktion, die nicht kompiliert ist und das Paket `SwiftyJSON` verwendet, müssen Sie Ihre Aktion vorkompilieren und die Version von `SwiftyJSON` angeben, die Sie für die Aktion des Typs `swift:4.2` verwenden möchten. 


## PHP-Laufzeit
{: #openwhisk_ref_php}

Standardmäßig werden alle PHP-Aktionen in einer Umgebung der Version 7.3 ausgeführt.
{: note}

Die folgenden PHP-Erweiterungen sind zusätzlich zu den Standarderweiterungen verfügbar:

- `bcmath`
- `curl`
- `gd`
- `intl`
- `mbstring`
- `mysqli`
- `pdo_mysql`
- `pdo_pgsql`
- `pdo_sqlite`
- `soap`
- `zip`

## Docker-Laufzeit
{: #openwhisk_ref_docker}

Docker-Aktionen werden in einer vom Benutzer bereitgestellten ausführbaren Datei in einem Docker-Container ausgeführt. Die ausführbare Datei wird in einem Docker-Image auf Basis von [python:3.6-alpine](https://hub.docker.com/_/python){: external} ausgeführt. Daher muss die ausführbare Datei mit dieser Distribution kompatibel sein. 

Das Docker-Gerüst (Skeleton) ist eine bequeme Methode, OpenWhisk-kompatible Docker-Images zu erstellen. Sie können das Gerüst mit dem CLI-Plug-in-Befehl `ibmcloud fn sdk install docker` installieren.

Das ausführbare Hauptprogramm muss sich in `/action/exec` innerhalb des Containers befinden. Die ausführbare Datei empfängt die Eingabeargumente von einer einzelnen Befehlszeilenargumentzeichenfolge, die als `JSON`-Objekt deserialisiert werden kann. Es muss ein Ergebnis über `stdout` in Form einer einzeiligen Zeichenfolge aus serialisierten `JSON`-Daten zurückgeben.

Sie können darüber hinaus auch Kompilierungsschritte oder Abhängigkeiten einbeziehen, indem Sie die `Dockerfile` ändern, die in `dockerSkeleton` enthalten ist.

## Weitere Laufzeitunterstützung

| Laufzeit |
| --- | --- | 
| Standardmäßig werden alle Go-Aktionen in einer Umgebung der Version 1.11 ausgeführt. |
{: caption="Tabelle 1. Go." caption-side="top"}
{: #runtimes-1}
{: tab-title="Go"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

| Laufzeit |
| --- | --- | 
| Standardmäßig werden alle Java-Aktionen in einer Umgebung der Version 8 ausgeführt. |
{: caption="Tabelle 2. Java." caption-side="top"}
{: #runtimes-2}
{: tab-title="Java"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

| Laufzeit |
| --- |
| Standardmäßig werden alle Ruby-Aktionen in einer Umgebung der Version 2.5 ausgeführt. |
{: caption="Tabelle 3. Ruby." caption-side="top"}
{: #runtimes-3}
{: tab-title="Ruby"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

| Laufzeit |
| --- |
| Standardmäßig werden alle .NET Core-Aktionen in einer Umgebung der Version 2.2 ausgeführt. |
{: caption="Tabelle 4. .NET Core." caption-side="top"}
{: #runtimes-4}
{: tab-title=".NET"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}




