---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: deploy, deployment templates, templates, example, quickstart, functions, serverless

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


# Quickstart-Vorlagen bereitstellen
{: #templates}

{{site.data.keyword.openwhisk}} stellt einen Katalog von Vorlagen bereit, um Ihren Einstieg in das nächste Projekt zu beschleunigen. Vorlagen sind eine Kombination aus Aktionen, Auslösern und Sequenzen. Manche Vorlagen können auch andere Serviceinstanzen aus {{site.data.keyword.cloud_notm}} beinhalten. Mithilfe von Vorlagen können Sie rasch und ohne großen Aufwand ein Projekt erstellen und unverzüglich mit der Programmierung beginnen.
{: shortdesc}


## Verfügbare Quickstart-Vorlagen
{: #available-templates}

| Name | Beschreibung | Unterstützte Laufzeiten | Quellenrepository |
|:-----------------|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} Events](#cloudant-template) | Wenn ein Dokument in einer {{site.data.keyword.cloudantfull}}-Datenbank bearbeitet oder hinzugefügt wird, protokollieren Sie die Änderung in der Konsole. | Node.js, Swift, Python, PHP | [Code](https://github.com/ibm-functions/template-cloudant-trigger){: external} |
| [Upload Image](#image-template) | Verwenden Sie eine Webaktion zum Hochladen eines Bilds in ein Bucket der {{site.data.keyword.cos_full}}-Instanz und zum anschließenden Abrufen eines Piktogramms dieses Bilds. | Node.js | [Code](https://github.com/ibm-functions/template-cloud-object-storage){: external}
| [Get HTTP Resource](#get-http-resource-template) | Rufen Sie eine Webaktion mithilfe eines HTTP-Ereignisses aus und rufen Sie Daten aus der Yahoo Weather-API ab. | Node.js, Python | [Code](https://github.com/ibm-functions/template-get-external-resource){: external}
| [Hello World](#hello-world-template) | Erstellen Sie eine Basisaktion, die ein JSON-Objekt als einzelnen Parameter akzeptiert. | Node.js, Swift, Python, PHP | [Code](https://github.com/ibm-functions/template-hello-world){: external}
| [{{site.data.keyword.messagehub}} Events](#messagehub-events-template) | Wenn neue Daten in einem {{site.data.keyword.messagehub_full}}-Thema hinzugefügt werden, protokollieren Sie die Änderung in der Konsole. | Node.js, Swift, Python, PHP | [Code](https://github.com/ibm-functions/template-messagehub-trigger){: external}
| [Periodic Slack Reminder](#slack-reminder-template) | Verwenden Sie einen Webhook zu Slack aud Basis eines regelmäßigen Auslösers. | Node.js, Swift, Python, PHP | [Code](https://github.com/ibm-functions/template-reminder-slack){: external}

Sie können auf den Code für jede Vorlage in ihrem Code-Repository zugreifen, ihn nach Bedarf ändern und mit eigener Funktionalität ausstatten.
{: tip}


## {{site.data.keyword.cloudant_short_notm}} Events-Vorlage bereitstellen
{: #cloudant-template}

Die {{site.data.keyword.cloudant_short_notm}}-Vorlage erstellt eine Sequenz von Aktionen und einen Auslöser, der diese Sequenz aktiviert. Der Auslöser wird aktiviert, wenn in der verbundenen {{site.data.keyword.cloudant_short_notm}}-Beispieldatenbank eine Änderung vorgenommen wird. Das erwartete Datenelement ist eine Katze mit einem definierten Namen und einer definierten Farbe. Wenn eine neue Katze zu der Datenbank hinzugefügt oder eine dort vorhandene Katze bearbeitet wird, werden die Daten in der Konsole protokolliert.

### {{site.data.keyword.cloudant_short_notm}} Events-Vorlage über die Benutzerschnittstelle bereitstellen
{: #cloudant-template-ui}

1. Rufen Sie die [Seite 'Erstellen'](https://cloud.ibm.com/openwhisk/create){: external} in der {{site.data.keyword.openwhisk_short}}-Konsole auf. 

2. Klicken Sie auf **Quickstart-Vorlagen**.

3. Klicken Sie auf **Cloudant Events**.

4. Erstellen Sie die {{site.data.keyword.cloudant_short_notm}}-Aktion.
    1. Geben Sie einen Namen für Ihr Paket an oder verwenden Sie den bereitgestellten Standardnamen `new-cloudant-item`.
    2. Wählen Sie in der Liste **Aktionen** den Eintrag `Node.js 6` für die Beispielkatzendatenbank aus und klicken Sie auf **Weiter**.

5. Erstellen Sie den {{site.data.keyword.cloudant_short_notm}}-Auslöser. Auslöser rufen Aktionen auf, wenn sie Ereignisse aus Ereignisquellen empfangen. Um den Auslöser zu erstellen, müssen Sie eine {{site.data.keyword.messagehub}}-Serviceinstanz und Serviceberechtigungsnachweise bereitstellen.
    1. Wählen Sie in der Liste **Cloudant-Instanz** den Eintrag **Eigene Instanz erstellen** aus. Die {{site.data.keyword.cloudant_short_notm}}-Konfigurationsseite wird geöffnet.
    2. Erstellen Sie die {{site.data.keyword.cloudant_short_notm}}-Serviceinstanz.
    3. Erstellen Sie einen Satz von Serviceberechtigungsnachweisen.
    4. Klicken Sie auf **OK**, um die {{site.data.keyword.cloudant_short_notm}}-Konfigurationsseite zu schließen und zur {{site.data.keyword.openwhisk_short}}-Konsole zurückzukehren.
    5. Jetzt können Sie in der Liste **Cloudant-Instanz** den Eintrag **Eigene Berechtigungsnachweise eingeben** auswählen und die folgenden Informationen bereitstellen. 
        
        * `Benutzername` - Ihr {{site.data.keyword.cloudant_short_notm}}-Benutzername. 
        * `Kennwort` - Ihr {{site.data.keyword.cloudant_short_notm}}-Kennwort. 
        * `Host` - `<username>.cloudant.com`.  
        * `Datenbank` - Der Name Ihrer {{site.data.keyword.cloudant_short_notm}}-Datenbank. 

6. Klicken Sie auf **Bereitstellen**. Nach der Vorlagenbereitstellung können Sie den Code durch weiteres Bearbeiten nach Bedarf anpassen oder zurückgehen und den Katalog mit den verfügbaren Vorlagen überprüfen.

### {{site.data.keyword.cloudant_short_notm}} Events-Vorlage über die Befehlszeilenschnittstelle bereitstellen
{: #cloudant-template-cli}

1. Klonen Sie das Repository für die Vorlage.
    ```
    git clone https://github.com/ibm-functions/template-cloudant-trigger.git
    ```
    {: pre}

2. Navigieren Sie zu dem Verzeichnis für die Aktionslaufzeit, die Sie verwenden möchten: `nodejs-6`, `nodejs`, `php`, `python` oder `swift`.
    ```
    cd template-cloudant-trigger/runtimes/nodejs
    ```
    {: pre}

3. Stellen Sie unter Verwendung der folgenden Umgebungsvariablen die Vorlage bereit.
    ```
    CLOUDANT_HOSTNAME=<host> CLOUDANT_USERNAME=<benutzername> CLOUDANT_PASSWORD=<kennwort> CLOUDANT_DATABASE=<datenbank> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>Informationen zu den Umgebungsvariablen</caption>
    <thead>
    <th colspan=2>Informationen zu den Umgebungsvariablen</th>
    </thead>
    <tbody>
    <tr><td><code>CLOUDANT_HOSTNAME</code></td><td><code>&lt;benutzername&gt;.cloudant.com</code></td></tr>
    <tr><td><code>CLOUDANT_USERNAME</code></td><td>Ihr {{site.data.keyword.cloudant_short_notm}}-Benutzername</td></tr>
    <tr><td><code>CLOUDANT_PASSWORD</code></td><td>Ihr {{site.data.keyword.cloudant_short_notm}}-Kennwort</td></tr>
    <tr><td><code>CLOUDANT_DATABASE</code></td><td>Der Name Ihrer {{site.data.keyword.cloudant_short_notm}}-Datenbank</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>Ein angepasster Name für das Paket</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>Ein angepasster Name für die Regel</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>Ein angepasster Name für den Auslöser</td></tr>
    </tbody></table>

## Upload Image-Vorlage bereitstellen
{: #image-template}

Die Upload Image-Vorlage erstellt eine Webaktion, mit der Sie über eine kleine Schnittstelle ein Bild in ein {{site.data.keyword.cos_full_notm}}-Bucket hochladen können. Die Vorlage ruft dann das Bild als Piktogramm ab und zeigt es auf der Schnittstelle der Webaktion an.


### Upload Image-Vorlage über die UI bereitstellen
{: #image-template-ui}

1. Rufen Sie die [Seite 'Erstellen'](https://cloud.ibm.com/openwhisk/create){: external} in der {{site.data.keyword.openwhisk_short}}-Konsole auf. 

2. Klicken Sie auf **Quickstart-Vorlagen**.

3. Klicken Sie auf **Upload Image**.

4. Geben Sie einen Namen für das Paket ein oder verwenden Sie den Standardnamen `upload-image` und klicken Sie auf **Weiter**.

6. Für die Vorlage sind Serviceberechtigungsnachweise einer {{site.data.keyword.cos_full_notm}}-Serviceinstanz erforderlich. Wählen Sie in der **{{site.data.keyword.cos_full_notm}}**-Liste eine der folgenden Optionen aus:
  * **Instanz erstellen**. Wenn keine Serviceinstanz vorhanden ist, wählen Sie diese Option aus, um eine Serviceinstanz zu erstellen. 
      1. Erstellen Sie auf der Seite zum Erstellen einer {{site.data.keyword.cos_full_notm}}-Serviceinstanz, die geöffnet wird, eine Serviceinstanz.
      2. [Erstellen Sie eine Gruppe von HMAC-Serviceberechtigungsnachweisen](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials).
      3. [Erstellen Sie mindestens ein Bucket](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets).
  * **Eigene Berechtigungsnachweise eingeben**. Wählen Sie diese Option aus, um Ihre eigenen Berechtigungsnachweise für eine {{site.data.keyword.cos_full_notm}}-Serviceinstanz manuell einzugeben. Die Berechtigungsnachweise müssen über HMAC-Schlüssel verfügen und die Serviceinstanz muss mindestens ein Bucket besitzen.
  * **Vorhandene Instanzen**. Wenn {{site.data.keyword.cos_full_notm}}-Instanzen vorhanden sind, wählen Sie eine der Instanzen in der Liste aus. Die Berechtigungsnachweise müssen über HMAC-Schlüssel verfügen und die Serviceinstanz muss mindestens ein Bucket besitzen.

7. Klicken Sie auf **Bereitstellen**.

8. Klicken Sie im Navigationsbereich links auf den Abschnitt **Endpunkte**.

9. Kopieren Sie im Abschnitt **Webaktion** den Link ohne das Suffix '.json' und fügen Sie ihn in die Adressleiste des Browsers ein. Die Schnittstelle für die Webaktion der Vorlage wird angezeigt.

10. Optional: Nach der Bereitstellung der Vorlage können Sie zum Dashboard **Aktionen** navigieren und dort den Code in den folgenden zwei neuen Paketen anpassen:
    * Paket `cloud-object-storage`, das Aktionen enthält, die mit {{site.data.keyword.cos_full_notm}}-Instanzen arbeiten
    * Vorlagenpaket (Standardname `upload-image`), das die Aktion `app` enthält

### Upload Image-Vorlage über die CLI bereitstellen
{: #image-template-cli}

1. [Installieren Sie das {{site.data.keyword.cos_full_notm}}-Paket](/docs/openwhisk?topic=cloud-functions-pkg_obstorage).

2. Klonen Sie das Repository für die Vorlage.
    ```
    git clone https://github.com/ibm-functions/template-cloud-object-storage.git
    ```
    {: pre}

3. Navigieren Sie zum Laufzeitverzeichnis `nodejs`.
    ```
    cd template-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Stellen Sie die Vorlage bereit. Geben Sie dabei einen angepassten Paketnamen und den Namen Ihres {{site.data.keyword.cos_full_notm}}-Buckets als Umgebungsvariablen an.
    ```
    PACKAGE_NAME=<name> BUCKET_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Klicken Sie auf der [Seite 'Aktionen'](https://cloud.ibm.com/openwhisk/actions){: external} der {{site.data.keyword.openwhisk_short}}-Konsole auf die Aktion `process-change`. 

5. Klicken Sie in der linken Navigation auf **Endpunkte**.

6. Kopieren Sie im Abschnitt **Webaktion** den Link ohne das Suffix '.json' und fügen Sie ihn in die Adressleiste des Browsers ein. Die Schnittstelle für die Webaktion der Vorlage wird angezeigt.

## Get HTTP Resource-Vorlage bereitstellen
{: #get-http-resource-template}

Die Get HTTP Resource-Vorlage erstellt eine Aktion, um eine externe Ressource (die Yahoo Weather-API) abzurufen, und gibt dann Daten zurück. Die Aktion ist als Webaktion aktiviert, sodass sie mit einer CORS-fähigen URL aufgerufen werden kann und keinen Authentifizierungsschlüssel benötigt, was für die Erstellung von Back-Ends für Webanwendungen nützlich ist. 

Standardmäßig ist der Endpunkt `get-http-resource` öffentlich für alle Benutzer verfügbar, die ihn aufrufen.
{: note}

### Get HTTP Resource-Vorlage über die Benutzerschnittstelle bereitstellen
{: #get-http-resource-template-ui}

1. Rufen Sie die [Seite 'Erstellen'](https://cloud.ibm.com/openwhisk/create){: external} in der {{site.data.keyword.openwhisk_short}}-Konsole auf. 

2. Klicken Sie auf **Quickstart-Vorlagen**.

3. Klicken Sie auf **Get HTTP Resource**.

3. Geben Sie einen Namen für Ihr Paket ein oder übernehmen Sie den bereitgestellten Standardnamen `get-http-resource`.

4. Wählen Sie die Laufzeit für Ihre Aktionen aus: Node.js 8, Node.js 6 oder Python 3.

5. Klicken Sie auf **Bereitstellen**.

6. Rufen Sie die Aktion auf, indem Sie die folgende URL abwickeln: `https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`. Beispiel:
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

Nach der Vorlagenbereitstellung können Sie den Code durch weiteres Bearbeiten nach Bedarf anpassen oder zurückgehen und den Katalog mit den verfügbaren Vorlagen überprüfen.

### Get HTTP Resource-Vorlage über die Befehlszeilenschnittstelle bereitstellen
{: #get-http-resource-template-cli}

1. Klonen Sie das Repository für die Vorlage.
    ```
    git clone https://github.com/ibm-functions/template-get-external-resource.git
    ```
    {: pre}

2. Navigieren Sie zu dem Verzeichnis für die Aktionslaufzeit, die Sie verwenden möchten: `nodejs-6`, `nodejs` oder `python`.
    ```
    cd template-get-external-resource/runtimes/nodejs
    ```
    {: pre}

3. Stellen Sie unter Angabe eines angepassten Paketnamens als Umgebungsvariable die Vorlage bereit.
    ```
    PACKAGE_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Rufen Sie die Aktion auf, indem Sie die folgende URL abwickeln: `https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`. Beispiel:
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

## Hello World-Vorlage bereitstellen
{: #hello-world-template}

Sie können diese grundlegende Hello World-Aktion bereitstellen, um Ihre ersten Schritte mit {{site.data.keyword.openwhisk_short}} zu machen oder um andere Entitäten zu testen, die Sie erstellen, z. B. Auslöser und Regeln.

### Hello World-Vorlage über die Benutzerschnittstelle bereitstellen
{: #hello-world-template-ui}

1. Rufen Sie die [Seite 'Erstellen'](https://cloud.ibm.com/openwhisk/create){: external} in der {{site.data.keyword.openwhisk_short}}-Konsole auf. 

2. Klicken Sie auf **Quickstart-Vorlagen**.

3. Klicken Sie auf **Hello World**.

4. Geben Sie einen Namen für Ihr Paket ein oder übernehmen Sie den bereitgestellten Standardnamen `hello-world`.

5. Wählen Sie die Laufzeit für Ihre Aktionen aus: Node.js 8, Node.js 6, Python 3, Swift 4 oder PHP 7.1.

6. Klicken Sie auf **Bereitstellen**.

Nach der Vorlagenbereitstellung können Sie den Code durch weiteres Bearbeiten nach Bedarf anpassen oder zurückgehen und den Katalog mit den verfügbaren Vorlagen überprüfen.

### Hello World-Vorlage über die Befehlszeilenschnittstelle bereitstellen
{: #hello-world-template-cli}

1. Klonen Sie das Repository für die Hello World-Vorlage.
    ```
    git clone https://github.com/ibm-functions/template-hello-world.git
    ```
    {: pre}

2. Navigieren Sie zu dem Verzeichnis für die Aktionslaufzeit, die Sie verwenden möchten: `nodejs-6`, `nodejs`, `php`, `python` oder `swift`.
    ```
    cd template-hello-world/runtimes/nodejs
    ```
    {: pre}

3. Stellen Sie die Vorlage bereit.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

## {{site.data.keyword.messagehub}} Events-Vorlage bereitstellen
{: #messagehub-events-template}

Die {{site.data.keyword.messagehub}} Events-Vorlage erstellt eine Aktion und einen Auslöser, der diese Aktion startet. Der Auslöser wird immer dann aktiviert, wenn ein neues Element zu dem {{site.data.keyword.messagehub}}-Thema hinzugefügt wird, das bei der Vorlagenerstellung ausgewählt wurde. 

### {{site.data.keyword.messagehub}} Events-Vorlage über die Benutzerschnittstelle bereitstellen
{: #messagehub-events-template-ui}

1. Rufen Sie die [Seite 'Erstellen'](https://cloud.ibm.com/openwhisk/create){: external} in der {{site.data.keyword.openwhisk_short}}-Konsole auf. 

2. Klicken Sie auf **Quickstart-Vorlagen**.

3. Klicken Sie auf **{{site.data.keyword.messagehub}} Events**.

4. Erstellen Sie die {{site.data.keyword.messagehub}}-Aktion.
    1. Geben Sie einen Namen für Ihr Paket an oder verwenden Sie den bereitgestellten Standardnamen `message-hub-events`.
    2. Wählen Sie die Laufzeit für Ihre Aktionen aus: Node.js 8, Node.js 6, Python 3, Swift 4 oder PHP 7.1.
    3. Klicken Sie auf **Weiter**.

5. Erstellen Sie den {{site.data.keyword.messagehub}}-Auslöser. Auslöser rufen Aktionen auf, wenn sie Ereignisse aus Ereignisquellen empfangen. Um den Auslöser zu erstellen, müssen Sie eine {{site.data.keyword.messagehub}}-Serviceinstanz und Serviceberechtigungsnachweise bereitstellen. 
    1. Wählen Sie in der Liste **MessageHub-Instanz** den Eintrag **Eigene Instanz erstellen** aus. Die {{site.data.keyword.messagehub}}-Erstellungsseite wird geöffnet.
    2. Erstellen Sie die {{site.data.keyword.messagehub}}-Serviceinstanz.
    3. Erstellen Sie einen Satz von Serviceberechtigungsnachweisen.
    4. Klicken Sie auf **OK**, um die {{site.data.keyword.messagehub}}-Erstellungsseite zu schließen und zur {{site.data.keyword.messagehub}}-Konsole zurückzukehren.
    5. Jetzt können Sie in der Liste **MessageHub-Instanz** den Eintrag **Eigene Berechtigungsnachweise eingeben** auswählen und die folgenden Informationen bereitstellen:
        * `Benutzername` - Ihr {{site.data.keyword.messagehub}}-Benutzername. 
        * `Kennwort` - Ihr {{site.data.keyword.messagehub}}-Kennwort. 
        * `kafka_admin_url` - Ihre {{site.data.keyword.messagehub}}-Administrator-REST-URL. 
        * `Datenbank` - Der Name Ihrer {{site.data.keyword.messagehub}}-Datenbank. 
        * `Thema` - Das Thema, das abonniert werden soll. 

6. Klicken Sie auf **Bereitstellen**.

Nach der Vorlagenbereitstellung können Sie den Code durch weiteres Bearbeiten nach Bedarf anpassen oder zurückgehen und den Katalog mit den verfügbaren Vorlagen überprüfen.

### {{site.data.keyword.messagehub}} Events-Vorlage über die Befehlszeilenschnittstelle bereitstellen
{: #messagehub-events-template-cli}

1. Klonen Sie das Repository für die Vorlage.
    ```
    git clone https://github.com/ibm-functions/template-messagehub-trigger.git
    ```
    {: pre}

2. Navigieren Sie zu dem Verzeichnis für die Aktionslaufzeit, die Sie verwenden möchten: `nodejs-6`, `nodejs`, `php`, `python` oder `swift`.
    ```
    cd template-messagehub-trigger/runtimes/nodejs
    ```
    {: pre}

3. Stellen Sie die Vorlage bereit. Verwenden Sie dabei die folgenden Umgebungsvariablen.
    ```
    KAFKA_BROKERS=<host> KAFKA_TOPIC=<thema> MESSAGEHUB_USER=<benutzername> MESSAGEHUB_PASS=<kennwort> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>Informationen zu den Umgebungsvariablen</caption>
    <thead>
    <th colspan=2>Informationen zu den Umgebungsvariablen</th>
    </thead>
    <tbody>
    <tr><td><code>KAFKA_BROKERS</code></td><td>Ihre {{site.data.keyword.messagehub}}-Administrator-REST-URL</td></tr>
    <tr><td><code>KAFKA_TOPIC</code></td><td>Das Thema, das abonniert werden soll</td></tr>
    <tr><td><code>MESSAGEHUB_USER</code></td><td>Ihr {{site.data.keyword.messagehub}}-Benutzername</td></tr>
    <tr><td><code>MESSAGEHUB_PASS</code></td><td>Ihr {{site.data.keyword.messagehub}}-Kennwort</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>Ein angepasster Name für das Paket</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>Ein angepasster Name für die Regel</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>Ein angepasster Name für den Auslöser</td></tr>
    </tbody></table>

## Periodic Slack Reminder-Vorlage bereitstellen
{: #slack-reminder-template}

Die Vorlage 'Periodic Slack Reminder' sendet Beiträge an Slack auf der Basis eines Intervalls, das der Benutzer bei der Erstellung des Auslösers angibt.

### Periodic Slack Reminder-Vorlage über die Benutzerschnittstelle bereitstellen
{: #slack-reminder-template-ui}

1. Rufen Sie 'https://api.slack.com/incoming-webhooks' auf, um die erforderliche URL für eingehende Webhooks einzurichten.

1. Rufen Sie die [Seite 'Erstellen'](https://cloud.ibm.com/openwhisk/create){: external} in der {{site.data.keyword.openwhisk_short}}-Konsole auf. 

2. Klicken Sie auf **Quickstart-Vorlagen**.

3. Klicken Sie auf **Periodic Slack Reminder Events**.

4. Erstellen Sie die Aktion 'Periodic Slack Reminder'.
    1. Geben Sie einen Namen für Ihr Paket an oder verwenden Sie den bereitgestellten Standardnamen `periodic-slack-reminder`.
    2. Wählen Sie die Laufzeit für Ihre Aktionen aus: Node.js 8, Node.js 6, Python 3, Swift 4 oder PHP 7.1.
    3. Geben Sie im Abschnitt 'Parameter' die Webhook-URL im Feld **Parameterwert** ein, z. B. `https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX`.
    4. Klicken Sie auf **Weiter**.

5. Erstellen Sie den Periodic Slack Reminder-Auslöser. Auslöser rufen Aktionen auf, wenn sie Ereignisse aus Ereignisquellen empfangen.
    1. Geben Sie das Zeitintervall des Auslösers an, indem Sie ein Muster oder einen `Cron`-Ausdruck verwenden. 
        * `Muster` - Wählen Sie UTC-Zeiten für Wochentage, Stunden und Minuten aus. 
        * `Cron` - Geben Sie eine cron-Sequenz basierend auf der <a href="http://crontab.org">UNIX-crontab-Syntax</a> an. Verwenden Sie höchstens fünf Felder, getrennt durch Leerzeichen, mit dem Format `X X X X X`.
    2. Fügen Sie die JSON-Nutzdaten des Auslösers hinzu.

6. Klicken Sie auf **Bereitstellen**.

Nach der Vorlagenbereitstellung können Sie den Code durch weiteres Bearbeiten nach Bedarf anpassen oder zurückgehen und den Katalog mit den verfügbaren Vorlagen überprüfen.

### Periodic Slack Reminder-Vorlage über die Befehlszeilenschnittstelle bereitstellen
{: #slack-reminder-template-cli}

1. Rufen Sie 'https://api.slack.com/incoming-webhooks' auf, um die erforderliche URL für eingehende Webhooks einzurichten.

1. Klonen Sie das Repository für die Vorlage.
    ```
    git clone https://github.com/ibm-functions/template-reminder-slack.git
    ```
    {: pre}

2. Navigieren Sie zu dem Verzeichnis für die Aktionslaufzeit, die Sie verwenden möchten: `nodejs-6`, `nodejs`, `php`, `python` oder `swift`.
    ```
    cd template-reminder-slack/runtimes/nodejs
    ```
    {: pre}

3. Stellen Sie unter Verwendung der folgenden Umgebungsvariablen die Vorlage bereit.
    ```
    SLACK_WEBHOOK_URL=<url> ALARM_CRON=<cron> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>Informationen zu den Umgebungsvariablen</caption>
    <thead>
    <th colspan=2>Informationen zu den Umgebungsvariablen</th>
    </thead>
    <tbody>
    <tr><td><code>SLACK_WEBHOOK_URL</code></td><td>Die Webhook-URL, z. B. <code>https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX</code>.</td></tr>
    <tr><td><code>ALARM_CRON</code></td><td>Eine cron-Sequenz basierend auf der <a href="http://crontab.org">UNIX-crontab-Syntax</a>. Verwenden Sie höchstens 5 Felder mit dem Format <code>X X X X X</code>. </td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>Ein angepasster Name für das Paket</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>Ein angepasster Name für die Regel</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>Ein angepasster Name für den Auslöser</td></tr>
    </tbody></table>


