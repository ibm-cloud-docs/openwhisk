---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# {{site.data.keyword.messagehub}}
{: #openwhisk_catalog_message_hub}

Un pacchetto che consente la comunicazione con le istanze [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub) per la pubblicazione e il consumo di messaggi utilizzando l'API Kafka nativa ad elevate prestazioni.
{: shortdesc}

## Configurazione di un pacchetto {{site.data.keyword.messagehub}} mediante {{site.data.keyword.Bluemix_notm}}
{: #create_message_hub_ibm}

1. Crea un'istanza del servizio {{site.data.keyword.messagehub}} nell'organizzazione e nello spazio correnti che stai utilizzando per {{site.data.keyword.openwhisk}}.

2. Verifica che l'argomento che desideri ascoltare sia disponibile in {{site.data.keyword.messagehub}} o crea un nuovo argomento, intitolato ad esempio **mytopic**.

3. Aggiorna i pacchetti nel tuo spazio dei nomi. L'aggiornamento crea automaticamente un bind di pacchetto per l'istanza del servizio {{site.data.keyword.messagehub}} da te creata.
  ```
  ibmcloud wsk package refresh
  ```
  {: pre}

  Output di esempio:
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```
  {: screen}

4. Elenca i pacchetti nel tuo spazio dei nomi per verificare che il bind di pacchetto sia ora disponibile.
  ```
  ibmcloud wsk package list
  ```
  {: pre}

  Output di esempio:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```
  {: screen}

  Il bind di pacchetto contiene ora le credenziali associate alla tua istanza {{site.data.keyword.messagehub}}.

## Configurazione di un pacchetto {{site.data.keyword.messagehub}} all'esterno di {{site.data.keyword.Bluemix_notm}}

Se vuoi configurare {{site.data.keyword.messagehub}} all'esterno di {{site.data.keyword.Bluemix_notm}}, devi creare manualmente un bind di pacchetto per il tuo servizio {{site.data.keyword.messagehub}}. Ti servono le credenziali del servizio {{site.data.keyword.messagehub}} e le informazioni di connessione.

Crea un bind di pacchetto configurato per il tuo servizio {{site.data.keyword.messagehub}}.
```
ibmcloud wsk package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

## Ascolto di messaggi utilizzando eventi

Per informazioni dettagliate su come utilizzare i trigger in {{site.data.keyword.messagehub}} per ascoltare i messaggi, vedi l'argomento
[Origine eventi{{site.data.keyword.messagehub}}](./openwhisk_messagehub.html) in cui vengono completate le seguenti attività:
* [Creazione di un trigger che ascolta un'istanza di {{site.data.keyword.messagehub}}](./openwhisk_messagehub.html#create_message_hub_trigger)
* [Creazione di un trigger per un pacchetto {{site.data.keyword.messagehub}} all'esterno di {{site.data.keyword.Bluemix_notm}}](./openwhisk_messagehub.html#create_message_hub_trigger_outside)
* [Ascolto di messaggi](./openwhisk_messagehub.html#message_hub_listen)
* [Esempi](./openwhisk_messagehub.html#examples)

## Produzione di messaggi in {{site.data.keyword.messagehub}}
{: #producing_messages}

L'azione `/messaging/messageHubProduce` è obsoleta e verrà rimossa in una data futura. Per mantenere prestazioni ottimali, migra l'utilizzo dell'azione `/messaging/messageHubProduce` per utilizzare una connessione persistente quando i dati vengono prodotti in {{site.data.keyword.messagehub}}/Kafka.
{: tip}

Se vuoi utilizzare un'azione {{site.data.keyword.openwhisk_short}} per produrre facilmente un messaggio in {{site.data.keyword.messagehub}}, puoi utilizzare l'azione `/messaging/messageHubProduce`. Questa azione accetta i seguenti parametri:

|Nome|Tipo|Descrizione|
|---|---|---|
|kafka_brokers_sasl|Array di stringhe JSON|Questo parametro è un array di stringhe `<host>:<port>` che comprendono i broker nella tua istanza di {{site.data.keyword.messagehub}}.|
|user|Stringa|Il tuo nome utente {{site.data.keyword.messagehub}}.|
|password|Stringa|La tua password {{site.data.keyword.messagehub}}.|
|topic|Stringa|L'argomento per cui vuoi che il trigger sia in ascolto.|
|value|Stringa|Il valore per il messaggio che vuoi produrre.|
|key|Stringa (facoltativo)|La chiave per il messaggio che vuoi produrre.|

Anche se i primi tre parametri possono essere associati automaticamente utilizzando `ibmcloud wsk package refresh`, vedi il seguente esempio che richiama l'azione con tutti i parametri necessari:
```
ibmcloud wsk action invoke /messaging/messageHubProduce -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p topic mytopic -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p value "This is the content of my message"
```
{: pre}

## Riferimenti
- [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
