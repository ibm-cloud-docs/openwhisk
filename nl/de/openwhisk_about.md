---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Informationen zu {{site.data.keyword.openwhisk_short}}

{{site.data.keyword.openwhisk}} ist eine ereignisgesteuerte Berechnungsplattform (auch als serverunabhängiges Computing oder Function as a Service (FaaS) bezeichnet), die Code als Reaktion auf Ereignisse oder direkte Aufrufe ausführt. In der folgenden Abbildung ist die allgemeine Architektur von {{site.data.keyword.openwhisk}} dargestellt.
{: shortdesc}

![{{site.data.keyword.openwhisk_short}}-Architektur](./images/OpenWhisk.png)

Beispiele für Ereignisse sind Änderungen an Datenbanksätzen, IoT-Sensormesswerte (IoT, Internet of Things - Internet der Dinge), die einen bestimmten Temperaturwert überschreiten, neue Codefestschreibungen (Commits) in einem GitHub-Repository oder einfache HTTP-Anforderungen von Web-Apps oder mobilen Apps. Ereignisse aus externen und internen Ereignisquellen werden durch einen Auslöser kanalisiert. Regeln ermöglichen es Aktionen, auf diese Ereignisse zu reagieren.

Aktionen können kleine Snippets aus JavaScript- oder Swift-Code oder angepasster Binärcode sein, die in einem Docker-Container eingebettet sind. Aktionen in {{site.data.keyword.openwhisk_short}} werden sofort bereitgestellt und ausgeführt, wenn ein Auslöser aktiviert wird. Je mehr Auslöser aktiviert werden, desto mehr Aktionen werden aufgerufen. Wird kein Auslöser aktiviert, wird kein Aktionscode ausgeführt und Kosten bleiben bei null.

Neben der Verknüpfung von Aktionen mit Auslösern ist es möglich, eine Aktion direkt über die API, die CLI oder das iOS-SDK von {{site.data.keyword.openwhisk_short}} aufzurufen. Eine Gruppe von Aktionen kann außerdem verkettet werden, ohne dass dazu Code geschrieben werden muss. Jede Aktion in der Kette wird in der Reihenfolge aufgerufen, wobei die Ausgabe einer Aktion als Eingabe an die nächste Aktion in der Folge übergeben wird.

Bei traditionellen virtuellen Maschinen mit langer Laufzeit oder Containern ist es ein allgemein übliches Verfahren, mehrere virtuelle Maschinen (VMs) bzw. Container bereitzustellen, um flexibel auf Ausfälle einer einzelnen Instanz reagieren zu können. {{site.data.keyword.openwhisk_short}} bietet jedoch ein alternatives Modell ohne Kostenaufwand für den Ausfallschutz an. Die bedarfsgesteuerte Ausführung von Aktionen ermöglicht eine inhärente Skalierbarkeit und eine optimale Auslastung, da die Anzahl der aktiven Aktionen immer der Auslöserrate entspricht. Darüber hinaus kann sich der Entwickler jetzt allein auf den Code konzentrieren und braucht sich nicht um die Überwachung, die Programmkorrekturen oder den Schutz für den zugrunde liegenden Server, den Speicher, das Netz und die Betriebssysteminfrastruktur zu kümmern.

Integrationen in Services und Ereignisprovider können durch Pakete hinzugefügt werden. Ein Paket ist ein Bündel aus Feeds und Aktionen. Ein Feed ist ein Codeabschnitt, der eine externe Ereignisquelle zum Aktivieren (Auslösen) von Auslöserereignissen konfiguriert. Zum Beispiel konfiguriert ein Auslöser, der mit einem Feed für Cloudant-Änderungen erstellt wurde, einen Service, der den Auslöser jedes Mal dann aktiviert, wenn ein Dokument in einer Cloudant-Datenbank geändert oder hinzugefügt wird. Aktionen in Paketen stellen wiederverwendbare Logik da, die ein Service-Provider verfügbar machen kann, sodass Entwickler den Service als Ereignisquelle verwenden und APIs dieses Service aufrufen können.

Ein bestehender Katalog mit Paketen bietet eine schnelle Möglichkeit, Anwendungen um nützliche Funktionen zu erweitern und auf externe Services im direkten Geschäftsumfeld zuzugreifen. Zu den externen Services, die für {{site.data.keyword.openwhisk_short}} eingerichtet sind, gehören zum Beispiel Cloudant, The Weather Company, Slack und GitHub.


## Funktionsweise von {{site.data.keyword.openwhisk_short}}
{: #openwhisk_how}

Als Open-Source-Projekt stützt sich OpenWhisk auf die Grundlage großer Komponenten wie Nginx, Kafka, Consul, Docker und CouchDB. Alle diese Komponenten bilden zusammen einen 'serverunabhängigen, ereignisgesteuerten Programmierservice'. Zur eingehenderen Erläuterung aller Komponenten soll ein Aufruf einer Aktion durch das System in seinem Verlauf verfolgt werden. Ein Aufruf in OpenWhisk führt zur Kernaufgabe einer serverunabhängigen Engine: Die Ausführung des Codes, den der Benutzer dem System zugeführt hat und die Rückgabe der Ergebnisse dieser Ausführung.

### Aktion erstellen

Um der Erläuterung etwas Kontext zu verleihen, wird im System zunächst eine Aktion erstellt. Anhand dieser Aktion werden später die verschiedenen Konzepte während der Verarbeitung durch das System erläutert. Die folgenden Befehle gehen davon aus, dass die [OpenWhisk-CLI ordnungsgemäß eingerichtet ist](https://github.com/openwhisk/openwhisk/tree/master/docs#setting-up-the-openwhisk-cli).

Erstellen Sie zuerst eine Datei mit dem Namen *action.js*,die den folgenden Code enthält, der die Zeichenfolge “Hello World” an die Standardausgabe (STDOUT) ausgibt und ein JSON-Objekt zurückgibt, das die Zeichenfolge “world” unter dem Schlüssel “hello” enthält.
```javascript
function main() {
    console.log('Hello World');
    return { hello: 'world' };
}
```
{: codeblock}

Erstellen Sie die Aktion, indem Sie den folgenden Befehl ausführen:
```
wsk action create myAction action.js
```
{: pre}

Führen Sie nun den folgenden Befehl aus, um diese Aktion aufzurufen:
```
wsk action invoke myAction --result
```
{: pre}

## Interner Verarbeitungsablauf
Was geschieht hinter den Kulissen in OpenWhisk?

![OpenWhisk-Verarbeitungsablauf](images/OpenWhisk_flow_of_processing.png)

### Eingang in das System: nginx

Zunächst ist die Benutzerinteraktions-API von OpenWhisk vollständig HTTP-basiert und entspricht einem REST-konformen Design. Daher ist der Befehl, der über die wsk-Befehlszeilenschnittstelle (CLI) gesendet wird, eigentlich eine HTTP-Anforderung an das OpenWhisk-System. Der spezielle Befehl lässt sich grob wie folgt übersetzen:
```
POST /api/v1/namespaces/$userNamespace/actions/myAction
Host: $openwhiskEndpoint
```
{: screen}

Beachten Sie hier die Variable *$userNamespace*. Ein Benutzer hat Zugriff auf mindestens einen Namensbereich. Aus Gründen der Einfachheit sei hier angenommen, dass der Benutzer Eigner des Namensbereichs ist, in den *myAction* eingefügt wird.

Der erste Eingangspunkt in das System ist **nginx**, bei dem es sich um einen “HTTP- und Reverse-Proxy-Server” handelt. Dieser Server dient zur SSL-Terminierung und zur Weiterleitung entsprechender HTTP-Aufrufe an die nächste Komponente.

### Eingang in das System: Controller

Nginx leitet die HTTP-Anforderung an den **Controller** weiter, die nächste Komponente auf dem Pfad durch OpenWhisk. Dabei handelt es sich um eine Scala-basierte Implementierung der tatsächlichen REST-API (auf der Basis von **Akka** und **Spray**) , die dementsprechend als Schnittstelle für alle Aktionen dient, die ein Benutzer ausführen kann. Dazu gehören auch [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)-Anforderungen für Ihre Entitäten (CRUD - Erstellen, Abrufen, Aktualisieren, Löschen) in OpenWhisk und der Aufruf von Aktionen.

Der Controller ermittelt zuerst die eindeutige Aktion, die der Benutzer auszuführen versucht. Er tut dies anhand der HTTP-Methode, die Sie in Ihrer HTTP-Anforderung verwenden. Wie in der obigen Übersetzung zu sehen, gibt der Benutzer eine POST-Anforderung an eine vorhandene Aktion aus, die vom Controller in einen **Aufruf einer Aktion** übersetzt wird.

Aufgrund der zentralen Rolle des Controllers (daher der Name) ist er an den folgenden Schritten in gewissem Umfang beteiligt.

### Authentifizierung und Berechtigung: CouchDB

Nun überprüft der Controller Ihre Identität (*Authentifizierung*) und stellt fest, ob Sie die Aktion ausführen dürfen, die Sie mit der betreffenden Entität ausführen wollen (*Berechtigung*). Die Berechtigungsnachweise, die in die Anforderung eingeschlossen wurden, werden anhand der so genannten Datenbank für **Subjekte** in einer **CouchDB**-Instanz geprüft.

In diesem Fall wird geprüft, ob der Benutzer in der Datenbank von OpenWhisk vorhanden ist und ob er die Berechtigung für den Aufruf der Aktion *myAction* hat, von der angenommen wird, dass es sich um eine Aktion in einem Namensbereich handelt, dessen Eigner der Benutzer ist. Durch die letztere Prüfung wird dem Benutzer effektiv die Berechtigung zum Aufrufen der Aktion erteilt.

Da alles in Ordnung ist, öffnet sich das Tor für die nächste Verarbeitungsphase.

### Aktion abrufen: wiederum CouchDB

Da der Controller jetzt sicher ist, dass der Benutzer berechtigt ist, diese Aktion aufzurufen, lädt er diese Aktion (in diesem Fall *myAction*) aus der Datenbank **whisks** in CouchDB.

Der Datensatz der Aktion enthält hauptsächlich den auszuführenden Code sowie die Standardparameter, die Sie an die Aktion übergeben möchten, zusammen mit den Parametern, die Sie in die tatsächliche Aufrufanforderung eingeschlossen haben. Er enthält außerdem die Ressourcenbeschränkungen, die für die Ausführung gelten, wie zum Beispiel für den Hauptspeicher, der von der Aktion genutzt werden kann.

In diesem speziellen Fall hat die Aktion keine Parameter (die Parameterdefinition der Funktion ist eine leere Liste), Daher wird davon ausgegangen, dass keine Standardparameter festgelegt sind und dass keine bestimmten Parameter an die Aktion gesendet wurden, sodass dies der trivialste Fall in dieser Hinsicht ist.

### Wer ruft die Aktion auf: Consul

Der Controller (oder genauer seine Lastverteilungskomponente) verfügt nun über alle Daten, um die Ausführung Ihres Codes zu starten. Allerdings muss ihm noch bekannt sein, wer für diese Aktion verfügbar ist. **Consul**, eine Serviceerkennung, dient dazu, die ausführenden Komponenten (Executors) durch kontinuierliches Prüfen des Allgemeinzustands dieser Komponenten zu überwachen. Diese ausführenden Komponenten werden als **Aufrufer** (Invoker) bezeichnet.

Der Controller, der jetzt ermittelt hat, welche Aufrufer verfügbar sind, wählt einen von diesen aus, um die angeforderte Aktion aufzurufen.

Nehmen Sie für diesen Fall an, dass das System drei verfügbare Aufrufer hat (Aufrufer 0 - 2) und dass der Controller Aufrufer 2 (*Invoker 2*) zum Aufrufen der betreffenden Aktion ausgewählt hat.

### Eine Zeile erstellen: Kafka

Ab hier können im Wesentlichen zwei nachteilige Ereignisse für die von Ihnen eingesendete Aufrufanforderung eintreten:

1. Das System kann abstürzen, sodass Ihr Aufruf verloren geht.
2. Das System kann so hoch ausgelastet sein, dass der Aufruf zunächst auf die Beendigung anderer Aufrufe warten muss.

Die Antwort auf beide Fälle heißt **Kafka**, ein durchsatzstarkes, verteiltes Publish/Subscribe-Nachrichtenübermittlungssystem. Der Controller und der Aufrufer kommunizieren nur über Nachrichten, die von Kafka gepuffert und gespeichert werden. Kafka nimmt die Last der Pufferung im Speicher, bei der das Risiko einer Ausnahmebedingung aufgrund von nicht ausreichendem Speicher (*OutOfMemoryException*) besteht, sowohl vom Controller als auch vom Aufrufer weg und stellt gleichzeitig sicher, dass Nachrichten im Fall eines Systemabsturzes nicht verloren gehen.

Um die Aktion nun aufzurufen, veröffentlicht der Controller eine Nachricht an Kafka, die die aufzurufende Aktion und die Parameter, die an diese Aktion zu übergeben sind (in diesem Fall keine), enthält. Diese Nachricht wird an den Aufrufer adressiert, den der Controller in der Liste ausgewählt hat, die er von Consul empfangen hatte.

Wenn Kafka den Eingang der Nachricht bestätigt, wird die HTTP-Anforderung an den Benutzer mit einer Aktivierungs-ID (**ActivationId**) beantwortet. Der Benutzer kann diese später verwenden, um auf die Ergebnisse dieses speziellen Aufrufs zuzugreifen. Dies ist ein asynchrones Aufrufmodell, bei dem die HTTP-Anforderung beendet wird, sobald das System die Anforderung zum Aufruf einer Aktion akzeptiert hat. Es ist außerdem ein synchrones Modell (das als blockierender Aufruf bezeichnet wird) verfügbar, das in diesem Abschnitt jedoch nicht behandelt wird.

### Code wird aufgerufen: Aufrufer (Invoker)

Der Aufrufer (**Invoker**) ist das Herzstück von OpenWhisk. Der Aufrufer hat die Aufgabe, eine Aktion aufzurufen. Er ist ebenfalls in Scala implementiert. Jedoch ist dazu noch wesentlich mehr zu sagen. Zum Ausführen von Aktionen auf eine isolierte und sichere Weise wird **Docker** verwendet.

Docker wird dazu verwendet, eine neue und in sich eingekapselte Umgebung (einen so genannten *Container*) für jede Aktion einzurichten, die in schneller, isolierter und gesteuerter Weise aufgerufen wird. Für jeden Aktionsaufruf wird ein Docker-Container generiert und der Aktionscode dort eingefügt. Der Code wird anschließend mit den übergebenen Parametern ausgeführt, das Ergebnis abgerufen und der Container wieder gelöscht. In diesem Stadium können Leistungsoptimierungen vorgenommen werden, um den Systemaufwand zu verringern und schnelle Antwortzeiten zu erzielen. 

In diesem Fall, in dem eine auf *Node.js* basierende Aktion vorhanden ist, startet der Aufrufer einen Node.js-Container. Anschließend fügt er den Code aus *myAction* ein, führt ihn ohne Parameter aus, extrahiert das Ergebnis, speichert die Protokolle und löscht den Node.js-Container wieder.

### Ergebnisse speichern: erneut CouchDB

Wenn das Ergebnis vom Aufrufer abgerufen wurde, wird es in der Datenbank **whisks** als Aktivierung unter der Aktivierung-ID (ActivationId) gespeichert. Die Datenbank **whisks** wird in **CouchDB** betrieben.

In diesem besonderen Fall ruft der Aufrufer das resultierende JSON-Objekt aus der Aktion ab, erfasst das Protokoll, das von Docker geschrieben wurde, fügt alle diese Elemente in den Aktivierungsdatensatz ein und speichert diesen in der Datenbank. Schauen Sie sich das folgende Beispiel an:

```json
{
   "activationId": "31809ddca6f64cfc9de2937ebd44fbb9",
   "response": {
       "statusCode": 0,
       "result": {
           "hello": "world"
       }
   },
   "end": 1474459415621,
   "logs": [
       "2016-09-21T12:03:35.619234386Z stdout: Hello World"
   ],
   "start": 1474459415595,
}
```
{: codeblock}

Beachten Sie, wie der Datensatz sowohl das zurückgegebene Ergebnis als auch die geschriebenen Protokolle enthält. Er enthält außerdem die Startzeit und die Endzeit des Aufrufs der Aktion. Aktivierungsdatensätze enthalten noch weitere Felder, die in diesem Beispiel jedoch aus Gründen der Einfachheit nicht gezeigt werden.

Sie können jetzt die REST-API erneut verwenden (beginnen Sie wieder bei Schritt 1), um Ihre Aktivierung und damit das Ergebnis Ihrer Aktion abzurufen. Führen Sie dazu den folgenden Befehl aus:

```bash
wsk activation get 31809ddca6f64cfc9de2937ebd44fbb9
```
{: pre} 

### Zusammenfassung

Sie haben gesehen, wie ein einfacher Befehl **wsk action invoke myAction** verschiedene Komponenten des {{site.data.keyword.openwhisk_short}}-Systems durchlauft. Das System besteht selbst hauptsächlich aus zwei angepassten Komponenten: dem **Controller** und dem **Aufrufer** (Invoker). Alles andere ist bereits vorhanden - entwickelt von vielen Mitarbeitern in der Open-Source-Community.

Weitere Informationen zu {{site.data.keyword.openwhisk_short}} finden Sie in den folgenden Abschnitten:

* [Entitätsnamen](./openwhisk_reference.html#openwhisk_entities)
* [Aktionssemantik](./openwhisk_reference.html#openwhisk_semantics)
* [Begrenzungen](./openwhisk_reference.html#openwhisk_syslimits)
* [REST-API](./openwhisk_reference.html#openwhisk_ref_restapi)
