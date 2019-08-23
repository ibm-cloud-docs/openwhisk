---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: web actions, serverless, functions

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


# Webaktionen erstellen
{: #actions_web}

Wenn Sie eine Webaktion erstellen, ist das Ergebnis eine URL, die verwendet werden kann, um die Aktion von einer beliebigen Web-App aus auszulösen.
{: shortdesc}

## Warum ist es sinnvoll, Webaktionen anstelle von Standardaktionen zu verwenden? 

### 1. Webaktionen anonym ausführen

Aktivierungen von Webaktionen sind dem Benutzer zugeordnet, der die Aktion erstellt hat, und nicht dem Aufrufer der Aktion. In der Regel geben Sie für API-Aufrufe von Apps wie GitHub einen Benutzernamen und ein Token mit dem API-Aufruf für einen bestimmten Benutzer oder eine Funktions-ID an. Wenn Sie eine Webaktion verwenden, sind diese Arten von Berechtigungsnachweisen nicht erforderlich. Eine Webaktion ist über eine REST-Schnittstelle zugänglich, ohne dass Berechtigungsnachweise erforderlich sind. 

Die Verwendung von Berechtigungsnachweisen ist bei Webaktionen zwar nicht erforderlich. Sie können jedoch einen eigenen Ablauf für Authentifizierung und Berechtigung (oder OAuth) implementieren. Informationen zum Konfigurieren von Webaktionen mit Berechtigungsnachweisen finden Sie in [Webaktionen schützen](#actions_web_secure). 

### 2. Beliebigen Typ der HTTP-Anforderung verwenden

Standardmäßig akzeptieren Aktionen nur `POST`-Anforderungen. Webaktionen können jedoch über jede der folgenden HTTP-Methoden aufgerufen werden: `GET`, `POST`, `PUT`, `PATCH` und `DELETE` sowie `HEAD` und `OPTIONS`. 

### 3. Webaktion von überall auslösen

Wenn Sie eine {{site.data.keyword.openwhisk}}-Webaktion erstellen, generieren Sie eine URL, um diese Aktion von einer beliebigen webbasierten App aus aufzurufen. Aktionen, bei denen es sich nicht um Webaktionen handelt, erfordern eine Authentifizierung und müssen mit einem JSON-Objekt antworten.  

Ein API-Pfad für Webaktionen kann mit cURL oder `wget` verwendet und sogar direkt in den Browser eingegeben werden. Eine Webaktion kann mit einer URL aufgerufen werden, die wie folgt strukturiert ist: `https://<apihost>/api/v1/web/<namespace>/<packageName>/<actionName><ext>`.

### 4. Weniger {{site.data.keyword.openwhisk_short}}-Entitäten erstellen

Da Sie eine Webaktion standortunabhängig aufrufen können, brauchen Sie keine anderen {{site.data.keyword.openwhisk_short}}-Entitäten wie Auslöser oder Regeln zu erstellen. 

## Wie funktionieren Webaktionen? 

Webaktionen können ohne Authentifizierung aufgerufen werden und lassen sich dazu verwenden, HTTP-Handler zu implementieren, die mit verschiedenartigen Inhalten für die Elemente `headers`, `statusCode code` und `body` antworten.

Webaktionen müssen ein JSON-Objekt zurückgeben. Der Controller behandelt eine Webaktion jedoch anders, wenn das Ergebnis eines oder mehrere der folgenden Elemente als [JSON-Eigenschaften](#web_action_properties) der höchsten Ebene enthält.
{: shortdesc}

## Verfügbare Features von Webaktionen
{: #actions_web_extra}

Webaktionen unterstützen die folgenden Features: 

| Feature | Beschreibung |
| --- | --- |
| [Inhaltserweiterungen](#extra_features) | Sie können einen Inhaltstyp für HTTP-Anforderungen angeben, z. B. `.json`, `.html`, `.http`, `.svg` oder `.text`. Ist kein Inhaltstyp angegeben, wird die Erweiterung `.http` angenommen. Sie können einen Inhaltstyp angeben, indem Sie eine Erweiterung zum Aktionsnamen in der URI hinzufügen, so dass auf eine Aktion `demo/hello` als `/demo/hello.svg` verwiesen wird. Für die Erweiterungen `.json` und `.http` ist kein Projektionspfad erforderlich, während für die Erweiterungen `.html`, `.svg` und `.text` ein solcher Pfad erforderlich ist. Es wird angenommen, dass der Standardpfad mit dem Erweiterungsnamen übereinstimmt. Um eine Webaktion aufzurufen und eine `.html`-Antwort zu empfangen, muss die Aktion mit einem JSON-Objekt antworten, das eine Eigenschaft auf höchster Ebene mit dem Namen `html` enthält (oder der Antworttyp muss im expliziten Pfad enthalten sein). In anderen Worten: `/<namespace>/demo/hello.html` ist zu der expliziten Projizierung der Eigenschaft `html` wie in `/<namespace>/demo/hello.html/html` äquivalent. Der vollständig qualifizierte Name der Aktion muss den entsprechenden Paketnamen enthalten, der `default` lautet, wenn die Aktion nicht in einem benannten Paket enthalten ist. |
| [Felder aus dem Ergebnis projizieren](#projecting_fields) | Der Pfad, der auf den Aktionsnamen folgt, wird dazu verwendet, eine oder mehrere Ebenen der Antwort herauszuprojizieren. Beispiel: `/demo/hello.html/body`. Durch dieses Feature kann jede Webaktion, die ein Wörterverzeichnis wie `{body: "..." }` zurückgibt, die Eigenschaft `body` projizieren und direkt den entsprechenden Zeichenfolgewert anstelle des Wörterverzeichniswerts zurückgeben. Der projizierte Pfad folgt einem absoluten Pfadmodell (wie in XPath). |
| [Abfrage- und Hauptteilparameter als Eingabe](#query_test) | Die Aktion empfängt Abfrageparameter und Parameter im Anforderungshauptteil. Die Rangordnung für das Zusammenfügen von Parametern sieht wie folgt aus: Paketparameter, Aktionsparameter, Abfrageparameter und Hauptteilparameter. Jeder dieser Parameter kann vorherige Werte überschreiben, wenn diese sich überschneiden. Beispiel: `/demo/hello.http?name=Jane` kann das Argument `{name: "Jane"}` an die Aktion übergeben. |
| [Formulardaten](#form_data) | Neben den Standarddaten des Typs `application/json` können Webaktionen URL-codierte Formulardaten des Typs `application/x-www-form-urlencoded data` als Eingabe empfangen.
| [Aktivierung mit mehreren HTTP-Verben](#actions_web_options) | Eine Webaktion kann über jede der folgenden HTTP-Methoden aufgerufen werden: `GET`, `POST`, `PUT`, `PATCH` und `DELETE` sowie `HEAD` und `OPTIONS`. |
| [Verarbeitung von Nicht-JSON-Hauptteilen und unaufbereiteten HTTP-Entitäten](#actions_web_raw_enable) | Eine Webaktion kann einen HTTP-Anforderungshauptteil akzeptieren, der kein JSON-Objekt ist, und solche Werte immer als nicht transparente Werte empfangen (einfacher Text, falls keine binäre Datei, oder andernfalls Zeichenfolge in Base64-Codierung). |

## Webaktion erstellen
{: #actions_web_example}

So erstellen Sie eine Webaktion:  

1. Speichern Sie den folgenden JavaScript-Code als `hello.js`. 

  ```javascript
  function main({name}) {
    var msg = 'You did not tell me who you are.';
  if (name) {
      msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
  }
  ```
  {: codeblock}

2. Erstellen Sie das Paket `demo`. Der Paketname lautet `default`, wenn er nicht explizit angegeben wird. 
  ```
  ibmcloud fn package create demo
  ```
  {: pre}

3. Erstellen Sie die Aktion `hello`. In diesem Beispiel ist `packageName/actionName` `demo/hello`. Ersetzen Sie die Variable `<filepath>` durch den Dateipfad Ihrer Datei `hello.js` und setzen Sie das Flag `--web` auf `true`.  

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

4. Rufen Sie die Webaktion `hello` zum Testen ohne Parameter auf. Ersetzen Sie die Variablen `<apihost>` und `<namespace>`. Zum Abrufen von `<apihost>` führen Sie `ibmcloud fn property get --apihost` aus. Beispiel für `<apihost>`: `us-south.functions.cloud.ibm.com`. 

  Bei Namensbereichen, die für IAM aktiviert sind, ersetzen Sie die Variable `<namespace>` durch die Namensbereichs-ID. Zum Abrufen der ID führen Sie `ibmcloud fn namespace get <namespace_name>` aus.
  {: note}

  a. Sie können die Webaktion mit einer der folgenden Maßnahmen testen:  
    * Öffnen Sie eine URL mit der Struktur `https://<apihost>/api/v1/web/<namespace>/demo/hello` in Ihrem Browser. 
    * Testen Sie die Aktion mit einem cURL-Befehl.
      ```
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

    * Testen Sie die Aktion mit einem `wget`-Befehl.   
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

  b. Der Aktionscode gibt das folgende Wörterverzeichnis zurück.
    ```
    {body: `<html><body><h3>${msg}</h3></body></html>`}
    ``` 
    {: screen}
    
  Sie können die Aktion auch testen, indem Sie mit dem folgenden Befehl nur die Eigenschaft `body` zurückgeben:
  {: #projecting_fields}

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello.html/body
    ```
    {: pre}

    **Beispielausgabe**

    Da der Parameter `<name>` nicht angegeben wurde, wird die folgende Nachricht zurückgegeben.
    ```
    <html><body><h3>You did not tell me who you are.</h3></body></html>
    ```
    {: screen}

5. Versuchen Sie jetzt, den Parameter `<name>` zu definieren. Testen Sie die Aktion mit einem Parameter `<name>`, indem Sie eine der folgenden Maßnahmen ausführen: 
  * Öffnen Sie `https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane` in Ihrem Browser.  
  * Testen Sie die Aktion mit einem cURL-Befehl.
      

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}
  * Testen Sie die Aktion mit einem `wget`-Befehl.   
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}

  **Beispielausgabe**
  ```
  <html><body><h3>Hello, Jane!</h3></body></html>
  ```
  {: screen}


**Nächste Schritte**

Fügen Sie die URL für Ihre Webaktion `hello` zu Ihrer Web-App hinzu und testen Sie sie dort. 

### JSON-Eigenschaften der Webaktion
{: #web_action_properties}

Der standardmäßige Inhaltstyp (`Content-Type`) für eine HTTP-Antwort ist `application/json` und der Hauptteil kann ein beliebiger zulässiger JSON-Wert sein. Wenn Ihr `Content-Type` nicht `application/json`ist, müssen Sie einen `Content-Type` im Headerabschnitt (`headers`) Ihres Aktionscodes angeben. 

Wenn die [Ergebnisgrößenbegrenzung](/docs/openwhisk?topic=cloud-functions-limits) für Aktionen erreicht wird, schlägt die Antwort fehl. Wenn Sie wissen, dass Ihr Aktionsergebnis größer als 5 MB ist, richten Sie einen [Objektspeicher](/docs/openwhisk?topic=cloud-functions-pkg_obstorage) ein. 

| JSON-Eigenschaft | Beschreibung |
| --- | --- |
| `headers`| Ein JSON-Objekt, in dem die Schlüssel Headernamen und die Werte Zeichenfolgewerte, numerische Werte oder boolesche Werte sind. Für das Senden mehrerer Werte für einen einzelnen Header ist der Wert des Headers ein JSON-Array mit den verschiedenen Werten. Standardmäßig sind keine Header festgelegt. |
| `statusCode` | Ein gültiger HTTP-Statuscode. Ist Hauptteilinhalt vorhanden, ist der Standardwert `200 OK`. Ist kein Hauptteilinhalt vorhanden, ist der Standardwert `204 No Content`. |
| `body` | Eine Zeichenfolge, bei der es sich um einfachen Text, ein JSON-Objekt oder -Array oder eine Base64-codierte Zeichenfolge für Binärdaten handelt. Der Hauptteil wird als leer angenommen, wenn er `null`, die leere Zeichenfolge `""` oder nicht definiert ist. Standardmäßig ist der Hauptteil leer. |

Der [Controller](/docs/openwhisk?topic=cloud-functions-about#about_controller) übergibt alle von Aktionen angegebenen Header, Statuscodes oder Hauptteile an den HTTP-Client, der die Anforderung oder Antwort beendet. Wenn der Header `Content-Type` nicht im Element `headers` des Aktionsergebnisses deklariert ist, wird der Hauptteil als `application/json` für Werte, die keine Zeichenfolgen sind, und andernfalls als `text/html` interpretiert. Wenn der Header `Content-Type` definiert ist, bestimmt der Controller, ob die Antwort aus Binärdaten oder einfachem Text besteht, und decodiert die Zeichenfolge bei Bedarf mit einem base64-Decoder. Lässt sich der Hauptteil nicht ordnungsgemäß dekodieren, wird ein Fehler an den Client zurückgegeben.

Der Eigner der Webaktion ist der Eigner aller Aktivierungsdatensätze und trägt die Kosten für die Ausführung der Aktion im System, unabhängig davon, wie die Aktion aufgerufen wurde.
{: note}

#### Geschützte Parameter
Aktionsparameter sind geschützt und können nur durch Aktualisierung der Aktion geändert werden. Parameter werden automatisch als endgültig festgelegt, um Webaktionen zu aktivieren.

```
ibmcloud fn action create /<namespace>/demo/hello hello.js --parameter name Jane --web true
```
{: pre}


Das Ergebnis dieser Änderungen besteht darin, dass das Element `name` an `Jane` gebunden wird und wegen der Annotation 'final' nicht durch Abfrage- oder Hauptteilparameter überschrieben werden kann. Dieses Funktionsweise schützt die Aktion gegen Abfrage- oder Hauptteilparameter, die versehentlich oder absichtlich versuchen, diesen Wert zu ändern.

### Mit einer Webaktion eine HTTP-Umleitung ausführen
{: #http_redirect}
Sie können diese Funktion in einer Webanwendung verwenden, um einen Benutzer auf die neue Version Ihrer Site umzuleiten. 

**Vorbereitende Schritte**
Erstellen Sie das Paket `demo` und die Webaktion `hello`, indem Sie die Schritte in [Webaktion erstellen](#actions_web_example) ausführen. 

So erstellen Sie eine Webaktion, die eine HTTP-Umleitung ausführt: 

1. Speichern Sie den Code als `hello.js`. 

  ```javascript
  function main() {
    return {
      headers: { location: 'https://cloud.ibm.com/openwhisk/' },
      statusCode: 302
    }
  }
  ```
  {: codeblock}

2. Aktualisieren Sie Ihre Webaktion `hello` mit der neuen Version für Ihren `hello.js`-Code. Ersetzen Sie `<filepath>` durch den Dateipfad Ihrer Datei `hello.js`. 

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Testen Sie die Webaktion `hello`. Ersetzen Sie die Variablen `<apihost>` und `<namespace>`. Sie können die Webaktion mit einer der folgenden Maßnahmen testen: 

  * Öffnen Sie die URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` in Ihrem Browser.  
  * Führen Sie den folgenden cURL-Befehl aus:
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * Führen Sie den folgenden `wget`-Befehl aus:
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

  **Beispielergebnis** 
  
  Diese Beispielwebaktion leitet Ihren Browser zum [{{site.data.keyword.openwhisk_short}}-Dashboard](https://cloud.ibm.com/openwhisk/){: external} um. 

### Mit einer Webaktion Cookies setzen
{: #multiple_cookie}
Sie können diese Funktion in einer Webanwendung verwenden, um nach einer erfolgreichen Anmeldung ein JSON-Web-Token als Sitzungscookie zu speichern. 

So erstellen Sie eine Webaktion, die mehrere Cookies setzt: 

**Vorbereitende Schritte**
Erstellen Sie das Paket `demo` und die Webaktion `hello`, indem Sie die Schritte in [Webaktion erstellen](#actions_web_example) ausführen. 

1. Speichern Sie den Code als `hello.js`. 
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

2. Aktualisieren Sie Ihre Webaktion `hello` mit der neuen Version für Ihren `hello.js`-Code. Ersetzen Sie `<filepath>` durch den Dateipfad Ihrer Datei `hello.js`. 

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}
    
3. Löschen Sie die Cookies Ihres Browsers, bevor Sie die Aktion testen. 

4. Testen Sie die Webaktion `hello`, indem Sie die URL in Ihrem Browser öffnen. Ersetzen Sie die Variablen `<apihost>` und `<namespace>` und öffnen Sie `https://<apihost>/api/v1/web/<namespace>/demo/hello`. Beispiel für `<apihost>`: `us-south.functions.cloud.ibm.com`. 

**Ergebnis**

Die Cookies `UserID=Jane` und `SessionID=asdfgh123456` sind in den Entwicklertools Ihres Browsers definiert. 


### Mit einer Webaktion ein Bild zurückgeben
{: #return_image}
Sie können diese Funktion in einer Webanwendung verwenden, um das Bild einer Nationalflagge auf der Basis der Ländereinstellung des Benutzers zurückzugeben. 

**Vorbereitende Schritte** 

Erstellen Sie das Paket `demo` und die Webaktion `hello`, indem Sie die Schritte in [Webaktion erstellen](#actions_web_example) ausführen. 

So erstellen Sie eine Webaktion, die ein `image/png` zurückgibt:  

1. Speichern Sie den Code als `hello.js`. 

  ```javascript
  function main() {
      let png = '<base 64 encoded string';
      return { headers: { 'Content-Type': 'image/png' },
              statusCode: 200,
              body: png };
  }
  ```
  {: codeblock}

2. Aktualisieren Sie Ihre Webaktion `hello` mit der neuen Version für Ihren `hello.js`-Code. Ersetzen Sie `<filepath>` durch den Dateipfad Ihrer Datei `hello.js`. 

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Testen Sie die Aktion in Ihrem Browser oder mit einem cURL-Befehl. Ersetzen Sie die Variablen `<apihost>` und `<namespace>`. Sie können die Webaktion mit einer der folgenden Maßnahmen testen: 

  * Öffnen Sie die URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` in Ihrem Browser.  
  * Führen Sie den folgenden cURL-Befehl aus.
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * Führen Sie den folgenden `wget`-Befehl aus.
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}



### Mit einer Webaktion JSON zurückgeben
{: #return_json}
Sie können diese Funktion in einer Webanwendung verwenden, um ein JSON-Objekt mit den Benutzer-IP-Informationen zurückzugeben. 

**Vorbereitende Schritte** 

Erstellen Sie das Paket `demo` und die Webaktion `hello`, indem Sie die Schritte in [Webaktion erstellen](#actions_web_example) ausführen. 

So erstellen eine Webaktion, die `application/json` zurückgibt: 

1. Speichern Sie den Code als `hello.js`. 
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

2. Aktualisieren Sie Ihre Webaktion `hello` mit der neuen Version für Ihren `hello.js`-Code. Ersetzen Sie `<filepath>` durch den Dateipfad Ihrer Datei `hello.js`. 
  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Testen Sie die Aktion in Ihrem Browser oder mit einem cURL-Befehl. Ersetzen Sie die Variablen `<apihost>` und `<namespace>`. Sie können die Webaktion mit einer der folgenden Maßnahmen testen: 
  * Öffnen Sie die URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` in Ihrem Browser.  
  * Führen Sie den folgenden cURL-Befehl aus:
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * Führen Sie den folgenden `wget`-Befehl aus:
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

    **Beispielausgabe**

    ```
    {
      "__ow_headers": {
        "accept": "*/*",
        "accept-encoding": "gzip",
        "cdn-loop": "cloudflare",
        "cf-connecting-ip": "XX.XXX.XXX.XXX",
        "cf-ipcountry": "US",
        "cf-ray": "4d9f3e442a86cf28-IAD",
        "cf-visitor": "{\"scheme\":\"https\"}",
        "host": "<apihost>",
        "user-agent": "curl/7.54.0",
        "x-forwarded-for": "XX.XXX.XX.XXX, XX.XXX.XX.XXX",
        "x-forwarded-host": "<apihost>",
        "x-forwarded-port": "443",
        "x-forwarded-proto": "https",
        "x-global-k8fdic-transaction-id": "11fd03071bd0841d3a00f52354ab880f",
        "x-real-ip": "XXX.XX.XX.XX",
        "x-request-id": "11fd03071bd0841d3a00f52354ab880f"
      },
      "__ow_method": "get",
      "__ow_path": ""
    }
    ```
    {: screen}


### HTTP-Kontext
{: #actions_web_context}

Alle Webaktionen empfangen bei ihrem Aufruf HTTP-Anforderungsdetails als Eingabeparameter für das Aktionsargument. 

| HTTP-Parameter | Typ | Beschreibung |
| --- | --- | --- |
| `__ow_method` | Zeichenfolge | Die HTTP-Methode der Anforderung. |
| `__ow_headers` | Zuordnung Zeichenfolge zu Zeichenfolge | Die Anforderungsheader. |
| `__ow_path` | Zeichenfolge | Der nicht abgeglichene Pfad der Anforderung (der Abgleich wird nach Verarbeitung der Aktionserweiterung beendet). |
| `__ow_user` | Zeichenfolge | Der Namensbereich, der das von {{site.data.keyword.openwhisk_short}} authentifizierte Subjekt identifiziert. |
| `__ow_body` | Zeichenfolge | Die Entität des Anforderungshauptteils als Zeichenfolge in Base64-Codierung, wenn der Inhalt eine binäre Datei ist, oder andernfalls als normale Zeichenfolge. |
| `__ow_query` | Zeichenfolge | Die Abfrageparameter aus der Anforderung als nicht geparste Zeichenfolge. |

Eine Anforderung kann keine der benannten `__ow_`-Parameter überschreiben. Falls dies doch geschieht, schlägt die Anforderung mit dem Status 400 "Bad Request" fehl.

Die Eigenschaft `__ow_user` ist nur vorhanden, wenn die Webaktion eine [Annotation für eine erforderliche Authentifizierung](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions) besitzt, und ermöglicht einer Webaktion die Implementierung ihrer eigenen Berechtigungsrichtlinie. Die Eigenschaft `__ow_query` ist nur dann verfügbar, wenn eine Webaktion die Verarbeitung der ["unaufbereiteten" HTTP-Anforderung](#actions_web_raw_enable) wählt. Die Zeichenfolge `__ow_query` enthält die aus dem URI geparsten Abfrageparameter (getrennt durch `&`). Die Eigenschaft `__ow_body` ist vorhanden, wenn es sich um eine unaufbereitete HTTP-Anforderung handelt oder wenn die HTTP-Anforderung weder ein JSON-Objekt noch Formulardaten ist. Andernfalls empfangen Webaktionen Abfrage- und Hauptteilparameter als Eigenschaften erster Klasse im Aktionsargument. Hauptteilparameter haben Vorrang vor Abfrageparametern, die wiederum Vorrang vor Aktions- und Paketparametern haben.

### HTTPS-Endpunktunterstützung
{: #actions_web_endpoint}

Unterstützte SSL-Protokolle: TLS 1.2, TLS 1.3 ([Entwurfsversion 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18){: external})

### Antwortinhalt der Webaktion ändern
{: #extra_features}
Sie können den Antwortinhalt einer Webaktion ändern, um mit [Inhaltserweiterungen](#actions_web_extra) verschiedene Inhaltstypen zurückzugeben.
{: shortdesc}

**Vorbereitende Schritte**

Erstellen Sie das Paket `demo` und die Webaktion `hello`, indem Sie die Schritte in [Webaktion erstellen](#actions_web_example) ausführen. 

So ändern Sie die Antwort einer Webaktion: 

1. Speichern Sie den folgenden Code als `hello.js`. 

  ```javascript
  function main(params) {
      return { response: params };
}
  ```
  {: codeblock}

2. Aktualisieren Sie Ihre Webaktion `hello` mit der neuen Version Ihres `hello.js`-Codes. Ersetzen Sie `<filepath>` durch den Dateipfad Ihrer Datei `hello.js`. 

  ```bash
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Testen Sie die Aktion in Ihrem Browser oder mit einem cURL-Befehl. Ersetzen Sie die Variablen `<apihost>` und `<namespace>`. 

  a. Geben Sie JSON zurück, indem Sie eine der folgenden Maßnahmen ausführen: 
    * Öffnen Sie `https://<apihost>/api/v1/web/<namespace>/demo/hello.json` in Ihrem Web-Browser.  
    * Führen Sie den folgenden cURL-Befehl aus.
    ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}
    * Führen Sie den folgenden `wget`-Befehl aus.
    ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}

      **Beispielausgabe**

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

  b. Testen Sie die Aktion mit einem Abfrageparameter. Sie können die Aktion mit einer der folgenden Maßnahmen testen:
  {: #query_test}

    * Führen Sie den folgenden cURL-Befehl aus.
    

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

    * Führen Sie den folgenden `wget`-Befehl aus.
    

        ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

      **Beispielausgabe**
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

  c. Sie können die Webaktion auch mit Formulardaten testen. Sie können die Webaktion mit einer der folgenden Maßnahmen testen: {: #form_data}
  
    * Führen Sie den folgenden cURL-Befehl aus.
    

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}
      
    * Führen Sie den folgenden `wget`-Befehl aus.
    ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}

      **Beispielausgabe**

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

  d. Sie können ein JSON-Objekt angeben, indem Sie den folgenden Befehl ausführen. Sie können die Webaktion mit einer der folgenden Maßnahmen testen: 
    * Führen Sie den folgenden cURL-Befehl aus.
    ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}
      
    * Führen Sie den folgenden `wget`-Befehl aus.
    ```
        wget https://<apihost>/api/v1/web/{namespace/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}

      **Beispielausgabe**

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

  e. Sie können auch den Wert für `name` als Text zurückgeben. Führen Sie dazu eine der folgenden Maßnahmen aus: 
  * Führen Sie den folgenden cURL-Befehl aus.
    

      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}
  * Führen Sie den folgenden `wget`-Befehl aus.
    ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}

    **Beispielausgabe**

    ```
    Jane
    ```
    {: screen}

    Bei Standardaktionen werden Abfrageparameter und Formulardaten sowie Hauptteilentitäten für JSON-Objekte als Wörterverzeichnisse behandelt und ihre Werte sind direkt als Aktionseingabeeigenschaften zugänglich. Dieses Verhalten gilt nicht für Webaktionen, die HTTP-Anforderungsentitäten verarbeiten, oder wenn die Webaktion eine Entität empfängt, die kein JSON-Objekt ist.
    {: note}

  f. Sie können den Inhaltstyp (`Content-Type`) mit einer der folgenden Maßnahmen definieren. 
  * Führen Sie den folgenden cURL-Befehl aus.
      
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}
    
  * Führen Sie den folgenden `wget`-Befehl aus.
    ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}

    **Beispielausgabe**

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

## Webaktionen schützen
{: #actions_web_secure}

**Vorbereitende Schritte**
Erstellen Sie das Paket `demo` und die Webaktion `hello`, indem Sie die Schritte in [Webaktion erstellen](#actions_web_example) ausführen. 

Standardmäßig kann jeder Benutzer eine Webaktion mit der Aufruf-URL aufrufen. Mit der [Webaktionsannotation](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions) `require-whisk-auth` können Sie die Webaktion schützen, indem Sie eine der folgenden Maßnahmen ausführen: 
  1. Setzen sie die Annotation `require-whisk-auth` auf `true`. Wenn die Annotation `require-whisk-auth` auf `true` gesetzt ist, authentifiziert die Webaktion die Berechtigungsnachweise für die Basisberechtigung der Aufrufanforderung anhand des Whisk-Authentifizierungsschlüssels des Webaktionseigners. Bei Festlegung auf eine Zahl oder eine Zeichenfolge, bei der die Groß-/Kleinschreibung beachtet werden muss, muss der Header `X-Require-Whisk-Auth` der Aufrufanforderung der Webaktion auf dieselbe Zahl oder dieselbe Zeichenfolge mit identischer Groß-/Kleinschreibung gesetzt sein. Geschützte Webaktionen geben die Nachricht `Not Authorized` zurück, wenn die Validierung der Berechtigungsnachweise fehlschlägt. 

  2. Lassen Sie zu, dass die Annotation `require-whisk-auth` automatisch gesetzt wird, indem Sie das Flag `--web-secure` verwenden. Wenn das Flag `--web-secure` auf `true` gesetzt ist, wird eine Zufallszahl als Wert der Annotation `require-whisk-auth` generiert. Bei der Einstellung `false` wird die Annotation `require-whisk-auth` entfernt.  Bei Einstellung auf einen anderen Wert wird dieser Wert als Annotationswert `require-whisk-auth` verwendet.

So testen Sie eine geschützte Webaktion: 

1. Speichern Sie den folgenden JavaScript-Code als `hello.js`. 
  ```javascript
  function main({name}) {
    var msg = 'You did not tell me who you are.';
  if (name) {
      msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
  }
  ```
  {: codeblock}

2. Aktualisieren Sie Ihre Webaktion `hello` mit der neuen Version Ihres `hello.js`-Codes und setzen Sie das Flag `--web secure` auf `true`. 
  ```bash
  ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true
  ```
  {: pre}

3. Rufen Sie die Webaktion `hello` ab, um den zufällig generierten Wert für `require-whisk-auth` anzuzeigen. 

  ```bash
  ibmcloud fn action get demo/hello
  ```
  {: pre}

    **Beispielausgabe**

    Der Wert für `require-whisk-auth` wurde auf `7819991076995522` gesetzt.
    ```
    {
      "namespace": "<namespace>/demo",
      "name": "hello",
      "version": "0.0.34",
      "exec": {
          "kind": "nodejs:10",
          "binary": false
      },
      "annotations": [
          {
              "key": "web-export",
              "value": true
          },
          {
              "key": "raw-http",
              "value": false
          },
          {
              "key": "final",
              "value": true
          },
          {
              "key": "require-whisk-auth",
              "value": 7819991076995522
          },
          {
              "key": "exec",
              "value": "nodejs:10"
          }
      ],
    "limits": {
          "timeout": 60000,
          "memory": 256,
          "logs": 10,
          "concurrency": 1
      },
    "publish": false
}
    ```
    {: screen}

So testen Sie, ob die Authentifizierung funktioniert: 

1. Testen Sie die Webaktion `hello`, ohne den Parameter `X-Require-Whisk-Auth` zu setzen, um zu prüfen, ob die Authentifizierung erforderlich ist. Dieser Test führt zu einem Fehler. Sie können die Webaktion mit einer der folgenden Maßnahmen testen: 

  * Testen Sie die Webaktion mit einem cURL-Befehl.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}
    
  * Testen Sie die Webaktion mit einem `wget`-Befehl.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}

   **Beispielausgabe**

    ```
      {
      "code": "4c4423556547f6ac764ee192d4ed27a6",
      "error": "Authentication is possible but has failed or not yet been provided."
    }
    ```
    {: screen}

    Der Aufruf schlägt fehl, weil der Wert für `X-Require-Whisk-Auth` nicht bereitgestellt wurde.
    {: note}

2. Testen Sie nun die Webaktion `hello` und stellen Sie den zufällig generierten Wert für `X-Require-Whisk-Auth` bereit. Ersetzen Sie die Werte für `<apihost>` und `<namespace>`. Ersetzen Sie den Wert für `<my-secret>` durch die zufällig generierte Zahl, die Sie in Schritt 3 erstellt haben. Sie können die Webaktion mit einer der folgenden Maßnahmen testen: 
  * Testen Sie die Webaktion mit einem cURL-Befehl.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  * Testen Sie die Webaktion mit einem `wget`-Befehl.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  **Beispielausgabe**
    
    ```
    {
    "body": "<html><body><h3>Hello, Jane!</h3></body></html>"
    }
    ```
    {: screen}

So testen Sie eine Webaktion mit einem angepassten Wert für `require-whisk-auth`: 

1. Aktualisieren Sie Ihre Webaktion `hello` mit Ihrem eigenen Wert für `require-whisk-auth`. Versuchen Sie anschließend, Ihre Webaktion zu testen, indem Sie den Wert für `X-Require-Whisk-Auth` beim Aufruf angeben. 

  a. Legen Sie einen Wert für `require-whisk-auth` fest, wobei `<my-secret>` Ihr Authentifizierungstoken ist, bei dem die Groß-/Kleinschreibung beachtet werden muss.
    ```bash
    ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true --require-whisk-auth <mysecret>
    ```
    {: pre}
  
  b. Testen Sie die Webaktion und geben Sie dabei Ihren Wert für `<my-secret>` ein. Sie können die Webaktion mit einer der folgenden Maßnahmen testen: 
  * Testen Sie die Webaktion mit einem cURL-Befehl.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}
  * Testen Sie die Aktion mit einem `wget`-Befehl.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}


## Unaufbereitete HTTP-Anforderungen verarbeiten
{: #actions_web_raw}

Eine Webaktion kann einen eingehenden HTTP-Hauptteil direkt interpretieren und verarbeiten, ohne Weiterleitung eines JSON-Objekts an Eigenschaften erster Klasse, die für die Aktionseingabe verfügbar sind (z. B. `args.name` statt Parsing von `args.__ow_query`). Dieser Prozess erfolgt durch eine [Annotation](/docs/openwhisk?topic=cloud-functions-annotations) `raw-http`. Beim obigen Beispiel wird nun eine "unaufbereitete" HTTP-Webaktion verwendet, die `name` sowohl als Abfrageparameter als auch als JSON-Wert im HTTP-Anforderungshauptteil empfängt:
```bash
curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}


**Beispielausgabe**
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

{{site.data.keyword.openwhisk_short}} nutzt das Framework [Akka HTTP](https://doc.akka.io/docs/akka-http/current/?language=scala){: external}, um zu [ermitteln, welche Inhaltstypen binäre Dateien und welche einfacher Text sind](https://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html){: external}. 

### Verarbeitung von unaufbereiteten HTTP-Anforderungen aktivieren
{: #actions_web_raw_enable}

Sie können unaufbereitete HTTP-Webaktionen erstellen, indem Sie `--web` auf `raw` setzen. 
```bash
ibmcloud fn action create demo/hello /<filepath>/hello.js --web raw
```
{: pre}

### Binären Hauptteilinhalt aus der Base64-Codierung decodieren
{: #actions_web_decode}

Bei der Verarbeitung von unaufbereitetem HTTP-Inhalt wird der Inhalt von `__ow_body` in Base64-Codierung codiert, wenn der Inhaltstyp (`Content-Type`) der Anforderung der binäre Typ ist. Die nachfolgenden Beispiele für Funktionen demonstrieren, wie der Hauptteilteilinhalt in Node, Python und Swift decodiert werden kann.

1. Speichern Sie den Beispielcode in Ihrer bevorzugten Sprache in einer Datei mit dem Namen `decode.<ext>`. Ersetzen Sie `<ext>` durch die Dateierweiterung des Beispielcodes Ihrer bevorzugten Sprache. 

  **Node**
  {: #actions_web_decode_js}

  ```javascript
  function main(args) {
      decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
      return {body: decoded}
  }
  ```
  {: codeblock}

  **Python**
  {: #actions_web_decode_python}

  ```python
  def main(args):
    try:
        decoded = args['__ow_body'].decode('base64').strip()
        return {"body": decoded}
    except:
        return {"body": "Could not decode body from Base64."}
  ```
  {: codeblock}

  **Swift**
  {: #actions_web_decode_swift}

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

2. Erstellen Sie mit dem Beispielcode eine unaufbereitete HTTP-Webaktion, indem Sie den folgenden Befehl ausführen. In diesem Beispiel wird die Node-Funktion als `decode.js` gespeichert. Ersetzen Sie den Dateipfad durch den Dateipfad Ihrer `decode`-Datei und aktualisieren Sie die Dateierweiterung so, dass sie mit der Erweiterung des verwendeten Beispielcodes übereinstimmt. 

  ```bash
  ibmcloud fn action create decode <filepath>/decode.js --web raw
  ```
  {: pre}

  **Beispielausgabe**
  ```
  ok: created action decode
  ```
  {: screen}

3. Testen Sie die Aktion `decode`, indem Sie den folgenden cURL-Befehl ausführen.
    ```bash
    curl -k -H "content-type: application" -X POST -d "Decoded body" https://<apihost>/api/v1/web/<namespace>/default/decode.json
    ```
    {: pre}

  **Beispielausgabe**
    ```
    {
      "body": "Decoded body"
    }
    ```
    {: screen}

## Options-Anforderungen
{: #actions_web_options}

Standardmäßig bewirkt eine `OPTIONS`-Anforderung an eine Webaktion, dass den Antwortheadern automatisch CORS-Header hinzugefügt werden. Diese Header lassen alle Ursprünge sowie die HTTP-Verben `OPTIONS`, `GET`, `DELETE`, `POST`, `PUT`, `HEAD` und `PATCH` zu.
{: shortdesc}

Betrachten Sie zum Beispiel die folgenden Header:
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

Alternativ können `OPTIONS`-Anforderungen manuell durch eine Webaktion verarbeitet werden. Um dies zu ermöglichen, fügen Sie eine Annotation
`web-custom-options` mit dem Wert `true` einer Webaktion hinzu. Wenn dieses Feature aktiviert ist, werden CORS-Header nicht automatisch zur Anforderungsantwort hinzugefügt. Stattdessen müssen Sie Header programmgesteuert anhängen. 

So erstellen Sie angepasste Antworten für `OPTIONS`-Anforderungen: 

1. Speichern Sie den folgenden Code in einer Datei mit dem Namen `custom-options.js`. 

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

2. Erstellen Sie die Webaktion. Setzen Sie `--web-custom-options` auf `true`. 

  ```bash
  ibmcloud fn action create custom-option <filepath>/custom-options.js --web true -a web-custom-options true
  ```
  {: pre}

3. Testen Sie die Aktion mit dem folgenden cURL-Befehl. 

  ```bash
  $ curl https://<apihost>/api/v1/web/<namespace>/default/custom-option.http -kvX OPTIONS
  ```
  {: pre}

  **Beispielausgabe**
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
{: #actions_web_errors}

Eine {{site.data.keyword.openwhisk_short}}-Aktion kann in zwei unterschiedlichen möglichen Fehlermodi fehlschlagen. Der erste wird als _Anwendungsfehler_ bezeichnet und entspricht in etwa einer abgefangenen Ausnahmebedingung: Die Aktion gibt ein JSON-Objekt zurück, das eine Eigenschaft `error` auf höchster Ebene enthält. Der zweite Modus ist ein _Entwicklerfehler_, der auftritt, wenn die Aktion fehlschlägt und keine Antwort generiert (ähnlich wie bei einer nicht abgefangenen Ausnahmebedingung). Bei Webaktionen behandelt der Controller Anwendungsfehler wie folgt:

- Jede angegebene Pfadprojektion wird ignoriert und der Controller projiziert stattdessen die Eigenschaft `error`.
- Der Controller wendet die Inhaltsbehandlung, die durch die Aktionserweiterung impliziert wird, auf den Wert der Eigenschaft `error` an.

Entwickler müssen wissen, wie Webaktionen möglicherweise verwendet werden, und entsprechende Fehlerantworten generieren. Beispielsweise sollte eine Webaktion, die mit der Erweiterung `.http` verwendet wird, eine HTTP-Antwort wie `{error: { statusCode: 400 }` zurückgeben. Wenn dies nicht so gehandhabt wird, entsteht eine Diskrepanz zwischen dem durch die Erweiterung implizierten Inhaltstyp und dem Aktionsinhaltstyp in der Fehlerantwort. Eine besondere Aufmerksamkeit erfordern Webaktionen, die Sequenzen sind, damit Komponenten, die eine Sequenz bilden, bei Bedarf adäquate Fehlernachrichten generieren können.



## Webaktionen inaktivieren
{: #actions_web_disable}

Sie können eine Webaktion inaktivieren, indem Sie das Flag `-- web` in der Befehlszeilenschnittstelle auf `false` oder `no` setzen. Ersetzen Sie `<packageName>/<actionName>` und `<filepath>/<filename>` durch den Paketnamen, den Namen der Webaktion, den Dateipfad und den Dateinamen Ihrer Codedatei. 

```bash
ibmcloud fn action update <packageName>/<actionName> <filepath>/<filename> --web false
```
{: pre}



