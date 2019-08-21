---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: managing actions, manage, activation, action logs, changing runtime, delete

subcollection: cloud-functions-cli-plugin

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




# CLI di {{site.data.keyword.openwhisk_short}}
{: #functions-cli}

Esegui questi comandi per gestire le entità che costituiscono le tue funzioni.
{: shortdec}

<br />

## Comandi azione
{: #cli_action}



### `ibmcloud fn action create`
{: #cli_action_create}

Crea un'azione.

```
ibmcloud fn action create ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web yes|true|raw|no|false] [--web-secure SECRET]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>Le annotazioni vengono indicate in formato `KEY` `VALUE`. Per includere più annotazioni, specifica queste opzioni per ciascuna annotazione. Questo indicatore è facoltativo.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Un file JSON che contiene l'annotazione in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

   <dt>`ACTION_NAME`</dt>
   <dd>Il nome dell'azione. Per includere l'azione in un pacchetto, immette il nome nel formato `PACKAGE_NAME`/`ACTION_NAME`. Questo valore è obbligatorio. </dd>

   <dt>`APP_FILE`</dt>
   <dd>Il percorso del file delle applicazioni o del pacchetto da eseguire come azione. Questa opzione è obbligatoria.</dd>
   
   <dt>`--copy`</dt>
   <dd>Considera l'azione come nome di un'azione esistente.</dd>

   <dt>`--docker` `DOCKER_HUB_USERNAME`/`IMAGE_NAME`</dt>
   <dd>Il nome utente Docker Hub e il nome dell'immagine Docker del Docker Hub utilizzati per eseguire l'azione. Questo indicatore è obbligatorio per la creazione di azioni da immagini Docker.</dd>

   <dt>`--kind` `LANGUAGE`</dt>
   <dd>Il runtime per la tua applicazione. Questo indicatore è facoltativo. Se non viene specificato alcun `VALUE`, viene utilizzata la versione predefinita del runtime rilevato.
     Valori (`VALUES`) possibili per l'opzione `--kind`.
     <table>
  <tr>
    <th>Linguaggio</th>
    <th>Identificativo del tipo</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (predefinito), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (predefinito)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (predefinito)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (predefinito)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (predefinito)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (predefinito)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (predefinito)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (predefinito)</td>
  </tr>
  <tr>
    <td>Sono supportati altri linguaggi utilizzando le azioni Docker.</td>
  </tr>
</table>
{: caption="Tabella 1. Runtime supportati" caption-side="top"}
       </dd>

   <dt>`--logsize` `LIMIT`, `-l` `LIMIT`</dt>
   <dd>Dimensione massima del log, in MB, per l'azione. Il valore predefinito è 10 MB.</dd>

   <dt>`--main` `ENTRY_METHOD_NAME`</dt>
   <dd>Se il metodo di immissione dell'azione non è `main`, specifica il nome personalizzato. Questo indicatore è obbligatorio se il metodo di immissione non è `main`. Per alcuni runtime, come Java, il nome deve essere il metodo completo.</dd>

   <dt>`--native`</dt>
   <dd>Puoi utilizzare l'argomento `--native` come abbreviazione per `--docker openwhisk/dockerskeleton`. Utilizzando questo argomento, puoi creare e distribuire un eseguibile che viene eseguito all'interno dell'SDK di azione Docker standard.
       <ol><li>Quando crei un'immagine Docker, viene creato un eseguibile all'interno del contenitore in `/action/exec`. Copia il file `/action/exec` nel tuo file system locale e comprimilo in `exec.zip`.</li>
       <li>Crea un'azione Docker che riceve l'eseguibile come dati di inizializzazione. L'argomento `--native` sostituisce l'argomento `--docker openwhisk/dockerskeleton`.</li></ol>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Valori (`VALUES`) del parametro in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Un file JSON che contiene chiavi (`KEYS`) e valori (`VALUES`) parametro. Questo indicatore è facoltativo.</dd>

   <dt>`--sequence` `ACTION_NAME`, `ACTION_NAME`</dt>
   <dd>Crea una sequenza di azioni e includi i nomi delle azioni correlate. Separa gli `ACTION_NAME` tramite virgole.</dd>

   <dt>`--timeout` `LIMIT`, `-t` `LIMIT`</dt>
   <dd>Il limite (`LIMIT`) di timeout in millisecondi. Il valore predefinito è di 60.000 millisecondi. L'azione viene terminata al raggiungimento del timeout.</dd>

   <dt>`--web yes|true|raw|no|false`</dt>
   <dd>Considera l'azione come un'azione Web, un'azione Web HTTP non elaborata o un'azione standard. Specifica `yes` o `true` per un'azione Web, `raw` per un'azione Web HTTP non elaborata o `no` o `false` per un'azione standard. Per proteggere la tua azione Web, includi anche l'opzione `--web-secure`.</dd>

   <dt>`--web-secure` `SECRET`</dt>
   <dd>Proteggi l'azione Web. Il `VALUE` per `SECRET` può essere `true`, `false` o qualsiasi stringa. Questa opzione può essere utilizzata solo con l'opzione `--web`.</dd>
   </dl>

<br />**Esempio**

  ```
  ibmcloud fn action create hello folder/hello_world.js
  ```
  {: pre}

  **Output**
  ```
  ok: created hello
  ```
  {: screen}


<br />

### `ibmcloud fn action delete`
{: #cli_action_delete}

Puoi effettuare una pulizia del tuo spazio dei nomi eliminando le azioni che non desideri più utilizzare.

```
ibmcloud fn action delete ACTION_NAME
```
{: pre}

<br />**Esempio**

  ```
  ibmcloud fn action delete helloworld
  ```
  {: pre}

  **Output**
  ```
  ok: deleted hello
  ```
  {: screen}


<br />

### `ibmcloud fn action get`
{: #cli_action_get}

Ottieni i metadati che descrivono un'azione specifica.

```
ibmcloud fn action get ACTION_NAME [--save] [--save-as FILENAME] [--summary] [--url]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>Il nome di un'azione. Questo valore è obbligatorio.</dd>

   <dt>`--save`</dt>
   <dd>Puoi ottenere e salvare localmente il codice associato a un'azione esistente, ad eccezione delle sequenze e delle azioni Docker. Il `FILENAME` corrisponde a un nome azione esistente nella directory di lavoro corrente e l'estensione file corrisponde al tipo di azione. Ad esempio, se il codice di azione è un file di archivio, viene utilizzata l'estensione .zip. Questo indicatore è facoltativo.</dd>

  <dt>`--save-as` `FILENAME`</dt>
  <dd>Salva il codice per le azioni in un file con nome personalizzato fornendo percorso file, `FILENAME` ed estensione. Questo indicatore è facoltativo.</dd>

  <dt>`--summary`</dt>
  <dd>Ottieni un riepilogo dei dettagli dell'azione. I parametri con prefisso "*" sono associati, quelli con prefisso "**" sono associati e finalizzati. Questo indicatore è facoltativo.</dd>

  <dt>`--url`</dt>
  <dd>Ottieni solo l'URL dell'azione. Questo indicatore è facoltativo.</dd>
   </dl>

<br />**Esempio**

```
ibmcloud fn action get hello
```
{: pre}

**Output**
```
ok: got action hello
{
    "namespace": "user@email.com",
    "name": "hello",
    "version": "0.0.1",
    "exec": {
        "kind": "nodejs:6",
        "binary": false
    },
    "annotations": [
        {
            "KEY": "exec",
            "VALUE": "nodejs:6"
        }
    ],
    "LIMIT s": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}




<br />

### `ibmcloud fn action invoke`
{: #cli_action_invoke}

Esegui un'azione per testarla.

```
ibmcloud fn action invoke ACTION_NAME [--blocking] [--param KEY VALUE] [--param-file FILE] [--result]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>Il nome dell'azione. Questo valore è obbligatorio. </dd>

   <dt>`--blocking, -b`</dt>
   <dd>Le chiamate bloccanti utilizzano uno stile di richiesta e risposta per attendere che il risultato dell'attivazione sia disponibile. Il periodo di attesa è inferiore a 60 secondi o il [valore (`VALUE`) del limite (`LIMIT`) di tempo](/docs/openwhisk?topic=cloud-functions-limits) dell'azione. Questo indicatore è facoltativo.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Valori (`VALUES`) del parametro in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Un file JSON che contiene chiavi (`KEYS`) e valori (`VALUES`) parametro. Questo indicatore è facoltativo.</dd>

   <dt>`--result, -r`</dt>
   <dd>Il risultato del codice dell'applicazione viene visualizzato come output del comando. Se questa opzione non viene specificata, viene visualizzato l'ID di attivazione. Quando questa opzione è specificata, la chiamata è bloccante. Questo indicatore è facoltativo.</dd>

   </dl>

<br />**Esempio**
```
ibmcloud fn action invoke hello --blocking
```
{: pre}


<br />

### `ibmcloud fn action list`
{: #cli_action_list}

Elenco di tutte le azioni che hai creato o di un numero specifico di azioni.

```
ibmcloud fn action list ACTION_NAME [--limit NUMBER_OF_ACTIONS] [--name-sort] [--skip NUMBER_OF_ACTIONS]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>Nome di un pacchetto di azioni. Questo valore è facoltativo. Se non viene specificato, vengono elencate tutte le azioni.</dd>

   <dt>`--limit` `NUMBER_OF_ACTIONS`, -l `NUMBER_OF_ACTIONS`</dt>
   <dd>Elenca il numero specificato di azioni. Il valore predefinito è 30 azioni.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Ordina l'elenco di azioni restituite in base al nome; diversamente, l'elenco viene ordinato per data di creazione.</dd>

   <dt>`--skip` `NUMBER_OF_ACTIONS`, -s `NUMBER_OF_ACTIONS`</dt>
   <dd>Escludi dal risultato il numero specificato di ultime azioni create.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn action list
  ```
  {: pre}


<br />

### `ibmcloud fn action update`
{: #cli_action_update}

Aggiorna un'azione o l'applicazione all'interno di un'azione.

Quando aggiorni i parametri per un pacchetto, un'azione o un trigger, devi specificare tutti i parametri creati in precedenza. Altrimenti, i parametri precedentemente creati vengono rimossi. Per i pacchetti, vengono rimossi anche tutti i servizi associati al pacchetto, quindi dopo aver aggiornato gli altri parametri devi eseguire di nuovo il [bind dei servizi](/docs/openwhisk?topic=cloud-functions-services) al tuo pacchetto.
{: important}

```
ibmcloud fn action update ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web yes|true|raw|no|false] [--web-secure SECRET]
```
{: pre}

<br />**Opzioni del comando**

  <dl>
  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>Le annotazioni vengono indicate in formato `KEY` `VALUE`. Per includere più annotazioni, specifica queste opzioni per ciascuna annotazione. Questo indicatore è facoltativo.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>Un file JSON che contiene l'annotazione in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

  <dt>`ACTION_NAME`</dt>
  <dd>Il nome dell'azione. Per includere l'azione in un pacchetto, immette il nome nel formato `PACKAGE_NAME`/`ACTION_NAME`. Questo valore è obbligatorio. </dd>

  <dt>`APP_FILE`</dt>
  <dd>Il percorso del file delle applicazioni o del pacchetto da eseguire come azione. Questa opzione è necessaria se desideri aggiornare la tua applicazione all'interno dell'azione..</dd>

  <dt>`--copy`</dt>
  <dd>Considera l'azione come nome di un'azione esistente.</dd>

  <dt>`--docker DOCKER_HUB_USERNAME/IMAGE_NAME`</dt>
  <dd>Il nome utente Docker Hub e il nome dell'immagine Docker del Docker Hub utilizzati per eseguire l'azione. Questo indicatore è obbligatorio per la creazione di azioni da immagini Docker.</dd>

  <dt>`--kind LANGUAGE`</dt>
  <dd>Il runtime per la tua applicazione. Questo indicatore è facoltativo. Se non si specifica alcun VALORE, viene utilizzata la versione predefinita del runtime rilevato.
    Valori possibili per l'opzione `--kind`.
    <table>
  <tr>
    <th>Linguaggio</th>
    <th>Identificativo del tipo</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (predefinito), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (predefinito)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (predefinito)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (predefinito)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (predefinito)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (predefinito)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (predefinito)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (predefinito)</td>
  </tr>
  <tr>
    <td>Sono supportati altri linguaggi utilizzando le azioni Docker.</td>
  </tr>
</table>
{: caption="Tabella 1. Runtime supportati" caption-side="top"}
      </dd>

  <dt>`--logsize` `LIMIT`, `-l` `LIMIT`</dt>
  <dd>Dimensione massima del log, in MB, per l'azione. Il valore predefinito è 10 MB.</dd>

  <dt>`--main ENTRY_METHOD_NAME`</dt>
  <dd>Se il metodo di immissione dell'azione non è `main`, specifica il nome personalizzato. Questo indicatore è obbligatorio se il metodo di immissione non è `main`. Per alcuni runtime, come Java, il nome deve essere il metodo completo.</dd>

  <dt>`--native`</dt>
  <dd>Puoi utilizzare l'argomento `--native` come abbreviazione per `--docker openwhisk/dockerskeleton`. Utilizzando l'argomento, puoi creare e distribuire un eseguibile che viene eseguito all'interno dell'SDK di azione Docker standard.
      <ol><li>Quando crei un'immagine Docker, viene creato un eseguibile all'interno del contenitore in `/action/exec`. Copia il file `/action/exec` nel tuo file system locale e comprimilo in `exec.zip`.</li>
      <li>Crea un'azione Docker che riceve l'eseguibile come dati di inizializzazione. L'argomento `--native` sostituisce l'argomento `--docker openwhisk/dockerskeleton`.</li></ol>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>Valori (`VALUES`) del parametro in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>Un file JSON che contiene chiavi (`KEYS`) e valori (`VALUES`) dei parametri. Questo indicatore è facoltativo.</dd>

  <dt>`--sequence` `ACTION_NAME`, `ACTION_NAME`</dt>
  <dd>Crea una sequenza di azioni specificando il nome delle azioni correlate.</dd>

  <dt>`--timeout` `LIMIT`, `-t` `LIMIT`</dt>
  <dd>Il limite di timeout in millisecondi. Il valore predefinito è di 60.000 millisecondi. L'azione viene terminata al raggiungimento del timeout.</dd>

  <dt>`--web yes|true|raw|no|false`</dt>
  <dd>Considera l'azione come un'azione Web, un'azione Web HTTP non elaborata o un'azione standard. Specifica `yes` o `true` per un'azione Web, `raw` per un'azione Web HTTP non elaborata o `no` o `false` per un'azione standard. Per proteggere la tua azione Web, includi anche l'opzione `--web-secure`.</dd>

  <dt>`--web-secure` `SECRET`</dt>
  <dd>Proteggi l'azione Web. Il `VALUE` per `SECRET` può essere `true`, `false` o qualsiasi stringa. Questa opzione può essere utilizzata solo con l'opzione `--web`.</dd>
  </dl>

<br />**Esempio**
```
ibmcloud fn action update hello folder/hello_world.js
```
{: pre}




<br /><br />
## Comandi di attivazione
{: #cli_activation}


### `ibmcloud fn activation get`
{: #cli_activation_get}

Ottieni i metadati che descrivono un'attivazione specifica.

```
ibmcloud fn activation get [ACTIVATION_ID] [FIELD_FILTER] [--last] [--summary]
```
{: pre}


<br />**Opzioni del comando**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>ID di un'attivazione specifica. Utilizza `ibmcloud fn activation list` per richiamare un elenco di ID disponibili. Questo valore è obbligatorio, a meno che non venga specificata l'opzione `--last` o `-l`.</dd>

  <dt>`FIELD_FILTER`</dt>
  <dd>Un campo dei metadati da cui visualizzare informazioni. Ad esempio, per visualizzare il campo dei log, esegui `ibmcloud fn activation get ACTIVATION_ID logs`. Questo valore è facoltativo.</dd>

  <dt>`--last, -l`</dt>
  <dd>Visualizza i metadati per l'attivazione più recente. Questo indicatore è facoltativo.</dd>

  <dt>`--summary, -s`</dt>
  <dd>Visualizza la risposta del risultato solo dai dettagli di attivazione. Questo indicatore è facoltativo.</dd>
  </dl>


<br />**Esempio**
```
ibmcloud fn activation get 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />

### `ibmcloud fn activation list`
{: #cli_activation_list}

Elenca tutti gli ID di attivazione per tutte le azioni di un pacchetto.

```
ibmcloud fn activation list [--full] [--limit NUMBER_OF_ACTIVATIONS] [--since UNIX_EPOCH_TIME] [--skip NUMBER_OF_ACTIVATIONS] [--upto UNIX_EPOCH_TIME]
```
{: pre}


<br />**Opzioni del comando**

  <dl>
  <dt>`--full, -f`</dt>
  <dd>Visualizza la descrizione completa dell'attivazione.</dd>

  <dt>`--limit` `NUMBER_OF_ACTIVATIONS`, `-l` `NUMBER_OF_ACTIVATIONS`</dt>
  <dd>Elenca il numero specificato di attivazioni. Il valore predefinito è di 30 attivazioni e il massimo è di 200 attivazioni.</dd>

  <dt>`--since` `UNIX_EPOCH_TIME`</dt>
  <dd>Elenca le attivazioni create dalla data specificata. La durata è misurata in millisecondi a partire dal 1° gennaio 1970. Esempio: `1560371263` è il 12 giugno 2019 08:27:43 UTC.</dd>

  <dt>`--skip` `NUMBER_OF_ACTIVATIONS`, -s `NUMBER_OF_ACTIVATIONS`</dt>
  <dd>Escludi dal risultato il numero specificato di ultime attivazioni.</dd>

  <dt>`--upto` `UNIX_EPOCH_TIME`</dt>
  <dd>Elenca le attivazioni create prima della data specificata. La durata è misurata in millisecondi a partire dal 1° gennaio 1970. Esempio: `1560371263` è il 12 giugno 2019 08:27:43 UTC.</dd>
  </dl>

<br />**Esempio**
```
ibmcloud fn activation list
```
{: pre}

**Output**
```
activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
```
{: screen}


<br />

### `ibmcloud fn activation logs`
{: #cli_activation_logs}

Ottieni log per un'attivazione specifica.

```
ibmcloud fn activation logs [ACTIVATION_ID] [--last] [--strip]
```
{: pre}

<br />**Opzioni del comando**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>ID di un'attivazione specifica. Utilizza `ibmcloud fn activation list` per richiamare un elenco di ID disponibili. Questo valore è obbligatorio, a meno che non venga specificata l'opzione `--last` o `-l`.</dd>

  <dt>`--last, -l`</dt>
  <dd>Visualizza i log per l'attivazione più recente. Questo indicatore è facoltativo.</dd>

  <dt>`--strip, -r`</dt>
  <dd>Visualizza solo il messaggio di log; escludi le informazioni di data/ora e flusso. Questo indicatore è facoltativo.</dd>
  </dl>

<br />**Esempio**
```
ibmcloud fn activation logs 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />

### `ibmcloud fn activation poll`
{: #cli_activation_poll}

Visualizza uno streaming, un elenco dinamico di attivazioni per un'azione o uno spazio dei nomi. Puoi premere `CTRL+C` per terminare il polling.

```
ibmcloud fn activation poll [NAMESPACE] [ACTION_NAME] [--exit SECONDS] [--since-days DAYS] [-since-hours HOURS] [--since-minutes MINUTES] [--since-seconds SECONDS]
```
{: pre}

<br />**Opzioni del comando**

  <dl>
  <dt>/`NAMESPACE`</dt>
  <dd>Uno spazio dei nomi, che comincia con /. Esegui il polling delle attivazioni per uno spazio dei nomi, un'azione o uno spazio. Questo valore è facoltativo. Se non viene specificato uno spazio dei nomi o un'azione, lo spazio viene sottoposto a polling.</dd>

  <dt>`ACTION_NAME`</dt>
  <dd>Esegui il polling delle attivazioni per uno spazio dei nomi, un'azione o uno spazio. Questo valore è facoltativo. Se non viene specificato uno spazio dei nomi o un'azione, lo spazio viene sottoposto a polling.</dd>

  <dt>`--exit` `SECONDS`, `-e` `SECONDS`</dt>
  <dd>Esegui il polling delle attivazioni per il numero specificato di secondi, poi chiudi. Questo indicatore è facoltativo.</dd>

  <dt>`--since-days` `DAYS`</dt>
  <dd>Avvia il polling per le attivazioni per un determinato numero di giorni precedenti. Questo indicatore è facoltativo.</dd>

  <dt>`--since-hours` `HOURS`</dt>
  <dd>Avvia il polling per le attivazioni per un determinato numero di ore precedenti. Questo indicatore è facoltativo.</dd>

  <dt>`--since-minutes` `MINUTES`</dt>
  <dd>Avvia il polling per le attivazioni per un determinato numero di minuti precedenti. Questo indicatore è facoltativo.</dd>

  <dt>`--since-seconds` `SECONDS`</dt>
  <dd>Avvia il polling per le attivazioni per un determinato numero di secondi precedenti. Questo indicatore è facoltativo.</dd>
  </dl>

<br />**Esempio**
```
ibmcloud fn activation poll
```
{: pre}


<br />

### `ibmcloud fn activation result`
{: #cli_activation_result}

Ottieni il risultato di un'attivazione specifica.

```
ibmcloud fn activation result [ACTIVATION_ID] [--last] [--strip]
```
{: pre}


<br />**Opzioni del comando**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>ID di un'attivazione specifica. Utilizza `ibmcloud fn activation list` per richiamare un elenco di ID disponibili. Questo valore è obbligatorio, a meno che non venga specificata l'opzione `--last` o `-l`.</dd>

  <dt>`--last, -l`</dt>
  <dd>Visualizza il risultato per l'attivazione più recente. Questo indicatore è facoltativo.</dd>

  </dl>

<br />**Esempio**
```
ibmcloud fn activation result 8694a4501be6486a94a4501be6886a1e
```
{: pre}



<br /><br />

## Comandi API
{: #cli_api}


### `ibmcloud fn api create`
{: #cli_api_create}

Crea un'API.

```
ibmcloud fn api create BASE_PATH API_PATH API_VERB ACTION_NAME] [--apiname API_NAME] [--config-file FILE] [--response-type TYPE]
```
{: pre}

<br />**Opzioni del comando**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>Il percorso di base dell'API.</dd>

   <dt>`API_NAME`</dt>
   <dd>Il nome dell'API. Il nome dell'API può coincidere con il percorso di base.</dd>

   <dt>`API_VERB`</dt>
   <dd>Il verbo per l'API, ad esempio `get` o `post`.</dd>

   <dt>`ACTION_NAME`</dt>
   <dd>Il nome dell'azione.</dd>

   <dt>`--apiname API_NAME`, `-n API_NAME`</dt>
   <dd>Il nome dell'API. Se viene specificato un file di configurazione, questo indicatore viene ignorato. Il nome predefinito è il `BASE_PATH`. Questo indicatore è facoltativo.</dd>

   <dt>`--config-file` `FILE`, `-c` `FILE`</dt>
   <dd>Un file JSON che contiene la configurazione API Swagger. Quando viene utilizzato questo indicatore, quello del nome API viene ignorato. Questo indicatore è obbligatorio.</dd>

   <dt>`--response-type TYPE`</dt>
   <dd>Imposta il tipo di risposta azione Web, quale `html`, `http`, `json`, `text` o `svg`. Il valore predefinito è `json`. Questo indicatore è facoltativo.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  **Output**
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world

  ```
  {: screen}


<br />

### `ibmcloud fn api delete`
{: #cli_api_delete}

Elimina un'API.

```
ibmcloud fn api delete BASE_PATH API_NAME API_PATH API_VERB
```
{: pre}

<br />**Opzioni del comando**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>Il percorso di base dell'API.</dd>

   <dt>`API_NAME`</dt>
   <dd>Il nome dell'API. Il nome dell'API può coincidere con il percorso di base.</dd>

   <dt>`API_PATH`</dt>
   <dd>Il percorso dell'API.</dd>

   <dt>`API_VERB`</dt>
   <dd>Il verbo per l'API, ad esempio `GET` o `POST`.</dd>

   <dt>`--format OUTPUT_TYPE`</dt>
   <dd>Specifica il tipo di output API quale `json` o `yaml`. Il valore predefinito è `json`.</dd>

   <dt>`--full, -f`</dt>
   <dd>Visualizza i dettagli di configurazione dell'API completa.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn api delete /hello /world get
  ```
  {: pre}



<br />

### `ibmcloud fn api get`
{: #cli_api_get}

Ottieni i metadati per un'API.

```
ibmcloud fn api get BASE_PATH API_NAME [--format OUTPUT_TYPE] [--full]
```
{: pre}

<br />**Opzioni del comando**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>Il percorso di base dell'API.</dd>

   <dt>`API_NAME`</dt>
   <dd>Il nome dell'API. Il nome dell'API può coincidere con il percorso di base.</dd>

   <dt>`--format OUTPUT_TYPE`</dt>
   <dd>Specifica il tipo di output API quale `json` o `yaml`. Il valore predefinito è `json`.</dd>

   <dt>`--full, -f`</dt>
   <dd>Visualizza i dettagli di configurazione dell'API completa.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn api get /hello /world
  ```
  {: pre}


<br />

### `ibmcloud fn api list`
{: #cli_api_list}

Elenca tutte le API che hai creato o un numero specifico di API. Se non viene specificato alcun nome o percorso di base, vengono elencate tutte le API.

```
ibmcloud fn api list BASE_PATH API_NAME API_PATH API_VERB [--full] [--limit NUMBER_OF_APIS] [--name-sort] [--skip NUMBER_OF_APIS]
```
{: pre}

<br />**Opzioni del comando**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>Il percorso di base dell'API.</dd>

   <dt>`API_NAME`</dt>
   <dd>Il nome dell'API. Il nome dell'API può coincidere con il percorso di base.</dd>

   <dt>`API_PATH`</dt>
   <dd>Il percorso dell'API.</dd>

   <dt>`API_VERB`</dt>
   <dd>Il verbo per l'API, ad esempio `GET` o `POST`.</dd>

   <dt>`--full, -f`</dt>
   <dd>Visualizza i dettagli dell'API completa. Questo indicatore è facoltativo. </dd>

   <dt>`--limit NUMBER_OF_APIS`, `-l NUMBER_OF_APIS`</dt>
   <dd>Elenca il numero specificato di API. Il valore predefinito è 30 API. Questo indicatore è facoltativo. </dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Ordina l'elenco di API restituite in base al nome; diversamente, l'elenco viene ordinato per data di creazione. Questo indicatore è facoltativo. </dd>

   <dt>`--skip NUMBER_OF_APIS`, `-s NUMBER_OF_APIS`</dt>
   <dd>Escludi dal risultato il numero specificato di ultime API create. Questo indicatore è facoltativo. </dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn api list
  ```
  {: pre}


<br /><br />
## Comandi distribuzione
{: #cli_deploy_cmds}


### `ibmcloud fn deploy`
{: #cli_deploy}

Utilizza un file manifest per distribuire una raccolta di pacchetti, azioni, trigger e regole.

```
ibmcloud fn deploy [--apihost HOST] [--auth KEY] [--config FILE] [--deployment FILE] [--manifest FILE] [--namespace NAMESPACE] [--param KEY VALUE] [--param-file FILE] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br />**Opzioni del comando**

   <dl>

   <dt>`--apihost HOST`</dt>
   <dd>L'host API `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>La chiave (`KEY`) autorizzazione `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--config` `FILE`</dt>
   <dd>Il file di configurazione. Il valore predefinito è `$HOME/.wskprops`.</dd>

   <dt>`--deployment` `FILE`</dt>
   <dd>Il percorso al file di distribuzione.</dd>

   <dt>`--manifest` `FILE`, `-m` `FILE`</dt>
   <dd>Il percorso al file manifest. Questo indicatore è necessario se il file manifest.yaml non è presente nella directory corrente.</dd>

   <dt>`--namespace` `NAMESPACE`, `-n` `NAMESPACE`</dt>
   <dd>Il nome o l'ID di uno spazio dei nomi.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Valori (`VALUES`) del parametro in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Un file JSON che contiene chiavi (`KEYS`) e valori (`VALUES`) parametro. Questo indicatore è facoltativo.</dd>

   <dt>`--preview` </dt>
   <dd>Visualizza il piano di distribuzione prima della distribuzione.</dd>

   <dt>`--project PATH`</dt>
   <dd>Il percorso al progetto senza server. Il valore predefinito è <code>.</code> (directory corrente).</dd>

   <dt>`--strict`</dt>
   <dd>Consenti una versione di runtime definita dall'utente.</dd>

   <dt>`--verbose, -v`</dt>
   <dd>Visualizza l'output dettagliato.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn deploy --manifest folder/manifest.yaml
  ```
  {: pre}


<br />

### `ibmcloud fn undeploy`
{: #cli_undeploy}

Utilizza un file manifest per annullare la distribuzione di una raccolta di pacchetti, azioni, trigger e regole.

```
ibmcloud fn undeploy [--apihost HOST] [--auth KEY] [--config FILE] [--deployment FILE] [--manifest FILE] [--namespace NAMESPACE] [--param KEY VALUE] [--param-file FILE] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`--apihost HOST`</dt>
   <dd>L'host API `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>La chiave (`KEY`) autorizzazione `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--config` `FILE`</dt>
   <dd>Il file di configurazione. Il valore predefinito è `$HOME/.wskprops`.</dd>

   <dt>`--deployment` `FILE`</dt>
   <dd>Il percorso al file di distribuzione.</dd>

   <dt>`--manifest` `FILE`, -m `FILE`</dt>
   <dd>Il percorso al file manifest. Questo indicatore è necessario se il file manifest.yaml non è presente nella directory corrente.</dd>

   <dt>`--namespace` `NAMESPACE`, `-n` `NAMESPACE`</dt>
   <dd>Il nome o l'ID di uno spazio dei nomi.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Valori (`VALUES`) del parametro in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Un file JSON che contiene chiavi (`KEYS`) e valori (`VALUES`) parametro. Questo indicatore è facoltativo.</dd>

   <dt>`--preview` </dt>
   <dd>Visualizza il risultato del comando senza eseguire il comando.</dd>

   <dt>`--project PATH`</dt>
   <dd>Il percorso al progetto senza server. Il valore predefinito è `.` (directory corrente).</dd>

   <dt>`--strict`</dt>
   <dd>Consenti una versione di runtime definita dall'utente.</dd>

   <dt>`--verbose, -v`</dt>
   <dd>Visualizza l'output dettagliato.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn undeploy --manifest folder/manifest.yaml
  ```
  {: pre}



<br /><br />

## Comando list
{: #cli_list_cmd}


### `ibmcloud fn list`
{: #cli_list}

Visualizza un elenco raggruppato di pacchetti, azioni. trigger e regole nello spazio dei nomi.

```
ibmcloud fn list [--name-sort]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`--name-sort, -n`</dt>
   <dd>Ordina ciascun gruppo di entità restituite in base al nome; diversamente ogni gruppo verrà ordinato in base alla data di creazione.</dd>
   </dl>

<br />**Esempio**

  ```
  ibmcloud fn list
  ```
  {: pre}




<br /><br />
## Comandi spazio dei nomi
{: #cli_namespace}


### `ibmcloud fn namespace create`
{: #cli_namespace_create}

Crea uno spazio dei nomi IAM.

```
ibmcloud fn namespace create NAMESPACE [--description DESCRIPTION] 
```
{: pre}

<br />**Opzioni del comando**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>Il nome di uno spazio dei nomi. Non includere trattini (-) nel nome. Questo valore è obbligatorio.</dd>

   <dt>`--description DESCRIPTION`, `-n DESCRIPTION`</dt>
   <dd>Scrivi la tua descrizione univoca per identificare più facilmente lo spazio dei nomi. Se la tua descrizione contiene più parole, racchiudere la descrizione tra virgolette ("). Questo indicatore è facoltativo.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn namespace create HBCTeamProd --description "HBC Team Prod Environment. See Beth for information about this namespace."
  ```
  {: pre}


<br />

### `ibmcloud fn namespace delete`
{: #cli_namespace_delete}

Elimina uno spazio dei nomi IAM.

```
ibmcloud fn namespace delete NAMESPACE
```
{: pre}


<br />**Esempio**

  ```
  ibmcloud fn namespace delete mynamespace
  ```
  {: pre}



<br />

### `ibmcloud fn namespace get`
{: #cli_namespace_get}

Ottieni le entità per/le informazioni metadati da uno spazio dei nomi Cloud Foundry o IAM.

```
ibmcloud fn namespace list NAMESPACE [--auth KEY] [--name-sort] [--properties] 
```
{: pre}

<br />**Opzioni del comando**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>Il nome o l'ID di uno spazio dei nomi. Questo valore è obbligatorio.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>La chiave (`KEY`) autorizzazione `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Ordinare l'elenco degli spazi dei nomi restituiti in base al nome; diversamente, l'elenco viene ordinato per data di creazione. Questo indicatore è facoltativo. </dd>

   <dt>`--properties`</dt>
   <dd>Visualizza le proprietà dello spazio dei nomi invece delle entità contenute al suo interno. Questo indicatore è facoltativo. </dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn namespace get user@domain.com_dev --properties
  ```
  {: pre}

  **Output**
  ```
  Name: user@domain.com_dev
  Description: This is a description of my namespace.
  Resource Plan Id: functions-base-plan
  Location: us-south
  ID: 58c2e718-8c60-48bc-96de-cf9373fe6709
  ```
  {: screen}



<br />

### `ibmcloud fn namespace list`
{: #cli_namespace_list}

Elenca gli spazi dei nomi Cloud Foundry e IAM disponibili.

```
ibmcloud fn namespace list [--auth KEY] [--cf] [--iam] [--limit NUMBER_OF_NAMESPACES] [--name-sort] [--skip NUMBER_OF_NAMESPACES] 
```
{: pre}

<br />**Opzioni del comando**

   <dl>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>La chiave (`KEY`) autorizzazione `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--cf`</dt>
   <dd>Visualizza solo gli spazi dei nomi Cloud Foundry. Gli spazi dei nomi IAM non vengono visualizzati. Questo indicatore è facoltativo.</dd>

   <dt>`--iam`</dt>
   <dd>Visualizza solo gli spazi dei nomi IAM. Gli spazi dei nomi Cloud Foundry non vengono visualizzati. Questo indicatore è facoltativo.</dd>

   <dt>`--limit NUMBER_OF_``NAMESPACE``S`, `-l NUMBER_OF_``NAMESPACE``S`</dt>
   <dd>Elenca un numero specificato di spazi dei nomi. Il valore predefinito è di 30 spazi dei nomi. Questo indicatore è facoltativo. </dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Ordinare l'elenco degli spazi dei nomi restituiti in base al nome; diversamente, l'elenco viene ordinato per data di creazione. Questo indicatore è facoltativo. </dd>

   <dt>`--skip NUMBER_OF_NAMESPACES`, `-s NUMBER_OF_``NAMESPACE``S`</dt>
   <dd>Escludi dal risultato il numero specificato di ultimi spazi dei nomi creati. Questo indicatore è facoltativo. </dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn namespace list
  ```
  {: pre}


<br />

### `ibmcloud fn namespace update`
{: #cli_namespace_update}

Modifica il nome o la descrizione di uno spazio dei nomi IAM.

```
ibmcloud fn namespace update NAMESPACE [NEW_NAMESPACE_NAME] [--description DESCRIPTION] 
```
{: pre}

<br />**Opzioni del comando**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>Il nome di uno spazio dei nomi. Non includere trattini (-) nel nome. Questo valore è obbligatorio.</dd>

   <dt>`NEW_``NAMESPACE``_NAME`</dt>
   <dd>Il nuovo nome per uno spazio dei nomi. Non includere trattini (-) nel nome. Questo valore è facoltativo.</dd>

   <dt>`--description DESCRIPTION`, `-n DESCRIPTION`</dt>
   <dd>Scrivi la tua descrizione univoca per identificare più facilmente lo spazio dei nomi. Se la tua descrizione contiene più parole, racchiudere la descrizione tra virgolette ("). Questo indicatore è facoltativo.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn namespace update HBCTeamProd HBCTeamStaging
  ```
  {: pre}




<br /><br />
## Comandi pacchetto
{: #cli_pkg}


### `ibmcloud fn package bind`
{: #cli_pkg_bind}

Esegui il bind dei parametri a un pacchetto. Se non diversamente specificato, tutte le azioni del pacchetto ereditano questi parametri.

```
ibmcloud fn package bind PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opzioni del comando**

  <dl>
  <dt>`PACKAGE_NAME`</dt>
  <dd>Il nome del pacchetto. Questo valore è obbligatorio. </dd>

  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>Le annotazioni vengono indicate in formato `KEY` `VALUE`. Per includere più annotazioni, specifica queste opzioni per ciascuna annotazione. Questo indicatore è facoltativo.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>Un file JSON che contiene l'annotazione in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>Valori (`VALUES`) del parametro in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>Un file JSON che contiene chiavi (`KEYS`) e valori (`VALUES`) parametro. Questo indicatore è facoltativo.</dd>
  </dl>

<br />**Esempio**

  ```
  ibmcloud fn package bind --param name Bob
  ```
  {: pre}



<br />

### `ibmcloud fn package create`
{: #cli_pkg_create}

Crea un pacchetto progettato per contenere una o più azioni. Per aggiungere un'azione nel pacchetto, includi il nome del pacchetto con il nome dell'azione quando crei o aggiorni l'azione.

```
ibmcloud fn package create PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opzioni del comando**

  <dl>
  <dt>`PACKAGE_NAME`</dt>
  <dd>Il nome del pacchetto. Questo valore è obbligatorio. </dd>

  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>Le annotazioni vengono indicate in formato `KEY` `VALUE`. Per includere più annotazioni, specifica queste opzioni per ciascuna annotazione. Questo indicatore è facoltativo.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>Un file JSON che contiene l'annotazione in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>Valori (`VALUES`) del parametro in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>Un file JSON che contiene parametri in formato `KEYS` `VALUE`. Questo indicatore è facoltativo.</dd>

  <dt>`--shared yes|no`</dt>
  <dd>Se viene specificato senza un valore o con il valore yes, il pacchetto è condiviso con altri utenti.</dd>
  </dl>

<br />**Esempio**

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  **Output**
  ```
  ok: created hellopkg
  ```
  {: screen}


<br />

### `ibmcloud fn package delete`
{: #cli_pkg_delete}

Puoi effettuare una pulizia del tuo spazio dei nomi eliminando i pacchetti che non desideri più utilizzare.

```
ibmcloud fn package delete PACKAGE_NAME
```
{: pre}

<br />**Esempio**

  ```
  ibmcloud fn package delete hello
  ```
  {: pre}

  **Output**
  ```
  ok: deleted hello
  ```
  {: screen}


<br />

### `ibmcloud fn package get`
{: #cli_pkg_get}

Ottieni i metadati che descrivono un pacchetto specifico.

```
ibmcloud fn package get PACKAGE_NAME [--summary]
```
{: pre}

<br />**Opzioni del comando**

  <dl>
   <dt>`PACKAGE_NAME`</dt>
   <dd>Il nome di un pacchetto. Questo valore è obbligatorio.</dd>

   <dt>`--summary`</dt>
   <dd>Ottieni un riepilogo dei dettagli del pacchetto. I parametri con prefisso "*" sono associati. Questo indicatore è facoltativo.</dd>
   </dl>

<br />**Esempio**

```
ibmcloud fn package get hello
```
{: pre}


<br />

### `ibmcloud fn package list`
{: #cli_pkg_list}

Elenca tutti i pacchetti che hai creato o un numero specifico di pacchetti.

```
ibmcloud fn package list [NAMESPACE] [--limit NUMBER_OF_PACKAGES] [--name-sort] [--skip NUMBER_OF_PACKAGES]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`NAMESPACE`</dt>
   <dd>Elenca i pacchetti di uno spazio dei nomi specifico. Questo valore è facoltativo. Se non viene specificato, vengono elencati tutti i pacchetti.</dd>

   <dt>`--limit NUMBER_OF_PACKAGES`, `-l NUMBER_OF_PACKAGES`</dt>
   <dd>Elenca il numero specificato di pacchetti. Il valore predefinito è 30 pacchetti.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Ordina l'elenco di pacchetti restituiti in base al nome; diversamente, l'elenco viene ordinato per data di creazione.</dd>

   <dt>`--skip NUMBER_OF_PACKAGES`, `-s NUMBER_OF_PACKAGES`</dt>
   <dd>Escludi dal risultato il numero specificato di ultimi pacchetti creati.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn package list
  ```
  {: pre}

  Esegui `ibmcloud fn package list /whisk.system` per visualizzare l'elenco dei pacchetti preinstallati.
  {: tip}


<br />

### `ibmcloud fn package refresh`
{: #cli_pkg_refresh}

Aggiorna i bind di pacchetto per tutti i pacchetti di uno spazio dei nomi specifico.

```
ibmcloud fn package refresh /NAMESPACE
```
{: pre}

<br />**Opzioni del comando**

   <dl>

   <dt>/`NAMESPACE`</dt>
   <dd>Uno spazio dei nomi, che comincia con /. Questo indicatore è obbligatorio. Esegui `ibmcloud fn namespace list` per ottenere un elenco di spazi dei nomi da cui scegliere.</dd>
   </dl>

<br />**Esempio**

  ```
  ibmcloud fn package refresh /user@domain.com_dev
  ```
  {: pre}


<br />

### `ibmcloud fn package update`
{: #cli_pkg_update}

Aggiorna un pacchetto progettato per contenere una o più azioni. Per aggiungere un'azione nel pacchetto, includi il nome del pacchetto con il nome dell'azione quando crei o aggiorni l'azione.

Quando aggiorni i parametri per un pacchetto, un'azione o un trigger, devi specificare tutti i parametri creati in precedenza. Altrimenti, i parametri precedentemente creati vengono rimossi. Per i pacchetti, vengono rimossi anche tutti i servizi associati al pacchetto, quindi dopo aver aggiornato gli altri parametri devi eseguire di nuovo il [bind dei servizi](/docs/openwhisk?topic=cloud-functions-services) al tuo pacchetto.
{: important}

```
ibmcloud fn package update PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opzioni del comando**

   <dl>

   <dt>`PACKAGE_NAME`</dt>
   <dd>Il nome del pacchetto. Questo valore è obbligatorio. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>Le annotazioni vengono indicate in formato `KEY` `VALUE`. Per includere più annotazioni, specifica queste opzioni per ciascuna annotazione. Questo indicatore è facoltativo.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Un file JSON che contiene l'annotazione in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Valori (`VALUES`) del parametro in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Un file JSON che contiene chiavi (`KEYS`) e valori (`VALUES`) parametro. Questo indicatore è facoltativo.</dd>

   <dt>`--shared yes|no`</dt>
   <dd>Se viene specificato senza un valore o con il valore `yes`, il pacchetto è condiviso con altri utenti.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  **Output**
  ```
  ok: created hellopkg
  ```
  {: screen}




<br /><br />
## Comandi proprietà
{: #cli_prop}

Imposta le proprietà globali per l'ambiente della tua CLI o visualizza le proprietà della CLI `wsk` eseguita nell'ambito della CLI `ibmcloud fn`.

### `ibmcloud fn property get`
{: #cli_prop_get}

Visualizza i dettagli dei metadati per una proprietà dalla CLI `wsk`.

```
ibmcloud fn property get [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`--all`</dt>
   <dd>Visualizza tutte le proprietà per la CLI `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`---apibuild`</dt>
   <dd>Informazioni sulla build API `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--apibuildno`</dt>
   <dd>Numero della build API `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--apihost` `HOST`</dt>
   <dd>L'host API `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>La versione API `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>La chiave (`KEY`) autorizzazione `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>Il certificato client `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--cliversion`</dt>
   <dd>La versione della CLI `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>La chiave (`KEY`) client `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>Uno spazio dei nomi IAM. Questo indicatore non può essere impostato per gli spazi dei nomi Cloud Foundry. Questo indicatore è facoltativo.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn property get --auth
  ```
  {: pre}


<br />

### `ibmcloud fn property set`
{: #cli_prop_set}

Imposta una proprietà. È richiesto almeno un indicatore. Una volta impostata, la proprietà viene conservata sulla tua workstation in `<home_dir>/.bluemix/plugins/cloud-functions/config.json`. Per rimuovere una proprietà, esegui [`ibmcloud fn property unset --<property>`](#cli_prop_set). 

```
ibmcloud fn property set [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`--apihost` `HOST`</dt>
   <dd>L'host API `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>La versione API `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--auth` `KEY`, -u</dt>
   <dd>La chiave (`KEY`) autorizzazione `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>Il certificato client `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>La chiave (`KEY`) client `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>Uno spazio dei nomi IAM. Questo indicatore non può essere impostato per gli spazi dei nomi Cloud Foundry. Questo indicatore è facoltativo.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn property set --namespace myNamespace
  ```
  {: pre}

  **Output**
  ```
  ok: whisk namespace set to myNamespace
  ```
  {: screen}

<br />

### `ibmcloud fn property unset`
{: #cli_prop_unset}

Annulla l'impostazione di una proprietà per la CLI `wsk`. È richiesto almeno un indicatore.

```
ibmcloud fn property unset [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`--apihost` `HOST`</dt>
   <dd>L'host API `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>La versione API `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--auth` `KEY`, `-u`</dt>
   <dd>La chiave (`KEY`) autorizzazione `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>Il certificato client `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>La chiave (`KEY`) client `wsk`. Questo indicatore è facoltativo.</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>Uno spazio dei nomi IAM. Questo indicatore non può essere impostato per gli spazi dei nomi Cloud Foundry. Questo indicatore è facoltativo.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn property unset --namespace
  ```
  {: pre}



<br /><br />
## Comandi regola
{: #cli_rule}


### `ibmcloud fn rule create`
{: #cli_rule_create}

Crea una regola per associare un trigger a un'azione. Per poter creare una regola, devi prima creare un trigger e un'azione.

```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}

<br />**Esempio**

  ```
  ibmcloud fn rule create myrule mytrigger myaction
  ```
  {: pre}

  **Output**
  ```
  ok: created myrule
  ```
  {: screen}


<br />

### `ibmcloud fn rule delete`
{: #cli_rule_delete}

Per ripulire il tuo spazio dei nomi, rimuovi le regole che non ti servono più.

```
ibmcloud fn rule delete RULE_NAME [--disable]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>Il nome di una regola. Questo valore è obbligatorio.</dd>

  <dt>`--disable`</dt>
  <dd>Disabilita la regola prima di eliminarla.</dd>
  </dl>


<br />**Esempio**

```
ibmcloud fn rule delete myrule
```
{: pre}


<br />

### `ibmcloud fn rule disable`
{: #cli_rule_disable}

Modifica lo stato di una regola su inattivo e impediscile di eseguire un'azione quando viene attivato un trigger.

```
ibmcloud fn rule disable RULE_NAME
```
{: pre}

<br />**Esempio**

  ```
  ibmcloud fn rule disable myrule
  ```
  {: pre}

<br />

### `ibmcloud fn rule enable`
{: #cli_rule_enable}

Modifica lo stato di una regola da inattivo ad attivo. Quando lo stato è attivo, l'azione viene eseguita quando viene attivato un trigger.

```
ibmcloud fn rule enable RULE_NAME
```
{: pre}

<br />**Esempio**

  ```
  ibmcloud fn rule enable myrule
  ```
  {: pre}

<br />

### `ibmcloud fn rule get`
{: #cli_rule_get}

Ottieni i metadati che descrivono una regola specifica.

```
ibmcloud fn rule get RULE_NAME [--summary]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>Il nome di una regola. Questo valore è obbligatorio.</dd>

  <dt>`--summary`</dt>
  <dd>Ottieni un riepilogo dei dettagli della regola.</dd>
  </dl>

<br />**Esempio**

```
ibmcloud fn rule get myrule
```
{: pre}


<br />

### `ibmcloud fn rule list`
{: #cli_rule_list}

Elenca tutte le regole che hai creato o un numero specifico di regole.

```
ibmcloud fn rule list RULE_NAME [--limit NUMBER_OF_RULES] [--name-sort] [--skip NUMBER_OF_RULES]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>Il nome di una regola. Questo valore è facoltativo. Se non specificato, vengono elencate tutte le regole.</dd>

   <dt>`--limit NUMBER_OF_RULES`, `-l NUMBER_OF_RULES`</dt>
   <dd>Elenca il numero specificato di regole. Il valore predefinito è di 30 regole.</dd>

   <dt>`--name-sort`, `-n`</dt>
   <dd>Ordina l'elenco delle regole restituite in base al nome; diversamente l'elenco viene ordinato in base alla data di creazione.</dd>

   <dt>`--skip NUMBER_OF_RULES`, `-s NUMBER_OF_RULES`</dt>
   <dd>Escludi dal risultato il numero specificato di ultime regole create.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn rule list
  ```
  {: pre}


<br />

### `ibmcloud fn rule status`
{: #cli_rule_status}

Vedi se una regola è attiva o inattiva. Esegui i comandi `ibmcloud fn rule disable` o `ibmcloud fn run enable` per modificare lo stato.

```
ibmcloud fn rule status RULE_NAME
```
{: pre}

<br />**Esempio**

  ```
  ibmcloud fn rule status myrule
  ```
  {: pre}


<br />

### `ibmcloud fn rule update`
{: #cli_rule_update}

Per modificare le associazioni tra trigger e regole, puoi aggiornare una regola.

```
ibmcloud fn rule update RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}

<br />**Esempio**

  ```
  ibmcloud fn rule update myrule mytrigger myaction
  ```
  {: pre}



<br /><br />

## Comando SDK
{: #cli_sdk}


### `ibmcloud fn sdk install`
{: #cli_sdk_install}

Installa un SDK.

```
ibmcloud fn sdk install COMPONENT [--limit NUMBER_OF_TRIGGERS]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`COMPONENT`</dt>
   <dd>Il componente SDK, come ad esempio `docker`, `iOS` e `bashauto`. Questo valore è obbligatorio.</dd>

   <dt>`--stdout, --s`</dt>
   <dd>Stampa i risultati del comando bash in `STDOUT`. Questo indicatore è facoltativo.</dd>


   </dl>

<br />**Esempio**

  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}



<br /><br />

## Comandi servizi
{: #cli_service}


### `ibmcloud fn service bind`
{: #cli_service_bind}

Esegui il bind delle credenziali del servizio con un'azione o un pacchetto.

```
ibmcloud fn service bind SERVICE PACKAGE_or_ACTION_NAME [--instance SERVICE_INSTANCE] [--keyname SERVICE_KEY]
```
{: pre}

<br />**Opzioni del comando**

   <dl>

   <dt>`SERVICE`</dt>
   <dd>Il nome del servizio.</dd>

   <dt>`PACKAGE_or_``ACTION_NAME`</dt>
   <dd>Il nome del pacchetto o dell'azione per il bind delle credenziali.</dd>

   <dt>`--instance SERVICE_INSTANCE`</dt>
   <dd>Il nome dell'istanza del servizio.</dd>

   <dt>`--keyname SERVICE_``KEY`</dt>
   <dd>Il nome delle credenziali `KEY` del servizio di cui eseguire il bind.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn service bind cloudant hello --instance CLOUDANT_SERVICE
  ```
  {: pre}


<br />

### `ibmcloud fn service unbind`
{: #cli_service_unbind}

Annulla bind delle credenziali del servizio per un'azione o un pacchetto.

```
ibmcloud fn service unbind SERVICE PACKAGE_or_ACTION_NAME
```
{: pre}

<br />**Opzioni del comando**

   <dl>

   <dt>`SERVICE`</dt>
   <dd>Il nome del servizio.</dd>

   <dt>`PACKAGE_or_``ACTION_NAME`</dt>
   <dd>Il nome del pacchetto o dell'azione da cui annullare il bind delle credenziali.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn service unbind cloudant hello
  ```
  {: pre}



<br /><br />

## Comandi trigger
{: #cli_trigger}


### `ibmcloud fn trigger create`
{: #cli_trigger_create}

Crea un trigger.

```
ibmcloud fn trigger create TRIGGER_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--feed ACTION_NAME] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>Il nome del trigger. Questo valore è obbligatorio. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>Le annotazioni vengono indicate in formato `KEY` `VALUE`. Per includere più annotazioni, specifica queste opzioni per ciascuna annotazione. Questo indicatore è facoltativo.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Un file JSON che contiene l'annotazione in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

   <dt>`--feed` `ACTION_NAME`, `-f` `ACTION_NAME`</dt>
   <dd>Imposta il tipo di trigger come feed. Questo indicatore è facoltativo.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Valori (`VALUES`) del parametro in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Un file JSON che contiene chiavi (`KEYS`) e valori (`VALUES`) parametro. Questo indicatore è facoltativo.</dd>


   </dl>

<br />**Esempio**
```
ibmcloud fn trigger create mytrigger --param name Bob
```
{: pre}


<br />

### `ibmcloud fn trigger delete`
{: #cli_trigger_delete}

Elimina un trigger.

```
ibmcloud fn trigger delete TRIGGER_NAME
```
{: pre}

<br />**Esempio**

```
ibmcloud fn trigger delete mytrigger
```
{: pre}


<br />

### `ibmcloud fn trigger fire`
{: #cli_trigger_fire}

Testa un trigger attivandolo, invece di aspettare che venga attivato automaticamente.

```
ibmcloud fn trigger fire TRIGGER_NAME [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opzioni del comando**

   <dl>

   <dt>`TRIGGER_NAME`</dt>
   <dd>Il nome del trigger. Questo valore è obbligatorio. </dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Valori (`VALUES`) del parametro in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Un file JSON che contiene chiavi (`KEYS`) e valori (`VALUES`) parametro. Questo indicatore è facoltativo.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn trigger fire --param name Bob
  ```
  {: pre}


<br />

### `ibmcloud fn trigger get`
{: #cli_trigger_get}

Ottieni i metadati che descrivono un trigger specifico.

```
ibmcloud fn trigger get TRIGGER_NAME [--summary]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>Il nome di un trigger. Questo valore è obbligatorio.</dd>

  <dt>`--summary`</dt>
  <dd>Ottieni un riepilogo dei dettagli del trigger.</dd>
  </dl>

<br />**Esempio**

```
ibmcloud fn trigger get mytrigger
```
{: pre}

<br />

### `ibmcloud fn trigger list`
{: #cli_trigger_list}

Elenca tutti i trigger che hai creato o un numero specifico di trigger.

```
ibmcloud fn trigger list TRIGGER_NAME [--limit NUMBER_OF_TRIGGERS] [--name-sort] [--skip NUMBER_OF_TRIGGERS]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>Il nome di un trigger. Questo valore è facoltativo. Se non viene specificato, vengono elencati tutti i trigger.</dd>

   <dt>`--limit` `NUMBER_OF_TRIGGERS`, `-l` `NUMBER_OF_TRIGGERS`</dt>
   <dd>Elenca il numero specificato di trigger. Il valore predefinito è di 30 trigger.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Ordina l'elenco di trigger restituiti in base al nome; diversamente, l'elenco viene ordinato per data di creazione.</dd>

   <dt>`--skip` `NUMBER_OF_TRIGGERS`, `-s` `NUMBER_OF_TRIGGERS`</dt>
   <dd>Escludi dal risultato il numero specificato di ultimi trigger creati.</dd>

   </dl>

<br />**Esempio**

  ```
  ibmcloud fn trigger list
  ```
  {: pre}


<br />

### `ibmcloud fn trigger update`
{: #cli_trigger_update}

Aggiorna un trigger.

Quando aggiorni i parametri per un pacchetto, un'azione o un trigger, devi specificare tutti i parametri creati in precedenza. Altrimenti, i parametri precedentemente creati vengono rimossi. Per i pacchetti, vengono rimossi anche tutti i servizi associati al pacchetto, quindi dopo aver aggiornato gli altri parametri devi eseguire di nuovo il [bind dei servizi](/docs/openwhisk?topic=cloud-functions-services) al tuo pacchetto.
{: important}

```
ibmcloud fn trigger update TRIGGER_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opzioni del comando**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>Il nome del trigger. Questo valore è obbligatorio. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>Le annotazioni vengono indicate in formato `KEY` `VALUE`. Per includere più annotazioni, specifica queste opzioni per ciascuna annotazione. Questo indicatore è facoltativo.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Un file JSON che contiene l'annotazione in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Valori parametro in formato `KEY` `VALUE`. Questo indicatore è facoltativo.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Un file JSON che contiene chiavi (`KEYS`) e valori (`VALUES`) parametro. Questo indicatore è facoltativo.</dd>
   </dl>

<br />**Esempio**
```
ibmcloud fn trigger update mytrigger --param name Jim
```
{: pre}






