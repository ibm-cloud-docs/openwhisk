---

copyright:
  years: 2018
lastupdated: "2018-07-19"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}

# Utilizzo di pacchetti installabili
{: #installable-packages-overview}

I pacchetti installabili rappresentano un nuovo modello per la gestione e l'interazione con i pacchetti in {{site.data.keyword.openwhisk}}.
{: shortdesc}

## Panoramica
{: #overview}

**Cosa sono i pacchetti installabili?**

I pacchetti sono insiemi assemblati di azioni e feed correlati. Ogni pacchetto è progettato per una specifica interazione con i servizi e i provider di eventi. I pacchetti installabili sono pacchetti a tua disposizione che puoi scegliere, installare e modificare in base alle tue esigenze.

**In che modo i pacchetti installabili sono diversi dai pacchetti preinstallati?**

I [pacchetti preinstallati](openwhisk_packages.html#browse-packages) vengono registrati automaticamente in {{site.data.keyword.openwhisk_short}} nello spazio dei nomi `/whisk.system`. Per memorizzare le credenziali o altri parametri in un pacchetto preinstallato, devi creare i [bind di pacchetto](openwhisk_packages.html#openwhisk_package_bind).

I pacchetti installabili non si trovano all'interno del sistema {{site.data.keyword.openwhisk_short}}. I pacchetti installabili sono invece ospitati esternamente in singoli repository Github. Puoi installare questi pacchetti direttamente nel tuo spazio dei nomi utilizzando lo strumento [wskDeploy](https://github.com/apache/incubator-openwhisk-wskdeploy#whisk-deploy-wskdeploy) e puoi dare a un pacchetto un nome personalizzato. Poiché il pacchetto è installato nel tuo spazio dei nomi, puoi modificare le azioni e i feed nel pacchetto come necessario.

## Installazione dei pacchetti utilizzando wskDeploy
{: #installing}

Prima di iniziare:
  1. [Installa il plug-in {{site.data.keyword.openwhisk_short}} per la CLI {{site.data.keyword.Bluemix_notm}}](bluemix_cli.html#cloudfunctions_cli).
  2. Installa il comando [`wskdeploy` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) e aggiungi il file binario scaricato al tuo PERCORSO.

Per installare un pacchetto:

1. Clona il repository del pacchetto. I repository del pacchetto sono disponibili nelle singole pagine per ciascun pacchetto in questa serie di documentazione.
    ```
    git clone https://github.com/<repository_pacchetto>
    ```
    {: pre}

2. Passa alla directory del pacchetto che contiene un file `manifest.yaml` o `manifest.yml`. Il file `manifest.yaml` specifica la struttura generale del pacchetto, inclusi il pacchetto e le azioni da installare nel tuo spazio dei nomi e gli eventuali metadati che devono essere inclusi con il comando `wskdeploy`. Per ulteriori informazioni sui file `manifest.yaml`, vedi la [documentazione di wskdeploy![Icona link esterno](../icons/launch-glyph.svg "EIcona link esterno")](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example).
    ```
    cd <filepath>/<nome_pacchetto>
    ```
    {: pre}

3. Distribuisci il pacchetto. Alcuni pacchetti richiedono delle specifiche variabili di ambiente per abilitare un loro corretto funzionamento
    ```
    wskdeploy -m manifest.yaml
    ```
    {: pre}

### Esempio di utilizzo del pacchetto {{site.data.keyword.cos_full_notm}}
{: #example}

Per visualizzare un esempio di come installare un pacchetto, consulta il [pacchetto {{site.data.keyword.cos_short}}](cloud_object_storage_actions.html). {{site.data.keyword.cos_full}} è un servizio che consente agli utenti di memorizzare tutti i tipi di file, quali immagini, video, musica e testo. Per interagire con i file, un archivio dati basato sul cloud di coppie chiave/valore viene memorizzato in un bucket. Quindi, per utilizzare il [pacchetto {{site.data.keyword.cos_short}}](cloud_object_storage_actions.html), devi prima creare un'istanza del servizio {{site.data.keyword.cos_short}} e creare quindi un bucket. Il bucket viene utilizzato come una variabile di ambiente richiesta per installare questo pacchetto.

Dopo che hai creato l'istanza del servizio e il bucket, l'installazione del pacchetto richiede i seguenti comandi:

1. Clona il repository del pacchetto.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Passa alla directory del pacchetto che contiene il file `manifest.yaml`. In questo esempio, viene utilizzata la versione di runtime Node.js del pacchetto {{site.data.keyword.cos_short}}.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Distribuisci il pacchetto, utilizzando il tuo bucket come una variabile di ambiente. La dipendenza sulla variabile di ambiente ` PACKAGE_NAME` ti consente di fornire a questo pacchetto un nome personalizzato.
    ```
    PACKAGE_NAME=<nome_pacchetto_personalizzato> BUCKET=<nome_bucket> wskdeploy
    ```
    {: pre}
