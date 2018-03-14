---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Création et appel d'actions
{: #openwhisk_actions}

Les actions sont des fragments de code sans état qui s'exécutent sur la plateforme {{site.data.keyword.openwhisk}}. Par exemple, une action peut être utilisée pour détecter les visages dans une image, répondre à une modification de base de données, agréger un ensemble d'appels API ou publier un tweet.
Une action peut être écrite en tant que fonction JavaScript, Swift, Python, fonction PHP, méthode Java ou n'importe quel exécutable compatible binaire, y compris des programmes Go et des programmes exécutables personnalisés dans des conteneurs Docker. {:shortdesc}

Les actions peuvent être appelées explicitement ou s'exécuter en réponse à un événement. Dans tous les cas, chaque exécution d'une action génère un enregistrement d'activation identifié par un ID d'activation unique. L'entrée pour une action et le résultat d'une action constituent un dictionnaire de paires clé-valeur, où la clé est une chaîne et la valeur est une valeur JSON valide. Les actions peuvent également être composées d'appels à d'autres actions ou d'une séquence définie d'actions.

Apprenez à créer, à appeler et à déboguer des actions dans votre environnement de développement préféré :
* [JavaScript](#creating-and-invoking-javascript-actions)
* [Swift](#creating-swift-actions)
* [Python](#creating-python-actions)
* [Java](#creating-java-actions)
* [PHP](#creating-php-actions)
* [Docker](#creating-docker-actions)
* [Go](#creating-go-actions)
* [Programmes exécutables arbitraires](#creating-actions-arbitrary)

Découvrez également les opérations suivantes :
* [Surveillance de la sortie des actions](#watching-action-output)
* [Prise en charge des applications volumineuses](#large-app-support)
* [Affichage de la liste des actions](#listing-actions)
* [Suppression d'actions](#deleting-actions)
* [Accès aux métadonnées d'action dans le corps de l'action](#accessing-action-metadata-within-the-action-body)


## Création et appel des actions JavaScript
{: #creating-and-invoking-javascript-actions}

Les sections ci-après expliquent comment utiliser des actions dans JavaScript. Vous commencez par créer et appeler une action simple. Ensuite, vous ajoutez des paramètres à une action et vous appelez cette action avec des paramètres. Viennent ensuite les phases de définition et d'appel des paramètres par défaut. Ensuite, vous créez des actions asynchrones et, enfin, vous gérez des séquences d'actions.


### Création et appel d'une simple action JavaScript
{: #openwhisk_single_action_js}

Suivez les étapes et les exemples ci-dessous pour créer votre première action JavaScript.

1. Créez un fichier JavaScript avec le contenu ci-après. Dans cet exemple, le nom de fichier est 'hello.js'.

  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  Le fichier JavaScript peut contenir des fonctions supplémentaires. Toutefois, par convention, une fonction appelée `main` doit exister pour fournir le point d'entrée de l'action.

2. Créez une action à partir de la fonction JavaScript ci-dessous. Dans cet exemple, l'action s'appelle 'hello'.

  ```
  wsk action create hello hello.js
  ```
  {: pre}

  ```
  ok: created action hello
  ```
L'interface de ligne de commande infère automatiquement le type d'action en utilisant l'extension du fichier source. Pour les fichiers source `.js`, l'action est lancée dans un environnement d'exécution Node.js 6. Vous pouvez également créer une action opérant avec Node.js 8 en mentionnant spécifiquement le paramètre `--kind nodejs:8`. Pour plus d'informations, voir le [document de référence](./openwhisk_reference.html#openwhisk_ref_javascript_environments) au sujet des environnements Node.js 6 et 8. 
  
3. Répertoriez les actions que vous avez créées :

  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  hello       private
  ```

  Vous pouvez voir l'action `hello` que vous avez créée. 

4. Une fois l'action créée, vous pouvez l'exécuter dans le cloud dans OpenWhisk avec la commande 'invoke'. Vous pouvez appeler des actions avec un appel *bloquant* (style demande/réponse) ou *non bloquant* en spécifiant un indicateur dans la commande. Une demande d'appel bloquante _attend_ que le résultat de l'activation soit disponible. Le délai d'attente est inférieur à 60 secondes ou à la [limite de temps](./openwhisk_reference.html#openwhisk_syslimits) de l'action. Le résultat de l'activation est renvoyé s'il est disponible avant la fin du délai d'attente. Sinon, le traitement de l'activation continue dans le système et un ID d'activation est renvoyé de sorte que les résultats puissent être consultés ultérieurement, comme dans le cas des demandes non bloquantes (voir [ici](#watching-action-output) pour obtenir des astuces sur la surveillance des activations).

  Cet exemple utilise le paramètre de blocage, `--blocking`:

  ```
  wsk action invoke --blocking hello
  ```
  {: pre}

  ```
  ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b
  ```

  ```json
  {
      "result": {
          "payload": "Hello world"
      },
      "status": "success",
      "success": true
  }
  ```

  La commande génère deux informations importantes :
  * L'ID d'activation (`44794bd6aab74415b4e42a308d880e5b`)
  * Le résultat de l'appel s'il est disponible avant la fin du délai d'attente

  Dans ce cas, le résultat est la chaîne `Hello world` renvoyée par la fonction JavaScript. L'ID d'activation peut être utilisé pour extraire les journaux ou le résultat de l'appel ultérieurement.  

5. Si vous n'avez pas besoin du résultat de l'action immédiatement, vous pouvez omettre l'indicateur `--blocking` pour effectuer un appel non bloquant. Vous pouvez afficher le résultat ultérieurement avec l'ID d'activation. Exemple :

  ```
  wsk action invoke hello
  ```
  {: pre}

  ```
  ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
  ```

  ```
  wsk activation result 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: pre}

  ```json
  {
      "payload": "Hello world"
  }
  ```

6. Si vous oubliez de noter l'ID d'activation, vous pouvez afficher la liste des activations demandées, de la plus récente à la plus ancienne. Exécutez la commande suivante pour obtenir la liste de vos activations :

  ```
  wsk activation list
  ```
  {: pre}

  ```
  activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
  ```

### Transmission de paramètres à une action
{: #openwhisk_pass_params}

Des paramètres peuvent être transmis à l'action lorsqu'elle est appelée.

1. Utilisez des paramètres dans l'action. Par exemple, mettez à jour le fichier 'hello.js' avec le contenu suivant :

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' de ' + params.place};
  }
  ```
  {: codeblock}

  Les paramètres d'entrée sont transmis sous forme de paramètre d'objet JSON à la fonction `main`. Remarquez la façon dont les paramètres `name` et `place` sont extraits de l'objet `params` dans cet exemple.

2. Mettez à jour l'action `hello` et appelez-la en lui transmettant les valeurs de paramètre `name` et `place`. Exemple :

  ```
  wsk action update hello hello.js
  ```
  {: pre}

  Si vous devez modifier vos paramètres de données d'identification qui ne concernent pas un service, sachez que le fait d'exécuter une commande `action update` avec de nouveaux paramètres aura pour conséquence de retirer des paramètres qui existent mais qui ne sont pas spécifiés dans la commande `action update`. Prenons l'exemple de deux paramètres en plus de `__bx_creds`, avec des clés nommées key1 et key2. Si vous exécutez une commande `action update` avec `-p key1 new-value -p key2 new-value` et que vous omettez le paramètre `__bx_creds`, ce dernier aura disparu après l'exécution de la commande `action update`. Vous devrez alors redéfinir les accès aux données d'identification du service. Il s'agit d'une limitation connue pour laquelle il n'existe pas de solution de contournement.
  {: tip}  

3.  Les paramètres peuvent être indiqués de manière explicite sur la ligne de commande, ou fournis dans un fichier. 

  Pour transmettre des paramètres directement via la ligne de commande, indiquez une paire clé/valeur pour l'indicateur `--param` :
  ```
  wsk action invoke --result hello --param name Bernie --param place Vermont
  ```
  {: pre}

  Pour utiliser un fichier indiquant le contenu des paramètres, créez un fichier contenant les paramètres au format JSON. Le nom de fichier doit être ensuite transmis à l'indicateur `param-file` :

  Exemple de fichier de paramètres nommé `parameters.json` :
  ```json
  {
      "name": "Bernie",
      "place": "Vermont"
  }
  ```

  ```
  wsk action invoke --result hello --param-file parameters.json
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

  Remarquez l'utilisation de l'option `--result` : elle implique un appel bloquant où l'interface de ligne de commande attend la fin de l'activation, puis affiche uniquement le résultat. Pour plus de commodité, cette option peut être utilisée sans `--blocking` qui est automatiquement déduit.

  Par ailleurs, si les valeurs des paramètres spécifiés sur la ligne de commande constituent des structures JSON valides, elles sont analysées et envoyées à votre action en tant qu'objet structuré. Par exemple, mettez à jour l'action hello avec le contenu suivant :

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  L'action s'attend à présent qu'un paramètre `person` comporte les `name` et `place`. Ensuite, appelez l'action avec un seul paramètre `person` doté d'une structure JSON valide, comme illustré ci-dessous :

  ```
  wsk action invoke --result hello -p person '{"name": "Bernie", "place": "Vermont"}'
  ```
  {: pre}

  Le résultat est identique puisque l'interface CLI analyse automatiquement le paramètre `person` en débouchant sur l'objet structuré attendu à présent par l'action :
  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

### Définition de paramètres par défaut
{: #openwhisk_binding_actions}

Les actions peuvent être appelées avec plusieurs paramètres nommés. Souvenez-vous : l'action `hello` de l'exemple précédent attend deux paramètres, le nom d'une personne (*name*) et l'endroit d'où elle vient (*place*).

Plutôt que de transmettre tous les paramètres à une action à chaque fois, vous pouvez lier certains paramètres. L'exemple suivant lie le paramètre *place* pour que l'action prenne par défaut la valeur "Vermont" pour le paramètre place :

1. Mettez à jour l'action avec l'option `--param` pour lier des valeurs de paramètre, ou en transmettant un fichier contenant les paramètres à `--param-file`.

  Pour indiquer les paramètres par défaut de manière explicite sur la ligne de commande, indiquez une paire clé/valeur pour l'indicateur `param` :

  ```
  wsk action update hello --param place Vermont
  ```
  {: pre}

  La transmission des paramètres à partir d'un fichier nécessite la création d'un fichier comportant le contenu souhaité au format JSON.
  Le nom de fichier doit être ensuite transmis à l'indicateur `-param-file` :

  Exemple de fichier de paramètres nommé `parameters.json` :
  ```json
  {
      "place": "Vermont"
  }
  ```
  {: codeblock}

  ```
  wsk action update hello --param-file parameters.json
  ```
  {: pre}

2. Appelez l'action en ne transmettant cette fois que le paramètre `name`.

  ```
  wsk action invoke --result hello --param name Bernie
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

  Notez que vous n'avez pas eu besoin de spécifier le paramètre place lorsque vous avez appelé l'action. Toutefois, vous pouvez remplacer les paramètres liés en spécifiant la valeur de paramètre au moment de l'appel.

3. Appelez l'action en transmettant les valeurs `name` et `place`. Cette dernière remplace la valeur liée à l'action.

  Utilisation de l'indicateur `--param` :

  ```
  wsk action invoke --result hello --param name Bernie --param place "Washington, DC"
  ```
  {: pre}

  Utilisation de l'indicateur `--param-file` :

  Fichier parameters.json :
  ```json
  {
    "name": "Bernie",
    "place": "Vermont"
  }
  ```
  {: codeblock}
  ```
  wsk action invoke --result hello --param-file parameters.json
  ```
  {: pre}

  ```json
  {  
      "payload": "Hello, Bernie from Washington, DC"
  }
  ```

### Obtention d'une adresse URL d'action

Une action peut être appelée en utilisant l'interface REST via une demande HTTPS. Pour obtenir une adresse URL d'action, exécutez la commande suivante :

```
wsk action get actionName --url
```
{: pre}

```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```

Une authentification doit être fournie lors de l'appel d'une action via une demande HTTPS. Pour plus d'informations concernant les appels d'action à l'aide de l'interface REST, voir [Utilisation des API REST avec OpenWhisk](./openwhisk_rest_api.html#openwhisk_rest_api).
{: tip}

### Sauvegarde de code d'action

Le code associé à une action existante est extrait et sauvegardé en local. La sauvegarde est effectuée sur toutes les actions, à l'exception des séquences et des actions Docker. Lorsqu'un code d'action est sauvegardé dans un fichier, le code est sauvegardé dans le répertoire de travail en cours et le chemin du fichier sauvegardé s'affiche. 

1. Sauvegardez un code d'action dans un nom de fichier qui correspond à un nom d'action existant. Une extension de fichier correspondant au type d'action est utilisée ou une extension de type `.zip` sera utilisée pour un code d'action qui correspond à un fichier zip. 
  ```
  wsk action get actionName --save
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```

2. Au lieu d'autoriser l'interface de ligne de commande à déterminer le nom de fichier et l'extension du code sauvegardé, un nom de fichier et une extension personnalisés peuvent être fournis à l'aide de l'indicateur `--save-as`. 
  ```
  wsk action get actionName --save-as codeFile.js
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```

### Création d'actions asynchrones
{: #openwhisk_asynchrony_js}

Les fonctions JavaScript qui s'exécutent de manière asynchrone peuvent renvoyer le résultat d'activation après le retour de la fonction `main` en renvoyant une promesse (objet Promise) dans votre action. 

1. Sauvegardez le contenu ci-dessous dans un fichier appelé `asyncAction.js`.

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

  Notez que la fonction `main` renvoie une promesse (objet Promise), qui indique que l'activation n'est pas encore terminée, mais que cela est prévu.

  Dans ce cas, la fonction JavaScript `setTimeout()` attend deux secondes avant d'appeler la fonction de rappel, qui représente le code asynchrone, et s'insère dans la fonction de rappel de la promesse (objet Promise).  

  La fonction de rappel de la promesse (objet Promise) prend deux arguments, resolve et reject, qui sont tous les deux des fonctions.  L'appel vers `resolve()` satisfait la promesse (objet Promise) et indique que l'activation aboutit. 

  Un appel vers `reject()` peut être utilisé pour rejeter la promesse (objet Promise) et signaler que l'activation ne s'est pas terminée normalement.

2. Exécutez les commandes suivantes pour créer l'action et l'appeler :

  ```
  wsk action create asyncAction asyncAction.js
  ```
  {: pre}

  ```
  wsk action invoke --result asyncAction
  ```
  {: pre}

  ```json
  {
      "done": true
  }
  ```

  Notez que vous avez effectué un appel bloquant d'une action asynchrone.

3. Procédez à l'extraction du journal d'activation pour savoir combien de temps a duré l'activation :

  ```
  wsk activation list --limit 1 asyncAction
  ```
  {: pre}
  
  ```
  activations
  b066ca51e68c4d3382df2d8033265db0             asyncAction
  ```

  ```
  wsk activation get b066ca51e68c4d3382df2d8033265db0
  ```
  {: pre}
 
  ```json
  {
      "start": 1455881628103,
      "end":   1455881648126,
      ...
  }
  ```

  Prenez connaissance des horodatages de `start` et de `end` dans l'enregistrement d'activation : vous constatez que cette activation a pris un peu plus de 2 secondes pour s'exécuter.

### Utilisation d'actions pour appeler une API externe
{: #openwhisk_apicall_action}

Jusqu'à présent, les exemples sont des fonctions JavaScript autonomes. Vous pouvez également créer une action qui appelle une API externe.

Cet exemple appelle un service météorologique Yahoo afin de prendre connaissance des conditions en cours à un endroit spécifique.

1. Sauvegardez le contenu ci-dessous dans un fichier appelé `weather.js`.

  ```javascript
  var request = require('request');

  function main(params) {
      var location = params.location || 'Vermont';
      var url = 'https://query.yahooapis.com/v1/public/yql?q=select item.condition from weather.forecast where woeid in (select woeid from geo.places(1) where text="' + location + '")&format=json';

      return new Promise(function(resolve, reject) {
          request.get(url, function(error, response, body) {
              if(error) {
                  reject(error);    
                }
                else {
                  var condition = JSON.parse(body).query.results.channel.item.condition;
                    var text = condition.text;
                    var temperature = condition.temp;
                    var output = 'Il fait ' + temperature + ' degrés à ' + location + ', ' + text;
                    resolve({msg: output});
                }
          });
      });
  }
  ```
  {: codeblock}

 L'action dans l'exemple utilise la bibliothèque JavaScript `request` pour envoyer une demande HTTP à l'API Yahoo Weather, et extrait des zones du résultat JSON. La section [Références](./openwhisk_reference.html#openwhisk_ref_javascript_environments) présente en détail les packages Node.js que vous pouvez utiliser dans vos actions.

  Cet exemple illustre également la nécessité d'actions asynchrones. L'action renvoie une promesse (objet Promise) pour indiquer que le résultat de cette action n'est pas encore disponible au retour de la fonction. A la place, le résultat est disponible dans le rappel `request` une fois l'appel HTTP terminé, et est transmis sous forme d'argument à la fonction `resolve()`.

2. Exécutez les commandes suivantes pour créer l'action et l'appeler :

  ```
  wsk action create weather weather.js
  ```
  {: pre}

  ```
  wsk action invoke --result weather --param location "Brooklyn, NY"
  ```
  {: pre}

  ```json
  {
      "msg": "It is 28 degrees in Brooklyn, NY and Cloudy"
  }
  ```

### Conditionnement d'une action dans un module Node.js
{: #openwhisk_js_packaged_action}

Comme alternative à l'écriture de l'ensemble du code de votre action dans un seul fichier source JavaScript, vous pouvez écrire une action en tant que package `npm`. Prenez par exemple un répertoire contenant les fichiers suivants :

D'abord, `package.json` :

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

Ensuite, `index.js` :

```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}

exports.main = myAction;
```
{: codeblock}

L'action est exposée via `exports.main`. Le gestionnaire d'action proprement dit peut avoir n'importe quel nom, tant que celui-ci est conforme à la signature habituelle pour l'acceptation et le renvoi d'un objet (ou d'une promesse d'objet (`Promise`)). Conformément à la convention Node.js, vous devez soit nommer ce fichier `index.js`, soit spécifier le nom de fichier de votre choix via la propriété `main` dans package.json.

Pour créer une action OpenWhisk depuis ce package :

1. Installez d'abord toutes les dépendances localement :

  ```
  npm install
  ```
  {: pre}

2. Créez une archive `.zip` contenant tous les fichiers (notamment toutes les dépendances) :

  ```
  zip -r action.zip *
  ```
  {: pre}

  L'utilisation d'une action Windows Explorer pour créer le fichier zip génère une structure incorrecte. Les actions zip d'OpenWhisk nécessitent que `package.json` soit placé à la racine du fichier zip, alors que Windows Explorer le place dans un dossier imbriqué. L'option la plus sûre consiste à utiliser la commande `zip` de ligne de commande.
  {: tip}

3. Créez l'action :

  ```
  wsk action create packageAction --kind nodejs:6 action.zip
  ```
  {: pre}

  Lorsque vous créez une action depuis une archive `.zip` via l'outil d'interface de ligne de commande, vous devez fournir explicitement une valeur pour l'indicateur `--kind` à l'aide de `nodejs:6` ou `nodejs:8`.

4. Vous pouvez appeler l'action à l'instar de n'importe quelle autre action :

  ```
  wsk action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
  ```
  {: pre}
  
  ```json
  {
      "padded": [
          ".......................and now",
          "......for something completely",
          ".....................different"
      ]
  }
  ```

Enfin, notez qu'alors que la plupart des packages `npm` installent des sources JavaScript avec la commande `npm install`, certains installent et compilent également des artefacts binaires. Actuellement, le téléchargement de fichier archive ne prend pas en charge les dépendances binaires et n'admet que les dépendances JavaScript. Par conséquent, il se peut que des appels d'action échouent si l'archive inclut des dépendances binaires.

### Conditionnement d'une action dans un bundle unique
{: #openwhisk_js_webpack_action}

Il est pratique d'inclure uniquement le code minimal dans un seul fichier `.js` qui inclut les dépendances. Cette approche permet d'accélérer les déploiements, et dans certaines circonstances lorsque le conditionnement de l'action en tant que zip peut être trop volumineux car il inclut des fichiers inutiles.

Vous pouvez utiliser un programme de mise en bundle de module JavaScript, tel que [webpack](https://webpack.js.org/concepts/). Lorsque webpack traite votre code, il génère de manière récursive un graphique de dépendance incluant chaque module dont votre action a besoin. 

Voici un exemple rapide d'utilisation de webpack :

En prenant l'exemple `package.json` précédent, ajoutez `webpack` en tant que dépendance de développement et ajoutez des commandes de script npm. 
```json
{
  "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
    "build": "webpack --config webpack.config.js",
    "deploy": "bx wsk action update my-action dist/bundle.js --kind nodejs:8"
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

Créez le fichier de configuration de webpack `webpack.config.js`.
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

Affectez à la variable `global.main` la fonction main de l'action.
A partir de l'exemple précédent :
```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}
global.main = myAction;
```
{: codeblock}

Si votre nom de fonction est `main`, utilisez plutôt cette syntaxe :
```javascript
global.main = main;
```
{: codeblock}


Pour générer et déployer une action OpenWhisk à l'aide de `npm` et `webpack` :

1. Commencez par installer les dépendances en local :

  ```
  npm install
  ```
  {: pre}

2. Générez le bundle webpack :

  ```
  npm run build
  ```
  {: pre}

  Le fichier `dist/bundle.js` est créé et utilisé pour un déploiement en tant que ce code source d'action. 

3. Créez l'action à l'aide du script `npm` ou de l'interface de ligne de commande.
  Utilisation du script `npm` :
  ```
  npm run deploy
  ```
  {: pre}
  Utilisation de l'interface de ligne de commande :
  ```
  bx wsk action update my-action dist/bundle.js
  ```
  {: pre}


Enfin, le fichier bundle qui est généré par `webpack` ne prend pas en charge des dépendances binaires mais plutôt des dépendances JavaScript. Par conséquent, les appels d'action échouent si le bundle dépend de dépendances binaires, car cela n'est pas inclus avec le fichier `bundle.js`.

## Création de séquences d'actions
{: #openwhisk_create_action_sequence}

Vous pouvez créer une action qui assemble des actions dans une séquence.

Plusieurs actions d'utilitaire sont fournies dans un package appelé `/whisk.system/utils`, que vous pouvez utiliser pour créer votre première séquence. Pour en savoir plus sur les packages, voir la section [Packages](./openwhisk_packages.html).

1. Affichez les actions du package `/whisk.system/utils`.

  ```
  wsk package get --summary /whisk.system/utils
  ```
  {: pre}

  ```
  package /whisk.system/utils: Building blocks that format and assemble data
   action /whisk.system/utils/head: Extract prefix of an array
   action /whisk.system/utils/split: Split a string into an array
   action /whisk.system/utils/sort: Sorts an array
   action /whisk.system/utils/echo: Returns the input
   action /whisk.system/utils/date: Current date and time
   action /whisk.system/utils/cat: Concatenates input into a string
  ```

  Vous utilisez dans cet exemple les actions `split` et `sort`.

2. Créez une séquence d'actions pour que le résultat d'une action soit transmis sous forme d'argument à l'action suivante.

  ```
  wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

  Cette séquence d'actions convertit des lignes de texte en tableau et trie les lignes.

3. Appelez l'action :

  ```
  wsk action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
  ```
  {: pre}

  ```json
  {
      "length": 3,
      "lines": [
          "Est plein de regrets.,
          "Le maître",
          "Sushis périmés,"
      ]
  }
  ```

  Vous constatez que les lignes sont triées dans le résultat.

**Remarque** : les paramètres transmis entre les actions de la séquence sont explicites, sauf les paramètres par défaut.
Par conséquent, les paramètres qui sont transmis à la séquence d'actions ne sont disponibles que pour la première action de la séquence.
Le résultat de la première action dans la séquence devient l'objet JSON d'entrée de la deuxième action de la séquence (et ainsi de suite).
Cet objet ne comporte pas les paramètres qui sont transmis initialement à la séquence sauf si la première action les inclut explicitement dans son résultat.
Les paramètres d'entrée d'une action sont fusionnés avec ses paramètres par défaut, les premiers étant prioritaires et remplaçant tout paramètre par défaut correspondant.
Pour plus d'informations sur l'appel de séquences d'actions avec plusieurs paramètres nommés, voir [Définition de paramètres par défaut](./openwhisk_actions.html#openwhisk_binding_actions).

## Création d'actions Python
{: #creating-python-actions}

Le processus de création d'actions Python est similaire au processus de création d'actions JavaScript. Les sections ci-après vous expliquent comment créer et appeler une action Python unique, et comment ajouter des paramètres à cette action.

### Création et appel d'une action Python
{: #openwhisk_actions_python_invoke}

Une action est simplement une fonction Python de premier niveau. Par exemple, créez un fichier nommé `hello.py` contenant le code source suivant :

```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
    print(greeting)
        return {"greeting": greeting}
```
{: codeblock}

Les actions Python consomment et produisent toujours un dictionnaire. Par défaut, la méthode d'entrée pour l'action est `main`, mais elle peut être spécifiée explicitement pour créer l'action avec l'interface CLI `wsk` en utilisant `--main`, comme pour n'importe quel type d'action.

Vous pouvez créer une action OpenWhisk appelée `helloPython` depuis cette fonction comme suit :
```
wsk action create helloPython hello.py
```
{: pre}

L'interface CLI infère automatiquement le type d'action d'après l'extension du fichier source. Pour les fichiers source `.py`, l'action est lancée dans un environnement d'exécution Python 2. Vous pouvez également créer une action opérant avec Python 3 en mentionnant explicitement le paramètre `--kind python:3`. Il existe également un environnement d'exécution Python 3 avec le type `python-jessie:3` qui contient des packages supplémentaires pour les services IBM Cloud, telles qu'IBM Cloudant, IBM DB2, IBM COS et IBM Watson.
Pour plus d'informations sur les packages inclus dans cet environnement d'exécution Python 3, voir la section de [référence](./openwhisk_reference.html#openwhisk_ref_python_environments) de l'environnement d'exécution Python.

L'appel d'action est identique pour les actions Python et pour les actions JavaScript :
```
wsk action invoke --result helloPython --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

### Conditionnement des actions Python dans des fichiers zip
{: #openwhisk_actions_python_zip}

Vous pouvez conditionner une action Python et les modules dépendants dans un fichier zip.
Le nom du fichier source contenant le point d'entrée (par exemple, `main`) doit être `__main__.py`.
Par exemple, pour créer une action avec un module auxiliaire appelé `helper.py`, créez d'abord une archive contenant vos fichiers source :

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}

Créez ensuite l'action :

```bash
wsk action create helloPython --kind python:3 helloPython.zip
```
{: pre}

Ces étapes s'appliquent à Python 3 (avec le type `python:3`), mais vous pouvez faire de même avec les autres types Python `python:2` ou `python-jessie:3`.


### Conditionnement d'actions Python avec un environnement virtuel dans des fichiers zip
{: #openwhisk_actions_python_virtualenv}

L'autre moyen de conditionner des dépendances Python consiste à utiliser un environnement virtuel (`virtualenv`), ce qui vous permet de lier des packages supplémentaires pouvant être installés via [`pip`](https://packaging.python.org/installing/), par exemple. 


Comme avec la prise en charge de fichier zip de base, le nom du fichier source qui contient le point d'entrée principal doit être `__main__.py`. Plus précisément, le contenu de `__main__.py` est la fonction main, par conséquent, pour cet exemple, vous pouvez renommer `hello.py` `__main__.py` à partir de la section précédente. De plus, le répertoire virtualenv doit s'appeler `virtualenv`. Vous trouverez ci-dessous un exemple de scénario d'installation des dépendances, de conditionnement de ces dernières dans un environnement virtuel et de création d'une action OpenWhisk compatible.

Pour assurer la compatibilité avec le conteneur d'exécution OpenWhisk, les installations de package dans un environnement virtuel (virtualenv) doivent être effectuées dans l'environnement cible en utilisant l'image correspondante pour le type.

- Pour le type `python:2`, utilisez l'image Docker `openwhisk/python2action`.
- Pour le type `python:3`, utilisez l'image Docker `openwhisk/python3action`.
- Pour le type `python-jessie:3`, utilisez l'image Docker `ibmfunctions/action-python-v3`.

1. Avec un fichier [requirements.txt ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) contenant les versions et les modules `pip` à installer, exécutez les commandes suivantes pour installer les dépendances et créer un environnement virtuel à l'aide d'une image Docker compatible :
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
    ```
    {: pre}

2. Archivez le répertoire virtualenv ainsi que les éventuels fichiers Python supplémentaires :
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

3. Créez l'action :
    ```
    wsk action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}


## Création d'actions PHP
{: #creating-php-actions}

Le processus de création d'actions PHP est similaire au processus de création d'actions JavaScript. Les sections ci-après expliquent comment créer et appeler une action Java PHP unique, et comment ajouter des paramètres à cette action.

### Création et appel d'une action PHP
{: #openwhisk_actions_php_invoke}

Une action est simplement une fonction PHP de premier niveau. Par exemple, créez un fichier nommé `hello.php` contenant le code source suivant :

```php
<?php
function main(array $args) : array
{
    $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
```

Les actions PHP consomment et renvoient toujours un tableau associatif. Par défaut, la méthode d'entrée pour l'action est `main`, mais elle peut être spécifiée explicitement lorsque vous créez l'action avec l'interface de ligne de commande `wsk` en utilisant `--main`, comme pour n'importe quel autre type d'action.

Vous pouvez créer une action OpenWhisk nommée `helloPHP` depuis cette fonction en procédant comme suit :

```
wsk action create helloPHP hello.php
```
{: pre}

L'interface CLI infère automatiquement le type d'action d'après l'extension du fichier source. Pour les fichiers source `.php`, l'action est lancée dans un environnement d'exécution PHP 7.1. Pour plus d'informations, voir le [document de référence](./openwhisk_reference.html#openwhisk_ref_php) au sujet de PHP.

L'appel d'action est identique pour les actions PHP et pour les actions JavaScript :

```
wsk action invoke --result helloPHP --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

### Conditionnement des actions PHP dans des fichiers zip
{: #openwhisk_actions_php_zip}

Vous pouvez créer un package pour une action PHP, ainsi que pour d'autres fichiers et packages dépendants, dans un fichier zip.
Le nom du fichier source contenant le point d'entrée (par exemple, `main`) doit être `index.php`.
Par exemple, pour créer une action qui inclut un second fichier nommé `helper.php`, créez d'abord une archive contenant vos fichiers source :

```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

Créez ensuite l'action :

```bash
wsk action create helloPHP --kind php:7.1 helloPHP.zip
```
{: pre}

## Création d'actions Swift
{: #creating-swift-actions}

Le processus de création d'actions Swift est similaire au processus de création d'actions JavaScript. Les sections ci-après expliquent comment créer et appeler une action Swift unique, et comment ajouter des paramètres à cette action.

Vous pouvez aussi utiliser le [bac à sable Swift](https://swiftlang.ng.bluemix.net) en ligne pour tester votre code Swift sans avoir à installer Xcode sur votre machine.

### Création et appel d'une action

Une action est simplement une fonction Swift de niveau supérieur. Par exemple, créez un fichier appelé
`hello.swift` avec le contenu suivant :

```swift
func main(args: [String:Any]) -> [String:Any] {
    if let name = args["name"] as? String {
        return [ "greeting" : "Hello \(name)!" ]
    } else {
        return [ "greeting" : "Hello stranger!" ]
    }
}
```
{: codeblock}

Les actions Swift consomment et produisent toujours un dictionnaire. 

Vous pouvez créer une action {{site.data.keyword.openwhisk_short}} appelée `helloSwift` depuis cette fonction comme
suit :

```
wsk action create helloSwift hello.swift --kind swift:3.1.1
```
{: pre}
 

Spécifiez toujours `swift:3.1.1` car les versions Swift précédentes ne sont pas prises en charge.
{: tip}

L'appel d'action est identique pour les actions Swift et pour les actions JavaScript :

```
wsk action invoke --result helloSwift --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

**Attention :** les actions Swift qui sont exécutées dans un environnement Linux sont encore en cours de développement, et {{site.data.keyword.openwhisk_short}} utilise généralement l'édition disponible la plus récente, qui n'est pas nécessairement stable. De plus, il se peut que la version de Swift qui est utilisée avec {{site.data.keyword.openwhisk_short}} ne corresponde pas aux versions de Swift provenant d'éditions stables de Xcode sous MacOS.

### Conditionnement d'une action en tant qu'exécutable Swift
{: #openwhisk_actions_swift_zip}

Lorsque vous créez une action Swift OpenWhisk avec un fichier source Swift, celui-ci doit être compilé en fichier binaire avant l'exécution de l'action. Après quoi, les appels ultérieurs de l'action sont beaucoup plus rapides jusqu'à ce que le conteneur qui héberge votre action soit purgé. Ce délai est dénommé délai de démarrage à froid.

Pour éviter ce délai, vous pouvez compiler votre fichier Swift en binaire, puis le télécharger dans OpenWhisk sous forme de fichier zip. Comme vous avez besoin de l'échafaudage OpenWhisk, la manière la plus facile de créer le binaire consiste à le générer dans le même environnement que celui où il sera exécuté. Procédez comme suit :


- Exécutez un conteneur d'actions Swift interactif à l'aide de la commande suivante :
  ```
  docker run --rm -it -v "$(pwd):/owexec" openwhisk/action-swift-v3.1.1 bash
  ```
  {: pre}
  
- Copiez le code source et préparez sa génération.
  ```
  cp /owexec/hello.swift /swift3Action/spm-build/main.swift 
  ```
  {: pre}

  ```
  cat /swift3Action/epilogue.swift >> /swift3Action/spm-build/main.swift
  ```
  {: pre}

  ```
  echo '_run_main(mainFunction:main)' >> /swift3Action/spm-build/main.swift
  ```
  {: pre}

- (Facultatif) Créez le fichier `Package.swift` afin d'ajouter des dépendances.
   ```
   swift import PackageDescription
   
   let package = Package(
     name: "Action",
         dependencies: [
             .Package(url: "https://github.com/apple/example-package-deckofplayingcards.git", majorVersion: 3),
             .Package(url: "https://github.com/IBM-Swift/CCurl.git", "0.2.3"),
             .Package(url: "https://github.com/IBM-Swift/Kitura-net.git", "1.7.10"),
             .Package(url: "https://github.com/IBM-Swift/SwiftyJSON.git", "15.0.1"),
             .Package(url: "https://github.com/watson-developer-cloud/swift-sdk.git", "0.16.0")
         ]
   )
   ```
   {: pre}

  Cet exemple ajoute les dépendances `swift-watson-sdk` et `example-package-deckofplayingcards`.
  Notez que les dépendances `CCurl`, `Kitura-net` et `SwiftyJSON` sont fournies dans l'action Swift standard et que vous pouvez les inclure dans votre propre fichier `Package.swift`.

- Copiez le fichier Package.swift dans le répertoire spm-build
  ```
  cp /owexec/Package.swift /swift3Action/spm-build/Package.swift
  ```
  {: pre}

- Basculez vers le répertoire spm-build
  ```
  cd /swift3Action/spm-build
  ```
  {: pre}

- Compilez votre action Swift.
  ```
  swift build -c release
  ```
  {: pre}

- Créez l'archive zip.
  ```
  zip /owexec/hello.zip .build/release/Action
  ```
  {: pre}

- Quittez le conteneur Docker.
  ```
  exit
  ```
  {: pre}

Comme vous pouvez le voir, hello.zip a été créé dans le même répertoire que hello.swift. 

- Téléchargez-le dans OpenWhisk avec le nom d'action helloSwifty :
  ```
  wsk action update helloSwiftly hello.zip --kind swift:3.1.1
  ```
  {: pre}

- Pour vous convaincre du gain en rapidité, exécutez 
  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

Le temps qui a été nécessaire à l'exécution de l'action figure dans la propriété "duration" et vous pouvez le comparer avec le temps qui est nécessaire à l'exécution via une étape de compilation dans l'action hello.

## Création d'actions Java
{: #creating-java-actions}

Le processus de création d'actions Java est similaire au processus de création d'actions JavaScript et Swift. Les sections ci-après expliquent comment créer et appeler une action Java unique, et comment ajouter des paramètres à cette action.

Pour pouvoir compiler, tester et archiver des fichiers Java, vous devez avoir installé un logiciel [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) en local. 

### Création et appel d'une action
{: #openwhisk_actions_java_invoke}

Une action Java est un programme Java comportant une méthode appelée `main` qui possède l'exacte signature suivante :
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Par exemple, créez un fichier Java appelé `Hello.java` avec le contenu suivant :

```java
import com.google.gson.JsonObject;
public class Hello {
    public static JsonObject main(JsonObject args) {
        String name = "stranger";
        if (args.has("name"))
            name = args.getAsJsonPrimitive("name").getAsString();
        JsonObject response = new JsonObject();
        response.addProperty("greeting", "Hello " + name + "!");
        return response;
    }
}
```
{: codeblock}

Ensuite, compilez `Hello.java` dans un fichier JAR appelé `hello.jar` comme suit :
```
javac Hello.java
```
{: pre}

```
jar cvf hello.jar Hello.class
```
{: pre}

[google-gson](https://github.com/google/gson) doit exister dans votre variable Java CLASSPATH pour vous permettre de compiler le fichier Java.
{: tip}

Vous pouvez créer une action OpenWhisk appelée `helloJava` depuis ce fichier JAR comme
suit :

```
wsk action create helloJava hello.jar --main Hello
```
{: pre}

Lorsque vous utilisez la ligne de commande et un fichier source `.jar`, il n'est pas nécessaire
de spécifier que vous créez une action Java ;
l'outil le détermine à partir de l'extension de fichier.

Vous devez spécifier le nom de la classe principale à l'aide de `--main`. Une classe principale est
éligible si elle implémente une méthode `main` statique. Si la
classe ne se trouve pas dans le package par défaut, utilisez le nom de classe qualifié complet Java,
par exemple, `--main com.example.MyMain`.

Si besoin, vous pouvez également personnaliser le nom de méthode de votre action Java. Pour cela, spécifiez le nom de méthode qualifié complet Java de votre action, par exemple, `--main com.example.MyMain#methodName`.

L'appel d'action est identique pour les actions Java et les actions Swift et JavaScript :

```
wsk action invoke --result helloJava --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

## Création d'actions Docker
{: #creating-docker-actions}

Avec des actions Docker {{site.data.keyword.openwhisk_short}}, vous pouvez écrire vos actions dans n'importe quel langage.

Votre code est compilé dans un fichier binaire exécutable et imbriqué dans une image Docker. Le programme binaire interagit avec le système en prenant l'entrée dans `stdin` et en répondant par le biais de `stdout`.

Au préalable, vous devez disposer d'un compte Docker Hub.  Pour configurer un ID et un compte Docker gratuits, visitez le site [Docker Hub](https://hub.docker.com).

Dans les instructions qui suivent, l'ID utilisateur Docker est `janesmith` et le mot de passe est `janes_password`. Si l'on part du principe que l'interface de ligne de commande est configurée, il reste trois étapes à effectuer pour configurer un fichier binaire personnalisé pouvant être utilisé par {{site.data.keyword.openwhisk_short}}. Ensuite, l'image Docker téléchargée peut être utilisée en tant qu'action.

1. Téléchargez le squelette Docker. Vous pouvez le télécharger et l'installer via l'interface de ligne de commande en procédant comme suit :

  ```
  wsk sdk install docker
  ```
  {: pre}

  A présent, le squelette Docker est installé dans le répertoire en cours.
  
  ```
  ls dockerSkeleton/
  ```
  {: pre}

  ```
  Dockerfile      README.md       buildAndPush.sh example.c
  ```

  Le squelette est un modèle de conteneur Docker dans lequel vous pouvez injecter votre code sous la forme de fichiers binaires personnalisés.

2. Configurez votre fichier binaire personnalisé dans le squelette Docker. Le squelette inclut déjà un programme C que vous pouvez utiliser.

  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

  Vous pouvez modifier ce fichier en fonction de vos besoins ou ajouter un code et des dépendances supplémentaires à l'image Docker.
  Afin d'ajouter
un code et des dépendances supplémentaires, vous pouvez le cas échéant personnaliser le document `Dockerfile` pour générer votre fichier exécutable.
  Le fichier binaire doit se trouver dans le conteneur dans le répertoire `/action/exec`.

  Le fichier exécutable reçoit un argument simple depuis la ligne de commande. Il s'agit d'une sérialisation
de chaîne de l'objet JSON représentant les arguments de l'action.
Le programme peut effectuer la journalisation dans `stdout` ou `stderr`.
  Par convention, la
dernière ligne de la sortie _doit_ être un objet JSON transformé en chaîne qui représente le résultat de l'action.

3. Générez l'image Docker et téléchargez-la à l'aide d'un script fourni. Vous devez d'abord exécuter `docker
login` pour l'authentification, puis exécuter le script avec le nom d'image de votre choix.

  ```
  docker login -u janesmith -p janes_password
  ```
  {: pre}

  ```
  cd dockerSkeleton
  ```
  {: pre}

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  Notez qu'une partie du fichier `example.c` est compilée dans le cadre du processus de génération de l'image Docker ; par conséquent, il n'est pas
nécessaire que C soit compilé sur votre machine.
  En fait, sauf si vous le compilez sur une machine hôte compatible, le fichier binaire ne pourra pas s'exécuter dans le conteneur car les
formats ne concorderont pas.

  Votre conteneur Docker peut désormais être utilisé en tant qu'action OpenWhisk.


  ```
  wsk action create example --docker janesmith/blackboxdemo
  ```
  {: pre}

  Vous remarquerez l'utilisation de `--docker` pour créer une action. On suppose que toutes les images Docker sont hébergées dans Docker Hub.
  L'action peut être appelée comme n'importe quelle autre action {{site.data.keyword.openwhisk_short}}. 

  ```
  wsk action invoke --result example --param payload Rey
  ```
  {: pre}

  ```json
  {
      "args": {
          "payload": "Rey"
      },
      "msg": "Bonjour du programme C arbitraire !"
  }
  ```

  Afin de mettre à jour l'action Docker, exécutez `buildAndPush.sh` pour télécharger l'image la plus récente dans le dockerhub. Ainsi, le système
pourra extraire votre nouvelle image Docker à la prochaine exécution du code pour votre action.
  Si aucun conteneur n'est en cours d'exécution, les nouveaux appels utilisent la nouvelle image Docker.
  Toutefois, s'il existe un conteneur en cours d'exécution utilisant une version précédente de votre image Docker, les nouveaux appels continuent d'utiliser cette image sauf si vous exécutez `wsk action update`. Cette commande indique au système que pour les nouveaux appels, une commande docker pull doit être exécutée afin d'obtenir votre nouvelle image Docker.

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  ```
  wsk action update example --docker janesmith/blackboxdemo
  ```
  {: pre}

  D'autres informations sur la création d'actions Docker sont disponibles dans la section
[Références](./openwhisk_reference.html#openwhisk_ref_docker).

  La version antérieure de l'interface de ligne de commande prenait en charge l'option `--docker` sans paramètre et le nom d'image était un argument de position.
  Pour permettre aux actions Docker d'accepter des données d'initialisation vis un fichier (zip), normalisez l'expérience utilisateur pour les actions Docker de telle sorte que, si un argument de position est présent, il doit s'agir d'un fichier (par exemple, un fichier zip) à la place. Le nom de l'image doit être spécifié à la suite de l'option `--docker`. Suite aux commentaires en retour des utilisateurs, l'argument `--native` a été inclus comme abréviation de `--docker openwhisk/dockerskeleton` afin que les exécutables qui s'exécutent dans le SDK d'action Docker standard soient plus faciles à créer et à déployer.
  
  Par exemple, ce tutoriel crée un exécutable binaire dans le conteneur situé sous `/action/exec`. Si vous copiez ce fichier sur votre système de fichiers local et que vous le
compressez dans un fichier `exec.zip`, vous pouvez alors utiliser les commandes suivantes pour créer une action docker qui reçoit l'exécutable en tant que données d'initialisation. 

  ```
  wsk action create example exec.zip --native
  ```
  {: pre}

  Qui est équivalente à la commande suivante :
   
  ```
  wsk action create example exec.zip --docker openwhisk/dockerskeleton
  ```
  {: pre}

## Création d'actions à l'aide d'exécutables arbitraires
{: #creating-actions-arbitrary}

Comme vous pouvez le voir, avec l'option `--native`, n'importe quel exécutable peut être exécuté en tant qu'action OpenWhisk. Cela inclut les scripts `bash` ou les binaires ayant fait l'objet d'une compilation croisée. Pour ces derniers, il existe une contrainte liée au fait que les binaires doivent être compatibles avec l'image `openwhisk/dockerskeleton`. 

## Création d'actions Go
{: #creating-go-actions}

L'option `--native` permet de conditionner n'importe quel exécutable en tant qu'action. Cela fonctionne pour Go, par exemple.
A l'instar des actions Docker, l'exécutable Go reçoit un argument unique de la part de la ligne de commande.
Il s'agit d'une sérialisation
de chaîne de l'objet JSON représentant les arguments de l'action.
Le programme peut effectuer la journalisation dans `stdout` ou `stderr`.
Par convention, la
dernière ligne de la sortie _doit_ être un objet JSON transformé en chaîne qui représente le résultat de l'action.

Voici un exemple d'action Go :
```go
package main

import "encoding/json"
import "fmt"
import "os"

func main() {
    //program receives one argument: the JSON object as a string
    arg := os.Args[1]
   
    // unmarshal the string to a JSON object
    var obj map[string]interface{}
    json.Unmarshal([]byte(arg), &obj)

    // can optionally log to stdout (or stderr)
    fmt.Println("hello Go action")

    name, ok := obj["name"].(string)
    if !ok { name = "Stranger" }

    // last line of stdout is the result JSON object as a string
    msg := map[string]string{"msg": ("Hello, " + name + "!")}
    res, _ := json.Marshal(msg)
    fmt.Println(string(res))
}
```

Sauvegardez le code ci-dessus dans un fichier `sample.go` et soumettez-le à une compilation croisée pour OpenWhisk. L'exécutable doit s'appeler `exec`.
```bash
GOOS=linux GOARCH=amd64 go build -o exec
zip exec.zip exec
wsk action create helloGo --native exec.zip
```

L'action peut être exécutée comme n'importe quelle autre action. 
```bash
wsk action invoke helloGo -r -p name gopher
{
    "msg": "Hello, gopher!"
}
```

Les journaux sont eux aussi extraits de manière similaire. 
```bash
wsk activation logs --last --strip
my first Go action.
```

## Surveillance de la sortie des actions
{: #watching-action-output}

Des actions {{site.data.keyword.openwhisk_short}} peuvent être appelées par d'autres utilisateurs en réponse à divers événements, ou dans
le cadre d'une séquence d'actions. Dans ces cas, il peut être utile de surveiller les appels.

Vous pouvez utiliser l'interface de ligne de commande {{site.data.keyword.openwhisk_short}} pour surveiller la sortie des actions, au fur et
à mesure qu'elles sont appelées.

1. Emettez la commande suivante depuis un interpréteur de commandes :
  ```
  wsk activation poll
  ```
  {: pre}

  Cette commande démarre une boucle d'interrogation qui consulte en permanence les journaux des activations.

2. Passez dans une autre fenêtre et appelez une action :

  ```
  wsk action invoke /whisk.system/samples/helloWorld --param payload Bob
  ```
  {: pre}

  ```
  ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
  ```

3. Observez le journal d'activation dans la fenêtre d'interrogation :

  ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
  ```

  De même, chaque fois que vous exécutez l'utilitaire d'interrogation (poll), vous pouvez consulter en temps réel les journaux de n'importe quelle action qui est exécutée pour votre compte dans OpenWhisk.

## Prise en charge des applications volumineuses
{: #large-app-support}

La taille de code maximale pour l'action est 48 Mo. Les applications contenant un grand nombre de modules tiers, de bibliothèques natives ou d'outils externes peuvent être exécutées dans le respect de cette limite. 

Si vous créez une action de conditionnement (zip ou jar) de plus de 48 Mo, la solution consiste à étendre l'image d'exécution avec des dépendances, puis d'utiliser un seul fichier source ou une archive de moins de 48 Mo. 

Par exemple, si vous créez un environnement d'exécution Docker personnalisé, incluant les bibliothèques partagées nécessaires, ces dépendances ne sont pas tenues d'être présentes dans le fichier archive. Des fichiers source privés peuvent toujours être intégrés dans l'archive et injectés lors de l'exécution. 

La réduction de la taille du fichier archive présente l'autre avantage de diminuer les temps de déploiement. 

### Exemple Python

Dans l'exemple Python suivant, opencv peut inclure la bibliothèque `opencv-python`, puis installer le binaire opencv dans l'image du système d'exploitation. Vous pouvez ensuite utiliser `requirements.txt` et exécuter `pip install requirements.txt` pour augmenter l'image avec davantage de bibliothèques Python. Vous pouvez ensuite utiliser `action.py` avec la nouvelle image. 

### Exemple Node.js

Dans l'exemple Node.js suivant, vous pouvez installer des packages supplémentaires sur l'image du système d'exploitation :

Installez opencv à l'aide de `npm` :
```
npm install opencv
```
{: pre}

De même, si vous avez un package `package.json`, installez-le à l'aide de `npm`:
```
npm install package.json
```
{: pre}

Utilisez ensuite `action.js` avec la nouvelle image. 

## Affichage d'une liste d'actions
{: #listing-actions}

Vous pouvez répertorier toutes les actions que vous avez créées en utilisant la commande suivante :

```
wsk action list
```
{: pre}

A mesure que vous créez d'autres actions, cette liste s'allonge et il peut être opportun de regrouper les actions connexes dans des [packages](./openwhisk_packages.html). Pour filtrer votre liste d'actions de manière à afficher uniquement celles qui appartiennent à un package spécifique, utilisez la syntaxe de commande suivante : 

```
wsk action list [PACKAGE NAME]
```
{: pre}

## Suppression d'actions
{: #deleting-actions}

Vous pouvez procéder à un nettoyage en supprimant les actions que vous ne voulez pas utiliser.

1. Exécutez la commande suivante pour supprimer une action :
  ```
  wsk action delete hello
  ```
  {: pre}

  ```
  ok: deleted hello
  ```

2. Vérifiez que l'action n'apparaît plus dans la liste des actions.
  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  ```

## Accès aux métadonnées d'action dans le corps de l'action
{: #accessing-action-metadata-within-the-action-body}

L'environnement de l'action contient plusieurs propriétés spécifiques à l'action en cours d'exécution.
Elles permettent à l'action de fonctionner à l'aide d'un programme avec des actifs OpenWhisk via l'API REST,
ou de définir une alarme interne lorsque l'action est sur le point d'utiliser la totalité de son budget temps alloué.
Les propriétés sont accessibles via l'environnement système de tous les contextes d'exécution pris en charge : actions Node.js, Python, Swift, Java et Docker lorsque le squelette Docker OpenWhisk est employé.

* `__OW_API_HOST` : hôte d'API pour le déploiement OpenWhisk qui exécute cette action
* `__OW_API_KEY` : clé d'API pour l'objet qui appelle l'action ; peut être une clé d'API restreinte
* `__OW_NAMESPACE` : espace de nom pour l'_activation_ (peut être différent de l'espace de nom pour l'action)
* `__OW_ACTION_NAME` : nom qualifié complet de l'action en cours d'exécution
* `__OW_ACTIVATION_ID` : ID d'activation pour cette instance d'action en cours d'exécution
* `__OW_DEADLINE` : délai approximatif au terme duquel cette action aura consommé la totalité de son quota de durée (mesuré en millisecondes sur l'époque)
