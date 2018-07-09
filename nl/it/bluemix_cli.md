---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Plug-in CLI {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk_short}} offre un potente plug-in per la CLI {{site.data.keyword.Bluemix_notm}} che consente la completa gestione del sistema {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

## Configurazione della CLI {{site.data.keyword.Bluemix_notm}}
{: #bluemix_cli_setup}

Scarica e installa la [CLI {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html).

## Configurazione del plug-in {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_plugin_setup}

Per installare il plug-in {{site.data.keyword.openwhisk_short}}, immetti il seguente comando:
```
bx plugin install cloud-functions
```
{: pre}


Per convalidare la corretta installazione del plug-in {{site.data.keyword.openwhisk_short}}, immetti il seguente comando:
```
bx plugin list cloud-functions
```
{: pre}


L'output visualizza le informazioni sulla versione di {{site.data.keyword.openwhisk_short}} installata:
```
Plugin Name          Version
Cloud-Functions      1.0.0
```

Puoi aggiornare il plug-in {{site.data.keyword.openwhisk_short}} immettendo il seguente comando:
```
bx plugin update Cloud-Functions
```
{: pre}


Per ulteriori informazioni sul comando plug-in, utilizza `bx plugin --help` o consulta la seguente documentazione:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_plugin_list

## Autenticazione della CLI {{site.data.keyword.Bluemix_notm}}
{: #bluemix_cli_auth}

{{site.data.keyword.openwhisk_short}} è disponibile in due regioni {{site.data.keyword.Bluemix_notm}}.

Quando accedi alla CLI {{site.data.keyword.Bluemix_notm}}, puoi specificare l'endpoint API {{site.data.keyword.Bluemix_notm}} per la regione di destinazione insieme all'organizzazione e allo spazio.

Per accedere alla regione Stati Uniti Sud:
```
bx login -a api.ng.bluemix.net
```
{: pre}


Per accedere alla regione Regno Unito:
```
bx login -a api.eu-gb.bluemix.net
```
{: pre}


L'indicatore `-a` specifica l'endpoint API {{site.data.keyword.Bluemix_notm}} da utilizzare. Se viene specificato l'endpoint dell'API, l'opzione `-a` non è necessaria. Puoi utilizzare il comando `bx api` per impostare esplicitamente l'endpoint API {{site.data.keyword.Bluemix_notm}}. Per visualizzare l'impostazione dell'endpoint API corrente, utilizza il comando `bx target`.

Se non sono specificate, il comando `login` ti richiede informazioni quali organizzazioni, spazio e password. Puoi specificare l'organizzazione e lo spazio nella riga di comando per ignorare tali richieste.
```
bx login -o <LA_MIA_ORGANIZZAZIONE> -s <IL_MIO_SPAZIO>
```
{: pre}


Per eseguire l'accesso, puoi anche utilizzare una chiave API {{site.data.keyword.Bluemix_notm}}. Questo metodo è utile quando il tuo account è configurato con un accesso federato che richiede di accedere con l'indicatore `--sso`. L'utilizzo di una chiave API è utile anche se desideri impostare un'integrazione continua (IC) e vuoi configurare una pipeline automatica.
https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key

Per creare una nuova chiave API utilizzando la CLI {{site.data.keyword.Bluemix_notm}}, immetti il seguente comando:
```
bx iam api-key-create MyKey
```
{: pre}


Quindi, utilizza il valore generato dalla chiave API per eseguire l'accesso, come indicato nel seguente esempio:
```
bx login -a api.ng.bluemix.net -o <LA_MIA_ORGANIZZAZIONE> -s <IL_MIO_SPAZIO> --apikey <LA_MIA_CHIAVE>
```
{: pre}


Per ulteriori informazioni sul comando login, utilizza `bx login --help` o consulta la seguente documentazione:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login


## Utilizzo del plug-in CLI {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_plugin_usage}

Verifica la tua configurazione. Esegui una chiamata bloccante (sincrona) di echo, passando `hello` come argomento, come nel seguente esempio:
```
bx wsk action invoke whisk.system/utils/echo -p message hello --result
```
{: pre}


Vedi il seguente output di esempio:
```
{
    "message":"hello"
}
```

Dopo aver configurato il tuo ambiente, puoi utilizzare la CLI {{site.data.keyword.openwhisk_short}} per effettuare le seguenti attività:

* Eseguire i tuoi frammenti di codice, o azioni, su {{site.data.keyword.openwhisk_short}}. Vedi [Creazione e richiamo di azioni](./openwhisk_actions.html).
* Utilizzare i trigger e le regole per consentire alle tue azioni di rispondere agli eventi. Vedi [Creazione di trigger e regole](./openwhisk_triggers_rules.html).
* Imparare in che modo i pacchetti riuniscono le azioni e configurano origini eventi esterne. Vedi [Crea e utilizza i pacchetti](./openwhisk_packages.html).
* Esplorare il catalogo dei pacchetti e ampliare le tue applicazioni con servizi esterni, quali un'[origine eventi Cloudant](./openwhisk_cloudant.html). Vedi [Utilizzo di servizi abilitati a {{site.data.keyword.openwhisk_short}}](./openwhisk_catalog.html).

Per ottenere un elenco di comandi per il plug-in {{site.data.keyword.openwhisk_short}}, esegui `bx wsk` senza argomenti.

## Utilizzo dei servizi dalle azioni
{: #binding_services}

{{site.data.keyword.openwhisk_short}} fornisce il comando `service bind` in modo da rendere disponibili le tue credenziali del servizio {{site.data.keyword.Bluemix_notm}} al tuo codice in fase di runtime. Il comando `service bind` può quindi essere utilizzato per consentirti di eseguire il bind di un qualsiasi servizio {{site.data.keyword.Bluemix_notm}} a qualsiasi azione definita in {{site.data.keyword.openwhisk_short}}.

Per la procedura dettagliata su come utilizzare i servizi dalle azioni, vedi l'argomento [Utilizzo dei servizi dalle azioni](./binding_services.html).

## Configura la CLI {{site.data.keyword.openwhisk_short}} per utilizzare un proxy HTTPS
{: #cli_https_proxy}

La CLI {{site.data.keyword.openwhisk_short}} può essere configurata per utilizzare un proxy HTTPS. Per configurare un proxy HTTPS, è necessario creare una variabile di ambiente denominata `HTTPS_PROXY`. La variabile deve essere impostata sull'indirizzo del proxy HTTPS e sulla sua porta utilizzando il seguente formato:
`{PROXY IP}:{PROXY PORT}`.

## Gestione di regioni, organizzazioni e spazi
{: #region_info}

Puoi creare spazi per gestire le tue distribuzioni di pre-produzione (preparazione) e produzione creando degli spazi per ognuna. La creazione di spazi consente a {{site.data.keyword.openwhisk_short}} di avere due diversi spazi dei nomi che vengono definiti per te.

Puoi utilizzare `bx iam space-create` per creare più spazi sotto la tua organizzazione, come "preparazione" e "produzione". Vedi il seguente esempio:
```
bx iam space-create "staging"
bx iam space-create "production"
```

Per ulteriori informazioni sul comando `bx iam`, fai riferimento alla seguente documentazione: https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create

{{site.data.keyword.openwhisk_short}} ha delle restrizioni sui nomi dello spazio dei nomi. Per ulteriori informazioni su queste restrizioni, fai riferimento alla seguente documentazione: https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities

Se hai già effettuato l'accesso, puoi eseguire il comando `bx target` nella CLI {{site.data.keyword.Bluemix_notm}} per passare tra regioni, organizzazioni e spazi.

Utilizza il seguente comando per visualizzare tutte le regioni {{site.data.keyword.Bluemix_notm}}.

{{site.data.keyword.openwhisk_short}} è supportato solo nelle regioni `us-south` e `eu-gb`.
{: tip}

```
bx regions

Name       Geolocation          Customer   Deployment   Domain                   CF API Endpoint                  Type
us-south   US South             IBM        Production   ng.bluemix.net     https://api.ng.bluemix.net             public
eu-gb      United Kingdom       IBM        Production   eu-gb.bluemix.net  https://api.eu-gb.bluemix.net          public
```

Utilizza il comando `bx target` per cambiare le regioni.  Ad esempio, se vuoi passare alla regione Regno Unito e allo spazio `staging`:
```
bx target -r eu-gb -s staging
```
{: pre}


Se devi cambiare gli spazi all'interno della stessa regione, ad esempio dalla preparazione alla produzione, immetti il seguente comando:
```
bx target -s production
```
{: pre}


Per ulteriori informazioni sul comando `target`, utilizza `bx target --help` o consulta la seguente documentazione:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_target

## Migrazione dalla CLI OpenWhisk al plug-in CLI {{site.data.keyword.openwhisk_short}}
{: #cli_migration}

Con l'introduzione del plug-in CLI {{site.data.keyword.openwhisk_short}}, la CLI OpenWhisk non è più necessaria.

### Sintassi dei comandi
{: #command_syntax}

Tutti i comandi `wsk`, ad eccezione del comando `wsk bluemix login` che non è più necessario, funzionano allo stesso modo del comando `bx wsk`.  Le opzioni e gli argomenti di comando sono tutti gli stessi.

### Autenticazione e host dell'API
{: #api_authentication}

La CLI OpenWhisk ti richiede di configurare la chiave API di autenticazione e l'host dell'API.
Con il plug-in CLI {{site.data.keyword.openwhisk_short}}, non è necessario configurare in modo esplicito la chiave API e l'host API. Dovrai accedere con `bx login` e specificare la tua regione e il tuo spazio dei nomi utilizzando il comando `bx target`. Successivamente, tutti i comandi successivi iniziano con `bx wsk`. L'autenticazione effettuata in questo modo evita anche l'accesso alla pagina Web {{site.data.keyword.Bluemix_notm}} per ottenere le informazioni specifiche per configurare l'ambiente della tua CLI.

Se hai bisogno di ottenere la chiave API di autenticazione per {{site.data.keyword.openwhisk_short}} perché sia utilizza da un client http esterno come cURL o Postman, puoi recuperarla con i seguenti comandi:

Per ottenere la chiave API corrente, immetti il seguente comando:
```
bx wsk property get --auth
```
{: pre}


Per ottenere l'host API corrente, immetti il seguente comando:
```
bx wsk property get --apihost
```
{: pre}


La chiave API è specifica per ogni regione, organizzazione e spazio designato dal plug-in CLI {{site.data.keyword.openwhisk_short}}.
{: tip}

### Autenticazione del gateway API
{: #apigw_authentication}

Attualmente, la CLI OpenWhisk richiede di eseguire `wsk bluemix login` per poter configurare l'autorizzazione del gateway API per la gestione delle tue API utilizzando il comando `wsk api`.

Con il plug-in CLI {{site.data.keyword.openwhisk}}, non è più necessario eseguire `wsk bluemix login`. Utilizza invece il comando `bx login` per accedere a {{site.data.keyword.Bluemix_notm}} e il plug-in {{site.data.keyword.openwhisk}} utilizzerà automaticamente le informazioni di accesso e di destinazione correnti. Adesso puoi gestire le tue API utilizzando il comando `bx wsk api`.

### Migrazione degli script di distribuzione
{: #migrating_deploy_scripts}

Se disponi di script che utilizzano la CLI OpenWhisk con il binario `wsk`, tutti i comandi funzionano allo stesso modo del comando `bx wsk`. Puoi modificare i tuoi script per utilizzare il plug-in CLI {{site.data.keyword.Bluemix_notm}} o puoi creare un alias o wrapper in modo che le esecuzioni correnti di `wsk` vengano tradotte in `bx wsk`. I comandi `bx login` e `bx target` nella CLI {{site.data.keyword.Bluemix_notm}} funzionano in modalità automatica. Con la modalità automatica, puoi configurare il tuo ambiente prima di eseguire i comandi `bx wsk` per distribuire e gestire le tue entità {{site.data.keyword.openwhisk_short}}.


## Cronologia delle versioni
{: #version_history}

Un record cronologico delle versioni che mostrano evidenziazioni e correzioni di bug.

1.0.7 (02-02-2018)
* `bx wsk api` ora accetta parametri percorso come `/api/{id}`. Per informazioni, vedi [Gateway API](./openwhisk_apigateway.html)
* Ripristinare il supporto proxy.
* Rimuovere `swift:3`.

1.0.6 (30-01-2018)
* Bug corretto per il comando `bx wsk service bind` per le azioni in un pacchetto.
