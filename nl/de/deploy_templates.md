---

copyright:

  years: 2018

lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Vorlagen in {{site.data.keyword.openwhisk_short}} bereitstellen
{: #serviceauth}

{{site.data.keyword.openwhisk}} stellt einen Katalog von Vorlagen bereit, um Ihren Einstieg in das nächste Projekt zu beschleunigen. Vorlagen sind eine Kombination aus Aktionen, Auslösern und Sequenzen und können darüber hinaus Serviceinstanzen aus {{site.data.keyword.Bluemix}} beinhalten. Mithilfe von Vorlagen können Sie rasch und ohne großen Aufwand ein Projekt erstellen und unverzüglich mit der Programmierung beginnen. 

Dieses Lernprogramm führt Sie durch die Schritte zur Bereitstellung der Cloudant-Vorlage (Cloudant Template).
{: shortdesc}

## Verfügbare Vorlagen
{: #available-templates}

| Name | Beschreibung | Unterstützte Laufzeiten | 
|:-----------------|:-----------------|:-----------------|
| Cloudant Events | Wenn eine Cloudant-Datenbank dokumentiert, bearbeitet oder hinzugefügt wurde, wird die Änderung in der Konsole protokolliert. | Node.js, Swift, Python, PHP |
| Get HTTP Resource | Eine Webaktion, die als Reaktion auf ein HTTP-Ereignis aufgerufen wird und Daten aus der Yahoo Weather-API abruft. | Node.js, Python |
| Hello World | Diese Aktion akzeptiert nur einen Parameter, bei dem es sich um ein JSON-Objekt handeln muss. | Node.js, Swift, Python, PHP |
| Message Hub Events | Wenn für ein Message Hub-Thema neue Daten hinzugefügt wurden, wird die Änderung in der Konsole protokolliert. | Node.js, Swift, Python, PHP | 
| Periodic Slack Reminder | Eine Aktion, die Beiträge an Slack auf Basis eines regelmäßigen Auslösers sendet. | Node.js, Swift, Python, PHP |

## Cloudant Events-Vorlage bereitstellen
{: #cloudant-template}

Die Cloudant-Vorlage erstellt eine Sequenz von Aktionen und einen Auslöser, der diese Sequenz startet. Der Auslöser wird gestartet, wenn eine Änderung in der verbundenen Cloudant-Datenbank erfolgt, bei der es sich um eine Datenbank mit Katzen handelt, die Name und Farbe enthält. Das erwartete Datenelement ist eine Katze mit einem definierten Namen und einer definierten Farbe. Wenn in der Datenbank eine neue Katze hinzugefügt oder eine aktuelle Katze bearbeitet wird, werden die Daten in der Konsole protokolliert. 

1. Zum Erstellen einer Vorlage wechseln Sie zu [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://dev-console.stage1.bluemix.net/openwhisk/) und klicken auf **Erstellung starten**. 

2. Klicken Sie auf **Vorlage bereitstellen**.

3. Klicken Sie auf **Neues Cloudant-Element**.

### Cloudant-Aktion erstellen

1. Geben Sie als Nächstes einen Namen für Ihr Paket an oder verwenden Sie den bereitgestellten Standardnamen `new-cloudant-item`. 

2. Wählen Sie über die Dropdown-Liste **Aktionen** die Laufzeit für die Aktionen aus, deren Eigner Sie sein werden (nodejs, swift, python oder php). Wählen Sie zum Beispiel **nodejs** aus und klicken Sie auf **Weiter**.

### Cloudant-Auslöser erstellen

Auslöser rufen Aktionen auf, wenn sie Ereignisse aus Ereignisquellen empfangen. Geben Sie zum Erstellen eines Auslösers für die Cloudant-Vorlage (Cloudant Template) den Auslöser mit den erforderlichen Informationen zur Cloudant-Serviceinstanz an.

#### Cloudant-Serviceinstanz erstellen

Sie haben die folgenden Auswahlmöglichkeiten:
  * **Eigene Instanz erstellen**
  * **Eigene Berechtigungsnachweise eingeben** 

1. Wählen Sie zum Beispiel die Option **Eigene Instanz erstellen** aus.

2. Ein Popup-Fenster führt Sie zu einer neuen Registerkarte mit der Cloudant-Konfigurationsseite (Cloudant Setup). Nach der Erstellung der Cloudant-Instanz müssen Sie eine Gruppe von Serviceberechtigungsnachweisen erstellen und anschließend die Registerkarte schließen, um zu dieser Seite zurückzukehren, indem Sie auf **OK** klicken.

3. Wählen Sie jetzt **Eigene Berechtigungsnachweise eingeben** aus und geben Sie die folgenden Informationen an:
  * Benutzername - _Ihr Cloudant-Benutzername_
  * Kennwort - _Ihr Cloudant-Kennwort_
  * Host - _Dies ist in der Regel der für Sie gültige `benutzername.cloudant.com`_
  * Datenbank - _Der Name Ihrer Cloudant-Datenbank_

### Cloudant-Vorlage bereitstellen

1. Klicken Sie auf **Bereitstellen**.

Nach der Vorlagenbereitstellung können Sie weitere Bearbeitungen am Code vornehmen, um ihn nach Bedarf anzupassen. Oder Sie kehren zurück und prüfen den Katalog mit den verfügbaren Vorlagen.

