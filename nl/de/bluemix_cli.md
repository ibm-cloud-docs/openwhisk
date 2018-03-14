---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}}-CLI-Plug-in
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk_short}} stellt ein leistungsfähiges Plug-in für die {{site.data.keyword.Bluemix_notm}}-CLI (Befehlszeilenschnittstelle) bereit, die eine vollständige Verwaltung des {{site.data.keyword.openwhisk_short}}-Systems ermöglicht.
{: shortdesc}

## {{site.data.keyword.Bluemix_notm}}-CLI einrichten
{: #bluemix_cli_setup}

Laden Sie die [{{site.data.keyword.Bluemix_notm}}-CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html) herunter und installieren Sie sie.

## {{site.data.keyword.openwhisk_short}}-Plug-in einrichten
{: #cloudfunctions_plugin_setup}

Führen Sie den folgenden Befehl aus, um das {{site.data.keyword.openwhisk_short}}-Plug-in zu installieren: 
```
bx plugin install cloud-functions
```
{: pre}


Führen Sie den folgenden Befehl aus, um die erfolgreiche {{site.data.keyword.openwhisk_short}}-Plug-in-Installation zu überprüfen:
```
bx plugin list cloud-functions
```
{: pre}


In der Ausgabe werden die Informationen zu der installierten {{site.data.keyword.openwhisk_short}}-Versionen angezeigt:
```
Plugin Name          Version
Cloud-Functions      1.0.0
```

Sie können ein Upgrade für das {{site.data.keyword.openwhisk_short}}-Plug-in mithilfe des folgenden Befehls durchführen:
```
bx plugin update Cloud-Functions
```
{: pre}


Weitere Informationen zum Befehl 'plugin' können Sie durch `bx plugin --help` abrufen oder in der folgenden Dokumentation nachlesen:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_plugin_list

## {{site.data.keyword.Bluemix_notm}}-CLI-Authentifizierung
{: #bluemix_cli_auth}

{{site.data.keyword.openwhisk_short}} ist in zwei {{site.data.keyword.Bluemix_notm}}-Regionen verfügbar.

Wenn Sie sich an der {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle (CLI) anmelden, können Sie den {{site.data.keyword.Bluemix_notm}}-API-Endpunkt für die Zielregion neben der Organisation und dem Bereich angeben.

Geben Sie den folgenden Befehl ein, um sich an der Region 'Vereinigte Staaten (Süden)' (US South) anzumelden:
```
bx login -a api.ng.bluemix.net
```
{: pre}


Geben Sie den folgenden Befehl ein, um sich an der Region 'Vereintes Königreich' (United Kingdom) anzumelden:
```
bx login -a api.eu-gb.bluemix.net
```
{: pre}


Das Flag `-a` gibt den zu verwendenden {{site.data.keyword.Bluemix_notm}}-API-Endpunkt an. Wenn der API-Endpunkt festgelegt ist, ist die Option `-a` nicht erforderlich. Sie können den Befehl `bx api` verwenden, um den {{site.data.keyword.Bluemix_notm}}-API-Endpunkt explizit festzulegen. Zum Anzeigen der aktuellen Einstellung für den API-Endpunkt verwenden Sie den Befehl `bx target`.

Der Befehl `login` fordert Sie zur Eingabe von Informationen an, wie zum Beispiel die Organisation, den Bereich und das Kennwort, falls diese nicht angegeben wurden. Sie können die Organisation und den Bereich in der Befehlszeile angeben, um die Eingabeaufforderungen zu überspringen.
```
bx login -o <MY_ORG> -s <MY_SPACE>
```
{: pre}


Sie können für die Anmeldung auch einen {{site.data.keyword.Bluemix_notm}}-API-Schlüssel verwenden. Diese Methode ist nützlich, wenn Ihr Konto mit einer eingebundenen Anmeldung (Federated Login) konfiguriert ist, bei der Sie sich mit dem Flag `--sso` anmelden müssen. Die Verwendung eines API-Schlüssels ist außerdem nützlich, wenn Sie eine kontinuierliche Integration (Continuous Integration - CI) einrichten und eine unbeaufsichtigte Pipeline konfigurieren wollen.
https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key

Zum Erstellen eines neuen API-Schlüssels über die {{site.data.keyword.Bluemix_notm}}-CLI führen Sie den folgenden Befehl aus:
```
bx iam api-key-create MyKey
```
{: pre}


Verwenden Sie anschließend den generierten Wert des API-Schlüssels wie im folgenden Beispiel, um sich anzumelden:
```
bx login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
```
{: pre}


Weitere Informationen zum Befehl 'login' können Sie durch `bx login --help` abrufen oder in der folgenden Dokumentation nachlesen:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login


## {{site.data.keyword.openwhisk_short}}-CLI-Plug-in verwenden
{: #cloudfunctions_plugin_usage}

Überprüfen Sie Ihre Konfiguration. Führen Sie einen blockierenden (synchronen) Aufruf von 'echo' aus, indem Sie `hello` als Argument wie im folgenden Beispiel übergeben:
```
bx wsk action invoke whisk.system/utils/echo -p message hello --result
```
{: pre}


Sehen Sie sich die folgende Beispielausgabe an:
```
{
    "message":"hello"
}
```

Nach der Konfiguration Ihrer Umgebung können Sie die {{site.data.keyword.openwhisk_short}}-CLI für die folgenden Tasks verwenden:

* Sie können Code-Snippets oder Aktionen in {{site.data.keyword.openwhisk_short}} ausführen. Siehe [Aktionen erstellen und aufrufen](./openwhisk_actions.html).
* Sie können Ihre Aktionen durch Auslöser und Regeln so einrichten, dass sie auf Ereignisse reagieren. Siehe [Auslöser und Regeln erstellen](./openwhisk_triggers_rules.html).
* Sie können sich informieren, wie Aktionen in Paketen gebündelt und externe Ereignisquellen konfiguriert werden. Siehe [Pakete verwenden und erstellen](./openwhisk_packages.html).
* Sie können den Katalog der Pakete durchsuchen und Ihre Anwendungen durch externe Services wie zum Beispiel eine [Cloudant-Ereignisquelle](./openwhisk_cloudant.html) erweitern. Siehe [Für {{site.data.keyword.openwhisk_short}} eingerichtete Services verwenden](./openwhisk_catalog.html).

Zum Abrufen einer Liste von Befehlen für das {{site.data.keyword.openwhisk_short}}-Plug-in führen Sie den Befehl `bx wsk` ohne Argumente aus.

## Services über Aktionen verwenden
{: #binding_services}

{{site.data.keyword.openwhisk_short}} stellt den Befehl `service bind` bereit, um Ihre {{site.data.keyword.Bluemix_notm}}-Serviceberechtigungsnachweise für Ihren Code zur Laufzeit verfügbar zu machen. Der Befehl `service bind` ermöglicht Ihnen, einen beliebigen {{site.data.keyword.Bluemix_notm}}-Service an eine beliebige Aktion zu binden, die in {{site.data.keyword.openwhisk_short}} definiert ist.

Die einzelnen Schritte zur Verwendung von Services über Aktionen finden Sie im Abschnitt [Services über Aktionen verwenden](./binding_services.html).

## {{site.data.keyword.openwhisk_short}}-CLI zur Verwendung eines HTTPS-Proxys konfigurieren
{: #cli_https_proxy}

Die {{site.data.keyword.openwhisk_short}}-CLI kann zur Verwendung eines HTTPS-Proxys eingerichtet werden. Zur Einrichtung eines HTTPS-Proxys muss die Umgebungsvariable `HTTPS_PROXY` erstellt werden. In der Variablen müssen die Adresse des HTTPS-Proxys und der zugehörige Port im folgenden Format angegeben werden:
`{PROXY IP}:{PROXY PORT}`.

## Mit Regionen, Organisationen und Bereichen arbeiten
{: #region_info}

Zur Verwaltung Ihrer Bereitstellungen in der Vorproduktionsumgebung (Staging) und in der Produktionsumgebung können Sie Bereiche für beide Zwecke erstellen. Durch diese Bereiche kann {{site.data.keyword.openwhisk_short}} zwei verschiedene für Sie definierte Namensbereiche haben.

Sie können den Befehl `bx iam space-create` verwenden, um weitere Bereiche unter Ihrer Organisation wie "staging" und "production" zu erstellen. Beispiel:
```
bx iam space-create "staging"
bx iam space-create "production"
```

Weitere Informationen zum Befehl `bx iam` finden Sie in der folgenden Dokumentation: https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create

{{site.data.keyword.openwhisk_short}} unterliegt Einschränkungen für Namen von Namensbereichen. Weitere Informationen zu diesen Einschränkungen finden Sie in der folgenden Dokumentation: https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities

Wenn Sie bereits angemeldet sind, können Sie den Befehl `bx target` in der {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle (CLI) ausführen, um Regionen, Organisationen und Bereiche zu wechseln.

Verwenden Sie den folgenden Befehl, um alle {{site.data.keyword.Bluemix_notm}}-Regionen anzuzeigen.

{{site.data.keyword.openwhisk_short}} wird nur in den Regionen `us-south` und `eu-gb` unterstützt.
{: tip}

```
bx regions

Name       Geoortung                    Kunde  Bereitstellung  Domäne              CF-API-Endpunkt                  Typ
us-south   Vereinigte Staaten (Süden)   IBM    Produktion      ng.bluemix.net      https://api.ng.bluemix.net       Öffentlich
eu-gb      Vereintes Königreich         IBM    Produktion      eu-gb.bluemix.net   https://api.eu-gb.bluemix.net    Öffentlich
```

Verwenden Sie den Befehl `bx target` zum Wechseln von Regionen. Wenn Sie zum Beispiel zur Region 'Vereinigtes Königreich' und zum Bereich `staging` wechseln möchten, geben Sie den folgenden Befehl ein:
```
bx target -r eu-gb -s staging
```
{: pre}


Wenn Sie Bereiche innerhalb derselben Region wechseln müssen, zum Beispiel von 'staging' zu 'production', führen Sie den folgenden Befehl aus:
```
bx target -s production
```
{: pre}


Weitere Informationen zum Befehl `target` können Sie durch `bx target --help` abrufen oder in der folgenden Dokumentation nachlesen:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_target

## Von der OpenWhisk-CLI auf das {{site.data.keyword.openwhisk_short}}-CLI-Plug-in migrieren
{: #cli_migration}

Mit der Einführung des {{site.data.keyword.openwhisk_short}}-CLI-Plug-in wird die OpenWhisk-Befehlszeilenschnittstelle (CLI) nicht mehr benötigt.

### Befehlssyntax
{: #command_syntax}

Alle `wsk`-Befehle mit Ausnahme des Befehls `wsk bluemix login`, der nicht mehr erforderlich ist, funktionieren auf die gleiche Weise wie bei Verwendung des Befehls `bx wsk`. Alle Befehlsoptionen und -argumente sind gleich.

### API-Authentifizierung und API-Host
{: #api_authentication}

Für die OpenWhisk-Befehlszeilenschnittstelle (CLI) müssen Sie den Authentifizierungs-API-Schlüssel und den API-Host konfigurieren.
Mit dem {{site.data.keyword.openwhisk_short}}-CLI-Plug-in brauchen Sie den API-Schlüssel und den API-Host nicht explizit zu konfigurieren. Sie müssen Sie mit dem Befehl `bx login` und Ihre Region und Ihren Namensbereich mit dem Befehl `bx target` als Ziel festlegen. Anschließend beginnen alle weiteren Befehle mit `bx wsk`. Durch diese Art der Authentifizierung lässt sich außerdem ein Zugriff auf die {{site.data.keyword.Bluemix_notm}}-Webseite vermeiden, um bestimmte Informationen zur Konfiguration Ihrer CLI-Umgebung abzurufen.

Wenn Sie den Authentifizierungs-API-Schlüssel für {{site.data.keyword.openwhisk_short}} für die Verwendung über einen externen HTTP-Client wie cURL oder Postman abrufen müssen, können Sie ihn mit den folgenden Befehlen abrufen:

Führen Sie zum Abrufen des aktuellen API-Schlüssels den folgenden Befehl aus:
```
bx wsk property get --auth
```
{: pre}


Führen Sie zum Abrufen des aktuellen API-Hosts den folgenden Befehl aus:
```
bx wsk property get --apihost
```
{: pre}


Der API-Schlüssel ist jeweils für die Region, die Organisation und den Bereich spezifisch, die für das {{site.data.keyword.openwhisk_short}}-CLI-Plug-in als Ziel festgelegt sind.
{: tip}

### API-Gateway-Authentifizierung
{: #apigw_authentication}

Gegenwärtig muss für die OpenWhisk-Befehlszeile der Befehl `wsk bluemix login` ausgeführt werden, um die API-Gateway-Berechtigung für das Management Ihrer APIs mit dem Befehl `wsk api` konfigurieren zu können.

Mit dem {{site.data.keyword.openwhisk}}-CLI-Plug-in ist es nicht mehr erforderlich, den Befehl `wsk bluemix login` auszuführen. Stattdessen verwenden Sie den Befehl `bx login`, um sich bei {{site.data.keyword.Bluemix_notm}} anzumelden. Anschließend verwendet das {{site.data.keyword.openwhisk}}-Plug-in automatisch die Informationen zu Ihrer aktuellen Anmeldung und Ihrem aktuellen Bereich. Jetzt können Sie Ihre APIs mit dem Befehl `bx wsk api` verwalten.

### Implementierungsscripts migrieren
{: #migrating_deploy_scripts}

Wenn Sie Scripts haben, die die OpenWhisk-Befehlszeilenschnittstelle mit dem Binärprogramm `wsk` verwenden, funktionieren alle Befehle auf die gleiche Weise wie bei Verwendung des Befehls `bx wsk`. Sie können Ihre Scripts so ändern, dass sie das {{site.data.keyword.Bluemix_notm}}-CLI-Plug-in verwenden, oder einen Alias oder Wrapper erstellen, sodass aktuelle Ausführungen von `wsk` in `bx wsk` übersetzt werden. Die Befehle `bx login` und `bx target` in der {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle arbeiten im unbeaufsichtigten Modus. Beim unbeaufsichtigten Modus können Sie Ihre Umgebung konfigurieren, bevor Sie Befehle `bx wsk` ausführen, um Ihre {{site.data.keyword.openwhisk_short}}-Entitäten bereitzustellen und zu verwalten.


## Versionsprotokoll
{: #version_history}

Hierbei handelt es sich um eine Protokollaufzeichnung der Versionen, die Schwerpunkte und Fehlerkorrekturen aufzeigt.

1.0.7 (2018-02-02)
* Der Befehl `bx wsk api` akzeptiert jetzt Pfadparameter wie `/api/{id}`. Informationen finden Sie unter [API-Gateway](./openwhisk_apigateway.html).
* Proxy-Unterstützung wiederhergestellt.
* `swift:3` entfernt.

1.0.6 (2018-01-30)
* Fehler für den Befehl `bx wsk service bind` für Aktionen in einem Paket behoben.
