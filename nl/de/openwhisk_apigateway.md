---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# API-Gateway
{: #openwhisk_apigateway}

OpenWhisk-Aktionen können von der Verwaltung durch das API-Management profitieren.

Das API-Gateway fungiert als Proxy zu [Webaktionen](./openwhisk_webactions.html) und stattet diese mit zusätzlichen Funktionen aus. Zu diesen zusätzlichen Funktionen gehören zum Beispiel: HTTP-Methodenrouting, Client-IDs/geheime Client-Schlüssel, Ratenbegrenzungen, CORS, Anzeigen der API-Nutzung, Anzeigen von Antwortprotokollen und Definieren von Richtlinien zur gemeinsamen Nutzung von APIs.
Weitere Informationen zum API-Management finden Sie in der [Dokumentation zum API-Management](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis).
{: shortdesc}

## APIs aus OpenWhisk-Webaktionen im Browser erstellen

Mithilfe des API-Gateways können Sie eine OpenWhisk-Aktion als API verfügbar machen. Wenn Sie die API definiert haben, können Sie Sicherheits- und Ratenbegrenzungsrichtlinien anwenden, die API-Nutzung und Antwortprotokolle anzeigen und Richtlinien zur gemeinsamen Nutzung der API definieren.
Klicken Sie im OpenWhisk-Dashboard auf die Registerkarte [APIs](https://console.ng.bluemix.net/openwhisk/apimanagement).


## APIs aus OpenWhisk-Webaktionen mithilfe der Befehlszeilenschnittstelle (CLI) erstellen

### OpenWhisk-CLI-Konfiguration

Konfigurieren Sie die OpenWhisk-Befehlszeilenschnittstelle mit dem API-Host.

Es sind zwei {{site.data.keyword.Bluemix_notm}}-Regionen verfügbar, für die jeweils ein eigener eindeutiger API-Host und Berechtigungsschlüssel erforderlich sind.

* Vereinigte Staaten (Süden)
  * API-Host: `openwhisk.ng.bluemix.net`

* Vereintes Königreich
  * API-Host: `openwhisk.eu-gb.bluemix.net`

Führen Sie den folgenden Befehl aus, um den API-Host für die gewünschte Bluemix-Region festzulegen:

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

Zur Verwendung des Befehls `wsk api` muss die Konfigurationsdatei `~/.wskprops` der Befehlszeilenschnittstelle das Bluemix-Zugriffstoken enthalten.

Verwenden Sie den folgenden Befehl der Befehlszeilenschnittstelle, um das Zugriffstoken abzurufen:
```
wsk bluemix login
```
{: pre}

Führen Sie den folgenden Befehl aus, um weitere Informationen zu diesem Befehl aufzurufen:
```
wsk bluemix login -h
```
{: pre}

Wenn der Befehl `wsk bluemix login` mit dem Fehler `BMXLS0202E: You are using a federated user ID, please use one time code to login with option --sso` fehlschlägt, melden Sie sich mit dem Befehl `bluemix login` bei der {{site.data.keyword.Bluemix_notm}}-CLI an und führen Sie den Befehl `wsk bluemix login --sso` aus.
{: tip}

### Erste API über die CLI erstellen

1. Erstellen Sie eine JavaScript-Datei mit dem folgenden Inhalt. Für dieses Beispiel wird der Dateiname 'hello.js' verwendet.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}
  
2. Erstellen Sie wie folgt eine Webaktion aus der JavaScript-Funktion. Für dieses Beispiel heißt die Aktion 'hello'. Stellen Sie sicher, dass Sie das Flag `--web true` hinzufügen.
  ```
  wsk action create hello hello.js --web true
  ```
  {: pre}

  ```
  ok: created action hello
  ```
  
3. Erstellen Sie eine API mit dem Basispfad `/hello` und dem Pfad `/world` sowie mit der Methode `get` und dem Antworttyp `json`:
  ```
  wsk api create /hello /world get hello --response-type json
  ```

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  Eine neue URL wird generiert, über die die Aktion `hello` mit der HTTP-Methode __GET__ verfügbar gemacht wird.
  
4. Senden Sie zum Schluss eine HTTP-Anforderung an die URL.
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world?name=OpenWhisk
  ```

  ```json
  {
  "payload": "Hello world OpenWhisk"
  }
  ```

  Die Webaktion `hello` wird aufgerufen, die ein JSON-Objekt zurückgibt, das den Parameter `name` enthält, der durch einen Abfrageparameter gesendet wurde. Sie können Parameter durch einfache Abfrageparameter oder im Anforderungshauptteil übergeben. Webaktionen können eine Aktion auf öffentlichem Wege ohne den API-Schlüssel für die OpenWhisk-Berechtigung aufrufen.
  
### Vollständige Kontrolle über die HTTP-Antwort
  
  Das Flag `--response-type` steuert die Ziel-URL der Webaktion, sodass sie durch das API-Gateway als Proxy geleitet wird. Bei Verwendung des Flags `--response-type json` wird das vollständige Ergebnis der Aktion im JSON-Format zurückgegeben und der Inhaltstyp (Content-Type) des Headers automatisch auf `application/json` gesetzt. 
  
  Nach dem Einstieg in die Arbeit mit Webaktionen möchten Sie vollständige Kontrolle über die Eigenschaften von HTTP-Antworten wie `statusCode` oder `headers` haben und verschiedene Inhaltstypen im Hauptteil (`body`) zurückgeben. Dazu können Sie das Flag `--response-type http` verwenden, das die Ziel-URL der Webaktion mit der Erweiterung `http` konfiguriert.

  Sie haben die Möglichkeit, den Code der Aktion so zu ändern, dass er mit der Rückgabe von Webaktionen mit der Erweiterung `http` konform ist, oder Sie können die Aktion in eine Sequenz einfügen, um ihr Ergebnis an eine neue Aktion zu übergeben. Die neue Aktion kann dann das Ergebnis in das ordnungsgemäße Format für eine HTTP-Antwort umsetzen. Weitere Informationen zu Antworttypen und Erweiterungen von Webaktionen finden Sie in der Dokumentation zu [Webaktionen](./openwhisk_webactions.html).

  Ändern Sie den Code für `hello.js`, sodass er die JSON-Eigenschaften `body`, `statusCode` und `headers` zurückgibt.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: new Buffer(JSON.stringify({payload:`Hello world ${name}`})).toString('base64'), 
        statusCode:200, 
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}
  Beachten Sie, dass der Hauptteil 'body' in `base64` codiert und nicht als Zeichenfolge ('string') zurückgegeben werden muss.
  
  Aktualisieren Sie die Aktion mit dem geänderten Ergebnis. 
  ```
  wsk action update hello hello.js --web true
  ```
  {: pre}

  Aktualisieren Sie die API mit dem Flag `--response-type http`: 
  ```
  wsk api create /hello /world get hello --response-type http
  ```
  {: pre}
  
  Rufen Sie die aktualisierte API auf: 
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  {: pre}

  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  Sie haben nun die vollständige Kontrolle über Ihre APIs und können den Inhalt steuern. Sie können zum Beispiel HTML zurückgeben oder den Statuscode für bestimmte Situationen wie 'Nicht gefunden' (404) oder 'Nicht berechtigt' (401) oder sogar 'Interner Fehler' (500) festlegen.

### Mehrere Webaktionen verfügbar machen

Wenn Sie zum Beispiel eine Reihe von Aktionen für einen Buchclub für Freunde verfügbar machen möchten, können Sie eine Reihe von Aktionen verwenden, um das Back-End für den Buchclub zu implementieren:

| Aktion | HTTP-Methode | Beschreibung |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Abrufen von Buchdetails |
| postBooks   | POST | Hinzufügen eines Buches |
| putBooks    | PUT | Aktualisieren von Buchdetails |
| deleteBooks | DELETE | Löschen eines Buches |

Erstellen Sie nun eine API für den Buchclub mit dem Namen `Book Club` und dem HTTP-URL-Basispfad `/club` und `books` als zugehöriger Ressource.
```
wsk api create -n "Book Club" /club /books get getBooks --response-type http
wsk api create /club /books post postBooks              --response-type http
wsk api create /club /books put putBooks                --response-type http
wsk api create /club /books delete deleteBooks          --response-type http
```

Beachten Sie, dass die erste mit dem Basispfad `/club` verfügbar gemachte Aktion die API-Bezeichnung mit dem Namen `Book Club` erhält. Alle weiteren unter `/club` verfügbar gemachten Aktionen werden `Book Club` zugeordnet.

Listen Sie mit dem folgenden Befehl alle Aktionen auf, die verfügbar gemachten wurden:
```
wsk api list -f
```
{: pre}

```
ok: APIs
Action: getBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: get
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: postBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: post
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: putBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: put
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: deleteBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: delete
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

Sie können spaßeshalber ein Buch mit dem Titel `JavaScript: The Good Parts` mit einer HTTP-Anforderung __POST__ hinzufügen:
```
curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": "success"
}
```

Rufen Sie eine Liste ab, indem Sie die Aktion `getBooks` über eine HTTP-Anforderung __GET__ verwenden:
```
curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
}
```

### Konfiguration exportieren
Exportieren Sie die API mit dem Namen `Book Club` in eine Datei, die als Basis zum erneuten Erstellen der APIs mit einer Datei als Eingabe verwendet werden kann. 
```
wsk api get "Book Club" > club-swagger.json
```
{: pre}

Testen Sie die Swagger-Datei, indem Sie zuerst alle verfügbar gemachten URLs unter einem gemeinsamen Basispfad löschen.
Sie können alle verfügbar gemachten entweder über den Basispfad `/club` oder über die API-Namensbezeichnung `Book Club` löschen:
```
wsk api delete /club
```
```
ok: deleted API /club
```
### Konfiguration ändern

Sie können die Konfiguration im OpenWhisk-Dashboard bearbeiten. Klicken Sie dazu auf die Registerkarte [APIs](https://console.ng.bluemix.net/openwhisk/apimanagement), um die Sicherheit, die Ratenbegrenzungen und andere Eigenschaften festzulegen.

### Konfiguration importieren

Stellen Sie jetzt die API `Book Club` mithilfe der Datei `club-swagger.json` wieder her.
```
wsk api create --config-file club-swagger.json
```
{: pre}

```
ok: created api /books delete for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books get for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books post for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books put for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

Überprüfen Sie, ob die API erneut erstellt wurde:
```
wsk api list /club
```
{: pre}

```
ok: apis
Action                    Verb         API Name        URL
getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
