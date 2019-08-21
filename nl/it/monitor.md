---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: monitoring, viewing, performance, dashboard, metrics, health, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Monitoraggio dell'attività
{: #monitor}

Ottieni delle informazioni approfondite sulle prestazioni delle tue azioni distribuite con {{site.data.keyword.openwhisk}}. Le metriche possono aiutarti a trovare colli di bottiglia o a prevedere possibili problemi di produzione in base alla durata dell'azione, ai risultati delle attivazioni dell'azione o se raggiungi i limiti di attivazione dell'azione.
{: shortdesc}

Le metriche vengono raccolte per tutte le entità. A seconda se le tue azioni sono in uno spazio dei nomi basato su IAM o Cloud Foundry, le metriche vengono ubicate nell'account o nello spazio IBM Cloud. Queste metriche vengono inviate a {{site.data.keyword.monitoringlong}} e vengono rese disponibili tramite Grafana, in cui puoi configurare i tuoi dashboard, creare degli avvisi basati sui valori di evento delle metriche e altro ancora. Per ulteriori informazioni sulle metriche, vedi la[documentazione {{site.data.keyword.monitoringlong_notm}} ](/docs/services/cloud-monitoring?topic=cloud-monitoring-getting-started#getting-started).

## Creazione di un dashboard
{: #monitor_dash}

Inizia creando una dashboard di monitoraggio Grafana.

1. Vai a uno dei seguenti URL.
  <table>
    <thead>
      <tr>
        <th>Regione {{site.data.keyword.openwhisk_short}}</th>
        <th>Indirizzo di monitoraggio</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Europa Centrale</td>
        <td>`metrics.eu-de.bluemix.net`</td>
      </tr>
      <tr>
        <td>Regno Unito Sud</td>
        <td>`metrics.eu-gb.bluemix.net`</td>
      </tr>
      <tr>
        <td>Stati Uniti Sud</td>
        <td>`metrics.ng.bluemix.net`</td>
      </tr>
      <tr>
        <td>Stati Uniti Est</td>
        <td>Non disponibile</td>
      </tr>
    </tbody>
  </table>

2. Seleziona il dominio delle metriche.
    * Spazi dei nomi basati su IAM:
        1. Fai clic sul tuo nome utente.
        2. Nell'elenco a discesa **Domain**, seleziona **account**.
        3. Nell'elenco a discesa **Account**, seleziona l'account IBM Cloud in cui è ubicato il tuo spazio dei nomi basato su IAM.
    * Spazi dei nomi basati su Cloud Foundry:
        1. Fai clic sul tuo nome utente.
        2. Nell'elenco a discesa **Domain**, seleziona **space**.
        3. Utilizza gli elenchi a discesa **Organization** e **Space** per selezionare il tuo spazio dei nomi basato su Cloud Foundry.

3. Crea un dashboard.
    * Per utilizzare il dashboard {{site.data.keyword.openwhisk_short}} predefinito:
        1. Vai a **Home > Import**.
        3. Immetti l'ID del dashboard {{site.data.keyword.openwhisk_short}} predefinito, `8124`, nel campo **Grafana.net Dashboard**.
        4. Fai clic su **Import**.
    * Per creare un dashboard personalizzato, vai a **Home > Create New**.

Dopo aver eseguito un'azione, vengono generate delle nuove metriche e sono ricercabili in Grafana. Nota: possono essere necessari fino a 10 minuti perché l'azione venga visualizzata in Grafana.




## Utilizzo dei dashboard
{: #monitor_dash_use}

Il [dashboard {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk/dashboard){: external} fornisce un riepilogo grafico della tua attività. Utilizza il dashboard per determinare le prestazioni e l'integrità delle tue azioni {{site.data.keyword.openwhisk_short}}.
{:shortdesc}

Puoi filtrare i log selezionando quali log di azione vuoi visualizzare e selezionare l'intervallo di tempo dell'attività registrata. Questi filtri vengono applicati a tutte le viste nel dashboard. Fai clic su **Ricarica** in qualsiasi momento per aggiornare il dashboard con i più recenti dati di log delle attivazioni.

### Riepilogo attività
{: #monitor_dash_sum}

La vista **Riepilogo attività** fornisce un riepilogo di alto livello del tuo ambiente {{site.data.keyword.openwhisk_short}}. Utilizza questa vista per monitorare l'integrità e le prestazioni generali del tuo servizio abilitato a {{site.data.keyword.openwhisk_short}}. Partendo dalle metriche della vista, puoi eseguire le seguenti operazioni:
* Determina il tasso di utilizzo delle azioni abilitate a {{site.data.keyword.openwhisk_short}} del tuo servizio visualizzando il numero di volte in cui sono state richiamate.
* Determina la frequenza complessiva di errori tra tutte le azioni. Se vedi un errore, puoi isolare i servizi o le azioni contenenti l'errore attraverso la vista **Istogramma attività**. Isola gli errori stessi visualizzando il **Log attività**.
* Determinare le prestazioni delle tue azioni, visualizzando il tempo medio di completamento associato a ciascuna azione.

### Sequenza temporale attività
{: #monitor_dash_time}

La vista **Sequenza temporale attività** visualizza un grafico a barre verticale per mostrare l'attività delle azioni passate e presenti. Il colore rosso indica la presenza di errori in determinate azioni. Metti questa vista in correlazione con il **Log attività** per ricavare ulteriori dettagli sugli errori.



### Log attività
{: #monitor_dash_log}

Questa vista **Log attività** visualizza una versione formattata del log di attivazione. Questa vista mostra i dettagli di ogni attivazione, ma richiede informazioni sulle nuove attivazioni una volta al minuto. Fai clic su un'azione per visualizzare un log dettagliato.

Per ottenere l'output visualizzato nel log attività mediante la CLI, utilizza il seguente comando:
```
ibmcloud fn activation poll
```
{: pre}




## Formato della metrica
{: #monitor_metric}

Le metriche riflettono i dati raccolti dalle attivazioni della tua azione che vengono aggregati ogni minuto. Le metriche sono ricercabili sulle prestazioni dell'azione o al livello di simultaneità dell'azione.


### Metriche delle prestazioni dell'azione
{: #monitor_metric_perf}

Le metriche delle prestazioni dell'azione sono valori che vengono calcolati per una singola azione. Le metriche delle prestazioni dell'azione includono le caratteristiche temporali delle esecuzioni e lo stato delle attivazioni. Nota: se non specifichi il nome di un pacchetto durante la creazione, viene utilizzato il nome di pacchetto predefinito. Queste metriche prendono il seguente formato:

```
ibmcloud.public.functions.<region>.action.namespace.<namespace>.<package>.<action>.<metric_name>
```
{: codeblock}

I seguenti caratteri vengono convertiti in trattini (`-`): punto (.), simbolo chiocciola (@), spazio vuoto ( ), e commerciale (&), carattere di sottolineatura (_), due punti (:)
{: tip}

Esempio: se hai un'azione denominata `hello-world` nello spazio dei nomi basato su Cloud Foundry `user@email.com_dev` nella regione `us-south`, una metrica delle prestazioni dell'azione è simile a quanto segue:

```
ibmcloud.public.functions.us-south.action.namespace.user-ibm-com-dev.action-performance.default.hello-world.duration
```
{: screen}

</br>

### Metriche della simultaneità dell'azione
{: #monitor_metric_con}

Le metriche della simultaneità dell'azione vengono calcolate in base ai dati da tutte le azioni attive in uno spazio dei nomi. La simultaneità dell'azione include il numero di richiami simultanei e le limitazioni di sistema che potrebbero potenzialmente verificarsi quando vengono superati i limiti di simultaneità. Queste metriche prendono il seguente formato:

```
ibmcloud.public.functions.<region>.action.namespace.<namespace>.action-performance.<package>.<action>.<metric_name>
```
{: codeblock}

Esempio: se hai uno spazio dei nomi basato su IAM denominato `myNamespace` nella regione `us-south`, una metrica della simultaneità dell'azione è simile a quanto segue:

```
ibmcloud.public.functions.us-south.action.namespace.all.concurrent-invocations
```
{: screen}

</br>

### Metriche disponibili
{: #monitor_metric_av}

Poiché potresti avere migliaia o milioni di attivazioni dell'azione, i valori della metrica vengono rappresentati come un'aggregazione di eventi prodotti da molte attivazioni. I valori vengono aggregati nei seguenti modi:
* Somma: vengono aggiunti tutti i valori della metrica.
* Media: viene calcolata una media aritmetica.
* Media totale: viene calcolata una media aritmetica basata sui componenti e aggiungendo diversi componenti tra loro.

Consulta la seguente tabella per vedere le metriche che sono disponibili.

<table>
  <thead>
    <tr>
      <th>Nome metrica</th>
      <th>Descrizione</th>
      <th>Tipo</th>
      <th>Categoria</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>duration</code></td>
      <td>La durata dell'azione media, tempo di esecuzione dell'azione fatturato.</td>
      <td>Media</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>init-time</code></td>
      <td>Il tempo impiegato per inizializzare il contenitore di azioni.</td>
      <td>Media</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>wait-time</code></td>
      <td>Il tempo medio trascorso in una coda in attesa della pianificazione di un'attivazione.</td>
      <td>Media</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>activation</code></td>
      <td>Il numero complessivo di attivazioni che sono state attivate nel sistema.</td>
      <td>Somma</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.success</code></td>
      <td>Il numero di attivazioni riuscite del codice azione.</td>
      <td>Somma</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.application</code></td>
      <td>Il numero di attivazioni non riuscite causate da errori dell'applicazione. Ad esempio, errori normali delle azioni. Per ulteriori informazioni su come vengono calcolate le metriche action-performance, consulta [Understanding the activation record](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions.md#understanding-the-activation-record){: external}.</td>
      <td>Somma</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.developer</code></td>
      <td>Il numero di attivazioni non riuscite causate dallo sviluppatore. Ad esempio, la violazione dell'[interfaccia del proxy di azione](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions-new.md#action-interface){: external} da parte di eccezioni non gestite nel codice di azione.</td>
      <td>Somma</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.internal</code></td>
      <td>Il numero di attivazioni non riuscite dovute ad errori interni {{site.data.keyword.openwhisk_short}}.</td>
      <td>Somma</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>concurrent-rate-limit</code></td>
      <td>La somma delle attivazioni che sono state limitate a causa del superamento del limite della frequenza di simultaneità. Non viene emessa alcuna metrica se non viene raggiunto il limite.</td>
      <td>Somma</td>
      <td><code>action-concurrency</code></td>
    </tr>
    <tr>
      <td><code>timed-rate-limit</code></td>
      <td>La somma delle attivazioni che sono state limitate a causa del superamento del limite al minuto. Non viene emessa alcuna metrica se non viene raggiunto il limite.</td>
      <td>Somma</td>
      <td><code>action-concurrency</code></td>
    </tr>
    <tr>
      <td><code>concurrent-invocations</code></td>
      <td>Il numero di richiami simultanei nel sistema.</td>
      <td>Media totale</td>
      <td><code>action-concurrency</code></td>
    </tr>
  </tbody>
</table>

Le metriche per le azioni che fanno parte dello spazio dei nomi predefinito sono disponibili nella categoria predefinita.
{: tip}




