---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: docker, actions, serverless

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

# Go-Aktionen erstellen
{: #creating-go-actions}

In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen Go-Aktion sowie zum Hinzufügen von Parametern zu dieser beschrieben. Sie können Go-Aktionen mit Go 1.11 ausführen. Zur Verwendung
dieser Laufzeit geben Sie den Parameter `--kind go:1.11` in der Befehlszeilenschnittstelle `ibmcloud fn` an, wenn Sie eine Aktion erstellen oder aktualisieren.
{: shortdesc}

## Go-Aktion erstellen und aufrufen
{: #invoking-go-actions}

Eine Go-Aktion ist einfach eine öffentliche Funktion aus dem `main`-Paket. Verwenden Sie eine einzelne Datei für schnelle Test- oder Entwicklungszwecke. Für Produktionsanwendungen [müssen Sie Ihre
Go-Aktionen in einer ausführbaren Datei vorkompilieren](#packaging-go-actions), um eine bessere Leistung oder Unterstützung für mehrere Quellendateien, einschließlich Anbieterbibliotheken, zu realisieren.
{: shortdesc}

Erstellen Sie eine Go-Aktion. 

1. Speichern Sie den folgenden Code in einer Datei namens `hello.go`. In dem Beispiel heißt die Funktion `Main`, aber Sie können den Namen mithilfe des Flags `--main` ändern. Wenn Sie die Funktion umbenennen, muss der Name mit einem Großbuchstaben beginnen und darf nicht `main` lauten, weil es sich dabei um den Namen des Go-Pakets handelt. 

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

    </br>
    Die erwartete Signatur für eine `Main`-Funktion ist: 

    ```go
    func Main(event map[string]interface{}) map[string]interface{}
    ```
    {: codeblock}

2. Erstellen oder aktualisieren Sie eine Aktion namens `helloGo`. 

    ```bash
    ibmcloud fn action update helloGo hello.go --kind go:1.11
    ```
    {: pre}

3. Rufen Sie die Aktion auf. 

    ```bash
    ibmcloud fn action invoke --result helloGo --param name gopher
    ```
    {: pre}

    Beispielausgabe:
    ```json
      {
          "greeting": "Hello gopher!"
      }
    ```
    {: screen}

## Aktion als ausführbare Go-Datei packen
{: #packaging-go-actions}

Obwohl Sie Binärdateien auf allen Go-Plattformen erstellen können, indem Sie eine Kreuzkompilierung mit `GOOS=Linux` und `GOARCH=amd64` durchführen, verwenden Sie das Vorkompilierungsfeature,
das im Container-Image der Laufzeit eingebettet ist. Sie können [mehrere Quellendateien](#multiple-packages-go-actions) oder [Anbieterbibliotheken](#vendor-libs-go-actions) packen.
{: shortdesc}

### Mit mehreren Paketquellendateien arbeiten
{: #multiple-packages-go-actions}

Wenn Sie mehrere Paketquellendateien verwenden möchten, verwenden Sie ein `src`-Verzeichnis der höchsten Ebene, legen Sie die Quellendateien, die zum Hauptpaket gehören, im Stammverzeichnis von
`src` oder in einem `main`-Verzeichnis ab und erstellen Sie Verzeichnisse für andere Pakete. Das Paket `hello` wird beispielsweise zum Verzeichnis `src/hello`.
{: shortdesc}

```
go-action-hello/
└── src
    ├── hello
    │   └── hello.go
    └── main
        └── main.go
```
{: screen}

Mit diesem Layout können Sie Unterpakete importieren (`import "hello"`), wie im folgenden Beispiel gezeigt. 

Sie können eine lokale Kompilierung durchführen, indem Sie `GOPATH` auf das übergeordnete Element des `src`-Verzeichnisses setzen. Wenn Sie VSCode verwenden, müssen Sie die Einstellung `go.inferGopath` auf `true` setzen.
{: note}

Beispiel für `main/main.go`: 

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

Beispiel für `hello/hello.go`: 

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

Sie können unter Verwendung der Laufzeitumgebung kompilieren. Erstellen Sie ein ZIP-Archiv des Inhalts des `src`-Verzeichnisses. Schließen Sie **nicht** das Projektverzeichnis der höchsten Ebene `go-action-project/` ein. So erstellen Sie das ZIP-Archiv `hello-src.zip`: 

```bash
cd src
zip -r ../hello-src.zip *
cd ..
```
{: pre}

Kompilieren und packen Sie die ausführbare Go-Datei als `exec` in das Stammverzeichnis des ZIP-Archivs. Erstellen Sie das Archiv `hello-bin.zip`, indem Sie den folgenden Befehl ausführen. Dabei wird vorausgesetzt, dass Sie die Docker-Befehlszeilenschnittstelle in Ihrer Workstation installiert und `docker` in `PATH` angegeben haben. 

```bash
docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
```
{: pre}

In diesem Beispiel lautet die 'main'-Funktion `-compile main`. Um eine andere 'main'-Funktion zu verwenden, ändern Sie den Wert `-compile`.
Die 'main'-Funktion wird zur Kompilierzeit ausgewählt. Bei einer Vorkompilierung ignoriert `ibmcloud fn action [update | create]` das Element `--main`. 

Der Container erhält die Inhalte der Quellen-ZIP-Datei in `stdin`, kompiliert sie und erstellt ein neues ZIP-Archiv mit der ausführbaren Datei (`exec`) im Stammverzeichnis. Der Inhalt des
ZIP-Archivs wird an `stdout` gestreamt, was wiederum in das Archiv `hello-bin.zip` weitergeleitet wird, um als Go-Aktion bereitgestellt zu werden. 

Jetzt können Sie Ihre Aktion mithilfe der Befehlszeilenschnittstelle und dem neuen ZIP-Archiv `hello-bin.zip` für die Produktion aktualisieren. 

```bash
ibmcloud fn action update helloGo hello-bin.zip --kind go:1.11
```
{: codeblock}

### Mit Anbieterbibliotheken arbeiten
{: #vendor-libs-go-actions}

Sie können Abhängigkeiten einschließen, indem Sie ein Verzeichnis `vendor` im Quellen-ZIP-Archiv (`zip`) befüllen, wenn Sie die Go-Aktion kompilieren. Das Verzeichnis
`vendor` funktioniert nicht auf höchster Ebene. Sie müssen das Verzeichnis `vendor` in `src/` und in einem Paketverzeichnis platzieren.
{: shortdesc}

Basierend auf dem vorherigen Beispiel verwenden Sie das Protokollpaket `logrus` in `hello.go`. 

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
In diesem Beispiel befindet sich das Verzeichnis `vendor` in `src/hello/vendor`. Sie können Bibliotheken anderer Anbieter hinzufügen, die vom Paket `hello` verwendet werden. Sie können mehrere Tools wie [dep ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://golang.github.io/dep/docs/installation.html) verwenden, um Abhängigkeiten zu füllen und zu verwalten. 

Mit `dep` können Sie eine Datei `src/main/Gopkg.toml` erstellen, die die Version und Position der Bibliotheken beschreibt. 

```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

Füllen Sie das Verzeichnis `vendor`, führen Sie `dep ensure` aus. 

Sie können diesen Prozess mithilfe eines Bereitstellungstools oder -scripts wie [Makefile](#makefile) automatisieren.
{: tip}

### Makefile für Go-Aktionen verwenden
{: #makefile}

Basierend auf dem vorherigen Beispiel lautet Ihr Projektverzeichnis wie folgt. 

```bash
go-action-hello/
├── Makefile
└── src
    ├── hello
    │   ├── Gopkg.toml
    │   ├── hello.go
    │   └── vendor/
    └── main
        └── main.go
```
{: screen}

1. Erstellen Sie die Datei `Makefile`, um den Bereitstellungsprozess zu automatisieren. 

```Makefile
GO_COMPILER?=openwhisk/actionloop-golang-v1.11
CLI?=ibmcloud fn
MAIN=main
APP=hello
SRCS=$(MAIN)/$(MAIN).go $(APP)/$(APP).go
VENDORS=$(APP)/vendor
NAME=go-action-$(APP)
BINZIP=$(APP)-bin.zip
SRCZIP=$(APP)-src.zip

deploy: $(BINZIP)
	$(CLI) action update $(NAME) $(BINZIP) --main $(MAIN) --kind go:1.11

$(BINZIP): $(SRCZIP)
	docker run -i $(GO_COMPILER) -compile $(MAIN) <$(SRCZIP) >$(BINZIP)

$(SRCZIP): src/$(VENDORS)
	cd src ; zip ../$(SRCZIP) -r $(SRCS) $(VENDORS)

src/%/vendor:
	cd $(@D) ; DEPPROJECTROOT=$(realpath $(@D)/../..) dep ensure

clean:
	-rm -rf $(BINZIP) $(SRCZIP) $(VENDORS)

invoke:
	$(CLI) action invoke $(NAME) -r -p name Gopher
```
{: codeblock}

2. Löschen Sie die ZIP-Archive und das Anbieterverzeichnis. 

```bash
make clean
```
{: pre}

3. Füllen Sie das Anbieterverzeichnis, erstellen Sie die Quellen-ZIP-Datei, kompilieren Sie den Quellcode, archivieren Sie die ausführbare Datei in eine ZIP-Datei und stellen Sie die Go-Aktion bereit, indem Sie sie ausführen. 

```bash
make deploy
```
{: pre}

4. Nachdem `go-action-hello` erstellt wurde, rufen Sie die Aktion auf. 

```bash
ibmcloud fn action invoke go-action-hello -r -p name Go
```
{: pre}

Beispielausgabe:

```json
    {
        "greeting": "Hello Go"
    }
```
{: screen}
