---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: actions, serverless, javascript, node, node.js

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Verifica delle applicazioni senza server
{: #test}

Verifica ogni entità che hai creato dalla CLI per verificare che l'applicazione senza server stia funzionando o per risolvere gli eventuali problemi.
{: shortdesc}


## Verifica delle azioni
{: #test-js}

Puoi testare le azioni eseguendo il comando `invoke`. Puoi testare l'azione con o senza parametri.
{: shortdesc}

```bash
ibmcloud fn action invoke --result ACTION_NAME --param PARAMETER VALUE
```
{: pre}

Esempio Hello world:
```bash
ibmcloud fn action invoke --result myAction --param name stranger
```
{: pre}

Output:
```json
  {
      "greeting": "Hello stranger!"
  }
```
{: screen}



### Verifica dei parametri memorizzati in file JSON
{: #test_json_file}

Puoi trasmettere un file di parametri in formato JSON.
{: shortdesc}

```
ibmcloud fn action invoke --result ACTION_NAME --param-file JSON_FILE
```
{: pre}

Output di esempio:
```
{
    "payload": "Hello, Dorothy from Kansas"
  }
```
{: screen}


### Verifica dei parametri immessi in formato JSON
{: #test_json}

Puoi trasmettere parametri in formato JSON tramite chiamata.
{: shortdesc}


```
ibmcloud fn action invoke --result ACTION_NAME -p person '{"PARAM_NAME": "PARAM_VALUE", "PARAM_NAME": "PARAM_VALUE"}'
```
{: pre}

Output di esempio:
```
{
    "payload": "Hello, Dorothy from Kansas"
  }
```
{: screen}


### Verifica delle azioni di blocco
{: #test-block}

La chiamata dell'azione può essere bloccante o non bloccante. Le chiamate sono non bloccanti per impostazione predefinita. Se non ti serve subito il risultato dell'azione, usa una chiamata non bloccante.
{: shortdesc}

Le chiamate bloccanti utilizzano uno stile richiesta/risposta e attendono che il risultato dell'attivazione sia disponibile. Il periodo di attesa è inferiore a 60 secondi o al [valore del limite di tempo](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits) dell'azione.

Esegui l'azione nel cloud eseguendo una chiamata bloccante:

```
ibmcloud fn action invoke --blocking ACTION_NAME
```
{: pre}


Output di esempio:
```
ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

{
    "result": {
        "payload": "Hello world"
    },
      "status": "success",
      "success": true
  }
```
{: screen}

Il comando genera in output le seguenti informazioni:
* Il risultato della chiamata, se disponibile entro il periodo di attesa previsto
* Senza l'opzione --result, l'ID attivazione viene visualizzato nel risultato. L'ID attivazione (`44794bd6aab74415b4e42a308d880e5b`) che può essere utilizzato per richiamare i log o il risultato della chiamata.






## Verifica dei trigger
{: #test_triggers}

I trigger possono essere attivati utilizzando un dizionario di coppie chiave-valore. Tale dizionario viene talvolta denominato evento. I trigger possono essere attivati esplicitamente da un utente o attivati per conto di un utente da un'origine eventi esterna. Come per le azioni, ogni attivazione di un trigger associato a una regola genera un ID di attivazione.
{: shortdesc}

1. Arriva il trigger.

    ```
    ibmcloud fn trigger fire TRIGGER_NAME --param PARAM_NAME PARAM_VALUE --param PARAM_NAME PARAM_VALUE
    ```
    {: pre}

    Un trigger che non è associato a una regola non ha alcun effetto visibile quando viene attivato. Poiché non c'è alcuna regola associata a questo trigger, i parametri passati non vengono utilizzati come input da alcuna azione.

    Output di esempio:

    ```
    ok: triggered TRIGGER_NAME with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

2. Verifica che l'azione sia stata richiamata controllando il record di attivazione più recente.
    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    Output di esempio:
    ```
    activations
    fa495d1223a2408b999c3e0ca73b2677             ACTION_NAME
    ```
    {: screen}

3. Ottieni ulteriori informazioni sull'ID attivazione dall'output del comando precedente.
    ```
    ibmcloud fn activation result ACTIVATION_ID
    ```
    {: pre}

    Output di esempio:
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}




## Verifica della durata delle attivazioni
{: #test_time}

Controlla quanto tempo ha impiegato il completamento di un'attivazione, ottenendo il log di attivazione. Se la durata è eccessiva o devi regolare il timeout predefinito per consentire un'esecuzione più lunga della funzione, puoi aggiornare la tua azione con un timeout.
{: shortdesc}

1. Ottieni l'ID attivazione.

    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    Output di esempio:
    ```
    activations
    b066ca51e68c4d3382df2d8033265db0             ACTION_NAME
    ```
    {: screen}

2. Ottieni il log per l'ID attivazione.

    ```
    ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
    ```
    {: pre}

    `duration` indica il tempo in millisecondi. Il completamento di questa attivazione ha richiesto poco più di 2 secondi:

    ```
    ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
        ...
        "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
        ...
    }
    ```
    {: screen}

3. Aggiorna l'azione con un timeout in millisecondi.

    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME --timeout VALUE
    ```
    {: pre}

    Esempio:
    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10 --timeout 1000
    ```
    {: pre}


## Verifica dell'utilizzo della memoria
{: #test_memory}

Se la tua applicazione è inserita in un pacchetto all'interno di un'immagine Docker, puoi utilizzare i comandi Docker per controllare l'utilizzo della memoria dell'applicazione.
{: shortdesc}

1. Crea un contenitore locale che esegue la tua immagine Docker.

    ```
    docker run IMAGE_NAME
    ```
    {: pre}

2. Ottieni l'elenco dei contenitori per ottenere un ID contenitore.

    ```
    docker ps
    ```
    {: pre}

3. Controlla le statistiche del contenitore in esecuzione.

    ```
    docker stats CONTAINER_ID
    ```
    {: pre}

4. Rivedi il valore di utilizzo della memoria per il contenitore. Se il valore non rientra nei limiti del sistema, apporta alcune modifiche al tuo script.

5. Dopo aver controllato le informazioni, puoi arrestare il contenitore in esecuzione.

    ```
    docker stop CONTAINER_ID
    ```
    {: pre}

6. Rimuovi il contenitore.

    ```
    docker rm CONTAINER_ID
    ```
    {: pre}






