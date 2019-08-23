---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-10"

keywords: actions, serverless, javascript, node, node.js, functions

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
{:external: target="_blank" .external}


# Apps für Aktionen vorbereiten
{: #prep}

Unabhängig davon, ob Sie eine eigene App zur Verfügung stellen oder ob Sie ein Script speziell für die Reaktion auf ein Ereignis schreiben, muss Ihr Code bestimmte Anforderungen erfüllen, bevor Sie eine Aktion aus ihm erstellen können.
{: shortdesc}

Für jede Programmiersprache gelten bestimmte Voraussetzungen, aber die meisten haben die folgenden allgemeinen Anforderungen:
- Der erwartete Name für den Eingangspunkt in den Code ist standardmäßig `main`. Wenn Ihr Eingangspunkt nicht `main` ist, kann beim Erstellen der Aktion ein angepasster Name angegeben werden. Notieren Sie diesen Namen.
- Die Eingabeparameter in Ihre App und die Ausgabeergebnisse aus Ihrer App müssen mit einer bestimmte Struktur formatiert sein, die zwischen den Entitäten übergeben werden kann. Die Struktur hängt von Ihrer Programmiersprache ab. Bei Python-Apps müssen die Eingabeparameter beispielsweise ein Wörterverzeichnis (Dictionary) sein und das Ergebnis Ihrer App muss als Wörterverzeichnis strukturiert sein. Sie können Parameter jedoch auch in einem strukturierten Objekt an Ihre Aktion übergeben. In JSON können Sie beispielsweise Ihren Code so strukturieren, dass ein Eingabeparameter mit JSON-Werten bei bestimmten Feldern wie `name` und `place` erwartet wird. 

    **JSON-Eingabebeispiel**
    ```json
    {"name": "Dorothy", "place": "Kansas"}
    ```
    {: codeblock}

    **JavaScript-Beispiel**
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
    ```
    {: codeblock}
- Wenn Ihre App mehrere Dateien enthält, müssen sie zu einer einzigen Datei kombiniert werden, die dann in einer Aktion verwendet werden kann. Sie können Ihren Code entweder neu in eine Datei schreiben oder die Dateien und Abhängigkeiten in eine einzige Archivdatei packen. Wenn Ihre Laufzeit nicht unterstützt wird, können Sie Ihre App in ein Docker-Image packen.
- Auch die Abhängigkeiten müssen mit Ihrer App gepackt werden. Die verfügbaren Laufzeiten werden mit vorinstallierten Paketen und Erweiterungen bereitgestellt. [Ermitteln Sie anhand der Referenzinformationen für Ihre Laufzeit](/docs/openwhisk?topic=cloud-functions-runtimes), ob eine Abhängigkeit Ihrer App bereits in der Laufzeit enthalten ist. Wenn die Abhängigkeit eingeschlossen ist, müssen Sie sie nicht mit Ihrer App packen.

    Eine Kompilierung des Codes ist nicht erforderlich. Wenn die Laufzeit es ermöglicht, kann eine Kompilierung des Codes im Voraus aber zur Verbesserung der Leistung beitragen.
    {: tip}

## Apps in Docker-Images vorbereiten
{: #prep_docker}

Bei {{site.data.keyword.openwhisk_short}} können Sie Ihre App in jeder Sprache schreiben und als Docker-Image packen.
{: shortdesc}

Sie können Images nur aus öffentlichen Registrys abrufen, z. B. Images, die auf Docker Hub öffentlich verfügbar sind. Private Registrys werden nicht unterstützt.
{: important}

### Code in Docker-Images packen
{: #prep_docker_pkg}

Ihr Code wird in eine ausführbare Datei kompiliert und in ein Docker-Image eingebettet. Die ausführbare Datei interagiert mit dem System durch den Empfang von Eingaben über `stdin` und Ausgabe von Antworten über `stdout`.
{: shortdesc}

**Vorbereitende Schritte**
- Sie müssen über ein Docker Hub-Konto verfügen. Sie können eine kostenlose Docker-ID und ein Konto in [Docker Hub](https://hub.docker.com){: external} einrichten. 
- [Installieren Sie Docker](https://hub.docker.com/search/?offering=community&type=edition){:external}.
- [Überprüfen Sie die Anforderungen für die Docker-Laufzeit](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker).

Führen Sie die folgenden Schritte aus, um Ihre App zu packen. 

Führen Sie den folgenden Befehl aus, um Ihren Code als Docker-Image zu packen. 
1. Laden Sie das Docker-Gerüst (Skeleton) herunter und installieren Sie es. Das Gerüst ist eine Docker-Containervorlage, in die Sie Ihren Code in Form von angepassten Binärdateien einfügen können.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Fügen Sie Ihren Code in das Blackbox-Gerüst ein. Das Gerüst schließt ein C-Programm ein, das Sie verwenden können. Ein Teil der Datei `example.c` wird im Rahmen des Buildprozesses für das Docker-Image kompiliert, sodass Sie keine C-Kompilierung auf Ihrer Maschine benötigen.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  **Beispielausgabe**
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. (Optional) Fügen Sie anhand entsprechender Änderungen an der `Dockerfile` zusätzlichen Code und Abhängigkeiten zum Docker-Image hinzu, um die ausführbare Datei zu erstellen. Beachten Sie die folgenden Anforderungen:
  * Ihr Code muss sich im Container unter `/action/exec` befinden. 
  * Die ausführbare Datei empfängt ein einzelnes Argument über die Befehlszeile. Bei diesem Argument handelt es sich um eine Zeichenfolgeserialisierung des JSON-Objekts, das die Argumente für die Aktion darstellt.
  * Das Programm kann die Protokolle in `stdout` oder `stderr` ausgeben.
  * Die letzte Zeile der Ausgabe muss der Konvention entsprechend ein <ph class="ignoreSpelling">in eine Zeichenfolge konvertiertes</ph> JSON-Objekt sein, das das Ergebnis der Aktion darstellt. Weitere Informationen zum Erstellen von Dockerfiles finden Sie in der Veröffentlichung [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/){: external}.

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




## JavaScript-Apps vorbereiten
{: #prep_js}

Bevor Sie eine Aktion erstellen, müssen Sie den JavaScript-Code vorbereiten. Stellen Sie sicher, dass Ihr Code richtig strukturiert ist, und entscheiden Sie dann, ob er gepackt werden muss.
{: shortdesc}

### JavaScript-Code strukturieren
{: #prep_js_struct}

- Der erwartete Name für die Eingangspunktfunktion ist `main`. Wenn die Funktion in Ihrem Code nicht `main` ist, notieren Sie den Namen, um ihn anzugeben, wenn die Aktion erstellt wird. 
- Die Eingabeparameter werden als JSON-Objekt übergeben.
- Das Ergebnis einer erfolgreichen Aktivierung ist ebenfalls ein JSON-Objekt. Dieses Objekt wird jedoch unterschiedlich zurückgegeben, je nachdem, ob die Aktion [synchron](#prep_js_sync) oder [asynchron](#prep_js_async) ausgeführt wird.



**Beispiel**
```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```

**Beispiel mit mehreren Funktionen**

  ```javascript
  function main() {
      return { payload: helper() }
  }

  function helper() {
      return new Date();
}
  ```
  {: codeblock}


### JavaScript-Code mit synchronem Verhalten strukturieren
{: #prep_js_sync}

Die JavaScript-Aktivierung ist synchron, wenn die Funktion 'main' entweder ohne Ausführung einer Anweisung `return` beendet wird oder wenn sie beendet wird, indem eine Anweisung `return` ausgeführt wird, die einen beliebigen Wert mit Ausnahme eines Promise-Objekts zurückgibt.
{: shortdesc}

**Beispiel für synchronen Code. **

```javascript
// Jeder Pfad führt zu einer synchronen Aktivierung
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




### JavaScript-Code mit asynchronem Verhalten strukturieren
{: #prep_js_async}

JavaScript-Funktionen können die Ausführung in einer Callback-Funktion fortsetzen, auch nachdem ihre Ausführung beendet ist. Die Aktivierung einer JavaScript-Aktion ist asynchron, wenn die Funktion 'main' mit der Rückgabe eines Promise-Objekts beendet wird. In diesem Fall nimmt das System an, dass die Aktion solange weiter ausgeführt wird, bis das Promise-Objekt erfüllt oder abgelehnt wurde. JavaScript-Funktionen, die asynchron ausgeführt werden, können das Aktivierungsergebnis zurückgeben, nachdem die Funktion `main` die Steuerung zurückgegeben hat, indem sie ein Promise-Objekt in Ihrer Aktion zurückgeben.
{: shortdesc}

Beginnen Sie, indem Sie ein neues Promise-Objekt instanziieren und eine Callback-Funktion übergeben. Der Callback verwendet zwei Argumente ('resolve' und 'reject'), die beide Funktionen sind. Der gesamte asynchrone Code geht in den Callback ein. Der Aktionshandler kann einen beliebigen Namen haben, solange dieser der üblichen Signatur für die Annahme und die Rückgabe eines Objekts (oder eines `Promise`-Objekts) entspricht.

Das folgende Beispiel zeigt, wie ein Promise-Objekt durch Aufruf der Funktion 'resolve' erfüllt wird.
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

Das folgende Beispiel zeigt, wie ein Promise-Objekt durch Aufruf der Funktion 'reject' zurückgewiesen wird.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 2000);
     })
}
```
{: codeblock}

In den Beispielen werden die folgenden Details ausgeführt. 
* Die Funktion `main` gibt ein Promise-Objekt zurück. Dieses Promise-Objekt weist darauf hin, dass die Aktivierung zwar noch nicht abgeschlossen wurde, ihr Abschluss erwartet wird.
* Die JavaScript-Funktion `setTimeout()` wartet 2 Sekunden, bevor sie die Callback-Funktion des Promise-Objekts aufruft, die den asynchronen Code darstellt.
* Die Callback-Funktion des Promise-Objekts akzeptiert die Argumente `resolve` und `reject`, die jeweils Funktionen sind.
  * Der Aufruf von `resolve()` erfüllt das Promise-Objekt und weist darauf hin, dass die Aktivierung normal abgeschlossen wird.
  * Mit einem Aufruf von `reject()` kann das Promise-Objekt zurückgewiesen und signalisiert werden, dass die Aktivierung abnormal beendet wird.


### JavaScript-Code mit synchronem und asynchronem Verhalten strukturieren
{: #prep_js_both}

Es ist möglich, dass eine Aktion für einige Eingaben synchron und für andere Eingaben asynchron ausgeführt wird, wie das folgende Beispiel zeigt.
{: shortdesc}

```javascript
function main(params) {
     if (params.payload) {
        // asynchrone Aktivierung
         return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
         }, 2000);
      })
     }  else {
        // synchrone Aktivierung
         return {done: true};
      }
}
```
{: codeblock}





### Beispiel für den Aufruf einer externen API mit JavaScript
{: #prep_js_api}

Das folgende Beispiel ruft die externe API für den APOD-Service der NASA auf (APOD = Astronomy Picture of the Day), der täglich ein Bild des Universums zur Verfügung stellt.
{: shortdesc}


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

Es wird ein Aufruf an die APOD-API der NASA durchgeführt und aus dem JSON-Ergebnis werden Felder extrahiert.

Als Nächstes müssen Sie die [Aktion erstellen](/docs/openwhisk?topic=cloud-functions-actions) und [aufrufen](/docs/openwhisk?topic=cloud-functions-test), um sie zu testen. Es wird das folgende Beispielobjekt zurückgegeben:

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






### JavaScript-Code mit dem `webpack`-Modul packen
{: #prep_js_pkg}

Sie können eine App mit einem Bundler für JavaScript-Module wie `[webpack ](https://webpack.js.org/concepts/){: external}` packen. Wenn der Modul-Bundler `webpack` Ihren Code verarbeitet, erstellt er rekursiv ein Abhängigkeitsdiagramm, das jedes für Ihre Aktion erforderliche Modul einschließt.
{: shortdesc}

Bevor Sie beginnen, [prüfen Sie die Pakete, die in der JavaScript-Laufzeit enthalten sind](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments), um zu ermitteln, ob eine Abhängigkeit Ihrer App bereits in der Laufzeit enthalten ist. Wenn die Abhängigkeit nicht eingeschlossen ist, müssen Sie sie mit Ihrer App packen.

1. Erstellen Sie eine Datei `package.json`. Fügen Sie `webpack` als Entwicklungsabhängigkeit hinzu.

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

2. Speichern Sie den folgenden `webpack`-Konfigurationscode in einer Datei mit dem Namen `webpack.config.js`. 

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

3. Bereiten Sie Ihren App-Code vor. In diesem Beispiel, das Sie als Datei mit dem Namen `index.js` speichern können, wird die Variable `global.main` als Funktion 'main' der App festgelegt. 

    **Beispiel**
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

5. Erstellen Sie das `webpack`-Bundle. 

    ```
    npm run build
    ```
    {: pre}

    Die Datei `dist/bundle.js` wird erstellt und als Quellcode der Aktion bereitgestellt.

6. Erstellen Sie die Aktion mit dem Script `npm` oder über die Befehlszeilenschnittstelle (CLI).

    * Führen Sie das folgende `npm`-Script aus. 

        ```
        npm run deploy
        ```
        {: pre}

    * Führen Sie den folgenden CLI-Befehl aus. 

        ```
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    Die Bundledatei, die von `webpack` erstellt wird, unterstützt nur JavaScript-Abhängigkeiten. Aktionsaufrufe schlagen möglicherweise fehl, wenn das Bundle über andere Abhängigkeiten verfügt, da diese Abhängigkeiten nicht in der Datei `bundle.js` enthalten sind.
    {: tip}



### JavaScript-Code als NPM-Dateien packen
{: #prep_js_npm}

Als Alternative zum Schreiben des gesamten Aktionscodes in einer einzigen JavaScript-Quellendatei können Sie Ihren Code als `npm`-Paket in einer Datei mit der Erweiterung .zip packen.
{: shortdesc}

Bevor Sie beginnen, [prüfen Sie die Pakete, die in der JavaScript-Laufzeit enthalten sind](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments), um zu ermitteln, ob eine Abhängigkeit Ihrer App bereits in der Laufzeit enthalten ist. Wenn die Abhängigkeit nicht eingeschlossen ist, müssen Sie sie mit Ihrer App packen.

1. Erstellen Sie im Stammverzeichnis eine Datei `package.json`. 

**Beispiel**

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

2. Installieren Sie alle Abhängigkeiten lokal.

    ```
    npm install DEPENDENCY
    ```
    {: pre}

    Die meisten `npm`-Pakete installieren zwar JavaScript-Quellen mit `npm install`. Andere Pakete installieren und kompilieren jedoch auch binäre Dateiartefakte. Der Upload von Archivdateien unterstützt nur JavaScript-Abhängigkeiten. Wenn das Archiv binäre Dateiabhängigkeiten enthält, sind die Aktionsaufrufe möglicherweise nicht erfolgreich.
    {: note}

3. Erstellen Sie ein `.zip`-Archiv, das alle Dateien einschließlich aller Abhängigkeiten enthält. 

    ```
    zip -r action.zip *
    ```
    {: pre}

    **Windows-Benutzer** Die Verwendung der Windows Explorer-Aktion zur Erstellung der .zip-Datei führt zu einer falschen Dateistruktur. {{site.data.keyword.openwhisk_short}}-ZIP-Aktionen müssen `package.json` am Stammelement des Archivs aufweisen, aber Windows Explorer legt die Datei in einem verschachtelten Ordner ab. Verwenden Sie stattdessen den Befehl `zip`.
    {: tip}





## Go-Apps vorbereiten
{: #prep_go}

Verwenden Sie eine einzelne Datei für schnelle Test- oder Entwicklungszwecke. Für Produktionsapps müssen Sie Ihre Go-Aktionen in einer ausführbaren Datei vorkompilieren, um eine bessere Leistung oder Unterstützung für mehrere Quellendateien, einschließlich Anbieterbibliotheken, zu realisieren.
{: shortdesc}

Sie können zwar auf jeder Go-Plattform eine komprimierte Datei durch Cross-Compiling mit `GOOS=Linux` und `GOARCH=amd64` erstellen. Dennoch sollten Sie das Vorkompilierungsfeature verwenden, das im Container-Image der Laufzeit eingebettet ist. Sie können [mehrere Quellendateien](#prep_go_multi) oder [Anbieterbibliotheken](#prep_go_vendor) packen.
{: tip}


### Go-Code strukturieren
{: #prep_go_struct}

- Der erwartete Name für das Eingangspunktpaket ist `main`. Wenn das Paket in Ihrem Code nicht `main` ist, notieren Sie den Namen, um ihn anzugeben, wenn die Aktion erstellt wird.
- Das Paket muss öffentlich sein.

**Beispiel**
```go
    package main

    import "fmt"

    // Main ist die Funktion zur Implementierung der Aktion
    func Main(params map[string]interface{}) map[string]interface{} {
        // Eingabe-JSON parsen
        name, ok := params["name"].(string)
        if !ok {
            name = "World"
        }
        msg := make(map[string]interface{})
        msg["body"] = "Hello " + name + "!"
        // optionale Protokollierung in stdout (oder stderr)
        fmt.Println("hello Go action")
        // Ausgabe-JSON zurückgeben
        return msg
    }
  ```
  {: codeblock}

### Mehrere Go-Quellendateien packen
{: #prep_go_multi}

1. Erstellen Sie ein `src`-Verzeichnis der höchsten Ebene. Legen Sie die Quellendateien, die zum Hauptpaket gehören, im Stammverzeichnis von `src` oder in einem `main`-Verzeichnis ab und erstellen Sie Unterverzeichnisse für andere Pakete. Das Paket `hello` wird beispielsweise zum Verzeichnis `src/hello`.
  ```
  go-action-hello/
  └── src
      ├── hello
      │   └── hello.go
      └── main
          └── main.go
  ```
  {: screen}

2. Importieren Sie Unterpakete. Beispiel für `main/main.go` für den Import des Unterpakets 'hello'. 

  ```go
  package main

  import (
  	"fmt"
  	"hello"
  )

  // Main-Weiterleitung zu Hello
func Main(args map[string]interface{}) map[string]interface{} {
  	fmt.Println("This is main.Main")
	greetings := "World"
	name, ok := args["name"].(string)
	if ok {
  		greetings = name
  	}
  	return hello.Hello(greetings)
  }
  ```
  {: codeblock}

  Beispiel für `hello/hello.go`. 

  ```go
  package hello

  import "fmt"

  // Hello gibt eine Begrüßungsnachricht zurück
func Hello(name string) map[string]interface{} {
  	fmt.Println("This is hello.Hello")
	res := make(map[string]interface{})
	res["body"] = "Hello " + name
	return res
}
  ```
  {: codeblock}

3. Kompilieren Sie den Code. Erstellen Sie ein ZIP-Archiv des `src`-Verzeichnisses. Schließen Sie nicht das Projektverzeichnis auf der höchsten Ebene, `go-action-project/`, ein. 

  ```bash
  cd src
zip -r ../hello-src.zip *
cd ..
  ```
  {: pre}

  Sie können eine lokale Kompilierung durchführen, indem Sie `GOPATH` auf das übergeordnete Element des `src`-Verzeichnisses setzen. Wenn Sie VS Code verwenden, müssen Sie die Einstellung für `go.inferGopath` in `true` ändern.
  {: note}

4. Kompilieren und packen Sie die ausführbare Go-Datei als `exec` in das Stammverzeichnis des ZIP-Archivs. Erstellen Sie das Archiv `hello-bin.zip`, indem Sie den folgenden Befehl ausführen. Sie müssen die Docker-Befehlszeilenschnittstelle in Ihrer Workstation installiert und `docker` in `PATH` angegeben haben.

  ```bash
  docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
  ```
  {: pre}

  In diesem Beispiel lautet die 'main'-Funktion `-compile main`. Um eine andere 'main'-Funktion zu verwenden, ändern Sie den Wert `-compile`. Die 'main'-Funktion wird zur Kompilierzeit ausgewählt. Bei einer Vorkompilierung ignoriert `ibmcloud fn action [update | create]` das Element `--main`.

  Der Container erhält die Inhalte der Quellen-ZIP-Datei in `stdin`, kompiliert sie und erstellt ein neues ZIP-Archiv mit der ausführbaren Datei (`exec`) im Stammverzeichnis. Der Inhalt des
ZIP-Archivs wird an `stdout` gestreamt, was wiederum in das Archiv `hello-bin.zip` weitergeleitet wird, um als Go-Aktion bereitgestellt zu werden.




### Go-Code mit Anbieterbibliotheken packen
{: #prep_go_vendor}

Sie können Abhängigkeiten einschließen, indem Sie ein Verzeichnis `vendor` im Quellen-ZIP-Archiv (`zip`) befüllen, wenn Sie die Go-Dateien kompilieren. Das Verzeichnis `vendor` funktioniert nicht auf höchster Ebene. Sie müssen das Verzeichnis `vendor` in `src/` und in einem Paketverzeichnis platzieren.
{: shortdesc}

Beispielprotokollpaket `logrus` in einer `hello.go`-App. 

```go
package hello

import (
	"os"

	"github.com/Sirupsen/logrus"
)

var log = logrus.New()

func init() {
	log.Out = os.Stdout
}

// Hello gibt eine Begrüßungsnachricht zurück
func Hello(name string) map[string]interface{} {
	log.WithFields(logrus.Fields{"greetings": name}).Info("Hello")
	res := make(map[string]interface{})
	res["body"] = "Hello, " + name
	return res
}
```
{: codeblock}

</br>
In diesem Beispiel befindet sich das Verzeichnis `vendor` in `src/hello/vendor`. Sie können Bibliotheken anderer Anbieter hinzufügen, die vom Paket `hello` verwendet werden. 

Sie können verschiedene Tools wie [<code>dep</code>](https://golang.github.io/dep/docs/installation.html){: external} verwenden, um Abhängigkeiten zu füllen und zu verwalten. 

Mit `dep` können Sie eine Datei `src/main/Gopkg.toml` erstellen, die die Version und Position der Bibliotheken beschreibt.

```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

Füllen Sie das Verzeichnis `vendor`, führen Sie `dep ensure` aus.




## Swift-Apps vorbereiten
{: #prep_swift}

Swift-Dateien müssen kompiliert werden, bevor eine Aktion ausgeführt wird. Diese Verzögerung wird als Kaltstartverzögerung bezeichnet. Zur Vermeidung der Kaltstartverzögerung können Sie Ihre Swift-Datei kompilieren und anschließend in einer .zip-Datei in {{site.data.keyword.openwhisk_short}} hochladen. Die Docker-Laufzeit enthält einen Compiler, der Sie beim Kompilieren und Packen von Swift 4.2-Aktionen unterstützt. Nachfolgende Aufrufe der Aktion werden viel schneller ausgeführt, bis der Container mit Ihrer Aktion bereinigt wird. 

Swift-Aktionen werden in einer Linux-Umgebung ausgeführt. Swift unter Linux befindet sich noch in der Entwicklung und {{site.data.keyword.openwhisk_short}} verwendet das neueste verfügbare Release. Diese Releases sind möglicherweise nicht stabil. Die mit {{site.data.keyword.openwhisk_short}} verwendete Swift-Version kann mit Swift-Versionen aus stabilen Releases von Xcode auf Mac OS inkonsistent sein.
{: important}


### Swift-Code strukturieren
{: #prep_swift_struc}

Der erwartete Name für die Eingangspunktfunktion ist `main`. Wenn die Funktion in Ihrem Code nicht `main` ist, notieren Sie den Namen, um ihn anzugeben, wenn die Aktion erstellt wird. 

Neben der Hauptfunktionssignatur stellt Swift 4 zwei weitere Signaturen bereit, die den Typ [<code>Codable</code>](https://developer.apple.com/documentation/swift/codable){: external} nutzen. Sie können mehr über Datentypen erfahren, die [für Kompatibilität mit externen Darstellungen wie JSON codierbar und decodierbar sind](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types){: external}. 

**Beispiel**
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


#### Fehler in Swift behandeln
{: #prep_swift_error}

Mit dem Completion-Handler `Codable` können Sie einen Fehler übergeben, um auf einen Fehler in Ihrer Aktion hinzuweisen. Die [Fehlerbehandlung in Swift](https://docs.swift.org/swift-book/LanguageGuide/ErrorHandling.html){: external} ähnelt der Behandlung von Ausnahmebedingungen in anderen Sprachen, in denen die Schlüsselwörter `try`, `catch` und `throw` verwendet werden.
{: shortdesc}

Das folgende Snippet zeigt ein Beispiel für die Fehlerbehandlung.

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


### Swift 4.2-Datei packen
{: #prep_swift42_single}

Kompilieren Sie eine einzelne Quellendatei ohne Abhängigkeiten von externen Bibliotheken. Verwenden Sie das Flag `-compile` mit dem Namen der Hauptmethode.

**Vorbereitende Schritte**
- [Installieren Sie Docker](https://hub.docker.com/search/?offering=community&type=edition){: external}.
- [Prüfen Sie die Pakete, die in der Swift-Laufzeit enthalten sind](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions), um zu ermitteln, ob eine Abhängigkeit Ihrer App bereits in der Laufzeit enthalten ist. Wenn die Abhängigkeit nicht eingeschlossen ist, müssen Sie sie mit Ihrer App packen.

Packen Sie Ihre App. 

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

Der Docker-Container liest den Inhalt der Datei aus `stdin` und schreibt ein ZIP-Archiv mit der kompilierten ausführbaren Swift-Datei in `stdout`.



### Abhängigkeiten und Projekte mit mehreren Dateien für Swift 4.2 packen
{: #prep_swift42_multi}

**Vorbereitende Schritte**
- [Installieren Sie Docker](https://hub.docker.com/search/?offering=community&type=edition){: external}.
- [Prüfen Sie die Pakete, die in der Swift-Laufzeit enthalten sind](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions), um zu ermitteln, ob eine Abhängigkeit Ihrer App bereits in der Laufzeit enthalten ist. Wenn die Abhängigkeit nicht eingeschlossen ist, müssen Sie sie mit Ihrer App packen.

Packen Sie Ihre App. 

1. Erstellen Sie die folgende Verzeichnisstruktur, um mehrere Dateien zu kompilieren und externe Abhängigkeiten einzuschließen.

  ```
  .
  ├── Package.swift
  └── Sources
      └── main.swift
  ```
  {: codeblock}

  Das Verzeichnis `Sources/` enthält eine Datei mit dem Namen `main.swift`. 

  `Package.swift` muss mit einem Kommentar beginnen, der die Version `4.2` für die Swift-Tools angibt: 

  ```swift
  // swift-tools-version:4.2
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
      .package(url: "https://github.com/IBM-Swift/SwiftyRequest.git", .upToNextMajor(from: "1.0.0"))
      ],
      targets: [
      .target(
        name: "Action",
        dependencies: ["SwiftyRequest"],
        path: "."
    )
      ]
  )
  ```
  {: codeblock}

2. Erstellen Sie ein ZIP-Archiv mit dem Inhalt des Verzeichnisses. 

  ```bash
  zip ../action-src.zip -r *
  ```
  {: codeblock}

3. Übergeben Sie das ZIP-Archiv an den Docker-Container über `stdin`. `stdout` ist ein neues ZIP-Archiv mit der kompilierten ausführbaren Datei. Der Docker-Container liest den Inhalt des ZIP-Archivs aus `stdin` und schreibt ein neues ZIP-Archiv mit der kompilierten ausführbaren Swift-Datei in `stdout`.

  ```
  docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
  ```
  {: codeblock}

  Auf einem Linux-basierten System können Sie die Schritte `zip` und `docker run` in einem einzigen Befehl kombinieren:

  ```
  zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
  ```
  {: codeblock}





## Python-Apps packen
{: #prep_python}


### Python-Code strukturieren
{: #prep_python_struct}

- Python-Apps müssen stets ein Wörterverzeichnis (Dictionary) lesen und ein Wörterverzeichnis generieren.
- Der erwartete Name für die Eingangspunktmethode ist `main`. Wenn die Funktion in Ihrem Code nicht `main` ist, notieren Sie den Namen, um ihn anzugeben, wenn die Aktion erstellt wird.
{: shortdesc}

**Beispiel**
```python
def main(args):
	name = args.get("name", "stranger")
	greeting = "Hello " + name + "!"
	print(greeting)
    return {"greeting": greeting}
```

### Python-Code packen
{: #prep_python_pkg}

Sie können Python-Code und abhängige Module in eine .zip-Datei packen. In diesem Beispiel ist `__main__.py` die Quellendatei, die den Eingangspunkt enthält, und die Helper-Module befinden sich in einer Datei mit dem Namen `helper.py`.

Bevor Sie beginnen, [prüfen Sie die Pakete, die in der Python-Laufzeit enthalten sind](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments), um zu ermitteln, ob eine Abhängigkeit Ihrer App bereits in der Laufzeit enthalten ist. Wenn die Abhängigkeit nicht eingeschlossen ist, müssen Sie sie mit Ihrer App packen.

Führen Sie den folgenden Befehl aus, um Ihre App zu packen. 

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}


### Python-Code mit einer virtuellen Umgebung in ZIP-Dateien packen
{: #prep_python_virtenv}

Zum Packen von Python-Abhängigkeiten können Sie eine virtuelle Umgebung (`virtualenv`) verwenden. Durch Verwendung der virtuellen Umgebung können Sie weitere Pakete verknüpfen, die mithilfe von [`pip`](https://packaging.python.org/tutorials/installing-packages/){: external} installiert werden können. 

Bevor Sie beginnen, [prüfen Sie die Pakete, die in der Python-Laufzeit enthalten sind](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments), um zu ermitteln, ob eine Abhängigkeit Ihrer App bereits in der Laufzeit enthalten ist. Wenn die Abhängigkeit nicht eingeschlossen ist, müssen Sie sie mit Ihrer App packen.

Führen Sie die folgenden Schritte aus, um Ihre App zu packen. 

1. Erstellen Sie eine Datei [requirements.txt](https://pip.pypa.io/en/latest/user_guide/#requirements-files){: external}, die die zu installierenden `pip`-Module und -Versionen enthält. 

  Um `virtualenv` auf einer Mindestgröße zu halten, fügen Sie nur solche Module zu der Datei `requirements.txt` hinzu, die nicht Teil der ausgewählten Laufzeitumgebung sind. Weitere Informationen zu den Paketen, die in Python-Laufzeiten enthalten sind, finden Sie in der Python-[Laufzeitreferenz](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments).
  {: tip}

2. Extrahieren Sie eines der folgenden Images per Pull-Operation für Ihre Laufzeit. Um die Kompatibilität mit dem Laufzeitcontainer sicherzustellen, müssen gepackte Installationen in einer virtuellen Umgebung das Image verwenden, das der angegebenen Laufzeit entspricht.
    * Für `python:3.7` verwenden Sie das Docker-Image `ibmfunctions/action-python-v3.7`.
    * Für `python:3.6` verwenden Sie das Docker-Image `ibmfunctions/action-python-v3.6`.
    * Für `python:2` verwenden Sie das Docker-Image `openwhisk/python2action`.

   **Beispiel**
   ```
   docker pull ibmfunctions/action-python-v3.7
   ```
   {: pre}

2. Installieren Sie die Abhängigkeiten und erstellen Sie eine virtuelle Umgebung. Das Verzeichnis für die virtuelle Umgebung muss den Namen `virtualenv` aufweisen.

   ```
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. Packen Sie das Verzeichnis `virtualenv` und alle weiteren Python-Dateien. Die Quellendatei, die den Eingangspunkt enthält, muss `__main__.py` heißen.

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}


## Ruby-Apps vorbereiten
{: #prep_ruby}

Bevor Sie eine Aktion erstellen, müssen Sie den Ruby-Code vorbereiten.

### Ruby-Code strukturieren
{: #prep_ruby_struct}

* Ruby-Aktionen verarbeiten immer einen Hash (wörterbuchähnliche Sammlung) und geben einen Hash zurück.
* Der erwartete Name für die Eingangspunktfunktion ist `main`. Wenn die Funktion in Ihrem Code nicht `main` ist, notieren Sie den Namen, um ihn anzugeben, wenn die Aktion erstellt wird. 


**Beispiel**

```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
```
{: codeblock}

### Ruby-Code packen
{: #prep_ruby_pkg}

Sie können eine Ruby-App und abhängige Pakete in eine ZIP-Datei packen. Sie können beispielsweise eine Aktion mit einer zweiten Datei namens `helper.rb` packen.

Erstellen Sie ein Archiv, das Ihre Quellendateien enthält. Die Quellendatei, die den Eingangspunkt enthält, muss `main.rb` heißen.

```bash
zip -r helloRuby.zip main.rb helper.rb
```
{: pre}

Die Gems `mechanize` und `jwt` sind zusätzlich zu den standardmäßig bereitgestellten und den gebündelten Gems verfügbar. Sie können beliebige Gems verwenden, solange Sie gezippte Aktionen verwenden, um alle Abhängigkeiten zu packen.



## PHP-Apps vorbereiten
{: #prep_php}

Bevor Sie eine Aktion erstellen, müssen Sie den PHP-Code vorbereiten.

### PHP-Code strukturieren
{: #prep_php_struct}

- PHP-Aktionen verarbeiten stets eine assoziative Feldgruppe und geben auch eine assoziative Feldgruppe zurück.
- Der erwartete Name für die Eingangspunktfunktion ist `main`. Wenn die Funktion in Ihrem Code nicht `main` ist, notieren Sie den Namen, um ihn anzugeben, wenn die Aktion erstellt wird. 

**Beispiel**
```php
<?php
function main(array $args) : array
{
    $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
?>
```
{: codeblock}

### PHP-Code packen
{: #prep_php_pkg}

Sie können PHP-Dateien und abhängige Pakete in eine ZIP-Datei packen.

Bevor Sie beginnen, [prüfen Sie die Pakete, die in der PHP-Laufzeit enthalten sind](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php), um zu ermitteln, ob eine Abhängigkeit Ihrer App bereits in der Laufzeit enthalten ist. Wenn die Abhängigkeit nicht eingeschlossen ist, müssen Sie sie mit Ihrer App packen.

Führen Sie den folgenden Befehl aus, um Ihre App zu packen. 

```bash
zip -r ARCHIVE_NAME.zip FILE_1.php FILE_2.php
```
{: pre}

**Beispiel**
```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

## Java-Apps vorbereiten
{: #prep_java}

Bevor Sie eine Aktion erstellen, müssen Sie den Java-Code vorbereiten.

### Java-Code strukturieren
{: #prep_java_struct}

Eine Java-Aktion ist ein Java-Programm mit einer Methode namens `main`. Die Methode `main` muss die folgende Signatur aufweisen.

**Beispiel**
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

* Sie müssen den Namen der Hauptklasse mithilfe von `--main` angeben. Eine infrage kommende Hauptklasse ist eine Klasse, die eine statische Methode `main` implementiert. Wenn sich die Klasse nicht im Standardpaket befindet, verwenden Sie den vollständig qualifizierten Java-Klassennamen (Beispiel: `--main com.example.MyMain`).
* Sie können den Methodennamen Ihrer Java-Aktion anpassen, indem Sie den vollständig qualifizierten Methodennamen Ihrer Aktion angeben, z. B. `--main com.example.MyMain#methodName`. 

### Java-Code packen
{: #prep_java_pkg}


**Vorbereitende Schritte**
[JDK 8](http://openjdk.java.net/install/){: external} muss lokal installiert sein. In diesem Beispiel wird [`google-gson-2.8.5.jar`](http://central.maven.org/maven2/com/google/code/gson/gson/2.8.5/){: external} verwendet. 

Wenn Sie mit einer anderen JDK-Version als JDK 8 arbeiten, müssen Sie `--release 8` angeben, wenn Sie Ihren Code mit dem Befehl `javac` kompilieren.
{: note}

Führen Sie die folgenden Schritte aus, um eine Java-Aktion zu erstellen. 

1. Speichern Sie den folgenden Code in einer Datei namens `Hello.java`.

  ```java
  import com.google.gson.JsonObject;
    public class Hello {
      public static JsonObject main(JsonObject args) {
          String name = "stranger";
          if (args.has("name"))
              name = args.getAsJsonPrimitive("name").getAsString();
          JsonObject response = new JsonObject();
          response.addProperty("greeting", "Hello, " + name + "!");
        return response;
    }
  }
  ```
  {: codeblock}

2. Laden Sie die Datei [`gson-2.8.5.jar`](http://central.maven.org/maven2/com/google/code/gson/gson/2.8.5/) herunter. 

3. Fügen Sie die Datei `gson-2.8.5.jar` zu Ihrem `ClASSPATH`hinzu. In diesem Beispiel wird die Datei `gson-2.8.5.jar` verwendet, die in einem Ordner `test` im Verzeichnis `Desktop` gespeichert ist. 
  
  ```
  export CLASSPATH=$CLASSPATH:/Users/Desktop/test/gson-2.8.5.jar
  ```
  {: pre}

4. Fügen Sie den Ordner `bin` Ihres JDK zu Ihrem `CLASSPATH` hinzu. In diesem Beispiel wird `openjdk-8`verwendet. 
  
  ```
  export CLASSPATH=$CLASSPATH:/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/bin
  ```
  {: pre}

5. Überprüfen Sie, ob sich der JDK-Ordner `bin` und `gson-2.8.5.jar` in Ihrem `CLASSPATH` befinden. 
  ```
  echo $CLASSPATH
  ```
  {: pre}

  **Beispielausgabe**
  ```
  /Desktop/test/gson-2.8.5.jar:/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/bin
  ```
  {: screen}

6. Navigieren Sie zu dem Ordner, in dem Ihre Datei `Hello.java` gespeichert ist. In diesem Beispiel ist die Datei `Hello.java` in dem Ordner `Desktop/test` gespeichert. 
  ```
  cd Desktop/test
  ```
  {: pre}

7. Kompilieren Sie Ihre Datei `Hello.java` in eine Klassendatei. 
  ```
  javac Hello.java
  ```
  {: pre}

8. Komprimieren Sie die Klassendatei in eine .jar-Datei namens `hello.jar`. 

  ```
  jar cvf hello.jar Hello.class
  ```
  {: pre}

**Nächste Schritte**
Sie können eine Aktion mit der Datei `hello.jar` erstellen. Da die von Ihnen erstellte Klassendatei nicht den Standardnamen `main` verwendet, müssen Sie bei der Erstellung der Aktion das Flag `-- main` auf `Hello` setzen. Das Flag `-- main` muss mit Ihrer Java-Klasse (`class`) übereinstimmen. Weitere Informationen finden Sie in [Aktionen erstellen](/docs/openwhisk?topic=cloud-functions-actions). 
 
Wenn Sie Ihren Java-Code aktualisieren, müssen Sie diese Schritte wiederholen, um Ihren Code in eine neue `.jar`-Datei zu kompilieren.
{: note}

### Java-Code mit Gradle packen
{: #prep_java_gradle}

Anstatt über die Befehlszeile zu kompilieren, können Sie ein Build-Tool wie [Gradle](https://gradle.org){: external} verwenden, um die Bibliotheken aus einem Repository wie Maven Central abzurufen. Mit Gradle können Sie ein finales .jar-Archiv abrufen erstellen, das Ihren Code und alle Abhängigkeiten enthält. 

Im Folgenden finden Sie ein Beispiel für die Verwendung von Gradle zum Erstellen einer Java-Aktion, die die Bibliothek `com.google.zxing` nutzt, in der die Funktionalität zum Generieren eines QR-Code-Images enthalten ist.

1. Erstellen Sie eine Datei mit dem Namen `build.gradle` und geben Sie die Abhängigkeiten an. 

  ```gradle
  apply plugin: 'java'

  version = '1.0'

  repositories {
     mavenCentral()
  }

  configurations {
      provided
      compile.extendsFrom provided
  }

  dependencies {
       provided 'com.google.code.gson:gson:2.6.2'
      compile 'com.google.zxing:core:3.3.0'
       compile 'com.google.zxing:javase:3.3.0'
  }

  jar {
      dependsOn configurations.runtime

     from {
          (configurations.runtime - configurations.provided).collect {
              it.isDirectory() ? it : zipTree(it)
          }
      }
  }
  ```
  {: codeblock}

2. Führen Sie den Befehl `gradle jar` aus, der ein .jar-Archiv im Verzeichnis `build/libs/` generiert. 

  Weitere Informationen finden Sie in der Gradle-Dokumentation zum [Deklarieren von Abhängigkeiten](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies){: external}. 


## .NET Core-Apps vorbereiten
{: #prep_dotnet}

Bevor Sie eine Aktion erstellen, müssen Sie den .NET Core-Code vorbereiten.

### .NET Core-Code strukturieren
{: #prep_dotnet_struct}

Eine .NET Core-Aktion ist eine .NET Core-Klassenbibliothek mit einer Methode, die den Namen `Main` erhalten soll. Wenn die Methode in Ihrem Code nicht `main` ist, notieren Sie den Namen, um ihn anzugeben, wenn die Aktion erstellt wird. Format: `--main {Assembly}::{Class Full Name}::{Method}`

**Beispiel**
```
Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main
```

### .NET Core-Code packen
{: #prep_dotnet_pkg}

**Vorbereitende Schritte**
Um .NET Core-Projekte zu kompilieren, zu testen und zu archivieren, müssen Sie folgende Schritte ausführen: 
- Installieren Sie das [.NET Core-SDK](https://dotnet.microsoft.com/download){: external} lokal.
- Setzen Sie die Umgebungsvariable `DOTNET_HOME` auf die Position, an der sich die ausführbare Datei `dotnet` befindet.



Führen Sie die folgenden Befehle aus, um Ihren Code zu packen. 

  1. Erstellen Sie ein C#-Projekt namens `Apache.OpenWhisk.Example.Dotnet`.

      ```bash
      dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
      ```
      {: pre}

  2. Navigieren Sie zum Verzeichnis `Apache.OpenWhisk.Example.Dotnet`.

      ```bash
      cd Apache.OpenWhisk.Example.Dotnet
      ```
      {: pre}

  3. Installieren Sie das Paket [<ph class="ignoreSpelling">Newtonsoft.Json NuGet</ph>](https://www.nuget.org/packages/Newtonsoft.Json/){: external}. 

      ```bash
      dotnet add package Newtonsoft.Json -v 12.0.1
      ```
      {: pre}

  4. Speichern Sie den folgenden Code in einer Datei namens `Hello.cs`.

      ```csharp
      using System;
    using Newtonsoft.Json.Linq;

      namespace Apache.OpenWhisk.Example.Dotnet
    {
          public class Hello
        {
              public JObject Main(JObject args)
            {
                  string name = "stranger";
                if (args.ContainsKey("name")) {
                      name = args["name"].ToString();
                }
                JObject message = new JObject();
                message.Add("greeting", new JValue($"Hello, {name}!"));
                return (message);
            }
          }
      }
      ```
      {: codeblock}

  5. Kompilieren Sie `Hello.cs` und alle anderen Dateien und die Ausgabe im Verzeichnis `out`.

      ```bash
      dotnet publish -c Release -o out
      ```
      {: pre}

  6. Navigieren Sie zum Verzeichnis `out`. 

      ```bash
      cd out
      ```
      {: pre}

  7. Komprimieren Sie die veröffentlichten Dateien.

      ```bash
      zip -r -0 ../helloDotNet.zip *
      ```
      {: pre}



