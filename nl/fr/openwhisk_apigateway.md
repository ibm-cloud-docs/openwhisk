---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-04"

keywords: serverless, rest api, gateway, web actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Création d'API REST sans serveur
{: #openwhisk_apigateway}

Utilisez des API pour gérer directement les actions {{site.data.keyword.openwhisk}}. La passerelle API fait office de proxy pour les [actions Web](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions) et fournit HTTP le routage de méthode HTTP, un ID et des valeurs confidentielles client, des limites de débit, CORS, l'affichage de l'utilisation de l'API, l'affichage des journaux de réponses et les stratégies de partage d'API.
{: shortdesc}

Pour plus d'informations sur la fonction de gestion d'API, voir la [documentation de gestion d'API](/docs/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis)



## Création de la première API
{: #create_cli_api}

Avant de commencer, installez le plug-in d'interface de ligne de commande [{{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli).

1. Sauvegardez le code suivant dans un fichier JavaScript nommé `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. Créez une action Web nommée `hello` à l'aide du fichier que vous avez créé. **Remarque :** Veillez à ajouter l'indicateur `--web true`.
  ```
  ibmcloud fn action create hello hello.js --web true
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: created action hello
  ```
  {: screen}

3. Créez une API avec le chemin de base `/hello`, le chemin `/world` et la méthode `get` avec le type de réponse `json`
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  Une nouvelle URL est générée pour exposer l'action `hello` en utilisant une méthode HTTP GET.

4. Envoyez une demande HTTP à l'URL en exécutant la commande cURL.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

L'action Web `hello` est appelée, ce qui entraîne le renvoi d'un objet JSON incluant le paramètre **name** dans le paramètre de requête. Vous pouvez transmettre des paramètres à l'action avec de simples paramètres de requêtes ou en utilisant le corps de la demande. Les actions Web peuvent publiquement appeler une action sans utiliser l'authentification. 

## Utilisation du contrôle complet sur la réponse HTTP
{: #full_control}

L'indicateur `--response-type` contrôle l'URL cible de l'action Web qui sera envoyée par proxy par la passerelle d'API. Par exemple, lorsque vous utilisez l'indicateur `--response-type json`, le résultat complet de l'action est retourné au format JSON et l'en-tête **Content-Type** est automatiquement défini sur `application/json`.

Pour renvoyer différents types de contenu dans le corps, utilisez le contrôle complet sur les propriétés de réponse HTTP, par exemple **codestatut** et **headers**. Vous pouvez utiliser l'indicateur `--response-type http` pour configurer l'URL cible de l'action Web avec l'extension `http`. Vous pouvez modifier le code de l'action pour qu'il soit conforme au retour d'actions Web portant l'extension `http`, ou inclure l'action dans une séquence et transmettre son résultat à une nouvelle action. Cette dernière peut ensuite convertir le résultat au format approprié pour une réponse HTTP. Pour en savoir plus sur les types de réponse et les extensions des actions Web, voir la documentation sur les [actions Web](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions).

1. Modifiez le code de l'action `hello.js` renvoyant les propriétés JSON `body`, `statusCode` et `headers` .
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello world ${name}`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. Mettez à jour l'action avec le résultat modifié.
  ```
  ibmcloud fn action update hello hello.js --web true
  ```
  {: pre}

3. Mettez à jour le type de réponse d'API en utilisant l'indicateur `--response-type http`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

4. Appelez l'API mise à jour en exécutant la commande cURL suivante.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  {: screen}

## Exposition de plusieurs actions Web
{: #multiple_web_actions}

Vous pouvez exposer plusieurs actions Web pour implémenter l'application de back end de votre application. Par exemple, pour exposer un ensemble d'actions pour un groupe de lecture, vous pouvez utiliser une série d'actions afin d'implémenter l'application de back end de ce groupe de lecture.

| Action | méthode HTTP | Description |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Extraire les détails d'un livre  |
| postBooks   | POST | Ajouter un livre |
| putBooks    | PUT | Mettre à jour les détails d'un livre |
| deleteBooks | DELETE | Supprimer un livre |

Dans cet exemple, l'API est définie avec un paramètre de chemin. Lorsque vous utilisez des paramètres de chemin, l'API doit être définie avec le type de réponse `http`. La valeur du chemin, qui commence par le chemin de base et comprend une ou plusieurs valeurs réelles de paramètre de chemin, est disponible dans la zone `__ow_path` du paramètre JSON de l'action. Pour plus de détails sur les zones de contexte HTTP, consultez la documentation relative au [Contexte HTTP des actions Web](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#http-context).

Vous pouvez tester cet exemple d'actions Web de club de lecture :

1. Créez une API pour le club de lecture nommé `Book Club`, avec `/club` comme chemin de base URL HTTP, `books` comme ressource et `{isbn}` comme paramètre de chemin utilisé pour identifier un livre spécifique par son code ISBN.
  ```
  ibmcloud fn api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud fn api create /club /books get getBooks                       --response-type http
  ibmcloud fn api create /club /books post postBooks                     --response-type http
  ibmcloud fn api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud fn api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: pre}

  La première action exposée avec le chemin de base `/club` est libellée avec le nom `Book Club`. Toute autre action exposée sous `/club` est désormais associée à `Book Club`.

2. Affichez la liste de toutes les actions `Book Club` qui sont exposées.
  ```
  ibmcloud fn api list /club -f
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: APIs
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: postBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: post
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: putBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: put
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: deleteBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: delete
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

3. Ajoutez un livre intitulé `JavaScript: The Good Parts` à l'aide d'un HTTP POST.
  ```
  curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' -H "Content-Type: application/json" https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
    "result": "success"
  }
  ```
  {: screen}

4. Obtenez une liste de livres à l'aide d'un appel HTTP GET à l'action `getBooks`.
  ```
  curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
    "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
  }
  ```
  {: screen}

5. Supprimez un livre spécifique en utilisant un appel HTTP DELETE à l'action `deleteBooks`. Dans cet exemple, la valeur de la zone `__ow_path` de l'action `deleteBooks` est `/club/books/978-0596517748`, où `978-0596517748` est la valeur réelle `{isbn}` du chemin.
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

## Exportation et importation de la configuration
{: #export_import_config}

Pour importer ou exporter une configuration, vous pouvez continuer d'utiliser l'exemple du club de livres.

1. Exportez l'API `Book Club` dans un fichier nommé `club-swagger.json`. Ce fichier peut être utilisé comme base pour recréer les API en utilisant un fichier en entrée.
  ```
  ibmcloud fn api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. Testez le fichier swagger en commençant par supprimer toutes les URL exposées sous un chemin de base commun.
  ```
  ibmcloud fn api delete /club
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: deleted API /club
  ```
  {: screen}

  Vous pouvez supprimer toutes les URL exposées en utilisant le chemin de base `/club` ou le libellé du nom de l'API `"Book Club"`.
  {: tip}

3. Restaurez l'API `Book Club` à l'aide du fichier `club-swagger.json`.
  ```
  ibmcloud fn api create --config-file club-swagger.json
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: created api /club/books/{isbn} get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} put for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} delete for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books post for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: screen}

4. Vérifiez que l'API
  ```
  ibmcloud fn api list /club
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: apis
  Action                    Verb         API Name        URL
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

## Modification de la configuration
{: #modify_config}

Après que vous avez créé votre configuration, vous pouvez utiliser l'[**onglet API**](https://cloud.ibm.com/openwhisk/apimanagement) dans le tableau de bord {{site.data.keyword.openwhisk_short}} pour modifier la configuration de plusieurs manières.

* [Création d'une API {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/docs/services/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis) qui encapsule un ensemble d'actions {{site.data.keyword.openwhisk_short}}.
* [Sécurisation de votre API](https://cloud.ibm.com/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis) par l'application d'une sécurité d'API et de stratégies de limitation de débit.
* [Gestion du trafic](https://cloud.ibm.com/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis) par l'affichage de statistiques d'utilisation des API et la consultation des journaux de réponses.
* [Socialisation et partage](https://cloud.ibm.com/docs/services/api-management?topic=api-management-manage_apis#share_api_manage_apis) de votre API avec des développeurs à la fois à l'intérieur et à l'extérieur de {{site.data.keyword.Bluemix_notm}}.

Lorsque vous avez terminé la mise à jour de la configuration, vous pouvez télécharger le fichier de définition au format JSON, puis l'importer à nouveau à l'aide de l'interface de ligne de commande. Le téléchargement et l'importation de la configuration est utile, notamment dans le cadre d'un déploiement automatisé dans un pipeline d'intégration et de livraison continues (CICD). Vous avez également la possibilité de transférer et de réimporter le fichier de définition de l'API à l'aide de l'interface utilisateur.
