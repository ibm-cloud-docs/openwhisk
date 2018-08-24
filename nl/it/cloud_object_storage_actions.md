---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Pacchetto Object Storage
{: #cloud_object_storage_actions}

Il pacchetto {{site.data.keyword.cos_full_notm}} fornisce una serie di azioni per interagire con le istanze {{site.data.keyword.cos_full}}. Queste azioni ti consentono di leggere, scrivere ed eliminare dai bucket che sono presenti in un'istanza {{site.data.keyword.cos_short}}.
{: shortdesc}

Il pacchetto {{site.data.keyword.cos_short}} include le seguenti azioni:

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/cloud-object-storage` | pacchetto | apikey, resource_instance_id, cos_hmac_keys.access_key_id, cos_hmac_keys.secret_access_key | Lavorare con un'istanza {{site.data.keyword.cos_full_notm}}. |
| `/cloud-object-storage/object-write` | azione | bucket, key, body | Scrivere un oggetto in un bucket. |
| `/cloud-object-storage/object-read` | azione | bucket, key | Leggere un oggetto da un bucket. |
| `/cloud-object-storage/object-delete` | azione | bucket, key | Eliminare un oggetto da un bucket. |
| `/cloud-object-storage/bucket-cors-put` | azione | bucket, corsConfig | Assegnare una configurazione CORS a un bucket. |
| `/cloud-object-storage/bucket-cors-get` | azione | bucket | Leggere la configurazione CORS di un bucket. |
| `/cloud-object-storage/bucket-cors-delete` | azione | bucket | Eliminare la configurazione CORS di un bucket. |
| `/cloud-object-storage/client-get-signed-url` | azione | bucket, key, operation | Ottenere un URL firmato per limitare la scrittura, la lettura e l'eliminazione di un oggetto da un bucket. |

## Creazione di un'istanza del servizio {{site.data.keyword.cos_full_notm}}
{: #cloud_object_storage_service_instance}

Prima di installare un pacchetto, devi richiedere un'istanza di {{site.data.keyword.cos_short}} e creare almeno un bucket.

1. [Crea un'istanza del servizio {{site.data.keyword.cos_full_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](/docs/services/cloud-object-storage/basics/order-storage.html#creating-a-new-service-instance).

2. [Crea una serie di credenziali del servizio HMAC ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials) per l'istanza del servizio {{site.data.keyword.cos_short}}. Nel campo **Aggiungi parametri di configurazione inline (facoltativo)**, aggiungi `{"HMAC":true}`.

3. [Crea almeno un bucket ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](/docs/services/cloud-object-storage/getting-started.html#create-buckets).

## Installazione del pacchetto {{site.data.keyword.cos_short}}
{: #cloud_object_storage_installation}

Una volta che disponi di un'istanza del servizio {{site.data.keyword.cos_short}}, utilizza l'UI o la CLI {{site.data.keyword.openwhisk}} per installare il pacchetto {{site.data.keyword.cos_short}} nel tuo spazio dei nomi.
{: shortdesc}

### Installazione dalla CLI {{site.data.keyword.openwhisk_short}}
{: #cloud_object_storage_cli}

Prima di iniziare:
  1. [Installa il plug-in {{site.data.keyword.openwhisk_short}} per la CLI {{site.data.keyword.Bluemix_notm}}](bluemix_cli.html#cloudfunctions_cli).
  2. Installa il comando `wskdeploy`. Vedi la [documentazione di Apache OpenWhisk![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/apache/incubator-openwhisk-wskdeploy#building-the-project).

Per installare il pacchetto {{site.data.keyword.cos_short}}:

1. Clona il repository del pacchetto {{site.data.keyword.cos_short}}.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Passa alla directory `runtimes/nodejs` o a quella `runtimes/python`. Le azioni nel pacchetto {{site.data.keyword.cos_short}} vengono
distribuite nel runtime da te scelto.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Distribuisci il pacchetto. Se successivamente decidi di eseguire le azioni in questo pacchetto nell'altro runtime, puoi ripetere il passo precedente e questo passo per ridistribuire il pacchetto.
    ```
    wskdeploy -m manifest.yaml
    ```
    {: pre}

4. Verifica che il pacchetto `cloud-object-storage` venga aggiunto al tuo elenco pacchetti.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Output:
    ```
    packages
    /myOrg_mySpace/cloud-object-storage-pkg private
    ```
    {: screen}

5. Esegui il bind delle credenziali dall'istanza {{site.data.keyword.cos_short}} da te creata al pacchetto.
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage-pkg
    ```
    {: pre}

    Output di esempio:
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage-pkg'.
    ```
    {: screen}

6. Verifica che il pacchetto sia configurato con le tue credenziali dell'istanza del servizio {{site.data.keyword.cos_short}}.
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/cloud-object-storage-pkg parameters
    ```
    {: pre}

    Output di esempio:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/cloud-object-storage-pkg, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "cloud-object-storage": {
            "apikey": "sdabac98wefuhw23erbsdufwdf7ugw",
            "credentials": "Credentials-1",
            "endpoints": "https://cos-service-s.us-south.containers.mybluemix.net/endpoints",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:cloud-object-storage:global:a/ddkgdaf89uawefoujhasdf:sd8238-sdfhwej33-234234-23423-213d::",
            "iam_apikey_name": "auto-generated-apikey-sduoiw98wefuhw23erbsdufwdf7ugw",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Reader",
            "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e::serviceid:ServiceId-sd8238-sdfhwej33-234234-23423-213d",
            "instance": "Cloud Object Storage-r1",
            "resource_instance_id": "crn:v1:staging:public:cloud-object-storage:global:a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e:sd8238-sdfhwej33-234234-23423-213d::"
          }
         }
      }
    ]
    ```
    {: screen}

### Installazione dall'IU {{site.data.keyword.openwhisk_short}}
{: #cloud_object_storage_ui}

1. Nella console {{site.data.keyword.openwhisk_short}}, vai alla [pagina Crea ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/openwhisk/create).

2. Utilizzando gli elenchi **Organizzazione Cloud Foundry** e **Spazio Cloud Foundry**, seleziona lo spazio dei nomi in cui vuoi installare il pacchetto {{site.data.keyword.cos_short}}. Gli spazi dei nomi sono formati dai nomi di organizzazione e spazio combinati.

3. Fai clic su **Installa pacchetti**.

4. Fai clic sul gruppo di pacchetto **IBM Cloud Object Storage** e fai quindi clic sul pacchetto **IBM Cloud Object Storage**.

5. Nella sezione Runtime disponibili, seleziona NodeJS o Python dall'elenco a discesa e fai clic su **Installa**.

6. Dopo che il pacchetto è stato installato, verrai reindirizzato alla pagina Azioni e puoi cercare il tuo nuovo pacchetto, che è denominato **cloud-object-storage**.

7. Per utilizzare le azioni nel pacchetto **cloud-object-storage**, devi eseguire il bind delle credenziali del servizio alle azioni.
  * Per eseguire il bind delle credenziali del servizio a tutte le azioni nel pacchetto, attieniti ai passi 5 e 6 nelle istruzioni della CLI sopra elencate.
  * Per eseguire il bind delle credenziali del servizio a singole azioni, completa la seguente procedura nell'IU. **Nota**: devi completare la seguente procedura per ogni azione che desideri utilizzare.
    1. Fai clic su un'azione dal pacchetto **cloud-object-storage** che vuoi utilizzare. Viene aperta la pagina dei dettagli per tale azione.
    2. Nella navigazione sulla sinistra, fai clic sulla sezione **Parametri**.
    3. Immetti un nuovo **parametro**. Per la chiave, immetti `__bx_creds`. Per il valore, incolla l'oggetto JSON delle credenziali del servizio dall'istanza del servizio che hai creato in precedenza.

## Scrittura in un bucket {{site.data.keyword.cos_short}}
{: #cloud_object_storage_write}

Puoi utilizzare l'azione `object-write` per scrivere un oggetto in un bucket {{site.data.keyword.cos_short}}.
{: shortdesc}

**Nota**: nella seguente procedura, il nome `testbucket` viene utilizzato come un esempio. I bucket in {{site.data.keyword.cos_full_notm}} devono essere globalmente univoci; devi pertanto sostituire `testbucket` con un nome bucket univoco.

### Scrivi in un bucket dalla CLI
{: #write_bucket_cli}

Scrivi un oggetto nel tuo bucket utilizzando l'azione `object-write`.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-write --blocking --result --param bucket testbucket --param key data.txt --param body "my_test_data"
```
{: pre}

Output di esempio:
```
{
  "body": {
      "ETag": "\"32cef9b573122b1cf8fd9aec5fdb898c\""
  },
  "bucket": "testbucket",
  "key": "data.txt"
}
```
{: screen}

### Scrivi in un bucket dall'IU
{: #write_bucket_ui}

1. Vai alla [pagina Azioni nella console {{site.data.keyword.openwhisk_short}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/openwhisk/actions).

2. Nel pacchetto `cloud-object-storage`, fai clic sull'azione **object-write**.

3. Nel riquadro Codice, fai clic su **Modifica input**.

4. Immetti un oggetto JSON che contiene il tuo bucket, la chiave e il corpo come chiavi oggetto.
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": "my_test_data"
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
      "bucket": "testbucket",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}

## Lettura da un bucket {{site.data.keyword.cos_short}}
{: #cloud_object_storage_read}

Puoi utilizzare l'azione `object-read` per leggere da un oggetto in un bucket {{site.data.keyword.cos_short}}.
{: shortdesc}

**Nota**: nella seguente procedura, il nome `testbucket` viene utilizzato come un esempio. I bucket in {{site.data.keyword.cos_full_notm}} devono essere globalmente univoci; devi pertanto sostituire `testbucket` con un nome bucket univoco.

### Leggi da un bucket dalla CLI
{: #read_bucket_cli}

Leggi da un oggetto nel tuo bucket utilizzando l'azione `object-read`.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket testbucket --param key data.txt
```
{: pre}

Output di esempio:
```
{
  "body": "my_test_data",
  "bucket": "testbucket,
  "key": "data.txt"
}
```
{: screen}

### Leggi da un bucket dall'IU
{: #read_bucket_ui}

1. Vai alla [pagina Azioni nella console {{site.data.keyword.openwhisk_short}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/openwhisk/actions).

2. Nel pacchetto `cloud-object-storage`, fai clic sull'azione **object-read**.

3. Nel riquadro Codice, fai clic su **Modifica input**.

4. Immetti un oggetto JSON che contiene il tuo bucket e la chiave come chiavi oggetto.
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
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
      "bucket": "testbucketeweit",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}
