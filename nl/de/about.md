---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: platform architecture, openwhisk, couchdb, kafka, functions

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


# Funktionsweise von {{site.data.keyword.openwhisk_short}}
{: #about}

{{site.data.keyword.openwhisk}} ist eine ereignisgesteuerte Berechnungsplattform (auch als serverunabhängiges Computing oder Function as a Service (FaaS) bezeichnet), die Code als Reaktion auf Ereignisse oder direkte Aufrufe ausführt.
{: shortdesc}

## {{site.data.keyword.openwhisk_short}}-Technologie
{: #about_technology}

Im Folgenden werden einige grundlegende Konzepte der Technologie beschrieben, die {{site.data.keyword.openwhisk_short}} zugrunde liegt:

**Was ist eine Aktion?**

Eine Aktion ist ein kleiner Teil des Codes, der aufgerufen werden kann oder in Antwort auf ein Ereignis ausgeführt wird. So oder so resultiert jede Ausführung in einem Datensatz, der durch eine eindeutige Aktivierungs-ID angegeben wird. Die Eingabe und das Ergebnis einer Aktion können als Schlüssel/Wert-Paar angesehen werden. Der Schlüssel ist eine Zeichenfolge und der Wert ein gültiger JSON-Wert. Eine Aktion kann in der Sprache Ihrer Wahl geschrieben sein und wird dem Service entweder als Quellcode oder als Docker-Image bereitgestellt. Der Aktionscode wird ausgeführt, wenn er direkt von der Cloud Functions-API, der CLI oder dem iOS-SDK aufgerufen wird. Eine Aktion kann automatisch auf Ereignisse von IBM Cloud oder Services anderer Anbieter antworten.

**Wann verwende ich eine Aktion?**

Durch die Verwendung von Aktionen begrenzen Sie die Zeitspanne, in der Ihr Code ausgeführt wird, wodurch sich die Kosten verringern. 

Aktionen können zum Beispiel verwendet werden, um Gesichter in einem Bild zu erkennen, auf Änderungen in einer Datenbank zu reagieren, eine Gruppe von API-Aufrufen zu aggregieren oder auch um einen Tweet zu posten.

**Kann ich mehrere Aktionen gleichzeitig ausführen?**

Ja. Sie können Aktionen verwenden, um andere Aktionen aufzurufen, oder Sie können Aktionen zum [Erstellen von Sequenzen](/docs/openwhisk?topic=cloud-functions-actions#actions_seq) zusammenzuführen. Damit dies funktioniert, muss die Ausgabe einer Aktion die Eingabe der nächsten Aktion sein, die wiederum eine Ausgabe erzeugt, mit der eine weitere Aktion ausgelöst werden kann usw. Sie können die Gruppe von erstellten Aktionen sogar bündeln, um ein Paket zu formen. Mit einem Paket können Sie gängige Aktionen oder Sequenzen wiederverwenden, indem Sie das Paket aufrufen, statt die Aktion oder die Sequenz erneut zu konfigurieren. 

## {{site.data.keyword.openwhisk_short}}-Terminologie

<dl>
  <dt>Namensbereich</dt>
    <dd>[Namensbereiche](/docs/openwhisk?topic=cloud-functions-namespaces) enthalten {{site.data.keyword.openwhisk_short}}-Entitäten (wie z. B. Aktionen und Auslöser) und gehören zu einer Ressourcengruppe. Sie können anderen Benutzern Zugriff auf Ihre {{site.data.keyword.openwhisk_short}}-Entitäten gewähren, indem Sie ihnen Zugriff auf den Namensbereich erteilen.</dd>
  <dt>Aktion</dt>
    <dd>Eine [Aktion](/docs/openwhisk?topic=cloud-functions-actions) ist ein Abschnitt Code, der eine bestimmte Task ausführt. Aktionen können in einer Sprache Ihrer Wahl geschrieben sein und es kann sich um kleine Snippets aus JavaScript- oder Swift-Code oder angepasstem Binärcode handeln, die in einem Docker-Container eingebettet sind. Sie stellen Ihre Aktion für Cloud Functions entweder als Quellcode oder als Docker-Image bereit.
    <br><br>Eine Aktion führt Verarbeitungsprozesse aus, wenn sie direkt über die API, die CLI oder das iOS-SDK von {{site.data.keyword.openwhisk_short}} aufgerufen wird. Eine Aktion kann auch automatisch auf Ereignisse aus {{site.data.keyword.cloud_notm}}-Services und Services von Drittanbietern unter Verwendung eines Auslösers reagieren. </dd>
  <dt>Sequenz</dt>
    <dd>Eine Gruppe von Aktionen kann außerdem zu einer [Sequenz](/docs/openwhisk?topic=cloud-functions-actions#actions_seq) verkettet werden, ohne dass dazu Code geschrieben werden muss. Eine Sequenz ist eine Kette von Aktionen, die nacheinander aufgerufen werden, wobei die Ausgabe einer Aktion als Eingabe an die nächste Aktion übergeben wird. Auf diese Weise können Sie vorhandene Aktionen kombinieren und so schnell und einfach wiederverwenden. Eine Sequenz kann dann genau wie eine Aktion über eine REST-API oder automatisch als Reaktion auf Ereignisse aufgerufen werden.
  </dd>
  <dt>Ereignis</dt>
    <dd>Beispiele für Ereignisse sind Änderungen an Datenbanksätzen, IoT-Sensormesswerte (IoT, Internet of Things - Internet der Dinge), die einen bestimmten Temperaturwert überschreiten, neue Codefestschreibungen (Commits) in einem GitHub-Repository oder einfache HTTP-Anforderungen von Web-Apps oder mobilen Apps. Ereignisse aus externen und internen Ereignisquellen werden durch einen Auslöser kanalisiert. Regeln ermöglichen es Aktionen, auf diese Ereignisse zu reagieren.</dd>
  <dt>Auslöser</dt>
    <dd>Ein [Auslöser](/docs/openwhisk?topic=cloud-functions-triggers) ist ein benannter Kanal für eine Klasse von Ereignissen. Bei einem Auslöser handelt es sich um eine Deklaration, die auf einen bestimmten Typ von Ereignis reagieren soll, entweder durch einen Benutzer oder eine Ereignisquelle.</dd>
  <dt>Regel</dt>
    <dd>Eine [Regel](/docs/openwhisk?topic=cloud-functions-rules) ordnet einen Auslöser einer Aktion zu. Jedes Mal, wenn der Auslöser zur Anwendung kommt, verwendet die Regel das Auslöserereignis als Eingabe und ruft die zugehörige Aktion auf. Mit dem entsprechenden Satz von Regeln kann ein einzelnes Auslöserereignis mehrere Aktionen aufrufen oder eine Aktion als Reaktion auf Ereignisse aus mehreren Auslösern aufgerufen werden.</dd>
  <dt>Feed</dt>
    <dd>Ein [Feed](/docs/openwhisk?topic=cloud-functions-triggers#triggers_feeds) ist eine bequeme Methode zum Konfigurieren einer externen Ereignisquelle, damit diese Auslöserereignisse aktiviert, die von {{site.data.keyword.openwhisk_short}} verarbeitet werden können. Ein Git-Feed könnte zum Beispiel ein Auslöserereignis für jede Festschreibung (Commit) in einem Git-Repository aktivieren.</dd>
  <dt>Paket</dt>
    <dd>Integrationen in Services und Ereignisprovider können durch Pakete hinzugefügt werden. Ein [Paket](/docs/openwhisk?topic=cloud-functions-pkg_ov) ist ein Bündel aus Feeds und Aktionen. Ein Feed ist ein Codeabschnitt, der eine externe Ereignisquelle zum Aktivieren (Auslösen) von Auslöserereignissen konfiguriert. Zum Beispiel konfiguriert ein Auslöser, der mit einem Feed für {{site.data.keyword.cloudant}}-Änderungen erstellt wurde, einen Service, der den Auslöser jedes Mal dann aktiviert, wenn ein Dokument in einer {{site.data.keyword.cloudant_short_notm}}-Datenbank geändert oder hinzugefügt wird. Aktionen in Paketen stellen wiederverwendbare Logik da, die ein Service-Provider verfügbar machen kann, sodass Entwickler den Service als Ereignisquelle verwenden und APIs dieses Service aufrufen können.
    <br><br>Ein bestehender Katalog mit Paketen bietet eine schnelle Möglichkeit, Anwendungen um nützliche Funktionen zu erweitern und auf externe Services im direkten Geschäftsumfeld zuzugreifen. Zu den externen Services, die über {{site.data.keyword.openwhisk_short}}-Paket verfügen, gehören zum Beispiel {{site.data.keyword.cloudant_short_notm}}, The Weather Company, Slack und GitHub.</dd>
</dl>

### Weitere Schritte
{: #quiz}
Testen Sie Ihr Wissen und [nehmen Sie an einem Quiz teil](https://ibmcloud-quizzes.mybluemix.net/functions/terms_quiz/quiz.php){: external}! 


## Funktionsweise der internen Verarbeitung von OpenWhisk
{: #about_internal}

Zur eingehenderen Erläuterung aller Komponenten soll ein Aufruf einer Aktion durch das {{site.data.keyword.openwhisk_short}}-System verfolgt werden. Ein Aufruf führt den Code aus, den der Benutzer dem System zugeführt hat, und gibt die Ergebnisse dieser Ausführung zurück. In der folgenden Abbildung ist die allgemeine Architektur von {{site.data.keyword.openwhisk_short}} dargestellt.

![{{site.data.keyword.openwhisk_short}}-Architektur](./images/OpenWhisk.png)

### Was geschieht hinter den Kulissen in OpenWhisk?
{: #about_scenes}

OpenWhisk ist ein Open-Source-Projekt, das Komponenten wie NGINX, Kafka, Docker und CouchDB kombiniert und so einen serverunabhängigen, ereignisbasierten Programmierservice darstellt. 

<img src="images/OpenWhisk_flow_of_processing.png" width="550" alt="Der interne Verarbeitungsablauf hinter den Kulissen in OpenWhisk" style="width:550px; border-style: none"/>

#### 1. Eingang in das System: NGINX
{: #about_ngnix}

Zunächst ist die Benutzerinteraktions-API von OpenWhisk HTTP-basiert und entspricht einem REST-konformen Design. Daher ist der Befehl, der über die Befehlszeilenschnittstelle (CLI) gesendet wird, eine HTTP-Anforderung an das OpenWhisk-System. Der spezielle Befehl lässt sich grob wie folgt übersetzen:
```
POST /api/v1/namespaces/$userNamespace/actions/myAction
Host: $openwhiskEndpoint
```
{: screen}

Beachten Sie hier die Variable *$userNamespace*. Ein Benutzer hat Zugriff auf mindestens einen Namensbereich. Aus Gründen der Einfachheit sei hier angenommen, dass der Benutzer Eigner des Namensbereichs ist, in den *myAction* eingefügt wird.

Der erste Eingangspunkt in das System ist **NGINX**, ein “HTTP- und Reverse-Proxy-Server”. Dieser Server dient zur SSL-Terminierung und zur Weiterleitung entsprechender HTTP-Aufrufe an die nächste Komponente.

#### 2. Eingang in das System: Controller
{: #about_controller}


NGINX leitet die HTTP-Anforderung an den **Controller** weiter, die nächste Komponente auf dem Pfad durch OpenWhisk. Dabei handelt es sich um eine Scala-basierte Implementierung der tatsächlichen REST-API (auf der Basis von **Akka** und **Spray**) , die dementsprechend als Schnittstelle für alle Aktionen dient, die ein Benutzer ausführen kann. Dazu gehören auch Anforderungen zum [Erstellen, Abrufen, Aktualisieren und Löschen](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete){: external} für Ihre Entitäten in OpenWhisk und der Aufruf von Aktionen. 

Der Controller ermittelt zuerst die eindeutige Aktion, die der Benutzer auszuführen versucht. Er tut dies anhand der HTTP-Methode, die Sie in Ihrer HTTP-Anforderung verwenden. Der Benutzer gibt eine POST-Anforderung an eine vorhandene Aktion aus, die vom Controller in einen **Aufruf einer Aktion** übersetzt wird. 

Aufgrund der zentralen Rolle des Controllers (daher der Name) ist er an allen folgenden Schritten in gewissem Umfang beteiligt. 

#### 3. Authentifizierung und Berechtigung: CouchDB
{: #about_auth}

Nun überprüft der Controller Ihre Identität (*Authentifizierung*) und stellt fest, ob Sie die Aktion ausführen dürfen, die Sie mit der betreffenden Entität ausführen wollen (*Berechtigung*). Die Berechtigungsnachweise, die in die Anforderung eingeschlossen wurden, werden anhand der so genannten Datenbank für **Subjekte** in einer **CouchDB**-Instanz geprüft.

In diesem Fall wird geprüft, ob der Benutzer in der Datenbank von OpenWhisk vorhanden ist und ob er die Berechtigung für den Aufruf der Aktion *myAction* hat, von der angenommen wird, dass es sich um eine Aktion in einem Namensbereich handelt, dessen Eigner der Benutzer ist. Durch die letztere Prüfung wird dem Benutzer effektiv die Berechtigung zum Aufrufen der Aktion erteilt.

Da alles in Ordnung ist, öffnet sich das Tor für die nächste Verarbeitungsphase.

#### 4. Aktion abrufen: wiederum CouchDB
{: #about_couchdb}

Da der Controller jetzt sicher ist, dass der Benutzer berechtigt ist, diese Aktion aufzurufen, lädt er diese Aktion (in diesem Fall *myAction*) aus der Datenbank **whisks** in CouchDB.

Der Datensatz der Aktion enthält hauptsächlich den auszuführenden Code sowie die Standardparameter, die Sie an die Aktion übergeben möchten, zusammen mit den Parametern, die Sie in die tatsächliche Aufrufanforderung eingeschlossen haben. Er enthält außerdem die Ressourcenbeschränkungen, die für die Ausführung gelten, wie zum Beispiel für den Hauptspeicher, der von der Aktion genutzt werden kann.

In diesem speziellen Fall hat die Aktion keine Parameter (die Parameterdefinition der Funktion ist eine leere Liste), Daher wird davon ausgegangen, dass keine Standardparameter festgelegt sind und dass keine bestimmten Parameter an die Aktion gesendet wurden, sodass dies der trivialste Fall in dieser Hinsicht ist.


#### 5. Aufruf der Aktion durch die Lastausgleichsfunktion
{: #about_lb}

Die Lastausgleichsfunktion (Load Balancer), die Bestandteil des Controllers ist, hat durch kontinuierliche Statusüberprüfungen einen globalen Überblick über die im System verfügbaren ausführenden Komponenten (Executor). Diese ausführenden Komponenten werden als **Aufrufer** (Invoker) bezeichnet. Die Lastausgleichsfunktion, die jetzt ermittelt hat, welche Aufrufer verfügbar sind, wählt einen von diesen aus, um die angeforderte Aktion aufzurufen.

#### 6. Eine Reihe bilden: Kafka
{: #about_kafka}

Ab hier können im Wesentlichen zwei nachteilige Ereignisse für die von Ihnen eingesendete Aufrufanforderung eintreten:

1. Das System kann abstürzen, sodass Ihr Aufruf verloren geht.
2. Das System kann so hoch ausgelastet sein, dass der Aufruf zunächst auf die Beendigung anderer Aufrufe warten muss.

Die Antwort auf beide Fälle heißt **Kafka**, ein durchsatzstarkes, verteiltes Publish/Subscribe-Nachrichtenübermittlungssystem. Der Controller und der Aufrufer kommunizieren nur über Nachrichten, die von Kafka gepuffert und gespeichert werden. Kafka nimmt die Last der Pufferung im Speicher, bei der das Risiko einer Ausnahmebedingung aufgrund von nicht ausreichendem Speicher (*OutOfMemoryException*) besteht, sowohl vom Controller als auch vom Aufrufer weg und stellt gleichzeitig sicher, dass Nachrichten im Fall eines Systemabsturzes nicht verloren gehen.

Um die Aktion nun aufzurufen, veröffentlicht der Controller eine Nachricht an Kafka, die die aufzurufende Aktion und die Parameter, die an diese Aktion zu übergeben sind (in diesem Fall keine), enthält. Diese Nachricht wird an den Aufrufer adressiert, den der Controller in der Liste ausgewählt hat, die er von Consul empfangen hatte.

Wenn Kafka den Eingang der Nachricht bestätigt, wird die HTTP-Anforderung an den Benutzer mit einer Aktivierungs-ID (**Activation ID**) beantwortet. Der Benutzer kann diese später verwenden, um auf die Ergebnisse dieses speziellen Aufrufs zuzugreifen. Dies ist ein asynchrones Aufrufmodell, bei dem die HTTP-Anforderung beendet wird, sobald das System die Anforderung zum Aufruf einer Aktion akzeptiert hat. Es ist außerdem ein synchrones Modell (das als blockierender Aufruf bezeichnet wird) verfügbar, das in diesem Abschnitt jedoch nicht behandelt wird.

#### 7. Code aufrufen: Aufrufer (Invoker)
{: #about_invoker}

Der Aufrufer (**Invoker**) ist das Herzstück von OpenWhisk. Der Aufrufer hat die Aufgabe, eine Aktion aufzurufen. Er ist ebenfalls in Scala implementiert. Jedoch ist dazu noch wesentlich mehr zu sagen. Zum Ausführen von Aktionen auf eine isolierte und sichere Weise wird **Docker** verwendet.

Docker wird verwendet, um eine neue, in sich eingekapselte Umgebung (einen so genannten *Container*) für jede Aktion einzurichten, die in schneller, isolierter und gesteuerter Weise aufgerufen wird. Für jeden Aktionsaufruf wird ein Docker-Container generiert und der Aktionscode dort eingefügt. Der Code wird anschließend mit den übergebenen Parametern ausgeführt, das Ergebnis abgerufen und der Container wieder gelöscht. In diesem Stadium können Leistungsoptimierungen vorgenommen werden, um die Wartungsanforderungen zu verringern und schnelle Antwortzeiten zu erzielen. 

In diesem Fall ist eine auf *Node.js* basierende Aktion vorhanden und der Aufrufer startet einen Node.js-Container. Anschließend fügt er den Code aus *myAction* ein, führt ihn ohne Parameter aus, extrahiert das Ergebnis, speichert die Protokolle und löscht den Node.js-Container wieder. 

#### 8. Ergebnisse speichern: erneut CouchDB
{: #about_storing}

Wenn das Ergebnis vom Aufrufer abgerufen wurde, wird es in der Datenbank **whisks** als Aktivierung unter der Aktivierungs-ID gespeichert. Die Datenbank **whisks** wird in **CouchDB** betrieben.

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
ibmcloud fn activation get 31809ddca6f64cfc9de2937ebd44fbb9
```
{: pre}

### Zusammenfassung
{: #about_summary}

Sie haben gesehen, wie ein einfacher Befehl **ibmcloud fn action invoked myAction** verschiedene Stufen des {{site.data.keyword.openwhisk_short}}-Systems durchläuft. Das System besteht selbst hauptsächlich aus zwei angepassten Komponenten: dem **Controller** und dem **Aufrufer** (Invoker). Alles andere ist bereits vorhanden - entwickelt von vielen Mitarbeitern in der Open-Source-Community.

Weitere Informationen zu {{site.data.keyword.openwhisk_short}} finden Sie in den folgenden Abschnitten:

* [Entitätsnamen](/docs/openwhisk?topic=cloud-functions-limits#limits_entities)
* [Aktionssemantik](/docs/openwhisk?topic=cloud-functions-limits#limits_semantics)
* [Begrenzungen](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)
* [REST-API-Referenz](/apidocs/functions)




