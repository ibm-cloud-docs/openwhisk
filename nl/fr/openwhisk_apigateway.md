---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Passerelle d'API
{: #openwhisk_apigateway}

Il est possible de gérer les actions OpenWhisk grâce à la gestion d'API.

La passerelle d'API fait office de proxy pour les [actions Web](./openwhisk_webactions.html) et leur fournit des fonctions supplémentaires.
Ces fonctions supplémentaires incluent notamment le routage de méthode HTTP, les ID/secrets client, les limites de débit, CORS, l'affichage de l'utilisation de l'API, l'affichage des journaux de réponses et la définition des règles de partage.
Pour plus d'informations sur la fonction de gestion d'API, voir la [documentation de gestion d'API](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis)
{: shortdesc}

## Création d'API à partir d'actions Web OpenWhisk à l'aide de votre navigateur

La passerelle d'API vous permet d'exposer une action OpenWhisk en tant qu'API. Après avoir défini l'API, vous pouvez appliquer les règles de sécurité et de limite de débit, afficher l'utilisation de l'API ainsi que les journaux de réponses, et définir les règles de partage d'API.
Dans le tableau de bord OpenWhisk, cliquez sur l'onglet [API](https://console.ng.bluemix.net/openwhisk/apimanagement).


## Création d'API à partir d'actions Web OpenWhisk à l'aide de l'interface de ligne de commande

### Configuration de l'interface de ligne de commande d'OpenWhisk

Configurez l'interface de ligne de commande OpenWhisk avec l'hôte d'API.

Deux régions {{site.data.keyword.Bluemix_notm}} requérant leur propre hôte d'API et leur propre clé d'autorisation uniques sont disponibles. 

* Sud des Etats-Unis
  * Hôte d'API : `openwhisk.ng.bluemix.net`

* Royaume-Uni
  * Hôte d'API : `openwhisk.eu-gb.bluemix.net`

Exécutez la commande suivante pour définir l'hôte d'API pour la région Bluemix concernée :

Sud des Etats-Unis :
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

Royaume-Uni :
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

Si vous avez besoin de basculer vers une autre région, vous devez reconfigurer l'interface de ligne de commande avec l'hôte d'API et la clé d'autorisation car celle-ci est spécifique de la région.
{: tip}

Les artefacts, tels que les actions, les règles et les packages sont spécifiques des régions. Par conséquent, si vous utilisez le même artefact dans plusieurs régions, il vous faut le déployer dans chaque région concernée. 

Pour pouvoir utiliser la commande `wsk api`, le fichier de configuration `~/.wskprops` de l'interface de ligne de commande doit contenir le jeton d'accès Bluemix.

Pour obtenir le jeton d'accès, utilisez la commande CLI suivante :
```
wsk bluemix login
```
{: pre}

Pour plus d'informations sur cette commande, exécutez la commande suivante :
```
wsk bluemix login -h
```
{: pre}

Si la commande `wsk bluemix login` échoue en renvoyant l'erreur `BMXLS0202E: vous utilisez un ID utilisateur fédéré. Utilisez un code à utilisation unique pour vous connecter avec l'option --sso`, connectez-vous à l'interface CLI de {{site.data.keyword.Bluemix_notm}} via la commande `bluemix login`, puis lancez la commande `wsk bluemix login --sso`.
{: tip}

### Création de votre première API à l'aide de l'interface de ligne de commande

1. Créez un fichier JavaScript avec le contenu ci-après. Dans cet exemple, le nom de fichier est 'hello.js'.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}
  
2. Créez une action Web à partir de la fonction JavaScript suivante. Dans cet exemple, l'action s'appelle 'hello'. Prenez soin d'ajouter l'indicateur `--web true`. 
  ```
  wsk action create hello hello.js --web true
  ```
  {: pre}

  ```
  ok: created action hello
  ```
  
3. Créez une API avec le chemin de base `/hello`, le chemin `/world` et la méthode `get` avec le type de réponse `json`
  ```
  wsk api create /hello /world get hello --response-type json
  ```

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  Une nouvelle URL est générée pour exposer l'action `hello` via une méthode HTTP __GET__. 
  
4. Enfin, envoyez une demande HTTP à l'URL.
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world?name=OpenWhisk
  ```

  ```json
  {
  "payload": "Hello world OpenWhisk"
  }
  ```

  L'action Web `hello` est appelée, ce qui entraîne le renvoi d'un objet JSON incluant le paramètre `name` envoyé via un paramètre de requête. Vous pouvez transmettre des paramètres à l'action grâce à de simples paramètres de requête, ou via le corps de demande. Les actions Web peuvent appeler une action de manière publique sans la clé d'API d'autorisation OpenWhisk.

  
### Contrôle complet sur la réponse HTTP
  
  L'indicateur `--response-type` contrôle l'URL cible de l'action Web qui sera envoyée par proxy par la passerelle d'API. L'utilisation de `--response-type json` renvoie le résultat complet de l'action au format JSON et affecte automatiquement `application/json` à l'en-tête Content-Type.  
  
  Une fois que vous avez commencé, vous souhaitez contrôler entièrement les propriétés de réponses HTTP, telles que `statusCode` et `headers`, et renvoyer différents types de contenu dans `body`. Pour ce faire, utilisez `--response-type http` afin de configurer l'URL cible de l'action Web avec l'extension `http`.

  Vous pouvez choisir de modifier le code de l'action pour qu'il soit conforme au retour d'actions Web portant l'extension `http`, ou inclure l'action dans une séquence et transmettre son résultat à une nouvelle action. Cette dernière peut ensuite convertir le résultat au format approprié pour une réponse HTTP. Pour en savoir plus sur les types de réponse et les extensions des actions Web, voir la documentation [Actions Web](./openwhisk_webactions.html).

  Modifiez le code pour `hello.js` en renvoyant les propriétés JSON `body`, `statusCode` et `headers`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: new Buffer(JSON.stringify({payload:`Hello world ${name}`})).toString('base64'), 
        statusCode:200, 
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}
  Notez que le corps doit être renvoyé codé en `base64` et non sous forme de chaîne.
  
  Mettez à jour l'action avec le résultat modifié : 
  ```
  wsk action update hello hello.js --web true
  ```
  {: pre}

  Mettez à jour l'API avec `--response-type http` : 
  ```
  wsk api create /hello /world get hello --response-type http
  ```
  {: pre}
  
  Appelez l'API mise à jour : 
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  {: pre}

  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  Maintenant que vous contrôlez entièrement vos API, vous pouvez contrôler le contenu. Par exemple, le contenu HTML renvoyé, ou définir le code de statut pour des erreurs telles que Introuvable (404), Non autorisé (401) ou Erreur interne (500). 

### Exposition de plusieurs actions Web

Par exemple, si vous souhaitez exposer un ensemble d'actions dans le cadre d'un groupe de lecture, vous pouvez utiliser une série d'actions afin d'implémenter le back end du groupe de lecture :


| Action | méthode HTTP | Description |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Extraire les détails d'un livre  |
| postBooks   | POST | Ajouter un livre |
| putBooks    | PUT | Mettre à jour les détails d'un livre |
| deleteBooks | DELETE | Supprimer un livre |

Créez une API pour le groupe de lecture, nommée `Book Club`, avec `/club` comme chemin de base de l'URL HTTP, et `books` comme ressource.
```
wsk api create -n "Book Club" /club /books get getBooks --response-type http
wsk api create /club /books post postBooks              --response-type http
wsk api create /club /books put putBooks                --response-type http
wsk api create /club /books delete deleteBooks          --response-type http
```

Notez que la première action exposée avec le chemin de base `/club` extrait le libellé d'API portant le nom `Book Club`. Toute autre action exposée sous `/club` est associée à `Book Club`. 

Répertoriez toutes les actions qui sont exposées en utilisant la commande suivante :
```
wsk api list -f
```
{: pre}

```
ok: APIs
Action: getBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: get
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: postBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: post
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: putBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: put
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: deleteBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: delete
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

Pour le plaisir, vous pouvez ajouter un livre, intitulé `JavaScript: The Good Parts`, à l'aide d'une méthode HTTP __POST__ :
```
curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": "success"
}
```

Obtenez une liste de livres à l'aide de l'action `getBooks` via la méthode HTTP __GET__
```
curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
}
```

### Exportation de la configuration
Exportez l'API nommée `Book Club` dans un fichier que vous pouvez utiliser comme base pour recréer les API en utilisant un fichier en entrée. 
```
wsk api get "Book Club" > club-swagger.json
```
{: pre}

Testez le fichier swagger en supprimant d'abord toutes les URL exposées sous un chemin de base commun.
Vous pouvez supprimer toutes les URL exposées en utilisant le chemin de base `/club` ou l'intitulé d'API `"Book Club"` :
```
wsk api delete /club
```
```
ok: deleted API /club
```
### Modification de la configuration

Vous pouvez éditer la configuration dans le tableau de bord OpenWhisk, cliquer sur l'onglet [API](https://console.ng.bluemix.net/openwhisk/apimanagement) pour configurer la sécurité, les limites de débit et d'autres fonctions.

### Importation de la configuration

A présent, restaurez l'API nommée `Book Club` à l'aide du fichier `club-swagger.json` :
```
wsk api create --config-file club-swagger.json
```
{: pre}

```
ok: created api /books delete for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books get for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books post for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books put for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

Assurez-vous que l'API est recréée :
```
wsk api list /club
```
{: pre}

```
ok: apis
Action                    Verb         API Name        URL
getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
