---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Introduzione a {{site.data.keyword.openwhisk_short}}

{{site.data.keyword.openwhisk}} è un servizio di calcolo distribuito guidato dagli eventi, indicato anche come calcolo senza server o FaaS (Function as a Service). {{site.data.keyword.openwhisk_short}} esegue la logica dell'applicazione in risposta a eventi o chiamate dirette provenienti da applicazioni web o mobili su HTTP. Gli eventi possono essere forniti da servizi {{site.data.keyword.Bluemix}} come Cloudant e da origini esterne. Gli sviluppatori possono concentrarsi sulla scrittura della logica dell'applicazione e sulla creazione di azioni che vengono eseguite su richiesta.
Il vantaggio principale di questo nuovo paradigma è che non esegui esplicitamente il provisioning dei server, eliminando così la preoccupazione per il ridimensionamento automatico, la disponibilità elevata, gli aggiornamenti, la manutenzione e il costo per le ore del tempo di processore quando il server è in esecuzione ma non serve le richieste.
Il codice viene eseguito quando c'è una chiamata HTTP, una modifica dello stato del database o un altro tipo di evento che attiva l'esecuzione del tuo codice.
Ti viene addebitato ogni millisecondo del tempo di esecuzione (arrotondato per eccesso a 100 ms) e non all'ora di utilizzo della VM, indipendentemente dal fatto che la VM abbia svolto un lavoro utile o meno.
{: shortdesc}

Questo modello di programmazione è perfetto per microservizi, dispositivi mobili, IoT e molte altre applicazioni. Ottieni il ridimensionamento automatico e il bilanciamento del carico predefiniti intrinseci, senza dover configurare manualmente cluster, programmi di bilanciamento del carico, plug-in HTTP, eccetera. Se utilizzi {{site.data.keyword.openwhisk}}, hai anche il vantaggio di non doverti preoccupare di alcuna forma di amministrazione, il che significa che l'hardware, la rete e il software sono tutti gestiti da IBM. Tutto ciò che devi fare è fornire il codice che desideri eseguire e darlo a {{site.data.keyword.openwhisk}}. Il resto è “magia”. Un'accurata introduzione al modello di programmazione senza server è disponibile sul [blog di Martin Fowler](https://martinfowler.com/articles/serverless.html).

Puoi anche ottenere il [codice sorgente di Apache OpenWhisk](https://github.com/openwhisk/openwhisk) ed eseguire il sistema autonomamente.

Per ulteriori dettagli sul funzionamento di {{site.data.keyword.openwhisk_short}}, vedi [Informazioni su {{site.data.keyword.openwhisk_short}}](./openwhisk_about.html).

Per sviluppare le tue applicazioni {{site.data.keyword.openwhisk_short}}, puoi utilizzare il browser o la CLI.
Entrambi hanno funzionalità simili per sviluppare le applicazioni, tuttavia la CLI fornisce maggiore controllo sulle attività di distribuzione e operative.

## Sviluppa nel tuo browser
{: #openwhisk_start_editor}

Prova {{site.data.keyword.openwhisk_short}} nel tuo [browser](https://console.{DomainName}/openwhisk/actions) per creare azioni, automatizzare azioni utilizzando i trigger ed esplorare i pacchetti pubblici. Visita la pagina [Ulteriori informazioni](https://console.{DomainName}/openwhisk/learn) per un'introduzione rapida dell'interfaccia utente {{site.data.keyword.openwhisk_short}}.

## Sviluppa con la CLI
{: #openwhisk_start_configure_cli}

Puoi utilizzare l'interfaccia di riga comando (CLI) {{site.data.keyword.openwhisk_short}} per configurare il tuo spazio dei nomi e la tua chiave di autorizzazione.
Passa a [Configura CLI](https://console.{DomainName}/openwhisk/cli) e segui le istruzioni per installarla.

## Panoramica
{: #openwhisk_start_overview}
- [Come funziona OpenWhisk](./openwhisk_about.html)
- [Casi di utilizzo comuni per le applicazioni senza server](./openwhisk_use_cases.html)
- [Configurazione e utilizzo della CLI OpenWhisk](./openwhisk_cli.html)
- [Utilizzo di OpenWhisk da un'applicazione iOS](./openwhisk_mobile_sdk.html)
- [Articoli, esempi ed esercitazioni](https://github.com/openwhisk/openwhisk-external-resources)
- [FAQ su Apache OpenWhisk](http://openwhisk.org/faq)
- [Prezzi](https://console.ng.bluemix.net/openwhisk/learn/pricing)

## Modello di programmazione
{: #openwhisk_start_programming}
- [Dettagli di sistema](./openwhisk_reference.html)
- [Catalogo dei servizi forniti da OpenWhisk](./openwhisk_catalog.html)
- [Azioni](./openwhisk_actions.html)
- [Trigger e regole](./openwhisk_triggers_rules.html)
- [Feed](./openwhisk_feeds.html)
- [Pacchetti](./openwhisk_packages.html)
- [Annotazioni](./openwhisk_annotations.html)
- [Azioni Web](./openwhisk_webactions.html)
- [Gateway API](./openwhisk_apigateway.html)
- [Nomi delle entità](./openwhisk_reference.html#openwhisk_entities)
- [Semantica delle azioni](./openwhisk_reference.html#openwhisk_semantics)
- [Limiti](./openwhisk_reference.html#openwhisk_syslimits)

## Esempio {{site.data.keyword.openwhisk_short}} Hello World
{: #openwhisk_start_hello_world}
Per iniziare a utilizzare {{site.data.keyword.openwhisk_short}}, prova il seguente esempio di codice JavaScript.

```javascript
/**
 * Hello world come azione OpenWhisk.
 */
function main(params) {
    var name = params.name || 'World';
    return {payload:  'Hello, ' + name + '!'};
}
```
{: codeblock}

Per utilizzare questo esempio, completa la seguente procedura:

1. Salva il codice in un file. Ad esempio, *hello.js*.

2. Dalla riga di comando della CLI {{site.data.keyword.openwhisk_short}}, crea l'azione immettendo questo comando:
    ```
    wsk action create hello hello.js
    ```
    {: pre}

3. Quindi, richiama l'azione immettendo i seguenti comandi.
    ```
    wsk action invoke hello --blocking --result
    ```
    {: pre}  

    Questo comando ha il seguente output:
    ```json
    {
        "payload": "Hello, World!"
    }
    ```
    
    ```
    wsk action invoke hello --blocking --result --param name Fred
    ```
    {: pre}  

    Questo comando ha il seguente output:
    ```json
    {
        "payload": "Hello, Fred!"
    }
    ```

Puoi anche utilizzare le funzionalità guidate dagli eventi in {{site.data.keyword.openwhisk_short}} per richiamare questa azione in risposta agli eventi. Segui l'[esempio del servizio di allarme](./openwhisk_packages.html#openwhisk_package_trigger) per configurare un'origine eventi che richiami l'azione `hello` ogni volta che viene generato un evento periodico.

Un elenco completo di [Esercitazioni ed esempi OpenWhisk può essere trovato qui](https://github.com/openwhisk/openwhisk-external-resources#sample-applications). Oltre agli esempi, questo repository contiene link ad articoli, presentazioni, podcast, video e altre risorse correlate a {{site.data.keyword.openwhisk_short}}.

## Guida di riferimento API
{: #openwhisk_start_api notoc}
* [Documentazione API REST](./openwhisk_reference.html#openwhisk_ref_restapi)
* [API REST](https://console.{DomainName}/apidocs/98)

## Link correlati
{: #general notoc}
* [Scopri: {{site.data.keyword.openwhisk_short}}](http://www.ibm.com/cloud-computing/bluemix/openwhisk/)
* [{{site.data.keyword.openwhisk_short}} su IBM developerWorks](https://developer.ibm.com/openwhisk/)
* [Sito web dei progetti Apache {{site.data.keyword.openwhisk_short}}](http://openwhisk.org)
