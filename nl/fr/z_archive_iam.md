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


# Gestion des accès
{: #iam}

{{site.data.keyword.openwhisk}} prend en charge Identity and Access Management (IAM). Vous pouvez maintenant définir des règles IAM pour vos ressources comme des espaces de nom.
{: shortdesc}

</br>

## Mappage de rôles IAM vers {{site.data.keyword.openwhisk_short}}
{: #user-roles}

Dans {{site.data.keyword.openwhisk_short}}, les espaces de nom sont considérés comme une ressource {{site.data.keyword.Bluemix_notm}}, ce qui vous permet d'utiliser des règles et des rôles IAM pour la  gestion des accès. Toutes les règles que vous définissez pour un espace de nom s'appliquent également aux entités {{site.data.keyword.openwhisk_short}}, telles que des actions ou des déclencheurs, que contient l'espace de nom.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} utilise à la fois les rôles de gestion de service et de plateforme. Vous pouvez définir des règles concernant les personnes autorisées à créer des espaces de nom au niveau de la plateforme, tout en utilisant les rôles de service pour gérer l'interaction avec les espaces de nom eux-mêmes.

Vous souhaitez en savoir plus sur les concepts clés d'IAM ? Consultez la [documentation IAM](/docs/iam?topic=iam-iamconcepts#iamconcepts).
{: tip}

</br>

### Rôles de gestion de plateforme

Le tableau suivant décrit en détail les actions qui sont mappées aux rôles de gestion de plateforme. Les rôles de gestion de plateforme permettent aux utilisateurs d'effectuer des tâches sur des ressources de service au niveau de la plateforme. Par exemple, affecter un accès utilisateur pour le service, créer ou supprimer des ID de service, créer des instances et lier des instances à des applications.
{: shortdesc}

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
      <td>Les utilisateurs peuvent créer des espaces de nom.</td>
    </tr>
  </tbody>
</table>

Vous devez disposer du rôle administrateur pour la gestion de plateforme pour utiliser le service. Pour plus d'informations sur les rôles, consultez [Rôles de gestion de plateforme](/docs/iam?topic=iam-userroles).

</br>

### Rôles propres à un service
{: #service_specific_roles}

Les rôles propres à un service déterminent la portée d'une règle d'accès au sein d'un service spécifique. Pour {{site.data.keyword.openwhisk_short}}, les rôles peuvent s'appliquer à la capacité des utilisateurs à utiliser le service, par exemple accéder à l'interface utilisateur ou exécuter des appels d'API.
{: shortdesc}


Il est important de noter que les droits d'accès s'appuient les uns sur les autres. Par exemple, toute opération que peut effectuer le rôle `Auteur`, peut également être exécutée par le rôle `Responsable`. Cependant, d'autres droits d'accès sont ajoutés au rôle `Responsable`. Pour afficher les droits d'accès généraux pour chaque rôle, consultez [Rôles d'accès au service](/docs/iam?topic=iam-userroles).

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

Pour plus d'informations sur l'affectation de rôles utilisateur dans l'interface utilisateur, voir [Gestion de l'accès IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

</br>


## Définition de règles d'accès IAM pour un espace de nom
{: #set-iam}

Lors de la gestion d'un espace d'un nom ou des entités qu'il renferme, vous pouvez accorder l'accès à d'autres utilisateurs en utilisant les [rôles spécifiques à un service](#service_specific_roles) répertoriés ci-dessus. Lors de la création de l'espace de nom, un ID de service représentant l'espace de nom est créé, ainsi qu'un ID utilisateur fonctionnel. Par défaut, le rôle Lecteur est affecté à l'ID utilisateur fonctionnel. Les lecteurs peuvent lire des entités d'espace de nom et appeler les actions. Le rôle Lecteur est utilisé par les déclencheurs pour appeler des actions. Pour contrôler le trafic entrant, vous souhaiterez peut-être accorder des accès à d'autres utilisateurs, par exemple, leur attribuer le rôle Lecteur pour appeler des actions.
{: shortdesc}

Pour plus d'informations sur l'affectation, l'édition, la consultation ou la suppression de règles d'accès à la ressource, voir [Gestion d'accès IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).
{: tip}




</br>
</br>

## Accès à d'autres ressources depuis un espace de nom
{: #namespace-access}

En principe, les actions appellent d'autres ressources et services {{site.data.keyword.Bluemix_notm}} qui nécessitent l'authentification appropriée.
Si ces services sont activés avec IAM et acceptent des jetons IAM, vous pouvez tirer parti de l'ID fonctionnel de l'espace de nom pour établir une communication sortante.
Comme indiqué à la section [Gestion de l'accès IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser), pour chaque espace de nom, un ID de service est créé pour représenter l'espace de nom. Vous pouvez autoriser l'accès à d'autres services et ressources pour cet ID de service en affectant les rôles appropriés à l'aide de la gestion des règles IAM.

Lors de l'exécution, {{site.data.keyword.openwhisk_short}} transmet une clé d'API de l'ID de service de l'espace de nom au code d'action en tant que valeur de la variable d'environnement `__OW_IAM_NAMESPACE_API_KEY`. Le code d'action peut utiliser cette clé d'API pour générer un jeton IAM. La plupart des kits SDK {{site.data.keyword.openwhisk_short}}, tels que Cloudant, {{site.data.keyword.watson}} et {{site.data.keyword.cos_full_notm}} s'authentifient avec la clé d'API IAM elle-même. Pour d'autres services ou ressources gérés par IAM qui utilisent une API REST, vous pouvez vous authentifier avec le jeton dérivé de la clé d'API IAM. Pour plus d'informations, voir la rubrique sur la [création d'un jeton d'accès IAM pour un utilisateur ou un ID de service](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i).

Vous n'êtes pas sûr que les clés d'API et les jetons puissent fonctionner ensemble ? Pour en savoir plus, consultez [la documentation IAM](/docs/iam?topic=iam-iamapikeysforservices).

