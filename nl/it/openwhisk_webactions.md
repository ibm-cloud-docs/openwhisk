---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Azioni web
{: #openwhisk_webactions}

Le azioni web sono azioni OpenWhisk, che sono annotate per consentire rapidamente agli sviluppatori di creare applicazioni basate sul web. Queste azioni annotate permettono agli sviluppatori di programmare la logica di backend a cui la tua applicazione web può accedere in modo anonimo, senza richiedere una chiave di autenticazione OpenWhisk. Spetta allo sviluppatore di azioni implementare la propria autenticazione e autorizzazione desiderata (ovvero, il flusso OAuth).
{: shortdesc}

Le attivazioni dell'azione web sono associate all'utente che ha creato l'azione. Questa azione rimanda il costo dell'attivazione dal chiamante al proprietario dell'azione.

Guarda la seguente azione JavaScript `hello.js`,
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

Puoi creare un'_azione web_ `hello` nel pacchetto `demo` per lo spazio dei nomi `guest` utilizzando l'indicatore `--web` della CLI con un valore di `true` o `yes`:
```
wsk package create demo
```
{: pre}

```
wsk action create /guest/demo/hello hello.js --web true
```
{: pre}

L'utilizzo dell'indicatore `--web` con un valore di `true` o `yes` consente ad un'azione di essere accessibile tramite l'interfaccia REST senza bisogno di credenziali. Un'azione web può essere richiamata utilizzando un URL strutturato nel seguente modo:
`https://{APIHOST}/api/v1/web/{namespace}/{packageName}/{actionName}.{EXT}`.

Il nome del pacchetto è `default` se l'azione non si trova in un pacchetto denominato.

Un esempio è `guest/demo/hello`. Il percorso dell'API dell'azione web può essere utilizzato con `curl` o `wget` senza una chiave API. Può anche essere immesso direttamente nel tuo browser.

Prova ad aprire [https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane](https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane) nel tuo browser web oppure prova richiamare l'azione tramite `curl`:
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane
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

It is important to be aware of the [limite della dimensione della risposta](./openwhisk_reference.html) per le azioni poiché una risposta che supera i limiti predefiniti del sistema ha esisto negativo. Ad esempio, gli oggetti di grandi dimensioni non vengono inviati in linea tramite OpenWhisk, ma vengono invece rimandati ad un archivio oggetti.

## Gestione di richieste HTTP con le azioni
{: #openwhisk_webactions_http}

Un'azione OpenWhisk che non è un'azione web richiede l'autenticazione e deve rispondere con un oggetto JSON. Al contrario, le azioni web possono essere richiamate senza autenticazione e possono essere utilizzate per implementare gestori HTTP che rispondono con il contenuto di _headers_, _statusCode_ e _body_ di diversi tipi. L'azione web deve restituire un oggetto JSON. Tuttavia, il sistema OpenWhisk (ossia il `controller`), considera un'azione web in modo diverso se il suo risultato include una o più delle seguenti proprietà JSON di livello superiore:

- `headers`: un oggetto JSON in cui le chiavi sono nomi intestazione e i valori sono valori stringa, numero o booleano per tali intestazioni ('impostazione predefinita è nessuna intestazione). Per inviare più valori per una singola intestazione, il valore dell'intestazione è un array di valori JSON.
- `statusCode`: un codice di stato HTTP valido (il valore predefinito è 200 OK).
- `body`: una stringa di testo semplice o una stringa con codifica base64 (per i dati binari).

Il controller deve inviare le intestazioni specificate dall'azione, se presenti, al client HTTP che termina la richiesta/risposta. Allo stesso modo, il controller risponde con il codice di stato, quando presente. Infine, il corpo viene inviato come corpo della risposta. A meno che un'intestazione `Content-Type` non sia dichiarata nelle `intestazioni` del risultato dell'azione, il corpo viene inviato come fosse una stringa (altrimenti restituisce un errore). Quando viene definito il `Content-Type`, il controller determina se la risposta è costituita da dati binari o testo semplice e decodifica la stringa utilizzando un decodificatore base64 secondo necessità. Se il corpo non viene decodificato correttamente, viene restituito un errore al chiamante.

_Nota_: un oggetto o array JSON viene considerato come dati binari e deve essere codificato in base64.

## Contesto HTTP

Tutte le azioni web, quando richiamate, ricevono i dettagli della richiesta HTTP come parametri per l'argomento di input dell'azione. 

Vedi i seguenti parametri HTTP:

- `__ow_method` (tipo: stringa). Il metodo HTTP della richiesta.
- `__ow_headers` (tipo: associazione da stringa a stringa): le intestazioni della richiesta.
- `__ow_path` (tipo: stringa): il percorso non corrispondente della richiesta (la corrispondenza si interrompe una volta consumata l'estensione dell'azione).
- `__ow_user` (tipo: stringa): lo spazio dei nomi che identifica il soggetto autenticato OpenWhisk
- `__ow_body` (tipo: stringa): l'entità del corpo della richiesta, come stringa codificata in base64 quando il contenuto è binario o altrimenti stringa semplice
- `__ow_query` (tipo: stringa): i parametri di query dalla richiesta sotto forma di stringa non analizzata

Una richiesta non può sovrascrivere alcuno dei parametri `__ow_` nominati. Questo potrebbe comportare una richiesta con esito negativo con stato uguale a 400 Richiesta non valida.

Il parametro `__ow_user` è presente solo se l'azione web è [annotata per richiedere l'autenticazione](./openwhisk_annotations.html#openwhisk_annotations_webactions) e consente a un'azione web di implementare la sua propria politica di autorizzazione. Il parametro `__ow_query` è disponibile solo quando un'azione web sceglie di gestire la [richiesta HTTP "non elaborato"](#raw-http-handling). Si tratta di una stringa che contiene i parametri di query analizzati dall'URI (separati da `&`). La proprietà `__ow_body` è presente nelle richieste HTTP "non elaborato" o se l'entità della richiesta HTTP non è un oggetto JSON o dati di un modulo. In caso contrario, le azioni web ricevono parametri di query e corpo come proprietà di prima classe nell'argomento dell'azione. I parametri di corpo hanno la precedenza sui parametri di query, che a loro volta hanno la precedenza sui parametri di azione e pacchetto.

## Supporto per l'endpoint HTTPS

Protocolli SSL supportati: TLS 1.0, TLS 1.1, TLS 1.2, TLS 1.3 ([versione in bozza 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18))

Protocolli SSL non supportati: SSLv2, SSLv3

## Funzioni aggiuntive
{: #extra-features}

Le azioni web offrono funzioni aggiuntive che includono:

- `Estensioni di contenuto`: la richiesta deve specificare il suo tipo di contenuto desiderato come  `.json`, `.html`, `.http`, `.svg` o `.text`. Il tipo viene specificato aggiungendo un'estensione al nome dell'azione nell'URI, in modo che l'azione `/guest/demo/hello` sia indicata come `/guest/demo/hello.http`, ad esempio, per ricevere una risposta HTTP. Per praticità, viene utilizzata l'estensione `.http` se non viene rilevata alcuna estensione.
- `Proiezione dei campi dal risultato`: il percorso che segue il nome dell'azione viene utilizzato per proiettare uno o più livelli della risposta.
`/guest/demo/hello.html/body`. Questa funzione consente a un'azione che restituisce un dizionario `{body: "..." }` di proiettare la proprietà `body` e di restituire direttamente il suo valore stringa. Il percorso proiettato segue un modello di percorso assoluto (come in XPath).
- `Parametri di query e corpo come input`: l'azione riceve i parametri di query così come i parametri del corpo della richiesta. L'ordine di precedenza per l'unione dei parametri è: parametri del pacchetto, parametri di azione, parametri di query e parametri di corpo. Ognuno di questi parametri può sovrascrivere qualsiasi valore precedente se si verifica una sovrapposizione. Ad esempio, `/guest/demo/hello.http?name=Jane` può passare l'argomento `{name: "Jane"}` all'azione.
- `Dati del modulo`: oltre allo standard `application/json`, le azioni web possono ricevere come input l'URL codificato dai dati `application/x-www-form-urlencoded data`.
- `Attivazione tramite più verbi HTTP`: un'azione web può essere richiamata attraverso uno di questi metodi HTTP: `GET`, `POST`, `PUT`, `PATCH` e `DELETE`, oltre a `HEAD` e `OPTIONS`.
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
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json
 ```
{: pre}
```json
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

Per l'esecuzione con un parametro di query, vedi il seguente comando di esempio:
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json?name=Jane
 ```
{: pre}
```json
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

Puoi anche eseguire i dati del modulo:
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -d "name":"Jane"
 ```
{: pre}
```json
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

Immetti il seguente comando per un oggetto JSON:
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
```
{: pre}
```json
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

Immetti il seguente comando per proiettare il nome (come testo):
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.text/response/name?name=Jane
```
{: pre}
```
Jane
```

Per praticità, i parametri di query, i dati del modulo e le entità del corpo dell'oggetto JSON vengono tutti trattati come dizionari e i relativi valori sono direttamente accessibili come proprietà di input dell'azione. Questo comportamento non vale per le azioni web, che scelgono di gestire le entità della richiesta HTTP in modo più diretto o quando l'azione web riceve un'entità che non è un oggetto JSON.

Vedi il seguente esempio che utilizza un tipo di contenuto "testo", come mostrato in precedenza.
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
```
{: pre}
```json
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


## Estensioni di contenuto
{: #openwhisk_webactions_extensions}

Un'estensione di contenuto è necessaria solitamente per richiamare un'azione web. L'assenza di un'estensione presuppone `.http` come valore predefinito. Le estensioni `.json` e `.http` non richiedono un percorso di proiezione, che è richiesto invece dalle estensioni `.html`, `.svg` e `.text`. Per comodità, si presume che il percorso predefinito corrisponda al nome dell'estensione. Per richiamare un'azione web e ricevere una risposta `.html`, l'azione deve rispondere con un oggetto JSON contenente una proprietà di livello superiore denominata `html` (o la risposta deve essere nel percorso esplicito). In altre parole, `/guest/demo/hello.html` equivale a proiettare esplicitamente la proprietà `html`, come in `/guest/demo/hello.html/html`. Il nome completo dell'azione deve includere il suo nome pacchetto, che è `default` se l'azione non si trova in un pacchetto denominato.

## Parametri protetti
{: #openwhisk_webactions_protected}

I parametri di azione sono protetti e trattati come immutabili. I parametri vengono finalizzati automaticamente per abilitare le azioni web.

```
 wsk action create /guest/demo/hello hello.js \
      --parameter name Jane \
      --web true
```

Come risultato di queste modifiche, `name` viene collegato a `Jane` e non può essere sovrascritto dai parametri di query o corpo per via dell'annotazione finale. Questo intento protegge l'azione dai parametri di query o corpo che tentano di modificare questo valore, sia per errore che intenzionalmente.  

## Disabilitazione delle azioni web

Per disabilitare il richiamo di un'azione web tramite la nuova API (`https://openwhisk.ng.bluemix.net/api/v1/web/`), passa il valore `false` o `no` all'indicatore `--web` per aggiornare un'azione con la CLI.

```
 wsk action update /guest/demo/hello hello.js --web false
```
{: pre}

## Gestione HTTP non elaborato

Un'azione web può scegliere di interpretare ed elaborare un corpo HTTP in entrata direttamente, senza la promozione di un oggetto JSON a proprietà di prima classe disponibili per l'input dell'azione (ad esempio, `args.name` rispetto all'analisi di `args.__ow_query`). Questo processo viene eseguito attraverso un [annotazione](./openwhisk_annotations.html) `raw-http`. Utilizzando lo stesso esempio mostrato in precedenza, ma adesso come azione web HTTP "non elaborato" che riceve `name`, sia come parametro di query sia come valore JSON nel corpo della richiesta HTTP, si avrà il seguente risultato:
```
 curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}' 
```
{: pre}
```json 
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

OpenWhisk utilizza il framework [Http Akka](http://doc.akka.io/docs/akka-http/current/scala/http/) per [determinare](http://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html) quali tipi di contenuto sono binari e quali sono in testo semplice.

### Abilitazione della gestione HTTP non elaborato

Le azioni web HTTP non elaborato sono abilitate tramite l'indicatore `--web` utilizzando il valore `raw`.

```
 wsk action create /guest/demo/hello hello.js --web raw
```

### Disabilitazione della gestione HTTP non elaborato

La disabilitazione di HTTP non elaborato può essere eseguita passando il valore `false` o `no` all'indicatore `--web`.

```
 wsk update create /guest/demo/hello hello.js --web false
```

### Decodifica del contenuto del corpo binario da Base64

Quando viene elaborato il contenuto HTTP non elaborato, il contenuto di `__ow_body` viene codificato in Base64 quando il `Content-Type` della richiesta è binario.
Le seguenti funzioni mostrano come decodificare il contenuto del corpo in Node, Python e Swift. Salva semplicemente un metodo in un file, crea un'azione web HTTP non elaborato che utilizza la risorsa salvata e richiama l'azione web.

#### Node

```javascript
function main(args) {
    decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
    return {body: decoded}
}
```
{: codeblock}

#### Python

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
 wsk action create decode decode.js --web raw
```
{: pre}

```
ok: created action decode
```

```
curl -k -H "content-type: application" -X POST -d "Decoded body" https:// openwhisk.ng.bluemix.net/api/v1/web/guest/default/decodeNode.json
```
{: pre}

```json
{
  "body": "Decoded body"
}
```

## Richieste Options
{: #options-requests}

Per impostazione predefinita, una richiesta OPTIONS eseguita su un'azione web genera intestazioni CORS che vengono automaticamente aggiunte alle intestazioni di risposta. Queste intestazioni consentono tutte le origini e tutti i verbi HTTP: options, get, delete, post, put, head e patch.

Vedi le seguenti intestazioni:

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

In alternativa, le richieste OPTIONS possono essere gestite manualmente da un'azione web. Per abilitare questa opzione, aggiungi
un'annotazione `web-custom-options` con un valore `true` all'azione web. Se questa funzione è abilitata, le intestazioni CORS non vengono aggiunte automaticamente alla risposta della richiesta. È invece responsabilità dello sviluppatore aggiungere le intestazioni desiderate a livello di programmazione. Vedi il seguente esempio per creare risposte personalizzate alla richieste OPTIONS.

```
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

Salva la funzione in `custom-options.js` ed esegui questi comandi:

```
$ wsk action create custom-option custom-options.js --web true -a web-custom-options true
$ curl https://${APIHOST}/api/v1/web/guest/default/custom-options.http -kvX OPTIONS
< HTTP/1.1 200 OK
< Server: nginx/1.11.13
< Content-Length: 0
< Connection: keep-alive
< Access-Control-Allow-Methods: OPTIONS, GET
< Access-Control-Allow-Origin: example.com
```

## Gestione degli errori
{: #openwhisk_webactions_errors}

Un'azione OpenWhisk non riesce in due diverse possibili modalità di errore. La prima è nota come _application error_ ed è analoga a un'eccezione rilevata: l'azione restituisce un oggetto JSON che contiene una proprietà `error` di livello superiore. La seconda è un _developer error_, che si verifica quando l'azione non riesce in modo irreversibile e non produce una risposta (simile a un'eccezione non rilevata). Per le azioni web, il controller gestisce gli errori applicazione nel seguente modo:

- Qualsiasi proiezione di percorso specificata viene ignorata e il controller proietta invece la proprietà  `error`.
- Il controller applica la gestione dei contenuti prevista dall'estensione dell'azione al valore della proprietà `error`.

Gli sviluppatori devono sapere come possono essere utilizzate le azioni web e generare risposte di errore appropriate. Ad esempio, un'azione web utilizzata con l'estensione `.http` restituisce una risposta HTTP come `{error: { statusCode: 400 }`. In caso contrario, si verifica una mancata corrispondenza tra il `Content-Type` previsto dall'estensione e il `Content-Type` dell'azione nella risposta di errore. È necessario prestare particolare attenzione alle azioni web che sono sequenze, in modo che i componenti che costituiscono una sequenza possano generare errori adeguati laddove necessario. 

