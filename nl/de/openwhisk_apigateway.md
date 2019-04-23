---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-04"

keywords: serverless, rest api, gateway, web actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Serverunabhängige REST-APIs erstellen
{: #openwhisk_apigateway}

Mit APIs können Sie {{site.data.keyword.openwhisk}}-Aktionen direkt verwalten. Das API-Gateway fungiert als Proxy für [Webaktionen](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions) und stellt HTTP-Methodenrouting, Client-IDs und geheime Schlüssel, Ratenbegrenzungen, CORS, Anzeigen der API-Nutzung, Anzeigen von Antwortprotokollen und Definieren von Richtlinien zur gemeinsamen Nutzung von APIs bereit.
{: shortdesc}

Weitere Informationen zum API-Management finden Sie in der [Dokumentation zum API-Management](/docs/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis).



## Erste API erstellen
{: #create_cli_api}

Bevor Sie beginnen, installieren Sie das [{{site.data.keyword.openwhisk_short}} CLI-Plug-in](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli).

1. Speichern Sie den folgenden Code in einer JavaScript-Datei namens `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. Erstellen Sie mithilfe der soeben erstellten Datei eine Webaktion namens `hello`. **Hinweis:** Stellen Sie sicher, dass Sie das Flag `--web true` hinzufügen.
  ```
  ibmcloud fn action create hello hello.js --web true
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: created action hello
  ```
  {: screen}

3. Erstellen Sie eine API mit dem Basispfad `/hello` und dem Pfad `/world` sowie mit der Methode `get` und dem Antworttyp `json`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  Es wird eine neue URL generiert, über die die Aktion `hello` mit der HTTP-Methode 'GET' verfügbar gemacht wird.

4. Senden Sie unter Verwendung des Befehls 'cURL' eine Test-HTTP-Anforderung an die URL.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  Beispielausgabe:
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

Es wird die Webaktion `hello` aufgerufen. Diese gibt ein JSON-Objekt zurück, das den Parameter **name** im Abfrageparameter enthält. Sie können Parameter mit einfachen Abfrageparametern oder mit dem Anforderungshauptteil an die Aktion übergeben. Webaktionen können eine Aktion ohne Authentifizierung öffentlich aufrufen. 

## Uneingeschränkte Kontrolle über die HTTP-Antwort ausüben
{: #full_control}

Das Flag `--response-type` steuert die Ziel-URL der Webaktion, sodass sie durch das API-Gateway als Proxy geleitet wird. Wenn Sie zum Beispiel das Flag `--response-type json` verwenden, wird das vollständige Ergebnis der Aktion in JSON-Format zurückgegeben und der Header **Content-Type** wird automatisch auf den Wert `application/json` gesetzt.

Damit verschiedene Inhaltstypen im Hauptteil zurückgegeben werden, sollten Sie die volle Bandbreite an Kontrolle über die Eigenschaften in HTTP-Antworten wie **statusCode** und **headers** nutzen. Durch Verwenden des Flags `--response-type http` können Sie die Ziel-URL der Webaktion mit der Erweiterung `http` konfigurieren. Sie können den Code der Aktion so zu ändern, dass er mit der Rückgabe von Webaktionen mit der Erweiterung `http` konform ist, oder Sie können die Aktion in eine Sequenz einfügen, um ihr Ergebnis an eine neue Aktion zu übergeben. Die neue Aktion kann dann das Ergebnis in das ordnungsgemäße Format für eine HTTP-Antwort umsetzen. Weitere Informationen zu Antworttypen und Erweiterungen von Webaktionen finden Sie in der Dokumentation zu [Webaktionen](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions).

1. Ändern Sie den Code für die Aktion `hello.js` so, dass er die JSON-Eigenschaften `body`, `statusCode` und `headers` zurückgibt.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello world ${name}`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. Aktualisieren Sie die Aktion mit dem geänderten Ergebnis.
  ```
  ibmcloud fn action update hello hello.js --web true
  ```
  {: pre}

3. Aktualisieren Sie den API-Antworttyp mit dem Flag `--response-type http`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

4. Rufen Sie die aktualisierte API mit dem folgenden cURL-Befehl auf.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  Beispielausgabe:
  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  {: screen}

## Mehrere Webaktionen verfügbar machen
{: #multiple_web_actions}

Es ist möglich, zum Bereitstellen des Back-Ends Ihrer App mehrere Webaktionen verfügbar zu machen. Wenn zum Beispiel eine Reihe von Aktionen für einen Buchclub verfügbar gemacht werden soll, können Sie eine Reihe von Aktionen verwenden, um das Back-End für den Buchclub zu implementieren:

| Aktion | HTTP-Methode | Beschreibung |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Abrufen von Buchdetails  |
| postBooks   | POST | Hinzufügen eines Buches |
| putBooks    | PUT | Aktualisieren von Buchdetails |
| deleteBooks | DELETE | Löschen eines Buches |

In diesem Beispiel wird die API mit einem Pfadparameter definiert. Wenn Sie Pfadparameter verwenden, muss die API mit dem Antworttyp `http` definiert werden. Der Pfadwert, der mit dem Basispfad beginnt und die tatsächlichen Pfadparameterwerte enthält, ist im Feld `__ow_path` des JSON-Parameters der Aktion verfügbar. Weitere Details zu HTTP-Kontextfeldern enthält die Dokumentation zum [HTTP-Kontext bei Webaktionen](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#http-context).

Gehen Sie wie folgt vor, um das Buchclub-Beispiel für Webaktionen auszuprobieren:

1. Erstellen Sie für den Buchclub eine API namens `Book Club` mit dem HTTP-URL-Basispfad `/club`, der zugehörigen Ressource `books` und dem Pfadparameter `{isbn}`, der verwendet wird, um ein bestimmtes Buch anhand seiner ISBN (International Standard Book Number) anzugeben.
  ```
  ibmcloud fn api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud fn api create /club /books get getBooks                       --response-type http
  ibmcloud fn api create /club /books post postBooks                     --response-type http
  ibmcloud fn api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud fn api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: pre}

  Die erste mit dem Basispfad `/club` verfügbar gemachte Aktion erhält den Namen `Book Club`. Alle weiteren unter `/club` verfügbar gemachten Aktionen sind nun `Book Club` zugeordnet.

2. Listen Sie alle `Book Club`-Aktionen auf, die verfügbar gemacht wurden.
  ```
  ibmcloud fn api list /club -f
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: APIs
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: postBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: post
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: putBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: put
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: deleteBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: delete
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

3. Fügen Sie ein Buch mit dem Titel `JavaScript: The Good Parts` hinzu, indem Sie eine entsprechende HTTP-Anforderung vom Typ POST verwenden.
  ```
  curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' -H "Content-Type: application/json" https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  Beispielausgabe:
  ```
  {
    "result": "success"
  }
  ```
  {: screen}

4. Rufen Sie eine Liste von Büchern ab. Führen Sie dazu einen HTTP-Aufruf vom Typ GET für die Aktion `getBooks` aus.
  ```
  curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  Beispielausgabe:
  ```
  {
    "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
  }
  ```
  {: screen}

5. Löschen Sie ein bestimmtes Buch. Verwenden Sie dazu einen HTTP-Aufruf vom Typ DELETE für die Aktion `deleteBooks`. In diesem Beispiel ist `/club/books/978-0596517748` der Wert des Felds `__ow_path` der Aktion `deleteBooks`. Dabei ist `978-0596517748` der tatsächliche Wert `{isbn}` des Pfads.
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

## Konfiguration exportieren und importieren
{: #export_import_config}

Zum Exportieren oder Importieren einer Konfiguration können Sie weiterhin das Buchclub-Beispiel verwenden.

1. Exportieren Sie die `Book Club`-API in eine Datei namens `club-swagger.json`. Diese Datei kann als Basis zum erneuten Erstellen der APIs mit einer Datei als Eingabe verwendet werden.
  ```
  ibmcloud fn api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. Testen Sie die Swagger-Datei, indem Sie zuerst alle unter einem gemeinsamen Basispfad verfügbar gemachten URLs löschen.
  ```
  ibmcloud fn api delete /club
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: deleted API /club
  ```
  {: screen}

  Sie können alle verfügbar gemachten URLs unter Verwendung des Basispfads `/club` oder aber unter Verwendung der API-Namensbezeichnung `Book Club` löschen.
  {: tip}

3. Stellen Sie die `Book Club`-API mithilfe der Datei `club-swagger.json` wieder her.
  ```
  ibmcloud fn api create --config-file club-swagger.json
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: created api /club/books/{isbn} get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} put for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} delete for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books post for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: screen}

4. Überprüfen Sie, ob die API `Book Club` erneut erstellt wurde.
  ```
  ibmcloud fn api list /club
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: apis
  Action                    Verb         API Name        URL
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

## Konfiguration ändern
{: #modify_config}

Nachdem Sie Ihre Konfiguration erstellt haben, können Sie sie im {{site.data.keyword.openwhisk_short}}-Dashboard über die [**Registerkarte 'APIs'**](https://cloud.ibm.com/openwhisk/apimanagement) auf die folgenden Arten ändern.

* [Erstellen Sie eine {{site.data.keyword.openwhisk_short}} API](https://cloud.ibm.com/docs/services/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis), die eine Gruppe von {{site.data.keyword.openwhisk_short}}-Aktionen einschließt. 
* [API sichern](https://cloud.ibm.com/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis): Sichern Sie Ihre API durch Anwendung von Richtlinien für die API-Sicherheit und die Ratenbegrenzung.
* [Datenverkehr verwalten](https://cloud.ibm.com/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis): Verwalten Sie den Datenverkehr durch Anzeigen von API-Nutzungsstatistiken und Überprüfen von Antwortprotokollen.
* [Teilen und gemeinsam nutzen](https://cloud.ibm.com/docs/services/api-management?topic=api-management-manage_apis#share_api_manage_apis): Nutzen Sie Ihre API mit Entwicklern innerhalb und außerhalb von {{site.data.keyword.Bluemix_notm}}.

Nachdem Sie die Konfiguration aktualisiert haben, können Sie die Definitionsdatei im JSON-Format herunterladen und sie anschließend über die CLI erneut importieren. Das Herunterladen und Importieren der Konfiguration kann beispielsweise bei einer unbeaufsichtigten Bereitstellung in einer CICD-Pipeline (CICD = Continuous Integration and Deployment) nützlich sein. Sie haben auch die Möglichkeit, die API-Definitionsdatei unter Verwendung der Benutzerschnittstelle hochzuladen und erneut zu importieren.
