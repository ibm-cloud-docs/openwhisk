---
copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: access policies, iam, roles, functions

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



# Impostazione delle politiche di accesso
{: #iam}

## Associazione dei ruoli IAM a {{site.data.keyword.openwhisk_short}}
{: #user-roles}

In {{site.data.keyword.openwhisk_short}}, gli spazi dei nomi sono risorse {{site.data.keyword.cloud_notm}} che puoi utilizzare per lavorare con i ruoli e le politiche IAM per la gestione dell'accesso. Tutte le politiche che configuri per uno spazio dei nomi vengono applicate anche alle entità {{site.data.keyword.openwhisk_short}}, ad esempio le azioni o i trigger, contenuti nello spazio dei nomi.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} utilizza i ruoli di gestione sia della piattaforma che del servizio. Puoi impostare politiche su chi può creare spazi dei nomi a livello di piattaforma e utilizzare i ruoli del servizio per gestire l'interazione con gli spazi dei nomi stessi.

Vuoi sapere di più sui concetti chiave di IAM? Consulta [la documentazione IAM](/docs/iam?topic=iam-iamconcepts#iamconcepts).
{: tip}

</br>

### Ruoli di gestione della piattaforma

La seguente tabella illustra nel dettaglio le azioni associate ai ruoli di gestione della piattaforma. I ruoli di gestione della piattaforma consentono agli utenti di eseguire attività sulle risorse del servizio a livello di piattaforma. Ad esempio, assegnare l'accesso utente per il servizio, creare o eliminare gli ID del servizio, creare le istanze e associare le istanze alle applicazioni.
{: shortdesc}

Per ulteriori informazioni su come assegnare, modificare, riesaminare o eliminare le politiche di accesso alle risorse, vedi [Gestione dell'accesso IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).
{: tip}

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
      <td>Gli utenti possono creare gli spazi dei nomi. È richiesto il ruolo Amministratore per eseguire le operazioni `service id` e `apikey lock` durante la creazione del servizio.</td>
    </tr>
  </tbody>
</table>

Poiché non è necessario eseguire il provisioning del servizio, il ruolo di editor è l'unico ruolo della piattaforma che ti serve per lavorare con il servizio. Per ulteriori informazioni sugli altri ruoli, consulta [Ruoli di gestione della piattaforma](/docs/iam?topic=iam-userroles).

</br>

### Ruoli specifici per il servizio
{: #service_specific_roles}

I ruoli specifici per il servizio determinano l'ambito di una politica di accesso all'interno di un servizio specifico. Per {{site.data.keyword.openwhisk_short}}, i ruoli possono fornire a un utente la capacità di utilizzare il servizio, come accedere all'IU o eseguire delle chiamate API.
{: shortdesc}

Le autorizzazioni si basano l'una sull'altra. Ad esempio, tutte le operazioni che il ruolo `writer` può eseguire, possono essere eseguite anche dal ruolo `manager`. Tuttavia, il ruolo `manager` dispone di ulteriori autorizzazioni. Per vedere le autorizzazioni generali per ogni ruolo, consulta [Ruoli di accesso al servizio](/docs/iam?topic=iam-userroles).

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

</br>

### Impostazione delle politiche tramite la CLI
{: #cli-set}

Per dare a una risorsa, ad esempio a un'azione, in uno spazio dei nomi basato su IAM l'accesso a un servizio basato su IAM, puoi creare una politica di accesso IAM per lo spazio dei nomi in cui si trova la risorsa.

```
ibmcloud iam service-policy-create <namespace_service_ID> --roles <IAM_role1,IAM_role2> --service-name <other_service_type> --service-instance <other_service_GUID>
```
{: pre}

<table>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del comando <code>ibmcloud iam service-policy-create</code></th>
  </thead>
  <tbody>
    <tr>
      <td><code>&lt;namespace_service_ID&gt;</code></td>
      <td>L'ID servizio per lo spazio dei nomi. Per visualizzare tutti gli ID servizio, esegui <code>ibmcloud iam service-ids</code>.</td>
    </tr>
    <tr>
      <td>`--roles` <code>&lt;IAM_role&gt;</code></td>
      <td>Il tipo di ruolo di accesso al servizio IAM che l'azione deve avere per utilizzare il servizio di destinazione. Per visualizzare i ruoli supportati per l'altro servizio, esegui <code>ibmcloud iam roles --service SERVICE_NAME</code>. Per ulteriori informazioni, vedi [Ruoli di accesso IAM](/docs/iam?topic=iam-userroles#service-access-roles).</td>
    </tr>
    <tr>
      <td>`--service-name` <code>&lt;other_service_type&gt;</code></td>
      <td>Il nome dell'altro tipo di servizio {{site.data.keyword.cloud_notm}}.</td>
    </tr>
    <tr>
      <td>`--service-instance` <code>&lt;other_service_GUID&gt;</code></td>
      <td>Il GUID dell'altra istanza del servizio a cui vuoi che l'azione abbia accesso. Per ottenere il GUID dell'istanza del servizio, esegui <code>ibmcloud resource service-instance &lt;other_service_instance_name&gt;</code>.</td>
    </tr>
  </tbody>
</table>

</br>

**Passi successivi**
Per ulteriori informazioni sulla gestione delle credenziali del servizio, consulta il blog [Manage service credentials for serverless applications](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external}.



