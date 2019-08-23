---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: functions cli, serverless, cli, install, functions plug-in

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


# Installazione della CLI e del plug-in
{: #cli_install}

{{site.data.keyword.openwhisk}} offre un potente plug-in per la CLI {{site.data.keyword.cloud_notm}} che consente la completa gestione del sistema {{site.data.keyword.openwhisk_short}}. Puoi utilizzare il plugin CLI {{site.data.keyword.openwhisk_short}} per gestire i tuoi frammenti di codice nelle azioni, creare dei trigger e delle regole per consentire alle tue azioni di rispondere agli eventi e aggregare le azioni in pacchetti.
{:shortdesc}


## Configurazione della CLI {{site.data.keyword.cloud_notm}}
{: #cli_setup}

**Prima di iniziare**

Devi creare un [account {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.

Scarica e installa la CLI {{site.data.keyword.cloud_notm}} ed esegui l'accesso.
{: shortdesc}

1. Scarica e installa la [CLI {{site.data.keyword.cloud_notm}}](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli).

2. Accedi alla CLI {{site.data.keyword.cloud_notm}}. Per specificare una regione {{site.data.keyword.cloud_notm}}, [includi l'endpoint API](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions).

  ```
  ibmcloud login
  ```
  {: pre}

3. Segui le istruzioni per selezionare il tuo account {{site.data.keyword.cloud_notm}}.

4. Ottieni un elenco di gruppi di risorse. 

```
ibmcloud resource groups
```
{: pre}

**Output di esempio**

```
Retrieving all resource groups under account <account_name> as email@ibm.com...
OK
Name      ID                                 Default Group   State   
default   a8a12accd63b437bbd6d58fb8b462ca7   true            ACTIVE
test      a8a12accd63b437bbd6d58fb8b462ca7   false           ACTIVE
```
{: screen}

5. Facoltativo: specifica un gruppo di risorse diverso da quello predefinito immettendo il seguente comando.
```
ibmcloud target -g <resource_group>
```
{: pre}


**Output di esempio**

```
Targeted resource group <resource_group>
```
{: screen}

## Configurazione del plug-in della CLI {{site.data.keyword.openwhisk_short}}
{: #cli_plugin_setup}

Per utilizzare {{site.data.keyword.openwhisk_short}}, scarica e installa il plugin CLI.
{: shortdesc}

Puoi utilizzare il plug-in della CLI {{site.data.keyword.openwhisk_short}} per effettuare le seguenti attività.

* Eseguire i tuoi frammenti di codice, o le azioni, su {{site.data.keyword.openwhisk_short}}. Vedi [Creazione e chiamata di azioni](/docs/openwhisk?topic=cloud-functions-actions).
* Creare trigger e regole per consentire alle tue azioni di rispondere agli eventi. Vedi [Creazione di trigger e regole](/docs/openwhisk?topic=cloud-functions-triggers).
* Raggruppare le azioni e configurare origini eventi esterne. Vedi [Crea e utilizza i pacchetti](/docs/openwhisk?topic=cloud-functions-pkg_ov).
* Esplorare il catalogo dei pacchetti e migliorare le tue applicazioni con servizi esterni. Vedi [Aggiunta di {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-services).

Per installare il plug-in della CLI {{site.data.keyword.openwhisk_short}}, completa i seguenti passi

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

  **Output**
  ```
  Plugin Name          Version
  cloud-functions/wsk/functions/fn      1.0.32
  ```
  {: screen}

3. Dopo l'accesso, tutti i comandi {{site.data.keyword.openwhisk_short}} iniziano con `ibmcloud fn`. Per vedere tutto quello che puoi fare con il plug-in {{site.data.keyword.openwhisk_short}}, esegui `ibmcloud fn` senza argomenti.
  ```
  ibmcloud fn
  ```
  {: pre}




## Specifica degli spazi dei nomi {{site.data.keyword.openwhisk_short}}
{: #cli_regions}
Per impostazione predefinita, {{site.data.keyword.openwhisk_short}} utilizza [spazi dei nomi abilitati a IAM](/docs/iam?topic=iam-iamoverview){: external}. Non puoi più creare spazi dei nomi basati su Cloud Foundry.
{: important}

### Crea o specifica uno spazio dei nomi.
Per ottenere un elenco dei tuoi spazi dei nomi {{site.data.keyword.openwhisk_short}}, esegui `ibmcloud fn namespace list`.

#### Crea uno spazio dei nomi abilitato a IAM.
  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description">]
  ```
  {: pre}

**Risposta**
  ```
  ok: created namespace <namespace_name>
  ```
  {: screen}


#### Specifica uno spazio dei nomi abilitato a IAM. 
  ```
  ibmcloud fn property set --namespace <namespace_name>
  ``` 
  {: pre}


**Risposta**
  ```
  ok: whisk namespace set to <namespace_name>
  ```
  {: screen}
  
#### Specifica uno spazio dei nomi basato su Cloud Foundry. 
  
Puoi utilizzare gli indicatori `-o` e `-s` per puntare a una `org` e uno `space` specifici o puoi seguire le istruzioni.

* Specifica uno spazio dei nomi Cloud Foundy includendo i nomi `org` e `space` nel comando `target`.

```
ibmcloud target --cf  -o <org> -s <space>
```
{: pre}

* Specifica Cloud Foundry come destinazione e segui le istruzioni per selezionare una `org` e uno `space`.

```
ibmcloud target --cf
```
{: pre}


**Risposta**
  ```
  Targeted Cloud Foundry (https://api.ng.bluemix.net)

  Targeted org <org_name>

  Targeted space <space_name>
                        
  API endpoint:      https://cloud.ibm.com   
  Region:            us-south   
  User:              <email>   
  Account:           (<account_id>) <-> <account>   
  Resource group:    default   
  CF API endpoint:   https://api.ng.bluemix.net (API version: 2.128.0)   
  Org:               <org_name>   
  Space:             <space_name>  
  ```
  {: screen} 





#### Facoltativo: creazione di spazi dei nomi per le distribuzioni di preparazione e produzione.

Puoi creare spazi dei nomi abilitati a IAM per gestire le tue distribuzioni {{site.data.keyword.openwhisk_short}} di pre-produzione (preparazione) e produzione creando degli spazi dei nomi per ognuna. Esegui [`ibmcloud fn namespace create`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace_create) per creare più spazi dei nomi nella tua organizzazione, come "preparazione" e "produzione":

Crea uno spazio dei nomi di preparazione.
```
ibmcloud fn namespace create staging
```
{: pre}

Crea uno spazio dei nomi di produzione.
```
ibmcloud fn namespace create production
```
{: pre}

{{site.data.keyword.openwhisk_short}} ha delle restrizioni sui nomi dello spazio dei nomi. Per ulteriori informazioni, vedi la documentazione [Dettagli e limiti del sistema](/docs/openwhisk?topic=cloud-functions-limits#limits_entities).
{: tip}


## Configurazione della CLI {{site.data.keyword.openwhisk_short}} per utilizzare un proxy HTTPS
{: #cli_proxy}

La CLI {{site.data.keyword.openwhisk_short}} può essere configurata per utilizzare un proxy HTTPS. Per configurare un proxy HTTPS, è necessario creare una variabile di ambiente denominata `HTTPS_PROXY`. La variabile deve essere impostata sull'indirizzo del proxy HTTPS e sulla sua porta utilizzando il seguente formato: `{PROXY IP}:{PROXY PORT}`.

La modifica del nome `org` o `space` crea un nuovo spazio dei nomi basato sul nome modificato. Le entità nel vecchio spazio dei nomi non sono visibili nel nuovo e potrebbero venire eliminate.
{: important}


## Migrazione dalla CLI OpenWhisk al plug-in CLI {{site.data.keyword.openwhisk_short}}
{: #cli_migrate}

Puoi ora utilizzare il plugin CLI {{site.data.keyword.openwhisk_short}} per interagire con le entità {{site.data.keyword.openwhisk_short}}. Anche se puoi continuare a utilizzare la CLI OpenWhisk autonoma, questa non dispone delle funzioni più recenti supportate da {{site.data.keyword.openwhisk_short}}, come gli spazi dei nomi basati su IAM e `service bind`.
{: shortdesc}


### Sintassi dei comandi
{: #cli_syntax}

Tutte le opzioni di comando e gli argomenti per i comandi nel plugin CLI Cloud Functions sono uguali alle opzioni disponibili per la [CLI autonoma OpenWhisk](https://github.com/apache/incubator-openwhisk-cli){: external}. Tuttavia, nota le seguenti differenze.

* Il plugin {{site.data.keyword.openwhisk}} utilizza automaticamente le tue informazioni di accesso e di destinazione correnti.
* I comandi `wsk` vengono ora eseguiti come `ibmcloud fn`.
* Il comando `wsk ibmcloud login` non è più necessario. Puoi accedere utilizzando `ibmcloud login`.
* Puoi gestire le tue API utilizzando `ibmcloud fn api`.

Per ulteriori informazioni, vedi il [riferimento alla CLI {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli).

### Autenticazione e host dell'API
{: #cli_api_auth}

Con il plug-in della CLI {{site.data.keyword.openwhisk_short}}, non devi specificare esplicitamente la chiave API e l'host API. Puoi invece accedere con `ibmcloud login`. Puoi specificare uno spazio dei nomi abilitato a IAM eseguendo `ibmcloud fn property set --namespace <namespace_name>` o uno spazio dei nomi basato su Cloud Foundry eseguendo `ibmcloud target --cf`. Dopo l'accesso, tutti i comandi iniziano con `ibmcloud fn`.


Se hai bisogno di utilizzare la chiave API di autenticazione per {{site.data.keyword.openwhisk_short}} in un client HTTP esterno come cURL o Postman, puoi recuperarla con i seguenti comandi.

Ottieni la chiave API corrente immettendo il seguente comando.
```
ibmcloud fn property get --auth
```
{: pre}

Ottieni l'host API corrente immettendo il seguente comando.
```
ibmcloud fn property get --apihost
```
{: pre}

La chiave API è specifica per ogni regione, organizzazione e spazio designato dal plug-in della CLI {{site.data.keyword.openwhisk_short}}.
{: tip}


### Autenticazione del gateway API
{: #cli_apigw_authentication}

La CLI OpenWhisk richiedeva l'esecuzione di `wsk ibmcloud login` per poter configurare l'autorizzazione del gateway API per la gestione delle tue API tramite il comando `wsk api`. Con il plug-in della CLI {{site.data.keyword.openwhisk_short}}, non devi eseguire `wsk ibmcloud login`. Invece, quando utilizzi il comando `ibmcloud login` per accedere a {{site.data.keyword.cloud_notm}}, il plug-in {{site.data.keyword.openwhisk}} utilizza automaticamente le tue informazioni di accesso e di destinazione correnti. Adesso puoi gestire le tue API utilizzando il comando `ibmcloud fn api`.


### Migrazione degli script di distribuzione
{: #cli_migrating_deploy_scripts}

Se disponi di script che utilizzano la CLI OpenWhisk con i comandi `wsk`, tutti i comandi funzionano allo stesso modo del comando `ibmcloud fn`. Puoi modificare i tuoi script per utilizzare il plug-in della CLI {{site.data.keyword.cloud_notm}} o creare un alias o wrapper in modo che i comandi correnti che utilizzano `wsk` vengano convertiti in `ibmcloud fn`. I comandi `ibmcloud login` e `ibmcloud target` nella CLI {{site.data.keyword.cloud_notm}} funzionano in modalità non presidiata. Con la modalità non presidiata, puoi configurare il tuo ambiente prima di eseguire i comandi `ibmcloud fn` per distribuire e gestire le tue entità {{site.data.keyword.openwhisk_short}}.

## Cronologia versione CLI
{: #cli_versions}

Un record cronologico delle versioni che mostrano evidenziazioni e correzioni di bug.

v1.0.30 (3 aprile 2019)
* Migliorata la gestione `service bind` di IAM e dei servizi basati sull'organizzazione e lo spazio.
* Aggiunta una correzione per la gestione dell'endpoint API https://cloud.ibm.com.

v1.0.29 (6 febbraio 2019)
* Aggiunti i comandi `deploy` e `undeploy` per distribuire o annullare la distribuzione di una raccolta di entità Functions tramite il file manifest. Per ulteriori informazioni, consulta la documentazione sulla [Distribuzione](/docs/openwhisk?topic=cloud-functions-deploy#deploy).

v1.0.28 (21 gennaio 2019)
* Aggiunto un messaggio di errore quando `update|delete|get namespace name` esiste più volte.

v1.0.27 (11 dicembre 2018)
* Aggiunte le correzioni `namespace get`.
* Aggiunta una correzione per `--save-as` quando un'azione è black box.

v1.0.26 (30 novembre 2018)
* Abilitato `fn property get --auth` per restituire correntemente la chiave di autenticazione in un nuovo ambiente.

v1.0.25 (23 novembre 2018)
* Migliorata la visualizzazione del risultato del messaggio di errore.
* Aggiunta una correzione `fn namespace get` per visualizzare correttamente le proprietà dello spazio dei nomi.

1.0.23 (15 ottobre 2018)
* Aggiunto il supporto per il riconoscimento del codice di azione ruby (`.rb`).

1.0.22 (20 agosto 2018)
* Aggiunta la regione us-east.

1.0.21 (1 agosto 2018)
* Gli alias `fn` e `functions` possono ora essere utilizzati per i comandi {{site.data.keyword.openwhisk_short}}: `ibmcloud fn <command>` e `ibmcloud fn <command>`. Puoi anche utilizzare ancora `ibmcloud wsk <command>`.

1.0.19 (2 luglio 2018)
* Correzioni di bug minori e miglioramenti.

1.0.18 (20 giugno 2018)
* Aggiunta una correzione per l'annullamento del bind delle istanze del servizio fornite dall'utente.
* Miglioramento delle prestazioni.

1.0.17 (12 giugno 2018)
* Aggiunto il supporto per il bind (`ibmcloud wsk service bind`) e l'annullamento del bind (`ibmcloud wsk service unbind`) delle istanze del servizio fornite dall'utente create mediante il comando `ibmcloud cf create-user-provided-service`.

1.0.16 (24 maggio 2018)
* Correzioni di bug minori e miglioramenti.

1.0.15 (21 maggio 2018)
* Correzioni di bug minori e miglioramenti.

1.0.14 (17 maggio 2018)
* Abilitato il supporto per il carattere `&` nei nomi di organizzazione e spazio.

1.0.13 (7 maggio 2018)
* Correzioni di bug minori e miglioramenti nella gestione degli errori.

1.0.12 (30 aprile 2018)
* Aggiornamenti all'SDK {{site.data.keyword.cloud_notm}} per mantenere la compatibilità con la CLI `bx`.

1.0.11 (23 aprile 2018)
* Correzioni di bug minori e miglioramenti.

1.0.10 (9 aprile 2018)
* Aggiunta la nuova opzione `--web-secure` ai comandi `ibmcloud wsk action create|update` per proteggere gli endpoint delle azioni web.
* Corretto il [difetto](https://github.com/apache/incubator-openwhisk-cli/issues/237){: external} del parametro percorso back-to-back.

1.0.9 (16 marzo 2018)
* Abilitato il supporto per il bind dei servizi a livello di pacchetto.

1.0.8 (22 febbraio 2018)
* Abilitato il supporto per il bind dei servizi IAM.

1.0.7 (2 febbraio 2018)
* Aggiornato `ibmcloud wsk api` per accettare i parametri percorso come `/api/{id}`. Per ulteriori informazioni, vedi [Gateway API](/docs/openwhisk?topic=cloud-functions-apigateway).
* Ripristinato il supporto proxy.
* Rimosso `swift:3`.

1.0.6 (30 gennaio 2018)
* Corretto un bug del comando `ibmcloud wsk service bind` per le azioni in un pacchetto.



