---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: object storage, bucket, package, functions

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
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Object Storage
{: #pkg_obstorage}

Puoi ampliare la funzionalità della tua applicazione {{site.data.keyword.openwhisk}} integrandola con un'istanza {{site.data.keyword.cos_full}}.

**Prima di iniziare**
Per saperne di più su {{site.data.keyword.cos_full_notm}}, vedi [Informazioni su Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api). Per ulteriori informazioni sulla configurazione dell'istanza {{site.data.keyword.cos_full_notm}}, vedi [Provisioning di un'istanza {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#gs-dev-provision).

## Pacchetti
{: #obstorage_packages}
| Pacchetto | Disponibilità | Descrizione |
| --- | --- | --- |
| [Pacchetto {{site.data.keyword.cos_full_notm}}](#pkg_obstorage_install)| Installabile | Leggi, scrivi ed elimina da un'istanza{{site.data.keyword.cos_full_notm}}. |
| [(Sperimentale) Origine eventi {{site.data.keyword.cos_full_notm}}](#pkg_obstorage_ev) | Preinstallato (solo Stati Uniti Sud) | Resta in ascolto delle modifiche a un'istanza {{site.data.keyword.cos_full_notm}}. |

## Creazione di un'istanza del servizio IBM Cloud Object Storage
{: #pkg_obstorage_service}

Prima di poter usare un pacchetto, devi richiedere un'istanza di {{site.data.keyword.cos_full_notm}} e creare almeno un bucket.

1. [Crea un'istanza del servizio {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-gs-dev#gs-dev-provision).

2. [Crea una serie di credenziali del servizio HMAC](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) per l'istanza del servizio {{site.data.keyword.cos_full_notm}}. Nel campo **Aggiungi parametri di configurazione inline (facoltativo)**, aggiungi `{"HMAC":true}`.

3. [Crea almeno un bucket](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets).

## Installazione del pacchetto {{site.data.keyword.cos_full_notm}} dalla CLI {{site.data.keyword.openwhisk_short}}
{: #pkg_obstorage_install}

Una volta che disponi di un'istanza del servizio {{site.data.keyword.cos_full_notm}}, puoi utilizzare l'IU o la CLI {{site.data.keyword.openwhisk}} per installare il pacchetto {{site.data.keyword.cos_full_notm}} nel tuo spazio dei nomi.
{: shortdesc}
{: #pkg_obstorage_cli}

Il pacchetto {{site.data.keyword.cos_full_notm}} installabile distribuisce una serie di azioni che puoi utilizzare per lavorare con la tua istanza {{site.data.keyword.cos_full_notm}}. Queste vengono eseguite in Node.js o Python. Dopo aver installato il pacchetto, puoi selezionare un runtime. Per un elenco delle azioni disponibili nel pacchetto `cloud-object-storage`, vedi [Entità disponibili](#pkg_obstorage_actions).
{: note}

**Prima di iniziare**

[Installa il plug-in {{site.data.keyword.openwhisk_short}} per la CLI {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Per installare il pacchetto {{site.data.keyword.cos_full_notm}}:

1. Clona il repository del pacchetto {{site.data.keyword.cos_full_notm}}.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Passa alla directory `runtimes/nodejs` o `runtimes/python` per selezionare un runtime per le azioni nel pacchetto.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

4. Distribuisci il pacchetto `cloud-object-storage`.
    ```
    ibmcloud fn deploy
    ```
    {: pre}

    **Risposta**
    ```
    Success: Deployment completed successfully.
    ```
    {: scree}

5. Verifica che il pacchetto `cloud-object-storage` venga aggiunto al tuo elenco pacchetti.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Output**
    ```
    packages
    /<org_space>/cloud-object-storage         private
    ```
    {: screen}

6. Esegui il bind delle credenziali dall'istanza {{site.data.keyword.cos_full_notm}} da te creata al pacchetto. Puoi includere l'indicatore `--keyname` per eseguire il bind di specifiche credenziali di servizio. Per ulteriori informazioni sul bind dei servizi, vedi [Comandi servizi](/docs/cloud-functions-cli-plugin?topic=cloud-functions-cli-plugin-functions-cli#cli_service).

    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage --keyname `<service_key>`
    ```
    {: pre}

    **Output di esempio**
      ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

7. Verifica che il pacchetto sia configurato con le tue credenziali dell'istanza del servizio {{site.data.keyword.cos_full_notm}}.
    ```
    ibmcloud fn package get /<org_space>/cloud-object-storage parameters
    ```
    {: pre}

    **Output di esempio**
      ```
    ok: got package /<org_space>/cloud-object-storage, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "cloud-object-storage": {
            "apikey": "sdabac98wefuhw23erbsdufwdf7ugw",
            "credentials": "Credentials-1",
            "endpoints": "https://cos-service.bluemix.net/endpoints",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:cloud-object-storage:global:a/ddkgdaf89uawefoujhasdf:sd8238-sdfhwej33-234234-23423-213d::",
            "iam_apikey_name": "auto-generated-apikey-sduoiw98wefuhw23erbsdufwdf7ugw",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Reader",
            "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e::serviceid:ServiceId-sd8238-sdfhwej33-234234-23423-213d",
            "instance": "Cloud Object Storage-r1",
            "resource_instance_id": "crn:v1:staging:public:cloud-object-storage:global:a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e:sd8238-sdfhwej33-234234-23423-213d::"
          }
         }
      }
    ]
    ```
    {: screen}

## Bind dei parametri

Puoi utilizzare il comando [`package update`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_pkg_bind) per eseguire il bind dell'endpoint di un bucket a un'azione specifica o al pacchetto `cloud-object-storage`. Sostituisci `<bucket_endpoint>` con l'endpoint del tuo bucket.

Quando aggiorni i parametri per un pacchetto, un'azione o un trigger, devi specificare tutti i parametri creati in precedenza. Altrimenti, i parametri precedentemente creati vengono rimossi. Vengono rimossi anche tutti i servizi associati al pacchetto, quindi dopo aver aggiornato gli altri parametri devi eseguire di nuovo il [bind dei servizi](/docs/openwhisk?topic=cloud-functions-services) al tuo pacchetto.
{: important}

**Esegui il bind dei parametri a tutte le azioni in un pacchetto**
```
ibmcloud fn package update cloud-object-storage --param endpoint <bucket_endpoint>
```
{: pre}

**Esegui il bind dei parametri a un'azione specifica**
```
ibmcloud fn action update cloud-object-storage/object-write --param endpoint <bucket_endpoint>
```
{: pre}

Per eseguire il bind dei parametri alle azioni, puoi anche utilizzare la scheda **Parametri** nell'IU. Per aggiungere i parametri nell'IU, vai alla [pagina **Azioni**](https://cloud.ibm.com/openwhisk/actions){: external} e fai clic su una delle tue azioni. Quindi, fai clic su **Parametri** > **Aggiungi parametri**. Devi aggiungere i parametri in coppie di `<key>` e `<value>`.
{: tip}


## Installazione del pacchetto {{site.data.keyword.cos_full_notm}} dall'IU {{site.data.keyword.openwhisk_short}}
{: #pkg_obstorage_ui}

1. Nella console {{site.data.keyword.openwhisk_short}}, vai alla [pagina di creazione](https://cloud.ibm.com/openwhisk/create){: external}.

2. Seleziona lo spazio dei nomi in cui vuoi installare il pacchetto {{site.data.keyword.cos_full_notm}} utilizzando il menu a discesa dello spazio dei nomi

3. Fai clic su **Installa pacchetti**.

4. Fai clic sul gruppo di pacchetti **IBM Cloud Object Storage** e fai quindi clic sul pacchetto **IBM Cloud Object Storage**.

5. Nella sezione **Runtime disponibili**, seleziona `Node.JS` o `Python` dall'elenco a discesa. Quindi, fai clic su **Installa**.

6. Una volta installato il pacchetto, vieni reindirizzato alla pagina **Azioni** e puoi cercare il tuo nuovo pacchetto, che è denominato `cloud-object-storage`.

7. Per utilizzare le azioni nel pacchetto `cloud-object-storage`, devi eseguire il bind delle credenziali del servizio alle azioni.
  * Per eseguire il bind delle credenziali del servizio a tutte le azioni nel pacchetto, segui il passo 5 nelle [istruzioni della CLI](#pkg_obstorage_cli).
  * Per eseguire il bind delle credenziali del servizio a singole azioni, completa la seguente procedura nell'IU. 
  
Se esegui il bind delle tue credenziali del servizio a singole azioni, devi completare la seguente procedura per ogni azione che vuoi utilizzare.
{: note}

1. Fai clic su un'azione dal pacchetto `cloud-object-storage` che vuoi utilizzare. Viene aperta la pagina dei dettagli per tale azione.
2. Nella navigazione sulla sinistra, fai clic su **Parametri**.
3. Immetti un nuovo parametro. Per la chiave, immetti `__bx_creds`. Per il valore, incolla l'oggetto JSON delle credenziali del servizio dall'istanza del servizio che hai creato in precedenza.


Per ulteriori informazioni sul bind dei parametri, vedi [Esecuzione del bind dei parametri ai pacchetti](/docs/openwhisk?topic=cloud-functions-actions#actions_pkgs_params).


## Entità disponibili
{: #pkg_obstorage_actions}

Il pacchetto {{site.data.keyword.cos_full_notm}} include le seguenti azioni:

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/cloud-object-storage` | Pacchetto | `apikey`, `resource_instance_id`, `cos_hmac_keys.access_key_id`, `cos_hmac_keys.secret_access_key` | Lavorare con un'istanza {{site.data.keyword.cos_full_notm}}. |
| `/cloud-object-storage/object-write` | Azione | `bucket`, `key`, `body`, `endpoint`, `ibmAuthEndpoint` | Scrivere un oggetto in un bucket. |
| `/cloud-object-storage/object-read` | Azione | `bucket`, `key`, `endpoint`, `ibmAuthEndpoint` | Leggere un oggetto da un bucket. |
| `/cloud-object-storage/object-delete` | Azione | `bucket`, `key`, `endpoint`, `ibmAuthEndpoint` | Eliminare un oggetto da un bucket. |
| `/cloud-object-storage/bucket-cors-put` | Azione | `bucket`, `corsConfig`, `endpoint`, `ibmAuthEndpoint` | Assegnare una configurazione CORS a un bucket. |
| `/cloud-object-storage/bucket-cors-get` | Azione | `bucket`, `endpoint`, `ibmAuthEndpoint` | Leggere la configurazione CORS di un bucket. |
| `/cloud-object-storage/bucket-cors-delete` | Azione | `bucket`, `endpoint`, `ibmAuthEndpoint` | Eliminare la configurazione CORS di un bucket. |
| `/cloud-object-storage/client-get-signed-url` | Azione | `bucket`, `key`, `operation`, `expires`, `endpoint`, `ibmAuthEndpoint` | Ottenere un URL firmato per limitare la scrittura, la lettura e l'eliminazione di un oggetto da un bucket. |

Per ottenere un elenco completo delle entità disponibili, esegui `ibmcloud fn package get cloud-object-storage`.
{: note}

### Parametri pacchetto
{: #pkg_obstorage_pkgparams}

I seguenti parametri di pacchetto devono essere associati al pacchetto e sono automaticamente disponibili per tutte le azioni. È anche possibile specificare questi parametri quando richiami una delle azioni.

| Parametro pacchetto | Descrizione |
| --- | --- |
| `apikey` | Il parametro `apikey ` è una chiave API IAM per l'istanza {{site.data.keyword.cos_full_notm}}. |
| `resource_instance_id` | Il parametro `resource_instance_id` è l'identificativo dell'istanza {{site.data.keyword.cos_full_notm}}. |
| `cos_hmac_keys` | Il parametro `cos_hmac_keys` rappresenta la credenziali HMAC dell'istanza {{site.data.keyword.cos_full_notm}}, che includono i valori `access_key_id` e `secret_access_key`.  Queste credenziali vengono utilizzate esclusivamente dall'azione `client-get-signed-url`.  Fai riferimento a [Utilizzo delle credenziali IAM](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials) per le istruzioni su come generare le credenziali HMAC per la tua istanza {{site.data.keyword.cos_full_notm}}. |
 
### Parametri azione
{: #pkg_obstorage_actparams}

I seguenti parametri di azione vengono specificati quando richiami le singole azioni.  Non tutti questi parametri sono supportati da ogni azione. Fai riferimento alla tabella [Entità disponibili](#pkg_obstorage_actions) per vedere i parametri supportati da ogni azione.

| Parametro azione | Descrizione |
| --- | --- |
| `bucket` | Il parametro `bucket` è il nome del bucket {{site.data.keyword.cos_full_notm}}. |
| `endpoint` | Il parametro `endpoint` è l'endpoint {{site.data.keyword.cos_full_notm}} utilizzato per connetterti alla tua istanza {{site.data.keyword.cos_full_notm}}. Puoi localizzare il tuo endpoint nella [documentazione {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints). |
| `expires` | Il parametro `expires` è il numero di secondi per la scadenza dell'operazione dell'URL pre-firmato.  Il valore `expires` predefinito è 15 minuti. |
| `ibmAuthEndpoint` | Il parametro `ibmAuthEndpoint ` è l'endpoint di autorizzazione IBM Cloud utilizzato da {site.data.keyword.cos_short}} per generare un token dalla `apikey`. L'endpoint di autorizzazione predefinito funziona per tutte le regioni IBM Cloud. |
| `key` | Il parametro `key` è la chiave dell'oggetto bucket. |
| `operation` | Il parametro `operation` è l'operazione dell'URL pre-firmato da richiamare. |
| `corsConfig` | Il parametro `corsConfig` è la configurazione CORS di un bucket. |


## Scrittura di oggetti in un bucket
{: #pkg_obstorage_write}

Puoi utilizzare l'azione `object-write` per scrivere un oggetto in un bucket {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

Nella seguente procedura, il nome `test-bucket` viene utilizzato come esempio. I bucket in {{site.data.keyword.cos_full_notm}} devono essere univoci a livello globale, pertanto devi sostituire `test-bucket` con un nome bucket univoco.
{: note}

### Scrittura di un oggetto in un bucket utilizzando la CLI
{: #pkg_obstorage_write_cli}
Scrivi un oggetto nel tuo bucket utilizzando l'azione `object-write`.
{: shortdesc}


Richiama l'azione `object-write` per scrivere un oggetto nel tuo bucket. Se hai associato l'endpoint del bucket al tuo pacchetto o all'azione `object-write`, non è necessario includere l'endpoint come parametro. Sostituisci `<org_space>` con il nome della tua organizzazione e del tuo spazio Cloud Foundry, `<test-bucket>` con il nome del tuo bucket e `<test.txt>` con il nome dell'oggetto che vuoi scrivere.

```
ibmcloud fn action invoke /<org_space>/cloud-object-storage/object-write --blocking --result --param bucket <test-bucket> --param key <test.txt> --param body <test> --param endpoint <bucket_endpoint>
```
{: pre}

**Output di esempio**

```
{
  "body": {
      "ETag": "\"32cef9b573122b1cf8fd9aec5fdb898c\""
  },
  "bucket": "test-bucket",
  "key": "test.txt"
}
```
{: screen}

### Scrittura di un oggetto in un bucket utilizzando l'IU
{: #pkg_obstorage_write_ui}


1. Vai alla [pagina Azioni](https://cloud.ibm.com/openwhisk/actions){: external} nella console{{site.data.keyword.openwhisk_short}}.

2. Nel pacchetto `cloud-object-storage`, fai clic sull'azione **object-write**.

3. Nel riquadro Codice, fai clic su **Modifica input**.

4. Immetti un oggetto JSON che contiene il tuo bucket, la chiave e il corpo come chiavi oggetto.
    ```
    {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": "test"
    }
    ```
    {: pre}

5. Fai clic su **Salva**.

6. Fai clic su **Richiama**.

7. Verifica che l'output sia simile al seguente:
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}

## Lettura di oggetti da un bucket
{: #pkg_obstorage_read}

Puoi utilizzare l'azione `object-read` per leggere da un oggetto in un bucket {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

Nella seguente procedura, il nome `test-bucket` viene utilizzato come esempio. I bucket in {{site.data.keyword.cos_full_notm}} devono essere univoci a livello globale, pertanto devi sostituire `test-bucket` con un nome bucket univoco.
{: note}

### Lettura di un oggetto da un bucket utilizzando la CLI
{: #pkg_obstorage_read_cli}

Leggi da un oggetto nel tuo bucket utilizzando l'azione `object-read`.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket test-bucket --param key data.txt
```
{: pre}

**Output di esempio**
```
{
  "body": "test",
  "bucket": "test-bucket,
  "key": "data.txt"
}
```
{: screen}

### Lettura di un oggetto da un bucket utilizzando l'IU
{: #pkg_obstorage_read_ui}

1. Vai alla [pagina Azioni](https://cloud.ibm.com/openwhisk/actions){: external}.

2. Nel pacchetto `cloud-object-storage`, fai clic sull'azione `object-read`.

3. Nel riquadro Codice, fai clic su **Modifica input**.

4. Immetti un oggetto JSON che contiene il tuo bucket e la chiave come chiavi oggetto.
    ```
    {
      "bucket": "test-bucket",
      "key": "test.txt",
    }
    ```
    {: pre}

5. Fai clic su **Salva**.

6. Fai clic su **Richiama**.

7. Verifica che l'output sia simile al seguente:
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}


## Ascolto delle modifiche a un bucket utilizzando l'origine eventi Object Storage (sperimentale)
{: #pkg_obstorage_ev}

Il pacchetto `/whisk.system/cos-experimental` potrebbe essere instabile, venire modificato frequentemente in modi non compatibili con le versioni precedenti e potrebbe essere sospeso con breve preavviso. Questo pacchetto non è consigliato per l'utilizzo in ambienti di produzione. Questo pacchetto sperimentale è disponibile solo nella regione Stati Uniti Sud.
{: important}

Puoi utilizzare {{site.data.keyword.openwhisk}} per restare in ascolto delle modifiche a un [bucket {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations) e utilizzare un'azione per elaborare uno o più oggetti dal bucket.

<br>

**Caso di utilizzo di esempio:** con il pacchetto `/whisk.system/cos-experimental`, puoi ascoltare le modifiche ai dati stradali del GPS memorizzati in un bucket {{site.data.keyword.cos_full_notm}}. Quindi, quando si verificano delle modifiche, puoi attivare la rigenerazione automatica di una mappa GPS in modo che gli utenti possano avere accesso ai dati stradali più recenti per la loro applicazione GPS.

### Parametri dell'origine eventi Object Storage (sperimentale)
{: #pkg_obstorage_ev_ch}

Con il pacchetto `/whisk.system/cos-experimental` puoi configurare gli eventi da un'istanza {{site.data.keyword.cos_full_notm}}; tale pacchetto include il seguente feed:

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` | Pacchetto | `apikey`, `auth_endpoint`, `bucket`, `endpoint`, `interval` | Pacchetto che contiene l'azione feed `changes`. |
| `/whisk.system/cos-experimental/changes` | Feed | `apikey`, `auth_endpoint`, `bucket`, `endpoint`, `interval` | Attivare eventi trigger in caso di modifiche a un bucket {{site.data.keyword.cos_full_notm}}. |
{: shortdesc}

Puoi utilizzare il feed `changes` per configurare il servizio di origine degli eventi {{site.data.keyword.cos_full_notm}} per attivare un trigger per ogni modifica a un bucket nella tua istanza {{site.data.keyword.cos_full_notm}}.

Parametri utilizzati in questo esempio:

| Parametro | Descrizione |
| --- | --- |
| `apikey` | (Obbligatorio, a meno che non sia associato al pacchetto). Il parametro `apikey ` è una chiave API IAM per l'istanza {{site.data.keyword.cos_full_notm}}.  Normalmente questo valore viene associato al pacchetto. Tuttavia, se il valore `apikey` viene specificato quando utilizzi l'azione feed `changes`, il valore specificato viene utilizzato per le credenziali invece dell'apikey delle credenziali associate. |
| ` auth_endpoint` | (Facoltativo). Il parametro `auth_endpoint` è l'endpoint di autorizzazione utilizzato da {{site.data.keyword.cos_full_notm}} per generare un token dalla `apikey`.  L'endpoint predefinito è quello di {{site.data.keyword.cloud}}. |
| `bucket` | (Obbligatorio). Il parametro `bucket` è il nome del bucket {{site.data.keyword.cos_full_notm}}. |
| `endpoint` | (Obbligatorio). Il parametro `endpoint` è l'endpoint {{site.data.keyword.cos_full_notm}} utilizzato per la connessione alla tua istanza {{site.data.keyword.cos_full_notm}}. Puoi localizzare il tuo endpoint nella [documentazione {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints). |
| `interval` | (Facoltativo). Il parametro `interval` è l'intervallo di polling del bucket, in minuti completi. Il valore di `interval` deve essere almeno 1 minuto e viene impostato su 1 minuto per impostazione predefinita. |

## Creazione di un trigger per rispondere al feed changes
{: #pkg_obstorage_ev_trig}

Quando crei il trigger, puoi evitare di passare le tue credenziali {{site.data.keyword.cos_full_notm}} all'azione feed `changes` associando le tue credenziali direttamente al pacchetto `cos-experimental`.
{: shortdesc}

 1. Per prima cosa, crea un'associazione del pacchetto che può essere modificata per contenere le tue credenziali. Il seguente comando crea un'associazione del pacchetto, `myCosPkg`, nel tuo spazio dei nomi.

  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}

 2. Esegui il bind delle tue credenziali {{site.data.keyword.cos_full_notm}} al pacchetto. L'esecuzione del bind delle tue credenziali {{site.data.keyword.cos_full_notm}} al pacchetto associa il valore `apikey` al pacchetto, per cui non devi specificare il valore `apikey` quando viene richiamata l'azione di feed `changes`.

  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}

 3. Crea un trigger denominato `cosTrigger` con il feed `changes` nel bind di pacchetto che hai creato. Utilizza il tuo nome del bucket e i valori di parametro dell'endpoint {{site.data.keyword.cos_full_notm}}.

  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
--param bucket myBucket \
--param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}

  **Output di esempio**

    ```
    ok: created trigger feed cosTrigger
    ```
  {: pre}
 

Crea un'azione per verificare che il trigger, il feed di modifica e la regola siano tutti configurati e funzionino correttamente.
 
  1. Salva il seguente codice JavaScript come `cosChange.js`. 

  ```javascript
  function main(data) {
      console.log(data);
  }
  ```
  {: codeblock}

  2. Crea un'azione denominata `cosChange` utilizzando il codice `cosChange.js`.

  ```
  ibmcloud fn action create cosChange <filepath>/cosChange.js
  ```
  {: pre}

  3. Crea una regola per connettere l'azione `cosChange` al trigger `cosTrigger`.

  ```
  ibmcloud fn rule create cosRule cosTrigger cosChange
  ```
  {: pre}

  4. In una finestra distinta, avvia il polling delle attivazioni per dare una chiara visibilità di ciò che sta accadendo. Quando il trigger si attiva e l'azione viene eseguita, questo comando elenca i record di attivazione per ciascuna di queste operazioni nel momento in cui si verificano.

  ```
  ibmcloud fn activation poll
  ```
  {: pre}
  
  5. Nel tuo dashboard {{site.data.keyword.cos_full_notm}}, modifica un oggetto bucket esistente oppure creane uno. Per ulteriori informazioni su come aggiungere un oggetto al tuo bucket, vedi [Aggiungi alcuni oggetti al tuo bucket](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-add-objects).
  
  6. Per ogni modifica all'oggetto bucket, osserva le nuove attivazioni per il trigger `cosTrigger` e l'azione `cosChange`. Puoi visualizzare queste attivazioni eseguendo il comando `ibmcloud fn activation poll` all'interno dell'intervallo di polling del bucket configurato.
  
  7. Se non riesci ad osservare le nuove attivazioni, verifica che i valori di parametro `apikey`, `endpoint` e `bucket` siano corretti.
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}


### Struttura dei dati di un'attivazione del trigger Object Storage
{: #pkg_obstorage_ev_data}

Il contenuto degli eventi generati include i seguenti parametri:

| Parametro | Descrizione |
| --- | --- |
| `file` | I metadati del file o dell'oggetto. Questa struttura è descritta in [Elenco di oggetti in un bucket specifico](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations#compatibility-api-list-buckets). |
| `status` | La modifica rilevata.  Questo valore è `added`, `modified` o `deleted`. |
| `bucket`| Il nome del bucket {{site.data.keyword.cos_full_notm}}. |
| `endpoint` | L'endpoint {{site.data.keyword.cos_full_notm}} utilizzato per la connessione all'istanza {{site.data.keyword.cos_full_notm}}. |
| `key` | L'identificativo dell'oggetto bucket modificato. Questo valore è lo stesso di `file.Key`, ma disponibile all'inizio del JSON dell'evento trigger. |

**Rappresentazione JSON di esempio dell'attivazione del trigger di modifica del bucket**

```json
{
  "file": {
    "ETag": "\"fb47672a6f7c34339ca9f3ed55c6e3a9\"",
    "Key": "file-86.txt",
    "LastModified": "2018-12-19T08:33:27.388Z",
    "Owner": {
      "DisplayName": "80a2054e-8d16-4a47-a46d-4edf5b516ef6",
      "ID": "80a2054e-8d16-4a47-a46d-4edf5b516ef6"
    },
    "Size": 25,
    "StorageClass": "STANDARD"
  },
  "status": "added",
  "bucket": "myBucket",
  "endpoint": "s3.us-south.cloud-object-storage.appdomain.cloud",
  "key": "file-86.txt"
}
```
{: codeblock}

## Creazione di un'azione per elaborare un oggetto modificato
{: #pkg_obstorage_ev_act}

Puoi creare una singola azione che richiama ed elabora l'oggetto. Oppure puoi creare una sequenza che utilizza un'azione per richiamare l'oggetto e un'altra azione per elaborarlo.

### Creazione di un'azione per richiamare ed elaborare l'oggetto
{: #pkg_obstorage_ev_act_ret}

Questo codice dell'azione di esempio richiama ed elabora il documento di notifica di modifica del bucket. Puoi passare i parametri `apikey` e `serviceInstanceId` direttamente all'azione durante la chiamata manuale dell'azione, ma quando questa azione viene richiamata da un trigger, questi valori devono essere ottenuti da {{site.data.keyword.cos_full_notm}}, che deve essere associato all'azione con il comando `ibmcloud fn service bind`.

1. Salva il seguente codice in un file .zip denominato `myCosAction.zip`.

  ```javascript
  const COS = require('ibm-cos-sdk')

  function main(params){
  const apikey = params.apikey || params.__bx_creds['cloud-object-storage'].apikey
  const serviceInstanceId = params.serviceInstanceId || params.__bx_creds['cloud-object-storage'].resource_instance_id
  const ibmAuthEndpoint = params.ibmAuthEndpoint
  const endpoint = params.endpoint
  const bucket = params.bucket
  const file = params.key

    const cos_config = { endpoint: endpoint, apiKeyId: apikey, ibmAuthEndpoint: ibmAuthEndpoint, serviceInstanceId: serviceInstanceId }
  const client = new COS.S3(cos_config);

    return new Promise(function(resolve, reject) {
      client.getObject({ Bucket: bucket, Key: file }, (err, results) => {
        if (err != null) {
          console.log(err)
        reject({ err: err })
        } else {
          console.log(results)
        resolve({ contents: Buffer.from(results.Body).toString() })
      }
      })
    });
}
exports.main = main;
  ```
  {: codeblock}

  Poiché questa azione utilizza il pacchetto NPM `ibm-cos-sdk`, l'azione deve essere impacchettata come [modulo Node.js](/docs/openwhisk?topic=cloud-functions-prep#prep_js_npm) o come [singolo bundle](/docs/openwhisk?topic=cloud-functions-prep#prep_js_pkg).

2. Crea l'azione per richiamare ed elaborare l'oggetto da {{site.data.keyword.cos_full_notm}}:

  ```
  ibmcloud fn action create myCosAction <filepath>/myCosAction.zip --kind nodejs:10
  ```
  {: pre}



### Creazione di una sequenza di azioni per richiamare ed elaborare l'oggetto
{: #pkg_obstorage_ev_act_seq}

Invece di includere il codice di richiamo dell'oggetto nella tua azione, puoi utilizzare l'azione `object-read` dal pacchetto `cloud-object-storage`, che deve essere [installato manualmente](#pkg_obstorage_install).  Il tuo codice di azione deve elaborare solo i risultati restituiti da `object-read`.
{: shortdesc}

Per creare un'azione che elabora solo l'oggetto bucket:

1. Salva il seguente codice come `myCosAction.js`.

  ```javascript
  function main(data) {
    if (data) {
      // Process the object
  }
  }
  ```
  {: codeblock}

2. Crea l'azione per elaborare solo l'oggetto da {{site.data.keyword.cos_full_notm}}.

  ```
  ibmcloud fn action create myCosProcessObjectAction <filepath>/myCosAction.js
  ```
  {: pre}

3. Esegui il bind delle tue credenziali {{site.data.keyword.cos_full_notm}} al tuo pacchetto`cloud-object-storage` installato manualmente.

  ```
  ibmcloud fn service bind cloud-object-storage cloud-object-storage
  ```
  {: pre}

4. L'azione `object-read` può essere composta con `myCosProcessObjectAction` per creare una sequenza di azioni.
  ```
  ibmcloud fn action create myCosAction --sequence cloud-object-storage/object-read,myCosProcessObjectAction
  ```
  {: pre}

Oltre all'azione `object-read`, puoi utilizzare le altre azioni incluse nel pacchetto `cloud-object-storage` installabile.
{: tip}

Per ottenere un elenco di entità disponibili, immetti il seguente comando.
```
ibmcloud fn package get cloud-object-storage
```
{: pre}

### Esecuzione del bind delle credenziali alla tua azione
{: #pkg_obstorage_ev_bind}

Puoi evitare di passare credenziali sensibili durante la chiamata eseguendo il bind delle tue credenziali {{site.data.keyword.cos_full_notm}} all'azione utilizzando il seguente comando:
  ```
  ibmcloud fn service bind cloud-object-storage myCosAction
  ```
  {: pre}

### Creazione di una regola per associare l'azione al trigger della modifica
{: #pkg_obstorage_ev_rule}

Puoi utilizzare un'azione o una sequenza di azioni in una [regola](/docs/openwhisk?topic=cloud-functions-rules) per recuperare ed elaborare l'oggetto associato a un evento di modifica {{site.data.keyword.cos_full_notm}}.

Crea una regola che attiva l'azione `MyCosAction` sui nuovi eventi di trigger {{site.data.keyword.cos_full_notm}}.
  ```
  ibmcloud fn rule create myRule myCosTrigger myCosAction
  ```
  {: pre}



