---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Creazione di API REST API senza server
{: #openwhisk_apigateway}

Utilizza le API per gestire direttamente le azioni {{site.data.keyword.openwhisk}}. Il gateway API funge da proxy per le [azioni web](./openwhisk_webactions.html) e fornisce l'instradamento del metodo HTTP, segreti e ID client, limiti di frequenza, CORS, visualizzazione dell'utilizzo dell'API, visualizzazione dei log di risposta e politiche di condivisione API.
{: shortdesc}

Per ulteriori informazioni sulla gestione delle API, puoi consultare la [documentazione sulla gestione delle API](/docs/api-management/manage_openwhisk_apis.html#manage_openwhisk_apis).

## Creazione della tua prima API
{: #create_cli_api}

Prima di iniziare, installa il plug-in della CLI [{{site.data.keyword.openwhisk_short}}](bluemix_cli.html).

1. Salva il seguente codice in un file JavaScript denominato `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. Crea un'azione web denominata `hello` utilizzando il file che hai creato. **Nota:** assicurati di aggiungere l'indicatore `--web true`.
  ```
  ibmcloud fn action create hello hello.js --web true
  ```
  {: pre}

  Output di esempio:
  ```
  ok: created action hello
  ```
  {: screen}

3. Crea un'API con il percorso di base `/hello`, il percorso `/world`, il metodo `get` e il tipo di risposta `json`
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

  Viene generato un nuovo URL che espone l'azione `hello` utilizzando un metodo HTTP GET

4. Invia una richiesta HTTP di test all'URL utilizzando il comando cURL.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  Output di esempio:
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

Viene richiamata l'azione web `hello` che restituisce un oggetto JSON che include il parametro **name** nel parametro di query. Puoi passare i parametri all'azione con semplici parametri di query o utilizzando il corpo della richiesta. Le azioni web possono richiamare pubblicamente un'azione senza utilizzare la chiave API di autorizzazione {{site.data.keyword.openwhisk_short}}.

## Utilizzo del controllo completo sulla risposta HTTP
{: #full_control}

L'indicatore `--response-type` controlla l'URL di destinazione dell'azione web che il gateway API deve trasmettere tramite proxy. Ad esempio, quando utilizzi l'indicatore `--response-type json`, il risultato completo dell'azione viene restituito in formato JSON e l'intestazione **Content-Type** viene automaticamente impostata su `application/json`.

Per restituire tipi di contenuto differenti nel corpo, usa il controllo completo sulle proprietà della risposta HTTP quali **statusCode** e **headers**. Puoi utilizzare l'indicatore `--response-type http` per configurare l'URL di destinazione dell'azione web con l'estensione `http`. Puoi modificare il codice dell'azione per rispettare la restituzione delle azioni web con l'estensione `http` o includere l'azione in una sequenza per passare il suo risultato a una nuova azione. La nuova azione può quindi trasformare il risultato in modo che abbia una formattazione appropriata per una risposta HTTP. Per ulteriori informazioni sui tipi di risposta e sulle estensioni delle azioni web, consulta la documentazione delle [azioni web](./openwhisk_webactions.html).

1. Modifica il codice per l'azione `hello.js` restituendo le proprietà JSON `body`, `statusCode` e `headers`.
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

2. Aggiorna l'azione con il risultato modificato.
  ```
  ibmcloud fn action update hello hello.js --web true
  ```
  {: pre}

3. Aggiorna il tipo di risposta API utilizzando l'indicatore `--response-type http`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

4. Richiama l'API aggiornata utilizzando il seguente comando cURL.
  ```
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

## Esposizione di più azioni web
{: #multiple_web_actions}

Puoi esporre più azioni web per implementare il tuo backend applicazione. Ad esempio, per esporre una serie di azioni per un club letterario, puoi utilizzare la serie di azioni per implementare il tuo backend per tale club:

| Azione | Metodo HTTP | Descrizione |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Ottenere i dettagli del libro  |
| postBooks   | POST | Aggiungere un libro |
| putBooks    | PUT | Aggiornare i dettagli del libro |
| deleteBooks | DELETE | Eliminare un libro |

In questo esempio, l'API è definita con un parametro percorso. Quando utilizzi i parametri percorso, l'API deve essere definita con un tipo di risposta `http`. Il valore del percorso, che inizia con il percorso di base e include i valori effettivi del parametro di percorso, è disponibile nel campo `__ow_path` del parametro JSON dell'azione. Per ulteriori dettagli sui campi di contesto HTTP, vedi la documentazione [Contesto HTTP delle azioni web](./openwhisk_webactions.html#http-context).

Per provare questo esempio di azioni web per il club letterario:

1. Crea un'API per il club letterario, denominata `Book Club`, con `/club` come percorso di base dell'URL HTTP, `books` come risorsa e `{isbn}` come parametro di percorso che viene utilizzato per identificare un libro specifico utilizzando il suo codice ISBN (International Standard Book Number).
  ```
  ibmcloud fn api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud fn api create /club /books get getBooks                       --response-type http
  ibmcloud fn api create /club /books post postBooks                     --response-type http
  ibmcloud fn api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud fn api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: pre}

  La prima azione esposta con il percorso di base `/club` è etichettata con il nome `Book Club`. Tutte le altre azioni esposte in `/club` vengono ora associate a `Book Club`.

2. Elenca tutte le azioni `Book Club` esposte.
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

3. Aggiungi un libro intitolato `JavaScript: The Good Parts` utilizzando un HTTP POST.
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

4. Ottieni un elenco dei libri utilizzando una chiamata HTTP GET all'azione `getBooks`.
  ```
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

5. Elimina uno specifico libro utilizzando una chiamata HTTP DELETE all'azione `deleteBooks`. In questo esempio, il valore del campo `__ow_path` dell'azione `deleteBooks` è `/club/books/978-0596517748`, dove `978-0596517748` è il valore effettivo `{isbn}` del percorso.
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

## Esportazione ed importazione della configurazione
{: #export_import_config}

Per esportare o importare una configurazione, puoi continuare ad utilizzare l'esempio del club letterario.

1. Esporta l'API `Book Club` in un file denominato `club-swagger.json`. Questo file può essere utilizzato come base per creare di nuovo le API utilizzando un file come input.
  ```
  ibmcloud fn api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. Verifica il file swagger eliminando prima tutti gli URL esposti in un percorso di base comune.
  ```
  ibmcloud fn api delete /club
  ```
  {: pre}

  Output di esempio:
  ```
  ok: deleted API /club
  ```
  {: screen}

  Puoi eliminare tutti gli URL esposti utilizzando il percorso di base `/club` o l'etichetta del nome API `"Book Club"`.
  {: tip}

3. Ripristina l'API `Book Club` utilizzando il file `club-swagger.json`.
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

4. Verifica che l'API `Book Club` sia stata creata nuovamente.
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

## Modifica della configurazione
{: #modify_config}

Dopo che hai creato la tua configurazione, puoi utilizzare la [**scheda API**](https://console.bluemix.net/openwhisk/apimanagement) nel dashboard {{site.data.keyword.openwhisk_short}} per modificare la configurazione nei seguenti modi.

* [Crea una {{site.data.keyword.openwhisk_short}} API](https://console.bluemix.net/openwhisk/apimanagement) che racchiude una serie di azioni {{site.data.keyword.openwhisk_short}}.
* [Proteggi la tua API](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) applicando la sicurezza API e le politiche di limitazione delle frequenze.
* [Gestisci il traffico](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) visualizzando le statistiche di utilizzo API e controllando i log di risposte.
* [Socializza e condividi](https://console.bluemix.net/docs/apis/management/manage_apis.html#share_api) la tua API con gli sviluppatori all'interno e all'esterno di {{site.data.keyword.Bluemix_notm}}.

Una volta terminato l'aggiornamento della configurazione, puoi scaricare il file di definizione in formato JSON e quindi reimportarlo utilizzando la CLI. Scaricare e importare la configurazione è utile, ad esempio, per una distribuzione non presidiata in una pipeline di integrazione e distribuzione continua (CICD). Hai anche la possibilità di caricare e reimportare il file di definizione dell'API utilizzando l'interfaccia utente.
