---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: cloudant, event, action, trigger, sequence

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


# Cloudant
{: #pkg_cloudant}

Il pacchetto `/whisk.system/cloudant` preinstallato ti consente di utilizzare un database[{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started). L'uso di questo pacchetto non richiede un bind dei servizi.


## Feed e azioni disponibili
{: #cloudant_available}

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | pacchetto | dbname, host, username, password | Lavorare con un database Cloudant. |
| `/whisk.system/cloudant/read` | azione | dbname, id | Leggere un documento da un database. |
| `/whisk.system/cloudant/write` | azione | dbname, overwrite, doc | Scrivere un documento in un database. |
| `/whisk.system/cloudant/changes` | feed | dbname, iamApiKey, iamUrl, filter, query_params, maxTriggers | Attivare eventi trigger in caso di modifiche a un database. |
{: shortdesc}

### Configurazione di un database {{site.data.keyword.cloudant_short_notm}} in {{site.data.keyword.Bluemix_notm}}
{: #cloudant_db}

Se stai utilizzando {{site.data.keyword.openwhisk}} da {{site.data.keyword.Bluemix_notm}}, puoi utilizzare il {{site.data.keyword.openwhisk}} plug-in della CLI per eseguire il bind di un servizio a un'azione o a un pacchetto.

Devi prima creare manualmente un bind di pacchetto per il tuo account {{site.data.keyword.cloudant_short_notm}}.

1. Crea un bind di pacchetto configurato per il tuo account {{site.data.keyword.cloudant_short_notm}}.
  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. Verifica che il bind di pacchetto esista.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Output di esempio:
  ```
  packages
  /myNamespace/myCloudant private
  ```
  {: screen}

3. Ottieni il nome dell'istanza del servizio di cui desideri eseguire il bind a un'azione o un pacchetto.
    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    Output di esempio:
    ```
    Name          Location   State    Type
    Cloudant-gm   us-south   active   service_instance
    ```
    {: screen}

4. Ottieni il nome delle credenziali definite per l'istanza del servizio che hai ottenuto nel passo precedente.
    ```
    ibmcloud resource service-keys --instance-name Cloudant-gm
    ```
    {: pre}

    Output di esempio:
    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. Bind del servizio al pacchetto che hai creato nel passo 1.
    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1'
    ```
    {: pre}

6. Verifica che le credenziali siano state associate correttamente.
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    Output di esempio:
    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
        {
                "key": "bluemixServiceName",
                "value": "cloudantNoSQLDB"
            },
            {
                "key": "apihost",
                "value": "us-south.functions.cloud.ibm.com"
            },
            {
                "key": "__bx_creds",
                "value": {
                    "cloudantnosqldb": {
                        "apikey": "[Service apikey]",
                        "credentials": "Service credentials-1",
                        "iam_apikey_description": "[Service description]",
                        "iam_apikey_name": "[Service apikey name]",
                        "iam_role_crn": "[Service role crn]",
                        "iam_serviceid_crn": "[Service id crn]",
                        "instance": "Cloudant-gm",
                        "url": "[Service url]",
                        "username": "[Service username]"
                    }
                }
            }
        ],
    }
    ```
    {: screen}

    In questo esempio, le credenziali per il servizio Cloudant appartengono a un pacchetto denominato `__bx_creds`.


### Lettura da un database {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_read}

Puoi utilizzare un'azione per recuperare un documento da un database {{site.data.keyword.cloudant_short_notm}} denominato **testdb**. Assicurati che questo database esista nel tuo account {{site.data.keyword.cloudant_short_notm}}.

- Recupera un documento utilizzando l'azione **read** nel bind di pacchetto che hai creato in precedenza. Assicurati di sostituire `/_/myCloudant` con il tuo nome pacchetto.
  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}

  Output di esempio:
  ```
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```
  {: screen}

### Scrittura in un database {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_write}

Puoi utilizzare un'azione per memorizzare un documento in un database {{site.data.keyword.cloudant_short_notm}} denominato **testdb**. Assicurati che questo database esista nel tuo account {{site.data.keyword.cloudant_short_notm}}.

1. Memorizza un documento utilizzando l'azione **write** nel bind di pacchetto che hai creato in precedenza. Assicurati di sostituire `/_/myCloudant` con il tuo nome pacchetto.
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  {: pre}

  Output di esempio:
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. Verifica che il documento esista cercandolo nel tuo dashboard {{site.data.keyword.cloudant_short_notm}}.

  L'URL del dashboard per il database **testdb** ha un aspetto simile al seguente: `https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100`.


### Crea un trigger utilizzando la funzione di filtro
{: #cloudant_trigger}

Puoi utilizzare il feed `changes` per configurare un servizio in modo da attivare un trigger ogni volta che viene apportata una modifica al tuo database {{site.data.keyword.cloudant_short_notm}}.

I parametri utilizzati in questo esempio sono i seguenti:

**dbname**: il nome del database {{site.data.keyword.cloudant_short_notm}} _(obbligatorio)_.

**iamApiKey**: la chiave API IAM per il database Cloudant.  Se specificata sarà utilizzata per le credenziali invece di nome utente e password _(facoltativo)_.

**iamUrl**: l'URL del servizio token IAM che viene utilizzato quando viene specificato `iamApiKey`.  L'impostazione predefinita è `https://iam.bluemix.net/identity/token` _(facoltativo)_.

**maxTriggers**: arresta l'attivazione dei trigger quando viene raggiunto questo limite _(facoltativo)_. Il valore predefinito è infinito.

**filter**: funzione di filtro definita su un documento di progettazione _(facoltativo)_

**query_params**: parametri di query aggiuntivi per la funzione di filtro _(facoltativo)_.

1. Crea un trigger denominato **myCloudantTrigger** con il feed `changes` nel bind di pacchetto che hai creato in precedenza. Includi le funzioni `filter` e `query_params` per attivare il trigger nel momento in cui un documento viene aggiunto (o modificato) quando lo stato è `new`.

  Assicurati di sostituire `/_/myCloudant` con il tuo nome pacchetto.
  ```
  ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes \
  --param dbname testdb \
  --param filter "mailbox/by_status" \
  --param query_params '{"status":"new"}'
  ```
  {: pre}

  Output di esempio:
  ```
  ok: created trigger feed myCloudantTrigger
  ```
  {: screen}

2. Avvia il polling delle attivazioni per dare una chiara visibilità di ciò che sta accadendo.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

3. Crea un'azione che possiamo usare per osservare l'effetto del feed di modifica. Ad esempio, un'azione denominata **showCloudantChange** contenente il seguente codice JavaScript:
  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

4. Crea una regola per collegare l'azione **showCloudantChange** al trigger creato in precedenza:
  ```
  ibmcloud fn rule update aCloudantRule myCloudantTrigger showCloudantChange
  ```
  {: pre}

5. Crea le azioni e una regola per associarle al trigger **myCloudantTrigger**.

6. Nel tuo dashboard {{site.data.keyword.cloudant_short_notm}}, modifica un documento esistente oppure creane uno nuovo. Il documento deve avere un campo _status_, impostato su **new**.

7. Puoi osservare le nuove attivazioni per il trigger **myCloudantTrigger** per ogni modifica del documento solo se lo stato del documento è **new** in base alla funzione di filtro e al parametro di query.

Verifica i passi di lettura e scrittura per controllare che le credenziali di {{site.data.keyword.cloudant_short_notm}} siano corrette.

### Struttura dei dati di un evento trigger
{: #cloudant_struct}

Il contenuto degli eventi generati include i seguenti parametri:

  - `id`: l'ID documento.
  - `seq`: l'identificativo della sequenza generato da {{site.data.keyword.cloudant_short_notm}}.
  - `changes`: un array di oggetti, ciascuno dei quali con un campo `rev` che contiene l'ID revisione del documento.

La rappresentazione JSON dell'evento trigger è la seguente:
```json
{
    "dbname": "testdb",
    "id": "6ca436c44074c4c2aa6a40c9a188b348",
    "seq": "2-g1AAAAL9aJyV-GJCaEuqx4-BktQkYp_dmIfC",
    "changes": [
        {
            "rev": "2-da3f80848a480379486fb4a2ad98fa16"
          }
    ]
}
```
{: codeblock}

### Filtro degli eventi di modifica del database
{: #cloudant_filter}

Puoi facoltativamente definire una funzione di filtro per evitare di avere eventi di modifica non necessari che attivano il tuo trigger.

Per creare una nuova funzione di filtro, puoi utilizzare un'azione.

Crea un file di documento json `design_doc.json` con la seguente funzione di filtro:
```json
{
  "doc": {
    "_id": "_design/mailbox",
    "filters": {
      "by_status": "function(doc, req){if (doc.status != req.query.status){return false;} return true;}"
    }
  }
}
```
{: codeblock}

Crea un documento di progettazione sul database con la seguente funzione di filtro:
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
```
{: pre}

Le informazioni per il nuovo documento di progettazione vengono riprodotte sullo schermo:
```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}

### Utilizzo di una sequenza di azioni e di un trigger di modifica per elaborare un documento da un database {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_seq}

Puoi utilizzare una sequenza di azioni in una regola per recuperare ed elaborare il documento associato a un evento di modifica {{site.data.keyword.cloudant_short_notm}}.

Codice di esempio di un'azione che gestisce un documento:
```javascript
function main(doc){
  return { "isWalter:" : doc.name === "Walter White"};
}
```
{: codeblock}

Crea l'azione per elaborare il documento da {{site.data.keyword.cloudant_short_notm}}:
```
ibmcloud fn action create myAction myAction.js
```
{: pre}

Per leggere un documento dal database, puoi utilizzare l'azione `read` dal pacchetto {{site.data.keyword.cloudant_short_notm}}.
L'azione `read` può essere composta con `myAction` per creare una sequenza di azioni.
```
ibmcloud fn action create sequenceAction --sequence /_/myCloudant/read,myAction
```
{: pre}

L'azione `sequenceAction` può essere utilizzata in una regola che attiva l'azione sui nuovi eventi trigger {{site.data.keyword.cloudant_short_notm}}.
```
ibmcloud fn rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**Nota:** il trigger `changes` di {{site.data.keyword.cloudant_short_notm}} era utilizzato per supportare il parametro `includeDoc`, che non è più supportato.

Puoi ricreare i trigger precedentemente creati con `includeDoc`. Segui questa procedura per ricreare il trigger:
```
ibmcloud fn trigger delete myCloudantTrigger
```
{: pre}

```
ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
```
{: pre}

L'esempio può essere utilizzato per creare una sequenza di azioni per leggere il documento modificato e chiamare le tue azioni esistenti. Ricordati di disabilitare tutte le regole che non sono più valide e creane di nuove utilizzando il modello della sequenza di azioni.

