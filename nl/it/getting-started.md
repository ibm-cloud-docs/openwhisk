---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: functions, ibm cloud functions, cloud functions, getting started, creating actions

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


# Introduzione a IBM {{site.data.keyword.openwhisk_short}}
{: #getting-started}

Con {{site.data.keyword.openwhisk}} puoi utilizzare il tuo linguaggio di programmazione preferito per scrivere un codice leggero che esegue la logica dell'applicazione in un modo scalabile. Puoi eseguire il codice su richiesta tramite richieste API basate su HTTP provenienti dalle applicazioni o automaticamente in risposta a eventi di terze parti e servizi {{site.data.keyword.cloud_notm}}. La piattaforma di programmazione Faas (Function-as-a-Service) si basa sul progetto open source Apache OpenWhisk.
{: shortdesc}

## Utilizzo delle azioni
{: #gs_actions}

Con {{site.data.keyword.openwhisk}}, puoi creare dei frammenti di codice senza stato configurati per eseguire un'attività specifica nota come un'azione. Per ulteriori informazioni sulle azioni e su altri termini Functions, vedi la [terminologia](/docs/openwhisk?topic=cloud-functions-about).
{:shortdesc}

Fai clic su un'azione per iniziare.

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="Fai clic su un'icona per iniziare ad utilizzare velocemente {{site.data.keyword.openwhisk_short}}." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#gs_hello_world" alt="Crea un'azione" title="Crea un'azione" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=cloud-functions-cli_install" alt="Configura il plug-in della CLI {{site.data.keyword.openwhisk_short}}" title="Configura il plug-in della CLI {{site.data.keyword.openwhisk_short}}" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=cloud-functions-about" alt="Vedi l'architettura della piattaforma" title="Vedi l'architettura della piattaforma" shape="rect" coords="326, -10, 448, 218" />
</map>

## Creazione di un'azione nella GUI
{: #gs_hello_world}

Per iniziare con {{site.data.keyword.openwhisk_short}}, prova a creare il template quickstart Hello World.

1. Crea un account [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/registration){: external} o accedi a un account esistente.

2. Passa al [dashboard {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk){: external}.

2. Fai clic su **Avvia creazione** > **Template Quickstart** e seleziona il template **Hello World**.

3. Crea un pacchetto per le tue azioni immettendo un nome univoco nel campo **Nome pacchetto**.

4. Seleziona un runtime dall'elenco a discesa nella sezione **Azione helloworld**. Puoi visualizzare un'anteprima del codice per l'azione di esempio in ogni runtime disponibile prima di distribuire il template.

5. Fai clic su **Distribuisci**. Hai creato un'azione. Ottimo lavoro!

6. Esegui l'azione facendo clic su **Richiamo**. La chiamata di un'azione esegue manualmente la logica dell'applicazione definita dall'azione. Nel pannello **Attivazioni**, puoi vedere il saluto "Hello stranger!" prodotto dall'azione.

7. Facoltativo: fai clic su **Modifica input** per modificare l'azione e provare da solo.

  a. Incolla il seguente codice nella casella **Modifica input** e cambia il valore del nome.
  ```
  { "name": "xxxx" }
  ```
  {: codeblock}
  b. Successivamente, fai clic su **Richiama** per eseguire l'azione con le tue modifiche. Il risultato viene mostrato nel pannello **Attivazioni**. Puoi ripetere questo processo quante volte desideri.

Ottimo lavoro! Hai creato la tua prima azione. Per ripulire questa azione, fai clic sul menu di overflow e seleziona **Elimina azione**.

## Creazione di un'azione nella CLI
{: #gs_hello_world_cli}

Diventa subito operativo con il codice di esempio [JavaScript Hello World](/docs/openwhisk?topic=cloud-functions-prep#prep-js). Questo esempio crea un'azione `hello` di base che puoi richiamare manualmente per eseguire la sua logica dell'applicazione.

## Operazioni successive
{: #gs_next_steps}

Hai completato la tua prima distribuzione del template; cosa devi fare adesso?

* Prendere familiarità con la [terminologia](/docs/openwhisk?topic=cloud-functions-about#about_technology).
* Iniziare ad utilizzare le [tue azioni](/docs/openwhisk?topic=cloud-functions-actions).
* Imparare ad organizzare le azioni in [pacchetti](/docs/openwhisk?topic=cloud-functions-pkg_ov).
* Opzione avanzata - Creare un'[API REST senza server](/docs/openwhisk?topic=cloud-functions-apigateway).



