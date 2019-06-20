---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: alarms, serverless

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


# Allarmi
{: #pkg_alarms}

Il pacchetto `/whisk.system/alarms` può essere utilizzato per attivare un trigger con una frequenza specifica. Gli allarmi sono utili per la configurazione di lavori o attività ricorrenti, come la chiamata oraria di un'azione di backup del sistema.
{: shortdesc}

Il pacchetto include i seguenti feed.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/alarms` | pacchetto | - | Allarmi e utilità periodica. |
| `/whisk.system/alarms/once` | feed | date, trigger_payload, deleteAfterFire | Attivare l'evento trigger una volta in una data specifica. |
| `/whisk.system/alarms/interval` | feed | minutes, trigger_payload, startDate, stopDate | Attivare l'evento trigger in base a una pianificazione basata sull'intervallo. |
| `/whisk.system/alarms/alarm` | feed | cron, timezone, trigger_payload, startDate, stopDate | Attivare l'evento trigger secondo una pianificazione basata sul tempo utilizzando cron. |



## Attivazione unica di un evento trigger
{: #pkg_alarms_one}

Il feed `/whisk.system/alarms/once` configura il servizio Allarme per attivare un evento trigger una volta in una data specificata. Per creare un allarme a singola attivazione, esegui questo comando:
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
<td>Il filepath del pacchetto degli allarmi per il feed fireOnce.</td>
</tr>
<tr>
<td><code>--param date</code></td>
<td>Sostituisci <code> &lt;date&gt;</code> con la data in cui il trigger verrà attivato. Il trigger viene attivato una sola volta all'ora specificata. Nota: il parametro `date` supporta un valore numerico intero o stringa. Il valore numerico intero rappresenta il numero di millisecondi a partire dal 1° gennaio 1970 00:00:00 UTC e la stringa deve essere nel <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">formato ISO 8601</a>.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>Facoltativo: sostituisci <code>&lt;key&gt;</code> e <code>&lt;value&gt;</code> con i parametri del trigger quando il trigger viene attivato.</td>
</tr>
<tr>
<td><code>--param deleteAfterFire</code></td>
<td>Facoltativo: indica se il trigger e le eventuali regole associate sono eliminati dopo che il trigger è stato attivato. Sostituisci <code>&lt;delete_option&gt;</code> con uno dei seguenti:<ul><li><code>false</code> (default): non viene eseguita alcuna azione dopo l'attivazione del trigger.</li><li><code>true</code>: il trigger viene eliminato dopo essere stato attivato.</li><li><code>rules</code>: il trigger e tutte le sue regole associate vengono eliminati dopo l'attivazione del trigger.</li></ul></td>
</tr>
</tbody></table>

Di seguito è riportato un esempio di creazione di un trigger che verrà attivato una volta il 25 dicembre 2019, 12:30:00 UTC. Ogni evento trigger ha i parametri `name=Odin` e `place=Asgard`. Dopo la sua attivazione, il trigger e tutte le regole associate verranno eliminati.

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

Il feed `/whisk.system/alarms/interval` configura il servizio Allarme per attivare un evento trigger secondo una pianificazione basata sull'intervallo. Per creare un allarme basato sull'intervallo, esegui questo comando:
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
<td>Il filepath del pacchetto degli allarmi per il feed di intervallo.</td>
</tr>
<tr>
<td><code>--param minutes</code></td>
<td>Sostituisci <code>&lt;minutes&gt;</code> con un numero intero che rappresenta la lunghezza dell'intervallo, in minuti, tra le attivazioni del trigger.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>Facoltativo: sostituisci <code>&lt;key&gt;</code> e <code>&lt;value&gt;</code> con i parametri del trigger quando il trigger viene attivato.</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>Facoltativo: sostituisci <code>&lt;startDate&gt;</code> con la data in cui verrà attivato il primo trigger. Le attivazioni successive si verificano in base alla lunghezza dell'intervallo specificato dal parametro dei minuti. Nota: questo parametro supporta un valore numerico intero o stringa. Il valore numerico intero rappresenta il numero di millisecondi a partire dal 1° gennaio 1970 00:00:00 UTC e la stringa deve essere nel <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">formato ISO 8601</a>.</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>Facoltativo: sostituisci <code>&lt;stopDate&gt;</code> con la data in cui l'esecuzione del trigger verrà arrestata. Una volta raggiunta questa data, i trigger non vengono attivati. Nota: questo parametro supporta un valore numerico intero o stringa. Il valore numerico intero rappresenta il numero di millisecondi a partire dal 1° gennaio 1970 00:00:00 UTC e la stringa deve essere nel <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">formato ISO 8601</a>.</td>
</tr>
</tbody></table>

Il seguente esempio crea un trigger che viene attivato una volta ogni 2 minuti. Il trigger viene attivato il prima possibile e terminerà l'attivazione il 31 gennaio 2019, 23:59:00 UTC. Ogni evento trigger ha i parametri `name=Odin` e `place=Asgard`.

```
ibmcloud fn trigger create interval \
  --feed /whisk.system/alarms/interval \
  --param minutes 2 \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}



## Attivazione di un trigger secondo una pianificazione basata sul tempo utilizzando cron
{: #pkg_alarms_cron}

Il feed `/whisk.system/alarms/alarm` configura il servizio Allarme per attivare un evento trigger a una frequenza specifica. Per creare un allarme su base temporale, esegui questo comando:
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
<td>Il filepath del pacchetto degli allarmi per il feed di allarme periodico.</td>
</tr>
<tr>
<td><code>--param cron</code></td>
<td>Sostituisci <code>&lt;cron&gt;</code> con una stringa che indica quando attivare il trigger in Coordinated Universal Time (UTC). La stringa è basata sulla <a href="http://crontab.org">sintassi crontab di UNIX</a> ed è una sequenza di massimo 5 campi. I campi sono separati da spazi nel formato <code>X X X X X</code>. Le seguenti stringhe sono esempi che utilizzano durate variabili di frequenza.<ul><li><code>\* \* \* \* \*</code>: il trigger viene attivato all'inizio di ogni minuto.</li><li><code>0 \* \* \* \*</code>: il trigger viene attivato all'inizio di ogni ora.</li><li><code>0 \*/2 \* \* \*</code>: il trigger viene attivato ogni 2 ore (ossia 02:00:00, 04:00:00, ...).</li><li><code>0 9 8 \* \*</code>: il trigger viene attivato alle 9:00:00AM (UTC) dell'ottavo giorno di ogni mese.</li></ul></td>
</tr>
<tr>
<tr>
<td><code>--param timezone</code></td>
<td>Facoltativo: sostituisci <code>&lt;timezone&gt;</code> con una stringa che specifica il fuso orario. L'ora reale di attivazione del trigger sarà modificata in base al fuso orario specificato. Se il fuso orario non è valido, viene generato un errore. Puoi controllare tutti i fusi orari disponibili nel [sito Web Moment Timezone](http://momentjs.com/timezone/docs/#/data-loading/getting-zone-names).</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>Facoltativo: sostituisci <code>&lt;key&gt;</code> e <code>&lt;value&gt;</code> con i parametri del trigger quando il trigger viene attivato.</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>Facoltativo: sostituisci <code>&lt;startDate&gt;</code> con la data in cui verrà attivato il primo trigger. Le attivazioni successive si verificano in base alla lunghezza dell'intervallo specificato dal parametro dei minuti. Nota: questo parametro supporta un valore numerico intero o stringa. Il valore numerico intero rappresenta il numero di millisecondi a partire dal 1° gennaio 1970 00:00:00 UTC e la stringa deve essere nel <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">formato ISO 8601</a>.</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>Facoltativo: sostituisci <code>&lt;stopDate&gt;</code> con la data in cui l'esecuzione del trigger verrà arrestata. Una volta raggiunta questa data, i trigger non vengono attivati. Nota: questo parametro supporta un valore numerico intero o stringa. Il valore numerico intero rappresenta il numero di millisecondi a partire dal 1° gennaio 1970 00:00:00 UTC e la stringa deve essere nel <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">formato ISO 8601</a>.</td>
</tr>
</tbody></table>

Di seguito è riportato un esempio di creazione di un trigger che viene attivato ogni 2 minuti. Il trigger non verrà attivato fino al
1° gennaio 2019, 00:00:00 UTC e terminerà l'attivazione il 31 gennaio 2019, 23:59:00 UTC. Ogni evento trigger ha i parametri `name=Odin` e `place=Asgard`.

```
ibmcloud fn trigger create periodic \
  --feed /whisk.system/alarms/alarm \
  --param cron "*/2 * * * *" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param startDate "2019-01-01T00:00:00.000Z" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}


