---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: message hub, package, messages, events

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.messagehub}}-Paket

{: #catalog_message_hub}

Ein Paket, das die Kommunikation mit [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub)-Instanzen zur Veröffentlichung und Verarbeitung von Nachrichten unter Verwendung der nativen hochleistungsfähigen Kafka-API ermöglicht.
{: shortdesc}

## {{site.data.keyword.messagehub}}-Paket mit {{site.data.keyword.Bluemix_notm}} einrichten
{: #create_message_hub_ibm}

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

## {{site.data.keyword.messagehub}}-Paket außerhalb von {{site.data.keyword.Bluemix_notm}} einrichten

Wenn Sie {{site.data.keyword.messagehub}} außerhalb von {{site.data.keyword.Bluemix_notm}} einrichten wollen, müssen Sie manuell eine Paketbindung für Ihren {{site.data.keyword.messagehub}}-Service erstellen. Dazu benötigen Sie die Berechtigungsnachweise für den {{site.data.keyword.messagehub}}-Service und die Verbindungsinformationen.

Erstellen Sie eine Paketbindung, die für Ihren {{site.data.keyword.messagehub}}-Service konfiguriert ist.
```
ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

## Nachrichten mithilfe von Ereignissen überwachen

Ausführliche Informationen zur Verwendung von Auslösern in {{site.data.keyword.messagehub}} zum Überwachen von Nachrichten finden Sie im Abschnitt [{{site.data.keyword.messagehub}}-Ereignisquelle](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub), in dem die folgenden Tasks behandelt werden:
* [Auslöser erstellen, der eine {{site.data.keyword.messagehub}}-Instanz überwacht](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#create_message_hub_trigger)
* [Auslöser für ein {{site.data.keyword.messagehub}}-Paket außerhalb von {{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#create_message_hub_trigger_outside) erstellen
* [Nachrichten überwachen](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#message_hub_listen)
* [Beispiele](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#examples)

## Nachrichten für {{site.data.keyword.messagehub}} erstellen
{: #producing_messages}

Die Aktion `/messaging/messageHubProduce` ist veraltet und wird in Zukunft entfernt. Sie wurde bereits aus der Region 'Tokio' entfernt. Um eine optimale Leistung zu erzielen, stellen Sie Ihre Nutzung der Aktion `/messaging/messageHubProduce` um, indem Sie eine persistente Verbindung verwenden, wenn Daten für {{site.data.keyword.messagehub}}/Kafka generiert werden.
{: tip}

Weitere Informationen zum Erzeugen von Nachrichten finden Sie in der Dokumentation zu [Event Streams](/docs/services/EventStreams?topic=eventstreams-producing_messages#producing_messages). 

## Referenzinformationen
- [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub)
- [Apache Kafka](https://kafka.apache.org)
