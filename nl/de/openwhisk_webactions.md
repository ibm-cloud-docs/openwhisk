---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Webaktionen
{: #openwhisk_webactions}

Webaktionen sind OpenWhisk-Aktionen, die Annotationen versehen wurden, um Entwickler schnell in die Lage zu versetzen, webbasierte Anwendungen zu erstellen. Diese annotierten Aktionen bieten Entwicklern die Möglichkeit, Back-End-Logik zu programmieren, auf die Ihre Webanwendung anonym zugreifen kann, ohne einen OpenWhisk-Authentifizierungsschlüssel zu benötigen. Es liegt in der Zuständigkeit des Aktionsentwicklers, die gewünschte Authentifizierung und Autorisierung (d. h. den OAuth-Ablauf) zu implementieren.
{: shortdesc}

Aktivierungen von Webaktionen werden dem Benutzer zugeordnet, der die Aktion erstellt hat. Bei dieser Aktion werden also die Kosten einer Aktionsaktivierung vom Aufrufer auf den Eigner der Aktion verlagert.

Betrachten Sie zum Beispiel die folgende JavaScript-Aktion `hello.js`:
```javascript
function main({name}) {
  var msg = 'you did not tell me who you are.';
  if (name) {
    msg = `hello ${name}!`
  }
  return {body: `<html><body><h3>${msg}</h3></body></html>`}
}
```
{: codeblock}  

Sie können nun eine _Webaktion_ **hello** im Paket `demo` für den Namensbereich `guest` mithilfe des CLI-Flags `--web` mit dem Wert `true` oder `yes` erstellen:
```
ibmcloud wsk package create demo
```
{: pre}

```
ibmcloud wsk action create /guest/demo/hello hello.js --web true
```
{: pre}

Durch das Flag `--web` mit dem Wert `true` oder `yes` kann eine Aktion durch eine REST-Schnittstelle zugänglich gemacht werden, ohne dass der Zugriff Berechtigungsnachweise erfordert. Informationen zum Konfigurieren einer Webaktion mit Berechtigungsnachweisen finden Sie im Abschnitt [Webaktionen sichern](./openwhisk_webactions.html#securing-web-actions). Eine Webaktion kann mit einer URL aufgerufen werden, die wie folgt strukturiert ist:
`https://{APIHOST}/api/v1/web/{namespace}/{packageName}/{actionName}.{EXT}`.

Der Paketname ist **default**, wenn sich die Aktion nicht in einem benannten Paket befindet.

Ein Beispiel ist `guest/demo/hello`. Der API-Pfad für die Webaktion kann mit `curl` oder `wget` ohne API-Schlüssel verwendet werden. Er kann sogar direkt in den Browser eingegeben werden.

Versuchen Sie, den Pfad [https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane](https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane) in Ihrem Web-Browser zu öffnen. Oder versuchen Sie, die Aktion über `curl` aufzurufen:
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane
```
{: pre}

Im folgenden Beispiel führt eine Webaktion eine HTTP-Umleitung aus:
```javascript
function main() {
  return {
    headers: { location: 'http://openwhisk.org' },
    statusCode: 302
  }
}
```
{: codeblock}    

Im folgenden Beispiel setzt eine Webaktion ein einzelnes Cookie:
```javascript
function main() {
  return {
    headers: {
      'Set-Cookie': 'UserID=Jane; Max-Age=3600; Version=',
      'Content-Type': 'text/html'
    }, 
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
}
```
{: codeblock}  

Im folgenden Beispiel setzt eine Webaktion mehrere Cookies:
```javascript
function main() {
  return {
    headers: {
      'Set-Cookie': [
        'UserID=Jane; Max-Age=3600; Version=',
        'SessionID=asdfgh123456; Path = /'
      ],
      'Content-Type': 'text/html'
    }, 
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
}
```
{: codeblock}

Im folgenden Beispiel wird `image/png` zurückgegeben:
```javascript
function main() {
    let png = <base 64 encoded string>
    return { headers: { 'Content-Type': 'image/png' },
             statusCode: 200,
             body: png };
}
```
{: codeblock}  

The following example returns `application/json`:
```javascript
function main(params) {
    return {
        statusCode: 200,
        headers: { 'Content-Type': 'application/json' },
        body: params
    };
}
```
{: codeblock}  

The default `Content-Type` for an HTTP response is `application/json`, and the body can be any allowed JSON value. The default `Content-Type` can be omitted from the headers.

It is important to be aware of the [Antwortgrößenbegrenzung](./openwhisk_reference.html) für Aktionen zu beachten, da eine Antwort, die die vordefinierten Systembegrenzungen überschreitet, fehlschlägt. Große Objekte werden nicht inline über OpenWhisk gesendet, sondern werden zum Beispiel in einen Objektspeicher verlagert.

## HTTP-Anforderungen mit Aktionen verarbeiten
{: #openwhisk_webactions_http}

Eine OpenWhisk-Aktion, die keine Webaktion ist, erfordert eine Authentifizierung und muss mit einem JSON-Objekt antworten. Im Gegensatz dazu können Webaktionen ohne Authentifizierung aufgerufen werden und lassen sich dazu verwenden, HTTP-Handler zu implementieren, die mit verschiedenartigen Inhalten für die Elemente _headers_, _statusCode code_ und _body_ antworten. Die Webaktion muss ein JSON-Objekt zurückgeben. Das OpenWhisk-System (d. h. der `Controller`) behandelt jedoch eine Webaktion anders, wenn das Ergebnis eines oder mehrere der folgenden Elemente als JSON-Eigenschaften der höchsten Ebene enthält:

- `headers`: Ein JSON-Objekt, in dem die Schlüssel Headernamen (header-names) und die Werte Zeichenfolgewerte, numerische Werte oder boolesche Werte für diese Header sind (Standardwert: keine Header). Für das Senden mehrerer Werte für einen einzelnen Header ist der Wert des Headers ein JSON-Array mit Werten.
- `statusCode`: Ein gültiger HTTP-Statuscode (Standardwert: 200 OK).
- `body`: Eine Zeichenfolge aus einfachem Text oder in Base64-Codierung (für Binärdaten).

Der Controller gibt die durch die Aktion angegebenen Header, sofern vorhanden, an den HTTP-Client weiter, der die Anforderung/Antwort-Operation beendet. Ganz ähnlich antwortet der Controller auch mit einem Statuscode, wenn dieser vorhanden ist. Zuletzt wird das Body-Element als Hauptteil der Antwort übergeben. Wenn kein Inhaltstypheader (`Content-Type`) in den Headern (`headers`) des Aktionsergebnisses deklariert ist, wird der Hauptteil so übergeben, als wäre er eine Zeichenfolge (was einen Fehler verursacht, wenn dies nicht der Fall ist). Wenn der Inhaltstyp (`Content-Type`) definiert ist, ermittelt der Controller, ob die Antwort aus Binärdaten oder Klartext besteht, und dekodiert die Zeichenfolge bei Bedarf mit einem Base64-Dekoder. Lässt sich der Hauptteil nicht ordnungsgemäß dekodieren, wird ein Fehler an den Aufrufer zurückgegeben.

_Hinweis:_ Ein JSON-Objekt oder -Array wird wie Binärdaten behandelt und muss in Base64-Codierung codiert sein.

## HTTP-Kontext
{: #http-context}

Alle Webaktionen empfangen beim Aufruf HTTP-Anforderungsinformationen als Parameter für das Aktionseingabeargument.

Betrachten Sie die folgenden HTTP-Parameter:

- `__ow_method` (Typ: Zeichenfolge): Die HTTP-Methode der Anforderung.
- `__ow_headers` (Typ: Zuordnung von Zeichenfolge zu Zeichenfolge): Die Anforderungsheader.
- `__ow_path` (Typ: Zeichenfolge): Der unabgeglichene Pfad der Anforderung (der Abgleich wird nach Verarbeitung der Aktionserweiterung beendet).
- `__ow_user` (Typ: Zeichenfolge): Der Namensbereich, der das für OpenWhisk authentifizierte Subjekt identifiziert.
- `__ow_body` (Typ: Zeichenfolge): Die Anforderungshauptteilentität als Zeichenfolge in Base64-Codierung, wenn der Inhalt binär ist, oder andernfalls als normale Zeichenfolge.
- `__ow_query` (Typ: Zeichenfolge): Die Abfrageparameter aus der Anforderung als nicht geparste Zeichenfolge.

Eine Anforderung kann keine der benannten `__ow_`-Parameter überschreiben. Falls dies doch geschieht, schlägt die Anforderung mit dem Status 400 "Bad Request" fehl.

Die Eigenschaft `__ow_user` ist nur vorhanden, wenn die Webaktion eine [Annotation für eine erforderliche Authentifizierung](./openwhisk_annotations.html#annotations-specific-to-web-actions) besitzt, und ermöglicht einer Webaktion die Implementierung ihrer eigenen Berechtigungsrichtlinie. Die Eigenschaft `__ow_query` ist nur dann verfügbar, wenn eine Webaktion die Verarbeitung der ["unaufbereiteten" HTTP-Anforderung](#raw-http-handling) wählt. Es handelt sich um eine Zeichenfolge, die die aus dem URI geparsten Abfrageparameter (getrennt durch `&`) enthält. Die Eigenschaft `__ow_body` ist entweder in "unaufbereiteten" HTTP-Anforderungen vorhanden oder ist vorhanden, wenn die HTTP-Anforderung kein JSON-Objekt oder keine Formulardaten darstellt. Andernfalls empfangen Webaktionen Abfrage- und Hauptteilparameter als Eigenschaften erster Klasse im Aktionsargument. Hauptteilparameter haben Vorrang vor Abfrageparametern, die wiederum Vorrang vor Aktions- und Paketparametern haben.

## HTTPS-Endpunktunterstützung

Unterstütztes SSL-Protokolle: TLS 1.0, TLS 1.1, TLS 1.2, TLS 1.3 ([Entwurfsversion 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18))

Nicht unterstützte SSL-Protokolle: SSLv2, SSLv3

## Zusätzliche Features
{: #extra-features}

Webaktionen stellen einige zusätzlichen Features bereit wie zum Beispiel:

- `Inhaltserweiterungen:` Die Anforderung muss den gewünschten Inhaltstyp durch `.json`, `.html`, `.http`, `.svg` oder `.text` angeben. Der Typ wird angegeben, indem dem Aktionsnamen im URI eine Erweiterung hinzugefügt wird, sodass zum Beispiel eine Aktion `/guest/demo/hello` durch `/guest/demo/hello.http` angegeben wird, um eine HTTP-Antwort zu empfangen. Wenn keine Erweiterung erkannt wird, wird der Einfachheit halber die Erweiterung `.http` angenommen.
- `Felder aus dem Ergebnis projizieren:` Der Pfad, der auf den Aktionsnamen folgt, wird dazu verwendet, eine oder mehrere Ebenen der Antwort herauszuprojizieren.
`/guest/demo/hello.html/body`. Durch dieses Feature kann eine Aktion, die ein Wörterverzeichnis `{body: "..." }` zurückgibt, die Eigenschaft `body` projizieren und direkt den entsprechenden Zeichenfolgewert zurückgeben. Der projizierte Pfad folgt einem absoluten Pfadmodell (wie in XPath).
- `Abfrage- und Hauptteilparameter als Eingabe:` Die Aktion empfängt Abfrageparameter und Parameter im Anforderungshauptteil. Die Rangordnung für das Zusammenfügen von Parametern sieht wie folgt aus: Paketparameter, Aktionsparameter, Abfrageparameter und Hauptteilparameter. Jeder dieser Parameter kann vorherige Werte überschreiben, wenn diese sich überschneiden. Beispiel: Der Parameter `/guest/demo/hello.http?name=Jane` kann das Argument `{name: "Jane"}` an die Aktion übergeben.
- `Formulardaten:` Neben den Standarddaten vom Typ `application/json` können Webaktionen als Eingabe URL-codierte Daten vom Typ `application/x-www-form-urlencoded data` empfangen.
- `Aktivierung mithilfe mehrerer HTTP-Verben:` Eine Webaktion kann durch eine der folgenden HTTP-Methoden aufgerufen werden: `GET`, `POST`, `PUT`, `PATCH` und `DELETE` sowie `HEAD` und `OPTIONS`.
- `Verarbeitung von Nicht-JSON-Hauptteil und unaufbereiteter HTTP-Entität:` Eine Webaktion kann einen HTTP-Anforderungshauptteil akzeptieren, der kein JSON-Objekt ist, und auswählen, dass solche Werte immer als nicht transparente Werte zu empfangen sind (einfacher Text, falls nicht binär, oder andernfalls Zeichenfolge in Base64-Codierung).

Im nachfolgenden Beispiel wird skizziert, wie Sie diese Features in einer Webaktion nutzen könnten. Gegeben sei eine Aktion `/guest/demo/hello` mit dem folgenden Hauptteil:
```javascript
function main(params) {
    return { response: params };
}
```

Wenn diese Aktion als Webaktion aufgerufen wird, können Sie die Antwort der Webaktion ändern, indem Sie unterschiedliche Pfade aus dem Ergebnis projizieren.

Im Folgenden wird beispielsweise das gesamte Objekt zurückgegeben und es kann festgestellt werden, welche Argumente die Aktion empfängt:
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json
 ```
{: pre}

Beispielausgabe:
```
{
  "response": {
    "__ow_method": "get",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```
{: screen}

Der folgende Beispielbefehl zeigt, wie die Aktion mit einem Abfrageparameter ausgeführt wird:
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json?name=Jane
 ```
{: pre}

Beispielausgabe:
```
{
  "response": {
    "name": "Jane",
    "__ow_method": "get",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```
{: screen}

Sie können die Aktion auch mit Formulardaten ausführen:
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -d "name":"Jane"
 ```
{: pre}

Beispielausgabe:
```
{
  "response": {
    "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "10",      
      "content-type": "application/x-www-form-urlencoded",      
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```
{: screen}

Führen Sie den folgenden Befehl für ein JSON-Objekt aus:
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
```
{: pre}

Beispielausgabe:
```
{
  "response": {
    "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "15",      
      "content-type": "application/json",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```
{: screen}

Führen Sie den folgenden Befehl aus, um den Namen (als Text) zu projizieren:
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.text/response/name?name=Jane
```
{: pre}

Beispielausgabe:
```
Jane
```
{: screen}

Abfrageparameter, Formulardaten und JSON-Objekthauptteilentitäten werden der Einfachheit halber als Wörterverzeichnisse behandelt und ihre Werte sind direkt als Aktionseingabeeigenschaften zugänglich. Dieses Verhalten gilt nicht für Webaktionen, die eine direktere Verarbeitung von HTTP-Anforderungsentitäten wählen, oder wenn die Webaktion eine Entität empfängt, die kein JSON-Objekt ist.

Im folgenden Beispiel wird der Inhaltstyp "text" wie zuvor gezeigt verwendet.
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
```
{: pre}

Beispielausgabe:
```
{
  "response": {
    "__ow_method": "post",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "4",      
      "content-type": "text/plain",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": "",
    "__ow_body": "Jane"
  }
}
```
{: screen}

## Inhaltserweiterungen
{: #openwhisk_webactions_extensions}

Eine Inhaltserweiterung ist in der Regel zum Aufrufen einer Webaktion erforderlich. Wenn keine Erweiterung angegeben ist, wird standardmäßig `.http` angenommen. Für die Erweiterungen `.json` und `.http` ist kein Projektionspfad erforderlich, während für die Erweiterungen `.html`, `.svg` und `.text` ein solcher Pfad erforderlich ist. Aus Benutzerkomfortgründen wird standardmäßig angenommen, dass der Pfad dem Erweiterungsnamen entspricht. Um eine Webaktion aufzurufen und eine `.html`-Antwort zu empfangen, muss die Aktion mit einem JSON-Objekt antworten, das eine Eigenschaft auf höchster Ebene mit dem Namen `html` enthält (oder der Antworttyp muss im expliziten Pfad enthalten sein). Dies heißt mit anderen Worten, dass die Angabe `/guest/demo/hello.html` zum Beispiel zur Angabe `/guest/demo/hello.html/html`, in der die Eigenschaft `html` explizit projiziert wird, äquivalent ist. Der vollständig qualifizierte Name der Aktion muss den entsprechenden Paketnamen enthalten, der `default` lautet, wenn die Aktion nicht in einem benannten Paket enthalten ist.

## Geschützte Parameter
{: #openwhisk_webactions_protected}

Aktionsparameter werden geschützt und als unveränderlich behandelt. Parameter werden automatisch als endgültig festgelegt, um Webaktionen zu aktivieren.
```
ibmcloud wsk action create /guest/demo/hello hello.js --parameter name Jane --web true
```
{: pre}

Das Ergebnis dieser Änderungen besteht darin, dass das Element `name` an `Jane` gebunden wird und wegen der Annotation 'final' nicht durch Abfrage- oder Hauptteilparameter überschrieben werden kann. Dieses Funktionsweise schützt die Aktion gegen Abfrage- oder Hauptteilparameter, die versehentlich oder absichtlich versuchen, diesen Wert zu ändern.

## Webaktionen sichern
{: #securing-web-actions}

Standardmäßig kann eine Webaktion von jedem aufgerufen werden, der die Aufruf-URL der Webaktion hat. Verwenden Sie zum Sichern der Webaktion die [Webaktionsannotation](./openwhisk_annotations.html#annotations-specific-to-web-actions) `require-whisk-auth`. Wenn die Annotation `require-whisk-auth` auf `true` gesetzt ist, authentifiziert die Aktion die Berechtigungsnachweis-Basisberechtigung der Aufrufanforderung gegenüber dem Whisk-Authentifizierungsschlüssel des Aktionseigners. Bei Festlegung auf eine Zahl oder eine Zeichenfolge, bei der Groß-/Kleinschreibung beachtet werden muss, muss die Aufrufanforderung der Aktion einen Header `X-Require-Whisk-Auth` mit demselben Wert enthalten. Gesicherte Webaktionen geben die Nachricht `NotAuthorized` zurück, wenn die Berechtigungsnachweisvalidierung fehlschlägt.

Verwenden Sie alternativ das Flag `--web-secure`, um die Annotation `require-whisk-auth` automatisch festzulegen. Bei der Einstellung `true` wird eine Zufallszahl als Annotationswert `require-whisk-auth` generiert. Bei der Einstellung `false` wird die Annotation `require-whisk-auth` entfernt. Bei Einstellung auf einen anderen Wert wird dieser Wert als Annotationswert `require-whisk-auth` verwendet.

Beispiel mit **--web-secure**:
```bash
ibmcloud wsk action update /guest/demo/hello hello.js --web true --web-secure my-secret
```
{: pre}

Beispiel mit **require-whisk-auth**:
```bash
ibmcloud wsk action update /guest/demo/hello hello.js --web true -a require-whisk-auth my-secret
```
{: pre}

Beispiel mit **X-Require-Whisk-Auth**:
```bash
curl https://${APIHOST}/api/v1/web/guest/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: my-secret"
```
{: pre}

Es ist wichtig zu beachten, dass dem Eigner der Webaktion alle Aktivierungsdatensätze zugeordnet sind und dass er die Kosten für die Ausführung der Aktion im System trägt - unabhängig davon, wie die Aktion aufgerufen wurde.

## Webaktionen inaktivieren

Zum Inaktivieren einer Webaktion, sodass sie nicht mehr über die Web-API (`https://openwhisk.bluemix.net/api/v1/web/`) aufgerufen werden kann, übergeben Sie den Wert `false` oder `no` an das Flag `--web`, um eine Aktion über die Befehlszeilenschnittstelle zu aktualisieren.
```
ibmcloud wsk action update /guest/demo/hello hello.js --web false
```
{: pre}

## Unaufbereitete HTTP-Anforderungen verarbeiten

Eine Webaktion kann wählen, dass ein eingehender HTTP-Hauptteil direkt interpretiert und verarbeitet wird, ohne Weiterleitung eines JSON-Objekts an Eigenschaften erster Klasse, die für die Aktionseingabe verfügbar sind (z. B. `args.name` statt Parsing von `args.__ow_query`). Dieser Prozess erfolgt durch eine [Annotation](./openwhisk_annotations.html) `raw-http`. Beim obigen Beispiel wird nun eine "unaufbereitete" HTTP-Webaktion verwendet, die `name` sowohl als Abfrageparameter als auch als JSON-Wert im HTTP-Anforderungshauptteil empfängt:
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}

Beispielausgabe:
```
{
  "response": {
    "__ow_method": "post",
    "__ow_query": "name=Jane",
    "__ow_body": "eyJuYW1lIjoiSmFuZSJ9",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "15",
      "content-type": "application/json",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"      
    },
    "__ow_path": ""
  }
}
```
{: screen}

OpenWhisk verwendet das [Akka Http](http://doc.akka.io/docs/akka-http/current/scala/http/)-Framework, um [festzustellen](http://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html), welche Inhaltstypen binär und welche Typen einfacher Text sind.

### Verarbeitung von unaufbereiteten HTTP-Anforderungen aktivieren

Webaktionen für unaufbereitete HTTP-Anforderungen werden über das Flag `--web` mit dem Wert `raw` aktiviert.
```
ibmcloud wsk action create /guest/demo/hello hello.js --web raw
```
{: pre}

### Verarbeitung von unaufbereiteten HTTP-Anforderungen inaktivieren

Die Verarbeitung von unaufbereiteten HTTP-Anforderungen kann inaktiviert werden, indem der Wert `false` oder `no` an das Flag `--web` übergeben wird.
```
ibmcloud wsk update create /guest/demo/hello hello.js --web false
```
{: pre}

### Binären Hauptteilinhalt aus der Base64-Codierung decodieren

Wenn unaufbereiteter HTTP-Inhalt verarbeitet wird, wird der Inhalt von `__ow_body` in Base64 codiert, wenn der Inhaltstyp (`Content-Type`) der Anforderung binär ist. Die nachfolgenden Beispiele für Funktionen demonstrieren, wie der Hauptteilteilinhalt in Node, Python und Swift decodiert werden kann. Speichern Sie einfach eine Methode in einer Datei, erstellen Sie eine Webaktion mit Verarbeitung von unaufbereiteten HTTP-Anforderungen unter Verwendung des gespeicherten Artefakts und rufen Sie die Webaktion anschließend auf.

#### Node

```javascript
function main(args) {
    decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
    return {body: decoded}
}
```
{: codeblock}

#### Python

```python
def main(args):
    try:
        decoded = args['__ow_body'].decode('base64').strip()
        return {"body": decoded}
    except:
        return {"body": "Could not decode body from Base64."}
```
{: codeblock}

#### Swift

```swift
extension String {
    func base64Decode() -> String? {
        guard let data = Data(base64Encoded: self) else {
            return nil
        }

        return String(data: data, encoding: .utf8)
    }
}

func main(args: [String:Any]) -> [String:Any] {
    if let body = args["__ow_body"] as? String {
        if let decoded = body.base64Decode() {
            return [ "body" : decoded ]
        }
    }

    return ["body": "Could not decode body from Base64."]
}
```
{: codeblock}

Beispiel: Speichern Sie die Node-Funktion in einer Datei mit dem Namen `decode.js` und führen Sie die folgenden Befehle aus:
```
ibmcloud wsk action create decode decode.js --web raw
```
{: pre}

Beispielausgabe:
```
ok: created action decode
```
{: screen}

```
curl -k -H "content-type: application" -X POST -d "Decoded body" https:// openwhisk.ng.bluemix.net/api/v1/web/guest/default/decodeNode.json
```
{: pre}

Beispielausgabe:
```
{
  "body": "Decoded body"
}
```
{: screen}

## Optionsanforderungen
{: #options-requests}

Standardmäßig bewirkt eine Optionsanforderung an eine Webaktion, dass den Antwortheadern automatisch CORS-Header hinzugefügt werden. Diese Header lassen alle Werte für 'origin' sowie die HTTP-Verben OPTIONS, GET, DELETE, POST, PUT, HEAD und PATCH zu.

Betrachten Sie zum Beispiel die folgenden Header:
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

Alternativ dazu können Optionsanforderungen manuell durch eine Webaktion verarbeitet werden. Um dies zu ermöglichen, fügen Sie eine Annotation
`web-custom-options` mit dem Wert `true` einer Webaktion hinzu. Wenn dieses Feature aktiviert ist, werden CORS-Header nicht automatisch zur Anforderungsantwort hinzugefügt. Stattdessen muss der Entwickler dafür sorgen, dass die gewünschten Header programmgesteuert hinzugefügt werden.

Das folgende Beispiel zeigt, wie angepasste Antworten auf Optionsanforderungen erstellt werden:
```js
function main(params) {
  if (params.__ow_method == "options") {
    return {
      headers: {
        'Access-Control-Allow-Methods': 'OPTIONS, GET',
        'Access-Control-Allow-Origin': 'example.com'
      },
      statusCode: 200
    }
  }
}
```
{: codeblock}

Speichern Sie die Funktion in `custom-options.js` und führen Sie die folgenden Befehle aus:
```
ibmcloud wsk action create custom-option custom-options.js --web true -a web-custom-options true
```
{: pre}

```
$ curl https://${APIHOST}/api/v1/web/guest/default/custom-options.http -kvX OPTIONS
```
{: pre}

Beispielausgabe:
```
< HTTP/1.1 200 OK
< Server: nginx/1.11.13
< Content-Length: 0
< Connection: keep-alive
< Access-Control-Allow-Methods: OPTIONS, GET
< Access-Control-Allow-Origin: example.com
```
{: screen}

## Fehlerbehandlung
{: #openwhisk_webactions_errors}

Eine {{site.data.keyword.openwhisk_short}}-Aktion kann in zwei unterschiedlichen möglichen Fehlermodi fehlschlagen. Der erste wird als _Anwendungsfehler_ bezeichnet und entspricht in etwa einer abgefangenen Ausnahmebedingung: Die Aktion gibt ein JSON-Objekt zurück, das eine Eigenschaft `error` auf höchster Ebene enthält. Der zweite Modus ist ein _Entwicklerfehler_, der auftritt, wenn die Aktion einen sehr schwerwiegenden Fehler verursacht und keine Antwort zurückgibt (ähnlich wie bei einer nicht abgefangenen Ausnahmebedingung). Bei Webaktionen behandelt der Controller Anwendungsfehler wie folgt:

- Jede angegebene Pfadprojektion wird ignoriert und der Controller projiziert stattdessen die Eigenschaft `error`.
- Der Controller wendet die Inhaltsbehandlung, die durch die Aktionserweiterung impliziert wird, auf den Wert der Eigenschaft `error` an.

Entwickler müssen wissen, wie Webaktionen möglicherweise verwendet werden, und entsprechende Fehlerantworten generieren. Beispielsweise sollte eine Webaktion, die mit der Erweiterung `.http` verwendet wird, eine HTTP-Antwort wie `{error: { statusCode: 400 }` zurückgeben. Wenn dies nicht so gehandhabt wird, entsteht eine Diskrepanz zwischen dem durch die Erweiterung implizierten Inhaltstyp und dem Aktionsinhaltstyp in der Fehlerantwort. Eine besondere Aufmerksamkeit erfordern Webaktionen, die Sequenzen sind, damit Komponenten, die eine Sequenz bilden, bei Bedarf adäquate Fehlernachrichten generieren können.
