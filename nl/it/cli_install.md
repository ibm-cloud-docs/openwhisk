---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: functions cli, serverless, bluemix cli, install, functions plug-in

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

# Installazione della CLI e del plug-in
{: #cli_install}

{{site.data.keyword.openwhisk}} offre un potente plug-in per la CLI {{site.data.keyword.Bluemix_notm}} che consente la completa gestione del sistema {{site.data.keyword.openwhisk_short}}. Puoi utilizzare il plugin CLI {{site.data.keyword.openwhisk_short}} per gestire i tuoi frammenti di codice nelle azioni, creare dei trigger e delle regole per consentire alle tue azioni di rispondere agli eventi e aggregare le azioni in pacchetti.
{:shortdesc}


## Configurazione della CLI {{site.data.keyword.Bluemix_notm}}
{: #cli_setup}

Scarica e installa la CLI {{site.data.keyword.Bluemix_notm}} ed esegui l'accesso.
{: shortdesc}

1. Scarica e installa la [CLI {{site.data.keyword.Bluemix_notm}}](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli).

2. Accedi alla CLI {{site.data.keyword.Bluemix_notm}}. Per specificare una regione IBM Cloud, [includi l'endpoint API](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions).

  ```
  ibmcloud login
  ```
  {: pre}

  Per ignorare tali richieste, puoi specificare l'organizzazione e lo spazio quando esegui l'accesso utilizzando i seguenti indicatori: `ibmcloud login -o <ORG> -s <SPACE>`
  {: tip}

3. Se non hai specificato un'organizzazione e uno spazio, completa le istruzioni seguenti al comando login.


## Configurazione del plug-in {{site.data.keyword.openwhisk_short}}
{: #cli_plugin_setup}

Per utilizzare {{site.data.keyword.openwhisk_short}}, scarica e installa il plugin CLI.
{: shortdesc}

Puoi utilizzare il plugin per:

* Eseguire i tuoi frammenti di codice, o le azioni, su {{site.data.keyword.openwhisk_short}}. Vedi [Creazione e chiamata di azioni](/docs/openwhisk?topic=cloud-functions-actions).
* Utilizzare i trigger e le regole per consentire alle tue azioni di rispondere agli eventi. Vedi [Creazione di trigger e regole](/docs/openwhisk?topic=cloud-functions-triggers).
* Imparare in che modo i pacchetti riuniscono le azioni e configurano origini eventi esterne. Vedi [Crea e utilizza i pacchetti](/docs/openwhisk?topic=cloud-functions-pkg_ov).
* Esplorare il catalogo dei pacchetti e ampliare le tue applicazioni con servizi esterni, quali un'[origine eventi {{site.data.keyword.cloudant}}](/docs/openwhisk?topic=cloud-functions-pkg_cloudant).

Per vedere tutto quello che puoi fare con il plug-in {{site.data.keyword.openwhisk_short}}, esegui `ibmcloud fn` senza argomenti.
{: tip}

1. Installa il plug-in {{site.data.keyword.openwhisk_short}}.

  ```
  ibmcloud plugin install cloud-functions
  ```
  {: pre}

2. Verifica che il plug-in sia installato.

  ```
  ibmcloud plugin list
  ```
  {: pre}

  Output:
  ```
  Plugin Name          Version
  cloud-functions/wsk/functions/fn      1.0.30
  ```
  {: screen}

Hai già il plugin ma devi aggiornarlo? Esegui `ibmcloud plugin update cloud-functions`.
{:tip}



## Configurazione della CLI {{site.data.keyword.openwhisk_short}} per utilizzare un proxy HTTPS
{: #cli_proxy}

La CLI {{site.data.keyword.openwhisk_short}} può essere configurata per utilizzare un proxy HTTPS. Per configurare un proxy HTTPS, è necessario creare una variabile di ambiente denominata `HTTPS_PROXY`. La variabile deve essere impostata sull'indirizzo del proxy HTTPS e sulla sua porta utilizzando il seguente formato: `{PROXY IP}:{PROXY PORT}`.



## Passaggio a diverse regioni, organizzazioni e spazi<
{: #cli_regions}

Se hai già effettuato l'accesso, puoi eseguire il comando `ibmcloud target` nella CLI {{site.data.keyword.Bluemix_notm}} per passare tra regioni, organizzazioni e spazi.

Per creare e gestire le entità, devi selezionare uno spazio dei nomi. Lo spazio dei nomi predefinito, che può essere individuato da un carattere di sottolineatura (`_`) in alcune situazioni, corrisponde allo spazio dei nomi basato su Cloud Foundry che è al momento selezionato.

Puoi creare spazi basati su IAM per gestire le tue distribuzioni di pre-produzione (preparazione) e produzione creando degli spazi per ognuna. La creazione di spazi consente a {{site.data.keyword.openwhisk_short}} di avere due diversi spazi dei nomi che vengono definiti per te. Esegui [`ibmcloud iam space-create`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_account#ibmcloud_account_space_create) per creare più spazi nella tua organizzazione, come "preparazione" e "produzione":

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}} ha delle restrizioni sui nomi dello spazio dei nomi. Per ulteriori informazioni, fai riferimento alla documentazione [Dettagli e limiti del sistema](/docs/openwhisk?topic=cloud-functions-limits#limits_entities).
{: tip}

**Avvertenza**: la modifica del nome dell'organizzazione o dello spazio crea un nuovo spazio dei nomi basato sul nome modificato. Le entità nel vecchio spazio dei nomi non sono visibili nel nuovo e potrebbero venire eliminate.


## Migrazione dalla CLI OpenWhisk al plug-in CLI {{site.data.keyword.openwhisk_short}}
{: #cli_migrate}

Puoi ora utilizzare il plugin CLI {{site.data.keyword.openwhisk_short}} per interagire con le entità {{site.data.keyword.openwhisk_short}}. Sebbene sia possibile continuare ad utilizzare la CLI OpenWhisk autonoma, non saranno presenti le funzioni più recenti supportate da {{site.data.keyword.openwhisk_short}}, come gli spazi dei nomi basati su IAM e `service bind`.
{: shortdesc}

### Sintassi dei comandi
{: #cli_syntax}

Tutti i comandi `wsk`, ad eccezione del comando `wsk bluemix login` che non è più necessario, funzionano allo stesso modo del comando `ibmcloud fn`. Tutte le opzioni e gli argomenti del comando per i comandi nel plugin CLI Cloud Functions sono uguali a quelli della CLI autonoma OpenWhisk. Per ulteriori informazioni, vedi il [progetto Apache OpenWhisk CLI ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/apache/incubator-openwhisk-cli).

### Autenticazione e host dell'API
{: #cli_api_auth}

La CLI OpenWhisk ti richiedeva di configurare la chiave API di autenticazione e l'host API. Con il plug-in della CLI {{site.data.keyword.openwhisk_short}}, non devi specificare esplicitamente la chiave API e l'host API. Puoi invece accedere con `ibmcloud login`. Puoi specificare uno spazio dei nomi con abilitazione IAM eseguendo `ibmcloud fn property set --namespace NAME` o uno spazio dei nomi basato su Cloud Foundry eseguendo `ibmcloud target -o ORG -s SPACE`. Dopo l'accesso, tutti i comandi iniziano con `ibmcloud fn`.


Se devi utilizzare la chiave API di autenticazione per {{site.data.keyword.openwhisk_short}} in un client HTTP esterno come cURL o Postman, puoi richiamarla con i seguenti comandi:

Per ottenere la chiave API corrente:
```
ibmcloud fn property get --auth
```
{: pre}

Per ottenere l'host API corrente:
```
ibmcloud fn property get --apihost
```
{: pre}

La chiave API è specifica per ogni regione, organizzazione e spazio designato dal plug-in della CLI {{site.data.keyword.openwhisk_short}}.
{: tip}

### Autenticazione del gateway API
{: #cli_apigw_authentication}

La CLI OpenWhisk ti richiedeva di eseguire `wsk bluemix login` per poter configurare l'autorizzazione del gateway API per la gestione delle tue API utilizzando il comando `wsk api`. Con il plug-in della CLI {{site.data.keyword.openwhisk_short}}, non devi eseguire `wsk bluemix login`. Invece, quando utilizzi il comando `ibmcloud login` per accedere a {{site.data.keyword.Bluemix_notm}}, il plug-in {{site.data.keyword.openwhisk}} utilizza automaticamente le tue informazioni di accesso e di destinazione correnti. Adesso puoi gestire le tue API utilizzando il comando `ibmcloud fn api`.

### Migrazione degli script di distribuzione
{: #cli_migrating_deploy_scripts}

Se disponi di script che utilizzano la CLI OpenWhisk con il binario `wsk`, tutti i comandi funzionano allo stesso modo del comando `ibmcloud fn`. Puoi modificare i tuoi script per utilizzare il plug-in della CLI {{site.data.keyword.Bluemix_notm}} o creare un alias o wrapper in modo che i comandi correnti che utilizzano `wsk` vengano convertiti in `ibmcloud fn`. I comandi `ibmcloud login` e `ibmcloud target` nella CLI {{site.data.keyword.Bluemix_notm}} funzionano in modalità non presidiata. Con la modalità non presidiata, puoi configurare il tuo ambiente prima di eseguire i comandi `ibmcloud fn` per distribuire e gestire le tue entità {{site.data.keyword.openwhisk_short}}.




## Cronologia versione CLI
{: #cli_versions}

Un record cronologico delle versioni che mostrano evidenziazioni e correzioni di bug.

v1.0.30 (03-04-2019)
* Migliorata la gestione `service bind` di IAM e dei servizi basati sull'organizzazione e lo spazio.
* Aggiunta una correzione per la gestione dell'endpoint API https://cloud.ibm.com.

v1.0.29 (06-02-2019)
* Aggiunti i comandi `deploy` e `undeploy` per distribuire o annullare la distribuzione di una raccolta di entità Functions tramite il file manifest. Per ulteriori informazioni, fai riferimento alla documentazione [Deployment](/docs/openwhisk?topic=cloud-functions-deploy#deploy).

v1.0.28 (21-01-2019)
* Aggiunto un messaggio di errore quando `update|delete|get namespace name` esiste più volte.

v1.0.27 (11-12-2018)
* Aggiunte le correzioni `namespace get`.
* Aggiunta una correzione per `--save-as` quando un'azione è black box.
* Aggiunto l'indicatore `--concurrency` per i comandi action create e action update.

v1.0.26 (30-11-2018)
* Abilitato `fn property get --auth` per restituire correntemente la chiave di autenticazione in un nuovo ambiente.

v1.0.25 (23-11-2018)
* Migliorata la visualizzazione del risultato del messaggio di errore.
* Aggiunta una correzione `fn namespace get` per visualizzare correttamente le proprietà dello spazio dei nomi.

1.0.23 (15-10-2018)
* Aggiunto il supporto per il riconoscimento del codice dell'azione ruby (.rb).

1.0.22 (20-08-2018)
* Aggiunta la regione us-east.

1.0.21 (01-08-2018)
* Gli alias `fn` e `functions` possono ora essere utilizzati per i comandi {{site.data.keyword.openwhisk_short}}: `ibmcloud fn <command>` e `ibmcloud fn <command>`. Puoi anche utilizzare ancora `ibmcloud wsk <command>`.

1.0.19 (02-07-2018)
* Correzioni di bug minori e miglioramenti.

1.0.18 (20-06-2018)
* Aggiunta una correzione per l'annullamento del bind delle istanze del servizio fornite dall'utente.
* Miglioramento delle prestazioni.

1.0.17 (12-06-2018)
* Aggiunto il supporto per il bind (`ibmcloud wsk service bind`) e l'annullamento del bind (`ibmcloud wsk service unbind`) delle istanze del servizio fornite dall'utente create mediante il comando `ibmcloud cf create-user-provided-service`.

1.0.16 (24-05-2018)
* Correzioni di bug minori e miglioramenti.

1.0.15 (21-05-2018)
* Correzioni di bug minori e miglioramenti.

1.0.14 (17-05-2018)
* Abilitato il supporto per il carattere `&` nei nomi di organizzazione e spazio.

1.0.13 (07-05-2018)
* Correzioni di bug minori e miglioramenti nella gestione degli errori.

1.0.12 (30-04-2018)
* Aggiornamenti all'SDK {{site.data.keyword.Bluemix_notm}} per mantenere la compatibilità con la CLI `bx`.

1.0.11 (23-04-2018)
* Correzioni di bug minori e miglioramenti.

1.0.10 (09-04-2018)
* Aggiunta la nuova opzione `--web-secure` ai comandi `ibmcloud wsk action create|update` per proteggere gli endpoint delle azioni web.
* Corretto il [difetto](https://github.com/apache/incubator-openwhisk-cli/issues/237) del parametro percorso back-to-back.

1.0.9 (16-03-2018)
* Abilitato il supporto per il bind dei servizi a livello di pacchetto.

1.0.8 (22-02-2018)
* Abilitato il supporto per il bind dei servizi IAM.

1.0.7 (02-02-2018)
* Aggiornato `ibmcloud wsk api` per accettare i parametri percorso come `/api/{id}`. Per informazioni, vedi [Gateway API](/docs/openwhisk?topic=cloud-functions-apigateway).
* Ripristinato il supporto proxy.
* Rimosso `swift:3`.

1.0.6 (30-01-2018)
* Corretto un bug del comando `ibmcloud wsk service bind` per le azioni in un pacchetto.

