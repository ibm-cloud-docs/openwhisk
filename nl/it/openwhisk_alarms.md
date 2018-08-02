---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Allarmi
{: #openwhisk_catalog_alarm}

Il pacchetto `/whisk.system/alarms` può essere utilizzato per attivare un trigger con una frequenza specifica. Gli allarmi sono utili per la configurazione di lavori o attività ricorrenti, come la chiamata oraria di un'azione di backup del sistema.
{: shortdesc}

Il pacchetto include i seguenti feed.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/alarms` | pacchetto | - | Allarmi e utilità periodica. |
| `/whisk.system/alarms/interval` | feed | minutes, trigger_payload, startDate, stopDate | Attivare l'evento trigger secondo una pianificazione basata sull'intervallo. |
| `/whisk.system/alarms/once` | feed | date, trigger_payload, deleteAfterFire | Attivare l'evento trigger una volta in una data specifica. |
| `/whisk.system/alarms/alarm` | feed | cron, trigger_payload, startDate, stopDate | Attivare l'evento trigger secondo una pianificazione basata sul tempo utilizzando cron. |


## Attivazione periodica di un evento trigger secondo una pianificazione basata sull'intervallo.
{: #openwhisk_catalog_alarm_fire}

Il feed `/whisk.system/alarms/interval` configura il servizio Allarme per attivare un evento trigger secondo una pianificazione basata sull'intervallo. I parametri sono i seguenti:

- `minutes` (*obbligatorio*): un numero intero che rappresenta la lunghezza dell'intervallo (in minuti) tra le attivazioni del trigger.
- `trigger_payload` (*facoltativo*): il valore di questo parametro diventa il contenuto del trigger ogni volta che il trigger viene attivato.
- `startDate` (*facoltativo*): la data in cui verrà attivato il primo trigger.  Le attivazioni successive si verificheranno in base alla lunghezza dell'intervallo specificata dal parametro `minutes`.
- `stopDate` (*facoltativo*): la data in cui il trigger terminerà l'esecuzione. Una volta raggiunta questa data, i trigger non verranno più attivati.

  **Nota**: i parametri `startDate` e `stopDate` supportano un valore intero o stringa. Il valore intero rappresenta il numero di millisecondi a partire dal 1° gennaio 1970 00:00:00 UTC e il valore stringa deve essere nel formato ISO 8601 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15).

Il seguente esempio crea un trigger che viene attivato una volta ogni 2 minuti. Il trigger viene attivato il prima possibile e terminerà l'attivazione il 31 gennaio 2019, 23:59:00 UTC.

  ```
  ibmcloud fn trigger create interval \
    --feed /whisk.system/alarms/interval \
    --param minutes 2 \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

Ogni evento generato include dei parametri, che sono le proprietà specificate dal valore `trigger_payload`. In questo caso, ogni evento trigger ha i parametri `name=Odin` e `place=Asgard`.

## Attivazione unica di un evento trigger

Il feed `/whisk.system/alarms/once` configura il servizio Allarme per attivare un evento trigger in una data specificata. I parametri sono i seguenti:

- `date` (*obbligatorio*): la data in cui il trigger verrà attivato. Il trigger verrà attivato solo una volta all'ora specificata.

  **Nota**: il parametro `date` supporta un valore intero o stringa. Il valore intero rappresenta il numero di millisecondi
  a partire dal 1° gennaio 1970 00:00:00 UTC e il valore stringa deve essere nel formato ISO 8601 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15).

- `trigger_payload` (*facoltativo*): il valore di questo parametro diventa il contenuto del trigger quando il trigger viene attivato.

- `deleteAfterFire` (*facoltativo*, valore predefinito:false): il valore di questo parametro determina se il trigger e potenzialmente tutte le sue regole associate verranno eliminati dopo l'attivazione del trigger.
  - `false`: non verrà eseguita alcuna azione dopo l'attivazione del trigger.
  - `true`: il trigger verrà eliminato dopo l'attivazione.
  - `rules`: il trigger e tutte le regole associate verranno eliminati dopo la sua attivazione.

Di seguito è riportato un esempio di creazione di un trigger che verrà attivato una volta il 25 dicembre 2019, 12:30:00 UTC. Dopo l'attivazione, il trigger verrà eliminato insieme a tutte le sue regole associate.

  ```
  ibmcloud fn trigger create fireOnce \
    --feed /whisk.system/alarms/once \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param date "2019-12-25T12:30:00.000Z" \
    --param deleteAfterFire "rules"
  ```
  {: pre}

## Attivazione di un trigger secondo una pianificazione basata sul tempo utilizzando cron

Il feed `/whisk.system/alarms/alarm` configura il servizio Allarme per attivare un evento trigger a una frequenza specifica. I parametri sono i seguenti:

- `cron` (*obbligatorio*): una stringa, basata sulla sintassi crontab UNIX, che indica quando attivare il trigger in UTC (Coordinated Universal Time). La stringa è una sequenza di cinque campi separati da spazi: `X X X X X`.
Per ulteriori informazioni, vedi: http://crontab.org. Le seguenti stringhe sono esempi che utilizzano durate variabili di frequenza.

  - `* * * * *`: il trigger viene attivato all'inizio di ogni minuto.
  - `0 * * * *`: il trigger viene attivato all'inizio di ogni ora.
  - `0 */2 * * *`: il trigger viene attivato ogni 2 ore (ossia, 02:00:00, 04:00:00, ...).
  - `0 9 8 * *`: il trigger viene attivato alle 9:00:00AM (UTC) nell'ottavo giorno di ogni mese.

  **Note**: il parametro `cron` supporta solo 5 campi.

- `trigger_payload` (*facoltativo*): il valore di questo parametro diventa il contenuto del trigger ogni volta che il trigger viene attivato.

- `startDate` (*facoltativo*): la data in cui il trigger avvierà l'esecuzione. Il trigger viene attivato in base alla pianificazione specificata dal parametro cron.

- `stopDate` (*facoltativo*): la data in cui il trigger terminerà l'esecuzione. Una volta raggiunta questa data, i trigger non verranno più attivati.

  **Nota**: i parametri `startDate` e `stopDate` supportano un valore intero o stringa. Il valore intero rappresenta il numero di millisecondi a partire dal 1° gennaio 1970 00:00:00 UTC e il valore stringa deve essere nel formato ISO 8601 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15).

Di seguito è riportato un esempio di creazione di un trigger che viene attivato ogni 2 minuti con i valori `name` e `place` nell'evento trigger. Il trigger non verrà attivato fino al
1° gennaio 2019, 00:00:00 UTC e terminerà l'attivazione il 31 gennaio 2019, 23:59:00 UTC.

  ```
  ibmcloud fn trigger create periodic \
    --feed /whisk.system/alarms/alarm \
    --param cron "*/2 * * * *" \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param startDate "2019-01-01T00:00:00.000Z" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

 **Nota**: il parametro `maxTriggers` è obsoleto e verrà rimosso a breve. Per arrestare il trigger, utilizza il parametro `stopDate`.
