---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Crea e richiama le azioni
{: #openwhisk_actions}

Le azioni sono frammenti di codice senza stato eseguiti sulla piattaforma {{site.data.keyword.openwhisk}}. Ad esempio, un'azione può essere utilizzata per rilevare i volti in un'immagine, rispondere a una modifica del database, aggregare una serie di chiamate API o pubblicare un Tweet. Un'azione può essere scritta sotto forma di funzione JavaScript, Swift, Python, PHP, di metodo Java o di qualsiasi eseguibile compatibile a livello binario, inclusi programmi Go ed eseguibili personalizzati forniti come contenitori Docker.
{:shortdesc}

Le azioni possono essere richiamate esplicitamente o eseguite in risposta a un evento. In entrambi i casi, ogni esecuzione di un'azione produce un record di attivazione identificato da un ID di attivazione univoco. L'input di un'azione e il risultato di un'azione fungono da dizionario di coppie chiave-valore, in cui la chiave è una stringa e il valore è un valore JSON valido. Le azioni possono anche essere composte da chiamate ad altre azioni o da una sequenza definita di azioni.

Impara a creare, richiamare ed eseguire il debug di azioni nel tuo ambiente di sviluppo preferito:
* [JavaScript](#creating-and-invoking-javascript-actions)
* [Swift](#creating-swift-actions)
* [Python](#creating-python-actions)
* [Java](#creating-java-actions)
* [PHP](#creating-php-actions)
* [Docker](#creating-docker-actions)
* [Go](#creating-go-actions)
* [Eseguibili arbitrari](#creating-actions-arbitrary)

Inoltre, scopri:
* [Visualizzazione dell'output di un'azione](#watching-action-output)
* [Supporto per le applicazioni di grandi dimensioni](#large-app-support)
* [Elenco di azioni](#listing-actions)
* [Eliminazione di azioni](#deleting-actions)
* [Accesso ai metadati dell'azione nel corpo dell'azione](#accessing-action-metadata-within-the-action-body)


## Crea e richiama le azioni JavaScript
{: #creating-and-invoking-javascript-actions}

Le seguenti sezioni ti illustrano come utilizzare le azioni in JavaScript. Inizierai con la creazione e la chiamata di un'azione semplice. Procederai quindi all'aggiunta di parametri a un'azione e al richiamo di tale azione con i parametri. La fase successiva, prevede l'impostazione e il richiamo dei parametri predefiniti. Potrai quindi creare azioni asincrone e, infine, lavorare con le sequenze di azioni.


### Crea e richiama una semplice azione JavaScript
{: #openwhisk_single_action_js}

Consulta la procedura e gli esempi di seguito riportati per creare la tua prima azione JavaScript.

1. Crea un file JavaScript con il seguente contenuto. In questo esempio, il nome file è "hello.js".

  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  Il file JavaScript potrebbe contenere ulteriori funzioni. Tuttavia, per convenzione, è necessaria una funzione denominata `main` per fornire il punto di ingresso per l'azione.

2. Crea un'azione partendo dalla seguente funzione JavaScript. In questo esempio, l'azione è denominata "hello".

  ```
  wsk action create hello hello.js
  ```
  {: pre}

  ```
  ok: created action hello
  ```
  La CLI deduce automaticamente il tipo di azione utilizzando l'estensione del file di origine. Per i file di origine `.js`, l'azione viene eseguita utilizzando un runtime Node.js 6. Puoi anche creare un'azione che venga eseguita con Node.js 8 specificando esplicitamente il parametro `--kind nodejs:8`. Per ulteriori informazioni, vedi il [riferimento](./openwhisk_reference.html#openwhisk_ref_javascript_environments) per Node.js 6 e 8 .
  
3. Elenca le azioni che hai creato:

  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  hello       private
  ```

  Puoi vedere l'azione `hello` che hai creato.

4. Una volta creata la tua azione, puoi eseguirla nel cloud in OpenWhisk con il comando 'invoke'. Puoi richiamare le azioni con una chiamata *bloccante* (ossia, in stile richiesta/risposta) o una chiamata *non bloccante* specificando un indicatore nel comando. Una richiesta di chiamata bloccante _attende_ che il risultato di attivazione sia disponibile. Il periodo di attesa è inferiore a 60 secondi o al [valore del limite di tempo](./openwhisk_reference.html#openwhisk_syslimits) dell'azione. Il risultato dell'attivazione viene restituito se è disponibile entro il periodo di attesa. In caso contrario, l'attivazione continua l'elaborazione nel sistema e viene restituito un ID di attivazione in modo che sia possibile verificare il risultato in un secondo momento, come per le richieste non bloccanti (vedi [qui](#watching-action-output) per suggerimenti sul monitoraggio delle attivazioni).

  Questo esempio utilizza il parametro di blocco `--blocking`:

  ```
  wsk action invoke --blocking hello
  ```
  {: pre}

  ```
  ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b
  ```

  ```json
  {
      "result": {
          "payload": "Hello world"
      },
      "status": "success",
      "success": true
  }
  ```

  Il comando restituisce due informazioni importanti:
  * L'ID di attivazione (`44794bd6aab74415b4e42a308d880e5b`)
  * Il risultato della chiamata se è disponibile entro il periodo di attesa previsto

  Il risultato, in questo caso, è la stringa `Hello world` restituita dalla funzione JavaScript. L'ID di attivazione può essere utilizzato per richiamare i log o il risultato della chiamata in un momento successivo.  

5. Se non ti serve subito il risultato dell'azione, puoi omettere l'indicatore `--blocking` per effettuare una chiamata non bloccante. Puoi ottenere il risultato successivamente utilizzando l'ID di attivazione. Vedi il seguente esempio:

  ```
  wsk action invoke hello
  ```
  {: pre}

  ```
  ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
  ```

  ```
  wsk activation result 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: pre}

  ```json
  {
      "payload": "Hello world"
  }
  ```

6. Se dimentichi di registrare l'ID di attivazione, puoi ottenere un elenco di attivazioni ordinate dalla più recente alla più obsoleta. Immetti il seguente comando per ottenere un elenco delle tue attivazioni:

  ```
  wsk activation list
  ```
  {: pre}

  ```
  activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
  ```

### Passa i parametri a un'azione
{: #openwhisk_pass_params}

I parametri possono essere passati all'azione quando viene richiamata.

1. Utilizza i parametri nell'azione. Ad esempio, aggiorna il file "hello.js" con il seguente contenuto:

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  I parametri di input vengono passati alla funzione `main` come parametro dell'oggetto JSON. Osserva in che modo i parametri `name` e `place` vengono richiamati dall'oggetto `params` in questo esempio.

2. Aggiorna e richiama l'azione `hello`, passandole i valori dei parametri `name` e `place`. Vedi il seguente esempio:

  ```
  wsk action update hello hello.js
  ```
  {: pre}

  Se devi modificare i parametri delle tue credenziali non di servizio, tieni presente che eseguendo un comando `action update` con nuovi parametri si rimuovono tutti i parametri attualmente esistenti, ma non vengono specificati nel comando `action update`. Ad esempio, supponiamo che ci siano due parametri oltre a `__bx_creds`, con le chiavi denominate key1 e key2.  Se esegui un comando `action update` con `-p key1 new-value -p key2 new-value` ma ometti il parametro `__bx_creds`, il parametro `__bx_creds` non esisterà più dopo il completamento di `action update`. Dovrai eseguire nuovamente il bind delle credenziali del servizio. Questa è una limitazione nota senza soluzione temporanea.{: tip}  

3.  I parametri possono essere forniti esplicitamente sulla riga di comando o fornendo un file che contiene i parametri desiderati.

  Per passare i parametri direttamente tramite la riga di comando, fornisci una coppia chiave/valore all'indicatore `--param`:
  ```
  wsk action invoke --result hello --param name Bernie --param place Vermont
  ```
  {: pre}

  Per utilizzare un file che include il contenuto dei parametri, crea un file che contenga i parametri in formato JSON. Il nome file deve essere quindi passato all'indicatore `param-file`:

  Vedi il seguente file dei parametri di esempio denominato `parameters.json`:
  ```json
  {
      "name": "Bernie",
      "place": "Vermont"
  }
  ```

  ```
  wsk action invoke --result hello --param-file parameters.json
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

  Osserva l'uso dell'opzione `--result`: implica una chiamata bloccante in cui la CLI attende che l'attivazione sia completata e quindi visualizza solo il risultato. Per comodità, questa opzione può essere utilizzata senza `--blocking` che viene dedotto automaticamente.

  Inoltre, se i valori di parametro specificati sulla riga di comando sono JSON validi, vengono analizzati e inviati alla tua azione come oggetto strutturato. Ad esempio, aggiorna l'azione hello nel seguente modo:

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  Ora l'azione prevede che un singolo parametro `person` contenga i campi `name` e `place`. Quindi, richiama l'azione con un singolo parametro `person` che è un JSON valido, come nel seguente esempio:

  ```
  wsk action invoke --result hello -p person '{"name": "Bernie", "place": "Vermont"}'
  ```
  {: pre}

  Il risultato è lo stesso perché la CLI analizza automaticamente il valore del parametro `person` nell'oggetto strutturato ora previsto dall'azione:
  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

### Impostazione dei parametri predefiniti
{: #openwhisk_binding_actions}

Le azioni possono essere richiamate con più parametri indicati. Ricorda che l'azione `hello` dell'esempio precedente prevede due parametri: il *nome* di una persona e il *luogo* di provenienza.

Anziché passare ogni volta tutti i parametri a un'azione, puoi eseguire il bind di determinati parametri. Il seguente esempio esegue il bind del parametro *place* in modo che l'azione assuma come valore predefinito il luogo "Vermont":

1. Aggiorna l'azione utilizzando l'opzione `--param` per eseguire il bind dei valori di parametro o passando un file che contiene i parametri a `--param-file`

  Per specificare i parametri predefiniti esplicitamente nella riga di comando, fornisci una coppia chiave/valore all'indicatore `param`:

  ```
  wsk action update hello --param place Vermont
  ```
  {: pre}

  Il passaggio dei parametri da un file richiede la creazione di un file che contenga il contenuto desiderato in formato JSON. Il nome file deve essere quindi passato all'indicatore `-param-file`:

  Vedi il seguente file dei parametri di esempio denominato `parameters.json`:
  ```json
  {
      "place": "Vermont"
  }
  ```
  {: codeblock}

  ```
  wsk action update hello --param-file parameters.json
  ```
  {: pre}

2. Richiama l'azione, passando questa volta solo il parametro `name`.

  ```
  wsk action invoke --result hello --param name Bernie
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

  Nota che non hai dovuto specificare il parametro del luogo quando hai richiamato l'azione. I parametri associati possono ancora essere sovrascritti specificando il valore del parametro al momento della chiamata.

3. Richiama l'azione, passando il valore sia di `name` che di `place`. Quest'ultimo sovrascrive il valore associato mediante bind all'azione.

  Utilizzo dell'indicatore `--param`:

  ```
  wsk action invoke --result hello --param name Bernie --param place "Washington, DC"
  ```
  {: pre}

  Utilizzo dell'indicatore `--param-file`:

  File parameters.json:
  ```json
  {
    "name": "Bernie",
    "place": "Vermont"
  }
  ```
  {: codeblock}
  ```
  wsk action invoke --result hello --param-file parameters.json
  ```
  {: pre}

  ```json
  {  
      "payload": "Hello, Bernie from Washington, DC"
  }
  ```

### Ottieni un URL azione

Un'azione può essere richiamata attraverso l'interfaccia REST tramite una richiesta HTTPS. Per ottenere un URL azione, immetti il seguente comando:

```
wsk action get actionName --url
```
{: pre}

```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```

È necessario fornire l'autenticazione quando si richiama un'azione tramite una richiesta HTTPS. Per ulteriori informazioni relative
alle chiamate delle azioni mediante l'interfaccia REST, vedi [Utilizzo delle API REST con OpenWhisk](./openwhisk_rest_api.html#openwhisk_rest_api).
{: tip}

### Salva il codice azione

Il codice associato a un'azione esistente viene recuperato e salvato localmente. Il salvataggio viene eseguito su tutte le azioni ad eccezione delle sequenze e delle azioni docker. Quando si salva il codice azione in un file, il codice viene salvato nella directory di lavoro corrente e viene visualizzato il percorso file salvato.

1. Salva il codice azione in un nome file che corrisponde a un nome di azione esistente. Viene utilizzata un'estensione file che corrisponde al tipo di azione oppure verrà utilizzata un'estensione di tipo `.zip` per il codice azione che è un file zip.
  ```
  wsk action get actionName --save
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```

2. Invece di consentire alla CLI di determinare il nome file e l'estensione del codice salvato, è possibile fornire un nome file e un'estensione personalizzati utilizzando l'indicatore `--save-as`.
  ```
  wsk action get actionName --save-as codeFile.js
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```

### Crea azioni asincrone
{: #openwhisk_asynchrony_js}

Le funzioni JavaScript eseguite in modo asincrono possono restituire il risultato dell'attivazione dopo il ritorno della funzione `main` restituendo una Promessa nella tua azione.

1. Salva il seguente contenuto in un file denominato `asyncAction.js`.

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

  Nota che la funzione `main` restituisce una Promessa, che indica che l'attivazione non è stata ancora completata, ma il suo completamento è previsto in futuro.

  La funzione JavaScript `setTimeout()` in questo caso attende 2 secondi prima di chiamare la funzione di callback, che rappresenta il codice asincrono e va all'interno della funzione di callback della Promessa.

  Il callback della Promessa utilizza due argomenti, resolve e reject, che sono entrambi funzioni.  La chiamata a `resolve()` soddisfa la Promessa e indica che l'attivazione è stata completata normalmente.

  Una chiamata a `reject()` può essere utilizzata per rifiutare la Promessa e segnalare che l'attivazione è stata completata in modo anomalo.

2. Immetti i seguenti comandi per creare l'azione e richiamarla.

  ```
  wsk action create asyncAction asyncAction.js
  ```
  {: pre}

  ```
  wsk action invoke --result asyncAction
  ```
  {: pre}

  ```json
  {
      "done": true
  }
  ```

  Nota che hai eseguito una chiamata bloccante di un'azione asincrona.

3. Richiama il log di attivazione per vedere quanto tempo è stato impiegato per il completamento dell'attivazione:

  ```
  wsk activation list --limit 1 asyncAction
  ```
  {: pre}
  
  ```
  activations
  b066ca51e68c4d3382df2d8033265db0             asyncAction
  ```

  ```
  wsk activation get b066ca51e68c4d3382df2d8033265db0
  ```
  {: pre}
 
  ```json
  {
      "start": 1455881628103,
      "end":   1455881648126,
      ...
  }
  ```

  Confrontando gli indicatori di data/ora `start` e `end` nel record di attivazione, puoi notare che per il completamento di questa attivazione sono stati impiegati poco più due secondi. 

### Utilizza le azioni per chiamare un'API esterna
{: #openwhisk_apicall_action}

Gli esempi forniti finora rappresentano funzioni JavaScript autonome. Puoi creare anche un'azione che richiama un'API esterna.

Questo esempio richiama un servizio Yahoo Meteo per ottenere le condizioni attuali in un'ubicazione specifica.

1. Salva il seguente contenuto in un file denominato `weather.js`.

  ```javascript
  var request = require('request');

  function main(params) {
      var location = params.location || 'Vermont';
      var url = 'https://query.yahooapis.com/v1/public/yql?q=select item.condition from weather.forecast where woeid in (select woeid from geo.places(1) where text="' + location + '")&format=json';

      return new Promise(function(resolve, reject) {
          request.get(url, function(error, response, body) {
              if (error) {
                  reject(error);
              }
              else {
                  var condition = JSON.parse(body).query.results.channel.item.condition;
                  var text = condition.text;
                  var temperature = condition.temp;
                  var output = 'It is ' + temperature + ' degrees in ' + location + ' and ' + text;
                  resolve({msg: output});
              }
          });
      });
  }
  ```
  {: codeblock}

 Nell'esempio, l'azione utilizza la libreria JavaScript `request` per effettuare una richiesta HTTP all'API Yahoo Meteo ed estrae campi dal risultato JSON. I [Riferimenti](./openwhisk_reference.html#openwhisk_ref_javascript_environments) descrivono in dettaglio i pacchetti Node.js che puoi utilizzare nelle tue azioni.

  Questo esempio mostra anche la necessità di azioni asincrone. L'azione restituisce una Promessa per indicare che il risultato di questa azione non è ancora disponibile nel momento in cui viene restituita la funzione. Il risultato è invece disponibile nel callback `request` al completamento della chiamata HTTP e viene trasmesso come argomento alla funzione `resolve()`.

2. Immetti i seguenti comandi per creare l'azione e richiamarla.

  ```
  wsk action create weather weather.js
  ```
  {: pre}

  ```
  wsk action invoke --result weather --param location "Brooklyn, NY"
  ```
  {: pre}

  ```json
  {
      "msg": "It is 28 degrees in Brooklyn, NY and Cloudy"
  }
  ```

### Assemblaggio di un'azione come modulo Node.js
{: #openwhisk_js_packaged_action}

Come alternativa alla scrittura di tutto il tuo codice azione in un unico file di origine JavaScript, puoi scrivere un'azione come pacchetto `npm`. Considera come esempio una directory con i seguenti file:

Prima, `package.json`:

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

Quindi, `index.js`:

```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}

exports.main = myAction;
```
{: codeblock}

L'azione viene esposta tramite `exports.main`. L'handler azioni stesso può avere qualsiasi nome purché sia conforme alla solita indicazione di accettare un oggetto e restituire un oggetto (o una `Promessa` di un oggetto). Per convenzione Node.js, devi denominare questo file `index.js` o specificare il nome file che preferisci come proprietà `main` in package.json.

Per creare un'azione OpenWhisk da questo pacchetto:

1. Installa prima tutte le dipendenze localmente

  ```
  npm install
  ```
  {: pre}

2. Crea un archivio `.zip` contenente tutti i file (incluse tutte le dipendenze):

  ```
  zip -r action.zip *
  ```
  {: pre}

  L'utilizzo dell'azione di Windows Explorer per la creazione del file zip comporterà una struttura errata. Le azioni zip OpenWhisk devono avere `package.json` al livello root dello zip, mentre Windows Explorer lo inserisce in una cartella nidificata. L'opzione più sicura è quella di utilizzare il comando `zip` nella riga di comando.
  {: tip}

3. Crea l'azione:

  ```
  wsk action create packageAction --kind nodejs:6 action.zip
  ```
  {: pre}

  Durante la creazione di un'azione da un archivio `.zip` con lo strumento CLI, devi fornire esplicitamente un valore per l'indicatore `--kind` utilizzando `nodejs:6` o `nodejs:8`.

4. Puoi richiamare l'azione come qualsiasi altra:

  ```
  wsk action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
  ```
  {: pre}
  
  ```json
  {
      "padded": [
          ".......................and now",
          "......for something completely",
          ".....................different"
      ]
  }
  ```

Infine, nota che mentre la maggior parte dei pacchetti `npm` installa le origini JavaScript su `npm install`, altri installano e compilano anche delle risorse binarie. Il caricamento dei file di archivio non supporta attualmente le dipendenze binarie, ma solo le dipendenze JavaScript. Le chiamate di azioni potrebbero non riuscire se l'archivio include dipendenze binarie.

### Assemblaggio di un'azione come un unico bundle
{: #openwhisk_js_webpack_action}

È opportuno includere solo il codice minimo in un singolo file `.js` che contiene le dipendenze. Questo approccio consente distribuzioni più veloci nei casi in cui l'assemblaggio dell'azione come zip potrebbe essere troppo grande perché include file non necessari.

Puoi utilizzare un bundler di moduli JavaScript come il [webpack](https://webpack.js.org/concepts/). Quando webpack elabora il tuo codice, crea in modo ricorsivo un grafico delle dipendenze che include tutti i moduli necessari per l'azione.

Ecco un rapido esempio con webpack:

Utilizzando l'esempio precedente `package.json`, aggiungi `webpack` come dipendenza di sviluppo e aggiungi alcuni comandi di script npm.
```json
{
  "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
    "build": "webpack --config webpack.config.js",
    "deploy": "bx wsk action update my-action dist/bundle.js --kind nodejs:8"
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

Crea il file di configurazione webpack `webpack.config.js`.
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

Imposta la variabile `global.main` sulla funzione principale dell'azione.
Dall'esempio precedente:
```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}
global.main = myAction;
```
{: codeblock}

Se il nome della tua funzione è `main`, utilizza invece questa sintassi:
```javascript
global.main = main;
```
{: codeblock}


Per creare e distribuire un'azione OpenWhisk con `npm` e `webpack`:

1. Innanzitutto, installa le dipendenze localmente:

  ```
  npm install
  ```
  {: pre}

2. Crea il bundle webpack:

  ```
  npm run build
  ```
  {: pre}

  Il file `dist/bundle.js` è stato creato e viene utilizzato per la distribuzione come codice sorgente dell'azione.

3. Crea l'azione utilizzando lo script `npm` o la CLI.
  Utilizzo dello script `npm`:
  ```
  npm run deploy
  ```
  {: pre}
  Utilizzo della CLI:
  ```
  bx wsk action update my-action dist/bundle.js
  ```
  {: pre}


Infine, il file bundle creato da `webpack` non supporta le dipendenze binarie ma piuttosto le dipendenze JavaScript. Quindi, le chiamate di azioni non riusciranno se il bundle dipende dalle dipendenze binarie, perché questo non è incluso nel file `bundle.js`.

## Crea le sequenze di azioni
{: #openwhisk_create_action_sequence}

Puoi creare un'azione che concatena una sequenza di azioni.

In un pacchetto denominato `/whisk.system/utils` vengono fornite varie azioni di programmi di utilità che puoi utilizzare per creare la tua prima sequenza. Per ulteriori informazioni sui pacchetti, vedi la sezione [Pacchetti](./openwhisk_packages.html).

1. Visualizza le azioni nel pacchetto `/whisk.system/utils`.

  ```
  wsk package get --summary /whisk.system/utils
  ```
  {: pre}

  ```
  package /whisk.system/utils: Mattoni che formattano e assemblano i dati
   action /whisk.system/utils/head: Estrae il prefisso di un array
   action /whisk.system/utils/split: Suddivide una stringa in un array
   action /whisk.system/utils/sort: Ordina un array
   action /whisk.system/utils/echo: Restituisce l'input
   action /whisk.system/utils/date: Data e ora corrente
   action /whisk.system/utils/cat: Concatena l'input in una stringa
  ```

  In questo esempio utilizzi le azioni `split` e `sort`.

2. Crea una sequenza di azioni in modo che il risultato di un'azione venga passato come argomento all'azione successiva.

  ```
  wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

  Questa sequenza di azioni converte alcune righe di testo in array e ordina le righe.

3. Richiama l'azione:

  ```
  wsk action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
  ```
  {: pre}

  ```json
  {
      "length": 3,
      "lines": [
          "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
  }
  ```

  Nel risultato, noterai che le righe sono ordinate.

**Nota**: i parametri che vengono passati tra le azioni nella sequenza sono espliciti, ad eccezione dei parametri predefiniti.
Pertanto, i parametri passati alla sequenza di azioni sono disponibili solo per la prima azione nella sequenza.
Il risultato della prima azione della sequenza diventa l'oggetto JSON di input per la seconda azione della sequenza (e così via).
Questo oggetto non include nessuno dei parametri che erano stati originariamente passati alla sequenza, a meno che la prima azione non li includa esplicitamente nel suo risultato.
I parametri passati a un'azione vengono uniti ai parametri predefiniti dell'azione. I parametri passati hanno la precedenza e sostituiscono tutti i parametri predefiniti corrispondenti.
Per ulteriori informazioni sul richiamo delle sequenze di azioni con più parametri denominati, vedi [Impostazione dei parametri predefiniti](./openwhisk_actions.html#openwhisk_binding_actions).

## Crea azioni Python
{: #creating-python-actions}

Il processo di creazione delle azioni Python è simile a quello delle azioni JavaScript. Le seguenti sezioni ti guidano lungo la creazione e la chiamata di una singola azione Python e l'aggiunta di parametri a tale azione.

### Crea e richiama un'azione Python
{: #openwhisk_actions_python_invoke}

Un'azione è semplicemente una funzione Python di livello superiore. Ad esempio, crea un file denominato `hello.py` con il seguente codice sorgente:

```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
    print(greeting)
    return {"greeting": greeting}
```
{: codeblock}

Le azioni Python utilizzano sempre un dizionario e ne producono un altro. Il metodo di immissione per l'azione è `main` per impostazione predefinita, ma può essere specificato esplicitamente per creare l'azione con la CLI `wsk` utilizzando `--main`, come con qualsiasi altro tipo di azione.

Puoi creare un'azione OpenWhisk denominata `helloPython` partendo da questa funzione nel seguente modo:
```
wsk action create helloPython hello.py
```
{: pre}

La CLI deduce automaticamente il tipo di azione dall'estensione del file di origine. Per i file di origine `.py`, l'azione viene eseguita utilizzando un runtime Python 2. Puoi anche creare un'azione che venga eseguita con Python 3 specificando esplicitamente il parametro `--kind python:3`.
Inoltre, esiste un runtime Python 3 con tipo `python-jessie:3` che contiene pacchetti aggiuntivi per i servizi IBM Cloud come IBM Cloudant, IBM DB2, IBM COS e IBM Watson.
Per ulteriori informazioni sui pacchetti inclusi in questo runtime Python 3, vedi il [riferimento](./openwhisk_reference.html#openwhisk_ref_python_environments) al runtime Python.

Le azioni Python vengono richiamate allo stesso modo delle azioni JavaScript:
```
wsk action invoke --result helloPython --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

### Assemblaggio di azioni Python in file zip
{: #openwhisk_actions_python_zip}

Puoi assemblare un'azione Python e i moduli dipendenti in un file zip.
Il nome del file di origine che contiene il punto di ingresso (ad esempio, `main`) deve essere `__main__.py`.
Ad esempio, per creare un'azione con un modulo helper denominato `helper.py`, crea prima un archivio contenente i tuoi file di origine:

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}

Quindi, crea l'azione:

```bash
wsk action create helloPython --kind python:3 helloPython.zip
```
{: pre}

Anche se questa procedura viene mostrata per Python 3 (con il tipo `python:3`), puoi utilizzarla anche con i tipi Python alternativi `python:2` o `python-jessie:3`.


### Assemblaggio di azioni Python con un ambiente virtuale in file zip
{: #openwhisk_actions_python_virtualenv}

Un altro modo per assemblare le dipendenze Python è quello di utilizzare un ambiente virtuale (`virtualenv`) che ti consente di collegare pacchetti aggiuntivi che possono essere installati, ad esempio, tramite [`pip`](https://packaging.python.org/installing/).


Come nel caso del supporto del file zip di base, il nome del file di origine che contiene il punto di ingresso principale deve essere `__main__.py`. Per chiarire, il contenuto di `__main__.py` è la funzione principale, quindi per questo esempio puoi rinominare `hello.py` in `__main__.py` dalla sezione precedente. Inoltre, la directory dell'ambiente virtuale deve essere denominata `virtualenv`. Vedi il seguente scenario di esempio per installare le dipendenze, assemblarle in un ambiente virtuale e creare un'azione OpenWhisk compatibile.

Per garantire la compatibilità con il contenitore di runtime OpenWhisk, le installazioni dei pacchetti all'interno di un ambiente virtuale devono essere eseguite nell'ambiente di destinazione utilizzando l'immagine corrispondente al tipo.
- Per il tipo `python:2`, utilizza l'immagine docker `openwhisk/python2action`.
- Per il tipo `python:3`, utilizza l'immagine docker `openwhisk/python3action`.
- Per il tipo `python-jessie:3`, utilizza l'immagine docker `ibmfunctions/action-python-v3`.

1. Dato un file [requirements.txt ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) che contiene i moduli `pip` e le versioni da installare, esegui quanto segue per installare le dipendenze e creare un ambiente virtuale utilizzando un'immagine Docker compatibile:
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
    ```
    {: pre}

2. Archivia la directory dell'ambiente virtuale e qualsiasi ulteriore file Python:
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

3. Crea l'azione:
    ```
    wsk action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}


## Crea azioni PHP
{: #creating-php-actions}

Il processo di creazione delle azioni PHP è simile a quello delle azioni JavaScript. Le seguenti sezioni ti guidano lungo la creazione e la chiamata di una singola azione PHP e l'aggiunta di parametri a tale azione.

### Crea e richiama un'azione PHP
{: #openwhisk_actions_php_invoke}

Un'azione è semplicemente una funzione PHP di livello superiore. Ad esempio, crea un file denominato `hello.php` con il seguente codice sorgente:

```php
<?php
function main(array $args) : array
{
    $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
```

Le azioni PHP utilizzano sempre un array associativo e ne restituiscono un altro. Il metodo di immissione per l'azione è `main` per impostazione predefinita, ma può essere specificato esplicitamente quando crei l'azione con la CLI `wsk` utilizzando `--main`, come con qualsiasi altro tipo di azione.

Puoi creare un'azione OpenWhisk denominata `helloPHP` partendo da questa funzione nel seguente modo:

```
wsk action create helloPHP hello.php
```
{: pre}

La CLI deduce automaticamente il tipo di azione dall'estensione del file di origine. Per i file di origine `.php`, l'azione viene eseguita utilizzando un runtime PHP 7.1. Per ulteriori informazioni, vedi il [riferimento](./openwhisk_reference.html#openwhisk_ref_php) per PHP.

Le azioni PHP vengono richiamate allo stesso modo delle azioni JavaScript:

```
wsk action invoke --result helloPHP --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

### Assemblaggio di azioni PHP in file zip
{: #openwhisk_actions_php_zip}

Puoi assemblare un'azione PHP insieme ad altri file e pacchetti dipendenti in un file zip.
Il nome del file di origine che contiene il punto di ingresso (ad esempio, `main`) deve essere `index.php`.
Ad esempio, per creare un'azione che include un secondo file denominato `helper.php`,  crea prima un archivio contenente i tuoi file di origine:

```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

Quindi, crea l'azione:

```bash
wsk action create helloPHP --kind php:7.1 helloPHP.zip
```
{: pre}

## Crea azioni Swift
{: #creating-swift-actions}

Il processo di creazione delle azioni Swift è simile a quello delle azioni JavaScript. Le seguenti sezioni ti guidano lungo la creazione e la chiamata di una singola azione swift e l'aggiunta di parametri a tale azione.

Puoi inoltre utilizzare [Swift Sandbox](https://swiftlang.ng.bluemix.net) online per testare il tuo codice Swift senza dover installare Xcode sulla tua macchina.

### Crea e richiama un'azione

Un'azione è semplicemente una funzione Swift di livello superiore. Ad esempio, crea un file denominato
`hello.swift` con il seguente contenuto:

```swift
func main(args: [String:Any]) -> [String:Any] {
    if let name = args["name"] as? String {
        return [ "greeting" : "Hello \(name)!" ]
    } else {
        return [ "greeting" : "Hello stranger!" ]
    }
}
```
{: codeblock}

Le azioni Swift utilizzano sempre un dizionario e ne producono un altro.

Puoi creare un'azione {{site.data.keyword.openwhisk_short}} denominata `helloSwift` partendo da questa funzione
nel seguente modo:

```
wsk action create helloSwift hello.swift --kind swift:3.1.1
```
{: pre}
 

Specifica sempre `swift:3.1.1` in quanto le versioni Swift precedenti non sono supportate.
{: tip}

Le azioni Swift vengono richiamate allo stesso modo delle azioni JavaScript:

```
wsk action invoke --result helloSwift --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

**Attenzione:** le azioni Swift eseguite in un ambiente Linux sono ancora in fase di sviluppo e {{site.data.keyword.openwhisk_short}} in genere utilizza l'ultima release disponibile, che non è necessariamente stabile. Inoltre, la versione di Swift utilizzata con {{site.data.keyword.openwhisk_short}} potrebbe essere incoerente con le versioni di Swift delle release stabili di XCode su MacOS.

### Assemblaggio di un'azione come eseguibile Swift
{: #openwhisk_actions_swift_zip}

Quando crei un'azione OpenWhisk Swift con un file di origine Swift, questo deve essere compilato in un file binario prima che l'azione venga eseguita. Una volta terminato, le chiamate successive all'azione sono molto più veloci finché il contenitore che include l'azione non viene eliminato. Questo ritardo è noto come ritardo di avvio a freddo.

Per evitare il ritardo di avvio a freddo, puoi compilare il file Swift in un file binario e quindi caricarlo in OpenWhisk in un file zip. Poiché hai bisogno della struttura OpenWhisk, il modo più semplice per creare il file binario è quello di costruirlo all'interno dello stesso ambiente in cui viene eseguito. Vedi la seguente procedura:

- Esegui un contenitore di azioni Swift interattivo utilizzando il seguente comando:
  ```
  docker run --rm -it -v "$(pwd):/owexec" openwhisk/action-swift-v3.1.1 bash
  ```
  {: pre}
  
- Copia il codice sorgente e preparati a costruirlo.
  ```
  cp /owexec/hello.swift /swift3Action/spm-build/main.swift 
  ```
  {: pre}

  ```
  cat /swift3Action/epilogue.swift >> /swift3Action/spm-build/main.swift
  ```
  {: pre}

  ```
  echo '_run_main(mainFunction:main)' >> /swift3Action/spm-build/main.swift
  ```
  {: pre}

- (Facoltativo) Crea il file `Package.swift` per aggiungere le dipendenze.
   ```
   swift import PackageDescription
   
   let package = Package(
     name: "Action",
         dependencies: [
             .Package(url: "https://github.com/apple/example-package-deckofplayingcards.git", majorVersion: 3),
             .Package(url: "https://github.com/IBM-Swift/CCurl.git", "0.2.3"),
             .Package(url: "https://github.com/IBM-Swift/Kitura-net.git", "1.7.10"),
             .Package(url: "https://github.com/IBM-Swift/SwiftyJSON.git", "15.0.1"),
             .Package(url: "https://github.com/watson-developer-cloud/swift-sdk.git", "0.16.0")
         ]
   )
   ```
   {: pre}

  Questo esempio aggiunge le dipendenze `swift-watson-sdk` e `example-package-deckofplayingcards`.
  Nota che `CCurl`, `Kitura-net` e `SwiftyJSON` sono forniti nell'azione Swift standard in modo da poterli includere nel tuo `Package.swift`.

- Copia Package.swift nella directory spm-build
  ```
  cp /owexec/Package.swift /swift3Action/spm-build/Package.swift
  ```
  {: pre}

- Passa alla directory spm-build
  ```
  cd /swift3Action/spm-build
  ```
  {: pre}

- Compila la tua azione Swift.
  ```
  swift build -c release
  ```
  {: pre}

- Crea l'archivio zip.
  ```
  zip /owexec/hello.zip .build/release/Action
  ```
  {: pre}

- Esci dal contenitore Docker.
  ```
  exit
  ```
  {: pre}

Puoi vedere che hello.zip viene creato nella stessa directory di hello.swift. 

- Caricalo in OpenWhisk con il nome azione helloSwifty:
  ```
  wsk action update helloSwiftly hello.zip --kind swift:3.1.1
  ```
  {: pre}

- Per verificare la sua velocità, esegui 
  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

Il tempo impiegato per l'esecuzione dell'azione è nella proprietà "duration" e viene confrontato con il tempo impiegato per l'esecuzione con un passo di compilazione nell'azione hello.

## Crea azioni Java
{: #creating-java-actions}

Il processo di creazione delle azioni Java è simile a quello delle azioni JavaScript e Swift. Le seguenti sezioni ti guidano lungo la creazione e la chiamata di una singola azione Java e l'aggiunta di parametri a tale azione.

Per compilare, testare e archiviare i file Java, devi avere un [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) installato localmente.

### Crea e richiama un'azione
{: #openwhisk_actions_java_invoke}

Un'azione Java è un programma Java con un metodo denominato `main` con la firma esatta come nel seguente esempio:
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Ad esempio, crea un file Java denominato `Hello.java` con il seguente contenuto:

```java
import com.google.gson.JsonObject;
public class Hello {
    public static JsonObject main(JsonObject args) {
        String name = "stranger";
        if (args.has("name"))
            name = args.getAsJsonPrimitive("name").getAsString();
        JsonObject response = new JsonObject();
        response.addProperty("greeting", "Hello " + name + "!");
        return response;
    }
}
```
{: codeblock}

Quindi, compila `Hello.java` in un file JAR `hello.jar` nel seguente modo:
```
javac Hello.java
```
{: pre}

```
jar cvf hello.jar Hello.class
```
{: pre}

Per compilare il file Java, è necessario che [google-gson](https://github.com/google/gson) sia presente nel tuo CLASSPATH Java.
{: tip}

Puoi creare un'azione OpenWhisk denominata `helloJava` partendo da questo file JAR nel seguente
modo:

```
wsk action create helloJava hello.jar --main Hello
```
{: pre}

Quando utilizzi la riga di comando e un file di origine `.jar`, non devi necessariamente specificare che stai creando un'azione Java; lo strumento lo desume dall'estensione del file.

Devi specificare il nome della classe principale utilizzando `--main`. Una classe principale idonea è una classe che implementa un metodo `main` statico. Se la classe non si trova nel pacchetto predefinito, utilizza il nome classe Java completo, ad esempio, `--main com.example.MyMain`.

Se necessario, puoi anche personalizzare il nome del metodo della tua azione Java. Per farlo, specifica il nome del metodo Java completo della tua azione, ad esempio, `--main com.example.MyMain#methodName`.

Le azioni Java vengono richiamate allo stesso modo delle azioni Swift e JavaScript:

```
wsk action invoke --result helloJava --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

## Crea azioni Docker
{: #creating-docker-actions}

Con le azioni {{site.data.keyword.openwhisk_short}} Docker, puoi scrivere le tue azioni in qualsiasi linguaggio.

Il tuo codice viene compilato in un file binario eseguibile e incorporato in un'immagine Docker. Il programma binario interagisce con il sistema, prendendo l'input proveniente da `stdin` e rispondendo tramite `stdout`.

È necessario, come requisito, disporre di un account Docker Hub.  Per impostare un account e un ID Docker gratuiti, vai a [Docker Hub](https://hub.docker.com).

Per le seguenti istruzioni, supponiamo che l'ID utente Docker sia `janesmith` e che password sia `janes_password`.  Supponendo che la CLI sia configurata, rimangono tre passaggi per configurare un file binario personalizzato che verrà utilizzato da {{site.data.keyword.openwhisk_short}}. Al termine di questa procedura, l'immagine Docker caricata può essere utilizzata come azione.

1. Scarica la struttura di base Docker. Puoi scaricarla e installarla utilizzando la CLI nel seguente modo:

  ```
  wsk sdk install docker
  ```
  {: pre}

  La struttura di base Docker è ora installata nella directory corrente.
  
  ```
  ls dockerSkeleton/
  ```
  {: pre}

  ```
  Dockerfile      README.md       buildAndPush.sh example.c
  ```

  La struttura di base è un template del contenitore Docker in cui puoi inserire il tuo codice sotto forma di file binari personalizzati.

2. Configura il tuo file binario personalizzato nella struttura di base black box. La struttura di base include già un programma C che puoi utilizzare.

  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

  Puoi modificare il file come desideri o puoi aggiungere ulteriore codice e dipendenze all'immagine Docker.
  In quest'ultimo caso, puoi modificare il `Dockerfile` come necessario per creare il tuo eseguibile.
  Il binario deve trovarsi all'interno del contenitore in `/action/exec`.

  L'eseguibile riceve un singolo argomento dalla riga di comando. Si tratta di una serializzazione di stringa dell'oggetto JSON che rappresenta gli argomenti dell'azione. Il programma può accedere a `stdout` o `stderr`.
  Per convenzione, l'ultima riga dell'output _deve_ essere un oggetto JSON in stringhe che rappresenta il risultato dell'azione.

3. Crea l'immagine Docker e caricala attraverso uno script fornito. Devi prima eseguire `docker login` per autenticarti, quindi eseguire lo script con un nome immagine a scelta.

  ```
  docker login -u janesmith -p janes_password
  ```
  {: pre}

  ```
  cd dockerSkeleton
  ```
  {: pre}

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  Nota che una parte del file `example.c` viene compilata nell'ambito del processo di creazione dell'immagine Docker, quindi non hai bisogno di avere C compilato sulla tua macchina.
  In effetti, a meno che tu non stia compilando il binario su una macchina host compatibile, non può essere eseguito all'interno del contenitore poiché i formati non corrispondono.

  Il tuo contenitore Docker può ora essere utilizzato come azione OpenWhisk.


  ```
  wsk action create example --docker janesmith/blackboxdemo
  ```
  {: pre}

  Nota l'uso di `--docker` per creare un'azione. Si presume che tutte le immagini Docker siano ospitate su Docker Hub.
  L'azione può essere richiamata come qualsiasi altra azione {{site.data.keyword.openwhisk_short}}. 

  ```
  wsk action invoke --result example --param payload Rey
  ```
  {: pre}

  ```json
  {
      "args": {
          "payload": "Rey"
      },
      "msg": "Hello from arbitrary C program!"
  }
  ```

  Per aggiornare l'azione Docker, esegui `buildAndPush.sh` per caricare l'immagine più recente in Docker Hub. Ciò consente al sistema di estrarre la tua nuova immagine Docker la prossima volta che eseguirà il codice per la tua azione. Se non ci sono contenitori caldi, le nuove chiamate utilizzano la nuova immagine Docker. Tuttavia, se è presente un contenitore caldo che utilizza una versione precedente della tua immagine Docker, qualsiasi nuova chiamata continuerà a utilizzare tale immagine a meno che tu non esegua il comando `wsk action update`. Ciò indica al sistema, che per le nuove chiamate, si eseguirà un docker pull per ottenere la tua nuova immagine Docker.

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  ```
  wsk action update example --docker janesmith/blackboxdemo
  ```
  {: pre}

  Per ulteriori informazioni sulla creazione di azioni Docker, vedi la sezione [Riferimenti](./openwhisk_reference.html#openwhisk_ref_docker).

  La versione precedente della CLI supportava `--docker` senza un parametro e il nome dell'immagine era un argomento posizionale. Per consentire alle azioni Docker di accettare i dati di inizializzazione tramite un file (zip), normalizza l'esperienza utente per le azioni Docker in modo che un argomento posizionale, se presente, debba essere un file (ad esempio, un file zip). Il nome dell'immagine deve essere specificato dopo l'opzione `--docker`. Grazie al feedback degli utenti, l'argomento `--native` è incluso come abbreviazione di `--docker openwhisk/dockerskeleton`, in modo che gli eseguibili eseguiti all'interno dell'SDK di azione Docker standard siano più pratici da creare e distribuire.
  
  Ad esempio, questa esercitazione crea un eseguibile binario all'interno del contenitore ubicato in `/action/exec`. Se copi questo file nel tuo file system locale e lo comprimi in `exec.zip`, puoi utilizzare i seguenti comandi per creare un'azione docker che riceve l'eseguibile come dati di inizializzazione. 

  ```
  wsk action create example exec.zip --native
  ```
  {: pre}

  Che è equivalente al seguente comando. 
  ```
  wsk action create example exec.zip --docker openwhisk/dockerskeleton
  ```
  {: pre}

## Creazione di azioni utilizzando eseguibili arbitrari
{: #creating-actions-arbitrary}

Utilizzando `--native`, puoi vedere che qualsiasi eseguibile può essere eseguito come un'azione OpenWhisk. Ciò include script `bash` o binari compilati in modo incrociato. Per quest'ultimo caso, la restrizione è che il binario sia compatibile con l'immagine `openwhisk/dockerskeleton`.

## Creazione di azioni Go
{: #creating-go-actions}

L'opzione `--native` consente di assemblare qualsiasi eseguibile come azione. Come esempio, questo funziona per Go.
Come per le azioni Docker, l'eseguibile Go riceve un singolo argomento dalla riga di comando.
Si tratta di una serializzazione di stringa dell'oggetto JSON che rappresenta gli argomenti dell'azione.
Il programma può accedere a `stdout` o `stderr`.
Per convenzione, l'ultima riga dell'output _deve_ essere un oggetto JSON in stringhe che rappresenta il risultato dell'azione.

Ecco un esempio di azione Go.
```go
package main

import "encoding/json"
import "fmt"
import "os"

func main() {
    //program receives one argument: the JSON object as a string
    arg := os.Args[1]
   
    // unmarshal the string to a JSON object
    var obj map[string]interface{}
    json.Unmarshal([]byte(arg), &obj)

    // can optionally log to stdout (or stderr)
    fmt.Println("hello Go action")

    name, ok := obj["name"].(string)
    if !ok { name = "Stranger" }

    // last line of stdout is the result JSON object as a string
    msg := map[string]string{"msg": ("Hello, " + name + "!")}
    res, _ := json.Marshal(msg)
    fmt.Println(string(res))
}
```

Salva il codice precedente in un file `sample.go` ed effettuane la compilazione incrociata per OpenWhisk. L'eseguibile deve essere denominato `exec`.
```bash
GOOS=linux GOARCH=amd64 go build -o exec
zip exec.zip exec
wsk action create helloGo --native exec.zip
```

L'azione può essere eseguita come qualsiasi altra azione.
```bash
wsk action invoke helloGo -r -p name gopher
{
    "msg": "Hello, gopher!"
}
```

I log vengono recuperati in modo simile.
```bash
wsk activation logs --last --strip
my first Go action.
```

## Monitora l'output dell'azione
{: #watching-action-output}

Le azioni {{site.data.keyword.openwhisk_short}} potrebbero essere richiamate da altri utenti, in risposta a vari eventi o come parte di una sequenza di azioni. In questi casi, può essere utile monitorare le chiamate.

Puoi utilizzare la CLI {{site.data.keyword.openwhisk_short}} per visualizzare l'output delle azioni non appena vengono richiamate.

1. Immetti il seguente comando da una shell:
  ```
  wsk activation poll
  ```
  {: pre}

  Questo comando avvia un ciclo di polling che cerca continuamente attivazioni nei log.

2. Passa a un'altra finestra e richiama un'azione:

  ```
  wsk action invoke /whisk.system/samples/helloWorld --param payload Bob
  ```
  {: pre}

  ```
  ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
  ```

3. Osserva il log delle attivazioni nella finestra di polling:

  ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
  ```

  Allo stesso modo, ogni volta che esegui il programma di utilità di polling, puoi vedere i log in tempo reale per individuare eventuali azioni eseguite per tuo conto in OpenWhisk.

## Supporto per le applicazioni di grandi dimensioni
{: #large-app-support}

La dimensione massima del codice per un'azione è 48 MB. Le applicazioni che contengono molti moduli di terze parti, librerie native o strumenti esterni possono incorrere in questo limite.

Se ti capita di creare un pacchetto Azione (zip o jar) di dimensioni superiori a 48 MB, la soluzione è di estendere l'immagine di runtime con le dipendenze e quindi utilizzare un singolo file di origine o un archivio più piccolo di 48 MB.

Ad esempio, creando un runtime Docker personalizzato, che include le librerie condivise necessarie, la presenza di queste dipendenze non è richiesta nel file di archivio. I file di origine privati possono ancora essere raggruppati nell'archivio e inseriti in fase di runtime.

Un altro vantaggio nella riduzione delle dimensioni dei file di archivio è il miglioramento dei tempi di distribuzione.

### Esempio di Python

Nel seguente esempio di Python, opencv può includere la libreria `opencv-python` e quindi installare il file binario opencv nell'immagine del sistema operativo. Puoi quindi utilizzare `requirements.txt` ed eseguire `pip install requirements.txt` per ampliare l'immagine con più librerie Python. Infine, puoi utilizzare `action.py` con la nuova immagine.

### Esempio di Node.js

Nel seguente esempio di Node.js, puoi installare pacchetti aggiuntivi nell'immagine del sistema operativo:

Installa opencv utilizzando `npm`:
```
npm install opencv
```
{: pre}

Allo stesso modo, se hai un `package.json`, installalo utilizzando `npm`:
```
npm install package.json
```
{: pre}

Quindi, continua a usare `action.js` con la nuova immagine.

## Elenca le azioni
{: #listing-actions}

Puoi elencare tutte le azioni create utilizzando il seguente comando:

```
wsk action list
```
{: pre}

Man mano che scrivi più azioni, l'elenco diventa più lungo e può essere utile raggruppare le azioni correlate in dei [pacchetti](./openwhisk_packages.html). Per filtrare l'elenco di azioni per ottenere solo quelle all'interno di uno specifico pacchetto, puoi utilizzare la seguente sintassi di comando: 

```
wsk action list [NOME PACCHETTO]
```
{: pre}

## Elimina le azioni
{: #deleting-actions}

Puoi effettuare una pulizia eliminando le azioni che non desideri utilizzare.

1. Immetti il seguente comando per eliminare un'azione:
  ```
  wsk action delete hello
  ```
  {: pre}

  ```
  ok: deleted hello
  ```

2. Verifica che l'azione non venga più mostrata nell'elenco di azioni.
  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  ```

## Accesso ai metadati dell'azione nel corpo dell'azione
{: #accessing-action-metadata-within-the-action-body}

L'ambiente dell'azione contiene diverse proprietà specifiche per l'azione in esecuzione.
Queste consentono all'azione di funzionare a livello di programmazione con le risorse OpenWhisk tramite l'API REST
o di impostare un allarme interno quando l'azione sta per esaurire il budget di tempo assegnato.
Le proprietà sono accessibili tramite l'ambiente di sistema per tutti i runtime supportati:
azioni Node.js, Python, Swift, Java e Docker quando si utilizza la struttura di base Docker OpenWhisk.

* `__OW_API_HOST` l'host API per la distribuzione OpenWhisk che esegue questa azione
* `__OW_API_KEY` la chiave API per il soggetto che richiama l'azione, questa chiave potrebbe essere una chiave API limitata
* `__OW_NAMESPACE` lo spazio dei nomi per l'_attivazione_ (potrebbe essere diverso dallo spazio dei nomi per l'azione)
* `__OW_ACTION_NAME` il nome completo dell'azione in esecuzione
* `__OW_ACTIVATION_ID` l'ID di attivazione per questa istanza dell'azione in esecuzione
* `__OW_DEADLINE` il tempo approssimativo in cui questa azione avrà consumato la propria intera quota di durata (misurato in millisecondi)
