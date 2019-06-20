---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: actions, serverless, javascript, node, node.js

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


# Associazione di trigger e azioni tramite regole
{: #rules}

Ogni volta che un trigger viene attivato, la regola utilizza l'evento di trigger e richiama l'azione associata. Con l'insieme appropriato di regole, è possibile che un singolo evento trigger richiami più azioni o che un'azione venga richiamata in risposta a eventi appartenenti a più trigger.
{: shortdesc}


## Creazione di regole dalla UI
{: #rules_ui}

Viene creata automaticamente una regola per l'associazione di un'azione a un trigger dall'interfaccia utente.
{: shortdesc}

Quando crei dettagli per un'azione o un trigger, o vi accedi, puoi collegare un'azione o un trigger nuovo o esistente. Quando crei il collegamento, viene creata automaticamente una regola denominata secondo il formato `ACTION_NAME-TRIGGER_NAME`.

Dalla CLI, puoi eseguire `ibmcloud fn rule list` per verificare che la regola sia stata creata automaticamente.


## Creazione di regole dalla CLI
{: #rules_create}

Le regole vengono utilizzate per associare un trigger a un'azione. Ogni volta che un evento trigger viene attivato, l'azione viene richiamata con i parametri dall'evento di trigger.

Prima di iniziare, crea [un'azione](/docs/openwhisk?topic=cloud-functions-actions) e [un trigger](/docs/openwhisk?topic=cloud-functions-triggers).


Crea una regola per associare un trigger a un'azione. Le regole devono essere create direttamente all'interno di uno spazio dei nomi e non possono essere create all'interno di pacchetti.
```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}


Per disabilitare la regola, puoi eseguire il seguente comando.
```
ibmcloud fn rule disable RULE_NAME
```
{: pre}


## Creazione di regole per sequenze di azioni
{: #rules_seq}

Puoi utilizzare le regole per associare i trigger a delle sequenze di regole.

Prima di iniziare, crea [una sequenza di azioni](/docs/openwhisk?topic=cloud-functions-actions#actions_seq) e [un trigger](/docs/openwhisk?topic=cloud-functions-triggers).

```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_SEQUENCE_NAME
```
{: pre}


## Associazione di più trigger e azioni
{: #rules_assoc}

Puoi utilizzare varie combinazioni di trigger e azioni creando una regola per ciascuna combinazione. Non è richiesto un rapporto 1:1 tra azioni e trigger.

Ad esempio, considera le seguenti azioni.
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
