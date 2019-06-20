---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: logging, monitoring, viewing, logs, query, performance, dashboard, metrics, health

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

# Visualizzazione dei log
{: #logs}

La registrazione viene abilitata automaticamente in {{site.data.keyword.openwhisk}} per aiutarti a risolvere i problemi. Puoi utilizzare il servizio {{site.data.keyword.cloudaccesstraillong}} per tenere traccia di come gli utenti e le applicazioni interagiscono con il servizio {{site.data.keyword.openwhisk_short}}.


## Visualizzazione dei log di azione al verificarsi dell'azione
{: #logs_poll}

Le azioni {{site.data.keyword.openwhisk_short}} possono essere richiamate da altri utenti, in risposta a vari eventi o nell'ambito di una sequenza di azioni. Per ottenere informazioni su quando le azioni sono state richiamate e su quale è stato l'output, può essere utile monitorare i log di azione.

Puoi utilizzare la CLI {{site.data.keyword.openwhisk_short}} per visualizzare l'output delle azioni non appena vengono richiamate.

1. Avvia un loop di polling che verifica continuamente l'eventuale presenza di log dalle attivazioni.

    ```
    ibmcloud fn activation poll
    ```
    {: pre}

2. passa a un'altra finestra e richiama un'azione.

    ```
    ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
    ```
    {: pre}

    Output di esempio:
    ```
    ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
    ```
    {: screen}

3. Nella finestra di polling, puoi vedere il log di attivazione.
    ```
    Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
    ```
    {: screen}
    Puoi anche vedere i log per le eventuali azioni eseguite per tuo conto in {{site.data.keyword.openwhisk_short}} in tempo reale.




## Visualizzazione dei dettagli dell'attivazione
{: #activation_details}

Le azioni {{site.data.keyword.openwhisk_short}} possono essere richiamate da altri utenti, in risposta a vari eventi o nell'ambito di una sequenza di azioni. Ogni volta che un'azione viene richiamata, per tale chiamata viene creato un record di attivazione. Per ottenere informazioni sul risultato della chiamata dell'azione, puoi ottenere i dettagli sulle attivazioni.

Per ottenere tutti gli ID di record di attivazione in uno spazio dei nomi:
```
ibmcloud fn activation list
```
{: pre}

Per ottenere i dettagli su uno specifico record di attivazione generato da una chiamata dell'azione:
```
ibmcloud fn activation get <activation_ID>
```
{: pre}

Output di esempio:
```
ok: got activation c2b36969fbe94562b36969fbe9856215
{
    "namespace": "myNamespace",
    "name": "hello",
    "version": "0.0.1",
    "subject": "user@email.com",
    "activationId": "c2b36969fbe94562b36969fbe9856215",
    "start": 1532456307768,
    "end": 1532456309838,
    "duration": 2070,
    "response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
            "done": true
  }
    },
    "logs": [],
    "annotations": [
        {
            "key": "path",
            "value": "myNamespace/hello"
        },
        {
            "key": "waitTime",
            "value": 50
        },
        {
            "key": "kind",
    "value": "nodejs:6"
        },
        {
            "key": "limits",
    "value": {
                "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
        },
        {
            "key": "initTime",
            "value": 53
        }
    ],
    "publish": false
}
```
{: screen}

<table>
<caption>Descrizione dell'output del comando <code>activation get</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dell'output del comando <code>activation get</code></th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>Lo spazio dei nomi in cui si trova questa attivazione. Potrebbe essere diverso dallo spazio dei nomi in cui si trova l'azione.</td>
</tr>
<tr>
<td><code>name</code></td>
<td>Il nome dell'azione.</td>
</tr>
<tr>
<td><code>version</code></td>
<td>La versione semantica dell'azione.</td>
</tr>
<tr>
<td><code>subject</code></td>
<td>L'account utente che ha attivato l'elemento.</td>
</tr>
<tr>
<td><code>activationId</code></td>
<td>L'ID di questo record di attivazione.</td>
</tr>
<tr>
<td><code>start</code></td>
<td>L'ora in cui è iniziata l'attivazione.</td>
</tr>
<tr>
<td><code>end
</code></td>
<td>L'ora in cui è stata completata l'attivazione.</td>
</tr>
<tr>
<td><code>duration</code></td>
<td>Il tempo, in millisecondi, impiegato per il completamento dell'attivazione.</td>
</tr>
<tr>
<td><code>response</code></td>
<td><ul><li><code>status</code>: lo stato uscita dell'attivazione.</li>
<li><code>statusCode</code>: il codice di stato. Se per l'azione si è verificato un errore, il codice di errore HTTP.</li>
<li><code>success</code>: indica se l'azione è stata completata correttamente.</li>
<li><code>result</code>: il valore restituito dall'attivazione.</li>
</ul></td>
</tr>
<tr>
<td><code>logs</code></td>
<td>I log per questa attivazione.</td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>Le annotazioni su questa azione. Per un elenco delle possibili annotazioni di attivazione, vedi l'[argomento di riferimento delle annotazioni](/docs/openwhisk?topic=cloud-functions-annotations#annotations_activation).</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>Indica se l'azione è pubblicata pubblicamente.</td>
</tr>
</tbody></table>



## Visualizzazione dei log in {{site.data.keyword.loganalysisfull_notm}}
{: #logs_view}

Puoi visualizzare i log di attivazione direttamente dal dashboard di monitoraggio {{site.data.keyword.openwhisk_short}}. I log vengono anche inoltrati a [{{site.data.keyword.loganalysisfull}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) dove vengono indicizzati, abilitando la ricerca di testo completo in tutti i messaggi generati e pratiche query basate su campi specifici.
{:shortdesc}

**Nota**: la registrazione non è disponibile per la regione Stati Uniti Est.

1. Apri la [pagina Monitoraggio di {{site.data.keyword.openwhisk_short}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/openwhisk/dashboard).

2. Facoltativo: per visualizzare i log solo per una specifica azione, limita il riepilogo di monitoraggio a tale azione. Nella sezione Opzioni di filtro, seleziona il nome dell'azione dall'elenco a discesa **Limita a**.

3. Nella navigazione sulla sinistra, fai clic su **Log**. Viene aperta la pagina {{site.data.keyword.loganalysisshort_notm}} Kibana.

4. Facoltativo: per visualizzare i log meno recenti, modifica il valore di intervallo temporale predefinito di 15 minuti facendo clic su **Ultimi 15 minuti** nell'angolo superiore destro e selezionando un intervallo di tempo differente.

### Query dei log
{: #logs_query}

Puoi trovare dei log di attivazione specifici in [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) utilizzando la sintassi di query di Kibana.

Le seguenti query di esempio possono aiutarti a eseguire il debug degli errori:
  * Trova tutti i log degli errori:
      ```
      type: user_logs AND stream_str: stderr
      ```
      {: codeblock}

  * Trova tutti i log degli errori generati da "myAction":
      ```
      type: user_logs AND stream_str: stderr AND action_str: "*myAction"
      ```
      {: codeblock}

### Query dei risultati
{: #logs_query_results}

Oltre ai file di log, [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) indicizza anche i risultati, o i record di attivazione, generati da {{site.data.keyword.openwhisk_short}}. I risultati contengono i metadati di attivazione come la durata dell'attivazione o il codice del risultato dell'attivazione. Eseguire query dei campi di risultato può aiutarti a capire come si stanno comportando le tue azioni {{site.data.keyword.openwhisk_short}}.

Puoi trovare degli specifici log di attivazione utilizzando la sintassi di query di Kibana. Le seguenti query di esempio possono aiutarti a eseguire il debug degli errori:

* Trova tutte le attivazioni non riuscite:
    ```
    type: activation_record AND NOT status_str: 0
    ```
    {: codeblock}
    Nei risultati, uno `0` indica un'azione che è stata chiusa correttamente e tutti gli altri valori indicano un errore.

* Trova tutte le attivazioni che non sono riuscite con uno specifico errore:
    ```
    type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
    ```
    {: codeblock}

