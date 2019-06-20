---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: annotations, annotate, package, parameters, actions

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

# Annotazioni
{: #annotations}

Le azioni, i trigger, le regole e i pacchetti {{site.data.keyword.openwhisk}} (indicati collettivamente come entità) possono includere delle annotazioni.
{: shortdesc}

Le annotazioni sono collegate alle entità come parametri. Le annotazioni consistono in una `key` che definisce un nome e un `value` che definisce il valore. Le annotazioni sono più comunemente utilizzate per documentare le azioni e i pacchetti. Molti dei pacchetti nel catalogo {{site.data.keyword.openwhisk_short}} contengono annotazioni quali: una descrizione della funzionalità offerta dalle sue azioni, i parametri da utilizzare al momento del bind del pacchetto, i parametri del tempo di chiamata o se un parametro è un segreto. Le annotazioni vengono inventate secondo necessità, ad esempio per consentire l'integrazione dell'interfaccia utente.

Puoi documentare un'entità dalla CLI utilizzando l'indicatore `--annotation` o quello `-a`.

## Annotazioni di azioni
{: #annotations_action}

Le annotazioni che descrivono le azioni includono:

- `description`: una descrizione concisa dell'azione.
- `parameters`: un array che descrive le azioni richieste per eseguire l'azione.
- `sampleInput`: un esempio che mostra lo schema di input con valori tipici.
- `sampleOutput`: un esempio che mostra lo schema di output, solitamente per `sampleInput`.



Il seguente codice è una serie di annotazioni di esempio per un'azione `echo`, che restituisce i suoi argomenti di input non modificati. Questa azione è utile per registrare i parametri di input, ad esempio, come parte di una sequenza o regola.

```
ibmcloud fn action create echo echo.js \
    -a description 'An action which returns its input. Utile per la registrazione dell'input per abilitare il debug o la riproduzione.' \
    -a parameters  '[{ "required":false, "description": "Qualsiasi entità JSON" }]' \
    -a sampleInput  '{ "msg": "Cinque felini indistinti"}' \
    -a sampleOutput '{ "msg": "Cinque felini indistinti"}'
```
{: pre}

## Annotazioni di azioni web
{: #annotations-specific-to-web-actions}

Le seguenti annotazioni di azioni web devono essere impostate esplicitamente su `true` per abilitare l'interattività API:

- `web-export`: quando è applicata a un'azione, l'azione diventa una [azione web](/docs/openwhisk?topic=cloud-functions-actions_web). L'azione diventa accessibile alle chiamate REST senza autenticazione in modo che gli utenti possano accedere alle azioni da un browser.
    * **Nota**: il proprietario dell'azione web sostiene il costo della loro esecuzione nel sistema. In altre parole, il proprietario dell'azione possiede anche il record delle attivazioni.
- `final`: quando è applicata a un'azione, qualsiasi parametro di azione che era stato definito in precedenza diventa immutabile. I parametri non possono essere sovrascritti dai parametri forniti durante la chiamata.
- `raw-http`: quando è applicata a un'azione che ha l'annotazione `web-export`, i parametri di query e corpo della richiesta HTTP vengono passati all'azione come proprietà riservate.
- `web-custom-options`: abilita un'azione web a rispondere alle richieste OPTIONS con le intestazioni personalizzate. Altrimenti, si applica una [risposta CORS predefinita](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_options).
- `require-whisk-auth`: l'azione web può essere richiamata solo da richieste che forniscono le appropriate credenziali di autenticazione.
    * Quando è impostata su un valore booleano, controlla se il valore di autenticazione di base della richiesta è autenticato. Il valore `true` autentica le credenziali e il valore `false` richiama l'azione senza alcuna autenticazione.
    * Quando è impostata su un numero intero o una stringa, questo valore deve corrispondere al valore dell'intestazione `X-Require-Whisk-Auth` della richiesta.
    * **Nota**: il proprietario dell'azione web sostiene il costo della loro esecuzione nel sistema. In altre parole, il proprietario dell'azione possiede anche il record delle attivazioni.

## Annotazioni di pacchetto
{: #annotations_package}

Le annotazioni che descrivono i pacchetti includono:

- `description`: una descrizione concisa del pacchetto.
- `parameters`: un array che descrive i parametri applicabili al pacchetto.

## Annotazioni di parametro
{: #annotations_parameter}

Le annotazioni che descrivono i parametri includono:

- `name`: il nome del parametro.
- `description`: una descrizione concisa del parametro.
- `doclink`: un link ad ulteriore documentazione per il parametro (utile per i token OAuth).
- `required`: true per i parametri obbligatori e false per quelli facoltativi.
- `bindTime`: true se il parametro viene specificato quando si esegue il bind del pacchetto.
- `type`: il tipo di parametro, uno tra `password`, `array` (ma può essere utilizzato in modo più ampio).

## Annotazioni di attivazione
{: #annotations_activation}

Puoi documentare i record di attivazione con le seguenti annotazioni:

- `path`: il nome del percorso completo dell'azione che ha generato l'attivazione. Nota che se questa attivazione è stata il risultato di un'azione in un bind di pacchetto, il percorso si riferisce al pacchetto principale.
- `kind`: il tipo di azione eseguita e uno dei tipi di runtime OpenWhisk di supporto.
- `limits`: i limiti di tempo, memoria e log a cui questa attivazione era soggetta.

Per le attivazioni correlate alla sequenza, il sistema genera le seguenti annotazioni:

- `topmost`: è presente solo per un'azione di sequenza più esterna.
- `causedBy`: è presente solo per le azioni contenute in una sequenza.

Per fornire la trasparenza delle prestazioni, le attivazioni registrano anche quanto segue:

- `waitTime`: il tempo di attesa nel sistema OpenWhisk interno. Questo è approssimativamente il tempo trascorso tra il momento in cui il controller riceve la richiesta di attivazione e il momento in cui l'invoker fornisce un contenitore per l'azione. Questo valore è attualmente presente solo per le attivazioni non correlate alla sequenza. Per le sequenze, queste informazioni possono essere ricavate dal record di attivazione della sequenza `topmost`.
- `initTime`: il tempo trascorso per inizializzare la funzione. Se questo valore è presente, l'azione ha richiesto l'inizializzazione e rappresenta un avvio a freddo. Un'attivazione a caldo salta l'inizializzazione e in questo caso l'annotazione non viene generata.

Il seguente esempio mostra queste annotazioni così come potrebbero presentarsi in un record di attivazione:

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

