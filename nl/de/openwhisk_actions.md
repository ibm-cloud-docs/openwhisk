---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-09"

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
* [Aktionsausgaben beobachten](#watching-action-output)
* [Unterstützung für große Anwendungen](#large-app-support)
* [Aktionen auflisten](#listing-actions)
* [Aktionen löschen](#deleting-actions)
* [In der Aktionskomponente auf Aktionsmetadaten zugreifen](#accessing-action-metadata-within-the-action-body)


## JavaScript-Aktionen erstellen und aufrufen
{: #creating-and-invoking-javascript-actions}

In den folgenden Abschnitten werden Sie in die Arbeit mit Aktionen in JavaScript eingeführt. Sie beginnen mit dem Erstellen und Aufrufen einer einfachen Aktion. Anschließend werden Sie einer Aktion Parameter hinzufügen und diese Aktion mit Parametern aufrufen. Als Nächstes folgt das Festlegen von Standardparametern und das Aufrufen dieser Parameter. Danach erstellen Sie asynchrone Aktionen und zum Schluss arbeiten Sie mit Aktionssequenzen.


### Einfache JavaScript-Aktion erstellen und aufrufen
{: #openwhisk_single_action_js}

Sehen Sie sich die folgenden Schritte und Beispiele an, um Ihre erste JavaScript-Aktion zu erstellen.

1. Erstellen Sie eine JavaScript-Datei mit dem folgenden Inhalt. Für dieses Beispiel wird der Dateiname 'hello.js' verwendet.

  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  Die JavaScript-Datei könnte noch weitere Funktionen enthalten. Allerdings muss nach allgemeiner Konvention eine Funktion mit dem Namen `main` vorhanden sein, um den Einstiegspunkt für die Aktion bereitzustellen.

2. Erstellen Sie eine Aktion aus der folgenden JavaScript-Funktion. Für dieses Beispiel heißt die Aktion 'hello'.

  ```
  wsk action create hello hello.js
  ```
  {: pre}

  ```
  ok: created action hello
  ```
  Die CLI leitet den Typ der Aktion automatisch aus der Erweiterung der Quellendatei ab. Für `.js`-Quellendateien wird die Aktion in einer Laufzeit mit Node.js 6 ausgeführt. Sie können auch eine Aktion erstellen, die mit Node.js 8 ausgeführt wird, indem Sie den Parameter `--kind nodejs:8` explizit angeben. Weitere Informationen finden unter den Hinweisen zum Unterschied von Node.js 6 und 8 in der [Referenz](./openwhisk_reference.html#openwhisk_ref_javascript_environments).
  
3. Listen Sie die Aktionen auf, die Sie erstellt haben:

  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  hello       private
  ```

  Die erstellte Aktion `hello` wird angezeigt.

4. Nach dem Erstellen der Aktion können Sie sie in der Cloud in OpenWhisk mit dem Befehl 'invoke' ausführen. Sie können Aktionen mit einem blockierenden (Flag *blocking*, d. h. Anforderung/Antwort) oder mit einem nicht blockierenden (Flag *non-blocking*) Aufruf ausführen. Eine Anforderung für einen blockierenden Aufruf _wartet_, bis das Aktivierungsergebnis verfügbar ist. Der Wartezeitraum beträgt weniger als 60 Sekunden oder das [Zeitlimit](./openwhisk_reference.html#openwhisk_syslimits) der Aktion, je nachdem, welcher Wert kürzer ist. Das Ergebnis der Aktivierung wird zurückgegeben, wenn es innerhalb des Wartezeitraums verfügbar ist. Anderenfalls setzt die Aktivierung die Verarbeitung im System fort und eine Aktivierungs-ID wird zurückgegeben, sodass das Ergebnis wie bei nicht blockierenden Anforderungen später geprüft werden kann (weitere Tipps zur Überwachung von Aktivierungen finden Sie [hier](#watching-action-output)).

  Im folgenden Beispiel wird der Blockierungsparameter `--blocking` verwendet:

  ```
  wsk action invoke --blocking hello
  ```
  {: pre}

  ```
  ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b
  ```

  ```json
  {
      "result": {
          "payload": "Hello world"
      },
      "status": "success",
      "success": true
  }
  ```

  Der Befehl gibt zwei wichtige Informationen aus:
  * Die Aktivierungs-ID (`44794bd6aab74415b4e42a308d880e5b`)
  * Das Aufrufergebnis, wenn es innerhalb des erwarteten Wartezeitraums verfügbar ist

  Das Ergebnis ist in diesem Fall die Zeichenfolge `Hello world`, die von der JavaScript-Funktion zurückgegeben wird. Mithilfe der Aktivierungs-ID können später die Protokolle oder das Ergebnis des Aufrufs abgerufen werden.  

5. Wenn Sie das Aktionsergebnis nicht sofort benötigen, können Sie das Flag `--blocking` weglassen und einen nicht blockierenden Aufruf ausführen. Später können Sie das Ergebnis über die Aktivierungs-ID abrufen. Beispiel:

  ```
  wsk action invoke hello
  ```
  {: pre}

  ```
  ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
  ```

  ```
  wsk activation result 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: pre}

  ```json
  {
      "payload": "Hello world"
  }
  ```

6. Wenn Sie vergessen, die Aktivierungs-ID zu notieren, können Sie eine Liste der Aktivierungen in der Reihenfolge von der jüngsten bis zur ältesten abrufen. Führen Sie den folgenden Befehl aus, um eine Liste Ihrer Aktivierungen abzurufen:

  ```
  wsk activation list
  ```
  {: pre}

  ```
  activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
  ```

### Parameter an eine Aktion übergeben
{: #openwhisk_pass_params}

Beim Aufruf können Parameter an die Aktion übergeben werden.

1. Verwenden Sie Parameter in der Aktion. Aktualisieren Sie die Datei 'hello.js' zum Beispiel mit dem folgenden Inhalt:

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  Die Eingabeparameter werden in Form eines JSON-Objektparameters an die Funktion `main` übergeben. Beachten Sie, wie die Parameter `name` und `place` in diesem Beispiel aus dem Objekt `params` abgerufen werden.

2. Aktualisieren Sie die Aktion `hello` und rufen Sie sie auf, `name` und `place` übergeben. Beispiel:

  ```
  wsk action update hello hello.js
  ```
  {: pre}

  Wenn Sie Ihre nicht zum Service gehörenden Berechtigungsnachweisparameter ändern müssen, beachten Sie, dass durch einen Befehl `action update` mit neuen Parametern alle Parameter entfernt werden, die zurzeit vorhanden sind, jedoch nicht in dem Befehl `action update` angegeben werden. Beispiel: Nehmen Sie an, neben dem Parameter `__bx_creds` sind noch zwei Parameter mit den Schlüsseln 'key1' und 'key2' vorhanden. Wenn Sie einen Befehl `action update` mit `-p key1 neuer-wert -p key2 neuer-wert` ausführen, den Parameter `__bx_creds` jedoch nicht angeben, ist der Parameter `__bx_creds` nach erfolgreicher Ausführung des Befehls `action update` nicht mehr vorhanden. In diesem Fall müssen Sie die Serviceberechtigungsnachweise erneut binden. Dies ist eine bekannte Einschränkung, für die es keine Ausweichlösung gibt.
  {: tip}  

3.  Parameter können in der Befehlszeile explizit angegeben oder in einer Datei bereitgestellt werden, die die gewünschten Parameter enthält.

  Zur Übergabe von Parametern direkt über die Befehlszeile geben Sie ein Schlüssel/Wert-Paar für das Flag `--param` an:
  ```
  wsk action invoke --result hello --param name Bernie --param place Vermont
  ```
  {: pre}

  Wenn Sie eine Datei mit Parameterwerten verwenden wollen, erstellen Sie eine Datei, die die Parameter im JSON-Format enthält. Der Dateiname muss anschließend an das Flag `param-file` übergeben werden:

  Sehen Sie sich die folgende Beispielparameterdatei `parameters.json` an:
  ```json
  {
      "name": "Bernie",
      "place": "Vermont"
  }
  ```

  ```
  wsk action invoke --result hello --param-file parameters.json
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

  Beachten Sie die Verwendung der Option `--result`: Sie bewirkt einen blockierenden Aufruf, bei dem die Befehlszeilenschnittstelle auf den Abschluss der Aktivierung wartet und dann nur das Ergebnis anzeigt. Aus Gründen des Bedienungskomforts kann diese Option ohne die Option `--blocking` verwendet werden, die automatisch abgeleitet wird.

  Darüber hinaus gilt: Wenn die in der Befehlszeile angegebenen Parameterwerte gültige JSON-Werte sind, dann werden sie analysiert und als strukturiertes Objekt an Ihre Aktion gesendet. Aktualisieren Sie die Datei 'action' zum Beispiel wie folgt:

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  Jetzt erwartet die Aktion, dass ein einzelner Parameter `person` die Felder `name` und `place` aufweist. Rufen Sie als Nächstes die Aktion mit einem einzelnen Parameter `person` auf, der ein gültiger JSON-Wert ist, wie im folgenden Beispiel:

  ```
  wsk action invoke --result hello -p person '{"name": "Bernie", "place": "Vermont"}'
  ```
  {: pre}

  Das Ergebnis ist dasselbe, weil die CLI automatisch den Parameter `person` in das strukturierte Objekt analysiert, das jetzt von der Aktion erwartet wird:
  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

### Standardparameter festlegen
{: #openwhisk_binding_actions}

Aktionen können mit mehreren benannten Parameter aufgerufen werden. Die Aktion `hello` aus dem vorherigen Beispiel erwartet beispielsweise zwei Parameter: den Namen (*name*) einer Person und den Ort (*place*), aus dem sie kommt.

Anstatt nun jedes Mal alle Parameter an eine Aktion zu übergeben, können Sie bestimmte Parameter binden. Im folgenden Beispiel wird der Parameter *place* gebunden, sodass die Aktion mit dem Standardwert "Vermont" arbeitet:

1. Aktualisieren Sie die Aktion mit der Option `--param`, um Parameterwerte zu binden, oder durch die Übergabe einer Datei mit den Parametern an `--param-file`.

  Zur expliziten Angabe von Standardparametern über die Befehlszeile übergeben Sie ein Schlüssel/Wert-Paar an das Flag `param`:

  ```
  wsk action update hello --param place Vermont
  ```
  {: pre}

  Zur Übergabe von Parametern aus einer Datei muss eine Datei erstellt werden, die den gewünschten Inhalt im JSON-Format enthält. Anschließend muss der Dateiname an das Flag `-param-file` übergeben werden:

  Sehen Sie sich die folgende Beispielparameterdatei `parameters.json` an:
  ```json
  {
      "place": "Vermont"
  }
  ```
  {: codeblock}

  ```
  wsk action update hello --param-file parameters.json
  ```
  {: pre}

2. Rufen Sie die Aktion auf, indem Sie diesmal nur den Parameter `name` übergeben.

  ```
  wsk action invoke --result hello --param name Bernie
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

  Sie stellen fest, dass Sie den Parameter "place" beim Aufruf der Aktion nicht angeben mussten. Gebundene Parameter können weiterhin durch eine entsprechende Angabe eines Parameterwerts im Aufruf überschrieben werden.

3. Rufen Sie die Aktion auf, indem Sie Werte für `name` und `place` übergeben. Der letztere Wert überschreibt den Wert, der an die Aktion gebunden ist.

  Verwendung des Flags `--param`:

  ```
  wsk action invoke --result hello --param name Bernie --param place "Washington, DC"
  ```
  {: pre}

  Verwendung des Flags `--param-file`:

  Datei: parameters.json:
  ```json
  {
    "name": "Bernie",
    "place": "Vermont"
  }
  ```
  {: codeblock}
  ```
  wsk action invoke --result hello --param-file parameters.json
  ```
  {: pre}

  ```json
  {  
      "payload": "Hello, Bernie from Washington, DC"
  }
  ```

### Aktions-URL abrufen

Eine Aktion kann über die REST-Schnittstelle durch eine HTTPS-Anforderung aufgerufen werden. Führen Sie den folgenden Befehl aus, um eine Aktions-URL abzurufen:

```
wsk action get actionName --url
```
{: pre}

```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```

Beim Aufruf einer Aktion durch eine HTTPS-Anforderung muss die Authentifizierung angegeben werden. Weitere Informationen zu Aktionsaufrufen
über die REST-Schnittstelle finden Sie unter [REST-APIs mit OpenWhisk verwenden](./openwhisk_rest_api.html#openwhisk_rest_api).
{: tip}

### Aktionscode speichern

Code, der einer vorhandenen Aktion zugeordnet ist, wird abgerufen und lokal gespeichert. Das Speichern wird für alle Aktionen außer Sequenzen und Docker-Aktionen ausgeführt. Wenn Aktionscode in einer Datei gespeichert wird, wird der Code im aktuellen Arbeitsverzeichnis gespeichert und der Pfad der gespeicherten Datei angezeigt.

1. Speichern Sie Aktionscode unter einem Dateinamen, der dem Namen einer vorhandenen Aktion entspricht. Es wird eine Dateierweiterung verwendet, die der Art der verwendeten Aktion entspricht, oder es wird der Erweiterungstyp `.zip` für Aktionscode verwendet, der sich in einer ZIP-Datei befindet.
  ```
  wsk action get actionName --save
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```

2. Anstatt den Datennamen und die Erweiterung des gespeicherten Codes durch die Befehlszeilenschnittstelle bestimmen zu lassen, kann ein angepasster Dateiname und eine angepasste Erweiterung durch das Flag `--save-as` angegeben werden.
  ```
  wsk action get actionName --save-as codeFile.js
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```

### Asynchrone Aktionen erstellen
{: #openwhisk_asynchrony_js}

JavaScript-Funktionen, die asynchron ausgeführt werden, können das Aktivierungsergebnis zurückgeben, nachdem die Funktion `main` die Steuerung zurückgegeben hat, indem sie ein Promise-Objekt in Ihrer Aktion zurückgeben.

1. Speichern Sie den folgenden Inhalt in einer Datei mit dem Namen `asyncAction.js`.

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

  Der Callback-Funktion des Promise-Objekts verwendet zwei Argumente ('resolve' und 'reject'), die beide Funktionen sind. Der Aufruf von `resolve()` erfüllt das Promise-Objekt und weist darauf hin, dass die Aktivierung normal abgeschlossen wird.

  Mit einem Aufruf von `reject()` kann das Promise-Objekt zurückgewiesen und signalisiert werden, dass die Aktivierung abnormal beendet wird.

2. Führen Sie die folgenden Befehle aus, um die Aktion zu erstellen und aufzurufen:

  ```
  wsk action create asyncAction asyncAction.js
  ```
  {: pre}

  ```
  wsk action invoke --result asyncAction
  ```
  {: pre}

  ```json
  {
      "done": true
  }
  ```

  Beachten Sie, dass Sie einen blockierenden Aufruf einer asynchronen Aktion ausgeführt haben.

3. Rufen Sie das Aktivierungsprotokoll ab, um zu prüfen, wie lange die Ausführung der Aktivierung gedauert hat:

  ```
  wsk activation list --limit 1 asyncAction
  ```
  {: pre}
  
  ```
  activations
  b066ca51e68c4d3382df2d8033265db0             asyncAction
  ```

  ```
  wsk activation get b066ca51e68c4d3382df2d8033265db0
  ```
  {: pre}
 
  ```json
  {
      "start": 1455881628103,
      "end":   1455881648126,
      ...
  }
  ```

  Beim Vergleich der Zeitmarken für `start` und `end` im Aktivierungsdatensatz können Sie erkennen, dass diese Aktivierung geringfügig länger als zwei Sekunden zur Ausführung benötigt hat.

### Externe API mit Aktionen aufrufen
{: #openwhisk_apicall_action}

Die bisherigen Beispiele sind eigenständige JavaScript-Funktionen. Sie können jedoch auch eine Aktion erstellen, die eine externe API aufruft.

Im folgenden Beispiel wird ein Yahoo Weather-Services aufgerufen, um die aktuellen Wetterbedingungen an einem bestimmten Standort abzurufen.

1. Speichern Sie den folgenden Inhalt in einer Datei mit dem Namen `weather.js`.

  ```javascript
  var request = require('request');

  function main(params) {
      var location = params.location || 'Vermont';
      var url = 'https://query.yahooapis.com/v1/public/yql?q=select item.condition from weather.forecast where woeid in (select woeid from geo.places(1) where text="' + location + '")&format=json';

      return new Promise(function(resolve, reject) {
          request.get(url, function(error, response, body) {
              if (error) {
                  reject(error);
              }
              else {
                  var condition = JSON.parse(body).query.results.channel.item.condition;
                  var text = condition.text;
                  var temperature = condition.temp;
                  var output = 'It is ' + temperature + ' degrees in ' + location + ' and ' + text;
                  resolve({msg: output});
              }
          });
      });
  }
  ```
  {: codeblock}

 Die Aktion in diesem Beispiel verwendet die JavaScript-Bibliothek `request`, um eine HTTP-Anforderung an die Yahoo Weather-API zu senden, und extrahiert Felder aus dem JSON-Ergebnis. In den [Referenzinformationen](./openwhisk_reference.html#openwhisk_ref_javascript_environments) finden Sie detaillierte Informationen zu den Node.js-Paketen, die Sie in Ihren Aktionen verwenden können.

  Dieses Beispiel demonstriert außerdem den Bedarf an asynchronen Aktionen. Die Aktion gibt ein Promise-Objekt zurück, um anzugeben, dass das Ergebnis dieser Aktion noch nicht verfügbar ist, wenn die Funktion die Ausführung beendet. Das Ergebnis ist erst im Callback `request` verfügbar, wenn der HTTP-Aufruf abgeschlossen ist, und wird als Argument an die Funktion `resolve()` übergeben.

2. Führen Sie die folgenden Befehle aus, um die Aktion zu erstellen und aufzurufen:

  ```
  wsk action create weather weather.js
  ```
  {: pre}

  ```
  wsk action invoke --result weather --param location "Brooklyn, NY"
  ```
  {: pre}

  ```json
  {
      "msg": "It is 28 degrees in Brooklyn, NY and Cloudy"
  }
  ```

### Aktion als Node.js-Modul packen
{: #openwhisk_js_packaged_action}

Als Alternative zum Schreiben des gesamten Aktionscodes in einer einzigen JavaScript-Quellendatei können Sie eine Aktion als `npm`-Paket schreiben. Nehmen Sie als Beispiel ein Verzeichnis mit den folgenden Dateien:

Zunächst `package.json`:

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

Dann `index.js`:

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

Gehen Sie wie folgt vor, um aus diesem Paket eine OpenWhisk-Aktion zu erstellen:

1. Installieren Sie zunächst alle Abhängigkeiten lokal.

  ```
  npm install
  ```
  {: pre}

2. Erstellen Sie ein `.zip`-Archiv, in dem alle Dateien (einschließlich aller Abhängigkeiten) enthalten sind:

  ```
  zip -r action.zip *
  ```
  {: pre}

  Die Verwendung der Windows Explorer-Aktion zur Erstellung der ZIP-Datei führt zu einer falschen Struktur. OpenWhisk-ZIP-Aktionen müssen `package.json` am Stammelement der ZIP-Datei aufweisen, während Windows Explorer die Datei in einem verschachtelten Ordner ablegt. Am sichersten ist die Verwendung des Befehlszeilenbefehls `zip`.
  {: tip}

3. Erstellen Sie die Aktion:

  ```
  wsk action create packageAction --kind nodejs:6 action.zip
  ```
  {: pre}

  Bei der Erstellung einer Aktion aus einem `.zip`-Archiv mithilfe des CLI-Tools müssen Sie explizit einen Wert für das Flag `--kind` (d. h. `nodejs:6` oder `nodejs:8`) angeben.

4. Sie können die Aktion wie jede andere aufrufen:

  ```
  wsk action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
  ```
  {: pre}
  
  ```json
  {
      "padded": [
          ".......................and now",
          "......for something completely",
          ".....................different"
      ]
  }
  ```

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
    "deploy": "bx wsk action update my-action dist/bundle.js --kind nodejs:8"
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
  bx wsk action update my-action dist/bundle.js
  ```
  {: pre}


Zum Schluss ist zu beachten, dass die Bundledatei, die von `webpack` erstellt wird, keine binären Abhängigkeiten, sondern JavaScript-Abhängigkeiten unterstützt. Daher schlagen Aktionsaufrufe fehl, wenn das Bundle von binären Abhängigkeiten abhängig ist, weil diese nicht in die Datei `bundle.js` eingeschlossen sind.

## Aktionssequenzen erstellen
{: #openwhisk_create_action_sequence}

Sie können eine Aktion erstellen, die eine Sequenz von Aktionen miteinander verkettet.

Verschiedene Dienstprogrammaktionen werden in einem Paket mit dem Namen `/whisk.system/utils` bereitgestellt, die Sie zum Erstellen Ihrer ersten Sequenz verwenden können. Weitere Informationen zu Paketen finden Sie im Abschnitt zu [Paketen](./openwhisk_packages.html).

1. Zeigen Sie die Aktionen im Paket `/whisk.system/utils` an.

  ```
  wsk package get --summary /whisk.system/utils
  ```
  {: pre}

  ```
  package /whisk.system/utils: Building blocks that format and assemble data
   action /whisk.system/utils/head: Extract prefix of an array
   action /whisk.system/utils/split: Split a string into an array
   action /whisk.system/utils/sort: Sorts an array
   action /whisk.system/utils/echo: Returns the input
   action /whisk.system/utils/date: Current date and time
   action /whisk.system/utils/cat: Concatenates input into a string
  ```

  In diesem Beispiel verwenden Sie die Aktionen `split` (Aufteilen) und `sort` (Sortieren).

2. Erstellen Sie eine Aktionssequenz, sodass das Ergebnis der einen Aktion als Argument an die nächste Aktion übergeben wird.

  ```
  wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

  Diese Aktionssequenz konvertiert Zeilen von Text in ein Array und sortiert die Zeilen.

3. Rufen Sie die Aktion auf:

  ```
  wsk action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
  ```
  {: pre}

  ```json
  {
      "length": 3,
      "lines": [
          "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
  }
  ```

  Wie leicht zu erkennen ist, sind die Zeilen im Ergebnis sortiert.

**Hinweis:** Außer den Standardparametern sind Parameter, die zwischen Aktionen in der Sequenz übergeben werden, sind explizit.
Daher sind die Parameter, die an die Aktionssequenz übergeben werden, nur für die erste Aktion in der Sequenz verfügbar.
Das Ergebnis der ersten Aktion in der Sequenz wird zu dem JSON-Eingabeobjekt für die zweite Aktion in der Sequenz usw.
Das Objekt enthält keine Parameter, die ursprünglich an die Sequenz übergeben wurden, es sei denn, die erste Aktion schließt sie explizit in ihr Ergebnis ein.
Die Eingabeparameter für eine Aktion werden mit den Standardparametern der Aktion zusammengeführt. Erstere haben Vorrang und überschreiben alle übereinstimmenden Standardparameter.
Weitere Informationen zum Aufrufen von Aktionssequenzen mit mehreren benannten Parametern finden Sie unter [Standardparameter festlegen](./openwhisk_actions.html#openwhisk_binding_actions).

## Python-Aktionen erstellen
{: #creating-python-actions}

Das Verfahren zur Erstellung von Python-Aktionen ist dem von JavaScript-Aktionen ähnlich. In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen Python-Aktion sowie zum Übergeben von Parametern an diese Aktion beschrieben.

### Python-Aktion erstellen und aufrufen
{: #openwhisk_actions_python_invoke}

Eine Aktion ist eine einfache Python-Funktion der höchsten Ebene. Erstellen Sie zum Beispiel eine Datei mit dem Namen `hello.py` und dem folgenden Quellcode:

```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
    print(greeting)
    return {"greeting": greeting}
```
{: codeblock}

Python-Aktionen lesen stets ein Wörterverzeichnis (Dictionary) und generieren ein Wörterverzeichnis. Die Eingangsmethode für die Aktion ist standardmäßig `main`. Sie kann jedoch wie bei jedem anderen Aktionstyp explizit zum Erstellen der Aktion über die Befehlszeilenschnittstelle `wsk` mit der Option `--main` angegeben werden.

Sie können eine OpenWhisk-Aktion mit dem Namen `helloPython` wie folgt aus dieser Funktion erstellen:
```
wsk action create helloPython hello.py
```
{: pre}

Die CLI leitet den Typ der Aktion automatisch aus der Erweiterung der Quellendatei ab. Für `.py`-Quellendateien wird die Aktion in einer Laufzeit mit Python 2 ausgeführt. Sie können auch eine Aktion erstellen, die mit Python 3 ausgeführt wird, indem Sie den Parameter `--kind python:3` explizit angeben.
Darüber hinaus gibt es eine Python 3-Laufzeit mit der Art `python-jessie:3`, die zusätzliche Pakete für IBM Cloud Services wie IBM Cloudant, IBM DB2, IBM COS und IBM Watson enthält.
Weitere Informationen zu Paketen in dieser Python 3-Laufzeit finden Sie in den [Referenzinformationen zur Python-Laufzeit](./openwhisk_reference.html#openwhisk_ref_python_environments).

Der Aktionsaufruf für Python-Aktionen entspricht dem für JavaScript-Aktionen:
```
wsk action invoke --result helloPython --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

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
wsk action create helloPython --kind python:3 helloPython.zip
```
{: pre}

Diese Schritte werden zwar für Python 3 (mit der Art `python:3`) gezeigt, können jedoch genau so mit den anderen Python-Arten `python:2` oder `python-jessie:3` ausgeführt werden.


### Packen Sie Python-Aktionen mit einer virtuellen Umgebung in ZIP-Dateien.
{: #openwhisk_actions_python_virtualenv}

Eine andere Methode zum Packen von Python-Abhängigkeiten ist die Verwendung einer virtuellen Umgebung (`virtualenv`). In dieser Umgebung können Sie zusätzliche Pakete verknüpfen, die zum Beispiel über [`pip`](https://packaging.python.org/installing/) installiert werden können.


Wie bei der grundlegenden ZIP-Dateiunterstützung muss der Name der Quellendatei, die den Haupteingangspunkt enthält, `__main__.py` sein. Zur Verdeutlichung: Der Inhalt von `__main__.py` ist die Hauptfunktion, sodass Sie für dieses Beispiel `hello.py` in `__main__.py` umbenennen können (siehe vorherigen Abschnitt). Außerdem muss das virtualenv-Verzeichnis den Namen `virtualenv` haben. Das nachfolgende Beispielszenario zeigt die Installation von Abhängigkeiten, das Packen in einem Verzeichnis für die virtuelle Umgebung (virtualenv) sowie das Erstellen einer kompatiblen OpenWhisk-Aktion.

Um die Kompatibilität mit dem OpenWhisk-Laufzeitcontainer sicherzustellen, müssen Paketinstallationen in einer virtuellen Umgebung (virtualenv) in der Zielumgebung unter Verwendung des der Art entsprechenden Image erfolgen.
- Für die Art `python:2` verwenden Sie das Docker-Image `openwhisk/python2action`.
- Für die Art `python:3` verwenden Sie das Docker-Image `openwhisk/python3action`.
- Für die Art `python-jessie:3` verwenden Sie das Docker-Image `ibmfunctions/action-python-v3`.

1. Bei einer angegebenen Datei mit dem Namen [requirements.txt ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://pip.pypa.io/en/latest/user_guide/#requirements-files), die die `pip`-Module und -Versionen für die Installation enthält, führen Sie den folgenden Befehl aus, um die Abhängigkeiten zu installieren und eine virtuelle Umgebung (virtualenv) unter Verwendung eines kompatiblen Docker-Image zu erstellen:
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
    ```
    {: pre}

2. Archivieren Sie das virtualenv-Verzeichnis und alle weiteren Python-Dateien:
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

3. Erstellen Sie die Aktion:
    ```
    wsk action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}


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

PHP-Aktionen verarbeiten stets eine assoziative Feldgruppe und geben auch eine assoziative Feldgruppe zurück. Die Eingangsmethode für die Aktion ist standardmäßig `main`. Sie kann jedoch wie bei jedem anderen Aktionstyp explizit beim Erstellen der Aktion über die Befehlszeilenschnittstelle `wsk` mit der Option `--main` angegeben werden.

Sie können eine OpenWhisk-Aktion mit dem Namen `helloPHP` wie folgt aus dieser Funktion erstellen:

```
wsk action create helloPHP hello.php
```
{: pre}

Die CLI leitet den Typ der Aktion automatisch aus der Erweiterung der Quellendatei ab. Für `.php`-Quellendateien wird die Aktion in einer Laufzeit mit PHP 7.1 ausgeführt. Weitere Informationen finden Sie in den [Referenzinformationen zu PHP](./openwhisk_reference.html#openwhisk_ref_php).

Der Aktionsaufruf für PHP-Aktionen entspricht dem für JavaScript-Aktionen:

```
wsk action invoke --result helloPHP --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

### PHP-Aktionen in ZIP-Dateien packen
{: #openwhisk_actions_php_zip}

Sie können eine PHP-Aktion zusammen mit anderen Dateien und abhängigen Paketen in eine ZIP-Datei packen.
Der Dateiname der Quellendatei, die den Eingangspunkt (z. B. `main`) enthält, muss `index.php` lauten.
Beispiel: Zum Erstellen einer Aktion, die eine zweite Datei mit der Bezeichnung `helper.php` enthält, erstellen Sie zuerst ein Archiv mit den Quellendateien:

```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

Anschließend erstellen Sie die Aktion:

```bash
wsk action create helloPHP --kind php:7.1 helloPHP.zip
```
{: pre}

## Swift-Aktionen erstellen
{: #creating-swift-actions}

Das Verfahren zur Erstellung von Swift-Aktionen ist dem von JavaScript-Aktionen ähnlich. In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen Swift-Aktion sowie zum Übergeben von Parametern an diese Aktion beschrieben.

Sie können Ihren Swift-Code auch online mithilfe der [Swift-Sandbox](https://swiftlang.ng.bluemix.net) testen, ohne Xcode auf Ihrer Maschine installieren zu müssen.

### Aktion erstellen und aufrufen

Eine Aktion ist eine einfache Swift-Funktion der höchsten Ebene. Erstellen Sie zum Beispiel eine Datei mit dem Namen
`hello.swift` und dem folgenden Inhalt:

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

Swift-Aktionen lesen stets ein Wörterverzeichnis (Dictionary) und generieren ein Wörterverzeichnis. 

Sie können eine {{site.data.keyword.openwhisk_short}}-Aktion mit dem Namen `helloSwift` wie folgt aus dieser Funktion erstellen:

```
wsk action create helloSwift hello.swift --kind swift:3.1.1
```
{: pre}
 

Geben Sie immer `swift:3.1.1` an, da vorherige Swift-Versionen nicht unterstützt werden.
{: tip}

Der Aktionsaufruf für Swift-Aktionen entspricht dem für JavaScript-Aktionen:

```
wsk action invoke --result helloSwift --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

**Achtung:** Swift-Aktionen, die in einer Linux-Umgebung ausgeführt werden, befinden sich noch in Entwicklung und {{site.data.keyword.openwhisk_short}} arbeitet in der Regel mit dem neuesten verfügbaren Release, das jedoch nicht unbedingt stabil ist. Darüber hinaus ist es möglich, dass die mit {{site.data.keyword.openwhisk_short}} verwendete Version von Swift nicht mit den Versionen von Swift aus stabilen Releases von XCode on MacOS konsistent ist.

### Aktion als ausführbare Swift-Datei packen
{: #openwhisk_actions_swift_zip}

Wenn Sie eine OpenWhisk-Swift-Aktion mit einer Swift-Quellendatei erstellen, muss diese in eine Binärdatei kompiliert werden, bevor die Aktion ausgeführt wird. Danach werden Aufrufe der Aktion viel schneller durchgeführt, bis der Container, in dem die Aktion enthalten ist, bereinigt wird. Diese Verzögerung wird als Kaltstartverzögerung bezeichnet.

Zur Vermeidung der Kaltstartverzögerung können Sie Ihre Swift-Datei in eine Binärdatei kompilieren und anschließend in einer ZIP-Datei in OpenWhisk hochladen. Da Sie das OpenWhisk-Scaffolding benötigen, ist es am einfachsten, die Binärdatei innerhalb derselben Umgebung zu erstellen, in der sie ausgeführt wird. Sehen Sie sich die folgenden Schritte an:

- Führen Sie einen interaktiven Container für Swift-Aktionen mit dem folgenden Befehl aus:
  ```
  docker run --rm -it -v "$(pwd):/owexec" openwhisk/action-swift-v3.1.1 bash
  ```
  {: pre}
  
- Kopieren Sie den Quellcode und bereiten Sie den Build vor:
  ```
  cp /owexec/hello.swift /swift3Action/spm-build/main.swift 
  ```
  {: pre}

  ```
  cat /swift3Action/epilogue.swift >> /swift3Action/spm-build/main.swift
  ```
  {: pre}

  ```
  echo '_run_main(mainFunction:main)' >> /swift3Action/spm-build/main.swift
  ```
  {: pre}

- (Optional) Erstellen Sie die Datei `Package.swift`, um Abhängigkeiten hinzuzufügen.
   ```
   swift import PackageDescription
   
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
   {: pre}

  In diesem Beispiel werden die Abhängigkeiten `swift-watson-sdk` und `example-package-deckofplayingcards` hinzugefügt.
  Beachten Sie, dass `CCurl`, `Kitura-net` und `SwiftyJSON` in der Standard-Swift-Aktion zur Verfügung gestellt werden, sodass Sie sie in Ihre eigene Datei `Package.swift` einfügen können.

- Kopieren Sie die Datei 'Package.swift' in das Verzeichnis 'spm-build'.
  ```
  cp /owexec/Package.swift /swift3Action/spm-build/Package.swift
  ```
  {: pre}

- Wechseln Sie in das Verzeichnis 'spm-build'.
  ```
  cd /swift3Action/spm-build
  ```
  {: pre}

- Kompilieren Sie Ihre Swift-Aktion.
  ```
  swift build -c release
  ```
  {: pre}

- Erstellen Sie das ZIP-Archiv.
  ```
  zip /owexec/hello.zip .build/release/Action
  ```
  {: pre}

- Beenden Sie den Docker-Container.
  ```
  exit
  ```
  {: pre}

Sie können sehen, dass die Datei 'hello.zip' im selben Verzeichnis wie die Datei 'hello.swift' erstellt wurde. 

- Laden Sie sie mit dem Aktionsnamen 'helloSwifty' in OpenWhisk hoch:
  ```
  wsk action update helloSwiftly hello.zip --kind swift:3.1.1
  ```
  {: pre}

- Führen Sie den folgenden Befehl aus, um zu prüfen, wie viel schneller die Aktion ist: 
  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

Die Zeit, die zur Ausführung der Aktion benötigt wurde, ist in der Eigenschaft 'duration' (Dauer) enthalten. Vergleichen Sie diese mit der Zeit, die zur Ausführung mit einem Kompilierungsschritt in der Aktion 'hello' benötigt wird.

## Java-Aktionen erstellen
{: #creating-java-actions}

Das Verfahren zur Erstellung von Java-Aktionen ist dem von JavaScript- und Swift-Aktionen ähnlich. In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen Java-Aktion sowie zum Übergeben von Parametern an diese Aktion beschrieben.

Damit Sie Java-Dateien kompilieren, testen und archivieren können, muss lokal ein [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) installiert sein.

### Aktion erstellen und aufrufen
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

Aus dieser JAR-Datei können Sie folgendermaßen eine OpenWhisk-Aktion
namens `helloJava` erstellen:

```
wsk action create helloJava hello.jar --main Hello
```
{: pre}

Bei Verwendung der Befehlszeile und einer JAR-Quellendatei (`.jar`) brauchen Sie nicht anzugeben, dass Sie eine Java-Aktion erstellen. Das Tool bestimmt dies anhand der Dateierweiterung.

Sie müssen den Namen der Hauptklasse durch `--main` angeben. Eine infrage kommende Hauptklasse ist eine Klasse, die eine statische Methode `main` implementiert. Wenn sich die Klasse nicht im Standardpaket befindet, verwenden Sie den vollständig qualifizierten Java-Klassennamen (Beispiel: `--main com.example.MyMain`).

Sie können den Methodennamen Ihrer Java-Aktion auch anpassen, wenn dies erforderlich ist. Dies geschieht, indem Sie den vollständig qualifizierten Namen der Java-Methode Ihrer Aktion angeben. Beispiel: `--main com.example.MyMain#methodName`.

Der Aktionsaufruf für Java-Aktionen stimmt mit dem für Swift- und JavaScript-Aktionen überein:

```
wsk action invoke --result helloJava --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

## Docker-Aktionen erstellen
{: #creating-docker-actions}

Bei {{site.data.keyword.openwhisk_short}}-Docker-Aktionen können Sie Ihre Aktionen in einer beliebigen Sprache schreiben.

Ihr Code wird in eine ausführbare Binärdatei kompiliert und in ein Docker-Image eingebettet. Das Binärprogramm interagiert mit dem System durch den Empfang von Eingaben über `stdin` und Ausgabe von Antworten über `stdout`.

Voraussetzung ist, dass Sie über ein Docker Hub-Konto verfügen. Rufen Sie zur Einrichtung einer kostenlosen Docker-ID und eines Kontos [Docker Hub](https://hub.docker.com) auf.

In den nachfolgenden Anweisungen wird die Docker-Benutzer-ID `janesmith` und das Kennwort `janes_password` verwendet. Wenn die CLI eingerichtet ist, bleiben drei Schritte übrig, um eine angepasste Binärdatei zur Verwendung durch {{site.data.keyword.openwhisk_short}} einzurichten. Anschließend kann das hochgeladene Docker-Image als Aktion verwendet werden.

1. Laden Sie das Docker-Gerüst (Skeleton) herunter. Sie können es über die CLI wie folgt herunterladen und installieren:

  ```
  wsk sdk install docker
  ```
  {: pre}

  Das Docker-Gerüst ist jetzt im aktuellen Verzeichnis installiert.
  
  ```
  ls dockerSkeleton/
  ```
  {: pre}

  ```
  Dockerfile      README.md       buildAndPush.sh example.c
  ```

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

  Beachten Sie, dass ein Teil der Datei `example.c` im Rahmen des Buildprozesses für das Docker-Image kompiliert wird, sodass Sie keine C-Kompilierung auf Ihrer Maschine benötigen.
  Tatsächlich lässt sich die Binärdatei in dem Container nicht ausführen, da die Formate nicht übereinstimmen, wenn Sie sie nicht auf einer kompatiblen Hostmaschine kompilieren.

  Ihr Docker-Container kann jetzt als OpenWhisk-Aktion verwendet werden.


  ```
  wsk action create example --docker janesmith/blackboxdemo
  ```
  {: pre}

  Beachten Sie die Verwendung von `--docker` zum Erstellen einer Aktion. Es wird angenommen, dass alle Docker-Images auf einem Docker Hub gehostet werden.
  Die Aktion kann wie alle anderen {{site.data.keyword.openwhisk_short}}-Aktionen aufgerufen werden. 

  ```
  wsk action invoke --result example --param payload Rey
  ```
  {: pre}

  ```json
  {
      "args": {
          "payload": "Rey"
      },
      "msg": "Hello from arbitrary C program!"
  }
  ```

  Zum Aktualisieren der Docker-Aktion führen Sie `buildAndPush.sh` aus, um das neueste Image auf Docker Hub hochzuladen. Dies ermöglicht dem System das Extrahieren Ihres neuen Docker-Images bei der nächsten Ausführung des Codes für Ihre Aktion. Wenn keine aktiven Container vorhanden sind, verwenden die Aufrufe das neue Docker-Image. Wenn jedoch ein aktiver Container vorhanden ist, der eine ältere Version Ihres Docker-Images verwendet, verwenden neue Aufrufe weiterhin dieses Image, solange Sie nicht den Befehl `wsk action update` ausführen. Damit wird dem System angezeigt, dass es für neue Aufrufe eine Docker-Pull-Operation ausführen muss, um Ihr neues Docker-Image abzurufen.

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  ```
  wsk action update example --docker janesmith/blackboxdemo
  ```
  {: pre}

  Weitere Informationen zur Erstellung von Docker-Aktionen finden Sie im Abschnitt mit den [Referenzinformationen](./openwhisk_reference.html#openwhisk_ref_docker).

  Die vorherige Version der Befehlszeilenschnittstelle unterstützte `--docker` ohne Parameter und der Imagename war ein positionsgebundenes Argument. Damit die Docker-Aktionen Initialisierungsdaten mithilfe einer Datei (ZIP-Datei) akzeptieren können, normalisieren Sie das Benutzererlebnis für Docker-Aktionen so, dass ein positionsgebundenes Argument (falls vorhanden) eine Datei (z. B. eine ZIP-Datei) sein muss. Der Name des Images muss nach der Option `--docker` angegeben werden. Auf Kundenwunsch wurde das Argument `--native` als Kurzform für `--docker openwhisk/dockerskeleton` hinzugefügt, damit ausführbare Dateien, die im Standard-Docker-Aktion-SDK ausgeführt werden, bequemer erstellt und bereitgestellt werden können.
  
  Beispiel: Dieses Lernprogramm erstellt eine ausführbare Binärdatei innerhalb des Containers im Verzeichnis `/action/exec`. Wenn Sie diese Datei in Ihr lokales Dateisystem kopieren und in die Datei `exec.zip` komprimieren, können Sie die folgenden Befehle verwenden, um eine Docker-Aktion zu erstellen, die die ausführbare Datei als Initialisierungsdaten empfängt. 

  ```
  wsk action create example exec.zip --native
  ```
  {: pre}

  Dies entspricht folgendem Befehl: 
  ```
  wsk action create example exec.zip --docker openwhisk/dockerskeleton
  ```
  {: pre}

## Aktionen mit beliebigen ausführbaren Dateien erstellen
{: #creating-actions-arbitrary}

Mit der Option `--native` können Sie anzeigen, ob ausführbare Dateien als OpenWhisk-Aktion ausgeführt werden können. Dies schließt `Bash-Scripts` oder auch übergreifend kompilierte Binärprogramme mit ein. Für die letztere besteht die Einschränkung darin, dass das Binärprogramm mit dem Image von `openwhisk/dockerskeleton` kompatibel sein muss.

## Go-Aktionen erstellen
{: #creating-go-actions}

Mit der Option `--native` können ausführbare Dateien als Aktion gepackt werden. Dies funktioniert für Go als Beispiel.
Wie bei Docker-Aktionen empfängt die ausführbare Go-Datei ein einzelnes Argument über die Befehlszeile.
Dabei handelt sich um eine Zeichenfolgeserialisierung des JSON-Objekts, das die Argumente für die Aktion darstellt.
Das Programm kann die Protokolle in `stdout` oder `stderr` ausgeben.
Die letzte Zeile der Ausgabe _muss_ der Konvention entsprechend ein in eine Zeichenfolge konvertiertes JSON-Objekt sein, das das Aktionsergebnis darstellt.

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

Speichern Sie den obigen Code in einer Datei mit dem Namen `sample.go` und kompilieren Sie sie für OpenWhisk. Die ausführbare Datei muss den Namen `exec` haben.
```bash
GOOS=linux GOARCH=amd64 go build -o exec
zip exec.zip exec
wsk action create helloGo --native exec.zip
```

Die Aktion kann wie jede andere Aktion ausgeführt werden.
```bash
wsk action invoke helloGo -r -p name gopher
{
    "msg": "Hello, gopher!"
}
```

Protokolle werden ebenfalls auf ähnliche Weise abgerufen.
```bash
wsk activation logs --last --strip
my first Go action.
```

## Aktionsausgabe überwachen
{: #watching-action-output}

{{site.data.keyword.openwhisk_short}}-Aktionen können von anderen Benutzern, als Reaktion auf verschiedene Ereignisse oder als Teil einer Aktionssequenz aufgerufen werden. In solchen Fällen kann es nützlich sein, die Aufrufe zu überwachen.

Sie können die Ausgabe von Aktionen, wenn sie aufgerufen werden, über die {{site.data.keyword.openwhisk_short}}-CLI beobachten.

1. Geben Sie den folgenden Befehl über eine Shell ein:
  ```
  wsk activation poll
  ```
  {: pre}

  Dieser Befehl startet eine Polling-Schleife, die kontinuierlich auf Protokolle aus Aktivierungen prüft.

2. Wechseln Sie zu einem anderen Fenster und rufen Sie eine Aktion auf:

  ```
  wsk action invoke /whisk.system/samples/helloWorld --param payload Bob
  ```
  {: pre}

  ```
  ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
  ```

3. Beobachten Sie das Aktivierungsprotokoll in dem Polling-Fenster:

  ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
  ```

  Immer wenn Sie das Dienstprogramm 'poll' ausführen, werden die Protokolle für Aktionen, die für Sie in OpenWhisk ausgeführt werden, in Echtzeit angezeigt.

## Unterstützung für große Anwendungen
{: #large-app-support}

Die maximale Codegröße für eine Aktion ist 48 MB. Anwendungen, die viele Drittanbietermodule, native Bibliotheken oder externe Tools enthalten, könnten auf diese Begrenzung stoßen.

Wenn Sie ein Paket (ZIP- oder JAR-Datei) erstellen, das größer als 48 MB ist, besteht die Lösung darin, das Laufzeitimage um Abhängigkeiten zu erweitern und anschießend eine einzelne Quellendatei oder ein kleineres Archiv als 48 MB zu verwenden.

Wenn zum Beispiel eine Docker-Laufzeit erstellt wird, die erforderliche gemeinsam genutzte Bibliotheken einschließt, müssen diese Abhängigkeiten nicht in der Archivdatei enthalten sein. Private Quellendateien können weiterhin in das Archiv eingebunden und während der Ausführung eingefügt werden.

Ein weiterer Vorteil der Verringerung von Archivdateigrößen besteht darin, dass sich auch Bereitstellungszeiten verbessern.

### Python-Beispiel

Im folgenden Python-Beispiel kann 'opencv' die Bibliothek `opencv-python` enthalten und dann die Binärdatei für 'opencv' im Betriebssystemimage installieren. Anschließend können Sie die Datei `requirements.txt` verwenden und den Befehl `pip install requirements.txt` ausführen, um das Image um weitere Python-Bibliotheken zu erweitern. Sie können die Datei `action.py` anschließend mit dem neuen Image verwenden.

### Node.js-Beispiel

Im folgenden Node.js-Beispiel können Sie zusätzliche Pakete im Betriebssystemimage installieren:

Installieren Sie 'opencv' mithilfe von `npm`:
```
npm install opencv
```
{: pre}

Wenn Sie eine Datei `package.json` haben, können Sie die Installation mithilfe von `npm` ganz ähnlich durchführen:
```
npm install package.json
```
{: pre}

Fahren Sie anschließend mit der Verwendung von `action.js` mit dem neuen Image fort.

## Aktionen auflisten
{: #listing-actions}

Sie können alle Aktionen auflisten, die mit dem folgenden Befehl erstellt wurden:

```
wsk action list
```
{: pre}

Je mehr Aktionen Sie schreiben, desto umfangreicher wird die Liste. Es kann daher hilfreich sein, zusammengehörige Aktionen in [Paketen](./openwhisk_packages.html) zu gruppieren. Mit der folgenden Befehlssyntax können Sie die Liste der Aktionen so filtern, dass nur Aktionen in einem bestimmten Paket angezeigt werden: 

```
wsk action list [PAKETNAME]
```
{: pre}

## Aktionen löschen
{: #deleting-actions}

Sie können eine Bereinigung durchführen, indem Sie Aktionen löschen, die nicht mehr verwendet werden sollen.

1. Führen Sie den folgenden Befehl zum Löschen einer Aktion aus:
  ```
  wsk action delete hello
  ```
  {: pre}

  ```
  ok: deleted hello
  ```

2. Überprüfen Sie, ob die Aktion nicht mehr in der Liste der Aktionen angezeigt wird.
  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  ```

## In der Aktionskomponente auf Aktionsmetadaten zugreifen
{: #accessing-action-metadata-within-the-action-body}

Die Aktionsumgebung enthält mehrere Eigenschaften, die für die aktive Aktion spezifisch sind.
Mit diesen kann die Aktion programmgestützt über die REST-API mit OpenWhisk-Assets arbeiten oder einen internen
Alarm auslösen, wenn die Aktion kurz davor ist, das zugeteilte Zeitbudget aufzubrauchen.
Auf die Eigenschaften kann über die Systemumgebung für alle unterstützten Laufzeiten zugegriffen werden:
Node.js-, Python-, Swift-, Java- und Docker-Aktionen bei Verwendung des Docker-Gerüsts für OpenWhisk.

* `__OW_API_HOST`: Der API-Host für die OpenWhisk-Bereitstellung, die diese Aktion ausführt.
* `__OW_API_KEY`: Der API-Schlüssel für das Subjekt, das die Aktion aufruft; dieser Schlüssel kann ein eingeschränkter API-Schlüssel sein.
* `__OW_NAMESPACE`: Der Namensbereich für die _Aktivierung_ (dieser kann sich von dem Namensbereich für die Aktion unterscheiden).
* `__OW_ACTION_NAME`: Der vollständig qualifizierte Name der ausgeführten Aktion.
* `__OW_ACTIVATION_ID`: Die Aktivierungs-ID für diese ausgeführte Aktionsinstanz.
* `__OW_DEADLINE`: Der näherungsweise berechnete Zeitpunkt, zu dem diese Aktion das gesamte Zeitkontingent aufbraucht (in Epochenmillisekunden).
