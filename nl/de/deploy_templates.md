---

copyright:

  years: 2018

lastupdated: "2018-05-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Quickstart-Vorlagen bereitstellen
{: #serviceauth}

{{site.data.keyword.openwhisk}} stellt einen Katalog von Vorlagen bereit, um Ihren Einstieg in das nächste Projekt zu beschleunigen. Vorlagen sind eine Kombination aus Aktionen, Auslösern und Sequenzen und können darüber hinaus Serviceinstanzen aus {{site.data.keyword.Bluemix}} beinhalten. Mithilfe von Vorlagen können Sie rasch und ohne großen Aufwand ein Projekt erstellen und unverzüglich mit der Programmierung beginnen.

Dieses Lernprogramm führt Sie durch die Schritte zur Bereitstellung der Cloudant-Vorlage (Cloudant Template).
{: shortdesc}

## Verfügbare Quickstart-Vorlagen
{: #available-templates}

| Name | Beschreibung | Unterstützte Laufzeiten |
|:-----------------|:-----------------|:-----------------|
| [Cloudant Events](./deploy_templates.html#cloudant-template) | Wenn eine Cloudant-Datenbank dokumentiert, bearbeitet oder hinzugefügt wurde, wird die Änderung in der Konsole protokolliert. | Node.js, Swift, Python, PHP |
| [Get HTTP Resource](./deploy_templates.html#get-http-resource-template) | Eine Webaktion, die als Reaktion auf ein HTTP-Ereignis aufgerufen wird und Daten aus der Yahoo Weather-API abruft. | Node.js, Python |
| [Hello World](./deploy_templates.html#hello-world-template) | Diese Aktion akzeptiert nur einen Parameter, bei dem es sich um ein JSON-Objekt handeln muss. | Node.js, Swift, Python, PHP |
| [Message Hub Events](./deploy_templates.html#messagehub-events-template) | Wenn für ein Message Hub-Thema neue Daten hinzugefügt wurden, wird die Änderung in der Konsole protokolliert. | Node.js, Swift, Python, PHP |
| [Periodic Slack Reminder](./deploy_templates.html#slack-reminder-template) | Eine Aktion, die Beiträge an Slack auf Basis eines regelmäßigen Auslösers sendet. | Node.js, Swift, Python, PHP |

## Cloudant Events-Vorlage bereitstellen
{: #cloudant-template}

Die Cloudant-Vorlage erstellt eine Sequenz von Aktionen und einen Auslöser, der diese Sequenz startet. Der Auslöser wird gestartet, wenn eine Änderung in der verbundenen Cloudant-Datenbank erfolgt, bei der es sich um eine Datenbank mit Katzen handelt, die Name und Farbe enthält. Das erwartete Datenelement ist eine Katze mit einem definierten Namen und einer definierten Farbe. Wenn in der Datenbank eine neue Katze hinzugefügt oder eine aktuelle Katze bearbeitet wird, werden die Daten in der Konsole protokolliert.

1. Zum Erstellen einer Vorlage wechseln Sie zu [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) und klicken auf **Erstellung starten**.

2. Klicken Sie auf **Quickstart-Vorlagen**.

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

Klicken Sie auf **Bereitstellen**.

Nach der Vorlagenbereitstellung können Sie weitere Bearbeitungen am Code vornehmen, um ihn nach Bedarf anzupassen. Oder Sie kehren zurück und prüfen den Katalog mit den verfügbaren Vorlagen.

## Get HTTP Resource-Vorlage bereitstellen
{: #get-http-resource-template}

Die Get HTTP Resource-Vorlage erstellt eine Aktion, um eine externe Ressource abzurufen (die Yahoo Weather-API) und gibt dann Daten zurück. Die Aktion ist als Webaktion aktiviert, sodass sie mit einer URL aufgerufen werden kann, die für CORS aktiviert ist und keinen Authentifizierungsschlüssel benötigt, was für die Erstellung von Back-Ends für Webanwendungen nützlich ist. **Hinweis**: Standardmäßig ist der Endpunkt `get-http-resource` öffentlich für alle Benutzer verfügbar, die ihn aufrufen wollen.

1. Zum Erstellen einer Vorlage wechseln Sie zu [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) und klicken auf **Erstellung starten**.

2. Klicken Sie auf **Quickstart-Vorlagen**.

3. Überprüfen Sie das Feld **Paketname** und aktualisieren Sie es bei Bedarf. Der Standardwert wird für Sie auf `get-http-resource` festgelegt.

4. Wählen Sie die Laufzeit für die Aktionen aus, deren Eigner Sie sein werden: Node.js 8, Node.js 6 oder Python 3.

5. Klicken Sie auf **Bereitstellen**.

Nach der Vorlagenbereitstellung können Sie weitere Bearbeitungen am Code vornehmen, um ihn nach Bedarf anzupassen. Oder Sie kehren zurück und prüfen den Katalog mit den verfügbaren Vorlagen.

## Hello World-Vorlage bereitstellen
{: #hello-world-template}

Diese Aktion akzeptiert nur einen Parameter, bei dem es sich um ein JSON-Objekt handeln muss.

1. Zum Erstellen einer Vorlage wechseln Sie zu [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) und klicken auf **Erstellung starten**.

2. Klicken Sie auf **Quickstart-Vorlagen**.

3. Überprüfen Sie das Feld **Paketname** und aktualisieren Sie es bei Bedarf. Der Standardwert wird für Sie auf `hello-world` festgelegt.

4. Wählen Sie die Laufzeit für die Aktionen aus, deren Eigner Sie sein werden: Node.js 8, Node.js 6, Python 3, Swift 4 oder PHP 7.1.

5. Klicken Sie auf **Bereitstellen**.

Nach der Vorlagenbereitstellung können Sie weitere Bearbeitungen am Code vornehmen, um ihn nach Bedarf anzupassen. Oder Sie kehren zurück und prüfen den Katalog mit den verfügbaren Vorlagen.

## Message Hub-Vorlage bereitstellen
{: #messagehub-events-template}

Die Message Hub Events-Vorlage erstellt eine Aktion und einen Auslöser, der diese Aktion startet. Der Auslöser wird immer dann aktiviert, wenn ein neues Element zum Thema "Message Hub" hinzugefügt wird, das während der Vorlagenerstellung ausgewählt wird.

1. Zum Erstellen einer Vorlage wechseln Sie zu [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) und klicken auf **Erstellung starten**.

2. Klicken Sie auf **Quickstart-Vorlagen**.

3. Überprüfen Sie das Feld **Paketname** und aktualisieren Sie es bei Bedarf. Der Standardwert wird für Sie auf `message-hub-events` festgelegt.

4. Wählen Sie die Laufzeit für die Aktionen aus, deren Eigner Sie sein werden: Node.js 8, Node.js 6, Python 3, Swift 4 oder PHP 7.1.

5. Klicken Sie auf **Weiter**.

### Message Hub-Auslöser erstellen

Auslöser rufen Aktionen auf, wenn sie Ereignisse aus Ereignisquellen empfangen. Geben Sie zum Erstellen eines Auslösers für die Message Hub-Vorlage den Auslöser mit den erforderlichen Informationen zur Message Hub-Serviceinstanz an.

Überprüfen Sie das Feld **Auslösername** und aktualisieren Sie es bei Bedarf. Der Standardwert wird für Sie auf `message-hub-events-trgr` festgelegt.

### Message Hub-Serviceinstanz erstellen

Sie haben die folgenden Auswahlmöglichkeiten:
  * **Eigene Instanz erstellen**
  * **Eigene Berechtigungsnachweise eingeben**

1. Wählen Sie zum Beispiel die Option **Eigene Instanz erstellen** aus.

2. Ein Popup-Fenster führt Sie zu einer neuen Registerkarte mit der Message Hub-Konfigurationsseite. Nach der Erstellung der Message Hub-Instanz müssen Sie eine Gruppe von Serviceberechtigungsnachweisen erstellen und anschließend die Registerkarte schließen, um zu dieser Seite zurückzukehren, indem Sie auf **OK** klicken.

3. Wählen Sie jetzt **Eigene Berechtigungsnachweise eingeben** aus und geben Sie die folgenden Informationen an:
  * Benutzername - _Ihr Message Hub-Benutzername_
  * Kennwort - _Ihr Message Hub-Kennwort_
  * kafka_admin_url - _Die REST-URL für den Message Hub-Administrator_
  * Datenbank - _Der Name Ihrer Message Hub-Datenbank_
  * Thema - _Zu abonnierendes Thema_

### Message Hub-Vorlage bereitstellen

Klicken Sie auf **Bereitstellen**.

Nach der Vorlagenbereitstellung können Sie weitere Bearbeitungen am Code vornehmen, um ihn nach Bedarf anzupassen. Oder Sie kehren zurück und prüfen den Katalog mit den verfügbaren Vorlagen.

## Periodic Slack Reminder-Vorlage bereitstellen
{: #slack-reminder-template}

Die Vorlage 'Periodic Slack Reminder' sendet Beiträge an Slack auf der Basis eines Intervalls, das vom Benutzer bei der Auslösererstellung angegeben wird. Bevor Sie diese Vorlage erstellen, rufen Sie die Website https://api.slack.com/incoming-webhooks auf, um die erforderliche URL für eingehende Webhooks einzurichten.

1. Überprüfen Sie das Feld **Paketname** und aktualisieren Sie es bei Bedarf. Der Standardwert wird für Sie auf `periodic-slack-reminder` festgelegt.

2. Wählen Sie die Laufzeit für die Aktionen aus, deren Eigner Sie sein werden: Node.js 8, Node.js 6, Python 3, Swift 4 oder PHP 7.1.

3. Geben Sie im Abschnitt **Parameter** die Webhook-URL in das Feld **Parameterwert** ein und klicken Sie anschließend auf **Weiter**. (Beispiel: https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX).

### Slack Reminder-Auslöser erstellen

Auslöser rufen Aktionen auf, wenn sie Ereignisse aus Ereignisquellen empfangen. Geben Sie zum Erstellen eines Auslösers für die Slack Reminder-Vorlage den Auslöser mit den erforderlichen Informationen zur Message Hub-Serviceinstanz an.

1. Überprüfen Sie das Feld **Auslösername** und aktualisieren Sie es bei Bedarf. Der Standardwert wird für Sie auf `periodic-slack-reminder-trgr` festgelegt.

2. Als Nächstes können Sie das Intervall angeben, in dem der Auslöser aktiviert werden soll, indem Sie ein Muster oder einen Cron-Ausdruck verwenden. Sie können UTC-Zeiten für Wochentage, Stunden und Minuten auswählen. Wenn Sie die gewünschten Intervalloptionen ausgewählt haben, sind Sie für die Vorlagenbereitstellung bereit.

3. Klicken Sie auf **Bereitstellen**.

Nach der Vorlagenbereitstellung können Sie weitere Bearbeitungen am Code vornehmen, um ihn nach Bedarf anzupassen. Oder Sie kehren zurück und prüfen den Katalog mit den verfügbaren Vorlagen.
