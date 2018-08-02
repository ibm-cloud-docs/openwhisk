---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}}-CLI-Plug-in einrichten
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk}} stellt ein leistungsfähiges Plug-in für die {{site.data.keyword.Bluemix_notm}}-CLI (Befehlszeilenschnittstelle) bereit, die eine vollständige Verwaltung des {{site.data.keyword.openwhisk_short}}-Systems ermöglicht.
{: shortdesc}

## {{site.data.keyword.Bluemix_notm}}-CLI einrichten
{: #bluemix_cli_setup}

Sie können die {{site.data.keyword.Bluemix_notm}}-CLI herunterladen, installieren und sich dann anmelden.
{: shortdesc}

1. Laden Sie die [{{site.data.keyword.Bluemix_notm}}-CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html) herunter und installieren Sie sie.

2. Melden Sie sich an der {{site.data.keyword.Bluemix_notm}}-CLI an. {{site.data.keyword.openwhisk_short}} ist in den {{site.data.keyword.Bluemix_notm}}-Regionen 'Vereinigte Staaten (Süden)' und 'Vereinigtes Königreich' verfügbar. Wenn der {{site.data.keyword.Bluemix_notm}}-API-Endpunkt nicht angegeben ist, geben Sie einen mit dem Flag `-a` an.

    * Geben Sie den folgenden Befehl ein, um sich an der Region 'Vereinigte Staaten (Süden)' (US South) anzumelden:
      ```
      ibmcloud login -a api.ng.bluemix.net
      ```
      {: pre}

    * Geben Sie den folgenden Befehl ein, um sich an der Region 'Vereintes Königreich' (United Kingdom) anzumelden:
      ```
      ibmcloud login -a api.eu-gb.bluemix.net
      ```
      {: pre}

  Sie können den Befehl `ibmcloud api` verwenden, um den {{site.data.keyword.Bluemix_notm}}-API-Endpunkt explizit festzulegen. Zum Anzeigen der aktuellen Einstellung für den API-Endpunkt verwenden Sie den Befehl `ibmcloud target`.
 {: tip}

3. Der Befehl `ibmcloud login` fordert Sie zur Eingabe von Informationen auf, wie zum Beispiel die Organisation, den Bereich und das Kennwort, falls diese nicht angegeben wurden.

  Sie können bei der Anmeldung die Organisation und den Bereich angeben, um die Eingabeaufforderungen zu überspringen. Verwenden Sie die folgenden Flags: `ibmcloud login -o <ORG> -s <SPACE>`.
  {: tip}

Sie können für die Anmeldung auch einen {{site.data.keyword.Bluemix_notm}}-API-Schlüssel verwenden. Diese Methode ist nützlich, wenn Ihr Konto mit einer eingebundenen Anmeldung (federated login) konfiguriert ist, bei der Sie sich mit dem Flag `--sso` anmelden müssen. [Die Verwendung eines API-Schlüssels](https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key) ist außerdem nützlich, wenn Sie eine kontinuierliche Integration (Continuous Integration - CI) einrichten und eine unbeaufsichtigte Pipeline konfigurieren wollen.

1. Erstellen Sie einen neuen API-Schlüssel.
```
    ibmcloud iam api-key-create MyKey
    ```
    {: pre}

2. Verwenden Sie den generierten Wert des API-Schlüssels, um sich anzumelden.
    ```
    ibmcloud login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
    ```
    {: pre}
</br>
Weitere Informationen zum Befehl `ibmcloud login` können Sie durch `ibmcloud login --help` abrufen oder in folgendem Abschnitt nachlesen: [IBM Cloud-Befehle (bx)](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login).

## {{site.data.keyword.openwhisk_short}}-Plug-in einrichten
{: #cloudfunctions_plugin_setup}

Sie können das {{site.data.keyword.openwhisk_short}}-Plug-in herunterladen und installieren.
{: shortdesc}

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

3. Führen Sie einen blockierenden (synchronen) Aufruf von `echo` aus. Übergeben Sie `hello` als Argument.
    ```
    ibmcloud fn action invoke whisk.system/utils/echo -p message hello --result
    ```
    {: pre}

4. Überprüfen Sie, ob die Nachricht `hello` in der Ausgabe zurückgegeben wird.
    Beispielausgabe:
    ```
    {
        "message":"hello"
    }
    ```
    {: screen}

5. Sie können ein Upgrade für das {{site.data.keyword.openwhisk_short}}-Plug-in mithilfe des folgenden Befehls durchführen:
    ```
    ibmcloud plugin update cloud-functions
    ```
    {: pre}

Sie können die {{site.data.keyword.openwhisk_short}}-CLI für folgende Aktivitäten verwenden:

* Sie können Code-Snippets oder Aktionen in {{site.data.keyword.openwhisk_short}} ausführen. Siehe [Aktionen erstellen und aufrufen](./openwhisk_actions.html).
* Sie können Ihre Aktionen durch Auslöser und Regeln so einrichten, dass sie auf Ereignisse reagieren. Siehe [Auslöser und Regeln erstellen](./openwhisk_triggers_rules.html).
* Sie können sich informieren, wie Aktionen in Paketen gebündelt und externe Ereignisquellen konfiguriert werden. Siehe [Pakete verwenden und erstellen](./openwhisk_packages.html).
* Sie können den Katalog der Pakete durchsuchen und Ihre Anwendungen durch externe Services wie zum Beispiel eine [{{site.data.keyword.cloudant}}-Ereignisquelle](./openwhisk_cloudant.html) erweitern.

Zum Auflisten von Befehlen für das {{site.data.keyword.openwhisk_short}}-Plug-in führen Sie den Befehl `ibmcloud fn` ohne Argumente aus.
{: tip}

## Services über Aktionen verwenden
{: #binding_services}

{{site.data.keyword.openwhisk_short}} stellt den Befehl `service bind` bereit, um Ihre {{site.data.keyword.Bluemix_notm}}-Serviceberechtigungsnachweise für Ihren Code zur Laufzeit verfügbar zu machen. Anschließend können Sie den Befehl `service bind` verwenden, um einen beliebigen {{site.data.keyword.Bluemix_notm}}-Service an eine beliebige Aktion zu binden, die in {{site.data.keyword.openwhisk_short}} definiert ist.

Die einzelnen Schritte zur Verwendung von Services über Aktionen finden Sie im Abschnitt [Services an Aktionen binden](./binding_services.html).

## {{site.data.keyword.openwhisk_short}}-CLI zur Verwendung eines HTTPS-Proxys konfigurieren
{: #cli_https_proxy}

Die {{site.data.keyword.openwhisk_short}}-CLI kann zur Verwendung eines HTTPS-Proxys eingerichtet werden. Zur Einrichtung eines HTTPS-Proxys muss die Umgebungsvariable `HTTPS_PROXY` erstellt werden. In der Variablen müssen die Adresse des HTTPS-Proxys und der zugehörige Port im folgenden Format angegeben werden:
`{PROXY IP}:{PROXY PORT}`.

## Regionen, Organisationen und Bereichen wechseln
{: #region_info}

Wenn Sie bereits angemeldet sind, können Sie den Befehl `ibmcloud target` in der {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle (CLI) ausführen, um Regionen, Organisationen und Bereiche zu wechseln.

{{site.data.keyword.openwhisk_short}} ist in den {{site.data.keyword.Bluemix_notm}}-Regionen 'Vereinigte Staaten (Süden)' und 'Vereinigtes Königreich' verfügbar. Um zu einer anderen Region zu wechseln, verwenden Sie den Befehl `ibmcloud target`. Wenn Sie zum Beispiel zur Region 'Vereinigtes Königreich' und zum Bereich `staging` wechseln möchten, verwenden Sie den folgenden Befehl:
```
ibmcloud target -r eu-gb -s staging
```
{: pre}

Zur Verwaltung Ihrer Bereitstellungen in der Vorproduktionsumgebung (Staging) und in der Produktionsumgebung können Sie Bereiche für beide Zwecke erstellen. Durch diese Bereiche kann {{site.data.keyword.openwhisk_short}} zwei verschiedene für Sie definierte Namensbereiche haben. Führen Sie den Befehl [`ibmcloud iam space-create`](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create) aus, um weitere Bereiche unter Ihrer Organisation wie "staging" und "production" zu erstellen:

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}} unterliegt Einschränkungen für Namen von Namensbereichen. Weitere Informationen finden Sie in der Dokumentation zu [Systemdetails und Begrenzungen](https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities).
{: tip}

## Von der OpenWhisk-CLI auf das {{site.data.keyword.openwhisk_short}}-CLI-Plug-in migrieren
{: #cli_migration}

Mit der Einführung des {{site.data.keyword.openwhisk_short}}-CLI-Plug-ins wird die eigenständige OpenWhisk-Befehlszeilenschnittstelle (CLI) nicht mehr benötigt.

### Befehlssyntax
{: #command_syntax}

Alle `wsk`-Befehle mit Ausnahme des Befehls `wsk bluemix login`, der nicht mehr erforderlich ist, funktionieren auf die gleiche Weise wie bei Verwendung des Befehls `ibmcloud fn`. Alle Befehlsoptionen und -argumente sind gleich.

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

Wenn Sie Scripts haben, die die OpenWhisk-Befehlszeilenschnittstelle mit dem Binärprogramm `wsk` verwenden, funktionieren alle Befehle auf die gleiche Weise wie bei Verwendung des Befehls `ibmcloud fn`. Sie können Ihre Scripts so ändern, dass sie das {{site.data.keyword.Bluemix_notm}}-CLI-Plug-in verwenden, oder einen Alias oder Wrapper erstellen, sodass die aktuellen Befehle, die `wsk` verwenden, in `ibmcloud fn` übersetzt werden. Die Befehle `ibmcloud login` und `ibmcloud target` in der {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle arbeiten im unbeaufsichtigten Modus. Beim unbeaufsichtigten Modus können Sie Ihre Umgebung konfigurieren, bevor Sie `ibmcloud fn`-Befehle ausführen, um Ihre {{site.data.keyword.openwhisk_short}}-Entitäten bereitzustellen und zu verwalten.

## Versionsprotokoll
{: #version_history}

Hierbei handelt es sich um eine Protokollaufzeichnung der Versionen, die Schwerpunkte und Fehlerkorrekturen aufzeigt.

1.0.18 (2018-06-20)
* Korrektur für das Aufheben von Bindungen von Serviceinstanzen, die vom Benutzer zur Verfügung gestellten wurden.
* Leistungsverbesserungen.

1.0.17 (2018-06-12)
* Neu hinzugefügte Unterstützung für das Binden (`ibmcloud wsk service bind`) und das Aufheben von Bindungen (`ibmcloud wsk service unbind`) bei Serviceinstanzen, die vom Benutzer zur Verfügung gestellt wurden und die mit dem Befehl `ibmcloud cf create-user-provided-service` erstellt wurden.

1.0.16 (2018-05-24)
* Kleinere Fehlerkorrekturen und Verbesserungen.

1.0.15 (2018-05-21)
* Kleinere Fehlerkorrekturen und Verbesserungen.

1.0.14 (2018-05-17)
* Unterstützung des Zeichens `&` in Organisations- und Bereichsnamen.

1.0.13 (2018-05-07)
* Kleinere Fehlerkorrekturen und verbesserte Fehlerbehandlung.

1.0.12 (2018-04-30)
* {{site.data.keyword.Bluemix_notm}}-SDK-Updates, um die `bx`-CLI-Kompatibilität zu erhalten.

1.0.11 (2018-04-23)
* Kleinere Fehlerkorrekturen und Verbesserungen.

1.0.10 (2018-04-09)
* Neu hinzugefügte Option `--web-secure` in `ibmcloud wsk action create|update`-Befehlen, um Webaktionsendpunkte zu schützen.
* Korrektur von [Fehler](https://github.com/apache/incubator-openwhisk-cli/issues/237) bei Back-to-back-Pfadparameter.

1.0.9 (2018-03-16)
* Unterstützung für Servicebindung auf Paketebene.

1.0.8 (2018-02-22)
* Unterstützung für IAM-Servicebindung.

1.0.7 (2018-02-02)
* `ibmcloud wsk api` akzeptiert jetzt Pfadparameter wie z. B. `/api/{id}`. Informationen finden Sie im Abschnitt [API-Gateway](./openwhisk_apigateway.html).
* Proxy-Unterstützung wiederhergestellt.
* `swift:3` entfernt.

1.0.6 (2018-01-30)
* Fehlerkorrektur für den Befehl `ibmcloud wsk service bind` für Aktionen in einem Paket.
