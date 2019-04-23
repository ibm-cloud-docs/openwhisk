---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: feed, event, polling, hooks, trigger, 

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Angepasste Feeds für Ereignisprovider erstellen
{: #openwhisk_feeds}

{{site.data.keyword.openwhisk_short}} unterstützt eine offene API, mit der jeder Benutzer einen Ereignisproduzentenservice als Feed in einem Paket verfügbar machen kann. Im folgenden Abschnitt werden die Architektur- und Implementierungsoptionen beschrieben, mit denen Sie Ihren eigenen angepassten Feed bereitstellen können.
{: shortdesc}

Die folgenden Informationen sind für erfahrene {{site.data.keyword.openwhisk_short}}-Benutzer gedacht, die eigene Feeds veröffentlichen möchten. Die meisten {{site.data.keyword.openwhisk_short}}-Benutzer können den Abschnitt zur Architektur ohne Weiteres überspringen.

## Feedarchitektur

Es gibt mindestens drei Architekturmuster für die Erstellung eines Feeds: **Hooks**, **Polling** und **Verbindungen**.

### Hooks
Im Muster *Hooks* wird der Feed mit einer [Webhook-Funktion](https://en.wikipedia.org/wiki/Webhook) eingerichtet, die von einem anderen Service verfügbar gemacht wird.   Bei diesem Verfahren wird ein Webhook in einem externen Service konfiguriert, um direkt POST-Anforderungen an eine URL zu senden, durch die ein Auslöser aktiviert wird. Diese Methode ist mit Abstand die einfachste und bequemste Option, Feeds mit geringer Häufigkeit zu implementieren.



### Polling
Im Muster "Polling" wird eine {{site.data.keyword.openwhisk_short}}-Aktion so eingerichtet, dass sie einen Endpunkt regelmäßig abfragt, um neue Daten abzurufen. Dieses Muster ist relativ einfach zu erstellen, aber die Häufigkeiten der Ereignisse werden durch das Polling-Intervall begrenzt.

### Verbindungen
Im Muster "Verbindungen" verwaltet ein separater Service eine persistente Verbindung zu einer Feedquelle. Die verbindungsbasierte Implementierung kann mit einem Serviceendpunkt durch lange Polling-Intervalle interagieren. Es kann auch eine Push-Benachrichtigung eingerichtet werden.





## Unterschied zwischen Feed und Auslöser

Feeds und Auslöser sind sich sehr ähnlich, aber technisch gesehen unterschiedliche Konzepte.   

- {{site.data.keyword.openwhisk_short}} verarbeitet **Ereignisse**, die im System eintreffen.

- Ein **Auslöser** ist technisch betrachtet ein Name für eine Klasse von Ereignissen. Jedes Ereignis gehört zu genau einem Auslöser. Ein Auslöser ähnelt daher einem *Thema* ('topic') in themenbasierten Publish/Subscribe-Systemen. Eine **Regel** wie *T -> A* bedeutet: Wenn ein Ereignis des Auslösers *T* eintrifft, soll die Aktion *A* mit den Auslösernutzdaten aufgerufen werden.

- Ein **Feed** ist ein Strom von Ereignissen, die allesamt zu einem Auslöser *T* gehören. Ein Feed wird durch eine **Feedaktion** gesteuert, die das Erstellen, Löschen, Anhalten und Fortsetzen des Ereignisstroms abwickelt, aus dem der Feed besteht. Die Feedaktion interagiert in der Regel mit externen Services, die die Ereignisse über eine REST-API, die Benachrichtigungen verwaltet, erstellen.

##  Feedaktionen implementieren

Die *Feedaktion* ist eine normale {{site.data.keyword.openwhisk_short}}-*Aktion*, die die folgenden Parameter verarbeiten kann:
* **lifecycleEvent**: Einer der folgenden Werte ist gültig: 'CREATE', 'READ', 'UPDATE', 'DELETE', 'PAUSE' oder 'UNPAUSE'.
* **triggerName**: Der vollständig qualifizierte Name des Auslösers, der die Ereignisse enthält, die von diesem Feed generiert werden.
* **authKey**: Die grundlegenden Berechtigungsnachweise des {{site.data.keyword.openwhisk_short}}-Benutzers, der Eigner des Auslösers ist.

Die Feedaktion kann außerdem alle anderen Parameter verarbeiten, die zum Verwalten des Feeds erforderlich sind. Die Feedaktion für {{site.data.keyword.cloudant}}-Änderungen erwartet beispielsweise, dass Parameter mit Werten für *'dbname'*, *'username'* usw. empfangen werden.

Wenn der Benutzer über die Befehlszeilenschnittstelle (CLI) einen Auslöser mit dem Parameter **--feed** erstellt, wird die Feedaktion mit den entsprechenden Parametern automatisch vom System aufgerufen.

Nehmen Sie zum Beispiel an, dass der Benutzer eine Bindung **mycloudant** für das Paket `cloudant` mit einem Benutzernamen und einem Kennwort als gebundene Parameter erstellt. Der Benutzer führt nun den folgenden Befehl über die CLI aus:
```
ibmcloud fn trigger create T --feed mycloudant/changes -p dbName myTable
```
{: pre}

Dadurch wird im System eine Operation ausgeführt, die dem folgenden Befehl entspricht:
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

Die Feedaktion namens *changes* akzeptiert diese Parameter. Von der Feedaktion wird erwartet, dass sie die erforderliche Aktion ausführt, um einen Ereignisstrom von {{site.data.keyword.cloudant_short_notm}} einzurichten. Die Feedaktion erfolgt durch Verwenden der entsprechenden Konfiguration, die auf den Auslöser *T* ausgerichtet ist.

Beim {{site.data.keyword.cloudant_short_notm}}-Feed *changes* kommuniziert die Aktion direkt mit einem *{{site.data.keyword.cloudant_short_notm}}-Auslöser*service, der mit einer verbindungsbasierten Architektur implementiert ist.

Ein ähnliches Feedaktionsprotokoll trifft bei `ibmcloud fn trigger delete`, `ibmcloud fn trigger update` und `ibmcloud fn trigger get` zu.

## Feeds mit Hooks implementieren

Das Einrichten eines Feeds über einen Hook ist einfach, wenn der Ereignisproduzent eine Webhook/Callback-Funktion unterstützt.

Mit dieser Methode ist es _nicht erforderlich_, einen persistenten Service außerhalb von {{site.data.keyword.openwhisk_short}} automatisch zu installieren. Die gesamte Feedverwaltung erfolgt natürlich über statusunabhängige {{site.data.keyword.openwhisk_short}}-*Feedaktionen*, die direkt mit der Webhook-API eines Drittanbieters kommunizieren. 

Bei einem Aufruf mit `CREATE` installiert die Feedaktion einfach einen Webhook für einen anderen Service und fordert diesen fernen Service dazu auf, POST-Benachrichtigungen an die entsprechende URL für die Auslöseraktivierung (`fireTrigger`) in {{site.data.keyword.openwhisk_short}} zu senden.

Der Webhook wird angewiesen, Benachrichtigungen an eine URL wie die folgende zu senden:

`POST /namespaces/{namespace}/triggers/{triggerName}`

Das Formular mit der POST-Anforderung wird als JSON-Dokument interpretiert, das Parameter zum Auslöserereignis definiert. {{site.data.keyword.openwhisk_short}}-Regeln übergeben diese Auslöserparameter an Aktionen, die als Ergebnis des Ereignisses ausgelöst werden.

## Feeds mit Polling implementieren

Eine {{site.data.keyword.openwhisk_short}}-*Aktion* kann so eingerichtet werden, dass eine Feedquelle vollständig innerhalb von {{site.data.keyword.openwhisk_short}} abgefragt wird, ohne dass persistente Verbindungen oder ein externer Service automatisch installiert werden müssen.

Für Feeds, bei denen kein Webhook verfügbar ist, die aber auch nicht auf schnelle Antwortzeiten bei hohem Volumen oder mit geringer Latenz angewiesen sind, ist Polling eine attraktive Option.

Um einen Polling-basierten Feed einzurichten, führt die Feedaktion die folgenden Schritte aus, wenn sie für `CREATE` (Erstellen) aufgerufen wird:

1. Die Feedaktion richtet einen regelmäßigen Auslöser (*T*) mit der gewünschten Häufigkeit ein. Hierzu wird der Feed `whisk.system/alarms` verwendet.
2. Der Feed-Entwickler erstellt die Aktion `pollMyService`, die den fernen Service abruft und alle neuen Ereignisse zurückgibt.
3. Die Feedaktion richtet eine *Regel* *T -> pollMyService* ein.

Mit dieser Vorgehensweise wird ein polling-basierter Auslöser implementiert, der nur {{site.data.keyword.openwhisk_short}}-Aktionen verwendet, ohne dass dazu ein separater Service benötigt wird.

## Feeds mit Verbindungen implementieren

Die vorherigen zwei Architekturoptionen sind einfach zu implementieren. Wenn Sie jedoch einen sehr leistungsfähigen Feed benötigen, sind persistente Verbindungen und Long-Polling- bzw. ähnliche Verfahren notwendig.

Da {{site.data.keyword.openwhisk_short}}-Aktionen eine kurze Laufzeit haben müssen, kann eine Aktion keine persistente Verbindung zu einer dritten Partei verwalten. Stattdessen können Sie einen separaten Service, der als *Provider-Service* bezeichnet wird, außerhalb von {{site.data.keyword.openwhisk_short}} einrichten, der die gesamte Zeit aktiv ist. Ein Provider-Service kann Verbindungen zu Drittanbieter-Ereignisquellen verwalten, die Long-Polling- oder andere verbindungsbasierte Benachrichtigungen unterstützen.

Der Provider-Service hat eine REST-API, die es der {{site.data.keyword.openwhisk_short}}-*Feedaktion* ermöglicht, den Feed zu steuern. Der Provider-Service fungiert als Proxy zwischen dem Ereignisprovider und {{site.data.keyword.openwhisk_short}}. Wenn er vom Drittanbieter Ereignisse empfängt, sendet er sie an {{site.data.keyword.openwhisk_short}}, indem er einen Auslöser aktiviert.

Der {{site.data.keyword.cloudant_short_notm}}-Feed *changes* ist ein kanonisches Beispiel. Er veranlasst die automatische Installation des Service `cloudanttrigger`, der über eine persistente Verbindung zwischen {{site.data.keyword.cloudant_short_notm}}-Benachrichtigungen und {{site.data.keyword.openwhisk_short}}-Auslösern vermittelt.


Der Feed *alarm* wird anhand eines ähnlichen Musters implementiert.

Die verbindungsbasierte Architektur ist die beste Leistungsoption. Allerdings fällt ein hoher Aufwand an Operationen im Vergleich zu Polling- und Hook-Architekturen an.
