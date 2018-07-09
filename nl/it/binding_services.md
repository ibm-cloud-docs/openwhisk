---

copyright:
  years: 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Utilizzo dei servizi dalle azioni
{: #binding_services}

Puoi sfruttare il [plug-in CLI {{site.data.keyword.openwhisk}}](./bluemix_cli.html) per eseguire il bind di un servizio a un'azione. {{site.data.keyword.openwhisk_short}} fornisce il comando `service bind` in modo da rendere disponibili le tue credenziali del servizio {{site.data.keyword.Bluemix}} al codice Cloud Functions in fase di runtime. Il comando `service bind` non deve essere confuso con il comando `cf bind-service` disponibile in Cloud Foundry. Rappresenta semplicemente un modo automatizzato per creare un nuovo parametro sulla tua azione esistente che contiene le credenziali del servizio. Il comando {{site.data.keyword.openwhisk_short}} `service bind` è più flessibile e ti consente di eseguire il bind di un qualsiasi servizio {{site.data.keyword.Bluemix_notm}} a qualsiasi azione definita in {{site.data.keyword.openwhisk_short}}. L'unico avvertimento è che devi aver definito le credenziali per il servizio di cui vuoi eseguire il bind.
{: shortdesc}

## Come eseguire il bind di un servizio a un'azione
{: #cli_bind}

Esegui il bind di un servizio a un'azione utilizzando il comando `bx wsk service bind` fornito dal plug-in CLI [{{site.data.keyword.openwhisk_short}}](./bluemix_cli.html). Ulteriori informazioni sono disponibili nella sezione [Limitazioni](./binding_services.html#limitations).

Sintassi di utilizzo con `bind`:
```
bx wsk service bind NOME_SERVIZIO NOME_AZIONE [--instance nome_istanza] [--keyname nome]
```
{: pre}

Il comando `service bind` richiede un tipo di servizio e un nome azione a cui eseguire il bind. Ad esempio, se vuoi eseguire il bind di un servizio di conversazione Watson a un'azione denominata `hello`, la tua chiamata dovrà essere simile al seguente comando:
```
bx wsk service bind conversation hello
```
{: pre}

Che produce il seguente output:
``` 
Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
```

Questo comando cerca i servizi di conversazione Watson esistenti nello spazio corrente, prende il primo servizio di conversazione che trova e quindi recupera tutte le credenziali che appartengono a tale servizio. Utilizzando la prima serie di credenziali che appartengono al servizio, esegue il bind di queste credenziali sotto forma di parametro all'azione `hello` specificata. L'output ti mostra esattamente a quale servizio è associata l'azione e quale serie di credenziali di quel servizio sono state utilizzate per il bind.

Per verificare che le credenziali siano associate correttamente, immetti il seguente comando:
```
bx wsk action get hello parameters
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

Da qui, puoi vedere che le credenziali per questo servizio di conversazione (insieme a qualsiasi altra credenziale per gli altri tipi di servizio) appartengono a un parametro denominato `__bx_creds`, che può ora essere utilizzato all'interno del codice azione allo stesso modo di qualsiasi altro parametro associato. L'azione seleziona il primo servizio di conversazione disponibile che include la prima serie di credenziali definita in quel servizio.  

Per ulteriori informazioni sul passaggio dei parametri a un'azione e su come vengono influenzate le credenziali quando si esegue un'operazione `action update`, vedi il seguente documento [Crea e richiama le azioni](openwhisk_actions.html#openwhisk_pass_params).

Il comando `wsk service` supporta i seguenti due indicatori:

<dl>
    <dt>--instance</dt>
    <dd>Il nome dello specifico servizio del tipo che vuoi utilizzare.</dd>
    <dt>--keyname</dt>
    <dd>Il nome delle specifiche credenziali all'interno del servizio che vuoi utilizzare.</dd>
</dl>

Per comprendere come utilizzare questi indicatori, vedi il seguente esempio. Utilizzando il comando `bx wsk service bind` precedente, supponiamo che esistano effettivamente due servizi di conversazione e che alla fine il valore predefinito dell'azione sia stato associato al servizio o credenziali non corretti. Potresti rieseguire il comando con gli indicatori `--instance` e `--keyname` per assicurarti di associare il servizio corretto all'azione corretta. Innanzitutto, guarda quali servizi sono disponibili e quali sono le credenziali associate ad essi. Se dovessimo elencare i nostri servizi dovremmo vedere un output come il seguente:

```
bx service list
name              service        plan   bound apps   last operation
Conversation-qp   conversation   free                create succeeded
Conversation-uc   conversation   free                create succeeded
Discovery-37      discovery      lite                create succeeded
```

Da questo output vediamo che **Conversation-qp** è il primo dei due servizi elencati ed è quello che il comando iniziale `bx wsk service bind conversation hello` aveva finito per associare. Forse vuoi eseguire invece il bind al servizio **Conversation-uc**. Quindi, per essere assolutamente sicuro, puoi verificare quali credenziali contiene **Conversation-uc** per assicurarti di eseguire il bind utilizzando la serie corretta di credenziali.

```
bx service keys Conversation-uc
Invoking 'cf service-keys Conversation-uc'...

Getting keys for service instance Conversation-uc as [your id]...

name
Credentials-1
Credentials-2
```

Vuoi eseguire il bind a "Credentials-2" da questo servizio. Per assicurarti che l'azione esegua il comportamento desiderato, immetti il seguente comando:
```
bx wsk service bind conversation hello --instance Conversation-uc --keyname Credentials-2
```
{: pre}

Che produce il seguente output:
```
Service credentials 'Credentials-2' from service 'Conversation-uc' bound to action 'hello'.
```

Dall'output, puoi vedere che all'azione è associata la serie di credenziali corretta. Di nuovo, per verificare, puoi vedere il seguente comando `bx wsk action get`.
```
bx wsk action get hello parameters
```
{: pre}

Che produce i seguente risultati:
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
                    "instance": "Conversation-uc",
                    "credentials": "Credentials-2"
                }
            }
        }
    ],
}
```

Sono supportati i normali indicatori di debug, che stampano le intestazioni di risposta dalle chiamate.

## Come annullare il bind di un servizio da un'azione
{: #cli_unbind}

Annulla il bind di un servizio da un'azione utilizzando `bx wsk service unbind`. Il comando `service unbind` rimuove i bind esistenti creati dal comando `service bind`.

Sintassi di utilizzo con `unbind`:
```
bx wsk service unbind NOME_SERVIZIO NOME_AZIONE
```
{: pre}

## Limitazioni
{: #limitations}

L'azione `service` non supporta alcun indicatore personalizzato, ma supporta il normale debug e gli indicatori dettagliati. L'azione cerca il parametro associato `__bx_creds` e rimuove il riferimento al tipo di servizio elencato. Se quel tipo di servizio è l'unico elencato, l'azione annulla il valore del parametro  `__bx_creds`. Se più di un servizio è associato all'azione, il parametro `__bx_creds` rimane con qualsiasi servizio ancora associato.

Puoi eseguire solo il bind di un servizio di ciascun tipo a un'azione. Il bind di più servizi dello stesso tipo in una singola azione non è supportato.
{: tip}

