---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-28"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Risposta agli eventi con trigger e regole
{: #openwhisk_triggers}

I trigger e le regole {{site.data.keyword.openwhisk}} forniscono alla piattaforma funzionalità guidate dagli eventi. Gli eventi provenienti da origini eventi interne ed esterne vengono incanalati attraverso un trigger e le regole consentono alle tue azioni di reagire a tali eventi.
{: shortdesc}

## Concetti generali
{: #definitions}

### Trigger
{: #openwhisk_triggers_create}

I trigger sono un canale denominato per una classe di eventi.
{: shortdesc}

Un trigger è una dichiarazione da te scelta per reagire a un determinato tipo di evento, sia che provenga da un utente che da un'origine eventi. I seguenti sono esempi di trigger.
- Un trigger di eventi di aggiornamento dell'ubicazione
- Un trigger di caricamenti di documenti su un sito web
- Un trigger di e-mail in entrata

I trigger possono essere attivati utilizzando un dizionario di coppie chiave-valore. Tale dizionario viene talvolta denominato evento. I trigger possono essere attivati esplicitamente da un utente o attivati per conto di un utente da un'origine eventi esterna. Come per le azioni, ogni attivazione di un trigger associato a una regola genera un ID di attivazione. Un trigger che non è associato a una regola non ha alcun effetto visibile quando viene attivato.

Un feed rappresenta un modo pratico per configurare un'origine eventi esterna
per l'attivazione di eventi trigger che possono essere utilizzati da {{site.data.keyword.openwhisk_short}}. I seguenti sono esempi di feed.
- Un feed di modifica dati {{site.data.keyword.cloudant}} che attiva un evento trigger ogni volta che si aggiunge o modifica un documento in un database
- Un feed Git che attiva un evento trigger per ogni commit a un repository Git

### Regole
{: #openwhisk_rules_use}

Una regola associa un trigger a un'azione.
{: shortdesc}

Ogni volta che un trigger viene attivato, la regola utilizza l'evento di trigger e richiama l'azione associata. Con l'insieme appropriato di regole, è possibile che un singolo evento trigger richiami più azioni o che un'azione venga richiamata in risposta a eventi appartenenti a più trigger.

Ad esempio, considera un sistema con le seguenti azioni.
- `classifyImage` - Un'azione che rileva gli oggetti in un'immagine e li classifica.
- `thumbnailImage` - Un'azione che crea una versione in miniatura di un'immagine.

Supponi, inoltre, che due origini eventi stiano attivando i seguenti trigger.
- `newTweet` - Un trigger che viene attivato alla pubblicazione di un nuovo tweet.
- `imageUpload` - Un trigger che viene attivato al caricamento di un'immagine in un sito web.

Puoi configurare le regole in modo che un unico evento trigger richiami più azioni e far sì che più trigger richiamino la stessa azione.
- `newTweet -> classifyImage` rule
- `imageUpload -> classifyImage` rule
- `imageUpload -> thumbnailImage` rule

Le tre regole stabiliscono il seguente comportamento.
- Vengono classificate le immagini di entrambi i tweet
- Vengono classificate le immagini caricate
- Viene generata una versione in miniatura.

## Creazione di trigger per incanalare eventi
{: #openwhisk_triggers_fire}

La seguente procedura ti mostra come creare un trigger di evento per inviare aggiornamenti sull'ubicazione dell'utente e come attivare manualmente il trigger.

1. Crea il trigger. I trigger devono essere creati direttamente all'interno di uno spazio dei nomi e non possono essere creati all'interno di pacchetti.
    ```
    ibmcloud fn trigger create locationUpdate
    ```
    {: pre}

    Output di esempio:
    ```
    ok: created trigger locationUpdate
    ```
    {: screen}

2. Verifica che il trigger venga creato.
    ```
    ibmcloud fn trigger list
    ```
    {: pre}

    Output di esempio:
    ```
    triggers
  /someNamespace/locationUpdate                            private
    ```
    {: screen}
    Il trigger funge da canale denominato in cui è possibile attivare gli eventi.

3. Attiva un evento di trigger.
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    Poiché non c'è alcuna regola associata a questo trigger, i parametri passati non vengono utilizzati come input da alcuna azione. Output di esempio:
    ```
    ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

Nella sezione successiva, puoi associare il trigger a un'azione creando una regola.

## Utilizzo di regole per associare i trigger alle azioni
{: #openwhisk_rules_assoc}

Le regole vengono utilizzate per associare un trigger a un'azione. Ogni volta che un evento trigger viene attivato, l'azione viene richiamata con i parametri dall'evento di trigger.

Dopo che hai creato il [trigger `locationUpdate`](#openwhisk_triggers_fire), i seguenti passi ti mostrano come creare una regola di esempio che richiama l'azione `hello` ogni volta che viene pubblicato un aggiornamento dell'ubicazione.

1. Crea un file denominato 'hello.js' con il seguente codice azione:
    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
    }
    ```
    {: pre}

2. Crea l'azione `hello`.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Crea la regola `myRule` da associare al trigger `locationUpdate` con l'azione `hello`. Le regole devono essere create direttamente all'interno di uno spazio dei nomi e non possono essere create all'interno di pacchetti.
    ```
    ibmcloud fn rule create myRule locationUpdate hello
    ```
    {: pre}

4. Attiva il trigger `locationUpdate`. Ogni volta che si verifica un evento trigger, l'azione `hello` viene richiamata con i parametri dell'evento.
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    Output di esempio:
    ```
    ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
    ```
    {: screen}

5. Verifica che l'azione `hello` sia stata richiamata controllando il record di attivazione più recente.
    ```
    ibmcloud fn activation list --limit 1 hello
    ```
    {: pre}

    Output di esempio:
    ```
    activations
  9c98a083b924426d8b26b5f41c5ebc0d             hello
    ```
    {: screen}

6. Ottieni ulteriori informazioni sull'ID attivazione dall'output del comando precedente.
    ```
    ibmcloud fn activation result 9c98a083b924426d8b26b5f41c5ebc0d
    ```
    {: pre}

    Output di esempio:
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}
    Puoi vedere che l'azione `hello` ha ricevuto il payload dell'evento e ha restituito la stringa prevista.

7. Per disabilitare la regola, puoi eseguire il seguente comando.
    ```
    ibmcloud fn rule disable myRule
    ```
    {: pre}

Puoi anche utilizzare le regole per associare i trigger alle sequenze. Ad esempio, puoi creare una sequenza di
azioni denominata `recordLocationAndHello` che viene attivata dalla regola `anotherRule`:
```
ibmcloud fn action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
```
{: pre}

```
ibmcloud fn rule create anotherRule locationUpdate recordLocationAndHello
```
{: pre}
