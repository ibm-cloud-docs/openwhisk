---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-10"

keywords: actions, serverless, javascript, node, node.js, functions

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
{:external: target="_blank" .external}


# Preparazione di applicazioni per le azioni
{: #prep}

Indipendentemente dal fatto che porti un'applicazione o scrivi uno script specificamente per rispondere a un evento, il tuo codice deve soddisfare determinati requisiti affinché tu possa utilizzarlo per creare un'azione.
{: shortdesc}

L'esecuzione di ciascun linguaggio di programmazione presenta dei requisiti specifici, ma la maggior parte dei linguaggi presenta i seguenti requisiti generali:
- Per impostazione predefinita, il nome previsto del punto di ingresso nel codice è `main`. Se il punto di ingresso non è `main`, alla creazione dell'azione può essere specificato un nome personalizzato; in tal caso, prendi nota del nome.
- I parametri di input nella tua applicazione e i risultati di output da essa prodotti devono essere formattati secondo una struttura specifica che può essere trasmessa tra le entità. La struttura dipende dal linguaggio del tuo codice. Ad esempio, con le applicazioni Python, i parametri di input devono essere un dizionario e il risultato dell'applicazione deve essere strutturato come un dizionario. Puoi passare i parametri alla tua azione anche in un oggetto strutturato. Ad esempio, in JSON, potresti strutturare il tuo codice in modo che preveda un parametro di input con valori JSON da determinati campi, come `name` e `place`.

    **Esempio di input JSON**
    ```json
    {"name": "Dorothy", "place": "Kansas"}
    ```
    {: codeblock}

    **Esempio JavaScript**
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
    ```
    {: codeblock}
- Se la tua applicazione contiene più file, devono essere combinati in un unico file da utilizzare in un'azione. Puoi riscrivere il tuo codice in un unico file o inserire file e dipendenze in un pacchetto all'interno di un unico archivio di file. Se il tuo runtime non è supportato, puoi inserire la tua applicazione in un pacchetto sotto forma di immagine Docker.
- Anche le dipendenze devono essere inserite nel pacchetto con l'applicazione. I runtime disponibili vengono forniti con pacchetti ed estensioni preinstallati. [Controlla le informazioni di riferimento per il tuo runtime](/docs/openwhisk?topic=cloud-functions-runtimes) per vedere se una dipendenza della tua applicazione è già inclusa nel runtime. Se la dipendenza è inclusa, non è necessario che tu la inserisca nel pacchetto insieme alla tua applicazione.

    La compilazione del codice non è obbligatoria; tuttavia, se possibile per il tuo runtime, la compilazione anticipata del codice può migliorare le prestazioni.
    {: tip}

## Preparazione delle applicazioni nelle immagini Docker
{: #prep_docker}

Con {{site.data.keyword.openwhisk_short}}, puoi scrivere la tua applicazione in qualsiasi lingua e inserirla in un pacchetto come immagine Docker.
{: shortdesc}

Puoi utilizzare solo immagini di registri pubblici, come ad esempio un'immagine pubblicamente disponibile su Docker Hub. I registri privati non sono supportati.
{: important}

### Inserimento del codice in un pacchetto in immagini Docker
{: #prep_docker_pkg}

Il tuo codice viene compilato in un eseguibile e incorporato in un'immagine Docker. L'eseguibile interagisce con il sistema prendendo l'input da `stdin` e rispondendo tramite `stdout`.
{: shortdesc}

**Prima di iniziare**
- Devi disporre di un account Docker Hub.   [Docker Hub](https://hub.docker.com){: external}.
- [Installa Docker](https://hub.docker.com/search/?offering=community&type=edition){:external}.
- [Rivedi i requisiti per il runtime Docker](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker).

Per inserire la tua applicazione in un pacchetto, completa la seguente procedura.

Per inserire il tuo codice in un pacchetto come immagine Docker, immetti il seguente comando.
1. Scarica e installa la struttura di base Docker. La struttura di base è un template del contenitore Docker in cui puoi inserire il tuo codice sotto forma di file binari personalizzati.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Configura il tuo codice nella struttura di base black box. La struttura di base include un programma C che puoi utilizzare. Parte del file `example.c` viene compilata nell'ambito del processo di creazione dell'immagine Docker, quindi non hai bisogno di avere C compilato sulla tua macchina.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  **Output di esempio**
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. (Facoltativo) Aggiungi più codice e dipendenze all'immagine Docker modificando il `Dockerfile` per creare il tuo eseguibile. Considera i seguenti requisiti:
  * Il tuo codice deve trovarsi all'interno del contenitore in `/action/exec`.
  * L'eseguibile riceve un singolo argomento dalla riga di comando. Questo argomento è una serializzazione di stringa dell'oggetto JSON che rappresenta gli argomenti per l'azione.
  * Il programma può accedere a `stdout` o `stderr`.
  * Per convenzione, l'ultima riga dell'output deve essere un oggetto JSON <ph class="ignoreSpelling">in stringhe</ph>, che rappresenta il risultato dell'azione.
  Per ulteriori informazioni sulla creazione di Dockerfile, consultare la [Guida di riferimento per Dockerfile](https://docs.docker.com/engine/reference/builder/){: external}.

4. Crea l'immagine Docker e caricala attraverso uno script fornito.
    1. Esegui l'accesso a Docker.
        ```
        docker login -u <nomeutente> -p <password>
        ```
        {: pre}

    2. Passa alla directory `dockerSkeleton`.
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. Esegui lo script.
        ```
        ./buildAndPush.sh <nomeutente>/blackboxdemo
        ```
        {: pre}




## Preparazione di applicazioni JavaScript
{: #prep_js}

Prima di creare un'azione, prepara il tuo codice JavaScript. Verifica che il tuo codice sia strutturato correttamente, quindi decidi se deve essere impacchettato.
{: shortdesc}

### Strutturazione del codice JavaScript
{: #prep_js_struct}

- Il nome previsto per la funzione del punto di ingresso è `main`. Se la funzione contenuta nel tuo codice non è `main`, prendi nota del nome per specificarlo alla creazione dell'azione.
- I parametri di input vengono trasmessi come oggetto JSON.
- Il risultato di un'attivazione eseguita correttamente è anch'esso un oggetto JSON, che viene tuttavia restituito in modo differente a seconda che l'azione sia [sincrona](#prep_js_sync) o [asincrona](#prep_js_async).



**Esempio**
```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```

**Esempio con più funzioni**

  ```javascript
  function main() {
      return { payload: helper() }
  }

  function helper() {
      return new Date();
}
  ```
  {: codeblock}


### Strutturazione del codice JavaScript con comportamento sincrono
{: #prep_js_sync}

L'attivazione JavaScript è sincrona quando la funzione principale termina senza eseguire un'istruzione `return` o eseguendo un'istruzione `return` che restituisce qualsiasi valore eccetto una promessa.
{: shortdesc}

**Esempio di codice sincrono.**

```javascript
// each path results in a synchronous activation
function main(params) {
  if (params.payload == 0) {
     return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
     return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}




### Strutturazione del codice JavaScript con un comportamento asincrono
{: #prep_js_async}

Le funzioni JavaScript possono proseguire la propria esecuzione in una funzione di callback anche dopo una restituzione. L'attivazione JavaScript è asincrona se la funzione principale termina restituendo una promessa. In questo caso, il sistema presuppone che l'azione sia ancora in esecuzione fino a quando la promessa non viene soddisfatta o rifiutata. Le funzioni JavaScript eseguite in modo asincrono possono restituire il risultato dell'attivazione dopo il ritorno della funzione `main` restituendo una promessa nella tua azione.
{: shortdesc}

Inizia creando un'istanza di un nuovo oggetto promessa e trasmettendo una funzione di callback. Il callback utilizza due argomenti, resolve e reject, che sono entrambi funzioni. Tutto il codice asincrono va all'interno di tale callback. L'handler azioni può avere qualsiasi nome purché sia conforme alla classica firma di accettazione di un oggetto e di restituzione di un oggetto (o una `Promise` di un oggetto).

Nel seguente esempio, puoi vedere come soddisfare una promossa richiamando la funzione resolve.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         resolve({ done: true });
           }, 2000);
        })
}
```
{: codeblock}

Questo esempio mostra come rifiutare una promessa richiamando la funzione reject.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 2000);
     })
}
```
{: codeblock}

Negli esempi, vengono eseguiti i seguenti dettagli.
* La funzione `main` restituisce una promessa. La promessa indica che l'attivazione non è stata ancora completata, ma il suo completamento è previsto in futuro.
* La funzione JavaScript `setTimeout()` attende 2 secondi prima di chiamare la funzione di callback della promessa, che rappresenta il codice asincrono.
* Il callback della promessa accetta gli argomenti `resolve` e `reject`, che sono entrambi funzioni.
  * La chiamata a `resolve()` soddisfa la promessa e indica che l'attivazione è stata completata normalmente.
  * Una chiamata a `reject()` può essere utilizzata per rifiutare la promessa e segnalare che l'attivazione è stata completata in modo anomalo.


### Strutturazione del codice JavaScript con un comportamento sincrono e asincrono
{: #prep_js_both}

Un'azione può essere sincrona per alcuni input e asincrona per altri, come mostrato nel seguente esempio.
{: shortdesc}

```javascript
function main(params) {
     if (params.payload) {
        // asynchronous activation
         return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
           }, 2000);
        })
     }  else {
        // synchronous activation
        return {done: true};
     }
}
```
{: codeblock}





### Esempio di chiamata di un'API esterna con JavaScript
{: #prep_js_api}

Il seguente esempio richiama l'API esterna per il servizio della NASA APOD (Astronomy Picture of the Day), che fornisce ogni giorno un'immagine univoca del nostro universo.
{: shortdesc}


```javascript
let rp = require('request-promise')

function main(params) {
    const options = {
        uri: "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo",
        json: true
    }
    return rp(options)
    .then(res => {
        return { response: res }
    })
}
```
{: codeblock}

Viene effettuata una chiamata all'API NASA APOD e i campi vengono estratti dal risultato JSON.

Successivamente, [crea](/docs/openwhisk?topic=cloud-functions-actions) e [richiama l'azione](/docs/openwhisk?topic=cloud-functions-test) per testarla. Viene restituito il seguente oggetto di esempio:

```
{
  "copyright": "Eric Houck",
    "date": "2018-03-28",
    "explanation": "Does an alignment like this occur only once in a blue moon? ...",
    "hdurl": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_1799.jpg",
    "media_type": "image",
    "service_version": "v1",
    "title": "Blue Moon Tree",
    "url": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_960.jpg"
  }
```
{: screen}






### Inserimento del codice JavaScript in un pacchetto con il modulo `webpack`
{: #prep_js_pkg}

Puoi inserire un'applicazione in un pacchetto utilizzando un bundler di moduli JavaScript come `[webpack ](https://webpack.js.org/concepts/){: external}`. Quando `webpack` elabora il tuo codice, crea in modo ricorsivo un grafico delle dipendenze che include ogni modulo di cui la tua azione ha bisogno.
{: shortdesc}

Prima di iniziare, [rivedi i pacchetti contenuti nel runtime JavaScript](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments) per vedere se una dipendenza della tua applicazione è già inclusa nel runtime. Se la dipendenza non è inclusa, devi inserirla nel pacchetto insieme alla tua applicazione.

1. Crea un file `package.json`. Aggiungi `webpack` come dipendenza di sviluppo.

    ```json
    {
      "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
        "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10"
      },
  "dependencies": {
        "left-pad": "1.1.3"
      },
  "devDependencies": {
        "webpack": "^3.8.1"
      }
    }
    ```
    {: codeblock}

2. Salva il seguente codice di configurazione `webpack` in un file denominato `webpack.config.js`.

    ```javascript
    var path = require('path');
module.exports = {
      entry: './index.js',
  output: {
        path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
      },
  target: 'node'
    };
    ```
    {: codeblock}

3. Prepara il tuo codice applicazione. In questo esempio, che puoi salvare come file denominato `index.js`, la variabile `global.main` è impostata come funzione principale dell'applicazione.

    **Esempio**
    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    global.main = myAction;
    ```
    {: codeblock}

4. Installa tutte le dipendenze localmente.

    ```
    npm install
    ```
    {: pre}

5. Crea il bundle `webpack`.

    ```
    npm run build
    ```
    {: pre}

    Il file `dist/bundle.js` viene creato e viene distribuito come codice sorgente dell'azione.

6. Crea l'azione utilizzando lo script `npm` o la CLI.

    * Esegui il seguente script `npm`.

        ```
        npm run deploy
        ```
        {: pre}

    * Immetti il seguente comando della CLI.

        ```
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    Il file bundle creato da `webpack` supporta solo le dipendenze JavaScript. Se il bundle ha altre dipendenze, le chiamate di azioni potrebbero non riuscire perché queste dipendenze non sono incluse nel file `bundle.js`.
    {: tip}



### Inserimento del codice JavaScript in un pacchetto come file NPM
{: #prep_js_npm}

Come alternativa alla scrittura di tutto il tuo codice azione in un unico file di origine JavaScript, puoi inserire il tuo codice in un pacchetto `npm` all'interno di un file .zip.
{: shortdesc}

Prima di iniziare, [rivedi i pacchetti contenuti nel runtime JavaScript](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments) per vedere se una dipendenza della tua applicazione è già inclusa nel runtime. Se la dipendenza non è inclusa, devi inserirla nel pacchetto insieme alla tua applicazione.

1. Nella directory root, crea un file `package.json`. 

**Esempio**

    ```json
    {
      "name": "my-action",
  "main": "index.js",
  "dependencies" : {
        "left-pad" : "1.1.3"
      }
    }
    ```
    {: codeblock}

2. Installa tutte le dipendenze localmente.

    ```
    npm install DEPENDENCY
    ```
    {: pre}

    Mentre la maggior parte dei pacchetti `npm` installa le origini JavaScript su `npm install`, altri installano e compilano anche risorse di file binari. Il caricamento dei file di archivio supporta solo le dipendenze JavaScript. Se l'archivio include dipendenze di file binari, le chiamate di azioni potrebbero non riuscire.
    {: note}

3. Crea un archivio `.zip` che contiene tutti i file, incluse tutte le dipendenze.

    ```
    zip -r action.zip *
    ```
    {: pre}

    **Utenti Windows** L'utilizzo dell'azione Esplora risorse per la creazione del file .zip produce una struttura di file errata. Le azioni del file .zip {{site.data.keyword.openwhisk_short}} devono avere `package.json` al livello root dell'archivio, mentre Esplora risorse lo inserisce in una cartella nidificata. Utilizza invece il comando `zip`.
    {: tip}





## Preparazione delle applicazioni Go
{: #prep_go}

Utilizza un singolo file per scopi di sviluppo o test rapidi. Per le applicazioni di produzione, precompila le tue azioni Go in un eseguibile per ottenere prestazioni migliori o supporto da più file di origine, incluse le librerie dei fornitori.
{: shortdesc}

Anche se puoi creare un file compresso su qualsiasi piattaforma Go eseguendo la compilazione incrociata con `GOOS=Linux` e `GOARCH=amd64`, utilizza la funzione di pre-compilazione incorporata nell'immagine del contenitore di runtime. Puoi assemblare [più file di origine](#prep_go_multi) o [librerie del fornitore](#prep_go_vendor).
{: tip}


### Strutturazione del codice Go
{: #prep_go_struct}

- Il nome previsto per il pacchetto del punto di ingresso è `main`. Se il pacchetto contenuto nel tuo codice non è `main`, prendi nota del nome per specificarlo alla creazione dell'azione.
- Il pacchetto deve essere pubblico.

**Esempio**
```go
    package main

    import "fmt"

    // Main is the function implementing the action
    func Main(params map[string]interface{}) map[string]interface{} {
        // parse the input JSON
        name, ok := params["name"].(string)
        if !ok {
            name = "World"
        }
        msg := make(map[string]interface{})
        msg["body"] = "Hello " + name + "!"
        // can optionally log to stdout (or stderr)
        fmt.Println("hello Go action")
        // return the output JSON
        return msg
    }
  ```
  {: codeblock}

### Inserimento di più file di origine Go in un pacchetto
{: #prep_go_multi}

1. Crea una directory `src` di livello superiore. Inserisci i file di origine che appartengono al pacchetto main nella root di `src` o in una directory `main` e crea le sottodirectory per gli altri pacchetti. Ad esempio, il pacchetto `hello` diventa la directory `src/hello`.
  ```
  go-action-hello/
  └── src
      ├── hello
      │   └── hello.go
      └── main
          └── main.go
  ```
  {: screen}

2. Importa sottopacchetti. Esempio di `main/main.go` che importa il sottopacchetto hello.

  ```go
  package main

  import (
  	"fmt"
  	"hello"
  )

  // Main forwading to Hello
func Main(args map[string]interface{}) map[string]interface{} {
  	fmt.Println("This is main.Main")
	greetings := "World"
	name, ok := args["name"].(string)
	if ok {
  		greetings = name
  	}
  	return hello.Hello(greetings)
  }
  ```
  {: codeblock}

  Esempio di `hello/hello.go`.

  ```go
  package hello

  import "fmt"

  // Hello return a greeting message
func Hello(name string) map[string]interface{} {
  	fmt.Println("This is hello.Hello")
	res := make(map[string]interface{})
	res["body"] = "Hello " + name
	return res
}
  ```
  {: codeblock}

3. Compila il codice. Crea un archivio .zip della directory `src`. Non includere la directory del progetto di livello superiore `go-action-project/`.

  ```bash
  cd src
zip -r ../hello-src.zip *
cd ..
  ```
  {: pre}

  Puoi eseguire la compilazione in locale impostando il tuo `GOPATH` sull'elemento sovraordinato della directory `src`. Se utilizzi il codice VS, devi modificare l'impostazione `go.inferGopath` su `true`.
  {: note}

4. Compila e comprimi l'eseguibile Go come `exec` nella root dell'archivio .zip. Crea l'archivio `hello-bin.zip` immettendo il seguente comando. Devi aver installato la CLI Docker nella tua workstation e `docker` nel tuo `PATH`.

  ```bash
  docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
  ```
  {: pre}

  In questo esempio, la funzione main è `-compile main`. Per utilizzare una diversa funzione main, modifica il valore per `-compile`. La funzione main viene selezionata al momento della compilazione. Quando esegui la pre-compilazione, `ibmcloud fn action [update | create]` ignora `--main`.

  Il contenitore ottiene il contenuto del file .zip di origine in `stdin`, lo compila e crea un nuovo archivio .zip con l'eseguibile `exec` nella root. Il contenuto dell'archivio .zip viene trasmesso a `stdout` che viene reindirizzato all'archivio `hello-bin.zip` per essere distribuito come l'azione Go.




### Inserimento del codice Go in un pacchetto con le librerie del fornitore
{: #prep_go_vendor}

Puoi includere delle dipendenze popolando una directory `vendor` nell'archivio `zip` di origine durante la compilazione dei file Go. La directory `vendor` non funziona al livello superiore. Devi posizionare la directory `vendor` in `src/` e in una directory del pacchetto.
{: shortdesc}

Esempio di pacchetto di log `logrus` in un'applicazione `hello.go`.

```go
package hello

import (
	"os"

	"github.com/Sirupsen/logrus"
)

var log = logrus.New()

func init() {
	log.Out = os.Stdout
}

// Hello return a greeting message
func Hello(name string) map[string]interface{} {
	log.WithFields(logrus.Fields{"greetings": name}).Info("Hello")
	res := make(map[string]interface{})
	res["body"] = "Hello, " + name
	return res
}
```
{: codeblock}

</br>
In questo esempio, la directory `vendor` è ubicata in `src/hello/vendor`. Puoi aggiungere delle librerie di terze parti che vengono utilizzate dal pacchetto `hello`. 

Puoi utilizzare più strumenti come [<code>dep</code> ](https://golang.github.io/dep/docs/installation.html){: external} per popolare e gestire le dipendenze.

Puoi utilizzare `dep` creando un file `src/main/Gopkg.toml` descrivendo la versione e l'ubicazione delle librerie.

```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

Popola la directory `vendor` ed esegui `dep ensure`.




## Preparazione delle applicazioni Swift
{: #prep_swift}

I file Swift devono essere compilati prima di eseguire un'azione. Questo ritardo è noto come ritardo di avvio a freddo. Per evitare il ritardo di avvio a freddo, puoi compilare il tuo file Swift e quindi caricarlo su {{site.data.keyword.openwhisk_short}} in un file .zip. Il runtime Docker include un compilatore per aiutare gli utenti a compilare ed impacchettare le azioni Swift 4.2. Le chiamate successive all'azione sono molto più veloci fino a quando il contenitore con l'azione non viene eliminato.

Le azioni Swift vengono eseguite in un ambiente Linux. Swift su Linux è ancora in fase di sviluppo e {{site.data.keyword.openwhisk_short}} utilizza l'ultima release disponibile. Tali release potrebbero non essere stabili. La versione di Swift utilizzata con {{site.data.keyword.openwhisk_short}} potrebbe essere incongruente con le versioni di Swift dalle release stabili di Xcode su macOS.
{: important}


### Strutturazione del codice Swift
{: #prep_swift_struc}

Il nome previsto per la funzione del punto di ingresso è `main`. Se la funzione contenuta nel tuo codice non è `main`, prendi nota del nome per specificarlo alla creazione dell'azione.

Oltre alla firma della funzione principale, Swift 4 fornisce altre due firme che si avvalgono del tipo [<code>Codable</code> ](https://developer.apple.com/documentation/swift/codable){: external}. Puoi ottenere ulteriori informazioni sui tipi di dati che sono [codificabili e decodificabili per la compatibilità con rappresentazioni esterne quali JSON](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types){: external}.

**Esempio**
```swift
struct Input: Codable {
    let name: String?
}
struct Output: Codable {
    let greeting: String
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    let result = Output(greeting: "Hello \(param.name ?? "stranger")!")
    print("Log greeting:\(result.greeting)")
    completion(result, nil)
}
```
{: codeblock}

Questo esempio utilizza un parametro di input come `Codable Input` con il campo `name` e restituisce un `Codable output` con un campo `greetings`.


#### Gestione degli errori in Swift
{: #prep_swift_error}

Utilizzando il gestore di completamento `Codable`, puoi passare un errore per indicare un malfunzionamento nella tua azione. La [gestione degli errori in Swift](https://docs.swift.org/swift-book/LanguageGuide/ErrorHandling.html){: external} è simile alla gestione delle eccezioni in altri linguaggi, con l'utilizzo delle parole chiave `try`, `catch` e `throw`.
{: shortdesc}

Il seguente frammento di codice mostra un esempio di gestione di un errore.

```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    // Return real error
    do{
        throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
    } catch {
        completion(nil, error)
    }
}
```
{: codeblock}


### Inserimento di un file Swift 4.2 in un pacchetto
{: #prep_swift42_single}

Compila un singolo file di origine che non dipende da librerie esterne. Utilizza l'indicatore `-compile` con il nome del metodo principale.

**Prima di iniziare**
- [Installa Docker](https://hub.docker.com/search/?offering=community&type=edition){: external}.
- [Rivedi i pacchetti contenuti nel runtime Swift](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions) per vedere se una dipendenza della tua applicazione è già inclusa nel runtime. Se la dipendenza non è inclusa, devi inserirla nel pacchetto insieme alla tua applicazione.

Inserisci la tua applicazione in un pacchetto.

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

Il contenitore Docker legge il contenuto del file da `stdin` e scrive un archivio .zip con l'eseguibile swift compilato in `stdout`.



### Inserimento di progetti e dipendenze con più file per Swift 4.2
{: #prep_swift42_multi}

**Prima di iniziare**
- [Installa Docker](https://hub.docker.com/search/?offering=community&type=edition){: external}.
- [Rivedi i pacchetti contenuti nel runtime Swift](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions) per vedere se una dipendenza della tua applicazione è già inclusa nel runtime. Se la dipendenza non è inclusa, devi inserirla nel pacchetto insieme alla tua applicazione.

Inserisci la tua applicazione in un pacchetto.

1. Per compilare più file e includere delle dipendenze esterne, crea la seguente struttura di directory.

  ```
  .
  ├── Package.swift
  └── Sources
      └── main.swift
  ```
  {: codeblock}

  La directory `Sources/` contiene un file denominato `main.swift`.

  `Package.swift` deve iniziare con un commento che specifica la versione `4.2` per gli strumenti Swift:

  ```swift
  // swift-tools-version:4.2
import PackageDescription

  let package = Package(
      name: "Action",
      products: [
      .executable(
          name: "Action",
          targets:  ["Action"]
        )
      ],
      dependencies: [
      .package(url: "https://github.com/IBM-Swift/SwiftyRequest.git", .upToNextMajor(from: "1.0.0"))
      ],
      targets: [
      .target(
        name: "Action",
        dependencies: ["SwiftyRequest"],
        path: "."
    )
      ]
  )
  ```
  {: codeblock}

2. Crea un archivio .zip con il contenuto della directory.

  ```bash
  zip ../action-src.zip -r *
  ```
  {: codeblock}

3. Passa l'archivio .zip al contenitore Docker su `stdin`. `stdout` è un nuovo archivio .zip con l'eseguibile compilato. Il contenitore Docker legge il contenuto dell'archivio .zip da `stdin` e scrive un nuovo archivio .zip con l'eseguibile Swift compilato in `stdout`.

  ```
  docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
  ```
  {: codeblock}

  In un sistema basato su Linux, puoi combinare i passi `zip` e `docker run` in un solo comando:

  ```
  zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
  ```
  {: codeblock}





## Inserimento di applicazioni Python in un pacchetto
{: #prep_python}


### Strutturazione del codice Python
{: #prep_python_struct}

- Le applicazioni Python devono utilizzare un dizionario e produrne un altro.
- Il nome previsto per il metodo del punto di ingresso è `main`. Se la funzione contenuta nel tuo codice non è `main`, prendi nota del nome per specificarlo alla creazione dell'azione.
{: shortdesc}

**Esempio**
```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
	print(greeting)
    return {"greeting": greeting}
```

### Inserimento di codice Python in un pacchetto
{: #prep_python_pkg}

Inserisci in un pacchetto il codice Python e i moduli dipendenti, all'interno di un file .zip. In questo esempio, il file di origine che contiene il punto di ingresso è `__main__.py` e i moduli helper si trovano in un file denominato `helper.py`.

Prima di iniziare, [rivedi i pacchetti contenuti nel runtime Python](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) per vedere se una dipendenza della tua applicazione è già inclusa nel runtime. Se la dipendenza non è inclusa, devi inserirla nel pacchetto insieme alla tua applicazione.

Per inserire la tua applicazione in un pacchetto, immetti il seguente comando.

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}


### Inserimento del codice Python con un ambiente virtuale in un pacchetto, all'interno di file .zip
{: #prep_python_virtenv}

Puoi assemblare le dipendenze Python utilizzando un ambiente virtuale, `virtualenv`. Utilizzando l'ambiente virtuale, puoi collegare più pacchetti che possono essere installati utilizzando [`pip` ](https://packaging.python.org/tutorials/installing-packages/){: external}.

Prima di iniziare, [rivedi i pacchetti contenuti nel runtime Python](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) per vedere se una dipendenza della tua applicazione è già inclusa nel runtime. Se la dipendenza non è inclusa, devi inserirla nel pacchetto insieme alla tua applicazione.

Inserisci la tua applicazione in un pacchetto completando la seguente procedura.

1. Crea un file [requirements.txt ](https://pip.pypa.io/en/latest/user_guide/#requirements-files){: external} che contiene i moduli `pip` e le versioni da installare.

  Per mantenere il `virtualenv` su una dimensione minima, aggiungi al file `requirements.txt` solo i moduli che non appartengono all'ambiente di runtime selezionato. Per ulteriori informazioni sui pacchetti inclusi nei runtime Python, vedi la [guida di riferimento al runtime](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) di Python.
  {: tip}

2. Esegui il pull di una delle seguenti immagini per il tuo runtime. Per garantire la compatibilità con il contenitore di runtime, all'interno di un ambiente virtuale, le installazioni contenute in pacchetti devono utilizzare l'immagine che corrisponde al runtime specificato.
    * Per `python:3.7`, utilizza l'immagine Docker `ibmfunctions/action-python-v3.7`.
    * Per `python:3.6`, utilizza l'immagine Docker `ibmfunctions/action-python-v3.6`.
    * Per `python:2`, utilizza l'immagine Docker `openwhisk/python2action`.

   **Esempio**
   ```
   docker pull ibmfunctions/action-python-v3.7
   ```
   {: pre}

2. Installa le dipendenze e crea un ambiente virtuale. La directory dell'ambiente virtuale deve essere denominata `virtualenv`.

   ```
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. Assembla la directory `virtualenv` e gli eventuali file Python aggiuntivi. Il file di origine che contiene il punto di ingresso deve essere denominato `__main__.py`.

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}


## Preparazione delle applicazioni Ruby
{: #prep_ruby}

Prima di creare un'azione, prepara il tuo codice Ruby.

### Strutturazione del codice Ruby
{: #prep_ruby_struct}

* Le azioni Ruby utilizzano sempre un hash (una raccolta simile a un dizionario) e restituiscono un hash.
* Il nome previsto per la funzione del punto di ingresso è `main`. Se la funzione contenuta nel tuo codice non è `main`, prendi nota del nome per specificarlo alla creazione dell'azione.


**Esempio**

```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
```
{: codeblock}

### Inserimento di codice Ruby in un pacchetto
{: #prep_ruby_pkg}

Puoi inserire un'applicazione Ruby e i pacchetti dipendenti in un pacchetto, sotto forma di file .zip. Ad esempio, puoi assemblare un'azione con un secondo file denominato `helper.rb`.

Crea un archivio contenente i tuoi file di origine. Il file di origine che contiene il punto di ingresso deve essere denominato `main.rb`.

```bash
zip -r helloRuby.zip main.rb helper.rb
```
{: pre}

Sono disponibili i gem `mechanize` e `jwt` in aggiunta ai gem predefiniti e in bundle. Puoi utilizzare dei gem arbitrari finché utilizzi delle azioni compresse per assemblare tutte le dipendenze.



## Preparazione delle applicazioni PHP
{: #prep_php}

Prima di creare un'azione, prepara il tuo codice PHP.

### Strutturazione del codice PHP
{: #prep_php_struct}

- Le azioni PHP utilizzano sempre un array associativo e ne restituiscono un altro.
- Il nome previsto per la funzione del punto di ingresso è `main`. Se la funzione contenuta nel tuo codice non è `main`, prendi nota del nome per specificarlo alla creazione dell'azione.

**Esempio**
```php
<?php
function main(array $args) : array
{
    $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
?>
```
{: codeblock}

### Inserimento di codice PHP in un pacchetto
{: #prep_php_pkg}

Puoi inserire file PHP o pacchetti dipendenti in un pacchetto, sotto forma di file .zip.

Prima di iniziare, [rivedi i pacchetti contenuti nel runtime PHP](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php) per vedere se una dipendenza della tua applicazione è già inclusa nel runtime. Se la dipendenza non è inclusa, devi inserirla nel pacchetto insieme alla tua applicazione.

Per inserire la tua applicazione in un pacchetto, immetti il seguente comando.

```bash
zip -r ARCHIVE_NAME.zip FILE_1.php FILE_2.php
```
{: pre}

**Esempio**
```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

## Preparazione delle applicazioni Java
{: #prep_java}

Prima di creare un'azione, prepara il tuo codice Java.

### Strutturazione del codice Java
{: #prep_java_struct}

Un'azione Java è un programma Java con un metodo denominato `main`. `main` deve avere la seguente firma.

**Esempio**
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

* Devi specificare il nome della classe principale utilizzando `--main`. Una classe principale idonea è una classe che implementa un metodo `main` statico. Se la classe non si trova nel pacchetto predefinito, utilizza il nome classe Java completo, ad esempio, `--main com.example.MyMain`.
* Puoi personalizzare il nome del metodo della tua azione Java specificando il nome del metodo completo dell'azione, ad esempio `--main com.example.MyMain#methodName`.

### Inserimento di codice Java in un pacchetto
{: #prep_java_pkg}


**Prima di iniziare**
Devi avere [JDK 8](http://openjdk.java.net/install/){: external} installato localmente. In questo esempio, viene utilizzato [`google-gson-2.8.5.jar`](http://central.maven.org/maven2/com/google/code/gson/gson/2.8.5/){: external}.

Se utilizzi una versione JDK diversa da JDK 8, quando compili il tuo codice devi specificare `--release 8` con il comando `javac`.
{: note}

Per creare un'azione Java, completa la seguente procedura.

1. Salva il seguente codice in un file denominato `Hello.java`.

  ```java
  import com.google.gson.JsonObject;
public class Hello {
      public static JsonObject main(JsonObject args) {
          String name = "stranger";
          if (args.has("name"))
              name = args.getAsJsonPrimitive("name").getAsString();
          JsonObject response = new JsonObject();
          response.addProperty("greeting", "Hello, " + name + "!");
        return response;
    }
  }
  ```
  {: codeblock}

2. Scarica [`gson-2.8.5.jar`](http://central.maven.org/maven2/com/google/code/gson/gson/2.8.5/).

3. Aggiungi `gson-2.8.5.jar` al tuo `ClASSPATH`. Questo esempio utilizza `gson-2.8.5.jar`, che viene salvato in una cartella `test` nella directory `Desktop`.
  
  ```
  export CLASSPATH=$CLASSPATH:/Users/Desktop/test/gson-2.8.5.jar
  ```
  {: pre}

4. Aggiungi la cartella `bin` del tuo JDK al `CLASSPATH`. Questo esempio utilizza `openjdk-8`.
  
  ```
  export CLASSPATH=$CLASSPATH:/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/bin
  ```
  {: pre}

5. Verifica che la cartella `bin` JDK e il `gson-2.8.5.jar` siano presenti nel tuo `CLASSPATH`.
  ```
  echo $CLASSPATH
  ```
  {: pre}

  **Output di esempio**
  ```
  /Desktop/test/gson-2.8.5.jar:/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/bin
  ```
  {: screen}

6. Passa alla cartella in cui è archiviato il tuo file `Hello.java`. In questo esempio, il file `Hello.java` è salvato nella cartella `Desktop/test`.
  ```
  cd Desktop/test
  ```
  {: pre}

7. Compila il tuo file `Hello.java` in un file di classe.
  ```
  javac Hello.java
  ```
  {: pre}

8. Comprimi il file di classe in un file .jar denominato `hello.jar`.

  ```
  jar cvf hello.jar Hello.class
  ```
  {: pre}

**Passi successivi**
Puoi creare un'azione con il tuo file `hello.jar`. Poiché il file di classe che hai creato non utilizza il nome predefinito `main`, devi impostare l'indicatore `--main` su `Hello` quando crei la tua azione. L'indicatore `--main` deve corrispondere alla tua `class` Java. Per ulteriori informazioni, vedi [Creazione di azioni](/docs/openwhisk?topic=cloud-functions-actions).
 
Quando aggiorni il tuo codice Java, devi ripetere questi passi per ricompilare il codice in un nuovo file `.jar`.
{: note}

### Inserimento di codice Java in un pacchetto con Gradle
{: #prep_java_gradle}

Invece di compilare dalla riga di comando, puoi utilizzare uno strumento di compilazione come [Gradle](https://gradle.org){: external} per recuperare le librerie da un repository come Maven Central. Puoi utilizzare Gradle per recuperare e creare un archivio .jar finale che include il tuo codice e tutte le dipendenze.

Questo è un esempio di utilizzo di Gradle per compilare un'azione Java che utilizza la libreria `com.google.zxing` che fornisce la funzionalità per generare un'immagine codice QR.

1. Crea un file denominato `build.gradle` e specifica le dipendenze.

  ```gradle
  apply plugin: 'java'

  version = '1.0'

  repositories {
     mavenCentral()
  }

  configurations {
      provided
      compile.extendsFrom provided
  }

  dependencies {
       provided 'com.google.code.gson:gson:2.6.2'
      compile 'com.google.zxing:core:3.3.0'
       compile 'com.google.zxing:javase:3.3.0'
  }

  jar {
      dependsOn configurations.runtime

     from {
          (configurations.runtime - configurations.provided).collect {
              it.isDirectory() ? it : zipTree(it)
          }
      }
  }
  ```
  {: codeblock}

2. Esegui il comando `gradle jar`, che genera un archivio .jar nella directory `build/libs/`.

  Per ulteriori informazioni, leggi la documentazione di Gradle [Declaring Dependencies](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies){: external}.


## Preparazione di applicazioni .NET Core
{: #prep_dotnet}

Prima di creare un'azione, prepara il tuo codice .NET Core.

### Strutturazione del codice .NET Core
{: #prep_dotnet_struct}

Un'azione .NET Core è una libreria di classi .NET Core con un metodo previsto denominato `Main`. Se il metodo contenuto nel tuo codice non è `Main`, prendi nota del nome per specificarlo alla creazione dell'azione in formato: `--main {Assembly}::{Class Full Name}::{Method}`

**Esempio**
```
Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main
```

### Inserimento di codice .NET Core in un pacchetto
{: #prep_dotnet_pkg}

**Prima di iniziare**
Per compilare, testare e archiviare progetti .NET Core, devi:
- Installare l'[SDK .NET Core](https://dotnet.microsoft.com/download){: external} in locale.
- Impostare la variabile di ambiente `DOTNET_HOME` sull'ubicazione in cui si trova l'eseguibile `dotnet`.



Per inserire il tuo codice in un pacchetto, immetti i seguenti comandi.

  1. Crea un progetto C# denominato `Apache.OpenWhisk.Example.Dotnet`.

      ```bash
      dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
      ```
      {: pre}

  2. Passa alla directory `Apache.OpenWhisk.Example.Dotnet`.

      ```bash
      cd Apache.OpenWhisk.Example.Dotnet
      ```
      {: pre}

  3. Installa il [pacchetto <ph class="ignoreSpelling">Newtonsoft.Json NuGet</ph>](https://www.nuget.org/packages/Newtonsoft.Json/){: external}.

      ```bash
      dotnet add package Newtonsoft.Json -v 12.0.1
      ```
      {: pre}

  4. Salva il seguente codice in un file denominato `Hello.cs`.

      ```csharp
      using System;
    using Newtonsoft.Json.Linq;

      namespace Apache.OpenWhisk.Example.Dotnet
    {
          public class Hello
        {
              public JObject Main(JObject args)
            {
                  string name = "stranger";
                if (args.ContainsKey("name")) {
                      name = args["name"].ToString();
                }
                JObject message = new JObject();
                message.Add("greeting", new JValue($"Hello, {name}!"));
                return (message);
            }
          }
      }
      ```
      {: codeblock}

  5. Compila `Hello.cs` e tutti gli altri file e output nella directory `out`.

      ```bash
      dotnet publish -c Release -o out
      ```
      {: pre}

  6. Passa alla directory `out`.

      ```bash
      cd out
      ```
      {: pre}

  7. Comprimi i file pubblicati.

      ```bash
      zip -r -0 ../helloDotNet.zip *
      ```
      {: pre}



