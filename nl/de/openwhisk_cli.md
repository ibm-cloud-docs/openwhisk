---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Eigenständige {{site.data.keyword.openwhisk_short}}-Befehlszeilenschnittstelle (CLI)

**Ab dem 9. März 2018 wird die eigenständige {{site.data.keyword.openwhisk_short}}-Befehlszeilenschnittstelle (CLI) nicht mehr zum Download zur Verfügung stehen. Verwenden Sie nach diesem Datum zur weiteren Verwaltung von {{site.data.keyword.openwhisk_short}}-Entitäten das [{{site.data.keyword.openwhisk_short}}-CLI-Plug-in](./bluemix_cli.html) für die {{site.data.keyword.Bluemix}}-Befehlszeilenschnittstelle**.

Die durch {{site.data.keyword.openwhisk}} verteilte Befehlszeilenschnittstelle **wsk** ermöglicht das Management aller {{site.data.keyword.openwhisk_short}}-Entitäten.
{: shortdesc}

<!--
This service is deprecated: All instances of this service are deprecated. Existing instances can be used until 09 December 2016. For more information, see the [deprecation announcement blog](http://www.com){: new_window}.
{:deprecated}

IBM recommends that you use the new [{{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.Bluemix_notm}} CLI](./bluemix_cli.html) to manage {{site.data.keyword.openwhisk_short}} entities. The following management tasks are easier if you use the plugin.
{: tip}
-->

* Konfiguration des {{site.data.keyword.openwhisk_short}}-Authentifizierungsschlüssels
  * Zugriff auf die {{site.data.keyword.openwhisk_short}}-Konsole zum Abruf des Authentifizierungsschlüssels nicht erforderlich
  * Automatische Authentifizierungsschlüsselgenerierung beim Wechsel zwischen Regionen, Organisationen und Bereichen
* Aktualisierung eines abgelaufenen {{site.data.keyword.openwhisk_short}}-Authentifizierungsschlüssels
* Aktualisierung der CLI auf eine spätere Version


Für die folgenden Management-Tasks müssen Sie das Plug-in verwenden.

* API-Management
  * Konfiguration des API-GW-Zugriffstokens
  * Aktualisierung eines abgelaufenen API-GW-Zugriffstokens

## {{site.data.keyword.openwhisk_short}}-CLI einrichten 
{: #openwhisk_cli_setup}

Sie können Ihren Namensbereich und Ihren Berechtigungsschlüssel über die {{site.data.keyword.openwhisk_short}}-Befehlszeilenschnittstelle einrichten.
Navigieren Sie zu [CLI konfigurieren](https://console.bluemix.net/openwhisk/cli?loadWsk=true) und gehen Sie gemäß den Anweisungen für die Installation vor.

Konfigurieren Sie zuerst die folgenden beiden Eigenschaften:

1. **API-Host:** Name oder IP-Adresse der {{site.data.keyword.openwhisk_short}}-Bereitstellung, die verwendet werden soll.
2. **Berechtigungsschlüssel:** Benutzername und Kennwort für den Zugriff auf die {{site.data.keyword.openwhisk_short}}-API.

Es sind zwei {{site.data.keyword.Bluemix_notm}}-Regionen verfügbar, für die jeweils ein eigener eindeutiger API-Host und Berechtigungsschlüssel erforderlich sind.

* Vereinigte Staaten (Süden)
  * API-Host: `openwhisk.ng.bluemix.net`

* Vereintes Königreich
  * API-Host: `openwhisk.eu-gb.bluemix.net`

Führen Sie den folgenden Befehl aus, um den API-Host für die gewünschte {{site.data.keyword.Bluemix_notm}}-Region festzulegen:

Vereinigte Staaten (Süden):
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

Vereinigtes Königreich:
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

Wenn Sie die Region wechseln müssen, müssen Sie die Befehlszeilenschnittstelle rekonfigurieren und dabei den API-Host und den Berechtigungsschlüssel ändern, da der Berechtigungsschlüssel für die jeweilige Region eindeutig ist.
{: tip}

Artefakte wie Aktionen, Regeln und Pakete sind für jede Region spezifisch. Wenn Sie also dasselbe Artefakt in mehreren Regionen verwenden wollen, müssen Sie es in jeder gewünschten Region bereitstellen.

Wenn Sie Ihren Berechtigungsschlüssel kennen, können Sie die CLI zur Verwendung dieses Schlüssels konfigurieren. 

Führen Sie den folgenden Befehl aus, um den Berechtigungsschlüssel festzulegen:

```
wsk property set --auth <Berechtigungsschlüssel>
```
{: pre}

**Tipp:** Die {{site.data.keyword.openwhisk_short}}-CLI speichert standardmäßig die in `~/.wskprops` festgelegten Eigenschaften. Die Speicherposition dieser Datei kann durch das Festlegen der Umgebungsvariablen `WSK_CONFIG_FILE` geändert werden. 

Versuchen Sie, [eine Aktion zu erstellen und auszuführen](./index.html#openwhisk_start_hello_world), um Ihre CLI-Konfiguration zu überprüfen.

## {{site.data.keyword.openwhisk_short}}-Befehlszeilenschnittstelle (CLI) verwenden
{: #using_openwhisk_cli}

Sobald die Umgebung konfiguriert ist, können Sie die {{site.data.keyword.openwhisk_short}}-CLI für die folgenden Tasks verwenden:

* Sie können Code-Snippets oder Aktionen in {{site.data.keyword.openwhisk_short}} ausführen. Siehe [Aktionen erstellen und aufrufen](./openwhisk_actions.html).
* Sie können Ihre Aktionen durch Auslöser und Regeln so einrichten, dass sie auf Ereignisse reagieren. Siehe [Auslöser und Regeln erstellen](./openwhisk_triggers_rules.html).
* Sie können sich informieren, wie Aktionen in Paketen gebündelt und externe Ereignisquellen konfiguriert werden. Siehe [Pakete verwenden und erstellen](./openwhisk_packages.html).
* Sie können den Katalog der Pakete durchsuchen und Ihre Anwendungen durch externe Services wie zum Beispiel eine [Cloudant-Ereignisquelle](./openwhisk_cloudant.html) erweitern. Siehe [Vorinstallierte {{site.data.keyword.openwhisk_short}}-Pakete](./openwhisk_catalog.html).

## CLI für die Verwendung eines HTTPS-Proxys konfigurieren
{: #cli_https_proxy}

Die CLI kann zur Verwendung eines HTTPS-Proxys eingerichtet werden. Zur Einrichtung eines HTTPS-Proxys muss die Umgebungsvariable `HTTPS_PROXY` erstellt werden. In der Variablen müssen die Adresse des HTTPS-Proxys und der zugehörige Port im folgenden Format angegeben werden: `{PROXY IP}:{PROXY PORT}`.
