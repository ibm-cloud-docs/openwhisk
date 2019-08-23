---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, functions, serverless, javascript, node, node.js

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



# Creazione di azioni
{: #actions}

Crea un'azione, ossia una funzione di livello superiore che restituisce un oggetto JSON. Puoi combinare le azioni in un pacchetto per semplificare la gestione delle tue azioni.
{: shortdesc}

Prima di iniziare:
per creare un'azione, il tuo codice sorgente deve soddisfare determinati requisiti. Ad esempio, se vuoi creare un'azione dal codice contenuto in più file, inserisci il tuo codice in un pacchetto come file singolo prima di creare l'azione. Vedi [Preparazione del codice della tua applicazione per l'uso senza server](/docs/openwhisk?topic=cloud-functions-prep) per informazioni dettagliate sui requisiti per ciascun runtime.


## Creazione di azioni dalla CLI
{: #actions_cli}

1. Crea un'azione.
  ```
  ibmcloud fn action create ACTION_NAME APP_FILE --kind RUNTIME
  ```
  {: pre}

  **Esempio**
  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  **Output di esempio**

  ```
  ok: created action hello
  ```
  {: screen}

  Suggerimenti:
  - Per diminuire i costi, puoi impostare dei limiti.
      - Per impostare un limite per l'utilizzo della memoria, nel comando di creazione includi `-- memory VALUE`, dove il valore è espresso in megabyte.
      - Per impostare un timeout, nel comando di creazione includi `-- timeout VALUE`, dove il valore è espresso in millisecondi.
  - Se hai inserito il tuo codice in un pacchetto sotto forma di immagine Docker, nel comando di creazione includi `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG` invece del percorso locale alla tua applicazione e l'indicatore di --kind. Gestisci bene le tue immagini evitando il più possibile di utilizzare la tag `latest`. Quando si utilizza la tag `latest`, viene utilizzata l'immagine che riporta quella tag, che potrebbe non essere l'ultima immagine creata.

      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
  
2. Verifica che l'azione sia riportata nell'elenco delle tue azioni.

  ```
  ibmcloud fn action list
  ```
  {: pre}

  **Output di esempio**

  ```
  actions
  hello       private
  ```
  {: screen}


## Aggiornamento di applicazioni o runtime nelle azioni
{: #actions_update}

Puoi eseguire il comando di aggiornamento ogni volta che devi aggiornare il codice nella tua applicazione o migrare a una versione più recente di un runtime. Ad esempio, poiché Node.js versione 8 è in modalità di manutenzione, potresti voler passare il runtime a Node.js versione 10.

Quando esegui la migrazione a una nuova versione del runtime, potresti dover modificare il codice nella tua applicazione per far sì che sia conforme alla nuova versione di runtime. Nella maggior parte dei casi, le versioni del runtime sono compatibili.
{: tip}

1. Aggiorna la tua applicazione in locale.

2. Se hai inserito la tua applicazione in un pacchetto sotto forma di immagine Docker, carica l'immagine più recente in Docker Hub. Ciò consente al sistema di estrarre la tua nuova immagine Docker la prossima volta che eseguirà il codice per la tua azione. Se hai un contenitore in esecuzione che utilizza una versione precedente della tua immagine Docker, qualsiasi nuova chiamata continua a utilizzare tale immagine. Devi eseguire il comando di aggiornamento in modo che le nuove chiamate inizino a essere eseguite sulla nuova immagine.

3. Aggiorna un'azione e includi il percorso locale alla tua applicazione o all'immagine Docker.

    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME
    ```
    {: pre}

    **Esempio**

    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10
    ```
    {: pre}

    **Output di esempio**

    ```
    ok: updated action hello
    ```
    {: screen}

    Se hai inserito il tuo codice in un pacchetto sotto forma di immagine Docker, nel comando di creazione includi `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG` invece del percorso dell'applicazione locale e dell'indicatore `--kind`. Gestisci bene le tue immagini evitando il più possibile di utilizzare la tag `latest`. Quando si utilizza la tag `latest`, viene utilizzata l'immagine che riporta quella tag, che potrebbe non essere l'ultima immagine creata. 

      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
    {: tip}
      


## Esecuzione del bind dei parametri alle azioni
{: #actions_params}

Puoi eseguire il bind dei parametri alle azioni per impostare parametri predefiniti. I parametri associati fungono da parametri predefiniti per le azioni a meno non vengano forniti al momento della chiamata.
{: shortdesc}

Prima di iniziare, [crea l'azione](#actions_cli).

Per eseguire il bind dei parametri:

1. Aggiorna un'azione ed esegui il bind dei parametri predefiniti a tale azione.

    ```
    ibmcloud fn action update ACTION_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    **Esempio**

    ```
    ibmcloud fn action update MyApp --param name World
    ```
    {: pre}

    **Output di esempio**

    ```
    ok: updated action MyApp
    ```
    {: screen}

    Se modifichi i parametri delle tue credenziali non di servizio, l'esecuzione di un comando `action update` con i nuovi parametri rimuove qualsiasi parametro attualmente esistente ma non specificato nel comando `action update`. Ad esempio, se esegui `action update -p key1 new-value -p key2 new-value` ma ometti tutti gli altri parametri che erano stati impostati, quei parametri non esisteranno più dopo l'aggiornamento dell'azione. Vengono rimossi anche tutti i servizi associati all'azione. Se hai eseguito il bind di un servizio, devi [eseguire nuovamente il bind dei servizi alla tua azione](/docs/openwhisk?topic=cloud-functions-services).
    {: tip}

3. Verifica che sia stato eseguito il bind dei parametri all'azione.

    ```
    ibmcloud fn action get MyApp parameters
    ```
    {: pre}

    **Output di esempio**

    ```
    ok: got action MyApp, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}

Facoltativo: per ripulire i parametri precedentemente sottoposti a bind, aggiornare l'azione senza includere alcun parametro.

```
ibmcloud fn action update ACTION_NAME APP_FILE
```
{: pre}


## Concatenamento di azioni come sequenze di azioni
{: #actions_seq}

Puoi creare un'azione che concatena una sequenza di azioni. Il risultato di un'azione viene passato come argomento all'azione successiva.
{: shortdesc}

```
ibmcloud fn action create SEQUENCE_NAME --sequence ACTION_1,ACTION_2
```
{: pre}

I parametri che vengono passati tra le azioni nella sequenza sono espliciti, ad eccezione dei parametri predefiniti. Pertanto, i parametri passati alla sequenza di azioni sono disponibili solo per la prima azione nella sequenza. Il risultato della prima azione della sequenza diventa l'oggetto JSON di input per la seconda azione della sequenza e così via. Questo oggetto non include nessuno dei parametri che erano stati originariamente passati alla sequenza, a meno che la prima azione non li includa nel suo risultato. I parametri passati a un'azione vengono uniti ai parametri predefiniti dell'azione. I parametri passati hanno la precedenza e sostituiscono tutti i parametri predefiniti corrispondenti.

Una sequenza non ha un timeout generale separato dai timeout di ciascuna azione all'interno della sequenza. Poiché una sequenza è una pipeline di operazioni, un errore in un'azione interrompe la pipeline. Se si verifica un timeout di una singola azione, l'intera sequenza viene chiusa con tale errore.

Successivamente, quando crei una regola o richiami le azioni, usa il nome della sequenza.


## Inserimento delle azioni in pacchetti
{: #actions_pkgs}

In {{site.data.keyword.openwhisk}}, puoi utilizzare i pacchetti per raggruppare una serie di azioni e feed correlati e condividerli con gli altri. I pacchetti consentono anche di condividere i parametri tra tutte le entità presenti nel pacchetto.
{: shortdesc}

Un pacchetto può includere *azioni* e *feed*.
- Un'azione è una parte di codice eseguita su {{site.data.keyword.openwhisk_short}}. Ad esempio, il pacchetto {{site.data.keyword.cloudant}} include azioni per la lettura e scrittura di record in un database {{site.data.keyword.cloudant_short_notm}}.
- Il feed serve a configurare un'origine eventi esterna per l'attivazione di eventi trigger. Ad esempio,il pacchetto Alarm include un feed che può attivare un trigger alla frequenza indicata.


1. Crea un pacchetto.

  ```
  ibmcloud fn package create PACKAGE_NAME
  ```
  {: pre}

2. Ottieni un riepilogo del pacchetto. Nota che il pacchetto è vuoto.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **Output di esempio**

  ```
  package /myNamespace/custom
  ```
  {: screen}

4. Creare un'azione e includila nel pacchetto. Per creare un'azione in un pacchetto devi prefissare il nome dell'azione con un nome pacchetto. La nidificazione dei pacchetti non è consentita. Un pacchetto può contenere solo azioni e non può contenere un altro pacchetto.

  ```
  ibmcloud fn package create PACKAGE_NAME/ACTION_NAME APP_FILE
  ```
  {: pre}

5. Ottieni un riepilogo del pacchetto.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **Output di esempio**

  ```
  package /NAMESPACE/PACKAGE_NAME
   action /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}




## Esecuzione del bind dei parametri ai pacchetti
{: #actions_pkgs_params}

Puoi impostare parametri predefiniti per tutte le entità di un pacchetto impostando parametri a livello di pacchetto che vengono ereditati da tutte le azioni nel pacchetto.

I parametri associati fungono da parametri predefiniti per le azioni nel pacchetto a meno che:

- L'azione stessa ha un parametro predefinito
- L'azione ha un parametro che viene fornito in fase di richiamo

Prima di iniziare, crea un pacchetto che includa almeno un'azione.

1. Aggiorna un pacchetto ed esegui il bind del parametro predefinito al pacchetto.

    ```
    ibmcloud fn package update PACKAGE_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    **Esempio**

    ```
    ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

    **Output di esempio**

    ```
    ok: updated package MyApp
    ```
    {: screen}

    Se modifichi i parametri delle tue credenziali non di servizio, l'esecuzione di un comando `package update` con i nuovi parametri rimuove qualsiasi parametro attualmente esistente, ma non specificato nel comando `package update`. Ad esempio, se esegui `package update -p key1 new-value -p key2 new-value`, ma ometti tutti gli altri parametri che erano stati impostati, quei parametri non esisteranno più dopo l'aggiornamento del pacchetto. Vengono rimossi anche tutti i servizi associati al pacchetto, quindi dopo aver aggiornato gli altri parametri devi [eseguire di nuovo il bind dei servizi al tuo pacchetto](/docs/openwhisk?topic=cloud-functions-services).
    {: tip}

3. Verifica che i parametri siano collegati al pacchetto.

    ```
    ibmcloud fn package get MyApp parameters
    ```
    {: pre}

    **Output di esempio**

    ```
    ok: got package MyApp, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}

4. Verifica che i parametri siano stati ereditati dal pacchetto.

    ```
    ibmcloud fn package get MyApp/MyAction parameters
    ```
    {: pre}

    **Output di esempio**

    ```
    ok: got package MyApp/MyAction, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}



## Condivisione di pacchetti di azioni
{: #actions_pkgs_share}

Una volta eseguiti i test e il debug delle azioni e dei feed compresi in un pacchetto, quest'ultimo può essere condiviso con tutti gli utenti {{site.data.keyword.openwhisk_short}}. La condivisione del pacchetto consente agli utenti di eseguire il bind del pacchetto, chiamare azioni del pacchetto e creare azioni sequenza e regole {{site.data.keyword.openwhisk_short}}. Le azioni e i feed di un pacchetto condiviso sono _pubblici_. Se il pacchetto è privato, lo sono anche tutti i suoi contenuti.
{: shortdesc}

1. Condividi il pacchetto con tutti gli utenti.

  ```
  ibmcloud fn package update PACKAGE_NAME --shared yes
  ```
  {: pre}

2. Visualizza la proprietà `publish` del pacchetto per verificare che ora sia true.

  ```
  ibmcloud fn package get PACKAGE_NAME publish
  ```
  {: pre}

  **Output di esempio**

  ```
  ok: got package PACKAGE_NAME, displaying field publish

  true
  ```
  {: screen}

3. Ottieni una descrizione del pacchetto per fornire agli altri il nome completo del pacchetto, cosicché possano eseguirne il bind o richiamarvi azioni. Il nome completo include lo spazio dei nomi, che in questo esempio è lo spazio dei nomi `myNamespace`.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **Output di esempio**

  ```
  package /NAMESPACE/PACKAGE_NAME
   action /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}



## Variabili di ambiente per le azioni
{: #actions_envvars}

L'ambiente dell'azione contiene diverse variabili di ambiente specifiche dell'azione in esecuzione. Le proprietà sono accessibili nell'ambiente di sistema per tutti i runtime supportati. Queste proprietà consentono alle azioni di funzionare a livello di programmazione con gli asset tramite l'API REST o di impostare un allarme interno quando l'azione sta per esaurire il suo budget di tempo assegnato.
{: shortdesc}

| Proprietà | Descrizione |
| -------- | ----------- |
| `__OW_API_HOST` | L'host API per la distribuzione che esegue questa azione. |
| `__OW_API_KEY` | La chiave API per il soggetto che richiama l'azione. Questa variabile viene fornita solo per i classici spazi dei nomi basati su CF. |
| `__OW_NAMESPACE` | L'ID spazio dei nomi (GUID). Per i classici spazi dei nomi basati su CF, questo ID viene costruito dai nomi di organizzazione e spazio. |
| `__OW_NAMESPACE_CRN` | Il nome della risorsa cloud (o [CRN](/docs/overview?topic=overview-crn)) dello spazio dei nomi. Il CRN è disponibile solo per gli spazi dei nomi abilitati a IAM
| `__OW_ACTION_NAME` | Il nome completo dell'azione in esecuzione. |
| `__OW_IAM_NAMESPACE_API_KEY` | La chiave API per gli spazi dei nomi abilitati a IAM. Vedi [Impostazione delle politiche di accesso](/docs/openwhisk?topic=cloud-functions-namespaces#namespace-access) per informazioni sull'utilizzo. |
| `__OW_IAM_API_URL` | L'endpoint del servizio utilizzato per le operazioni IAM, come il richiamo di un token dalla chiave API. Questa variabile è disponibile solo per gli spazi dei nomi abilitati a IAM |
| `__OW_ACTIVATION_ID` | L'ID di attivazione per questa istanza dell'azione in esecuzione. |
| `__OW_DEADLINE` | Il tempo approssimativo, espresso in millisecondi epoch, in cui questa azione consuma la sua intera quota di durata. |

### Incorporazione di variabili di ambiente di azione nella tua applicazione
{: #actions_envvars_app}

Per visualizzare i valori per un'azione, includine la visualizzazione nel codice della tua applicazione e visualizzali nei risultati.

**Esempio per Python**
```python
def main(dict):
  import os
  __OW_ACTION_NAME = os.environ.get('__OW_ACTION_NAME')
  result = {'__OW_ACTION_NAME':__OW_ACTION_NAME}
  return result

```
{: codeblock}

Una volta aggiornato e attivato il codice in un'azione, il risultato include il nome completo dell'azione.
```bash
"response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
                "__OW_ACTION_NAME": "/NAMESPACE/PACKAGE_NAME/ACTION_NAME"
            }

```
{: screen}



