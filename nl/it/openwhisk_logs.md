---

copyright:
  years: 2018
lastupdated: "2018-07-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Attività di registrazione e monitoraggio
{: #openwhisk_logs}

La registrazione e il monitoraggio sono abilitati automaticamente in {{site.data.keyword.openwhisk_short}} per aiutarti a risolvere i problemi e migliorare l'integrità e le prestazioni delle tue azioni.

## Visualizzazione dei log
{: #view-logs}

Puoi visualizzare i log di attivazione direttamente dal dashboard di monitoraggio {{site.data.keyword.openwhisk_short}}. I log vengono anche inoltrati a [{{site.data.keyword.loganalysisfull}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) dove vengono indicizzati, abilitando la ricerca di testo completo in tutti i messaggi generati e pratiche query basate su campi specifici.
{:shortdesc}

1. Apri la pagina Monitoraggio di [{{site.data.keyword.openwhisk_short}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/openwhisk/dashboard/).

2. Facoltativo: per visualizzare i log solo per una specifica azione, limita il riepilogo di monitoraggio a tale azione. Nella sezione Opzioni di filtro, seleziona il nome dell'azione dall'elenco a discesa **Limita a**.

3. Nella navigazione sulla sinistra, fai clic su **Log**.Viene aperta la pagina {{site.data.keyword.loganalysisshort_notm}} Kibana.

4. Facoltativo: per visualizzare i log meno recenti, modifica il valore di intervallo temporale predefinito di 15 minuti facendo clic su **Ultimi 15 minuti** nell'angolo superiore destro e selezionando un intervallo di tempo differente.

### Query dei log
{: #query-logs}

Puoi trovare dei log di attivazione specifici in [{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) utilizzando la sintassi di query di Kibana.

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
{: #query-results}

Oltre ai file di log, [{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) indicizza anche i risultati, o i record di attivazione, generati da {{site.data.keyword.openwhisk_short}}. I risultati contengono i metadati di attivazione come la durata dell'attivazione o il codice del risultato dell'attivazione. Eseguire query dei campi di risultato può aiutarti a capire come si stanno comportando le tue azioni {{site.data.keyword.openwhisk_short}}.

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

## Monitoraggio dell'attività
{: #openwhisk_monitoring}

Il [dashboard {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/dashboard/) fornisce un riepilogo grafico della tua attività. Utilizza il dashboard per determinare le prestazioni e l'integrità delle tue azioni {{site.data.keyword.openwhisk_short}}.
{:shortdesc}

Puoi filtrare i log selezionando quali log di azione vuoi visualizzare e selezionare l'intervallo di tempo dell'attività registrata. Questi filtri vengono applicati a tutte le viste nel dashboard. Fai clic su **Ricarica** in qualsiasi momento per aggiornare il dashboard con i più recenti dati di log delle attivazioni.

### Riepilogo attività
{: #summary}

La vista **Riepilogo attività** fornisce un riepilogo di alto livello del tuo ambiente {{site.data.keyword.openwhisk_short}}. Utilizza questa vista per monitorare l'integrità e le prestazioni generali del tuo servizio abilitato a {{site.data.keyword.openwhisk_short}}. Partendo dalle metriche della vista, puoi eseguire le seguenti operazioni:
* Determina il tasso di utilizzo delle azioni abilitate a {{site.data.keyword.openwhisk_short}} del tuo servizio visualizzando il numero di volte in cui sono state richiamate.
* Determina la frequenza complessiva di errori tra tutte le azioni. Se vedi un errore, puoi isolare i servizi o le azioni contenenti l'errore attraverso la vista **Istogramma attività**. Isola gli errori stessi visualizzando il **Log attività**.
* Determinare le prestazioni delle tue azioni, visualizzando il tempo medio di completamento associato a ciascuna azione.

### Sequenza temporale attività
{: #timeline}

La vista **Sequenza temporale attività** visualizza un grafico a barre verticale per mostrare l'attività delle azioni passate e presenti. Il colore rosso indica la presenza di errori in determinate azioni. Metti questa vista in correlazione con il **Log attività** per ricavare ulteriori dettagli sugli errori.

<!--
### Activity Histogram
{: #histogram}

The **Activity Histogram** view displays a horizontal bar graph for viewing the activity of past and present actions. Red bars indicate errors within specific actions. Correlate this view with the **Activity Log** to find more details about errors.
-->

### Log attività
{: #log}

Questa vista **Log attività** visualizza una versione formattata del log di attivazione. Questa vista mostra i dettagli di ogni attivazione, ma richiede informazioni sulle nuove attivazioni una volta al minuto. Fai clic su un'azione per visualizzare un log dettagliato.

Per ottenere l'output visualizzato nel log attività mediante la CLI, utilizza il seguente comando:
```
ibmcloud fn activation poll
```
{: pre}
