---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: planning, functions, actions, serverless

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


# Pianificazione di applicazioni senza server
{: #plan}

Prima di iniziare a creare la tua funzione, scopri tutto sulle decisioni che dovrai prendere lungo il cammino.
{: shortdesc}

## Esamina il supporto di runtime
{: #plan_runtime}

{{site.data.keyword.openwhisk_short}} fornisce [runtime](/docs/openwhisk?topic=cloud-functions-runtimes#runtimes) standard per l'esecuzione del codice. Tuttavia, poiché {{site.data.keyword.openwhisk_short}} è senza server, non sei limitato nei runtime che puoi utilizzare. Puoi creare runtime personalizzati, creando una tua [immagine Docker personalizzata](/docs/openwhisk?topic=cloud-functions-actions#actions-docker) per inserire il tuo codice in un pacchetto.
{: shortdesc}



## Progetta il tuo codice
{: #plan_architect}

È possibile che tu debba perfezionare il tuo codice esistente per poterlo eseguire in {{site.data.keyword.openwhisk}}. Se non hai ancora il tuo codice, tieni a mente queste cose mentre lo scrivi.
{: shortdesc}

1. Suddividi il codice.

    Se hai già un'applicazione e vuoi renderla senza server, pensa a come potresti dover suddividere la tua applicazione in parti più piccole. Ogni funzione contiene una serie di trigger per il codice che desideri eseguire. Ad esempio, se si verifica un problema in GitHub, esegui questo codice JavaScript. Se la tua applicazione contiene più di uno di questi eventi o azioni, valutala possibilità di suddividerla in funzioni differenti.

2. Utilizza i servizi {{site.data.keyword.cloud_notm}} invece dei framework.

    Invece di utilizzare i framework per rendere le funzionalità disponibili nella tua funzione al runtime, puoi utilizzare i servizi {{site.data.keyword.cloud}}. Molte attività comuni il cui completamento viene facilitato dai framework sono disponibili come servizi su {{site.data.keyword.cloud}}.
    {: shortdesc}

    Ad esempio, invece di utilizzare un framework per l'autenticazione, prova {{site.data.keyword.appid_full}}. Se hai bisogno di un'archiviazione file esterna, prova {{site.data.keyword.cos_full}}.

    Se la funzionalità che desideri incorporare non è disponibile come servizio in {{site.data.keyword.cloud}}, puoi sempre integrarla anche con feed e API, senza utilizzare framework.

3. [Verifica che il tuo codice rispetti i limiti di sistema.](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)

    Per essere eseguito, il tuo codice deve rientrare nei limiti di sistema. Ad esempio, se il pacchetto del codice ha una dimensione maggiore di 48 MB, potresti dover continuare a suddividerlo in parti più piccole o inserirlo in un pacchetto sotto forma di immagine Docker.

    Le applicazioni che contengono molti moduli di terze parti, librerie native o strumenti esterni potrebbero incorrere in questo limite. Se crei un'azione di pacchetto .zip o .jar più grande di 48 MB, devi estendere l'immagine di runtime con le dipendenze. Quindi, utilizza un singolo file di origine o un archivio più piccolo di 48 MB. Ad esempio, creando un runtime Docker personalizzato, che include le librerie condivise necessarie, la presenza delle dipendenze non è richiesta nel file di archivio. I file di origine privati possono ancora essere raggruppati nell'archivio e inseriti in fase di runtime.

4. Determina i parametri da inserire nel tuo codice.

    Nelle azioni senza server, i dati vengono forniti aggiungendo parametri alle azioni. I parametri vengono dichiarati come un argomento alla funzione senza server principale. Più comunemente, tali parametri corrispondono alle credenziali per un servizio, ma potrebbero essere qualsiasi cosa renda il tuo codice riutilizzabile con differenti trigger.

5. [Verifica i requisiti strutturali del tuo codice da utilizzare in una funzione.](/docs/openwhisk?topic=cloud-functions-prep)

    Se hai già un'applicazione pronta o prevedi di sviluppare uno script da utilizzare, il tuo codice potrebbe necessitare di qualche modifica per renderlo utilizzabile da {{site.data.keyword.openwhisk}}. Il codice stesso deve soddisfare alcuni requisiti strutturali, come i parametri di input e i risultati di output. Potrebbe anche dover essere impacchettato in un singolo file che include tutte le sue dipendenze.







## Determina l'origine del tuo evento
{: #plan_source}

Pensa alla tua funzione in termini di evento che vuoi che avvenga per attivare l'esecuzione del codice. Potresti volere che il codice venga eseguito per ogni commit con un repository GitHub o che venga eseguito ogni volta che viene effettuato un aggiornamento in un database Cloudant.
{: shortdesc}

Una volta scelto un evento per attivare la tua funzione, riesamina i pacchetti già pronti all'uso. Potresti riuscire a utilizzarne uno solo per semplificare lo sviluppo della tua funzione. In caso contrario, puoi creare il tuo proprio pacchetto per la tua origine evento.

Potresti dover riprendere il codice e fare alcune revisioni in base alle selezioni del tuo evento.


## Organizza le distribuzioni tra ambienti
{: #plan_environments}

Decidi come vuoi distribuire le tue funzioni tra ambienti quali gli ambienti di sviluppo, preparazione e produzione.
{: shortdesc}

Poiché {{site.data.keyword.openwhisk_short}} è un servizio basato su Cloud Foundry, puoi gestire la distribuzione delle tue funzioni nelle organizzazioni e negli spazi Cloud Foundry forniti in {{site.data.keyword.cloud_notm}}. Per organizzare le tue funzioni tra questi ambienti, puoi scegliere di creare un'organizzazione per ogni funzione. Quindi, crea uno spazio per ciascun ambiente di cui hai bisogno. Oppure potresti avere un'organizzazione per ciascun ambiente e creare uno spazio per ogni funzione. Indipendentemente da come organizzi le tue organizzazioni e i tuoi spazi, scegli una struttura che puoi utilizzare per gestire efficacemente le entità della tua funzione.

Puoi anche utilizzare gli [spazi dei nomi](/docs/openwhisk?topic=cloud-functions-namespaces) per isolare le risorse. Ciascuno spazio {{site.data.keyword.cloud_notm}} contiene per impostazione predefinita uno spazio dei nomi Open Whisk. Puoi raggruppare le entità, quali azioni o trigger, in uno spazio dei nomi e, successivamente, creare politiche IAM (Identity and Access) per gestire le autorizzazioni utente per quel gruppo.




