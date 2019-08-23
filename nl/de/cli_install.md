---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: functions cli, serverless, cli, install, functions plug-in

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


# Befehlszeilenschnittstelle und Plug-in installieren
{: #cli_install}

{{site.data.keyword.openwhisk}} stellt ein leistungsfähiges Plug-in für die {{site.data.keyword.cloud_notm}}-CLI (Befehlszeilenschnittstelle) bereit, die eine vollständige Verwaltung des {{site.data.keyword.openwhisk_short}}-Systems ermöglicht. Sie können das Plug-in der {{site.data.keyword.openwhisk_short}}-Befehlszeilenschnittstelle verwenden, um Ihre Code-Snippets in Aktion zu verwalten, um Auslöser und Regeln zu erstellen, damit Ihre Aktionen auf Ereignisse reagieren können, und um Aktionen in Paketen zu bündeln.
{:shortdesc}


## {{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle einrichten
{: #cli_setup}

**Vorbereitende Schritte**

Sie müssen ein [{{site.data.keyword.cloud_notm}}-Konto](https://cloud.ibm.com/){: external} erstellen. 

Sie können die {{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle herunterladen, installieren und sich dann anmelden.
{: shortdesc}

1. Laden Sie die [{{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli) herunter und installieren Sie sie.

2. Melden Sie sich an der {{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle an. Zum Angeben einer {{site.data.keyword.cloud_notm}}-Region [schließen Sie den API-Endpunkt ein](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions). 

  ```
  ibmcloud login
  ```
  {: pre}

3. Befolgen Sie die Eingabeaufforderungen, um Ihr {{site.data.keyword.cloud_notm}}-Konto auszuwählen. 

4. Rufen Sie eine Liste der Ressourcengruppen ab.  

```
ibmcloud resource groups
```
{: pre}

**Beispielausgabe**

```
Retrieving all resource groups under account <account_name> as email@ibm.com...
OK
Name      ID                                 Default Group   State
default   a8a12accd63b437bbd6d58fb8b462ca7   true            ACTIVE
test      a8a12accd63b437bbd6d58fb8b462ca7   false           ACTIVE
```
{: screen}

5. Optional: Geben Sie eine andere Ressourcengruppe als die Standardgruppe an, indem Sie den folgenden Befehl ausführen. 
```
ibmcloud target -g <resource_group>
```
{: pre}


**Beispielausgabe**

```
Targeted resource group <resource_group>
```
{: screen}

## {{site.data.keyword.openwhisk_short}}-Befehlszeilenschnittstellen-Plug-in einrichten
{: #cli_plugin_setup}

Laden Sie das Plug-in für die Befehlszeilenschnittstelle (CLI) herunter und installieren Sie es, um mit {{site.data.keyword.openwhisk_short}} zu arbeiten.
{: shortdesc}

Sie können das {{site.data.keyword.openwhisk_short}}-CLI-Plug-in für die Ausführung der folgenden Tasks verwenden. 

* Sie können Code-Snippets oder Aktionen in {{site.data.keyword.openwhisk_short}} ausführen. Siehe [Aktionen erstellen und aufrufen](/docs/openwhisk?topic=cloud-functions-actions).
* Erstellen Sie Auslöser und Regeln, um Ihre Aktionen so einzurichten, dass sie auf Ereignisse reagieren. Siehe [Auslöser und Regeln erstellen](/docs/openwhisk?topic=cloud-functions-triggers).
* Bündeln Sie Aktionen und konfigurieren Sie externe Ereignisquellen. Siehe [Pakete verwenden und erstellen](/docs/openwhisk?topic=cloud-functions-pkg_ov).
* Durchsuchen Sie den Katalog der Pakete und erweitern Sie Ihre Anwendungen mit externen Services. Siehe [{{site.data.keyword.cloud_notm}} hinzufügen](/docs/openwhisk?topic=cloud-functions-services). 

Führen Sie die folgenden Schritte aus, um das {{site.data.keyword.openwhisk_short}}-CLI-Plug-in zu installieren. 

1. Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in.

  ```
  ibmcloud plugin install cloud-functions
  ```
  {: pre}

2. Überprüfen Sie, ob das Plug-in installiert ist.

  ```
  ibmcloud plugin list
  ```
  {: pre}

  **Ausgabe**
  ```
  Plugin Name          Version
  cloud-functions/wsk/functions/fn      1.0.32
  ```
  {: screen}

3. Nach der Anmeldung beginnen alle {{site.data.keyword.openwhisk_short}}-Befehle mit `ibmcloud fn`. Um anzuzeigen, was Sie alles mit dem {{site.data.keyword.openwhisk_short}}-Plug-in tun können, führen Sie `ibmcloud fn` ohne Argumente aus.
  ```
  ibmcloud fn
  ```
  {: pre}




## {{site.data.keyword.openwhisk_short}}-Namensbereiche als Ziel angeben
{: #cli_regions}
Standardmäßig verwendet {{site.data.keyword.openwhisk_short}} [für IAM aktivierte Namensbereiche](/docs/iam?topic=iam-iamoverview){: external}. Sie können keine Namensbereiche mehr erstellen, die auf Cloud Foundry basieren.
{: important}

### Erstellen Sie einen Namensbereich oder geben Sie einen Namensbereich als Ziel an. 
Zum Abrufen einer Liste Ihrer {{site.data.keyword.openwhisk_short}}-Namensbereiche führen Sie `ibmcloud fn namespace list` aus. 

#### Erstellen Sie einen für IAM aktivierten Namensbereich. 
  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description">]
  ```
  {: pre}

**Antwort**
  ```
  ok: created namespace <namespace_name>
  ```
  {: screen}


#### Geben Sie einen für IAM aktivierten Namensbereich als Ziel an.  
  ```
  ibmcloud fn property set --namespace <namespace_name>
  ``` 
  {: pre}


**Antwort**
  ```
  ok: whisk namespace set to <namespace_name>
  ```
  {: screen}
  
#### Geben Sie einen Cloud Foundry-basierten Namensbereich als Ziel an.  
  
Sie können die Flags `-o` und `-s` verwenden, um eine bestimmte Organisation (`org`) und einen bestimmten Bereich (`space`) anzugeben, oder Sie können die Eingabeaufforderungen befolgen. 

* Geben Sie einen Cloud Foundry-Namensbereich als Ziel an, indem Sie den Namen der Organisation (`org`) und des Bereichs (`space`) in den Befehl `target` einschließen. 

```
ibmcloud target --cf  -o <org> -s <space>
```
{: pre}

* Geben Sie Cloud Foundry als Ziel an und befolgen Sie die Eingabeaufforderungen, um eine Organisation (`org`) und einen Bereich (`space`) auszuwählen. 

```
ibmcloud target --cf
```
{: pre}


**Antwort**
  ```
  Targeted Cloud Foundry (https://api.ng.bluemix.net)

  Targeted org <org_name>

  Targeted space <space_name>

  API endpoint:      https://cloud.ibm.com
  Region:            us-south
  User:              <email>
  Account:           (<account_id>) <-> <account>
  Resource group:    default
  CF API endpoint:   https://api.ng.bluemix.net (API version: 2.128.0)
  Org:               <org_name>
  Space:             <space_name>
  ```
  {: screen} 





#### Optional: Namensbereiche für Bereitstellungen in der Vorproduktionsumgebung (Staging) und in der Produktionsumgebung erstellen

Sie können für IAM aktivierte Namensbereiche erstellen, um Ihre {{site.data.keyword.openwhisk_short}}-Bereitstellungen in der Vorproduktionsumgebung (Staging) und in der Produktionsumgebung zu verwalten, indem sie für beide Bereitstellungen Namensbereiche erstellen. Führen Sie [`ibmcloud fn namespace create`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace_create) aus, um weitere Namensbereiche wie "staging" und "production" unter Ihrer Organisation zu erstellen: 

Erstellen Sie den Namensbereich 'staging'. 
```
ibmcloud fn namespace create staging
```
{: pre}

Erstellen Sie den Namensbereich 'production'. 
```
ibmcloud fn namespace create production
```
{: pre}

{{site.data.keyword.openwhisk_short}} unterliegt Einschränkungen für Namen von Namensbereichen. Weitere Informationen finden Sie in der Dokumentation [Systemdetails und Grenzwerte](/docs/openwhisk?topic=cloud-functions-limits#limits_entities).
{: tip}


## {{site.data.keyword.openwhisk_short}}-CLI zur Verwendung eines HTTPS-Proxys konfigurieren
{: #cli_proxy}

Die {{site.data.keyword.openwhisk_short}}-CLI kann zur Verwendung eines HTTPS-Proxys eingerichtet werden. Zur Einrichtung eines HTTPS-Proxys muss die Umgebungsvariable `HTTPS_PROXY` erstellt werden. In der Variablen müssen die Adresse des HTTPS-Proxys und der zugehörige Port im folgenden Format angegeben werden: `{PROXY IP}:{PROXY PORT}`.

Durch Ändern des Namens von `org` oder `space` wird ein neuer Namensbereich auf der Basis des geänderten Namens erstellt. Die Entitäten in dem alten Namensbereich sind im neuen Namensbereich nicht sichtbar und werden möglicherweise gelöscht.
{: important}


## Von der OpenWhisk-CLI auf das {{site.data.keyword.openwhisk_short}}-CLI-Plug-in migrieren
{: #cli_migrate}

Sie können jetzt das {{site.data.keyword.openwhisk_short}}-CLI-Plug-in verwenden, um mit {{site.data.keyword.openwhisk_short}}-Entitäten zu interagieren. Sie können zwar weiterhin die eigenständige OpenWhisk-CLI verwenden. Diese verfügt jedoch nicht über die neuesten von {{site.data.keyword.openwhisk_short}} unterstützten Features wie IAM-basierte Namensbereiche und `service bind`.
{: shortdesc}


### Befehlssyntax
{: #cli_syntax}

Alle Befehlsoptionen und Argumente für Befehle im Cloud Functions-CLI-Plug-in sind mit den Optionen für die [eigenständige OpenWhisk-CLI](https://github.com/apache/incubator-openwhisk-cli){: external} identisch. Beachten Sie jedoch die folgenden Unterschiede. 

* Das {{site.data.keyword.openwhisk}}-Plug-in verwendet automatisch Ihre aktuellen Anmelde- und Zielinformationen. 
* `wsk`-Befehle werden jetzt als `ibmcloud fn` ausgeführt. 
* Der Befehl `wsk ibmcloud login` ist nicht mehr erforderlich. Sie können sich mit `ibmcloud login` anmelden. 
* Sie können Ihre APIs mit `ibmcloud fn api` verwalten. 

Weitere Informationen finden Sie in der [{{site.data.keyword.openwhisk_short}}-CLI-Referenz](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli). 

### API-Authentifizierung und API-Host
{: #cli_api_auth}

Mit dem {{site.data.keyword.openwhisk_short}}-CLI-Plug-in brauchen Sie den API-Schlüssel und den API-Host nicht explizit zu konfigurieren. Stattdessen können Sie sich mit dem Befehl `ibmcloud login` anmelden. Sie können einen für IAM aktivierten Namensbereich als Ziel angeben, indem Sie `ibmcloud fn property set --namespace <namespace_name>` ausführen. Sie können einen Cloud Foundry-basierten Namensbereich als Ziel angeben, indem Sie `ibmcloud target --cf` ausführen. Nach dem Anmelden beginnen alle Befehle mit `ibmcloud fn`.


Wenn Sie den Authentifizierungs-API-Schlüssel für {{site.data.keyword.openwhisk_short}} in einem externen HTTP-Client wie cURL oder Postman verwenden müssen, können Sie ihn mit den folgenden Befehlen abrufen. 

Führen Sie den folgenden Befehl aus, um den aktuellen API-Schlüssel abzurufen. 
```
ibmcloud fn property get --auth
```
{: pre}

Führen Sie den folgenden Befehl aus, um den aktuellen API-Host abzurufen. 
```
ibmcloud fn property get --apihost
```
{: pre}

Der API-Schlüssel ist jeweils für die Region, die Organisation und den Bereich spezifisch, die für das {{site.data.keyword.openwhisk_short}}-CLI-Plug-in als Ziel festgelegt sind.
{: tip}


### API-Gateway-Authentifizierung
{: #cli_apigw_authentication}

Für die OpenWhisk-CLI musste der Befehl `wsk ibmcloud login` ausgeführt werden, damit die API-Gateway-Berechtigung für das Management Ihrer APIs mit dem Befehl `wsk api` konfiguriert werden konnte. Mit dem {{site.data.keyword.openwhisk_short}}-CLI-Plug-in ist es nicht erforderlich, den Befehl `wsk ibmcloud login` auszuführen. Wenn Sie stattdessen den Befehl `ibmcloud login` verwenden, um sich bei {{site.data.keyword.cloud_notm}} anzumelden, verwendet das {{site.data.keyword.openwhisk}}-Plug-in automatisch die Informationen zu Ihrer aktuellen Anmeldung und Ihrem aktuellen Bereich. Jetzt können Sie Ihre APIs mit dem Befehl `ibmcloud fn api` verwalten.


### Bereitstellungsscripts migrieren
{: #cli_migrating_deploy_scripts}

Wenn Sie über Scripts verfügen, die die OpenWhisk-CLI mit den `wsk`-Befehlen verwenden, funktionieren alle Befehle auf die gleiche Weise wie bei Verwendung des Befehls `ibmcloud fn`. Sie können Ihre Scripts so ändern, dass sie das {{site.data.keyword.cloud_notm}}-CLI-Plug-in verwenden, oder einen Alias oder Wrapper erstellen, sodass die aktuellen Befehle, die `wsk` verwenden, in `ibmcloud fn` übersetzt werden. Die Befehle `ibmcloud login` und `ibmcloud target` in der {{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle arbeiten im unbeaufsichtigten Modus. Beim unbeaufsichtigten Modus können Sie, bevor Sie `ibmcloud fn`-Befehle ausführen, Ihre Umgebung konfigurieren, um Ihre {{site.data.keyword.openwhisk_short}}-Entitäten bereitzustellen und zu verwalten.

## CLI-Versionsprotokoll
{: #cli_versions}

Hierbei handelt es sich um eine Protokollaufzeichnung der Versionen, die Schwerpunkte und Fehlerkorrekturen aufzeigt.

v1.0.30 (03. April 2019)
* Die `service bind`-Verarbeitung von IAM und organisations- und bereichsbasierten Services wurde verbessert.
* Ein Korrektur für die Verarbeitung des API-Endpunkts 'https://cloud.ibm.com' wurde hinzugefügt.

v1.0.29 (06. Februar 2019)
* Die Befehle `deploy` und `undeploy` wurden hinzugefügt, um eine Sammlung von Functions-Entitäten über eine Manifestdatei bereitzustellen oder ihre Bereitstellung zu beenden. Weitere Informationen finden Sie in der Dokumentation zur [Bereitstellung](/docs/openwhisk?topic=cloud-functions-deploy#deploy). 

v1.0.28 (21. Januar 2019)
* Es wurde eine Fehlernachricht hinzugefügt, wenn `update|delete|get namespace name` mehrfach auftritt.

v1.0.27 (11. Dezember 2018)
* Es wurden `namespace get`-Korrekturen hinzugefügt.
* Es wurde eine Korrektur für `--save-as` hinzugefügt, wenn eine Aktion eine Blackbox-Aktion ist.

v1.0.26 (30. November 2018)
* Es wurde `fn property get --auth` aktiviert, um den Authentifizierungsschlüssel in einer neuen Umgebung korrekt zurückzugeben.

v1.0.25 (23. November 2018)
* Verbesserte Anzeige von Fehlernachrichtenergebnissen.
* Es wurde eine `fn namespace get`-Korrektur für die korrekte Anzeige von Namensbereichseigenschaften hinzugefügt.

1.0.23 (15. Oktober 2018)
* Es wurde Unterstützung für die Erkennung von ruby-Aktionscode (`.rb`) hinzugefügt. 

1.0.22 (20. August 2018)
* Es wurde Unterstützung für die Region 'us-east' (USA (Osten)) hinzugefügt.

1.0.21 (01. August 2018)
* Die Aliasnamen `fn` und `functions` können jetzt für {{site.data.keyword.openwhisk_short}}-Befehle verwendet werden: `ibmcloud fn <command>` und `ibmcloud fn <command>`. Parallel ist die Verwendung von `ibmcloud wsk <command>` weiterhin möglich.

1.0.19 (02. Juli 2018)
* Kleinere Fehlerkorrekturen und Verbesserungen.

1.0.18 (20. Juni 2018)
* Es wurde eine Korrektur für das Aufheben von Bindungen von Serviceinstanzen hinzugefügt, die vom Benutzer zur Verfügung gestellten wurden.
* Leistungsverbesserungen.

1.0.17 (12. Juni 2018)
* Es wurde Unterstützung für das Binden (`ibmcloud wsk service bind`) und das Aufheben von Bindungen (`ibmcloud wsk service unbind`) bei Serviceinstanzen, die vom Benutzer zur Verfügung gestellt wurden und die mit dem Befehl `ibmcloud cf create-user-provided-service` erstellt wurden, hinzugefügt.

1.0.16 (24. Mai 2018)
* Kleinere Fehlerkorrekturen und Verbesserungen.

1.0.15 (21. Mai 2018)
* Kleinere Fehlerkorrekturen und Verbesserungen.

1.0.14 (17. Mai 2018)
* Es wurde die Unterstützung des Zeichens `&` in Organisations- und Bereichsnamen aktiviert.

1.0.13 (07. Mai 2018)
* Kleinere Fehlerkorrekturen und verbesserte Fehlerbehandlung.

1.0.12 (30. April 2018)
* {{site.data.keyword.cloud_notm}}-SDK-Updates, um die `bx`-CLI-Kompatibilität zu erhalten.

1.0.11 (23. April 2018)
* Kleinere Fehlerkorrekturen und Verbesserungen.

1.0.10 (09. April 2018)
* Neue Option `--web-secure` in `ibmcloud wsk action create|update`-Befehlen hinzugefügt, um Webaktionsendpunkte zu schützen.
* Korrektur des [Fehlers](https://github.com/apache/incubator-openwhisk-cli/issues/237){: external} des Back-to-back-Pfadparameters. 

1.0.9 (16. März 2018)
* Unterstützung für Servicebindung auf Paketebene aktiviert.

1.0.8 (22. Februar 2018)
* Unterstützung für IAM-Servicebindung aktiviert.

1.0.7 (02. Februar 2018)
* `ibmcloud wsk api` aktualisiert, um Pfadparameter wie z. B. `/api/{id}` zu akzeptieren. Weitere Informationen finden Sie in [API-Gateway](/docs/openwhisk?topic=cloud-functions-apigateway). 
* Proxy-Unterstützung wiederhergestellt.
* `swift:3` entfernt.

1.0.6 (30. Januar 2018)
* Fehlerkorrektur für den Befehl `ibmcloud wsk service bind` für Aktionen in einem Paket.



