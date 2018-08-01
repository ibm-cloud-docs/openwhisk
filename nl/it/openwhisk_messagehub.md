---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Origine eventi Message Hub
{: #openwhisk_catalog_message_hub}

Puoi creare un trigger che reagisca quando si pubblicano messaggi in un'istanza {{site.data.keyword.messagehub_full}} utilizzando i feed. Scopri come creare i trigger {{site.data.keyword.messagehub}} con o senza {{site.data.keyword.Bluemix}}, ascoltare i messaggi e gestire i messaggi in batch.
{: shortdesc}

## Pacchetto {{site.data.keyword.messagehub}}

L'azione `/messaging/messageHubProduce` è obsoleta e verrà rimossa in una data futura. Per mantenere prestazioni ottimali, migra l'utilizzo dell'azione `/messaging/messageHubProduce` per utilizzare una connessione persistente quando i dati vengono prodotti in Message Hub/Kafka.
{: tip}

Questo pacchetto consente la comunicazione con le istanze [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub) per la pubblicazione e il consumo di messaggi utilizzando l'API Kafka nativa ad alte prestazioni. Per ulteriori informazioni sul pacchetto {{site.data.keyword.messagehub}}, su come configurarlo e come produrre i messaggi, vedi l'argomento [Pacchetto {{site.data.keyword.messagehub}}](./messagehub_actions.html).

## Creazione di un trigger che ascolta un'istanza {{site.data.keyword.messagehub}}
{: #create_message_hub_trigger}

Per creare un trigger che reagisca quando i messaggi vengono pubblicati in un'istanza {{site.data.keyword.messagehub}}, devi utilizzare il feed denominato `/messaging/messageHubFeed`. L'azione di feed supporta i seguenti parametri:

|Nome|Tipo|Descrizione|
|---|---|---|
|kafka_brokers_sasl|Array di stringhe JSON|Questo parametro è un array di stringhe `<host>:<port>` che comprendono i broker nella tua istanza {{site.data.keyword.messagehub}} |
|user|Stringa|Il tuo nome utente {{site.data.keyword.messagehub}}.|
|password|Stringa|La tua password {{site.data.keyword.messagehub}}.|
|topic|Stringa|L'argomento per cui vuoi che il trigger sia in ascolto.|
|kafka_admin_url|Stringa URL|L'URL dell'interfaccia REST di gestione {{site.data.keyword.messagehub}}.|
|isJSONData|Booleano (Facoltativo - predefinito=false)|Se impostato su `true`, il provider tenta di analizzare il valore del messaggio come JSON prima di passarlo come payload del trigger.|
|isBinaryKey|Booleano (Facoltativo - predefinito=false)|Se impostato su `true`, il provider codifica il valore della chiave come Base64 prima di passarlo come payload del trigger.|
|isBinaryValue|Booleano (Facoltativo - predefinito=false)|Se impostato su `true`, il provider codifica il valore del messaggio come Base64 prima di passarlo come payload del trigger.|

Sebbene questo elenco di parametri possa sembrare scoraggiante, è possibile impostarli automaticamente utilizzando il comando del plug-in della CLI `ibmcloud fn package refresh`.

1. Crea un'istanza del servizio {{site.data.keyword.messagehub}} nell'organizzazione e nello spazio correnti che stai utilizzando per {{site.data.keyword.openwhisk}}.

2. Verifica che l'argomento che desideri ascoltare sia disponibile in {{site.data.keyword.messagehub}} o crea un nuovo argomento, ad esempio, **mytopic**.

3. Aggiorna i pacchetti nel tuo spazio dei nomi. L'aggiornamento crea automaticamente un bind di pacchetto per l'istanza del servizio {{site.data.keyword.messagehub}} da te creata.
  ```
  ibmcloud fn package refresh
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
  ibmcloud fn package list
  ```
  {: pre}

  Output di esempio:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```
  {: screen}

  Il bind di pacchetto contiene ora le credenziali associate alla tua istanza {{site.data.keyword.messagehub}}.

5. Ora tutto ciò che devi fare è creare un trigger che viene attivato quando vengono pubblicati nuovi messaggi nel tuo argomento {{site.data.keyword.messagehub}}.
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## Creazione di un trigger per un pacchetto {{site.data.keyword.messagehub}} all'esterno di {{site.data.keyword.Bluemix_notm}}
{: #create_message_hub_trigger_outside}

Se vuoi configurare {{site.data.keyword.messagehub}} all'esterno di {{site.data.keyword.Bluemix_notm}}, devi creare manualmente un bind di pacchetto per il tuo servizio {{site.data.keyword.messagehub}}. Ti servono le credenziali del servizio {{site.data.keyword.messagehub}} e le informazioni di connessione.

1. Crea un bind di pacchetto configurato per il tuo servizio {{site.data.keyword.messagehub}}.
  ```
  ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. Adesso, con il tuo nuovo pacchetto, puoi creare un trigger che viene attivato quando vengono pubblicati nuovi messaggi nell'argomento {{site.data.keyword.messagehub}}.
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}

## Ascolto di messaggi
{: #message_hub_listen}

Una volta creato un trigger, il sistema monitora l'argomento specificato nel tuo servizio di messaggistica. Quando vengono pubblicati nuovi messaggi, il trigger viene attivato.

Il payload di questo trigger contiene un campo `messages`, che è un array di messaggi che sono stati pubblicati dall'ultima volta in cui il trigger è stato attivato. Ogni oggetto del messaggio nell'array contiene i seguenti campi:
- topic
- partition
- offset
- key
- value

In termini Kafka, i campi sono evidenti. Tuttavia, `key` ha una funzione denominata `isBinaryKey` che consente alla `key` di trasmettere dati binari. Inoltre, il `value` richiede una considerazione speciale. I campi `isJSONData` e `isBinaryValue` sono disponibili per gestire i messaggi JSON e binari. I campi `isJSONData` e `isBinaryValue` non possono essere utilizzati insieme.

Ad esempio, se `isBinaryKey` è stato impostato su `true` alla creazione del trigger, la `key` viene codificata come stringa in Base64 quando viene restituita dal payload di un trigger attivato.

Se una `key` di `Some key` viene pubblicata con `isBinaryKey` impostato su `true`, il payload del trigger è simile al seguente esempio:
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

Se il parametro `isJSONData` è stato impostato su `false` (o non impostato affatto) durante la creazione del trigger, il campo `value` è il valore non elaborato del messaggio pubblicato. Tuttavia, se `isJSONData` è stato impostato su `true` alla creazione del trigger, il sistema tenterà di analizzare al meglio questo valore come oggetto JSON. Se l'analisi ha esito positivo, il `value` nel payload del trigger è l'oggetto JSON risultante.

Se viene pubblicato il messaggio `{"title": "Some string", "amount": 5, "isAwesome": true}` con `isJSONData` impostato su `true`, il payload del trigger potrebbe essere simile al seguente esempio:
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

Tuttavia, se lo stesso contenuto del messaggio viene pubblicato con `isJSONData` impostato su `false`, il payload del trigger sarà simile al seguente esempio:
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

Analogamente a `isJSONData`, se `isBinaryValue` è stato impostato su `true` durante la creazione del trigger, il `value` risultante nel payload del trigger viene codificato come stringa in Base64.

Se un `value` di `Some data` viene pubblicato con `isBinaryValue` impostato su `true`, il payload del trigger potrebbe essere simile al seguente esempio:
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

Se lo stesso messaggio viene pubblicato senza `isBinaryData` impostato su `true`, il payload del trigger sarà simile al seguente esempio:
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

### Raggruppamento dei messaggi
Nota che il payload del trigger contiene un array di messaggi. Se questi messaggi vengono prodotti rapidamente nel tuo sistema di messaggistica, il feed tenta di raggruppare i messaggi inviati in una singola attivazione del tuo trigger. L'elaborazione in batch consente di pubblicare i messaggi nel trigger in modo più rapido ed efficiente.

Tieni presente che quando si codificano le azioni attivate dal trigger, il numero di messaggi nel payload è tecnicamente illimitato, ma è sempre maggiore di 0. Vedi il seguente esempio di messaggio organizzato in batch (nota la modifica nel valore *offset*):
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

## Esempi
{: #examples}

### Integrazione di OpenWhisk con {{site.data.keyword.messagehub}}, Node Red, IBM Watson IoT, {{site.data.keyword.cos_full_notm}} e IBM Data Science Experience
L'esempio che integra OpenWhisk con i servizi {{site.data.keyword.messagehub}}, Node Red, IBM Watson IoT, {{site.data.keyword.cos_full}}, IBM Data Science Experience (Spark) è [disponibile qui](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0).

## Riferimenti
- [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
