---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: services, serverless

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


# Aggiunta di servizi IBM Cloud
{: #services}

Puoi utilizzare la funzionalità di incorporazione dei servizi IBM Cloud nella tua applicazione.
{: shortdesc}

**Come posso aggiungere i servizi IBM Cloud alla mia applicazione?**

1. È possibile codificare le chiamate API REST nella tua applicazione. Questa opzione potrebbe costituire il modo più rapido di comunicare con un servizio IBM Cloud.
2. Puoi utilizzare un pacchetto preinstallato o installabile per incorporare la funzionalità. Puoi utilizzare le azioni e i feed memorizzati nei pacchetti all'interno del codice della tua applicazione. Questa opzione potrebbe snellire un po' il tuo codice, il che potrebbe essere utile se la tua applicazione rasenta i limiti del sistema.


**Come configuro i parametri a cui deve accedere la mia applicazione?**

Questi parametri potrebbero includere i valori che rendono la tua applicazione riutilizzabile con dati differenti o i valori richiesti dal servizio, come ad esempio le credenziali. 
1. Puoi codificare i parametri nella tua applicazione. Questa opzione potrebbe non essere il modo più sicuro per archiviare informazioni riservate quali le credenziali.
2. Puoi eseguire il bind dei parametri alla tua applicazione, collegandoli a un'azione o a un pacchetto.


## Bind di un servizio a un'azione o un pacchetto
{: #services_bind}

Esegui il bind di qualsiasi servizio {{site.data.keyword.Bluemix_notm}} a qualsiasi azione. Quando si esegue il bind di un servizio, viene creato un nuovo parametro sulla tua azione esistente che contiene le credenziali dell'istanza del servizio.

**Nota**: non puoi eseguire il bind di più istanze dello stesso servizio a un'azione o a un pacchetto. Puoi solo eseguire il bind di una sola istanza di un servizio. 

Prima di iniziare, [crea un'azione](/docs/openwhisk?topic=cloud-functions-actions) e[definisci le credenziali](/docs/resources?topic=resources-externalapp#externalapp) per il servizio di cui vuoi eseguire il bind all'azione.

1. Ottieni il nome del servizio e dell'istanza del servizio di cui desideri eseguire il bind a un'azione o a un pacchetto. Nell'output di esempio, `composer` è il servizio e `Composer-qp` è il nome dell'istanza del servizio.
    ```
    ibmcloud service list
    ```
    {: pre}

    Output di esempio:
    ```
    name              service        plan   bound apps   last operation
    Composer-qp   composer   free                create succeeded
    Composer-uc   composer   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. Ottieni il nome delle credenziali definite per un'istanza del servizio.

    ```
    ibmcloud service keys SERVICE_NAME
    ```
    {: pre}

    Esempio:
    ```
    ibmcloud service keys Composer-qp
    ```
    {: pre}

    Output di esempio:
    ```
    Invoking 'cf service-keys Composer-qp'...

    Getting keys for service instance Composer-qp as <your ID>...

    name
Credentials-1
Credentials-2
    ```
    {: screen}

3. Esegui il bind del servizio a un'azione. Il comando `ibmcloud fn service bind` di {{site.data.keyword.openwhisk_short}} rende disponibili le tue credenziali del servizio {{site.data.keyword.Bluemix_notm}} al tuo codice {{site.data.keyword.openwhisk_short}} durante il runtime.
    ```
    ibmcloud fn service bind SERVICE ACTION_NAME [--instance INSTANCE_NAME] [--keyname CREDENTIALS_NAME]
    ```
    {: pre}

    <table>
    <caption>Descrizione dei componenti del comando <code>ibmcloud fn service bind</code></caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del comando <code>ibmcloud fn service bind</code></th>
    </thead>
    <tbody>
    <tr>
    <td><code>SERVICE</code></td>
    <td>Il nome del servizio di cui stai eseguendo il bind.</td>
    </tr>
    <tr>
    <td><code>ACTION_NAME</code></td>
    <td>Il nome dell'azione o del pacchetto di cui eseguire il bind al servizio.</td>
    </tr>
    <tr>
    <td>--instance <code>INSTANCE_NAME</code></td>
    <td>Facoltativo: specifica un nome per l'istanza del servizio. Se non specifichi un nome, viene selezionata la prima istanza per il servizio.</td>
    </tr>
    <tr>
    <td>--keyname <code>CREDENTIALS_NAME</code></td>
    <td>Facoltativo: specifica il nome delle credenziali. Se non specifichi il nome delle credenziali, vengono selezionate le prime credenziali per l'istanza del servizio.</td>
    </tr>
    </tbody></table>

    Ad esempio, per eseguire il bind di un {{site.data.keyword.ibmwatson}} servizio Composer a un'azione denominata `hello`:
    ```
    ibmcloud fn service bind composer hello --instance Composer-qp --keyname Credentials-1
    ```
    {: pre}

    Output:
    ```
    Service credentials 'Credentials-1' from service 'Composer-qp' bound to action 'hello'.
    ```
    {: screen}

4. Verifica che le credenziali siano state associate correttamente. L'azione a cui è stato associato il servizio tramite bind non supporta alcun indicatore personalizzato, ma supporta il debug e gli indicatori dettagliati.
    ```
    ibmcloud fn action get hello parameters
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
                    "composer": {
                        "password": "[Service password]",
                        "url": "[Service url]",
                        "username": "[Service username]",
                        "instance": "Composer-qp",
                        "credentials": "Credentials-1"
                    },
                }
            }
        ],
    }
    ```
    {: screen}

    In questo esempio, le credenziali per il servizio Composer, insieme a qualsiasi altra credenziale per altri tipi di servizi, appartengono a un parametro denominato `__bx_creds`. L'azione cerca il parametro associato `__bx_creds` e rimuove il riferimento al tipo di servizio elencato. Se quel tipo di servizio è l'unico elencato, l'azione annulla il valore del parametro `__bx_creds`. Se più di un servizio è associato all'azione, il parametro `__bx_creds` rimane con qualsiasi servizio ancora associato.

Per ulteriori informazioni sulla trasmissione di parametri a un'azione o a un pacchetto, vedi [Associazione di parametri ad azioni](/docs/openwhisk?topic=cloud-functions-actions#actions_params).




## Annullamento del bind di servizi alle azioni
{: #services_unbind}

L'annullamento del bind di un servizio a un'azione o un pacchetto rimuove i bind del servizio esistenti.

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}
