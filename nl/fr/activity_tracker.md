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



# Affichage d'événements {{site.data.keyword.cloudaccesstrailshort}}
{: #activity_tracker}

Vous pouvez visualiser, gérer et auditer des activités initiées par l'utilisateur et effectuées dans votre instance de service {{site.data.keyword.openwhisk}} en utilisant le service {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}


Pour plus d'informations sur le fonctionnement du service, consultez la [documentation {{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started).


## Emplacement de visualisation des événements
{: #view}

Les événements sont disponibles dans le **domaine de compte** {{site.data.keyword.cloudaccesstrailshort}} qui est accessible dans la région {{site.data.keyword.cloud_notm}} où se trouve la ressource d'espace de nom {{site.data.keyword.openwhisk_short}}. Pour plus d'informations, voir [Affichage des événements de compte](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-view_acc_events).

1. Connectez-vous à votre compte {{site.data.keyword.cloud_notm}}. 
2. Depuis le catalogue, mettez à disposition une instance du service {{site.data.keyword.cloudaccesstrailshort}} sur le même compte que votre instance de {{site.data.keyword.openwhisk}}.
3. Dans l'onglet  **Gérer** du tableau de bord {{site.data.keyword.cloudaccesstrailshort}}, cliquez sur **Afficher dans Kibana**.
4. Définissez la période pour laquelle vous voulez consulter les journaux. La valeur par défaut est 15 minutes.
5. Dans la liste **Zones disponibles**, cliquez sur **type**. Cliquez sur l'icône représentant une loupe pour qu'**Activity Tracker** limite les journaux uniquement à ceux dont le suivi est assuré par le service.
6. Vous pouvez utiliser les autres zones disponibles pour affiner la recherche.

Pour que les utilisateurs autres que le propriétaire du compte puissent afficher les journaux, vous devez utiliser le plan Premium. Pour laisser d'autres utilisateurs consulter des événements, voir [Octroi de droits de consultation des événements de compte](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions).
{: tip}


## Liste des événements
{: #events}

Consultez le tableau suivant pour obtenir une liste des événements envoyés à {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

<table>
  <thead>
    <tr>
      <th>Action</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>functions.namespace.create</td>
      <td>Création d'une ressource d'espace de nom IAM</td>
    </tr>
    <tr>
      <td>functions.namespace.migrate</td>
      <td>Migration d'un espace de nom Cloud Foundry pour l'activer pour IAM</td>
    </tr>
    <tr>
      <td>functions.namespace.update</td>
      <td>Mise à jour des propriétés de la ressource d'espace de nom IAM, comme le nom d'affichage ou la description</td>
    </tr>
    <tr>
      <td>functions.namespace.delete</td>
      <td>Suppression d'une ressource d'espace de nom</td>
    </tr>
  </tbody>
</table>



