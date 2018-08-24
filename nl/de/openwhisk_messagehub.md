---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Message Hub-Ereignisquelle
{: #openwhisk_catalog_message_hub}

Sie können einen Auslöser erstellen, der reagiert, wenn Nachrichten unter Verwendung von Feeds an eine {{site.data.keyword.messagehub_full}}-Instanz gesendet werden. Dieser Abschnitt enthält Informationen zum Erstellen von {{site.data.keyword.messagehub}}-Auslösern mit oder ohne {{site.data.keyword.Bluemix}}, zur Überwachung von Nachrichten und zur Verarbeitung von Stapelnachrichten.
{: shortdesc}

## {{site.data.keyword.messagehub}}-Paket

Die Aktion `/messaging/messageHubProduce` ist veraltet und wird in Zukunft entfernt. Um eine optimale Leistung zu erzielen, stellen Sie Ihre Nutzung der Aktion `/messaging/messageHubProduce` um, indem Sie eine persistente Verbindung verwenden, wenn Daten für Message Hub/Kafka generiert werden.
{: tip}

Dieses Paket ermöglicht die Kommunikation mit [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub)-Instanzen zur Veröffentlichung und Verarbeitung von Nachrichten unter Verwendung der nativen hochleistungsfähigen Kafka-API. Weitere Informationen zum {{site.data.keyword.messagehub}}-Paket, zum Einrichten des Pakets und zum Generieren von Nachrichten finden Sie im Abschnitt [{{site.data.keyword.messagehub}}-Paket](./messagehub_actions.html).

## Auslöser erstellen, der für eine {{site.data.keyword.messagehub}}-Instanz empfangsbereit ist
{: #create_message_hub_trigger}

Zum Erstellen eines Auslösers, der reagiert, wenn Nachrichten an eine {{site.data.keyword.messagehub}}-Instanz gesendet werden, müssen Sie den Feed `/messaging/messageHubFeed` verwenden. Die Feedaktion unterstützt die folgenden Parameter:

|Name|Typ|Beschreibung|
|---|---|---|
|kafka_brokers_sasl|JSON-Array aus Zeichenfolgen|Dieser Parameter ist ein Array aus Zeichenfolgen im Format `<host>:<port>`, die die Broker in Ihrer {{site.data.keyword.messagehub}}-Instanz umfassen.|
|user|Zeichenfolge|Ihr {{site.data.keyword.messagehub}}-Benutzername.|
|password|Zeichenfolge|Ihr {{site.data.keyword.messagehub}}-Kennwort.|
|topic|Zeichenfolge|Das Thema, für das der Auslöser empfangsbereit sein soll.|
|kafka_admin_url|URL-Zeichenfolge|Die URL der {{site.data.keyword.messagehub}}-REST-Schnittstelle für Administratoren.|
|isJSONData|Boolesch (optional: Standard=false)|Wenn dieser Parameter auf `true` gesetzt wird, versucht der Provider, den Nachrichtenwert als JSON-Daten zu parsen, bevor er ihn als Nutzdaten für den Auslöser weitergibt.|
|isBinaryKey|Boolesch (optional: Standard=false)|Wenn dieser Parameter auf `true` gesetzt wird, codiert der Provider den Schlüsselwert in Base64, bevor er ihn als Nutzdaten für den Auslöser weitergibt.|
|isBinaryValue|Boolesch (optional: Standard=false)|Wenn dieser Parameter auf `true` gesetzt wird, codiert der Provider den Nachrichtenwert in Base64, bevor er ihn als Nutzdaten für den Auslöser weitergibt.|

Diese Parameterliste kann zwar abschreckend wirken, jedoch können die Parameter automatisch mit dem CLI-Plug-in-Befehl `ibmcloud fn package refresh` für Sie festgelegt werden.

1. Erstellen Sie eine Instanz des {{site.data.keyword.messagehub}}-Service unter Ihrer aktuellen Organisation und dem aktuellen Bereich, die bzw. den Sie für {{site.data.keyword.openwhisk}} verwenden.

2. Stellen Sie sicher, dass das zu überwachende Thema in {{site.data.keyword.messagehub}} verfügbar ist, oder erstellen Sie ein neues Thema. Beispiel: **mytopic**.

3. Aktualisieren Sie die Pakete in Ihrem Namensbereich. Die Aktualisierung erstellt automatisch eine Paketbindung für die {{site.data.keyword.messagehub}}-Serviceinstanz, die Sie erstellt haben.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Beispielausgabe:
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```
  {: screen}

4. Listen Sie die Pakete in Ihrem Namensbereich auf, um zu zeigen, dass Ihre Paketbindung nun verfügbar ist.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Beispielausgabe:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```
  {: screen}

  Ihre Paketbindung enthält nun die Ihrer {{site.data.keyword.messagehub}}-Instanz zugeordneten Berechtigungsnachweise.

5. Nun müssen Sie nur noch einen Auslöser erstellen, der aktiviert wird, wenn neue Nachrichten an Ihr {{site.data.keyword.messagehub}}-Thema gesendet werden.
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## Auslöser für ein {{site.data.keyword.messagehub}}-Paket außerhalb von {{site.data.keyword.Bluemix_notm}} erstellen
{: #create_message_hub_trigger_outside}

Wenn Sie {{site.data.keyword.messagehub}} außerhalb von {{site.data.keyword.Bluemix_notm}} einrichten wollen, müssen Sie manuell eine Paketbindung für Ihren {{site.data.keyword.messagehub}}-Service erstellen. Dazu benötigen Sie die Berechtigungsnachweise für den {{site.data.keyword.messagehub}}-Service und die Verbindungsinformationen.

1. Erstellen Sie eine Paketbindung, die für Ihren {{site.data.keyword.messagehub}}-Service konfiguriert ist.
  ```
  ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. Jetzt können Sie einen Auslöser mit Ihrem neuen Paket erstellen, der aktiviert wird, wenn neue Nachrichten an Ihr {{site.data.keyword.messagehub}}-Thema gesendet werden.
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}

## Nachrichten überwachen
{: #message_hub_listen}

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
{: codeblock}

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
{: codeblock}

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
{: codeblock}

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
{: codeblock}

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
{: codeblock}

### Nachrichten werden stapelweise verarbeitet
Beachten Sie, dass die Nutzdaten für den Auslöser ein Array aus Nachrichten enthalten. Wenn diese Nachrichten sehr schnell für Ihr Messaging-System erstellt werden, versucht der Feed, die gesendeten Nachrichten in einem Stapel zusammenzufassen, der nur eine einzige Aktivierung des Auslösers zur Folge hat. Durch diese Stapelverarbeitung können die Nachrichten schneller und effizienter an Ihren Auslöser gesendet werden.

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

## Beispiele
{: #examples}

### OpenWhisk mit {{site.data.keyword.messagehub}}, Node Red, IBM Watson IoT, {{site.data.keyword.cos_full_notm}} und IBM Data Science Experience integrieren
Ein Beispiel, das OpenWhisk mit {{site.data.keyword.messagehub}}, Node Red, IBM Watson IoT, {{site.data.keyword.cos_full}} und dem Service von IBM Data Science Experience (Spark) integriert, finden Sie [hier](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0).

## Referenzinformationen
- [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
