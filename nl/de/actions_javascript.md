---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-27"

keywords: actions, serverless, javascript, node, node.js

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


# JavaScript-Aktionen erstellen
{: #actions_javascript
{: #creating-and-invoking-javascript-actions}

In den folgenden Abschnitten werden Sie in die Arbeit mit Aktionen in JavaScript eingeführt. Sie beginnen zunächst mit dem Erstellen und Aufrufen einer einfachen Aktion. Danach fügen Sie Parameter zu der Aktion hinzu und rufen diese Aktion dann mit Parametern auf. Anschließend legen Sie Standardparameter fest und rufen diese Parameter auf. Zum Abschluss erstellen Sie asynchrone Aktionen.
{: shortdesc}

## Einfache JavaScript-Aktion erstellen und aufrufen
{: #single_action_js}
{: #openwhisk_single_action_js}

Sehen Sie sich die folgenden Schritte und Beispiele an, um Ihre erste JavaScript-Aktion zu erstellen.

1. Speichern Sie den folgenden Code in einer Datei namens `hello.js`.

  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  Die JavaScript-Datei könnte noch weitere Funktionen enthalten. Allerdings muss nach allgemeiner Konvention eine Funktion mit dem Namen `main` vorhanden sein, um den Einstiegspunkt für die Aktion bereitzustellen.
  {: shortdesc}

2. Erstellen Sie mit der JavaScript-Funktion eine Aktion namens `hello`.

  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  Beispielausgabe:
  
  ```
  ok: created action hello
  ```
  {: screen}

  Der Typ der Aktion wird durch die Verwendung der Erweiterung der Quellendatei bestimmt. Für `.js`-Quellendateien wird die Aktion in einer Laufzeit mit Node.js ausgeführt. Sie können die Node.js-Laufzeitversion für Ihre JavaScript-Aktion angeben, indem Sie den Parameter `--kind` auf `nodejs:10` oder `nodejs:8` setzen. Weitere Informationen zur Node.js-Laufzeit finden Sie unter [Laufzeiten](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments).
  {: shortdesc}

3. Überprüfen Sie, ob Ihre Aktion namens `hello` in der Aktionsliste aufgeführt wird.

  ```
  ibmcloud fn action list
  ```
  {: pre}

  Beispielausgabe:
  
  ```
  actions
  hello       private
  ```
  {: screen}

4. Führen Sie die Aktion durch Ausführen eines blockierenden Aufrufs aus. Blockierende Aufrufe verwenden einen Typ von Anforderung/Antwort und warten, bis das Aktivierungsergebnis verfügbar ist. Der Wartezeitraum beträgt weniger als 60 Sekunden oder das [Zeitlimit](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits) der Aktion, je nachdem, welcher Wert kürzer ist.

    ```
    ibmcloud fn action invoke --blocking hello
    ```
    {: pre}

    Der Befehl gibt die folgenden Informationen aus:
        * Die Aktivierungs-ID (`44794bd6aab74415b4e42a308d880e5b`), mit der die Protokolle oder das Ergebnis der Aktivierung abgerufen werden kann
        * Das Aufrufergebnis, wenn es innerhalb des erwarteten Wartezeitraums verfügbar ist

    ```
    ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

    {
        "result": {
            "payload": "Hello world"
        },
      "status": "success",
      "success": true
  }
    ```
    {: screen}
    
    Tipps zur Überwachung von Aktivierungen enthält der Abschnitt [Aktionsausgabe überwachen](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#monitor-action-output).
    {: tip}

5. Wenn Sie das Aktionsergebnis nicht sofort benötigen, können Sie das Flag `--blocking` weglassen und so einen nicht blockierenden Aufruf ausführen.

    1. Führen Sie einen nicht blockierenden Aufruf aus.

        ```
        ibmcloud fn action invoke hello
        ```
        {: pre}

        Beispielausgabe:

        ```
        ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: screen}

    2. Rufen Sie das Aktionsergebnis mithilfe der Aktivierungs-ID ab.

        ```
        ibmcloud fn activation result 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: pre}

        Das Aktionsergebnis wird zurückgegeben:

        ```
        {
            "payload": "Hello world"
  }
        ```
        {: screen}

6. Wenn Sie vergessen, die Aktivierungs-ID zu notieren, können Sie eine Liste der Aktivierungen in der Reihenfolge der jüngsten bis zur ältesten abrufen.

    ```
    ibmcloud fn activation list
    ```
    {: pre}

    Ausgabe:
    ```
    activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
    ```
    {: screen}
    
### JavaScript-Funktionsprototyp
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

## Asynchrone Aktionen erstellen
{: #asynchronous_javascript}
{: #openwhisk_asynchrony_js}

JavaScript-Funktionen, die asynchron ausgeführt werden, können das Aktivierungsergebnis zurückgeben, nachdem die Funktion `main` die Steuerung zurückgegeben hat, indem sie ein Promise-Objekt in Ihrer Aktion zurückgeben.

1. Speichern Sie den folgenden Code in einer Datei namens `asyncAction.js`.

    ```javascript
    function main(args) {
         return new Promise(function(resolve, reject) {
           setTimeout(function() {
             resolve({ done: true });
         }, 2000);
      })
     }
    ```
    {: codeblock}

    * Die Funktion `main` gibt ein Promise-Objekt zurück. Dieses Promise-Objekt weist darauf hin, dass die Aktivierung zwar noch nicht abgeschlossen wurde, ihr Abschluss erwartet wird.

    * Die JavaScript-Funktion `setTimeout()` wartet 2 Sekunden, bevor sie die Callback-Funktion des Promise-Objekts aufruft, die den asynchronen Code darstellt.

    * Die Callback-Funktion des Promise-Objekts akzeptiert die Argumente `resolve` und `reject`, die jeweils Funktionen sind.

      * Der Aufruf von `resolve()` erfüllt das Promise-Objekt und weist darauf hin, dass die Aktivierung normal abgeschlossen wird.
      * Mit einem Aufruf von `reject()` kann das Promise-Objekt zurückgewiesen und signalisiert werden, dass die Aktivierung abnormal beendet wird.

2. Erstellen Sie eine Aktion namens `asyncAction`.
    ```
    ibmcloud fn action create asyncAction asyncAction.js --kind nodejs:10
    ```
    {: pre}

3. Rufen Sie die Aktion auf.

    ```
    ibmcloud fn action invoke --result asyncAction
    ```
    {: pre}

    Beispielausgabe:

    ```
    {
        "done": true
  }
    ```
    {: screen}

4. Prüfen Sie, wie lange die Ausführung der Aktivierung bis zu ihrem Abschluss gedauert hat. Rufen Sie dazu das Aktivierungsprotokoll ab.

  1. Rufen Sie die Aktivierungs-ID ab.

      ```
      ibmcloud fn activation list --limit 1 asyncAction
      ```
      {: pre}

      Beispielausgabe:
      ```
      activations
  b066ca51e68c4d3382df2d8033265db0             asyncAction
      ```
      {: screen}

  2. Rufen Sie das Protokoll für die Aktivierungs-ID ab.

      ```
      ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
      ```
      {: pre}

      Die Angabe bei `duration` zeigt an, dass für die vollständige Ausführung dieser Aktivierung etwas mehr als 2 Sekunden benötigt wurden:

      ```
      ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
          ...
          "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
          ...
      }
      ```
      {: screen}

## Externe API mit Aktionen aufrufen
{: #apicall_action}
{: #openwhisk_apicall_action}

Die bisherigen Beispiele sind eigenständige JavaScript-Funktionen. Sie können jedoch auch eine Aktion erstellen, die eine externe API aufruft.
{: shortdesc}

Das folgende Beispiel ruft den APOD-Service der NASA auf (APOD = Astronomy Picture of the Day), der täglich ein Bild des Universums zur Verfügung stellt.

1. Speichern Sie den folgenden Code in einer Datei namens `apod.js`.

    ```javascript
    let rp = require('request-promise')

    function main(params) {
        const options = {
            uri: "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo",
            json: true
        }
        return rp(options)
        .then(res => {
            return { response: res }
        })
    }
    ```
    {: codeblock}

2. Es wird ein Aufruf an die APOD-API der NASA durchgeführt und aus dem JSON-Ergebnis werden Felder extrahiert. Weitere Informationen dazu, welche Node.js-Pakete Sie in Ihren Aktionen verwenden können, finden Sie unter [Laufzeiten](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments). 

3. Erstellen Sie eine Aktion namens `apod`.
    ```
    ibmcloud fn action create apod apod.js --kind nodejs:10
    ```
    {: pre}

3. Rufen Sie die Aktion `apod` auf.
    ```
    ibmcloud fn action invoke --result apod
    ```
    {: pre}

    Es wird das folgende Beispielobjekt zurückgegeben:

    ```
    {
      "copyright": "Eric Houck",
    "date": "2018-03-28",
    "explanation": "Does an alignment like this occur only once in a blue moon? ...",
    "hdurl": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_1799.jpg",
    "media_type": "image",
    "service_version": "v1",
    "title": "Blue Moon Tree",
    "url": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_960.jpg"
  }
    ```
    {: screen}

## Aktion als Node.js-Modul packen
{: #packaging_javascript_actions}
{: #openwhisk_js_packaged_action}

Als Alternative zum Schreiben des gesamten Aktionscodes in einer einzigen JavaScript-Quellendatei können Sie eine Aktion als `npm`-Paket schreiben.

Stellen Sie sich zum Beispiel ein Verzeichnis mit den folgenden Dateien vor:

1. Speichern Sie den folgenden Code in einer Datei namens `package.json`.

    ```json
    {
      "name": "my-action",
  "main": "index.js",
  "dependencies" : {
        "left-pad" : "1.1.3"
      }
    }
    ```
    {: codeblock}

2. Speichern Sie den folgenden Code in einer Datei namens `index.js`.

    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    exports.main = myAction;
    ```
    {: codeblock}

    * Die Aktion wird durch `exports.main` verfügbar gemacht.
    * Der Aktionshandler kann einen beliebigen Namen haben, solange dieser der üblichen Signatur für die Annahme und die Rückgabe eines Objekts (oder eines `Promise`-Objekts) entspricht.
    * Diese Datei muss entweder den Namen `index.js` erhalten oder Sie müssen den für diese Datei bevorzugten Dateinamen in `package.json` in der Eigenschaft `main` angeben.

3. Installieren Sie alle Abhängigkeiten lokal.

    ```
    npm install
    ```
    {: pre}

    **Hinweis**: Beachten Sie, dass die meisten `npm`-Pakete zwar JavaScript-Quellen mit `npm install` installieren, andere jedoch auch Binärartefakte installieren und kompilieren. Der Upload von Archivdateien unterstützt nur JavaScript-Abhängigkeiten. Wenn das Archiv binäre Abhängigkeiten enthält, können Aktionsaufrufe fehlschlagen.

4. Erstellen Sie ein `.zip`-Archiv, das alle Dateien (einschließlich aller Abhängigkeiten) enthält.

    ```
    zip -r action.zip *
    ```
    {: pre}

    Die Verwendung der Windows Explorer-Aktion zur Erstellung der ZIP-Datei führt zu einer falschen Struktur. {{site.data.keyword.openwhisk_short}}-ZIP-Aktionen müssen `package.json` am Stammelement der ZIP-Datei aufweisen, während Windows Explorer die Datei in einem verschachtelten Ordner ablegt. Am sichersten ist die Verwendung des Befehlszeilenbefehls `zip`.
    {: tip}

5. Erstellen Sie die Aktion. Wenn Sie eine Aktion aus einem `.zip`-Archiv erstellen, müssen Sie einen Wert für den Parameter `--kind` festlegen, um Ihre Node.js-Laufzeitversion anzugeben. Entscheiden Sie sich zwischen `nodejs:8` oder `nodejs:10`. 

    ```
    ibmcloud fn action create packageAction action.zip --kind nodejs:10
    ```
    {: pre}

6. Rufen Sie die Aktion auf.

    ```
    ibmcloud fn action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
    ```
    {: pre}

    Beispielausgabe:

    ```
    {
        "padded": [
            ".......................and now",
          "......for something completely",
          ".....................different"
      ]
    }
    ```
    {: screen}

## Aktion als einzelnes Bundle packen
{: #webpack_javascript}
{: #openwhisk_js_webpack_action}

Wenn die Paketierung der Aktion in einer ZIP-Datei zu viele nicht benötigte Dateien einbezieht oder wenn Sie eine schnellere Bereitstellung benötigen, können Sie den Minimalcode unter Einbeziehung von Abhängigkeiten in eine einzelne `.js`-Datei schreiben.
{: shortdesc}

Sie können eine Aktion mit einem JavaScript-Modul-Bundler wie [webpack ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://webpack.js.org/concepts/) packen. Wenn `webpack` Ihren Code verarbeitet, erstellt er rekursiv ein Abhängigkeitsdiagramm, das jedes für Ihre Aktion erforderliche Modul einschließt.

1. Speichern Sie den folgenden Code in einer Datei namens `package.json`. `webpack` wird als Entwicklungsabhängigkeit hinzugefügt. 

    ```json
    {
      "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
        "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10"
      },
  "dependencies": {
        "left-pad": "1.1.3"
      },
  "devDependencies": {
        "webpack": "^3.8.1"
      }
    }
    ```
    {: codeblock}

2. Speichen Sie den folgenden Webpack-Konfigurationscode in einer Datei namens `webpack.config.js`.

    ```javascript
    var path = require('path');
    module.exports = {
      entry: './index.js',
  output: {
        path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
      },
  target: 'node'
    };
    ```
    {: codeblock}

3. Speichern Sie den folgenden Code in einer Datei namens `index.js`. Geben Sie für die Variable `global.main` die Hauptfunktion der Aktion an.

    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    global.main = myAction;
    ```
    {: codeblock}

4. Installieren Sie alle Abhängigkeiten lokal.

    ```
    npm install
    ```
    {: pre}

5. Erstellen Sie das Webpack-Bundle.

    ```
    npm run build
    ```
    {: pre}

    Die Datei `dist/bundle.js` wird erstellt und als Quellcode der Aktion bereitgestellt.

6. Erstellen Sie die Aktion mit dem Script `npm` oder über die Befehlszeilenschnittstelle (CLI).

    * Mit dem Script `npm`:

        ```
        npm run deploy
        ```
        {: pre}

    * Über die Befehlszeilenschnittstelle:

        ```
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    Die Bundledatei, die von `webpack` erstellt wird, unterstützt nur JavaScript-Abhängigkeiten. Daher schlagen Aktionsaufrufe möglicherweise fehl, wenn das Bundle von binären Dateiabhängigkeiten abhängig ist, weil diese nicht in die Datei `bundle.js` eingeschlossen sind. {: tip}
    



