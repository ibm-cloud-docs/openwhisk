---

copyright:
  years: 2018
lastupdated: "2018-10-01"

keywords: events, serverless, push notifications, functions

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



# Visualizzazione degli eventi {{site.data.keyword.cloudaccesstrailshort}}
{: #activity_tracker}

Puoi visualizzare, gestire e controllare le attività avviate dall'utente effettuate nella tua istanza del servizio {{site.data.keyword.openwhisk}} utilizzando il servizio {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}


Per ulteriori informazioni su come funziona il servizio, consulta la [documentazione {{site.data.keyword.cloudaccesstrailshort}} ](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started).


## Dove visualizzare gli eventi
{: #view}

Gli eventi sono disponibili nel **dominio dell'account** {{site.data.keyword.cloudaccesstrailshort}} collocato nella regione {{site.data.keyword.cloud_notm}} in cui si trova la risorsa dello spazio dei nomi {{site.data.keyword.openwhisk_short}}. Per ulteriori informazioni, vedi [Visualizzazione degli eventi dell'account](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-view_acc_events).

1. Accedi al tuo account {{site.data.keyword.cloud_notm}}.
2. Dal catalogo, esegui il provisioning di un'istanza del servizio {{site.data.keyword.cloudaccesstrailshort}} nello stesso account della tua istanza di {{site.data.keyword.openwhisk}}.
3. Sulla scheda **Gestisci** del dashboard {{site.data.keyword.cloudaccesstrailshort}}, fai clic su **Visualizza in Kibana**.
4. Imposta l'intervallo di tempo per il quale desideri visualizzare i log. Il valore predefinito è 15 minuti.
5. Nell'elenco **Campi disponibili**, fai clic su **type**. Fai clic sull'icona della lente di ingrandimento del **Programma di traccia dell'attività** per limitare i log a quelli tracciati dal servizio.
6. Puoi utilizzare gli altri campi disponibili per restringere la tua ricerca.

Per utenti diversi dal proprietario dell'account, per visualizzare i log, devi utilizzare il piano premium. Per consentire ad altri utenti di visualizzare gli eventi, consulta [Concessione di autorizzazioni per visualizzare gli eventi di account](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions).
{: tip}


## Elenco degli eventi
{: #events}

Consulta la seguente tabella per un elenco di eventi inviati a {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

<table>
  <thead>
    <tr>
      <th>Azione</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>functions.namespace.create</td>
      <td>Crea una risorsa dello spazio dei nomi basata su IAM</td>
    </tr>
    <tr>
      <td>functions.namespace.migrate</td>
      <td>Migra uno spazio dei nomi basato su Cloud Foundry per diventare abilitato IAM</td>
    </tr>
    <tr>
      <td>functions.namespace.update</td>
      <td>Aggiorna le proprietà della risorsa dello spazio dei nomi basato su IAM, ad esempio la descrizione o il nome di visualizzazione</td>
    </tr>
    <tr>
      <td>functions.namespace.delete</td>
      <td>Elimina una risorsa dello spazio dei nomi</td>
    </tr>
  </tbody>
</table>



