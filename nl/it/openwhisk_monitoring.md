---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Monitora l'attività con il dashboard
{: #openwhisk_monitoring}

Il [dashboard {{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/dashboard/) fornisce un riepilogo grafico della tua attività. Utilizza il dashboard per determinare le prestazioni e l'integrità delle tue azioni {{site.data.keyword.openwhisk_short}}.
{:shortdesc}

Fai clic su **Ricarica** in qualsiasi momento per aggiornare il dashboard con i più recenti dati di log delle attivazioni.

## Riepilogo attività
{: #summary}

Questa vista fornisce un riepilogo di alto livello del tuo ambiente {{site.data.keyword.openwhisk_short}}. Utilizza la vista **Riepilogo attività** per monitorare l'integrità e le prestazioni generali del tuo servizio abilitato a {{site.data.keyword.openwhisk_short}}. Partendo dalle metriche della vista, puoi eseguire le seguenti operazioni:
* Determinare il tasso di utilizzo delle azioni abilitate a {{site.data.keyword.openwhisk_short}} del tuo servizio visualizzando il numero di volte in cui sono state richiamate.
* Determinare la frequenza complessiva di errori tra tutte le azioni. Se individui un errore, puoi isolare i servizi o le azioni contenenti l'errore attraverso la vista **Istogramma attività**. Isola gli errori stessi visualizzando il **Log attività**.
* Determinare le prestazioni delle tue azioni, visualizzando il tempo medio di completamento associato a ciascuna azione.

<!-- For tips on improving performance, see troubleshooting? -->

## Sequenza temporale attività
{: #timeline}

La vista **Sequenza temporale attività** visualizza un grafico a barre verticale per mostrare l'attività delle azioni passate e presenti. Il colore rosso indica la presenza di errori in determinate azioni. Metti questa vista in correlazione con il **Log attività** per ricavare ulteriori dettagli sugli errori.

## Istogramma attività
{: #histogram}

La vista **Istogramma attività** visualizza un grafico a barre orizzontale per mostrare l'attività delle azioni passate e presenti. Il colore rosso indica la presenza di errori in determinate azioni. Metti questa vista in correlazione con il **Log attività** per ricavare ulteriori dettagli sugli errori.

## Log attività
{: #log}

Questa vista visualizza una versione formattata del log di attivazione. Mostra i dettagli di ogni attivazione, ma richiede informazioni sulle nuove attivazioni una volta al minuto. Fai clic su un'azione per visualizzare un log dettagliato.

Per visualizzare l'output nel Log attività mediante la CLI, utilizza il seguente comando:
```
ibmcloud fn activation poll
```
{: pre}

## Opzioni di filtro
{: #filtering}

Seleziona i log delle azioni che desideri visualizzare e l'intervallo di tempo dell'attività registrata.

Questi filtri vengono applicati a tutte le viste nel dashboard.
{: tip}
