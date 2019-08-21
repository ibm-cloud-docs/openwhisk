---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: packages, installable packages, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Incorporazione di pacchetti
{: #pkg_ov}

I pacchetti sono insiemi assemblati di azioni e feed correlati. Ogni pacchetto è progettato per una specifica interazione con i servizi e i provider di eventi. Alcuni pacchetti sono già installati con {{site.data.keyword.openwhisk}} e pronti all'uso, ma puoi anche installarne altri.
{: shortdesc}

## Panoramica
{: #pkg_overview}

I [pacchetti preinstallati](/docs/openwhisk?topic=cloud-functions-pkg_ov#pkg_browse) vengono registrati automaticamente in {{site.data.keyword.openwhisk_short}} nello spazio dei nomi `/whisk.system`. Puoi utilizzarli senza completare alcuna procedura di installazione.

I pacchetti installabili sono pacchetti disponibili, che puoi installare, modificare o usare in base alle tue esigenze. I pacchetti installabili non si trovano all'interno del sistema {{site.data.keyword.openwhisk_short}}. I pacchetti installabili sono invece ospitati esternamente in singoli repository GitHub.

Puoi installare questi pacchetti o un tuo pacchetto direttamente nel tuo spazio dei nomi e puoi dare un nome personalizzato a un pacchetto. Poiché il pacchetto è installato nel tuo spazio dei nomi, puoi modificare le azioni e i feed nel pacchetto come necessario.



## Navigazione tra pacchetti preinstallati
{: #pkg_browse}

In {{site.data.keyword.openwhisk_short}} sono registrati vari pacchetti per te. Puoi ottenere un elenco dei pacchetti di uno spazio dei nomi, elencare le entità di un pacchetto e ottenere una descrizione delle singole entità di un pacchetto.
{: shortdesc}

1. Ottieni un elenco dei pacchetti dello spazio dei nomi `/whisk.system`.
  ```
  ibmcloud fn package list /whisk.system
  ```
  {: pre}

  Output dell'elenco dei pacchetti:
  ```
  packages
  /whisk.system/cloudant                                                 shared
  /whisk.system/alarms                                                   shared
  /whisk.system/watson                                                   shared
  /whisk.system/websocket                                                shared
  /whisk.system/weather                                                  shared
  /whisk.system/system                                                   shared
  /whisk.system/utils                                                    shared
  /whisk.system/slack                                                    shared
  /whisk.system/samples                                                  shared
  /whisk.system/github                                                   shared
  /whisk.system/pushnotifications                                        shared
  ```
  {: screen}

2. Ottieni un elenco di entità di un pacchetto.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **Esempio**
  ```
  ibmcloud fn package get --summary /whisk.system/cloudant
  ```
  {: pre}

  **Output di esempio**
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.cloud_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  Questo output mostra che il pacchetto {{site.data.keyword.cloudant_short_notm}} include azioni e un feed. Ad esempio, due azioni,`read` e `write`, e un feed trigger denominato `changes`. Il feed `changes` provoca l'attivazione dei trigger quando si aggiungono documenti al database {{site.data.keyword.cloudant_short_notm}} specificato.

  Il pacchetto {{site.data.keyword.cloudant_short_notm}} definisce, inoltre, i parametri `username`, `password`, `host` e `port`. Affinché le azioni e i feed siano significativi, è necessario specificare questi parametri. Ad esempio, i parametri consentono alle azioni di operare su un account {{site.data.keyword.cloudant_short_notm}} specifico.

3. Ottieni una descrizione di un'azione o di un feed per visualizzare i parametri richiesti.

  **Esempio**
  ```
  ibmcloud fn action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  **Output di esempio**
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```
  {: screen}

  Questo output mostra che l'azione `read` di {{site.data.keyword.cloudant_short_notm}} richiede tre parametri, compreso il database e l'ID documento da richiamare.



## Esecuzione del bind dei parametri a pacchetti preinstallati
{: #pkg_bind}

Sebbene tu possa usare direttamente le entità di un pacchetto, potresti ritrovarti ogni volta a trasmettere all'azione gli stessi parametri. Puoi semplificare il processo eseguendo il bind a un pacchetto e specificando i parametri predefiniti, che vengono ereditati dalle azioni del pacchetto.
{: shortdesc}

Ad esempio, nel pacchetto `/whisk.system/cloudant`, puoi impostare i valori `username`, `password` e `dbname` predefiniti in un bind di pacchetto, che verranno passati automaticamente a qualsiasi azione del pacchetto.

Nell'esempio di seguito riportato, puoi eseguire il bind del pacchetto `/whisk.system/samples`.

1. Esegui il bind del pacchetto `/whisk.system/samples` e importa un valore predefinito per il parametro `place`.
  ```
  ibmcloud fn package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  **Output di esempio**
  ```
  ok: created binding valhallaSamples
  ```
  {: screen}

2. Ottieni una descrizione del bind di pacchetto.
  ```
  ibmcloud fn package get --summary valhallaSamples
  ```
  {: pre}

  **Output di esempio**
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Returns a friendly greeting
   action /myNamespace/valhallaSamples/wordCount: Count words in a string
   action /myNamespace/valhallaSamples/helloWorld: Demonstrates logging facilities
   action /myNamespace/valhallaSamples/curl: Curl a host url
  ```
  {: screen}

  Nota che tutte le azioni del pacchetto `/whisk.system/samples` sono disponibili nel bind di pacchetto `valhallaSamples`.

3. Richiama un'azione nel bind di pacchetto.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  **Output di esempio**
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  Dal risultato, puoi notare che l'azione eredita il parametro `place` che hai impostato durante la creazione del bind di pacchetto `valhallaSamples`.

4. Richiama un'azione e sovrascrivi il valore predefinito del parametro.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  **Output di esempio**
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  Nota che il valore del parametro `place` specificato con la chiamata dell'azione sovrascrive il valore predefinito impostato nel bind di pacchetto `valhallaSamples`.



## Aggiunta dei tuoi pacchetti
{: #pkg_add}

Puoi creare un pacchetto di codice locale o un clone di qualsiasi repository GitHub.
{: shortdesc}

**Prima di iniziare**
- [Installa il plug-in {{site.data.keyword.openwhisk_short}} per la CLI {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).
- Crea un file `manifest.yaml` o `manifest.yml` per la tua applicazione e memorizzalo nella directory root. Il file`manifest.yaml` specifica la struttura generale del pacchetto, compresi eventuali metadati che devono essere inclusi con il comando `ibmcloud fn deploy`. Per ulteriori informazioni sui file `manifest.yaml`, vedi la [documentazione di <ph class="ignoreSpelling">wskdeploy</ph>](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example){: external}.

Per aggiungere un pacchetto:

1. Clona il repository del pacchetto.
    ```
    git clone https://github.com/ORG_NAME/REPOSITORY_NAME
    ```
    {: pre}

2. Passa alla directory che contiene il file `manifest.yaml`.
    ```
    cd <filepath>/<nome_pacchetto>
    ```
    {: pre}

3. Distribuisci il pacchetto.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    Alcuni pacchetti richiedono delle specifiche variabili di ambiente per abilitare un loro corretto funzionamento In tal caso, includere le variabili di ambiente con il comando `deploy` . Ad esempio, è possibile scegliere un nome per il pacchetto e specificarlo con la variabile PACKAGE_NAME.

    ```
    PACKAGE_NAME=CUSTOM_PACKAGE_NAME VARIABLE_NAME=VARIABLE_VALUE ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

### Esempio di pacchetto {{site.data.keyword.cos_full_notm}}
{: #pkg_ex}

Per vedere un esempio di modalità di installazione di un pacchetto, consulta il [pacchetto {{site.data.keyword.cos_full_notm}}](/docs/openwhisk?topic=cloud-functions-pkg_obstorage). {{site.data.keyword.cos_full}} è un servizio che consente agli utenti di memorizzare tutti i tipi di file, quali immagini, video, musica e testo. Per interagire con i file, un archivio dati basato su cloud di coppie chiave-valore viene memorizzato in un bucket. Quindi, per utilizzare il [pacchetto {{site.data.keyword.cos_full_notm}}](/docs/openwhisk?topic=cloud-functions-pkg_obstorage), devi creare prima un'istanza del servizio {{site.data.keyword.cos_full_notm}}, poi un bucket. Il bucket viene utilizzato come una variabile di ambiente richiesta per installare questo pacchetto.

Dopo aver creato l'istanza del servizio e il bucket, puoi installare il pacchetto utilizzando i seguenti comandi:

1. Clona il repository del pacchetto.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Passa alla directory del pacchetto che contiene il file `manifest.yaml`. In questo esempio, viene utilizzata la versione di runtime Node.js del pacchetto {{site.data.keyword.cos_full_notm}}.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Distribuisci il pacchetto, utilizzando il tuo bucket come una variabile di ambiente.  Puoi fornire un nome personalizzato al pacchetto utilizzando la variabile di ambiente `PACKAGE_NAME`.
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> ibmcloud fn deploy
    ```
    {: pre}



