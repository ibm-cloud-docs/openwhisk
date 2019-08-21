---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: cloudant, event, action, trigger, sequence, functions

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


# Cloudant
{: #pkg_cloudant}

Con il pacchetto `/whisk.system/cloudant` preinstallato, puoi lavorare con un database [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started). L'uso di questo pacchetto non richiede un bind dei servizi.
{: shortdesc}


## Entità disponibili
{: #cloudant_available}
La seguente tabella mostra una selezione delle entità disponibili nel pacchetto `whisk.system/cloudant`. Puoi utilizzare il pacchetto `whisk.system/cloudant` per leggere, scrivere, aggiornare o eliminare i documenti. Puoi anche utilizzare il feed `changes` per restare in ascolto delle modifiche a un database {{site.data.keyword.cloudant_short_notm}}.
{: shortdesc}

Per un elenco completo delle entità disponibili nel pacchetto `/whisk.system/cloudant`, esegui `ibmcloud fn package get /whisk.system/cloudant`.
{: note}

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | Pacchetto | `dbname`, `host`, `username`, `password` | Lavorare con un database Cloudant. |
| `/whisk.system/cloudant/read` | Azione | `dbname`, `id` | Leggere un documento da un database. |
| `/whisk.system/cloudant/write` | Azione | `dbname`, `overwrite`, `doc` | Scrivere un documento in un database. |
| `/whisk.system/cloudant/update-document` | Azione | `dbname`, `doc` | Aggiornare un documento in un database. |
| `/whisk.system/cloudant/changes` | Feed | `dbname`, `iamApiKey`, `iamUrl`, `filter`, `query_params`, `maxTriggers` | Attivare eventi trigger in caso di modifiche a un database. |

Il parametro `includeDoc` non è più supportato per l'utilizzo con il feed `/whisk.system/cloudant/changes`. Se hai creato dei trigger che utilizzano questo parametro, devi ricrearli senza il parametro `includeDoc`.
{: deprecated}

## Bind del pacchetto `/whisk.system/cloudant` al tuo database {{site.data.keyword.cloudant_short_notm}}.
Se stai utilizzando {{site.data.keyword.openwhisk}} da {{site.data.keyword.cloud_notm}}, puoi utilizzare il plug-in della CLI {{site.data.keyword.openwhisk}} per eseguire il bind di un servizio a un'azione o a un pacchetto.
{: #cloudant_db}

**Prima di iniziare**
Devi avere un'istanza di {{site.data.keyword.cloudant_short_notm}}. Per creare un'istanza, vedi [Introduzione a {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started).

1. Crea un bind di pacchetto `/whisk.system/cloudant` configurato per il tuo account {{site.data.keyword.cloudant_short_notm}}. In questo esempio, il nome del pacchetto è `myCloudant`.

  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. Verifica che il bind di pacchetto esista.

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Output di esempio**

  ```
  packages
  /<namespace>/myCloudant private
  ```
  {: screen}

3. Ottieni il nome dell'istanza del servizio di cui desideri eseguire il bind a un'azione o un pacchetto.

    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    **Output di esempio**
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

    **Output di esempio**

    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. Esegui il bind del servizio al pacchetto creato nel passo uno.

    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1'
    ```
    {: pre}

6. Verifica che le credenziali siano state associate correttamente.
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    **Output di esempio**

    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
        {
                "key": "serviceName",
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

In questo esempio, le credenziali per il servizio {{site.data.keyword.cloudant_short_notm}} appartengono a un parametro denominato `__bx_creds`.

## Gestione dei documenti in un database {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_read}

Puoi utilizzare un'azione per leggere, scrivere, aggiornare o eliminare un documento da un database {{site.data.keyword.cloudant_short_notm}}.
{: shortdesc}

### Lettura di un documento
Puoi utilizzare l'azione `/whisk.system/cloudant/read` per leggere un documento dal tuo database {{site.data.keyword.cloudant_short_notm}}.

**Prima di iniziare**
Se non hai un documento nel tuo database {{site.data.keyword.cloudant_short_notm}}, puoi crearne uno utilizzando il dashboard [{{site.data.keyword.cloudant_short_notm}}. L'URL per il dashboard è `https://<mycloudantaccount>.cloudant.com/dashboard.html#database/<database_name>/_all_docs?limit=100`.

Recupera un documento utilizzando l'azione `read` action. Sostituisci `/_/myCloudant` con il nome del tuo pacchetto, `<database_name>` con il nome del database e `<document_id>` con l'ID file. Richiama l'azione per verificare il recupero di un documento.

**Sintassi dei comandi**

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id <document_id>
  ```
  {: pre}

**Esempio di azione di lettura da un database `test`**
Richiama l'azione per verificare la lettura di un file. Questo esempio legge un file con l'`id` di `9f86f4955e7a38ab0169462e6ac0f476` che è un file vuoto.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id 9f86f4955e7a38ab0169462e6ac0f476
  ```
  {:pre}

**Output di esempio**
  ```
  {
      "_id": "9f86f4955e7a38ab0169462e6ac0f476",
      "_rev": "1-967a00dff5e02add41819138abb3284d"
  }
  ```
  {: screen}

### Scrittura di un documento in un database {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_write}

Puoi utilizzare un'azione per creare o aggiornare i documenti in un database {{site.data.keyword.cloudant_short_notm}}.
{: shortdesc}

**Prima di iniziare**
  Crea un [bind di pacchetto](#cloudant_db) `/whisk.system/cloudant` configurato per il tuo account {{site.data.keyword.cloudant_short_notm}}.

1. Archivia un documento utilizzando l'azione `write` nel bind di pacchetto che hai creato. Sostituisci `/_/myCloudant` con il nome del tuo pacchetto, `<database_name>` con il nome del tuo database, `<document_id>` con l'ID documento e `<test_name>` con un nome.

  **Sintassi dei comandi**
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param <database_name> test --param doc "{\"_id\":\"<document_id>\",\"name\":\"<test_name>\"}"
  ```
  {: pre}

  **Esempio di azione di scrittura in un database `test`**

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\"}"
  ```
  {: pre}

  **Output di esempio**

  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "color",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. Verifica che il documento esista nel dashboard {{site.data.keyword.cloudant_short_notm}}. L'URL del dashboard per il database `test` è nel seguente formato: `https://<mycloudantaccount>.cloudant.com/dashboard.html#database/test/_all_docs?limit=100`.

  **Documento di esempio nel dashboard {{site.data.keyword.cloudant_short_notm}}**
  ```
  {
  "_id": "color",
  "_rev": "1-f413f4b74a724e391fa5dd2e9c8e9d3f",
  "name": "blue"
  }
  ```
  {: screen}

### Aggiornamento di un documento
Puoi utilizzare l'azione `/update-document` per aggiornare un documento nel tuo database {{site.data.keyword.cloudant_short_notm}}.
{: short desc}

**Prima di iniziare**
Crea un [bind di pacchetto](#cloudant_db) `/whisk.system/cloudant` configurato per il tuo account {{site.data.keyword.cloudant_short_notm}}.

Il seguente esempio aggiorna il documento creato nella sezione [Scrittura di un documento in un database {{site.data.keyword.cloudant_short_notm}}](#cloudant_write).
{: note}

Puoi aggiornare un documento nel tuo database sostituendo `<test>` con il nome del tuo database e sostituendo l'indicatore `--param doc` con l'`id` e i contenuti del documento nel database che vuoi aggiornare.


1. Puoi aggiornare un documento nel database `test` immettendo il seguente comando. Questo esempio aggiunge il valore `shade` al documento `color`. 

  ```
  ibmcloud fn action invoke /_/myCloudant/update-document --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}"
  ```
  {: pre}

  **Output**
  ```
  {
    "id": "color",
    "ok": true,
    "rev": "2-8b904347bfe52e0f388ef6f39d6ba84f"
    }
  ```
  {: screen}

2. Per visualizzare l'aggiornamento, recupera di nuovo il documento.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id color
  ```
  {: pre}

  **Documento di esempio**
  ```
  {
    "_id": "color",
    "_rev": "2-8b904347bfe52e0f388ef6f39d6ba84f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

## Crea un trigger utilizzando la funzione di filtro
{: #cloudant_trigger}

Puoi utilizzare il feed `changes` per configurare un servizio in modo da attivare un trigger ogni volta che viene apportata una modifica al tuo database {{site.data.keyword.cloudant_short_notm}}.

**Prima di iniziare**
  Crea un [bind di pacchetto](#cloudant_db) `/whisk.system/cloudant` configurato per il tuo account {{site.data.keyword.cloudant_short_notm}}.

Parametri utilizzati in questo esempio.

| Parametro | Descrizione |
| --- | --- |
| `dbname` | (Obbligatorio) Il nome del database {{site.data.keyword.cloudant_short_notm}}. |
| `iamApiKey` | (Facoltativo) La chiave API IAM per il database Cloudant.  Se specificato, questo valore viene utilizzato come credenziali al posto di nome utente e password. |
| `iamUrl` | (Facoltativo) L'URL del servizio token IAM che viene utilizzato quando è specificato `iamApiKey`.  Il valore predefinito è `https://iam.cloud.ibm.com/identity/token`. | 
| `maxTriggers` | (Facoltativo) Arresta l'attivazione dei trigger quando viene raggiunto questo limite. Il valore predefinito è infinito. |
| `filter` | (Facoltativo) La funzione di filtro definita in un documento di progettazione. |
| `query_params` | (Facoltativo) Eventuali parametri di query aggiuntivi che potrebbero essere necessari per la funzione di filtro. |
| `includeDoc` | (Obsoleto) Il parametro `includeDoc` non è più supportato per l'utilizzo con il feed `/whisk.system/cloudant/changes`. |

</br>

1. Crea un trigger denominato `cloudantTrigger` con il feed `changes` nel bind di pacchetto che hai creato in precedenza. Includi le funzioni `filter` e `query_params` per attivare il trigger nel momento in cui un documento viene aggiunto (o modificato) quando lo stato è `new`.

  Sostituisci `/_/myCloudant` con il nome del tuo pacchetto. Questo esempio utilizza un database denominato `test`.
  ```
  ibmcloud fn trigger create cloudantTrigger --feed /_/myCloudant/changes \ --param dbname test
  ```
  {: pre}

  **Output di esempio**

  ```
  ok: created trigger feed cloudantTrigger
  ```
  {: screen}

2. Salva il seguente codice JavaScript come `cloudantChange.js`.

  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

3. Crea un'azione denominata `cloudantChange` che puoi utilizzare per osservare il feed delle modifiche (changes). Sostituisci `<file_path>` con il percorso del tuo file `cloudantChange.js` sul computer.

  ```
  ibmcloud fn action create cloudantChange <file_path>/cloudantChange.js
  ```
  {: pre}

4. Crea una regola denominata `cloudantRule` per connettere l'azione `cloudantChange` al `cloudantTrigger` che hai creato in precedenza.

  ```
  ibmcloud fn rule create cloudantRule cloudantTrigger cloudantChange
  ```
  {: pre}

5. In un'altra finestra del terminale, avvia il polling in modo da poter vedere quando si verificano le attivazioni.

  ```
  ibmcloud fn activation poll
  ```
  {: pre}

6. Nel tuo dashboard {{site.data.keyword.cloudant_short_notm}}, modifica un documento esistente o creane uno.

7. Osserva le attivazioni per il trigger `cloudantTrigger` per ogni modifica del documento.

**Attivazione di esempio di `cloudantTrigger`**

```
Activation: 'cloudantTrigger' (ef6605cc05e04589a605cc05e04589d8)
[
    "{\"statusCode\":0,\"success\":true,\"activationId\":\"6067ed0d28774a68a7ed0d28771a684d\",\"rule\":\"<namespace>/cloudantRule\",\"action\":\"<namespace>/cloudantChange\"}"
]

Activation: 'cloudantChange' (6067ed0d28774a68a7ed0d28771a684d)
[
    "2019-06-24T16:46:10.428643Z    stdout: { changes: [ { rev: '19-f7f6d8607d6381d224321acfcfb8887e' } ],",
    "2019-06-24T16:46:10.428693Z    stdout: dbname: 'test',",
    "2019-06-24T16:46:10.428697Z    stdout: id: '6ca436c44074c4c2aa6a40c9a188b348',",
    "2019-06-24T16:46:10.428700Z    stdout: seq: '103-g1AAAAeLeJy91M9NwzAUBnCrrVQqDvTKCa4gpcT5YycnugFsAH5' }"
```
{: screen}

### Struttura dei dati di un'attivazione del trigger
{: #cloudant_struct}

Il contenuto dell'evento generato ha i seguenti parametri.

| Parametro | Descrizione |
| --- | --- |
| `id` | L'ID documento. |
| `seq` | L'identificativo della sequenza generato da {{site.data.keyword.cloudant_short_notm}}. |
| `changes` | Un array di oggetti, ognuno dei quali ha un campo `rev` che contiene l'ID di revisione del documento. |

**Rappresentazione JSON dell'attivazione del trigger**

```json
{
    "dbname": "test",
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

## Filtro degli eventi di modifica del database
{: #cloudant_filter}

Potresti definire una funzione di filtro per evitare eventi di modifica non necessari che attivano il tuo trigger.

**Prima di iniziare**
Crea un [bind di pacchetto](#cloudant_db) `/whisk.system/cloudant` configurato per il tuo account {{site.data.keyword.cloudant_short_notm}}.

Per creare una funzione di filtro, puoi utilizzare un'azione.

1. Salva il seguente filtro JSON in un file denominato `design_doc.json`.

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

2. Crea un documento di progettazione nel database con la seguente funzione di filtro. Sostituisci `<database_name>` con il nome del tuo database e `<file_path>` con il percorso del tuo file `design_doc.json`. Richiama l'azione `write` per verificare la creazione di un documento di progettazione.

**Sintassi dei comandi**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname <database_name> -p overwrite true -P <file_path>/design_doc.json
```
{: pre}

**Comando di esempio per scrivere un file `design_doc.json` in un database `test`**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname test -p overwrite true -P <file_path>/design_doc.json -r
```
{: pre}

**Output di esempio**

```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}


Per ulteriori informazioni sui documenti di progettazione {{site.data.keyword.cloudant_short_notm}}, vedi [Documenti di progettazione](/docs/services/Cloudant?topic=cloudant-design-documents)

## Elaborazione di un singolo documento utilizzando una sequenza di azioni
{: #cloudant_seq}

Puoi utilizzare una sequenza di azioni in una regola per recuperare ed elaborare il documento associato a un evento di modifica {{site.data.keyword.cloudant_short_notm}}.

**Prima di iniziare**
Crea un [bind di pacchetto](#cloudant_db) `/whisk.system/cloudant` configurato per il tuo account {{site.data.keyword.cloudant_short_notm}}. 

Questo esempio aggiorna il documento creato nella sezione [Scrittura di un documento in un database {{site.data.keyword.cloudant_short_notm}}](#cloudant_write).
{: note}

### Creazione di un'azione per elaborare un singolo documento

Per creare un'azione che gestisca le modifiche a un singolo documento, immetti i seguenti comandi.
{: shortdesc}

1. Salva il seguente codice come `docChange.js`

  ```javascript
  function main(doc){
    return { "isBlue:" : doc.name === "blue"};
  }
  ```
  {: codeblock}

2. Crea un'azione denominata `docChange` per elaborare il documento con il nome `blue` che hai creato in precedenza. Sostituisci `<file_path>` con il percorso del tuo file `docChange.js`

  ```
  ibmcloud fn action create docChange <file_path>/docChange.js
  ```
  {: pre}

  **Output**
  ```
  ok: created action docChange
  ```
  {: screen}

### Crea una sequenza con l'azione `read` 

L'azione `read` può essere composta con la tua azione `docChange` per creare una sequenza di azioni.
{: shortdesc}

  ```
  ibmcloud fn action create docSequence --sequence /_/myCloudant/read,docChange
  ```
  {: pre}

  **Output**
  ```
  ok: created action docSequence
  ```
  {: screen}

### Crea un trigger con il feed `changes`

  ```
  ibmcloud fn trigger create docTrigger --feed /_/myCloudant/changes \
  --param dbname test
  ```
  {: pre}

### Crea una regola per associare il trigger alla sequenza

L'azione `docSequence` può essere utilizzata in una regola che attiva l'azione sui nuovi eventi trigger {{site.data.keyword.cloudant_short_notm}}.

  ```
  ibmcloud fn rule create docRule docTrigger docSequence
  ```
  {: pre}

  **Output**
  ```
  ok: created rule docRule
  ```

  **Attivazione di esempio**
  ```
  "{\"statusCode\":0,\"success\":true,\"activationId\":\"144a4f95198a49ec8a4f95198a79ecc8\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ```
  {: screen}

### Verifica la sequenza

1. Verifica `docSequence` apportando una modifica al file `blue` che hai creato in precedenza. In questo esempio, il valore `shade` viene modificato in `indigo`.

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}" -p overwrite true
  ```
  {: pre}

  **Attivazione di esempio**

  ```
  Activation: 'docChange' (aa3e8fc3030446b2be8fc3030406b2eb)
  []

  Activation: 'docSequence' (23e0a17bebd3486ca0a17bebd3186c8d)
  [
      "8d42679127f3400382679127f300039d",
      "aa3e8fc3030446b2be8fc3030406b2eb"
  ]

  Activation: 'docTrigger' (db6de778bb084366ade778bb08036685)
  [
      "{\"statusCode\":0,\"success\":true,\"activationId\":\"23e0a17bebd3486ca0a17bebd3186c8d\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ]
  ```
  {: screen}

2. Verifica che il file sia stato aggiornato per includere il valore `shade` richiamando l'azione `read`. Sostituisci il nome `<database>` con il nome del tuo database.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id color
  ```
  {: pre}

  **Output**
  ```
  {
    "_id": "color",
    "_rev": "3-6845b04618338f717676f16edf32a78f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

### Passi successivi
Ora che stai ascoltando le modifiche a un documento nel tuo database {{site.data.keyword.cloudant_short_notm}}, puoi attivare le notifiche Slack per le modifiche utilizzando il [pacchetto `/whisk.system/slack`](/docs/openwhisk?topic=cloud-functions-pkg_slack).


