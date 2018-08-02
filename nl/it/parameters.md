---

copyright:
  years: 2018
lastupdated: "2018-05-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Utilizzo dei parametri

Scopri come impostare i parametri su pacchetti e azioni per la distribuzione e come passare i parametri alle azioni durante la chiamata. Puoi anche utilizzare un file per memorizzare i parametri e passare il nome file all'azione, anziché fornire ogni parametro singolarmente sulla riga di comando.
{: shortdesc}

Con le azioni senza server, i dati vengono forniti aggiungendo parametri alle azioni, che vengono dichiarate come argomento della funzione senza server principale. Tutti i dati arrivano in questo modo e i valori possono essere impostati in diversi modi. La prima opzione è quella di fornire i parametri quando viene creata (o aggiornata) un'azione o un pacchetto. Questa opzione è utile per i dati che rimangono invariati a ogni esecuzione, equivalenti alle variabili di ambiente su altre piattaforme, o per i valori predefiniti che potrebbero essere sovrascritti in fase di chiamata. La seconda opzione è quella di fornire i parametri quando l'azione viene richiamata, che sovrascrive tutti i parametri precedentemente impostati.

## Passaggio dei parametri a un'azione durante la chiamata
{: #pass-params-action}

I parametri possono essere passati a un'azione quando viene richiamata. Gli esempi forniti utilizzano JavaScript, ma tutti gli altri linguaggi funzionano allo stesso modo. Per esempi più dettagliati, controlla i seguenti argomenti in [Azioni Javascript](./openwhisk_actions.html#creating-and-invoking-javascript-actions), [Azioni Swift](./openwhisk_actions.html#creating-swift-actions), [Azioni Python](./openwhisk_actions.html#creating-python-actions), [Azioni Java](./openwhisk_actions.html#creating-java-actions), [Azioni PHP](./openwhisk_actions.html#creating-php-actions), [Azioni Docker](./openwhisk_actions.html#creating-docker-actions) o [Azioni Go](./openwhisk_actions.html#creating-go-actions).

1. Utilizza i parametri nell'azione. Ad esempio, crea un file denominato **hello.js** con il seguente contenuto:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  I parametri di input vengono passati alla funzione **main** come parametro dell'oggetto JSON. Osserva in che modo i parametri `name` e `place` vengono richiamati dall'oggetto `params` in questo esempio.

2. Aggiorna l'azione **hello** in modo che si pronta per l'uso:
  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

  Se modifichi i parametri delle tue credenziali non di servizio, l'esecuzione di un comando `action update` con i nuovi parametri rimuove qualsiasi parametro attualmente esistente ma non specificato nel comando `action update`. Ad esempio, se esegui `action update -p key1 new-value -p key2 new-value` ma ometti tutti gli altri parametri che erano stati impostati, quei parametri non esisteranno più dopo l'aggiornamento dell'azione. Vengono rimossi anche tutti i servizi associati all'azione, quindi dopo aver aggiornato gli altri parametri devi eseguire di nuovo il [bind dei servizi alla tua azione](./binding_services.html).
  {: tip}

3. I parametri possono essere forniti esplicitamente utilizzando la riga di comando oppure [fornendo un file](./parameters.html#using-parameter-files) che contiene i parametri desiderati.

  Per passare i parametri direttamente tramite la riga di comando, fornisci una coppia chiave/valore all'indicatore `--param`:
  ```
  ibmcloud fn action invoke --result hello --param name Dorothy --param place Kansas
  ```
  {: pre}

  **Risposta:**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  Osserva l'uso dell'opzione `--result`: implica una chiamata bloccante in cui la CLI attende che l'attivazione sia completata e quindi visualizza solo il risultato. Per comodità, questa opzione può essere utilizzata senza `--blocking` che viene dedotto automaticamente.

  Inoltre, se i valori di parametro specificati sulla riga di comando sono JSON validi, vengono analizzati e inviati alla tua azione come oggetto strutturato.

  Ad esempio, aggiorna l'azione **hello** nel seguente modo:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  Ora l'azione prevede che un singolo parametro `person` contenga i campi `name` e `place`.

  Quindi, richiama l'azione con un singolo parametro `person` che è un JSON valido, come nel seguente esempio:
  ```
  ibmcloud fn action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
  ```
  {: pre}

  **Risposta:**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  Il risultato è lo stesso perché la CLI analizza automaticamente il valore del parametro `person` nell'oggetto strutturato ora previsto dall'azione.

## Impostazione dei parametri predefiniti su un'azione
{: #default-params-action}

Le azioni possono essere richiamate con più parametri indicati. Ricorda che l'azione **hello** dell'esempio precedente prevede due parametri: il *nome* di una persona e il *luogo* di provenienza.

Anziché passare ogni volta tutti i parametri a un'azione, puoi eseguire il bind di determinati parametri. Il seguente esempio esegue il bind del parametro *place* in modo che l'azione assuma come valore predefinito il luogo "Kansas":

1. Aggiorna l'azione utilizzando l'opzione  `--param` per eseguire il bind dei valori di parametro o passando un file che contiene i parametri a `--param-file`. (Per esempi che utilizzano i file, vedi la sezione sull'[utilizzo di file di parametri](./parameters.html#using-parameter-files).)

  Per specificare i parametri predefiniti esplicitamente nella riga di comando, fornisci una coppia chiave/valore all'indicatore `param`:
  ```
  ibmcloud fn action update hello --param place Kansas
  ```
  {: pre}

2. Richiama l'azione, questa volta passando solo il parametro `name`.
  ```
  ibmcloud fn action invoke --result hello --param name Dorothy
  ```
  {: pre}

  Output di esempio:
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  Nota che non hai dovuto specificare il parametro del luogo quando hai richiamato l'azione. I parametri associati possono ancora essere sovrascritti specificando il valore del parametro al momento della chiamata.

3. Richiama l'azione passando entrambi i valori `name` e `place` e osserva l'output:

  Richiama l'azione utilizzando l'indicatore `--param`:
  ```
  ibmcloud fn action invoke --result hello --param name Dorothy --param place "Washington, DC"
  ```
  {: pre}

  Output di esempio:
  ```
  {  
      "payload": "Hello, Dorothy from Washington, DC"
  }
  ```
  {: screen}

  I parametri impostati su un'azione quando questa viene creata o aggiornata sono sempre sovrascritti da un parametro che viene fornito direttamente durante la chiamata.
  {: tip}

## Impostazione dei parametri predefiniti su un pacchetto
{: #default-params-package}

I parametri possono essere impostati a livello di pacchetto e fungono da parametri predefiniti per le azioni _a meno che_ non si verifichi una delle seguenti condizioni:

- L'azione stessa ha un parametro predefinito.
- L'azione ha un parametro che viene fornito in fase di chiamata, che ha sempre la "priorità" quando è disponibile più di un parametro.

Il seguente esempio imposta un parametro predefinito di `name` sul pacchetto **MyApp** e mostra un'azione che lo utilizza.

1. Crea un pacchetto con un parametro impostato:

  ```
  ibmcloud fn package update MyApp --param name World
  ```
  {: pre}

2. Crea un'azione nel pacchetto **MyApp**:
  ```javascript
     function main(params) {
         return {payload: "Hello, " + params.name};
     }
  ```
  {: codeblock}

  Crea l'azione:
  ```
  ibmcloud fn action update MyApp/hello hello.js
  ```
  {: pre}

3. Richiama l'azione e osserva il parametro del pacchetto predefinito in uso:
  ```
  ibmcloud fn action invoke --result MyApp/hello
  ```
  {: pre}

  Output di esempio:
  ```
     {
         "payload": "Hello, World"
     }
  ```
  {: screen}

## Utilizzo di file di parametri
{: #using-parameter-files}

Puoi inserire parametri in un file in formato JSON e quindi passare i parametri fornendo il nome file con l'indicatore `--param-file`. Questo metodo può essere utilizzato per la creazione (o gli aggiornamenti) di pacchetti e azioni e durante la chiamata dell'azione.

1. Considera l'esempio **hello** precedente utilizzando `hello.js` con il seguente contenuto:

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. Aggiorna l'azione con i contenuti aggiornati di `hello.js`:

  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

3. Crea un file di parametri denominato `parameters.json` contenente parametri in formato JSON:

  ```json
  {
      "name": "Dorothy",
      "place": "Kansas"
  }
  ```
  {: codeblock}

4. Utilizza il nome file `parameters.json` durante la chiamata dell'azione **hello** e osserva l'output:

  ```
  ibmcloud fn action invoke --result hello --param-file parameters.json
  ```

  Output di esempio:
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}
