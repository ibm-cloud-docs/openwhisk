---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-27"

keywords: actions, serverless, javascript, node, node.js

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


# Création d'actions JavaScript
{: #actions_javascript
{: #creating-and-invoking-javascript-actions}

Les sections ci-après expliquent comment utiliser des actions dans JavaScript. Commencez par créer et appeler une action simple. Ensuite, ajoutez des paramètres à l'action et appelez cette action avec des paramètres. Définissez aussi des paramètres par défaut et appelez-les. Enfin, créez des actions asynchrones.
{: shortdesc}

## Création et appel d'une action JavaScript simple
{: #single_action_js}
{: #openwhisk_single_action_js}

Suivez les étapes et les exemples ci-dessous pour créer votre première action JavaScript.

1. Sauvegardez le code suivant dans un fichier nommé `hello.js`.

  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  Le fichier JavaScript peut contenir des fonctions supplémentaires. Toutefois, par convention, une fonction appelée `main` doit exister pour fournir le point d'entrée de l'action.
  {: shortdesc}

2. Créez une action nommée `hello` à l'aide de la fonction JavaScript.

  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  Exemple de sortie :
  
  ```
  ok: created action hello
  ```
  {: screen}

  Le type d'action est déterminé à l'aide de l'extension du fichier source. Pour les fichiers source `.js`, l'action est lancée dans un environnement d'exécution Node.js. Vous pouvez spécifier la version de l'environnement d'exécution Node.js pour votre action JavaScript en définissant le paramètre `--kind` sur `nodejs:10` ou `nodejs:8`. Pour plus d'informations sur l'environnement d'exécution Node.js, voir [Contextes d'exécution](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments).
  {: shortdesc}

3. Vérifiez que votre action `hello` est dans votre liste d'actions.

  ```
  ibmcloud fn action list
  ```
  {: pre}

  Exemple de sortie :
  
  ```
  actions
  hello       private
  ```
  {: screen}

4. Exécutez l'action dans le cloud en exécutant un appel de blocage. Les appels de blocage utilisent un style demande/réponse et attendent que le résultat de l'activation soit disponible. Le délai d'attente est inférieur à 60 secondes ou à la [limite de temps](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits) de l'action.

    ```
    ibmcloud fn action invoke --blocking hello
    ```
    {: pre}

    La commande génère les informations suivantes :
        * ID d'activation (`44794bd6aab74415b4e42a308d880e5b`) qui peut être utilisé pour extraire les journaux ou le résultat de l'appel.
        * Résultat de l'appel s'il est disponible avant la fin du délai d'attente

    ```
    ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

    {
        "result": {
            "payload": "Hello world"
        },
      "status": "success",
      "success": true
  }
    ```
    {: screen}
    
    Pour des conseils sur la surveillance des activations, consultez [Monitoring action output](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#monitor-action-output).
    {: tip}

5. Si vous n'avez pas besoin du résultat de l'action immédiatement, vous pouvez omettre l'indicateur `--blocking` pour effectuer un appel non bloquant.

    1. Exécutez un appel non bloquant.

        ```
        ibmcloud fn action invoke hello
        ```
        {: pre}

        Exemple de sortie :

        ```
        ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: screen}

    2. Utilisez l'ID d'activation pour obtenir le résultat de l'action.

        ```
        ibmcloud fn activation result 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: pre}

        Le résultat de l'action est renvoyé :

        ```
        {
            "payload": "Hello world"
  }
        ```
        {: screen}

6. Si vous oubliez de noter l'ID d'activation, vous pouvez afficher la liste des activations demandées, de la plus récente à la plus ancienne.

    ```
    ibmcloud fn activation list
    ```
    {: pre}

    Sortie :
    ```
    activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
    ```
    {: screen}
    
### Prototype de fonction Javascript
{: #openwhisk_ref_javascript_fnproto}

Les actions {{site.data.keyword.openwhisk_short}} JavaScript s'exécutent dans un environnement d'exécution Node.js.

Les actions écrites en JavaScript doivent être confinées dans un seul fichier. Ce dernier peut contenir plusieurs fonctions mais par convention, une fonction nommée `main` doit exister. Il s'agit de la fonction appelée lors de l'appel de l'action. Voici un exemple d'action avec plusieurs fonctions :
```javascript
function main() {
    return { payload: helper() }
} function helper() {
    return new Date();
}
```
{: codeblock}

Les paramètres d'entrée de l'action sont transmis sous forme d'objet JSON en tant que paramètre dans la fonction `main`. Le résultat d'une activation réussie est également un objet JSON ; toutefois, il est renvoyé différemment selon que l'action est synchrone ou asynchrone, comme décrit dans la section suivante.

### Comportement synchrone et asynchrone
{: #openwhisk_ref_javascript_synchasynch}

Il est fréquent que l'exécution des fonctions JavaScript continue dans une fonction de rappel même après leur retour. En conséquence, l'activation d'une action JavaScript peut être *synchrone* ou *asynchrone*.

L'activation d'une action JavaScript est **synchrone** si la fonction main se termine dans l'une des conditions suivantes :

- La fonction main se termine sans exécuter d'instruction `return`.
- La fonction main se termine en exécutant une instruction `return` qui renvoie n'importe quelle valeur *sauf* une promesse (objet Promise).

Voici un exemple d'action synchrone :

```javascript
// an action in which each path results in a synchronous activation
function main(params) {
  if (params.payload == 0) {
     return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
     return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}

L'activation d'une action JavaScript est **asynchrone** si la fonction main se termine en renvoyant une promesse (objet Promise). Dans ce cas, le système suppose que l'action est toujours en cours d'exécution jusqu'à ce que la promesse (objet Promise) soit satisfaite ou rejetée.
Commencez par instancier une nouvelle promesse (objet Promise) que vous transmettez à une fonction de rappel. La fonction de rappel admet deux arguments, resolve et reject, qui sont tous les deux des fonctions. Tout le code asynchrone est inséré dans ce rappel.

Voici un exemple illustrant la manière de satisfaire une promesse (objet Promise) en appelant la fonction resolve.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         resolve({ done: true });
       }, 100);
    })
}
```
{: codeblock}

Voici un exemple illustrant la manière de rejeter une promesse (objet Promise) en appelant la fonction reject.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 100);
    })
}
```
{: codeblock}

Il est possible qu'une action soit synchrone pour certaines entrées et asynchrone pour d'autres, comme illustré dans l'exemple suivant :
```javascript
function main(params) {
     if (params.payload) {
        // asynchronous activation
         return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
       }, 100);
    })
     } else {
        // synchronous activation
         return {done: true};
      }
}
```
{: codeblock}

Que l'activation soit synchrone ou asynchrone, l'appel de l'action peut être bloquant ou non bloquant.

## Création d'actions asynchrones
{: #asynchronous_javascript}
{: #openwhisk_asynchrony_js}

Les fonctions JavaScript qui s'exécutent de manière asynchrone peuvent renvoyer le résultat d'activation après le retour de la fonction `main` en renvoyant une promesse (objet Promise) dans votre action.

1. Sauvegardez le code suivant dans un fichier nommé `asyncAction.js`.

    ```javascript
    function main(args) {
         return new Promise(function(resolve, reject) {
           setTimeout(function() {
             resolve({ done: true });
         }, 2000);
      })
     }
    ```
    {: codeblock}

    * La fonction `main` renvoie une promesse. Celle-ci indique que l'activation n'est pas encore terminée mais qu'elle le sera prochainement .

    * La fonction JavaScript `setTimeout()` observe un délai de 2 secondes avant d'appeler la fonction de rappel de promesse, laquelle représente le code asynchrone.

    * La rappel de promesse accepte les arguments `resolve` et `reject`, lesquels sont tous deux des fonctions.

      * L'appel vers `resolve()` satisfait la promesse (objet Promise) et indique que l'activation aboutit.
      * Un appel vers `reject()` peut être utilisé pour rejeter la promesse (objet Promise) et signaler que l'activation ne s'est pas terminée normalement.

2. Créez une action nommée `asyncAction`.
    ```
    ibmcloud fn action create asyncAction asyncAction.js --kind nodejs:10
    ```
    {: pre}

3. Appelez l'action.

    ```
    ibmcloud fn action invoke --result asyncAction
    ```
    {: pre}

    Exemple de sortie :

    ```
    {
        "done": true
  }
    ```
    {: screen}

4. Vérifiez combien de temps a duré l'activation en consultant le journal d'activation.

  1. Obtenez l'ID d'activation.

      ```
      ibmcloud fn activation list --limit 1 asyncAction
      ```
      {: pre}

      Exemple de sortie :
      ```
      activations
  b066ca51e68c4d3382df2d8033265db0             asyncAction
      ```
      {: screen}

  2. Obtenez le journal de l'ID d'activation.

      ```
      ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
      ```
      {: pre}

      `duration` indique que l'exécution de cette activation a pris légèrement plus de 2 secondes :

      ```
      ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
          ...
          "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
          ...
      }
      ```
      {: screen}

## Utilisation d'actions pour appeler une API externe
{: #apicall_action}
{: #openwhisk_apicall_action}

Jusqu'à présent, les exemples fournis sont des fonctions JavaScript autonomes. Vous pouvez également créer une action qui appelle une API externe.
{: shortdesc}

L'exemple suivant appelle le service APOD (Astronomy Picture of the Day) de la NASA qui fournit tous les jours une image unique de notre univers.

1. Sauvegardez le code suivant dans un fichier nommé `apod.js`.

    ```javascript
    let rp = require('request-promise')

    function main(params) {
        const options = {
            uri: "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo",
            json: true
        }
        return rp(options)
        .then(res => {
            return { response: res }
        })
    }
    ```
    {: codeblock}

2. Un appel de l'API de la NASA APOD est émis et les zones sont extraites du résultat au format JSON. Pour plus d'informations sur les packages Node.js que vous pouvez utiliser dans vos actions, voir [Contextes d'exécution](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments).

3. Créez une action nommée `apod`.
    ```
    ibmcloud fn action create apod apod.js --kind nodejs:10
    ```
    {: pre}

3. Appelez l'action `apod`.
    ```
    ibmcloud fn action invoke --result apod
    ```
    {: pre}

    l'exemple d'objet suivant est renvoyé :

    ```
    {
      "copyright": "Eric Houck",
      "date": "2018-03-28",
      "explanation": "Does an alignment like this occur only once in a blue moon? ...",
      "hdurl": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_1799.jpg",
      "media_type": "image",
      "service_version": "v1",
      "title": "Blue Moon Tree",
      "url": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_960.jpg"
    }
    ```
    {: screen}

## Conditionnement d'une action dans un module Node.js
{: #packaging_javascript_actions}
{: #openwhisk_js_packaged_action}

Comme alternative à l'écriture de l'ensemble du code de votre action dans un seul fichier source JavaScript, vous pouvez écrire une action en tant que package `npm`.

Par exemple, imaginez un répertoire avec les fichiers suivants :

1. Sauvegardez le code suivant dans un fichier nommé `package.json`.

    ```json
    {
      "name": "my-action",
      "main": "index.js",
      "dependencies" : {
        "left-pad" : "1.1.3"
      }
    }
    ```
    {: codeblock}

2. Sauvegardez le code suivant dans un fichier nommé `index.js`.

    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
        const lines = args.lines || [];
        return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    exports.main = myAction;
    ```
    {: codeblock}

    * L'action est exposée via `exports.main`.
    * Le gestionnaire d'action proprement dit peut avoir n'importe quel nom, tant que celui-ci est conforme à la signature habituelle pour l'acceptation et le renvoi d'un objet (ou d'une promesse d'objet (`Promise`)).
    * Vous devez soit nommer ce fichier `index.js`, soit spécifier le nom de fichier de votre choix via la propriété `main` dans `package.json`.

3. Installez toutes les dépendances en local.

    ```
    npm install
    ```
    {: pre}

    **Remarque **: Alors que la plupart des packages `npm` installent des sources JavaScript avec la commande `npm install`, certains installent et compilent également des artefacts binaires. Le téléchargement de fichier archive prend en charge uniquement les dépendances JavaScript. Les appels d'action risquent d'échouer si l'archive comprend des dépendances binaires.

4. Créez une archive `.zip` contenant tous les fichiers ,notamment toutes les dépendances.

    ```
    zip -r action.zip *
    ```
    {: pre}

    L'utilisation d'une action Windows Explorer pour créer le fichier zip génère une structure incorrecte. Les actions .zip {{site.data.keyword.openwhisk_short}} nécessitent que `package.json` soit placé à la racine du fichier zip, alors que l'Explorateur Windows le place dans un dossier imbriqué. L'option la plus sûre consiste à utiliser la commande `zip` de ligne de commande.
    {: tip}

5. Créez l'action. Lorsque vous créez une action à partir d'une archive `.zip`, vous devez définir une valeur pour le paramètre `--kind` pour spécifier la version de votre environnement d'exécution Node.js. Choisissez entre `nodejs:8` et `nodejs:10`.

    ```
    ibmcloud fn action create packageAction action.zip --kind nodejs:10
    ```
    {: pre}

6. Appelez l'action.

    ```
    ibmcloud fn action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
    ```
    {: pre}

    Exemple de sortie :

    ```
    {
        "padded": [
            ".......................and now",
          "......for something completely",
          ".....................different"
      ]
    }
    ```
    {: screen}

## Conditionnement d'une action dans un bundle unique
{: #webpack_javascript}
{: #openwhisk_js_webpack_action}

Si le conditionnement de l'action sous forme de .zip inclut trop de fichiers inutiles ou si vous avez besoin d'un déploiement plus rapide, vous pouvez écrire le code minimal dans un seul fichier `.js` qui inclut les dépendances.
{: shortdesc}

Vous pouvez conditionner une action à l'aide d'un programme de mise en bundle de module JavaScript, tel que [webpack ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://webpack.js.org/concepts/). Lorsque `webpack` traite votre code, il génère de manière récursive un graphique de dépendance incluant chaque module dont votre action a besoin.

1. Sauvegardez le code suivant dans un fichier nommé `package.json`. `webpack` est ajouté en tant que dépendance de développement. 

    ```json
    {
      "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
        "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10"
      },
  "dependencies": {
        "left-pad" : "1.1.3"
      },
  "devDependencies": {
        "webpack": "^3.8.1"
      }
    }
    ```
    {: codeblock}

2. Sauvegardez le code de configuration webpack suivant dans un fichier nommé `webpack.config.js`.

    ```javascript
    var path = require('path');
    module.exports = {
      entry: './index.js',
  output: {
        path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
      },
  target: 'node'
    };
    ```
    {: codeblock}

3. Sauvegardez le code suivant dans un fichier nommé `index.js`. La variable `global.main` est définie sur la fonction main de l'action.

    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
        const lines = args.lines || [];
        return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    global.main = myAction;
    ```
    {: codeblock}

4. Installez toutes les dépendances en local.

    ```
    npm install
    ```
    {: pre}

5. Générez le bundle webpack.

    ```
    npm run build
    ```
    {: pre}

    Le fichier `dist/bundle.js` est créé et déployé en tant que code source de l'action.

6. Créez l'action à l'aide du script `npm` ou de l'interface de ligne de commande.

    * Utilisation du script `npm` :

        ```
        npm run deploy
        ```
        {: pre}

    * Utilisation de l'interface de ligne de commande :

        ```
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    Le fichier bundle qui est généré par `webpack` prend en charge uniquement les dépendances JavaScript. Les appels d'action échouent si le bundle dépend de dépendances de fichiers binaires, car cela n'est pas inclus avec le fichier `bundle.js`.{: tip}
    



