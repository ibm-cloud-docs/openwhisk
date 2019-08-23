---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: serverless, rest api, gateway, web actions, functions

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


# Création d'API REST sans serveur
{: #apigateway}

Vous pouvez utiliser des API pour gérer directement les [actions Web](/docs/openwhisk?topic=cloud-functions-actions_web) {{site.data.keyword.openwhisk}}.
{: shortdesc}

La création d'API avec la passerelle d'API n'est pas prise en charge pour les espaces de nom basés IAM. Utilisez à la place un espace de nom Cloud Foundry.
{: important}

## Pourquoi utiliser des API REST avec {{site.data.keyword.openwhisk_short}} ?

Vous pouvez utiliser la passerelle d'API comme proxy pour vos actions Web. La passerelle API fournit HTTP le routage de méthode HTTP, un ID et des valeurs confidentielles client, des limites de débit, CORS, l'affichage de l'utilisation de l'API, l'affichage des journaux de réponses et les stratégies de partage d'API.


Pour plus d'informations sur la fonction de gestion d'API, voir la [documentation de gestion d'API](/docs/api-management?topic=api-management-manage_openwhisk_apis)

## Création de la première API
{: #api_create}

Vous devez disposer des droits `SpaceDeveloper` dans votre espace Cloud Foundry pour créer des API REST. Les droits d'accès à l'espace peuvent être visibles en exécutant la commande `ibmcloud account space-roles <org>`.
{: note}

Avant de commencer, installez le [plug-in de l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-cli_install).

1. Sauvegardez le code suivant dans un fichier JavaScript nommé `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello, ${name}!`};
  }
  ```
  {: codeblock}

2. Créez une action Web nommée `hello` à l'aide du fichier que vous avez créé. Veillez à ajouter l'indicateur `--web true`. Remplacez `<filepath>` par le chemin de votre fichier `hello.js`.

  ```
  ibmcloud fn action create hello <filepath>/hello.js --web true
  ```
  {: pre}

  **Exemple de sortie**
  ```
  ok: created action hello
  ```
  {: screen}

3. Créez une API avec le chemin de base `/hello`, le chemin `/world` et la méthode `get` avec le type de réponse `json`
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  **Exemple de sortie**
  Une nouvelle URL est générée pour exposer l'action `hello` en utilisant une méthode HTTP `GET`.

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  
4. Envoyez une demande HTTP à l'URL en exécutant la commande cURL suivante.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=Jane
  ```
  {: pre}

  **Exemple de sortie**
  L'action Web `hello` est appelée, ce qui entraîne le renvoi d'un objet JSON incluant le paramètre `name` dans le paramètre de requête. Vous pouvez transmettre des paramètres à l'action avec de simples paramètres de requêtes ou en utilisant le corps de la demande. Les actions Web peuvent publiquement appeler une action sans utiliser l'authentification.

  ```
  {
  "payload": "Hello, Jane!"
  }
  ```
  {: screen}



## Utilisation du contrôle complet sur la réponse HTTP
{: #api_control}

L'indicateur `--response-type` contrôle l'URL cible de l'action Web qui sera envoyée par proxy par la passerelle d'API. Par exemple, lorsque vous utilisez l'indicateur `--response-type json`, le résultat complet de l'action est retourné au format JSON et l'en-tête `Content-Type` est automatiquement défini sur `application/json`.

Pour renvoyer différents types de contenu dans le corps, utilisez le contrôle complet sur les propriétés de réponse HTTP, par exemple `codestatut` et `headers`. Vous pouvez utiliser l'indicateur `--response-type http` pour configurer l'URL cible de l'action Web avec l'extension `http`. Vous pouvez modifier le code de l'action pour qu'il soit conforme au retour d'actions Web portant l'extension `http`, ou inclure l'action dans une séquence et transmettre son résultat à une nouvelle action. Cette dernière peut ensuite convertir le résultat au format approprié pour une réponse HTTP. Pour en savoir plus sur les types de réponse et les extensions des actions Web, voir la documentation sur les [actions Web](/docs/openwhisk?topic=cloud-functions-actions_web).

1. Sauvegardez le code suivant en tant que `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello, ${name}!`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. Mettez à jour votre action Web `hello` avec la nouvelle version de votre code `hello.js`.
  ```
  ibmcloud fn action update hello <filepath>/hello.js --web true
  ```
  {: pre}

  **Sortie**
  ```
  ok: updated action hello
  ```
  {: screen}

3. Mettez à jour le type de réponse d'API en utilisant l'indicateur `--response-type http`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

  **Sortie**
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

4. Appelez l'API mise à jour en exécutant la commande cURL suivante.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  **Exemple de sortie**
  ```
  {
  "payload": "Hello, Serverless API!"
  }
  ```
  {: screen}

## Modification de la configuration
{: #api_modify_config}

Après avoir créé votre configuration, vous pouvez utiliser l'[onglet API](https://cloud.ibm.com/openwhisk/apimanagement){: external} dans le tableau de bord {{site.data.keyword.openwhisk_short}} pour modifier la configuration de plusieurs manières.

* [Création d'une API {{site.data.keyword.openwhisk_short}}](/docs/services/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis) pour encapsuler un ensemble d'actions {{site.data.keyword.openwhisk_short}}.
* [Sécurisation de votre API](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis) par l'application d'une sécurité d'API et de stratégies de limitation de débit.
* [Gestion du trafic](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis) par l'affichage de statistiques d'utilisation des API et la consultation des journaux de réponses.
* [Diffusion sur les réseaux sociaux et partage](/docs/services/api-management?topic=api-management-manage_apis#share_api_manage_apis) de votre API avec des développeurs à la fois à l'intérieur et à l'extérieur d'{{site.data.keyword.cloud_notm}}.

</br>
Une fois la mise à jour de la configuration terminée, vous pouvez télécharger le fichier de définition au format JSON, puis l'importer à nouveau à l'aide de l'interface de ligne de commande. Le téléchargement et l'importation de la configuration est utile, notamment dans le cadre d'un déploiement automatisé dans un pipeline d'intégration et de livraison continues (CICD). Vous pouvez également télécharger et réimporter le fichier de définition d'API via l'interface utilisateur.



