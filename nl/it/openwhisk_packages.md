---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Crea e utilizza i pacchetti
{: #openwhisk_packages}

In {{site.data.keyword.openwhisk}}, puoi utilizzare i pacchetti per raggruppare una serie di azioni correlate e condividerle con altri.
{: shortdesc}

Un pacchetto può includere *azioni* e *feed*.
- Un'azione è una parte di codice eseguita su {{site.data.keyword.openwhisk_short}}. Ad esempio, il pacchetto Cloudant include azioni per la lettura e scrittura di record in un database Cloudant.
- Un feed è utilizzato per configurare un'origine eventi esterna per l'attivazione di eventi trigger. Ad esempio, il pacchetto Allarme include un feed che può attivare un trigger a una frequenza specificata.

Ogni entità {{site.data.keyword.openwhisk_short}}, inclusi i pacchetti, appartiene a uno *spazio dei nomi* e il nome completo di un'entità è `/nomeSpazioNomi[/nomePacchetto]/nomeEntità`. Per ulteriori informazioni, vedi le [linee guida per la denominazione](./openwhisk_reference.html#openwhisk_entities).

Le seguenti sezioni descrivono come esplorare i pacchetti e utilizzare i trigger e i feed in essi contenuti. Inoltre, se sei interessato a portare i tuoi pacchetti nel catalogo, leggi le sezioni sulla creazione e la condivisione dei pacchetti.

## Esplora i pacchetti
{: #browse-packages}

In {{site.data.keyword.openwhisk_short}} sono registrati vari pacchetti. Puoi ottenere un elenco dei pacchetti di uno spazio dei nomi, elencare le entità di un pacchetto e ottenere una descrizione delle singole entità di un pacchetto.

1. Ottieni un elenco dei pacchetti dello spazio dei nomi `/whisk.system`.

  ```
  wsk package list /whisk.system
  ```
  {: pre}
  ```
  packages
  /whisk.system/cloudant                                                 shared
  /whisk.system/alarms                                                   shared
  /whisk.system/watson                                                   shared
  /whisk.system/websocket                                                shared
  /whisk.system/weather                                                  shared
  /whisk.system/system                                                   shared
  /whisk.system/utils                                                    shared
  /whisk.system/slack                                                    shared
  /whisk.system/samples                                                  shared
  /whisk.system/github                                                   shared
  /whisk.system/pushnotifications                                        shared
  ```

2. Ottieni un elenco delle entità del pacchetto `/whisk.system/cloudant`.

  ```
  wsk package get --summary /whisk.system/cloudant
  ```
  {: pre}
  ```
  package /whisk.system/cloudant: Cloudant database service
     (params: {{site.data.keyword.Bluemix_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```

  Questo output mostra che il pacchetto Cloudant fornisce due azioni, `read` e `write` e un solo feed trigger denominato `changes`. Il feed `changes` provoca l'attivazione dei trigger quando si aggiungo documenti al database Cloudant specificato. 

  Il pacchetto Cloudant definisce, inoltre, i parametri `username`, `password`, `host` e `port`. Affinché le azioni e i feed siano significativi, è necessario specificare questi parametri. Ad esempio, i parametri consentono alle azioni di operare su un account Cloudant specifico.

3. Ottieni una descrizione dell'azione `/whisk.system/cloudant/read`.
  ```
  wsk action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```

  Questo output mostra che l'azione `read` di Cloudant richiede tre parametri, compreso il database e l'ID documento da richiamare.


## Richiama le azioni in un pacchetto
{: #openwhisk_package_invoke}

Puoi richiamare le azioni in un pacchetto con la stessa modalità seguita per le altre azioni. I prossimi passi ti mostrano come richiamare l'azione `greeting` nel pacchetto `/whisk.system/samples` con parametri diversi.

1. Ottieni una descrizione dell'azione `/whisk.system/samples/greeting`.
  ```
  wsk action get --summary /whisk.system/samples/greeting
  ```
  {: pre}
  
  ```
  action /whisk.system/samples/greeting: Print a friendly greeting
     (params: name place)
  ```

  Nota che l'azione `greeting` accetta due parametri: `name` e `place`.

2. Richiama l'azione senza alcun parametro.
  ```
  wsk action invoke --blocking --result /whisk.system/samples/greeting
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, stranger from somewhere!"
  }
  ```

  L'output è un messaggio generico perché non è stato specificato alcun parametro.

3. Richiama l'azione con i parametri.
  ```
  wsk action invoke --blocking --result /whisk.system/samples/greeting --param name Mork --param place Ork
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Mork from Ork!"
  }
  ```

  Nota che l'output utilizza i parametri `name` e `place` che sono stati passati all'azione.


## Crea e utilizza i bind di pacchetto
{: #openwhisk_package_bind}

Sebbene tu possa usare direttamente le entità in un pacchetto, potresti ritrovarti a passare all'azione ogni volta gli stessi parametri. Puoi semplificare il processo eseguendo il bind di un pacchetto e specificando i parametri predefiniti, che vengono ereditati dalle azioni nel pacchetto.

Ad esempio, nel pacchetto `/whisk.system/cloudant`, puoi impostare i valori predefiniti `username`, `password` e `dbname` in un bind di pacchetto e questi valori verranno passati automaticamente a qualsiasi azione nel pacchetto.

Nel semplice esempio di seguito riportato, puoi eseguire il bind del pacchetto `/whisk.system/samples`.

1. Esegui il bind del pacchetto `/whisk.system/samples` e importa un valore predefinito per il parametro `place`.

  ```
  wsk package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}
  ```
  ok: created binding valhallaSamples
  ```

2. Ottieni una descrizione del bind di pacchetto.

  ```
  wsk package get --summary valhallaSamples
  ```
  {: pre}
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Returns a friendly greeting
   action /myNamespace/valhallaSamples/wordCount: Count words in a string
   action /myNamespace/valhallaSamples/helloWorld: Demonstrates logging facilities
   action /myNamespace/valhallaSamples/curl: Curl a host url
  ```

  Nota che tutte le azioni nel pacchetto `/whisk.system/samples` sono disponibili nel bind di pacchetto `valhallaSamples`.

3. Richiama un'azione nel bind di pacchetto.

  ```
  wsk action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```

  Dal risultato, puoi notare che l'azione eredita il parametro `place` che hai impostato durante la creazione del bind di pacchetto `valhallaSamples`.

4. Richiama un'azione e sovrascrivi il valore predefinito del parametro.
  ```
  wsk action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```

  Nota che il valore del parametro `place` specificato con la chiamata dell'azione sovrascrive il valore predefinito impostato nel bind di pacchetto `valhallaSamples`.


## Crea e utilizza i feed di trigger
{: #openwhisk_package_trigger}

I feed possono essere opportunamente utilizzati per configurare un'origine eventi esterna per l'attivazione di questi eventi in un trigger {{site.data.keyword.openwhisk_short}}. Questo esempio mostra come utilizzare un feed nel pacchetto Allarmi per attivare un trigger e come utilizzare una regola per richiamare un'azione ogni secondo.

1. Ottieni una descrizione del feed nel pacchetto `/whisk.system/alarms`.
  ```
  wsk package get --summary /whisk.system/alarms
  ```
  {: pre}

  ```
  package /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
  ```

  ```
  wsk action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  ```
  action /whisk.system/alarms/alarm: Fire Trigger when alarm occurs
     (params: cron trigger_payload)
  ```

  Il feed `/whisk.system/alarms/alarm` utilizza due parametri:
  - `cron`: una specifica crontab che indica quando attivare il trigger.
  - `trigger_payload`: il valore del parametro payload da impostare in ogni evento trigger.

2. Crea un trigger che si attiva ogni 8 secondi.
  ```
  wsk trigger create everyEightSeconds --feed /whisk.system/alarms/alarm -p cron "*/8 * * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  ```
  ok: created trigger feed everyEightSeconds
  ```

3. Crea un file 'hello.js' con il seguente codice azione.
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. Assicurati che l'azione esista.
  ```
  wsk action update hello hello.js
  ```
  {: pre}

5. Crea una regola che richiama l'azione `hello` ogni volta che il trigger `everyEightSeconds` viene attivato.
  ```
  wsk rule create myRule everyEightSeconds hello
  ```
  {: pre}
  ```
  ok: created rule myRule
  ```

6. Controlla che l'azione venga richiamata tramite il polling dei log di attivazione.

  ```
  wsk activation poll
  ```
  {: pre}

  Puoi vedere che le attivazioni vengono osservate ogni 8 secondi per il trigger, la regola e l'azione. L'azione riceve i parametri `{"name":"Mork", "place":"Ork"}` ad ogni chiamata.


## Crea un pacchetto
{: #openwhisk_packages_create}

Un pacchetto viene utilizzato per organizzare una serie di azioni e feed correlati.
Inoltre, consente la condivisione dei parametri tra tutte le entità del pacchetto.

Per creare un pacchetto personalizzato con una semplice azione al suo interno, prova il seguente esempio:

1. Crea un pacchetto denominato "custom".
  ```
  wsk package create custom
  ```
  {: pre}

  ```
  ok: created package custom
  ```

2. Ottieni un riepilogo del pacchetto.
  ```
  wsk package get --summary custom
  ```
  {: pre}

  ```
  package /myNamespace/custom
  ```

  Nota che il pacchetto è vuoto.

3. Crea un file denominato `identity.js` contenente il seguente codice azione. Questa azione restituisce tutti i parametri di input.
  ```javascript
  function main(args) { return args; }
  ```
  {: codeblock}

4. Crea un'azione `identity` nel pacchetto `custom`.
  ```
  wsk action create custom/identity identity.js
  ```
  {: pre}
  
  ```
  ok: created action custom/identity
  ```

  La creazione di un'azione in un pacchetto richiede di aggiungere il nome pacchetto come prefisso per il nome azione. La nidificazione dei pacchetti non è consentita. Un pacchetto può contenere solo azioni e non può contenere altri pacchetti.

5. Ottieni nuovamente un riepilogo del pacchetto.
  ```
  wsk package get --summary custom
  ```
  {: pre}

  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```

  Adesso puoi vedere l'azione `custom/identity` nel tuo spazio dei nomi.

6. Richiama l'azione nel pacchetto.
  ```
  wsk action invoke --blocking --result custom/identity
  ```
  {: pre}

  ```json
  {}
  ```


Puoi impostare parametri predefiniti per tutte le entità in un pacchetto impostando parametri a livello di pacchetto che vengono ereditati da tutte le azioni nel pacchetto. Per vedere come funziona questa eredità, prova il seguente esempio:

1. Aggiorna il pacchetto `custom` con due parametri: `city` e `country`.
  ```
  wsk package update custom --param city Austin --param country USA
  ```
  {: pre}

  ```
  ok: updated package custom
  ```

2. Visualizza i parametri del pacchetto e dell'azione e osserva in che modo l'azione `identity` eredita parametri dal pacchetto.
  ```
  wsk package get custom parameters
  ```
  {: pre}

  ```
  ok: got package custom, displaying field parameters
  ```

  ```json
  [
      {
          "key": "city",
          "value": "Austin"
      },
      {
          "key": "country",
          "value": "USA"
      }
  ]
  ```

  ```
  wsk action get custom/identity parameters
  ```
  {: pre}

  ```
  ok: got action custom/identity, , displaying field parameters
  ```

  ```json
  [
      {
          "key": "city",
          "value": "Austin"
      },
      {
          "key": "country",
          "value": "USA"
      }
  ]
  ```

3. Richiama l'azione identity senza alcun parametro, al fine di verificare che l'azione erediti effettivamente i parametri.

  ```
  wsk action invoke --blocking --result custom/identity
  ```
  {: pre}
  ```json
  {
      "city": "Austin",
      "country": "USA"
  }
  ```

4. Richiama l'azione identity con alcuni parametri. I parametri della chiamata vengono uniti ai parametri del pacchetto e li sostituiscono.
  ```
  wsk action invoke --blocking --result custom/identity --param city Dallas --param state Texas
  ```
  {: pre}

  ```json
  {
      "city": "Dallas",
      "country": "USA",
      "state": "Texas"
  }
  ```

## Condividi un pacchetto
{: #openwhisk_packages_share}

Una volta eseguito il test e il debug delle azioni e dei feed compresi in un pacchetto, quest'ultimo può essere condiviso con tutti gli utenti {{site.data.keyword.openwhisk_short}}. La condivisione del pacchetto consente agli utenti di eseguire il bind del pacchetto, richiamare azioni nel pacchetto e creare regole e azioni di sequenza {{site.data.keyword.openwhisk_short}}.

1. Condividi i pacchetti con tutti gli utenti:
  ```
  wsk package update custom --shared yes
  ```
  {: pre}

  ```
  ok: updated package custom
  ```

2. Visualizza la proprietà `publish` del pacchetto per verificare che ora sia true.
  ```
  wsk package get custom publish
  ```
  {: pre}

  ```
  ok: got package custom, displaying field publish
  ```

  ```json
  true
  ```


Altri utenti possono ora utilizzare il tuo pacchetto `custom`, incluso l'esecuzione del bind di pacchetto o il richiamo diretto di un'azione al suo interno. Per eseguire il bind o richiamare le azioni, è necessario che gli altri utenti conoscano i nomi completi del pacchetto. Le azioni e i feed di un pacchetto condiviso sono _pubblici_. Se il pacchetto è privato, lo sono anche tutti i suoi contenuti.

1. Ottieni una descrizione del pacchetto per visualizzare i nomi completi del pacchetto e dell'azione.

  ```
  wsk package get --summary custom
  ```
  {: pre}
  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```

  Nell'esempio precedente, hai utilizzato lo spazio dei nomi `myNamespace`, che appare nel nome completo.
