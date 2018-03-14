---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Message Hub-Paket verwenden
{: #openwhisk_catalog_message_hub}

Ein Paket, das die Kommunikation mit [Message Hub-Instanzen](https://developer.ibm.com/messaging/message-hub) zur Veröffentlichung und Verarbeitung von Nachrichten unter Verwendung der nativen hochleistungsfähigen Kafka-API ermöglicht.
{: shortdesc}

## Auslöser erstellen, der für eine IBM Message Hub-Instanz empfangsbereit ist
{: #openwhisk_catalog_message_hub_trigger}

Zum Erstellen eines Auslösers, der reagiert, wenn Nachrichten an eine Message Hub-Instanz gesendet werden, müssen Sie den Feed `/messaging/messageHubFeed` verwenden. Diese Feedaktion unterstützt die folgenden Parameter:

|Name|Typ|Beschreibung|
|---|---|---|
|kafka_brokers_sasl|JSON-Array aus Zeichenfolgen|Dieser Parameter ist ein Array aus Zeichenfolgen im Format `<host>:<port>`, die die Broker in Ihrer Message Hub-Instanz umfassen.|
|user|Zeichenfolge|Ihr Message Hub-Benutzername|
|password|Zeichenfolge|Ihr Message Hub-Kennwort|
|topic|Zeichenfolge|Das Thema, für das der Auslöser empfangsbereit sein soll|
|kafka_admin_url|URL-Zeichenfolge|Die URL der Message Hub-REST-Schnittstelle für Administratoren|
|isJSONData|Boolesch (optional: Standard=false)|Wenn dieser Parameter auf `true` gesetzt wird, versucht der Provider, den Nachrichtenwert als JSON-Daten zu parsen, bevor er ihn als Nutzdaten für den Auslöser weitergibt.|
|isBinaryKey|Boolesch (optional: Standard=false)|Wenn dieser Parameter auf `true` gesetzt wird, codiert der Provider den Schlüsselwert in Base64, bevor er ihn als Nutzdaten für den Auslöser weitergibt.|
|isBinaryValue|Boolesch (optional: Standard=false)|Wenn dieser Parameter auf `true` gesetzt wird, codiert der Provider den Nachrichtenwert in Base64, bevor er ihn als Nutzdaten für den Auslöser weitergibt.|

Diese Parameterliste kann zwar abschreckend wirken, jedoch können die Parameter automatisch mit dem CLI-Befehl zum Aktualisieren des Pakets für Sie festgelegt werden:

1. Erstellen Sie eine Instanz des Message Hub-Service unter Ihrer aktuellen Organisation und dem aktuellen Bereich, die bzw. den Sie für OpenWhisk verwenden.

2. Stellen Sie sicher, dass das zu überwachende Thema in Message Hub verfügbar ist, oder erstellen Sie ein neues Thema. Beispiel: `mytopic`.

3. Aktualisieren Sie die Pakete in Ihrem Namensbereich. Die Aktualisierung erstellt automatisch eine Paketbindung für die Message Hub-Serviceinstanz, die Sie erstellt haben.

  ```
  wsk package refresh
  ```
  {: pre}
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```

  Ihre Paketbindung enthält nun die Ihrer Message Hub-Instanz zugeordneten Berechtigungsnachweise.

4. Nun müssen Sie nur noch einen Auslöser erstellen, der aktiviert wird, wenn neue Nachrichten an Ihr Message Hub-Thema gesendet werden.

  ```
  wsk trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## Message Hub-Paket außerhalb von {{site.data.keyword.Bluemix_notm}} einrichten

Wenn Sie Message Hub außerhalb von {{site.data.keyword.Bluemix_notm}} einrichten wollen, müssen Sie manuell eine Paketbindung für Ihren Message Hub-Service erstellen. Dazu benötigen Sie die Berechtigungsnachweise für den Message Hub-Service und die Verbindungsinformationen.

1. Erstellen Sie eine Paketbindung, die für Ihren Message Hub-Service konfiguriert ist.

  ```
  wsk package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your Message Hub user> -p password <your Message Hub password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. Jetzt können Sie einen Auslöser mit Ihrem neuen Paket erstellen, der aktiviert wird, wenn neue Nachrichten an Ihr Message Hub-Thema gesendet werden.

  ```
  wsk trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}


## Nachrichten überwachen
{: #openwhisk_catalog_message_hub_listen}

Nach dem Erstellen eines Auslösers überwacht das System das angegebene Thema in Ihrem Messaging-Service. Wenn neue Nachrichten gesendet werden, wird der Auslöser aktiviert.

Die Nutzdaten dieses Auslösers enthalten das Feld `messages`. Dabei handelt es sich um ein Array aus Nachrichten, die seit der letzten Aktivierung des Auslösers gesendet wurden. Jedes Nachrichtenobjekt im Array enthält die folgenden Felder:
- topic
- partition
- offset
- key
- value

In Bezug auf Kafka sind diese Felder selbsterklärend. Allerdings ist das Feld `key` mit einer Funktion `isBinaryKey` ausgestattet, die die Übertragung von binären Daten im Feld `key` ermöglicht. Außerdem erfordert das Feld `value` besondere Aufmerksamkeit. Die Felder `isJSONData` und `isBinaryValue` sind verfügbar, um JSON- und Binärnachrichten zu verarbeiten. Diese Felder (d. h. `isJSONData` und `isBinaryValue`) können nicht zusammen verwendet werden.

Beispiel: Wenn für `isBinaryKey` der Wert `true` bei der Erstellung des Auslösers festgelegt wurde, wird das Feld `key` bei der Rückgabe aus den Nutzdaten eines aktivierten Auslösers als Base64-Zeichenfolge codiert.

Beispiel: Wenn ein Feld `key` mit dem Wert `Some key` gesendet wird und für `isBinaryKey` der Wert `true` festgelegt ist, sehen die Nutzdaten des Auslösers in etwa folgendermaßen aus:

```json
{
    "messages": [
        {
            "partition": 0,
            "key": "U29tZSBrZXk=",
            "offset": 421760,
            "topic": "mytopic",
            "value": "Some value"
        }
    ]
}
```

Wenn der Parameter `isJSONData` bei der Erstellung des Auslösers auf den Wert `false` (oder gar nicht) festgelegt wurde, enthält das Feld `value` den Rohwert der gesendeten Nachricht. Wurde `isJSONData` bei der Erstellung des Auslösers jedoch auf `true` festgelegt, versucht das System, diesen Wert nach Möglichkeit als JSON-Objekt zu parsen. Wenn das Parsing erfolgreich war, enthält das Feld `value` in den Nutzdaten des Auslösers das resultierende JSON-Objekt.

Wenn eine Nachricht mit dem Inhalt `{"title": "Some string", "amount": 5, "isAwesome": true}` gesendet wird und dabei `isJSONData` auf `true` gesetzt ist, sehen die Nutzdaten für den Auslöser in etwa wie im folgenden Beispiel aus:

```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": {
          "amount": 5,
          "isAwesome": true,
          "title": "Some string"
      }
    }
  ]
}
```

Wird jedoch derselbe Nachrichteninhalt mit dem Wert `false` für `isJSONData` gesendet, sehen die Nutzdaten für den Auslöser wie im folgenden Beispiel aus:

```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421761,
      "topic": "mytopic",
      "value": "{\"title\": \"Some string\", \"amount\": 5, \"isAwesome\": true}"
    }
  ]
}
```

Analog zu `isJSONData` gilt: Wenn für das Feld `isBinaryValue` bei der Erstellung des Auslösers der Wert `true` festgelegt wurde, wird das resultierende Feld `value` in den Nutzdaten des Auslösers als Base64-Zeichenfolge codiert.

Wenn ein Feld `value` mit dem Wert `Some data` gesendet wird und für `isBinaryValue` der Wert `true` festgelegt ist, sehen die Nutzdaten des Auslösers in etwa wie im folgenden Beispiel aus:

```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": "U29tZSBkYXRh"
    }
  ]
}
```

Wird dieselbe Nachricht gesendet, ohne dass für `isBinaryData` der Wert `true` festgelegt ist, ähneln die Nutzdaten des Auslösers dem folgenden Beispiel:

```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": "Some data"
    }
  ]
}
```

### Nachrichten werden stapelweise verarbeitet
Beachten Sie, dass die Nutzdaten für den Auslöser ein Array aus Nachrichten enthalten. Wenn diese Nachrichten sehr schnell für Ihr Messaging-System erstellt werden, versucht der Feed, die gesendeten Nachrichten in einem Stapel zusammenzufassen, der nur eine Aktivierung des Auslösers zur Folge hat. Durch diese Stapelverarbeitung können die Nachrichten schneller und effizienter an Ihren Auslöser gesendet werden.

Berücksichtigen Sie beim Codieren von Aktionen, die von Ihrem Auslöser gestartet werden, dass die Anzahl der Nachrichten in den Nutzdaten technisch unbegrenzt ist, aber immer größer als 0 ist. Das folgende Beispiel zeigt eine in einem Stapel erfasste Nachricht (beachten Sie die Änderungen im Wert *offset*):
 
 ```json
 {
   "messages": [
       {
         "partition": 0,
         "key": null,
         "offset": 100,
         "topic": "mytopic",
         "value": {
             "amount": 5
         }
       },
       {
         "partition": 0,
         "key": null,
         "offset": 101,
         "topic": "mytopic",
         "value": {
             "amount": 1
         }
       },
       {
         "partition": 0,
         "key": null,
         "offset": 102,
         "topic": "mytopic",
         "value": {
             "amount": 999
         }
       }
   ]
 }
 ```

## Nachrichten für Message Hub erstellen
Wenn Sie eine OpenWhisk-Aktion verwenden wollen, um eine Nachricht für Message Hub zu erstellen, können Sie die Aktion `/messaging/messageHubProduce` verwenden. Diese Aktion verwendet die folgenden Parameter:

|Name|Typ|Beschreibung|
|---|---|---|
|kafka_brokers_sasl|JSON-Array aus Zeichenfolgen|Dieser Parameter ist ein Array aus Zeichenfolgen im Format `<host>:<port>`, die die Broker in Ihrer Message Hub-Instanz umfassen.|
|user|Zeichenfolge|Ihr Message Hub-Benutzername.|
|password|Zeichenfolge|Ihr Message Hub-Kennwort.|
|topic|Zeichenfolge|Das Thema, für das der Auslöser empfangsbereit sein soll.|
|value|Zeichenfolge|Der Wert für die Nachricht, die Sie erstellen wollen.|
|key|Zeichenfolge (optional)|Der Schlüssel für die Nachricht, die Sie erstellen wollen.|

Die ersten drei Parameter können durch den Befehl `wsk package refresh` automatisch gebunden werden. Das folgende Beispiel zeigt hingegen, wie die Aktion mit allen erforderlichen Parametern aufgerufen wird:

```
wsk action invoke /messaging/messageHubProduce -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p topic mytopic -p user <your Message Hub user> -p password <your Message Hub password> -p value "This is the content of my message"
```
{: pre}

## Beispiele

### OpenWhisk mit IBM Message Hub, Node Red, IBM Watson IoT, IBM Object Storage und IBM Data Science Experience integrieren
Ein Beispiel, das OpenWhisk mit IBM Message Hub, Node Red, IBM Watson IoT, IBM Object Storage und dem Service von IBM Data Science Experience (Spark) integriert, finden Sie [hier](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0).

## Referenzinformationen
- [IBM Message Hub](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
