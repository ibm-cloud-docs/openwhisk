---

copyright:
  years: 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Visualizzazione dei log di attivazione in IBM Cloud
{: #openwhisk_logs}

I log di attivazione possono essere visualizzati direttamente dalla [pagina Monitoraggio di {{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/dashboard/). I log vengono inoltre inoltrati a [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) dove vengono indicizzati, consentendo la ricerca di testo completo in tutti i messaggi generati e pratiche query basate su campi specifici (come il livello di log).
{:shortdesc}

## Query dei log
{: #query-logs}

Quando si utilizza Kibana ospitato in [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana), l'esecuzione di query dei tuoi log è semplice. Utilizza la sintassi di query di Kibana per trovare i log che stai cercando.

L'interfaccia utente di {{site.data.keyword.openwhisk_short}} ti consente di passare direttamente ai log e ai risultati delle tue azioni in Kibana. Il link **Log** si trova nel menu di navigazione interno a sinistra della [pagina Monitoraggio di {{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/dashboard/). Durante l'accesso alla pagina dei dettagli di un'azione specifica, il link **Log** ti porta ai risultati (record di attivazione) di quella particolare azione. Il valore predefinito impostato per il tempo di visualizzazione dei log è di 15 minuti. Puoi modificare questo valore direttamente in Kibana nell'angolo superiore destro se desideri visualizzare i record più vecchi.

Ecco alcuni esempi di query utili per eseguire il debug degli errori.

### Ricerca di tutti i log di errori:
```
type: user_logs AND stream_str: stderr
```
{: codeblock}

### Ricerca di tutti i log di errori generati da "myAction":
```
type: user_logs AND stream_str: stderr AND action_str: "*myAction"
```
{: codeblock}

## Query dei risultati
{: #query-results}

Oltre alle righe dei log, [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) indicizza anche i risultati (record di attivazione) generati da {{site.data.keyword.openwhisk_short}}. I risultati contengono metadati completi rilevanti per le attivazioni, come la durata o il codice del risultato (esito positivo, errore). Tutti i campi sono interrogabili tramite query e, come tali, possono aiutarti a capire come si comportano le tue azioni {{site.data.keyword.openwhisk_short}}.

Utilizza la sintassi di query di Kibana per trovare le attivazioni che stai cercando. Ecco alcuni esempi di query utili per eseguire il debug degli errori.

### Ricerca di tutte le attivazioni non riuscite.
```
type: activation_record AND NOT status_str: 0
```
{: codeblock}

Come nei comandi Unix, uno "`0`" indica un'azione terminata correttamente mentre tutto il resto è considerato un errore.

<!--
### Finding all activations that took longer than 30 seconds:

```
type: activation_record AND duration > 30000
```

Duration is in milliseconds.
-->

### Ricerca di tutte le attivazioni non riuscite con un errore specifico:
```
type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
```
{: codeblock}
