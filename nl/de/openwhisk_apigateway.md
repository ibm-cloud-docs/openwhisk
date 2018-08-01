---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Serverunabhängige REST-APIs erstellen
{: #openwhisk_apigateway}

{{site.data.keyword.openwhisk}}-Aktionen können durch Einführung des API-Gateways von der direkten Verwaltung durch APIs profitieren. Das API-Gateway fungiert als Proxy zu [Webaktionen](./openwhisk_webactions.html) und stattet diese mit zusätzlichen Funktionen aus. Zu diesen zusätzlichen Funktionen gehören zum Beispiel: HTTP-Methodenrouting, Client-IDs/geheime Client-Schlüssel, Ratenbegrenzungen, CORS, Anzeigen der API-Nutzung, Anzeigen von Antwortprotokollen und Definieren von Richtlinien zur gemeinsamen Nutzung von APIs. Weitere Informationen zum API-Management finden Sie in der [Dokumentation zum API-Management](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis).
{: shortdesc}

## APIs aus OpenWhisk-Webaktionen im Browser erstellen
{: #create_api_browser}

Sie können die [**Registerkarte 'APIs'**](https://console.bluemix.net/openwhisk/apimanagement) im [{{site.data.keyword.openwhisk_short}}-Dashboard](https://console.bluemix.net/openwhisk/) für die folgenden Tasks verwenden:

* [Cloud Functions-API erstellen](https://console.bluemix.net/openwhisk/apimanagement) - Sie können eine API erstellen, die eine Gruppe von OpenWhisk-Aktionen einschließt.
* [API schützen](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - Sie können API-Sicherheit und Richtlinien zur Ratenbegrenzung anwenden, um Ihre API zu schützen.
* [Datenverkehr verwalten](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - Sie können API-Nutzungsstatistiken anzeigen und Antwortprotokolle ausprobieren.
* [Teilen & gemeinsam nutzen](https://console.bluemix.net/docs/apis/management/manage_apis.html#share_api) - Sie können Ihre API mit Entwicklern innerhalb und außerhalb von {{site.data.keyword.Bluemix_notm}} gemeinsam nutzen.

## APIs aus OpenWhisk-Webaktionen mithilfe dem CLI-Plug-in erstellen
{: #create_api_cli}

Der folgende Abschnitt führt Sie durch die API-Verwaltungstasks unter Verwendung des {{site.data.keyword.openwhisk_short}}-CLI-Plug-ins. Um APIs über die CLI zu erstellen und zu verwalten, müssen Sie zuerst das [{{site.data.keyword.openwhisk_short}}-CLI-Plug-in](https://console.bluemix.net/docs/openwhisk/bluemix_cli.html) für {{site.data.keyword.Bluemix_notm}} installieren.

Für eine bessere Übersicht werden die Schritte in kleinere Unterthemen unterteilt, zu denen Sie über die folgende Liste von API-Tasks wechseln können:

* [Erste API erstellen](openwhisk_apigateway.html#create_cli_api)
* [Vollständige Kontrolle über die HTTP-Antwort](openwhisk_apigateway.html#full_control)
* [Mehrere Webaktionen verfügbar machen](openwhisk_apigateway.html#multiple_web_actions)
* [Konfiguration exportieren](openwhisk_apigateway.html#export_config)
* [Konfiguration importieren](openwhisk_apigateway.html#import_config)
* [Konfiguration ändern](openwhisk_apigateway.html#modify_config)

### Erste API über die CLI erstellen
{: #create_cli_api}

1. Erstellen Sie eine JavaScript-Datei mit dem Namen **hello.js** und mit folgendem Inhalt:
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. Erstellen Sie eine Webaktion mit dem Namen **hello** unter Verwendung der Datei `hello.js`, die in Schritt 1 erstellt wurde. **Hinweis:** Stellen Sie sicher, dass Sie das Flag `--web true` hinzufügen.
  ```
  ibmcloud fn action create hello hello.js --web true
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: created action hello
  ```
  {: screen}

3. Erstellen Sie eine API mit dem Basispfad `/hello` und dem Pfad `/world` sowie mit der Methode `get` und dem Antworttyp `json`:
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

  Eine neue URL wird generiert, über die die Aktion `hello` mit der HTTP-Methode __GET__ verfügbar gemacht wird.

4. Senden Sie zum Schluss unter Verwendung des Befehls **curl** eine HTTP-Anforderung an die URL:
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  Beispielausgabe:
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

Die Webaktion **hello** wird aufgerufen, die ein JSON-Objekt zurückgibt, das den Parameter **name** enthält, der durch den Abfrageparameter gesendet wurde. Sie können Parameter mit einfachen Abfrageparametern oder mit dem Anforderungshauptteil übergeben. Webaktionen können eine Aktion auf öffentlichem Wege ohne den API-Schlüssel für die OpenWhisk-Berechtigung aufrufen.

### Vollständige Kontrolle über die HTTP-Antwort
{: #full_control}

Das Flag `--response-type` steuert die Ziel-URL der Webaktion, sodass sie durch das API-Gateway als Proxy geleitet wird. Bei Verwendung des Flags `--response-type json` wird das vollständige Ergebnis der Aktion im JSON-Format zurückgegeben und der Inhaltstyp (Content-Type) des Headers automatisch auf `application/json` gesetzt.

Wenn Sie die vollständige Kontrolle über die Eigenschaften von HTTP-Antworten wie `statusCode` oder `headers` haben, können Sie verschiedene Inhaltstypen im Hauptteil (`body`) zurückgeben. Dazu können Sie das Flag `--response-type http` verwenden, das die Ziel-URL der Webaktion mit der Erweiterung `http` konfiguriert.

Sie haben die Möglichkeit, den Code der Aktion so zu ändern, dass er mit der Rückgabe von Webaktionen mit der Erweiterung `http` konform ist, oder Sie können die Aktion in eine Sequenz einfügen, um ihr Ergebnis an eine neue Aktion zu übergeben. Die neue Aktion kann dann das Ergebnis in das ordnungsgemäße Format für eine HTTP-Antwort umsetzen. Weitere Informationen zu Antworttypen und Erweiterungen von Webaktionen finden Sie in der Dokumentation zu [Webaktionen](./openwhisk_webactions.html).

1. Ändern Sie den Code für `hello.js`, sodass er die JSON-Eigenschaften `body`, `statusCode` und `headers` zurückgibt:
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

3. Aktualisieren Sie den API-Antworttyp mit dem Flag `--response-type http`:
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

4. Rufen Sie die aktualisierte API mit dem folgenden **curl**-Befehl auf:
  ```bash
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

Sie haben nun die vollständige Kontrolle über Ihre APIs und können den Inhalt steuern. Sie können zum Beispiel HTML zurückgeben oder den Statuscode für bestimmte Situationen wie 'Nicht gefunden' (404) oder 'Nicht berechtigt' (401) oder sogar 'Interner Fehler' (500) festlegen.

### Mehrere Webaktionen verfügbar machen
{: #multiple_web_actions}

Wenn Sie zum Beispiel eine Reihe von Aktionen für einen Buchclub für Freunde verfügbar machen möchten, können Sie eine Reihe von Aktionen verwenden, um das Back-End für den Buchclub zu implementieren:

| Aktion | HTTP-Methode | Beschreibung |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Abrufen von Buchdetails  |
| postBooks   | POST | Hinzufügen eines Buches |
| putBooks    | PUT | Aktualisieren von Buchdetails |
| deleteBooks | DELETE | Löschen eines Buches |

In diesem Beispiel wird die API mit einem **Pfadparameter** definiert. Wenn Sie Pfadparameter verwenden, muss die API mit dem Antworttyp `http` definiert werden. Der Pfadwert, der mit dem Basispfad beginnt und die tatsächlichen Pfadparameterwerte enthält, ist im Feld `__ow_path` des JSON-Parameters der Aktion verfügbar. Weitere Informationen finden Sie in der Dokumentation zum [HTTP-Kontext bei Webaktionen](./openwhisk_webactions.html#http-context). Dies umfasst Informationen zu weiteren HTTP-Kontextfeldern, die für Webaktionen verfügbar sind, die mit einem Antworttyp `http` aufgerufen werden.

1. Erstellen Sie nun eine API für den Buchclub mit dem Namen **Book Club** und dem HTTP-URL-Basispfad `/club`, `books` als zugehöriger Ressource und `{isbn}` als Pfadparameter, der verwendet wird, um ein bestimmtes Buch mithilfe der ISBN (International Standard Book Number) anzugeben.
  ```bash
  ibmcloud fn api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud fn api create /club /books get getBooks                       --response-type http
  ibmcloud fn api create /club /books post postBooks                     --response-type http
  ibmcloud fn api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud fn api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: codeblock}

  Beachten Sie, dass die erste mit dem Basispfad `/club` verfügbar gemachte Aktion die API-Bezeichnung mit dem Namen **Book Club** erhält. Alle weiteren unter `/club` verfügbar gemachten Aktionen sind nun **Book Club** zugeordnet.

2. Listen Sie mit dem folgenden Befehl alle **Book Club**-Aktionen auf, die verfügbar gemachten wurden:
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

3. Sie können zur Übung ein Buch mit dem Titel **JavaScript: The Good Parts** mit einer HTTP-Anforderung __POST__ hinzufügen:
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

4. Rufen Sie eine Liste mit Büchern ab, indem Sie die Aktion **getBooks** mit HTTP __GET__ verwenden:
  ```bash
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

5. Sie können ein bestimmtes Buch löschen, indem Sie die Aktion **deleteBooks** mit HTTP __DELETE__ verwenden. In diesem Beispiel ist `/club/books/978-0596517748` der Wert des Felds `__ow_path` der Aktion **deleteBooks**. Dabei ist `978-0596517748` der tatsächliche Wert `{isbn}` des Pfads.
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

### Konfiguration exportieren
{: #export_config}

1. Exportieren Sie die API mit dem Namen **Book Club** in eine Datei, die als Basis zum erneuten Erstellen der APIs mit einer Datei als Eingabe verwendet werden kann.
  ```
  ibmcloud fn api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. Testen Sie die Swagger-Datei, indem Sie zuerst alle verfügbar gemachten URLs unter einem gemeinsamen Basispfad mit dem folgenden Befehl löschen:
  ```
  ibmcloud fn api delete /club
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: deleted API /club
  ```
  {: screen}

  Sie können alle verfügbar gemachten URLs entweder über den Basispfad `/club` oder über die API-Namensbezeichnung **Book Club** löschen:
  {: tip}

### Konfiguration importieren
{: #import_config}

1. Stellen Sie jetzt die API **Book Club** mithilfe der Datei `club-swagger.json` wieder her:
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

2. Überprüfen Sie, ob die API **Book Club** erneut erstellt wurde:
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

### Konfiguration mithilfe der Benutzerschnittstelle ändern
{: #modify_config}

Sie können die Konfiguration im {{site.data.keyword.openwhisk_short}}-Dashboard bearbeiten. Klicken Sie dazu auf die Registerkarte [APIs](https://console.ng.bluemix.net/openwhisk/apimanagement), um die Sicherheit, die Ratenbegrenzungen und andere Eigenschaften festzulegen. Nachdem Sie die Konfiguration aktualisiert haben, können Sie die Definitionsdatei im JSON-Format herunterladen und sie anschließend über die CLI erneut importieren. Dies kann beispielsweise bei einer unbeaufsichtigten Bereitstellung in einer CICD-Pipeline (CICD = Continuous Integration and Deployment) nützlich sein. Sie haben auch die Möglichkeit, die API-Definitionsdatei unter Verwendung der Benutzerschnittstelle hochzuladen und erneut zu importieren.
