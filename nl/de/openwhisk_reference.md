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

# Systemdetails und Begrenzungen
{: #openwhisk_reference}

Die folgenden Abschnitte enthalten technische Details zum {{site.data.keyword.openwhisk}}-System und Informationen zu Begrenzungen.
{: shortdesc}

## {{site.data.keyword.openwhisk_short}}-Entitäten
{: #openwhisk_entities}

### Namensbereiche und Pakete
{: #openwhisk_entities_namespaces}

Aktionen, Auslöser (triggers) und Regeln von {{site.data.keyword.openwhisk_short}} gehören in einen Namensbereich und manchmal in ein Paket.

Pakete können Aktionen und Feeds enthalten. Ein Paket kann kein anderes Paket enthalten, sodass eine Paketverschachtelung nicht zulässig ist. Darüber hinaus müssen Entitäten nicht in einem Paket enthalten sein.

In {{site.data.keyword.Bluemix_notm}} entspricht ein Paar aus Organisation und Bereich einem {{site.data.keyword.openwhisk_short}}-Namensbereich. Zum Beispiel entsprechen die Organisation `BobsOrg` und der Bereich `dev` dem {{site.data.keyword.openwhisk_short}}-Namensbereich `/BobsOrg_dev`.

Sie können eigene Namensbereiche erstellen, wenn Sie dazu berechtigt sind. Der Namensbereich `/whisk.system` ist für Entitäten reserviert, die mit dem {{site.data.keyword.openwhisk_short}}-System verteilt werden.

### Vollständig qualifizierte Namen
{: #openwhisk_entities_fullyqual}

Der vollständig qualifizierte Name einer Entität sieht wie folgt aus: `/Namensbereichsname[/Paketname]/Entitätsname`. Beachten Sie, dass das Zeichen `/` zum Abgrenzen von Namensbereichen, Paketen und Entitäten verwendet wird. Darüber hinaus muss Namensbereichen das Zeichen `/` als Präfix vorangestellt werden.

Aus Gründen des Komforts kann der Namensbereich weggelassen werden, wenn es sich um den *Standardnamensbereich* des Benutzers handelt.

Betrachten Sie zum Beispiel einen Benutzer mit dem Standardnamensbereich `/myOrg`. Die folgenden Beispiele zeigen die vollständig qualifizierten Namen einer Reihe von Entitäten und ihre Aliasnamen.

| Vollständig qualifizierter Name | Alias | Namensbereich | Paket | Name |
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read` |  | `/whisk.system` | `cloudant` | `read` |
| `/myOrg/video/transcode` | `video/transcode` | `/myOrg` | `video` | `transcode` |
| `/myOrg/filter` | `filter` | `/myOrg` |  | `filter` |

Sie können dieses Benennungsschema zum Beispiel bei Verwendung der {{site.data.keyword.openwhisk_short}}-CLI sowie an anderen Stellen verwenden.

### Entitätsnamen
{: #openwhisk_entities_names}

Die Namen aller Entitäten, zu denen Aktionen, Auslöser, Regeln, Pakete und Namensbereiche gehören, sind eine Folge von Zeichen mit dem folgenden Format:

* Das erste Zeichen muss ein alphanumerisches Zeichen oder ein Unterstreichungszeichen sein.
* Die nachfolgenden Zeichen können alphanumerische Zeichen, Leerzeichen oder die folgenden Zeichen sein: `_`, `@`, `.`, `-`.
* Das letzte Zeichen kann kein Leerzeichen sein.

Präziser formuliert, muss der Name dem folgenden regulären Ausdruck (in Java-Metazeichensyntax) entsprechen: `\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`.

## Aktionssemantik
{: #openwhisk_semantics}

In den folgenden Abschnitten werden Details zu {{site.data.keyword.openwhisk_short}}-Aktionen beschrieben.

### Statusunabhängigkeit
{: #openwhisk_semantics_stateless}

Aktionsimplementierungen sind statusunabhängig oder *idempotent*. Das System setzt diese Eigenschaft zwar nicht durch, jedoch ist nicht garantiert, dass ein Status, der von einer Aktion verwaltet wird, über Aufrufe hinweg verfügbar ist.

Darüber hinaus könnten mehrere Instanziierungen einer Aktion mit jeweils eigenem Status vorhanden sein. Ein Aktionsaufruf könnte an irgendeine dieser Instanziierungen gesendet werden.

### Aufrufeingabe und Aufrufausgabe
{: #openwhisk_semantics_invocationio}

Die Eingabe für eine Aktion und die Ausgabe aus einer Aktion ist ein Wörterverzeichnis mit Schlüssel/Wert-Paaren. Der Schlüssel ist eine Zeichenfolge und der Wert ein gültiger JSON-Wert.

### Aufrufreihenfolge von Aktionen
{: #openwhisk_ordering}

Aufrufe einer Aktion werden nicht geordnet. Wenn der Benutzer eine Aktion zweimal über die Befehlszeile oder die REST-API aufruft, ist es möglich, dass der zweite Aufruf vor dem ersten ausgeführt wird. Wenn die Aktionen Nebeneffekte haben, werden diese möglicherweise in irgendeiner Reihenfolge beobachtet.

Außerdem ist nicht garantiert, dass Aktionen automatisch ausgeführt werden. Zwei Aktionen können gleichzeitig ausgeführt werden, sodass ihre Nebeneffekte verzahnt auftreten. OpenWhisk garantiert kein bestimmtes Konsistenzmodell für Nebeneffekte bei gleichzeitiger Ausführung. Alle Nebeneffekte einer gleichzeitigen Ausführung hängen von der Implementierung ab.

### Garantien zur Aktionsausführung
{: #openwhisk_atmostonce}

Wenn eine Aufrufanforderung empfangen wird, zeichnet das System die Anforderung auf und sendet eine Aktivierung.

Das System gibt eine Aktivierungs-ID zurück (bei einem nicht blockierenden Aufruf), die bestätigt, dass der Aufruf empfangen wurde.
Bei einem Netzausfall oder bei anderen Fehlern, die auftreten, bevor Sie eine HTTP-Antwort erhalten, ist es möglich, dass die Anforderung von {{site.data.keyword.openwhisk_short}} empfangen und verarbeitet wurde.

Das System versucht, die Aktion einmal aufzurufen, was zu einem der folgenden vier Ergebnisse führt:
- *success*: Der Aktionsaufruf wurde erfolgreich ausgeführt.
- *application error*: Der Aktionsaufruf war erfolgreich, jedoch hat die Aktion absichtlich einen Fehlerwert zurückzugeben, zum Beispiel weil eine Vorbedingung für die Argumente nicht erfüllt war.
- *action developer error*: Die Aktion wurde aufgerufen, aber abnormal beendet, zum Beispiel weil die Aktion eine Ausnahmebedingung nicht erkannt hat oder weil ein Syntaxfehler vorhanden war.
- *whisk internal error*: Das System konnte die Aktion nicht aufrufen.
Das Ergebnis wird im Feld `status` des Aktivierungsdatensatzes wie im folgenden Abschnitt dokumentiert aufgezeichnet.

Jeder Aufruf, der erfolgreich empfangen wurde und der dem Benutzer möglicherweise in Rechnung gestellt wird, erhält einen Aktivierungsdatensatz.

Wenn das Ergebnis *action developer error* ist, wird die Aktion vielleicht teilweise ausgeführt und generiert extern sichtbare Nebeneffekte. Es liegt in der Verantwortung des Benutzers, zu prüfen, ob solche Nebeneffekte aufgetreten sind, und bei Bedarf eine Retry-Logik auszuführen. Bestimmte whisk-interne Fehler (*whisk internal errors*) weisen darauf hin, dass die Ausführung einer Aktion gestartet wird, jedoch fehlschlägt, bevor die Aktion die Beendigung feststellt.

## Aktivierungsdatensatz
{: #openwhisk_ref_activation}

Jeder Aktionsaufruf und jeder aktivierte Auslöser hat einen Aktivierungsdatensatz zur Folge.

Ein Aktivierungsdatensatz enthält die folgenden Felder:

- *activationId*: Die Aktivierungs-ID.
- *start* und *end*: Zeitmarken, die den Start und das Ende der Aktivierung aufzeichnen. Die Werte haben das [UNIX-Zeitformat](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15).
- *namespace* und `name`: Der Namensbereich und der Name der Entität.
- *logs*: Ein Array von Zeichenfolgen mit den Protokollen, die durch die Aktion während ihrer Aktivierung generiert wurden. Jedes Array-Element entspricht einer Zeilenausgabe an `stdout` oder `stderr` durch die Aktion und enthält die Zeit und den Datenstrom der Protokollausgabe. Die Struktur ist wie folgt: `TIMESTAMP STREAM: LOG_OUTPUT`.
- *response*: Ein Wörterverzeichnis, in dem die Schlüssel `success`, `status` und `result` definiert werden:
  - *status*: Das Aktivierungsergebnis, das einen der folgenden Werte haben kann: "success", "application error", "action developer error", "whisk internal error".
  - *success*: Hat den Wert `true`, wenn und nur wenn der Status `"success"` ist.
- *result*: Ein Wörterverzeichnis, das das Aktivierungsergebnis enthält. Wenn die Aktivierung erfolgreich war, enthält das Ergebnis den Wert, der von der Aktion zurückgegeben wurde. Wenn die Aktivierung nicht erfolgreich war, enthält `result` den Schlüssel `error` und im Regelfall eine Erläuterung des Fehlers.

## JavaScript-Aktionen
{: #openwhisk_ref_javascript}

### Funktionsprototyp
{: #openwhisk_ref_javascript_fnproto}

{{site.data.keyword.openwhisk_short}}-JavaScript-Aktionen werden in einer Node.js-Laufzeit ausgeführt.

Aktionen, die in JavaScript geschrieben sind, müssen auf eine einzige Datei beschränkt werden. Die Datei kann mehrere Funktionen enthalten, jedoch muss konventionsgemäß eine Funktion mit dem Namen `main` vorhanden sein, die aufgerufen wird, wenn die Aktion aufgerufen wird. Das folgende Beispiel zeigt eine Aktion mit mehreren Funktionen.
```javascript
function main() {
    return { payload: helper() }
}

function helper() {
    return new Date();
}
```
{: codeblock}

Die Aktionseingabeparameter werden als JSON-Objekt an die Funktion `main` übergeben. Das Ergebnis einer erfolgreichen Aktivierung ist ebenfalls ein JSON-Objekt. Dieses Objekt wird jedoch, wie im folgenden Abschnitt beschrieben, unterschiedlich zurückgegeben, je nachdem, ob die Aktion synchron oder asynchron ausgeführt wird.

### Synchrones und asynchrones Verhalten
{: #openwhisk_ref_javascript_synchasynch}

Es ist für JavaScript-Funktionen durchaus üblich, die Ausführung in einer Callback-Funktion fortzusetzen, auch nachdem ihre Ausführung beendet ist. Um diesem Verhalten Rechnung zu tragen, kann die Aktivierung einer JavaScript-Aktion *synchron* oder *asynchron* sein.

Die Aktivierung einer JavaScript-Aktion ist **synchron**, wenn die Funktion 'main' unter einer der folgenden Bedingungen vorhanden ist:

- Die Funktion 'main' ist ohne Ausführung der Anweisung `return` vorhanden.
- Die Funktion 'main' ist vorhanden und führt die Anweisung `return` aus, die einen beliebigen Wert *mit Ausnahme eines* Promise-Objekts zurückgibt.

Das folgende Beispiel zeigt eine synchrone Aktion:

```javascript
// Aktion, bei der jeder Pfad zu einer synchronen Aktivierung führt
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

Die Aktivierung einer JavaScript-Aktion ist **asynchron**, wenn die Funktion 'main' mit der Rückgabe eines Promise-Objekts beendet wird. In diesem Fall nimmt das System an, dass die Aktion solange weiter ausgeführt wird, bis das Promise-Objekt erfüllt oder abgelehnt wurde.
Beginnen Sie, indem Sie ein neues Promise-Objekt instanziieren und an eine Callback-Funktion übergeben. Der Callback verwendet zwei Argumente ('resolve' und 'reject'), die beide Funktionen sind. Der gesamte asynchrone Code geht in den Callback ein.

Das folgende Beispiel zeigt, wie ein Promise-Objekt durch Aufruf der Funktion 'resolve' erfüllt wird.
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

Das folgende Beispiel zeigt, wie ein Promise-Objekt durch Aufruf der Funktion 'reject' zurückgewiesen wird.
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

Es ist möglich, dass eine Aktion für einige Eingaben synchron und für andere Eingaben asynchron ausgeführt wird, wie das folgende Beispiel zeigt.
```javascript
function main(params) {
     if (params.payload) {
        // asynchrone Aktivierung
         return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
                }, 100);
             })
     }  else {
        // synchrone Aktivierung
         return {done: true};
      }
}
```
{: codeblock}

Der Aufruf einer Aktion kann blockierend oder nicht blockierend sein, unabhängig davon, ob die Aktivierung synchron oder asynchron ist.

### Globales JavaScript-Objekt 'whisk' wurde entfernt

Das globale Objekt `whisk` wurde entfernt. Migrieren Sie Ihre Node.js-Aktionen, um alternative Methoden zu verwenden.
Verwenden Sie für die Funktionen `whisk.invoke()` und `whisk.trigger()` die bereits installierte Clientbibliothek [openwhisk](https://www.npmjs.com/package/openwhisk).
Für `whisk.getAuthKey()` können Sie den API-Schlüsselwert aus der Umgebungsvariablen `__OW_API_KEY` abrufen.
Für `whisk.error()` können Sie ein abgelehntes Promise-Objekt zurückgeben (d. h. Promise.reject).

### JavaScript-Laufzeitumgebungen
{: #openwhisk_ref_javascript_environments}

JavaScript-Aktionen können in Node.js Version 6 oder Node.js Version 8 ausgeführt werden.
Gegenwärtig werden JavaScript-Aktionen standardmäßig in einer Node.js-Umgebung der Version 6 ausgeführt.
### Npm-Pakete mit Aktionen packen
Für `npm`-Pakete, die in der Node.js-Umgebung nicht vorinstalliert sind, können Sie diese Pakete als Abhängigkeiten packen, wenn Sie Ihre Aktion erstellen oder aktualisieren.

Weitere Informationen finden Sie unter [Aktion als Node.js-Modul packen](./openwhisk_actions.html#openwhisk_js_packaged_action) oder [Aktion als einzelnes Bundle packen](./openwhisk_actions.html#openwhisk_js_webpack_action).

### Umgebung mit Node.js Version 8 und IBM SDKs
{: #openwhisk_ref_javascript_environments_8}
Die Umgebung mit Node.js Version 8.11.1 wird verwendet, wenn beim Erstellen oder Aktualisieren einer Aktion das Flag `--kind` explizit mit dem Wert `nodejs:8` angegeben wird.

Die folgenden Pakete sind in der Umgebung von Node.js Version 8.11.1 vorinstalliert:
  - [amqplib v0.5.2](https://www.npmjs.com/package/amqplib) - Eine Bibliothek zum Erstellen von AMQP 0-9-1-Clients für Node.JS.
  - [apn v2.2.0](https://www.npmjs.com/package/apn) - Ein Node.js-Modul, das als Schnittstelle zum Apple Push Notification-Service fungiert.
  - [async v2.6.1](https://www.npmjs.com/package/async) - Stellt Funktionen zur Arbeit mit asynchronen Funktionen bereit.
  - [bent v1.1.0](https://www.npmjs.com/package/bent) - Funktionaler HTTP-Client für Node.js mit async/await.
  - [body-parser v1.18.3](https://www.npmjs.com/package/body-parser) - Node.js-Middleware für Hauptteilparsing. Parsen eingehender Anforderungen in einer Middleware (vor den Handlern), verfügbar unter der Eigenschaft 'req.body'.
  - [btoa v1.2.1](https://www.npmjs.com/package/btoa) - Ein Port der Funktion 'btoa' des Browsers.
  - [cassandra-driver v3.5.0](https://www.npmjs.com/package/cassandra-driver) - DataStax-Node.js-Treiber für Apache Cassandra.
  - [cloudant v1.10.0](https://www.npmjs.com/package/cloudant) - Dies ist die offizielle Cloudant-Bibliothek für Node.js.
  - [@cloudant/cloudant v2.2.0](https://www.npmjs.com/package/cloudant) - Dies ist die offizielle Cloudant-Bibliothek für Node.js.
  - [commander v2.15.1](https://www.npmjs.com/package/commander) - Die vollständige Lösung für Node.js-Befehlszeilenschnittstellen.
  - [composeaddresstranslator v1.0.4](https://www.npmjs.com/package/composeaddresstranslator) - Adressumsetzer von der Compose-UI oder -API für Scylla-Datenbanken.
  - [consul v0.32.0](https://www.npmjs.com/package/consul) - Ein Client für Consul, der Serviceerkennung und Konfiguration beinhaltet.
  - [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - Parst Cookie-Header und füllt req.cookies mit einem Objekt, dessen Schlüssel durch die Cookie-Namen bestimmt werden.
  - [cradle v0.7.1](https://www.npmjs.com/package/cradle) - Ein allgemeiner CouchDB-Client für Node.js mit Caching-Funktion.
  - [elasticsearch v15.0.0](https://www.npmjs.com/package/elasticsearch) - Der offizielle Elasticsearch-Low-Level-Client für Node.js.
  - [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - Middleware ausschließlich zur Fehlerbehandlung während der Entwicklung.
  - [etcd3 v0.2.11](https://www.npmjs.com/package/etcd3) - Ein qualitativ hochwertiger, einsatzbereiter Client für die auf dem Protokollpuffer basierende etcdv3-API.
  - [express v4.16.3](https://www.npmjs.com/package/express) - Schnelles, eigenständiges, minimalistisches Web-Framework für Node.
  - [express-session v1.15.6](https://www.npmjs.com/package/express-session) - Ermöglicht Sitzungsverarbeitung aus einer App.
  - [formidable v1.2.1](https://www.npmjs.com/package/formidable) - Node.js-Modul für das Parsing von Daten, besonders Dateiuploads.
  - [glob v7.1.2](https://www.npmjs.com/package/glob) - Abgleich von Dateien mithilfe von durch die Shell verwendeten Mustern wie Sternen u. a.
  - [gm v1.23.1](https://www.npmjs.com/package/gm) - GraphicsMagick und ImageMagick für Node.
  - [ibm-cos-sdk v1.2.1](https://www.npmjs.com/package/ibm-cos-sdk) - {{site.data.keyword.cos_full}} SDK für Node.js
  - [ibm_db v2.4.0](https://www.npmjs.com/package/ibm_db) - Eine asynchrone/synchrone Schnittstelle für Node.js zu IBM DB2 und IBM Informix.
  - [ibmiotf v0.2.41](https://www.npmjs.com/package/ibmiotf) - Der Node.js-Client wird zur Vereinfachung der Interaktion mit der IBM Watson Internet of Things Platform verwendet.
  - [iconv-lite v0.4.23](https://www.npmjs.com/package/iconv-lite) - Reine JS-Zeichencodierungskonvertierung.
  - [jsdom v11.10.0](https://www.npmjs.com/package/jsdom) - jsdom ist eine reine JavaScript-Implementierung vieler Webstandards, insbesondere der WHATWG DOM- und HTML-Standards.
  - [jsonwebtoken v8.2.2](https://www.npmjs.com/package/jsonwebtoken) - Eine Implementierung von JSON Web Tokens.
  - [lodash v4.17.10](https://www.npmjs.com/package/lodash) - Die Lodash-Bibliothek, die in Form von Node.js-Modulen exportiert ist.
  - [log4js v2.8.0](https://www.npmjs.com/package/log4js) - Eine Konvertierung des log4js-Frameworks für die Arbeit mit Node.
  - [marked v0.4.0](https://www.npmjs.com/package/marked) - Ein voll ausgestatteter Markdown-Parser und -Compiler, der in JavaScript geschrieben ist. Auf Geschwindigkeit ausgerichtet.
  - [merge v1.2.0](https://www.npmjs.com/package/merge) - Zusammenführen mehrerer Objekte zu einem, um optional ein neues geklontes Objekt zu erstellen.
  - [moment v2.22.2](https://www.npmjs.com/package/moment) - Eine einfache JavaScript-Datumsbibliothek zum Parsen, Validieren, Bearbeiten und Formatieren von Datumswerten.
  - [mongodb v3.0.10](https://www.npmjs.com/package/mongodb) - Der offizielle MongoDB-Treiber für Node.js.
  - [mysql v2.15.0](https://www.npmjs.com/package/mysql) - Dies ist ein Node.js-Treiber für mysql.
  - [mustache v2.3.0](https://www.npmjs.com/package/mustache) - mustache.js ist eine Implementierung des Mustache-Vorlagensystems (Mustache Template System) in JavaScript.
  - [nano v6.4.4](https://www.npmjs.com/package/nano) - Ein minimalistischer CouchDB-Treiber für Node.js.
  - [nodemailer v4.6.5](https://www.npmjs.com/package/nodemailer) - Senden von E-Mails über Node.js – ganz einfach!
  - [oauth2-server v3.0.0](https://www.npmjs.com/package/oauth2-server) - Vollständiges, kompatibles und gründlich getestetes Modul zur Implementierung eines OAuth2-Servers/-Providers mit Express in Node.js.
  - [openwhisk v3.15.0](https://www.npmjs.com/package/openwhisk) - JavaScript-Clientbibliothek für die OpenWhisk-Plattform. Stellt einen Wrapper um die OpenWhisk-APIs herum bereit.
  - [path-to-regex v2.2.1](https://www.npmjs.com/package/path-to-regexp) - Geben Sie eine Pfadzeichenfolge wie /user/:name in einen regulären Ausdruck ein, der dann für den Abgleich mit URL-Pfaden verwendet werden kann.
  - [pg v7.4.3](https://www.npmjs.com/package/pg) - Nicht blockierender PostgreSQL-Client für Node.js. Reines JavaScript und optionale native libpq-Bindungen.
  - [process v0.11.10](https://www.npmjs.com/package/process) - require('process'); wie jedes andere Modul.
  - [pug v2.0.3](https://www.npmjs.com/package/pug) - Implementiert die Vorlagensprache Pug.
  - [redis v2.8.0](https://www.npmjs.com/package/redis) - Dies ist ein vollständiger und umfassend ausgestatteter Redis-Client für Node.js.
  - [request v2.87.0](https://www.npmjs.com/package/request) - Request wurde als einfachste Methode zum Senden von HTTP-Aufrufen konzipiert.
  - [request-promise v4.2.2](https://www.npmjs.com/package/request-promise) - Der vereinfachte HTTP-Anforderungsclient 'request' mit Promise-Unterstützung. Powered by Bluebird.
  - [rimraf v2.6.2](https://www.npmjs.com/package/rimraf) - Der UNIX-Befehl rm -rf für Node.
  - [semver v5.5.0](https://www.npmjs.com/package/semver) - Semantische Versionssteuerung für Node.js
  - [@sendgrid/mail@6.2.1](https://www.npmjs.com/package/@sendgrid/mail) - Stellt E-Mail-Unterstützung über die SendGrid-API bereit.
  - [serve-favicon v2.5.0](https://www.npmjs.com/package/serve-favicon) - Node.js-Middleware zum Bereitstellen eines Favicons.
  - [superagent v3.8.3](https://www.npmjs.com/package/superagent) - SuperAgent ist eine kleine, progressive clientseitige HTTP-Anforderungsbibliothek und ein Node.js-Modul mit derselben API und zahlreichen allgemeinen HTTP-Clientfunktionen.
  - [twilio v3.17.2](https://www.npmjs.com/package/twilio) - Ein Wrapper für die Twilio-API für Sprache, Video und Messaging.
  - [underscore v1.9.1](https://www.npmjs.com/package/underscore) - Underscore.js ist eine Dienstprogrammbibliothek für JavaScript, die die üblichen verdächtigen Funktionen (each, map, reduce, filter...) ohne Erweiterung von JavaScript-Kernobjekten unterstützt.
  - [url-pattern v1.0.3](https://www.npmjs.com/package/url-pattern) - Einfacheres Parsen von URLs für Pfadparameter als bei Verwendung eines Regex-String-Matchers.
  - [uuid v3.2.1](https://www.npmjs.com/package/uuid) - Einfache und schnelle Generierung von RFC4122-UUIDS.
  - [validator v10.3.0](https://www.npmjs.com/package/validator) - Eine Bibliothek von Validatoren und Korrekturfunktionen für Zeichenfolgen.
  - [watson-developer-cloud v3.4.5](https://www.npmjs.com/package/watson-developer-cloud) - Node.js-Clientbibliothek zur Verwendung der Watson Developer Cloud-Services, eine Sammlung von APIs, die Cognitive Computing zum Lösen komplexer Probleme verwenden.
  - [when v3.7.8](https://www.npmjs.com/package/when) - When.js ist eine sehr solide, einsatzerprobte Implementierung von Promises/A+ und when(), einschließlich eines kompletten ES6 Promise-Shims.
  - [winston v2.4.2](https://www.npmjs.com/package/winston) - Eine Multitransport-Bibliothek für asynchrone Protokollierung für Node.js. "Nur die Ruhe, Winston! ... Ich hab die Protokolle eingefügt."
  - [ws v5.2.0](https://www.npmjs.com/package/ws) - 'ws' ist eine bedienungsfreundliche, äußerst schnelle und gründlich getestete WebSocket-Client/Server-Implementierung.
  - [xml2js v0.4.19](https://www.npmjs.com/package/xml2js) - Einfacher Konverter von XML in JavaScript-Objekte. Unterstützt die bidirektionale Konvertierung.
  - [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - XMLHttpRequest für Node ist ein Wrapper für den integrierten HTTP-Client zum Emulieren des XMLHttpRequest-Browserobjekts.
  - [yauzl v2.9.1](https://www.npmjs.com/package/yauzl) - Eine weitere Dekomprimierungsbibliothek (Unzip) für Node. Für das Komprimieren (Zippen).

### Umgebung mit Node.js Version 6
{: #openwhisk_ref_javascript_environments_6}
Die Umgebung mit Node.js 6.14.0 wird verwendet, wenn beim Erstellen oder Aktualisieren einer Aktion das Flag `--kind` explizit mit dem Wert `nodejs:6` angegeben wird.

In der Node.js-Umgebung der Version 6.14.0 sind die folgenden Pakete zur Verwendung verfügbar:

- [apn v2.1.2](https://www.npmjs.com/package/apn) - Ein Node.js-Modul, das als Schnittstelle zum Apple Push Notification-Service fungiert.
- [async v2.1.4](https://www.npmjs.com/package/async) - Stellt Funktionen zur Arbeit mit asynchronen Funktionen bereit.
- [btoa v1.1.2](https://www.npmjs.com/package/btoa) - Ein Port der Funktion 'btoa' des Browsers.
- [cheerio v0.22.0](https://www.npmjs.com/package/cheerio) - Schnelle, flexible und schlanke Implementierung der jQuery Core-Bibliothek, die speziell auf den Server ausgelegt ist.
- [cloudant v1.6.2](https://www.npmjs.com/package/cloudant) - Die offizielle Cloudant-Bibliothek für Node.js.
- [commander v2.9.0](https://www.npmjs.com/package/commander) - Die vollständige Lösung für Node.js-Befehlszeilenschnittstellen.
- [consul v0.27.0](https://www.npmjs.com/package/consul) - Ein Client für Consul, der Serviceerkennung und Konfiguration beinhaltet.
- [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - Parst Cookie-Header und füllt req.cookies mit einem Objekt, dessen Schlüssel durch die Cookie-Namen bestimmt werden.
- [cradle v0.7.1](https://www.npmjs.com/package/cradle) - Ein allgemeiner CouchDB-Client für Node.js mit Caching-Funktion.
- [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - Middleware ausschließlich zur Fehlerbehandlung während der Entwicklung.
- [glob v7.1.1](https://www.npmjs.com/package/glob) - Abgleich von Dateien mithilfe von durch die Shell verwendeten Mustern wie Sternen u. a.
- [gm v1.23.0](https://www.npmjs.com/package/gm) - GraphicsMagick und ImageMagick für Node.
- [lodash v4.17.2](https://www.npmjs.com/package/lodash) - Die Lodash-Bibliothek, die in Form von Node.js-Modulen exportiert ist.
- [log4js v0.6.38](https://www.npmjs.com/package/log4js) - Eine Konvertierung des log4js-Frameworks für die Arbeit mit Node.
- [iconv-lite v0.4.15](https://www.npmjs.com/package/iconv-lite) - Reine JS-Zeichencodierungskonvertierung.
- [marked v0.3.6](https://www.npmjs.com/package/marked) - Ein voll ausgestatteter Markdown-Parser und -Compiler, der in JavaScript geschrieben ist. Auf Geschwindigkeit ausgerichtet.
- [merge v1.2.0](https://www.npmjs.com/package/merge) - Zusammenführen mehrerer Objekte zu einem, um ein neues geklontes Objekt zu erstellen.
- [moment v2.17.0](https://www.npmjs.com/package/moment) - Eine einfache JavaScript-Datumsbibliothek zum Parsen, Validieren, Bearbeiten und Formatieren von Datumswerten.
- [mongodb v2.2.11](https://www.npmjs.com/package/mongodb) - Der offizielle MongoDB-Treiber für Node.js.
- [mustache v2.3.0](https://www.npmjs.com/package/mustache) - Mustache.js ist eine Implementierung des Mustache-Vorlagensystems (Mustache Template System) in JavaScript.
- [nano v6.2.0](https://www.npmjs.com/package/nano) - Ein minimalistischer CouchDB-Treiber für Node.js.
- [node-uuid v1.4.7](https://www.npmjs.com/package/node-uuid) - Veraltetes Paket für UUID.
- [nodemailer v2.6.4](https://www.npmjs.com/package/nodemailer) - Senden von E-Mails über Node.js – ganz einfach!
- [oauth2-server v2.4.1](https://www.npmjs.com/package/oauth2-server) - Vollständiges, kompatibles und gründlich getestetes Modul zur Implementierung eines OAuth2-Servers/-Providers mit Express in Node.js.
- [openwhisk v3.14.0](https://www.npmjs.com/package/openwhisk) - JavaScript-Clientbibliothek für die OpenWhisk-Plattform. Stellt einen Wrapper um die OpenWhisk-APIs herum bereit.
- [pkgcloud v1.4.0](https://www.npmjs.com/package/pkgcloud) - pkgcloud ist eine Standardbibliothek für Node.js, die Abstraktionen von Unterschieden unter mehreren Cloud-Providern bereitstellt.
- [process v0.11.9](https://www.npmjs.com/package/process) - require('process'); wie jedes andere Modul.
- [pug v2.0.0-beta6](https://www.npmjs.com/package/pug) - Implementiert die Vorlagensprache Pug.
- [redis v2.6.3](https://www.npmjs.com/package/redis) - Ein vollständiger und umfassend ausgestatteter Redis-Client für Node.js.
- [request v2.79.0](https://www.npmjs.com/package/request) - Request ist die einfachste Methode zum Senden von HTTP-Aufrufen.
- [request-promise v4.1.1](https://www.npmjs.com/package/request-promise) - Der vereinfachte HTTP-Anforderungsclient 'request' mit Promise-Unterstützung. Powered by Bluebird.
- [rimraf v2.5.4](https://www.npmjs.com/package/rimraf) - Der UNIX-Befehl rm -rf für Node.
- [semver v5.3.0](https://www.npmjs.com/package/semver) - Unterstützt semantische Versionierung.
- [sendgrid v4.7.1](https://www.npmjs.com/package/sendgrid) - Stellt E-Mail-Unterstützung mit der SendGrid-API bereit.
- [serve-favicon v2.3.2](https://www.npmjs.com/package/serve-favicon) - Node.js-Middleware zum Bereitstellen eines Favicons.
- [socket.io v1.6.0](https://www.npmjs.com/package/socket.io) - Socket.IO ermöglicht eine bidirektionale, ereignisgesteuerte Echtzeitkommunikation.
- [socket.io-client v1.6.0](https://www.npmjs.com/package/socket.io-client) - Clientseitige Unterstützung für Socket.IO.
- [superagent v3.0.0](https://www.npmjs.com/package/superagent) - SuperAgent ist eine kleine, progressive clientseitige HTTP-Anforderungsbibliothek und ein Node.js-Modul mit derselben API und zahlreichen allgemeinen HTTP-Clientfunktionen.
- [swagger-tools v0.10.1](https://www.npmjs.com/package/swagger-tools) - Tools für die Arbeit mit Swagger, eine Möglichkeit, APIs zu dokumentieren.
- [tmp v0.0.31](https://www.npmjs.com/package/tmp) - Ein einfacher Ersteller für temporäre Dateien und Verzeichnisse für Node.js.
- [twilio v2.11.1](https://www.npmjs.com/package/twilio) - Ein Wrapper für die Twilio-API für Sprache, Video und Messaging.
- [underscore v1.8.3](https://www.npmjs.com/package/underscore) - Underscore.js ist eine Dienstprogrammbibliothek für JavaScript, die die üblichen verdächtigen Funktionen (each, map, reduce, filter...) ohne Erweiterung von JavaScript-Kernobjekten unterstützt.
- [uuid v3.0.0](https://www.npmjs.com/package/uuid) - Einfache und schnelle Generierung von RFC4122-UUIDS.
- [validator v6.1.0](https://www.npmjs.com/package/validator) - Eine Bibliothek von Validatoren und Korrekturfunktionen für Zeichenfolgen.
- [watson-developer-cloud v2.29.0](https://www.npmjs.com/package/watson-developer-cloud) - Node.js-Clientbibliothek zur Verwendung der Watson Developer Cloud-Services, eine Sammlung von APIs, die Cognitive Computing zum Lösen komplexer Probleme verwenden.
- [when v3.7.7](https://www.npmjs.com/package/when) - When.js ist eine sehr solide, einsatzerprobte Implementierung von Promises/A+ und when(), einschließlich eines  kompletten ES6 Promise-Shims.
- [winston v2.3.0](https://www.npmjs.com/package/winston) - Eine Multitransport-Bibliothek für asynchrone Protokollierung für Node.js. "Nur die Ruhe, Winston! ... Ich hab die Protokolle eingefügt."
- [ws v1.1.1](https://www.npmjs.com/package/ws) - 'ws' ist eine bedienungsfreundliche, äußerst schnelle und gründlich getestete WebSocket-Client/Server-Implementierung.
- [xml2js v0.4.17](https://www.npmjs.com/package/xml2js) - Einfacher Konverter von XML in JavaScript-Objekte. Unterstützt die bidirektionale Konvertierung.
- [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - XMLHttpRequest für Node ist ein Wrapper für den integrierten HTTP-Client zum Emulieren des XMLHttpRequest-Browserobjekts.
- [yauzl v2.7.0](https://www.npmjs.com/package/yauzl) - Eine weitere Dekomprimierungsbibliothek (Unzip) für Node. Für das Komprimieren (Zippen).


## Python-Laufzeitumgebungen
{: #openwhisk_ref_python_environments}

OpenWhisk unterstützt die Ausführung von Python-Aktionen mit zwei verschiedenen Laufzeitversionen.

### Python 3-Aktionen (auf Basis von Jessie)
{: #openwhisk_ref_python_environments_jessie}

Python 3-Aktionen werden mit Python 3.6.5 ausgeführt. Zur Verwendung dieser Laufzeit geben Sie den Parameter `--kind python-jessie:3` in der `wsk`-Befehlszeilenschnittstelle an, wenn Sie eine Aktion erstellen oder aktualisieren.
Wenn Sie Python-Aktionen mit 'virtualenv' erstellen, verwenden Sie das Docker-Image `ibmfunctions/action-python-v3`.
Die Laufzeit enthält neben den Standardbibliotheken von Python 3.6 auch SDK-Pakete für IBM Cloud-Services, die zur Verwendung durch Python-Aktionen verfügbar sind.

Python-Version:
- [3.6.5](https://github.com/docker-library/python/blob/a1aa406bfd8c7b129e6e0ee0ba972b863624ac0d/3.6/jessie/Dockerfile)

Python-Pakete:
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

### Python 3-Aktionen (auf Basis von Alpine)
{: #openwhisk_ref_python_environments_alpine}

Python 3-Aktionen werden mit Python 3.6.1 ausgeführt. Zur Verwendung dieser Laufzeit geben Sie den Parameter `--kind python:3` in der `wsk`-Befehlszeilenschnittstelle an, wenn Sie eine Aktion erstellen oder aktualisieren.
Wenn Sie Python-Aktionen mit 'virtualenv' erstellen, verwenden Sie das Docker-Image `openwhisk/python3action`.
Die folgenden Pakete sind neben den Standardbibliotheken von Python 3.6 zur Verwendung durch Python-Aktionen verfügbar.

Python-Pakete:
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

### Python 2-Aktionen

Python 2-Aktionen werden mit Python 2.7.12, der Standardlaufzeit für Python-Aktionen, ausgeführt. Dies gilt, sofern Sie nicht das Flag `--kind` beim Erstellen oder Aktualisieren einer Aktion angeben. Wenn Sie diese Laufzeit explizit auswählen wollen, verwenden Sie das Flag `--kind python:2`.
Wenn Sie Python-Aktionen mit 'virtualenv' erstellen, verwenden Sie das Docker-Image `openwhisk/python2action`.
Die folgenden Pakete sind neben der Standardbibliothek von Python 2.7 zur Verwendung durch Python 2-Aktionen verfügbar:

Python-Pakete:
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

## Swift-Aktionen
{: #swift-actions}

### Swift 3
Swift 3-Aktionen werden mit Swift 3.1.1 (`--kind swift:3.1.1`) ausgeführt. Geben Sie immer `--kind swift:3.1.1` an, da vorherige Swift-Versionen nicht unterstützt werden.

Sie müssen alle Swift-Aktionen zur Verwendung der Art `swift:3.1.1` migrieren. Geben Sie als bewährtes Verfahren immer die bestimmte Art an, wenn Sie Aktionen erstellen oder aktualisieren.
{: tip}

Swift 3.1.1-Aktionen können die folgenden Pakete verwenden, wenn eine einzelne Swift-Quellendatei verwendet wird:
- KituraNet Version 1.7.6, https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON Version 15.0.1, https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK Version 0.16.0, https://github.com/watson-developer-cloud/swift-sdk

### Swift 4
Swift 4-Aktionen werden mit Swift 4.1 `--kind swift:4.1` ausgeführt.

Befolgen Sie die Anweisungen für [gepackte Swift-Aktionen](./openwhisk_actions.html#packaging-an-action-as-a-swift-executable), um Abhängigkeiten mit einer Datei 'Package.swift' hinzuzufügen.

Swift 4.1-Aktionen können die folgenden Pakete verwenden, wenn eine einzelne Swift-Quellendatei verwendet wird:
- Watson Developer Cloud SDK Version 0.27.0, https://github.com/watson-developer-cloud/swift-sdk

### Swift 3.1.1 auf Swift 4.1 migrieren

#### SwiftyJSON mit einer einzelnen Quellenaktionsdatei
Wenn Sie eine `swift:3.1.1`-Aktion haben, die nicht kompiliert ist (so wie eine Quellendatei mit dem **SwiftyJSON**), müssen Sie Ihre Aktion vorkompilieren und die Version von SwiftyJSON angeben, die Sie für die `swift:4.1`-Aktionsart verwenden wollen. Wenn Sie mit Swift 4.1 beginnen, steht für die native Verwaltung von JSON-Daten eine bessere Unterstützung zur Verfügung.

## PHP-Aktionen
{: #openwhisk_ref_php}

PHP-Aktionen werden mit PHP 7.1 ausgeführt. Zur Verwendung dieser Laufzeit geben Sie den Parameter `--kind php:7.1` in der `wsk`-Befehlszeilenschnittstelle an, wenn Sie eine Aktion erstellen oder aktualisieren. Dies ist das Standardverhalten, wenn Sie eine Aktion mit einer Datei erstellen, die die Erweiterung `.php` hat.

Die folgenden PHT-Erweiterungen sind zusätzlich zu den Standarderweiterungen verfügbar:

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

### Composer-Pakete
Die folgenden Composer-Pakete sind ebenfalls verfügbar:

- guzzlehttp/guzzle       v6.7.3
- ramsey/uuid             v3.6.3

## Docker-Aktionen
{: #openwhisk_ref_docker}

Docker-Aktionen werden in einer vom Benutzer bereitgestellten Binärdatei in einem Docker-Container ausgeführt. Die Binärdatei wird in einem Docker-Image auf der Basis von [python:2.7.12-alpine](https://hub.docker.com/r/library/python) ausgeführt, sodass die Binärdatei mit dieser Distribution kompatibel sein muss.

Das Docker-Gerüst (Skeleton) ist eine bequeme Methode, OpenWhisk-kompatible Docker-Images zu erstellen. Sie können das Gerüst mit dem CLI-Plug-in-Befehl `ibmcloud wsk sdk install docker` installieren.

Das Hauptbinärprogramm muss sich in `/action/exec` im Container befinden. Die ausführbare Datei empfängt die Eingabeargumente von einer einzelnen Befehlszeilenargumentzeichenfolge, die als `JSON`-Objekt deserialisiert werden kann. Es muss ein Ergebnis über `stdout` in Form einer einzeiligen Zeichenfolge aus serialisierten `JSON`-Daten zurückgeben.

Sie können darüber hinaus auch Kompilierungsschritte oder Abhängigkeiten einbeziehen, indem Sie die `Dockerfile` ändern, die in `dockerSkeleton` enthalten ist.

## REST-API
{: #openwhisk_ref_restapi}
Informationen zur {{site.data.keyword.openwhisk_short}}-REST-API finden Sie in der [REST-API-Referenz](https://console.bluemix.net/apidocs/98-cloud-functions?&language=node#introduction).

## Systembegrenzungen
{: #openwhisk_syslimits}

### Aktionen
{{site.data.keyword.openwhisk_short}} unterliegt einigen wenigen Systembegrenzungen, wie zum Beispiel in Bezug auf die Speicherkapazität, die eine Aktion verwenden kann, oder auf die zulässige Anzahl von Aktionsaufrufen pro Minute.

In der folgenden Tabelle sind die Standardbegrenzungen für Aktionen aufgeführt.

| Begrenzung | Beschreibung | Standardwert | Min | Max |
| ----- | ----------- | :-------: | :---: | :---: |
| [codeSize (Codegröße)](openwhisk_reference.html#openwhisk_syslimits_codesize) |Die maximale Größe des Aktionscodes in MB. | 48 | 1 | 48 |
| [concurrent (Gleichzeitig)](openwhisk_reference.html#openwhisk_syslimits_concurrent) | Pro Namensbereich können nicht mehr als N aktuell ausgeführte oder in die Warteschlange für die Ausführung gestellte Aktivierungen übergeben werden. | 1000 | 1 | 1000* |
| [logs (Protokolle)](openwhisk_reference.html#openwhisk_syslimits_logs) | Ein Container darf nicht mehr als N MB in die Standardausgabe schreiben. | 10 | 0 | 10 |
| [memory (Speicher)](openwhisk_reference.html#openwhisk_syslimits_memory) | Ein Container darf nicht mehr als N MB Speicher zuordnen. | 256 | 128 | 512 |
| [minuteRate (Minutenrate)](openwhisk_reference.html#openwhisk_syslimits_minuterate) | Pro Namensbereich können nicht mehr als N Aktivierungen pro Minute übergeben werden. | 5000 | 1 | 5000* |
| [openulimit (Ulimit für offene Dateien)](openwhisk_reference.html#openwhisk_syslimits_openulimit) | Die maximale Anzahl geöffneter Dateien für eine Aktion. | 1024 | 0 | 1024 |
| [parameters (Parameter)](openwhisk_reference.html#openwhisk_syslimits_parameters) | Die maximale Größe der Parameter, die angehängt werden können (in MB). | 1 | 0 | 1 |
| [proculimit (Ulimit für Prozesse)](openwhisk_reference.html#openwhisk_syslimits_proculimit) | Die maximale Anzahl der für eine Aktion verfügbaren Prozesse. | 1024 | 0 | 1024 |
| [result (Ergebnis)](openwhisk_reference.html#openwhisk_syslimits_result) | Die maximale Größe des Ergebnisses des Aktionsaufrufs in MB. | 1 | 0 | 1 |
| [sequenceMaxActions](openwhisk_reference.html#openwhisk_syslimits_sequencemax) | Die maximale Anzahl von Aktionen, die eine bestimmte Sequenz umfassen. | 50 | 0 | 50* |
| [timeout (Zeitlimit)](openwhisk_reference.html#openwhisk_syslimits_timeout) | Ein Container darf nicht länger als N Millisekunden aktiv sein. | 60000 | 100 | 600000 |

### Festgelegte Begrenzungen erhöhen
{: #increase_fixed_limit}

Begrenzungswerte, die mit einem Stern (*) enden, sind festgelegt, können jedoch erhöht werden, wenn eine Kosten-Nutzen-Analyse höhere Sicherheitsbegrenzungswerte rechtfertigt. Wenn Sie den Begrenzungswert erhöhen möchten, wenden Sie sich an IBM Support, indem Sie ein Ticket direkt über die IBM [{{site.data.keyword.openwhisk_short}}-Webkonsole](https://console.bluemix.net/openwhisk/) öffnen.
  1. Wählen Sie **Support** aus.
  2. Wählen Sie **Ticket hinzufügen** im Dropdown-Menü aus.
  3. Wählen Sie **Technisch** als Tickettyp aus.
  4. Wählen Sie **Funktionen** als technischen Bereich der Unterstützung aus.

#### codeSize (MB) (Festgelegt: 48 MB)
{: #openwhisk_syslimits_codesize}
* Die maximale Codegröße für eine Aktion ist 48 MB.
* Es wird empfohlen, für eine JavaScript-Aktion ein Tool zum Verketten des gesamten Quellcodes, einschließlich der Abhängigkeiten, in einer einzelnen Bundledatei zu verwenden.
* Diese Begrenzung ist festgelegt und kann nicht geändert werden.

#### concurrent (Festgelegt: 1000*)
{: #openwhisk_syslimits_concurrent}
* Die Anzahl der Aktivierungen, die für einen Namensbereich entweder ausgeführt oder in die Warteschlange für die Ausführung gestellt werden, kann 1000 nicht überschreiten.
* Dieser Begrenzungswert ist festgelegt, kann jedoch erhöht werden, wenn eine Kosten-Nutzen-Analyse höhere Sicherheitsbegrenzungswerte rechtfertigt. Ausführliche Anweisungen zur Erhöhung dieser Begrenzung finden Sie im Abschnitt [Festgelegte Begrenzungen erhöhen](openwhisk_reference.html#increase_fixed_limit).

#### logs (MB) (Standardwert: 10 MB)
{: #openwhisk_syslimits_logs}
* Die Protokollbegrenzung N liegt im Bereich von [0 MB..10 MB] und wird pro Aktion festgelegt.
* Ein Benutzer kann die Begrenzung des Aktionsprotokolls ändern, wenn eine Aktion erstellt oder aktualisiert wird.
* Protokolle, die die festgelegte Begrenzung überschreiten, werden abgeschnitten, sodass neue Protokolleinträge ignoriert werden. Ferner wird als letzte Ausgabe der Aktivierung eine Warnung hinzugefügt, um darauf hinzuweisen, dass die Aktivierung die festgelegte Protokollbegrenzung überschritten hat.

#### memory (MB) (Standardwert: 256 MB)
{: #openwhisk_syslimits_memory}
* Die Speicherbegrenzung M liegt im Bereich von [128 MB..512 MB] und wird pro Aktion in MB festgelegt.
* Ein Benutzer kann die Speicherbegrenzung beim Erstellen der Aktion ändern.
* Ein Container kann nicht mehr Speicher nutzen, als durch die Begrenzung zugelassen wird.

#### minuteRate (Festgelegt: 5000*)
{: #openwhisk_syslimits_minuterate}
* Die Begrenzung N der Rate ist auf 5000 festgelegt und begrenzt die Anzahl von Aktionsaufrufen in Fenstern von 1 Minute.
* Ein CLI- oder API-Aufruf, der diese Begrenzung überschreitet, empfängt einen Fehlercode, der dem HTTP-Statuscode `429: TOO MANY REQUESTS` entspricht.
* Dieser Begrenzungswert ist festgelegt, kann jedoch erhöht werden, wenn eine Kosten-Nutzen-Analyse höhere Sicherheitsbegrenzungswerte rechtfertigt. Ausführliche Anweisungen zur Erhöhung dieser Begrenzung finden Sie im Abschnitt [Festgelegte Begrenzungen erhöhen](openwhisk_reference.html#increase_fixed_limit).

#### openulimit (Festgelegt: 1024:1024)
{: #openwhisk_syslimits_openulimit}
* Die maximale Anzahl geöffneter Dateien für eine Aktion beträgt 1024 (bezieht sich auf feste und veränderliche Begrenzungen).
* Diese Begrenzung ist festgelegt und kann nicht geändert werden.
* Wenn eine Aktion aufgerufen wird, verwendet der Docker-Ausführungsbefehl das Argument `--ulimit nofile=1024:1024`, um den Wert für `openulimit` festzulegen.
* Weitere Informationen finden Sie in der Referenzdokumentation zum [Docker-Ausführungsbefehl (run)](https://docs.docker.com/engine/reference/commandline/run).

#### parameters (Festgelegt: 1 MB)
{: #openwhisk_syslimits_parameters}
* Die Größenbegrenzung für alle Parameter bei der Erstellung oder Aktualisierung einer Aktion, eines Pakets oder eines Auslösers ist 1 MB.
* Eine Entität mit Parametern, die die Begrenzung überschreiten, wird bei der Erstellung oder Aktualisierung zurückgewiesen.
* Diese Begrenzung ist festgelegt und kann nicht geändert werden.

#### proculimit (Festgelegt: 1024:1024)
{: #openwhisk_syslimits_proculimit}
* Die maximale Anzahl der für den Aktionscontainer verfügbaren Prozesse ist 1024.
* Diese Begrenzung ist festgelegt und kann nicht geändert werden.
* Wenn eine Aktion aufgerufen wird, verwendet der Docker-Ausführungsbefehl das Argument `--pids-limit 1024`, um den Wert für `proculimit` festzulegen.
* Weitere Informationen finden Sie in der Referenzdokumentation zum [Docker-Ausführungsbefehl (run)](https://docs.docker.com/engine/reference/commandline/run).

#### result (Festgelegt: 1 MB)
{: #openwhisk_syslimits_result}
* Die maximale Ausgabegröße des Ergebnisses eines Aktionsaufrufs in MB.
* Diese Begrenzung ist festgelegt und kann nicht geändert werden.

#### sequenceMaxActions (Festgelegter Wert: 50*)
{: #openwhisk_syslimits_sequencemax}
* Die maximale Anzahl von Aktionen, die eine bestimmte Sequenz umfassen.
* Diese Begrenzung ist festgelegt und kann nicht geändert werden.

#### timeout (ms) (Standardwert: 60s)
{: #openwhisk_syslimits_timeout}
* Das Zeitlimit N liegt im Bereich von [100 ms..600000 ms] und wird pro Aktion in Millisekunden festgelegt.
* Ein Benutzer kann das Zeitlimit ändern, wenn eine Aktion erstellt wird.
* Ein Container, der länger als N Millisekunden aktiv ist, wird beendet.

### Auslöser

Auslöser unterliegen einer Aktivierungsrate pro Minute (wie in der folgenden Tabelle angegeben).

| Begrenzung | Beschreibung | Standardwert | Min | Max |
| ----- | ----------- | :-------: | :---: | :---: |
| [minuteRate (Minutenrate)](openwhisk_reference.html#openwhisk_syslimits_tminuterate) | Pro Namensbereich können nicht mehr als N Auslöser pro Minute aktiviert werden. | 5000* | 5000* | 5000* |

### Festgelegte Begrenzungen erhöhen
{: #increase_fixed_tlimit}

Begrenzungswerte, die mit einem Stern (*) enden, sind festgelegt, können jedoch erhöht werden, wenn eine Kosten-Nutzen-Analyse höhere Sicherheitsbegrenzungswerte rechtfertigt. Wenn Sie den Begrenzungswert erhöhen möchten, wenden Sie sich an IBM Support, indem Sie ein Ticket direkt über die IBM [{{site.data.keyword.openwhisk_short}}-Webkonsole](https://console.bluemix.net/openwhisk/) öffnen.
  1. Wählen Sie **Support** aus.
  2. Wählen Sie **Ticket hinzufügen** im Dropdown-Menü aus.
  3. Wählen Sie **Technisch** als Tickettyp aus.
  4. Wählen Sie **Funktionen** als technischen Bereich der Unterstützung aus.

#### minuteRate (Festgelegt: 5000*)
{: #openwhisk_syslimits_tminuterate}

* Die Begrenzung N der Rate ist auf 5000 festgelegt und begrenzt die Anzahl von Auslösern, die ein Benutzer in Fenstern von 1 Minute aktivieren kann.
* Ein Benutzer kann die Auslöserbegrenzung nicht ändern, wenn ein Auslöser erstellt wird.
* Ein CLI- oder API-Aufruf, der diese Begrenzung überschreitet, empfängt einen Fehlercode, der dem HTTP-Statuscode `429: TOO MANY REQUESTS` entspricht.
* Dieser Begrenzungswert ist festgelegt, kann jedoch erhöht werden, wenn eine Kosten-Nutzen-Analyse höhere Sicherheitsbegrenzungswerte rechtfertigt. Ausführliche Anweisungen zur Erhöhung dieser Begrenzung finden Sie im Abschnitt [Festgelegte Begrenzungen erhöhen](openwhisk_reference.html#increase_fixed_tlimit).
