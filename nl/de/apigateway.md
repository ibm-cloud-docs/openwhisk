---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: serverless, rest api, gateway, web actions, functions

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


# Serverunabhängige REST-APIs erstellen
{: #apigateway}

Sie können APIs verwenden, um {{site.data.keyword.openwhisk}}-[Webaktionen](/docs/openwhisk?topic=cloud-functions-actions_web) direkt zu verwalten.
{: shortdesc}

Das Erstellen von APIs mit API-Gateway wird für IAM-basierte Namensbereiche nicht unterstützt. Verwenden Sie stattdessen einen Cloud Foundry-basierten Namensbereich.
{: important}

## Warum ist es sinnvoll, REST-APIs mit {{site.data.keyword.openwhisk_short}} zu verwenden? 

Sie können das API-Gateway als Proxy für Ihre Webaktionen verwenden. Das API-Gateway stellt HTTP-Methodenrouting, Client-IDs und geheime Schlüssel, Grenzwerte, CORS, Anzeigen der API-Nutzung, Anzeigen von Antwortprotokollen und Richtlinien zur gemeinsamen API-Nutzung zur Verfügung. 

Weitere Informationen zum API-Management finden Sie in der [Dokumentation zum API-Management](/docs/api-management?topic=api-management-manage_openwhisk_apis).

## Erste API erstellen
{: #api_create}

Zum Erstellen von REST-APIs müssen Sie über die Berechtigungen `SpaceDeveloper` in Ihrem Cloud Foundry-Bereich verfügen. Die Bereichsberechtigungen können durch Ausführen von `ibmcloud account space-roles <org>` angezeigt werden.
{: note}

Bevor Sie beginnen, installieren Sie das [{{site.data.keyword.openwhisk_short}} CLI-Plug-in](/docs/openwhisk?topic=cloud-functions-cli_install).

1. Speichern Sie den folgenden Code in einer JavaScript-Datei namens `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello, ${name}!`};
  }
  ```
  {: codeblock}

2. Erstellen Sie eine Webaktion mit dem Namen `hello`, indem Sie die von Ihnen erstellte Datei verwenden. Stellen Sie sicher, dass Sie das Flag `--web true` hinzufügen. Ersetzen Sie `<filepath>` durch den Dateipfad Ihrer Datei `hello.js`. 

  ```
  ibmcloud fn action create hello <filepath>/hello.js --web true
  ```
  {: pre}

  **Beispielausgabe**
  ```
  ok: created action hello
  ```
  {: screen}

3. Erstellen Sie eine API mit dem Basispfad `/hello` und dem Pfad `/world` sowie mit der Methode `get` und dem Antworttyp `json`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  **Beispielausgabe**
Eine neue URL wird generiert, die die Aktion `hello` mit einer HTTP-Methode `GET` zugänglich macht. 

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  
4. Senden Sie mit dem folgenden cURL-Befehl eine HTTP-Testanforderung an die URL. 
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=Jane
  ```
  {: pre}

  **Beispielausgabe**
Die Webaktion `hello` wird aufgerufen und gibt ein JSON-Objekt zurück, das den Parameter `name` im Abfrageparameter enthält. Sie können Parameter mit einfachen Abfrageparametern oder mit dem Anforderungshauptteil an die Aktion übergeben. Webaktionen können eine Aktion ohne Authentifizierung öffentlich aufrufen.

  ```
  {
  "payload": "Hello, Jane!"
  }
  ```
  {: screen}



## Uneingeschränkte Kontrolle über die HTTP-Antwort ausüben
{: #api_control}

Das Flag `--response-type` steuert die Ziel-URL der Webaktion, sodass sie durch das API-Gateway als Proxy geleitet wird. Wenn Sie zum Beispiel das Flag `--response-type json` verwenden, wird das vollständige Ergebnis der Aktion in JSON-Format zurückgegeben und der Header `Content-Type` wird automatisch auf den Wert `application/json` gesetzt.

Damit verschiedene Inhaltstypen im Hauptteil zurückgegeben werden, sollten Sie die volle Bandbreite an Kontrolle über die Eigenschaften in HTTP-Antworten wie `statusCode` und `headers` nutzen. Durch Verwenden des Flags `--response-type http` können Sie die Ziel-URL der Webaktion mit der Erweiterung `http` konfigurieren. Sie können den Code der Aktion so zu ändern, dass er mit der Rückgabe von Webaktionen mit der Erweiterung `http` konform ist, oder Sie können die Aktion in eine Sequenz einfügen, um ihr Ergebnis an eine neue Aktion zu übergeben. Die neue Aktion kann dann das Ergebnis in das ordnungsgemäße Format für eine HTTP-Antwort umsetzen. Weitere Informationen zu Antworttypen und Erweiterungen von Webaktionen finden Sie in der Dokumentation zu [Webaktionen](/docs/openwhisk?topic=cloud-functions-actions_web).

1. Speichern Sie den folgenden Code als `hello.js`. 
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello, ${name}!`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. Aktualisieren Sie Ihre Webaktion `hello` mit der neuen Version Ihres `hello.js`-Codes. 
  ```
  ibmcloud fn action update hello <filepath>/hello.js --web true
  ```
  {: pre}

  **Ausgabe**
  ```
  ok: updated action hello
  ```
  {: screen}

3. Aktualisieren Sie den API-Antworttyp mit dem Flag `--response-type http`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

  **Ausgabe**
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

4. Rufen Sie die aktualisierte API mit dem folgenden cURL-Befehl auf.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  **Beispielausgabe**
  ```
  {
  "payload": "Hello, Serverless API!"
  }
  ```
  {: screen}

## Konfiguration ändern
{: #api_modify_config}

Nachdem Sie Ihre Konfiguration erstellt haben, können Sie die [Registerkarte 'APIs'](https://cloud.ibm.com/openwhisk/apimanagement){: external} im {{site.data.keyword.openwhisk_short}}-Dashboard verwenden, um die Konfiguration wie folgt zu ändern. 

* [Erstellen Sie eine {{site.data.keyword.openwhisk_short}} API](/docs/services/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis), die eine Gruppe von {{site.data.keyword.openwhisk_short}}-Aktionen einschließt.
* [Schützen Sie Ihre API](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis) durch Anwenden von Richtlinien zur API-Sicherheit und zu Grenzwerten. 
* [Datenverkehr verwalten](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis): Verwalten Sie den Datenverkehr durch Anzeigen von API-Nutzungsstatistiken und Überprüfen von Antwortprotokollen.
* [Teilen und gemeinsam nutzen](/docs/services/api-management?topic=api-management-manage_apis#share_api_manage_apis): Nutzen Sie Ihre API mit Entwicklern innerhalb und außerhalb von {{site.data.keyword.cloud_notm}}.

</br>
Nachdem Sie die Konfiguration aktualisiert haben, können Sie die Definitionsdatei im JSON-Format herunterladen und sie anschließend über die CLI erneut importieren. Das Herunterladen und Importieren der Konfiguration kann beispielsweise bei einer unbeaufsichtigten Bereitstellung in einer CICD-Pipeline (CICD = Continuous Integration and Deployment) nützlich sein. Sie können die API-Definitionsdatei auch mithilfe der Benutzerschnittstelle hochladen und erneut importieren. 



