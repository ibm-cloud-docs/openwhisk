---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Origine eventi Cloudant
{: #openwhisk_cloudant}

Scopri come ascoltare le modifiche a un database {{site.data.keyword.cloudant}}, filtrare gli eventi di modifica del database e utilizzare una sequenza di azioni per elaborare un documento da un database {{site.data.keyword.cloudant_short_notm}}. Il pacchetto `/whisk.system/cloudant` ti consente di lavorare con un database {{site.data.keyword.cloudant_short_notm}} e include le azioni e i feed che seguono:

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | pacchetto | dbname, host, username, password | Lavorare con un database Cloudant. |
| `/whisk.system/cloudant/read` | azione | dbname, id | Leggere un documento da un database. |
| `/whisk.system/cloudant/write` | azione | dbname, overwrite, doc | Scrivere un documento in un database. |
| `/whisk.system/cloudant/changes` | feed | dbname, filter, query_params, maxTriggers | Attivare eventi trigger in caso di modifiche a un database. |
{: shortdesc}

Le seguenti sezioni ti illustrano come configurare un pacchetto associato e come utilizzare le azioni e i feed nel pacchetto `/whisk.system/cloudant`. Per ulteriori informazioni sulla configurazione del database {{site.data.keyword.cloudant_short_notm}} e sulla lettura o scrittura nel database, vedi [Azioni {{site.data.keyword.cloudant_short_notm}}](./cloudant_actions.html).

## Crea un trigger utilizzando la funzione di filtro

Puoi utilizzare il feed `changes` per configurare un servizio in modo da attivare un trigger ogni volta che viene apportata una modifica al tuo database {{site.data.keyword.cloudant_short_notm}}.

I parametri utilizzati in questo esempio sono i seguenti:

**dbname**: il nome del database {{site.data.keyword.cloudant_short_notm}} _(obbligatorio)_.

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

Se non riesci ad osservare le nuove attivazioni, vedi l'argomento [{{site.data.keyword.cloudant_short_notm}}](./cloudant_actions.html) che illustra come leggere e scrivere in un database {{site.data.keyword.cloudant_short_notm}}. Verifica i passi di lettura e scrittura per controllare che le credenziali di {{site.data.keyword.cloudant_short_notm}} siano corrette.
{: tip}

## Struttura dei dati di un evento trigger

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

## Filtro degli eventi di modifica del database

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

## Utilizzo di una sequenza di azioni e di un trigger di modifica per elaborare un documento da un database {{site.data.keyword.cloudant_short_notm}}
{: #openwhisk_catalog_cloudant_read_change notoc}

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
