---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-09"

keywords: parameters, passing, invocation, binding

subcollection: cloud-functions

---





{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Utilizzo dei parametri

Nelle azioni senza server, i dati vengono forniti aggiungendo parametri alle azioni. I parametri vengono dichiarati come un argomento alla funzione senza server principale.
{: shortdesc}

Puoi fornire dei valori per i parametri in due modi:
* **Passa i parametri a un'azione durante il richiamo**: fornisci i parametri quando viene richiamata l'azione tramite gli indicatori CLI o un file. I parametri forniti al richiamo sovrascrivono tutti i parametri predefiniti che erano stati precedentemente configurati.
* **Associa i parametri a un'azione o un pacchetto**: imposta i parametri predefiniti quando viene creato o aggiornato un pacchetto o un'azione. Questa opzione è utile per i dati che rimangono invariati a ogni esecuzione, equivalenti alle variabili di ambiente su altre piattaforme, o per i valori predefiniti che potrebbero essere sovrascritti in fase di chiamata.

## Passaggio dei parametri a un'azione durante la chiamata
{: #pass-params-action}

I parametri possono essere passati a un'azione quando viene richiamata.

1. Salva il seguente codice in un file denominato `hello.js`.

    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
    }
    ```
    {: codeblock}

2. Crea l'azione `hello`.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Se hai precedentemente utilizzato questa azione, aggiornala per assicurati che sia stata cancellata da tutti i parametri impostati precedentemente.
    ```
    ibmcloud fn action update hello hello.js
    ```
    {: pre}

4. Richiama l'azione passando i parametri `name` e `place`.
    ```
    ibmcloud fn action invoke --result hello --param name Dorothy --param place Kansas
    ```
    {: pre}

    **Nota**: puoi invece passare un file di parametri formattati JSON:
    ```
    ibmcloud fn action invoke --result hello --param-file parameters.json
    ```
    {: pre}

    Output di esempio:
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
    ```
    {: screen}

5. Puoi inoltre passare i parametri in un oggetto strutturato alla tua azione. Ad esempio, aggiorna l'azione `hello` nel seguente modo:
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
    ```
    {: codeblock}

    Ora l'azione prevede che un singolo parametro `person` contenga i campi `name` e `place`.

6. Richiama l'azione con un singolo parametro `person` che è un oggetto JSON valido.
    ```
    ibmcloud fn action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
    ```
    {: pre}

    Output di esempio:
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
    ```
    {: screen}

## Associazione dei parametri a un'azione
{: #default-params-action}

Le azioni possono essere richiamate con più parametri indicati. Ad esempio, l'azione `hello` di base prevede due parametri: il nome (`name`) di una persona e il luogo (`place`) da dove proviene.

```javascript
function main(params) {
   return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
```
{: screen}

Anziché passare ogni volta tutti i parametri a un'azione, puoi eseguire il bind dei parametri predefiniti all'azione. La seguente procedura mostra come eseguire il bind del parametro `place` all'azione `hello` di base in modo che assuma come valore predefinito il luogo "Kansas".

1. Salva il seguente codice in un file denominato `hello.js`.

    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
    }
    ```
    {: codeblock}

2. Crea l'azione `hello`.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Aggiorna l'azione per eseguire il bind dei valori utilizzando l'indicatore `--param` e una coppia chiave/valore.

    ```
    ibmcloud fn action update hello --param place Kansas
    ```
    {: pre}

    **Nota**: puoi invece passare un file di parametri formattati JSON:
    ```
    ibmcloud fn action update hello --param-file parameters.json
    ```
    {: pre}

    Se modifichi i parametri delle tue credenziali non di servizio, l'esecuzione di un comando `action update` con i nuovi parametri rimuove qualsiasi parametro attualmente esistente ma non specificato nel comando `action update`. Ad esempio, se esegui `action update -p key1 new-value -p key2 new-value` ma ometti tutti gli altri parametri che erano stati impostati, quei parametri non esisteranno più dopo l'aggiornamento dell'azione. Vengono rimossi anche tutti i servizi associati all'azione, quindi dopo aver aggiornato gli altri parametri devi eseguire di nuovo il [bind dei servizi alla tua azione](/docs/openwhisk?topic=cloud-functions-binding_services).
    {: tip}

4. Richiama l'azione passando solo il parametro `name`.
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

    Poiché non hai specificato il parametro `place` quando hai richiamato l'azione, viene utilizzato il valore del parametro predefinito associato, ossia `Kansas`.

5. I parametri associati possono essere sovrascritti specificando il valore del parametro al momento della chiamata. Richiama l'azione passando `name` e `place`.
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

## Associazione dei parametri a un pacchetto
{: #default-params-package}

I parametri predefiniti possono essere configurati anche al livello di pacchetto. I parametri associati fungono da parametri predefiniti per le azioni nel pacchetto a meno che:

- L'azione stessa ha un parametro predefinito
- L'azione ha un parametro che viene fornito in fase di richiamo

Il seguente esempio imposta un parametro predefinito di `name` sul pacchetto `MyApp` e mostra un'azione che lo utilizza.

1. Crea un pacchetto e associagli il parametro predefinito `name`.
    ```
    ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

2. Salva il seguente codice in un file denominato `helloworld.js`.

    ```javascript
       function main(params) {
           return {payload: "Hello, " + params.name};
     }
    ```
    {: codeblock}

3. Crea l'azione nel pacchetto `MyApp`.
    ```
    ibmcloud fn action update MyApp/hello helloworld.js
    ```
    {: pre}

    Se modifichi i parametri delle tue credenziali non di servizio, l'esecuzione di un comando `action update` con i nuovi parametri rimuove qualsiasi parametro attualmente esistente ma non specificato nel comando `action update`. Ad esempio, se esegui `action update -p key1 new-value -p key2 new-value` ma ometti tutti gli altri parametri che erano stati impostati, quei parametri non esisteranno più dopo l'aggiornamento dell'azione. Vengono rimossi anche tutti i servizi associati all'azione, quindi dopo aver aggiornato gli altri parametri devi eseguire di nuovo il [bind dei servizi alla tua azione](/docs/openwhisk?topic=cloud-functions-binding_services).
    {: tip}

3. Richiama l'azione senza passare alcun parametro.
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

    Viene utilizzato il parametro associato al pacchetto.

