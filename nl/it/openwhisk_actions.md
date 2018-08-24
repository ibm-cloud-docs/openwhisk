---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-23"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Creazione e chiamata di azioni
{: #openwhisk_actions}

Le azioni sono frammenti di codice senza stato eseguiti sulla piattaforma {{site.data.keyword.openwhisk}}. Ad esempio, un'azione può essere utilizzata per rilevare i volti in un'immagine, rispondere a una modifica del database, aggregare una serie di chiamate API o pubblicare un tweet.
{:shortdesc}

Le azioni possono essere richiamate esplicitamente o eseguite in risposta a un evento. In entrambi i casi, ogni esecuzione di un'azione produce un record di attivazione identificato da un ID di attivazione univoco. L'input in un'azione e i risultati di un'azione fungono da dizionario di coppie chiave-valore, dove la chiave è una stringa e il valore è un valore JSON valido. Le azioni possono anche essere composte da chiamate ad altre azioni o da una sequenza definita di azioni.

Un'azione può essere scritta sotto forma di funzione JavaScript, Swift, Python, PHP, come un metodo Java o come qualsiasi eseguibile compatibile a livello binario, come i programmi Go ed eseguibili personalizzati forniti come contenitori Docker. Impara a creare, richiamare ed eseguire il debug di azioni nel tuo ambiente di sviluppo preferito.

## Creazione di azioni JavaScript
{: #creating-and-invoking-javascript-actions}

Le seguenti sezioni ti illustrano come utilizzare le azioni in JavaScript. Inizia creando e richiamando un'azione semplice. Aggiungi quindi i parametri all'azione e richiama tale azione con i parametri. Imposta quindi i parametri predefiniti e richiamali. Crea infine le azioni asincrone.

### Creazione e chiamata di un'azione JavaScript semplice
{: #openwhisk_single_action_js}

Consulta la procedura e gli esempi di seguito riportati per creare la tua prima azione JavaScript.

1. Salva il seguente codice in un file denominato `hello.js`.
  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  Il file JavaScript potrebbe contenere ulteriori funzioni. Tuttavia, per convenzione, è necessaria una funzione denominata `main` per fornire il punto di ingresso per l'azione.

2. Crea un'azione denominata `hello` utilizzando la funzione JavaScript.
  ```
  ibmcloud fn action create hello hello.js
  ```
  {: pre}

  Output di esempio:
  ```
  ok: created action hello
  ```
  {: screen}

  La CLI deduce automaticamente il tipo di azione utilizzando l'estensione del file di origine. Per i file di origine `.js`, l'azione viene eseguita utilizzando un runtime Node.js 6. Puoi anche creare un'azione che venga eseguita con Node.js 8 specificando esplicitamente il parametro `--kind nodejs:8`. Per ulteriori informazioni, vedi il [riferimento](./openwhisk_reference.html#openwhisk_ref_javascript_environments) per Node.js 6 e 8 .

3. Verifica che la tua azione `hello` sia presente nel tuo elenco delle azioni.
  ```
  ibmcloud fn action list
  ```
  {: pre}

  Output di esempio:
  ```
  actions
  hello       private
  ```
  {: screen}

4. Esegui l'azione nel cloud eseguendo una chiamata bloccante. Le chiamate bloccanti utilizzano uno stile richiesta/risposta e attendono che il risultato dell'attivazione sia disponibile. Il periodo di attesa è inferiore a 60 secondi o al [valore del limite di tempo](./openwhisk_reference.html#openwhisk_syslimits) dell'azione.
    ```
    ibmcloud fn action invoke --blocking hello
    ```
    {: pre}

    Il comando genera in output le seguenti informazioni:
        * L'ID attivazione (`44794bd6aab74415b4e42a308d880e5b`) che può essere utilizzato per richiamare i log o il risultato della chiamata
        * Il risultato della chiamata se è disponibile entro il periodo di attesa previsto
    ```
    ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

    {
        "result": {
            "payload": "Hello world"
        },
      "status": "success",
      "success": true
  }
    ```
    {: screen}
    Per suggerimenti sul monitoraggio delle attivazioni, vedi [Monitoraggio dell'output dell'azione](openwhisk_managing.html#monitor-action-output).
    {: tip}

5. Se non ti serve subito il risultato dell'azione, puoi omettere l'indicatore `--blocking` per eseguire una chiamata non bloccante.
    1. Esegui una chiamata non bloccante.
        ```
        ibmcloud fn action invoke hello
        ```
        {: pre}

        Output di esempio:
        ```
        ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: screen}

    2. Utilizza l'ID attivazione per ottenere il risultato dell'azione.
        ```
        ibmcloud fn activation result 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: pre}

        Viene restituito il risultato dell'azione:
        ```
        {
            "payload": "Hello world"
  }
        ```
        {: screen}

6. Se dimentichi di registrare l'ID attivazione, puoi ottenere un elenco di attivazioni ordinate dalla più recente alla meno recente.
    ```
    ibmcloud fn activation list
    ```
    {: pre}

    Output:
    ```
    activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
    ```
    {: screen}

### Creazione di azioni asincrone
{: #openwhisk_asynchrony_js}

Le funzioni JavaScript eseguite in modo asincrono possono restituire il risultato dell'attivazione dopo il ritorno della funzione `main` restituendo una Promessa nella tua azione.

1. Salva il seguente codice in un file denominato `asyncAction.js`.
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

    * La funzione `main` restituisce una Promessa. La Promessa indica che l'attivazione non è stata ancora completata, ma il suo completamento è previsto in futuro.
    * La funzione JavaScript `setTimeout()` attende 2 secondi prima di chiamare la funzione di callback della Promessa, che rappresenta il codice asincrono.
    * Il callback della Promessa accetta gli argomenti `resolve` e `reject`, che sono entrambi funzioni.
      * La chiamata a `resolve()` soddisfa la Promessa e indica che l'attivazione è stata completata normalmente.
      * Una chiamata a `reject()` può essere utilizzata per rifiutare la Promessa e segnalare che l'attivazione è stata completata in modo anomalo.

2. Crea un'azione denominata `asyncAction`.
    ```
    ibmcloud fn action create asyncAction asyncAction.js
    ```
    {: pre}

3. Richiama l'azione.
    ```
    ibmcloud fn action invoke --result asyncAction
    ```
    {: pre}

    Output di esempio:
    ```
    {
        "done": true
  }
    ```
    {: screen}

3. Controlla quanto tempo ha impiegato il completamento dell'attivazione ottenendo il log di attivazione.

  1. Ottieni l'ID attivazione.
      ```
      ibmcloud fn activation list --limit 1 asyncAction
      ```
      {: pre}

      Output di esempio:
      ```
      activations
  b066ca51e68c4d3382df2d8033265db0             asyncAction
      ```
      {: screen}

  2. Ottieni il log per l'ID attivazione.
      ```
      ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
      ```
      {: pre}

      `duration` mostra che il completamento di questa attivazione ha richiesto poco più di 2 secondi:
      ```
      ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
          ...
          "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
          ...
      }
      ```
      {: screen}

### Utilizzo di azioni per la chiamata di un'API esterna
{: #openwhisk_apicall_action}

Gli esempi forniti finora rappresentano funzioni JavaScript autonome. Puoi creare anche un'azione che richiama un'API esterna.
{: shortdesc}

Il seguente esempio richiama il servizio della NASA APOD (Astronomy Picture of the Day) che fornisce ogni giorno un'immagine univoca del nostro universo.

1. Salva il seguente codice in un file denominato `apod.js`.
    ```javascript
    var url = "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo";

    $.ajax({
    url: url,
    success: function(result){
    if("copyright" in result) {
        $("#copyright").text("Image Credits: " + result.copyright);
    }
    else {
        $("#copyright").text("Image Credits: " + "Public Domain");
      }

      if(result.media_type == "video") {
        $("#apod_img_id").css("display", "none");
      $("#apod_vid_id").attr("src", result.url);
    }
    else {
        $("#apod_vid_id").css("display", "none");
      $("#apod_img_id").attr("src", result.url);
    }
      $("#reqObject").text(url);
    $("#returnObject").text(JSON.stringify(result, null, 4));
    $("#apod_explaination").text(result.explanation);
    $("#apod_title").text(result.title);
  }
    });
    ```
    {: codeblock}

    Viene effettuata una chiamata all'API NASA APOD e i campi vengono estratti dal risultato JSON. Per ulteriori informazioni su quali pacchetti Node.js puoi utilizzare nelle tue azioni, vedi [Dettagli e limiti del sistema](./openwhisk_reference.html#openwhisk_ref_javascript_environments).

2. Crea un'azione denominata `apod`.
    ```
    ibmcloud fn action create apod apod.js
    ```
    {: pre}

3. Richiama l'azione `apod`.
    ```
    ibmcloud fn action invoke --result apod
    ```
    {: pre}

    Viene restituito il seguente oggetto di esempio:
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

### Assemblaggio di un'azione come modulo Node.js
{: #openwhisk_js_packaged_action}

Come alternativa alla scrittura di tutto il tuo codice azione in un unico file di origine JavaScript, puoi scrivere un'azione come pacchetto `npm`.

Ad esempio, considera una directory con i seguenti file:

1. Salva il seguente codice in un file denominato `package.json`.
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

2. Salva il seguente codice in un file denominato `index.js`.
    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    exports.main = myAction;
    ```
    {: codeblock}
    * L'azione viene esposta tramite `exports.main`.
    * L'handler azioni può avere qualsiasi nome purché sia conforme alla classica firma di accettazione di un oggetto e di restituzione di un oggetto (o una `Promessa` di un oggetto). 
    * Devi denominare questo file **index.js** o specificare il nome file da te preferito come proprietà `main` in **package.json**.

3. Installa tutte le dipendenze localmente.
    ```
    npm install
    ```
    {: pre}
    **Nota**: mentre la maggior parte dei pacchetti `npm` installa le origini JavaScript su `npm install`, altri installano e compilano anche delle risorse binarie. Il caricamento dei file di archivio attualmente supporta solo le dipendenze JavaScript. Le chiamate di azioni potrebbero non riuscire se l'archivio include dipendenze binarie.

4. Crea un archivio `.zip` che contiene tutti i file, incluse tutte le dipendenze.
    ```
    zip -r action.zip *
    ```
    {: pre}

    L'utilizzo dell'azione di Esplora risorse di Windows per la creazione del file zip comporterà una struttura errata. Le azioni dello zip {{site.data.keyword.openwhisk_short}} devono avere `package.json` al livello root dello zip, mentre Windows Explorer lo inserisce in una cartella nidificata. L'opzione più sicura è quella di utilizzare il comando `zip` nella riga di comando.
    {: tip}

5. Crea l'azione. Quando crei un'azione da un archivio `.zip`, devi fornire esplicitamente un valore per l'indicatore `--kind` utilizzando `nodejs:6` o `nodejs:8`.
```
    ibmcloud fn action create packageAction --kind nodejs:6 action.zip
    ```
    {: pre}

6. Richiama l'azione.
    ```
    ibmcloud fn action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
    ```
    {: pre}

    Output di esempio:
    ```
    {
        "padded": [
            ".......................and now",
          "......for something completely",
          ".....................different"
      ]
    }
    ```
    {: screen}

### Assemblaggio di un'azione come unico bundle
{: #openwhisk_js_webpack_action}

Se l'assemblaggio dell'azione come uno zip include troppi file non necessari oppure se hai bisogno di una distribuzione più rapida, puoi scrivere il codice minimo in un singolo file `.js` che include le dipendenze.
{: shortdesc}

Puoi assemblare un'azione utilizzando un bundler di moduli JavaScript come [webpack ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://webpack.js.org/concepts/). Quando `webpack` elabora il tuo codice, crea in modo ricorsivo un grafico delle dipendenze che include ogni modulo di cui la tua azione ha bisogno.

1. Salva il seguente codice in un file denominato `package.json`. `webpack` viene aggiunto come una dipendenza di sviluppo.
    ```json
    {
      "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
        "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:8"
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

2. Salva il seguente codice di configurazione webpack in un file denominato `webpack.config.js`.
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

3. Salva il seguente codice in un file denominato `index.js`. La variabile `global.main` è impostata sulla funzione principale dell'azione.
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

5. Crea il bundle webpack.
    ```
    npm run build
    ```
    {: pre}

    Il file `dist/bundle.js` viene creato e viene distribuito come codice sorgente dell'azione.

6. Crea l'azione utilizzando lo script `npm` o la CLI.
    * Utilizzo dello script `npm`:
        ```
        npm run deploy
        ```
        {: pre}

    * Utilizzo della CLI:
        ```
        ibmcloud fn action update my-action dist/bundle.js
        ```
        {: pre}

    **Nota**: il file bundle creato da `webpack` supporta solo le dipendenze JavaScript. Le chiamate di azioni potrebbero non riuscire se il bundle dipende dalle dipendenze binarie perché questo non è incluso con il file `bundle.js`.

## Creazione di azioni Python
{: #creating-python-actions}

Le seguenti sezioni ti guidano nella creazione e la chiamata di una singola azione Python e l'aggiunta di parametri a tale azione.

### Creazione e chiamata di un'azione Python
{: #openwhisk_actions_python_invoke}

Un'azione è semplicemente una funzione Python di livello superiore. Per creare un'azione Python:

1. Salva il seguente codice in un file denominato `hello.py`.
    ```python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
        print(greeting)
    return {"greeting": greeting}
    ```
    {: codeblock}

  * Le azioni Python utilizzano sempre un dizionario e ne producono un altro.
  * Il metodo di immissione per l'azione è `main` per impostazione predefinita ma può essere specificato esplicitamente per creare l'azione con la CLI `wsk` utilizzando l'indicatore `--main`.

2. Crea un'azione `helloPython`.
    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    Output di esempio:
    ```
    ok: created action helloPython
    ```
    {: screen}

    La CLI deduce automaticamente il tipo di azione utilizzando l'estensione del file di origine. Per i file di origine `.py`, l'azione viene eseguita utilizzando un runtime Python 2. Puoi anche creare un'azione che venga eseguita con Python 3 specificando esplicitamente il parametro `--kind python:3`. Puoi anche utilizzare il runtime Python 3 con tipo `python-jessie:3`, che contiene pacchetti aggiuntivi per i servizi IBM Cloud come {{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.Db2_on_Cloud_long_notm}}, {{site.data.keyword.cos_full_notm}} e {{site.data.keyword.ibmwatson_notm}}. Per ulteriori informazioni sui pacchetti inclusi in questo runtime Python 3, vedi il [riferimento](./openwhisk_reference.html#openwhisk_ref_python_environments) al runtime Python.

3. Richiama l'azione.
    ```
    ibmcloud fn action invoke --result helloPython --param name World
    ```
    {: pre}

    Output di esempio:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Assemblaggio di azioni Python in file zip
{: #openwhisk_actions_python_zip}

Puoi assemblare un'azione Python e i moduli dipendenti in un file zip. Ad esempio, per creare un'azione con un modulo helper denominato `helper.py`:

1. Crea un archivio contenente i tuoi file di origine. **Nota**: il file di origine che contiene il punto di ingresso deve essere denominato `__main__.py`.
    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. Crea l'azione.
    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

### Assemblaggio di azioni Python con un ambiente virtuale in file zip
{: #openwhisk_actions_python_virtualenv}

Puoi assemblare le dipendenze Python utilizzando un ambiente virtuale, `virtualenv`. L'ambiente virtuale ti consente di collegare pacchetti aggiuntivi che possono essere installati utilizzando [`pip` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://packaging.python.org/installing/), ad esempio.

Per installare le dipendenze, assemblale in un ambiente virtuale e crea un'azione OpenWhisk compatibile.

1. Crea un file [requirements.txt ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) che contiene i moduli `pip` e le versioni da installare.

2. Installa le dipendenze e crea un ambiente virtuale. La directory dell'ambiente virtuale deve essere denominata `virtualenv`. Per garantire la compatibilità con il contenitore di runtime OpenWhisk, le installazioni dei pacchetti all'interno di un ambiente virtuale devono utilizzare l'immagine che corrisponde al tipo.
    - Per il tipo `python:2`, utilizza l'immagine docker `openwhisk/python2action`.
    - Per il tipo `python:3`, utilizza l'immagine docker `openwhisk/python3action`.
    - Per il tipo `python-jessie:3`, utilizza l'immagine docker `ibmfunctions/action-python-v3`.
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp &&virtualenv virtualenv &&source virtualenv/bin/activate &&pip install -r requirements.txt"
    ```
    {: pre}

3. Assembla la directory `virtualenv` e gli eventuali file Python aggiuntivi. Il file di origine che contiene il punto di ingresso deve essere denominato `__main__.py`.
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. Crea l'azione `helloPython`.
    ```
    ibmcloud fn action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}

Aggiungi solo i moduli che non fanno parte dell'ambiente di runtime selezionato a `requirements.txt`. Questo aiuta a mantenere `virtualenv` a una dimensione minima.
{: tip}

## Creazione di azioni PHP
{: #creating-php-actions}

Le seguenti sezioni ti guidano nella creazione e la chiamata di una singola azione PHP e l'aggiunta di parametri a tale azione.

### Creazione e chiamata di un'azione PHP
{: #openwhisk_actions_php_invoke}

Un'azione è semplicemente una funzione PHP di livello superiore. Per creare un'azione PHP:

1. Salva il seguente codice in un file denominato `hello.php`.
    ```
    <?php
function main(array $args) : array
{
        $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
    ```
    {: codeblock}

    * Le azioni PHP utilizzano sempre un array associativo e ne restituiscono un altro.
    * Il metodo di immissione per l'azione è `main` per impostazione predefinita ma può essere specificato esplicitamente quando crei l'azione con la CLI `ibmcloud fn` utilizzando l'indicatore `--main`.

2. Crea un'azione denominata `helloPHP`.
    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    La CLI deduce automaticamente il tipo di azione dall'estensione del file di origine. Per i file di origine `.php`, l'azione viene eseguita utilizzando un runtime PHP 7.1. Per ulteriori informazioni, vedi il [riferimento](./openwhisk_reference.html#openwhisk_ref_php) per PHP.

3. Richiama l'azione.
    ```
    ibmcloud fn action invoke --result helloPHP --param name World
    ```
    {: pre}

    Output di esempio:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Assemblaggio di azioni PHP in file zip
{: #openwhisk_actions_php_zip}

Puoi assemblare un'azione PHP e altri file o pacchetti dipendenti in un file zip. Ad esempio, per assemblare un'azione con un secondo file denominato `helper.php`:

1. Crea un archivio contenente i tuoi file di origine. **Nota**: il file di origine che contiene il punto di ingresso deve essere denominato `index.php`.
    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. Crea l'azione.
    ```bash
    ibmcloud fn action create helloPHP --kind php:7.1 helloPHP.zip
    ```
    {: pre}

## Creazione di azioni Swift
{: #creating-swift-actions}

Le seguenti sezioni ti guidano nella creazione e la chiamata di una singola azione Swift e l'assemblaggio di un'azione in un file zip.

**Nota:** le azioni Swift vengono eseguite in un ambiente Linux. Swift su Linux è ancora in fase di sviluppo e {{site.data.keyword.openwhisk_short}} utilizza l'ultima release disponibile. Tali release potrebbero non essere stabili. La versione di Swift utilizzata con {{site.data.keyword.openwhisk_short}} potrebbe essere incongruente con le versioni di Swift dalle release stabili di Xcode su MacOS.

Per ulteriori informazioni sul runtime Swift, vedi la [guida di riferimento](./openwhisk_reference.html#swift-actions) Swift.
{: tip}

### Creazione e chiamata di un'azione
{: #openwhisk_actions_swift_invoke}

#### Swift 3
{: #openwhisk_actions_swift3_invoke}

Un'azione è semplicemente una funzione Swift di livello superiore. Per creare un'azione Swift 3:

1. Salva il seguente codice in un file denominato `hello.swift`.
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

    In questo esempio, l'azione Swift consuma un dizionario e ne produce un altro.

2. Crea un'azione denominata `helloSwift`.
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

    La CLI deduce automaticamente il tipo di azione dall'estensione del file di origine. Per i file di origine `.php`, l'azione viene eseguita utilizzando un runtime PHP 7.1. Per ulteriori informazioni, vedi il [riferimento](./openwhisk_reference.html#openwhisk_ref_php) per PHP.

3. Richiama l'azione.
    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Output di esempio:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

#### Swift 4
{: #openwhisk_actions_swift4_invoke}

Oltre alla firma della funzione principale, Swift 4 fornisce altre due firme che si avvalgono del tipo [Codable ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.apple.com/documentation/swift/codable). Puoi saperne di più sui tipi di dati codificabili e decodificabili per la compatibilità con rappresentazioni esterne quali JSON [qui ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types).

1. Salva il seguente codice in un file denominato `hello.swift`.
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

2. Crea un'azione denominata `helloSwift`.
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.1
    ```
    {: pre}

3. Richiama l'azione.
    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Output di esempio:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Assemblaggio di un'azione come eseguibile Swift
{: #packaging-an-action-as-a-swift-executable}

Quando crei un'azione {{site.data.keyword.openwhisk_short}} Swift con un file di origine Swift, il file deve essere compilato in un file binario prima che l'azione venga eseguita. Questo ritardo è noto come ritardo di avvio a freddo. Una volta che il file binario è stato creato, le successive chiamate all'azione sono molto più veloci finché il contenitore che include l'azione non viene eliminato. Per evitare il ritardo di avvio a freddo, puoi compilare il file Swift in un file binario e quindi caricarlo in {{site.data.keyword.openwhisk_short}} in un file zip.

Puoi utilizzare uno script per automatizzare la creazione del pacchetto dell'azione.

**Prerequisito**: lo script utilizzato nella seguente procedura presuppone che tu disponga di una directory denominata `actions`, con ogni directory di livello superiore che rappresenta un'azione.
```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```

1. Salva il seguente codice in un file script denominato `compile.sh`.
    ```bash
    #!/bin/bash
    set -ex

    if [ -z "$1" ] ; then
    echo 'Error: Missing action name'
    exit 1
fi
if [ -z "$2" ] ; then
    echo 'Error: Missing kind, for example swift:4.1'
        exit 2
    fi
    OUTPUT_DIR="build"
    if [ ${2} == "swift:3.1.1" ]; then
      BASE_PATH="/swift3Action"
      DEST_SOURCE="$BASE_PATH/spm-build"
      RUNTIME="openwhisk/action-swift-v3.1.1"
    elif [ ${2} == "swift:4.1" ]; then
      RUNTIME="ibmfunctions/action-swift-v4.1"
      BASE_PATH="/swift4Action"
      DEST_SOURCE="/$BASE_PATH/spm-build/Sources/Action"
    else
      echo "Error: Kind $2 not recognize"
      exit 3
    fi
    DEST_PACKAGE_SWIFT="$BASE_PATH/spm-build/Package.swift"

    BUILD_FLAGS=""
    if [ -n "$3" ] ; then
        BUILD_FLAGS=${3}
    fi

    echo "Using runtime $RUNTIME to compile swift"
docker run --rm --name=compile-ow-swift -it -v "$(pwd):/owexec" $RUNTIME bash -ex -c "

    if [ -f \"/owexec/$OUTPUT_DIR/$1.zip\" ] ; then
    rm \"/owexec/$OUTPUT_DIR/$1.zip\"
fi

    echo 'Setting up build...'
    cp /owexec/actions/$1/Sources/*.swift $DEST_SOURCE/

    # action file can be either {action name}.swift or main.swift
if [ -f \"$DEST_SOURCE/$1.swift\" ] ; then
    echo 'renaming $DEST_SOURCE/$1.swift $DEST_SOURCE/main.swift'
    mv \"$DEST_SOURCE/$1.swift\" $DEST_SOURCE/main.swift
fi
# Add in the OW specific bits
cat $BASE_PATH/epilogue.swift >> $DEST_SOURCE/main.swift
echo '_run_main(mainFunction:main)' >> $DEST_SOURCE/main.swift

    # Only for Swift4
    if [ ${2} != "swift:3.1.1" ]; then
      echo 'Adding wait to deal with escaping'
      echo '_ = _whisk_semaphore.wait(timeout: .distantFuture)' >> $DEST_SOURCE/main.swift
    fi

    echo \"Compiling $1...\"
    cd /$BASE_PATH/spm-build
cp /owexec/actions/$1/Package.swift $DEST_PACKAGE_SWIFT
# we have our own Package.swift, do a full compile
swift build ${BUILD_FLAGS} -c release

    echo 'Creating archive $1.zip...'
    #.build/release/Action
mkdir -p /owexec/$OUTPUT_DIR
zip \"/owexec/$OUTPUT_DIR/$1.zip\" .build/release/Action
"
    ```
    {: codeblock}

2. Per aggiungere dipendenze, crea il file `Package.swift`. Il seguente esempio aggiunge `example-package-deckofplayingcards` come una dipendenza. `CCurl`, `Kitura-net` e `SwiftyJSON` sono forniti nell'azione Swift standard, pertanto devi includerli nel tuo `Package.swift` solo per le azioni di Swift 3.
    * Sintassi di esempio per Swift 3:
        ```swift
        import PackageDescription

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
        {: codeblock}

    * Sintassi di esempio per Swift 4:
        ```swift
        // swift-tools-version:4.0
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
              .package(url: "https://github.com/apple/example-package-deckofplayingcards.git", .upToNextMajor(from: "3.0.0"))
            ],
      targets: [
              .target(
          name: "Action",
          dependencies: ["DeckOfPlayingCards"],
          path: "."
        )
            ]
        )
        ```
        {: codeblock}

3. Per creare uno `hello.zip` nel `build`, crea l'azione.
    * Swift 3:
      ```
      bash compile.sh hello swift:3.1.1
      ```
      {: pre}

    * Swift 4:
      ```
      bash compile.sh hello swift:4.1
      ```
      {: pre}

4. Carica lo zip in {{site.data.keyword.openwhisk_short}} con il nome azione `helloSwiftly`.
    * Swift 3:
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:3.1.1
      ```
      {: pre}

    * Swift 4:
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:4.1
      ```
      {: pre}

5. Richiama l'azione.
    ```
    ibmcloud fn action invoke helloSwiftly --blocking
    ```
    {: pre}

    Il tempo impiegato per l'esecuzione dell'azione è nella proprietà `duration".

6. Puoi confrontare la durata della chiamata dell'azione precompilata con la durata di una chiamata del comando con un passo di compilazione. Richiama l'azione dalla sezione precedente:
    ```
    ibmcloud fn action invoke --result helloSwift --param name World --blocking
    ```
    {: pre}

### Gestione degli errori in Swift 4
{: #error-handling-swift4}

Utilizzando il gestore di completamento Codable, puoi passare un errore per indicare un malfunzionamento nella tua azione. [Gestione degli errori in Swift ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) è simile alla gestione delle eccezioni in altri linguaggi, con l'utilizzo delle parole chiave `try`, `catch` e `throw`.

Il seguente frammento di codice mostra un esempio di gestione di un errore:
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

## Creazione di azioni Java
{: #creating-java-actions}

Le seguenti sezioni ti guidano nella creazione e la chiamata di una singola azione Java e l'aggiunta di parametri a tale azione.

Per compilare, testare ed archiviare i file Java, devi avere [JDK 8 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://openjdk.java.net/install) installato localmente.

### Creazione e chiamata di un'azione Java
{: #openwhisk_actions_java_invoke}

Un'azione Java è un programma Java con un metodo denominato `main`. `main` deve avere la seguente firma esatta:
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Per creare un'azione Java:

1. Salva il seguente codice in un file denominato `Hello.java`.
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

2. Compila `Hello.java` in un file JAR denominato `hello.jar`. **Nota**: [google-gson ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/google/gson) deve esistere nella tua CLASSPATH Java.
    ```
    javac Hello.java
    ```
    {: pre}
    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}

3. Crea un'azione.
    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}
  * Devi specificare il nome della classe principale utilizzando `--main`. Una classe principale idonea è una classe che implementa un metodo `main` statico. Se la classe non si trova nel pacchetto predefinito, utilizza il nome classe Java completo, ad esempio, `--main com.example.MyMain`.
  * Puoi personalizzare il nome del metodo della tua azione Java. Per farlo, specifica il nome del metodo completo della tua azione, ad esempio, `--main com.example.MyMain#methodName`.
  * La CLI deduce automaticamente il tipo di azione dall'estensione del file di origine.

4. Richiama l'azione.
    ```
    ibmcloud fn action invoke --result helloJava --param name World
    ```
    {: pre}

    Output di esempio:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Creazione di azioni Docker
{: #creating-docker-actions}

Le azioni {{site.data.keyword.openwhisk_short}} Docker ti permettono di scrivere le tue azioni in qualsiasi linguaggio.
{: shortdesc}

Il tuo codice viene compilato in un file binario eseguibile e incorporato in un'immagine Docker. Il programma binario interagisce con il sistema, prendendo l'input proveniente da `stdin` e rispondendo tramite `stdout`.   Per ulteriori informazioni sulla creazione di azioni Docker, vedi la sezione [Riferimenti](./openwhisk_reference.html#openwhisk_ref_docker).

Prerequisito: devi disporre di un account Docker Hub. Configura un ID e un account Docker gratuiti su [Docker Hub ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://hub.docker.com).

Per configurare un file binario personalizzato e utilizzare l'immagine Docker caricata come un'azione:

1. Scarica e installa la struttura di base Docker. La struttura di base è un template del contenitore Docker in cui puoi inserire il tuo codice sotto forma di file binari personalizzati.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Configura il tuo file binario personalizzato nella struttura di base black box. La struttura di base include un programma C che puoi utilizzare. Parte del file `example.c` viene compilata nell'ambito del processo di creazione dell'immagine Docker, quindi non hai bisogno di avere C compilato sulla tua macchina.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  Output di esempio:
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. Facoltativo: aggiungi codice e dipendenze aggiuntivi all'immagine Docker modificando il `Dockerfile` per creare il tuo eseguibile. Nota i seguenti requisiti e limiti:
  * Il binario deve trovarsi all'interno del contenitore in `/action/exec`.
  * L'eseguibile riceve un singolo argomento dalla riga di comando. Questo argomento è una serializzazione di stringa dell'oggetto JSON che rappresenta gli argomenti per l'azione.
  * Il programma può accedere a `stdout` o `stderr`.
  * Per convenzione, l'ultima riga dell'output deve essere un oggetto JSON in stringhe che rappresenta il risultato dell'azione.

4. Crea l'immagine Docker e caricala attraverso uno script fornito.
    1. Esegui l'accesso a Docker.
        ```
        docker login -u <nomeutente> -p <password>
        ```
        {: pre}

    2. Vai alla directory `dockerSkeleton`.
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. Esegui lo script.
        ```
        ./buildAndPush.sh <nomeutente>/blackboxdemo
        ```
        {: pre}

5. Usa il tuo contenitore Docker per creare un'azione.
    ```
    ibmcloud fn action create example --docker <nomeutente>/blackboxdemo
    ```
    {: pre}

6. Richiama l'azione.
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    Output di esempio:
    ```
    {
        "args": {
            "payload": "Rey"
        },
      "msg": "Hello from arbitrary C program!"
  }
    ```
    {: screen}

7. Per aggiornare l'azione Docker, carica l'immagine più recente in Docker Hub. Ciò consente al sistema di estrarre la tua nuova immagine Docker la prossima volta che eseguirà il codice per la tua azione.
    ```
    ./buildAndPush.sh <nomeutente>/blackboxdemo
    ```
    {: pre}

8. Se è presente un contenitore caldo che utilizza una versione precedente della tua immagine Docker, qualsiasi nuova chiamata continuerà a utilizzare tale immagine. Aggiorna l'azione in modo che le nuove chiamate inizino a usare la nuova immagine.
    ```
    ibmcloud fn action update example --docker <nomeutente>/blackboxdemo
    ```
    {: pre}

9. Facoltativo: puoi utilizzare l'argomento `--native` come abbreviazione per `--docker openwhisk/dockerskeleton`. Questo argomento rende più semplice creare e distribuire gli eseguibili che vengono eseguiti all'interno dell'SDK di azione Docker standard.
    1. La procedura sopra indicata crea un eseguibile binario all'interno del contenitore che si trova in `/action/exec`. Copia il file `/action/exec` nel tuo file system locale e comprimilo in `exec.zip`.
    2. Crea un'azione Docker che riceve l'eseguibile come dati di inizializzazione. L'argomento `--native` sostituisce l'argomento `--docker openwhisk/dockerskeleton`.
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}

## Creazione di azioni Go
{: #creating-go-actions}

L'argomento `--native` ti consente di assemblare qualsiasi eseguibile Go come un'azione.

Nota i seguenti requisiti e limiti.
  * L'eseguibile Go riceve un singolo argomento dalla riga di comando. L'argomento è una serializzazione di stringa dell'oggetto JSON che rappresenta gli argomenti per l'azione. 
  * Il programma può accedere a `stdout` o `stderr`.
  * Per convenzione, l'ultima riga dell'output deve essere un oggetto JSON in stringhe che rappresenta il risultato dell'azione.

Per creare un'azione Go:

1. Salva il seguente codice in un file denominato `sample.go`.
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
    {: codeblock}

2. Compila in modo incrociato `sample.go` per {{site.data.keyword.openwhisk_short}}. L'eseguibile deve essere denominato `exec`.
    ```bash
    GOOS=linux GOARCH=amd64 go build -o exec
    zip exec.zip exec
    ibmcloud fn action create helloGo --native exec.zip
    ```
    {: codeblock}

3. Richiama l'azione.
    ```bash
    ibmcloud fn action invoke helloGo -r -p name gopher
    {
        "msg": "Hello, gopher!"
    }
    ```
    {: pre}

## Creazione di sequenze di azioni
{: #openwhisk_create_action_sequence}

Puoi creare un'azione che concatena una sequenza di azioni. Il risultato di un'azione viene passato come argomento all'azione successiva.
{: shortdesc}

Nel pacchetto `/whisk.system/utils` vengono fornite varie azioni di programmi di utilità che puoi utilizzare per creare la tua prima sequenza.

1. Elenca le azioni nel pacchetto `/whisk.system/utils`.
    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    Output di esempio:
    ```
    package /whisk.system/utils: Mattoni che formattano e assemblano i dati
   action /whisk.system/utils/head: Estrae il prefisso di un array
   action /whisk.system/utils/split: Suddivide una stringa in un array
   action /whisk.system/utils/sort: Ordina un array
   action /whisk.system/utils/echo: Restituisce l'input
   action /whisk.system/utils/date: Data e ora corrente
   action /whisk.system/utils/cat: Concatena l'input in una stringa
    ```
    {: screen}

2. Utilizzando le azioni `split` and `sort`, crea una sequenza di azioni in modo che il risultato di `split` sia passato come un argomento a `sort`. Questa sequenza di azioni converte alcune righe di testo in array e ordina le righe.
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. Richiama l'azione.
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    Nell'output, le righe di suddivisione sono ordinate alfabeticamente.
    ```
    {
        "length": 3,
      "lines": [
            "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
    }
    ```
    {: screen}

**Nota**:
* I parametri che vengono passati tra le azioni nella sequenza sono espliciti, ad eccezione dei parametri predefiniti. Pertanto, i parametri passati alla sequenza di azioni sono disponibili solo per la prima azione nella sequenza. Il risultato della prima azione della sequenza diventa l'oggetto JSON di input per la seconda azione della sequenza e così via. Questo oggetto non include nessuno dei parametri che erano stati originariamente passati alla sequenza, a meno che la prima azione non li includa esplicitamente nel suo risultato. I parametri passati a un'azione vengono uniti ai parametri predefiniti dell'azione. I parametri passati hanno la precedenza e sostituiscono tutti i parametri predefiniti corrispondenti. Per ulteriori informazioni sulla chiamata delle sequenze di azioni con più parametri denominati, vedi [Impostazione dei parametri predefiniti su un'azione](./parameters.html#default-params-action).
* Una sequenza non ha un timeout generale separato dai timeout di ciascuna azione all'interno della sequenza. Poiché una sequenza è una pipeline di operazioni, un errore in un'azione interrompe la pipeline. Se si verifica un timeout di una singola azione, l'intera sequenza viene chiusa con tale errore.

## Gestione di azioni di grandi dimensioni
{: #large-app-support}

La dimensione massima del codice per un'azione è 48 MB. Le applicazioni che contengono molti moduli di terze parti, librerie native o strumenti esterni potrebbero incorrere in questo limite. Se crei un'azione di pacchetto .zip o .jar più grande di 48 MB, devi estendere l'immagine di runtime con le dipendenze e utilizzare quindi un singolo file di origine o un archivio più piccolo di 48 MB.

Ad esempio, creando un runtime Docker personalizzato, che include le librerie condivise necessarie, la presenza delle dipendenze non è richiesta nel file di archivio. I file di origine privati possono ancora essere raggruppati nell'archivio e inseriti in fase di runtime.

### Riduzione della dimensione delle azioni
{: #large-app-reduce}

Per ridurre la dimensione del codice di un'applicazione Python:

1. Inserisci la libreria `opencv-python` in `opencv`.
2. Installa il file binario opencv nell'immagine del sistema operativo.
3. Aumenta l'immagine con ulteriori librerie Python eseguendo `pip install requirements.txt`.
4. Utilizza `action.py` con la nuova immagine.

Per ridurre la dimensione del codice di un'applicazione Node.js:

1. Installa `opencv`.
   ```
   npm install opencv
   ```
   {: pre}

2. Installa `package.json`.
   ```
   npm install package.json
   ```
   {: pre}

3. Utilizza `action.js` con la nuova immagine.
