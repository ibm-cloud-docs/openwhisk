---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# CLI autonoma di {{site.data.keyword.openwhisk_short}}

**A partire dal 9 marzo 2018, la CLI autonoma di {{site.data.keyword.openwhisk_short}} non sarà più disponibile per il download. Per continuare a gestire le entità di {{site.data.keyword.openwhisk_short}} dopo questa data, utilizza il [plug-in CLI {{site.data.keyword.openwhisk_short}}](./bluemix_cli.html) per la CLI {{site.data.keyword.Bluemix}}.**

L'interfaccia riga di comando **wsk** distribuita di {{site.data.keyword.openwhisk}} consente la gestione di tutte le entità {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

<!--
This service is deprecated: All instances of this service are deprecated. Existing instances can be used until 09 December 2016. For more information, see the [deprecation announcement blog](http://www.com){: new_window}.
{:deprecated}

IBM recommends that you use the new [{{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.Bluemix_notm}} CLI](./bluemix_cli.html) to manage {{site.data.keyword.openwhisk_short}} entities. The following management tasks are easier if you use the plugin.
{: tip}
-->

* Configurazione della chiave di autenticazione {{site.data.keyword.openwhisk_short}}
  * Nessuna necessità di accedere alla console {{site.data.keyword.openwhisk_short}} per ottenere la chiave di autenticazione
  * Generazione automatica della chiave di autenticazione che si verifica quando si passa tra regioni, organizzazioni e spazi
* Aggiornamento di una chiave di autenticazione {{site.data.keyword.openwhisk_short}} scaduta
* Aggiornamento della CLI a una versione successiva


Le seguenti attività di gestione richiedono l'utilizzo del plug-in.

* Gestione delle API
  * Configurazione del token di accesso del gateway API
  * Aggiornamento di un token di accesso scaduto del gateway API

## Configurazione della CLI {{site.data.keyword.openwhisk_short}} 
{: #openwhisk_cli_setup}

Puoi utilizzare l'interfaccia di riga comando (CLI) {{site.data.keyword.openwhisk_short}} per configurare il tuo spazio dei nomi e la tua chiave di autorizzazione.
Passa a [Configura CLI](https://console.bluemix.net/openwhisk/cli?loadWsk=true) e segui le istruzioni per installarla.

Per prima cosa, configura le seguenti due proprietà:

1. **Host API** (nome o indirizzo IP) per la distribuzione {{site.data.keyword.openwhisk_short}} che vuoi utilizzare.
2. **Chiave di autorizzazione** (nome utente e password) che ti concede l'accesso all'API {{site.data.keyword.openwhisk_short}}.

Sono disponibili due regioni {{site.data.keyword.Bluemix_notm}} che richiedono i propri host API e chiave di autorizzazione univoci.

* Stati Uniti Sud
  * Host API: `openwhisk.ng.bluemix.net`

* Regno Unito
  * Host API: `openwhisk.eu-gb.bluemix.net`

Immetti il seguente comando per impostare l'host API per la regione {{site.data.keyword.Bluemix_notm}} desiderata:

Stati Uniti Sud:
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

Regno Unito:
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

Se hai bisogno di cambiare la regione, devi riconfigurare la CLI sia con l'host API che con la chiave di autorizzazione poiché la chiave di autorizzazione è specifica per regione.
{: tip}

Risorse come azioni, regole e pacchetti sono specifiche della regione. Pertanto, se utilizzi la stessa risorsa in più regioni, devi distribuirla in ciascuna regione desiderata.

Se conosci la tua chiave di autorizzazione, puoi configurare la CLI per utilizzarla. 

Immetti il seguente comando per impostare la chiave di autorizzazione:

```
wsk property set --auth <chiave_autorizzazione>
```
{: pre}

**Suggerimento:** la CLI {{site.data.keyword.openwhisk_short}} memorizza le proprietà impostate in `~/.wskprops` per impostazione predefinita. È possibile modificare la posizione di questo file impostando la variabile di ambiente `WSK_CONFIG_FILE`. 

Per verificare la configurazione della tua CLI, prova a [creare ed eseguire un'azione](./index.html#openwhisk_start_hello_world).

## Utilizzo della CLI {{site.data.keyword.openwhisk_short}}
{: #using_openwhisk_cli}

Una volta configurato l'ambiente, puoi utilizzare la CLI {{site.data.keyword.openwhisk_short}} per effettuare le seguenti attività:

* Eseguire i tuoi frammenti di codice, o azioni, su {{site.data.keyword.openwhisk_short}}. Vedi [Crea e richiama le azioni](./openwhisk_actions.html).
* Utilizzare i trigger e le regole per consentire alle tue azioni di rispondere agli eventi. Vedi [Crea trigger e regole](./openwhisk_triggers_rules.html).
* Imparare in che modo i pacchetti riuniscono le azioni e configurano origini eventi esterne. Vedi [Utilizzo e creazione di pacchetti](./openwhisk_packages.html).
* Esplorare il catalogo dei pacchetti e ampliare le tue applicazioni con servizi esterni, quali un'[origine eventi Cloudant](./openwhisk_cloudant.html). Vedi [Pacchetti {{site.data.keyword.openwhisk_short}} preinstallati](./openwhisk_catalog.html).

## Configura la CLI per utilizzare un proxy HTTPS
{: #cli_https_proxy}

La CLI può essere configurata per utilizzare un proxy HTTPS. Per configurare un proxy HTTPS, è necessario creare una variabile di ambiente denominata `HTTPS_PROXY`. La variabile deve essere impostata sull'indirizzo del proxy HTTPS e sulla sua porta utilizzando il seguente formato: `{PROXY IP}:{PROXY PORT}`.
