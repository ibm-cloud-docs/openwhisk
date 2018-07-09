---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Crea trigger e regole
{: #openwhisk_triggers}

I trigger e le regole {{site.data.keyword.openwhisk_short}} forniscono alla piattaforma funzionalità guidate dagli eventi. Gli eventi provenienti da origini eventi interne ed esterne vengono incanalati attraverso un trigger e le regole consentono alle tue azioni di reagire a tali eventi.
{: shortdesc}

## Crea i trigger
{: #openwhisk_triggers_create}

I trigger sono un canale denominato per una classe di eventi. Di seguito vengono riportati esempi di trigger:
- Un trigger di eventi di aggiornamento dell'ubicazione.
- Un trigger di caricamenti di documenti su un sito Web.
- Un trigger di e-mail in entrata.

I trigger possono essere *attivati* attraverso un dizionario di coppie chiave-valore. Tale dizionario viene talvolta denominato *evento*. Come per le azioni, ogni attivazione di un trigger genera un ID di attivazione.

I trigger possono essere attivati esplicitamente da un utente o attivati per conto di un utente da un'origine eventi esterna.
Un *feed* rappresenta un modo pratico per configurare un'origine eventi esterna per l'attivazione di eventi trigger utilizzabili da {{site.data.keyword.openwhisk_short}}. Fai riferimento ai seguenti feed di esempio:
- Un feed di modifica dati Cloudant che attiva un evento trigger ogni volta che si aggiunge o modifica un documento in un database.
- Un feed Git che attiva un evento trigger per ogni commit su un repository Git.

## Utilizzo delle regole
{: #openwhisk_rules_use}

Una regola associa un trigger a un'azione, per cui ogni attivazione del trigger comporta la chiamata dell'azione corrispondente con l'evento trigger utilizzato come input. 

Con l'insieme appropriato di regole, è possibile che un singolo evento trigger
richiami più azioni o che un'azione venga richiamata in risposta a eventi
appartenenti a più trigger.

Ad esempio, considera un sistema con le seguenti azioni:
- `classifyImage` - Un'azione che rileva gli oggetti in un'immagine e li classifica.
- `thumbnailImage` - Un'azione che crea una versione in miniatura di un'immagine.

Supponi, inoltre, che due origini eventi stiano attivando i seguenti trigger:
- `newTweet` - Un trigger che viene attivato alla pubblicazione di un nuovo tweet.
- `imageUpload` - Un trigger che viene attivato al caricamento di un'immagine in un sito Web.

Puoi configurare le regole in modo che un unico evento trigger richiami più azioni e far sì che più trigger richiamino la stessa azione:
- Regola `newTweet -> classifyImage`.
- Regola `imageUpload -> classifyImage`.
- Regola `imageUpload -> thumbnailImage`.

Le tre regole stabiliscono il seguente comportamento: 
- Vengono classificate le immagini di entrambi i tweet
- Vengono classificate le immagini caricate
- Viene generata una versione in miniatura.

## Crea e attiva i trigger
{: #openwhisk_triggers_fire}

I trigger possono essere attivati al verificarsi di determinati eventi o manualmente.

Ad esempio, crea un trigger per l'invio di aggiornamenti sull'ubicazione dell'utente e attivalo manualmente.
1. Per creare il trigger, immetti il seguente comando:
  ```
  wsk trigger create locationUpdate
  ```
  {: pre}

  ```
  ok: created trigger locationUpdate
  ```

2. Verifica di aver creato il trigger elencando l'insieme di trigger.
  ```
  wsk trigger list
  ```
  {: pre}

  ```
  triggers
  /someNamespace/locationUpdate                            private
  ```

  Adesso è stato creato un "canale" denominato in cui è possibile attivare gli eventi.

3. In seguito, attiva un evento trigger specificando il nome e i parametri del trigger:
  ```
  wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  ```
  ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
  ```

Un trigger che viene attivato senza una regola di accompagnamento da soddisfare non ha un effetto visibile.
I trigger non possono essere creati in un pacchetto, devono essere creati direttamente in uno spazio dei nomi.

## Associa trigger e azioni utilizzando le regole
{: #openwhisk_rules_assoc}

Le regole vengono utilizzate per associare un trigger a un'azione. Ogni volta che un evento trigger viene attivato, l'azione viene richiamata con i parametri dell'evento.

Ad esempio, crea una regola che richiama l'azione `hello` ogni volta che viene pubblicato un aggiornamento sull'ubicazione.
1. Crea un file 'hello.js' con il codice azione come il seguente:
  ```javascript
  function main(params) {
     return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. Assicurati che il trigger e l'azione esistano.
  ```
  wsk trigger update locationUpdate
  ```
  {: pre}

  ```
  wsk action update hello hello.js
  ```
  {: pre}

3. Il passo successivo è quello di creare la regola. La regola viene abilitata al momento della creazione, il che significa che è immediatamente disponibile per rispondere alle attivazioni del tuo trigger. I tre parametri sono: il nome della regola, il trigger e l'azione.
  ```
  wsk rule create myRule locationUpdate hello
  ```
  {: pre}

  In qualsiasi momento, puoi scegliere di disabilitare una regola.
  ```
  wsk rule disable myRule
  ```
  {: pre}

4. Attiva il trigger `locationUpdate`. Ogni volta che attivi un evento, l'azione `hello` viene richiamata con i parametri dell'evento.
  ```
  wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  ```
  ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
  ```

5. Verifica che l'azione sia stata richiamata controllando l'attivazione più recente.
  ```
  wsk activation list --limit 1 hello
  ```
  {: pre}

  ```
  activations
  9c98a083b924426d8b26b5f41c5ebc0d             hello
  ```
  ```
  wsk activation result 9c98a083b924426d8b26b5f41c5ebc0d
  ```
  {: pre}

  ```json
  {
     "payload": "Hello, Donald from Washington, D.C."
  }
  ```

  Puoi vedere che l'azione `hello` ha ricevuto il payload dell'evento e ha restituito la stringa prevista.

Puoi creare più regole che associano lo stesso trigger ad azioni differenti.
I trigger e le regole non possono appartenere a un pacchetto. Tuttavia, la regola può essere associata a un'azione
che appartiene a un pacchetto, ad esempio:
  ```
  wsk rule create recordLocation locationUpdate /whisk.system/utils/echo
  ```
  {: pre}

Puoi anche utilizzare le regole con le sequenze. Ad esempio, è possibile creare una sequenza
di azioni `recordLocationAndHello` che viene attivata dalla regola `anotherRule`.
  ```
  wsk action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
  ```
  {: pre}

  ```
  wsk rule create anotherRule locationUpdate recordLocationAndHello
  ```
  {: pre}
