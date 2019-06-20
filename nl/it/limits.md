---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: limits, details, entities, packages, runtimes, semantics, ordering actions

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

# Dettagli e limiti del sistema
{: #limits}

Le seguenti sezioni forniscono dettagli tecnici sulle impostazioni di sistema e dei limiti di {{site.data.keyword.openwhisk}}.
{: shortdesc}

## Limiti di sistema
{: #limits_syslimits}

### Azioni
{: #limits_actions}

{{site.data.keyword.openwhisk_short}} ha alcuni limiti di sistema, tra cui la quantità di memoria che un'azione può utilizzare e il numero di chiamate di azioni consentite al minuto.

La seguente tabella elenca i limiti predefiniti per le azioni.

| Limite | Descrizione | Impostazione predefinita | Minimo | Massimo |
| ----- | ----------- | :-------: | :---: | :---: |
| [codeSize](#limits_codesize) | La dimensione massima del codice azione in MB. | 48 | 1 | 48 |
| [concurrent](#limits_concurrent) | Non è possibile inviare più di N attivazioni per ogni spazio dei nomi in esecuzione o in coda per l'esecuzione. | 1000 | 1 | 1000* |
| [logs](#limits_logs) | Un contenitore non può scrivere più di N MB in stdout. | 10 | 0 | 10 |
| [memory](#limits_memory) | Un contenitore non può assegnare più di N MB di memoria. | 256 | 128 | 2048 |
| [minuteRate](#limits_minuterate) | Non è possibile inviare più di N attivazioni per ogni spazio dei nomi al minuto. | 5000 | 1 | 5000* |
| [openulimit](#limits_openulimit) | Il numero massimo di file aperti per un'azione. | 1024 | 0 | 1024 |
| [parameters](#limits_parameters) | La dimensione massima dei parametri che possono essere allegati in MB. | 5 | 0 | 5 |
| [proculimit](#limits_proculimit) | Il numero massimo di processi disponibili per un'azione. | 1024 | 0 | 1024 |
| [result](#limits_result) | La dimensione massima del risultato della chiamata di azione in MB. | 5 | 0 | 5 |
| [sequenceMaxActions](#limits_sequencemax) | Il numero massimo di azioni comprese in una determinata sequenza. | 50 | 0 | 50* |
| [timeout](#limits_timeout) | Un contenitore non può essere eseguito per più di N millisecondi. | 60000 | 100 | 600000 |

### Aumento dei limiti fissi
{: #limits_increase}

I valori limite che terminano con un (*) sono fissi, ma possono essere aumentati se un caso di business può giustificare valori limite di sicurezza più elevati. Se vuoi aumentare il valore limite, contatta il supporto IBM aprendo un ticket direttamente dalla [console web IBM {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk).
  1. Seleziona **Supporto**
  2. Seleziona **Aggiungi ticket** dal menu a discesa.
  3. Seleziona **Tecnico** per il tipo di ticket.
  4. Seleziona **Functions** per l'area tecnica di supporto.

#### codeSize (MB) (valore fisso: 48 MB)
{: #limits_codesize}
* La dimensione massima del codice per l'azione è 48 MB.
* Per le azioni JavaScript, utilizza uno strumento per concatenare tutto il codice sorgente, che include le dipendenze, in un singolo file di bundle.
* Questo limite è fisso e non può essere modificato.

#### concurrent (valore fisso: 1000*)
{: #limits_concurrent}
* Il numero di attivazioni che sono in esecuzione o in coda per l'esecuzione per uno spazio dei nomi non può essere maggiore di 1000.
* Questo valore limite è fisso, ma può essere aumentato se un caso di business può giustificare valori limite di sicurezza più elevati. Consulta la sezione [Aumento dei limiti fissi](/docs/openwhisk?topic=cloud-functions-limits#limits_increase) per istruzioni dettagliate su come aumentare questo limite.

#### logs (MB) (valore fisso: 10 MB)
{: #limits_logs}
* Il limite di log N è compreso nell'intervallo [0 MB..10 MB] ed è impostato per ogni azione.
* Un utente può modificare il limite del log delle azioni quando un'azione viene creata o aggiornata.
* I log che superano il limite impostato vengono troncati, quindi tutte le nuove voci di log vengono ignorate e viene aggiunta un'avvertenza come ultimo output dell'attivazione per indicare che l'attivazione ha superato il limite di log impostato.

#### memory (MB) (valore fisso: 256 MB)
{: #limits_memory}
* Il limite di memoria M è compreso nell'intervallo [128 MB..2048 MB] ed è impostato per ogni azione in MB.
* Un utente può modificare il limite di memoria quando viene creata un'azione.
* Un contenitore non può utilizzare più memoria di quella assegnata dal limite.

#### minuteRate (valore fisso: 5000*)
{: #limits_minuterate}
* Il limite di frequenza N è impostato su 5000 e limita il numero di chiamate di azioni in finestre di 1 minuto.
* Una chiamata CLI o API che superi questo limite riceverà un codice di errore corrispondente al codice di stato HTTP `429: TOO MANY REQUESTS`.
* Questo valore limite è fisso, ma può essere aumentato se un caso di business può giustificare valori limite di sicurezza più elevati. Consulta la sezione [Aumento dei limiti fissi](/docs/openwhisk?topic=cloud-functions-limits#limits_increase) per istruzioni dettagliate su come aumentare questo limite.

#### openulimit (valore fisso: 1024:1024)
{: #limits_openulimit}
* Il numero massimo di file aperti per un'azione è 1024 (per entrambi i limiti hard e soft).
* Questo limite è fisso e non può essere modificato.
* Quando viene richiamata un'azione, il comando docker run utilizza l'argomento `--ulimit nofile=1024:1024` per impostare il valore `openulimit`.
* Per ulteriori informazioni, vedi la documentazione di riferimento della riga di comando [docker run](https://docs.docker.com/engine/reference/commandline/run).

#### parameters (valore fisso: 5 MB)
{: #limits_parameters}
* Il limite di dimensione per i parametri totali durante la creazione o l'aggiornamento di un'azione, un pacchetto o un trigger è 5 MB.
* Un'entità con parametri troppo grandi viene rifiutata durante il tentativo di crearla o aggiornarla.
* Questo limite è fisso e non può essere modificato.

#### proculimit (valore fisso: 1024:1024)
{: #limits_proculimit}
* Il numero massimo di processi disponibili per un contenitore di azioni è 1024.
* Questo limite è fisso e non può essere modificato.
* Quando viene richiamata un'azione, il comando docker run utilizza l'argomento `--pids-limit 1024` per impostare il valore `proculimit`.
* Per ulteriori informazioni, vedi la documentazione di riferimento della riga di comando [docker run](https://docs.docker.com/engine/reference/commandline/run).

#### risultato (valore fisso: 5 MB)
{: #limits_result}
* La dimensione massima di output del risultato di una chiamata di azione in MB.
* Questo limite è fisso e non può essere modificato.

#### sequenceMaxActions (valore fisso: 50*)
{: #limits_sequencemax}
* Il numero massimo di azioni comprese in una determinata sequenza.
* Questo limite è fisso e non può essere modificato.

#### timeout (ms) (valore predefinito: 60s)
{: #limits_timeout}
* Il limite di timeout N è compreso nell'intervallo [100 ms..600000 ms] ed è impostato per ogni azione in millisecondi.
* Un utente può modificare il limite di timeout quando viene creata un'azione.
* Un contenitore in esecuzione per più di N millisecondi viene terminato.

### Trigger
{: #limits_triggers}

I trigger sono soggetti a una frequenza di attivazione al minuto, come indicato nella seguente tabella.

| Limite | Descrizione | Impostazione predefinita | Minimo | Massimo |
| ----- | ----------- | :-------: | :---: | :---: |
| [minuteRate](#limits_triggersminuterate) | Non è possibile attivare più di N trigger per spazio dei nomi al minuto. | 5000* | 5000* | 5000* |

### Aumento dei limiti fissi
{: #limits_triggersfixed}

I valori limite che terminano con un (*) sono fissi, ma possono essere aumentati se un caso di business può giustificare valori limite di sicurezza più elevati. Se vuoi aumentare il valore limite, contatta il supporto IBM aprendo un ticket direttamente dalla [console web IBM {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk).
  1. Seleziona **Supporto**
  2. Seleziona **Aggiungi ticket** dal menu a discesa.
  3. Seleziona **Tecnico** per il tipo di ticket.
  4. Seleziona **Functions** per l'area tecnica di supporto.

#### minuteRate (valore fisso: 5000*)
{: #limits_triggersminuterate}

* Il limite di frequenza N è impostato su 5000 e limita il numero di trigger che un utente può attivare in finestre di 1 minuto.
* Un utente non può modificare il limite di trigger quando viene creato un trigger.
* Una chiamata CLI o API che superi questo limite riceverà un codice di errore corrispondente al codice di stato HTTP `429: TOO MANY REQUESTS`.
* Questo valore limite è fisso, ma può essere aumentato se un caso di business può giustificare valori limite di sicurezza più elevati. Consulta la sezione [Aumento dei limiti fissi](#limits_triggersfixed) per istruzioni dettagliate su come aumentare questo limite.


## Entità di {{site.data.keyword.openwhisk_short}}
{: #limits_entities_ov}

### Spazi dei nomi e pacchetti
{: #limits_namespaces}

Le azioni, i trigger e le regole {{site.data.keyword.openwhisk_short}} appartengono a uno spazio dei nomi e, talvolta, a un pacchetto.

I pacchetti possono contenere azioni e feed. Un pacchetto non può contenerne un altro, pertanto la nidificazione dei pacchetti non è consentita. Inoltre, le entità non devono essere contenute in un pacchetto.

Puoi creare nuovi spazi dei nomi basati su IAM eseguendo `ibmcloud fn namespace create`.  Gli spazi dei nomi basati su Cloud Foundry sono costituiti da una combinazione di nomi spazio e organizzazione. Ad esempio, se vieni indirizzato all'organizzazione `user@email.com` e allo spazio `dev`, sei indirizzato allo {{site.data.keyword.openwhisk_short}} spazio dei nomi basato su Cloud Foundry denominato `user@email.com_dev`. 

Lo spazio dei nomi `/whisk.system` è riservato alle entità distribuite con il sistema {{site.data.keyword.openwhisk_short}}.


### Nomi completi
{: #limits_fullnames}

Il nome completo di un'entità è
`/namespaceName/[packageName]/entityName`. Nota che `/` viene utilizzato per delimitare gli spazi dei nomi, i pacchetti e le entità. Inoltre, gli spazi dei nomi devono essere preceduti da `/`.

Per praticità, lo spazio dei nomi può essere tralasciato se è lo spazio dei nomi predefinito dell'utente. Ad esempio, considera un utente il cui spazio dei nomi predefinito è `/myOrg`. Di seguito sono riportati esempi di nomi completi di una serie di entità e i rispettivi alias.

| Nome completo | Alias | Spazio dei nomi | Pacchetto | Nome |
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read` |  | `/whisk.system` | `cloudant` | `read` |
| `/myOrg/video/transcode` | `video/transcode` | `/myOrg` | `video` | `transcode` |
| `/myOrg/filter` | `filter` | `/myOrg` |  | `filter` |

Puoi adoperare questo schema di denominazione quando utilizzi, ad esempio, la CLI {{site.data.keyword.openwhisk_short}}.

### Nomi delle entità
{: #limits_entities}

I nomi di tutte le entità, inclusi azioni, trigger, regole, pacchetti e spazi dei nomi, sono una sequenza di caratteri aventi il seguente formato:

* Il primo carattere deve essere un carattere alfanumerico o un carattere di sottolineatura.
* I caratteri successivi possono essere alfanumerici, spazi o uno qualsiasi dei seguenti valori: `_`, `@`, `.`, `-`.
* L'ultimo carattere non può essere uno spazio.

Più precisamente, un nome deve corrispondere alla seguente espressione regolare (indicata utilizzando la sintassi di metacaratteri Java): `\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`.

## Semantica delle azioni
{: #limits_semantics}

Le seguenti sezioni forniscono informazioni dettagliate sulle azioni {{site.data.keyword.openwhisk_short}}.

### Assenza di stato
{: #limits_stateless}

Le implementazioni dell'azione sono senza stato o *idempotenti*. Anche se il sistema non impone questa proprietà, non è garantito che uno stato mantenuto da un'azione sia disponibile tra le chiamate.

Inoltre, potrebbero esserci più creazioni di istanze di un'azione, ciascuna delle quali con il proprio stato. La chiamata di un'azione potrebbe essere inviata a una qualsiasi di queste creazioni di istanze.

### Input e output delle chiamate
{: #limits_invocationio}

L'input e l'output di un'azione costituiscono un dizionario di coppie chiave-valore. La chiave è una stringa e il valore è un valore JSON valido.

### Ordine di chiamata delle azioni
{: #limits_ordering}

Le chiamate di un'azione non sono ordinate. Se l'utente richiama un'azione due volte dalla riga di comando o dall'API REST, la seconda chiamata potrebbe essere eseguita per prima. Se le azioni hanno effetti secondari, questi potrebbero essere osservati in qualsiasi ordine.

Inoltre, non è garantito che le azioni vengano eseguite automaticamente. Due azioni possono essere eseguite contemporaneamente e avere effetti secondari interfoliati. OpenWhisk non garantisce uno specifico modello di coerenza simultanea per gli effetti secondari. Eventuali effetti collaterali della simultaneità dipendono dall'implementazione.

### Esecuzioni di azioni
{: #limits_exec}

Alla ricezione di una richiesta di chiamata, il sistema registra la richiesta e invia un'attivazione.

Il sistema restituisce un ID di attivazione (con una chiamata non bloccante) che conferma che è stata ricevuta.
Se si verifica un errore di rete o un altro errore che avviene prima di ricevere una risposta HTTP, è possibile che {{site.data.keyword.openwhisk_short}} abbia ricevuto ed elaborato la richiesta.

Il sistema tenta di richiamare l'azione una volta, ottenendo uno dei quattro seguenti risultati:
- *success*: la chiamata dell'azione è stata completata correttamente.
- *application error*: la chiamata dell'azione ha avuto esito positivo, ma l'azione ha restituito un valore di errore di proposito, ad esempio perché non è stata soddisfatta una precondizione sugli argomenti.
- *action developer error*: l'azione è stata richiamata, ma è terminata in modo anomalo; ad esempio l'azione non ha rilevato un'eccezione o era presente un errore di sintassi.
- *whisk internal error*: il sistema non è stato in grado di richiamare l'azione.
Il risultato viene registrato nel campo `status` del record di attivazione, come illustrato in una delle seguenti sezioni.

Per ogni chiamata ricevuta correttamente, e che potrebbe essere addebitata all'utente, ci sarà un record di attivazione.

Se il risultato è *action developer error*, l'azione potrebbe essere eseguita parzialmente e generare effetti secondari visibili esterni. È responsabilità dell'utente controllare se si sono verificati tali effetti secondari ed immettere la logica del nuovo tentativo laddove necessario. Alcuni errori di tipo *whisk internal errors* indicano che un'azione inizia l'esecuzione, ma si verifica un errore prima che l'azione registri il completamento.

## Record di attivazione
{: #limits_activation}

Ogni chiamata di azione e attivazione di trigger produce un record di attivazione.

Un record di attivazione contiene i seguenti campi:

- *activationId*: l'ID di attivazione.
- *start* e *end*: data/ora di inizio e fine dell'attivazione. I valori sono espressi nel [formato temporale UNIX](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15).
- *namespace* e `name`: lo spazio dei nomi e il nome dell'entità.
- *logs*: un array di stringhe con i log prodotti dall'azione durante la sua attivazione. Ogni elemento dell'array corrisponde a un riga di output emessa dall'azione in `stdout` o `stderr` e include il tempo e il flusso dell'output del log. La struttura è la seguente: `TIMESTAMP STREAM: LOG_OUTPUT`.
- *response*: un dizionario che definisce le chiavi `success`, `status` e `result`:
  - *status*: il risultato dell'attivazione, che può assumere uno dei seguenti valori: "success", "application error", "action developer error", "whisk internal error".
  - *success*: è `true` se, e solo se, lo stato è `"success"`
- *result*: un dizionario che contiene il risultato dell'attivazione. Se l'attivazione ha avuto esito positivo, il risultato contiene il valore restituito dall'azione. Se l'attivazione ha avuto esito negativo, `result` contiene la chiave `error`, generalmente accompagnata da una spiegazione dell'errore.
