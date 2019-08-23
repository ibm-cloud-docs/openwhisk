---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: web actions, serverless, functions

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


# Création d'actions Web
{: #actions_web}

Lorsque vous créez une action Web, le résultat est une URL qui peut être utilisée pour déclencher l'action depuis n'importe quelle application Web.
{: shortdesc}

## Pourquoi utiliser des actions Web au lieu d'actions standard ?

### 1. Exécuter des actions Web de manière anonyme

Les activations d'action Web sont associées à l'utilisateur qui a créé l'action plutôt qu'à l'appelant de l'action. Généralement, pour des appels API vers des applications telles que Github, vous incluez un nom d'utilisateur et un jeton avec l'appel API pour un utilisateur spécifique ou un ID fonctionnel. Lorsque vous utilisez une action Web, ces types de données d'identification ne sont pas nécessaires. Une action Web est accessible via une interface REST sans que des données d'identification ne soient requises.

Même si vous n'êtes pas obligé d'utiliser des données d'identification avec des actions Web, vous pouvez implémenter vos propres authentification et autorisation, ou flux OAuth. Pour configurer une action Web avec des données d'identification, voir [Sécurisation des actions Web](#actions_web_secure).

### 2. Utiliser tout type de demande HTTP

Par défaut, les actions acceptent uniquement des demandes `POST`, mais les actions Web peuvent être appelées via n'importe laquelle de ces méthodes HTTP : `GET`, `POST`, `PUT`, `PATCH` et `DELETE`, ainsi que `HEAD` et `OPTIONS`.

### 3. Déclencher une action Web depuis n'importe où

Lorsque vous créez une action Web {{site.data.keyword.openwhisk}}, vous générez une URL pour appeler cette action depuis n'importe quelle application Web. Les actions qui ne sont pas des actions Web nécessitent une authentification et doivent répondre avec un objet JSON. 

Un chemin d'API d'action Web peut être utilisé avec cURL, `wget`, ou même être entré directement dans votre navigateur. Une action Web peut être appelée en utilisant une URL structurée comme suit : `https://<apihost>/api/v1/web/<namespace>/<packageName>/<actionName>.<ext>`.

### 4. Créer moins d'entités {{site.data.keyword.openwhisk_short}}

Comme vous pouvez appeler une action Web depuis n'importe où, vous n'êtes pas obligé de créer d'autres entités {{site.data.keyword.openwhisk_short}} telles que des déclencheurs ou des règles.

## Comment fonctionnent les actions Web ?

Les actions Web peuvent être appelées sans authentification et peuvent être utilisées pour implémenter des gestionnaires HTTP qui répondent avec un contenu `headers`, `statusCode` et `body` de différents types.

Les actions Web doivent renvoyer un objet JSON. Toutefois, le contrôleur traite une action Web différemment si son résultat inclut un ou plusieurs des éléments suivants comme des [propriétés JSON](#web_action_properties)de niveau supérieur.
{: shortdesc}

## Fonctions disponibles des actions Web
{: #actions_web_extra}

Les actions Web prennent en charge les fonctions suivantes : 

| Fonction | Description |
| --- | --- |
| [Extensions de contenu](#extra_features) | Vous pouvez spécifier un type de contenu pour les demandes HTTP telles que `.json`, `.html`, `.http`, `.svg` ou `.text`. Si aucun type de contenu n'est spécifié, l'extension `.http` est supposée. Vous pouvez spécifier un type de contenu en ajoutant une extension au nom d'action dans l'URI, afin qu'une action `demo/hello` soit référencée en tant que `/demo/hello.svg`. Les extensions `.json` et `.http` ne requièrent pas de chemin de projection, contrairement aux extensions `.html`, `.svg` et `.text`. Le chemin par défaut est supposé correspondre au nom d'extension. Pour appeler une action Web et recevoir une réponse `.html`, l'action doit répondre avec un objet JSON qui contient une propriété de niveau supérieur nommée `html` (ou bien la réponse doit figurer dans le chemin explicite). En d'autres termes, `/<namespace>/demo/hello.html` équivaut à projeter la propriété `html` de manière explicite, comme dans `/<namespace>/demo/hello.html/html`. Le nom qualifié complet de l'action doit inclure son nom de package, qui est `default` si l'action ne figure pas dans un package nommé. |
| [Projection de zones à partir du résultat](#projecting_fields) | Le chemin qui suit le nom de l'action est utilisé pour projeter un ou plusieurs niveaux de la réponse. Par exemple, `/demo/hello.html/body`. Cette fonction permet à toute action Web qui renvoie un dictionnaire, par exemple `{body: "..." }`, de projeter la propriété `body` et de renvoyer directement sa valeur de chaîne à la place. Le chemin projeté suit un modèle de chemin d'accès absolu (comme dans XPath). |
| [Paramètres de requête et de corps en tant qu'entrée](#query_test) | L'action reçoit des paramètres de requête ainsi que des paramètres dans le corps de la demande. L'ordre de priorité pour la fusion des paramètres est le suivant : paramètres de package, paramètres d'action, paramètres de requête et paramètres de corps. Chacun de ces paramètres peut remplacer les valeurs précédentes en cas de chevauchement. Par exemple, `/demo/hello.http?name=Jane` peut transmettre l'argument `{name: "Jane"}` à l'action.|
| [Données de formulaire](#form_data) | Outre l'élément `application/json` standard, les actions Web peuvent recevoir des données de formulaire codées en URL `application/x-www-form-urlencoded data` en tant qu'entrée.
| [Activations utilisant plusieurs instructions HTTP](#actions_web_options) | Une action Web peut être appelée via l'une des méthodes HTTP suivantes : `GET`, `POST`, `PUT`, `PATCH` et `DELETE`, ainsi que `HEAD` et `OPTIONS`. |
| [Traitement des entités non JSON et des entités HTTP brutes](#actions_web_raw_enable) | Une action Web peut accepter un corps de demande HTTP autre qu'un objet JSON et peut choisir de toujours recevoir ces valeurs sous forme de valeurs opaques (texte en clair lorsqu'il ne s'agit pas d'un fichier binaire ou d'une chaîne codée en base64). |

## Création d'une action Web
{: #actions_web_example}

Pour créer une action Web : 

1. Sauvegardez le code JavaScript suivant en tant que `hello.js`.

  ```javascript
  function main({name}) {
    var msg = 'You did not tell me who you are.';
  if (name) {
      msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
  }
  ```
  {: codeblock}

2. Créez le package `demo`. Le nom de package est `default` s'il n'est pas spécifié explicitement.
  ```
  ibmcloud fn package create demo
  ```
  {: pre}

3. Créez l'action `hello`. Dans cet exemple, `packageName/actionName` correspond à `demo/hello`. Remplacez la variable `<filepath>` par le chemin de votre fichier `hello.js` et définissez l'indicateur `--web` sur `true`. 

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

4. Appelez ou teste l'action Web `hello` sans aucun paramètre. Remplacez les variables `<apihost>` et `<namespace>`. Pour obtenir `<apihost>`, exécutez la commande `ibmcloud fn property get --apihost`. Exemple `<apihost>` : `us-south.functions.cloud.ibm.com`.

  Pour les espaces de nom activés pour IAM, remplacez la variable `<namespace>` par l'ID espace de nom. Pour obtenir l'ID, exécutez `ibmcloud fn namespace get <namespace_name>`.
  {: note}

  a. Vous pouvez tester l'action Web : 
    * En ouvrant une URL à l'aide de la structure `https://<apihost>/api/v1/web/<namespace>/demo/hello` dans votre navigateur. 
    * En testant l'action via une commande cURL.
      ```
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

    * En testant l'action via une commande `wget`.  
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

  b. Le code d'action renvoie le dictionnaire suivant.
    ```
    {body: `<html><body><h3>${msg}</h3></body></html>`}
    ``` 
    {: screen}
    
  Vous pouvez également tester l'action en renvoyant seulement la propriété `body` via la commande suivante :
  {: #projecting_fields}

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello.html/body
    ```
    {: pre}

    **Exemple de sortie**

    Comme le paramètre `<name>` n'a pas été spécifié, le message suivant est renvoyé :
    ```
    <html><body><h3>You did not tell me who you are. (vous ne m'avez pas dit qui vous êtes)</h3></body></html>
    ```
    {: screen}

5. A présent, essayons de définir le paramètre `<name>`. Testez l'action avec un paramètre `<name>` :
  * En ouvrant `https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane` dans votre navigateur.  
  * En testant l'action via une commande cURL.

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}
  * En testant l'action via une commande `wget`.  
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}

  **Exemple de sortie**
  ```
  <html><body><h3>Hello, Jane!</h3></body></html>
  ```
  {: screen}


**Etapes suivantes**

Ajoutez l'URL de votre action Web `hello` à votre application Web et testez-la ici.

### Propriétés JSON d'action Web
{: #web_action_properties}

La valeur par défaut `Content-Type` d'une réponse HTTP est `application/json` et le corps peut être toute valeur JSON autorisée. Si votre `Content-Type` n'est pas `application/json`, vous devez spécifier un `Content-Type` dans les en-têtes (`headers`) de votre code d'action. 

Si la [taille limite de résultat](/docs/openwhisk?topic=cloud-functions-limits) pour les actions est atteinte, la réponse échoue. Si vous savez que votre résultat d'action est supérieur à 5 Mo, définissez un [conteneur d'objets](/docs/openwhisk?topic=cloud-functions-pkg_obstorage).

| Propriété JSON | Description |
| --- | --- |
| `headers`| Objet JSON dans lequel les clés sont des noms d'en-tête et les valeurs sont sont des valeurs de chaîne, numériques ou booléennes. Pour que plusieurs valeurs puissent être envoyées pour un seul en-tête, la valeur de l'en-tête est un tableau de ces valeurs. Aucun en-tête n'est défini par défaut. |
| `statusCode` | Code de statut HTTP valide. Si le corps comporte un contenu, la valeur par défaut est `200 OK`. Si aucun contenu n'est présent pour le corps, la valeur par défaut est `204 No Content`. |
| `body` | Chaîne qui est un texte en clair, un objet JSON ou un tableau, ou une chaîne codée en base64 pour les données binaires. Le corps est considéré comme vide s'il comporte la valeur `null`, la chaîne vide `""`, ou s'il n'est pas défini. La valeur par défaut est un corps vide. |

Le [contrôleur](/docs/openwhisk?topic=cloud-functions-about#about_controller) transmet les en-têtes spécifiés par une action, le code d'état ou le corps au client HTTP qui met fin à la demande ou à la réponse. Si l'en-tête `Content-Type` n'est pas déclaré dans le contenu `headers` du résultat de l'action, le corps est interprété en tant que `application/json` pour les valeurs qui ne sont pas des chaînes, et en tant que `text/html` pour les autres. Lorsque l'en-tête `Content-Type` est défini, le contrôleur détermine si la réponse contient des données binaires ou un texte en clair et décode la chaîne à l'aide                  *d'un décodeur de base64 si besoin. Si le corps n'est pas décodé correctement, une erreur est renvoyée au client.

Le propriétaire de l'action Web détient tous les enregistrements des activations et supporte les coûts liés à l'exécution de l'action dans le système, quel que soit le mode d'appel de l'action employé.
{: note}

#### Paramètres protégés
Les paramètres d'action sont protégés et peuvent être modifiés uniquement par la mise à jour de votre action. Les paramètres sont finalisés automatiquement pour activer des actions Web.

```
ibmcloud fn action create /<namespace>/demo/hello hello.js --parameter name Jane --web true
```
{: pre}


Ces modifications ont pour conséquence de lier `name` à `Jane`, ce qui ne peut pas être remplacé par des paramètres de requête ou de corps en raison de l'annotation finale. Cette conception permet de sécuriser l'action sur les paramètres de requête ou de corps qui tentent de modifier cette valeur de manière intentionnelle ou fortuite.

### Exécution d'une redirection HTTP via une action Web
{: #http_redirect}
Vous pouvez utiliser cette fonction dans une application Web pour rediriger un utilisateur vers la nouvelle version de votre site.

**Avant de commencer**
Créez le package `demo` et l'action Web `hello` en exécutant la procédure de [Création d'une action Web](#actions_web_example).

Pour créer une action Web qui effectue une redirection HTTP :

1. Sauvegardez le code en tant que `hello.js`.

  ```javascript
  function main() {
    return {
      headers: { location: 'https://cloud.ibm.com/openwhisk/' },
      statusCode: 302
    }
  }
  ```
  {: codeblock}

2. Mettez à jour votre action Web `hello` avec la nouvelle version de votre code `hello.js`. Remplacez `<filepath>` par le chemin de votre fichier `hello.js`.

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Testez l'action Web `hello`. Remplacez les variables `<apihost>` et `<namespace>`. Vous pouvez tester l'action Web :

  * En ouvrant l'URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` dans votre navigateur.  
  * En exécutant la commande cURL suivante :
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * En exécutant la commande `wget` suivante :
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

  **Exemple de résultat** 
  
  Cet exemple d'action Web redirige votre navigateur vers le [tableau de bord {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk/){: external}.

### Définition de cookies via une action Web
{: #multiple_cookie}
Vous pouvez utiliser cette fonction dans une application Web pour stocker un jeton Web JSON en tant que cookie de session après une connexion réussie.

Pour créer une action Web qui définit plusieurs cookies :

**Avant de commencer**
Créez le package `demo` et l'action Web `hello` en exécutant la procédure de [Création d'une action Web](#actions_web_example).

1. Sauvegardez le code en tant que `hello.js`.
  ```javascript
  function main() {
    return {
      headers: {
        'Set-Cookie': [
          'UserID=Jane; Max-Age=3600; Version=',
        'SessionID=asdfgh123456; Path = /'
        ],
      'Content-Type': 'text/html'
      }, 
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
  }
  ```
  {: codeblock}

2. Mettez à jour votre action Web `hello` avec la nouvelle version de votre code `hello.js`. Remplacez `<filepath>` par le chemin de votre fichier `hello.js`.

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}
    
3. Effacez les cookies de votre navigateur avant de tester l'action.

4. Testez l'action Web `hello` en ouvrant l'URL dans votre navigateur. Remplacez les variables `<apihost>` et `<namespace>`, et ouvrez `https://<apihost>/api/v1/web/<namespace>/demo/hello`. Exemple `<apihost>` : `us-south.functions.cloud.ibm.com`.

**Résultat**

Les cookies `UserID=Jane` et `SessionID=asdfgh123456` sont définis dans les outils de développement de votre navigateur.


### Renvoi d'une image via une action Web
{: #return_image}
Vous pouvez utiliser cette fonction dans une application Web pour renvoyer l'image du drapeau d'un pays en fonction de l'environnement local de l'utilisateur.

**Avant de commencer** 

Créez le package `demo` et l'action Web `hello` en exécutant la procédure de [Création d'une action Web](#actions_web_example).

Pour créer une action Web qui renvoie une `image/png` : 

1. Sauvegardez le code en tant que `hello.js`.

  ```javascript
  function main() {
      let png = '<base 64 encoded string';
      return { headers: { 'Content-Type': 'image/png' },
              statusCode: 200,
              body: png };
  }
  ```
  {: codeblock}

2. Mettez à jour votre action Web `hello` avec la nouvelle version de votre code `hello.js`. Remplacez `<filepath>` par le chemin de votre fichier `hello.js`.

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Testez l'action dans votre navigateur ou en utilisant une commande cURL. Remplacez les variables `<apihost>` et `<namespace>`. Vous pouvez tester l'action Web :

  * En ouvrant l'URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` dans votre navigateur.  
  * En exécutant la commande cURL suivante.
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * En exécutant la commande `wget` suivante.
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}



### Renvoi de JSON via une action Web
{: #return_json}
Vous pouvez utiliser cette fonction dans une application Web pour renvoyer un objet JSON d'informations IP utilisateur.

**Avant de commencer** 

Créez le package `demo` et l'action Web `hello` en exécutant la procédure de [Création d'une action Web](#actions_web_example).

Pour créer une action Web qui renvoie `application/json` :

1. Sauvegardez le code en tant que `hello.js`.
  ```javascript
  function main(params) {
      return {
          statusCode: 200,
          headers: { 'Content-Type': 'application/json' },
          body: params
      };
  }
  ```
  {: codeblock}

2. Mettez à jour votre action Web `hello` avec la nouvelle version de votre code `hello.js`. Remplacez `<filepath>` par le chemin de votre fichier `hello.js`.
  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Testez l'action dans votre navigateur ou en utilisant une commande cURL. Remplacez les variables `<apihost>` et `<namespace>`. Vous pouvez tester l'action Web :
  * En ouvrant l'URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` dans votre navigateur.  
  * En exécutant la commande cURL suivante :
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * En exécutant la commande `wget` suivante :
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

    **Exemple de sortie**

    ```
    {
      "__ow_headers": {
        "accept": "*/*",
        "accept-encoding": "gzip",
        "cdn-loop": "cloudflare",
        "cf-connecting-ip": "XX.XXX.XXX.XXX",
        "cf-ipcountry": "US",
        "cf-ray": "4d9f3e442a86cf28-IAD",
        "cf-visitor": "{\"scheme\":\"https\"}",
        "host": "<apihost>",
        "user-agent": "curl/7.54.0",
        "x-forwarded-for": "XX.XXX.XX.XXX, XX.XXX.XX.XXX",
        "x-forwarded-host": "<apihost>",
        "x-forwarded-port": "443",
        "x-forwarded-proto": "https",
        "x-global-k8fdic-transaction-id": "11fd03071bd0841d3a00f52354ab880f",
        "x-real-ip": "XXX.XX.XX.XX",
        "x-request-id": "11fd03071bd0841d3a00f52354ab880f"
      },
      "__ow_method": "get",
      "__ow_path": ""
    }
    ```
    {: screen}


### Contexte HTTP
{: #actions_web_context}

Lorsqu'elles sont appelées, toutes les actions Web reçoivent des informations de demande HTTP en tant que paramètres d'entrée pour l'argument d'action.

| Paramètre HTTP | Type | Description |
| --- | --- | --- |
| `__ow_method` | Chaîne | méthode HTTP de la demande. |
| `__ow_headers` | Mappage chaîne à chaîne | En-têtes de demande. |
| `__ow_path` | Chaîne | Chemin d'accès sans correspondance de la demande (la mise en correspondance s'arrête une fois l'extension d'action consommée). |
| `__ow_user` | Chaîne | Espace de nom qui identifie le sujet authentifié par {{site.data.keyword.openwhisk_short}}. |
| `__ow_body` | Chaîne | Entité de corps de demande, sous la forme d'une chaîne codée en base64 lorsque le contenu est un fichier binaire, ou d'une chaîne en clair dans les autres cas. |
| `__ow_query` | Chaîne | Les paramètres de requête provenant de la demande en tant que chaîne non analysée. |

Une demande ne peut remplacer aucun des paramètres `__ow_` nommés. Si cela se produit, la demande échoue avec l'état 400 Bad Request.

Le paramètre `__ow_user` n'est présent que lorsque l'action Web est [annotée pour indiquer que l'authentification est requise](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions) et permet à une action Web d'implémenter sa propre règle d'autorisation. Le paramètre `__ow_query` n'est disponible que lorsqu'une action Web choisit de traiter la [demande HTTP "raw"](#actions_web_raw_enable). `__ow_query` est une chaîne qui contient les paramètres de requête analysés à partir de l'URI (séparés par `&`). La propriété `__ow_body` est présente dans les demandes HTTP brutes ou lorsque l'entité de demande HTTP n'est pas un objet JSON ni des données de formulaire. Sinon, les actions Web reçoivent les paramètres de requête et de corps comme propriétés de première classe dans les arguments d'action. Les paramètres de corps sont prioritaires sur les paramètres de requête, lesquels sont prioritaires sur les paramètres d'action et de package.

### Prise en charge de noeud final HTTPS
{: #actions_web_endpoint}

Protocoles SSL pris en charge : TLS 1.2, TLS 1.3 ([version brouillon 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18){: external})

### Modification du contenu de réponse de l'action Web
{: #extra_features}
Vous pouvez modifier le contenu de la réponse d'une action Web de façon à renvoyer différents types de contenu via des [extensions de contenu](#actions_web_extra).
{: shortdesc}

**Avant de commencer**

Créez le package `demo` et l'action Web `hello` en exécutant la procédure de [Création d'une action Web](#actions_web_example).

Pour modifier la réponse d'une action Web :

1. Sauvegardez le code suivant en tant que `hello.js`.

  ```javascript
  function main(params) {
      return { response: params };
}
  ```
  {: codeblock}

2. Mettez à jour votre action Web `hello` avec la nouvelle version de votre code `hello.js`.Remplacez `<filepath>` par le chemin de votre fichier `hello.js`.

  ```bash
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Testez l'action dans votre navigateur ou en utilisant une commande cURL. Remplacez les variables `<apihost>` et `<namespace>`. 

  a. Renvoyez le code JSON :
    * En ouvrant `https://<apihost>/api/v1/web/<namespace>/demo/hello.json` dans votre navigateur Web.  
    * En exécutant la commande cURL suivante.
    ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}
    * En exécutant la commande `wget` suivante.
    ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}

      **Exemple de sortie**

      ```
      {
        "response": {
          "__ow_method": "get",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  b. Testez l'action en utilisant un paramètre de requête. Vous pouvez tester l'action :
  {: #query_test}

    * En exécutant la commande cURL suivante. 

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

    * En exécutant la commande `wget` suivante. 

        ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

      **Exemple de sortie**
    ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "get",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  c. Vous pouvez également tester l'action Web en utilisant des données de formulaire. Vous pouvez tester l'action Web :
  {: #form_data}
  
    * En exécutant la commande cURL suivante. 

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}
      
    * En exécutant la commande `wget` suivante.
        ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}

      **Exemple de sortie**

      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "content-length": "10",
            "content-type": "application/x-www-form-urlencoded",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  d. Vous pouvez spécifier un objet JSON en exécutant la commande suivante. Vous pouvez tester l'action Web :
    * En exécutant la commande cURL suivante.
        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}
      
    * En exécutant la commande `wget` suivante.
        ```
        wget https://<apihost>/api/v1/web/{namespace/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}

      **Exemple de sortie**

      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "content-length": "15",
            "content-type": "application/json",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  e. Vous pouvez également renvoyer la valeur `name` sous forme de texte :
  * En exécutant la commande cURL suivante. 

      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}
  * En exécutant la commande `wget` suivante.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}

    **Exemple de sortie**

    ```
    Jane
    ```
    {: screen}

    Dans les actions standard, les paramètres de requête, les données de formulaire et les entités de corps d'objet JSON sont tous traités en tant que dictionnaires, car leurs valeurs sont directement accessibles en tant que propriétés d'entrée d'action. Ce n'est pas le cas des actions Web, qui traitent les entités de demande HTTP, ou quand l'action Web reçoit une entité qui n'est pas un objet JSON.
    {: note}

  f. Vous pouvez définir le type de contenu (`Content-Type`) :
  * En exécutant la commande cURL suivante.   
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}
    
  * En exécutant la commande `wget` suivante.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}

    **Exemple de sortie**

    ```
    {
      "response": {
        "__ow_method": "post",
    "__ow_headers": {
          "accept": "*/*",
          "connection": "close",
          "content-length": "4",
          "content-type": "text/plain",
          "host": "172.17.0.1",
          "user-agent": "curl/7.43.0"
        },
        "__ow_path": "",
        "__ow_body": "Jane"
      }
    }
    ```
    {: screen}

## Sécurisation des actions Web
{: #actions_web_secure}

**Avant de commencer**
Créez le package `demo` et l'action Web `hello` en exécutant la procédure de [Création d'une action Web](#actions_web_example).

Par défaut, quiconque peut appeler une action Web à l'aide de l'URL d'appel. Vous pouvez utiliser l'[annotation d'action Web](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions) `require-whisk-auth` afin de sécuriser l'action Web :
  1. En définissant l'annotation `require-whisk-auth` sur `true`. Lorsque l'annotation `requise-whisk-auth` est définie sur `true`, l'action Web authentifie les données d'identification de l'autorisation de base de la demande d'appel par rapport à la clé d'authentification whisk du propriétaire de l'action Web. Lorsqu'elle est définie sur un nombre ou une chaîne sensible à la casse, la demande d'appel de l'action Web doit inclure l'en-tête `X-Require-Whisk-Auth` défini sur ce même nombre ou cette même chaîne sensible à la casse. Les actions Web sécurisées renvoient le message `Not Authorized` lorsque la validation des données d'identification échoue.

  2. En autorisant la définition automatique de l'annotation `require-whisk-auth` via l'indicateur `--web-secure`. Lorsque l'option `--web-secure` est définie sur `true`, un nombre aléatoire est généré en tant que valeur d'annotation `exigeant-whisk-auth`. Si la valeur définie est `false`, l'annotation `require-whisk-auth` est retirée.  Avec une autre valeur définie, c'est cette valeur qui est utilisée comme valeur pour l'annotation `require-whisk-auth`.

Pour tester une action Web sécurisée :

1. Sauvegardez le code JavaScript suivant en tant que `hello.js`.
  ```javascript
  function main({name}) {
    var msg = 'You did not tell me who you are.';
  if (name) {
      msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
  }
  ```
  {: codeblock}

2. Mettez à jour votre action Web `hello` avec la nouvelle version de votre code `hello.js` et définissez l'indicateur `--web secure` sur `true`.
  ```bash
  ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true
  ```
  {: pre}

3. Obtenez l'action Web `hello` pour afficher la valeur `requise-whisk-auth` générée aléatoirement.

  ```bash
  ibmcloud fn action get demo/hello
  ```
  {: pre}

    **Exemple de sortie**

    The `require-whisk-auth` value was set to `7819991076995522`.
    ```
    {
      "namespace": "<namespace>/demo",
      "name": "hello",
      "version": "0.0.34",
      "exec": {
          "kind": "nodejs:10",
          "binary": false
      },
      "annotations": [
          {
              "key": "web-export",
              "value": true
          },
          {
              "key": "raw-http",
              "value": false
          },
          {
              "key": "final",
              "value": true
          },
          {
              "key": "require-whisk-auth",
              "value": 7819991076995522
          },
          {
              "key": "exec",
              "value": "nodejs:10"
          }
      ],
    "limits": {
          "timeout": 60000,
          "memory": 256,
          "logs": 10,
          "concurrency": 1
      },
    "publish": false
}
    ```
    {: screen}

Pour vérifier que l'authentification fonctionne :

1. Testez l'action Web `hello` sans définir le paramètre `X-Require-Whisk-Auth`, afin de vérifier que l'authentification est obligatoire. Ce test doit aboutir à une erreur. Vous pouvez tester l'action Web :

  * En testant l'action Web via une commande cURL.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}
    
  * En testant l'action Web via une commande `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}

   **Exemple de sortie**

    ```
      {
      "code": "4c4423556547f6ac764ee192d4ed27a6",
      "error": "Authentication is possible but has failed or not yet been provided."
    }
    ```
    {: screen}

    L'appel échoue car la valeur `X-Require-Whisk-Auth` n'est pas fournie.
    {: note}

2. A présent, testez l'action Web `hello` et indiquez la valeur `X-Require-Whisk-Auth` générée de manière aléatoire. Remplacez les valeurs `<apihost>` et `<namespace>`. Remplacez la valeur `<my-secret>` par le nombre généré aléatoirement créé à l'étape 3. Vous pouvez tester l'action Web :
  * En testant l'action Web via une commande cURL.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  * En testant l'action Web via une commande `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  **Exemple de sortie**
    
    ```
    {
    "body": "<html><body><h3>Hello, Jane!</h3></body></html>"
    }
    ```
    {: screen}

Pour tester une action Web à l'aide d'une valeur `requise-whisk-auth` personnalisée :

1. Mettez à jour votre action Web `hello` avec votre propre valeur `requise-whisk-auth`. Essayez ensuite de tester votre action Web en spécifiant la valeur `X-Require-Whisk-Auth` lors de l'appel.

  a. Définissez une valeur `require-whisk-auth`, où `<my-secret>` est votre jeton d'authentification sensible à la casse.
    ```bash
    ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true --require-whisk-auth <mysecret>
    ```
    {: pre}
  
  b. Testez l'action Web et incluez votre valeur `<my-secret>`. Vous pouvez tester l'action Web :
  * En testant l'action Web via une commande cURL.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}
  * En testant l'action via une commande `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}


## Traitement de demande HTTP brute
{: #actions_web_raw}

Une action Web peut choisir d'interpréter et de traiter directement un corps HTTP entrant, sans la promotion d'un objet JSON vers les propriétés de première classe disponibles pour l'entrée d'action Web (par exemple, `args.name` par rapport à l'analyse `args.__ow_query`). Ce processus s'effectue via une [annotation](/docs/openwhisk?topic=cloud-functions-annotations) `raw-http`. Utilisation de l'exemple précédent, mais cette fois-ci avec une action Web HTTP brute qui reçoit `name` comme paramètre de requête et comme valeur JSON dans le corps de demande HTTP :
```bash
curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}


**Exemple de sortie**
```
{
  "response": {
    "__ow_method": "post",
    "__ow_query": "name=Jane",
    "__ow_body": "eyJuYW1lIjoiSmFuZSJ9",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "15",      
      "content-type": "application/json",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```
{: screen}

{{site.data.keyword.openwhisk_short}} utilise l'infrastructure [HTTP Akka](https://doc.akka.io/docs/akka-http/current/?language=scala){: external} pour [déterminer quels types de contenu correspondent à des fichiers binaires et quels types correspondent à du texte en clair](https://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html){: external}.

### Activation du traitement de demande HTTP brute
{: #actions_web_raw_enable}

Vous pouvez créer une action Web HTTP brute en définissant `--web` sur `raw`.
```bash
ibmcloud fn action create demo/hello /<filepath>/hello.js --web raw
```
{: pre}

### Décodage de contenu de code binaire depuis Base64
{: #actions_web_decode}

Lorsque du contenu HTTP brut est traité, le contenu `__ow_body` est codé en Base64 lorsque la partie `Content-Type` de la demande est le fichier binaire. Les fonctions ci-dessous montrent comment décoder le contenu du corps dans Node, Python et Swift.

1. Sauvegardez l'exemple de code dans le langage de votre choix, dans un fichier appelé `decode.<ext>`. Remplacez `<ext>` par l'extension de fichier de l'exemple de code de votre langage préféré.

  **Node**
  {: #actions_web_decode_js}

  ```javascript
  function main(args) {
      decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
      return {body: decoded}
  }
  ```
  {: codeblock}

  **Python**
  {: #actions_web_decode_python}

  ```python
  def main(args):
    try:
        decoded = args['__ow_body'].decode('base64').strip()
        return {"body": decoded}
    except:
        return {"body": "Could not decode body from Base64."}
  ```
  {: codeblock}

  **Swift**
  {: #actions_web_decode_swift}

  ```swift
  extension String {
      func base64Decode() -> String? {
          guard let data = Data(base64Encoded: self) else {
              return nil
          }

          return String(data: data, encoding: .utf8)
      }
  }

func main(args: [String:Any]) -> [String:Any] {
      if let body = args["__ow_body"] as? String {
          if let decoded = body.base64Decode() {
              return [ "body" : decoded ]
        }
      }

      return ["body": "Could not decode body from Base64."]
  }
  ```
  {: codeblock}

2. Créez une action Web HTTP brute avec l'exemple de code en exécutant la commande suivante. Dans cet exemple, la fonction Node est sauvegardée en tant que `decode.js`. Remplacez le chemin de fichier par celui de votre fichier `decode` et mettez à jour l'extension de fichier pour qu'elle corresponde à l'extension du code exemple que vous avez utilisé.

  ```bash
  ibmcloud fn action create decode <filepath>/decode.js --web raw
  ```
  {: pre}

  **Exemple de sortie**
  ```
  ok: created action decode
  ```
  {: screen}

3. Testez l'action `decode` en exécutant la commande cURL suivante.
    ```bash
    curl -k -H "content-type: application" -X POST -d "Decoded body" https://<apihost>/api/v1/web/<namespace>/default/decode.json
    ```
    {: pre}

  **Exemple de sortie**
    ```
    {
      "body": "Decoded body"
    }
    ```
    {: screen}

## Demandes Options
{: #actions_web_options}

Par défaut, une demande `OPTIONS` adressée à une action Web entraîne l'ajout automatique d'en-têtes CORS dans les en-têtes de réponse. Ces en-têtes admettent toutes les origines et instructions HTTP `OPTIONS`, `GET`, `DELETE`, `POST`, `PUT`, `HEAD` et `PATCH`.
{: shortdesc}

Examinez les en-têtes suivants :
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

Les demandes `OPTIONS` peuvent également être traitées manuellement par une action Web. Pour activer cette option, ajoutez une annotation
`web-custom-options` avec la valeur `true` à une action Web. Lorsque cette fonction est activée, les en-têtes CORS ne sont pas ajoutés automatiquement dans la réponse à la demande. A la place, vous devez ajouter vos en-têtes à l'aide d'un programme.

Pour créer des réponses personnalisées aux demandes `OPTIONS` :

1. Sauvegardez le code suivant dans un fichier `custom-options.js`.

  ```js
  function main(params) {
    if (params.__ow_method == "options") {
      return {
        headers: {
          'Access-Control-Allow-Methods': 'OPTIONS, GET',
        'Access-Control-Allow-Origin': 'example.com'
        },
      statusCode: 200
    }
    }
  }
  ```
  {: codeblock}

2. Créez l'action Web. Définissez `--web-custom-options` sur `true`.

  ```bash
  ibmcloud fn action create custom-option <filepath>/custom-options.js --web true -a web-custom-options true
  ```
  {: pre}

3. Testez l'action via la commande cURL suivante.

  ```bash
  $ curl https://<apihost>/api/v1/web/<namespace>/default/custom-option.http -kvX OPTIONS
  ```
  {: pre}

  **Exemple de sortie**
  ```
  < HTTP/1.1 200 OK
< Server: nginx/1.11.13
< Content-Length: 0
< Connection: keep-alive
< Access-Control-Allow-Methods: OPTIONS, GET
< Access-Control-Allow-Origin: example.com
  ```
  {: screen}

## Traitement d'erreurs
{: #actions_web_errors}

Une action {{site.data.keyword.openwhisk_short}} échoue dans deux modes d'échec possibles. Le premier correspond à une erreur d'application (_application error_) et est semblable à une exception interceptée : l'action renvoie un objet JSON contenant une propriété `error` de niveau supérieur. Le second correspond à une erreur de développeur (_developer error_) et est activé lorsque l'action échoue sans générer de réponse (semblable à une exception non interceptée). Pour les actions Web, le contrôleur traite les erreurs d'application comme suit :

- Toute projection de chemin spécifiée est ignorée et le contrôleur projette la propriété `error` à la place.
- Le contrôleur applique le traitement de contenu induit par l'extension d'action à la valeur de la propriété `error`.

Les développeurs doivent savoir comment les actions Web peuvent être utilisées et ils doivent générer des réponses d'erreur en conséquence. Par exemple, une action Web qui est utilisée avec l'extension `.http` renvoie une réponse HTTP de ce type : `{error: { statusCode: 400 }`. Si tel n'est pas le cas, une non-concordance est générée entre le type de contenu (`Content-Type`) induit par l'extension et le type de contenu (`Content-Type`) de l'action dans la réponse d'erreur. Une attention particulière doit être accordée aux actions Web qui sont des séquences, de sorte que les composants qui constituent une séquence puissent générer les erreurs appropriées, le cas échéant.



## Désactivation d'actions Web
{: #actions_web_disable}

Vous pouvez désactiver une action Web en définissant l'indicateur `--web` sur `false` ou `no` dans l'interface de ligne de commande. Remplacez `<packageName>/<actionName>` et `<filepath>/<filename>` par le nom de package, le nom d'action Web, le chemin d'accès au fichier et le nom de fichier de votre fichier de code.

```bash
ibmcloud fn action update <packageName>/<actionName> <filepath>/<filename> --web false
```
{: pre}



