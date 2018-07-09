---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Gateway API
{: #openwhisk_apigateway}

Le azioni OpenWhisk possono trarre vantaggio dall'essere gestite dalla gestione delle API.

Il Gateway API funge da proxy per le [Azioni web](./openwhisk_webactions.html) e fornisce loro ulteriori funzioni. Le funzioni aggiuntive includono: instradamento del metodo HTTP, segreti/ID client, limite di frequenza, CORS, visualizzazione dell'utilizzo delle API, visualizzazione dei log di risposta e politiche di condivisione API.
Per ulteriori informazioni sulla gestione delle API, puoi consultare la [documentazione sulla gestione delle API](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis).
{: shortdesc}

## Crea API dalle azioni web OpenWhisk utilizzando il tuo browser.

Con Gateway API, puoi esporre un'azione OpenWhisk sotto forma di API. Dopo aver definito l'API, puoi applicare le politiche di sicurezza e di limite di frequenza, visualizzare i log di risposta e di utilizzo delle API e definire le politiche di condivisione API.
Nel dashboard OpenWhisk, fai clic sulla [scheda API](https://console.ng.bluemix.net/openwhisk/apimanagement).


## Crea API dalle azioni web OpenWhisk utilizzando la CLI

### Configurazione della CLI OpenWhisk

Configura la CLI OpenWhisk con l'host API.

Sono disponibili due regioni {{site.data.keyword.Bluemix_notm}} che richiedono i propri host API e chiave di autorizzazione univoci.

* Stati Uniti Sud
  * Host API: `openwhisk.ng.bluemix.net`

* Regno Unito
  * Host API: `openwhisk.eu-gb.bluemix.net`

Immetti il seguente comando per impostare l'host API per la regione Bluemix desiderata:

Stati Uniti Sud:
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

Regno Unito:
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

Se hai bisogno di cambiare la regione, devi riconfigurare la CLI sia con l'host API che con la chiave di autorizzazione poiché la chiave di autorizzazione è specifica per regione.
{: tip}

Risorse come azioni, regole e pacchetti sono specifiche della regione. Pertanto, se utilizzi la stessa risorsa in più regioni, devi distribuirla in ciascuna regione desiderata.

Per poter utilizzare il comando `wsk api`, il file di configurazione della CLI `~/.wskprops` deve contenere il token di accesso Bluemix.

Per ottenere il token di accesso, utilizza il seguente comando della CLI:
```
wsk bluemix login
```
{: pre}

Per ulteriori informazioni su questo comando, esegui:
```
wsk bluemix login -h
```
{: pre}

Se il comando `wsk bluemix login` non riesce e restituisce l'errore `BMXLS0202E: Stai utilizzando un ID utente federato, utilizza un passcode monouso per accedere con l'opzione --sso`, accedi con la CLI {{site.data.keyword.Bluemix_notm}} usando `bluemix login`, quindi immetti `wsk bluemix login --sso`.
{: tip}

### Crea la tua prima API mediante la CLI

1. Crea un file JavaScript con il seguente contenuto. In questo esempio, il nome file è "hello.js".
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}
  
2. Crea un'azione web dalla seguente funzione JavaScript. In questo esempio, l'azione è denominata 'hello'. Assicurati di aggiungere l'indicatore `--web true`.
  ```
  wsk action create hello hello.js --web true
  ```
  {: pre}

  ```
  ok: created action hello
  ```
  
3. Crea un'API con il percorso di base `/hello`, il percorso `/world` e il metodo `get` con il tipo di risposta `json`:
  ```
  wsk api create /hello /world get hello --response-type json
  ```

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  Viene generato un nuovo URL che espone l'azione `hello` tramite un metodo HTTP __GET__.
  
4. Infine, invia una richiesta HTTP all'URL.
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world?name=OpenWhisk
  ```

  ```json
  {
  "payload": "Hello world OpenWhisk"
  }
  ```

  Viene richiamata l'azione web `hello` che restituisce un oggetto JSON che include il parametro `name` inviato tramite il parametro di query. Puoi passare parametri all'azione tramite semplici parametri di query o tramite il corpo della richiesta. Le azioni web possono richiamare un'azione in modo pubblico senza la chiave API di autorizzazione OpenWhisk.
  
### Controllo completo sulla risposta HTTP
  
  L'indicatore `--response-type` controlla l'URL di destinazione dell'azione web che Gateway API deve trasmettere tramite proxy. L'utilizzo di `--response-type json` restituisce il risultato completo dell'azione in formato JSON e imposta automaticamente l'intestazione Content-Type su `application/json`. 
  
  Una volta iniziato, vuoi avere il controllo completo sulle proprietà della risposta HTTP come  `statusCode`, `headers` e restituire diversi tipi di contenuto nel `body`. L'indicatore `--response-type http` rende possibile tutto questo configurando l'URL di destinazione dell'azione web con l'estensione `http`.

  Puoi scegliere di modificare il codice dell'azione per rispettare la restituzione delle azioni web con estensione `http` o includere l'azione in una sequenza per passare il suo risultato a una nuova azione. La nuova azione può quindi trasformare il risultato in una formattazione appropriata per una risposta HTTP. Per ulteriori informazioni sui tipi di risposta e sulle estensioni delle azioni web, consulta la documentazione [Azioni web](./openwhisk_webactions.html).

  Modifica il codice per `hello.js` restituendo le proprietà JSON `body`, `statusCode` e `headers`
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: new Buffer(JSON.stringify({payload:`Hello world ${name}`})).toString('base64'), 
        statusCode:200, 
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}
  Nota che il corpo deve essere restituito con codifica `base64` e non come stringa.
  
  Aggiorna l'azione con il risultato modificato: 
  ```
  wsk action update hello hello.js --web true
  ```
  {: pre}

  Aggiorna l'API con `--response-type http`: 
  ```
  wsk api create /hello /world get hello --response-type http
  ```
  {: pre}
  
  Richiama l'API aggiornata: 
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  {: pre}

  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  Ora hai il pieno controllo delle tue API e puoi controllare il contenuto come l'URL di restituzione o impostare il codice di stato per errori come Non trovato (404), Non autorizzato (401) o Errore interno (500).

### Esposizione di più azioni web

Se, ad esempio, vuoi esporre una serie di azioni per un club letterario, puoi utilizzare la serie di azioni per implementare il tuo backend per il club letterario:

| Azione | Metodo HTTP | Descrizione |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Ottenere i dettagli del libro  |
| postBooks   | POST | Aggiungere un libro |
| putBooks    | PUT | Aggiornare i dettagli del libro |
| deleteBooks | DELETE | Eliminare un libro |

Crea un'API per il club letterario, denominata `Book Club`, con `/club` come percorso di base dell'URL HTTP e `books` come risorsa.
```
wsk api create -n "Book Club" /club /books get getBooks --response-type http
wsk api create /club /books post postBooks              --response-type http
wsk api create /club /books put putBooks                --response-type http
wsk api create /club /books delete deleteBooks          --response-type http
```

Nota che la prima azione esposta con il percorso di base `/club` ottiene l'etichetta dell'API con il nome `Book Club`. Le altre azioni esposte in `/club` vengono associate a `Book Club`.

Elenca tutte le azioni esposte utilizzando il seguente comando:
```
wsk api list -f
```
{: pre}

```
ok: APIs
Action: getBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: get
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: postBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: post
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: putBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: put
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: deleteBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: delete
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

Per divertimento, puoi aggiungere un libro, `JavaScript: The Good Parts`, con un HTTP __POST__:
```
curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": "success"
}
```

Ottieni un elenco di libri utilizzando l'azione `getBooks` tramite l'HTTP __GET__
```
curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
}
```

### Esporta la configurazione
Esporta l'API denominata `Book Club` in un file che può essere utilizzato come base per ricreare le API utilizzando un file come input. 
```
wsk api get "Book Club" > club-swagger.json
```
{: pre}

Verifica il file swagger eliminando prima tutti gli URL esposti in un percorso di base comune.
Puoi eliminare tutti gli URL esposti utilizzando il percorso di base `/club` o l'etichetta del nome API `"Book Club"`:
```
wsk api delete /club
```
```
ok: deleted API /club
```
### Modifica la configurazione

Puoi modificare la configurazione nel dashboard OpenWhisk; fai clic sulla [scheda API](https://console.ng.bluemix.net/openwhisk/apimanagement) per configurare la sicurezza, i limiti di frequenza e altre funzioni.

### Importa la configurazione

Adesso, ripristina l'API denominata `Book Club` utilizzando il file `club-swagger.json`
```
wsk api create --config-file club-swagger.json
```
{: pre}

```
ok: created api /books delete for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books get for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books post for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books put for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

Verifica che l'API sia stata ricreata:
```
wsk api list /club
```
{: pre}

```
ok: apis
Action                    Verb         API Name        URL
getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
