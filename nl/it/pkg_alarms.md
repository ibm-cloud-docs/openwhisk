---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: alarms, serverless, triggers, functions

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



# Allarmi
{: #pkg_alarms}

Il pacchetto `/whisk.system/alarms` può essere utilizzato per attivare un trigger con una frequenza specifica. Gli allarmi sono utili per la configurazione di lavori o attività ricorrenti, come la chiamata oraria di un backup del sistema.
{: shortdesc}

Il pacchetto include i seguenti feed.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/alarms` | Pacchetto | - | Allarmi e utilità periodica. |
| `/whisk.system/alarms/once` | Feed | `date`, `trigger_payload`, `deleteAfterFire` | Attivare l'evento trigger una volta in una data specifica. |
| `/whisk.system/alarms/interval` | Feed | `minutes`, `trigger_payload`, `startDate`, `stopDate` | Attivare l'evento trigger in base a una pianificazione basata sull'intervallo. |
| `/whisk.system/alarms/alarm` | Feed | `cron`, `timezone`, `trigger_payload`, `startDate`, `stopDate` | Attivare l'evento trigger in base a una pianificazione basata sul tempo utilizzando cron. |



## Attivazione unica di un evento trigger
{: #pkg_alarms_one}

Il feed `/whisk.system/alarms/once` configura il servizio Allarme per attivare un evento trigger una volta in una data specificata. Per creare un allarme a singola attivazione, immetti il seguente comando.

```
ibmcloud fn trigger create fireOnce --feed /whisk.system/alarms/once --param date "<date>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param deleteAfterFire "<delete_option>"
```
{: pre}
</br>

<table>
<caption>Descrizione dei componenti del comando <code>trigger create fireOnce</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del comando <code>trigger create fireOnce</code></th>
</thead>
<tbody>
<tr>
<td><code>fireOnce</code></td>
<td>Il tipo di trigger di allarme che stai creando.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/once</code></td>
<td>Il percorso del file del pacchetto di allarmi per il feed di singola attivazione.</td>
</tr>
<tr>
<td><code>--param date</code></td>
<td>Sostituisci <code>&lt;date&gt;</code> con la data in cui prevedi di attivare il trigger. Il trigger viene attivato una volta all'ora specificata. Il parametro `date` supporta un valore intero o stringa. Il valore intero rappresenta il numero di millisecondi a partire dal 1° `gennaio 1970 00:00:00` UTC e il valore stringa deve essere nel <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">formato ISO 8601</a>.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>(Facoltativo) Sostituisci <code>&lt;key&gt;</code> e <code>&lt;value&gt;</code> con i parametri del trigger quando il trigger viene attivato.</td>
</tr>
<tr>
<td><code>--param deleteAfterFire</code></td>
<td>(Facoltativo) Indica se il trigger e le regole associate vengono eliminati dopo l'attivazione del trigger. Sostituisci <code>&lt;delete_option&gt;</code> con uno dei seguenti valori.<ul><li><code>false</code> - (predefinito) Non viene eseguita alcuna azione dopo l'attivazione del trigger.</li><li><code>true</code> - Il trigger viene eliminato dopo l'attivazione.</li><li><code>rules</code> - Il trigger e tutte le sue regole associate vengono eliminati dopo l'attivazione.</li></ul></td>
</tr>
</tbody></table>

Il seguente comando è un esempio di creazione di un trigger che viene attivato una volta il 25 dicembre 2019, 12:30:00 UTC. Ogni evento trigger ha i parametri `name=Odin` e `place=Asgard`. Dopo la sua attivazione, il trigger e tutte le regole associate vengono eliminati.

```
ibmcloud fn trigger create fireOnce \
  --feed /whisk.system/alarms/once \
  --param date "2019-12-25T12:30:00.000Z" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param deleteAfterFire "rules"
```
{: pre}


## Attivazione periodica di un evento trigger secondo una pianificazione basata sull'intervallo.
{: #pkg_alarms_int}

Il feed `/whisk.system/alarms/interval` configura il servizio Allarme per attivare un evento trigger secondo una pianificazione basata sull'intervallo. Per creare un allarme basato sull'intervallo, immetti il seguente comando.
```
ibmcloud fn trigger create interval --feed /whisk.system/alarms/interval --param minutes "<minutes>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}
</br>

<table>
<caption>Descrizione dei componenti del comando <code>trigger create interval</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del comando <code>trigger create interval</code></th>
</thead>
<tbody>
<tr>
<td><code>interval</code></td>
<td>Il tipo di trigger di allarme che stai creando.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/interval</code></td>
<td>Il percorso del file del pacchetto di allarmi per il feed di intervallo.</td>
</tr>
<tr>
<td><code>--param minutes</code></td>
<td>Sostituisci <code>&lt;minutes&gt;</code> con un numero intero che rappresenta la durata dell'intervallo, in minuti, tra le attivazioni del trigger.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>(Facoltativo) Sostituisci <code>&lt;key&gt;</code> e <code>&lt;value&gt;</code> con i parametri del trigger quando il trigger viene attivato.</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>(Facoltativo) Sostituisci <code>&lt;startDate&gt;</code> con la data in cui prevedi di attivare il primo trigger. Le attivazioni successive si verificano in base alla durata dell'intervallo specificata dal parametro dei minuti. Questo parametro supporta un valore intero o stringa. Il valore intero rappresenta il numero di millisecondi a partire dal `1° gennaio 1970 00:00:00` UTC e il valore stringa deve essere nel <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">formato ISO 8601</a>.</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>(Facoltativo) Sostituisci <code>&lt;stopDate&gt;</code> con la data in cui prevedi di arrestare il trigger. Una volta raggiunta questa data, i trigger non vengono attivati. Questo parametro supporta un valore intero o stringa. Il valore intero rappresenta il numero di millisecondi a partire dal `1° gennaio 1970 00:00:00` UTC e il valore stringa deve essere nel <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">formato ISO 8601</a>.</td>
</tr>
</tbody></table>

Il seguente esempio crea un trigger che viene attivato una volta ogni 2 minuti. Il trigger viene attivato il prima possibile e interrompe l'attivazione in data `January 31, 2019, 23:59:00` UTC. Ogni evento trigger ha i parametri `name=Odin` e `place=Asgard`.

```
ibmcloud fn trigger create interval \
  --feed /whisk.system/alarms/interval \
  --param minutes 2 \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}



## Attivazione di un trigger in base a una pianificazione basata sul tempo utilizzando cron
{: #pkg_alarms_cron}

Il feed `/whisk.system/alarms/alarm` configura il servizio Allarme per attivare un evento trigger a una frequenza specifica. Per creare un allarme basato sul tempo, immetti il seguente comando.
```
ibmcloud fn trigger create periodic --feed /whisk.system/alarms/alarm --param cron "<cron>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}
</br>

<table>
<caption>Descrizione dei componenti del comando <code>trigger create periodic</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del comando <code>trigger create periodic</code></th>
</thead>
<tbody>
<tr>
<td><code>periodic</code></td>
<td>Il tipo di trigger di allarme che stai creando.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/alarm</code></td>
<td>Il percorso del file del pacchetto di allarmi per il feed di allarme periodico.</td>
</tr>
<tr>
<td><code>--param cron</code></td>
<td>Sostituisci <code>&lt;cron&gt;</code> con una stringa che indica quando attivare il trigger in Coordinated Universal Time (UTC). La stringa è basata sulla <a href="http://crontab.org">sintassi crontab di UNIX</a> ed è una sequenza di massimo cinque campi. I campi sono separati da spazi nel formato <code>X X X X X</code>. Le seguenti stringhe sono esempi che utilizzano durate variabili di frequenza.<ul><li><code>\* \* \* \* \*</code> - Il trigger viene attivato all'inizio di ogni minuto.</li><li><code>0 \* \* \* \*</code> - Il trigger viene attivato all'inizio di ogni ora.</li><li><code>0 \*/2 \* \* \*</code> - Il trigger viene attivato ogni 2 ore (ossia, 02:00:00, 04:00:00,...).</li><li><code>0 9 8 \* \*</code> - Il trigger viene attivato alle 9:00:00AM (UTC) dell'ottavo giorno di ogni mese.</li></ul></td>
</tr>
<tr>
<tr>
<td><code>--param timezone</code></td>
<td>(Facoltativo) Sostituisci <code>&lt;timezone&gt;</code> con una stringa che specifica il fuso orario. L'ora reale di attivazione del trigger viene modificata in base al fuso orario specificato. Se il fuso orario non è valido, viene generato un errore. Puoi controllare tutti i fusi orari disponibili nel [sito web Moment Timezone](http://momentjs.com/timezone/docs/#/data-loading/getting-zone-names){: external}.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>(Facoltativo) Sostituisci <code>&lt;key&gt;</code> e <code>&lt;value&gt;</code> con i parametri del trigger quando il trigger viene attivato.</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>(Facoltativo) Sostituisci <code>&lt;startDate&gt;</code> con la data in cui intendi attivare il primo trigger. Le attivazioni successive si verificano in base alla durata dell'intervallo specificata dal parametro dei minuti. Questo parametro supporta un valore intero o stringa. Il valore intero rappresenta il numero di millisecondi a partire dal `1° gennaio 1970 00:00:00` UTC e il valore stringa deve essere nel <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">formato ISO 8601</a>.</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>(Facoltativo) Sostituisci <code>&lt;stopDate&gt;</code> con la data in cui vuoi interrompere l'esecuzione del trigger. Una volta raggiunta questa data, i trigger non vengono attivati. Questo parametro supporta un valore intero o stringa. Il valore intero rappresenta il numero di millisecondi a partire dal `1° gennaio 1970 00:00:00` UTC e il valore stringa deve essere nel <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">formato ISO 8601</a>.</td>
</tr>
</tbody></table>

Il seguente comando è un esempio di creazione di un trigger che viene attivato una volta ogni 2 minuti. Il trigger non viene attivato fino
alla data `January 1, 2019, 00:00:00` UTC e interrompe l'attivazione in data `January 31, 2019, 23:59:00` UTC. Ogni evento trigger ha i parametri `name=Odin` e `place=Asgard`.

```
ibmcloud fn trigger create periodic \
  --feed /whisk.system/alarms/alarm \
  --param cron "*/2 * * * *" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param startDate "2019-01-01T00:00:00.000Z" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}




