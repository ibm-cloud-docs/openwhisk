---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Creazione di API REST API senza server
{: #openwhisk_apigateway}

Le azioni {{site.data.keyword.openwhisk}} possono trarre vantaggio dall'essere gestite direttamente dalle API introducendo il gateway API, che funge da proxy per le [azioni web](./openwhisk_webactions.html) e fornisce loro ulteriori funzioni. Le funzioni aggiuntive includono: instradamento del metodo HTTP, segreti/ID client, limite di frequenza, CORS, visualizzazione dell'utilizzo delle API, visualizzazione dei log di risposta e politiche di condivisione API. Per ulteriori informazioni sulla gestione delle API, puoi consultare la [documentazione sulla gestione delle API](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis).
{: shortdesc}

## Crea le API dalle azioni web OpenWhisk utilizzando il tuo browser
{: #create_api_browser}

Puoi utilizzare la [**scheda API**](https://console.bluemix.net/openwhisk/apimanagement) all'interno del [dashboard {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/) per effettuare le seguenti attività:

* [Creare un'API Cloud Functions](https://console.bluemix.net/openwhisk/apimanagement) - Crea un'API che contiene un insieme di azioni OpenWhisk.
* [Proteggere la tua API](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - Applica le politiche di sicurezza e limitazione della frequenza per proteggere la tua API.
* [Gestire il traffico](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - Visualizza le statistiche di utilizzo dell'API e controlla i log di risposte.
* [Collaborare e condividere](https://console.bluemix.net/docs/apis/management/manage_apis.html#share_api) - Condividi la tua API con gli sviluppatori all'interno e all'esterno di {{site.data.keyword.Bluemix_notm}}.

## Crea le API dalle azioni web OpenWhisk utilizzando il plug-in della CLI
{: #create_api_cli}

La seguente sezione ti guida attraverso le attività di gestione dell'API utilizzando il plug-in della CLI {{site.data.keyword.openwhisk_short}}. Per creare e gestire le API attraverso la CLI, devi prima installare il [plug-in della CLI {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/docs/openwhisk/bluemix_cli.html) per {{site.data.keyword.Bluemix_notm}}.

Per comodità, i passi sono suddivisi in argomenti secondari più piccoli a cui puoi saltare rapidamente utilizzando il seguente elenco di attività API:

* [Crea la tua prima API](openwhisk_apigateway.html#create_cli_api)
* [Controllo completo sulla risposta HTTP](openwhisk_apigateway.html#full_control)
* [Esposizione di più azioni web](openwhisk_apigateway.html#multiple_web_actions)
* [Esporta la configurazione](openwhisk_apigateway.html#export_config)
* [Importa la configurazione](openwhisk_apigateway.html#import_config)
* [Modifica la configurazione](openwhisk_apigateway.html#modify_config)

### Crea la tua prima API mediante la CLI
{: #create_cli_api}

1. Crea un file JavaScript denominato **hello.js** contenente il seguente contenuto:
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. Crea un'azione web denominata **hello** utilizzando il file `hello.js` creato nel passo 1. **Nota:** assicurati di aggiungere l'indicatore `--web true`.
  ```
  ibmcloud fn action create hello hello.js --web true
  ```
  {: pre}

  Output di esempio:
  ```
  ok: created action hello
  ```
  {: screen}

3. Crea un'API con il percorso di base `/hello`, il percorso `/world` e il metodo `get` con il tipo di risposta `json`:
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  Output di esempio:
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  Viene generato un nuovo URL che espone l'azione `hello` utilizzando un metodo HTTP __GET__.

4. Infine, invia una richiesta HTTP all'URL utilizzando il comando **curl**:
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  Output di esempio:
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

Viene richiamata l'azione web **hello** che restituisce un oggetto JSON che include il parametro **name** inviato attraverso il parametro di query. Puoi passare i parametri all'azione con semplici parametri di query o utilizzando il corpo della richiesta. Le azioni web possono richiamare un'azione in modo pubblico senza utilizzare la chiave API di autorizzazione OpenWhisk.

### Controllo completo sulla risposta HTTP
{: #full_control}

L'indicatore `--response-type` controlla l'URL di destinazione dell'azione web che il gateway API deve trasmettere tramite proxy. L'utilizzo di `--response-type json` restituisce il risultato completo dell'azione in formato JSON e imposta automaticamente l'intestazione Content-Type su `application/json`.

Vuoi avere il controllo completo sulle proprietà di risposta HTTP come `statusCode` e `headers`, in modo da poter restituire diversi tipi di contenuto nel `body`. L'indicatore `--response-type http` rende possibile tutto questo configurando l'URL di destinazione dell'azione web con l'estensione `http`.

Puoi scegliere di modificare il codice dell'azione per rispettare la restituzione delle azioni web con l'estensione `http` o includere l'azione in una sequenza per passare il suo risultato a una nuova azione. La nuova azione può quindi trasformare il risultato in modo che abbia una formattazione appropriata per una risposta HTTP. Per ulteriori informazioni sui tipi di risposta e sulle estensioni delle azioni web, consulta la documentazione delle [azioni web](./openwhisk_webactions.html).

1. Modifica il codice per `hello.js` restituendo le proprietà JSON `body`, `statusCode` e `headers`:
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello world ${name}`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. Aggiorna l'azione con il risultato modificato:
  ```
  ibmcloud fn action update hello hello.js --web true
  ```
  {: pre}

3. Aggiorna il tipo di risposta dell'API utilizzando l'indicatore `--response-type http`:
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

4. Richiama l'API aggiornata utilizzando il seguente comando **curl**:
  ```bash
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  Output di esempio:
  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  {: screen}

Ora hai il pieno controllo delle tue API e puoi controllare il contenuto come l'URL di restituzione o impostare il codice di stato per errori come Non trovato (404), Non autorizzato (401) o Errore interno (500).

### Esposizione di più azioni web
{: #multiple_web_actions}

Se, ad esempio, vuoi esporre una serie di azioni per un club letterario, puoi utilizzare la serie di azioni per implementare il tuo backend per tale club:

| Azione | Metodo HTTP | Descrizione |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Ottenere i dettagli del libro  |
| postBooks   | POST | Aggiungere un libro |
| putBooks    | PUT | Aggiornare i dettagli del libro |
| deleteBooks | DELETE | Eliminare un libro |

In questo esempio, l'API è definita con un **parametro percorso**. Quando si utilizzano i parametri percorso, l'API deve essere definita con un tipo di risposta `http`. Il valore del percorso, che inizia con il percorso di base e include i valori effettivi del parametro di percorso, è disponibile nel campo `__ow_path` del parametro JSON dell'azione. Per ulteriori dettagli, fai riferimento alla documentazione [Contesto HTTP delle azioni web](./openwhisk_webactions.html#http-context), che include informazioni sugli altri campi del contesto HTTP disponibili per le azioni web richiamate con un tipo di risposta `http`.

1. Crea un'API per il club letterario, denominata **Book Club**, con `/club` come percorso di base dell'URL HTTP, `books` come risorsa e `{isbn}` come parametro di percorso che viene utilizzato per identificare un libro specifico utilizzando il suo codice ISBN (International Standard Book Number).
  ```bash
  ibmcloud fn api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud fn api create /club /books get getBooks                       --response-type http
  ibmcloud fn api create /club /books post postBooks                     --response-type http
  ibmcloud fn api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud fn api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: codeblock}

  Nota che la prima azione esposta con il percorso di base `/club` ottiene l'etichetta dell'API con il nome **Book Club**. Tutte le altre azioni esposte in `/club` vengono ora associate a **Book Club**.

2. Elenca tutte le azioni **Book Club** esposte utilizzando il seguente comando:
  ```
  ibmcloud fn api list /club -f
  ```
  {: pre}

  Output di esempio:
  ```
  ok: APIs
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: postBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: post
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: putBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: put
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: deleteBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: delete
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

3. Per divertimento, puoi aggiungere un libro intitolato **JavaScript: The Good Parts** con un HTTP __POST__:
  ```
  curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' -H "Content-Type: application/json" https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  Output di esempio:
  ```
  {
    "result": "success"
  }
  ```
  {: screen}

4. Ottieni un elenco di libri utilizzando l'azione **getBooks** con l'HTTP __GET__:
  ```bash
  curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  Output di esempio:
  ```
  {
    "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
  }
  ```
  {: screen}

5. Puoi eliminare un libro specifico utilizzando l'azione **deleteBooks** con l'HTTP __DELETE__. In questo esempio, il valore del campo `__ow_path` dell'azione **deleteBooks** è `/club/books/978-0596517748`, dove `978-0596517748` è il valore effettivo `{isbn}` del percorso.
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

### Esporta la configurazione
{: #export_config}

1. Esporta l'API denominata **Book Club** in un file che può essere utilizzato come base per ricreare le API utilizzando un file come input.
  ```
  ibmcloud fn api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. Verifica il file swagger eliminando prima tutti gli URL esposti in un percorso di base comune utilizzando il seguente comando:
  ```
  ibmcloud fn api delete /club
  ```
  {: pre}

  Output di esempio:
  ```
  ok: deleted API /club
  ```
  {: screen}

  Puoi eliminare tutti gli URL esposti utilizzando il percorso di base `/club` o l'etichetta del nome API **"Book Club"**:
  {: tip}

### Importa la configurazione
{: #import_config}

1. Adesso, ripristina l'API denominata **Book Club** utilizzando il file denominato `club-swagger.json`:
  ```
  ibmcloud fn api create --config-file club-swagger.json
  ```
  {: pre}

  Output di esempio:
  ```
  ok: created api /club/books/{isbn} get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} put for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} delete for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books post for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: screen}

2. Verifica che l'API **Book Club** venga ricreata:
  ```
  ibmcloud fn api list /club
  ```
  {: pre}

  Output di esempio:
  ```
  ok: apis
  Action                    Verb         API Name        URL
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

### Modifica la configurazione utilizzando l'interfaccia utente
{: #modify_config}

Puoi modificare la configurazione nel dashboard {{site.data.keyword.openwhisk_short}}; fai clic sulla [scheda API](https://console.ng.bluemix.net/openwhisk/apimanagement) per configurare la sicurezza, i limiti di frequenza e altre funzioni. Una volta terminato l'aggiornamento della configurazione, puoi scaricare il file di definizione in formato JSON e quindi reimportarlo utilizzando la CLI. Ciò può essere utile, ad esempio, per una distribuzione non presidiata in una pipeline di integrazione e distribuzione continua (CICD). Hai anche la possibilità di caricare e reimportare il file di definizione dell'API utilizzando l'interfaccia utente.
