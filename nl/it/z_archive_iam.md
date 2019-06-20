---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-10"

keywords: iam, access managment, roles, service roles, policies, access

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
{:download: .download}


# Gestione dell'accesso
{: #iam}

{{site.data.keyword.openwhisk}} supporta Identity and Access Management (IAM). Puoi ora definire delle politiche IAM per le tue risorse come ad esempio gli spazi dei nomi.
{: shortdesc}

</br>

## Associazione dei ruoli IAM a {{site.data.keyword.openwhisk_short}}
{: #user-roles}

In {{site.data.keyword.openwhisk_short}}, gli spazi dei nomi sono considerati una risorsa {{site.data.keyword.Bluemix_notm}} che ti permette di utilizzare i ruoli e le politiche IAM per la gestione dell'accesso. Tutte le politiche che configuri per uno spazio dei nomi vengono applicate anche alle entità {{site.data.keyword.openwhisk_short}}, ad esempio le azioni o i trigger, contenuti nello spazio dei nomi.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} utilizza i ruoli di gestione sia della piattaforma che del servizio. Puoi configurare delle politiche su chi può creare gli spazi dei nomi al livello della piattaforma, mentre utilizzi i ruoli del servizio per gestire l'interazione con gli spazi dei nomi stessi.

Vuoi sapere di più sui concetti chiave di IAM? Consulta [la documentazione IAM](/docs/iam?topic=iam-iamconcepts#iamconcepts).
{: tip}

</br>

### Ruoli di gestione della piattaforma

La seguente tabella illustra nel dettaglio le azioni associate ai ruoli di gestione della piattaforma. I ruoli di gestione della piattaforma consentono agli utenti di eseguire attività sulle risorse del servizio a livello di piattaforma. Ad esempio, assegnare l'accesso utente per il servizio, creare o eliminare gli ID del servizio, creare le istanze e associare le istanze alle applicazioni.
{: shortdesc}

<table>
  <thead>
    <tr>
      <th>Ruolo piattaforma</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Amministratore</td>
      <td>Gli utenti sono in grado di creare gli spazi dei nomi.</td>
    </tr>
  </tbody>
</table>

Devi avere il ruolo di amministratore per la gestione della piattaforma per utilizzare il servizio. Per ulteriori informazioni sui ruoli, consulta [Ruoli di gestione della piattaforma](/docs/iam?topic=iam-userroles).

</br>

### Ruoli specifici del servizio
{: #service_specific_roles}

I ruoli specifici del servizio determinano l'ambito di una politica di accesso all'interno di un servizio specifico. Per {{site.data.keyword.openwhisk_short}}, i ruoli possono fornire a un utente la capacità di utilizzare il servizio, come accedere all'IU o eseguire delle chiamate API.
{: shortdesc}


È importate notare che le autorizzazioni si sviluppano una sull'altra. Ad esempio, tutte le operazioni che il ruolo `writer` può eseguire, possono essere eseguite anche dal ruolo `manager`. Tuttavia, il ruolo `manager` dispone di ulteriori autorizzazioni. Per vedere le autorizzazioni generali per ogni ruolo, consulta [Ruoli di accesso al servizio](/docs/iam?topic=iam-userroles).

Per vedere quale ruolo è necessario per eseguire ogni operazione, consulta la seguente tabella:

<table><caption>Quale ruolo può eseguire le operazioni?</caption>
  <tr>
    <th style="width:150px">Azione</th>
    <th style="width:2500px">Descrizione</th>
    <th style="width:50px">Lettore</th>
    <th style="width:50px">Scrittore</th>
    <th style="width:50px">Gestore</th>
  </tr>
  <tr>
    <td><code>functions.namespaces.update</code></td>
    <td>Aggiorna uno spazio dei nomi.</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.delete</code></td>
    <td>Elimina uno spazio dei nomi.</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.read</code></td>
    <td>Visualizza gli spazi dei nomi disponibili.</td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.create</code></td>
    <td>Crea un'entità, ad esempio un'azione, all'interno di uno spazio dei nomi Functions.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.update</code></td>
    <td>Aggiorna un'entità, ad esempio un pacchetto, all'interno di uno spazio dei nomi Functions.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.delete</code></td>
    <td>Elimina un'entità, ad esempio un trigger, da uno spazio dei nomi Functions.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.read</code></td>
    <td>Visualizza le entità disponibili, ad esempio le regole, all'interno di uno spazio dei nomi.</td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.activate</code></td>
    <td>Attiva un'entità, ad esempio un'azione, all'interno di uno spazio dei nomi.</td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Funzione disponibile" style="width:32px;" /></td>
  </tr>
</table>

Per informazioni sull'assegnazione dei ruoli utente nell'IU, consulta [Gestione dell'accesso IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

</br>


## Configurazione delle politiche di accesso IAM per uno spazio dei nomi
{: #set-iam}

Quando gestisci uno spazio dei nomi o le entità al suo interno, puoi concedere l'accesso ad altri utenti, attraverso i [ruoli specifici del servizio](#service_specific_roles) elencati in precedenza. Durante la creazione dello spazio dei nomi, viene creato un ID servizio che rappresenta lo spazio dei nomi insieme a un ID utente funzionale. Per impostazione predefinita, all'ID utente funzionale viene assegnato il ruolo Lettore. I lettori possono leggere le entità dello spazio dei nomi e richiamare azioni. Il ruolo Lettore viene utilizzato dai trigger per richiamare le azioni. Per controllare il traffico in entrata, potresti voler concedere l'accesso ad altri utenti, ad esempio assegnando il ruolo Lettore per richiamare delle azioni.
{: shortdesc}

Per informazioni su come assegnare, modificare, controllare o eliminare le politiche di accesso alla risorsa, vedi [Gestione dell'accesso IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).
{: tip}




</br>
</br>

## Accesso ad altre risorse da uno spazio dei nomi
{: #namespace-access}

Le azioni in genere richiamano altri servizi e risorse {{site.data.keyword.Bluemix_notm}} che richiedono l'autenticazione appropriata.
Se questi servizi sono abilitati IAM e accettano i token IAM, puoi sfruttare l'ID funzionale dello spazio dei nomi per la comunicazione in uscita.
Come descritto in [Gestione dell'accesso IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser), per ciascuno spazio dei nomi, viene creato un ID servizio che rappresenta lo spazio dei nomi. Puoi concedere l'accesso ad altri servizi e risorse per questo ID di servizio, assegnando i ruoli appropriati attraverso la gestione della politica IAM.

Al runtime, {{site.data.keyword.openwhisk_short}} trasmette una chiave API dell'ID servizio dello spazio dei nomi al codice dell'azione come valore della variabile di ambiente `__OW_IAM_NAMESPACE_API_KEY`. Il codice dell'azione può utilizzare questa chiave API per generare un token IAM. La maggior parte degli SDK {{site.data.keyword.openwhisk_short}} supportati, quali Cloudant, {{site.data.keyword.watson}} e {{site.data.keyword.cos_full_notm}} eseguono l'autenticazione con la chiave API IAM stessa. Per altri servizi o risorse gestiti da IAM che utilizzano un'API REST, puoi eseguire l'autenticazione con il token derivato dalla chiave API IAM. Per ulteriori informazioni, consultare [Crea un token di accesso IAM per un ID utente o di servizio](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i).

Non sei sicuro di come le chiavi API e i token si integrano tra loro? Trova ulteriori informazioni nella [documentazione IAM](/docs/iam?topic=iam-iamapikeysforservices).

