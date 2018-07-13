---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Configurazione del plug-in della CLI {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk}} offre un potente plug-in per la CLI {{site.data.keyword.Bluemix_notm}} che consente la completa gestione del sistema {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

## Configurazione della CLI {{site.data.keyword.Bluemix_notm}}
{: #bluemix_cli_setup}

Scarica e installa la CLI {{site.data.keyword.Bluemix_notm}} ed esegui l'accesso.
{: shortdesc}

1. Scarica e installa la [CLI {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html).

2. Accedi alla CLI {{site.data.keyword.Bluemix_notm}}. {{site.data.keyword.openwhisk_short}} è disponibile nelle regioni {{site.data.keyword.Bluemix_notm}} degli Stati Uniti Sud e del Regno Unito. Se l'endpoint API {{site.data.keyword.Bluemix_notm}} non è specificato, specificane uno con l'indicatore `-a`.

    * Per accedere alla regione Stati Uniti Sud:
      ```
      ibmcloud login -a api.ng.bluemix.net
      ```
      {: pre}

    * Per accedere alla regione Regno Unito:
      ```
      ibmcloud login -a api.eu-gb.bluemix.net
      ```
      {: pre}

  Puoi utilizzare il comando `ibmcloud api` per impostare esplicitamente l'endpoint API {{site.data.keyword.Bluemix_notm}}. Per visualizzare l'impostazione dell'endpoint API corrente, utilizza il comando `ibmcloud target`.
  {: tip}

3. Il comando `ibmcloud login` ti richiede informazioni quali organizzazione, spazio e password, laddove non siano specificate.

  Per ignorare tali richieste, puoi specificare l'organizzazione e lo spazio quando esegui l'accesso. Utilizza i seguenti indicatori: `ibmcloud login -o <ORG> -s <SPACE>`.
  {: tip}

Per eseguire l'accesso, puoi anche utilizzare una chiave API {{site.data.keyword.Bluemix_notm}}. Questo metodo è utile quando il tuo account è configurato con un accesso federato che richiede di accedere con l'indicatore `--sso`. [L'utilizzo di una chiave API](https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key) è utile anche se desideri impostare un'integrazione continua (IC) e vuoi configurare una pipeline automatica. 

1. Crea una nuova chiave API.
    ```
    ibmcloud iam api-key-create MyKey
    ```
    {: pre}

2. Utilizza il valore generato della chiave API per eseguire l'accesso.
    ```
    ibmcloud login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
    ```
    {: pre}
</br>
Per ulteriori informazioni sul comando `ibmcloud login`, utilizza `ibmcloud login --help` o consulta l'argomento [Comandi IBM Cloud (bx)](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login).

## Configurazione del plug-in {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_plugin_setup}

Scarica e installa il plug-in {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

1. Installa il plug-in {{site.data.keyword.openwhisk_short}}.
    ```
    ibmcloud plugin install cloud-functions
    ```
    {: pre}

2. Verifica che il plug-in sia installato.
    ```
    ibmcloud plugin list cloud-functions
    ```
    {: pre}

    Output:
    ```
    Plugin Name          Version
    Cloud-Functions      1.0.16
    ```
    {: screen}

3. Esegui una chiamata bloccante (sincrona) di `echo`. Passa `hello` come argomento.
    ```
    ibmcloud wsk action invoke whisk.system/utils/echo -p message hello --result
    ```
    {: pre}

4. Verifica che il messaggio `hello` venga restituito nell'output.
    Output di esempio:
    ```
    {
        "message":"hello"
    }
    ```
    {: screen}

5. Puoi aggiornare il plug-in {{site.data.keyword.openwhisk_short}} immettendo il seguente comando:
    ```
    ibmcloud plugin update cloud-functions
    ```
    {: pre}

Puoi utilizzare la CLI {{site.data.keyword.openwhisk_short}} per:

* Eseguire i tuoi frammenti di codice, o le azioni, su {{site.data.keyword.openwhisk_short}}. Vedi [Creazione e chiamata di azioni](./openwhisk_actions.html).
* Utilizzare i trigger e le regole per consentire alle tue azioni di rispondere agli eventi. Vedi [Creazione di trigger e regole](./openwhisk_triggers_rules.html).
* Imparare in che modo i pacchetti riuniscono le azioni e configurano origini eventi esterne. Vedi [Crea e utilizza i pacchetti](./openwhisk_packages.html).
* Esplorare il catalogo dei pacchetti e migliorare le tue applicazioni con servizi esterni, come un'[origine eventi {{site.data.keyword.cloudant}}](./openwhisk_cloudant.html).

Per elencare i comandi per il plug-in {{site.data.keyword.openwhisk_short}}, esegui `ibmcloud wsk` senza argomenti.
{: tip}

## Utilizzo dei servizi dalle azioni
{: #binding_services}

{{site.data.keyword.openwhisk_short}} fornisce il comando `service bind` in modo da rendere disponibili le tue credenziali del servizio {{site.data.keyword.Bluemix_notm}} al tuo codice in fase di runtime. Puoi utilizzare il comando `service bind` per eseguire il bind di un servizio {{site.data.keyword.Bluemix_notm}} a qualsiasi azione definita in {{site.data.keyword.openwhisk_short}}.

Per la procedura dettagliata su come utilizzare i servizi dalle azioni, vedi [Bind dei servizi alle azioni](./binding_services.html).

## Configurazione della CLI {{site.data.keyword.openwhisk_short}} per utilizzare un proxy HTTPS
{: #cli_https_proxy}

La CLI {{site.data.keyword.openwhisk_short}} può essere configurata per utilizzare un proxy HTTPS. Per configurare un proxy HTTPS, è necessario creare una variabile di ambiente denominata `HTTPS_PROXY`. La variabile deve essere impostata sull'indirizzo del proxy HTTPS e sulla sua porta utilizzando il seguente formato:
`{PROXY IP}:{PROXY PORT}`.

## Passaggio a diverse regioni, organizzazioni e spazi
{: #region_info}

Se hai già effettuato l'accesso, puoi eseguire il comando `ibmcloud target` nella CLI {{site.data.keyword.Bluemix_notm}} per passare tra regioni, organizzazioni e spazi.

{{site.data.keyword.openwhisk_short}} è disponibile nelle regioni {{site.data.keyword.Bluemix_notm}} degli Stati Uniti Sud e del Regno Unito. Per cambiare le regioni, utilizza il comando `ibmcloud target`. Ad esempio, per passare alla regione Regno Unito e allo spazio `staging` in tale regione:
```
ibmcloud target -r eu-gb -s staging
```
{: pre}

Puoi creare spazi per gestire le tue distribuzioni di pre-produzione (preparazione) e produzione creando degli spazi per ognuna. La creazione di spazi consente a {{site.data.keyword.openwhisk_short}} di avere due diversi spazi dei nomi che vengono definiti per te. Esegui [`ibmcloud iam space-create`](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create) per creare più spazi nella tua organizzazione, come "preparazione" e "produzione":

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}} ha delle restrizioni sui nomi dello spazio dei nomi. Per ulteriori informazioni, fai riferimento alla documentazione [Dettagli e limiti del sistema](https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities).
{: tip}

## Migrazione dalla CLI OpenWhisk al plug-in CLI {{site.data.keyword.openwhisk_short}}
{: #cli_migration}

Con l'introduzione del plug-in della CLI {{site.data.keyword.openwhisk_short}}, la CLI autonoma OpenWhisk non è più necessaria.

### Sintassi dei comandi
{: #command_syntax}

Tutti i comandi `wsk`, ad eccezione del comando `wsk bluemix login` che non è più necessario, funzionano allo stesso modo del comando `ibmcloud wsk`.  Le opzioni e gli argomenti di comando sono tutti gli stessi.

### Autenticazione e host dell'API
{: #api_authentication}

La CLI OpenWhisk ti richiedeva di configurare la chiave API di autenticazione e l'host API.
Con il plug-in della CLI {{site.data.keyword.openwhisk_short}}, non devi specificare esplicitamente la chiave API e l'host API. Puoi invece accedere con `ibmcloud login` e specificare la tua regione e il tuo spazio dei nomi utilizzando il comando `ibmcloud target`. Dopo l'accesso, tutti i comandi iniziano con `ibmcloud wsk`.

Se devi utilizzare la chiave API di autenticazione per {{site.data.keyword.openwhisk_short}} in un client HTTP esterno come cURL o Postman, puoi richiamarla con i seguenti comandi:

Per ottenere la chiave API corrente:
```
ibmcloud wsk property get --auth
```
{: pre}

Per ottenere l'host API corrente:
```
ibmcloud wsk property get --apihost
```
{: pre}

La chiave API è specifica per ogni regione, organizzazione e spazio designato dal plug-in della CLI {{site.data.keyword.openwhisk_short}}.
{: tip}

### Autenticazione del gateway API
{: #apigw_authentication}

La CLI OpenWhisk ti richiedeva di eseguire `wsk bluemix login` per poter configurare l'autorizzazione del gateway API per la gestione delle tue API utilizzando il comando `wsk api`. Con il plug-in della CLI {{site.data.keyword.openwhisk_short}}, non devi eseguire `wsk bluemix login`. Invece, quando utilizzi il comando `ibmcloud login` per accedere a {{site.data.keyword.Bluemix_notm}}, il plug-in {{site.data.keyword.openwhisk}} utilizza automaticamente le tue informazioni di accesso e di destinazione correnti. Adesso puoi gestire le tue API utilizzando il comando `ibmcloud wsk api`.

### Migrazione degli script di distribuzione
{: #migrating_deploy_scripts}

Se hai degli script che utilizzano la CLI OpenWhisk con il binario `wsk`, tutti i comandi funzionano allo stesso modo del comando `ibmcloud wsk`. Puoi modificare i tuoi script per utilizzare il plug-in della CLI {{site.data.keyword.Bluemix_notm}} o creare un alias o wrapper in modo che i comandi correnti che utilizzano `wsk` vengano convertiti in `ibmcloud wsk`. I comandi `ibmcloud login` e `ibmcloud target` nella CLI {{site.data.keyword.Bluemix_notm}} funzionano in modalità automatica. Con la modalità automatica, puoi configurare il tuo ambiente prima di eseguire i comandi `ibmcloud wsk` per distribuire e gestire le tue entità {{site.data.keyword.openwhisk_short}}.

## Cronologia delle versioni
{: #version_history}

Un record cronologico delle versioni che mostrano evidenziazioni e correzioni di bug.

1.0.18 (20-06-2018)
* Correzione per l'annullamento del bind delle istanze del servizio fornito dall'utente.
* Miglioramento delle prestazioni.

1.0.17 (12-06-2018)
* Aggiungere il supporto per il bind (`ibmcloud wsk service bind`) e l'annullamento del bind (`ibmcloud wsk service unbind`) delle istanze del servizio fornito dall'utente create mediante il comando `ibmcloud cf create-user-provided-service`.

1.0.16 (24-05-2018)
* Correzioni di bug minori e miglioramenti.

1.0.15 (21-05-2018)
* Correzioni di bug minori e miglioramenti.

1.0.14 (17-05-2018)
* Abilitare il supporto per il carattere `&` nei nomi di organizzazione e spazio.

1.0.13 (07-05-2018)
* Correzioni di bug minori e miglioramenti nella gestione degli errori.

1.0.12 (30-04-2018)
* Aggiornamenti all'SDK {{site.data.keyword.Bluemix_notm}} per mantenere la compatibilità con la CLI `bx`.

1.0.11 (23-04-2018)
* Correzioni di bug minori e miglioramenti.

1.0.10 (09-04-2018)
* Aggiungere la nuova opzione `--web-secure` ai comandi `ibmcloud wsk action create|update` per proteggere gli endpoint delle azioni web.
* Correggere il [difetto](https://github.com/apache/incubator-openwhisk-cli/issues/237) del parametro percorso back-to-back.

1.0.9 (16-03-2018)
* Abilitare il supporto per il bind dei servizi a livello di pacchetto.

1.0.8 (22-02-2018)
* Abilitare il supporto per il bind dei servizi IAM.

1.0.7 (02-02-2018)
* `ibmcloud wsk api` ora accetta i parametri percorso come `/api/{id}`. Per informazioni, vedi [Gateway API](./openwhisk_apigateway.html).
* Ripristinare il supporto proxy.
* Rimuovere `swift:3`.

1.0.6 (30-01-2018)
* Correzione del bug del comando `ibmcloud wsk service bind` per le azioni in un pacchetto.
