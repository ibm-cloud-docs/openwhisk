---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-12"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Annotazioni
{: #openwhisk_annotations}

Le azioni, i trigger, le regole e i pacchetti {{site.data.keyword.openwhisk}} (indicati collettivamente come asset) possono essere decorati con `annotazioni`. Le annotazioni vengono allegate agli asset proprio come i parametri, con una `key` che definisce un nome e un `value` che definisce il valore. È opportuno impostarle dalla CLI (command line interface) utilizzando l'indicatore `--annotation` o l'abbreviazione `-a`.
{: shortdesc}

Ragione fondamentale: le annotazioni sono state aggiunte a {{site.data.keyword.openWhisk_short}} per consentire la sperimentazione senza modificare lo schema di asset sottostante. Fino alla stesura di questo documento, è stato fatto un tentativo deliberato di non definire quali `annotazioni` siano consentite. Tuttavia, poiché le annotazioni sono utilizzate maggiormente per impartire modifiche semantiche, è importante iniziare a documentarle.

Fino ad oggi, l'utilizzo più diffuso delle annotazioni è quello di documentare le azioni e i pacchetti. Molti dei pacchetti nel catalogo {{site.data.keyword.openwhisk_short}} contengono annotazioni quali: una descrizione della funzionalità offerta dalle sue azioni, i parametri da utilizzare al momento del bind del pacchetto, i parametri del tempo di richiamo o se un parametro è un "segreto" (ad esempio, password) o meno. Le annotazioni vengono inventate secondo necessità, ad esempio per consentire l'integrazione dell'interfaccia utente.

Di seguito è riportata una serie di annotazioni di esempio per un'azione `echo`, che restituisce i suoi argomenti di input non modificati (ad esempio, `function main(args) { return args }`). Questa azione è utile per registrare i parametri di input, ad esempio, come parte di una sequenza o regola.
```
ibmcloud fn action create echo echo.js \
    -a description 'Un'azione che restituisce il suo input. Utile per la registrazione dell'input per abilitare il debug o la riproduzione.' \
    -a parameters  '[{ "required":false, "description": "Qualsiasi entità JSON" }]' \
    -a sampleInput  '{ "msg": "Cinque felini indistinti"}' \
    -a sampleOutput '{ "msg": "Cinque felini indistinti"}'
```
{: pre}

Le annotazioni che descrivono i **pacchetti** includono:

- `description`: una descrizione concisa del pacchetto.
- `parameters`: un array che descrive i parametri applicabili al pacchetto.

Le annotazioni che descrivono le **azioni** includono:

- `description`: una descrizione concisa dell'azione.
- `parameters`: un array che descrive le azioni richieste per eseguire l'azione.
- `sampleInput`: un esempio che mostra lo schema di input con valori tipici.
- `sampleOutput`: un esempio che mostra lo schema di output, solitamente per `sampleInput`.

Le annotazioni che descrivono i **parametri** includono:

- `name`: il nome del parametro.
- `description`: una descrizione concisa del parametro.
- `doclink`: un link ad ulteriore documentazione per il parametro (utile per i token OAuth).
- `required`: true per i parametri obbligatori e false per quelli facoltativi.
- `bindTime`: true se il parametro viene specificato quando si esegue il bind del pacchetto.
- `type`: il tipo di parametro, uno tra `password`, `array` (ma può essere utilizzato in modo più ampio).

Le annotazioni _non_ vengono verificate. Quindi, mentre è ipotizzabile utilizzare le annotazioni per dedurre se, ad esempio, una composizione di due azioni in una sequenza è legale, il sistema non lo fa ancora. 

## Annotazioni specifiche per le azioni web
{: #annotations-specific-to-web-actions}

Di recente, l'API principale è stata estesa con nuove funzioni. Per consentire a pacchetti e azioni di partecipare a queste funzioni, vengono introdotte nuove annotazioni semanticamente significative. Per avere effetto, queste annotazioni devono essere impostate esplicitamente su `true`. La modifica del valore da `true` a `false` esclude l'asset allegato dalla nuova API. Le annotazioni non hanno altro significato nel sistema. Vedi le seguenti annotazioni:

- `web-export`: si applica solo a un'azione. Se presente, rende la sua azione corrispondente accessibile alle chiamate REST _senza_ autenticazione. Sono chiamate [_azioni web_](openwhisk_webactions.html) perché consentono di utilizzare le azioni OpenWhisk, ad esempio, da un browser. È importate notare che il _proprietario_ delle azioni web sostiene il costo della loro esecuzione nel sistema. In altre parole, il _proprietario_ dell'azione possiede anche il record delle attivazioni.
- `final`: si applica solo a un'azione. Rende immutabili tutti i parametri di azione già definiti. Un parametro di un'azione che contiene l'annotazione non può essere sovrascritto dai parametri del tempo di richiamo una volta che il valore dei parametri viene definito tramite il relativo pacchetto di inclusione o la definizione dell'azione.
- `raw-http`: si applica solo a un'azione in presenza di un'annotazione `web-export`. Se presente, i parametri di query e corpo della richiesta HTTP vengono passati all'azione come proprietà riservate.
- `web-custom-options`: se impostata, questa annotazione consente a un'azione web di rispondere alle richieste OPTIONS con intestazioni personalizzate, altrimenti viene applicata una [risposta CORS predefinita](openwhisk_webactions.html#options-requests).
- `require-whisk-auth`: questa annotazione protegge l'azione web in modo che sia richiamata solo dalle richieste che forniscono credenziali di autenticazione appropriate. Quando è impostato su un valore booleano, controlla se il valore di autenticazione di base della richiesta (cioè la chiave di autenticazione Whisk) verrà autenticato o meno. Il valore `true` autentica le credenziali e il valore `false` richiama l'azione senza alcuna autenticazione. Quando è impostato su un numero o una stringa, questo valore deve corrispondere al valore dell'intestazione `X-Require-Whisk-Auth` della richiesta. In entrambi i casi, è importate notare che il _proprietario_ delle azioni web sostiene il costo della loro esecuzione nel sistema (ossia, il _proprietario_ dell'azione possiede anche il record delle attivazioni).

## Annotazioni specifiche per le attivazioni

Il sistema può decorare i record di attivazione con le seguenti annotazioni:

- `path`: il nome del percorso completo dell'azione che ha generato l'attivazione. Nota che se questa attivazione è stata il risultato di un'azione in un bind di pacchetto, il percorso si riferisce al pacchetto principale.
- `kind`: il tipo di azione eseguita e uno dei tipi di runtime OpenWhisk di supporto.
- `limits`: i limiti di tempo, memoria e log a cui questa attivazione era soggetta.

Per le attivazioni relative alla sequenza, il sistema genera le seguenti annotazioni:

- `topmost`: è presente solo per un'azione di sequenza più esterna.
- `causedBy`: è presente solo per le azioni contenute in una sequenza.

Infine, e per garantire la trasparenza delle prestazioni, le attivazioni registrano anche quanto segue:

- `waitTime`: il tempo di attesa nel sistema OpenWhisk interno. Questo è approssimativamente il tempo trascorso tra il momento in cui il controller riceve la richiesta di attivazione e il momento in cui l'invoker fornisce un contenitore per l'azione. Questo valore è attualmente presente solo per le attivazioni non correlate alla sequenza. Per le sequenze, queste informazioni possono essere ricavate dal record di attivazione della sequenza `topmost`.
- `initTime`: il tempo trascorso per inizializzare la funzione. Se questo valore è presente, l'azione ha richiesto l'inizializzazione e rappresenta un avvio a freddo. Un'attivazione a caldo salta l'inizializzazione e in questo caso l'annotazione non viene generata.

Di seguito viene mostrato un esempio di queste annotazioni così come potrebbero essere visualizzate in un record di attivazione.

```javascript
"annotations": [
  {
    "key": "path",
    "value": "guest/echo"
  },
  {
    "key": "waitTime",
    "value": 66
  },
  {
    "key": "kind",
    "value": "nodejs:6"
  },
  {
    "key": "initTime",
    "value": 50
  },
  {
    "key": "limits",
    "value": {
      "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
  }
]
```
{: codeblock}
