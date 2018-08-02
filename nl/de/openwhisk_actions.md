---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Aktionen erstellen und aufrufen
{: #openwhisk_actions}

Aktionen sind statusunabhängige Code-Snippets, die auf der {{site.data.keyword.openwhisk}}-Plattform ausgeführt werden. Eine Aktion kann zum Beispiel dazu verwendet werden, Gesichter in einem Bild zu erkennen, auf eine Datenbankänderung zu antworten, eine Gruppe von API-Aufrufen zu aggregieren oder einen Tweet zu posten. Eine Aktion kann als JavaScript-, Swift-, Python- oder PHP-Funktion, als Java-Methode oder als beliebige binärkompatible ausführbare Datei, einschließlich Go-Programmen und angepassten ausführbaren Dateien, geschrieben werden, die in einen Docker-Container gepackt werden.
{:shortdesc}

Aktionen können explizit aufgerufen oder als Reaktion auf ein Ereignis ausgeführt werden. In beiden Fällen führt jede Ausführung einer Aktion zu einem Aktivierungsdatensatz, der durch eine eindeutige Aktivierungs-ID identifiziert wird. Die Eingabe für eine Aktion und das Ergebnis einer Aktion bestehen jeweils in einem Wörterverzeichnis aus Schlüssel/Wert-Paaren, wobei der Schlüssel eine Zeichenfolge und der Wert ein gültiger JSON-Wert ist. Aktionen können außerdem aus Aufrufen weiterer Aktionen oder aus einer definierten Folge von Aktionen bestehen.

In den folgenden Abschnitten finden Sie Informationen dazu, wie Sie Aktionen in Ihrer bevorzugten Entwicklungsumgebung erstellen, aufrufen und debuggen:
* [JavaScript](#creating-and-invoking-javascript-actions)
* [Swift](#creating-swift-actions)
* [Python](#creating-python-actions)
* [Java](#creating-java-actions)
* [PHP](#creating-php-actions)
* [Docker](#creating-docker-actions)
* [Go](#creating-go-actions)
* [Beliebige ausführbare Dateien](#creating-actions-arbitrary)

Außerdem enthalten die folgenden Abschnitte wissenswerte Informationen:
* [Aktionsausgabe überwachen](#monitor-action-output)
* [Aktionen abrufen](#getting-actions)
* [Aktionen auflisten](#listing-actions)
* [Aktionen löschen](#deleting-actions)
* [Unterstützung für große Anwendungen](#large-app-support)
* [In der Aktionskomponente auf Aktionsmetadaten zugreifen](#accessing-action-metadata-within-the-action-body)

## JavaScript-Aktionen erstellen und aufrufen
{: #creating-and-invoking-javascript-actions}

In den folgenden Abschnitten werden Sie in die Arbeit mit Aktionen in JavaScript eingeführt. Sie beginnen mit dem Erstellen und Aufrufen einer einfachen Aktion. Anschließend werden Sie einer Aktion Parameter hinzufügen und diese Aktion mit Parametern aufrufen. Als Nächstes folgt das Festlegen von Standardparametern und das Aufrufen dieser Parameter. Danach erstellen Sie asynchrone Aktionen und zum Schluss arbeiten Sie mit Aktionssequenzen.

### Einfache JavaScript-Aktion erstellen und aufrufen
{: #openwhisk_single_action_js}

Sehen Sie sich die folgenden Schritte und Beispiele an, um Ihre erste JavaScript-Aktion zu erstellen.

1. Erstellen Sie eine JavaScript-Datei mit dem folgenden Inhalt. Geben Sie für dieses Beispiel die Datei **hello.js** an.
  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  Die JavaScript-Datei könnte noch weitere Funktionen enthalten. Allerdings muss nach allgemeiner Konvention eine Funktion mit dem Namen **main** vorhanden sein, um den Einstiegspunkt für die Aktion bereitzustellen.

2. Erstellen Sie eine Aktion aus der folgenden JavaScript-Funktion. Für dieses Beispiel heißt die Aktion **hello**.
  ```
  ibmcloud fn action create hello hello.js
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: created action hello
  ```
  {: screen}

  Die CLI leitet den Typ der Aktion automatisch aus der Erweiterung der Quellendatei ab. Für `.js`-Quellendateien wird die Aktion in einer Laufzeit mit Node.js 6 ausgeführt. Sie können auch eine Aktion erstellen, die mit Node.js 8 ausgeführt wird, indem Sie den Parameter `--kind nodejs:8` explizit angeben. Weitere Informationen finden unter den Hinweisen zum Unterschied von Node.js 6 und 8 in der [Referenz](./openwhisk_reference.html#openwhisk_ref_javascript_environments).

3. Listen Sie die Aktionen auf, die Sie erstellt haben:
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

  Die erstellte Aktion **hello** wird angezeigt.

4. Nach dem Erstellen der Aktion können Sie sie in der Cloud mit dem Befehl **invoke** ausführen. Sie können Aktionen mit einem blockierenden (Flag *blocking*, d. h. Anforderung/Antwort) oder mit einem nicht blockierenden (Flag *non-blocking*) Aufruf ausführen. Eine Anforderung für einen blockierenden Aufruf _wartet_, bis das Aktivierungsergebnis verfügbar ist. Der Wartezeitraum beträgt weniger als 60 Sekunden oder das [Zeitlimit](./openwhisk_reference.html#openwhisk_syslimits) der Aktion, je nachdem, welcher Wert kürzer ist. Das Ergebnis der Aktivierung wird zurückgegeben, wenn es innerhalb des Wartezeitraums verfügbar ist. Anderenfalls setzt die Aktivierung die Verarbeitung im System fort und eine Aktivierungs-ID wird zurückgegeben, sodass das Ergebnis wie bei nicht blockierenden Anforderungen später geprüft werden kann (weitere Tipps zur Überwachung von Aktivierungen finden Sie [hier](#monitor-action-output)).

  Im folgenden Beispiel wird der Blockierungsparameter `--blocking` verwendet:
  ```
  ibmcloud fn action invoke --blocking hello
  ```
  {: pre}

  Der Befehl gibt zwei wichtige Informationen aus:
  * Die Aktivierungs-ID (`44794bd6aab74415b4e42a308d880e5b`)
  * Das Aufrufergebnis, wenn es innerhalb des erwarteten Wartezeitraums verfügbar ist

  **Die Ausgabe zeigt die Aktivierungs-ID an:**
  ```
  ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b
  ```
  {: screen}

  **Aufrufergebnis:**
  ```
  {
      "result": {
          "payload": "Hello world"
      },
      "status": "success",
      "success": true
  }
  ```
  {: screen}

  Das Ergebnis ist in diesem Fall die Zeichenfolge `Hello world`, die von der JavaScript-Funktion zurückgegeben wird. Mithilfe der Aktivierungs-ID können später die Protokolle oder das Ergebnis des Aufrufs abgerufen werden.

5. Wenn Sie das Aktionsergebnis nicht sofort benötigen, können Sie das Flag `--blocking` weglassen und einen nicht blockierenden Aufruf ausführen. Später können Sie das Ergebnis über die Aktivierungs-ID abrufen.

  Sehen Sie sich die folgenden Beispiele an:
  ```
  ibmcloud fn action invoke hello
  ```
  {: pre}

  **Befehlsausgabe:**
  ```
  ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: screen}

  Da Sie die Aktivierungs-ID nun kennen, können Sie sie angeben, um das Aktionsergebnis abzurufen:
  ```
  ibmcloud fn activation result 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: pre}

  **Aktionsergebnis:**
  ```
  {
      "payload": "Hello world"
  }
  ```
  {: screen}

6. Wenn Sie vergessen, die Aktivierungs-ID zu notieren, können Sie eine Liste der Aktivierungen in der Reihenfolge von der jüngsten bis zur ältesten abrufen. Führen Sie den folgenden Befehl aus, um eine Liste Ihrer Aktivierungen abzurufen:

  **Auflisten von Aktivierungen:**
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

### Asynchrone Aktionen erstellen
{: #openwhisk_asynchrony_js}

JavaScript-Funktionen, die asynchron ausgeführt werden, können das Aktivierungsergebnis zurückgeben, nachdem die Funktion `main` die Steuerung zurückgegeben hat, indem sie ein Promise-Objekt in Ihrer Aktion zurückgeben.

1. Speichern Sie den folgenden Inhalt in einer Datei mit dem Namen **asyncAction.js**.
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

  Beachten Sie, dass die Funktion `main` ein Promise-Objekt zurückgibt, das darauf hinweist, dass die Aktivierung noch nicht abgeschlossen wurde, der Abschluss aber erwartet wird.

  Die JavaScript-Funktion `setTimeout()` wartet in diesem Fall 2 Sekunden ab, bevor sie die Callback-Funktion aufruft, die den asynchronen Code darstellt, und geht dann in die Callback-Funktion des Promise-Objekts hinein.

  Der Callback-Funktion des Promise-Objekts verwendet zwei Argumente ('resolve' und 'reject'), die beide Funktionen sind.  Der Aufruf von `resolve()` erfüllt das Promise-Objekt und weist darauf hin, dass die Aktivierung normal abgeschlossen wird.

  Mit einem Aufruf von `reject()` kann das Promise-Objekt zurückgewiesen und signalisiert werden, dass die Aktivierung abnormal beendet wird.

2. Führen Sie die folgenden Befehle aus, um die Aktion zu erstellen und aufzurufen.

  Erstellen Sie eine Aktion mit dem Namen **asyncAction**:
  ```
  ibmcloud fn action create asyncAction asyncAction.js
  ```
  {: pre}

  Rufen Sie die Aktion auf:
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

  Beachten Sie, dass Sie einen blockierenden Aufruf einer asynchronen Aktion ausgeführt haben.

3. Rufen Sie das Aktivierungsprotokoll ab, um zu prüfen, wie lange die Ausführung der Aktivierung gedauert hat.

  Um dies zu tun, müssen Sie zuerst die Aktion zum Abrufen der Aktivierungs-ID auflisten:
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

  Rufen Sie anschließend die Aktivierungsprotokollinformationen mit der Aktivierungs-ID ab:
  ```
  ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
  ```
  {: pre}

  ```
  {
      "start": 1455881628103,
      "end":   1455881648126,
      ...
  }
  ```
  {: screen}

  Beim Vergleich der Zeitmarken für `start` und `end` im Aktivierungsdatensatz können Sie erkennen, dass diese Aktivierung geringfügig länger als zwei Sekunden zur Ausführung benötigt hat.

### Externe API mit Aktionen aufrufen
{: #openwhisk_apicall_action}

Die bisherigen Beispiele sind eigenständige JavaScript-Funktionen. Sie können jedoch auch eine Aktion erstellen, die eine externe API aufruft.

Das folgende Beispiel ruft den APOD-Service der NASA auf (APOD = Astronomy Picture of the Day), der täglich ein Bild des Universums zur Verfügung stellt.

1. Speichern Sie den folgenden Inhalt in einer Datei mit dem Namen **apod.js**.
  ```javascript
  var url = "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo";

  $.ajax({
    url: url,
    success: function(result){
    if("copyright" in result) {
      $("#copyright").text("Image Credits: " + result.copyright);
    }
    else {
      $("#copyright").text("Image Credits: " + "Public Domain");
    }

    if(result.media_type == "video") {
      $("#apod_img_id").css("display", "none");
      $("#apod_vid_id").attr("src", result.url);
    }
    else {
      $("#apod_vid_id").css("display", "none");
      $("#apod_img_id").attr("src", result.url);
    }
    $("#reqObject").text(url);
    $("#returnObject").text(JSON.stringify(result, null, 4));
    $("#apod_explaination").text(result.explanation);
    $("#apod_title").text(result.title);
  }
  });
  ```
  {: codeblock}

  Es wird ein Aufruf an die APOD-API der NASA durchgeführt und es werden Felder aus dem JSON-Ergebnis extrahiert. In den [Referenzinformationen](./openwhisk_reference.html#openwhisk_ref_javascript_environments) finden Sie detaillierte Informationen zu den Node.js-Paketen, die Sie in Ihren Aktionen verwenden können.

2. Führen Sie die folgenden Befehle aus, um die Aktion zu erstellen und aufzurufen.

  Erstellen Sie die Aktion mit dem Namen **apod**:
  ```
  ibmcloud fn action create apod apod.js
  ```
  {: pre}

  Rufen Sie die Aktion **apod** auf:
  ```
  ibmcloud fn action invoke --result apod
  ```
  {: pre}

  **Zurückgegebenes Objekt:**
  ```
  {
    "copyright": "Eric Houck",
    "date": "2018-03-28",
    "explanation": "Does an alignment like this occur only once in a blue moon? No, although it was during a blue moon that this single-shot image was taken.  During a full moon that happened to be the second of the month -- the situation that defines a blue moon -- the photographer created the juxtaposition in late January by quickly moving around to find just the right spot to get the background Moon superposed behind the arc of a foreground tree.  Unfortunately, in this case, there seemed no other way than getting bogged down in mud and resting the camera on a barbed-wire fence.  The arc in the oak tree was previously created by hungry cows in Knight's Ferry, California, USA.  Quirky Moon-tree juxtapositions like this can be created during any full moon though, given enough planning and time.  Another opportunity will arise this weekend, coincidently during another blue moon. Then, the second blue moon in 2018 will occur, meaning that for the second month this year, two full moons will appear during a single month (moon-th).  Double blue-moon years are relatively rare, with the last occurring in 1999, and the next in 2037.",
    "hdurl": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_1799.jpg",
    "media_type": "image",
    "service_version": "v1",
    "title": "Blue Moon Tree",
    "url": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_960.jpg"
  }
  ```
  {: screen}

### Aktion als Node.js-Modul packen
{: #openwhisk_js_packaged_action}

Als Alternative zum Schreiben des gesamten Aktionscodes in einer einzigen JavaScript-Quellendatei können Sie eine Aktion als `npm`-Paket schreiben. Nehmen Sie als Beispiel ein Verzeichnis mit den folgenden Dateien:

**package.json:**
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

**index.js:**
```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}

exports.main = myAction;
```
{: codeblock}

Die Aktion wird durch `exports.main` verfügbar gemacht. Der Aktionshandler selbst kann einen beliebigen Namen haben, solange er der üblichen Signatur für die Annahme und die Rückgabe eines Objekts (oder einem `Promise`-Objekt für ein Objekt) entspricht. Den Node.js-Konventionen entsprechend muss diese Datei entweder den Namen `index.js` erhalten oder Sie müssen den bevorzugten Dateinamen in der Eigenschaft `main` in 'package.json' angeben.

Gehen Sie wie folgt vor, um aus diesem Paket eine {{site.data.keyword.openwhisk_short}}-Aktion zu erstellen:

1. Installieren Sie alle Abhängigkeiten lokal:
  ```
  npm install
  ```
  {: pre}

2. Erstellen Sie ein `.zip`-Archiv, in dem alle Dateien (einschließlich aller Abhängigkeiten) enthalten sind:
  ```
  zip -r action.zip *
  ```
  {: pre}

  Die Verwendung der Windows Explorer-Aktion zur Erstellung der ZIP-Datei führt zu einer falschen Struktur. {{site.data.keyword.openwhisk_short}}-ZIP-Aktionen müssen `package.json` am Stammelement der ZIP-Datei aufweisen, während Windows Explorer die Datei in einem verschachtelten Ordner ablegt. Am sichersten ist die Verwendung des Befehlszeilenbefehls `zip`.
  {: tip}

3. Erstellen Sie die Aktion:
  ```
  ibmcloud fn action create packageAction --kind nodejs:6 action.zip
  ```
  {: pre}

  Bei der Erstellung einer Aktion aus einem `.zip`-Archiv mithilfe des CLI-Tools müssen Sie explizit einen Wert für das Flag `--kind` (d. h. `nodejs:6` oder `nodejs:8`) angeben.

4. Sie können die Aktion wie jede andere aufrufen:
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

Zum Schluss beachten Sie, dass zwar die meisten `npm`-Pakete JavaScript-Quellen mit `npm install` installieren, andere jedoch auch Binärartefakte installieren und kompilieren. Der Upload von Archivdateien unterstützt derzeit keine binären Abhängigkeiten, sondern nur JavaScript-Abhängigkeiten. Wenn im Archiv binäre Abhängigkeiten eingeschlossen sind, können Aktionsaufrufe fehlschlagen.

### Aktion als einzelnes Bundle packen
{: #openwhisk_js_webpack_action}

Es ist bequem, nur minimalen Code in eine `.js`-Datei einzufügen, die Abhängigkeiten einschließt. Diese Lösung ermöglicht zwar schnellere Bereitstellungen, kann jedoch unter bestimmten Umständen, wenn die Aktion als ZIP-Datei gepackt wird, zu groß sein, weil das Paket unnötige Dateien enthält.

Sie können einen JavaScript-Modul-Bundler wie [webpack](https://webpack.js.org/concepts/) verwenden. Wenn der Modul-Bundler 'webpack' Ihren Code verarbeitet, erstellt er rekursiv ein Abhängigkeitsdiagramm, das jedes Modul einschließt, das für Ihre Aktion erforderlich ist.

Das folgende Beispiel vermittelt einen kurzen Eindruck von der Verwendung von 'webpack':

Fügen Sie dem vorherigen Beispiel von `package.json` den Bundler `webpack` als Entwicklungsabhängigkeit (devDependencies) hinzu und fügen Sie einige npm-Scriptbefehle hinzu.
```json
{
  "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
    "build": "webpack --config webpack.config.js",
    "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:8"
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

Erstellen Sie die Webpack-Konfigurationsdatei `webpack.config.js`.
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

Setzen Sie die Variable `global.main` auf die Hauptfunktion der Aktion.

Mit dem vorherigen Beispiel:
```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}
global.main = myAction;
```
{: codeblock}

Wenn Ihr Funktion `main` lautet, verwenden Sie stattdessen die folgende Syntax:
```javascript
global.main = main;
```
{: codeblock}

Gehen Sie wie folgt vor, um eine OpenWhisk-Aktion mit `npm` und `webpack` zu erstellen und bereitzustellen:

1. Installieren Sie zunächst die Abhängigkeiten lokal:
  ```
  npm install
  ```
  {: pre}

2. Erstellen Sie das Webpack-Bundle:
  ```
  npm run build
  ```
  {: pre}

  Die Datei `dist/bundle.js` wird erstellt und zur Bereitstellung als Quellcode der Aktion verwendet.

3. Erstellen Sie die Aktion mit dem Script `npm` oder über die Befehlszeilenschnittstelle (CLI).

  Mit dem Script `npm`:
  ```
  npm run deploy
  ```
  {: pre}

  Über die Befehlszeilenschnittstelle:
  ```
  ibmcloud fn action update my-action dist/bundle.js
  ```
  {: pre}

Zum Schluss ist zu beachten, dass die Bundledatei, die von `webpack` erstellt wird, keine binären Abhängigkeiten, sondern JavaScript-Abhängigkeiten unterstützt. Daher schlagen Aktionsaufrufe fehl, wenn das Bundle von binären Abhängigkeiten abhängig ist, weil diese nicht in die Datei `bundle.js` eingeschlossen sind.

## Aktionssequenzen erstellen
{: #openwhisk_create_action_sequence}

Sie können eine Aktion erstellen, die eine Sequenz von Aktionen miteinander verkettet.

Verschiedene Dienstprogrammaktionen werden in einem Paket mit dem Namen `/whisk.system/utils` bereitgestellt, die Sie zum Erstellen Ihrer ersten Sequenz verwenden können. Weitere Informationen zu Paketen finden Sie im Abschnitt zu [Paketen](./openwhisk_packages.html).

1. Zeigen Sie die Aktionen im Paket `/whisk.system/utils` an.
  ```
  ibmcloud fn package get --summary /whisk.system/utils
  ```
  {: pre}

  Beispielausgabe:
  ```
  package /whisk.system/utils: Building blocks that format and assemble data
   action /whisk.system/utils/head: Extract prefix of an array
   action /whisk.system/utils/split: Split a string into an array
   action /whisk.system/utils/sort: Sorts an array
   action /whisk.system/utils/echo: Returns the input
   action /whisk.system/utils/date: Current date and time
   action /whisk.system/utils/cat: Concatenates input into a string
  ```
  {: screen}

  In diesem Beispiel verwenden Sie die Aktionen `split` (Aufteilen) und `sort` (Sortieren).

2. Erstellen Sie eine Aktionssequenz, sodass das Ergebnis der einen Aktion als Argument an die nächste Aktion übergeben wird.
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

  Diese Aktionssequenz konvertiert Zeilen von Text in ein Array und sortiert die Zeilen.

3. Rufen Sie die Aktion auf:
  ```
  ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
  ```
  {: pre}

  Beispielausgabe:
  ```
  {
      "length": 3,
      "lines": [
          "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
  }
  ```
  {: screen}

  Wie leicht zu erkennen ist, sind die Zeilen im Ergebnis sortiert.

**Hinweis:** Außer den Standardparametern sind Parameter, die zwischen Aktionen in der Sequenz übergeben werden, sind explizit.
Daher sind die Parameter, die an die Aktionssequenz übergeben werden, nur für die erste Aktion in der Sequenz verfügbar. Das Ergebnis der ersten Aktion in der Sequenz wird zu dem JSON-Eingabeobjekt für die zweite Aktion in der Sequenz usw. Das Objekt enthält keine Parameter, die ursprünglich an die Sequenz übergeben wurden, es sei denn, die erste Aktion schließt sie explizit in ihr Ergebnis ein. Die Eingabeparameter für eine Aktion werden mit den Standardparametern der Aktion zusammengeführt. Erstere haben Vorrang und überschreiben alle übereinstimmenden Standardparameter. Weitere Informationen zum Aufrufen von Aktionssequenzen mit mehreren benannten Parametern finden Sie unter [Standardparameter für eine Aktion festlegen](./parameters.html#default-params-action).

## Python-Aktionen erstellen
{: #creating-python-actions}

Das Verfahren zur Erstellung von Python-Aktionen ist dem von JavaScript-Aktionen ähnlich. In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen Python-Aktion sowie zum Übergeben von Parametern an diese Aktion beschrieben.

### Python-Aktion erstellen und aufrufen
{: #openwhisk_actions_python_invoke}

Eine Aktion ist eine einfache Python-Funktion der höchsten Ebene. Erstellen Sie zum Beispiel eine Datei mit dem Namen **hello.py** und dem folgenden Quellcode:
```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
    print(greeting)
    return {"greeting": greeting}
```
{: codeblock}

Python-Aktionen lesen stets ein Wörterverzeichnis (Dictionary) und generieren ein Wörterverzeichnis. Die Eingangsmethode für die Aktion ist standardmäßig `main`. Sie kann jedoch wie bei jedem anderen Aktionstyp explizit zum Erstellen der Aktion über die Befehlszeilenschnittstelle `wsk` mit der Option `--main` angegeben werden.

Sie können eine {{site.data.keyword.openwhisk_short}}-Aktion mit dem Namen **helloPython** wie folgt aus dieser Funktion erstellen:
```
ibmcloud fn action create helloPython hello.py
```
{: pre}

Die CLI leitet den Typ der Aktion automatisch aus der Erweiterung der Quellendatei ab. Für `.py`-Quellendateien wird die Aktion in einer Laufzeit mit Python 2 ausgeführt. Sie können auch eine Aktion erstellen, die mit Python 3 ausgeführt wird, indem Sie den Parameter `--kind python:3` explizit angeben. Darüber hinaus gibt es eine Python 3-Laufzeit mit der Art `python-jessie:3`, die zusätzliche Pakete für IBM Cloud Services wie IBM Cloudant, IBM DB2, IBM COS und IBM Watson enthält.
Weitere Informationen zu Paketen in dieser Python 3-Laufzeit finden Sie in den [Referenzinformationen zur Python-Laufzeit](./openwhisk_reference.html#openwhisk_ref_python_environments).

Der Aktionsaufruf für Python-Aktionen entspricht dem für JavaScript-Aktionen:
```
ibmcloud fn action invoke --result helloPython --param name World
```
{: pre}

Beispielausgabe:
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

### Python-Aktionen in ZIP-Dateien packen
{: #openwhisk_actions_python_zip}

Sie können eine Python-Aktion und abhängige Module in eine ZIP-Datei packen.
Der Dateiname der Quellendatei, die den Eingangspunkt (z. B. `main`) enthält, muss `__main__.py` sein.
Beispiel: Zum Erstellen einer Aktion mit einem Helper-Modul mit dem Namen `helper.py` erstellen Sie zunächst ein Archiv, das Ihre Quellendateien enthält:
```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}

Anschließend erstellen Sie die Aktion:
```bash
ibmcloud fn action create helloPython --kind python:3 helloPython.zip
```
{: pre}

Diese Schritte werden zwar für Python 3 (mit der Art `python:3`) gezeigt, können jedoch genau so mit den anderen Python-Arten `python:2` oder `python-jessie:3` ausgeführt werden.

### Python-Aktionen mit einer virtuellen Umgebung in ZIP-Dateien packen
{: #openwhisk_actions_python_virtualenv}

Eine andere Methode zum Packen von Python-Abhängigkeiten ist die Verwendung einer virtuellen Umgebung (`virtualenv`). In dieser Umgebung können Sie zusätzliche Pakete verknüpfen, die zum Beispiel mit [`pip`](https://packaging.python.org/installing/) installiert werden können.

Wie bei der grundlegenden ZIP-Dateiunterstützung muss der Name der Quellendatei, die den Haupteingangspunkt enthält, `__main__.py` sein. Zur Verdeutlichung: Der Inhalt von `__main__.py` ist die Hauptfunktion, sodass Sie für dieses Beispiel `hello.py` in `__main__.py` umbenennen können (siehe vorherigen Abschnitt). Außerdem muss das virtualenv-Verzeichnis den Namen `virtualenv` haben. Das nachfolgende Beispielszenario zeigt die Installation von Abhängigkeiten, das Packen in einem Verzeichnis für die virtuelle Umgebung (virtualenv) sowie das Erstellen einer kompatiblen OpenWhisk-Aktion.

Um die Kompatibilität mit dem OpenWhisk-Laufzeitcontainer sicherzustellen, müssen Paketinstallationen in einer virtuellen Umgebung (virtualenv) in der Zielumgebung unter Verwendung des der Art entsprechenden Image erfolgen.
- Für die Art `python:2` verwenden Sie das Docker-Image `openwhisk/python2action`.
- Für die Art `python:3` verwenden Sie das Docker-Image `openwhisk/python3action`.
- Für die Art `python-jessie:3` verwenden Sie das Docker-Image `ibmfunctions/action-python-v3`.

1. Bei einer angegebenen Datei mit dem Namen [requirements.txt ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://pip.pypa.io/en/latest/user_guide/#requirements-files), die die `pip`-Module und -Versionen für die Installation enthält, führen Sie den folgenden Befehl aus, um die Abhängigkeiten zu installieren und eine virtuelle Umgebung (virtualenv) unter Verwendung eines kompatiblen Docker-Image zu erstellen:
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp &&virtualenv virtualenv &&source virtualenv/bin/activate &&pip install -r requirements.txt"
    ```
    {: pre}

2. Archivieren Sie das virtualenv-Verzeichnis und alle weiteren Python-Dateien:
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

3. Erstellen Sie die Aktion **helloPython**:
    ```
    ibmcloud fn action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}

Fügen Sie der Datei `requirements.txt` nur Module hinzu, die nicht Teil der ausgewählten Laufzeitumgebung sind. So kann die virtuelle Umgebung (virtualenv) möglichst klein gehalten werden.
{: tip}

## PHP-Aktionen erstellen
{: #creating-php-actions}

Das Verfahren zur Erstellung von PHP-Aktionen ist dem von JavaScript-Aktionen ähnlich. In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen PHP-Aktion sowie zum Übergeben von Parametern an diese Aktion beschrieben.

### PHP-Aktion erstellen und aufrufen
{: #openwhisk_actions_php_invoke}

Eine Aktion ist eine einfache PHP-Funktion der höchsten Ebene. Erstellen Sie zum Beispiel eine Datei mit dem Namen `hello.php` und dem folgenden Quellcode:

```php
<?php
function main(array $args) : array
{
    $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
```
{: codeblock}

PHP-Aktionen verarbeiten stets eine assoziative Feldgruppe und geben auch eine assoziative Feldgruppe zurück. Die Eingangsmethode für die Aktion ist standardmäßig `main`. Sie kann jedoch wie bei jedem anderen Aktionstyp explizit beim Erstellen der Aktion über die Befehlszeilenschnittstelle `ibmcloud fn` mit der Option `--main` angegeben werden.

Sie können eine {{site.data.keyword.openwhisk_short}}-Aktion mit dem Namen **helloPHP** wie folgt aus dieser Funktion erstellen:
```
ibmcloud fn action create helloPHP hello.php
```
{: pre}

Die CLI leitet den Typ der Aktion automatisch aus der Erweiterung der Quellendatei ab. Für `.php`-Quellendateien wird die Aktion in einer Laufzeit mit PHP 7.1 ausgeführt. Weitere Informationen finden Sie in den [Referenzinformationen zu PHP](./openwhisk_reference.html#openwhisk_ref_php).

Der Aktionsaufruf für PHP-Aktionen entspricht dem für JavaScript-Aktionen:
```
ibmcloud fn action invoke --result helloPHP --param name World
```
{: pre}

Beispielausgabe:
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

### PHP-Aktionen in ZIP-Dateien packen
{: #openwhisk_actions_php_zip}

Sie können eine PHP-Aktion zusammen mit anderen Dateien und abhängigen Paketen in eine ZIP-Datei packen.
Der Dateiname der Quellendatei, die den Eingangspunkt (z. B. `main`) enthält, muss `index.php` lauten.

Beispiel: Zum Erstellen einer Aktion, die eine zweite Datei mit der Bezeichnung `helper.php` enthält, erstellen Sie zuerst ein Archiv mit den Quellendateien:
```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

Erstellen Sie anschließend die Aktion **helloPHP**:
```bash
ibmcloud fn action create helloPHP --kind php:7.1 helloPHP.zip
```
{: pre}

## Swift-Aktionen erstellen
{: #creating-swift-actions}

Das Verfahren zur Erstellung von Swift-Aktionen ist dem von JavaScript-Aktionen ähnlich. In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen Swift-Aktion sowie zum Packen einer Aktion in einer ZIP-Datei beschrieben.

Sie können Ihren Swift-Code auch online mithilfe des [Online Swift Playground](http://online.swiftplayground.run) testen, ohne Xcode auf Ihrer Maschine installieren zu müssen.

**Achtung:** Swift-Aktionen werden in einer Linux-Umgebung ausgeführt. Swift für Linux befindet sich noch in Entwicklung und OpenWhisk arbeitet in der Regel mit dem neuesten verfügbaren Release, das jedoch nicht unbedingt stabil ist. Darüber hinaus ist es möglich, dass die mit OpenWhisk verwendete Version von Swift nicht mit den Versionen von Swift aus stabilen Releases von Xcode on MacOS konsistent ist.

### Aktion erstellen und aufrufen

#### Swift 3
Eine Aktion ist eine einfache Swift-Funktion der höchsten Ebene. Erstellen Sie zum Beispiel eine Datei mit dem Namen
**hello.swift** und dem folgenden Inhalt:

```swift
func main(args: [String:Any]) -> [String:Any] {
    if let name = args["name"] as? String {
        return [ "greeting" : "Hello \(name)!" ]
    } else {
        return [ "greeting" : "Hello stranger!" ]
    }
}
```
{: codeblock}

In diesem Beispiel liest die Swift-Aktion ein Wörterverzeichnis (Dictionary) und sie generiert ein Wörterverzeichnis.

Sie können eine OpenWhisk-Aktion mit dem Namen **helloSwift** wie folgt aus dieser Funktion erstellen:

```
ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
```
{: pre}

#### Swift 4

Neu in Swift 4 sind neben der obigen Hauptfunktionssignatur zwei weitere Out-of-the-box-Signaturen, die den [Codable](https://developer.apple.com/documentation/swift/codable)-Typ nutzen. Weitere Informationen zu Datentypen, die für die Kompatibilität mit externen Darstellungen wie JSON codierbar und decodierbar sind, finden Sie [hier](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types).

Im folgenden Beispiel wird ein Eingabeparameter als **Codable-Eingabe** mit dem Feld `name` verwendet und gibt eine **Codable-Ausgabe** mit einem Feld `greetings` zurück.
```swift
struct Input: Codable {
    let name: String?
}
struct Output: Codable {
    let greeting: String
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    let result = Output(greeting: "Hello \(param.name ?? "stranger")!")
    print("Log greeting:\(result.greeting)")
    completion(result, nil)
}
```
{: codeblock}

In diesem Beispiel liest die Swift-Aktion einen Codable-Typ und sie generiert einen Codable-Typ.
Wenn Sie keine Eingabe verarbeiten müssen, können Sie die Funktionssignatur verwenden, die keine Eingabe - sondern nur Codable-Ausgabe - verwendet.
```swift
struct Output: Codable {
    let greeting: String
}
func main(completion: (Output?, Error?) -> Void) -> Void {
    let result = Output(greeting: "Hello OpenWhisk!")
    completion(result, nil)
}
```
{: codeblock}

Sie können eine OpenWhisk-Aktion mit dem Namen `helloSwift` wie folgt aus dieser Funktion erstellen:

```
ibmcloud fn action create helloSwift hello.swift --kind swift:4.1
```
{: pre}

Weitere Informationen zur Swift-Laufzeit finden Sie in den [Referenzinformationen](./openwhisk_reference.html#swift-actions) zu Swift.

Der Aktionsaufruf für Swift-Aktionen entspricht dem für JavaScript-Aktionen:
```
ibmcloud fn action invoke --result helloSwift --param name World
```
{: pre}

Beispielausgabe:
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

Weitere Informationen zu Parametern finden Sie im Abschnitt [Mit Parametern arbeiten](./parameters.html).

### Aktion als ausführbare Swift-Datei packen
{: #packaging-an-action-as-a-swift-executable}

Wenn Sie eine OpenWhisk-Swift-Aktion mit einer Swift-Quellendatei erstellen, muss diese in eine Binärdatei kompiliert werden, bevor die Aktion ausgeführt wird. Danach werden Aufrufe der Aktion viel schneller durchgeführt, bis der Container, in dem die Aktion enthalten ist, bereinigt wird. Diese Verzögerung wird als Kaltstartverzögerung bezeichnet.

Zur Vermeidung der Kaltstartverzögerung können Sie Ihre Swift-Datei in eine Binärdatei kompilieren und anschließend in einer ZIP-Datei in OpenWhisk hochladen. Da Sie das OpenWhisk-Scaffolding benötigen, ist es am einfachsten, die Binärdatei innerhalb derselben Umgebung zu erstellen, in der sie ausgeführt wird. 

### Script zum Erstellen einer mit Swift gepackten Aktion verwenden

Sie können ein Script verwenden, um das Packen der Aktion zu automatisieren. Erstellen Sie eine Scriptdatei mit dem Namen `compile.sh` mit dem folgenden Beispielcode.
```bash
#!/bin/bash
set -ex

if [ -z "$1" ] ; then
    echo 'Error: Missing action name'
    exit 1
fi
if [ -z "$2" ] ; then
    echo 'Error: Missing kind, for example swift:4.1'
    exit 2
fi
OUTPUT_DIR="build"
if [ ${2} == "swift:3.1.1" ]; then
  BASE_PATH="/swift3Action"
  DEST_SOURCE="$BASE_PATH/spm-build"
  RUNTIME="openwhisk/action-swift-v3.1.1"
elif [ ${2} == "swift:4.1" ]; then
  RUNTIME="ibmfunctions/action-swift-v4.1"
  BASE_PATH="/swift4Action"
  DEST_SOURCE="/$BASE_PATH/spm-build/Sources/Action"
else
  echo "Error: Kind $2 not recognize"
  exit 3
fi
DEST_PACKAGE_SWIFT="$BASE_PATH/spm-build/Package.swift"

BUILD_FLAGS=""
if [ -n "$3" ] ; then
    BUILD_FLAGS=${3}
fi

echo "Using runtime $RUNTIME to compile swift"
docker run --rm --name=compile-ow-swift -it -v "$(pwd):/owexec" $RUNTIME bash -ex -c "

if [ -f \"/owexec/$OUTPUT_DIR/$1.zip\" ] ; then
    rm \"/owexec/$OUTPUT_DIR/$1.zip\"
fi

echo 'Setting up build...'
cp /owexec/actions/$1/Sources/*.swift $DEST_SOURCE/

# action file can be either {action name}.swift or main.swift
if [ -f \"$DEST_SOURCE/$1.swift\" ] ; then
    echo 'renaming $DEST_SOURCE/$1.swift $DEST_SOURCE/main.swift'
    mv \"$DEST_SOURCE/$1.swift\" $DEST_SOURCE/main.swift
fi
# Add in the OW specific bits
cat $BASE_PATH/epilogue.swift >> $DEST_SOURCE/main.swift
echo '_run_main(mainFunction:main)' >> $DEST_SOURCE/main.swift

# Only for Swift4
if [ ${2} != "swift:3.1.1" ]; then
  echo 'Adding wait to deal with escaping'
  echo '_ = _whisk_semaphore.wait(timeout: .distantFuture)' >> $DEST_SOURCE/main.swift
fi

echo \"Compiling $1...\"
cd /$BASE_PATH/spm-build
cp /owexec/actions/$1/Package.swift $DEST_PACKAGE_SWIFT
# we have our own Package.swift, do a full compile
swift build ${BUILD_FLAGS} -c release

echo 'Creating archive $1.zip...'
#.build/release/Action
mkdir -p /owexec/$OUTPUT_DIR
zip \"/owexec/$OUTPUT_DIR/$1.zip\" .build/release/Action
"
```
{: codeblock}

Das Script setzt voraus, dass Sie ein Verzeichnis mit dem Namen `actions` haben, wobei jedes Verzeichnis der höchsten Ebene eine Aktion darstellt.
```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```

- Erstellen Sie die Datei `Package.swift`, um Abhängigkeiten hinzuzufügen. **Hinweis:** Die Syntax unterscheidet sich bei Swift 3- und Swift 4-Tools.

  Swift 3 - Beispielsyntax:
  ```swift
  import PackageDescription

  let package = Package(
     name: "Action",
         dependencies: [
            .Package(url: "https://github.com/apple/example-package-deckofplayingcards.git", majorVersion: 3),
            .Package(url: "https://github.com/IBM-Swift/CCurl.git", "0.2.3"),
            .Package(url: "https://github.com/IBM-Swift/Kitura-net.git", "1.7.10"),
            .Package(url: "https://github.com/IBM-Swift/SwiftyJSON.git", "15.0.1"),
            .Package(url: "https://github.com/watson-developer-cloud/swift-sdk.git", "0.16.0")
         ]
  )
  ```
  {: codeblock}

  Swift 4 - Beispielsyntax:
  ```swift
  // swift-tools-version:4.0
  import PackageDescription

  let package = Package(
      name: "Action",
      products: [
        .executable(
          name: "Action",
          targets:  ["Action"]
        )
      ],
      dependencies: [
        .package(url: "https://github.com/apple/example-package-deckofplayingcards.git", .upToNextMajor(from: "3.0.0"))
      ],
      targets: [
        .target(
          name: "Action",
          dependencies: ["DeckOfPlayingCards"],
          path: "."
        )
      ]
  )
  ```
  {: codeblock}

  Wie Sie sehen können, wird `example-package-deckofplayingcards` in diesem Beispiel als Abhängigkeit hinzugefügt. Beachten Sie, dass `CCurl`, `Kitura-net` und `SwiftyJSON` in der Standard-Swift-Aktion zur Verfügung gestellt werden, sodass Sie sie bei Swift 3-Aktionen nur in Ihre eigene Datei `Package.swift` einfügen sollten.

- Erstellen Sie die Aktion, indem Sie den folgenden Befehl für eine Swift 3-Aktion ausführen:
  ```
  bash compile.sh hello swift:3.1.1
  ```
  {: pre}

  Bei der Kompilierung für Swift 4 verwenden Sie `swift:4.1` anstelle von `swift:3.1.1`:
  ```
  bash compile.sh hello swift:4.1
  ```
  {: pre}

  Dadurch wird `hello.zip` in `build` erstellt.

- Laden Sie sie mit dem Aktionsnamen **helloSwifty** in OpenWhisk hoch. Verwenden Sie für Swift 3 die folgende Art (kind): `swift:3.1.1`:
  ```
  ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:3.1.1
  ```
  {: pre}

  Verwenden Sie für Swift 4 die folgende Art (kind): `swift:4.1`:
  ```
  ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:4.1
  ```
  {: pre}

- Führen Sie den folgenden Befehl aus, um zu prüfen, wie viel schneller die Aktion ist:
  ```
  ibmcloud fn action invoke helloSwiftly --blocking
  ```
  {: pre}

  Die Zeit, die zur Ausführung der Aktion benötigt wurde, ist in der Eigenschaft 'duration' (Dauer) enthalten. Vergleichen Sie diese mit der Zeit, die zur Ausführung mit einem Kompilierungsschritt in der Aktion **hello** benötigt wird.

### Fehlerbehandlung in Swift 4

Mit dem neuen Codable-Completion-Handler können Sie einen Fehler übergeben, um auf einen Fehler in Ihrer Aktion hinzuweisen.
Die [Fehlerbehandlung in Swift](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) ähnelt der Behandlung von Ausnahmebedingungen in anderen Sprachen, in denen `try, catch` und `throw` verwendet werden.

Das folgende Snippet zeigt ein Beispiel für die Fehlerbehandlung:
```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    // Return real error
    do{
        throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
    } catch {
        completion(nil, error)
    }
}
```
{: codeblock}

## Java-Aktionen erstellen
{: #creating-java-actions}

Das Verfahren zur Erstellung von Java-Aktionen ist dem von JavaScript- und Swift-Aktionen ähnlich. In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen Java-Aktion sowie zum Übergeben von Parametern an diese Aktion beschrieben.

Damit Sie Java-Dateien kompilieren, testen und archivieren können, muss lokal ein [JDK 8](http://openjdk.java.net/install) installiert sein.

### Java-Aktion erstellen und aufrufen
{: #openwhisk_actions_java_invoke}

Eine Java-Aktion ist ein Java-Programm mit einer Methode namens `main`, deren exakte Signatur wie folgt aussieht:
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Erstellen Sie zum Beispiel eine Java-Datei mit dem Namen `Hello.java` und dem folgenden Inhalt:

```java
import com.google.gson.JsonObject;
public class Hello {
    public static JsonObject main(JsonObject args) {
        String name = "stranger";
        if (args.has("name"))
            name = args.getAsJsonPrimitive("name").getAsString();
        JsonObject response = new JsonObject();
        response.addProperty("greeting", "Hello " + name + "!");
        return response;
    }
}
```
{: codeblock}

Kompilieren Sie dann `Hello.java` wie folgt in die JAR-Datei `hello.jar`:
```
javac Hello.java
```
{: pre}

```
jar cvf hello.jar Hello.class
```
{: pre}

[google-gson](https://github.com/google/gson) muss in Ihrem Java-Klassenpfad (CLASSPATH) vorhanden sein, um die Java-Datei kompilieren zu können.
{: tip}

Aus dieser JAR-Datei können Sie folgendermaßen eine {{site.data.keyword.openwhisk_short}}-Aktion namens **helloJava** erstellen:
```
ibmcloud fn action create helloJava hello.jar --main Hello
```
{: pre}

Bei Verwendung der Befehlszeile und einer JAR-Quellendatei (`.jar`) brauchen Sie nicht anzugeben, dass Sie eine Java-Aktion erstellen. Das Tool bestimmt dies anhand der Dateierweiterung.

Sie müssen den Namen der Hauptklasse durch `--main` angeben. Eine infrage kommende Hauptklasse ist eine Klasse, die eine statische Methode `main` implementiert. Wenn sich die Klasse nicht im Standardpaket befindet, verwenden Sie den vollständig qualifizierten Java-Klassennamen (Beispiel: `--main com.example.MyMain`).

Sie können den Methodennamen Ihrer Java-Aktion auch anpassen, wenn dies erforderlich ist. Dies geschieht, indem Sie den vollständig qualifizierten Namen der Java-Methode Ihrer Aktion angeben. Beispiel: `--main com.example.MyMain#methodName`.

Der Aktionsaufruf für Java-Aktionen stimmt mit dem für Swift- und JavaScript-Aktionen überein:
```
ibmcloud fn action invoke --result helloJava --param name World
```
{: pre}

Beispielausgabe:
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

## Docker-Aktionen erstellen
{: #creating-docker-actions}

Bei {{site.data.keyword.openwhisk_short}}-Docker-Aktionen können Sie Ihre Aktionen in einer beliebigen Sprache schreiben.

Ihr Code wird in eine ausführbare Binärdatei kompiliert und in ein Docker-Image eingebettet. Das Binärprogramm interagiert mit dem System durch den Empfang von Eingaben über `stdin` und Ausgabe von Antworten über `stdout`.

Voraussetzung ist, dass Sie über ein Docker Hub-Konto verfügen.  Rufen Sie zur Einrichtung einer kostenlosen Docker-ID und eines Kontos [Docker Hub](https://hub.docker.com) auf.

In den nachfolgenden Anweisungen wird die Docker-Benutzer-ID `janesmith` und das Kennwort `janes_password` verwendet.  Wenn die CLI eingerichtet ist, bleiben drei Schritte übrig, um eine angepasste Binärdatei zur Verwendung durch {{site.data.keyword.openwhisk_short}} einzurichten. Anschließend kann das hochgeladene Docker-Image als Aktion verwendet werden.

1. Laden Sie das Docker-Gerüst (Skeleton) herunter. Sie können es über die CLI wie folgt herunterladen und installieren:
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

  Das Docker-Gerüst ist jetzt im aktuellen Verzeichnis installiert.
  ```
  ls dockerSkeleton/
  ```
  {: pre}

  Beispielausgabe:
  ```
  Dockerfile      README.md       buildAndPush.sh example.c
  ```
  {: screen}

  Das Gerüst ist eine Docker-Containervorlage, in die Sie Ihren Code in Form von angepassten Binärdateien einfügen können.

2. Richten Sie Ihre angepasste Binärdatei im Docker-Gerüst ('docherSkeleton') ein. Das Gerüst schließt bereits ein C-Programm ein, das Sie verwenden können.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

  Sie können diese Datei nach Bedarf ändern oder dem Docker-Image zusätzlichen Code und zusätzliche Abhängigkeiten hinzufügen.
  Im letzteren Fall können Sie die `Dockerfile` optimieren, um Ihre ausführbare Datei zu erstellen.
  Die Binärdatei muss sich im Container unter `/action/exec` befinden.

  Die Binärdatei empfängt ein einzelnes Argument über die Befehlszeile. Es handelt sich um eine Zeichenfolgeserialisierung des JSON-Objekts, das die Argumente für die Aktion darstellt. Das Programm kann die Protokolle in `stdout` oder `stderr` ausgeben.
  Die letzte Zeile der Ausgabe _muss_ der Konvention entsprechend ein in eine Zeichenfolge konvertiertes JSON-Objekt sein, das das Aktionsergebnis darstellt.

3. Erstellen Sie das Docker-Image und laden Sie es mithilfe eines bereitgestellten Scripts hoch. Sie müssen zunächst den Befehl `docker login` zur Authentifizierung und anschließend das Script mit einem ausgewählten Imagenamen ausführen.
  ```
  docker login -u janesmith -p janes_password
  ```
  {: pre}

  ```
  cd dockerSkeleton
  ```
  {: pre}

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  Beachten Sie, dass ein Teil der Datei `example.c` im Rahmen des Buildprozesses für das Docker-Image kompiliert wird, sodass Sie keine C-Kompilierung auf Ihrer Maschine benötigen. Tatsächlich lässt sich die Binärdatei in dem Container nicht ausführen, da die Formate nicht übereinstimmen, wenn Sie sie nicht auf einer kompatiblen Hostmaschine kompilieren.

  Ihr Docker-Container kann jetzt als {{site.data.keyword.openwhisk_short}}-Aktion verwendet werden:
  ```
  ibmcloud fn action create example --docker janesmith/blackboxdemo
  ```
  {: pre}

  Beachten Sie die Verwendung von `--docker` zum Erstellen einer Aktion. Es wird angenommen, dass alle Docker-Images auf einem Docker Hub gehostet werden. Die Aktion kann wie alle anderen {{site.data.keyword.openwhisk_short}}-Aktionen aufgerufen werden.
  ```
  ibmcloud fn action invoke --result example --param payload Rey
  ```
  {: pre}

  **Aufrufausgabe:**
  ```
  {
      "args": {
          "payload": "Rey"
      },
      "msg": "Hello from arbitrary C program!"
  }
  ```
  {: screen}

  Zum Aktualisieren der Docker-Aktion führen Sie `buildAndPush.sh` aus, um das neueste Image auf Docker Hub hochzuladen. Dies ermöglicht dem System das Extrahieren Ihres neuen Docker-Images bei der nächsten Ausführung des Codes für Ihre Aktion. Wenn keine aktiven Container vorhanden sind, verwenden die Aufrufe das neue Docker-Image. Wenn jedoch ein aktiver Container vorhanden ist, der eine ältere Version Ihres Docker-Images verwendet, verwenden neue Aufrufe weiterhin dieses Image, solange Sie nicht den Befehl `ibmcloud fn action update` ausführen. Damit wird dem System angezeigt, dass es für neue Aufrufe eine Docker-Pull-Operation ausführen muss, um Ihr neues Docker-Image abzurufen.

  **Laden Sie das aktuelle Image auf Docker Hub hoch:**
  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  **Aktualisieren Sie die Aktion, sodass neue Aufrufe mit dem neuen Image beginnen:***
  ```
  ibmcloud fn action update example --docker janesmith/blackboxdemo
  ```
  {: pre}

  Weitere Informationen zur Erstellung von Docker-Aktionen finden Sie im Abschnitt mit den [Referenzinformationen](./openwhisk_reference.html#openwhisk_ref_docker).

  Die vorherige Version der Befehlszeilenschnittstelle unterstützte `--docker` ohne Parameter und der Imagename war ein positionsgebundenes Argument. Damit die Docker-Aktionen Initialisierungsdaten mithilfe einer Datei (ZIP-Datei) akzeptieren können, normalisieren Sie das Benutzererlebnis für Docker-Aktionen so, dass ein positionsgebundenes Argument (falls vorhanden) eine Datei (z. B. eine ZIP-Datei) sein muss. Der Name des Images muss nach der Option `--docker` angegeben werden. Auf Kundenwunsch wurde das Argument `--native` als Kurzform für `--docker openwhisk/dockerskeleton` hinzugefügt, damit ausführbare Dateien, die im Standard-Docker-Aktion-SDK ausgeführt werden, bequemer erstellt und bereitgestellt werden können.

  Beispiel: Dieses Lernprogramm erstellt eine ausführbare Binärdatei innerhalb des Containers im Verzeichnis `/action/exec`. Wenn Sie diese Datei in Ihr lokales Dateisystem kopieren und in die Datei `exec.zip` komprimieren, können Sie die folgenden Befehle verwenden, um eine Docker-Aktion zu erstellen, die die ausführbare Datei als Initialisierungsdaten empfängt.

  **Erstellen Sie eine Aktion aus der ZIP-Datei:**
  ```
  ibmcloud fn action create example exec.zip --native
  ```
  {: pre}

  Dies entspricht folgendem Befehl:
  ```
  ibmcloud fn action create example exec.zip --docker openwhisk/dockerskeleton
  ```
  {: pre}

## Go-Aktionen erstellen
{: #creating-go-actions}

Mit der Option `--native` können ausführbare Dateien als Aktion gepackt werden. Dies funktioniert für Go als Beispiel. Wie bei Docker-Aktionen empfängt die ausführbare Go-Datei ein einzelnes Argument über die Befehlszeile. Dabei handelt sich um eine Zeichenfolgeserialisierung des JSON-Objekts, das die Argumente für die Aktion darstellt. Das Programm kann die Protokolle in `stdout` oder `stderr` ausgeben. Die letzte Zeile der Ausgabe _muss_ der Konvention entsprechend ein in eine Zeichenfolge konvertiertes JSON-Objekt sein, das das Aktionsergebnis darstellt.

Das folgende Beispiel zeigt eine Go-Aktion.
```go
package main

import "encoding/json"
import "fmt"
import "os"

func main() {
    //Das Programm empfängt ein Argument: das JSON-Objekt als Zeichenfolge
    arg := os.Args[1]

    // Unmarshalling der Zeichenfolge in ein JSON-Objekt
    var obj map[string]interface{}
    json.Unmarshal([]byte(arg), &obj)

    // Optioniale Protokollierung an stdout (oder stderr)
    fmt.Println("hello Go action")

    name, ok := obj["name"].(string)
    if !ok { name = "Stranger" }

    // Letzte Zeile von stdout ist das JSON-Ergebnisobjekt als Zeichenfolge
    msg := map[string]string{"msg": ("Hello, " + name + "!")}
    res, _ := json.Marshal(msg)
    fmt.Println(string(res))
}
```
{: codeblock}

Speichern Sie den obigen Code in einer Datei mit dem Namen `sample.go` und kompilieren Sie sie für OpenWhisk. Die ausführbare Datei muss den Namen `exec` haben.
```bash
GOOS=linux GOARCH=amd64 go build -o exec
zip exec.zip exec
ibmcloud fn action create helloGo --native exec.zip
```
{: codeblock}

Die Aktion kann wie jede andere Aktion ausgeführt werden.
```bash
ibmcloud fn action invoke helloGo -r -p name gopher
{
    "msg": "Hello, gopher!"
}
```

Protokolle werden ebenfalls auf ähnliche Weise abgerufen.
```bash
ibmcloud fn activation logs --last --strip
my first Go action.
```

## Aktionen mit beliebigen ausführbaren Dateien erstellen
{: #creating-actions-arbitrary}

Mit der Option `--native` können Sie anzeigen, ob __ausführbare Dateien als OpenWhisk-Aktion ausgeführt werden können. Dies schließt `Bash-Scripts` oder auch übergreifend kompilierte Binärprogramme mit ein. Für die letztere besteht die Einschränkung darin, dass das Binärprogramm mit dem Image von `openwhisk/dockerskeleton` kompatibel sein muss.

## Aktionsausgabe überwachen
{: #monitor-action-output}

{{site.data.keyword.openwhisk_short}}-Aktionen können von anderen Benutzern, als Reaktion auf verschiedene Ereignisse oder als Teil einer Aktionssequenz aufgerufen werden. In solchen Fällen kann es nützlich sein, die Aufrufe zu überwachen.

Sie können die Ausgabe von Aktionen, wenn sie aufgerufen werden, über die {{site.data.keyword.openwhisk_short}}-CLI beobachten.

1. Geben Sie den folgenden Befehl über eine Shell ein:
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

  Dieser Befehl startet eine Polling-Schleife, die kontinuierlich auf Protokolle aus Aktivierungen prüft.

2. Wechseln Sie zu einem anderen Fenster und rufen Sie eine Aktion auf:
  ```
  ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
  ```
  {: screen}

3. Beobachten Sie das Aktivierungsprotokoll in dem Polling-Fenster:
  ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
  ```
  {: screen}

  Immer wenn Sie das Dienstprogramm 'poll' ausführen, werden die Protokolle für Aktionen, die für Sie in OpenWhisk ausgeführt werden, in Echtzeit angezeigt.

## Aktionen abrufen
{: #getting-actions}

Metadaten, die vorhandene Aktionen beschreiben, können mit dem Befehl `ibmcloud fn action` abgerufen werden.

**Befehl:**
```
ibmcloud fn action get hello
```

***Ergebnis:**
```
ok: got action hello
{
    "namespace": "user@email.com",
    "name": "hello",
    "version": "0.0.1",
    "exec": {
        "kind": "nodejs:6",
        "binary": false
    },
    "annotations": [
        {
            "key": "exec",
            "value": "nodejs:6"
        }
    ],
    "limits": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}

### Aktions-URL abrufen
{: #get-action-url}

Eine Aktion kann über die REST-Schnittstelle durch eine HTTPS-Anforderung aufgerufen werden. Führen Sie den folgenden Befehl aus, um eine Aktions-URL abzurufen:
```
ibmcloud fn action get actionName --url
```
{: pre}

Für Standardaktionen wird eine URL mit dem folgenden Format zurückgegeben:
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

Für [Webaktionen](./openwhisk_webactions.html) wird eine URL im folgenden Format zurückgegeben:
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**Hinweis:** Bei Standardaktionen ist eine Authentifizierung erforderlich, wenn der Aufruf über eine HTTPS-Anforderung erfolgt. Weitere Informationen zu Aktionsaufrufen über die REST-Schnittstelle finden Sie in der [REST-API-Referenz](https://console.bluemix.net/apidocs/98-cloud-functions?&language=node#introduction).

### Aktionscode speichern
{: #save-action}

Code, der einer vorhandenen Aktion zugeordnet ist, kann abgerufen und lokal gespeichert werden. Das Speichern kann für alle Aktionen außer Sequenzen und Docker-Aktionen ausgeführt werden.

1. Speichern Sie den Aktionscode unter einem Dateinamen, der dem Namen einer vorhandenen Aktion im aktuellen Arbeitsverzeichnis entspricht. Es wird eine Dateierweiterung verwendet, die der Art der verwendeten Aktion entspricht, oder es wird der Erweiterungstyp '.zip' für den Aktionscode verwendet, der sich in einer ZIP-Datei befindet.
  ```
  ibmcloud fn action get actionName --save
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```
  {: screen}

2. Anstatt das Ziel des zu speichernden Codes durch die Befehlszeilenschnittstelle bestimmen zu lassen, können ein angepasster Dateipfad, ein Dateiname und eine Erweiterung durch das Flag `--save-as` angegeben werden.
  ```
  ibmcloud fn action get actionName --save-as codeFile.js
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```
  {: screen}

## Aktionen auflisten
{: #listing-actions}

Sie können alle Aktionen auflisten, die mit dem folgenden Befehl erstellt wurden:
```
ibmcloud fn action list
```
{: pre}

Je mehr Aktionen Sie schreiben, desto umfangreicher wird die Liste. Es kann daher hilfreich sein, zusammengehörige Aktionen in [Paketen](./openwhisk_packages.html) zu gruppieren. Mit der folgenden Befehlssyntax können Sie die Liste der Aktionen so filtern, dass nur Aktionen in einem bestimmten Paket angezeigt werden:
```
ibmcloud fn action list [PACKAGE NAME]
```
{: pre}

## Aktionen löschen
{: #deleting-actions}

Sie können eine Bereinigung durchführen, indem Sie Aktionen löschen, die nicht mehr verwendet werden sollen.

1. Führen Sie den folgenden Befehl zum Löschen einer Aktion aus:
  ```
  ibmcloud fn action delete hello
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: deleted hello
  ```
  {: screen}

2. Überprüfen Sie, ob die Aktion nicht mehr in der Liste der Aktionen angezeigt wird.
  ```
  ibmcloud fn action list
  ```
  {: pre}

  Beispielausgabe:
  ```
  actions
  ```
  {: screen}

## Unterstützung für große Anwendungen
{: #large-app-support}

Die maximale Codegröße für eine Aktion ist 48 MB. Anwendungen, die viele Drittanbietermodule, native Bibliotheken oder externe Tools enthalten, könnten auf diese Begrenzung stoßen.

Wenn Sie ein Paket (ZIP- oder JAR-Datei) erstellen, das größer als 48 MB ist, besteht die Lösung darin, das Laufzeitimage um Abhängigkeiten zu erweitern und anschießend eine einzelne Quellendatei oder ein kleineres Archiv als 48 MB zu verwenden.

Wenn zum Beispiel eine Docker-Laufzeit erstellt wird, die erforderliche gemeinsam genutzte Bibliotheken einschließt, müssen diese Abhängigkeiten nicht in der Archivdatei enthalten sein. Private Quellendateien können weiterhin in das Archiv eingebunden und während der Ausführung eingefügt werden.

Ein weiterer Vorteil der Verringerung von Archivdateigrößen besteht darin, dass sich auch Bereitstellungszeiten verbessern. In den folgenden Abschnitten werden zwei Laufzeitbeispiele gezeigt, die veranschaulichen, wie die Anwendungsgrößen durch die Verwendung dieser Technik reduziert werden können.

### Python-Beispiel

Orientieren Sie sich zum Reduzieren der Codegröße bei einer Python-Anwendung an den folgenden Schritten:

1. Stellen Sie die Bibliothek `opencv-python` in 'opencv'.
2. Installieren Sie anschließend die 'opencv'-Binärdatei im Betriebssystemimage.
3. Anschließend können Sie die Datei `requirements.txt` verwenden und den Befehl `pip install requirements.txt` ausführen, um das Image um weitere Python-Bibliotheken zu erweitern.
4. Sie können die Datei `action.py` anschließend mit dem neuen Image verwenden.

### Node.js-Beispiel

Um die Anwendungsgröße für eine Node.js-Anwendung zu reduzieren, orientieren Sie sich an den folgenden Schritten zum Installieren zusätzlicher Pakete im Betriebssystemimage:

1. Installieren Sie 'opencv' mithilfe von `npm`:
   ```
   npm install opencv
   ```
   {: pre}

2. Wenn Sie eine Datei `package.json` haben, können Sie die Installation mithilfe von `npm` ganz ähnlich durchführen:
   ```
   npm install package.json
   ```
   {: pre}

3. Fahren Sie anschließend mit der Verwendung von `action.js` mit dem neuen Image fort.

## In der Aktionskomponente auf Aktionsmetadaten zugreifen
{: #accessing-action-metadata-within-the-action-body}

Die Aktionsumgebung enthält mehrere Eigenschaften, die für die aktive Aktion spezifisch sind. Mit diesen Eigenschaften kann die Aktion programmgestützt über die REST-API mit OpenWhisk-Assets arbeiten oder einen internen Alarm auslösen, wenn die Aktion kurz davor ist, das zugeteilte Zeitbudget aufzubrauchen.
Auf die Eigenschaften kann über die Systemumgebung für alle unterstützten Laufzeiten zugegriffen werden: Node.js-, Python-, Swift-, Java- und Docker-Aktionen bei Verwendung des Docker-Gerüsts für OpenWhisk.

* `__OW_API_HOST`: Der API-Host für die OpenWhisk-Bereitstellung, die diese Aktion ausführt.
* `__OW_API_KEY`: Der API-Schlüssel für das Subjekt, das die Aktion aufruft; dieser Schlüssel kann ein eingeschränkter API-Schlüssel sein.
* `__OW_NAMESPACE`: Der Namensbereich für die _Aktivierung_ (dieser kann sich von dem Namensbereich für die Aktion unterscheiden).
* `__OW_ACTION_NAME`: Der vollständig qualifizierte Name der ausgeführten Aktion.
* `__OW_ACTIVATION_ID`: Die Aktivierungs-ID für diese ausgeführte Aktionsinstanz.
* `__OW_DEADLINE`: Der näherungsweise berechnete Zeitpunkt, zu dem diese Aktion das gesamte Zeitkontingent aufbraucht (in Epochenmillisekunden).
