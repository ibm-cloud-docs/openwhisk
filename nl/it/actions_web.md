---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: web actions, serverless

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


# Creazione di azioni web
{: #actions_web}

Le azioni web sono azioni {{site.data.keyword.openwhisk}} che sono annotate per consentire rapidamente agli sviluppatori di creare applicazioni basate sul web. Queste azioni annotate permettono agli sviluppatori di programmare la logica di backend a cui la tua applicazione web può accedere in modo anonimo, senza richiedere una chiave di autenticazione {{site.data.keyword.openwhisk_short}}. Spetta allo sviluppatore di azioni implementare la propria autenticazione e autorizzazione desiderate o il flusso OAuth.
{: shortdesc}

Le attivazioni dell'azione web sono associate all'utente che ha creato l'azione. Questa azione rimanda il costo dell'attivazione dal chiamante al proprietario dell'azione.

Guarda la seguente azione JavaScript `hello.js`:
```javascript
function main({name}) {
  var msg = 'you did not tell me who you are.';
  if (name) {
    msg = `hello ${name}!`
  }
  return {body: `<html><body><h3>${msg}</h3></body></html>`}
}
```
{: codeblock}

Puoi creare un'_azione web_ **hello** nel pacchetto `demo` per lo spazio dei nomi `guest` utilizzando l'indicatore `--web` della CLI con un valore di `true` o `yes`:
```
ibmcloud fn package create demo
```
{: pre}

```
ibmcloud fn action create /guest/demo/hello hello.js --web true
```
{: pre}

L'utilizzo dell'indicatore `--web` con un valore `true` o `yes` consente ad un'azione di essere accessibile tramite un'interfaccia REST senza necessità di credenziali. Per configurare un'azione web con le credenziali, vedi la sezione [Protezione delle azioni web](#actions_web_secure). Un'azione web può essere richiamata utilizzando un URL strutturato nel seguente modo:
`https://{APIHOST}/api/v1/web/{namespace}/{packageName}/{actionName}.{EXT}`.

Il nome del pacchetto è **default** se l'azione non si trova in un pacchetto denominato.

Un esempio è `guest/demo/hello`. Il percorso dell'API dell'azione web può essere utilizzato con `curl` o `wget` senza una chiave API. Può anche essere immesso direttamente nel tuo browser.

Prova ad aprire `https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello?name=Jane` nel tuo browser web oppure prova a richiamare l'azione utilizzando `curl`:
```
curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello?name=Jane
```
{: pre}

Nel seguente esempio, un'azione web esegue un reindirizzamento HTTP:
```javascript
function main() {
  return {
    headers: { location: 'http://openwhisk.org' },
    statusCode: 302
  }
}
```
{: codeblock}

Nel seguente esempio, un'azione web imposta un singolo cookie:
```javascript
function main() {
  return {
    headers: {
      'Set-Cookie': 'UserID=Jane; Max-Age=3600; Version=',
      'Content-Type': 'text/html'
    }, 
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
}
```
{: codeblock}

Nel seguente esempio, un'azione web imposta più cookie:
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

Il seguente esempio restituisce un `image/png`:
```javascript
function main() {
    let png = <base 64 encoded string>
    return { headers: { 'Content-Type': 'image/png' },
             statusCode: 200,
             body: png };
}
```
{: codeblock}

The following example returns `application/json`:
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

The default `Content-Type` for an HTTP response is `application/json`, and the body can be any allowed JSON value. The default `Content-Type` can be omitted from the headers.

It is important to be aware of the [limite della dimensione della risposta](/docs/openwhisk?topic=cloud-functions-limits) per le azioni poiché una risposta che supera i limiti predefiniti del sistema ha esisto negativo. Ad esempio, gli oggetti di grandi dimensioni non vengono inviati in linea tramite {{site.data.keyword.openwhisk_short}}, ma vengono invece rimandati ad un archivio oggetti.

## Gestione di richieste HTTP con le azioni
{: #actions_web_http}

Un'azione {{site.data.keyword.openwhisk_short}} che non è un'azione web richiede l'autenticazione e deve rispondere con un oggetto JSON.

Le azioni web possono essere richiamate senza autenticazione e possono essere utilizzate per implementare gestori HTTP che rispondono con il contenuto di `headers`, `statusCode` e `body` di diversi tipi.
Le azioni web devono restituire un oggetto JSON. Tuttavia, il controller considera un'azione web in modo diverso se il suo risultato include una o più delle seguenti proprietà JSON di livello superiore:

- `headers`: un oggetto JSON in cui le chiavi sono nomi intestazione e i valori sono valori stringa, numero o booleano. Per inviare più valori per una singola intestazione, il valore dell'intestazione è un array di più valori JSON. Non viene configurata alcuna intestazione per impostazione predefinita.
- `statusCode`: un codice di stato HTTP valido. Se il contenuto del corpo è presente, il valore predefinito è `200 OK`. Se il contenuto del corpo non è presente, il valore predefinito è `204 No Content`.
- `body`: una stringa di testo semplice, un array o un oggetto JSON o una stringa con codifica base64 per i dati binari. Il corpo viene considerato vuoto se è `null`, la stringa è vuota `""` o non definita. Il valore predefinito è un corpo vuoto.

Il controller passa tutte le intestazioni specificate dall'azione, il codice di stato o il corpo al client HTTP che termina la richiesta o la risposta. Se l'intestazione `Content-Type` non viene dichiarata nelle intestazioni (`headers`) del risultato dell'azione il corpo viene interpretato come `application/json` per i valori non stringa e altrimenti `text/html`. Se viene definita l'intestazione `Content-Type`, il controller determina se la risposta è costituita da dati binari o testo semplice e decodifica la stringa utilizzando un decodificatore base64 secondo necessità. Se il corpo non viene decodificato correttamente, viene restituito un errore al client.


## Contesto HTTP
{: #actions_web_context}

Tutte le azioni web, quando richiamate, ricevono i dettagli della richiesta HTTP come parametri per l'argomento di input dell'azione.

Vedi i seguenti parametri HTTP:

- `__ow_method` (tipo: stringa). Il metodo HTTP della richiesta.
- `__ow_headers` (tipo: associazione da stringa a stringa): le intestazioni della richiesta.
- `__ow_path` (tipo: stringa): il percorso non corrispondente della richiesta (la corrispondenza si interrompe una volta consumata l'estensione dell'azione).
- `__ow_user` (tipo: stringa): lo spazio dei nomi che identifica il soggetto autenticato {{site.data.keyword.openwhisk_short}}
- `__ow_body` (tipo: stringa): l'entità del corpo della richiesta, come stringa codificata in base64 quando il contenuto è binario o altrimenti stringa semplice
- `__ow_query` (tipo: stringa): i parametri di query dalla richiesta sotto forma di stringa non analizzata

Una richiesta non può sovrascrivere alcuno dei parametri `__ow_` nominati. Questo potrebbe comportare una richiesta con esito negativo con stato uguale a 400 Richiesta non valida.

Il parametro `__ow_user` è presente solo se l'azione web è [annotata per richiedere l'autenticazione](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions) e consente a un'azione web di implementare la sua propria politica di autorizzazione. Il parametro `__ow_query` è disponibile solo quando un'azione web sceglie di gestire la [richiesta HTTP "non elaborato"](#actions_web_raw_enable). Si tratta di una stringa che contiene i parametri di query analizzati dall'URI (separati da `&`). La proprietà `__ow_body` è presente nelle richieste HTTP "non elaborato" o se l'entità della richiesta HTTP non è un oggetto JSON o dati di un modulo. In caso contrario, le azioni web ricevono parametri di query e corpo come proprietà di prima classe nell'argomento dell'azione. I parametri di corpo hanno la precedenza sui parametri di query, che a loro volta hanno la precedenza sui parametri di azione e pacchetto.

## Supporto per l'endpoint HTTPS
{: #actions_web_endpoint}

Protocolli SSL supportati: TLS 1.2, TLS 1.3 ([versione in bozza 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18))

Protocolli SSL non supportati: SSLv2, SSLv3, TLS 1.0, TLS 1.1

## Funzioni aggiuntive
{: #actions_web_extra}

Le azioni web offrono funzioni aggiuntive che includono:

- `Estensioni di contenuto`: la richiesta deve specificare il suo tipo di contenuto desiderato come `.json`, `.html`, `.http`, `.svg` o `.text`. Il tipo viene specificato aggiungendo un'estensione al nome dell'azione nell'URI, in modo che l'azione `/guest/demo/hello` sia indicata come `/guest/demo/hello.http`, ad esempio, per ricevere una risposta HTTP. Per praticità, viene utilizzata l'estensione `.http` se non viene rilevata alcuna estensione.
- `Proiezione dei campi dal risultato`: il percorso che segue il nome dell'azione viene utilizzato per proiettare uno o più livelli della risposta.
`/guest/demo/hello.html/body`. Questa funzione consente a un'azione che restituisce un dizionario `{body: "..." }` di proiettare la proprietà `body` e di restituire direttamente il suo valore stringa. Il percorso proiettato segue un modello di percorso assoluto (come in XPath).
- `Parametri di query e corpo come input`: l'azione riceve i parametri di query così come i parametri del corpo della richiesta. L'ordine di precedenza per l'unione dei parametri è: parametri di pacchetto, parametri di azione, parametri di query e parametri di corpo. Ognuno di questi parametri può sovrascrivere qualsiasi valore precedente se si verifica una sovrapposizione. Ad esempio, `/guest/demo/hello.http?name=Jane` può passare l'argomento `{name: "Jane"}` all'azione.
- `Dati del modulo`: oltre allo standard `application/json`, le azioni web possono ricevere come input l'URL codificato dai dati `application/x-www-form-urlencoded data`.
- `Attivazione con più verbi HTTP`: un'azione web può essere richiamata tramite uno di questi metodi HTTP: `GET`, `POST`, `PUT`, `PATCH` e `DELETE`, oltre a `HEAD` e `OPTIONS`.
- `Gestione di entità HTTP non elaborato e corpo diverso da JSON`: un'azione web può accettare un corpo di richiesta HTTP diverso da un oggetto JSON e può scegliere di ricevere sempre tali valori come valori opachi (testo semplice quando non è binario o stringa codificata in Base64).

Il seguente esempio illustra brevemente come utilizzare queste funzioni in un'azione web. Considera un'azione `/guest/demo/hello` con il seguente corpo:
```javascript
function main(params) {
    return { response: params };
}
```

Quando questa azione viene richiamata come azione web, puoi modificare la risposta dell'azione web proiettando percorsi diversi dal risultato.

Ad esempio, per restituire l'intero oggetto e vedere quali argomenti riceve l'azione:
```
curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json
```
{: pre}

Output di esempio:
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

Per l'esecuzione con un parametro di query, vedi il seguente comando di esempio:
```
 curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json?name=Jane
 ```
{: pre}

Output di esempio:
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

Puoi anche eseguire i dati del modulo:
```
 curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json -d "name":"Jane"
 ```
{: pre}

Output di esempio:
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

Immetti il seguente comando per un oggetto JSON:
```
 curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
```
{: pre}

Output di esempio:
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

Immetti il seguente comando per proiettare il nome (come testo):
```
curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.text/response/name?name=Jane
```
{: pre}

Output di esempio:
```
Jane
```
{: screen}

Per praticità, i parametri di query, i dati del modulo e le entità del corpo dell'oggetto JSON vengono tutti considerati come dizionari e i relativi valori sono direttamente accessibili come proprietà di input dell'azione. Questo comportamento non vale per le azioni web, che scelgono di gestire le entità della richiesta HTTP in modo più diretto o quando l'azione web riceve un'entità che non è un oggetto JSON.

Vedi il seguente esempio che utilizza un tipo di contenuto "testo", come mostrato in precedenza.
```
curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
```
{: pre}

Output di esempio:
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

## Estensioni di contenuto
{: #actions_web_ext}

Un'estensione del contenuto è necessaria solitamente per richiamare un'azione web. L'assenza di un'estensione presuppone `.http` come valore predefinito. Le estensioni `.json` e `.http` non richiedono un percorso di proiezione, che è richiesto invece dalle estensioni `.html`, `.svg` e `.text`. Per comodità, si presume che il percorso predefinito corrisponda al nome dell'estensione. Per richiamare un'azione web e ricevere una risposta `.html`, l'azione deve rispondere con un oggetto JSON contenente una proprietà di livello superiore denominata `html` (o la risposta deve essere nel percorso esplicito). In altre parole, `/guest/demo/hello.html` equivale a proiettare esplicitamente la proprietà `html`, come in `/guest/demo/hello.html/html`. Il nome completo dell'azione deve includere il nome del suo pacchetto, che è `default` se l'azione non si trova in un pacchetto denominato.

## Parametri protetti
{: #actions_web_protect}

I parametri di azione sono protetti e trattati come immutabili. I parametri vengono finalizzati automaticamente per abilitare le azioni web.
```
ibmcloud fn action create /guest/demo/hello hello.js --parameter name Jane --web true
```
{: pre}

Come risultato di queste modifiche, `name` viene collegato a `Jane` e non può essere sovrascritto dai parametri di query o corpo per via dell'annotazione finale. Questo intento protegge l'azione dai parametri di query o corpo che tentano di modificare questo valore, sia per errore che intenzionalmente.

## Protezione delle azioni web
{: #actions_web_secure}

Per impostazione predefinita, un'azione web può essere richiamata da chiunque abbia l'URL di chiamata dell'azione web. Utilizza l'[annotazione di azione Web](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions) `require-whisk-auth` per proteggere l'azione Web. Quando l'annotazione `require-whisk-auth` è impostata su `true`, l'azione eseguirà l'autenticazione delle credenziali per l'autorizzazione di base della richiesta di chiamata rispetto alla chiave di autenticazione whisk del proprietario dell'azione. Quando è impostata su un numero o su una stringa sensibile al maiuscolo/minuscolo, la richiesta di chiamata dell'azione deve includere un'intestazione `X-Require-Whisk-Auth` avente questo stesso valore. Se la convalida delle credenziali ha esito negativo, le azioni web protette restituiranno il messaggio `Not Authorized`.

In alternativa, utilizza l'indicatore `--web-secure` per impostare automaticamente l'annotazione `require-whisk-auth`.  Se impostata su `true`, viene generato un numero casuale come valore dell'annotazione `require-whisk-auth`. Se impostata su `false`, l'annotazione `require-whisk-auth` viene rimossa.  Se impostata su qualsiasi altro valore, questo valore viene utilizzato come valore dell'annotazione `require-whisk-auth`.

Esempio con **--web-secure**:
```bash
ibmcloud fn action update /guest/demo/hello hello.js --web true --web-secure my-secret
```
{: pre}

Esempio con **require-whisk-auth**:
```bash
ibmcloud fn action update /guest/demo/hello hello.js --web true -a require-whisk-auth my-secret
```
{: pre}

Esempio con **X-Require-Whisk-Auth**:
```bash
curl https://${APIHOST}/api/v1/web/guest/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: my-secret"
```
{: pre}

È importante notare che il proprietario dell'azione web possiede tutti i record delle attivazioni e sostiene il costo dell'esecuzione dell'azione nel sistema indipendentemente dal modo in cui è stata richiamata l'azione.

## Disabilitazione delle azioni web
{: #actions_web_disable}

Per disabilitare la chiamata di un'azione web tramite l'API web (`https://openwhisk.bluemix.net/api/v1/web/`), passa il valore `false` o `no` all'indicatore `--web` per aggiornare un'azione con la CLI.
```
ibmcloud fn action update /guest/demo/hello hello.js --web false
```
{: pre}

## Gestione HTTP non elaborato
{: #actions_web_raw}

Un'azione web può scegliere di interpretare ed elaborare direttamente un corpo HTTP in entrata, senza la promozione di un oggetto JSON a proprietà di prima classe disponibili per l'input dell'azione (ad esempio, `args.name` rispetto all'analisi di `args.__ow_query`). Questo processo viene eseguito attraverso un'[annotazione](/docs/openwhisk?topic=cloud-functions-annotations) `raw-http`. Utilizzando lo stesso esempio mostrato in precedenza, ma adesso come azione web HTTP "non elaborato" che riceve `name`, sia come parametro di query sia come valore JSON nel corpo della richiesta HTTP, si avrà il seguente risultato:
```
curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}

Output di esempio:
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

OpenWhisk utilizza il framework [Http Akka](https://doc.akka.io/docs/akka-http/current/?language=scala) per [determinare](https://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html) quali tipi di contenuto sono binari e quali sono in testo semplice.

### Abilitazione della gestione HTTP non elaborato
{: #actions_web_raw_enable}

Le azioni web HTTP non elaborato sono abilitate tramite l'indicatore `--web` utilizzando il valore `raw`.
```
ibmcloud fn action create /guest/demo/hello hello.js --web raw
```
{: pre}

### Disabilitazione della gestione HTTP non elaborato
{: #actions_web_raw_disable}

La disabilitazione di HTTP non elaborato può essere eseguita passando il valore `false` o `no` all'indicatore `--web`.
```
ibmcloud fn update create /guest/demo/hello hello.js --web false
```
{: pre}

### Decodifica del contenuto del corpo binario da Base64
{: #actions_web_decode}

Quando viene elaborato il contenuto HTTP non elaborato, il contenuto di `__ow_body` viene codificato in Base64 quando il `Content-Type` della richiesta è binario. Le seguenti funzioni mostrano come decodificare il contenuto del corpo in Node, Python e Swift. Salva semplicemente un metodo in un file, crea un'azione web HTTP non elaborato che utilizza la risorsa salvata e richiama quindi l'azione web.

#### Node
{: #actions_web_decode_js}

```javascript
function main(args) {
    decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
    return {body: decoded}
}
```
{: codeblock}

#### Python
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

#### Swift
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

Come esempio, salva la funzione Node come `decode.js` ed esegui questi comandi:
```
ibmcloud fn action create decode decode.js --web raw
```
{: pre}

Output di esempio:
```
ok: created action decode
```
{: screen}

```
curl -k -H "content-type: application" -X POST -d "Decoded body" https:// us-south.functions.cloud.ibm.com/api/v1/web/guest/default/decodeNode.json
```
{: pre}

Output di esempio:
```
{
  "body": "Decoded body"
}
```
{: screen}

## Richieste Options
{: #actions_web_options}

Per impostazione predefinita, una richiesta OPTIONS eseguita su un'azione web genera intestazioni CORS che vengono automaticamente aggiunte alle intestazioni di risposta. Queste intestazioni consentono tutte le origini e tutti i verbi HTTP: options, get, delete, post, put, head e patch.

Vedi le seguenti intestazioni:
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

In alternativa, le richieste OPTIONS possono essere gestite manualmente da un'azione web. Per abilitare questa opzione, aggiungi
un'annotazione `web-custom-options` con valore `true` a un'azione web. Se questa funzione è abilitata, le intestazioni CORS non vengono aggiunte automaticamente alla risposta della richiesta. È invece responsabilità dello sviluppatore aggiungere le intestazioni desiderate a livello di programmazione.

Vedi il seguente esempio per creare risposte personalizzate alla richieste OPTIONS:
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

Salva la funzione in `custom-options.js` ed esegui questi comandi:
```
ibmcloud fn action create custom-option custom-options.js --web true -a web-custom-options true
```
{: pre}

```
$ curl https://${APIHOST}/api/v1/web/guest/default/custom-options.http -kvX OPTIONS
```
{: pre}

Output di esempio:
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

Un'azione {{site.data.keyword.openwhisk_short}} non riesce in due diverse possibili modalità di errore. La prima è nota come _application error_ ed è analoga a un'eccezione rilevata: l'azione restituisce un oggetto JSON che contiene una proprietà `error` di livello superiore. La seconda è un _developer error_, che si verifica quando l'azione non riesce in modo irreversibile e non produce una risposta (simile a un'eccezione non rilevata). Per le azioni web, il controller gestisce gli errori applicazione nel seguente modo:

- Qualsiasi proiezione di percorso specificata viene ignorata e il controller proietta invece la proprietà `error`.
- Il controller applica la gestione dei contenuti prevista dall'estensione dell'azione al valore della proprietà `error`.

Gli sviluppatori devono sapere come possono essere utilizzate le azioni web e generare risposte di errore appropriate. Ad esempio, un'azione web utilizzata con l'estensione `.http` restituisce una risposta HTTP come `{error: { statusCode: 400 }`. In caso contrario, si verifica una mancata corrispondenza tra il `Content-Type` previsto dall'estensione e il `Content-Type` dell'azione nella risposta di errore. È necessario prestare particolare attenzione alle azioni web che sono sequenze, in modo che i componenti che costituiscono una sequenza possano generare errori adeguati laddove necessario.

## Esempio: generazione di un'immagine di codice QR dall'input
{: #actions_web_qr}

Questo è un esempio di un'azione web Java che utilizza `text` come input e genera un'immagine codice QR.

1. Crea un `Generate.java` nella directory `java_example/src/main/java/qr`.

  ```java
  package qr;

  import java.io.*;
  import java.util.Base64;

  import com.google.gson.JsonObject;

  import com.google.zxing.*;
  import com.google.zxing.client.j2se.MatrixToImageWriter;
  import com.google.zxing.common.BitMatrix;

  public class Generate {
    public static JsonObject main(JsonObject args) throws Exception {
      String property = "text";
      String text = "Hello. Try with a 'text' value next time.";
      if (args.has(property)) {
        text = args.get(property).toString();
      }

      ByteArrayOutputStream baos = new ByteArrayOutputStream();
      OutputStream b64os = Base64.getEncoder().wrap(baos);

      BitMatrix matrix = new MultiFormatWriter().encode(text, BarcodeFormat.QR_CODE, 300, 300);
      MatrixToImageWriter.writeToStream(matrix, "png", b64os);
      b64os.close();

      String output = baos.toString("utf-8");

      JsonObject response = new JsonObject();
      JsonObject headers = new JsonObject();
      headers.addProperty("content-type", "image/png; charset=UTF-8");
      response.add("headers", headers);
      response.addProperty("body", output);
      return response;
    }
  }
  ```
  {: codeblock}

3. Compila il JAR dell'azione web immettendo il seguente comando dalla directory `java_example` in cui è ubicato il file `build.gradle`.

  ```bash
  gradle jar
  ```
  {: pre}

4. Distribuisci l'azione web utilizzando il JAR `build/libs/java_example-1.0.jar`.

  ```bash
  ibmcloud fn action update QRGenerate build/libs/java_example-1.0.jar --main qr.Generate -m 128 --web true
  ```
  {: pre}

5. Richiama l'URL pubblico dell'endpoint dell'azione web e assegnalo a una variabile di ambiente.

  ```bash
  ibmcloud fn action get QRGenerate --url
  URL=$(ibmcloud fn action get QRGenerate --url | tail -1)
  ```
  {: pre}

6. Puoi aprire un browser web utilizzando questo `URL` e accodando il parametro di query `text` al messaggio che deve essere codificato nell'immagine QR. Puoi anche utilizzare un client HTTP come `curl` per scaricare un'immagine QR.

  ```bash
  curl -o QRImage.png $URL\?text=https://cloud.ibm.com
  ```
  {: pre}
