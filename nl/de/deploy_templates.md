---

copyright:
  years: 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Quickstart-Vorlagen bereitstellen
{: #serviceauth}

{{site.data.keyword.openwhisk}} stellt einen Katalog von Vorlagen bereit, um Ihren Einstieg in das nächste Projekt zu beschleunigen. Vorlagen sind eine Kombination aus Aktionen, Auslösern und Sequenzen und können darüber hinaus Serviceinstanzen aus {{site.data.keyword.Bluemix}} beinhalten. Mithilfe von Vorlagen können Sie rasch und ohne großen Aufwand ein Projekt erstellen und unverzüglich mit der Programmierung beginnen.

In diesem Lernprogramm werden Sie Schritt für Schritt durch die Implementierung der Schablonen geführt, die über {{site.data.keyword.openwhisk_short}} verfügbar sind.
{: shortdesc}

## Verfügbare Quickstart-Schablonen
{: #available-templates}

| Name | Beschreibung | Unterstützte Laufzeiten |
|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} Events](./deploy_templates.html#cloudant-template) | Bei Hinzufügen eines Dokuments zu oder Bearbeiten eines Dokuments in einer {{site.data.keyword.cloudant}}-Datenbank wird die Änderung in der Konsole protokolliert. | Node.js, Swift, Python, PHP |
| [Upload Image](./deploy_templates.html#cos-upload-image) | Eine Webaktion, die das Hochladen eines Bildes in das Bucket einer {{site.data.keyword.cos_full}}-Instanz und das anschließende Abrufen eines Piktogramms dieses Bilds ermöglicht. | Node.js |
| [Get HTTP Resource](./deploy_templates.html#get-http-resource-template) | Eine Webaktion, die als Reaktion auf ein HTTP-Ereignis aufgerufen wird und dann Daten aus der Yahoo Weather-API abruft. | Node.js, Python |
| [Hello World](./deploy_templates.html#hello-world-template) | Diese Aktion akzeptiert nur einen Parameter, bei dem es sich um ein JSON-Objekt handeln muss. | Node.js, Swift, Python, PHP |
| [{{site.data.keyword.messagehub}} Events](./deploy_templates.html#messagehub-events-template) | Bei Hinzufügen neuer Daten für ein {{site.data.keyword.messagehub_full}}-Thema wird die Änderung in der Konsole protokolliert. | Node.js, Swift, Python, PHP |
| [Periodic Slack Reminder](./deploy_templates.html#slack-reminder-template) | Eine Aktion, die Beiträge an Slack auf Basis eines regelmäßigen Auslösers sendet. | Node.js, Swift, Python, PHP |

## {{site.data.keyword.cloudant_short_notm}} Events-Vorlage bereitstellen
{: #cloudant-template}

Die {{site.data.keyword.cloudant_short_notm}}-Vorlage erstellt eine Sequenz von Aktionen und einen Auslöser, der diese Sequenz startet. Der Auslöser wird gestartet, wenn eine Änderung in der verbundenen {{site.data.keyword.cloudant_short_notm}}-Datenbank erfolgt, bei der es sich um eine Datenbank mit Katzen handelt, die jeweils Namen und Farben enthält. Das erwartete Datenelement ist eine Katze mit einem definierten Namen und einer definierten Farbe. Wenn eine neue Katze zu der Datenbank hinzugefügt oder eine dort vorhandene Katze bearbeitet wird, werden die Daten in der Konsole protokolliert.

1. Wechseln Sie zum Erstellen einer Vorlage zu [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) und klicken Sie dann auf **Erstellung starten**.

2. Klicken Sie auf **Quickstart-Vorlagen**.

3. Klicken Sie auf **Cloudant Events**.

### {{site.data.keyword.cloudant_short_notm}}-Aktion erstellen

1. Geben Sie als Nächstes einen Namen für Ihr Paket an oder verwenden Sie den bereitgestellten Standardnamen `new-cloudant-item`.

2. Wählen Sie über die Dropdown-Liste **Aktionen** die Laufzeit für die Aktionen aus, deren Eigner Sie sein werden (nodejs, swift, python oder php). Wählen Sie zum Beispiel **nodejs** aus und klicken Sie auf **Weiter**.

### {{site.data.keyword.cloudant_short_notm}}-Auslöser erstellen

Auslöser rufen Aktionen auf, wenn sie Ereignisse aus Ereignisquellen empfangen. Geben Sie zum Erstellen eines Auslösers für die {{site.data.keyword.cloudant_short_notm}}-Vorlage den Auslöser mit den erforderlichen Informationen zur {{site.data.keyword.cloudant_short_notm}}-Serviceinstanz an.

#### {{site.data.keyword.cloudant_short_notm}}-Serviceinstanz erstellen

Sie haben die folgenden Auswahlmöglichkeiten:
  * **Eigene Instanz erstellen**
  * **Eigene Berechtigungsnachweise eingeben**

1. Wählen Sie zum Beispiel die Option **Eigene Instanz erstellen** aus.

2. Über ein Popup-Fenster gelangen Sie zu einer neuen Registerkarte mit der {{site.data.keyword.cloudant_short_notm}}-Konfigurationsseite. Nachdem Sie die {{site.data.keyword.cloudant_short_notm}}-Instanz erstellt haben, müssen Sie eine Gruppe von Serviceberechtigungsnachweisen erstellen und anschließend zu dieser Seite zurückkehren, indem Sie die Registerkarte durch Klicken auf **OK** schließen.

3. Wählen Sie jetzt **Eigene Berechtigungsnachweise eingeben** aus und geben Sie die folgenden Informationen an:
  * Benutzername - _Ihr {{site.data.keyword.cloudant_short_notm}}-Benutzername_
  * Kennwort - _Ihr {{site.data.keyword.cloudant_short_notm}}-Kennwort_
  * Host - _Dies ist in der Regel Ihr `benutzername.{{site.data.keyword.cloudant_short_notm}}.com`_
  * Datenbank - _Der Name Ihrer {{site.data.keyword.cloudant_short_notm}}-Datenbank_

### {{site.data.keyword.cloudant_short_notm}}-Vorlage bereitstellen

Klicken Sie auf **Bereitstellen**.

Nach der Vorlagenbereitstellung können Sie den Code durch weiteres Bearbeiten nach Bedarf anpassen oder zurückgehen und den Katalog mit den verfügbaren Vorlagen überprüfen.

## Upload Image-Vorlage bereitstellen
{: #cos-upload-image}

Die Upload Image-Vorlage bewirkt die Erstellung einer Webaktion, die Ihnen ermöglicht, ein Bild über eine kleine Schnittstelle in ein {{site.data.keyword.cos_short_notm}}-Bucket hochzuladen. Die Vorlage ruft dann das Bild als Piktogramm ab und zeigt es auf der Schnittstelle der Webaktion an.

So implementieren Sie die Schablone:

1. Öffnen Sie die {{site.data.keyword.openwhisk_short}}-Konsole in [{{site.data.keyword.Bluemix_notm}} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/openwhisk/).

2. Klicken Sie auf **Erstellung starten**.

2. Klicken Sie auf **Quickstart-Vorlagen**.

3. Klicken Sie auf die **Upload Image**-Vorlage.

4. Geben Sie einen Namen für Ihr Paket ein oder übernehmen Sie den bereitgestellten Standardnamen `upload-image`.

5. Klicken Sie auf **Weiter**.

6. Für die Vorlage sind Serviceberechtigungsnachweise einer {{site.data.keyword.cos_full_notm}}-Serviceinstanz erforderlich. Wählen Sie in der **{{site.data.keyword.cos_short}}**-Liste eine der folgenden Optionen aus:
  * **Neue Instanz erstellen**: Wenn Sie über keine Serviceinstanz verfügen, wählen Sie diese Option aus, um eine Instanz zu erstellen.
      1. Erstellen Sie auf der Seite zum Erstellen einer {{site.data.keyword.cos_full_notm}}-Serviceinstanz, die geöffnet wird, eine Serviceinstanz.
      2. [Erstellen Sie eine Gruppe von HMAC-Serviceberechtigungsnachweisen](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials).
      3. [Erstellen Sie mindestens ein Bucket](/docs/services/cloud-object-storage/getting-started.html#create-buckets).
  * **Eigene Berechtigungsnachweise eingeben**: Wählen Sie diese Option aus, um Ihre eigenen Berechtigungsnachweise für eine {{site.data.keyword.cos_short}}-Serviceinstanz manuell einzugeben. Die Berechtigungsnachweise müssen über HMAC-Schlüssel verfügen und die Serviceinstanz muss mindestens ein Bucket besitzen.
  * **Vorhandene Instanzen**: Falls Sie über vorhandene {{site.data.keyword.cos_short}}-Instanzen verfügen, wählen Sie eine dieser Instanzen aus der Liste aus. Die Berechtigungsnachweise müssen über HMAC-Schlüssel verfügen und die Serviceinstanz muss mindestens ein Bucket besitzen.

7. Klicken Sie auf **Bereitstellen**.

8. Klicken Sie im Navigationsbereich links auf den Abschnitt **Endpunkte**.

9. Kopieren Sie im Abschnitt 'Webaktion' den Link ohne das Suffix '.json' und fügen Sie ihn in die Adressleiste des Browsers ein. Die Schnittstelle für die Webaktion der Vorlage wird angezeigt.

10. Optional: Nach der Bereitstellung der Vorlage können Sie zum Dashboard 'Aktionen' navigieren und dort den Code in den folgenden zwei neuen Paketen anpassen:
    * Paket `cloud-object-storage`, das Aktionen enthält, die mit {{site.data.keyword.cos_short}}-Instanzen arbeiten
    * Vorlagenpaket (Standardname `upload-image`), das die Aktion `app` enthält

## Get HTTP Resource-Vorlage bereitstellen
{: #get-http-resource-template}

Die Get HTTP Resource-Vorlage erstellt eine Aktion, um eine externe Ressource (die Yahoo Weather-API) abzurufen, und gibt dann Daten zurück. Die Aktion ist als Webaktion aktiviert, sodass sie mit einer CORS-fähigen URL aufgerufen werden kann und keinen Authentifizierungsschlüssel benötigt, was für die Erstellung von Back-Ends für Webanwendungen nützlich ist. **Hinweis**: Standardmäßig ist der Endpunkt `get-http-resource` öffentlich für alle Benutzer verfügbar, die ihn aufrufen wollen.

1. Wechseln Sie zum Erstellen einer Vorlage zu [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) und klicken Sie dann auf **Erstellung starten**.

2. Klicken Sie auf **Quickstart-Vorlagen**.

3. Überprüfen Sie das Feld **Paketname** und aktualisieren Sie es bei Bedarf. Der Standardwert wird für Sie auf `get-http-resource` festgelegt.

4. Wählen Sie die Laufzeit für die Aktionen aus, deren Eigner Sie sein werden: Node.js 8, Node.js 6 oder Python 3.

5. Klicken Sie auf **Bereitstellen**.

Nach der Vorlagenbereitstellung können Sie den Code durch weiteres Bearbeiten nach Bedarf anpassen oder zurückgehen und den Katalog mit den verfügbaren Vorlagen überprüfen.

## Hello World-Vorlage bereitstellen
{: #hello-world-template}

 Diese Aktion akzeptiert nur einen Parameter, bei dem es sich um ein JSON-Objekt handeln muss. 

1. Wechseln Sie zum Erstellen einer Vorlage zu [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) und klicken Sie dann auf **Erstellung starten**.

2. Klicken Sie auf **Quickstart-Vorlagen**.

3. Überprüfen Sie das Feld **Paketname** und aktualisieren Sie es bei Bedarf. Der Standardwert wird für Sie auf `hello-world` festgelegt.

4. Wählen Sie die Laufzeit für die Aktionen aus, deren Eigner Sie sein werden: Node.js 8, Node.js 6, Python 3, Swift 4 oder PHP 7.1.

5. Klicken Sie auf **Bereitstellen**.

Nach der Vorlagenbereitstellung können Sie den Code durch weiteres Bearbeiten nach Bedarf anpassen oder zurückgehen und den Katalog mit den verfügbaren Vorlagen überprüfen.

## {{site.data.keyword.messagehub}} Events-Vorlage bereitstellen
{: #messagehub-events-template}

Die {{site.data.keyword.messagehub}} Events-Vorlage erstellt eine Aktion und einen Auslöser, der diese Aktion startet. Der Auslöser wird immer dann aktiviert, wenn ein neues Element zu dem Thema '{{site.data.keyword.messagehub}}' hinzugefügt wird, das bei der Vorlagenerstellung ausgewählt wurde.

1. Wechseln Sie zum Erstellen einer Vorlage zu [{{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) und klicken Sie dann auf **Erstellung starten**.

2. Klicken Sie auf **Quickstart-Vorlagen**.

3. Überprüfen Sie das Feld **Paketname** und aktualisieren Sie es bei Bedarf. Der Standardwert wird für Sie auf `message-hub-events` festgelegt.

4. Wählen Sie die Laufzeit für die Aktionen aus, deren Eigner Sie sein werden: Node.js 8, Node.js 6, Python 3, Swift 4 oder PHP 7.1.

5. Klicken Sie auf **Weiter**.

### {{site.data.keyword.messagehub}}-Auslöser erstellen

Auslöser rufen Aktionen auf, wenn sie Ereignisse aus Ereignisquellen empfangen. Geben Sie zum Erstellen eines Auslösers für die {{site.data.keyword.messagehub}}-Vorlage den Auslöser mit den erforderlichen Informationen zur {{site.data.keyword.messagehub}}-Serviceinstanz an.

Überprüfen Sie das Feld **Auslösername** und aktualisieren Sie es bei Bedarf. Der Standardwert wird für Sie auf `message-hub-events-trgr` festgelegt.

### {{site.data.keyword.messagehub}}-Serviceinstanz erstellen

Sie haben die folgenden Auswahlmöglichkeiten:
  * **Eigene Instanz erstellen**
  * **Eigene Berechtigungsnachweise eingeben**

1. Wählen Sie zum Beispiel die Option **Eigene Instanz erstellen** aus.

2. Über ein Popup-Fenster gelangen Sie zu einer neuen Registerkarte mit der {{site.data.keyword.messagehub}}-Konfigurationsseite. Nachdem Sie die {{site.data.keyword.messagehub}}-Instanz erstellt haben, müssen Sie eine Gruppe von Serviceberechtigungsnachweisen erstellen und anschließend zu dieser Seite zurückkehren, indem Sie die Registerkarte durch Klicken auf **OK** schließen.

3. Wählen Sie jetzt **Eigene Berechtigungsnachweise eingeben** aus und geben Sie die folgenden Informationen an:
  * Benutzername - _Ihr {{site.data.keyword.messagehub}}-Benutzername_
  * Kennwort - _Ihr {{site.data.keyword.messagehub}}-Kennwort_
  * kafka_admin_url - _Ihre REST-URL für den {{site.data.keyword.messagehub}}-Administrator_
  * Datenbank - _Der Name Ihrer {{site.data.keyword.messagehub}}-Datenbank_
  * Thema - _Zu abonnierendes Thema_

### {{site.data.keyword.messagehub}}-Vorlage bereitstellen

Klicken Sie auf **Bereitstellen**.

Nach der Vorlagenbereitstellung können Sie den Code durch weiteres Bearbeiten nach Bedarf anpassen oder zurückgehen und den Katalog mit den verfügbaren Vorlagen überprüfen.

## Periodic Slack Reminder-Vorlage bereitstellen
{: #slack-reminder-template}

Die Vorlage 'Periodic Slack Reminder' sendet Beiträge an Slack auf der Basis eines Intervalls, das der Benutzer bei der Erstellung des Auslösers angibt. Bevor Sie diese Vorlage erstellen, rufen Sie die Website https://api.slack.com/incoming-webhooks auf, um die erforderliche URL für eingehende Webhooks einzurichten.

1. Überprüfen Sie das Feld **Paketname** und aktualisieren Sie es bei Bedarf. Der Standardwert wird für Sie auf `periodic-slack-reminder` festgelegt.

2. Wählen Sie die Laufzeit für die Aktionen aus, deren Eigner Sie sein werden: Node.js 8, Node.js 6, Python 3, Swift 4 oder PHP 7.1.

3. Geben Sie im Abschnitt **Parameter** die Webhook-URL in das Feld **Parameterwert** ein und klicken Sie anschließend auf **Weiter**. (Beispiel: https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX).

### Slack Reminder-Auslöser erstellen

Auslöser rufen Aktionen auf, wenn sie Ereignisse aus Ereignisquellen empfangen. Geben Sie zum Erstellen eines Auslösers für die Slack Reminder-Vorlage den Auslöser mit den erforderlichen Informationen zur {{site.data.keyword.messagehub}}-Serviceinstanz an.

1. Überprüfen Sie das Feld **Auslösername** und aktualisieren Sie es bei Bedarf. Der Standardwert wird für Sie auf `periodic-slack-reminder-trgr` festgelegt.

2. Als Nächstes können Sie das Intervall angeben, in dem der Auslöser aktiviert werden soll. Hierzu können Sie ein Muster oder einen Cron-Ausdruck verwenden. Sie können UTC-Zeiten für Wochentage, Stunden und Minuten auswählen. Nachdem Sie die gewünschten Intervalloptionen ausgewählt haben, kann die Bereitstellung der Vorlage erfolgen.

3. Klicken Sie auf **Bereitstellen**.

Nach der Vorlagenbereitstellung können Sie den Code durch weiteres Bearbeiten nach Bedarf anpassen oder zurückgehen und den Katalog mit den verfügbaren Vorlagen überprüfen.
