---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: deploying actions, manifest, manifest file, functions

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


# Distribuzione delle entità con un file manifest
{: #deploy}

Puoi utilizzare {{site.data.keyword.openwhisk_short}} per descrivere e distribuire tutte le entità del tuo spazio dei nomi utilizzando un file manifest scritto in YAML. Puoi utilizzare questo file per distribuire tutti i tuoi [Pacchetti](/docs/openwhisk?topic=cloud-functions-pkg_ov), [Azioni](/docs/openwhisk?topic=cloud-functions-actions), [Trigger](/docs/openwhisk?topic=cloud-functions-triggers) e [Regole](/docs/openwhisk?topic=cloud-functions-rules) Functions con un solo comando.

Il file manifest descrive una serie di entità che potresti voler distribuire e annullarne la distribuzione a un gruppo. I contenuti del file manifest devono rispettare la [specifica YAML di distribuzione OpenWhisk](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#package-specification){: external}. Una volta definito, puoi utilizzare il tuo file manifest per distribuire o ridistribuire un gruppo di entità Functions nello stesso o in un diverso spazio dei nomi Functions. Puoi utilizzare i comandi del plug-in Functions `ibmcloud fn deploy` e `ibmcloud fn undeploy` per distribuire e annullare la distribuzione delle entità Functions definite nel tuo file manifest.

## Creazione dell'esempio API Hello World
{: #deploy_helloworld_example}

Questo esempio utilizza un semplice codice Node.js `helloworld.js`, crea un'azione web `hello_world` all'interno di un pacchetto `hello_world_package` e definisce un'API REST per questa azione.
{: shortdesc}

1. Crea un file `helloworld.js` con il seguente codice.

    ```javascript
    function main() {
       return {body: 'Hello world'};
}
    ```
    {: codeblock}

    Il file manifest di distribuzione definisce le seguenti variabili.
    * Il nome del pacchetto.
    * Il nome dell'azione.
    * L'annotazione di azioni che indica che si tratta di un'azione web.
    * Il nome file del codice azione.
    * L'API con un percorso di base di `/hello`.
    * Il percorso endpoint di `/world`.

2. Crea il file `hello_world_manifest.yml`.

    ```yaml
    packages:
  hello_world_package:
    version: 1.0
    license: Apache-2.0
    actions:
      hello_world:
        function: helloworld.js
        web-export: true
    apis:
      hello-world:
        hello:
          world:
            hello_world:
              method: GET
              response: http
    ```
    {: codeblock}

3. Utilizza il comando `deploy` per distribuire il pacchetto, l'azione e l'API.

    ```sh
    ibmcloud fn deploy --manifest hello_world_manifest.yml
    ```
    {: pre}

4. Puoi elencare le azioni, i pacchetti e le API per confermare che le tre entità previste sono state create correttamente.

    1. Elenca le azioni utilizzando il seguente comando.

      ```sh
      ibmcloud fn action list
      ```
      {: pre}

    2. Elenca i pacchetti utilizzando il seguente comando.

      ```sh
      ibmcloud fn package list
      ```
      {: pre}

    3. Elenca le API utilizzando il seguente comando.

      ```sh
      ibmcloud fn api list
      ```
      {: pre}

5. Richiama l'API.

    ```sh
    curl URL-FROM-API-LIST-OUTPUT
    ```
    {: codeblock}

Facoltativo: puoi annullare la distribuzione delle stesse entità utilizzando il comando `undeploy`.

```sh
ibmcloud fn undeploy --manifest hello_world_manifest.yml
```
{: codeblock}

## Altri esempi di distribuzione OpenWhisk
{: more_deploy_examples}

La distribuzione Functions si basa sul progetto di distribuzione OpenWhisk, che dispone di [più esempi di manifest di distribuzione](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#guided-examples){: external} che possono essere utilizzati in Functions.  Puoi utilizzare il comando `ibmcloud fn deploy` invece di `wskdeploy`.

## Specifica del manifest di distribuzione
{: manifest_specification}

I manifest di distribuzione Functions devono rispettare la specifica del manifest di distribuzione OpenWhisk. Per i dettagli, fai riferimento alla [documentazione della specifica del manifest di distribuzione OpenWhisk](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#openwhisk-packaging-specification){: external}.




