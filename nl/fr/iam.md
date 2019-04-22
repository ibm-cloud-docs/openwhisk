---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-29"

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

Des règles IAM sont disponibles dans la région Tokyo uniquement pour {{site.data.keyword.openwhisk_short}}. Si vous utilisez la région Tokyo, vous devez vous servir de règles IAM pour contrôler les accès.
{: tip}

</br>

## Mappage de rôles IAM vers {{site.data.keyword.openwhisk_short}}
{: #user-roles}

Dans {{site.data.keyword.openwhisk_short}}, les espaces de nom sont considérés comme une ressource IBM Cloud, ce qui vous permet d'utiliser des règles et des rôles IAM pour la  gestion des accès. Toutes les règles que vous définissez pour un espace de nom s'appliquent également aux entités {{site.data.keyword.openwhisk_short}}, telles que des actions ou des déclencheurs, que contient l'espace de nom.
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
      <td>Les utilisateurs peuvent créer des espaces de nom. </td>
    </tr>
  </tbody>
</table>

Vous devez disposer du rôle administrateur pour la gestion de plateforme pour utiliser le service. Pour plus d'informations sur les rôles, consultez [Rôles de gestion de plateforme](/docs/iam?topic=iam-userroles). 

</br>

### Rôles propres à un service

Les rôles propres à un service déterminent la portée d'une règle d'accès au sein d'un service spécifique. Pour {{site.data.keyword.openwhisk_short}}, les rôles peuvent s'appliquer à la capacité des utilisateurs à utiliser le service, telles que l'accès à l'interface utilisateur ou l'exécution d'appels d'API.
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
    <td>Met à jour un espace de nom. </td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.delete</code></td>
    <td>Supprime un espace de nom. </td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.read</code></td>
    <td>Affiche les espaces de nom disponibles. </td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.create</code></td>
    <td>Crée une entité, comme une action, au sein d'un espace de nom Functions. </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.update</code></td>
    <td>Met à jour une entité, comme un package, au sein d'un espace de nom Functions. </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.delete</code></td>
    <td>Supprime une entité, comme un déclencheur, d'un espace de nom Functions. </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.read</code></td>
    <td>Affiche les entités disponibles, comme des règles, au sein d'un espace de nom. </td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.activate</code></td>
    <td>Active une entité, comme une action, au sein d'un espace de nom. </td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Fonction disponible" style="width:32px;" /></td>
  </tr>
</table>

Pour plus d'informations sur l'affectation de rôles utilisateur dans l'interface utilisateur, voir [Gestion de l'accès IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

</br>


## Définition des règles d'accès IAM
{: #set-iam}

Lorsqu'un service appelle une action, cette dernière a une réponse. La réponse étant envoyée à partir de l'espace de nom ou l'action étant acheminée vers un service, elle est considérée comme une information sortante. Si vous souhaitez limiter l'influence de votre espace de nom sur d'autres services, vous pouvez créer une règle d'accès.
{: shortdesc}

Pour plus d'informations sur l'affectation, l'édition, la consultation ou la suppression de règles d'accès à la ressource, voir [Gestion d'accès IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).
{: tip}




</br>
</br>

## Accès à d'autres ressources depuis un espace de nom
{: #namespace-access}

Vous pouvez accéder à d'autres ressources à partir d'un espace de nom géré par IAM à l'aide d'un jeton IAM. Un jeton représente l'authentification et vérifie l'identité de la ressource. Le jeton IAM est nécessaire pour l'authentification lors de l'accès à des ressources ou services gérés par IAM.
{: shortdesc}

De la même façon qu'un ID utilisateur identifie un utilisateur, un ID de service représente une ressource spécifique. Cela signifie que les règles IAM peuvent être appliquées aux ressources qui gèrent les droits d'accès. Tout comme un utilisateur, une ressource doit s'authentifier pour vérifier son identité. Dans Functions, cette opération peut être optimisée grâce à l'implémentation d'actions lors de l'accès à d'autres services ou ressources. 

Lorsque vous créez un nouvel espace de nom géré par IAM, Functions crée automatiquement un ID de service correspondant, qui identifie l'espace de nom et une clé d'API. Lors de l'exécution, Cloud Functions transmet la clé d'API au code d'action en tant que valeur de la variable d'environnement `__OW_IAM_NAMESPACE_API_KEY`. Le code d'action peut utiliser cette clé d'API pour générer un jeton IAM. La plupart des logiciels SDK pris en charge, tels que Cloudant, Watson et COS, s'authentifient avec la clé IAM elle-même. Les autres ressources ou services gérés par IAM qui utilisent une API REST s'authentifient avec le jeton qui est dérivé de la clé IAM. 

Vous n'êtes pas sûr que les clés d'API et les jetons puissent fonctionner ensemble ? Pour en savoir plus, consultez [la documentation IAM](/docs/iam?topic=iam-iamapikeysforservices). 
