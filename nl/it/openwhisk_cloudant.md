---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Utilizzo del pacchetto Cloudant
{: #openwhisk_catalog_cloudant}
Il pacchetto `/whisk.system/cloudant` ti consente di lavorare con un database Cloudant. Include le azioni e i feed indicati di seguito.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | Pacchetto | dbname, host, username, password | Lavorare con un database Cloudant|
| `/whisk.system/cloudant/read` | Azione | dbname, id | Leggere un documento da un database|
| `/whisk.system/cloudant/write` | Azione | dbname, overwrite, doc | Scrivere un documento in un database|
| `/whisk.system/cloudant/changes` | Feed | dbname, filter, query_params, maxTriggers | Attivare eventi trigger in caso di modifiche a un database|
{: shortdesc}

I seguenti argomenti illustrano la configurazione di un database Cloudant, la configurazione di un pacchetto associato e l'utilizzo delle azioni e dei feed nel pacchetto `/whisk.system/cloudant`.

## Configurazione di un database Cloudant in {{site.data.keyword.Bluemix_notm}}
{: #openwhisk_catalog_cloudant_in}

Se utilizzi OpenWhisk da {{site.data.keyword.Bluemix_notm}}, OpenWhisk crea automaticamente i bind di pacchetto per le tue istanze del servizio Cloudant. Se non utilizzi OpenWhisk e Cloudant da {{site.data.keyword.Bluemix_notm}}, vai direttamente al passo successivo.

1. Crea un'istanza del servizio Cloudant nel tuo [dashboard](http://console.ng.Bluemix.net) {{site.data.keyword.Bluemix_notm}}.

  Assicurati di creare una chiave credenziale per ogni nuova istanza del servizio.

2. Aggiorna i pacchetti nel tuo spazio dei nomi. L'aggiornamento crea automaticamente un bind di pacchetto per ogni istanza del servizio Cloudant con una chiave credenziale definita.

  ```
  wsk package refresh
  ```
  {: pre}
  ```
  created bindings:
  Bluemix_testCloudant_Credentials-1
  ```

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 private binding
  ```

  Il bind di pacchetto contiene ora le credenziali associate alla tua istanza del servizio Cloudant.

3. Verifica che il bind di pacchetto creato precedentemente sia configurato con l'host e le credenziali della tua istanza del servizio {{site.data.keyword.Bluemix_notm}} Cloudant.

  ```
  wsk package get /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 parameters
  ```
  {: pre}
  ```
  ok: got package /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1, displaying field parameters
  ```
  ```json
  [
      {
          "key": "username",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix"
      },
      {
          "key": "host",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix.cloudant.com"
      },
      {
          "key": "password",
          "value": "c9088667484a9ac827daf8884972737"
      }
  ]
  ```

## Configurazione di un database Cloudant all'esterno di {{site.data.keyword.Bluemix_notm}}
{: #openwhisk_catalog_cloudant_outside}

Se non utilizzi OpenWhisk in {{site.data.keyword.Bluemix_notm}} o se vuoi configurare il tuo database Cloudant all'esterno di {{site.data.keyword.Bluemix_notm}}, devi creare manualmente un bind di pacchetto per il tuo account Cloudant. Ti servono il nome host, il nome utente e la password dell'account Cloudant.

1. Crea un bind di pacchetto configurato per il tuo account Cloudant.

  ```
  wsk package bind /whisk.system/cloudant myCloudant -p username MYUSERNAME -p password MYPASSWORD -p host MYCLOUDANTACCOUNT.cloudant.com
  ```
  {: pre}
  

2. Verifica che il bind di pacchetto esista.

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myNamespace/myCloudant private binding
  ```


## In ascolto delle modifiche a un database Cloudant
{: #openwhisk_catalog_cloudant_listen}

### Filtro degli eventi di modifica del database

Puoi definire una funzione di filtro per evitare di avere eventi di modifica non necessari che attivano il tuo trigger.

Per creare una nuova funzione di filtro, puoi utilizzare un'azione.

Crea un file di documento json `design_doc.json` con la seguente funzione di filtro
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

Crea un documento di progettazione sul database con la funzione di filtro
```
wsk action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
```
Le informazioni per il nuovo documento di progettazione vengono riprodotte sullo schermo.
```json
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```

### Crea il trigger utilizzando la funzione di filtro

Puoi utilizzare il feed `changes` per configurare un servizio in modo da attivare un trigger ogni volta che viene apportata una modifica al tuo database Cloudant. I parametri sono i seguenti:

- `dbname`: nome del database Cloudant.
- `maxTriggers`: arresta l'attivazione dei trigger quando viene raggiunto questo limite. Il valore predefinito è infinito.
- `filter`: funzione di filtro definita su un documento di progettazione.
- `query_params`: parametri di query aggiuntivi per la funzione di filtro.


1. Crea un trigger con un feed `changes` nel bind di pacchetto che hai creato in precedenza. Includi le funzioni `filter` e `query_params` per attivare il trigger nel momento in cui un documento viene aggiunto o modificato quando lo stato è `new`.
Assicurati di sostituire `/_/myCloudant` con il tuo nome pacchetto.

  ```
  wsk trigger create myCloudantTrigger --feed /_/myCloudant/changes \
  --param dbname testdb \
  --param filter "mailbox/by_status" \
  --param query_params '{"status":"new"}'
  ```
  {: pre}
  ```
  ok: created trigger feed myCloudantTrigger
  ```

2. Esegui il polling per le attivazioni.

  ```
  wsk activation poll
  ```
  {: pre}

3. Nel tuo dashboard Cloudant, modifica un documento esistente oppure creane uno nuovo.

4. Puoi osservare le nuove attivazioni per il trigger `myCloudantTrigger` per ogni modifica del documento solo se lo stato del documento è `new` in base alla funzione di filtro e al parametro di query.
  
  **Nota**: se non sei in grado di osservare le nuove attivazioni, consulta le sezioni successive relative alla lettura e alla scrittura in un database Cloudant. Il test dei seguenti passi di lettura e scrittura aiuta a verificare che le credenziali Cloudant siano corrette.
  
  Adesso puoi creare le regole e associarle alle azioni per reagire agli aggiornamenti del documento.
  
  Il contenuto degli eventi generati include i seguenti parametri:
  
  - `id`: l'ID documento.
  - `seq`: l'identificativo della sequenza generato da Cloudant.
  - `changes`: un array di oggetti, ciascuno dei quali con un campo `rev` che contiene l'ID revisione del documento.
  
  La rappresentazione JSON dell'evento trigger è la seguente:
  
  ```json
  {
      "id": "6ca436c44074c4c2aa6a40c9a188b348",
      "seq": "2-g1AAAAL9aJyV-GJCaEuqx4-BktQkYp_dmIfC",
      "changes": [
          {
              "rev": "2-da3f80848a480379486fb4a2ad98fa16"
          }
      ]
  }
  ```
  
## Scrittura in un database Cloudant
{: #openwhisk_catalog_cloudant_write}

Puoi utilizzare un'azione per memorizzare un documento in un database Cloudant denominato `testdb`. Assicurati che questo database esista nel tuo account Cloudant.

1. Memorizza un documento utilizzando l'azione `write` nel bind di pacchetto che hai creato in precedenza. Assicurati di sostituire `/_/myCloudant` con il tuo nome pacchetto.

  ```
  wsk action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287
  ```
  ```json
  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```

2. Verifica che il documento esista cercandolo nel tuo dashboard Cloudant.

  L'URL del dashboard per il database `testdb` ha un aspetto simile al seguente: `https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100`.


## Lettura da un database Cloudant
{: #openwhisk_catalog_cloudant_read}

Puoi utilizzare un'azione per recuperare un documento da un database Cloudant denominato `testdb`. Assicurati che questo database esista nel tuo account Cloudant.

- Recupera un documento utilizzando l'azione `read` nel bind di pacchetto che hai creato in precedenza. Assicurati di sostituire `/_/myCloudant` con il tuo nome pacchetto.

  ```
  wsk action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}
  ```json
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```

## Utilizzo di una sequenza di azioni e di un trigger di modifica per elaborare un documento da un database Cloudant
{: #openwhisk_catalog_cloudant_read_change notoc}

Puoi utilizzare una sequenza di azioni in una regola per recuperare ed elaborare il documento associato a un evento di modifica Cloudant.

Codice di esempio di un'azione che gestisce un documento:
```javascript
function main(doc){
  return { "isWalter:" : doc.name === "Walter White"};
}
```

Crea l'azione per elaborare il documento da Cloudant:
```
wsk action create myAction myAction.js
```
{: pre}

Per leggere un documento dal database, puoi utilizzare l'azione `read` dal pacchetto Cloudant.
L'azione `read` può essere composta con `myAction` per creare una sequenza di azioni.
```
wsk action create sequenceAction --sequence /_/myCloudant/read,myAction
```
{: pre}

L'azione `sequenceAction` può essere utilizzata in una regola che attiva l'azione sui nuovi eventi trigger Cloudant.
```
wsk rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**Nota:** il trigger `changes` Cloudant è utilizzato per supportare il parametro `includeDoc` che non è più supportato.
  Puoi ricreare i trigger precedentemente creati con `includeDoc`. Segui questa procedura per ricreare il trigger: 
  ```
  wsk trigger delete myCloudantTrigger
  ```
  {: pre}
  ```
  wsk trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
  ```
  {: pre}

  L'esempio può essere utilizzato per creare una sequenza di azioni per leggere il documento modificato e chiamare le tue azioni esistenti.
  Ricordati di disabilitare tutte le regole che non sono più valide e creane di nuove utilizzando il modello della sequenza di azioni.

