---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Pacchetto Cloudant
{: #cloudant_actions}

Il pacchetto `/whisk.system/cloudant` ti consente di lavorare con un database [{{site.data.keyword.cloudant}}](/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant) e include le azioni e i feed che seguono:

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | pacchetto | dbname, host, username, password | Lavorare con un database Cloudant. |
| `/whisk.system/cloudant/read` | azione | dbname, id | Leggere un documento da un database. |
| `/whisk.system/cloudant/write` | azione | dbname, overwrite, doc | Scrivere un documento in un database. |
| `/whisk.system/cloudant/changes` | feed | dbname, filter, query_params, maxTriggers | Attivare eventi trigger in caso di modifiche a un database. |
{: shortdesc}

Le seguenti sezioni ti illustrano come configurare un database {{site.data.keyword.cloudant_short_notm}} e come leggere e scrivere in esso.
Per ulteriori informazioni su come utilizzare i feed con il pacchetto `/whisk.system/cloudant`, vedi [Origini eventi {{site.data.keyword.cloudant_short_notm}}](./openwhisk_cloudant.html).

## Configurazione di un database {{site.data.keyword.cloudant_short_notm}} in {{site.data.keyword.Bluemix_notm}}
{: #cloudantdb_cloud}

Se utilizzi {{site.data.keyword.openwhisk}} da {{site.data.keyword.Bluemix_notm}}, {{site.data.keyword.openwhisk_short}} crea automaticamente i bind di pacchetto per le tue istanze del servizio {{site.data.keyword.cloudant_short_notm}}. Se non utilizzi {{site.data.keyword.openwhisk_short}} e {{site.data.keyword.cloudant_short_notm}} da {{site.data.keyword.Bluemix_notm}}, passa alla sezione successiva.

1. Crea un'istanza del servizio {{site.data.keyword.cloudant_short_notm}} nel tuo [dashboard {{site.data.keyword.Bluemix_notm}}](http://console.bluemix.net).

  Assicurati di creare una chiave credenziale per ogni nuova istanza del servizio.

2. Aggiorna i pacchetti nel tuo spazio dei nomi. L'aggiornamento crea automaticamente un bind di pacchetto per ogni istanza del servizio {{site.data.keyword.cloudant_short_notm}} con una chiave credenziale definita.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Output di esempio:
  ```
  created bindings:
  Bluemix_testCloudant_Credentials-1
  ```
  {: screen}

  ```
  ibmcloud fn package list
  ```
  {: pre}

  Output di esempio:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 private binding
  ```
  {: screen}

  Il bind di pacchetto contiene ora le credenziali associate alla tua istanza del servizio {{site.data.keyword.cloudant_short_notm}}.

3. Verifica che il bind di pacchetto creato precedentemente sia configurato con l'host e le credenziali della tua istanza del servizio {{site.data.keyword.cloudant_short_notm}} {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud fn package get /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 parameters
  ```
  {: pre}

  Output di esempio:
  ```
  ok: got package /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1, displaying field parameters

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
  {: screen}

## Configurazione di un database {{site.data.keyword.cloudant_short_notm}} all'esterno di {{site.data.keyword.Bluemix_notm}}
{: #cloudantdb_nocloud}

Se non utilizzi {{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}} o se vuoi configurare il tuo database {{site.data.keyword.cloudant_short_notm}} all'esterno di {{site.data.keyword.Bluemix_notm}}, devi creare manualmente un bind di pacchetto per il tuo account {{site.data.keyword.cloudant_short_notm}}. Ti servono il nome host, il nome utente e la password dell'account {{site.data.keyword.cloudant_short_notm}}.

1. Crea un bind di pacchetto configurato per il tuo account {{site.data.keyword.cloudant_short_notm}}.
  ```
  wsk package bind /whisk.system/cloudant myCloudant -p username MYUSERNAME -p password MYPASSWORD -p host MYCLOUDANTACCOUNT.cloudant.com
  ```
  {: pre}


2. Verifica che il bind di pacchetto esista.
  ```
  wsk package list
  ```
  {: pre}

  Output di esempio:
  ```
  packages
  /myNamespace/myCloudant private binding
  ```
  {: screen}

## Lettura da un database {{site.data.keyword.cloudant_short_notm}}
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

## Scrittura in un database {{site.data.keyword.cloudant_short_notm}}
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
