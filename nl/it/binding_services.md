---

copyright:
  years: 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Bind dei servizi alle azioni
{: #binding_services}

Puoi utilizzare il [plug-in della CLI {{site.data.keyword.openwhisk}} ](./bluemix_cli.html) per eseguire il bind di un servizio a un'azione o un pacchetto. Il comando `ibmcloud wsk service bind` di {{site.data.keyword.openwhisk_short}} rende disponibili le tue credenziali del servizio {{site.data.keyword.Bluemix_notm}} al tuo codice {{site.data.keyword.openwhisk_short}} durante il runtime.
{: shortdesc}

Non confondere il comando `ibmcloud wsk service bind` con il comando `cf bind-service` che è disponibile in Cloud Foundry.
{: tip}

## Bind di un servizio a un'azione o un pacchetto
{: #cli_bind}

Esegui il bind di un servizio {{site.data.keyword.Bluemix_notm}} a qualsiasi azione definita in {{site.data.keyword.openwhisk_short}}. Il bind di un servizio crea un nuovo parametro sulla tua azione esistente che contiene le credenziali dell'istanza del servizio.

**Nota**: puoi eseguire il bind di un solo servizio di ciascun tipo a un'azione o pacchetto. Il bind di più servizi dello stesso tipo non è supportato.

Prima di iniziare, [definisci le credenziali](/docs/apps/reqnsi.html#accser_external) per il servizio di cui vuoi eseguire il bind.

1. Ottieni il nome dell'istanza del servizio di cui desideri eseguire il bind a un'azione o un pacchetto.
    ```
    ibmcloud service list
    ```
    {: pre}

    Output di esempio:
    ```
    name              service        plan   bound apps   last operation
    Conversation-qp   conversation   free                create succeeded
    Conversation-uc   conversation   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. Ottieni il nome delle credenziali definite per l'istanza del servizio che hai ottenuto nel passo precedente.
    ```
    ibmcloud service keys Conversation-qp
    ```
    {: pre}

    Output di esempio:
    ```
    Invoking 'cf service-keys Conversation-qp'...

    Getting keys for service instance Conversation-qp as <your ID>...

    name
Credentials-1
Credentials-2
    ```
    {: screen}

3. Esegui il bind del servizio a un'azione.
    ```
    ibmcloud wsk service bind SERVICE_TYPE ACTION_NAME [--instance instance_name] [--keyname credentials_name]
    ```
    {: pre}

    <table>
    <caption>Descrizione dei componenti del comando <code>ibmcloud wsk service bind</code></caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del comando <code>ibmcloud wsk service bind</code></th>
    </thead>
    <tbody>
    <tr>
    <td><code>SERVICE_TYPE</code></td>
    <td>Il tipo di servizio di cui eseguire il bind.</td>
    </tr>
    <tr>
    <td><code>ACTION_NAME</code></td>
    <td>Il nome dell'azione o del pacchetto di cui eseguire il bind al servizio.</td>
    </tr>
    <tr>
    <td>--instance <code>instance_name</code></td>
    <td>Facoltativo: specifica un nome per l'istanza del servizio. Se non specifichi un nome, viene selezionata la prima istanza per il servizio.</td>
    </tr>
    <tr>
    <td>--keyname <code>credentials_name</code></td>
    <td>Facoltativo: specifica un nome per l'insieme di credenziali. Se non specifichi un nome, viene selezionato il primo insieme di credenziali per l'istanza del servizio.</td>
    </tr>
    </tbody></table>

    Ad esempio, per eseguire il bind di un servizio di conversazione {{site.data.keyword.ibmwatson}} a un'azione denominata `hello`:
    ```
    ibmcloud wsk service bind conversation hello --instance Conversation-qp --keyname Credentials-1

    Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
    ```
    {: screen}

4. Verifica che le credenziali siano state associate correttamente. L'azione a cui è stato associato il servizio tramite bind non supporta alcun indicatore personalizzato, ma supporta il debug e gli indicatori dettagliati.
    ```
    ibmcloud wsk action get hello parameters
    ```
    {: pre}

    Output di esempio:
    ```
    ok: got action Hello World
{
        "parameters": [
        {
                "key": "var1",
            "value": "val1"
            },
            {
                "key": "dog",
            "value": "cat"
            },
            {
                "key": "__bx_creds",
            "value": {
                    "conversation": {
                        "password": "[Service password]",
                    "url": "[Service url]",
                    "username": "[Service username]",
                    "instance": "Conversation-qp",
                    "credentials": "Credentials-1"
                    },
                }
            }
        ],
    }
    ```
    {: screen}

    In questo esempio, le credenziali per il servizio di conversazione, insieme a qualsiasi altra credenziale per altri tipi di servizi, appartengono a un parametro denominato `__bx_creds`. L'azione cerca il parametro associato `__bx_creds` e rimuove il riferimento al tipo di servizio elencato. Se quel tipo di servizio è l'unico elencato, l'azione annulla il valore del parametro `__bx_creds`. Se più di un servizio è associato all'azione, il parametro `__bx_creds` rimane con qualsiasi servizio ancora associato.

Per ulteriori informazioni sul passaggio dei parametri a un'azione o un pacchetto e su come vengono interessate le credenziali durante le operazioni di `update`, vedi [Utilizzo dei parametri](./parameters.html#pass-params-action).


## Annullamento del bind di un servizio da un'azione o un pacchetto
{: #cli_unbind}

Annulla il bind di un servizio da un'azione o un pacchetto. L'annullamento del bind di un servizio rimuove i bind esistenti creati dal comando `service bind`.

```
ibmcloud wsk service unbind SERVICE_NAME ACTION_NAME
```
{: pre}
