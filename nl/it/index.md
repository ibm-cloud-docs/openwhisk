---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Introduzione
{: #index}

{{site.data.keyword.openwhisk}} è una piattaforma di programmazione FaaS (Functions-as-a-Service) poliglotta basata su Apache OpenWhisk. {{site.data.keyword.openwhisk_short}} consente agli sviluppatori di scrivere un codice leggero denominato azioni in grado di eseguire in modo scalabile la logica dell'applicazione. Puoi configurare le azioni in modo che si verifichino su richiesta in base a richieste API HTTP dalle applicazioni web o mobili oppure automaticamente in risposta alle richieste basate su eventi da servizi {{site.data.keyword.Bluemix_notm}} ed eventi di terze parti.
{: shortdesc}

Poiché {{site.data.keyword.openwhisk_short}} è una piattaforma senza server basata su eventi, non dovrai eseguire il provisioning esplicito dei server. Gli sviluppatori che lavorano con microservizi, dispositivi mobili, IoT e molte altre applicazioni possono concentrarsi sulla scrittura della logica dell'applicazione anziché preoccuparsi del ridimensionamento automatico, dell'alta disponibilità, degli aggiornamenti e della manutenzione. Il ridimensionamento automatico e il bilanciamento del carico predefiniti significano che non dovrai configurare manualmente cluster, plug-in HTTP e così via. IBM si occupa di tutta l'amministrazione dell'hardware, rete e software. Tutto quello che devi fare è fornire il codice.

Fai clic su un'azione per iniziare:

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="Fai clic su un'icona per iniziare rapidamente con {{site.data.keyword.openswhisk_short}}." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#openwhisk_start_hello_world" alt="Crea un'azione" title="Crea un'azione" shape="rect" coords="-7, -8, 108, 211" />
<area href="bluemix_cli.html" alt="Configura il plug-in della CLI {{site.data.keyword.openwhisk_short}} " title="Configura il plug-in della CLI {{site.data.keyword.openwhisk_short}} " shape="rect" coords="155, -1, 289, 210" />
<area href="openwhisk_about.html" alt="Vedi l'architettura della piattaforma" title="Vedi l'architettura della piattaforma" shape="rect" coords="326, -10, 448, 218" />
</map>


## Creazione di un'azione nella GUI
{: #openwhisk_start_hello_world}

Per iniziare con {{site.data.keyword.openwhisk_short}}, prova a utilizzare il template quickstart HelloWorld.

1.  Dal [ **Catalogo** {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/catalog/?category=whisk), nella categoria **Functions**, fai clic su Functions.

2. Fai clic su **Avvia creazione** > **Template Quickstart** e seleziona il template **Hello World**.

5. Controlla il codice per l'azione, quindi crea l'azione facendo clic su **Distribuisci**. Adesso hai un'azione chiamata `hello`.

6. Esegui l'azione facendo clic su **Richiamo**. Il richiamo di un'azione esegue manualmente il codice dell'applicazione definito dall'azione. Nel pannello **Attivazioni**, puoi vedere il saluto "Hello stranger!" prodotto dall'azione.

Ottimo lavoro! Hai creato la tua prima azione. Per ripulire questa azione, fai clic sul menu di overflow e seleziona **Elimina azione**.

## Creazione di un'azione nella CLI
{: #openwhisk_start_hello_world_cli}

Diventa subito operativo con il codice di esempio JavaScript di HelloWorld. Questo esempio crea un'azione `hello` di base che puoi richiamare manualmente per eseguire la sua logica dell'applicazione. 

1. [Configura il plug-in della CLI {{site.data.keyword.openwhisk_short}}](bluemix_cli.html).

2. Salva il seguente codice in un file denominato **hello.js**.

    ```javascript
    /**
     * Hello world as an OpenWhisk action.
     */
function main(params) {
        var name = params.name || 'World';
    return {payload:  'Hello, ' + name + '!'};
    }
    ```
    {: codeblock}

3. Crea l'azione `hello`.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

4. Richiama l'azione senza passare alcun parametro.
    ```
    ibmcloud fn action invoke hello --blocking --result
    ```
    {: pre}  

    Output:
    ```
    {
        "payload": "Hello, World!"
    }
    ```
    {: screen}

5. Richiama nuovamente l'azione, passando il parametro nome, per testare la logica dell'applicazione.
    ```
    ibmcloud fn action invoke hello --blocking --result --param name Fred
    ```
    {: pre}  

    Output:
    ```
    {
        "payload": "Hello, Fred!"
    }
    ```
    {: screen}

Ottimo lavoro! Hai creato la tua prima azione. Per ripulire questa azione, esegui `ibmcloud fn action delete hello`.

**Operazioni successive**
* [Prova l'esempio del servizio di allarme per richiamare l'azione **hello** ogni volta che viene generato un evento periodico.](./openwhisk_packages.html#openwhisk_package_trigger)
* [Crea un'API REST senza server.](openwhisk_apigateway.html)
* [Controlla i pacchetti di azioni pre-installati per i servizi {{site.data.keyword.Bluemix_notm}} come Cloudant.](cloudant_actions.html)
