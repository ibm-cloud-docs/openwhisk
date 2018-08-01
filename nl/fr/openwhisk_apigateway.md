---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Création d'API REST sans serveur
{: #openwhisk_apigateway}

Les actions {{site.data.keyword.openwhisk}} peuvent tirer parti d'une gestion directe par les API en introduisant la passerelle d'API qui fait office de proxy vers les [actions Web](./openwhisk_webactions.html) et leur fournit des fonctions supplémentaires. Ces fonctions incluent notamment le routage de méthode HTTP, les ID/secrets client, les limites de débit, CORS, l'affichage de l'utilisation de l'API, l'affichage des journaux de réponses et la définition des règles de partage. Pour plus d'informations sur la fonction de gestion d'API, voir la [documentation de gestion d'API](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis)
{: shortdesc}

## Création d'API à partir d'actions Web OpenWhisk à l'aide de votre navigateur
{: #create_api_browser}

Vous pouvez utiliser l'[**onglet API**](https://console.bluemix.net/openwhisk/apimanagement) dans le [tableau de bord {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/) pour effectuer les tâches suivantes :

* [Créer une API Cloud Functions](https://console.bluemix.net/openwhisk/apimanagement) - Créez une API qui encapsule un ensemble d'actions OpenWhisk.
* [Sécuriser votre API](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - Appliquez des règles de sécurité et de limite de débit pour l'API afin de protéger votre API.
* [Gérer le trafic](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - Affichez les statistiques d'utilisation de l'API et consultez les journaux de réponses.
* [Diffuser & partager sur les réseaux](https://console.bluemix.net/docs/apis/management/manage_apis.html#share_api) - Partagez votre API avec des développeurs dans {{site.data.keyword.Bluemix_notm}} et à l'extérieur.

## Création d'API à partir d'actions Web OpenWhisk à l'aide du plug-in d'interface de ligne de commande
{: #create_api_cli}

Les étapes figurant à la section suivante vous guideront tout au long des tâches de gestion d'API à l'aide du plug-in d'interface de ligne de commande (CLI) {{site.data.keyword.openwhisk_short}}. Pour créer et gérer des API via l'interface CLI, vous devez d'abord installer le [plug-in d'interface de ligne de commande (CLI) {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/docs/openwhisk/bluemix_cli.html) pour {{site.data.keyword.Bluemix_notm}}.

Pour des raisons pratiques, les étapes sont décomposées en sous-rubriques plus petites auxquelles vous pouvez rapidement accéder en utilisant la liste de tâches d'API suivante :

* [Création de la première API](openwhisk_apigateway.html#create_cli_api)
* [Contrôle complet sur la réponse HTTP](openwhisk_apigateway.html#full_control)
* [Exposition de plusieurs actions Web](openwhisk_apigateway.html#multiple_web_actions)
* [Exportation de la configuration](openwhisk_apigateway.html#export_config)
* [Importation de la configuration](openwhisk_apigateway.html#import_config)
* [Modification de la configuration](openwhisk_apigateway.html#modify_config)

### Création de votre première API à l'aide de l'interface de ligne de commande
{: #create_cli_api}

1. Créez un fichier JavaScript nommé **hello.js** intégrant le contenu suivant :
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. Créez une action Web nommée **hello** à l'aide du fichier `hello.js` que vous avez créé à l'étape 1. **Remarque :** Veillez à ajouter l'indicateur `--web true`.
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

  Une nouvelle URL est générée pour exposer l'action `hello` en utilisant une méthode HTTP __GET__.

4. Et enfin, envoyez une demande HTTP à l'URL en exécutant la commande **curl** :
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

L'action Web **hello** est appelée, ce qui entraîne le renvoi d'un objet JSON incluant le paramètre **name** envoyé via le paramètre de requête. Vous pouvez transmettre des paramètres à l'action avec de simples paramètres de requêtes ou en utilisant le corps de la demande. Les actions Web peuvent appeler une action de manière publique sans utiliser la clé d'API d'autorisation OpenWhisk.

### Contrôle complet sur la réponse HTTP
{: #full_control}

L'indicateur `--response-type` contrôle l'URL cible de l'action Web qui sera envoyée par proxy par la passerelle d'API. L'utilisation de `--response-type json` renvoie le résultat complet de l'action au format JSON et affecte automatiquement `application/json` à l'en-tête Content-Type. 

Vous souhaitez avoir un contrôle total sur les propriétés de réponse HTTP, telles que `statusCode` et `headers`, de sorte à pouvoir renvoyer différents types de contenu dans le corps (`body`). Pour ce faire, utilisez `--response-type http` afin de configurer l'URL cible de l'action Web avec l'extension `http`.

Vous pouvez choisir de modifier le code de l'action pour qu'il soit conforme au retour d'actions Web portant l'extension `http`, ou inclure l'action dans une séquence et transmettre son résultat à une nouvelle action. Cette dernière peut ensuite convertir le résultat au format approprié pour une réponse HTTP. Pour en savoir plus sur les types de réponse et les extensions des actions Web, voir la documentation sur les [actions Web](./openwhisk_webactions.html).

1. Modifiez le code du fichier `hello.js` renvoyant les propriétés JSON `body`, `statusCode` et `headers` :
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

2. Mettez à jour l'action avec le résultat modifié :
  ```
  ibmcloud fn action update hello hello.js --web true
  ```
  {: pre}

3. Mettez à jour le type de réponse d'API en utilisant l'indicateur `--response-type http` :
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

4. Appelez l'API mise à jour en exécutant la commande **curl** suivante :
  ```bash
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

Maintenant que vous contrôlez entièrement vos API, vous pouvez contrôler le contenu. Par exemple, le contenu HTML renvoyé, ou définir le code de statut pour des erreurs telles que Introuvable (404), Non autorisé (401) ou Erreur interne (500).

### Exposition de plusieurs actions Web
{: #multiple_web_actions}

Par exemple, si vous souhaitez exposer un ensemble d'actions pour un groupe de lecture, vous pouvez utiliser une série d'actions afin d'implémenter l'application de back end de ce groupe de lecture.

| Action | méthode HTTP | Description |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Extraire les détails d'un livre  |
| postBooks   | POST | Ajouter un livre |
| putBooks    | PUT | Mettre à jour les détails d'un livre |
| deleteBooks | DELETE | Supprimer un livre |

Dans cet exemple, l'API est définie avec un **paramètre de chemin**. Lorsque vous utilisez des paramètres de chemin, l'API doit être définie avec le type de réponse `http`. La valeur du chemin, qui commence par le chemin de base et comprend une ou plusieurs valeurs réelles de paramètre de chemin, est disponible dans la zone `__ow_path` du paramètre JSON de l'action. Consultez la documentation sur le [contexte HTTP des actions Web](./openwhisk_webactions.html#http-context) pour plus d'informations, notamment pour en savoir plus sur d'autres zones de contexte HTTP disponibles pour les actions Web appelées avec un type de réponse `http`.

1. Créez une API pour le groupe de lecture, nommée **Book Club**, avec `/club` comme chemin de base de l'URL HTTP, `books` comme ressource associée et `{isbn}` comme paramètre de chemin utilisé pour identifier un livre particulier à l'aide de son numéro ISBN (International Standard Book Number).
  ```bash
  ibmcloud fn api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud fn api create /club /books get getBooks                       --response-type http
  ibmcloud fn api create /club /books post postBooks                     --response-type http
  ibmcloud fn api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud fn api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: codeblock}

  Notez que la première action exposée avec le chemin de base `/club` extrait le libellé d'API nommé **Book Club**. Toute autre action exposée sous `/club` est désormais associée à **Book Club**.

2. Affichez la liste de toutes les actions **Book Club** qui sont exposées en utilisant la commande suivante :
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

3. Pour le plaisir, vous pouvez ajouter un livre intitulé **JavaScript: The Good Parts**, avec la méthode HTTP __POST__ :
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

4. Obtenez une liste d'ouvrages à l'aide de l'action **getBooks** avec la méthode HTTP __GET__ :
  ```bash
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

5. Vous pouvez supprimer un livre spécifique en utilisant l'action **deleteBooks** avec la méthode HTTP __DELETE__. Dans cet exemple, la valeur de la zone `__ow_path` de l'action **deleteBooks** est `/club/books/978-0596517748`, où `978-0596517748` est la valeur réelle `{isbn}` du chemin.
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

### Exportation de la configuration
{: #export_config}

1. Exportez l'API nommée **Book Club** dans un fichier que vous pouvez utiliser comme base pour recréer les API en utilisant un fichier en entrée.
  ```
  ibmcloud fn api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. Testez le fichier swagger en commençant par supprimer toutes les URL exposées sous un chemin de base commun en exécutant la commande suivante :
  ```
  ibmcloud fn api delete /club
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: deleted API /club
  ```
  {: screen}

  Vous pouvez supprimer toutes les URL exposées en utilisant le chemin de base `/club` ou le libellé du nom de l'API **"Book Club"**:
  {: tip}

### Importation de la configuration
{: #import_config}

1. A présent, restaurez l'API nommée **Book Club** en utilisant le nom de fichier `club-swagger.json` :
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

2. Vérifiez que l'API **Book Club** a bien été recréée :
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

### Modifiez la configuration en utilisant l'interface utilisateur
{: #modify_config}

Vous pouvez modifier la configuration dans le tableau de bord {{site.data.keyword.openwhisk_short}} et cliquer sur l'[onglet API](https://console.ng.bluemix.net/openwhisk/apimanagement) pour configurer la sécurité, les limites de débit et d'autres fonctions. Lorsque vous avez terminé la mise à jour de la configuration, vous pouvez télécharger le fichier de définition au format JSON, puis l'importer à nouveau à l'aide de l'interface de ligne de commande. C'est pratique, notamment dans le cadre d'un déploiement automatisé dans un pipeline d'intégration et de livraison continues (CICD). Vous avez également la possibilité de transférer et de réimporter le fichier de définition de l'API à l'aide de l'interface utilisateur.
