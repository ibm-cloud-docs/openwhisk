---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: feeds, serverless, functions

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



# Angepasste Feeds für Ereignisprovider erstellen
{: #feeds_custom}

{{site.data.keyword.openwhisk_short}} unterstützt eine offene API, mit der jeder Benutzer einen Ereignisproduzentenservice als Feed in einem Paket verfügbar machen kann.
{: shortdesc}


## Feedarchitektur
{: #feeds_arch}

Für die Erstellung von Feeds können Sie eines von drei Architekturmustern verwenden: **Hooks**, **Polling** und **Verbindungen**. 

### Hooks

Mit dem Muster 'Hooks' wird der Feed mit einem [Webhook](https://en.wikipedia.org/wiki/Webhook){: external} eingerichtet, der von einem anderen Service verfügbar gemacht wird. Bei diesem Verfahren wird ein Webhook in einem externen Service konfiguriert, um direkt POST-Anforderungen an eine URL zu senden, durch die ein Auslöser aktiviert wird. Diese Methode ist mit Abstand die einfachste und bequemste Option, Feeds mit geringer Häufigkeit zu implementieren.

Beispielsweise verwenden das [GitHub-Paket](/docs/openwhisk?topic=cloud-functions-pkg_github) und das [Push Notifications-Paket](/docs/openwhisk?topic=cloud-functions-pkg_push_notifications) einen Webhook. 


### Polling

Mit dem 'Polling' wird eine {{site.data.keyword.openwhisk_short}}-Aktion so eingerichtet, dass sie einen Endpunkt regelmäßig abfragt, um neue Daten abzurufen. Dieses Muster ist relativ einfach zu erstellen, aber die Häufigkeiten der Ereignisse werden durch das Polling-Intervall begrenzt.

### Verbindungen

Mit dem Muster 'Verbindungen' verwaltet ein separater Service eine persistente Verbindung zu einer Feedquelle. Die verbindungsbasierte Implementierung kann mit einem Serviceendpunkt durch lange Polling-Intervalle interagieren. Es kann auch eine Push-Benachrichtigung eingerichtet werden.

Beispielsweise verwendet das [{{site.data.keyword.cloudant}}-Paket](/docs/openwhisk?topic=cloud-functions-pkg_cloudant) das Muster 'Verbindungen'.



##  Feedaktionen implementieren
{: #feeds_actions}

Die Feedaktion ist eine Aktion, die die folgenden Parameter akzeptiert. 

| Parameter | Beschreibung |
| --- | --- |
| `lifecycleEvent` | `CREATE`, `READ`, `UPDATE`, `DELETE`, `PAUSE` oder `UNPAUSE`. |
| `triggerName` | Der vollständig qualifizierte Name des Auslösers. Dieser enthält Ereignisse, die von diesem Feed generiert werden. |
| `authKey` | Die grundlegenden Berechtigungsnachweise des {{site.data.keyword.openwhisk_short}}-Benutzers, der Eigner des Auslösers ist. |

Die Feedaktion kann außerdem alle anderen Parameter verarbeiten, die zum Verwalten des Feeds erforderlich sind. Die Feedaktion für {{site.data.keyword.cloudant}}-Änderungen erwartet beispielsweise, dass Parameter mit Werten für `dbname`, `username` usw. empfangen werden.

Wenn der Benutzer über die Befehlszeilenschnittstelle (CLI) einen Auslöser mit dem Parameter `--feed` erstellt, wird die Feedaktion mit den entsprechenden Parametern automatisch aufgerufen.

Beispiel: Der Benutzer erstellt eine Bindung **mycloudant** für das Paket `cloudant` mit einem Benutzernamen und einem Kennwort als gebundene Parameter. Der Benutzer führt nun den folgenden Befehl über die CLI aus:
```
ibmcloud fn trigger create my_cloudant_trigger --feed mycloudant/changes -p dbName myTable
```
{: pre}

Anschließend wird ein Befehl ähnlich dem folgenden ausgeführt:
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

Die Feedaktion namens *changes* akzeptiert diese Parameter. Von der Feedaktion wird erwartet, dass sie die erforderliche Aktion ausführt, um einen Ereignisstrom von {{site.data.keyword.cloudant_short_notm}} einzurichten. Die Feedaktion erfolgt durch Verwenden der entsprechenden Konfiguration, die auf den Auslöser ausgerichtet ist.

Beim {{site.data.keyword.cloudant_short_notm}}-Feed *changes* kommuniziert die Aktion direkt mit einem *{{site.data.keyword.cloudant_short_notm}}-Auslöser*service, der mit einer verbindungsbasierten Architektur implementiert ist.

Ein ähnliches Feedaktionsprotokoll trifft bei `ibmcloud fn trigger delete`, `ibmcloud fn trigger update` und `ibmcloud fn trigger get` zu. 

## Feeds mit Hooks implementieren
{: #feeds_hooks}

Richten Sie einen Feed mithilfe eines Hooks ein, wenn der Ereignisproduzent eine Webhook-Callback-Funktion unterstützt. 

Bei dieser Methode ist es nicht erforderlich, einen persistenten Service außerhalb von {{site.data.keyword.openwhisk_short}} automatisch zu installieren. Die gesamte Feedverwaltung erfolgt auf natürliche Weise über statusunabhängige {{site.data.keyword.openwhisk_short}}-**Feedaktionen**, die direkt mit der Webhook-API eines Drittanbieters kommunizieren. 

Bei einem Aufruf mit `CREATE` installiert die Feedaktion einfach einen Webhook für einen anderen Service und fordert diesen fernen Service dazu auf, POST-Benachrichtigungen an die entsprechende URL für die Auslöseraktivierung (`fireTrigger`) in {{site.data.keyword.openwhisk_short}} zu senden.

Der Webhook wird angewiesen, Benachrichtigungen an eine URL wie die folgende zu senden:

`POST /namespaces/{namespace}/triggers/{triggerName}`

Das Formular mit der POST-Anforderung wird als JSON-Dokument interpretiert, das Parameter zum Auslöserereignis definiert. {{site.data.keyword.openwhisk_short}}-Regeln übergeben diese Auslöserparameter an Aktionen, die als Ergebnis des Ereignisses ausgelöst werden.

## Feeds mit Polling implementieren
{: #feeds_polling}

Sie können eine Aktion so einrichten, dass eine Feedquelle vollständig innerhalb von {{site.data.keyword.openwhisk_short}} abgefragt wird, ohne dass persistente Verbindungen oder ein externer Service automatisch installiert werden müssen.

Für Feeds, bei denen kein Webhook verfügbar ist, die aber auch nicht auf schnelle Antwortzeiten bei hohem Volumen oder mit geringer Latenz angewiesen sind, können Sie das Polling verwenden.

Um einen Polling-basierten Feed einzurichten, führt die Feedaktion die folgenden Schritte aus, wenn sie für `CREATE` (Erstellen) aufgerufen wird:

1. Die Feedaktion richtet einen regelmäßigen Auslöser mit einer bestimmten Häufigkeit ein. Hierzu wird der Feed `whisk.system/alarms` verwendet. 
2. Der Feedentwickler erstellt die Aktion `pollMyService`, die den fernen Service abruft und alle neuen Ereignisse zurückgibt.
3. Die Feedaktion richtet eine *Regel* *T -> pollMyService* ein.

Mit dieser Vorgehensweise wird ein polling-basierter Auslöser implementiert, der nur {{site.data.keyword.openwhisk_short}}-Aktionen verwendet, ohne dass dazu ein separater Service benötigt wird.

## Feeds mit Verbindungen implementieren
{: #feeds_connections}

Die vorherigen zwei Architekturoptionen sind einfach zu implementieren. Wenn Sie jedoch einen sehr leistungsfähigen Feed wünschen, können Sie persistente Verbindungen und Long-Polling- bzw. ähnliche Verfahren verwenden. 

Da {{site.data.keyword.openwhisk_short}}-Aktionen eine kurze Laufzeit haben müssen, kann eine Aktion keine persistente Verbindung zu einer dritten Partei verwalten. Stattdessen können Sie einen separaten Service, der als **Provider-Service** bezeichnet wird, außerhalb von {{site.data.keyword.openwhisk_short}} einrichten, der die gesamte Zeit aktiv ist. Ein Provider-Service kann Verbindungen zu Drittanbieter-Ereignisquellen verwalten, die Long-Polling- oder andere verbindungsbasierte Benachrichtigungen unterstützen.

Der Provider-Service hat eine REST-API, die der {{site.data.keyword.openwhisk_short}}-**Feedaktion** die Steuerung des Feeds ermöglicht. Der Provider-Service fungiert als Proxy zwischen dem Ereignisprovider und {{site.data.keyword.openwhisk_short}}. Wenn er vom Drittanbieter Ereignisse empfängt, sendet er sie an {{site.data.keyword.openwhisk_short}}, indem er einen Auslöser aktiviert.

Der {{site.data.keyword.cloudant_short_notm}}-Feed **changes** ist ein kanonisches Beispiel. Er veranlasst die automatische Installation des Service `cloudanttrigger`, der über eine persistente Verbindung zwischen {{site.data.keyword.cloudant_short_notm}}-Benachrichtigungen und {{site.data.keyword.openwhisk_short}}-Auslösern vermittelt. 


Der Feed **alarm** wird anhand eines ähnlichen Musters implementiert.

Die verbindungsbasierte Architektur ist die Option mit der höchsten Leistung. Allerdings sind die Operationen im Vergleich zu Polling- und Hook-Architekturen arbeitsintensiver. 






