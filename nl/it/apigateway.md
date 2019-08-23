---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: serverless, rest api, gateway, web actions, functions

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


# Creazione di API REST API senza server
{: #apigateway}

Puoi utilizzare le API per gestire direttamente le [azioni web](/docs/openwhisk?topic=cloud-functions-actions_web) {{site.data.keyword.openwhisk}}.
{: shortdesc}

La creazione di API con il gateway API non è supportata per gli spazi dei nomi basati su IAM. Utilizza invece uno spazio dei nomi basato su Cloud Foundry.
{: important}

## Perché utilizzare le API REST con {{site.data.keyword.openwhisk_short}}?

Puoi utilizzare il gateway API come proxy per le tue azioni web. Il gateway API fornisce l'instradamento del metodo HTTP, segreti e ID client, limiti di frequenza, CORS, visualizzazione dell'utilizzo dell'API, visualizzazione dei log di risposta e politiche di condivisione dell'API.

Per ulteriori informazioni sulla gestione delle API, puoi consultare la [documentazione sulla gestione delle API](/docs/api-management?topic=api-management-manage_openwhisk_apis).

## Creazione della tua prima API
{: #api_create}

Per creare le API REST, devi disporre delle autorizzazioni di `SpaceDeveloper` nel tuo spazio Cloud Foundry. Le autorizzazioni dello spazio possono essere visualizzate eseguendo `ibmcloud account space-roles <org>`.
{: note}

Prima di iniziare, installa il [plug-in della CLI{{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-cli_install).

1. Salva il seguente codice in un file JavaScript denominato `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello, ${name}!`};
  }
  ```
  {: codeblock}

2. Crea un'azione web denominata `hello` utilizzando il file che hai creato. Assicurati di aggiungere l'indicatore `--web true`. Sostituisci `<filepath>` con il percorso del tuo file `hello.js`.

  ```
  ibmcloud fn action create hello <filepath>/hello.js --web true
  ```
  {: pre}

  **Output di esempio**
  ```
  ok: created action hello
  ```
  {: screen}

3. Crea un'API con il percorso di base `/hello`, il percorso `/world`, il metodo `get` e il tipo di risposta `json`
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  **Output di esempio**
  Viene generato un nuovo URL che espone l'azione `hello` utilizzando un metodo HTTP `GET`.

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  
4. Invia una richiesta HTTP di prova all'URL utilizzando il seguente comando cURL.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=Jane
  ```
  {: pre}

  **Output di esempio**
  Viene richiamata l'azione web `hello` che restituisce un oggetto JSON che include il parametro `name` nel parametro di query. Puoi passare i parametri all'azione con semplici parametri di query o utilizzando il corpo della richiesta. Le azioni web possono richiamare pubblicamente un'azione senza utilizzare l'autenticazione.

  ```
  {
  "payload": "Hello, Jane!"
  }
  ```
  {: screen}



## Utilizzo del controllo completo sulla risposta HTTP
{: #api_control}

L'indicatore `--response-type` controlla l'URL di destinazione dell'azione web che il gateway API deve trasmettere tramite proxy. Ad esempio, quando utilizzi l'indicatore `--response-type json`, il risultato completo dell'azione viene restituito in formato JSON e l'intestazione `Content-Type` viene automaticamente impostata su `application/json`.

Per restituire tipi di contenuto differenti nel corpo, usa il controllo completo sulle proprietà della risposta HTTP quali `statusCode` e `headers`. Puoi utilizzare l'indicatore `--response-type http` per configurare l'URL di destinazione dell'azione web con l'estensione `http`. Puoi modificare il codice dell'azione per rispettare la restituzione delle azioni web con l'estensione `http` o includere l'azione in una sequenza per passare il suo risultato a una nuova azione. La nuova azione può quindi trasformare il risultato in modo che abbia una formattazione appropriata per una risposta HTTP. Per ulteriori informazioni sui tipi di risposta e sulle estensioni delle azioni web, consulta la documentazione delle [azioni web](/docs/openwhisk?topic=cloud-functions-actions_web).

1. Salva il seguente codice come `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello, ${name}!`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. Aggiorna la tua azione web `hello` con la nuova versione del tuo codice `hello.js`. 
  ```
  ibmcloud fn action update hello <filepath>/hello.js --web true
  ```
  {: pre}

  **Output**
  ```
  ok: updated action hello
  ```
  {: screen}

3. Aggiorna il tipo di risposta API utilizzando l'indicatore `--response-type http`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

  **Output**
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

4. Richiama l'API aggiornata utilizzando il seguente comando cURL.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  **Output di esempio**
  ```
  {
  "payload": "Hello, Serverless API!"
  }
  ```
  {: screen}

## Modifica della configurazione
{: #api_modify_config}

Dopo aver creato la tua configurazione, puoi utilizzare la [scheda API](https://cloud.ibm.com/openwhisk/apimanagement){: external} nel dashboard {{site.data.keyword.openwhisk_short}} per modificare la configurazione nei seguenti modi.

* [Crea una {{site.data.keyword.openwhisk_short}} API](/docs/services/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis) che racchiude una serie di azioni {{site.data.keyword.openwhisk_short}}.
* [Proteggi la tua API](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis) applicando politiche di sicurezza API e di limitazione della frequenza.
* [Gestisci il traffico](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis) visualizzando le statistiche di utilizzo API e controllando i log di risposte.
* [Socializza e condividi](/docs/services/api-management?topic=api-management-manage_apis#share_api_manage_apis) la tua API con gli sviluppatori all'interno e all'esterno di {{site.data.keyword.cloud_notm}}.

</br>
Una volta terminato l'aggiornamento della configurazione, puoi scaricare il file di definizione in formato JSON e quindi reimportarlo utilizzando la CLI. Scaricare e importare la configurazione è utile, ad esempio, per una distribuzione non presidiata in una pipeline di integrazione e distribuzione continua (CICD). Puoi anche caricare e reimportare il file di definizione dell'API utilizzando l'IU.



