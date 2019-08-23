---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: web actions, serverless, functions

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


# Creazione di azioni web
{: #actions_web}

Quando crei un'azione web, il risultato è un URL che può essere utilizzato per attivare l'azione da qualsiasi applicazione web.
{: shortdesc}

## Perché utilizzare le azioni web anziché le azioni standard?

### 1. Esegui le azioni web in modo anonimo

Le attivazioni dell'azione web sono associate all'utente che ha creato l'azione, piuttosto che al chiamante dell'azione. Di solito, per le chiamate API ad applicazioni come Github, dovresti includere un nome utente e un token con la chiamata API per un utente specifico o un ID funzionale. Quando utilizzi un'azione web, questi tipi di credenziali non sono richiesti. Un'azione web è accessibile tramite un'interfaccia REST senza la necessità di credenziali.

Sebbene non sia necessario utilizzare le credenziali con le azioni web, puoi implementare la tua propria autenticazione e autorizzazione o flusso OAuth. Per configurare un'azione web con le credenziali, vedi [Protezione delle azioni web](#actions_web_secure).

### 2. Utilizza qualsiasi tipo di richiesta HTTP

Per impostazione predefinita, le azioni accettano solo le richieste `POST`, ma le azioni web possono essere richiamate tramite uno di questi metodi HTTP: `GET`, `POST`, `PUT`, `PATCH` e `DELETE`, nonché `HEAD` e `OPTIONS`.

### 3. Attiva un'azione web da qualsiasi luogo

Quando crei un'azione web {{site.data.keyword.openwhisk}}, generi un URL per richiamare tale azione da qualsiasi applicazione basata sul web. Le azioni che non sono azioni web richiedono l'autenticazione e devono rispondere con un oggetto JSON. 

Un percorso API dell'azione web può essere utilizzato con cURL, `wget` o può essere immesso direttamente nel tuo browser. Un'azione web può essere richiamata utilizzando un URL strutturato nel seguente modo: `https://<apihost>/api/v1/web/<namespace>/<packageName>/<actionName>.<ext>`.

### 4. Crea meno entità {{site.data.keyword.openwhisk_short}}

Poiché puoi richiamare un'azione web da qualsiasi luogo, non sei obbligato a creare altre entità {{site.data.keyword.openwhisk_short}} come trigger o regole.

## Come funzionano le azioni web?

Le azioni web possono essere richiamate senza autenticazione e possono essere utilizzate per implementare gestori HTTP che rispondono con il contenuto di `headers`, `statusCode` e `body` di diversi tipi.

Le azioni web devono restituire un oggetto JSON. Tuttavia, il controller considera un'azione web in modo diverso se il suo risultato include una o più delle seguenti [proprietà JSON](#web_action_properties) di livello superiore.
{: shortdesc}

## Funzioni disponibili delle azioni web
{: #actions_web_extra}

Le azioni web supportano le seguenti funzioni:

| Funzione | Descrizione |
| --- | --- |
| [Estensioni del contenuto](#extra_features) | Per le richieste HTTP, puoi specificare un tipo di contenuto come `.json`, `.html`, `.http`, `.svg` o `.text`. Se non viene specificato alcun tipo di contenuto, verrà utilizzata l'estensione `.http`. Puoi specificare un tipo di contenuto aggiungendo un'estensione al nome dell'azione nell'URI in modo che un'azione `demo/hello` venga denominata `/demo/hello.svg`. Le estensioni `.json` e `.http` non richiedono un percorso di proiezione, che è richiesto invece dalle estensioni `.html`, `.svg` e `.text`. Si presume che il percorso predefinito corrisponda al nome dell'estensione. Per richiamare un'azione web e ricevere una risposta `.html`, l'azione deve rispondere con un oggetto JSON contenente una proprietà di livello superiore denominata `html` (o la risposta deve essere nel percorso esplicito). In altre parole, `/<namespace>/demo/hello.html` equivale a proiettare esplicitamente la proprietà `html`, come in `/<namespace>/demo/hello.html/html`. Il nome completo dell'azione deve includere il nome del suo pacchetto, che è `default` se l'azione non si trova in un pacchetto denominato. |
| [Proiezione dei campi dal risultato](#projecting_fields) | Il percorso che segue il nome dell'azione viene utilizzato per proiettare uno o più livelli della risposta. Ad esempio, `/demo/hello.html/body`. Questa funzione consente a qualsiasi azione web che restituisce un dizionario, ad esempio `{body: "..." }`, di proiettare la proprietà `body` e di restituire direttamente il suo valore stringa anziché il suo valore di dizionario. Il percorso proiettato segue un modello di percorso assoluto (come in XPath). |
| [Parametri di query e corpo come input](#query_test) | L'azione riceve i parametri di query così come i parametri del corpo della richiesta. L'ordine di precedenza per l'unione dei parametri è: parametri di pacchetto, parametri di azione, parametri di query e parametri di corpo. Ognuno di questi parametri può sovrascrivere qualsiasi valore precedente se si verifica una sovrapposizione. Ad esempio, `/demo/hello.http?name=Jane` può passare l'argomento `{name: "Jane"}` all'azione. |
| [Dati del modulo](#form_data) | Oltre allo standard `application/json`, le azioni web possono ricevere dati del modulo con codifica URL `application/x-www-form-urlencoded data` come input.
| [Attivazioni mediante più verbi HTTP](#actions_web_options) | Un'azione web può essere richiamata tramite uno di questi metodi HTTP: `GET`, `POST`, `PUT`, `PATCH` e `DELETE`, nonché `HEAD` e `OPTIONS`. |
| [Gestione delle entità corpo diverso da JSON e HTTP non elaborato](#actions_web_raw_enable) | Un'azione web può accettare un corpo di richiesta HTTP diverso da un oggetto JSON e può scegliere di ricevere sempre tali valori come valori opachi (testo semplice quando non è un file binario, altrimenti stringa con codifica base64). |

## Creazione di un'azione web
{: #actions_web_example}

Per creare un'azione web: 

1. Salva il seguente codice JavaScript come `hello.js`.

  ```javascript
  function main({name}) {
    var msg = 'You did not tell me who you are.';
  if (name) {
      msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
  }
  ```
  {: codeblock}

2. Crea il pacchetto `demo`. Il nome del pacchetto è `default` a meno che non venga specificato esplicitamente.
  ```
  ibmcloud fn package create demo
  ```
  {: pre}

3. Crea l'azione `hello`. In questo esempio, i valori `packageName/actionName` corrispondono a `demo/hello`. Sostituisci la variabile `<filepath>` con il percorso del tuo file `hello.js` e imposta l'indicatore `--web` su `true`. 

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

4. Richiama o verifica l'azione web `hello` senza alcun parametro. Sostituisci le variabili `<apihost>` e `<namespace>`. Per ottenere l'`<apihost>`, esegui `ibmcloud fn property get --apihost`. Esempio di `<apihost>`: `us-south.functions.cloud.ibm.com`.

  Per gli spazi dei nomi abilitati a IAM, sostituisci la variabile `<namespace>` con l'ID spazio dei nomi. Per ottenere l'ID, esegui `ibmcloud fn namespace get <namespace_name>`.
  {: note}

  a. Puoi verificare l'azione web in uno dei seguenti modi: 
    * Aprendo un URL con la seguente struttura `https://<apihost>/api/v1/web/<namespace>/demo/hello` nel tuo browser.
    * Verificando l'azione con un comando cURL.
      ```
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

    * Verificando l'azione con un comando `wget`.  
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

  b. Il codice di azione restituisce il seguente dizionario.
    ```
    {body: `<html><body><h3>${msg}</h3></body></html>`}
    ``` 
    {: screen}
    
  Puoi anche verificare l'azione restituendo solo la proprietà `body` usando il seguente comando:
  {: #projecting_fields}

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello.html/body
    ```
    {: pre}

    **Output di esempio**

    Poiché il parametro `<name>` non è stato specificato, viene restituito il seguente messaggio.
    ```
    <html><body><h3>You did not tell me who you are.</h3></body></html>
    ```
    {: screen}

5. Ora prova a definire il parametro `<name>`. Verifica l'azione con un parametro `<name>` in uno dei seguenti modi:
  * Aprendo `https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane` nel tuo browser. 
  * Verificando l'azione con un comando cURL.

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}
  * Verificando l'azione con un comando `wget`.  
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}

  **Output di esempio**
  ```
  <html><body><h3>Hello, Jane!</h3></body></html>
  ```
  {: screen}


**Passi successivi**

Aggiungi l'URL per la tua azione web `hello` alla tua applicazione web e verificala da lì.

### Proprietà JSON dell'azione web
{: #web_action_properties}

Il `Content-Type` predefinito per una risposta HTTP è `application/json` e il corpo può essere qualsiasi valore JSON consentito. Se il tuo `Content-Type` non è `application/json`, devi specificare un `Content-Type` nelle `headers` del tuo codice di azione.

Se viene raggiunto il [limite della dimensione del risultato](/docs/openwhisk?topic=cloud-functions-limits) per le azioni, la risposta ha esito negativo. Se sai che il risultato della tua azione è maggiore di 5 MB, configura un [archivio oggetti](/docs/openwhisk?topic=cloud-functions-pkg_obstorage).

| Proprietà JSON | Descrizione |
| --- | --- |
| `headers`| Un oggetto JSON in cui le chiavi sono nomi di intestazione e i valori sono valori stringa, numero o booleano. Per inviare più valori per una singola intestazione, il valore dell'intestazione è un array di più valori JSON. Non viene configurata alcuna intestazione per impostazione predefinita. |
| `statusCode` | Un codice di stato HTTP valido. Se il contenuto del corpo è presente, il valore predefinito è `200 OK`. Se il contenuto del corpo non è presente, il valore predefinito è `204 No Content`. |
| `body` | Una stringa di testo semplice, un oggetto o un array JSON o una stringa con codifica in base64 per i dati binari. Il corpo viene considerato vuoto se è `null`, la stringa è vuota `""` o non definita. Il valore predefinito è un corpo vuoto. |

Il [controller](/docs/openwhisk?topic=cloud-functions-about#about_controller) passa qualsiasi intestazione specificata dall'azione, codice di stato o corpo al client HTTP che termina la richiesta o la risposta. Se l'intestazione `Content-Type` non viene dichiarata nelle intestazioni (`headers`) del risultato dell'azione il corpo viene interpretato come `application/json` per i valori non stringa e altrimenti `text/html`. Se viene definita l'intestazione `Content-Type`, il controller determina se la risposta è costituita da dati binari o testo semplice e decodifica la stringa utilizzando un decodificatore base64 secondo necessità. Se il corpo non viene decodificato correttamente, viene restituito un errore al client.

Il proprietario dell'azione web possiede tutti i record di attivazione e sostiene i costi di esecuzione dell'azione nel sistema indipendentemente dal modo in cui l'azione è stata richiamata.
{: note}

#### Parametri protetti
I parametri di azione sono protetti e possono essere modificati solo aggiornando l'azione. I parametri vengono finalizzati automaticamente per abilitare le azioni web.

```
ibmcloud fn action create /<namespace>/demo/hello hello.js --parameter name Jane --web true
```
{: pre}


Come risultato di queste modifiche, `name` viene collegato a `Jane` e non può essere sovrascritto dai parametri di query o corpo per via dell'annotazione finale. Questo intento protegge l'azione dai parametri di query o corpo che tentano di modificare questo valore, sia per errore che intenzionalmente.

### Esecuzione di un reindirizzamento HTTP utilizzando un'azione web
{: #http_redirect}
Potresti utilizzare questa funzione in un'applicazione web per reindirizzare un utente alla nuova versione del tuo sito.

**Prima di iniziare**
Crea il pacchetto `demo` e l'azione web `hello` completando i passi descritti in [Creazione di un'azione web](#actions_web_example).

Per creare un'azione web che esegue un reindirizzamento HTTP:

1. Salva il codice come `hello.js`.

  ```javascript
  function main() {
    return {
      headers: { location: 'https://cloud.ibm.com/openwhisk/' },
      statusCode: 302
    }
  }
  ```
  {: codeblock}

2. Aggiorna la tua azione web `hello` con la nuova versione del tuo codice `hello.js`. Sostituisci `<filepath>` con il percorso del tuo file `hello.js`.

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Verifica l'azione web `hello`. Sostituisci le variabili `<apihost>` e `<namespace>`. Puoi verificare l'azione web in uno dei seguenti modi:

  * Aprendo l'URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` nel tuo browser. 
  * Eseguendo il seguente comando cURL:
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * Eseguendo il seguente comando `wget`:
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

  **Risultato di esempio** 
  
  Questa azione web di esempio reindirizza il tuo browser al [dashboard {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk/){: external}.

### Impostazione dei cookie utilizzando un'azione web
{: #multiple_cookie}
Potresti utilizzare questa funzione in un'applicazione web per archiviare un token web JSON come cookie di sessione dopo un accesso riuscito.

Per creare un'azione web che imposta più cookie:

**Prima di iniziare**
Crea il pacchetto `demo` e l'azione web `hello` completando i passi descritti in [Creazione di un'azione web](#actions_web_example).

1. Salva il codice come `hello.js`.
  ```javascript
  function main() {
    return {
      headers: {
        'Set-Cookie': [
          'UserID=Jane; Max-Age=3600; Version=',
        'SessionID=asdfgh123456; Path = /'
        ],
      'Content-Type': 'text/html'
      }, 
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
  }
  ```
  {: codeblock}

2. Aggiorna la tua azione web `hello` con la nuova versione del tuo codice `hello.js`. Sostituisci `<filepath>` con il percorso del tuo file `hello.js`.

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}
    
3. Cancella i cookie del tuo browser prima di verificare l'azione.

4. Verifica l'azione web `hello` aprendo l'URL nel tuo browser. Sostituisci le variabili `<apihost>` e `<namespace>` e apri `https://<apihost>/api/v1/web/<namespace>/demo/hello`. Esempio di `<apihost>`: `us-south.functions.cloud.ibm.com`.

**Risultato**

I cookie `UserID=Jane` e `SessionID=asdfgh123456` sono impostati negli strumenti di sviluppo del tuo browser.


### Restituzione di un'immagine utilizzando un'azione web
{: #return_image}
Potresti utilizzare questa funzione in un'applicazione web per restituire l'immagine della bandiera di un paese in base alla locale dell'utente.

**Prima di iniziare** 

Crea il pacchetto `demo` e l'azione web `hello` completando i passi descritti in [Creazione di un'azione web](#actions_web_example).

Per creare un'azione web che restituisce `image/png`: 

1. Salva il codice come `hello.js`.

  ```javascript
  function main() {
      let png = '<base 64 encoded string';
      return { headers: { 'Content-Type': 'image/png' },
              statusCode: 200,
              body: png };
  }
  ```
  {: codeblock}

2. Aggiorna la tua azione web `hello` con la nuova versione del tuo codice `hello.js`. Sostituisci `<filepath>` con il percorso del tuo file `hello.js`.

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Verifica l'azione nel tuo browser o utilizzando un comando cURL. Sostituisci le variabili `<apihost>` e `<namespace>`. Puoi verificare l'azione web in uno dei seguenti modi:

  * Aprendo l'URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` nel tuo browser. 
  * Eseguendo il seguente comando cURL.
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * Eseguendo il seguente comando `wget`.
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}



### Restituzione di JSON utilizzando un'azione web
{: #return_json}
Potresti utilizzare questa funzione in un'applicazione web per restituire un oggetto JSON delle informazioni IP dell'utente.

**Prima di iniziare** 

Crea il pacchetto `demo` e l'azione web `hello` completando i passi descritti in [Creazione di un'azione web](#actions_web_example).

Per creare un'azione web che restituisce `application/json`:

1. Salva il codice come `hello.js`.
  ```javascript
  function main(params) {
      return {
          statusCode: 200,
          headers: { 'Content-Type': 'application/json' },
          body: params
      };
  }
  ```
  {: codeblock}

2. Aggiorna la tua azione web `hello` con la nuova versione del tuo codice `hello.js`. Sostituisci `<filepath>` con il percorso del tuo file `hello.js`.
  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Verifica l'azione nel tuo browser o utilizzando un comando cURL. Sostituisci le variabili `<apihost>` e `<namespace>`. Puoi verificare l'azione web in uno dei seguenti modi:
  * Aprendo l'URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` nel tuo browser. 
  * Eseguendo il seguente comando cURL:
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * Eseguendo il seguente comando `wget`:
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

    **Output di esempio**

    ```
    {
      "__ow_headers": {
        "accept": "*/*",
        "accept-encoding": "gzip",
        "cdn-loop": "cloudflare",
        "cf-connecting-ip": "XX.XXX.XXX.XXX",
        "cf-ipcountry": "US",
        "cf-ray": "4d9f3e442a86cf28-IAD",
        "cf-visitor": "{\"scheme\":\"https\"}",
        "host": "<apihost>",
        "user-agent": "curl/7.54.0",
        "x-forwarded-for": "XX.XXX.XX.XXX, XX.XXX.XX.XXX",
        "x-forwarded-host": "<apihost>",
        "x-forwarded-port": "443",
        "x-forwarded-proto": "https",
        "x-global-k8fdic-transaction-id": "11fd03071bd0841d3a00f52354ab880f",
        "x-real-ip": "XXX.XX.XX.XX",
        "x-request-id": "11fd03071bd0841d3a00f52354ab880f"
      },
      "__ow_method": "get",
      "__ow_path": ""
    }
    ```
    {: screen}


### Contesto HTTP
{: #actions_web_context}

Tutte le azioni web, quando richiamate, ricevono i dettagli della richiesta HTTP come parametri di input per l'argomento dell'azione.

| Parametro HTTP | Tipo | Descrizione |
| --- | --- | --- |
| `__ow_method` | Stringa | Il metodo HTTP della richiesta. |
| `__ow_headers` | Associazione da stringa a stringa | Le intestazioni della richiesta. |
| `__ow_path` | Stringa | Il percorso non corrispondente della richiesta (la corrispondenza si interrompe una volta consumata l'estensione dell'azione). |
| `__ow_user` | Stringa | Lo spazio dei nomi che identifica il soggetto autenticato da {{site.data.keyword.openwhisk_short}}. |
| `__ow_body` | Stringa | L'entità del corpo della richiesta, come stringa con codifica base64 quando il contenuto è un file binario, altrimenti come stringa semplice. |
| `__ow_query` | Stringa | I parametri di query dalla richiesta sotto forma di stringa non analizzata. |

Una richiesta non può sovrascrivere alcuno dei parametri `__ow_` nominati. Questo potrebbe comportare una richiesta con esito negativo con stato uguale a 400 Richiesta non valida.

Il parametro `__ow_user` è presente solo se l'azione web è [annotata per richiedere l'autenticazione](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions) e consente a un'azione web di implementare la sua propria politica di autorizzazione. Il parametro `__ow_query` è disponibile solo quando un'azione web sceglie di gestire la [richiesta HTTP "non elaborato"](#actions_web_raw_enable). `__ow_query` è una stringa che contiene i parametri di query analizzati dall'URI (separati da `&`). La proprietà `__ow_body` è presente nelle richieste HTTP non elaborate o quando l'entità della richiesta HTTP non è un oggetto JSON o dati del modulo. In caso contrario, le azioni web ricevono parametri di query e corpo come proprietà di prima classe nell'argomento dell'azione. I parametri di corpo hanno la precedenza sui parametri di query, che a loro volta hanno la precedenza sui parametri di azione e pacchetto.

### Supporto per l'endpoint HTTPS
{: #actions_web_endpoint}

Protocolli SSL supportati: TLS 1.2, TLS 1.3 ([versione in bozza 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18){: external})

### Modifica del contenuto della risposta dell'azione web
{: #extra_features}
Puoi modificare il contenuto della risposta di un'azione web in modo da restituire diversi tipi di contenuto utilizzando le [Estensioni del contenuto](#actions_web_extra).
{: shortdesc}

**Prima di iniziare**

Crea il pacchetto `demo` e l'azione web `hello` completando i passi descritti in [Creazione di un'azione web](#actions_web_example).

Per modificare la risposta di un'azione web:

1. Salva il seguente codice come `hello.js`.

  ```javascript
  function main(params) {
      return { response: params };
}
  ```
  {: codeblock}

2. Aggiorna la tua azione web `hello` con la nuova versione del tuo codice `hello.js`. Sostituisci `<filepath>` con il percorso del tuo file `hello.js`.

  ```bash
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Verifica l'azione nel tuo browser o utilizzando un comando cURL. Sostituisci le variabili `<apihost>` e `<namespace>`. 

  a. Restituisci il JSON in uno dei seguenti modi:
    * Aprendo `https://<apihost>/api/v1/web/<namespace>/demo/hello.json` nel tuo browser web. 
    * Eseguendo il seguente comando cURL.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}
    * Eseguendo il seguente comando `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}

      **Output di esempio**

      ```
      {
        "response": {
          "__ow_method": "get",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  b. Verifica l'azione utilizzando un parametro di query. Puoi verificare l'azione in uno dei seguenti modi:
  {: #query_test}

    * Eseguendo il seguente comando cURL.

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

    * Eseguendo il seguente comando `wget`.

        ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

      **Output di esempio**
      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "get",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  c. Puoi anche verificare l'azione web utilizzando i dati del modulo. Puoi verificare l'azione web in uno dei seguenti modi:
  {: #form_data}
  
    * Eseguendo il seguente comando cURL.

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}
      
    * Eseguendo il seguente comando `wget`.
      ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}

      **Output di esempio**

      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "content-length": "10",
            "content-type": "application/x-www-form-urlencoded",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  d. Puoi specificare un oggetto JSON immettendo il seguente comando. Puoi verificare l'azione web in uno dei seguenti modi:
    * Eseguendo il seguente comando cURL.
      ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}
      
    * Eseguendo il seguente comando `wget`.
      ```
        wget https://<apihost>/api/v1/web/{namespace/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}

      **Output di esempio**

      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "content-length": "15",
            "content-type": "application/json",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  e. Puoi anche restituire il valore `name` come testo in uno dei seguenti modi:
  * Eseguendo il seguente comando cURL.

      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}
  * Eseguendo il seguente comando `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}

    **Output di esempio**

    ```
    Jane
    ```
    {: screen}

    Nelle azioni standard, i parametri di query, i dati del modulo e le entità del corpo dell'oggetto JSON vengono tutti considerati come dizionari e i loro valori sono direttamente accessibili come proprietà di input dell'azione. Questo comportamento non vale per le azioni web, che gestiscono le entità di richiesta HTTP, o quando l'azione web riceve un'entità che non è un oggetto JSON.
    {: note}

  f. Puoi impostare il `Content-Type` in uno dei seguenti modi.
  * Eseguendo il seguente comando cURL.  
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}
    
  * Eseguendo il seguente comando `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}

    **Output di esempio**

    ```
    {
      "response": {
        "__ow_method": "post",
    "__ow_headers": {
          "accept": "*/*",
          "connection": "close",
          "content-length": "4",
          "content-type": "text/plain",
          "host": "172.17.0.1",
          "user-agent": "curl/7.43.0"
        },
        "__ow_path": "",
        "__ow_body": "Jane"
      }
    }
    ```
    {: screen}

## Protezione delle azioni web
{: #actions_web_secure}

**Prima di iniziare**
Crea il pacchetto `demo` e l'azione web `hello` completando i passi descritti in [Creazione di un'azione web](#actions_web_example).

Per impostazione predefinita, chiunque può richiamare un'azione web utilizzando l'URL di chiamata. Puoi utilizzare l'[annotazione di azione web](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions) `require-whisk-auth` per proteggere l'azione web in uno dei seguenti modi:
  1. Impostando l'annotazione `require-whisk-auth` su `true`. Quando l'annotazione `require-whisk-auth` è impostata su `true`, l'azione web autenticherà le credenziali dell'autorizzazione di base della richiesta di chiamata rispetto alla chiave di autenticazione del proprietario dell'azione web. Se è impostata su un numero o su una stringa con distinzione tra maiuscole e minuscole, la richiesta di chiamata dell'azione web deve includere l'intestazione `X-Require-Whisk-Auth` impostata su questo stesso numero o stringa con distinzione tra maiuscole e minuscole. Se la convalida delle credenziali non riesce, le azioni web protette restituiscono il messaggio `Not Authorized`.

  2. Consentendo di impostare automaticamente l'annotazione `require-whisk-auth` utilizzando l'indicatore `--web-secure`. Se l'indicatore `--web-secure` è impostato su `true`, viene generato un numero casuale come valore dell'annotazione `require-whisk-auth`. Se impostato su `false`, l'annotazione `require-whisk-auth` viene rimossa.  Se impostato su qualsiasi altro valore, questo valore viene utilizzato come valore dell'annotazione `require-whisk-auth`.

Per verificare un'azione web sicura:

1. Salva il seguente codice JavaScript come `hello.js`.
  ```javascript
  function main({name}) {
    var msg = 'You did not tell me who you are.';
  if (name) {
      msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
  }
  ```
  {: codeblock}

2. Aggiorna la tua azione web `hello` con la nuova versione del tuo codice `hello.js` e imposta l'indicatore `--web secure` su `true`.
  ```bash
  ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true
  ```
  {: pre}

3. Richiama l'azione web `hello` per visualizzare il valore `require-whisk-auth` generato in modo casuale.

  ```bash
  ibmcloud fn action get demo/hello
  ```
  {: pre}

    **Output di esempio**

    Il valore `require-whisk-auth` è stato impostato su `7819991076995522`.
    ```
    {
      "namespace": "<namespace>/demo",
      "name": "hello",
      "version": "0.0.34",
      "exec": {
          "kind": "nodejs:10",
          "binary": false
      },
      "annotations": [
          {
              "key": "web-export",
              "value": true
          },
          {
              "key": "raw-http",
              "value": false
          },
          {
              "key": "final",
              "value": true
          },
          {
              "key": "require-whisk-auth",
              "value": 7819991076995522
          },
          {
              "key": "exec",
              "value": "nodejs:10"
          }
      ],
    "limits": {
          "timeout": 60000,
          "memory": 256,
          "logs": 10,
          "concurrency": 1
      },
    "publish": false
}
    ```
    {: screen}

Per verificare che l'autenticazione funzioni:

1. Verifica l'azione web `hello` senza impostare il parametro `X-Require-Whisk-Auth` per controllare che l'autenticazione sia richiesta. Questa verifica provocherà un errore. Puoi verificare l'azione web in uno dei seguenti modi:

  * Verificando l'azione web con un comando cURL.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}
    
  * Verificando l'azione web con un comando `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}

   **Output di esempio**

    ```
      {
      "code": "4c4423556547f6ac764ee192d4ed27a6",
      "error": "Authentication is possible but has failed or not yet been provided."
    }
    ```
    {: screen}

    La chiamata non riesce perché non è stato fornito il valore `X-Require-Whisk-Auth`.
    {: note}

2. Adesso, verifica l'azione web `hello` e fornisci il valore `X-Require-Whisk-Auth` generato in modo casuale. Sostituisci i valori `<apihost>` e `<namespace>`. Sostituisci il valore `<my-secret>` con il numero generato in modo casuale creato nel passo 3. Puoi verificare l'azione web in uno dei seguenti modi:
  * Verificando l'azione web con un comando cURL.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  * Verificando l'azione web con un comando `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  **Output di esempio**
    
    ```
    {
    "body": "<html><body><h3>Hello, Jane!</h3></body></html>"
    }
    ```
    {: screen}

Per verificare un'azione web utilizzando un valore `require-whisk-auth` personalizzato:

1. Aggiorna la tua azione web `hello` con il tuo proprio valore `require-whisk-auth`. Quindi, prova a verificare l'azione web specificando il valore `X-Require-Whisk-Auth` durante la chiamata.

  a. Imposta un valore `require-whisk-auth` in cui `<my-secret>` è il tuo token di autenticazione con distinzione tra maiuscole e minuscole.
    ```bash
    ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true --require-whisk-auth <mysecret>
    ```
    {: pre}
  
  b. Verifica l'azione web e includi il tuo valore `<my-secret>`. Puoi verificare l'azione web in uno dei seguenti modi:
  * Verificando l'azione web con un comando cURL.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}
  * Verificando l'azione con un comando `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}


## Gestione HTTP non elaborato
{: #actions_web_raw}

Un'azione web può scegliere di interpretare ed elaborare direttamente un corpo HTTP in entrata, senza la promozione di un oggetto JSON a proprietà di prima classe disponibili per l'input dell'azione web (ad esempio, `args.name` rispetto all'analisi di `args.__ow_query`). Questo processo viene eseguito attraverso un'[annotazione](/docs/openwhisk?topic=cloud-functions-annotations) `raw-http`. Utilizzando lo stesso esempio mostrato in precedenza, ma adesso come azione web HTTP "non elaborato" che riceve `name`, sia come parametro di query sia come valore JSON nel corpo della richiesta HTTP, si avrà il seguente risultato:
```bash
curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}


**Output di esempio**
```
{
  "response": {
    "__ow_method": "post",
    "__ow_query": "name=Jane",
    "__ow_body": "eyJuYW1lIjoiSmFuZSJ9",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "15",      
      "content-type": "application/json",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```
{: screen}

{{site.data.keyword.openwhisk_short}} utilizza il framework [Akka HTTP](https://doc.akka.io/docs/akka-http/current/?language=scala){: external} per [determinare quali tipi di contenuto sono file binari e quali sono testo semplice](https://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html){: external}.

### Abilitazione della gestione HTTP non elaborato
{: #actions_web_raw_enable}

Puoi creare azioni web HTTP non elaborate impostando `--web` su `raw`.
```bash
ibmcloud fn action create demo/hello /<filepath>/hello.js --web raw
```
{: pre}

### Decodifica del contenuto del corpo binario da Base64
{: #actions_web_decode}

Quando viene elaborato il contenuto HTTP non elaborato, il contenuto di `__ow_body` viene codificato in Base64 se il `Content-Type` della richiesta è di tipo binario. Le seguenti funzioni mostrano come decodificare il contenuto del corpo in Node, Python e Swift.

1. Salva il codice di esempio con il linguaggio preferito in un file denominato `decode.<ext>`. Sostituisci `<ext>` con l'estensione del codice di esempio del tuo linguaggio preferito.

  **Node**
  {: #actions_web_decode_js}

  ```javascript
  function main(args) {
      decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
      return {body: decoded}
  }
  ```
  {: codeblock}

  **Python**
  {: #actions_web_decode_python}

  ```python
  def main(args):
    try:
        decoded = args['__ow_body'].decode('base64').strip()
        return {"body": decoded}
    except:
        return {"body": "Could not decode body from Base64."}
  ```
  {: codeblock}

  **Swift**
  {: #actions_web_decode_swift}

  ```swift
  extension String {
      func base64Decode() -> String? {
          guard let data = Data(base64Encoded: self) else {
              return nil
          }

          return String(data: data, encoding: .utf8)
      }
  }

func main(args: [String:Any]) -> [String:Any] {
      if let body = args["__ow_body"] as? String {
          if let decoded = body.base64Decode() {
              return [ "body" : decoded ]
        }
      }

      return ["body": "Could not decode body from Base64."]
  }
  ```
  {: codeblock}

2. Crea un'azione web HTTP non elaborata con il codice di esempio immettendo il seguente comando. In questo esempio, la funzione Node viene salvata come `decode.js`. Sostituisci il percorso file con il percorso del tuo file `decode` e aggiorna l'estensione in modo che corrisponda all'estensione del codice di esempio che hai utilizzato.

  ```bash
  ibmcloud fn action create decode <filepath>/decode.js --web raw
  ```
  {: pre}

  **Output di esempio**
  ```
  ok: created action decode
  ```
  {: screen}

3. Verifica l'azione `decode` eseguendo il seguente comando cURL.
    ```bash
    curl -k -H "content-type: application" -X POST -d "Decoded body" https://<apihost>/api/v1/web/<namespace>/default/decode.json
    ```
    {: pre}

  **Output di esempio**
    ```
    {
      "body": "Decoded body"
    }
    ```
    {: screen}

## Richieste Options
{: #actions_web_options}

Per impostazione predefinita, una richiesta `OPTIONS` effettuata a un'azione web produce intestazioni CORS che vengono aggiunte automaticamente alle intestazioni di risposta. Queste intestazioni consentono tutte le origini e i verbi HTTP `OPTIONS`, `GET`, `DELETE`, `POST`, `PUT`, `HEAD` e `PATCH`.
{: shortdesc}

Vedi le seguenti intestazioni:
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

In alternativa, le richieste `OPTIONS` possono essere gestite manualmente da un'azione web. Per abilitare questa opzione, aggiungi
un'annotazione `web-custom-options` con valore `true` a un'azione web. Se questa funzione è abilitata, le intestazioni CORS non vengono aggiunte automaticamente alla risposta della richiesta. Devi invece aggiungere le intestazioni a livello di programmazione.

Per creare risposte personalizzate alle richieste `OPTIONS`:

1. Salva il seguente codice in un file `custom-options.js`.

  ```js
  function main(params) {
    if (params.__ow_method == "options") {
      return {
        headers: {
          'Access-Control-Allow-Methods': 'OPTIONS, GET',
        'Access-Control-Allow-Origin': 'example.com'
        },
      statusCode: 200
    }
    }
  }
  ```
  {: codeblock}

2. Crea l'azione web. Imposta `--web-custom-options` su `true`.

  ```bash
  ibmcloud fn action create custom-option <filepath>/custom-options.js --web true -a web-custom-options true
  ```
  {: pre}

3. Verifica l'azione utilizzando il seguente comando cURL.

  ```bash
  $ curl https://<apihost>/api/v1/web/<namespace>/default/custom-option.http -kvX OPTIONS
  ```
  {: pre}

  **Output di esempio**
  ```
  < HTTP/1.1 200 OK
< Server: nginx/1.11.13
< Content-Length: 0
< Connection: keep-alive
< Access-Control-Allow-Methods: OPTIONS, GET
< Access-Control-Allow-Origin: example.com
  ```
  {: screen}

## Gestione degli errori
{: #actions_web_errors}

Un'azione {{site.data.keyword.openwhisk_short}} non riesce in due diverse possibili modalità di errore. La prima è nota come _application error_ ed è analoga a un'eccezione rilevata: l'azione restituisce un oggetto JSON che contiene una proprietà `error` di livello superiore. La seconda è un _developer error_, che si verifica quando l'azione non riesce e non produce una risposta (simile a un'eccezione non rilevata). Per le azioni web, il controller gestisce gli errori applicazione nel seguente modo:

- Qualsiasi proiezione di percorso specificata viene ignorata e il controller proietta invece la proprietà `error`.
- Il controller applica la gestione dei contenuti prevista dall'estensione dell'azione al valore della proprietà `error`.

Gli sviluppatori devono sapere come possono essere utilizzate le azioni web e generare risposte di errore appropriate. Ad esempio, un'azione web utilizzata con l'estensione `.http` restituisce una risposta HTTP come `{error: { statusCode: 400 }`. In caso contrario, si verifica una mancata corrispondenza tra il `Content-Type` previsto dall'estensione e il `Content-Type` dell'azione nella risposta di errore. È necessario prestare particolare attenzione alle azioni web che sono sequenze, in modo che i componenti che costituiscono una sequenza possano generare errori adeguati laddove necessario.



## Disabilitazione delle azioni web
{: #actions_web_disable}

Puoi disabilitare un'azione web impostando l'indicatore `--web` su `false` o su `no` nella CLI. Sostituisci `<packageName>/<actionName>` e `<filepath>/<filename>` con il nome del pacchetto, il nome dell'azione web, il percorso e il nome del tuo file del codice.

```bash
ibmcloud fn action update <packageName>/<actionName> <filepath>/<filename> --web false
```
{: pre}



