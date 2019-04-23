---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: functions cli, serverless, bluemix cli, install, functions plug-in

subcollection: cloud-functions

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.openwhisk_short}}-Befehlszeilenschnittstellen-Plug-in einrichten
{: #cloudfunctions_cli}


{{site.data.keyword.openwhisk}} stellt ein leistungsfähiges Plug-in für die {{site.data.keyword.Bluemix_notm}}-CLI (Befehlszeilenschnittstelle) bereit, die eine vollständige Verwaltung des {{site.data.keyword.openwhisk_short}}-Systems ermöglicht. Sie können das Plug-in der {{site.data.keyword.openwhisk_short}}-Befehlszeilenschnittstelle verwenden, um Ihre Code-Snippets in Aktion zu verwalten, um Auslöser und Regeln zu erstellen, damit Ihre Aktionen auf Ereignisse reagieren können, und um Aktionen in Paketen zu bündeln.
{:shortdesc}

Sie können jetzt den Aliasnamen `fn` wie folgt in Ihren {{site.data.keyword.openwhisk_short}}-Plug-in-Befehlen verwenden: `ibmcloud fn <command>`
{: tip}

## {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle einrichten
{: #bluemix_cli_setup}

Sie können die {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle herunterladen, installieren und sich dann anmelden.
{: shortdesc}

1. Laden Sie die [{{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli) herunter und installieren Sie sie.

2. Melden Sie sich an der {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle an. Um eine IBM Cloud-Region anzugeben, [schließen Sie den API-Endpunkt ein](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions).

  ```
  ibmcloud login
  ```
  {: pre}

  Sie können die Organisation und den Bereich angeben, wenn Sie sich anmelden, um die entsprechenden Eingabeaufforderungen zu überspringen. Verwenden Sie dazu die folgenden Flags: `ibmcloud login -o <ORG> -s <SPACE>`
  {: tip}

3. Wenn Sie keine Organisation und keinen Bereich angeben, vervollständigen Sie die Eingabeaufforderungen, die auf den Anmeldebefehl folgen. 


## {{site.data.keyword.openwhisk_short}}-Plug-in einrichten
{: #cloudfunctions_plugin_setup}

Laden Sie das Befehlszeilenschnittstellen-Plug-in herunter und installieren Sie es, um mit {{site.data.keyword.openwhisk_short}} zu arbeiten.
{: shortdesc}

Sie können das Plug-in für folgende Aufgaben einsetzen: 

* Sie können Code-Snippets oder Aktionen in {{site.data.keyword.openwhisk_short}} ausführen. Siehe [Aktionen erstellen und aufrufen](/docs/openwhisk?topic=cloud-functions-openwhisk_actions).
* Sie können Ihre Aktionen durch Auslöser und Regeln so einrichten, dass sie auf Ereignisse reagieren. Siehe [Auslöser und Regeln erstellen](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers).
* Sie können sich informieren, wie Aktionen in Paketen gebündelt und externe Ereignisquellen konfiguriert werden. Siehe [Pakete verwenden und erstellen](/docs/openwhisk?topic=cloud-functions-openwhisk_packages).
* Sie können den Katalog der Pakete durchsuchen und Ihre Anwendungen durch externe Services wie zum Beispiel eine [{{site.data.keyword.cloudant}}-Ereignisquelle](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant) erweitern.

Um anzuzeigen, was Sie alles mit dem {{site.data.keyword.openwhisk_short}}-Plug-in tun können, führen Sie `ibmcloud fn` ohne Argumente aus.
{: tip}

1. Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in.

  ```
  ibmcloud plugin install cloud-functions
  ```
  {: pre}

2. Überprüfen Sie, ob das Plug-in installiert ist.

  ```
  ibmcloud plugin list cloud-functions
  ```
  {: pre}

  Ausgabe:
  ```
  Plugin Name          Version
    Cloud-Functions      1.0.16
  ```
  {: screen}

Sie verfügen bereits über das Plug-in, müssen es aber aktualisieren? Führen Sie `ibmcloud plugin update cloud-functions` aus.
{:tip}



## Services über Aktionen verwenden
{: #binding_services_cli}

{{site.data.keyword.openwhisk_short}} stellt den Befehl `service bind` bereit, um Ihre Berechtigungsnachweise für den {{site.data.keyword.Bluemix_notm}}-Service für Ihren Code zur Laufzeit verfügbar zu machen. Anschließend können Sie den Befehl `service bind` verwenden, um einen beliebigen {{site.data.keyword.Bluemix_notm}}-Service an eine beliebige Aktion zu binden, die in {{site.data.keyword.openwhisk_short}} definiert ist.

Die einzelnen Schritte zur Verwendung von Services über Aktionen finden Sie im Abschnitt [Services an Aktionen binden](/docs/openwhisk?topic=cloud-functions-binding_services).


## {{site.data.keyword.openwhisk_short}}-CLI zur Verwendung eines HTTPS-Proxys konfigurieren
{: #cli_https_proxy}

Die {{site.data.keyword.openwhisk_short}}-CLI kann zur Verwendung eines HTTPS-Proxys eingerichtet werden. Zur Einrichtung eines HTTPS-Proxys muss die Umgebungsvariable `HTTPS_PROXY` erstellt werden. In der Variablen müssen die Adresse des HTTPS-Proxys und der zugehörige Port im folgenden Format angegeben werden: `{PROXY IP}:{PROXY PORT}`.



## Regionen, Organisationen und Bereichen wechseln
{: #region_info}

Wenn Sie bereits angemeldet sind, können Sie den Befehl `ibmcloud target` in der {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle (CLI) ausführen, um Regionen, Organisationen und Bereiche zu wechseln.


Um Entitäten zu erstellen und zu verwalten, müssen Sie einen Namensbereich als Ziel verwenden. Der Standardnamensbereich, der in manchen Fällen durch einen Unterstrich (`_`) gekennzeichnet sein kann, entspricht dem Cloud Foundry-basierten Namensbereich, der aktuell das Ziel ist. 

Zur Verwaltung Ihrer Bereitstellungen in der Vorproduktionsumgebung (Staging) und in der Produktionsumgebung können Sie Bereiche für beide Zwecke erstellen. Durch diese Bereiche kann {{site.data.keyword.openwhisk_short}} zwei verschiedene für Sie definierte Namensbereiche haben. Führen Sie den Befehl [`ibmcloud iam space-create`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_account#ibmcloud_account_space_create) aus, um weitere Bereiche unter Ihrer Organisation wie "staging" und "production" zu erstellen:

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}} unterliegt Einschränkungen für Namen von Namensbereichen. Weitere Informationen finden Sie in der Dokumentation zu [Systemdetails und Grenzwerte](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_entities).
{: tip}

**Warnung**: Wenn Sie den Namen der Organisation oder des Bereichs ändern, wird ein neuer Namensbereich auf der Basis des geänderten Namens erstellt. Die Entitäten in dem alten Namensbereich sind im neuen Namensbereich nicht sichtbar und werden möglicherweise gelöscht. 


## Von der OpenWhisk-CLI auf das {{site.data.keyword.openwhisk_short}}-CLI-Plug-in migrieren
{: #cli_migration}

Sie können jetzt das {{site.data.keyword.openwhisk_short}}-CLI-Plug-in verwenden, um mit {{site.data.keyword.openwhisk_short}}-Entitäten zu interagieren. Obwohl Sie weiterhin die eigenständige OpenWhisk-CLI verwenden können, verfügt sie nicht über dieselben Features, die von {{site.data.keyword.openwhisk_short}} unterstützt werden, z. B. IAM-basierte Namensbereiche und `service bind`.
{: shortdesc}

### Befehlssyntax
{: #command_syntax}

Alle `wsk`-Befehle mit Ausnahme des Befehls `wsk bluemix login`, der nicht mehr erforderlich ist, funktionieren auf die gleiche Weise wie bei Verwendung des Befehls `ibmcloud fn`. Alle Befehlsoptionen und Argumente für Befehle im Cloud Functions-CLI-Plug-in sind identisch mit den Befehlen für die eigenständige OpenWhisk-CLI. Weitere Informationen finden Sie im [Apache OpenWhisk-CLI-Projekt ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/apache/incubator-openwhisk-cli). 

### API-Authentifizierung und API-Host
{: #api_authentication}

Für die OpenWhisk-Befehlszeilenschnittstelle (CLI) müssen Sie den Authentifizierungs-API-Schlüssel und den API-Host konfigurieren. Mit dem {{site.data.keyword.openwhisk_short}}-CLI-Plug-in brauchen Sie den API-Schlüssel und den API-Host nicht explizit zu konfigurieren. Stattdessen können Sie sich mit dem Befehl `ibmcloud login` anmelden und Ihre Region und Ihren Namensbereich mit dem Befehl `ibmcloud target` als Ziel festlegen. Nach dem Anmelden beginnen alle Befehle mit `ibmcloud fn`.


Wenn Sie den Authentifizierungs-API-Schlüssel für {{site.data.keyword.openwhisk_short}} in einem externen HTTP-Client wie cURL oder Postman verwenden müssen, können Sie ihn mit den folgenden Befehlen abrufen:

Führen Sie zum Abrufen des aktuellen API-Schlüssels den folgenden Befehl aus:
```
ibmcloud fn property get --auth
```
{: pre}

Führen Sie zum Abrufen des aktuellen API-Hosts den folgenden Befehl aus:
```
ibmcloud fn property get --apihost
```
{: pre}

Der API-Schlüssel ist jeweils für die Region, die Organisation und den Bereich spezifisch, die für das {{site.data.keyword.openwhisk_short}}-CLI-Plug-in als Ziel festgelegt sind.
{: tip}

### API-Gateway-Authentifizierung
{: #apigw_authentication}

Für die OpenWhisk-Befehlszeile musste der Befehl `wsk bluemix login` ausgeführt werden, sodass die API-Gateway-Berechtigung für das Management Ihrer APIs mit dem Befehl `wsk api` konfiguriert werden konnte. Mit dem {{site.data.keyword.openwhisk_short}}-CLI-Plug-in ist es nicht erforderlich, den Befehl `wsk bluemix login` auszuführen. Wenn Sie stattdessen den Befehl `ibmcloud login` verwenden, um sich bei {{site.data.keyword.Bluemix_notm}} anzumelden, verwendet das {{site.data.keyword.openwhisk}}-Plug-in automatisch die Informationen zu Ihrer aktuellen Anmeldung und Ihrem aktuellen Bereich. Jetzt können Sie Ihre APIs mit dem Befehl `ibmcloud fn api` verwalten.

### Bereitstellungsscripts migrieren
{: #migrating_deploy_scripts}

Wenn Sie über Scripts verfügen, die die OpenWhisk-CLI mit dem Binärprogramm `wsk` verwenden, funktionieren alle Befehle auf die gleiche Weise wie bei Verwendung des Befehls `ibmcloud fn`. Sie können Ihre Scripts so ändern, dass sie das {{site.data.keyword.Bluemix_notm}}-CLI-Plug-in verwenden, oder einen Alias oder Wrapper erstellen, sodass die aktuellen Befehle, die `wsk` verwenden, in `ibmcloud fn` übersetzt werden. Die Befehle `ibmcloud login` und `ibmcloud target` in der {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle arbeiten im unbeaufsichtigten Modus. Beim unbeaufsichtigten Modus können Sie, bevor Sie `ibmcloud fn`-Befehle ausführen, Ihre Umgebung konfigurieren, um Ihre {{site.data.keyword.openwhisk_short}}-Entitäten bereitzustellen und zu verwalten.





## CLI-Versionsprotokoll
{: #version_history}

Hierbei handelt es sich um eine Protokollaufzeichnung der Versionen, die Schwerpunkte und Fehlerkorrekturen aufzeigt.

v1.0.30 (2019-04-03)
* Die `service bind`-Verarbeitung von IAM und organisations- und bereichsbasierten Services wurde verbessert. 
* Ein Korrektur für die Verarbeitung des API-Endpunkts 'https://cloud.ibm.com' wurde hinzugefügt. 

v1.0.29 (2019-02-06)
* Die Befehle `deploy` und `undeploy` wurden hinzugefügt, um eine Sammlung von Functions-Entitäten über eine Manifestdatei bereitzustellen oder ihre Bereitstellung zu beenden. Weitere Informationen finden Sie in der Dokumentation zur [Bereitstellung](/docs/openwhisk?topic=cloud-functions-deploy#deploy). 

v1.0.28 (2019-01-21)
* Es wurde eine Fehlernachricht hinzugefügt, wenn `update|delete|get namespace name` mehrfach auftritt. 

v1.0.27 (2018-12-11)
* Es wurden `namespace get`-Korrekturen hinzugefügt. 
* Es wurde eine Korrektur für `--save-as` hinzugefügt, wenn eine Aktion eine Blackbox-Aktion ist. 
* Es wurde ein Flag `--concurrency` für Befehle zum Erstellen und Aktualisieren von Aktionen hinzugefügt. 

v1.0.26 (2018-11-30)
* Es wurde `fn property get --auth` aktiviert, um den Authentifizierungsschlüssel in einer neuen Umgebung korrekt zurückzugeben. 

v1.0.25 (2018-11-23)
* Verbesserte Anzeige von Fehlernachrichtenergebnissen. 
* Es wurde eine `fn namespace get`-Korrektur für die korrekte Anzeige von Namensbereichseigenschaften hinzugefügt. 

1.0.23 (2018-10-15)
* Es wurde Unterstützung für die Erkennung von ruby-Aktionscode (.rb) hinzugefügt. 

1.0.22 (2018-08-20)
* Es wurde Unterstützung für die Region 'us-east' (USA (Osten)) hinzugefügt. 

1.0.21 (2018-08-01)
* Die Aliasnamen `fn` und `functions` können jetzt für {{site.data.keyword.openwhisk_short}}-Befehle verwendet werden: `ibmcloud fn <command>` und `ibmcloud fn <command>`. Parallel ist die Verwendung von `ibmcloud wsk <command>` weiterhin möglich.

1.0.19 (2018-07-02)
* Kleinere Fehlerkorrekturen und Verbesserungen.

1.0.18 (2018-06-20)
* Es wurde eine Korrektur für das Aufheben von Bindungen von Serviceinstanzen hinzugefügt, die vom Benutzer zur Verfügung gestellten wurden.
* Leistungsverbesserungen.

1.0.17 (2018-06-12)
* Es wurde Unterstützung für das Binden (`ibmcloud wsk service bind`) und das Aufheben von Bindungen (`ibmcloud wsk service unbind`) bei Serviceinstanzen, die vom Benutzer zur Verfügung gestellt wurden und die mit dem Befehl `ibmcloud cf create-user-provided-service` erstellt wurden, hinzugefügt. 

1.0.16 (2018-05-24)
* Kleinere Fehlerkorrekturen und Verbesserungen.

1.0.15 (2018-05-21)
* Kleinere Fehlerkorrekturen und Verbesserungen.

1.0.14 (2018-05-17)
* Es wurde die Unterstützung des Zeichens `&` in Organisations- und Bereichsnamen aktiviert. 

1.0.13 (2018-05-07)
* Kleinere Fehlerkorrekturen und verbesserte Fehlerbehandlung.

1.0.12 (2018-04-30)
* {{site.data.keyword.Bluemix_notm}}-SDK-Updates, um die `bx`-CLI-Kompatibilität zu erhalten.

1.0.11 (2018-04-23)
* Kleinere Fehlerkorrekturen und Verbesserungen.

1.0.10 (2018-04-09)
* Neue Option `--web-secure` in `ibmcloud wsk action create|update`-Befehlen hinzugefügt, um Webaktionsendpunkte zu schützen. 
* Korrektur des [Fehlers](https://github.com/apache/incubator-openwhisk-cli/issues/237) des Back-to-back-Pfadparameters. 

1.0.9 (2018-03-16)
* Unterstützung für Servicebindung auf Paketebene aktiviert. 

1.0.8 (2018-02-22)
* Unterstützung für IAM-Servicebindung aktiviert. 

1.0.7 (2018-02-02)
* `ibmcloud wsk api` aktualisiert, um Pfadparameter wie z. B. `/api/{id}` zu akzeptieren. Informationen finden Sie im Abschnitt [API-Gateway](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway).
* Proxy-Unterstützung wiederhergestellt. 
* `swift:3` entfernt. 

1.0.6 (2018-01-30)
* Fehlerkorrektur für den Befehl `ibmcloud wsk service bind` für Aktionen in einem Paket.
