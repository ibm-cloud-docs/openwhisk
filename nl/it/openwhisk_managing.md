---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-25"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Gestione delle azioni
{: #openwhisk_managing}

Gestisci le azioni monitorando l'output dell'azione, ottenendo informazioni specifiche su un'azione o eliminando le azioni.
{: shortdec}

## Richiamo di azioni
{: #getting-actions}

Dopo aver creato un'azione, puoi ottenere ulteriori informazioni sui dettagli dell'azione ed elencare le azioni nel tuo spazio dei nomi.
{: shortdesc}

Per elencare tutte le azioni che hai creato:
```
ibmcloud fn action list
```
{: pre}

Man mano che crei ulteriori azioni, può essere utile raggruppare le azioni correlate in [pacchetti](./openwhisk_packages.html). Per filtrare il tuo elenco di azioni alle sole azioni all'interno di uno specifico pacchetto:
```
ibmcloud fn action list [NOME PACCHETTO]
```
{: pre}

Per ottenere i metadati che descrivono specifiche azioni:

```
ibmcloud fn action get hello
```
{: pre}

Output di esempio:
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
            "key": "exec",
            "value": "nodejs:6"
        }
    ],
    "limits": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}

<table>
<caption>Descrizione dell'output del comando <code>action get</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dell'output del comando <code>action get</code></th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>Lo spazio dei nomi in cui si trova questa azione.</td>
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
<td><code>exec</code></td>
<td><ul><li><code>kind</code>: il tipo di azione. I valori possibili sono nodejs:6, nodejs:8, php:7.1, python:3, python-jessie:3, swift:3.1.1, swift:4.1, java, blackbox e sequence.</li>
<li><code>code</code>: codice Javascript o Swift da eseguire quando kind è nodejs o swift.</li>
<li><code>components</code>: le azioni nella sequenza quando kind è sequence. Le azioni sono elencate in ordine.</li>
<li><code>image</code>: il nome dell'immagine contenitore quando kind è blackbox.</li>
<li><code>init</code>: riferimento al file zip facoltativo quando kind è nodejs.</li>
<li><code>binary</code>: indica se l'azione è compilata in un eseguibile binario.</li></ul></td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>Le annotazioni su questa azione. Per un elenco delle possibili annotazioni, vedi gli argomenti di riferimento [annotazioni di azioni](openwhisk_annotations.html#action) e [annotazioni di azioni web](openwhisk_annotations.html#annotations-specific-to-web-actions).</td>
</tr>
<tr>
<td><code>limits</code></td>
<td><ul><li><code>timeout</code>: il timeout, in millisecondi, per l'azione dopo il quale l'azione viene terminata. Valore predefinito: 6000</li>
<li><code>memory</code>: il limite massimo di memoria, in MB, impostato per l'azione. Valore predefinito: 256</li>
<li><code>logs</code>: il limite di dimensione massima del log, in MB, impostato per l'azione. Valore predefinito: 10</li></ul></td>
</tr>
<tr>
<td><code>publish</code></td>
<td>Indica se l'azione è pubblicata pubblicamente.</td>
</tr>
</tbody></table>

## Visualizzazione dei dettagli dell'attivazione
{: #activation}

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
    "namespace": "BobsOrg_dev",
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
            "value": "BobsOrg_dev/hello"
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
<td>Le annotazioni su questa azione. Per un elenco delle possibili annotazioni di attivazione, vedi l'[argomento di riferimento delle annotazioni](openwhisk_annotations.html#activation).</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>Indica se l'azione è pubblicata pubblicamente.</td>
</tr>
</tbody></table>

## Accesso ai metadati dell'azione nel corpo dell'azione
{: #accessing-action-metadata-within-the-action-body}

L'ambiente dell'azione contiene diverse proprietà specifiche per l'azione in esecuzione. Queste proprietà consentono all'azione di funzionare a livello di programmazione con gli asset OpenWhisk tramite l'API REST o di impostare un allarme interno quando l'azione sta per esaurire il suo budget di tempo assegnato. Le proprietà sono accessibili nell'ambiente di sistema per tutti i runtime supportati: Node.js, Python, Swift, Java e Docker quando si utilizza la struttura di base OpenWhisk Docker.

| Proprietà | Descrizione |
| -------- | ----------- |
| `__OW_API_HOST` | L'host API per la distribuzione OpenWhisk che sta eseguendo questa azione. |
| `__OW_API_KEY` | la chiave API per il soggetto che richiama l'azione; questa chiave potrebbe essere una chiave API limitata. |
| `__OW_NAMESPACE` | Lo spazio dei nomi per l'attivazione. Questo spazio dei nomi potrebbe essere diverso dallo spazio dei nomi per l'azione. |
| `__OW_ACTION_NAME` | Il nome completo dell'azione in esecuzione. |
| `__OW_ACTIVATION_ID` | L'ID di attivazione per questa istanza dell'azione in esecuzione. |
| `__OW_DEADLINE` | Il tempo approssimativo, espresso in millisecondi epoch, quando questa azione consumerà la sua intera quota di durata. |

## Richiamo di un URL azione
{: #get-action-url}

Un'azione può essere richiamata utilizzando l'interfaccia REST attraverso una richiesta HTTPS.
{: shortdesc}

Per ottenere un URL di azione:
```
ibmcloud fn action get actionName --url
```
{: pre}

Output di esempio per le azioni standard:
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

Output di esempio per le [azioni web](./openwhisk_webactions.html):
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**Nota:** per le azioni standard, è necessario fornire l'autenticazione quando vengono richiamate tramite una richiesta HTTPS. Per ulteriori informazioni relative alle chiamate delle azioni mediante l'interfaccia REST, vedi la [Guida di riferimento API REST](https://console.bluemix.net/apidocs/openwhisk).

## Salvataggio del codice azione
{: #save-action}

Puoi ottenere e salvare localmente il codice associato a un'azione esistente. Puoi salvare il codice per tutte le azioni tranne le sequenze e le azioni Docker
{: shortdesc}

Salva il codice azione in un nome file che corrisponde a un nome di azione esistente nella directory di lavoro corrente.
```
ibmcloud fn action get actionName --save
```
{: pre}

Viene utilizzata un'estensione file che corrisponde al tipo di azione. Per il codice azione che è un file zip, viene utilizzata un'estensione .zip. Output di esempio:
```
ok: saved action code to /absolutePath/currentDirectory/actionName.js
```
{: screen}

Puoi invece fornire un percorso, nome ed estensione file personalizzati utilizzando l'indicatore `--save-as`.
```
ibmcloud fn action get actionName --save-as codeFile.js
```
{: pre}

Output di esempio:
```
ok: saved action code to /absolutePath/currentDirectory/codeFile.js
```
{: screen}

## Monitoraggio dei log di azione
{: #monitor-action-output}

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

## Eliminazione di azioni
{: #deleting-actions}

Puoi effettuare una pulizia eliminando le azioni che non desideri utilizzare.

1. Elimina un'azione.
    ```
    ibmcloud fn action delete hello
    ```
    {: pre}

    Output di esempio:
    ```
    ok: deleted hello
    ```
    {: screen}

2. Verifica che l'azione non venga più mostrata nell'elenco di azioni.
    ```
    ibmcloud fn action list
    ```
    {: pre}

    Output di esempio:
    ```
    actions
    ```
    {: screen}
