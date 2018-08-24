---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-23"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Aktionen erstellen und aufrufen
{: #openwhisk_actions}

Aktionen sind statusunabhängige Code-Snippets, die auf der {{site.data.keyword.openwhisk}}-Plattform ausgeführt werden. Eine Aktion kann zum Beispiel verwendet werden, um Gesichter in einem Bild zu erkennen, auf eine Datenbankänderung zu antworten, eine Gruppe von API-Aufrufen zu aggregieren oder einen Tweet zu posten.
{:shortdesc}

Aktionen können explizit aufgerufen oder als Reaktion auf ein Ereignis ausgeführt werden. In beiden Fällen führt jede Ausführung einer Aktion zu einem Aktivierungsdatensatz, der durch eine eindeutige Aktivierungs-ID identifiziert wird. Die Eingabe für eine Aktion und das Ergebnis einer Aktion bestehen jeweils aus einem Wörterverzeichnis aus Schlüssel/Wert-Paaren, wobei der Schlüssel eine Zeichenfolge und der Wert ein gültiger JSON-Wert ist. Aktionen können außerdem aus Aufrufen weiterer Aktionen oder aus einer definierten Folge von Aktionen bestehen.

Eine Aktion kann als JavaScript-, Swift-, Python- oder PHP-Funktion, als Java-Methode oder als beliebige binärkompatible ausführbare Datei, einschließlich Go-Programmen und angepassten ausführbaren Dateien, geschrieben werden, die in einen Docker-Container gepackt werden. In den folgenden Abschnitten erfahren Sie, wie Sie Aktionen in Ihrer bevorzugten Entwicklungsumgebung erstellen, aufrufen und debuggen.

## JavaScript-Aktionen erstellen
{: #creating-and-invoking-javascript-actions}

In den folgenden Abschnitten werden Sie in die Arbeit mit Aktionen in JavaScript eingeführt. Sie beginnen zunächst mit dem Erstellen und Aufrufen einer einfachen Aktion. Danach fügen Sie Parameter zu der Aktion hinzu und rufen diese Aktion dann mit Parametern auf. Anschließend legen Sie Standardparameter fest und rufen diese Parameter auf. Zum Abschluss erstellen Sie asynchrone Aktionen.

### Einfache JavaScript-Aktion erstellen und aufrufen
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

2. Erstellen Sie mit der JavaScript-Funktion eine Aktion namens `hello`.
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

4. Führen Sie die Aktion durch Ausführen eines blockierenden Aufrufs aus. Blockierende Aufrufe verwenden einen Typ von Anforderung/Antwort und warten, bis das Aktivierungsergebnis verfügbar ist. Der Wartezeitraum beträgt weniger als 60 Sekunden oder das [Zeitlimit](./openwhisk_reference.html#openwhisk_syslimits) der Aktion, je nachdem, welcher Wert kürzer ist.
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
    Tipps zur Überwachung von Aktivierungen enthält der Abschnitt [Aktionsausgabe überwachen](openwhisk_managing.html#monitor-action-output).
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

### Asynchrone Aktionen erstellen
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
    ibmcloud fn action create asyncAction asyncAction.js
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

3. Prüfen Sie, wie lange die Ausführung der Aktivierung bis zu ihrem Abschluss gedauert hat. Rufen Sie dazu das Aktivierungsprotokoll ab.

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

### Externe API mit Aktionen aufrufen
{: #openwhisk_apicall_action}

Die bisherigen Beispiele sind eigenständige JavaScript-Funktionen. Sie können jedoch auch eine Aktion erstellen, die eine externe API aufruft.
{: shortdesc}

Das folgende Beispiel ruft den APOD-Service der NASA auf (APOD = Astronomy Picture of the Day), der täglich ein Bild des Universums zur Verfügung stellt.

1. Speichern Sie den folgenden Code in einer Datei namens `apod.js`.
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

    Es wird ein Aufruf an die APOD-API der NASA durchgeführt und aus dem JSON-Ergebnis werden Felder extrahiert. Weitere Informationen dazu, welche Node.js-Pakete Sie in Ihren Aktionen verwenden können, finden Sie in [Systemdetails und Grenzwerte](./openwhisk_reference.html#openwhisk_ref_javascript_environments).

2. Erstellen Sie eine Aktion namens `apod`.
    ```
    ibmcloud fn action create apod apod.js
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

### Aktion als Node.js-Modul packen
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
    * Diese Datei muss entweder den Namen **index.js** erhalten oder Sie müssen den für diese Datei bevorzugten Dateinamen in **package.json** in der Eigenschaft `main` angeben.

3. Installieren Sie alle Abhängigkeiten lokal.
    ```
    npm install
    ```
    {: pre}
    **Hinweis**: Beachten Sie, dass die meisten `npm`-Pakete zwar JavaScript-Quellen mit `npm install` installieren, andere jedoch auch Binärartefakte installieren und kompilieren. Der Upload von Archivdateien unterstützt derzeit nur JavaScript-Abhängigkeiten. Wenn das Archiv binäre Abhängigkeiten enthält, können Aktionsaufrufe fehlschlagen.

4. Erstellen Sie ein `.zip`-Archiv, das alle Dateien (einschließlich aller Abhängigkeiten) enthält.
    ```
    zip -r action.zip *
    ```
    {: pre}

    Die Verwendung der Windows Explorer-Aktion zur Erstellung der ZIP-Datei führt zu einer falschen Struktur. {{site.data.keyword.openwhisk_short}}-ZIP-Aktionen müssen `package.json` am Stammelement der ZIP-Datei aufweisen, während Windows Explorer die Datei in einem verschachtelten Ordner ablegt. Am sichersten ist die Verwendung des Befehlszeilenbefehls `zip`.
    {: tip}

5. Erstellen Sie die Aktion. Bei der Erstellung einer Aktion aus einem `.zip`-Archiv müssen Sie explizit einen Wert für das Flag `--kind` angeben, d. h. `nodejs:6` oder `nodejs:8`.
```
    ibmcloud fn action create packageAction --kind nodejs:6 action.zip
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

### Aktion als einzelnes Bundle packen
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
        ibmcloud fn action update my-action dist/bundle.js
        ```
        {: pre}

    **Hinweis**: Die Bundledatei, die von `webpack` erstellt wird, unterstützt nur JavaScript-Abhängigkeiten. Daher schlagen Aktionsaufrufe möglicherweise fehl, wenn das Bundle von binären Abhängigkeiten abhängig ist, weil diese nicht in die Datei `bundle.js` eingeschlossen sind.

## Python-Aktionen erstellen
{: #creating-python-actions}

In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen Python-Aktion sowie zum Hinzufügen von Parametern an diese Aktion beschrieben.

### Python-Aktion erstellen und aufrufen
{: #openwhisk_actions_python_invoke}

Eine Aktion ist eine einfache Python-Funktion der höchsten Ebene. So erstellen Sie eine Python-Aktion:

1. Speichern Sie den folgenden Code in einer Datei namens `hello.py`.
    ```python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
        print(greeting)
    return {"greeting": greeting}
    ```
    {: codeblock}

  * Python-Aktionen lesen stets ein Wörterverzeichnis (Dictionary) und generieren ein Wörterverzeichnis.
  * Die Eingangsmethode für die Aktion ist standardmäßig `main`. Sie kann jedoch explizit zum Erstellen der Aktion über die Befehlszeilenschnittstelle `wsk` mit dem Flag `--main` angegeben werden.

2. Erstellen Sie eine Aktion namens `helloPython`.
    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    Beispielausgabe:
    ```
    ok: created action helloPython
    ```
    {: screen}

    Die CLI leitet den Typ der Aktion automatisch aus der Erweiterung der Quellendatei ab. Für `.py`-Quellendateien wird die Aktion in einer Laufzeit mit Python 2 ausgeführt. Sie können auch eine Aktion erstellen, die mit Python 3 ausgeführt wird, indem Sie den Parameter `--kind python:3` explizit angeben. Sie können auch die Python 3-Laufzeit mit der Art `python-jessie:3` verwenden, die zusätzliche Pakete für IBM Cloud Services wie {{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.Db2_on_Cloud_long_notm}}, {{site.data.keyword.cos_full_notm}} und {{site.data.keyword.ibmwatson_notm}} enthält.
Weitere Informationen zu Paketen in dieser Python 3-Laufzeit finden Sie in den [Referenzinformationen zur Python-Laufzeit](./openwhisk_reference.html#openwhisk_ref_python_environments).

3. Rufen Sie die Aktion auf.
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

Sie können eine Python-Aktion und abhängige Module in eine ZIP-Datei packen. So erstellen Sie zum Beispiel eine Aktion mit einem Helper-Modul mit dem Namen `helper.py`:

1. Erstellen Sie ein Archiv, das Ihre Quellendateien enthält. **Hinweis**: Die Quellendatei, die den Eingangspunkt enthält, muss `__main__.py` heißen.
    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. Erstellen Sie die Aktion.
    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

### Python-Aktionen mit einer virtuellen Umgebung in ZIP-Dateien packen
{: #openwhisk_actions_python_virtualenv}

Zum Packen von Python-Abhängigkeiten können Sie eine virtuelle Umgebung (`virtualenv`) verwenden. Die virtuelle Umgebung ermöglicht Ihnen, zusätzliche Pakete zu verknüpfen, die zum Beispiel mit [`pip` ![Symbl für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://packaging.python.org/installing/) installiert werden können.

Um Abhängigkeiten zu installieren, müssen Sie sie in einer virtuellen Umgebung packen und eine kompatible OpenWhisk-Aktion erstellen:

1. Erstellen Sie eine Datei namens [requirements.txt ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://pip.pypa.io/en/latest/user_guide/#requirements-files), die die `pip`-Module und Versionen enthält, die installiert werden sollen.

2. Installieren Sie die Abhängigkeiten und erstellen Sie eine virtuelle Umgebung. Das Verzeichnis für die virtuelle Umgebung muss den Namen `virtualenv` aufweisen. Um die Kompatibilität mit dem OpenWhisk-Laufzeitcontainer sicherzustellen, müssen Paketinstallationen in einer virtuellen Umgebung das Image verwenden, das der Art entspricht.
    - Für die Art `python:2` verwenden Sie das Docker-Image `openwhisk/python2action`.
    - Für die Art `python:3` verwenden Sie das Docker-Image `openwhisk/python3action`.
    - Für die Art `python-jessie:3` verwenden Sie das Docker-Image `ibmfunctions/action-python-v3`.
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp &&virtualenv virtualenv &&source virtualenv/bin/activate &&pip install -r requirements.txt"
    ```
    {: pre}

3. Packen Sie das Verzeichnis `virtualenv` und alle weiteren Python-Dateien. Die Quellendatei, die den Eingangspunkt enthält, muss `__main__.py` heißen.
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. Erstellen Sie die Aktion `helloPython`.
    ```
    ibmcloud fn action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}

Fügen Sie nur solche Module zu der Datei `requirements.txt` hinzu, die nicht Teil der ausgewählten Laufzeitumgebung sind. Auf diese Weise kann die virtuelle Umgebung (`virtualenv`) möglichst klein gehalten werden.
{: tip}

## PHP-Aktionen erstellen
{: #creating-php-actions}

In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen PHP-Aktion sowie zum Hinzufügen von Parametern zu dieser beschrieben.

### PHP-Aktion erstellen und aufrufen
{: #openwhisk_actions_php_invoke}

Eine Aktion ist eine einfache PHP-Funktion der höchsten Ebene. So erstellen Sie eine PHP-Aktion:

1. Speichern Sie den folgenden Code in einer Datei namens `hello.php`.
    ```
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

    * PHP-Aktionen verarbeiten stets eine assoziative Feldgruppe und geben auch eine assoziative Feldgruppe zurück.
    * Die Eingangsmethode für die Aktion ist standardmäßig `main`. Sie kann jedoch explizit beim Erstellen der Aktion über die Befehlszeilenschnittstelle `ibmcloud` mit dem Flag `--main` angegeben werden.

2. Erstellen Sie eine Aktion namens `helloPHP`.
    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    Die CLI leitet den Typ der Aktion automatisch aus der Erweiterung der Quellendatei ab. Für `.php`-Quellendateien wird die Aktion in einer Laufzeit mit PHP 7.1 ausgeführt. Weitere Informationen finden Sie in den [Referenzinformationen zu PHP](./openwhisk_reference.html#openwhisk_ref_php).

3. Rufen Sie die Aktion auf.
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

Sie können eine PHP-Aktion und andere Dateien oder abhängige Pakete in eine ZIP-Datei packen. So erstellen Sie zum Beispiel eine Aktion mit einer zweiten Datei namens `helper.php`:

1. Erstellen Sie ein Archiv, das Ihre Quellendateien enthält. **Hinweis**: Die Quellendatei, die den Eingangspunkt enthält, muss `index.php` heißen.
    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. Erstellen Sie die Aktion.
    ```bash
    ibmcloud fn action create helloPHP --kind php:7.1 helloPHP.zip
    ```
    {: pre}

## Swift-Aktionen erstellen
{: #creating-swift-actions}

In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen Swift-Aktion sowie zum Packen einer Aktion in eine ZIP-Datei beschrieben.

**Hinweis:** Swift-Aktionen werden in einer Linux-Umgebung ausgeführt. Swift unter Linux befindet sich noch in der Entwicklung und {{site.data.keyword.openwhisk_short}} verwendet das neueste verfügbare Release. Diese Releases sind möglicherweise nicht stabil. Darüber hinaus ist es möglich, dass die mit {{site.data.keyword.openwhisk_short}} verwendete Version von Swift nicht mit den Versionen von Swift aus stabilen Releases von Xcode unter MacOS konsistent ist.

Weitere Informationen zur Swift-Laufzeit enthalten die [Referenzinformationen](./openwhisk_reference.html#swift-actions) zu Swift.
{: tip}

### Aktion erstellen und aufrufen
{: #openwhisk_actions_swift_invoke}

#### Swift 3
{: #openwhisk_actions_swift3_invoke}

Eine Aktion ist eine einfache Swift-Funktion der höchsten Ebene. So erstellen Sie eine Swift 3-Aktion:

1. Speichern Sie den folgenden Code in einer Datei namens `hello.swift`.
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

2. Erstellen Sie eine Aktion namens `helloSwift`.
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

    Die CLI leitet den Typ der Aktion automatisch aus der Erweiterung der Quellendatei ab. Für `.php`-Quellendateien wird die Aktion in einer Laufzeit mit PHP 7.1 ausgeführt. Weitere Informationen finden Sie in den [Referenzinformationen zu PHP](./openwhisk_reference.html#openwhisk_ref_php).

3. Rufen Sie die Aktion auf.
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

#### Swift 4
{: #openwhisk_actions_swift4_invoke}

Neben der Hauptfunktionssignatur stellt Swift 4 zwei weitere Signaturen bereit, die den Typ [Codable ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.apple.com/documentation/swift/codable) nutzen. Weitere Informationen zu Datentypen, die zur Kompatibilität mit externen Darstellungen wie JSON codierbar und decodierbar sind, finden Sie [hier ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types).

1. Speichern Sie den folgenden Code in einer Datei namens `hello.swift`.
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
    Dieses Beispiel verwendet einen Eingabeparameter als `Codable-Eingabe` mit dem Feld `name` und gibt eine `Codable-Ausgabe` mit einem Feld `greetings` zurück.

2. Erstellen Sie eine Aktion namens `helloSwift`.
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.1
    ```
    {: pre}

3. Rufen Sie die Aktion auf.
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

### Aktion als ausführbare Swift-Datei packen
{: #packaging-an-action-as-a-swift-executable}

Wenn Sie eine {{site.data.keyword.openwhisk_short}}-Swift-Aktion mit einer Swift-Quellendatei erstellen, muss die Datei in eine Binärdatei kompiliert werden, bevor die Aktion ausgeführt wird. Diese Verzögerung wird als Kaltstartverzögerung bezeichnet. Nachdem die Binärdatei erst einmal erstellt worden ist, werden Aufrufe der Aktion viel schneller durchgeführt, bis der Container, der die Aktion enthält, bereinigt wird. Zur Vermeidung der Kaltstartverzögerung können Sie Ihre Swift-Datei in eine Binärdatei kompilieren und diese anschließend in einer ZIP-Datei in {{site.data.keyword.openwhisk_short}} hochladen.

Sie können ein Script verwenden, um das Packen der Aktion zu automatisieren.

**Voraussetzung**: Für das in den folgenden Schritten verwendete Script wird vorausgesetzt, dass ein Verzeichnis namens `actions` vorhanden ist, bei dem jedes Verzeichnis der höchsten Ebene jeweils eine Aktion darstellt.
```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```

1. Speichern Sie den folgenden Code in einer Scriptdatei namens `compile.sh`.
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

2. Wenn Sie Abhängigkeiten hinzufügen wollen, erstellen Sie die Datei `Package.swift`. Im folgenden Beispiel wird `example-package-deckofplayingcards` als Abhängigkeit hinzugefügt. `CCurl`, `Kitura-net` und `SwiftyJSON` werden in der Standard-Swift-Aktion zur Verfügung gestellt und sollten daher nur für Swift 3-Aktionen in Ihre eigene Datei `Package.swift` eingefügt werden.
    * Swift 3 - Beispielsyntax:
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

    * Swift 4 - Beispielsyntax:
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

3. Erstellen Sie die Aktion, um eine Datei `hello.zip` in `build` zu erstellen.
    * Swift 3:
      ```
      bash compile.sh hello swift:3.1.1
      ```
      {: pre}

    * Swift 4:
      ```
      bash compile.sh hello swift:4.1
      ```
      {: pre}

4. Laden Sie die ZIP-Datei mit dem Aktionsnamen `helloSwiftly` in {{site.data.keyword.openwhisk_short}} herunter.
    * Swift 3:
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:3.1.1
      ```
      {: pre}

    * Swift 4:
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:4.1
      ```
      {: pre}

5. Rufen Sie die Aktion auf.
    ```
    ibmcloud fn action invoke helloSwiftly --blocking
    ```
    {: pre}

    Die Zeit, die für die Ausführung der Aktion benötigt wurde, wird in der Eigenschaft 'duration1' (Dauer) angegeben.

6. Sie können die Zeitspanne für die Ausführung des vorkompilierten Aktionsaufrufs mit der Zeit vergleichen, die für die Ausführung eines Befehlsaufrufs mit einem Kompilierungsschritt benötigt wird. Rufen Sie die Aktion aus dem vorherigen Abschnitt auf:
    ```
    ibmcloud fn action invoke --result helloSwift --param name World --blocking
    ```
    {: pre}

### Fehlerbehandlung in Swift 4
{: #error-handling-swift4}

Anhand des neuen Codable-Completion-Handlers können Sie einen Fehler übergeben, um auf einen Fehler in Ihrer Aktion hinzuweisen. Die [Fehlerbehandlung in Swift ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) ähnelt der Behandlung von Ausnahmebedingungen in anderen Sprachen, in denen die Schlüsselwörter `try`, `catch` und `throw` verwendet werden.

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

In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen Java-Aktion sowie zum Hinzufügen von Parametern zu dieser beschrieben.

Damit Sie Java-Dateien kompilieren, testen und archivieren können, muss lokal ein [JDK 8 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://openjdk.java.net/install) installiert sein.

### Java-Aktion erstellen und aufrufen
{: #openwhisk_actions_java_invoke}

Eine Java-Aktion ist ein Java-Programm mit einer Methode namens `main`. Die Methode `main` muss genau die folgende Signatur aufweisen:
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

So erstellen Sie eine Java-Aktion:

1. Speichern Sie den folgenden Code in einer Datei namens `Hello.java`.
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

2. Kompilieren Sie `Hello.java` in eine JAR-Datei namens `hello.jar`. **Hinweis**: [google-gson ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/google/gson) muss in Ihrem CLASSPATH für Java vorhanden sein.
    ```
    javac Hello.java
    ```
    {: pre}
    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}

3. Erstellen Sie eine Aktion.
    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}
  * Sie müssen den Namen der Hauptklasse mithilfe von `--main` angeben. Eine infrage kommende Hauptklasse ist eine Klasse, die eine statische Methode `main` implementiert. Wenn sich die Klasse nicht im Standardpaket befindet, verwenden Sie den vollständig qualifizierten Java-Klassennamen (Beispiel: `--main com.example.MyMain`).
  * Sie können den Methodennamen Ihrer Java-Aktion anpassen. Diese Anpassung erfolgt, indem Sie den vollständig qualifizierten Namen Ihrer Aktion angeben. Beispiel: `--main com.example.MyMain#methodName`.
  * Die CLI leitet den Typ der Aktion automatisch aus der Erweiterung der Quellendatei ab.

4. Rufen Sie die Aktion auf.
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
{: shortdesc}

Ihr Code wird in eine ausführbare Binärdatei kompiliert und in ein Docker-Image eingebettet. Das Binärprogramm interagiert mit dem System durch den Empfang von Eingaben über `stdin` und Ausgabe von Antworten über `stdout`.   Weitere Informationen zur Erstellung von Docker-Aktionen finden Sie im Abschnitt mit den [Referenzinformationen](./openwhisk_reference.html#openwhisk_ref_docker).

Voraussetzung: Sie müssen über ein Docker Hub-Konto verfügen. Eine kostenlose Docker-ID mit entsprechendem Konto können Sie bei [Docker Hub ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://hub.docker.com) einrichten.

So richten Sie eine angepasste Binärdatei ein und verwenden das hochgeladene Docker-Image als Aktion:

1. Laden Sie das Docker-Gerüst (Skeleton) herunter und installieren Sie es. Das Gerüst ist eine Docker-Containervorlage, in die Sie Ihren Code in Form von angepassten Binärdateien einfügen können.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Richten Sie Ihre angepasste Binärdatei im Docker-Gerüst (Skeleton) ein. Das Gerüst schließt ein C-Programm ein, das Sie verwenden können. Ein Teil der Datei `example.c` wird im Rahmen des Buildprozesses für das Docker-Image kompiliert, sodass Sie keine C-Kompilierung auf Ihrer Maschine benötigen. 
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  Beispielausgabe:
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. Optional: Fügen Sie anhand entsprechender Änderungen an der `Dockerfile` zusätzlichen Code und Abhängigkeiten zum Docker-Image hinzu, um die ausführbare Datei zu erstellen. Beachten Sie die folgenden Voraussetzungen und Einschränkungen:
  * Die Binärdatei muss sich im Container unter `/action/exec` befinden.
  * Die Binärdatei empfängt ein einzelnes Argument über die Befehlszeile. Bei diesem Argument handelt es sich um eine Zeichenfolgeserialisierung des JSON-Objekts, das die Argumente für die Aktion darstellt.
  * Das Programm kann die Protokolle in `stdout` oder `stderr` ausgeben.
  * Die letzte Zeile der Ausgabe muss der Konvention entsprechend ein in eine Zeichenfolge konvertiertes JSON-Objekt sein, das das Aktionsergebnis darstellt.

4. Erstellen Sie das Docker-Image und laden Sie es mithilfe eines bereitgestellten Scripts hoch.
    1. Melden Sie sich bei Docker an.
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. Navigieren Sie zum Verzeichnis `dockerSkeleton`.
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. Führen Sie das Script aus.
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}

5. Verwenden Sie Ihren Docker-Container zum Erstellen einer Aktion.
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. Rufen Sie die Aktion auf.
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    Beispielausgabe:
    ```
    {
        "args": {
            "payload": "Rey"
        },
      "msg": "Hello from arbitrary C program!"
  }
    ```
    {: screen}

7. Laden Sie zum Aktualisieren der Docker-Aktion das neueste Image auf Docker Hub hoch. Dies ermöglicht dem System das Extrahieren Ihres neuen Docker-Images bei der nächsten Ausführung des Codes für Ihre Aktion.
    ```
    ./buildAndPush.sh <username>/blackboxdemo
    ```
    {: pre}

8. Wenn ein aktiver Container vorhanden ist, der eine ältere Version Ihres Docker-Images nutzt, verwenden alle neuen Aufrufe weiterhin dieses Image. Aktualisieren Sie die Aktion, sodass neue Aufrufe künftig das neue Image verwenden.
```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. Optional: Sie können das Argument `--native` als Kurzform für `--docker openwhisk/dockerskeleton` verwenden. Dieses Argument vereinfacht die Erstellung und Implementierung von ausführbaren Dateien, die innerhalb des standardmäßigen SDKs für Docker-Aktionen ausgeführt werden.
    1. In den Schritten oben wird eine ausführbare Binärdatei innerhalb des Containers im Verzeichnis `/action/exec` erstellt. Kopieren Sie die Datei `/action/exec` in Ihr lokales Dateisystem und komprimieren Sie sie in die Datei `exec.zip`.
    2. Erstellen Sie eine Docker-Aktion, die die ausführbare Datei als Initialisierungsdaten empfängt. Hierbei ersetzt das Argument `--native` das Argument `--docker openwhisk/dockerskeleton`.
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}

## Go-Aktionen erstellen
{: #creating-go-actions}

Mit dem Argument `--native` können Sie jede beliebige ausführbare Go-Datei als Aktion packen

Beachten Sie die folgenden Voraussetzungen und Einschränkungen:
  * Die ausführbare Go-Datei empfängt ein einzelnes Argument über die Befehlszeile. Bei diesem Argument handelt es sich um eine Zeichenfolgeserialisierung des JSON-Objekts, das die Argumente für die Aktion darstellt.
  * Das Programm kann die Protokolle in `stdout` oder `stderr` ausgeben.
  * Die letzte Zeile der Ausgabe muss der Konvention entsprechend ein in eine Zeichenfolge konvertiertes JSON-Objekt sein, das das Aktionsergebnis darstellt.

So erstellen Sie eine Go-Aktion:

1. Speichern Sie den folgenden Code in einer Datei namens `sample.go`.
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

        // Optionale Protokollierung an stdout (oder stderr)
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

2. Führen Sie Cross-Compiling für die Datei `sample.go` für {{site.data.keyword.openwhisk_short}} durch. Die ausführbare Datei muss den Namen `exec` haben.
    ```bash
    GOOS=linux GOARCH=amd64 go build -o exec
    zip exec.zip exec
    ibmcloud fn action create helloGo --native exec.zip
    ```
    {: codeblock}

3. Rufen Sie die Aktion auf.
    ```bash
    ibmcloud fn action invoke helloGo -r -p name gopher
    {
        "msg": "Hello, gopher!"
    }
    ```
    {: pre}

## Aktionssequenzen erstellen
{: #openwhisk_create_action_sequence}

Sie können eine Aktion erstellen, die eine Sequenz von Aktionen miteinander verkettet. Dabei wird das Ergebnis einer Aktion als Argument an die nächste Aktion übergeben.
{: shortdesc}

Im Paket `/whisk.system/utils` werden verschiedene Dienstprogrammaktionen bereitgestellt, die Sie zum Erstellen Ihrer ersten Sequenz verwenden können.

1. Listen Sie die Aktionen im Paket `/whisk.system/utils` auf.
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

2. Erstellen Sie mithilfe der Aktionen `split` und `sort` eine Aktionssequenz, sodass das Ergebnis von `split` als Argument an `sort` übergeben wird. Diese Aktionssequenz konvertiert Zeilen von Text in ein Array und sortiert die Zeilen.
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. Rufen Sie die Aktion auf.
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    In der Ausgabe werden die getrennten Zeilen alphabetisch sortiert.
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

**Hinweis**:
* Parameter, die zwischen den Aktionen in der Sequenz übergeben werden, sind (mit Ausnahme der Standardparameter) explizit. Daher sind die Parameter, die an die Aktionssequenz übergeben werden, nur für die erste Aktion in der Sequenz verfügbar. Das Ergebnis der ersten Aktion in der Sequenz wird zum JSON-Eingabeobjekt für die zweite Aktion in der Sequenz usw. Das Objekt enthält keine Parameter, die ursprünglich an die Sequenz übergeben wurden, es sei denn, die erste Aktion schließt sie explizit in ihr Ergebnis ein. Die Eingabeparameter für eine Aktion werden mit den Standardparametern der Aktion zusammengeführt. Erstere haben Vorrang und überschreiben alle übereinstimmenden Standardparameter. Weitere Informationen zum Aufrufen von Aktionssequenzen mit mehreren benannten Parametern finden Sie unter [Standardparameter für eine Aktion festlegen](./parameters.html#default-params-action).
* Eine Sequenz besitzt kein Gesamtzeitlimit, das jenseits der Zeitlimits für die einzelnen Aktionen innerhalb der Aktionssequenz gilt. Da es sich bei einer Sequenz um eine Hintereinanderschaltung oder Aneinanderkettung von Operationen ähnlich einer Pipeline handelt, bewirkt ein Fehler in einer Aktion die Unterbrechung dieser Pipeline. Wenn eine Aktion das Zeitlimit überschreitet, wird die gesamte Sequenz mit diesem Fehler beendet.

## Umfangreiche Aktionen verwalten
{: #large-app-support}

Die maximale Codegröße für eine Aktion ist 48 MB. Anwendungen, die viele Drittanbietermodule, native Bibliotheken oder externe Tools enthalten, erreichen diesen Grenzwert gegebenenfalls. Wenn Sie eine Aktion mit einem ZIP- oder JAR-Paket mit einer Größe von über 48 MB erstellen, müssen Sie das Laufzeitimage um Abhängigkeiten erweitern und anschließend eine einzelne Quellendatei oder ein Archiv mit einer Größe von weniger als 48 MB verwenden.

Wenn zum Beispiel eine Docker-Laufzeit erstellt wird, die erforderliche gemeinsam genutzte Bibliotheken einschließt, müssen eventuelle Abhängigkeiten nicht in der Archivdatei enthalten sein. Private Quellendateien können weiterhin in das Archiv eingebunden und während der Ausführung eingefügt werden.

### Größe von Aktionen reduzieren
{: #large-app-reduce}

So reduzieren Sie die Codegröße einer Python-App:

1. Platzieren Sie die Bibliothek `opencv-python` in `opencv`.
2. Installieren Sie die Binärdatei für 'opencv' im Betriebssystemimage.
3. Erweitern Sie das Image durch Ausführen von `pip install requirements.txt` um weitere Python-Bibliotheken.
4. Verwenden Sie `action.py` mit dem neuen Image.

So reduzieren Sie die Codegröße einer Node.js-App:

1. Installieren Sie `opencv`.
   ```
   npm install opencv
   ```
   {: pre}

2. Installieren Sie `package.json`.
   ```
   npm install package.json
   ```
   {: pre}

3. Verwenden Sie `action.js` mit dem neuen Image.
