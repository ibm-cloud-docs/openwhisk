---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# OpenWhisk-REST-APIs verwenden
{: #openwhisk_rest_api}

Nachdem Ihre OpenWhisk-Umgebung aktiviert ist, können Sie OpenWhisk mit den Web-Apps oder mobilen Apps mit REST-API-Aufrufen verwenden.
{: shortdesc}

Weitere Informationen zu den APIs für Aktionen, Aktivierungen, Pakete, Regeln und Auslöser finden Sie in der [OpenWhisk-API-Dokumentation](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/openwhisk/openwhisk/master/core/controller/src/main/resources/apiv1swagger.json).


Alle Funktionen im System stehen über eine REST-API zur Verfügung. Sammlungs- und Entitätsendpunkte sind für Aktionen, Auslöser, Regeln, Pakete, Aktivierungen und Namensbereiche verfügbar.

Verfügbare Sammlungsendpunkte:
- `https://{APIHOST}/api/v1/namespaces`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/actions`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/triggers`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/rules`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/packages`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/activations`

`{APIHOST}` ist der OpenWhisk-API-Hostname (z. B. openwhisk.ng.bluemix.net, 172.17.0.1, 192.168.99.100, 192.168.33.13 usw.).
Für `{namespace}` kann das Zeichen `_` verwendet werden, um den *Standardnamensbereich* des Benutzers anzugeben.

Sie können eine GET-Anforderung für die Sammlungsendpunkte ausführen, um eine Liste der Entitäten in der Sammlung abzurufen.

Die folgenden Entitätsendpunkte sind für die einzelnen Typen von Entität verfügbar:
- `https://{APIHOST}/api/v1/namespaces/{namespace}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/actions/[{packageName}/]{actionName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/triggers/{triggerName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/rules/{ruleName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/packages/{packageName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/activations/{activationName}`

Die Endpunkte für Namensbereiche und Aktivierungen unterstützen GET-Anforderungen. Die Endpunkte für Aktionen, Auslöser, Regeln und Pakete unterstützen GET-, PUT- und DELETE-Anforderungen. Die Endpunkte für Aktionen, Auslöser und Regeln unterstützen auch POST-Anforderungen, die zum Aufrufen von Aktionen und Auslösern sowie zum Aktivieren und Inaktivieren von Regeln verwendet werden. 

Alle APIs sind mit der HTTP-Basisauthentifizierung geschützt.
Sie können das Tool [wskadmin ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/apache/incubator-openwhisk/tree/master/tools/admin) verwenden, um einen neuen Namensbereich und eine neue Authentifizierung zu generieren.
Die durch einen Doppelpunkt voneinander getrennten Basic-Berechtigungsnachweise zur Authentifizierung befinden sich in der Eigenschaft `AUTH` in Ihrer Datei `~/.wskprops`.
Sie können diese Berechtigungsnachweise auch über die Befehlszeilenschnittstelle (CLI) abrufen, indem Sie den Befehl `wsk property get --auth` ausführen.


Im folgenden Beispiel wird das Befehlstool [cURL](https://curl.haxx.se) verwendet, um eine Liste aller Pakete im Namensbereich `whisk.system` abzurufen.
```bash
curl -u USERNAME:PASSWORD https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/packages
```
{: pre}

```json
[
  {
    "name": "slack",
    "binding": false,
    "publish": true,
    "annotations": [
      {
        "key": "description",
        "value": "Package that contains actions to interact with the Slack messaging service"
      }
    ],
    "version": "0.0.1",
    "namespace": "whisk.system"
  }
]
```

In diesem Beispiel wurde die Authentifizierung mithilfe des Flags `-u` übergeben. Sie können diesen Wert auch als Teil der URL, zum Beispiel als `https://$AUTH@{APIHOST}`, übergeben.

Von der OpenWhisk-API werden Anforderung/Antwort-Aufrufe von Web-Clients unterstützt. Von OpenWhisk wird auf `OPTIONS`-Anforderungen mit CORS-Headern (CORS - Cross-Origin Resource Sharing) geantwortet. Derzeit sind alle Ursprünge zulässig (d. h. Access-Control-Allow-Origin ist "`*`") und Access-Control-Allow-Header sorgen für die Autorisierung und den Inhaltstyp.

**Achtung:** Da von OpenWhisk derzeit nur ein Schlüssel pro Namensbereich unterstützt wird, wird empfohlen, CORS nur für einfache Experimente und nicht darüber hinaus zu verwenden. Verwenden Sie [Webaktionen](./openwhisk_webactions.html) oder das [API-Gateway](./openwhisk_apigateway.html), um Ihre Aktionen der Öffentlichkeit zugänglich zu machen und nicht die OpenWhisk-Berechtigungsschlüssel für Clientanwendungen zu verwenden, für die CORS erforderlich ist.

## Ausführlichen CLI-Modus verwenden
{: #openwhisk_rest_api_cli_v}

Die OpenWhisk-Befehlszeilenschnittstelle (CLI) ist eine Schnittstelle zur OpenWhisk-REST-API.
Sie können die Befehlszeilenschnittstelle im ausführlichen Modus mit dem Flag `-v` ausführen. Dadurch werden alle Informationen zur HTTP-Anforderung und Antwort ausgegeben.

Zeigen Sie den Wert des Namensbereichs für den aktuellen Benutzer an, indem Sie den folgenden Befehl ausführen:
```
wsk namespace list -v
```
{: pre}

```
REQUEST:
[GET]	https://openwhisk.ng.bluemix.net/api/v1/namespaces
Req Headers
{
  "Authorization": [
    "Basic XXXYYYY"
  ],
  "User-Agent": [
    "OpenWhisk-CLI/1.0 (2017-08-10T20:09:30+00:00)"
  ]
}
RESPONSE:Got response with code 200
Resp Headers
{
  "Content-Type": [
    "application/json; charset=UTF-8"
  ]
}
Response body size is 28 bytes
Response body received:
["john@example.com_dev"]
```

Die ausgegebenen Informationen geben die Eigenschaften der HTTP-Anforderung an. Außerdem wird die HTTP-Methode `GET` für die URL `https://openwhisk.ng.bluemix.net/api/v1/namespaces` unter Verwendung eines User-Agent-Headers `OpenWhisk-CLI/1.0 (<CLI-Buildversion>)` und eines mit dem BASIC-Berechtigungsheaders `Basic XXXYYYY` ausgeführt.
Beachten Sie, dass der Berechtigungswert die base64-codierte OpenWhisk-Berechtigungszeichenfolge ist.
Die Antwort hat den Inhaltstyp `application/json`.

## Aktionen
{: #openwhisk_rest_api_actions}

Zum Erstellen oder Aktualisieren einer Aktion senden Sie eine HTTP-Anforderung mit der Methode `PUT` für die Aktionssammlung. Verwenden Sie beispielsweise den folgenden Befehl, um eine Aktion des Typs `nodejs:6` mit dem Namen `hello` mithilfe des Inhalts einer einzelnen Datei zu erstellen:
```bash
curl -u $AUTH -d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"}}' -X PUT -H "Content-Type: application/json" https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true 
```
{: pre}

Führen Sie zum Durchführen eines Blockierungsaufrufs für eine Aktion den folgenden Befehl aus, um eine HTTP-Anforderung mit der Methode `POST` und einem Hauptteil mit dem Eingabeparameter `name` zu senden:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?blocking=true \
-X POST -H "Content-Type: application/json" \
-d '{"name":"John"}'  
```
{: pre}

Sie erhalten die folgende Antwort:
```json
{
  "duration": 2,
  "name": "hello",
  "subject": "john@example.com_dev",
  "activationId": "c7bb1339cb4f40e3a6ccead6c99f804e",
  "publish": false,
  "annotations": [{
    "key": "limits",
    "value": {
      "timeout": 60000,
      "memory": 256,
      "logs": 10
    }
  }, {
    "key": "path",
    "value": "john@example.com_dev/hello"
  }],
  "version": "0.0.1",
  "response": {
    "result": {
      "payload": "Hello John"
    },
    "success": true,
    "status": "success"
  },
  "end": 1493327653769,
  "logs": [],
  "start": 1493327653767,
  "namespace": "john@example.com_dev"
}
```
Zum Abrufen der Antwort (`response.result`) führen Sie den Befehl erneut mit dem Abfrageparameter `result=true` wie im folgenden Beispiel aus:
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?blocking=true&result=true" \
-X POST -H "Content-Type: application/json" \
-d '{"name":"John"}' 
```
{: pre}
Sie erhalten die folgende Antwort:
```json
{
  "payload": "hello John"
}
```

## Annotationen und Webaktionen
{: #openwhisk_rest_api_webactions}

Zum Erstellen einer Aktion als Webaktion müssen Sie eine [Annotation](./openwhisk_annotations.html) des Typs `web-export=true` für Webaktionen hinzufügen. Da Webaktionen öffentlich zugänglich sind, sollten Sie vordefinierte Parameter mithilfe der Annotation `final=true` schützen (d. h., sie als final behandeln). Wenn Sie eine Aktion mithilfe des CLI-Flags `--web true` erstellen oder aktualisieren, werden mit diesem Befehl sowohl die Annotation `web-export=true` als auch die Annotation `final=true` hinzugefügt.

Führen Sie den curl-Befehl aus und geben Sie die vollständige Liste der Annotationen an, die für die Aktion festgelegt werden sollen:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}'
```
{: pre}

Sie können diese Aktion jetzt als öffentliche URL ohne OpenWhisk-Berechtigung aufrufen. Verwenden Sie dabei die öffentliche URL der Webaktion, einschließlich einer Erweiterung wie `.json` oder `.http`, am Ende der URL.
```bash
curl https://openwhisk.ng.bluemix.net/api/v1/web/john@example.com_dev/default/hello.json?name=John
```
{: pre}

```json
{
  "payload": "Hello John"
}
```

Dieser Beispielquellcode funktioniert nicht mit `.http`. Informationen über entsprechende Änderungen finden Sie in der Dokumentation zu [Webaktionen](./openwhisk_webactions.html).

## Sequenzen
{: #openwhisk_rest_api_sequences}

Zum Erstellen einer Aktionssequenz geben Sie die Namen der Aktionen, aus denen sich die Sequenz zusammensetzt, in der gewünschten Reihenfolge an. Dabei wird die Ausgabe der ersten Aktion als Eingabe an die nächste Aktion übergeben.

$ wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort

Erstellen Sie eine Sequenz mit den Aktionen `/whisk.system/utils/split` und `/whisk.system/utils/sort`.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/sequenceAction?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"sequenceAction","exec":{"kind":"sequence","components":["/whisk.system/utils/split","/whisk.system/utils/sort"]},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}' 
```
{: pre}

Wenn Sie die Namen der Aktionen angeben, müssen Sie sie vollständig qualifiziert angeben.

## Auslöser
{: #openwhisk_rest_api_triggers}

Um einen Auslöser zu erstellen, müssen Sie als Mindestanforderung einen Namen für den Auslöser angeben. Sie können auch Standardparameter einschließen, die bei Aktivierung des Auslösers durch eine Regel an die Aktion übergeben werden.

Erstellen Sie einen Auslöser mit dem Namen `events` mit einem Standardparameter `type`, für den der Wert `webhook` festgelegt ist.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/events?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"events","parameters":[{"key":"type","value":"webhook"}]}' 
```
{: pre}

Wann immer Sie ein Ereignis haben, das diesen Auslöser aktivieren muss, benötigen Sie jetzt nur eine HTTP-Anforderung mit einer Methode `POST`, die den OpenWhisk-Berechtigungsschlüssel verwendet.

Senden Sie die folgende HTTP-Anforderung, um den Auslöser `events` mit dem Parameter `temperature` zu aktivieren.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/events \
-X POST -H "Content-Type: application/json" \
-d '{"temperature":60}' 
```
{: pre}

### Auslöser mit Feedaktionen
{: #openwhisk_rest_api_triggers_feed}

Spezielle Auslöser können mithilfe einer Feedaktion erstellt werden. Die Feedaktion ist eine Aktion, mit der die Konfiguration eines Feedanbieters unterstützt wird, der beim Auftreten eines Ereignisses für den Auslöser für die Aktivierung des Auslösers verantwortlich ist. Weitere Informationen zu diesen Feedanbietern finden Sie in der Dokumentation zu Feeds [feeds.md].

Einige der verfügbaren Auslöser, die eine Feedaktion nutzen, sind regelmäßige Auslöser bzw. Alarme, Slack, Github, Cloudant/Couchdb und Message Hub/Kafka. Sie können auch eigene Feedaktionen und Feedanbieter erstellen.

Erstellen Sie einen Auslöser mit dem Namen `periodic` und einer festgelegten Häufigkeit, der alle zwei Stunden (z. B. 02:00:00, 04:00:00, ...) aktiviert werden soll.

Führen Sie den folgenden Befehl über die Befehlszeilenschnittstelle aus, um den Auslöser zu erstellen:
```bash
wsk trigger create periodic --feed /whisk.system/alarms/alarm \
  --param cron "0 */2 * * *" -v
```
{: pre}

Da das Flag `-v` verwendet wird, werden zwei HTTP-Anforderungen gesendet. Die eine dient zum Erstellen eines Auslösers mit dem Namen `periodic` und die andere zum Aufrufen der Feedaktion. An die Feedaktion `/whisk.system/alarms/alarm` werden die Parameter gesendet, um den Feedanbieter so zu konfigurieren, dass der Auslöser alle zwei Stunden aktiviert wird.

Wenn diese Aktivität mit der REST-API erzielt werden soll, erstellen Sie den Auslöser zunächst wie folgt:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"periodic","annotations":[{"key":"feed","value":"/whisk.system/alarms/alarm"}]}'  
```
{: pre}

Wie Sie sehen, wird die Annotation `Feed` in dem Auslöser gespeichert. Später kann über diese Annotation ermittelt werden, welche Feedaktion zum Löschen des Auslösers zu verwenden ist.

Nach dem Erstellen des Auslösers können Sie die Feedaktion aufrufen.
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"cron\":\"0 */2 * * *\",\"lifecycleEvent\":\"CREATE\",\"triggerName\":\"/_/periodic\"}" 
```
{: pre}

Das Löschen eines Auslösers ist dem Erstellen eines Auslösers ähnlich. Löschen Sie den Auslöser mithilfe der Feedaktion, um den Feedanbieter so konfigurieren, dass auch der Handler für den Auslöser gelöscht wird.

Rufen Sie die Feedaktion zum Löschen des Auslösehandlers über den Feedanbieter mithilfe des folgenden Befehls auf:
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"lifecycleEvent\":\"DELETE\",\"triggerName\":\"/_/periodic\"}"  
```
{: pre}

Löschen Sie nun den Auslöser mit einer HTTP-Anforderung mit der Methode `DELETE`:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic \
-X DELETE -H "Content-Type: application/json" 
```
{: pre}

## Regeln
{: #openwhisk_rest_api_rules}

Zum Erstellen einer Regel, die einen Auslöser einer Aktion zuordnet, senden Sie eine HTTP-Anforderung mit einer Methode `PUT`, die den Auslöser und die Aktion im Hauptteil der Anforderung angibt.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"t2a","status":"","trigger":"/_/events","action":"/_/hello"}' 
```
{: pre}

Regeln können aktiviert oder inaktiviert werden und Sie können den Status der Regel durch eine Aktualisierung der Statuseigenschaft ändern. Um beispielsweise die Regel `t2a` zu inaktivieren, senden Sie `status: "inactive"` mit einer Methode `POST` im Hauptanteil der Anforderung.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X POST -H "Content-Type: application/json" \
-d '{"status":"inactive","trigger":null,"action":null}'  
```
{: pre}

## Pakete
{: #openwhisk_rest_api_packages}

Zum Erstellen einer Aktion in einem Paket müssen sie zunächst ein Paket erstellen. Erstellen Sie ein Paket mit dem Namen `iot` und senden Sie eine HTTP-Anforderung mit einer Methode `PUT` an dieses Paket, indem Sie den folgenden Befehl verwenden:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/packages/iot?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"iot"}' 
```
{: pre}

## Aktivierungen
{: #openwhisk_rest_api_activations}

Zum Abrufen der Liste der letzten drei Aktivierungen verwenden Sie wie folgt eine HTTP-Anforderung mit einer Methode `GET` und übergeben Sie den Abfrageparameter `limit=3`:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations?limit=3
```
{: pre}

Zum Abrufen aller Details einer Aktivierung, die Ergebnisse und Protokolle mit einschließen, senden Sie wie folgt eine HTTP-Anforderung mit einer Methode `GET` und übergeben Sie die Aktivierungskennung als Pfadparameter:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations/f81dfddd7156401a8a6497f2724fec7b
```
{: pre}
