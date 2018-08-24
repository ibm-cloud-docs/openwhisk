---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Organizzazione delle azioni nei pacchetti
{: #openwhisk_packages}

In {{site.data.keyword.openwhisk}}, puoi utilizzare i pacchetti per raggruppare una serie di azioni correlate e condividerle con altri.
{: shortdesc}

Un pacchetto può includere *azioni* e *feed*.
- Un'azione è una parte di codice eseguita su {{site.data.keyword.openwhisk_short}}. Ad esempio, il pacchetto {{site.data.keyword.cloudant}} include azioni per la lettura e scrittura di record in un database {{site.data.keyword.cloudant_short_notm}}.
- Il feed serve a configurare un'origine eventi esterna per l'attivazione di eventi trigger. Ad esempio,il pacchetto Alarm include un feed che può attivare un trigger alla frequenza indicata.

Ogni entità {{site.data.keyword.openwhisk_short}}, inclusi i pacchetti, appartiene a uno *spazio dei nomi* e il nome completo di un'entità è `/nomeSpazioNomi[/nomePacchetto]/nomeEntità`. Per ulteriori informazioni, vedi le [linee guida per la denominazione](./openwhisk_reference.html#openwhisk_entities).

Le seguenti sezioni descrivono come navigare tra i pacchetti e utilizzare i trigger e i feed al loro interno. Inoltre, se sei interessato a portare i tuoi pacchetti nel catalogo, leggi le sezioni sulla creazione e la condivisione dei pacchetti.

## Esplora i pacchetti
{: #browse-packages}

In {{site.data.keyword.openwhisk_short}} sono registrati vari pacchetti. Puoi ottenere un elenco dei pacchetti di uno spazio dei nomi, elencare le entità di un pacchetto e ottenere una descrizione delle singole entità di un pacchetto.

1. Ottieni un elenco dei pacchetti dello spazio dei nomi `/whisk.system`.
  ```
  ibmcloud fn package list /whisk.system
  ```
  {: pre}

  Output dell'elenco dei pacchetti:
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
  {: screen}

2. Ottieni un elenco delle entità del pacchetto `/whisk.system/cloudant`.
  ```
  ibmcloud fn package get --summary /whisk.system/cloudant
  ```
  {: pre}

  Output di esempio:
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.Bluemix_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  Questo output mostra che il pacchetto {{site.data.keyword.cloudant_short_notm}} fornisce due azioni, `read` e `write` e un solo feed di trigger denominato `changes`. Il feed `changes` provoca l'attivazione dei trigger quando si aggiungono documenti al database {{site.data.keyword.cloudant_short_notm}} specificato.

  Il pacchetto {{site.data.keyword.cloudant_short_notm}} definisce, inoltre, i parametri `username`, `password`, `host` e `port`. Affinché le azioni e i feed siano significativi, è necessario specificare questi parametri. Ad esempio, i parametri consentono alle azioni di operare su un account {{site.data.keyword.cloudant_short_notm}} specifico.

3. Ottieni una descrizione dell'azione `/whisk.system/cloudant/read`.
  ```
  ibmcloud fn action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  Output di esempio:
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```
  {: screen}

  Questo output mostra che l'azione `read` di {{site.data.keyword.cloudant_short_notm}} richiede tre parametri, compreso il database e l'ID documento da richiamare.

## Richiama le azioni in un pacchetto
{: #openwhisk_package_invoke}

Puoi richiamare le azioni in un pacchetto con la stessa modalità utilizzata per le altre azioni. La seguente procedura mostra come chiamare l'azione `greeting` nel pacchetto `/whisk.system/samples` con parametri differenti.

1. Ottieni una descrizione dell'azione `/whisk.system/samples/greeting`.
  ```
  ibmcloud fn action get --summary /whisk.system/samples/greeting
  ```
  {: pre}

  Output di esempio:
  ```
  action /whisk.system/samples/greeting: Print a friendly greeting
     (params: name place)
  ```
  {: screen}

  Nota che l'azione `greeting` utilizza due parametri: `name` e `place`.

2. Richiama l'azione senza passare alcun parametro.
  ```
  ibmcloud fn action invoke --blocking --result /whisk.system/samples/greeting
  ```
  {: pre}

  Output di esempio:
  ```
  {
      "payload": "Hello, stranger from somewhere!"
  }
  ```
  {: screen}

  L'output è un messaggio generico perché non è stato specificato alcun parametro.

3. Richiama l'azione con i parametri.
  ```
  ibmcloud fn action invoke --blocking --result /whisk.system/samples/greeting --param name Mork --param place Ork
  ```
  {: pre}

  Output di esempio:
  ```
  {
      "payload": "Hello, Mork from Ork!"
  }
  ```
  {: screen}

  Nota che l'output utilizza i parametri `name` e `place` che erano stati passati all'azione.

## Crea e utilizza i bind di pacchetto
{: #openwhisk_package_bind}

Sebbene tu possa usare direttamente le entità di un pacchetto, potresti ritrovarti ogni volta a trasmettere all'azione gli stessi parametri. Puoi semplificare il processo eseguendo il bind a un pacchetto e specificando i parametri predefiniti, che vengono ereditati dalle azioni del pacchetto.

Ad esempio, nel pacchetto `/whisk.system/cloudant`, puoi impostare i valori `username`, `password` e `dbname` predefiniti in un bind di pacchetto, che verranno passati automaticamente a qualsiasi azione del pacchetto.

Nel semplice esempio di seguito riportato, puoi eseguire il bind del pacchetto `/whisk.system/samples`.

1. Esegui il bind del pacchetto `/whisk.system/samples` e importa un valore predefinito per il parametro `place`.
  ```
  ibmcloud fn package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  Output di esempio:
  ```
  ok: created binding valhallaSamples
  ```
  {: screen}

2. Ottieni una descrizione del bind di pacchetto.
  ```
  ibmcloud fn package get --summary valhallaSamples
  ```
  {: pre}

  Output di esempio:
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Returns a friendly greeting
   action /myNamespace/valhallaSamples/wordCount: Count words in a string
   action /myNamespace/valhallaSamples/helloWorld: Demonstrates logging facilities
   action /myNamespace/valhallaSamples/curl: Curl a host url
  ```
  {: screen}

  Nota che tutte le azioni del pacchetto `/whisk.system/samples` sono disponibili nel bind di pacchetto `valhallaSamples`.

3. Richiama un'azione nel bind di pacchetto.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  Output di esempio:
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  Dal risultato, puoi notare che l'azione eredita il parametro `place` che hai impostato durante la creazione del bind di pacchetto `valhallaSamples`.

4. Richiama un'azione e sovrascrivi il valore predefinito del parametro.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  Output di esempio:
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  Nota che il valore del parametro `place` specificato con la chiamata dell'azione sovrascrive il valore predefinito impostato nel bind di pacchetto `valhallaSamples`.

## Crea e utilizza i feed di trigger
{: #openwhisk_package_trigger}

I feed possono essere opportunamente utilizzati per configurare un'origine eventi esterna per l'attivazione di questi eventi in un trigger {{site.data.keyword.openwhisk_short}}. Questo esempio mostra come utilizzare un feed nel pacchetto Allarmi per attivare un trigger e come utilizzare una regola per chiamare un'azione ogni secondo.

1. Ottieni una descrizione del feed del pacchetto `/whisk.system/alarms`.
  ```
  ibmcloud fn package get --summary /whisk.system/alarms
  ```
  {: pre}

  Output di esempio:
  ```
  package /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
  ```
  {: screen}

  ```
  ibmcloud fn action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  Output di esempio:
  ```
  action /whisk.system/alarms/alarm: Fire trigger when alarm occurs
     (params: cron trigger_payload)
  ```
  {: screen}

  Il feed `/whisk.system/alarms/alarm` utilizza due parametri:
  - `cron`: una specifica crontab che indica quando attivare il trigger.
  - `trigger_payload`: il valore del parametro payload da impostare in ogni evento trigger.

2. Crea un trigger che si attiva ogni 8 secondi.
  ```
  ibmcloud fn trigger create everyEightSeconds --feed /whisk.system/alarms/alarm -p cron "*/8 * * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  Output di esempio:
  ```
  ok: created trigger feed everyEightSeconds
  ```
  {: screen}

3. Crea un file denominato **hello.js** con il seguente codice azione:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. Assicurati che l'azione esista.
  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

5. Crea una regola che richiama l'azione **hello** ogni volta che viene attivato il trigger `everyEightSeconds`.
  ```
  ibmcloud fn rule create myRule everyEightSeconds hello
  ```
  {: pre}

  Output di esempio:
  ```
  ok: created rule myRule
  ```
  {: screen}

6. Controlla che l'azione venga richiamata tramite il polling dei log di attivazione.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

  Puoi vedere che le attivazioni vengono osservate ogni 8 secondi per il trigger, la regola e l'azione. L'azione riceve i parametri `{"name":"Mork", "place":"Ork"}` ad ogni chiamata.

## Crea un pacchetto
{: #openwhisk_packages_create}

Un pacchetto serve a organizzare un insieme di feed e azioni correlati.
Inoltre, consente la condivisione dei parametri tra tutte le entità del pacchetto.

Per creare un pacchetto personalizzato che contenga un'azione semplice, prova il seguente esempio:

1. Crea un pacchetto denominato **custom**.
  ```
  ibmcloud fn package create custom
  ```
  {: pre}

  Output di esempio:
  ```
  ok: created package custom
  ```
  {: screen}

2. Ottieni un riepilogo del pacchetto.
  ```
  ibmcloud fn package get --summary custom
  ```
  {: pre}

  Output di esempio:
  ```
  package /myNamespace/custom
  ```
  {: screen}

  Nota che il pacchetto è vuoto.

3. Crea un file denominato `identity.js` contenente il seguente codice azione. Questa azione restituisce tutti i parametri di input.
  ```javascript
  function main(args) { return args; }
  ```
  {: codeblock}

4. Crea un'azione denominata **identity** nel pacchetto `custom`.
  ```
  ibmcloud fn action create custom/identity identity.js
  ```
  {: pre}

  Output di esempio:
  ```
  ok: created action custom/identity
  ```
  {: screen}

  Per creare un'azione in un pacchetto devi prefissare il nome dell'azione con un nome pacchetto. La nidificazione dei pacchetti non è consentita. Un pacchetto può contenere solo azioni e non può contenere un altro pacchetto.

5. Ottieni nuovamente un riepilogo del pacchetto.
  ```
  ibmcloud fn package get --summary custom
  ```
  {: pre}

  Output di esempio:
  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```
  {: screen}

  Adesso l'azione **custom/identity** viene mostrata nel tuo spazio dei nomi.

6. Richiama l'azione del pacchetto.
  ```
  ibmcloud fn action invoke --blocking --result custom/identity
  ```
  {: pre}

  Output di esempio:
  ```
  {}
  ```
  {: screen}

Puoi impostare parametri predefiniti per tutte le entità di un pacchetto impostando parametri a livello di pacchetto che vengono ereditati da tutte le azioni nel pacchetto. Per vedere come funziona questa eredità, prova il seguente esempio:

1. Aggiorna il pacchetto **custom** con due parametri: `city` e `country`.
  ```
  ibmcloud fn package update custom --param city Austin --param country USA
  ```
  {: pre}

  Output di esempio:
  ```
  ok: updated package custom
  ```
  {: screen}

2. Visualizza i parametri del pacchetto **custom** e dell'azione **identidy** e osserva in che modo l'azione **identity** eredita parametri dal pacchetto.
  ```
  ibmcloud fn package get custom parameters
  ```
  {: pre}

  Output di esempio:
  ```
  ok: got package custom, displaying field parameters

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
  {: screen}

  ```
  ibmcloud fn action get custom/identity parameters
  ```
  {: pre}

  Output di esempio:
  ```
  ok: got action custom/identity, displaying field parameters

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
  {: screen}

3. Richiama l'azione **identity** senza alcun parametro, al fine di verificare che l'azione erediti effettivamente i parametri.
  ```
  ibmcloud fn action invoke --blocking --result custom/identity
  ```
  {: pre}

  Output di esempio:
  ```
  {
      "city": "Austin",
      "country": "USA"
  }
  ```
  {: screen}

4. Richiama l'azione **identity** con alcuni parametri. I parametri della chiamata vengono uniti ai parametri del pacchetto e li sostituiscono.
  ```
  ibmcloud fn action invoke --blocking --result custom/identity --param city Dallas --param state Texas
  ```
  {: pre}

  Output di esempio:
  ```
  {
      "city": "Dallas",
      "country": "USA",
      "state": "Texas"
  }
  ```
  {: screen}

## Condividi un pacchetto
{: #openwhisk_packages_share}

Una volta eseguiti i test e il debug delle azioni e dei feed compresi in un pacchetto, quest'ultimo può essere condiviso con tutti gli utenti {{site.data.keyword.openwhisk_short}}. La condivisione del pacchetto consente agli utenti di eseguire il bind del pacchetto, richiamare azioni nel pacchetto e creare regole e azioni in sequenza in {{site.data.keyword.openwhisk_short}}.

1. Condividi i pacchetti con tutti gli utenti:
  ```
  ibmcloud fn package update custom --shared yes
  ```
  {: pre}

  Output di esempio:
  ```
  ok: updated package custom
  ```
  {: screen}

2. Visualizza la proprietà `publish` del pacchetto per verificare che ora sia true.
  ```
  ibmcloud fn package get custom publish
  ```
  {: pre}

  Output di esempio:
  ```
  ok: got package custom, displaying field publish

  true
  ```
  {: screen}

Altri utenti possono ora utilizzare il tuo pacchetto **custom**, incluso il bind del pacchetto o la chiamata diretta di un'azione in esso contenuta. Gli altri utenti devono conoscere i nomi completi del pacchetto per poterne eseguire il bind o per chiamare azioni in esso contenute. Le azioni e i feed di un pacchetto condiviso sono _pubblici_. Se il pacchetto è privato, lo sono anche tutti i suoi contenuti.

1. Ottieni una descrizione del pacchetto per visualizzare i nomi completi del pacchetto e dell'azione.
  ```
  ibmcloud fn package get --summary custom
  ```
  {: pre}

  Output di esempio:
  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```
  {: screen}

  Nell'esempio precedente, hai utilizzato lo spazio dei nomi **myNamespace**, che appare nel nome completo.
