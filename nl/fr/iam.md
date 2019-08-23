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



# Définition des règles d'accès
{: #iam}

## Mappage de rôles IAM vers {{site.data.keyword.openwhisk_short}}
{: #user-roles}

Dans {{site.data.keyword.openwhisk_short}}, les espaces de nom sont des ressources {{site.data.keyword.cloud_notm}} que vous pouvez utiliser pour gérer les rôles et les règles IAM pour la gestion des accès. Toutes les règles que vous définissez pour un espace de nom s'appliquent également aux entités {{site.data.keyword.openwhisk_short}}, telles que des actions ou des déclencheurs, que contient l'espace de nom.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} utilise à la fois les rôles de gestion de service et de plateforme. Vous pouvez définir des règles concernant les personnes autorisées à créer des espaces de nom au niveau de la plateforme et utiliser les rôles de service pour gérer l'interaction avec les espaces de nom eux-mêmes.

Vous souhaitez en savoir plus sur les concepts clés d'IAM ? Consultez la [documentation IAM](/docs/iam?topic=iam-iamconcepts#iamconcepts).
{: tip}

</br>

### Rôles de gestion de plateforme

Le tableau suivant décrit en détail les actions qui sont mappées aux rôles de gestion de plateforme. Les rôles de gestion de plateforme permettent aux utilisateurs d'effectuer des tâches sur des ressources de service au niveau de la plateforme. Par exemple, affecter un accès utilisateur pour le service, créer ou supprimer des ID de service, créer des instances et lier des instances à des applications.
{: shortdesc}

Pour plus d'informations sur l'affectation, l'édition, la consultation ou la suppression de règles d'accès à la ressource, voir [Gestion d'accès IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).
{: tip}

<table>
  <thead>
    <tr>
      <th>Rôle de plateforme</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Administrateur</td>
      <td>Les utilisateurs peuvent créer des espaces de nom. Le rôle Administrateur est requis pour l'exécution des opérations `service id` et `apikey lock` lors de la création du service.</td>
    </tr>
  </tbody>
</table>

Comme le service n'a pas besoin d'être mis à disposition, le rôle éditeur est le seul rôle de plateforme dont vous avez besoin pour travailler avec le service. Pour plus d'informations sur les autres rôles, consultez [Rôles de gestion de plateforme](/docs/iam?topic=iam-userroles).

</br>

### Rôles spécifiques au service
{: #service_specific_roles}

Les rôles spécifiques au service déterminent la portée d'une règle d'accès au sein d'un service spécifique. Pour {{site.data.keyword.openwhisk_short}}, les rôles peuvent s'appliquer à la capacité des utilisateurs à utiliser le service, par exemple accéder à l'interface utilisateur ou exécuter des appels d'API.
{: shortdesc}

Les droits s'appuient les uns sur les autres. Par exemple, toute opération que peut effectuer le rôle `Auteur`, peut également être exécutée par le rôle `Responsable`. Cependant, d'autres droits d'accès sont ajoutés au rôle `Responsable`. Pour afficher les droits d'accès généraux pour chaque rôle, consultez [Rôles d'accès au service](/docs/iam?topic=iam-userroles).

Pour afficher les rôles requis pour réaliser chaque opération, consultez le tableau suivant :

<table><caption>Rôles nécessaires pour les opérations</caption>
  <tr>
    <th style="width:150px">Action</th>
    <th style="width:2500px">Description</th>
    <th style="width:50px">Lecteur</th>
    <th style="width:50px">Auteur</th>
    <th style="width:50px">Responsable</th>
  </tr>
  <tr>
    <td><code>functions.namespaces.update</code></td>
    <td>Met à jour un espace de nom.</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.delete</code></td>
    <td>Supprime un espace de nom.</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.read</code></td>
    <td>Affiche les espaces de nom disponibles.</td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.create</code></td>
    <td>Crée une entité, comme une action, au sein d'un espace de nom Functions.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.update</code></td>
    <td>Met à jour une entité, comme un package, au sein d'un espace de nom Functions.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.delete</code></td>
    <td>Supprime une entité, comme un déclencheur, d'un espace de nom Functions.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.read</code></td>
    <td>Affiche les entités disponibles, comme des règles, au sein d'un espace de nom.</td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.activate</code></td>
    <td>Active une entité, comme une action, au sein d'un espace de nom.</td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
  </tr>
</table>

</br>

### Définition de règles via l'interface de ligne de commande
{: #cli-set}

Pour accorder l'accès à une ressource, telle qu'une action, dans un espace de nom basé IAM à un service basé IAM, vous pouvez créer une règle d'accès IAM pour l'espace de nom dans lequel se trouve la ressource.

```
ibmcloud iam service-policy-create <namespace_service_ID> --roles <IAM_role1,IAM_role2> --service-name <other_service_type> --service-instance <other_service_GUID>
```
{: pre}

<table>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de la commande <code>ibmcloud iam service-policy-create</code></th>
  </thead>
  <tbody>
    <tr>
      <td><code>&lt;namespace_service_ID&gt;</code></td>
      <td>ID de service de l'espace de nom. Pour afficher tous les ID de service, exécutez la commande <code>ibmcloud iam service-ids</code>. </td>
    </tr>
    <tr>
      <td>`--roles` <code>&lt;IAM_role&gt;</code></td>
      <td>Type de rôle d'accès au service IAM que l'action doit posséder pour utiliser le service cible. Pour afficher les rôles pris en charge pour l'autre service, exécutez la commande <code>ibmcloud iam roles --service SERVICE_NAME</code>. Pour plus d'informations, voir [Rôles d'accès IAM](/docs/iam?topic=iam-userroles#service-access-roles).</td>
    </tr>
    <tr>
      <td>`--service-name` <code>&lt;other_service_type&gt;</code></td>
      <td>Nom de l'autre type de service {{site.data.keyword.cloud_notm}}.</td>
    </tr>
    <tr>
      <td>`--service-instance` <code>&lt;other_service_GUID&gt;</code></td>
      <td>Identificateur global unique de l'autre instance de service à laquelle vous souhaitez que l'action ait accès. Pour obtenir l'identificateur global unique de l'instance de service, exécutez la commande <code>ibmcloud resource service-instance &lt;other_service_instance_name&gt;</code>.</td>
    </tr>
  </tbody>
</table>

</br>

**Etapes suivantes**
Pour plus d'informations sur la gestion des données d'identification du service, voir le blog [Manage service credentials for serverless applications](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external}.



