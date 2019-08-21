---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: namespaces, iam, cloud foundry, classic namespaces, functions

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


# Gestion des espaces de nom
{: #namespaces}

Avec {{site.data.keyword.openwhisk}}, vous pouvez créer des espaces de nom gérés par IAM (Identity and Access Management) pour regrouper des entités, telles que des actions ou des déclencheurs. Vous pouvez ensuite créer des règles d'accès IAM pour l'espace de nom.
Pour une présentation d'IAM, voir le blog [{{site.data.keyword.openwhisk_short}} IAM-enablement announcement](https://www.ibm.com/cloud/blog/ibm-cloud-functions-is-now-identity-and-access-management-enabled){: external}.
{: shortdesc}

## Qu'est-ce qu'un espace de nom ?

Les espaces de nom contiennent des entités {{site.data.keyword.openwhisk_short}}, telles que des actions ou des déclencheurs et appartiennent à un groupe de ressources. Vous pouvez permettre à des utilisateurs d'accéder à vos entités en leur accordant l'accès à l'espace de nom.

Le nom qualifié complet d'une entité est `/namespaceName/packageName/entityName`.

### Que se passe-il lorsque je crée un espace de nom ?

Les espaces de nom créés dans {{site.data.keyword.openwhisk_short}} sont identifiés sous forme d'instance de service IAM.
Lors de la création d'un espace de nom, vous pouvez spécifier le [groupe de ressources](/docs/resources?topic=resources-rgs) dans lequel ajouter l'instance de service.

Lorsque vous créez un espace de nom, les composants suivants sont créés :

| Composant | Description |
| --- | --- | 
| Un ID de service | Vous pouvez utiliser l'ID service comme ID fonctionnel lorsque vous passez des appels sortants. Toutes les actions créées dans cet espace de nom peuvent utiliser cet ID de service pour accéder à d'autres ressources. Par défaut, l'utilisateur fonctionnel reçoit le rôle Lecteur. L'accès Lecteur signifie que l'utilisateur peut lire des entités d'espace de nom et appeler des actions. Le rôle Lecteur est utilisé par les déclencheurs pour appeler des actions. Pour contrôler le trafic entrant, vous souhaiterez peut-être accorder des accès à d'autres utilisateurs, par exemple, leur attribuer le rôle Lecteur pour appeler des actions. |
| Une clé d'API | Clé d'API pour l'ID service, peut être utilisée pour générer des jetons IAM. Vous pouvez ensuite utiliser ces jetons pour authentifier l'espace de nom auprès d'autres services {{site.data.keyword.cloud_notm}}. La clé d'API est fournie aux actions en tant que variable d'environnement `__OW_IAM_NAMESPACE_API_KEY`. |

Affichez tous vos ID de service.
```
ibmcloud iam service-ids
```
{: pre}

Affichez les clés d'API associées à un ID de service. 
```
ibmcloud iam service-api-keys <ServiceID-12345678-1234-abcd-1234-123456789abc>
```
{: pre}

</br>

Ne supprimez pas les clés d'API.
{: tip}

### Existe-t-il des limitations pour les espaces de nom ?

La [création d'API avec passerelle d'API](/docs/openwhisk?topic=cloud-functions-apigateway) et le [kit SDK mobile](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk) ne sont pas prises en charge pour les espaces de nom gérés par IAM.  

Les noms de toutes les entités, notamment les actions, les déclencheurs, les règles, les packages et les espaces de nom, forment une séquence de caractères au format suivant : 
* Le premier caractère doit être alphanumérique ou un trait de soulignement.
* Les caractères qui suivent doivent être des caractères alphanumériques, des espaces ou l'un des caractères suivants : `_`, `@`, `.`, `-`.
* Le dernier caractère ne peut pas être un espace.

### Que dois-je faire si je dispose d'un espace de nom Cloud Foundry ?

Vos espaces de nom Cloud Foundry sont toujours opérationnels. Cependant, pour bénéficier des nouvelles fonctions, vous devez créer un espace de nom activé pour IAM. 


## Création d'un espace de nom basé IAM dans l'interface utilisateur
{: #create_iam_ui}

1. Sur la [console {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk){: external}, cliquez sur le menu déroulant de l'espace de nom.

2. Cliquez sur **Créer un espace de nom**.

3. Entrez un nom d'affichage pour l'espace de nom et une brève description, comme les types d'actions ou de packages que vous prévoyez de créer dans cet espace de nom.

4. Choisissez le groupe de ressources dans lequel créer l'espace de nom, ainsi qu'un emplacement où déployer la ressource d'espace de nom.

5. Cliquez sur **Créer**.

6. Pour afficher l'instance de service pour la ressource d'espace de nom, accédez à votre [tableau de bord {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/resources){: external} et recherchez votre nom d'espace de nom dans le panneau **Espaces de nom de Functions**. 

Si nécessaire, vous pouvez mettre à jour le nom ou la description de l'espace de nom sur la page **Paramètres de l'espace de nom** de la console {{site.data.keyword.openwhisk_short}}. 

## Création d'un espace de nom basé IAM avec l'interface CLI
{: #namespaces_create}

Vous pouvez créer un espace de nom géré par IAM dans le cadre d'un groupe de ressources et gérer les règles d'accès pour vos ressources en ciblant le groupe de ressources lors de la création d'un espace de nom. Si d'autres utilisateurs ont besoin d'accéder à votre espace de nom ou si vous voulez accéder à d'autres ressources à partir des actions de
votre espace de nom, n'oubliez pas de définir des règles IAM après la création de votre espace de nom.
{: shortdesc}

1. Ciblez le groupe de ressources où vous voulez créer l'espace de nom. Si vous n'avez pas encore créé de [groupe de ressources](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create), vous pouvez cibler le groupe de ressources `default`.

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Créez un espace de nom activé pour IAM. Facultatif : Incluez une description pour votre espace de nom à l'aide de l'indicateur `-n` ou `--description`. Si votre description fait plusieurs mots, elle doit être placée entre guillemets.

  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description of your namespace">]
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
        <td>Facultatif : Ajoutez une description à l'espace de nom, comme le type d'actions ou de packages que vous prévoyez de créer. Si votre description fait plusieurs mots, elle doit être placée entre guillemets.</td>
      </tr>
      <tr>
        <td><code>--description &lt;description&gt;</code></td>
        <td>Facultatif : Ajoutez une description à l'espace de nom, comme le type d'actions ou de packages que vous prévoyez de créer. Si votre description fait plusieurs mots, elle doit être placée entre guillemets.</td>
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
  ibmcloud fn namespace get <namespace_name_or_id> --properties
  ```
  {: pre}

  Exemple de sortie :

  ```
  Details of namespace: myNamespace
  Description: short description
  Resource Plan Id: functions-base-plan
  Location: jp-tok
  ID: 05bae599-ead6-4ccb-9ca3-94ce8c8b3e43
  ```
  {: screen}

  Vous pouvez également répertorier tous les espaces de nom, notamment les espaces de nom IAM et Cloud Foundry :

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. Avant de pouvoir créer des entités dans l'espace de nom, vous devez définir le contexte de votre interface de ligne de commande sur l'espace de nom en le ciblant. 

  ```
  ibmcloud fn property set --namespace <namespace_name_or_id>
  ```
  {: pre}

Une fois que vous avez défini une propriété, comme la propriété `--namespace`, celle-ci est conservée jusqu'à ce que vous annuliez sa définition manuellement. Si vous voulez basculer entre des espaces de nom IAM ou entre Cloud Foundry et IAM, vous devez annuler la définition de la propriété d'espace de nom et la réinitialiser. Pour plus d'information,s voir [`ibmcloud fn property set`]
{: note}

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
  --header 'authorization: <IAM_token>' \
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
        <td>Votre jeton {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). Pour extraire votre jeton IAM, exécutez <code>ibmcloud iam oauth-tokens</code>.</td>
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

  **Exemple de sortie**

  ```
  {
    "description": "My new namespace for packages X, Y, and Z.",
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
    --header 'authorization: <IAM_token>'
  ```
  {: pre}

  Vous pouvez également répertorier tous les espaces de nom, notamment les espaces de nom IAM et Cloud Foundry :
  ```
  curl --request GET \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces?limit=0&offset=0' \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>'
  ```
  {: pre}

  **Exemple de sortie**

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

### Accès à d'autres ressources depuis un espace de nom
{: #namespace-access}

En principe, les actions appellent d'autres ressources et services {{site.data.keyword.cloud_notm}} qui nécessitent une authentification appropriée. Si ces services sont activés pour IAM et acceptent des jetons IAM, vous pouvez tirer parti de l'ID fonctionnel de l'espace de nom pour établir une communication sortante.
{: shortdesc}

Comme indiqué à la section [Gestion de l'accès IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser), pour chaque espace de nom, un ID de service est créé pour représenter l'espace de nom. Vous pouvez autoriser l'accès à d'autres services et ressources pour cet ID de service en affectant les rôles appropriés à l'aide de la gestion des règles IAM. Pour plus d'informations sur la création d'ID de service pour accéder à d'autres services activés pour IAM, voir [Création et gestion d'ID de service](/docs/iam?topic=iam-serviceids#serviceids).

Lors de l'exécution, {{site.data.keyword.openwhisk_short}} transmet une clé d'API de l'ID de service de l'espace de nom au code d'action en tant que variable d'environnement `__OW_IAM_NAMESPACE_API_KEY`. Le code d'action peut utiliser cette clé d'API pour générer un jeton IAM. La plupart des kits SDK {{site.data.keyword.openwhisk_short}}, tels que Cloudant, {{site.data.keyword.watson}} et {{site.data.keyword.cos_full_notm}} s'authentifient avec la clé d'API IAM elle-même. Pour d'autres services ou ressources gérés par IAM qui utilisent une API REST, vous pouvez vous authentifier avec le jeton dérivé de la clé d'API IAM. Pour plus d'informations, voir la rubrique sur la [création d'un jeton d'accès IAM pour un utilisateur ou un ID de service](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i).

Vous n'êtes pas sûr que les clés d'API et les jetons puissent fonctionner ensemble ? Pour en savoir plus, consultez [la documentation IAM](/docs/iam?topic=iam-iamapikeysforservices).

## Etapes suivantes
{: #namespaces_next}

A présent que vous avez créé un espace de nom, vous pouvez créer des règles d'accès IAM pour le protéger. Pour commencer, consultez [Gestion des accès](/docs/openwhisk?topic=cloud-functions-iam). 

Pour plus d'informations sur la gestion des espaces de nom basés IAM, voir le [{{site.data.keyword.openwhisk_short}}document de référence de l'API REST](/apidocs/functions).








