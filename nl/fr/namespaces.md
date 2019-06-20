---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-17"

keywords: namespaces, iam, cloud foundry, classic namespaces

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
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}



# Gestion des espaces de nom
{: #namespaces}

Avec {{site.data.keyword.openwhisk}}, vous pouvez créer des espaces de nom gérés par IAM (Identity and Access Management) pour regrouper des entités, telles que des actions ou des déclencheurs. Vous pouvez ensuite créer des règles d'accès IAM pour l'espace de nom.
{: shortdesc}


**Qu'est-ce qu'un espace de nom ?**

Les espaces de nom contiennent des entités {{site.data.keyword.openwhisk_short}}, telles que des actions ou des déclencheurs et appartiennent à un groupe de ressources. Vous pouvez permettre à des utilisateurs d'accéder à vos entités en leur accordant l'accès à l'espace de nom.

Le nom qualifié complet d'une entité est `/nom_espace_nom/[nom_package]/nom_entité`.


**Que se passe-t-il lorsque je crée un espace de nom ?**

Les espaces de nom créés dans {{site.data.keyword.openwhisk_short}} sont identifiés sous forme d'instance de service IAM.
Lors de la création d'un espace de nom, vous pouvez spécifier le [groupe de ressources](/docs/resources?topic=resources-rgs) dans lequel ajouter l'instance de service.

Lorsque vous créez votre espace de nom, les artefacts suivants sont créés en même temps :

* Un ID de service pouvant être utilisé comme ID fonctionnel lorsque vous effectuez des appels sortants. Toutes les actions créées dans cet espace de nom peuvent utiliser cet ID de service pour accéder à d'autres ressources. Pour afficher tous vos ID de service, exécutez la commande `ibmcloud iam service-ids`. 

* Une clé d'API pour l'ID de service qui peut être utilisée pour générer des jetons IAM. Vous pouvez ensuite utiliser ces jetons pour authentifier l'espace de nom auprès d'autres services {{site.data.keyword.Bluemix_notm}}. La clé d'API est fournie aux actions en tant que variable d'environnement.

    Ne supprimez pas les clés d'API.
    {: tip}

**Existe-t-il des limitations pour les espaces de nom ?**

La [création d'API avec passerelle d'API](/docs/openwhisk?topic=cloud-functions-apigateway) et l'utilisation du [kit SDK mobile](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk) ne sont pas prises en charge pour les espaces de nom gérés par IAM. 

{{site.data.keyword.openwhisk_short}} fait l'objet de restrictions en matière de noms d'espace de nom. Pour plus d'informations, consultez la documentation sur les [limites et détails du système](/docs/openwhisk?topic=cloud-functions-limits#limits_entities_ov).
{: tip}



**Que dois-je faire si je dispose d'un espace de nom Cloud Foundry ?**

Vos espaces de nom Cloud Foundry sont toujours opérationnels. Cependant, pour bénéficier des nouvelles fonctions, vous devez [effectuer la migration de vos espaces de nom vers IAM](/docs/resources?topic=resources-migrate).

</br>


## Création d'un espace de nom avec l'interface de ligne de commande
{: #namespaces_create}

Vous pouvez créer un espace de nom géré par IAM dans le cadre d'un groupe de ressources et gérer les règles d'accès pour vos ressources en ciblant le groupe de ressources lors de la création d'un espace de nom. Si d'autres utilisateurs ont besoin d'accéder à votre espace de nom ou si vous voulez accéder à d'autres ressources à partir des actions de
votre espace de nom, n'oubliez pas de définir des règles IAM après la création de votre espace de nom.
{: shortdesc}

1. Ciblez le groupe de ressources où vous voulez créer l'espace de nom. Si vous n'avez pas encore créé de [groupe de ressources](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create), vous pouvez cibler le groupe de ressources `default`.

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Créez un espace de nom activé pour IAM.

  ```
  ibmcloud fn namespace create <nom_espace_de_nom> [-n <description>]
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de cette commande</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;nom_espace_de_nom&gt;</code></td>
        <td>Nom d'affichage de l'espace de nom IAM.</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>Facultatif : ajoutez une description à l'espace de nom, comme le type d'actions ou de packages qu'il contiendra.</td>
      </tr>
    </tbody>
  </table>

  Exemple de sortie :

  ```
  ok: created namespace myNamespace
  ```
  {: screen}

3. Vérifiez que votre nouvel espace de nom est créé.

  ```
  ibmcloud fn namespace get <nom_ou_ID_espace_de_nom> --properties
  ```
  {: pre}

  Exemple de sortie :

  ```
  Details of namespace: 'myNamespace'
    Description: 'short description'
    Resource Plan Id: 'functions-base-plan'
    Location: 'jp-tok'
    ID: '05bae599-ead6-4ccb-9ca3-94ce8c8b3e43'
  ```
  {: screen}

  Vous pouvez également répertorier tous les espaces de nom, notamment les espaces de nom IAM et Cloud Foundry :

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. Avant de créer des entités dans l' espace de nom, définissez le contexte de votre interface de ligne de commande sur l'espace de nom en le ciblant. 

  ```
  ibmcloud fn property set --namespace <ID_ou_nom_espace_de_nom>
  ```
  {: pre}

</br>

## Création d'un espace de nom avec l'API
{: #namespaces_create_api}

Vous pouvez créer un espace de nom géré par IAM dans le cadre d'un groupe de ressources et gérer les règles d'accès pour vos ressources en ciblant le groupe de ressources lors de la création d'un espace de nom. Si d'autres utilisateurs ont besoin d'accéder à votre espace de nom ou si vous voulez accéder à d'autres ressources à partir des actions de
votre espace de nom, n'oubliez pas de définir des règles IAM après la création de votre espace de nom.
{: shortdesc}


1. Ciblez le groupe de ressources où vous voulez créer l'espace de nom. Si vous n'avez pas encore créé de [groupe de ressources](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create), vous pouvez cibler le groupe de ressources `default`.

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Créez un espace de nom activé pour IAM.

  ```
  curl --request POST \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces \
    --header 'accept: application/json' \
    --header 'authorization: <jeton_IAM>' \
    --data '{"description":"string","name":"string","resource_group_id":"string","resource_plan_id":"string"}'
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de cette commande</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;jeton_IAM&gt;</code></td>
        <td>Votre jeton {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). Pour extraire votre jeton IAM, exécutez <code>ibmcloud iam oauth-tokens</code>.</td>
      </tr>
      <tr>
        <td><code>-n &lt;name&gt;</code></td>
        <td>Nom de l'espace de nom.</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_group_id&gt;</code></td>
        <td>ID du groupe de ressources où vous voulez créer l'espace de nom. Pour afficher les ID de groupe de ressources, exécutez <code>ibmcloud resource groups</code>.</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_plan_id&gt;</code></td>
        <td>ID du plan de ressources, comme functions-base-plan</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>Facultatif : ajoutez une description à l'espace de nom, comme le type d'actions ou de packages qu'il contiendra.</td>
      </tr>
    </tbody>
  </table>

  Exemple de sortie :

  ```
  {
    "description": "Mon nouvel espace de nom pour les packages X, Y et Z.",
      "id": "12345678-1234-abcd-1234-123456789abc",
      "location": "jp-tok",
      "crn": "crn:v1:functions:jp-tok:a/1a22bb3c44dd1a22bb3c44dd1a22:12345678-1234-abcd-1234-123456789abc::",
      "name": "mynamespace",
      "resource_group_id": "1a22bb3c44dd1a22bb3c44dd1a22",
      "resource_plan_id": "functions-base-plan"
    }
  ```
  {: screen}

3. Vérifiez que votre nouvel espace de nom est créé.

  ```
  curl --request GET \
      --url 'https://us-south.functions.cloud.ibm.com/api/servicebroker/api/v1/namespaces/{id} \
      --header 'accept: application/json' \
      --header 'authorization: <jeton_IAM>'
  ```
  {: pre}

  Vous pouvez également répertorier tous les espaces de nom, notamment les espaces de nom IAM et Cloud Foundry :
  ```
  curl --request GET \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces?limit=0&offset=0' \
    --header 'accept: application/json' \
    --header 'authorization: <jeton_IAM>'
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
    "limit": 10,
      "offset": 0,
      "total_Count": 2,
      "namespaces": [
        {
        "id": "12345678-1234-abcd-1234-123456789abc",
          "location": "jp-tok"
      },
      {
        "id": "BobsOrg_dev",
          "classic_type": 1,
          "location": "jp-tok"
        }
    ]
  }
  ```
  {: screen}


Pour plus d'informations sur l'utilisation d'API REST HTTP, consultez la [documentation sur les API {{site.data.keyword.openwhisk_short}}](/apidocs/functions).
{: tip}



## Etapes suivantes
{: #namespaces_next}

Maintenant que vous avez créé un espace de nom, vous pouvez créer des règles d'accès IAM pour le protéger. Pour commencer, consultez [Gestion des accès](/docs/openwhisk?topic=cloud-functions-iam). Pour plus d'informations sur la gestion des espaces de nom IAM, voir le [document de référence de l'API REST {{site.data.keyword.openwhisk_short}}](/apidocs/functions).


