---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: annotations, annotate, package, parameters, actions, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
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

Le annotazioni sono collegate alle entità come parametri. Le annotazioni consistono in una `key` che definisce un nome e un `value` che definisce il valore. Le annotazioni sono più comunemente utilizzate per documentare le azioni e i pacchetti. I pacchetti nel catalogo {{site.data.keyword.openwhisk_short}} contengono annotazioni. Queste annotazioni includono descrizioni della funzionalità offerta dalle sue azioni, parametri da utilizzare in fase di bind del pacchetto, parametri per la fase di chiamata o se un parametro è un segreto. Le annotazioni vengono inventate secondo necessità, ad esempio per consentire l'integrazione dell'interfaccia utente.

Puoi documentare un'entità dalla CLI utilizzando l'indicatore `--annotation` o quello `-a`.

## Annotazioni di azioni
{: #annotations_action}

| Annotazione | Descrizione |
| --- | --- |
| `description` | Una descrizione dell'azione. |
| `parameters` | Un array che descrive le azioni necessarie per eseguire l'azione. |
| `sampleInput` | Un esempio che mostra lo schema di input con valori tipici. |
| `sampleOutput` | Un esempio che mostra lo schema di output, in genere per `sampleInput`. |



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

Le seguenti annotazioni di azioni web devono essere impostate esplicitamente su `true` per abilitare l'interattività API.

| Annotazione | Descrizione |
| --- | --- | 
| `web-export` | Quando applicata a un'azione, l'azione diventa un'[azione web](/docs/openwhisk?topic=cloud-functions-actions_web). L'azione diventa accessibile alle chiamate REST senza autenticazione in modo che gli utenti possano accedere alle azioni da un browser. Il proprietario dell'azione web sostiene i costi della loro esecuzione. In altre parole, il proprietario dell'azione possiede anche il record delle attivazioni. |
| `final` | Quando applicata a un'azione, tutti i parametri di azione precedentemente definiti non possono essere sovrascritti dai parametri forniti durante la chiamata. |
| `raw-http` | Quando applicata a un'azione che ha l'annotazione `web-export`, i parametri di query e corpo della richiesta HTTP vengono passati all'azione come proprietà riservate. |
| `web-custom-options` | Abilita un'azione web a rispondere alle richieste OPTIONS con intestazioni personalizzate. Altrimenti, si applica una [risposta CORS predefinita](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_options). |
| `require-whisk-auth` | L'azione web può essere richiamata solo da richieste che forniscono credenziali di autenticazione appropriate. Quando è impostata su un valore booleano, controlla se il valore di autenticazione di base della richiesta è autenticato. Il valore `true` autentica le credenziali e il valore `false` richiama l'azione senza alcuna autenticazione. Quando è impostata su un numero intero o una stringa, questo valore deve corrispondere al valore dell'intestazione `X-Require-Whisk-Auth` della richiesta. |

## Annotazioni di pacchetto
{: #annotations_package}

| Annotazione | Descrizione |
| --- | --- |
| `description` | Una descrizione del pacchetto. |
| `parameters` | Un array che descrive i parametri nell'ambito del pacchetto. |

## Annotazioni di parametro
{: #annotations_parameter}

| Annotazione | Descrizione |
| --- | --- |
| `name` | Il nome del parametro. |
| `description` | Una descrizione concisa del parametro. |
| `doclink` | Un link ad ulteriore documentazione per il parametro (utile per i token OAuth). |
| `required` | True per i parametri obbligatori e false per quelli facoltativi. |
| `bindTime` | True se il parametro viene specificato quando si esegue il bind del pacchetto. |
| `type` | Il tipo di parametro, uno tra `password`, `array` (ma può essere utilizzato in modo più ampio). |

## Annotazioni di attivazione
{: #annotations_activation}

Puoi documentare i record di attivazione con le seguenti annotazioni:

| Annotazione | Descrizione |
| --- | --- |
| `path` | Il nome del percorso completo dell'azione che ha generato l'attivazione. Se questa attivazione è stata il risultato di un'azione in un bind di pacchetto, il percorso si riferisce al pacchetto principale. |
| `kind` | Il tipo di azione eseguita e uno dei tipi di runtime {{site.data.keyword.openwhisk_short}} supportati. |
| `limits` | I limiti di tempo, memoria e log sottoposti a questa attivazione. |

Per le attivazioni correlate alla sequenza, il sistema genera le seguenti annotazioni:

| Annotazione | Descrizione |
| --- | --- |
| `topmost` | Questa annotazione è presente solo per un'azione di sequenza più esterna. |
| `causedBy` | Questa annotazione è presente solo per le azioni contenute in una sequenza. |
| `waitTime` | Il tempo trascorso in attesa nel sistema {{site.data.keyword.openwhisk_short}} interno. Questo tempo è approssimativamente il tempo trascorso tra la ricezione della richiesta di attivazione e il momento in cui l'invoker ha fornito un contenitore per l'azione. Questo valore è presente solo per le attivazioni non correlate alla sequenza. Per le sequenze, queste informazioni possono essere ricavate dal record di attivazione della sequenza `topmost`. |
| `initTime` | Il tempo trascorso per inizializzare la funzione. Se questo valore è presente, l'azione ha richiesto l'inizializzazione e rappresenta un avvio a freddo. Un'attivazione a caldo salta l'inizializzazione e in questo caso l'annotazione non viene generata. |

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




